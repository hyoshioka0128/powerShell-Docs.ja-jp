---
description: 変数
Locale: en-US
ms.date: 10/18/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_variable_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: 変数プロバイダー
ms.openlocfilehash: f93e58c24fdfc085983459d214db931274e164e2
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599517"
---
# <a name="variable-provider"></a>変数プロバイダー

## <a name="provider-name"></a>プロバイダー名
変数

## <a name="drives"></a>ドライブ

`Variable:`

## <a name="capabilities"></a>機能

**ShouldProcess**

## <a name="short-description"></a>簡単な説明

PowerShell 変数とその値へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

PowerShell **変数** プロバイダーを使用すると、現在のコンソールで powershell 変数を取得、追加、変更、消去、削除できます。

PowerShell **変数** プロバイダーは、powershell によって作成される変数 (自動変数、ユーザー設定変数、作成する変数など) をサポートしています。

**変数** drive は、変数オブジェクトのみを含むフラットな名前空間です。 変数に子項目はありません。

**変数** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)

PowerShell には、変数を表示および変更するために特に設計された一連のコマンドレットも含まれています。 **変数** コマンドレットを使用する場合は、名前にドライブを指定する必要はありません `Variable:` 。 この記事では、 **変数** コマンドレットの使用については説明しません。

- [Get-Variable](xref:Microsoft.PowerShell.Utility.Get-Variable)
- [New-Variable](xref:Microsoft.PowerShell.Utility.New-Variable)
- [変数の設定](xref:Microsoft.PowerShell.Utility.Set-Variable)
- [Remove-Variable](xref:Microsoft.PowerShell.Utility.Remove-Variable)
- [Clear-Variable](xref:Microsoft.PowerShell.Utility.Clear-Variable)

> [!NOTE]
> PowerShell 式パーサーを使用して、コマンドレットを使用せずに変数の値を作成、表示、変更することもできます。 変数を直接操作する場合は、ドル記号 () を使用し `$` て変数として名前を指定し、代入演算子 () を使用 `=` してその値を設定および変更します。 たとえば、は `$p = Get-Process` 変数を作成 `p` し、そこにコマンドの結果を格納し `Get-Process` ます。

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

変数には、いくつかの異なる型を指定できます。 ほとんどの変数は、クラスのインスタンスになり `PSVariable` ます。 その他の変数とその型を以下に示します。

- 変数は、 `?` クラスのインスタンスです `QuestionMarkVariable` 。
- 変数は、 `null` クラスのインスタンスです `NullVariable` 。
- 最大カウント変数は、クラスのインスタンスです `SessionStateCapacityVariable` 。
- `LocalVariable` インスタンスには、現在の実行に関する情報が含まれます。次に例を示します。
  - `MyInvocation`
  - `PSCommandPath`
  - `PSScriptRoot`
  - `PSBoundParameters`
  - `args`
  - `input`

## <a name="navigating-the-variable-drives"></a>変数ドライブの移動

**変数** プロバイダーは、ドライブ内のデータストアを公開し `Variable:` ます。 変数を使用するには、場所を `Variable:` ドライブ () に変更する `Set-Location Variable:` か、他の PowerShell ドライブから作業することができます。 別の場所から変数を参照するには、パスのドライブ名 () を使用し `Variable:` ます。

