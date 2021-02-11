---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/26/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/set-pssessionconfiguration?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-PSSessionConfiguration
ms.openlocfilehash: 788e7b9d261a862658f4cf7453f35228dd3ffab6
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94345852"
---
# Set-PSSessionConfiguration

## 概要
登録済みのセッション構成のプロパティを変更します。

## SYNTAX

### NameParameterSet (既定値)

```
Set-PSSessionConfiguration [-Name] <String> [-ApplicationBase <String>] [-RunAsCredential <PSCredential>]
 [-ThreadOptions <PSThreadOptions>] [-AccessMode <PSSessionConfigurationAccessMode>] [-UseSharedProcess]
 [-StartupScript <String>] [-MaximumReceivedDataSizePerCommandMB <Double>]
 [-MaximumReceivedObjectSizeMB <Double>] [-SecurityDescriptorSddl <String>] [-ShowSecurityDescriptorUI]
 [-Force] [-NoServiceRestart] [-PSVersion <Version>] [-SessionTypeOption <PSSessionTypeOption>]
 [-TransportOption <PSTransportOption>] [-ModulesToImport <Object[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### AssemblyNameParameterSet

```
Set-PSSessionConfiguration [-Name] <String> [-AssemblyName] <String> [-ApplicationBase <String>]
 [-ConfigurationTypeName] <String> [-RunAsCredential <PSCredential>] [-ThreadOptions <PSThreadOptions>]
 [-AccessMode <PSSessionConfigurationAccessMode>] [-UseSharedProcess] [-StartupScript <String>]
 [-MaximumReceivedDataSizePerCommandMB <Double>] [-MaximumReceivedObjectSizeMB <Double>]
 [-SecurityDescriptorSddl <String>] [-ShowSecurityDescriptorUI] [-Force] [-NoServiceRestart]
 [-PSVersion <Version>] [-SessionTypeOption <PSSessionTypeOption>] [-TransportOption <PSTransportOption>]
 [-ModulesToImport <Object[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### セッションのすべてのもの

```
Set-PSSessionConfiguration [-Name] <String> [-RunAsCredential <PSCredential>]
 [-ThreadOptions <PSThreadOptions>] [-AccessMode <PSSessionConfigurationAccessMode>] [-UseSharedProcess]
 [-StartupScript <String>] [-MaximumReceivedDataSizePerCommandMB <Double>]
 [-MaximumReceivedObjectSizeMB <Double>] [-SecurityDescriptorSddl <String>] [-ShowSecurityDescriptorUI]
 [-Force] [-NoServiceRestart] [-TransportOption <PSTransportOption>] -Path <String> [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`Set-PSSessionConfiguration`コマンドレットは、ローカルコンピューター上のセッション構成のプロパティを変更します。

**Name** パラメーターを使用して、変更するセッション構成を識別します。 その他のパラメーターは、セッション構成のプロパティに新しい値を指定するために使用します。 構成からプロパティ値を削除し、既定値を使用するには、対応するパラメーターに空の文字列 ("") または値を入力し `$Null` ます。

PowerShell 3.0 以降では、セッション構成ファイルを使用してセッション構成を定義できます。 この機能を使用すれば、セッション構成を使用するセッションのプロパティの設定や変更を迷わず簡単に実行できます。 セッション構成ファイルを指定するには、の **Path** パラメーターを使用し `Set-PSSessionConfiguration` ます。 セッション構成ファイルの詳細については、「 [about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)」を参照してください。
セッション構成ファイルを作成および変更する方法の詳細については、コマンドレットを参照してください `New-PSSessionConfigurationFile` 。

セッション構成は、ローカルコンピューターに接続するリモート **セッション (pssession** ) の環境を定義します。 すべての **PSSession** は、セッション構成を使用します。 セッション構成は、セッションで使用できるモジュール、実行を許可されているコマンドレット、言語モード、クォータ、タイムアウトなどの **PSSession** の機能を決定します。 セッション構成のセキュリティ記述子は、セッション構成を使用してローカルコンピューターに接続できるユーザーを決定します。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

セッション構成のプロパティを表示するに `Get-PSSessionConfiguration` は、コマンドレットまたは WSMan プロバイダーを使用します。 WSMan プロバイダーの詳細については、「」と入力 `Get-Help WSMan` してください。

## 例

### 例 1: セッション構成を作成して変更する

この例では、構成からスタートアップスクリプトを追加および削除する方法を示します。

最初のコマンドは、 **Adminshell** 構成を作成します。 2番目のコマンドは、 `AdminConfig.ps1` 構成にスクリプトを追加します。 変更は、 **WinRM** を再起動すると有効になります。
3番目のコマンドは、 `AdminConfig.ps1` 構成からスクリプトを削除します。

```powershell
Register-PSSessionConfiguration -Name "AdminShell" -AssemblyName "C:\Shells\AdminShell.dll" -ConfigurationTypeName "AdminClass"
Set-PSSessionConfiguration -Name "AdminShell" -StartupScript "AdminConfig.ps1"
Set-PSSessionConfiguration -Name "AdminShell" -StartupScript $Null
```

### 例 2: 結果を表示する

この例では、 **MaximumReceivedObjectSizeMB** プロパティの値を20に増やしています。 このコマンドでは、 **WinRM** サービスの再起動を求めるメッセージも表示されます。 変更は、 **WinRM** サービスが再起動されるまで有効になりません。

```powershell
Set-PSSessionConfiguration -Name "IncObj" -MaximumReceivedObjectSizeMB 20
```

```Output
WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Plugin\IncObj\InitializationParameters

ParamName                       ParamValue
---------                       ----------
psmaximumreceivedobjectsizemb   20

"Restart WinRM service"
WinRM service need to be restarted to make the changes effective. Do you want to run the command "restart-service winrm"?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

### 例 3: さまざまな方法で結果を表示する

この例では、によって、 `Set-PSSessionConfiguration` **MaintenanceShell** セッション構成のスタートアップスクリプトがに変更され `Maintenance.ps1` ます。 出力には変更が示され、 **WinRM** サービスの再起動を求めるメッセージが表示されます。 これに対する応答は "y" (はい) です。

`Get-PSSessionConfiguration`**MaintenanceShell** セッション構成を取得します。 パイプライン演算子 (|) は、コマンドの結果をに送信し `Format-List` ます。これにより、構成オブジェクトのすべてのプロパティがリストに表示されます。 次に、WSMan プロバイダーを使用して、 **MaintenanceShell** 構成の初期化パラメーターを表示します。 `Get-ChildItem`(エイリアス `dir` ) **MaintenanceShell** プラグインの **InitializationParameters** ノード内の子項目を取得します。 WSMan プロバイダーの詳細については、「」と入力 `Get-Help wsman` してください。

```powershell
Set-PSSessionConfiguration -Name "MaintenanceShell" -StartupScript "C:\ps-test\Maintenance.ps1"
```

```Output
WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Plugin\MaintenanceShell\InitializationParameters

ParamName            ParamValue
---------            ----------
startupscript        c:\ps-test\Mainte...

"Restart WinRM service"
WinRM service need to be restarted to make the changes effective. Do you want to run
the command "restart-service winrm"?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

```

```powershell
Get-PSSessionConfiguration MaintenanceShell | Format-List -Property *
```

```Output
xmlns            : http://schemas.microsoft.com/wbem/wsman/1/config/PluginConfiguration
Name             : MaintenanceShell
Filename         : %windir%\system32\pwrshplugin.dll
SDKVersion       : 1
XmlRenderingType : text
lang             : en-US
PSVersion        : 2.0
startupscript    : c:\ps-test\Maintenance.ps1
ResourceUri      : http://schemas.microsoft.com/powershell/MaintenanceShell
SupportsOptions  : true
ExactMatch       : true
Capability       : {Shell}
Permission       :
```

```powershell
dir WSMan:\localhost\Plugin\MaintenanceShell\InitializationParameters
```

```Output
ParamName     ParamValue
---------     ----------
PSVersion     2.0
startupscript c:\ps-test\Maintenance.ps1
```

## PARAMETERS

### -AccessMode

セッション構成を有効化および無効化し、コンピューター上のリモート セッションまたはローカル セッションにセッション構成を使用できるかどうかを決定します。 このパラメーターの有効値は、次のとおりです。

- 無効にします。 セッション構成を無効にします。 コンピューターへのリモート アクセスまたはローカル アクセスに使用できません。 この値は、セッション構成の **Enabled** プロパティ ( `WSMan:\<ComputerName>\PlugIn\<SessionConfigurationName>\Enabled` ) を **False** に設定します。
- Local。 **Network_Deny_All** エントリをセッション構成のセキュリティ記述子に追加します。
  ローカルコンピューターのユーザーは、セッション構成を使用して同じコンピューター上にローカルループバックセッションを作成できますが、リモートユーザーはアクセスを拒否されます。
- [Remote]\(リモート\)。 **Deny_All** エントリと **Network_Deny_All** エントリをセッション構成のセキュリティ記述子から削除します。 ローカル コンピューターおよびリモート コンピューターのユーザーは、セッション構成を使用して、このコンピューターでセッションを作成し、コマンドを実行できます。

既定値は **Remote** です。

他のコマンドレットでは、後でこのパラメーターの値をオーバーライドできます。 たとえば、 `Enable-PSRemoting` コマンドレットは、コンピューター上のすべてのセッション構成を有効にし、それらへのリモートアクセスを許可し `Disable-PSRemoting` ます。また、コマンドレットは、コンピューター上のすべてのセッション構成へのローカルアクセスのみを許可します。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.Runspaces.PSSessionConfigurationAccessMode
Parameter Sets: (All)
Aliases:
Accepted values: Disabled, Local, Remote

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ApplicationBase

\* **AssemblyName** パラメーターの値に指定されているアセンブリファイル (.dll) のパスを指定します。

```yaml
Type: System.String
Parameter Sets: NameParameterSet, AssemblyNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AssemblyName

アセンブリ名を指定します。 このコマンドレットは、アセンブリで定義されているクラスに基づいてセッション構成を作成します。

セッション構成を定義するアセンブリ .dll ファイルのファイル名または完全パスを入力します。 ファイル名だけを入力した場合は、 **ApplicationBase** パラメーターの値でパスを入力できます。

```yaml
Type: System.String
Parameter Sets: AssemblyNameParameterSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ConfigurationTypeName

**AssemblyName** パラメーターのアセンブリで定義されているセッションの構成の種類を指定します。 指定する型は、 **System.Management.Automation.Remoting.PSSessionConfiguration** クラスを実装する必要があります。

アセンブリ名を指定する場合、このパラメーターは必須です。

```yaml
Type: System.String
Parameter Sets: AssemblyNameParameterSet
Aliases:

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

すべてのユーザープロンプトを非表示にし、プロンプトを表示せずに **WinRM** サービスを再起動します。 サービスを再起動すると、構成の変更が有効になります。

再起動を回避し、再起動メッセージを表示しないようにするには、 **NoServiceRestart** パラメーターを使用します。

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

### -MaximumReceivedDataSizePerCommandMB

このコンピューターに1つのリモートコマンドで送信できるデータ量の制限を指定します。 データ サイズはメガバイト (MB) 単位で入力します。 既定値は 50 MB です。

**Configurationtypename** パラメーターで指定された構成の種類でデータサイズの制限が定義されている場合、構成の種類の制限が使用されます。 このパラメーターの値は無視されます。

```yaml
Type: System.Nullable`1[System.Double]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumReceivedObjectSizeMB

このコンピューターに1つのオブジェクトで送信できるデータ量の制限を指定します。
データサイズをメガバイト単位で入力します。 既定値は 10 MB です。

**Configurationtypename** パラメーターで指定された構成の種類でオブジェクトサイズの制限が定義されている場合、構成の種類の制限が使用されます。 このパラメーターの値は無視されます。

```yaml
Type: System.Nullable`1[System.Double]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModulesToImport

セッション構成を使用するセッションに自動的にインポートされたモジュールおよびスナップインを指定します。 モジュール名およびスナップイン名を入力します。

既定では、 **PowerShell** のみがセッションにインポートされますが、コマンドレットが除外されていない限り、 `Import-Module` および Add-PSSnapin コマンドレットを使用して、モジュールとスナップインをセッションに追加できます。

このパラメーター値に指定されたモジュールは、セッション構成ファイル () で指定されたモジュールに追加され `New-PSSessionConfigurationFile` ます。 ただし、セッション構成ファイル内の設定によって、モジュールでエクスポートされたコマンドが非表示になったり、ユーザーが使用できなくなったりすることがあります。

このパラメーター値に指定されたモジュールは、コマンドレットの **ModulesToImport** パラメーターを使用して指定されたモジュールの一覧に置き換わるもの `Register-PSSessionConfiguration` です。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Object[]
Parameter Sets: NameParameterSet, AssemblyNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

変更するセッション構成の名前を指定します。

このパラメーターを使用してセッション構成の名前を変更することはできません。

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

### -NoServiceRestart

は、 **WinRM** サービスを再起動せず、サービスの再起動を求めるメッセージを表示しません。

既定では、を実行すると `Set-PSSessionConfiguration` 、新しいセッション構成を有効にするために **WinRM** サービスを再起動するように求められます。 **WinRM** サービスが再起動されるまで、新しいセッション構成は有効になりません。

プロンプトを表示せずに **WinRM** サービスを再起動するには、 **Force** パラメーターを使用します。 **WinRM** サービスを手動で再起動するには、コマンドレットを使用し `Restart-Service` ます。

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

### -Path

セッション構成ファイル (.pssc) のパス (コマンドレットによって作成されたものなど) を指定します `New-PSSessionConfigurationFile` 。 パスを省略した場合、既定値は現在のディレクトリになります。

セッション構成ファイルを変更する方法の詳細については、コマンドレットのヘルプトピックを参照してください `New-PSSessionConfigurationFile` 。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: SessionConfigurationFile
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PSVersion

このセッション構成を使用するセッションの PowerShell のバージョンを指定します。

このパラメーターの値は、セッション構成ファイルの **PowerShellVersion** キーの値より優先されます。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Version
Parameter Sets: NameParameterSet, AssemblyNameParameterSet
Aliases: PowerShellVersion

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunAsCredential

セッションのコマンドの資格情報を指定します。 既定では、コマンドは現在のユーザーのアクセス許可で実行されます。

このパラメーターは、PowerShell 3.0 で導入されました。

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

### -Security記述子 Sddl

構成に対して別のセキュリティ記述子定義言語 (SDDL) 文字列を指定します。

この文字列によって、新しいセッション構成を使用するために必要なアクセス許可が決定します。 セッションでセッション構成を使用するには、その構成に対して少なくとも実行 (Invoke) アクセス許可が必要です。

構成に既定のセキュリティ記述子を使用するには、空の文字列 ("") またはの値を入力し `$Null` ます。 既定値は、WSMan: ドライブのルート SDDL です。

セキュリティ記述子が複雑な場合は、このパラメーターの代わりに **Showsecuritydescriptor ui** パラメーターを使用することを検討してください。 両方のパラメーターを同じコマンドで使用することはできません。

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

### -SessionTypeOption

セッション構成の種類固有のオプションを指定します。 コマンドレットが返す **Psworkflowexecutionoption** オブジェクトなど、セッションの種類のオプションオブジェクトを入力し `New-PSWorkflowExecutionOption` ます。

セッション構成を使用するセッションのオプションは、セッション オプションの値とセッション構成オプションによって決定されます。 指定しない限り、セッションで設定されているオプション (コマンドレットを使用するなど) は、 `New-PSSessionOption` セッション構成で設定されたオプションよりも優先されます。 ただし、セッション オプションの値は、セッション構成に設定されている最大値を超えることはできません。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSSessionTypeOption
Parameter Sets: NameParameterSet, AssemblyNameParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Showsecurity記述子 Ui

このコマンドレットが、セッション構成の新しい SDDL の作成に役立つプロパティシートであることを示します。 コマンドを実行し `Set-PSSessionConfiguration` てから **WinRM** サービスを再起動すると、プロパティシートが表示されます。

構成にアクセス許可を設定する場合は、セッションでセッション構成を使用するには、少なくとも実行 (Invoke) アクセス許可が必要です。

**SecurityDescriptorSDDL** パラメーターとこのパラメーターを同じコマンドで使用することはできません。

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

### -StartupScript

構成のスタートアップスクリプトを指定します。 PowerShell スクリプトの完全修飾パスを入力します。 指定されたスクリプトが、セッション構成を使用する新しいセッションで実行されます。

セッション構成からスタートアップスクリプトを削除するには、空の文字列 ("") または値を入力し `$Null` ます。

スタートアップスクリプトを使用して、ユーザーセッションをさらに構成することができます。 スクリプトでエラーが発生しても、終了しないエラーが発生しても、セッションは作成されず、 `New-PSSession` コマンドは失敗します。

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

### -ThreadOptions

構成のスレッドオプション設定を指定します。 この設定は、セッションでコマンドが実行されたときのスレッドの作成方法および使用方法を定義します。 このパラメーターの有効値は、次のとおりです。

- Default
- ReuseThread
- UseCurrentThread
- UseNewThread

既定値は **UseCurrentThread** です。

詳細については、「 [Psthreadoptions 列挙型](/dotnet/api/system.management.automation.runspaces.psthreadoptions)」を参照してください。

```yaml
Type: System.Management.Automation.Runspaces.PSThreadOptions
Parameter Sets: (All)
Aliases:
Accepted values: Default, UseNewThread, ReuseThread, UseCurrentThread

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TransportOption

セッション構成のトランスポートオプションを指定します。 トランスポートオプションオブジェクト (コマンドレットが返す **Wsmanconfigurationoption** オブジェクトなど) を入力し `New-PSTransportOption` ます。

セッション構成を使用するセッションのオプションは、セッション オプションの値とセッション構成オプションによって決定されます。 指定しない限り、セッションで設定されているオプション (コマンドレットを使用するなど) は、 `New-PSSessionOption` セッション構成で設定されたオプションよりも優先されます。 ただし、セッション オプションの値は、セッション構成に設定されている最大値を超えることはできません。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSTransportOption
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSharedProcess

同じユーザーによって開始され、同じセッション構成を使用するすべてのセッションをホストするには、1つのプロセスのみを使用します。 既定では、各セッションは、個別のプロセスでホストされます。

このパラメーターは、PowerShell 3.0 で導入されました。

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

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

### -ThreadApartmentState

使用するスレッドモジュールのアパートメント状態を指定します。 使用可能な値は、次のとおりです。

- 不明
- MTA
- STA

```yaml
Type: System.Threading.ApartmentState
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### WSManConfigLeafElement のようになります。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

このコマンドレットを実行するには、[管理者として実行] オプションを使用して PowerShell を起動します。

`Set-PSSessionConfiguration`コマンドレットは構成名を変更せず、 **WSMan** プロバイダーはコマンドレットをサポートしていません `Rename-Item` 。 セッション構成の名前を変更するには、コマンドレットを使用して構成を削除した後、コマンドレットを使用して、 `Unregister-PSSessionConfiguration` `Register-PSSessionConfiguration` 新しいセッション構成を作成して登録します。

コマンドレットを使用して、 `Set-PSSessionConfiguration` 既定の microsoft.powershell32 セッション構成を変更できます。 これらの構成は保護されません。 既定のセッション構成の元のバージョンに戻すには、コマンドレットを使用して `Unregister-PSSessionConfiguration` 既定のセッション構成を削除してから、コマンドレットを使用し `Enable-PSRemoting` て復元します。

セッション構成オブジェクトのプロパティは、セッション構成に設定されているオプションとその値によって異なります。 また、セッション構成ファイルを使用するセッション構成には追加のプロパティがあります。

WSMan: ドライブでコマンドを使用して、セッション構成のプロパティを変更できます。
ただし、powershell 2.0 で WSMan: ドライブを使用して、 **OutputBufferingMode** などの powershell 3.0 で導入されたセッション構成プロパティを変更することはできません。 Windows PowerShell 2.0 のコマンドではエラーは生成されませんが、効果はありません。 PowerShell 3.0 で導入されたプロパティを変更するには、PowerShell 3.0 で WSMan: ドライブを使用します。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSessionConfigurationFile](New-PSSessionConfigurationFile.md)

[New-PSSessionOption](New-PSSessionOption.md)

[New-PSSessionOption](New-PSTransportOption.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Test-PSSessionConfigurationFile](Test-PSSessionConfigurationFile.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)
