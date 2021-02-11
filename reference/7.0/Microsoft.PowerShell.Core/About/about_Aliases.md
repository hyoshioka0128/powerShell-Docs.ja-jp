---
description: PowerShell のコマンドレットとコマンドに代替名を使用する方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/27/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_aliases?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Aliases
ms.openlocfilehash: e160f1e11ec94142b04aca1dfc27eb24c4148f9b
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390747"
---
# <a name="about-aliases"></a>エイリアスについて

## <a name="short-description"></a>概要
PowerShell のコマンドレットとコマンドに代替名を使用する方法について説明します。

## <a name="long-description"></a>詳細説明

エイリアスは、コマンドレットまたは関数、スクリプト、ファイル、実行可能ファイルなどのコマンド要素の代替名またはニックネームです。 PowerShell コマンドでは、コマンド名の代わりにエイリアスを使用できます。

エイリアスを作成するには、New-Alias コマンドレットを使用します。 たとえば、次のコマンドは、コマンドレットの "ガス" エイリアスを作成し `Get-AuthenticodeSignature` ます。

```powershell
New-Alias -Name gas -Value Get-AuthenticodeSignature
```

コマンドレット名のエイリアスを作成した後は、コマンドレット名の代わりにエイリアスを使用できます。 たとえば、SqlScript.ps1 ファイルの Authenticode 署名を取得するには、次のように入力します。

```powershell
Get-AuthenticodeSignature SqlScript.ps1
```

または、次のように入力します。

```powershell
gas SqlScript.ps1
```

Microsoft Office Word のエイリアスとして "word" を作成した場合は、次のように「word」と入力することができます。

```powershell
"C:\Program Files\Microsoft Office\Office11\Winword.exe"
```

## <a name="built-in-aliases"></a>組み込みのエイリアス

PowerShell には、一連の組み込みエイリアスが含まれています。これには、Set-Location コマンドレットの "cd" と "chdir"、Get-ChildItem コマンドレットの "ls" と "dir" が含まれます。

組み込みエイリアスを含め、コンピューター上のすべてのエイリアスを取得するには、次のように入力します。

```powershell
Get-Alias
```

## <a name="alias-cmdlets"></a>エイリアスのコマンドレット

PowerShell には、エイリアスを使用するように設計された次のコマンドレットが含まれています。

- `Get-Alias` -現在のセッションのすべてのエイリアスを取得します。
- `New-Alias` -新しいエイリアスを作成します。
- `Set-Alias` -エイリアスを作成または変更します。
- `Export-Alias` -1 つまたは複数のエイリアスをファイルにエクスポートします。
- `Import-Alias` -エイリアスファイルを PowerShell にインポートします。

コマンドレットの詳細については、次のように入力してください。

```powershell
Get-Help <cmdlet-Name> -Detailed
```

たとえば、次のように入力します。

```powershell
Get-Help Export-Alias -Detailed
```

## <a name="creating-an-alias"></a>エイリアスの作成

新しいエイリアスを作成するには、New-Alias コマンドレットを使用します。 たとえば、Get-help の "gh" エイリアスを作成するには、次のように入力します。

```powershell
New-Alias -Name gh -Value Get-Help
```

コマンドでエイリアスを使用すると、完全なコマンドレット名を使用する場合と同様に、エイリアスをパラメーターと共に使用できます。

たとえば、Get-WmiObject コマンドレットの詳細なヘルプを表示するには、次のように入力します。

```powershell
Get-Help Get-WmiObject -Detailed
```

または、次のように入力します。

```powershell
gh Get-WmiObject -Detailed
```

## <a name="saving-aliases"></a>エイリアスの保存

作成したエイリアスは、現在のセッションにのみ保存されます。 別のセッションでエイリアスを使用するには、エイリアスを PowerShell プロファイルに追加します。 または、Export-Alias コマンドレットを使用して、エイリアスをファイルに保存します。

詳細については、次のように入力してください。

```powershell
Get-Help about_Profiles
```

## <a name="getting-aliases"></a>エイリアスの取得

組み込みエイリアス、PowerShell プロファイル内のエイリアス、現在のセッションで作成したエイリアスなど、現在のセッションのすべてのエイリアスを取得するには、次のように入力します。

```powershell
Get-Alias
```

特定のエイリアスを取得するには、Get-Alias コマンドレットの Name パラメーターを使用します。 たとえば、"p" で始まるエイリアスを取得するには、次のように入力します。

```powershell
Get-Alias -Name p*
```

特定の項目のエイリアスを取得するには、Definition パラメーターを使用します。 たとえば、Get-ChildItem コマンドレットのエイリアスを取得するには、次のように入力します。

```powershell
Get-Alias -Definition Get-ChildItem
```

### <a name="get-alias-output"></a>取得-エイリアスの出力

Get-Alias は、1つの型のオブジェクト (エイリアス情報オブジェクト) (エイリアス情報オブジェクト) のみを返します。 "Cd" など、ハイフンを含まないエイリアスの名前は、次の形式で表示されます。