```powershell
Set-Location Variable:
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、次のように入力します。

```powershell
Set-Location C:
```

その他の PowerShell ドライブから **変数** プロバイダーを使用することもできます。 別の場所から変数を参照するには、パス内のドライブ名を使用し `Variable:` ます。

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスです。これ `cd` は、 [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスです。 と `pwd` は、 [Get Location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

## <a name="displaying-the-value-of-variables"></a>変数の値の表示

### <a name="get-all-variables-in-the-current-session"></a>現在のセッションのすべての変数を取得します。

このコマンドは現在のセッションのすべての変数とその値の一覧を取得します。 このコマンドは、任意の PowerShell ドライブから使用できます。

```powershell
Get-ChildItem -Path Variable:
```

### <a name="get-a-variable-using-its-provider-path"></a>プロバイダーパスを使用して変数を取得する

このコマンドは、ドル記号 () で始まるプロバイダーパスを使用して、変数値を取得 `$` します。 これは、変数名の先頭にドル記号 () を付ける場合と同じ効果があり `$` ます。

```powershell
$variable:home
```

### <a name="get-variables-using-wildcards"></a>ワイルドカードを使用して変数を取得する

このコマンドは「max」で始まる名前を持つ変数を取得します。 このコマンドは、任意の PowerShell ドライブから使用できます。

```powershell
Get-ChildItem -Path Variable:max*
```

### <a name="get-the-value-of-the--variable"></a>の値を取得します。 可変

このコマンドは、 `-LiteralPath` [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem) のパラメーターを使用して、ドライブ内から変数の値を取得し `?` `Variable:` ます。 は `?` パス内のワイルドカードですが、 `Get-ChildItem` パラメーターの値ではワイルドカードの解決を試みません `-LiteralPath` 。

```powershell
Get-ChildItem -Literalpath ?
```

### <a name="get-readonly-and-constant-variables"></a>読み取り専用変数と定数変数を取得する

このコマンド `ReadOnly` は、Options プロパティにまたはの値を持つ変数を取得し `Constant` ます。 

```powershell
Get-ChildItem -Path Variable: | Where-Object {
   $_.options -Match "Constant" `
   -or $_.options -Match "ReadOnly"
 } | Format-List -Property name, value, options
```

## <a name="creating-variables"></a>変数の作成

### <a name="create-a-new-variable"></a>新しい変数を作成する

このコマンドは、 `services` 変数を作成し、その変数にコマンドの結果を格納し `Get-Service` ます。 現在の場所はドライブにあるため `Variable:` 、パラメーターの値 `-Path` は、 `.` 現在の場所を表すドット () です。

コマンドをかっこで囲んで、 `Get-Service` 変数が作成される前にコマンドが実行されることを確認します。 括弧がない場合、新しい変数の値は文字列「Get-Service」です。

```powershell
New-Item -Path . -Name services -Value (Get-Service)
```

### <a name="create-a-variable-using-an-absolute-path"></a>絶対パスを使用して変数を作成する

このコマンドは、 `services` 変数を作成し、その変数にコマンドの結果を格納し `Get-Service` ます。

```powershell
New-Item -Path Variable:services -Value Get-Service
```

 値なしで変数を作成するには、代入演算子を省略します。

## <a name="changing-variables"></a>変数の変更

### <a name="rename-a-variable"></a>変数名の変更

このコマンドは、コマンドレットを使用して、 `Rename-Item` 変数の名前をに変更し `a` `processes` ます。

```powershell
Rename-Item -Path Variable:a -NewName processes
```

### <a name="change-the-value-of-a-variable"></a>変数の値を変更する

このコマンドは、コマンドレットを使用して、 `Set-Item` 変数の値を `ErrorActionPreference` "Stop" に変更します。

```powershell
Set-Item -Path Variable:ErrorActionPreference -Value Stop
```

## <a name="copy-a-variable"></a>変数をコピーする

このコマンドは、 `Copy-Item` コマンドレットを使用して、変数をにコピーし `processes` `old_processes` ます。 これにより、変数と同じ値を持つという名前の新しい変数が作成さ `old_processes` `processes` れます。

```powershell
Copy-Item -Path Variable:processes -Destination Variable:old_processes
```

## <a name="delete-a-variable"></a>変数の削除

このコマンドは、 `serv` 現在のセッションから変数を削除します。 このコマンドは、任意の PowerShell ドライブで使用できます。

```powershell
Remove-Variable -Path Variable:serv
```

### <a name="delete-variables-using-the--force-parameter"></a>-Force パラメーターを使用して変数を削除する

このコマンドは、 **Options** プロパティの値がである変数を除き、現在のセッションからすべての変数を削除 `Constant` します。 パラメーターを指定しない場合、このコマンドでは、 `-Force` **Options** プロパティの値がである変数は削除されません `ReadOnly` 。

```powershell
Remove-Item Variable:* -Force
```

## <a name="setting-the-value-of-a-variable-to-null"></a>変数の値を NULL に設定する

このコマンドは、 `Clear-Item` コマンドレットを使用して、変数の値を `processes` NULL に変更します。

```powershell
Clear-Item -Path Variable:processes
```

## <a name="using-the-pipeline"></a>パイプラインの使用

プロバイダーのコマンドレットは、パイプラインの入力を受け入れます。 パイプラインを使用すると、1つのコマンドレットから別のプロバイダーのコマンドレットにプロバイダーデータを送信することにより、タスクを簡単に行うことができます。
プロバイダーコマンドレットでパイプラインを使用する方法の詳細については、この記事全体で説明されているコマンドレットリファレンスを参照してください。

## <a name="getting-help"></a>ヘルプの表示

Windows PowerShell 3.0 より、プロバイダー コマンドレットのためにカスタマイズされたヘルプ トピックを取得できます。これはファイル システム ドライブでのプロバイダー コマンドレットの動作を説明します。

ファイルシステムドライブ用にカスタマイズされたヘルプトピックを取得するには、ファイルシステムドライブで [get-help](xref:Microsoft.PowerShell.Core.Get-Help) コマンドを実行するか、 `-Path` [get-help](xref:Microsoft.PowerShell.Core.Get-Help) のパラメーターを使用してファイルシステムドライブを指定します。

```powershell
Get-Help Get-ChildItem
```

```powershell
Get-Help Get-ChildItem -Path variable:
```

## <a name="see-also"></a>関連項目

[about_Variables](../About/about_Variables.md)

[about_Automatic_Variables](../About/about_Automatic_Variables.md)

[about_Providers](../About/about_Providers.md)

