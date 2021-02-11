---
description: リモート コマンドのローカル変数とリモート変数の使用方法について説明します。
Locale: en-US
ms.date: 03/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_variables?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_Variables
ms.openlocfilehash: 1cd6d0c4562fcd63fc56f103ec41aa6f0bb4c01c
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604956"
---
# <a name="about-remote-variables"></a>リモート変数について

## <a name="short-description"></a>簡単な説明

リモート コマンドのローカル変数とリモート変数の使用方法について説明します。

## <a name="long-description"></a>長い説明

リモートコンピューターで実行するコマンドで変数を使用できます。 変数に値を割り当て、その変数を値の代わりに使用します。

既定では、リモートコマンドの変数は、コマンドを実行するセッションで定義されていると見なされます。 ローカルセッションで定義されている変数は、コマンドでローカル変数として識別される必要があります。

## <a name="using-remote-variables"></a>リモート変数の使用

PowerShell では、リモートコマンドで使用される変数が、コマンドを実行するセッションで定義されていることを前提としています。

この例では、 `$ps` 変数は、コマンドが実行される一時的なセッションで定義されてい `Get-WinEvent` ます。

```powershell
Invoke-Command -ComputerName S1 -ScriptBlock {
  $ps = "*PowerShell*"; Get-WinEvent -LogName $ps
}
```

コマンドが永続的なセッションで実行されている場合、 **PSSession** では、そのセッションでリモート変数を定義する必要があります。

```powershell
$s = New-PSSession -ComputerName S1
Invoke-Command -Session $s -ScriptBlock {$ps = "*PowerShell*"}
Invoke-Command -Session $s -ScriptBlock {Get-WinEvent -LogName $ps}
```

## <a name="using-local-variables"></a>ローカル変数の使用

リモートコマンドでローカル変数を使用できますが、変数はローカルセッションで定義する必要があります。

PowerShell 3.0 以降では、スコープ修飾子を使用して、 `Using` リモートコマンドでローカル変数を識別できます。

の構文 `Using` は次のとおりです。

```
$Using:<VariableName>
```

次の例では、 `$ps` 変数はローカルセッションで作成されますが、コマンドを実行するセッションで使用されます。 `Using`スコープ修飾子は、 `$ps` ローカル変数としてを識別します。

```powershell
$ps = "*PowerShell*"
Invoke-Command -ComputerName S1 -ScriptBlock {
  Get-WinEvent -LogName $Using:ps
}
```

`Using`スコープ修飾子は **PSSession** で使用できます。

```powershell
$s = New-PSSession -ComputerName S1
$ps = "*PowerShell*"
Invoke-Command -Session $s -ScriptBlock {Get-WinEvent -LogName $Using:ps}
```

などの変数参照は、 `$using:var` `$var` 呼び出し元のコンテキストから変数の値に展開されます。 呼び出し元の変数オブジェクトにアクセスすることはできません。
スコープ修飾子を使用して、 `Using` **PSSession** 内のローカル変数を変更することはできません。 たとえば、次のコードは機能しません。

```powershell
$s = New-PSSession -ComputerName S1
$ps = "*PowerShell*"
Invoke-Command -Session $s -ScriptBlock {$Using:ps = 'Cannot assign new value'}
```

の詳細について `Using` は、「」を参照してください [about_Scopes](./about_Scopes.md)

### <a name="using-splatting"></a>スプラッティングの使用

PowerShell スプラッティングは、パラメーターの名前と値のコレクションをコマンドに渡します。 詳細については、「 [about_Splatting](about_Splatting.md)」を参照してください。

この例では、スプラッティング変数は、 `$Splat` ローカルコンピューターに設定されているハッシュテーブルです。 は、 `Invoke-Command` リモートコンピューターセッションに接続します。 **ScriptBlock** は、 `Using` splatted 変数を表すために、アットマーク () を使用して、スコープ修飾子を使用し `@` ます。

```powershell
$Splat = @{ Name = "Win*"; Include = "WinRM" }
Invoke-Command -Session $s -ScriptBlock { Get-Service @Using:Splat }
```

