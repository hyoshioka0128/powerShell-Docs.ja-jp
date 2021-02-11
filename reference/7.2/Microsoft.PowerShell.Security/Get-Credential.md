---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 10/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Credential
ms.openlocfilehash: 5630c4d09a2806eb117d005466d4925c1740d3a7
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602754"
---
# Get-Credential

## 概要
ユーザー名とパスワードに基づいて資格情報オブジェクトを取得します。

## SYNTAX

### CredentialSet (既定)

```
Get-Credential [[-Credential] <PSCredential>] [<CommonParameters>]
```

### MessageSet

```
Get-Credential [-Message <String>] [[-UserName] <String>] [-Title <String>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Get-Credential` 指定されたユーザー名とパスワードの資格情報オブジェクトを作成します。 作成した資格情報オブジェクトは、セキュリティ操作で使用できます。

`Get-Credential`コマンドレットは、パスワードまたはユーザー名とパスワードの入力をユーザーに求めます。 **Message** パラメーターを使用すると、コマンドラインプロンプトでカスタマイズされたメッセージを指定できます。

## 例

### 例 1

```powershell
$c = Get-Credential
```

このコマンドは、資格情報オブジェクトを取得し、変数に保存し `$c` ます。

コマンドを入力すると、ユーザー名とパスワードを入力するように求められます。 要求された情報を入力すると、コマンドレットは、ユーザーの資格情報を表す **PSCredential** オブジェクトを作成し、変数に保存し `$c` ます。

このオブジェクトは、**Credential** パラメーターを持つコマンドレットなど、ユーザー認証を必要とするコマンドレットへの入力として使用できます。 ただし、PowerShell でインストールされる一部のプロバイダーでは、 **Credential** パラメーターはサポートされていません。

### 例 2

```powershell
$c = Get-Credential
Get-CimInstance Win32_DiskDrive -ComputerName Server01 -Credential $c
```

これらのコマンドは、Windows Management Instrumentation (WMI) を使用してコンピューターを管理できるように、コマンドレットから返される資格情報オブジェクトを使用して `Get-Credential` リモートコンピューター上のユーザーを認証します。

最初のコマンドは、資格情報オブジェクトを取得し、変数に保存し `$c` ます。 2番目のコマンドは、コマンドで credential オブジェクトを使用し `Get-CimInstance` ます。 このコマンドは、Server01 コンピューターのディスク ドライブに関する情報を取得します。

### 例 3

```powershell
Get-CimInstance Win32_BIOS -ComputerName Server01 -Credential (Get-Credential -Credential Domain01\User01)
```

このコマンドは、コマンドにコマンドを含める方法を示して `Get-Credential`  `Get-CimInstance` います。

このコマンドは、コマンドレットを使用して、 `Get-CimInstance` Server01 コンピューター上の BIOS に関する情報を取得します。 **Credential パラメーターを** 使用し `Get-Credential` て、 **credential** パラメーターの値として user、domain01\user01」、および command を認証します。

### 例 4

```powershell
$c = Get-Credential -credential User01
$c.Username
User01
```

この例では、ドメイン名なしのユーザー名を含む資格情報を作成しています。

最初のコマンドは、ユーザー名が User01 の資格情報を取得し、変数に格納し `$c` ます。
2 番目のコマンドは、結果として得られる資格情報オブジェクトの **Username** プロパティの値を表示します。

### 例 5

```powershell
$Credential = $host.ui.PromptForCredential("Need credentials", "Please enter your user name and password.", "", "NetBiosUserName")
```

このコマンドは、 **PromptForCredential** メソッドを使用して、ユーザーにユーザー名とパスワードを入力するよう求めます。 このコマンドは、結果として得られる資格情報を変数に保存し `$Credential` ます。

**Promptforcredential** メソッドは、コマンドレットを使用する代わりに使用し `Get-Credential` ます。 **Promptforcredential** を使用する場合は、プロンプトに表示されるキャプション、メッセージ、およびユーザー名を指定できます。

詳細については、SDK の [Promptforcredential](/dotnet/api/system.management.automation.host.pshostuserinterface.promptforcredential) のドキュメントを参照してください。

### 例 6

この例では、ユーザーにメッセージを表示せずにを返すオブジェクトと同じ資格情報オブジェクトを作成する方法を示し `Get-Credential` ます。 このメソッドではプレーンテキスト パスワードが必要ですが、企業によってはセキュリティ標準に違反する可能性があります。

