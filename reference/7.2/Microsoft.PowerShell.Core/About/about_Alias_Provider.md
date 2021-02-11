---
description: エイリアス
Locale: en-US
ms.date: 10/18/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_alias_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: エイリアス プロバイダー
ms.openlocfilehash: d6bfbaf878a6d971b1e01d963faec8c8ece5d1fc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601426"
---
# <a name="alias-provider"></a>エイリアスプロバイダー

## <a name="provider-name"></a>プロバイダー名
エイリアス

## <a name="drives"></a>ドライブ

`Alias:`

## <a name="capabilities"></a>機能

**ShouldProcess**

## <a name="short-description"></a>簡単な説明

PowerShell エイリアスとそれが表す値へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

PowerShell **エイリアス** プロバイダーを使用すると、powershell のエイリアスを取得、追加、変更、消去、削除できます。

エイリアスは、コマンドレット、関数、およびスクリプトを含む実行可能ファイルの代替名です。 PowerShell には、一連の組み込みエイリアスが含まれています。 独自のエイリアスを現在のセッションと PowerShell プロファイルに追加できます。

**エイリアス** ドライブは、エイリアスオブジェクトのみを含むフラットな名前空間です。
エイリアスに子項目はありません。

**エイリアス** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)

PowerShell には、エイリアスの表示と変更を行うための一連のコマンドレットが用意されています。 **Alias** コマンドレットを使用する場合は、名前にドライブを指定する必要はありません `Alias:` 。 この記事では、 **エイリアス** コマンドレットの使用については説明しません。

- [Export-Alias](xref:Microsoft.PowerShell.Utility.Export-Alias)
- [Get-Alias](xref:Microsoft.PowerShell.Utility.Get-Alias)
- [Import-Alias](xref:Microsoft.PowerShell.Utility.Import-Alias)
- [New-Alias](xref:Microsoft.PowerShell.Utility.New-Alias)
- [Set-Alias](xref:Microsoft.PowerShell.Utility.Set-Alias)

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

各エイリアスは、 [System. Automation. エイリアス情報](/dotnet/api/system.management.automation.aliasinfo) クラスのインスタンスです。

## <a name="navigating-the-alias-drive"></a>エイリアスドライブの移動

**エイリアス** プロバイダーは、ドライブ内のそのデータストアを公開し `Alias:` ます。 エイリアスを使用するには、 `Alias:` 次のコマンドを使用して場所をドライブに変更します。