### <a name="other-situations-where-the-using-scope-modifier-is-needed"></a>' Using ' スコープ修飾子が必要なその他の状況

セッションが不足しているスクリプトまたはコマンドについては、 `Using` 呼び出し元のセッションスコープから変数の値を埋め込むためにスコープ修飾子が必要です。これにより、セッションコードが不足してしまうことがあります。 `Using`スコープ修飾子は、次のコンテキストでサポートされています。

- リモートで実行されたコマンド。 `Invoke-Command` **ComputerName**、 **HostName**、 **SSHConnection** 、または **Session** パラメーター (リモートセッション) の使用を開始しました。
- バックグラウンドジョブ、で開始 `Start-Job` (アウトプロセスセッション)
- スレッドジョブ `Start-ThreadJob` 。または `ForEach-Object -Parallel` (個別のスレッドセッション) を使用して開始

コンテキストによっては、埋め込み変数の値は、呼び出し元のスコープ内のデータの独立したコピーか、またはそれに対する参照です。 リモートとアウトプロセスのセッションでは、これらは常に独立したコピーです。 スレッドセッションでは、参照によって渡されます。

## <a name="serialization-of-variable-values"></a>変数値のシリアル化

リモートで実行されるコマンドとバックグラウンドジョブはアウトプロセスで実行されます。
アウトプロセスセッションでは、XML ベースのシリアル化と逆シリアル化を使用して、プロセスの境界を越えて変数の値を使用できるようにします。 シリアル化プロセスでは、オブジェクトは元のオブジェクトのプロパティを含む **PSObject** に変換されますが、そのメソッドは含まれません。

型の限定されたセットでは、逆シリアル化によって元の型に復元オブジェクトが返されます。 復元オブジェクトは、元のオブジェクトインスタンスのコピーです。
これには、型のプロパティとメソッドがあります。 System.string などの単純型の **場合、コピー** は完全に一致します。 複合型の場合、コピーは不完全です。 たとえば、復元された証明書オブジェクトには、秘密キーは含まれません。

他のすべての型のインスタンスは **PSObject** インスタンスです。 **PSTypeNames** プロパティには、**逆シリアル化** された元の型名 (たとえば、Deserialized.System) が含まれてい **ます。Data. DataTable**

## <a name="using-local-variables-with-argumentlist-parameter"></a>**ArgumentList** パラメーターでのローカル変数の使用

リモートコマンドでローカル変数を使用するには、リモートコマンドのパラメーターを定義し、コマンドレットの **ArgumentList** パラメーターを使用して `Invoke-Command` ローカル変数をパラメーター値として指定します。

- キーワードを使用して `param` 、リモートコマンドのパラメーターを定義します。 パラメーター名は、ローカル変数の名前と一致する必要がないプレースホルダーです。

- コマンドでキーワードで定義されているパラメーターを使用し `param` ます。

- コマンドレットの **ArgumentList** パラメーターを使用し `Invoke-Command` て、ローカル変数をパラメーター値として指定します。

たとえば、次のコマンドでは、 `$ps` ローカルセッションで変数を定義し、リモートコマンドで使用します。 このコマンドでは、パラメーター名としてを使用し、ローカル変数を値として使用し `$log` `$ps` ます。

```powershell
$ps = "*PowerShell*"
Invoke-Command -ComputerName S1 -ScriptBlock {
  param($log)
  Get-WinEvent -LogName $log
} -ArgumentList $ps
```

## <a name="see-also"></a>関連項目

[about_PSSessions](about_PSSessions.md)

[about_Remote](about_Remote.md)

[about_Scopes](about_Scopes.md)

[about_Splatting](about_Splatting.md)

[about_Variables](about_Variables.md)

[Enter-PSSession](xref:Microsoft.PowerShell.Core.Enter-PSSession)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

[Start-ThreadJob](xref:ThreadJob.Start-ThreadJob)

@Microsoft.PowerShell.Core.ForEach-Object

