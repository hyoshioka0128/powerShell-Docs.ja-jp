---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 10/22/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-psdrive?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-PSDrive
ms.openlocfilehash: 54b65a636fe05ed82d4f022b5bb8cbf8acaf7bab
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602756"
---
# New-PSDrive

## 概要
一時的および永続的にマップされたネットワーク ドライブを作成します。

## SYNTAX

### すべて

```
New-PSDrive [-Name] <String> [-PSProvider] <String> [-Root] <String> [-Description <String>]
 [-Scope <String>] [-Persist] [-Credential <PSCredential>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`New-PSDrive`コマンドレットでは、ネットワークドライブ、ローカルコンピューター上のディレクトリ、レジストリキー、およびリモートコンピューター上のファイルシステムの場所に関連付けられている固定の Windows マップ済みネットワークドライブなど、データストア内の場所にマップまたは関連付けられている一時ドライブと永続ドライブを作成します。

一時ドライブは、現在の PowerShell セッションと、現在のセッションで作成したセッションにのみ存在します。 PowerShell で有効な任意の名前を持つことができ、任意のローカルまたはリモートリソースにマップできます。 割り当てられたネットワークドライブの場合と同様に、一時 PowerShell ドライブを使用して、関連付けられているデータストア内のデータにアクセスできます。 を使用してドライブの場所を変更 `Set-Location` し、またはを使用してドライブの内容にアクセスでき `Get-Item` `Get-ChildItem` ます。

一時ドライブは PowerShell のみで認識されるため、ファイルエクスプローラー、Windows Management Instrumentation (WMI)、コンポーネントオブジェクトモデル (COM)、Microsoft .NET フレームワークを使用したり、 **NET use** などのツールでアクセスしたりすることはできません。

PowerShell 3.0 では、次の機能がに追加されました `New-PSDrive` 。

- 割り当て済みのネットワーク ドライブ の **Persist** パラメーターを使用して、 `New-PSDrive` Windows マップ済みネットワークドライブを作成できます。 一時 PowerShell ドライブとは異なり、Windows マップ済みネットワークドライブはセッション固有ではありません。 これらのファイルは Windows に保存され、エクスプローラーや **net use** などの標準の windows ツールを使用して管理できます。 マップ済みネットワーク ドライブは、ドライブ文字名を持ち、リモート ファイル システムの場所に接続されている必要があります。 コマンドのスコープがローカルである場合、ドットソーシングではない場合、 **persist** パラメーターでは、コマンドが実行されているスコープを超える **PSDrive** の作成は保持されません。 スクリプト内で実行 `New-PSDrive` していて、ドライブを無期限に保持する場合は、スクリプトをドットで変換する必要があります。 最適な結果を得るために、新しいドライブを無期限に保持するには、 **スコープ** パラメーターをコマンドに追加し、その値を **Global** に設定します。 ドットソーシングの詳細については、「 [about_Scripts](../Microsoft.PowerShell.Core/About/about_Scripts.md#script-scope-and-dot-sourcing)」を参照してください。
- 外部ドライブ。 外部ドライブがコンピューターに接続されている場合、PowerShell は新しいドライブを表す **PSDrive** をファイルシステムに自動的に追加します。 PowerShell を再起動する必要はありません。 同様に、外部ドライブがコンピューターから切断されると、PowerShell は、削除されたドライブを表す **PSDrive** を自動的に削除します。
- UNC (汎用名前付け規則) パスの資格情報。

**ルート** パラメーターの値がのような UNC パスである場合 `\\Server\Share` は、 **credential** パラメーターの値で指定された資格情報を使用して **PSDrive** が作成されます。 そうしないと、新しいファイルシステムドライブを作成しているときに **資格情報** が有効になりません。

一部のコードサンプルでは、スプラッティングを使用して、行の長さを減らし、読みやすさを向上させます。 詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md)」を参照してください。

## 例

### 例 1: ネットワーク共有にマップされた一時ドライブを作成する

この例では、ネットワーク共有にマップされている一時 PowerShell ドライブを作成します。

```powershell
New-PSDrive -Name "Public" -PSProvider "FileSystem" -Root "\\Server01\Public"
```

```Output
Name       Provider      Root
----       --------      ----
Public     FileSystem    \\Server01\Public
```

`New-PSDrive`**Name** パラメーターを使用して、という名前の powershell ドライブを指定 `Public` し、 **psprovider** パラメーターを使用して powershell プロバイダーを指定し `FileSystem` ます。 **Root** パラメーターは、ネットワーク共有の UNC パスを指定します。

PowerShell セッションの内容を表示するには、次のようにします。 `Get-ChildItem -Path Public:`

### 例 2: ローカルディレクトリにマップされた一時ドライブを作成する

この例では、ローカルコンピューター上のディレクトリへのアクセスを提供する一時 PowerShell ドライブを作成します。

```powershell
$parameters = @{
    Name = "MyDocs"
    PSProvider = "FileSystem"
    Root = "C:\Users\User01\Documents"
    Description = "Maps to my My Documents folder."
}
New-PSDrive @parameters
```

```Output
Name        Provider      Root
----        --------      ----
MyDocs      FileSystem    C:\Users\User01\Documents
```

スプラッティングは、パラメーターのキーと値を作成します。 **Name** パラメーターは、ドライブ名 **mydocs** を指定します。 **Psprovider** パラメーターは、PowerShell プロバイダーを指定し `FileSystem` ます。 **Root** ローカルコンピューターのディレクトリを指定します。 **Description** パラメーターはドライブの目的を説明します。 `New-PSDrive` では、splatted パラメーターを使用してドライブを作成し `MyDocs` ます。

PowerShell セッションの内容を表示するには、次のようにします。 `Get-ChildItem -Path MyDocs:`

### 例 3: レジストリキーの一時ドライブを作成する

この例では、レジストリキーへのアクセスを提供する一時 PowerShell ドライブを作成します。 この例では、レジストリキーにマップされている MyCompany という名前のドライブを作成し `HKLM:\Software\MyCompany` ます。

```powershell
New-PSDrive -Name "MyCompany" -PSProvider "Registry" -Root "HKLM:\Software\MyCompany"
```

```Output
Name           Provider      Root
----           --------      ----
MyCompany      Registry      HKLM:\Software\MyCompany
```

`New-PSDrive`**Name** パラメーターを使用して、という名前の powershell ドライブを指定 `MyCompany` し、 **psprovider** パラメーターを使用して powershell プロバイダーを指定し `Registry` ます。 **Root** パラメーターは、レジストリの場所を指定します。

PowerShell セッションの内容を表示するには、次のようにします。 `Get-ChildItem -Path MyCompany:`

### 例 4: 資格情報を使用して、永続的にマップされたネットワークドライブを作成する

この例では、ドメインサービスアカウントの資格情報で認証されたネットワークドライブをマップします。
資格情報を格納する **PSCredential** オブジェクトと、パスワードを **SecureString** として保存する方法の詳細については、 **Credential** パラメーターの説明を参照してください。

```powershell
$cred = Get-Credential -Credential Contoso\ServiceAccount
New-PSDrive -Name "S" -Root "\\Server01\Scripts" -Persist -PSProvider "FileSystem" -Credential $cred
Net Use
```

```Output
Status       Local     Remote                    Network
---------------------------------------------------------
OK           S:        \\Server01\Scripts        Microsoft Windows Network
```

> [!NOTE]
> スクリプトで上記のスニペットを使用する場合は、 **スコープ** パラメーターの値を "Global" に設定して、ドライブが現在のスコープの外部に保持されるようにしてください。

変数には、 `$cred` サービスアカウントの資格情報を含む **PSCredential** オブジェクトが格納されます。 `Get-Credential`**SecureString** に格納されているパスワードを入力するように求めるメッセージが表示されます。

`New-PSDrive` 複数のパラメーターを使用して、マップされたネットワークドライブを作成します。 **名前** `S` Windows が受け入れるドライブ文字を指定します。 および **Root** `\\Server01\Scripts` は、リモートコンピューター上の場所として定義されます。 **Persist** は、ローカルコンピューターに保存されている Windows マップ済みネットワークドライブを作成します。 **Psprovider** はプロバイダーを指定し `FileSystem` ます。 **資格情報** は変数を使用して、 `$cred` 認証用のサービスアカウント資格情報を取得します。

マップされたドライブは、PowerShell セッション、エクスプローラー、および **net use** などのツールを使用して、ローカルコンピューターで表示できます。 PowerShell セッションの内容を表示するには、次のようにします。 `Get-ChildItem -Path S:`

### 例 5: 永続的なドライブと一時ドライブを作成する

この例では、永続的にマップされたネットワークドライブと、同じネットワーク共有にマップされている一時 PowerShell ドライブとの違いを示します。

PowerShell セッションを閉じた後で新しいセッションを開くと、一時的なドライブは使用できません `PSDrive:` が、永続的 `X:` なドライブは使用できます。 ネットワークドライブのマップに使用する方法を決定するときは、ドライブの使用方法を検討してください。 たとえば、永続化する必要があるかどうか、および他の Windows 機能に対してドライブを表示する必要があるかどうかなどです。

```powershell
# Create a temporary PowerShell drive called PSDrive:
# that's mapped to the \\Server01\Public network share.
New-PSDrive -Name "PSDrive" -PSProvider "FileSystem" -Root "\\Server01\Public"

