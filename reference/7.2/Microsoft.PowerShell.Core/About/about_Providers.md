---
description: PowerShell プロバイダーが、コマンドラインから簡単にアクセスできないデータおよびコンポーネントへのアクセスを提供する方法について説明します。 データは、ファイルシステムドライブに似た一貫した形式で表示されます。
Locale: en-US
ms.date: 03/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_providers?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Providers
ms.openlocfilehash: 6073ef13a6d0a02cded69073d7ffaef903a807ea
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600985"
---
# <a name="about-providers"></a>プロバイダーについて

## <a name="short-description"></a>簡単な説明
PowerShell プロバイダーが、コマンドラインから簡単にアクセスできないデータおよびコンポーネントへのアクセスを提供する方法について説明します。
データは、ファイルシステムドライブに似た一貫した形式で表示されます。

## <a name="long-description"></a>長い説明

PowerShell プロバイダーは、表示と管理を容易にするために特殊なデータストアへのアクセスを提供する .NET プログラムです。 データはドライブに表示され、ハードディスクドライブの場合と同様に、パス内のデータにアクセスします。 プロバイダーがサポートする組み込みのコマンドレットのいずれかを使用して、プロバイダードライブのデータを管理できます。 また、特にデータ用に設計されたカスタムコマンドレットを使用することもできます。

プロバイダーは、組み込みのコマンドレットに動的パラメーターを追加することもできます。 これらのパラメーターは、プロバイダーデータと共にコマンドレットを使用する場合にのみ使用できます。

## <a name="built-in-providers"></a>組み込みプロバイダー

PowerShell には、さまざまな種類のデータストアにアクセスするために使用できる一連の組み込みプロバイダーが用意されています。

| プロバイダー   |   ドライブ (s)  |OutputType                                                    |
|----------- |------------ |--------------------------------------------------------------|
|エイリアス       |エイリアス:       |システム管理. エイリアス情報                        |
|Certificate |Cert:        |X509StoreLocation です。               |
|            |             |System.Security.Cryptography.X509Certificates.X509Certificate2|
|環境 |Env:         |System.string. DictionaryEntry                            |
|FileSystem (ファイル システム)  |C: (*)       |システム. IO. FileInfo                                            |
|            |             |DirectoryInfo                                       |
|Function    |関数:    |システム管理. FunctionInfo                     |
|レジストリ    |HKLM: HKCU:  |Microsoft Win32. RegistryKey                                   |
|変数    |変数:    |システム管理. PSVariable                       |
|WSMan       |WSMan:       |WSManConfigContainerElement のようになります。        |

(*)ファイルシステムドライブは、システムによって異なります。

独自の PowerShell プロバイダーを作成することもできます。また、他のユーザーが開発するプロバイダーをインストールすることもできます。 セッションで使用可能なプロバイダーの一覧を表示するには、次のように入力します。

```powershell
Get-PSProvider
```

## <a name="installing-and-removing-providers"></a>プロバイダーのインストールと削除

プロバイダーは通常、PowerShell モジュールを使用してインストールされます。 モジュールをインポートすると、プロバイダーがセッションに読み込まれます。 組み込みプロバイダーをアンインストールすることはできません。 他のモジュールによって読み込まれたプロバイダーをアンインストールできます。

コマンドレットを使用して、現在のセッションからプロバイダーをアンロードでき `Remove-Module` ます。 このコマンドレットではプロバイダーはアンインストールされませんが、プロバイダーをセッションで使用できなくなります。

また、コマンドレットを使用して、 `Remove-PSDrive` 現在のセッションから任意のドライブを削除することもできます。 ドライブ上のこのデータは影響を受けませんが、そのセッションでは使用できなくなります。

## <a name="viewing-providers"></a>プロバイダーの表示

コンピューター上の PowerShell プロバイダーを表示するには、次のように入力します。

```powershell
Get-PSProvider
```

出力には、セッションに追加した組み込みプロバイダーとプロバイダーが一覧表示されます。

## <a name="the-provider-cmdlets"></a>プロバイダーのコマンドレット

次のコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 同じコマンドレットを同じ方法で使用して、プロバイダーが公開するさまざまな種類のデータを管理できます。 1つのプロバイダーのデータを管理する方法を学習したら、任意のプロバイダーのデータと同じ手順を使用できます。

たとえば、 `New-Item` コマンドレットは新しい項目を作成します。 FileSystem プロバイダーでサポートされて `C:` いるドライブで、を使用して `New-Item` 新しいファイルまたはフォルダーを作成することができます。 **レジストリ** プロバイダーでサポートされているドライブで、を使用して `New-Item` 新しいレジストリキーを作成できます。 ドライブで `Alias:` 、を使用して `New-Item` 新しいエイリアスを作成できます。

