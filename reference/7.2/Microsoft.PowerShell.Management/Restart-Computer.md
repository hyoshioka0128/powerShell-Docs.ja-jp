---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/17/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-computer?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Restart-Computer
ms.openlocfilehash: 0e6df859a19f2281cc835b725fbc52c232042e56
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600596"
---
# Restart-Computer

## 概要
ローカルコンピューターとリモートコンピューターのオペレーティングシステムを再起動します。

## SYNTAX

### DefaultSet (既定値)

```
Restart-Computer [-WsmanAuthentication <String>] [[-ComputerName] <String[]>]
 [[-Credential]<PSCredential>] [-Force] [-Wait] [-Timeout <Int32>] [-For <WaitForServiceTypes>]
 [-Delay <Int16>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットにより、 `Restart-Computer` ローカルコンピューターとリモートコンピューターのオペレーティングシステムが再起動されます。

のパラメーターを使用して、 `Restart-Computer` 再起動操作を実行し、認証レベルと代替資格情報を指定し、同時に実行される操作を制限し、即時再起動を強制することができます。

Windows PowerShell 3.0 以降では、再起動が完了するのを待ってから、次のコマンドを実行します。 待機中のタイムアウトとクエリ間隔を指定し、再起動されたコンピューターで特定のサービスが利用可能になるまで待ちます。 この機能により、スクリプトや関数でを使用することが実用的になり `Restart-Computer` ます。

## 例

### 例 1: ローカルコンピューターを再起動する

`Restart-Computer` ローカルコンピューターを再起動します。

```powershell
Restart-Computer
```

### 例 2: 複数のコンピューターを再起動する

`Restart-Computer` リモートコンピューターとローカルコンピューターを再起動できます。 **ComputerName** パラメーターは、コンピューター名の配列を受け入れます。

```powershell
Restart-Computer -ComputerName Server01, Server02, localhost
```

### 例 3: テキストファイルからコンピューター名を取得する

`Restart-Computer` テキストファイルからコンピューター名の一覧を取得し、コンピューターを再起動します。 **ComputerName** パラメーターが指定されていません。 ただし、これは最初の位置パラメーターなので、パイプラインで送信されるテキストファイルのコンピューター名を受け入れます。

```powershell
Get-Content -Path C:\Domain01.txt | Restart-Computer
```

`Get-Content`**Path** パラメーターを使用して、 **Domain01.txt** テキストファイルからコンピューター名の一覧を取得します。 コンピューター名は、パイプラインによって送信されます。 `Restart-Computer` 各コンピューターを再起動します。

### 例 4: テキストファイルに一覧表示されているコンピューターを強制的に再起動する

この例では、ファイルに一覧表示されているコンピューターを直ちに再起動 `Domain01.txt` します。 テキストファイルからのコンピューター名は、変数に格納されます。 **Force** パラメーターを指定すると、即時再起動が強制されます。

```powershell
$Names = Get-Content -Path C:\Domain01.txt
$Creds = Get-Credential
Restart-Computer -ComputerName $Names -Credential $Creds -Force
```

`Get-Content`**Path** パラメーターを使用して、 **Domain01.txt** テキストファイルからコンピューター名の一覧を取得します。 コンピューター名は変数に格納され `$Names` ます。 `Get-Credential` ユーザー名とパスワードの入力を求め、変数に値を格納し `$Creds` ます。 `Restart-Computer` では、 **ComputerName** パラメーターと **Credential** パラメーターをその変数と共に使用します。 **Force** パラメーターを指定すると、各コンピューターが直ちに再起動されます。

### 例 6: リモートコンピューターを再起動し、PowerShell を待機する

`Restart-Computer` リモートコンピューターを再起動してから、再起動されたコンピューターで PowerShell が使用できるようになるまで最大5分 (300 秒) 待機してから続行します。

```powershell
Restart-Computer -ComputerName Server01 -Wait -For PowerShell -Timeout 300 -Delay 2
```

`Restart-Computer`**ComputerName** パラメーターを使用して **Server01** を指定します。 **Wait** パラメーターは、再起動が完了するまで待機します。 **の** は、PowerShell がリモートコンピューターでコマンドを実行できることを指定します。 **Timeout** パラメーターは、5分間の待機を指定します。 **Delay** パラメータは、リモートコンピュータを再起動するかどうかを確認するために、2秒ごとにクエリを行います。

### 例 7: WsmanAuthentication を使用してコンピューターを再起動する

`Restart-Computer`**WsmanAuthentication** メカニズムを使用してリモートコンピューターを再起動します。
Kerberos 認証は、現在のユーザーがリモートコンピューターを再起動するアクセス許可を持っているかどうかを判断します。 詳細については、「 [Authenticationmechanism](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

```powershell
Restart-Computer -ComputerName Server01 -WsmanAuthentication Kerberos
```

`Restart-Computer`**ComputerName** パラメーターを使用して、リモートコンピューター **Server01** を指定します。
**WsmanAuthentication** パラメーターは、認証方法を **Kerberos** として指定します。

## PARAMETERS

### -ComputerName

1つのコンピューター名、またはコンピューター名のコンマ区切りの配列を指定します。 `Restart-Computer` パイプラインまたは変数から **ComputerName** オブジェクトを受け入れます。

リモート コンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。 ローカルコンピューターを指定するには、コンピューター名、ドット `.` 、または localhost を入力します。

このパラメーターは、PowerShell リモート処理に依存しません。 コンピューターがリモートコマンドを実行するように構成されていない場合でも、 **ComputerName** パラメーターを使用できます。

**ComputerName** パラメーターが指定されていない場合、は `Restart-Computer` ローカルコンピューターを再起動します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: CN, __SERVER, Server, IPAddress

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Credential

このアクションを実行するアクセス許可を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -Delay

クエリの頻度を秒単位で指定します。 PowerShell は、For パラメーターによって指定されたサービスに **対し** てクエリを実行し、コンピューターの再起動後にサービスを使用できるかどうかを確認します。

このパラメーターは、 **Wait** パラメーターと **For** パラメーターと共に使用する場合にのみ有効です。

このパラメーターは Windows PowerShell 3.0 で導入されました。

**Delay** パラメーターが指定されていない場合、で `Restart-Computer` は5秒の遅延が使用されます。

```yaml
Type: System.Int16
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -の場合