```powershell
$User = "Domain01\User01"
$PWord = ConvertTo-SecureString -String "P@sSwOrd" -AsPlainText -Force
$Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
```

最初のコマンドは、ユーザーアカウント名をパラメーターに保存し `$User` ます。 この値は、"Domain\User" または "ComputerName\User" の形式である必要があります。

2番目のコマンドは、コマンドレットを使用して、 `ConvertTo-SecureString` プレーンテキストパスワードからセキュリティで保護された文字列を作成します。 このコマンドは、**AsPlainText** パラメーターを使用して、文字列がプレーンテキストであることを示します。また、**Force** パラメーターを使用して、プレーンテキストの使用に伴うリスクを理解しているかどうかを確認します。

3番目のコマンドは、コマンドレットを使用して、 `New-Object` 変数と変数の値から **PSCredential** オブジェクトを作成し `$User` `$PWord` ます。

### 例 7

```powershell
Get-Credential -Message "Credential are required for access to the \\Server1\Scripts file share." -User Server01\PowerUser
```

```Output
PowerShell Credential Request
Credential are required for access to the \\Server1\Scripts file share.
Password for user Server01\PowerUser:
```

このコマンドは、コマンドレットの **Message** パラメーターと **UserName** パラメーターを使用し `Get-Credential` ます。 このコマンド形式は、共有スクリプトおよび共有関数向けに設計されています。 この例では、メッセージはユーザーに、資格情報が必要な理由を伝え、要求が正当であるという安心感を与えています。

### 例 8

```powershell
Invoke-Command -ComputerName Server01 {Get-Credential Domain01\User02}
```

```Output
PowerShell Credential Request : PowerShell Credential Request
Warning: This credential is being requested by a script or application on the SERVER01 remote computer. Enter your credentials only if you
 trust the remote computer and the application or script requesting it.

Enter your credentials.
Password for user Domain01\User02: ***************

PSComputerName     : Server01
RunspaceId         : 422bdf52-9886-4ada-ab2f-130497c6777f
PSShowComputerName : True
UserName           : Domain01\User01
Password           : System.Security.SecureString
```

このコマンドは、Server01 リモート コンピューターから資格情報を取得します。 コマンドは、コマンドレットを使用して、 `Invoke-Command` `Get-Credential` リモートコンピューター上でコマンドを実行します。 出力には、認証プロンプトに含まれるリモートセキュリティメッセージが表示され `Get-Credential` ます。

## PARAMETERS

### -Credential

**User01** や **domain01\user01」** など、資格情報のユーザー名を指定します。 パラメーター名は `-Credential` 省略可能です。

コマンドを送信し、ユーザー名を指定すると、パスワードの入力を求めるメッセージが表示されます。 このパラメーターを省略すると、ユーザー名とパスワードを入力するように求められます。

PowerShell 3.0 以降では、ドメインを使用せずにユーザー名を入力すると、 `Get-Credential` 名前の前に円記号が挿入されなくなりました。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: CredentialSet
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -メッセージ

認証プロンプトに表示されるメッセージを指定します。 このパラメーターは、関数またはスクリプトで使用するように設計されています。 メッセージを使用して、資格情報が必要な理由と使用方法をユーザーに説明できます。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: MessageSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Title

コンソールで認証プロンプトのタイトル行のテキストを設定します。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: MessageSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UserName

ユーザー名を指定します。 認証プロンプトで、ユーザー名に対応するパスワードの入力が要求されます。 既定では、ユーザー名は空白で、認証プロンプトでユーザー名とパスワードの両方が要求されます。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: MessageSet
Aliases:

Required: False
Position: 1
Default value: None (blank)
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システム.... PSCredential

`Get-Credential` 資格情報オブジェクトを返します。

## 注

 `Get-Credential` ユーザー認証を要求するコマンドレットでを作成する PSCredential オブジェクトを使用できます。たとえば、 **Credential** パラメーターを使用することができます。

**Credential** パラメーターは、PowerShell と共にインストールされるすべてのプロバイダーでサポートされていません。
PowerShell 3.0 以降では、 `Get-Content` コマンドレットやコマンドレットなど、select コマンドレットでサポートされてい `New-PSDrive` ます。

## 関連リンク

[PromptForCredential](/dotnet/api/system.management.automation.host.pshostuserinterface.promptforcredential)
