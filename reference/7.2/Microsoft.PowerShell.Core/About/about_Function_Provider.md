---
description: Function
Locale: en-US
ms.date: 10/18/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_function_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: 関数プロバイダー
ms.openlocfilehash: f72ad1e93e65848238afd9feacb38982b4986177
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602213"
---
# <a name="function-provider"></a>関数プロバイダー

## <a name="provider-name"></a>プロバイダー名
Function

## <a name="drives"></a>ドライブ

`Function:`

## <a name="capabilities"></a>機能

**ShouldProcess**

## <a name="short-description"></a>簡単な説明

PowerShell で定義されている関数へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

Powershell **関数** プロバイダーを使用すると、powershell で関数とフィルターを取得、追加、変更、消去、削除できます。

関数とは、アクションを実行するコードのブロックに名前が付けられたものです。 関数名を入力すると、関数のコードが実行されます。 フィルターとは、アクションの条件を確立するコードのブロックに名前が付けられたものです。 コマンドのように、条件の代わりにフィルターの名前を入力でき `Where-Object` ます。

**関数** ドライブは、関数オブジェクトとフィルターオブジェクトのみを含むフラットな名前空間です。 関数にもフィルターにも子項目はありません。

**関数** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

それぞれの関数は、system.servicemodel クラスのインスタンスに[なります。](/dotnet/api/system.management.automation.functioninfo) 各フィルターは、 [System. Automation. FilterInfo](/dotnet/api/system.management.automation.filterinfo) クラスのインスタンスです。

## <a name="navigating-the-function-drive"></a>関数ドライブの移動

**関数** プロバイダーは、ドライブ内のデータストアを公開し `Function:` ます。 関数を操作するには、場所を `Function:` ドライブ () に変更し `Set-Location Function:` ます。 または、別の PowerShell ドライブから作業することもできます。 別の場所から関数を参照するには、パスのドライブ名 () を使用し `Function:` ます。

```powershell
Set-Location Function:
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、次のように入力します。

```powershell
Set-Location C:
```

また、その他の PowerShell ドライブから **関数** プロバイダーを操作することもできます。 別の場所から関数を参照するには、パス内のドライブ名を使用し `Function:` ます。

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスです。これ `cd` は、 [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスです。 と `pwd` は、 [Get Location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

## <a name="getting-functions"></a>関数の取得

このコマンドは現在のセッションのすべての関数の一覧を取得します。 このコマンドは、任意の PowerShell ドライブから使用できます。

```powershell
Get-ChildItem -Path Function:
```

関数プロバイダーにはコンテナーがないため、上のコマンドはと共に使用した場合と同じ効果があり `Get-ChildItem` ます。

```powershell
Get-ChildItem -Path Function:
```

次に示すように、 **定義** プロパティにアクセスすることによって、関数の定義を取得できます。

```powershell
(Get-Item -Path function:more).Definition
```

また、ドル記号 () で始まるプロバイダーパスを使用して、関数の定義を取得することもでき `$` ます。

```powershell
$function:more
```

### <a name="getting-selected-functions"></a>選択した関数の取得

このコマンドは、 `man` ドライブから関数を取得し `Function:` ます。 コマンドレットを使用して `Get-Item` 関数を取得します。 パイプライン演算子 () は、 `|` 結果をに送信し `Format-Table` ます。 パラメーターは、 `-Wrap` 行に収まりきらないテキストを次の行にリダイレクトします。 パラメーターを指定すると、 `-Autosize` テーブルの列のサイズがテキストに合わせて変更されます。

```powershell
Get-Item -Path man | Format-Table -Wrap -Autosize
```

### <a name="working-with-function-provider-paths"></a>関数プロバイダーパスの操作

これらのコマンドはどちらもという名前の関数を取得 `c:` します。 最初のコマンドはすべてのドライブで使用できます。 2番目のコマンドはドライブで使用され `Function:` ます。 ドライブの構文であり、名前がコロンで終わるため、ドライブ名でパスを修飾する必要があります。 ドライブ内では、どちらの形式でも `Function:` 使用できます。 2番目のコマンドでは、ドット ( `.` ) は現在の場所を表します。

```
PS C:\> Get-Item -Path Function:c:
PS Function:\> Get-Item -Path .\c:
```

## <a name="creating-a-function"></a>関数の作成

このコマンドは、コマンドレットを使用して `New-Item` 、という名前の関数を作成 `Win32:` します。
括弧で囲まれた式は関数名により表されるスクリプト ブロックです。

```powershell
New-Item -Path Function:Win32: -Value {Set-Location C:\Windows\System32}
```

関数は、PowerShell コマンドラインで入力することによって作成することもできます。 たとえば、tpe `Function:Win32: {Set-Location C:\Windows\System32}` です。 ドライブにいる場合は、 `Function:` ドライブ名を省略できます。

## <a name="deleting-a-function"></a>関数の削除

このコマンドは、 `more:` 現在のセッションから関数を削除します。

```powershell
Remove-Item Function:more:
```

## <a name="changing-a-function"></a>関数の変更

このコマンドは、 `Set-Item` コマンドレットを使用して、 `prompt` パスの前の時間を表示するように関数を変更します。

```powershell
Set-Item -Path Function:prompt -Value {
  'PS '+ (Get-Date -Format t) + " " + (Get-Location) + '> '
  }
```

### <a name="rename-a-function"></a>関数の名前を変更する

このコマンドは、コマンドレットを使用して `Rename-Item` 関数の名前をに変更し `help` `gh` ます。

```powershell
Rename-Item -Path Function:help -NewName gh
```

## <a name="copying-a-function"></a>関数のコピー

このコマンドは、 `prompt` 関数をにコピーし `oldPrompt` 、prompt 関数に関連付けられているスクリプトブロックの新しい名前を効果的に作成します。
これを利用し、変更予定のプロンプト関数のオリジナルを保存できます。
新しい関数の **Options** プロパティの値は `None` です。 **Options** プロパティの値を変更するには、を使用 `Set-Item` します。

```powershell
Copy-Item -Path Function:prompt -Destination Function:oldPrompt
```

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターは、PowerShell プロバイダーによって追加されるコマンドレットパラメーターであり、プロバイダーに対応したドライブでコマンドレットが使用されている場合にのみ使用できます。

### <a name="options-systemmanagementautomationscopeditemoptions"></a>オプション < [ScopedItemOptions] > を選択します。

関数の **Options** プロパティの値を決定します。

- `None`: オプションがありません。 `None` は既定値です。
- `Constant`: 関数を削除することはできません。また、そのプロパティを変更することもできません。 `Constant` は、関数を作成する場合にのみ使用できます。
  既存の関数のオプションをに変更することはできません `Constant` 。
- `Private`: 関数は、現在のスコープでのみ表示されます。
- (子スコープ内にありません)。
- `ReadOnly`: パラメーターを使用しない限り、関数のプロパティを変更することはできません `-Force` 。 を使用すると `Remove-Item` 、関数を削除できます。
- `AllScope`: 関数は、作成された新しいスコープにコピーされます。

### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

- [Set-Item](xref:Microsoft.PowerShell.Management.Set-Item)

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
Get-Help Get-ChildItem -Path function:
```

## <a name="see-also"></a>関連項目

[about_Functions](../About/about_Functions.md)

[about_Providers](../About/about_Providers.md)