# Use the Persist parameter of New-PSDrive to create the X: mapped network drive,
# which is also mapped to the \\Server01\Public network share.
New-PSDrive -Persist -Name "X" -PSProvider "FileSystem" -Root "\\Server01\Public"

# Now, you can use the Get-PSDrive drive cmdlet to examine the two drives.
# The drives appear to be the same, although the network share name appears only
# in the root of the PSDrive: drive.
Get-PSDrive -Name "PSDrive", "X"
```

```Output
Name       Provider      Root
----       --------      ----

PsDrive    FileSystem    \\Server01\public
X          FileSystem    X:\
```

```powershell
# Get-Member cmdlet shows that the drives have the same object type,
# System.Management.Automation.PSDriveInfo.
Get-PSDrive "PSDrive", "x" | Get-Member
```

```Output
TypeName: System.Management.Automation.PSDriveInfo

Name                MemberType   Definition
----                ----------   ----------
CompareTo           Method       System.Int32 CompareTo(PSDriveInfo drive),
Equals              Method       System.Boolean Equals(Object obj),
GetHashCode         Method       System.Int32 GetHashCode()
...
```

```powershell
# Net Use and Get-CimInstance for the Win32_LogicalDisk class,
# and Win32_NetworkConnection class find only the persistent X: drive.
# PowerShell temporary drives are known only to PowerShell.
Net Use
Get-CimInstance Win32_LogicalDisk | Format-Table -Property DeviceID
Get-CimInstance Win32_NetworkConnection
```

```Output
Status       Local     Remote                    Network
--------------------------------------------------------
OK           X:        \\contoso-pc\data         Microsoft Windows Network

