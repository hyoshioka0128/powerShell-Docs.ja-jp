---
description: レジストリ
Locale: en-US
ms.date: 10/18/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_registry_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: レジストリ プロバイダー
ms.openlocfilehash: 2d57afc164885b4d207108a360215e63a5431632
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602596"
---
# <a name="registry-provider"></a>レジストリプロバイダー

## <a name="provider-name"></a>プロバイダー名

レジストリ

## <a name="drives"></a>ドライブ

`HKLM:`, `HKCU:`

## <a name="capabilities"></a>機能

"**プロセス**"、" **usetransactions** "

## <a name="short-description"></a>簡単な説明

PowerShell のレジストリキー、エントリ、および値へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

Powershell **レジストリ** プロバイダーを使用すると、powershell のレジストリキー、エントリ、および値を取得、追加、変更、消去、削除できます。

**レジストリ** ドライブは、コンピューター上のレジストリキーとサブキーを含む階層型の名前空間です。 レジストリ エントリと値はその階層の構成要素ではありません。 各キーのプロパティです。

**レジストリ** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)
- [Invoke-Item](xref:Microsoft.PowerShell.Management.Invoke-Item)
- [Move-Item](xref:Microsoft.PowerShell.Management.Move-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Get-ItemProperty](xref:Microsoft.PowerShell.Management.Get-ItemProperty)
- [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)
- [Remove-ItemProperty](xref:Microsoft.PowerShell.Management.Remove-ItemProperty)
- [Clear-ItemProperty](xref:Microsoft.PowerShell.Management.Clear-ItemProperty)
- [Get-Acl](xref:Microsoft.PowerShell.Security.Get-Acl)
- [Set-Acl](xref:Microsoft.PowerShell.Security.Set-Acl)

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

レジストリキーは、 [Microsoft の Win32](/dotnet/api/microsoft.win32.registrykey) クラスのインスタンスとして表されます。 レジストリエントリは、 [Pscustomobject](/dotnet/api/system.management.automation.pscustomobject) クラスのインスタンスとして表されます。

## <a name="navigating-the-registry-drives"></a>レジストリドライブの移動

**レジストリ** プロバイダーは、そのデータストアを2つの既定のドライブとして公開します。 レジストリの場所 HKEY_LOCAL_MACHINE がドライブにマップされ、 `HKLM:` HKEY_CURRENT_USER がドライブにマップされ `HKCU:` ます。 レジストリを操作するには、 `HKLM:` 次のコマンドを使用して場所をドライブに変更します。

```powershell
Set-Location HKLM:
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、次のように入力します。

```powershell
Set-Location C:
```

他の PowerShell ドライブから **レジストリ** プロバイダーを操作することもできます。 別の場所からレジストリキーを参照するには、パスのドライブ名 (、) を使用し `HKLM:` `HKCU:` ます。 \\**レジストリ** ドライブのレベルを示すには、円記号 () またはスラッシュ (/) を使用します。

```powershell
PS C:\> cd HKLM:\Software
```

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスであり、 `cd` [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスであり、 `pwd` [get location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

この最後の例は、 **レジストリ** プロバイダーの移動に使用できる別のパス構文を示しています。 この構文では、プロバイダー名に続けて2つのコロンが使用され `::` ます。 この構文では、マップされたドライブ名ではなく、完全な HIVE 名を使用でき `HKLM` ます。

```powershell
cd "Registry::HKEY_LOCAL_MACHINE\Software"
```

## <a name="displaying-the-contents-of-registry-keys"></a>レジストリキーの内容を表示しています

レジストリは、キー、サブキー、およびエントリに分割されます。 レジストリ構造の詳細については、「 [レジストリの構造](/windows/desktop/sysinfo/structure-of-the-registry)」を参照してください。

**レジストリ** ドライブでは、各キーはコンテナーです。 キーには、任意の数のキーを含めることができます。 親キーを持つレジストリキーは、サブキーと呼ばれます。 を使用し `Get-ChildItem` て、レジストリキーを表示したり `Set-Location` 、キーのパスに移動したりできます。

レジストリ値はレジストリキーの属性です。 **レジストリ** ドライブでは、**アイテムのプロパティ** と呼ばれます。 レジストリキーには、子キーと項目プロパティの両方を含めることができます。

この例では、との `Get-Item` 違い `Get-ChildItem` が示されています。 を `Get-Item` "スプーラ" レジストリキーに対して使用すると、そのプロパティを表示できます。

```
PS C:\ > Get-Item -Path HKLM:\SYSTEM\CurrentControlSet\Services\Spooler


    Hive: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services