コンピューターの再起動後に、指定されたサービスまたは機能が使用可能になるのを待機するときの PowerShell の動作を指定します。 このパラメーターは、 **Wait** パラメーターでのみ有効です。

このパラメーターの有効値は、次のとおりです。

- **既定値**: PowerShell が再開されるまで待機します。
- **Powershell**: コンピューター上の powershell リモートセッションでコマンドを実行できます。
- **WMI**: コンピューターの Win32_ComputerSystem クエリへの応答を受信します。
- **WinRM**: ws-management を使用して、コンピューターへのリモートセッションを確立できます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: Microsoft.PowerShell.Commands.WaitForServiceTypes
Parameter Sets: (All)
Aliases:
Accepted values: Wmi, WinRM, PowerShell

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

コンピューターの即時再起動を強制します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: f

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -タイムアウト

待機期間を秒単位で指定します。 タイムアウトが経過すると、 `Restart-Computer` コンピューターが再起動されていない場合でも、はコマンドプロンプトに戻ります。

**Timeout** パラメーターは、 **Wait** パラメーターと共にのみ有効です。 **Timeout** は、 **待機** パラメーターの無期限の待機期間をオーバーライドします。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: TimeoutSec

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Wait

`Restart-Computer` PowerShell プロンプトを非表示にし、コンピューターが再起動されるまでパイプラインをブロックします。 このパラメーターをスクリプトで使用すると、コンピューターを再起動し、再起動が完了したときに処理を続行できます。

**Wait** パラメーターは、コンピューターが再起動するまで無期限に待機します。 **タイムアウト** を使用してタイミングを調整し、 **For** および **Delay** パラメーターを使用して、再起動されたコンピューターで特定のサービスが利用可能になるまで待機することができます。

ローカルコンピューターを再起動する場合、 **Wait** パラメーターは無効です。 **ComputerName** パラメーターの値にリモートコンピューターとローカルコンピューターの名前が含まれている場合、は、 `Restart-Computer` ローカルコンピューターの **待機** に対して終了しないエラーを生成しますが、リモートコンピューターの再起動を待機します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WsmanAuthentication

ユーザー資格情報の認証に使用するメカニズムを指定します。 このパラメーターは Windows PowerShell 3.0 で導入されました。

このパラメーターに指定できる値は、 **Basic**、 **CredSSP**、 **Default**、 **Digest**、 **Kerberos**、および **Negotiate** です。

詳細については、「 [Authenticationmechanism](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!WARNING]
> ユーザー資格情報が認証対象のリモートコンピューターに渡される Credential Security Service Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Basic, CredSSP, Default, Digest, Kerberos, Negotiate

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

実行前に確認メッセージを表示 `Restart-Computer` します。

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

### -WhatIf

が実行された場合に何が起こるかを示し `Restart-Computer` ます。 `Restart-Computer`コマンドレットは実行されません。

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

`Restart-Computer` パイプラインまたは変数のコンピューター名を受け入れます。

## 出力

### なし

`Restart-Computer` 出力を生成しません。

## 注

- Windows では、 `Restart-Computer` Windows Management Instrumentation (WMI) [Win32_OperatingSystem](/windows/desktop/CIMWin32Prov/win32-operatingsystem)クラスの[Win32Shutdown メソッド](/windows/desktop/CIMWin32Prov/win32shutdown-method-in-class-win32-operatingsystem)を使用します。 この方法では、コンピューターの再起動に使用するユーザーアカウントに対して、 **Seshutdownprivilege** 特権を有効にする必要があります。
- Linux および Mac OS では、は `Restart-Computer` bash ツールを使用し `/sbin/shutdown` ます。

## 関連リンク

[Windows リモート管理について](/windows/desktop/WinRM/about-windows-remote-management)

[Get-Credential](../Microsoft.PowerShell.Security/Get-Credential.md)

[WS-Management Protocol (WS-Management プロトコル)](/windows/desktop/WinRM/ws-management-protocol)