deviceid
--------
C:
D:
X:

LocalName    RemoteName              ConnectionState          Status
---------    ----------              ---------------          ------
X:           \\products\public       Disconnected             Unavailable
```

## PARAMETERS

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

このアクションを実行するアクセス許可を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。

PowerShell 3.0 以降、 **ルート** パラメーターの値が UNC パスの場合は、資格情報を使用してファイルシステムドライブを作成できます。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Description

ドライブの簡単な説明テキストを指定します。 任意の文字列を入力します。

セッションのすべてのドライブの説明を表示するには、「」を参照してください `Get-PSDrive | Format-Table Name, Description` 。

特定のドライブの説明を表示するには、「」と入力 `(Get-PSDrive <DriveName>).Description` します。

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

### -Name

新しいドライブの名前を指定します。 永続的にマップされたネットワークドライブの場合は、ドライブ文字を使用します。 一時 PowerShell ドライブの場合、ドライブ文字に制限されていないので、任意の有効な文字列を使用します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Persist

このコマンドレットによって、Windows マップ済みネットワークドライブが作成されることを示します。 **Persist** パラメーターは Windows でのみ使用できます。

マップ済みネットワーク ドライブは、ローカル コンピューターの Windows に保存されます。 これらは永続的で、セッション固有ではなく、エクスプローラーや他のツールで表示および管理できます。

ドットソーシングを使用せずにコマンドのスコープをローカルに指定した場合、 **persist** パラメーターでは、コマンドを実行するスコープを超える **PSDrive** の作成は保持されません。 スクリプト内でを実行 `New-PSDrive` し、新しいドライブを無期限に保持する場合は、スクリプトをドットで変換する必要があります。 最適な結果を得るために、新しいドライブを強制的に永続化するには、**スコープ** パラメーターの値として **Global** を指定し、コマンドに **persist** を含めます。

ドライブ名は、やなどの文字である必要が `D` あり `E` ます。 **ルート** パラメーターの値は、別のコンピューターの UNC パスである必要があります。 **Psprovider** パラメーターの値はである必要があり `FileSystem` ます。

Windows マップ済みネットワーク ドライブを切断するには、`Remove-PSDrive` コマンドレットを使用します。 Windows マップ済みネットワーク ドライブを切断すると、マッピングは現在のセッションから削除されるだけでなく、コンピューターから完全に削除されます。

マップ済みネットワーク ドライブは、ユーザー アカウントに固有です。 昇格されたセッションまたは別のユーザーの資格情報を使用するセッションで作成されたマップされたドライブは、別の資格情報を使用して開始されたセッションでは

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PSProvider

この種類のドライブをサポートする PowerShell プロバイダーを指定します。

たとえば、ドライブがネットワーク共有またはファイルシステムディレクトリに関連付けられている場合、PowerShell プロバイダーはに `FileSystem` なります。 ドライブがレジストリキーに関連付けられている場合、プロバイダーは `Registry` です。

一時 PowerShell ドライブは、任意の PowerShell プロバイダーに関連付けることができます。 マップされたネットワークドライブは、プロバイダーにのみ関連付けることができ `FileSystem` ます。

PowerShell セッションのプロバイダーの一覧を表示するには、コマンドレットを使用し `Get-PSProvider` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ルート

PowerShell ドライブをマップするデータストアの場所を指定します。

たとえば、のようなネットワーク共有、などの `\\Server01\Public` ローカルディレクトリ、レジストリキー (など) を指定し `C:\Program Files` `HKLM:\Software\Microsoft` ます。

一時 PowerShell ドライブは、サポートされている任意のプロバイダードライブ上のローカルまたはリモートの場所に関連付けることができます。 マップ済みネットワーク ドライブは、リモート コンピューター上のファイル システムの場所だけに関連付けることができます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -スコープ

ドライブのスコープを指定します。 このパラメーターに指定できる値は、 **Global**、 **Local**、および **Script**、または現在のスコープを基準とする数値です。 スコープは、スコープの数から0までの範囲です。 現在のスコープ番号は0で、その親は1です。 詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Local
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットに入力をパイプラインすることはできません。

## 出力

### System.Management.Automation.PSDriveInfo

## 注

`New-PSDrive` は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、を使用 `Get-PSProvider` します。 プロバイダーの詳細については、「 [about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

マップ済みネットワーク ドライブは、ユーザー アカウントに固有です。 昇格されたセッションまたは別のユーザーの資格情報を使用するセッションで作成されたマップされたドライブは、別の資格情報を使用して開始されたセッションでは

## 関連リンク

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Get-Credential](../Microsoft.PowerShell.Security/Get-Credential.md)

[Get-PSDrive](Get-PSDrive.md)

[Remove-PSDrive](Remove-PSDrive.md)