次のコマンドレットの詳細については、「」と入力してください。

```
Get-Help <cmdlet-name> -Detailed
```

### <a name="childitem-cmdlets"></a>Get-childitem コマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="content-cmdlets"></a>コンテンツのコマンドレット

- [Add-Content](xref:Microsoft.PowerShell.Management.Add-Content)
- [Clear-Content](xref:Microsoft.PowerShell.Management.Clear-Content)
- [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)
- [Set-Content](xref:Microsoft.PowerShell.Management.Set-Content)

### <a name="item-cmdlets"></a>項目のコマンドレット

- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)
- [Copy-Item](xref:Microsoft.PowerShell.Management.Copy-Item)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [Invoke-Item](xref:Microsoft.PowerShell.Management.Invoke-Item)
- [Move-Item](xref:Microsoft.PowerShell.Management.Move-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Rename-Item](xref:Microsoft.PowerShell.Management.Rename-Item)
- [Set-Item](xref:Microsoft.PowerShell.Management.Set-Item)

### <a name="itemproperty-cmdlets"></a>Get-itemproperty コマンドレット

- [Clear-ItemProperty](xref:Microsoft.PowerShell.Management.Clear-ItemProperty)
- [Copy-ItemProperty](xref:Microsoft.PowerShell.Management.Copy-ItemProperty)
- [Get-ItemProperty](xref:Microsoft.PowerShell.Management.Get-ItemProperty)
- [Move-ItemProperty](xref:Microsoft.PowerShell.Management.Move-ItemProperty)
- [New-ItemProperty](xref:Microsoft.PowerShell.Management.New-ItemProperty)
- [Remove-ItemProperty](xref:Microsoft.PowerShell.Management.Remove-ItemProperty)
- [Rename-ItemProperty](xref:Microsoft.PowerShell.Management.Rename-ItemProperty)
- [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)

### <a name="location-cmdlets"></a>Location コマンドレット

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Pop-Location](xref:Microsoft.PowerShell.Management.Pop-Location)
- [Push-Location](xref:Microsoft.PowerShell.Management.Push-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)

### <a name="path-cmdlets"></a>Path コマンドレット

- [Join-Path](xref:Microsoft.PowerShell.Management.Join-Path)
- [Convert-Path](xref:Microsoft.PowerShell.Management.Convert-Path)
- [Split-Path](xref:Microsoft.PowerShell.Management.Split-Path)
- [Resolve-Path](xref:Microsoft.PowerShell.Management.Resolve-Path)
- [Test-Path](xref:Microsoft.PowerShell.Management.Test-Path)

### <a name="psdrive-cmdlets"></a>PSDrive コマンドレット

- [Get-PSDrive](xref:Microsoft.PowerShell.Management.Get-PSDrive)
- [New-PSDrive](xref:Microsoft.PowerShell.Management.New-PSDrive)
- [Remove-PSDrive](xref:Microsoft.PowerShell.Management.Remove-PSDrive)

### <a name="psprovider-cmdlets"></a>PSProvider コマンドレット

- [Get-PSProvider](xref:Microsoft.PowerShell.Management.Get-PSProvider)

## <a name="viewing-provider-data"></a>プロバイダーデータの表示

プロバイダーの主な利点は、使い慣れた一貫性のある方法でデータを公開することです。 データ表現のモデルは、ファイルシステムドライブです。

プロバイダーを使用すると、ファイルシステム内のデータであるかのように、データストア内の項目を表示、移動、および変更できます。 データストアは、サポートしているドライブの名前によってアクセスされます。

このドライブはコマンドレットの既定の表示で表示され `Get-PSProvider` ますが、コマンドレットを使用してプロバイダーのドライブに関する情報を取得でき `Get-PSDrive` ます。 たとえば、Function: ドライブのすべてのプロパティを取得するには、次のように入力します。

```powershell
Get-PSDrive Function | Format-List *
```

ファイルシステムドライブの場合と同様に、プロバイダードライブのデータを表示したり、移動したりすることができます。

プロバイダードライブの内容を表示するには、Get-Item または Get-ChildItem コマンドレットを使用します。 ドライブ名の後にコロン (:) を入力します。 たとえば、Alias: ドライブの内容を表示するには、次のように入力します。

```powershell
Get-Item alias:
```

パスにドライブ名を含めることで、別のドライブのデータを表示および管理できます。 たとえば、別のドライブの HKLM: ドライブの、Hklm\software レジストリキーを表示するには、次のように入力します。

```powershell
Get-ChildItem HKLM:\SOFTWARE\
```

