---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/register-pssessionconfiguration?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-PSSessionConfiguration
ms.openlocfilehash: 028e28e071bf6e19f2d0aef72b4eec45da6c45b7
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94345886"
---
# Register-PSSessionConfiguration

## 概要
新しいセッション構成を作成して登録します。

## SYNTAX

### NameParameterSet (既定値)

```
Register-PSSessionConfiguration [-ProcessorArchitecture <String>] [-Name] <String> [-ApplicationBase <String>]
 [-RunAsCredential <PSCredential>] [-ThreadApartmentState <ApartmentState>] [-ThreadOptions <PSThreadOptions>]
 [-AccessMode <PSSessionConfigurationAccessMode>] [-UseSharedProcess] [-StartupScript <String>]
 [-MaximumReceivedDataSizePerCommandMB <Double>] [-MaximumReceivedObjectSizeMB <Double>]
 [-SecurityDescriptorSddl <String>] [-ShowSecurityDescriptorUI] [-Force] [-NoServiceRestart]
 [-PSVersion <Version>] [-SessionTypeOption <PSSessionTypeOption>] [-TransportOption <PSTransportOption>]
 [-ModulesToImport <Object[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### AssemblyNameParameterSet

```
Register-PSSessionConfiguration [-ProcessorArchitecture <String>] [-Name] <String> [-AssemblyName] <String>
 [-ApplicationBase <String>] [-ConfigurationTypeName] <String> [-RunAsCredential <PSCredential>]
 [-ThreadApartmentState <ApartmentState>] [-ThreadOptions <PSThreadOptions>]
 [-AccessMode <PSSessionConfigurationAccessMode>] [-UseSharedProcess] [-StartupScript <String>]
 [-MaximumReceivedDataSizePerCommandMB <Double>] [-MaximumReceivedObjectSizeMB <Double>]
 [-SecurityDescriptorSddl <String>] [-ShowSecurityDescriptorUI] [-Force] [-NoServiceRestart]
 [-PSVersion <Version>] [-SessionTypeOption <PSSessionTypeOption>] [-TransportOption <PSTransportOption>]
 [-ModulesToImport <Object[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### セッションのすべてのもの

```
Register-PSSessionConfiguration [-ProcessorArchitecture <String>] [-Name] <String>
 [-RunAsCredential <PSCredential>] [-ThreadApartmentState <ApartmentState>] [-ThreadOptions <PSThreadOptions>]
 [-AccessMode <PSSessionConfigurationAccessMode>] [-UseSharedProcess] [-StartupScript <String>]
 [-MaximumReceivedDataSizePerCommandMB <Double>] [-MaximumReceivedObjectSizeMB <Double>]
 [-SecurityDescriptorSddl <String>] [-ShowSecurityDescriptorUI] [-Force] [-NoServiceRestart]
 [-TransportOption <PSTransportOption>] -Path <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Register-PSSessionConfiguration`コマンドレットは、新しいセッション構成を作成し、ローカルコンピューターに登録します。 これは、リモートユーザー向けのカスタムセッションの作成に使用できる高度なコマンドレットです。

すべての PowerShell セッション ( **PSSession** ) は、エンドポイントとも呼ばれるセッション構成を使用します。
ユーザーは、コンピューターに接続するセッションを作成するときに、セッション構成を選択するか、PowerShell リモート処理を有効にしたときに登録される既定のセッション構成を使用することができます。
また、現在のセッションで作成されたリモート セッション用の既定の構成を指定する、$PSSessionConfigurationName 設定変数を設定することもできます。

このセッション構成は、リモート セッションの環境を定義します。 この構成により、セッションで使用できるコマンドおよび言語要素を決定できます。これには、セッションがリモートで取得できる 1 つのオブジェクトまたはコマンド内のデータの量を制限する設定など、コンピューターを保護する設定を含めることができます。 セッション構成のセキュリティ記述子によって、セッション構成を使用するアクセス許可を持つユーザーが決定されます。

構成の要素を定義するには、新しい構成クラスを実装するアセンブリを使用するか、セッションで実行されるスクリプトを使用します。 PowerShell 3.0 以降では、セッション構成ファイルを使用してセッション構成を定義することもできます。

セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。
セッション構成ファイルの詳細については、「 [about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)」を参照してください。

## 例

### 例 1: NewShell セッション構成を登録する

この例では、 **Newshell** セッション構成を登録します。 **AssemblyName** パラメーターと **ApplicationBase** パラメーターは、セッション構成のコマンドレットとプロバイダーを指定する **MyShell.dll** ファイルの場所を指定します。 **Configurationtypename** パラメーターは、アセンブリから使用する構成クラスを指定します。

```powershell
$sessionConfiguration = @{
    Name='NewShell'
    ApplicationBase='c:\MyShells\'
    AssemblyName='MyShell.dll'
    ConfigurationTypeName='MyClass'
}
Register-PSSessionConfiguration @sessionConfiguration
```

この構成を使用するには、「」と入力 `New-PSSession -ConfigurationName newshell` します。

### 例 2: MaintenanceShell セッション構成を登録する

この例では、ローカルコンピューターに **MaintenanceShell** セッション構成を登録します。 **Startupscript** パラメーターは、スクリプトを指定し `Maintenance.ps1` ます。

```powershell
Register-PSSessionConfiguration -Name MaintenanceShell -StartupScript C:\ps-test\Maintenance.ps1
```

ユーザーがコマンドを使用 `New-PSSession` して **MaintenanceShell** 構成を選択すると、 `Maintenance.ps1` スクリプトは新しいセッションで実行されます。 このスクリプトでは、セッションを構成できます。 これには、モジュールのインポートや、セッションの実行ポリシーの設定が含まれます。 スクリプトによってエラー (終了しないエラーを含む) が生成された場合、 `New-PSSession` コマンドは失敗します。

### 例 3: セッション構成を登録する

この例では、 **Adminshell** セッション構成を登録します。

変数は、 `$sessionParams` すべてのパラメーター値を含むハッシュテーブルです。 このハッシュテーブルは、PowerShell スプラッティングを使用してコマンドレットに渡されます。 このコマンドでは、 `Register-PSSessionConfiguration` **SecurityDescritorSDDL** パラメーターを使用して変数の値に SDDL を指定し、MaximumReceivedObjectSizeMB パラメーターを使用して `$sddl` オブジェクトサイズの制限を増やしています。 **MaximumReceivedObjectSizeMB** また、 **StartupScript** パラメーターを使用して、セッションを構成するスクリプトを指定します。

```powershell
$sddl = "O:NSG:BAD:P(A;;GA;;;BA)S:P(AU;FA;GA;;;WD)(AU;FA;SA;GWGX;;WD)"
$sessionParams = @{
    Name="AdminShell"
    SecurityDescriptorSDDL=$sddl
    MaximumReceivedObjectSizeMB=20
    StartupScript="C:\scripts\AdminShell.ps1"
}
Register-PSSessionConfiguration @sessionParams
```

### 例 4: 構成コンテナー要素を返す

この例では、 **MaintenanceShell** 構成を登録する方法を示します。
`Register-PSSessionConfiguration` 変数に格納されている **WSManConfigContainerElement** オブジェクトを返し `$s` ます。 `Format-List` 返されたオブジェクトのすべてのプロパティを表示します。 **PSPath** プロパティは、オブジェクトが WSMan: ドライブのディレクトリに格納されることを示します。 `Get-ChildItem` (エイリアス `dir` )パス内の項目を表示 `WSMan:\LocalHost\PlugIn` します。 これには、新しい **MaintenanceShell** 構成と、PowerShell に付属する2つの既定の構成が含まれます。

```powershell
$s = Register-PSSessionConfiguration -Name MaintenanceShell -StartupScript C:\ps-test\Maintenance.ps1
$s | Format-List -Property *
dir WSMan:\LocalHost\Plugin
```

```Output
PSPath            : Microsoft.WSMan.Management\WSMan::localhost\Plugin\MaintenanceShell
PSParentPath      : Microsoft.WSMan.Management\WSMan::localhost\Plugin
PSChildName       : MaintenanceShell
PSDrive           : WSMan
PSProvider        : Microsoft.WSMan.Management\WSMan
PSIsContainer     : True
Keys              : {Name=MaintenanceShell}
Name              : MaintenanceShell
TypeNameOfElement : Container

Name                      Type                 Keys
----                      ----                 ----
MaintenanceShell          Container            {Name=MaintenanceShell}
microsoft.powershell      Container            {Name=microsoft.powershell}
microsoft.powershell32    Container            {Name=microsoft.powershell32}
```

### 例 5: スタートアップスクリプトを使用してセッション構成を登録する

この例では、 **Withprofile** セッション構成を作成して登録します。 **Startupscript** パラメーターは、セッション構成を使用するすべてのセッションに対して、指定されたスクリプトを実行するように PowerShell に指示します。

```powershell
Register-PSSessionConfiguration -Name WithProfile -StartupScript Add-Profile.ps1
```

このスクリプトには、ドット ソースを使用してセッションの現在のスコープでユーザーの **CurrentUserAllHosts** プロファイルを実行する、1 つのコマンドが含まれています。

プロファイルの詳細については、「[about_Profiles](./About/about_Profiles.md)」を参照してください。 ドット ソースの詳細については、「[about_Scopes](./About/about_Scopes.md)」を参照してください。

## PARAMETERS

### -AccessMode

セッション構成を有効化および無効化し、コンピューター上のリモート セッションまたはローカル セッションにセッション構成を使用できるかどうかを決定します。 このパラメーターの有効値は、次のとおりです。

- 無効にします。 セッション構成を無効にします。 コンピューターへのリモート アクセスまたはローカル アクセスに使用できません。
- Local。 ローカルコンピューターのユーザーがセッション構成を使用して、同じコンピューター上にローカルループバックセッションを作成できますが、リモートユーザーへのアクセスは拒否されます。
- [Remote]\(リモート\)。 ローカル ユーザーおよびリモート ユーザーは、セッション構成を使用して、このコンピューターでセッションを作成し、コマンドを実行できます。

既定値は Remote です。

他のコマンドレットでは、後でこのパラメーターの値をオーバーライドできます。 たとえば、 `Enable-PSRemoting` コマンドレットを使用すると、すべてのセッション構成へのリモートアクセスが可能になり、コマンドレットによって `Enable-PSSessionConfiguration` セッション構成が有効になります。また、コマンドレットにより、 `Disable-PSRemoting` すべてのセッション構成へのリモートアクセスが禁止されます。

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

\* **AssemblyName** パラメーターの値に指定されているアセンブリファイル (.dll) のパスを指定します。 **AssemblyName** パラメーターの値にパスが含まれていない場合は、このパラメーターを使用します。 既定値は、現在のディレクトリです。

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

構成の種類が定義されているアセンブリファイル (.dll) の名前を指定し \* ます。 このパラメーターに .dll のパスを指定することも、 **ApplicationBase** パラメーターの値を指定することもできます。

**Configurationtypename** パラメーターを指定する場合、このパラメーターは必須です。

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

この構成に使用される Microsoft .NET Framework の型の完全修飾名を指定します。 指定する型は、 **System.Management.Automation.Remoting.PSSessionConfiguration** クラスを実装する必要があります。

構成の種類を実装するアセンブリファイル (.dll) を指定するには \* 、 **AssemblyName** パラメーターと **ApplicationBase** パラメーターを指定します。

型を作成すると、コマンドレットの特定のパラメーターを公開または非表示にしたり、ユーザーが上書きできないデータサイズやオブジェクトサイズの制限を設定したりするなど、セッション構成のさまざまな側面を制御できます。

このパラメーターを省略すると、 **DefaultRemotePowerShellConfiguration** クラスがセッション構成に使用されます。

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

すべてのユーザーにプロンプトを表示せずに、 **WinRM** サービスを再起動します。 サービスを再起動すると、構成の変更が有効になります。

再起動を回避し、再起動のプロンプトを非表示にするには、 **NoServiceRestart** パラメーターを指定します。

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

**ConfigurationTypeName** パラメーターで指定された構成の種類でデータ サイズの制限が定義されている場合、構成の種類の制限が使用され、このパラメーターの値は無視されます。

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

**ConfigurationTypeName** パラメーターで指定された構成の種類でオブジェクト サイズの制限が定義されている場合、構成の種類の制限が使用され、このパラメーターの値は無視されます。

```yaml
Type: System.Nullable`1[System.Double]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 10
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModulesToImport

セッション構成を使用するセッションに自動的にインポートされるモジュールを指定します。

既定では、セッションにインポートされるのは、 **Microsoft. PowerShell** だけです。 コマンドレットが除外されていない限り、を使用し `Import-Module` てモジュールをセッションに追加できます。

このパラメーター値に指定されているモジュールは、 **Sessiontype** パラメーターによって指定されたモジュールと、セッション構成ファイル () の **ModulesToImport** キーに記載されているモジュールに追加され `New-PSSessionConfigurationFile` ます。 ただし、セッション構成ファイル内の設定によって、モジュールでエクスポートされたコマンドが非表示になったり、ユーザーが使用できなくなったりすることがあります。

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

セッション構成の名前を指定します。 このパラメーターは必須です。

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

既定では、コマンドを実行すると、 `Register-PSSessionConfiguration` 新しいセッション構成を有効にするために **WinRM** サービスを再起動するように求められます。 **WinRM** サービスが再起動されるまで、新しいセッション構成は有効になりません。

プロンプトを表示せずに **WinRM** サービスを再起動するには、 **Force** パラメーターを指定します。 **WinRM** サービスを手動で再起動するには、コマンドレットを使用し `Restart-Service` ます。

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

によって作成されたセッション構成ファイル (.pssc) のパスとファイル名を指定します `New-PSSessionConfigurationFile` 。 パスを省略した場合、既定値は現在のディレクトリになります。

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

### -ProcessorArchitecture

このセッション構成を使用するセッションで、32ビットバージョンまたは64ビット版の PowerShell プロセスが開始されているかどうかを確認します。 このパラメーターに指定できる値は、x86 (32 ビット) と AMD64 (64 ビット) です。 既定値は、セッション構成をホストするプロセッサ アーキテクチャにより決定されます。

このパラメーターを使用すると、64 ビット コンピューターで 32 ビット セッションを作成できます。 32 ビット コンピューターでの 64 ビット プロセスの作成を試行します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: PA
Accepted values: x86, amd64

Required: False
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

構成のセキュリティ記述子定義言語 (SDDL) 文字列を指定します。

この文字列によって、新しいセッション構成を使用するために必要なアクセス許可が決定します。 セッションでセッション構成を使用するには、その構成に対して少なくとも実行 (Invoke) アクセス許可が必要です。

セキュリティ記述子が複雑な場合は、このパラメーターの代わりに **ShowSecurityDescriptorUI** パラメーターを使用することを検討してください。 両方のパラメーターを同じコマンドで使用することはできません。

このパラメーターを省略した場合、 **WinRM** サービスのルート SDDL がこの構成に使用されます。
ルート SDDL を表示または変更するには、WSMan プロバイダーを使用します。 たとえば、「 `Get-Item wsman:\localhost\service\rootSDDL` 」のように指定します。 WSMan プロバイダーの詳細については、「」と入力 `Get-Help wsman` してください。

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

このコマンドレットによって、セッション構成の SDDL の作成に役立つプロパティシートが表示されることを示します。 コマンドを入力 `Register-PSSessionConfiguration` してから **WinRM** サービスを再起動すると、プロパティシートが表示されます。

構成のアクセス許可を設定する場合は、セッションでセッション構成を使用するには、少なくとも実行 (Invoke) アクセス許可が必要です。

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

PowerShell スクリプトの完全修飾パスを指定します。 指定されたスクリプトが、セッション構成を使用する新しいセッションで実行されます。

スクリプトを使用すると、セッションをさらに構成できます。 スクリプトでエラーが発生しても、終了しないエラーが発生しても、セッションは作成されず、 `New-PSSession` コマンドは失敗します。

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

セッションでコマンドを実行するときに、スレッドを作成して使用する方法を指定します。 このパラメーターの有効値は、次のとおりです。

- Default
- ReuseThread
- UseCurrentThread
- UseNewThread

既定値は **UseCurrentThread** です。

詳細については、「 [Psthreadoptions 列挙型](/dotnet/api/system.management.automation.runspaces.psthreadoptions?view=powershellsdk-1.1.0)」を参照してください。

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

トランスポートオプションを指定します。

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

### WSManConfigContainerElement のようになります。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

このコマンドレットを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動する必要があります。

このコマンドレットは、管理用 Web サービス (WS-MANAGEMENT) プラグイン構成を表す XML を生成し、XML を WS-MANAGEMENT に送信します。これにより、ローカルコンピューター () にプラグインが登録され `New-Item wsman:\localhost\plugin` ます。

セッション構成オブジェクトのプロパティは、セッション構成に設定されているオプションとその値によって異なります。 また、セッション構成ファイルを使用するセッション構成には追加のプロパティがあります。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSessionConfigurationFile](New-PSSessionConfigurationFile.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Test-PSSessionConfigurationFile](Test-PSSessionConfigurationFile.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)
