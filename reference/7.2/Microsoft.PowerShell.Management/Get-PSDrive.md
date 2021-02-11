---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 05/14/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-psdrive?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSDrive
ms.openlocfilehash: aa83b6318b8e3b07d17cbb3e511bc7ab4aa9a774
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599697"
---
# Get-PSDrive

## 概要
現在のセッションのドライブを取得します。

## SYNTAX

### 名前 (既定値)

```
Get-PSDrive [[-Name] <String[]>] [-Scope <String>] [-PSProvider <String[]>] [<CommonParameters>]
```

### LiteralName

```
Get-PSDrive [-LiteralName] <String[]> [-Scope <String>] [-PSProvider <String[]>] [<CommonParameters>]
```

## Description

`Get-PSDrive`コマンドレットは、現在のセッションのドライブを取得します。 セッション内の特定のドライブを取得することも、すべてのドライブを取得することもできます。

このコマンドレットは、次の種類のドライブを取得します。

- ネットワーク共有にマップされたドライブを含む、コンピューター上の Windows 論理ドライブ。
- PowerShell プロバイダーによって公開されるドライブ (証明書:、関数:、エイリアス: ドライブなど)、Windows PowerShell レジストリプロバイダーによって公開される HKLM: および HKCU: ドライブ。
- New-PSDrive コマンドレットを使用して作成した、セッションで指定された一時ドライブと永続的にマップされたネットワークドライブ。

Windows PowerShell 3.0 以降では、コマンドレットの **Persist** パラメーターを使用して、マップされ `New-PSDrive` たネットワークドライブを作成し、ローカルコンピューターに保存し、他のセッションで使用することができます。 詳細については、「PSDrive」を参照してください。

また、Windows PowerShell 3.0 以降では、外部ドライブがコンピューターに接続されている場合、Windows PowerShell は新しいドライブを表す PSDrive をファイル システムに自動的に追加します。
Windows PowerShell を再起動する必要はありません。 同様に、外部ドライブがコンピューターから取り外された場合、Windows PowerShell は取り外されたドライブを表す PSDrive を自動的に削除します。

## 例

### 例 1: 現在のセッションのドライブを取得する

```
PS C:\> Get-PSDrive

Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
Alias                                  Alias
C                 202.06      23718.91 FileSystem    C:\
Cert                                   Certificate   \
D                1211.06     123642.32 FileSystem    D:\
Env                                    Environment
Function                               Function
HKCU                                   Registry      HKEY_CURRENT_USER
HKLM                                   Registry      HKEY_LOCAL_MACHINE
Variable                               Variable
```

このコマンドは、現在のセッションにあるドライブを取得します。

出力には、ハードドライブ (C:)、CD-ROM ドライブ (D:)、Windows PowerShell プロバイダーによって公開されているドライブ (Alias:、Cert:、Env:、Function:、HKCU:、HKLM:、Variable:) が表示されます。

### 例 2: コンピューターのドライブを取得する

```
PS C:\foo> Get-PSDrive D

Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
D                1211.06     123642.32 FileSystem    D:\
```

このコマンドは、コンピューターの D: ドライブを取得します。 コマンドではドライブ文字の後にコロンを指定していないことに注意してください。

### 例 3: Windows PowerShell ファイルシステムプロバイダーでサポートされているすべてのドライブを取得する

```
PS C:\> Get-PSDrive -PSProvider FileSystem
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
A                                                    A:\
C                 202.06      23718.91 FileSystem    C:\
D                1211.06     123642.32 FileSystem    D:\
G                 202.06        710.91 FileSystem    \\Music\GratefulDead
```

このコマンドは、Windows PowerShell FileSystem プロバイダーによってサポートされているすべてのドライブを取得します。 これには、固定ドライブ、論理パーティション、マップされたネットワークドライブ、および New-PSDrive コマンドレットを使用して作成した一時ドライブが含まれます。

### 例 4: ドライブが Windows PowerShell ドライブ名として使用されているかどうかを確認する

```powershell
if (Get-PSDrive X -ErrorAction SilentlyContinue) {
    Write-Host 'The X: drive is already in use.'
} else {
    New-PSDrive -Name X -PSProvider Registry -Root HKLM:\SOFTWARE
}
```

このコマンドは、X ドライブが Windows PowerShell ドライブ名として既に使用されているかどうかを確認します。
そうでない場合は、コマンドレットを使用して、 `New-PSDrive` HKLM:\ SOFTWARE レジストリキーにマップされている一時ドライブを作成します。

### 例 5: ファイルシステムドライブの種類を比較する