```powershell
Get-Alias ac
```

```Output
CommandType     Name                    Version    Source
-----------     ----                    -------    ------
Alias           ac -> Add-Content
```

これにより、必要な情報をすばやく簡単に取得できます。

矢印に基づくエイリアス名の形式は、ハイフンを含むエイリアスには使用されません。 これらは、一般的な省略形またはニックネームの代わりに、コマンドレットと関数に優先される代替名となる可能性があり、作成者はそれらを明確にする必要がない可能性があります。

## <a name="alternate-names-for-commands-with-parameters"></a>パラメーターを使用したコマンドの代替名

コマンドレット、スクリプト、関数、または実行可能ファイルにエイリアスを割り当てることができます。 コマンドとそのパラメーターにエイリアスを割り当てることはできません。 たとえば、コマンドレットにエイリアスを割り当てることはでき `Get-Eventlog` ますが、コマンドにエイリアスを割り当てることはできません `Get-Eventlog -LogName System` 。

コマンドを含む関数を作成できます。 関数を作成するには、"function" という単語に続けて関数の名前を入力します。 コマンドを入力し、中かっこ () で囲み {} ます。

たとえば、次のコマンドは、syslog 関数を作成します。 この関数は、 `Get-Eventlog -LogName System` 次のコマンドを表します。

```powershell
function Get-SystemEventlog {Get-Eventlog -LogName System}
Set-Alias -Name syslog -Value Get-SystemEventlog
```

コマンドではなく「syslog」と入力できるようになりました。 また、新しい関数のエイリアスを作成することもできます。

関数の詳細については、次のように入力してください。

```powershell
Get-Help about_Functions
```

## <a name="alias-objects"></a>エイリアスオブジェクト

PowerShell エイリアスは、system.string クラスのインスタンスであるオブジェクトによって表されます。 この種類のオブジェクトの詳細については、PowerShell SDK の「 [エイリアス情報クラス][aliasinfo] 」を参照してください。

エイリアスオブジェクトのプロパティとメソッドを表示するには、エイリアスを取得します。
次に、パイプを Get-Member コマンドレットに渡します。 次に例を示します。

```powershell
Get-Alias | Get-Member
```

エイリアスなど、特定のエイリアスのプロパティの値を表示するには、 `dir` エイリアスを取得します。 次に、パイプを Format-List コマンドレットに渡します。 たとえば、次のコマンドは、"dir" エイリアスを取得します。 次に、コマンドによって、エイリアスが Format-List コマンドレットにパイプされます。 次に、このコマンドは、Format-List の Property パラメーターをワイルドカード文字 () と共に使用して、 \* エイリアスのすべてのプロパティを表示し `dir` ます。 次のコマンドは、これらのタスクを実行します。

```powershell
Get-Alias -Name dir | Format-List -Property *
```

## <a name="powershell-alias-provider"></a>PowerShell エイリアスプロバイダー

PowerShell には、エイリアスプロバイダーが含まれています。 エイリアスプロバイダーを使用すると、ファイルシステムドライブ上にある場合と同様に、PowerShell でエイリアスを表示できます。

Alias プロバイダーは Alias: ドライブを公開します。 Alias: ドライブにアクセスするには、次のように入力します。

```powershell
Set-Location Alias:
```

ドライブの内容を表示するには、次のように入力します。

```powershell
Get-ChildItem
```

別の PowerShell ドライブのドライブの内容を表示するには、ドライブ名でパスを開始します。 コロン (:) を含めます。 次に例を示します。

```powershell
Get-ChildItem -Path Alias:
```

特定のエイリアスに関する情報を取得するには、ドライブ名とエイリアス名を入力します。 または、名前のパターンを入力します。 たとえば、"p" で始まるすべてのエイリアスを取得するには、次のように入力します。

```powershell
Get-ChildItem -Path Alias:p*
```

PowerShell エイリアスプロバイダーの詳細については、次のように入力してください。

```powershell
Get-Help Alias
```

## <a name="see-also"></a>関連項目

- [New-Alias](xref:Microsoft.PowerShell.Utility.New-Alias)
- [Get-Alias](xref:Microsoft.PowerShell.Utility.Get-Alias)
- [Set-Alias](xref:Microsoft.PowerShell.Utility.Set-Alias)
- [Export-Alias](xref:Microsoft.PowerShell.Utility.Export-Alias)
- [Import-Alias](xref:Microsoft.PowerShell.Utility.Import-Alias)
- [Get-PSProvider](xref:Microsoft.PowerShell.Management.Get-PSProvider)
- [Get-PSDrive](xref:Microsoft.PowerShell.Management.Get-PSDrive)
- [about_functions](about_functions.md)
- [about_profiles](about_profiles.md)
- [about_providers](about_providers.md)

<!-- External links -->
[aliasinfo]: /dotnet/api/system.management.automation.aliasinfo