ドライブを開くには、Set-Location コマンドレットを使用します。 ドライブパスを指定するときは、コロンを忘れないようにしてください。 たとえば、場所を Cert: ドライブのルートディレクトリに変更するには、次のように入力します。

```powershell
Set-Location cert:
```

次に、Cert: ドライブの内容を表示するには、次のように入力します。

```powershell
Get-ChildItem
```

## <a name="moving-through-hierarchical-data"></a>階層データの移動

ハードディスクドライブの場合と同様に、プロバイダードライブ間を移動することができます。
項目内の項目の階層にデータが配置されている場合は、円記号 () を使用して `\` 子項目を指定します。 次の形式を使用します。

```
drive:\location\child-location\...
```

たとえば、場所を、Hklm\software レジストリキーに変更するには、次のように Set-Location コマンドを入力します。

```powershell
Set-Location HKLM:\SOFTWARE\
```

完全修飾名の要素にスペースが含まれている場合は、名前を二重引用符 () で囲む必要があり `"` ます。 次の例は、スペースを含む完全修飾パスを示しています。

```
"C:\Program Files\Internet Explorer\iexplore.exe"
```

また、相対参照を使用して場所を参照することもできます。 ドット () は、 `.` 現在の場所を表します。 たとえば、 `HKLM:\Software\Microsoft` レジストリキーを使用していて、キーのレジストリサブキーを一覧表示する場合は、 `HKLM:\Software\Microsoft\PowerShell` 次のコマンドを入力します。

```powershell
Get-ChildItem .\PowerShell
```

また、二重ドット () は、 `..` 現在の場所のすぐ上にあるディレクトリまたはコンテナーを参照します。 2つのドット ( `..` ) を使用して、プロバイダー階層内を移動できます。

```
PS HKLM:\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters\> cd ..\..\LanmanWorkstation\Parameters
PS HKLM:\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters>
```

## <a name="provider-home"></a>プロバイダーホーム

また、プロバイダーには **ホーム** の場所もあります。  この場所は、プロバイダーによってサポートされるすべてので共有され `PSDrives` ます。 プロバイダーの **Home** プロパティを表示することによって取得できます。

```powershell
Get-PSProvider | Format-Table Name, Home
```

```Output
Name        Home
----        ----
Registry
Alias
Environment
FileSystem  C:\Users\username
Function
Variable
Certificate
```

**FileSystem** プロバイダーは、 **Home** の既定値を持つ唯一のプロバイダーです。 と同じ値 `$Home` です。 詳細については、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

現在のセッションでは、そのプロパティを使用して、プロバイダーの **ホーム** ディレクトリを設定できます。

```powershell
(Get-PSProvider FileSystem).Home = "C:\"
```

この `~` 文字は、プロバイダーのホームディレクトリを表すために使用できます。
プロバイダーに **ホーム** の場所が設定されていない場合は、エラーが表示されます。

```powershell
Cert:\> Set-Location ~
```

```Output
Set-Location : Home location for this provider isn't set. To set the home
location, call "(get-psprovider 'Certificate').Home = 'path'".
At line:1 char:1
+ Set-Location ~
+ ~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Set-Location],
                              PSInvalidOperationException
...
```

## <a name="finding-dynamic-parameters"></a>動的パラメーターの検索

動的パラメーターは、プロバイダーによってコマンドレットに追加されるコマンドレットパラメーターです。 これらのパラメーターは、コマンドレットを追加したプロバイダーと共に使用する場合にのみ使用できます。

たとえば、ドライブは、 `Cert:` およびコマンドレットに **Codesigningcert** パラメーターを追加し `Get-Item` `Get-ChildItem` ます。 このパラメーターは `Get-Item` 、ドライブでまたはを使用する場合にのみ使用でき `Get-ChildItem` `Cert:` ます。

プロバイダーがサポートする動的パラメーターの一覧については、プロバイダーのヘルプファイルを参照してください。 型:

```
Get-Help <provider-name>
```

次に例を示します。

```powershell
Get-Help certificate
```

## <a name="learning-about-providers"></a>プロバイダーについて学習する

すべてのプロバイダーデータはドライブに表示されますが、同じメソッドを使用して移動すると、類似性が停止します。 プロバイダーによって公開されるデータストアは、Active Directory の場所や Microsoft Exchange Server のメールボックスと同じように変化することがあります。

個々の PowerShell プロバイダーの詳細については、次のように入力してください。

```
Get-Help <ProviderName>
```

次に例を示します。

```powershell
Get-Help registry
```

プロバイダーに関するヘルプトピックの一覧を表示するには、次のように入力してください。

```powershell
Get-Help * -Category Provider
```

## <a name="see-also"></a>関連項目

[about_Locations](about_Locations.md)

[about_Path_Syntax](about_Path_Syntax.md)