Name        Property
----        --------
Spooler     DependOnService    : {RPCSS, http}
            Description        : @%systemroot%\system32\spoolsv.exe,-2
            DisplayName        : @%systemroot%\system32\spoolsv.exe,-1
            ErrorControl       : 1
            FailureActions     : {16, 14, 0, 0...}
            Group              : SpoolerGroup
            ImagePath          : C:\WINDOWS\System32\spoolsv.exe
            ObjectName         : LocalSystem
            RequiredPrivileges : {SeTcbPrivilege, SeImpersonatePrivilege, ...
            ServiceSidType     : 1
            Start              : 2
            Type               : 27
```

各レジストリキーには、サブキーを含めることもできます。 レジストリキーでを使用する場合 `Get-Item` 、サブキーは表示されません。 `Get-ChildItem`コマンドレットにより、各サブキーのプロパティを含む "スプーラ" キーの子項目が表示されます。 を使用する場合、[親キー] プロパティは表示されません `Get-ChildItem` 。

```
PS C:\> Get-ChildItem -Path HKLM:\SYSTEM\CurrentControlSet\Services\Spooler


    Hive: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Spooler


Name             Property
----             --------
Performance      Close           : PerfClose
                 Collect         : PerfCollect
                 Collect Timeout : 2000
                 Library         : C:\Windows\System32\winspool.drv
                 Object List     : 1450
                 Open            : PerfOpen
                 Open Timeout    : 4000
Security         Security : {1, 0, 20, 128...}
```

`Get-Item`コマンドレットは、現在の場所でも使用できます。 次の例では、"スプーラ" レジストリキーに移動し、アイテムのプロパティを取得します。
ドット `.` は、現在の場所を示すために使用されます。

```
PS C:\> cd HKLM:\System\CurrentControlSet\Services\Spooler
PS HKLM:\SYSTEM\CurrentControlSet\Services\Spooler> Get-Item .

    Hive: HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services

Name             Property
----             --------
Spooler          DependOnService    : {RPCSS, http}
                 Description        : @%systemroot%\system32\spoolsv.exe,-2
...
```

このセクションで説明するコマンドレットの詳細については、次の記事を参照してください。

-[項目](xref:Microsoft.PowerShell.Management.Get-Item) 
- の取得[Get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

## <a name="viewing-registry-key-values"></a>レジストリキー値の表示

レジストリキーの値は、各レジストリキーのプロパティとして格納されます。 `Get-ItemProperty`コマンドレットは、指定した名前を使用してレジストリキーのプロパティを表示します。 結果として、指定したプロパティを含む **Pscustomobject** 生成されます。

次の例では、 `Get-ItemProperty` コマンドレットを使用してすべてのプロパティを表示します。 生成されたオブジェクトを変数に格納すると、目的のプロパティ値にアクセスできるようになります。

```powershell
$p = Get-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Spooler
$p.DependOnService
```

```output
RPCSS
http
```

パラメーターの値を指定 `-Name` すると、指定したプロパティが選択され、 **Pscustomobject** 返されます。  次の例は、パラメーターを使用した場合の出力の違いを示して `-Name` います。

```
PS C:\> Get-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Wbem

BUILD                      : 17134.1
Installation Directory     : C:\WINDOWS\system32\WBEM
MOF Self-Install Directory : C:\WINDOWS\system32\WBEM\MOF
PSPath                     : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Wbem
PSParentPath               : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft
PSChildName                : Wbem
PSDrive                    : HKLM
PSProvider                 : Microsoft.PowerShell.Core\Registry

PS C:\> Get-ItemProperty -Path HKLM:\SOFTWARE\Microsoft\Wbem -Name BUILD

BUILD        : 17134.1
PSPath       : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Wbem
PSParentPath : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft
PSChildName  : Wbem
PSDrive      : HKLM
PSProvider   : Microsoft.PowerShell.Core\Registry
```

PowerShell 5.0 以降では、 `Get-ItemPropertyValue` コマンドレットは、指定したプロパティの値のみを返します。

```powershell
Get-ItemPropertyValue -Path HKLM:\SOFTWARE\Microsoft\Wbem -Name BUILD
```

```output
17134.1
```

このセクションで使用するコマンドレットの詳細については、次の記事を参照してください。

- [Get-ItemProperty](xref:Microsoft.PowerShell.Management.Get-ItemProperty)
- [Get-ItemPropertyValue](xref:Microsoft.PowerShell.Management.Get-ItemProperty)

## <a name="changing-registry-key-values"></a>レジストリキー値の変更

`Set-ItemProperty`コマンドレットでは、レジストリキーの属性を設定します。 次の例では、を使用して `Set-ItemProperty` 、スプーラサービスの開始の種類を手動に変更します。 この例では、コマンドレットを使用して、 **Starttype** を *Automatic* に戻し `Set-Service` ます。

```
PS C:\> Get-Service spooler | Select-Object Name, StartMode

Name    StartType
----    ---------
spooler Automatic

PS C:\> $path = "HKLM:\SYSTEM\CurrentControlSet\Services\Spooler\"
PS C:\> Set-ItemProperty -Path $path -Name Start -Value 3
PS C:\> Get-Service spooler | Select-Object Name, StartMode

Name    StartType
----    ---------
spooler    Manual

PS C:\> Set-Service -Name Spooler -StartupType Automatic
```

各レジストリキーには *既定* 値があります。 レジストリキーの *既定* 値は、またはのいずれかを使用して変更でき `Set-Item` `Set-ItemProperty` ます。

```powershell
Set-ItemProperty -Path HKLM:\SOFTWARE\Contoso -Name "(default)" -Value "one"
Set-Item -Path HKLM:\SOFTWARE\Contoso -Value "two"
```

このセクションで使用するコマンドレットの詳細については、次の記事を参照してください。

- [Set-Item](xref:Microsoft.PowerShell.Management.Set-Item)
- [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)

## <a name="creating-registry-keys-and-values"></a>レジストリキーと値の作成

コマンドレットにより、指定した `New-Item` 名前のレジストリキーが作成されます。
関数を使用することもでき `mkdir` ます。この関数は、内部でコマンドレットを呼び出します `New-Item` 。

```
PS HKLM:\SOFTWARE\> mkdir ContosoCompany

    Hive: HKEY_LOCAL_MACHINE\SOFTWARE

Name                           Property
----                           --------
ContosoCompany
```

コマンドレットを使用して、指定した `New-ItemProperty` レジストリキーに値を作成できます。 次の例では、ContosoCompany レジストリキーに新しい DWORD 値を作成します。

```powershell
$path = "HKLM:\SOFTWARE\ContosoCompany"
New-ItemProperty -Path  -Name Test -Type DWORD -Value 1
```

> [!NOTE]
> その他の許可されている型の値については、この記事の「動的パラメーター」セクションを参照してください。

コマンドレットの使用方法の詳細については、「 [get-itemproperty](xref:Microsoft.PowerShell.Management.New-ItemProperty)」を参照してください。

## <a name="copying-registry-keys-and-values"></a>レジストリキーと値のコピー

**レジストリ** プロバイダーで、コマンドレットを使用して、 `Copy-Item` レジストリキーと値をコピーします。 コマンドレットを使用して、 `Copy-ItemProperty` レジストリ値のみをコピーします。
次のコマンドは、"Contoso" レジストリキーとそのプロパティを、指定された場所 "HKLM:\ ソフトウェア \ Fabrikam" にコピーします。

`Copy-Item` コピー先キーが存在しない場合は、作成します。 コピー先のキーが存在する場合は、コピー先のキーの `Copy-Item` 子項目 (サブキー) として、ソースキーの複製が作成されます。

```powershell
Copy-Item -Path  HKLM:\Software\Contoso -Destination HKLM:\Software\Fabrikam
```

次のコマンドは、コマンドレットを使用して、" `Copy-ItemProperty` Contoso" キーから "Fabrikam" キーに "Server" の値をコピーします。

```powershell
$source = "HKLM:\SOFTWARE\Contoso"
$dest = "HKLM:\SOFTWARE\Fabrikam"
Copy-ItemProperty -Path $source -Destination $dest -Name Server
```

このセクションで使用するコマンドレットの詳細については、次の記事を参照してください。

- [Copy-Item](xref:Microsoft.PowerShell.Management.Copy-Item)
- [Copy-ItemProperty](xref:Microsoft.PowerShell.Management.Copy-ItemProperty)

## <a name="moving-registry-keys-and-values"></a>レジストリキーと値の移動

`Move-Item` `Move-ItemProperty` コマンドレットとコマンドレットは、対応する "Copy" のように動作します。 コピー先が存在する場合は、コピー `Move-Item` 先のキーの下にソースキーを移動します。 コピー先のキーが存在しない場合は、ソースキーが移行先のパスに移動されます。

次のコマンドを実行すると、"Contoso" キーがパス "HKLM:\ \ ソフトウェア \ Fabrikam" に移動します。

```powershell
Move-Item -Path HKLM:\SOFTWARE\Contoso -Destination HKLM:\SOFTWARE\Fabrikam
```

このコマンドは、すべてのプロパティを "HKLM:\ SOFTWARE\ContosoCompany" から "HKLM:\ Fabrikam" に移動します。

```powershell
$source = "HKLM:\SOFTWARE\Contoso"
$dest = "HKLM:\SOFTWARE\Fabrikam"
Move-ItemProperty -Path $source -Destination $dest -Name *
```

このセクションで使用するコマンドレットの詳細については、次の記事を参照してください。

- [Move-Item](xref:Microsoft.PowerShell.Management.Move-Item)
- [Move-ItemProperty](xref:Microsoft.PowerShell.Management.Move-ItemProperty)

## <a name="renaming-registry-keys-and-values"></a>レジストリキーと値の名前を変更する

レジストリキーと値の名前は、ファイルやフォルダーと同じように変更できます。
`Rename-Item` レジストリ値の名前を変更するときに、レジストリキーの名前を変更 `Rename-ItemProperty` します。

```powershell
$path = "HKLM:\SOFTWARE\Contoso"
Rename-ItemProperty -Path $path -Name ContosoTest -NewName FabrikamTest
Rename-Item -Path $path -NewName Fabrikam
```

## <a name="changing-security-descriptors"></a>セキュリティ記述子の変更

およびコマンドレットを使用して、レジストリキーへのアクセスを制限することができ `Get-Acl` `Set-Acl` ます。 次の例では、"HKLM:\ \ ソフトウェア \ Contoso" レジストリキーにフルコントロールを持つ新しいユーザーを追加します。

```powershell
$acl = Get-Acl -Path HKLM:\SOFTWARE\Contoso
$rule = New-Object System.Security.AccessControl.RegistryAccessRule `
("CONTOSO\jsmith", "FullControl", "Allow")
$acl.SetAccessRule($rule)
$acl | Set-Acl -Path HKLM:\SOFTWARE\Contoso
```

その他の例とコマンドレットの使用方法の詳細については、次の記事を参照してください。

- [Get-Acl](xref:Microsoft.PowerShell.Security.Get-Acl)
- [Set-Acl](xref:Microsoft.PowerShell.Security.Set-Acl)

## <a name="removing-and-clearing-registry-keys-and-values"></a>レジストリキーと値の削除と消去

**削除項目** を使用して、含まれる項目を削除できますが、項目に他の項目が含まれている場合は、削除を確認するメッセージが表示されます。 次の例では、キー "HKLM:\ \ ソフトウェア \ Contoso" を削除しようとしています。

```
PS C:\> dir HKLM:\SOFTWARE\Contoso\

    Hive: HKEY_LOCAL_MACHINE\SOFTWARE\Contoso

Name                           Property
----                           --------
ChildKey

PS C:\> Remove-Item -Path HKLM:\SOFTWARE\Contoso

Confirm
The item at HKLM:\SOFTWARE\Contoso has children and the -Recurse
parameter was not specified. If you continue, all children will be removed
with the item. Are you sure you want to continue?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help
(default is "Y"):
```

メッセージを表示せずに含まれる項目を削除するには、パラメーターを指定し `-Recurse` ます。

```powershell
Remove-Item -Path HKLM:\SOFTWARE\Contoso -Recurse
```

"Hklm:\ \ ソフトウェア \ contoso" 内のすべての項目を削除するが、"HKLM:\ $ $ contoso" 自体は削除しない場合は、末尾に円記号を付けた後にワイルドカードを使用し `\` ます。

```powershell
Remove-Item -Path HKLM:\SOFTWARE\Contoso\* -Recurse
```

このコマンドは、"HKLM:\ ContosoTest" レジストリキーから "" レジストリ値を削除します。

```powershell
Remove-ItemProperty -Path HKLM:\SOFTWARE\Contoso -Name ContosoTest
```

`Clear-Item` キーのすべてのレジストリ値をクリアします。 次の例では、"HKLM:\ \ ソフトウェア \ Contoso" レジストリキーからすべての値を削除します。 特定のプロパティのみをクリアするには、を使用し `Clear-ItemProperty` ます。

```
PS HKLM:\SOFTWARE\> Get-Item .\Contoso\

    Hive: HKEY_LOCAL_MACHINE\SOFTWARE

Name           Property
----           --------
Contoso        Server     : {a, b, c}
               HereString : {This is text which contains
               newlines. It also contains "quoted" strings}
               (default)  : 1

PS HKLM:\SOFTWARE\> Clear-Item .\Contoso\
PS HKLM:\SOFTWARE\> Get-Item .\Contoso\

    Hive: HKEY_LOCAL_MACHINE\SOFTWARE

Name                           Property
----                           --------
Contoso
```

その他の例とコマンドレットの使用方法の詳細については、次の記事を参照してください。

- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)
- [Clear-ItemProperty](xref:Microsoft.PowerShell.Management.Clear-ItemProperty)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Remove-ItemProperty](xref:Microsoft.PowerShell.Management.Remove-ItemProperty)

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターは、PowerShell プロバイダーによって追加されるコマンドレットパラメーターであり、プロバイダーに対応したドライブでコマンドレットが使用されている場合にのみ使用できます。

### <a name="type-microsoftwin32registryvaluekind"></a>「<>」と入力します。

レジストリ値のデータ型を確立または変更します。 既定値は `String` (REG_SZ) です。

このパラメーターは [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty) コマンドレットの設計に基づき動作します。 レジストリ ドライブの [Set-Item](xref:Microsoft.PowerShell.Management.Set-Item) コマンドレットでも利用できますが、効果はありません。

|値 | 説明 |
| ---- | ----------- |
| `String` | Null 終了文字列を指定します。 REG_SZ と同じです。 |
| `ExpandString` | 展開されていない null で終わる文字列を指定します |
|                | 環境変数への参照 |
|                | 値が取得されます。 REG_EXPAND_SZ と同じです。 |
| `Binary` | あらゆる形式のバイナリ データを指定します。 REG_BINARY と同じです。 |
| `DWord` | 32 ビットのバイナリ数値を指定します。 REG_DWORD と同じです。 |
| `MultiString` | Null で終わる文字列の配列を指定します。 |
|               | 2つの null 文字。 REG_MULTI_SZ と同じです。 |
| `QWord` | 64ビットのバイナリ数値を指定します。 REG_QWORD と同じです。 |
| `Unknown` | サポートされていないレジストリデータ型を示します。 |
|           | REG_RESOURCE_LIST。 |

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Set-Item](xref:Microsoft.PowerShell.Management.Set-Item)
- [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)

## <a name="using-the-pipeline"></a>パイプラインの使用

プロバイダーのコマンドレットは、パイプラインの入力を受け入れます。 パイプラインを使用すると、1つのコマンドレットから別のプロバイダーのコマンドレットにプロバイダーデータを送信することにより、タスクを簡単に行うことができます。 プロバイダーコマンドレットでパイプラインを使用する方法の詳細については、この記事全体で説明されているコマンドレットリファレンスを参照してください。

## <a name="getting-help"></a>ヘルプの表示

Windows PowerShell 3.0 より、プロバイダー コマンドレットのためにカスタマイズされたヘルプ トピックを取得できます。これはファイル システム ドライブでのプロバイダー コマンドレットの動作を説明します。

ファイルシステムドライブ用にカスタマイズされたヘルプトピックを取得するに `Get-Help` は、ファイルシステムドライブでコマンドを実行するか、 **Path** パラメーターを使用してファイルシステムドライブを指定します。

```powershell
Get-Help Get-ChildItem
```

```powershell
Get-Help Get-ChildItem -Path HKLM:
```

## <a name="see-also"></a>関連項目

 [about_Providers](../About/about_Providers.md)