```powershell
Set-Location Alias:
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、次のように入力します。

```powershell
Set-Location C:
```

エイリアスプロバイダーは、他の PowerShell ドライブからも使用できます。 別の場所からエイリアスを参照するには、 `Alias:` パス内のドライブ名を使用します。

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスです。これ `cd` は、 [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスです。 と `pwd` は、 [Get Location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

### <a name="displaying-the-contents-of-the-alias-drive"></a>Alias: ドライブの内容を表示しています

このコマンドは、現在の場所がドライブである場合に、すべてのエイリアスの一覧を取得し `Alias:` ます。 `*`現在の場所のすべての内容を示すために、ワイルドカード文字を使用します。

```powershell
PS Alias:\> Get-Item -Path *
```

ドライブでは、 `Alias:` 現在の `.` 場所を表すドットと、 `*` 現在の場所にあるすべての項目を表すワイルドカード文字が同じ効果を持ちます。 たとえば、 `Get-Item -Path .` またはは `Get-Item \*` 同じ結果を生成します。

エイリアスプロバイダーにはコンテナーがないため、上のコマンドはと共に使用する場合と同じ効果があり `Get-ChildItem` ます。

```powershell
Get-ChildItem -Path Alias:
```

### <a name="get-a-selected-alias"></a>選択したエイリアスを取得します

このコマンドは、エイリアスを取得し `ls` ます。
パスが含まれているため、任意の PowerShell ドライブで使用できます。

```powershell
Get-Item -Path Alias:ls
```

ドライブにいる場合は、 `Alias:` パスからドライブ名を省略できます。

プロバイダーのパスにドル記号 () を付けることによって、エイリアスの定義を取得することもでき `$` ます。

```powershell
$Alias:ls
```

### <a name="get-all-aliases-for-a-specific-cmdlet"></a>特定のコマンドレットのすべてのエイリアスを取得する

このコマンドは、コマンドレットに関連付けられているエイリアスの一覧を取得し `Get-ChildItem` ます。 この例では、コマンドレット名を格納する **Definition** プロパティを使用します。

```powershell
Get-Item -Path Alias:* | Where-Object {$_.Definition -eq "Get-ChildItem"}
```

## <a name="creating-aliases"></a>エイリアスの作成

### <a name="create-an-alias-from-the-alias-drive"></a>Alias: ドライブからエイリアスを作成します。

このコマンドは、 `serv` コマンドレットのエイリアスを作成し `Get-Service` ます。 現在の場所はドライブにあるため `Alias:` 、 `-Path` パラメーターは必要ありません。

また、このコマンドは動的パラメーターを使用して、 `-Options` エイリアスの **allscope** オプションを設定します。 パラメーターは、 `-Options` `New-Item` ドライブ内にある場合にのみ、コマンドレットで使用でき `Alias:` ます。 ドット () は、 `.` 現在のディレクトリ (エイリアスドライブ) を示します。

```
PS Alias:\> New-Item -Path . -Name serv -Value Get-Service -Options "AllScope"
```

### <a name="create-an-alias-with-an-absolute-path"></a>絶対パスを使用してエイリアスを作成する

コマンドを呼び出す項目にエイリアスを作成できます。
このコマンドは、 `np` の別名を作成し `Notepad.exe` ます。

```powershell
New-Item -Path Alias:np -Value c:\windows\notepad.exe
```

### <a name="create-an-alias-to-a-new-function"></a>新しい関数のエイリアスを作成する

あらゆる関数にエイリアスを作成できます。 この機能を利用し、コマンドレットとそのパラメーターの両方を含むエイリアスを作成できます。

最初のコマンドは、 `CD32` 現在のディレクトリをディレクトリに変更する関数を作成し `System32` ます。 2番目のコマンドは、 `go` 関数のエイリアスを作成し `CD32` ます。

コマンドが完了すると、 `CD32` またはのいずれか `go` を使用して関数を呼び出すことができます。

```powershell
function CD32 {Set-Location -Path c:\windows\system32}
Set-Item -Path Alias:go -Value CD32
```

## <a name="changing-aliases"></a>変更 (エイリアスを)

### <a name="change-the-options-of-an-alias"></a>エイリアスのオプションを変更する

`Set-Item`コマンドレットを動的パラメーターと共に使用して、 `-Options` エイリアスのプロパティの値を変更でき `-Options` ます。

このコマンドは、エイリアスの **Allscope** オプションと **ReadOnly** オプションを設定し `dir` ます。 コマンドは、コマンド `-Options` レットの動的パラメーターを使用し `Set-Item` ます。 パラメーターは、 `-Options` `Set-Item` **エイリアス** または **関数** プロバイダーと共に使用するときにで使用できます。

```powershell
Set-Item -Path Alias:dir -Options "AllScope,ReadOnly"
```

### <a name="change-an-aliases-referenced-command"></a>エイリアス参照コマンドの変更

このコマンドは、コマンドレットを使用して、コマンドレットではなくコマンドレットを `Set-Item` 表すようにエイリアスを変更し `gp` `Get-Process` `Get-ItemProperty` ます。
`-Force`エイリアスの **Options** プロパティの値 `gp` がに設定されているため、パラメーターが必要です `ReadOnly` 。 コマンドはドライブ内から送信されるため、 `Alias:` ドライブはパスに指定されていません。

```powershell
Set-Item -Path gp -Value Get-Process -Force
```

この変更は、エイリアスとコマンドの関係を定義する 4 つのプロパティに影響を与えます。 変更の効果を表示するには、次のコマンドを入力します。

```powershell
Get-Item -Path gp | Format-List -Property *
```

### <a name="rename-an-alias"></a>エイリアスの名前を変更する

このコマンドは、コマンドレットを使用して `Rename-Item` 、エイリアスをに変更し `popd` `pop` ます。

```powershell
Rename-Item -Path Alias:popd -NewName pop
```

## <a name="copying-an-alias"></a>エイリアスのコピー

このコマンドは、 `pushd` `push` コマンドレットに対して新しいエイリアスが作成されるようにエイリアスをコピーし `Push-Location` ます。

新しいエイリアスが作成されると、その **Description** プロパティの値は null になります。
また、その **Option** プロパティの値は `None` です。 コマンドがドライブ内から発行されている場合は、 `Alias:` パラメーターの値からドライブ名を省略でき `-Path` ます。

```powershell
Copy-Item -Path Alias:pushd -Destination Alias:push
```

## <a name="deleting-an-alias"></a>エイリアスの削除

このコマンドは、 `serv` 現在のセッションからエイリアスを削除します。
このコマンドは、任意の PowerShell ドライブで使用できます。

```powershell
Remove-Item -Path Alias:serv
```

このコマンドは「s」で始まるエイリアスを削除します。
読み取り専用のエイリアスは削除されません。

```powershell
Clear-Item -Path Alias:s*
```

### <a name="delete-read-only-aliases"></a>読み取り専用エイリアスの削除

このコマンドは、Options プロパティの値がであるものを除き、現在のセッションからすべてのエイリアスを削除し `Constant` ます。  `-Force`パラメーターを使用すると、 **Options** プロパティの値がであるエイリアスをコマンドで削除でき `ReadOnly` ます。

```powershell
Remove-Item Alias:* -Force
```

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターは、PowerShell プロバイダーによって追加されるコマンドレットパラメーターであり、プロバイダーに対応したドライブでコマンドレットが使用されている場合にのみ使用できます。

### <a name="options-systemmanagementautomationscopeditemoptions"></a>オプション ([ScopedItemOptions])

エイリアスの **Options** プロパティの値を決定します。

- **None**: オプションはありません。 この値は既定値です。
- **定数**: エイリアスを削除することはできません。また、そのプロパティを変更することもできません。
  **定数** は、エイリアスを作成した場合にのみ使用できます。 既存の別名のオプションを **定数** に変更することはできません。
- **プライベート**: 別名は、子スコープではなく、現在のスコープでのみ表示されます。
- **ReadOnly**: パラメーターを使用する場合を除き、別名のプロパティを変更することはできません `-Force` 。 を使用し `Remove-Item` てエイリアスを削除できます。
- **Allscope**: 作成された新しいスコープにエイリアスがコピーされます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

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
Get-Help Get-ChildItem -Path alias:
```

## <a name="see-also"></a>関連項目

[about_Aliases](../About/about_Aliases.md)

[about_Providers](../About/about_Providers.md)