```
PS C:\> Get-PSDrive -PSProvider FileSystem
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
A                                                    A:\
C                 202.06      23718.91 FileSystem    C:\
D                1211.06     123642.32 FileSystem    D:\
G                 202.06        710.91 FileSystem    \\Music\GratefulDead
X                                      Registry      HKLM:\Network

PS C:\> net use
New connections will be remembered.
Status       Local     Remote                    Network
-------------------------------------------------------------------------------
OK           G:        \\Server01\Public         Microsoft Windows Network

PS C:\> [System.IO.DriveInfo]::GetDrives() | Format-Table
Name DriveType DriveFormat IsReady AvailableFreeSpace TotalFreeSpace TotalSize     RootDirectory VolumeLabel
---- --------- ----------- ------- ------------------ -------------- ---------     ------------- -----------
A:\    Network               False                                                 A:\
C:\      Fixed NTFS          True  771920580608       771920580608   988877418496  C:\           Windows
D:\      Fixed NTFS          True  689684144128       689684144128   1990045179904 D:\           Big Drive
E:\      CDRom               False                                                 E:\
G:\    Network NTFS          True      69120000           69120000       104853504 G:\           GratefulDead

PS N:\> Get-CimInstance -Class Win32_LogicalDisk

DeviceID DriveType ProviderName   VolumeName         Size          FreeSpace
-------- --------- ------------   ----------         ----          ---------
A:       4
C:       3                        Windows            988877418496  771926069248
D:       3                        Big!              1990045179904  689684144128
E:       5
G:       4         \\Music\GratefulDead              988877418496  771926069248


PS C:\> Get-CimInstance -Class Win32_NetworkConnection
LocalName RemoteName            ConnectionState Status
--------- ----------            --------------- ------
G:        \\Music\GratefulDead  Connected       OK
```

この例では、に表示されているファイルシステムドライブの種類 `Get-PSDrive` を、他の方法で表示されているものと比較します。 この例は、Windows PowerShell でドライブを表示するさまざまな方法を示しています。また、New-PSDrive コマンドレットを使用して作成されたセッション固有のドライブには、Windows PowerShell でのみアクセスできることが示されています。

最初のコマンドは、を使用して、 `Get-PSDrive` セッション内のすべてのファイルシステムドライブを取得します。 これには、固定ドライブ (C: および D:)、マップされたネットワークドライブ (G:) が含まれます。の **Persist** パラメーターを使用して作成された、 `New-PSDrive` PowerShell ドライブ (T:) `New-PSDrive` **Persist** パラメーターを指定せずにを使用して作成された。

**Net use** コマンドは、Windows のマップされたネットワークドライブを表示します。この場合は、G ドライブのみが表示されます。 によって作成された X: ドライブは表示されません `New-PSDrive` 。 これは、G: ドライブが Music GratefulDead にもマップされていることを示して \\ \\ \\ います。

3 番目のコマンドは、Microsoft .NET Framework **System.IO.DriveInfo** クラスの **GetDrives** メソッドを使用します。 このコマンドは、ドライブ G: を含む Windows ファイルシステムドライブを取得しますが、によって作成されたドライブは取得しません `New-PSDrive` 。

4 番目のコマンドは、`Get-CimInstance` コマンドレットを使用して、**Win32_LogicalDisk** クラスのインスタンスを取得します。 A:、C:、D:、E:、および G: ドライブを返しますが、によって作成されたドライブは返しません `New-PSDrive` 。

最後のコマンドは、`Get-CimInstance` コマンドレットを使用して、**Win32_NetworkConnection** クラスのインスタンスを表示します。 **Net use** と同様に、は、によって作成された永続的な G: ドライブのみを返し `New-PSDrive` ます。

## PARAMETERS

### -LiteralName

ドライブの名前を指定します。

**LiteralName** の値は、入力した内容のまま使用されます。 ワイルドカードとして解釈される文字はありません。 名前にエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲んだ文字はエスケープ シーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

文字列配列として、このコマンドレットが操作で取得するドライブの名前または名前を指定します。
ドライブ名または文字をコロン (:) なしで入力します。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PSProvider

Windows PowerShell プロバイダーを文字列配列として指定します。 このコマンドレットは、このプロバイダーによってサポートされるドライブのみを取得します。 FileSystem、Registry、Certificate など、プロバイダーの名前を入力します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -スコープ

このコマンドレットがドライブを取得するスコープを指定します。

このパラメーターの有効値は、次のとおりです。

- グローバル
- ローカル
- スクリプト
- 現在のスコープに対して相対的な数値 (0 ~ スコープの数。0は現在のスコープ、1はその親)。 既定値は "Local" です。

詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### System.Management.Automation.PSDriveInfo

このコマンドレットは、セッション内のドライブを表すオブジェクトを返します。

## 注

* このコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、`Get-PSProvider` コマンドレットを使用します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。
* New-PSDrive コマンドレットの **Persist** パラメーターを使用して作成されたマップされたネットワークドライブは、ユーザーアカウントに固有です。 [管理者として実行] オプションまたは別のユーザーの資格情報で開始されたセッションで作成したマップ済みネットワークドライブは、明示的な資格情報または現在のユーザーの資格情報を使用せずに開始されたセッションには表示されません。

## 関連リンク

[New-PSDrive](New-PSDrive.md)

[Remove-PSDrive](Remove-PSDrive.md)

[Get-PSProvider](Get-PSProvider.md)

