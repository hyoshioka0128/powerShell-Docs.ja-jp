---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 11/18/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-service?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-Service
ms.openlocfilehash: 758b0a8ef9a5f65f0e7cfa7f3633086cf9f0445d
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94891770"
---
# New-Service

## 概要
新しい Windows サービスを作成します。

## SYNTAX

```
New-Service [-Name] <String> [-BinaryPathName] <String> [-DisplayName <String>] [-Description <String>]
 [-StartupType <ServiceStartMode>] [-Credential <PSCredential>] [-DependsOn <String[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットにより、 `New-Service` レジストリとサービスデータベースに Windows サービスの新しいエントリが作成されます。 新しいサービスには、サービス中に実行される実行可能ファイルが必要です。

このコマンドレットのパラメーターを使用すると、サービスの表示名、説明、スタートアップの種類と依存関係を設定できます。

## 例

### 例 1: サービスを作成する

```powershell
New-Service -Name "TestService" -BinaryPathName '"C:\WINDOWS\System32\svchost.exe -k netsvcs"'
```

このコマンドは、TestService という名前のサービスを作成します。

### 例 2: 説明、スタートアップの種類、および表示名を含むサービスを作成する

```powershell
$params = @{
  Name = "TestService"
  BinaryPathName = '"C:\WINDOWS\System32\svchost.exe -k netsvcs"'
  DependsOn = "NetLogon"
  DisplayName = "Test Service"
  StartupType = "Manual"
  Description = "This is a test service."
}
New-Service @params
```

このコマンドは、TestService という名前のサービスを作成します。 この例では、のパラメーターを使用して、 `New-Service` 新しいサービスの説明、スタートアップの種類、および表示名を指定しています。

### 例 3: 新しいサービスを表示する

```powershell
Get-CimInstance -ClassName Win32_Service -Filter "Name='testservice'"
```

```Output
ExitCode  : 0
Name      : testservice
ProcessId : 0
StartMode : Auto
State     : Stopped
Status    : OK
```

このコマンドは `Get-CimInstance` 、を使用して、新しいサービスの **Win32_Service** オブジェクトを取得します。 このオブジェクトには開始モードとサービスの説明が含まれています。

### 例 4: サービスを削除する

```powershell
sc.exe delete TestService
# - or -
(Get-CimInstance -Class Win32_Service -Filter "name='TestService'").delete()
```

この例では、TestService サービスを削除する 2 つの方法を示しています。 最初のコマンドは、の delete オプションを使用し `Sc.exe` ます。 2番目のコマンドは、を返す **Win32_Service** オブジェクトの **Delete** メソッドを使用し `Get-CimInstance` ます。

## PARAMETERS

### -ビン名

サービスの実行可能ファイルのパスを指定します。 このパラメーターは必須です。

サービスバイナリファイルへの完全修飾パス。 パスにスペースが含まれている場合は、正しく解釈されるように引用符で囲む必要があります。 たとえば、は `d:\my share\myservice.exe` として指定する必要があり `'"d:\my share\myservice.exe"'` ます。

パスには、自動開始サービスの引数を含めることもできます。 たとえば、「 `'"d:\myshare\myservice.exe arg1 arg2"'` 」のように入力します。 これらの引数は、サービスのエントリポイントに渡されます。

詳細については、 [CreateServiceW](/windows/win32/api/winsvc/nf-winsvc-createservicew) API の **Lpbinaryp name** パラメーターを参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

サービスが [サービスログオンアカウント](/windows/desktop/ad/about-service-logon-accounts)として使用するアカウントを指定します。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成されるような **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、このコマンドレットによってパスワードの入力が求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DependsOn

新しいサービスが依存する他のサービスの名前を指定します。 複数のサービス名を入力するには、コンマを使用して名前を区切ります。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description

サービスの説明を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisplayName

サービスの表示名を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

サービスの名前を指定します。 このパラメーターは必須です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: ServiceName

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -StartupType

サービスのスタートアップの種類を設定します。 このパラメーターの有効値は、次のとおりです。

- **自動** -サービスが開始されているか、システムの起動時にオペレーティングシステムによって開始されました。
  自動的に開始されるサービスが手動で開始されるサービスに依存する場合は、手動で開始されるサービスもシステムの起動時に自動的に開始されます。
- [**無効**]-サービスは無効になっており、ユーザーまたはアプリケーションが開始することはできません。
- **手動** -サービスは、ユーザー、サービスコントロールマネージャー、またはアプリケーションによって手動でのみ開始されます。
- **ブート** -サービスがシステムローダーによって開始されたデバイスドライバーであることを示します。 この値は、デバイス ドライバーに対してのみ有効です。
- **システム** -サービスが、' IOInitSystem () ' 関数によって開始されたデバイスドライバーであることを示します。 この値は、デバイス ドライバーに対してのみ有効です。

 既定値は **Automatic** です。

```yaml
Type: System.ServiceProcess.ServiceStartMode
Parameter Sets: (All)
Aliases:
Accepted values: Boot, System, Automatic, Manual, Disabled

Required: False
Position: Named
Default value: Automatic
Accept pipeline input: False
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

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### System.ServiceProcess.ServiceController

このコマンドレットは、新しいサービスを表すオブジェクトを返します。

## 注

このコマンドレットを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

サービスを削除するには、Sc.exe を使用するか、コマンドレットを使用して `Get-CimInstance` サービスを表す **Win32_Service** オブジェクトを取得し、 **delete** メソッドを使用してサービスを削除します。 `Get-Service`を返すオブジェクトには、delete メソッドがありません。

## 関連リンク

[Get-Service](Get-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)
