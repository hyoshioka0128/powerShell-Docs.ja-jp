---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 12/11/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/stop-computer?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Stop-Computer
ms.openlocfilehash: 9ba056a7c85b62ac02137959a5586fdd87d9ceb4
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346311"
---
# Stop-Computer

## 概要
ローカルとリモートのコンピューターを停止 (シャットダウン) します。

## SYNTAX

### すべて

```
Stop-Computer [-WsmanAuthentication <String>] [[-ComputerName] <String[]>]
 [[-Credential] <PSCredential>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

この `Stop-Computer` コマンドレットは、ローカルコンピューターとリモートコンピューターをシャットダウンします。

のパラメーターを使用して、 `Stop-Computer` 認証レベルと代替資格情報を指定し、即時シャットダウンを強制することができます。

## 例

### 例 1: ローカルコンピューターをシャットダウンする

この例では、ローカルコンピューターをシャットダウンします。

```powershell
Stop-Computer -ComputerName localhost
```

### 例 2: 2 台のリモートコンピューターとローカルコンピューターをシャットダウンする

この例では、2台のリモートコンピューターとローカルコンピューターを停止します。

```powershell
Stop-Computer -ComputerName "Server01", "Server02", "localhost"
```

`Stop-Computer`**ComputerName** パラメーターを使用して、2台のリモートコンピューターとローカルコンピューターを指定します。 各コンピューターがシャットダウンされます。

### 例 3: バックグラウンドジョブとしてリモートコンピューターをシャットダウンする

この例では、は `Stop-Computer` 2 台のリモートコンピューター上でバックグラウンドジョブとして実行されます。

バックグラウンド操作では、 `&` `Stop-Computer` バックグラウンドジョブとしてコマンドが実行されます。 詳細については、「 [about_Operators](../microsoft.powershell.core/about/about_operators.md#background-operator-)」を参照してください。

```powershell
$j = Stop-Computer -ComputerName "Server01", "Server02" &
$results = $j | Receive-Job
$results
```

`Stop-Computer`**ComputerName** パラメーターを使用して、2台のリモートコンピューターを指定します。 バックグラウンド操作では、 `&` バックグラウンドジョブとしてコマンドが実行されます。 ジョブオブジェクトは変数に格納され `$j` ます。

変数内のジョブオブジェクト `$j` は、にパイプラインから送信され、 `Receive-Job` ジョブの結果を取得します。 オブジェクトは変数に格納され `$results` ます。 変数は、 `$results` PowerShell コンソールにジョブ情報を表示します。

### 例 4: リモートコンピューターをシャットダウンする

この例では、指定された認証を使用してリモートコンピューターをシャットダウンします。

```powershell
Stop-Computer -ComputerName "Server01" -WsmanAuthentication Kerberos
```

`Stop-Computer`**ComputerName** パラメーターを使用して、リモートコンピューターを指定します。 **WsmanAuthentication** パラメーターは、リモート接続を確立するために Kerberos を使用することを指定します。

### 例 5: ドメイン内のコンピューターをシャットダウンする

この例では、コマンドを実行すると、指定したドメイン内のすべてのコンピューターが即座にシャットダウンされます。

```powershell
$s = Get-Content -Path ./Domain01.txt
$c = Get-Credential -Credential Domain01\Admin01
Stop-Computer -ComputerName $s -Force -Credential $c
```

`Get-Content`**Path** パラメーターを使用して、現在のディレクトリにあるファイルをドメインコンピューターの一覧と共に取得します。 オブジェクトは変数に格納され `$s` ます。

`Get-Credential` は、 **Credential** パラメーターを使用して、ドメイン管理者の資格情報を指定します。 資格情報は変数に格納され `$c` ます。

`Stop-Computer` 変数内の **ComputerName** パラメーターのコンピューターの一覧で指定されたコンピューターをシャットダウンし `$s` ます。 **Force** パラメーターを指定すると、即時シャットダウンが強制されます。 **Credential** パラメーターは、変数に保存されている資格情報を送信し `$c` ます。

## PARAMETERS

### -ComputerName

停止するコンピューターを指定します。 既定値はローカル コンピューターです。

コンマ区切りのリストで、1 台または複数のコンピューターの NETBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。 ローカルコンピューターを指定するには、コンピューター名または localhost を入力します。

このパラメーターは、PowerShell リモート処理に依存しません。 コンピューターがリモートコマンドを実行するように構成されていない場合でも、 **ComputerName** パラメーターを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: CN, __SERVER, Server, IPAddress

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

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

### -Force

コンピューターの即時シャットダウンを強制的に実行します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WsmanAuthentication

このコマンドレットで WSMan プロトコルを使用するときに、ユーザー資格情報の認証に使用されるメカニズムを指定します。 既定値は **Default** です。

このパラメーターの有効値は、次のとおりです。

- 基本
- CredSSP
- Default
- ダイジェスト
- Kerberos
- Negotiate

このパラメーターの値の詳細については、「 [Authenticationmechanism](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!CAUTION]
> ユーザー資格情報が認証対象のリモートコンピューターに渡される Credential Security Service Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Default, Basic, Negotiate, CredSSP, Digest, Kerberos

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットに入力をパイプすることはできません。

## 出力

### なし

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

このコマンドレットは、Windows でのみ機能し、 **Win32_OperatingSystem** WMI クラスの **Win32Shutdown** メソッドを使用します。 この方法では、コンピューターの再起動に使用するユーザーアカウントに対して、 **Seshutdownprivilege** 特権を有効にする必要があります。

## 関連リンク

[Rename-Computer](Rename-Computer.md)

[Restart-Computer](Restart-Computer.md)

[Test-Connection](Test-Connection.md)
