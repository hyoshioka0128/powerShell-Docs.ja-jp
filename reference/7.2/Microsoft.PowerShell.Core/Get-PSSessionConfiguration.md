---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/26/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-pssessionconfiguration?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSSessionConfiguration
ms.openlocfilehash: a5aa70521841b3b05d34623ff92ebbf9e3471fd1
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601026"
---
# Get-PSSessionConfiguration

## 概要
コンピューターに登録されたセッション構成を取得します。

## SYNTAX

```
Get-PSSessionConfiguration [[-Name] <String[]>] [-Force] [<CommonParameters>]
```

## Description

`Get-PSSessionConfiguration`コマンドレットは、ローカルコンピューターに登録されているセッション構成を取得します。 これは、ユーザーに応じてカスタマイズされたセッション構成を管理するために、システム管理者向けに設計された高度なコマンドレットです。

PowerShell 3.0 以降では、セッション構成 (.pssc) ファイルを使用して、セッション構成のプロパティを定義できます。 この機能を使用すると、コンピューター プログラムを記述せずに、カスタマイズされ、制限されたセッションを作成できます。 セッション構成ファイルの詳細については、「[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)」を参照してください。

また、PowerShell 3.0 以降では、を返すセッション構成オブジェクトに新しい note プロパティが追加されてい `Get-PSSessionConfiguration` ます。 ユーザーとセッション構成の作成者は、これらのプロパティによって、セッション構成の比較検討が容易になります。

セッション構成を作成して登録するには、コマンドレットを使用し `Register-PSSessionConfiguration` ます。
セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

## 例

### 例 1-ローカルコンピューター上のセッション構成を取得する

```powershell
Get-PSSessionConfiguration
```

### 例 2-既定の2つのセッション構成を取得する

このコマンドは、の **Name** パラメーターを使用して、 `Get-PSSessionConfiguration` 名前が "Microsoft" で始まるセッション構成のみを取得します。

```powershell
Get-PSSessionConfiguration -Name Microsoft*
```

```Output
Name                      PSVersion  StartupScript        Permission
----                      ---------  -------------        ----------
microsoft.powershell      5.1                             BUILTIN\Administrators AccessAll...
microsoft.powershell32    5.1                             BUILTIN\Administrators AccessAll...
```

### 例 3-セッション構成のプロパティと値を取得する

この例は、セッション構成ファイルを使用して作成されたセッション構成のプロパティとプロパティ値を示しています。

```powershell
Get-PSSessionConfiguration -Name Full  | Format-List -Property *
```

```Output
Copyright                     : (c) 2011 User01. All rights reserved.
AliasDefinitions              : {System.Collections.Hashtable}
SessionType                   : Default
CompanyName                   : Unknown
GUID                          : 1e9cb265-dae0-4bd3-89a9-8338a47698a1
Author                        : User01
ExecutionPolicy               : Restricted
SchemaVersion                 : 1.0.0.0
LanguageMode                  : FullLanguage
Architecture                  : 64
Filename                      : %windir%\system32\pwrshplugin.dll
ResourceUri                   : http://schemas.microsoft.com/powershell/Full
MaxConcurrentCommandsPerShell : 1500
UseSharedProcess              : false
ProcessIdleTimeoutSec         : 0
xmlns                         : http://schemas.microsoft.com/wbem/wsman/1/config/PluginConfiguration
MaxConcurrentUsers            : 10
lang                          : en-US
SupportsOptions               : true
ExactMatch                    : true
configfilepath                : C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\Full_1e9cb265-dae0-4bd3-89a9-8338a47698a1.pssc
RunAsUser                     :
IdleTimeoutms                 : 7200000
PSVersion                     : 3.0
OutputBufferingMode           : Block
AutoRestart                   : false
MaxShells                     : 300
MaxMemoryPerShellMB           : 1024
MaxIdleTimeoutms              : 43200000
SDKVersion                    : 1
Name                          : Full
XmlRenderingType              : text
Capability                    : {Shell}
RunAsPassword                 :
MaxProcessesPerShell          : 25
Enabled                       : True
MaxShellsPerUser              : 30
Permission                    :
```

この例では、コマンドレットを使用して、 `Get-PSSessionConfiguration` 完全なセッション構成を取得します。 パイプライン演算子は、完全なセッション構成をコマンドレットに送信し `Format-List` ます。 **プロパティ** パラメーターの値が (all) の場合は、 `*` `Format-List` リスト内のオブジェクトのすべてのプロパティと値を表示するように指示されます。

出力には、セッション構成の作成者、セッションの種類、言語モード、このセッション構成で作成されたセッションの実行ポリシー、セッションクォータ、セッション構成ファイルへの完全パスなど、有用な情報が含まれています。

このセッション構成の表示は、セッション構成ファイルを含むセッションに使用されます。
セッション構成ファイルの詳細については、「[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)」を参照してください。

### 例 4-セッション構成を確認する別の方法

この例では、 `Get-ChildItem` WSMan: provider ドライブのコマンドレット (エイリアス) を使用して、 `dir` プラグインノードの内容を確認します。 これは、コンピューター上のセッション構成を確認するもう 1 つの方法です。

```powershell
dir wsman:\localhost\plugin
```

```Output
Type            Keys                                Name
----            ----                                ----
Container       {Name=Event Forwarding Plugin}      Event Forwarding Plugin
Container       {Name=Full}                         Full
Container       {Name=microsoft.powershell}         microsoft.powershell
Container       {Name=microsoft.powershell.workf... microsoft.powershell.workflow
Container       {Name=microsoft.powershell32}       microsoft.powershell32
Container       {Name=microsoft.ServerManager}      microsoft.ServerManager
Container       {Name=WMI Provider}                 WMI Provider
```

**プラグイン** ノードには、登録済みの PowerShell セッション構成を表す **ContainerElement** オブジェクト (WSMANCONFIGCONTAINERELEMENT) と、ws-management 用の他のプラグインが含まれています。

### 例 6-リモートコンピューター上のセッション構成を表示する

この例は、WSMan プロバイダーを使用して、リモート コンピューター上のセッション構成を表示する方法を示しています。 この方法では、コマンドほど多くの情報は提供されません `Get-PSSessionConfiguration` が、このコマンドレットを実行するには、ユーザーが Administrators グループのメンバーである必要はありません。

```powershell
Connect-WSMan -ComputerName Server01
dir WSMan:\Server01\Plugin
```

```Output
   WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Plugin

Type            Keys                                Name
----            ----                                ----
Container       {Name=Empty}                        Empty
Container       {Name=Event Forwarding Plugin}      Event Forwarding Plugin
Container       {Name=Full}                         Full
Container       {Name=microsoft.powershell}         microsoft.powershell
Container       {Name=microsoft.powershell.workf... microsoft.powershell.workflow
Container       {Name=microsoft.powershell32}       microsoft.powershell32
Container       {Name=microsoft.ServerManager}      microsoft.ServerManager
Container       {Name=NoLanguage}                   NoLanguage
Container       {Name=RestrictedLang}               RestrictedLang
Container       {Name=RRS}                          RRS
Container       {Name=SEL Plugin}                   SEL Plugin
Container       {Name=WithProfile}                  WithProfile
Container       {Name=WMI Provider}                 WMI Provider
```

`Connect-WSMan`コマンドレットは、Server01 リモートコンピューター上の WinRM サービスに接続します。 `Get-ChildItem`WSMan: ドライブのコマンドレット (エイリアス) は、 `dir` **Server01\Plugin** パス内の項目を取得します。 出力は、Server01 コンピューターの プラグイン ディレクトリにあるアイテムを示しています。 このアイテムには、WSMan プラグインの一種であるセッション構成のほか、コンピューター上にあるその他の種類のプラグインが含まれます。

### 例 7-リモートコンピューターから詳細なセッション構成を取得する

この例では、リモートコンピューターでコマンドを実行する方法を示し `Get-PSSessionConfiguration` ます。 例に挙げたコマンドは、ローカル コンピューター上のクライアント設定およびリモート コンピューター上のサービス設定で、CredSSP 委任が有効になっている必要があります。

この例のコマンドを実行するには、ローカルコンピューターとリモートコンピューターの Administrators グループのメンバーである必要があります。また、[ **管理者として実行** ] オプションを使用して PowerShell を起動する必要があります。

```powershell
Enable-WSManCredSSP -Delegate Server02
Connect-WSMan Server02
Set-Item WSMan:\Server02*\Service\Auth\CredSSP -Value $true
Invoke-Command -ScriptBlock {Get-PSSessionConfiguration} -ComputerName Server02 -Authentication CredSSP -Credential Domain01\Admin01
```

```Output
Name                      PSVersion  StartupScript        Permission                          PSComputerName
----                      ---------  -------------        ----------                          --------------
microsoft.powershell      5.1                             BUILTIN\Administrators AccessAll... server02.corp.fabrikam.com
microsoft.powershell32    5.1                             BUILTIN\Administrators AccessAll... server02.corp.fabrikam.com
MyX86Shell                5.1        c:\test\x86Shell.ps1 BUILTIN\Administrators AccessAll... server02.corp.fabrikam.com
```

コマンドレットでは、 `Enable-WSManCredSSP` ローカルコンピューター Server01 の **CredSSP** 委任を有効にします。 `Connect-WSMan`コマンドレットは、Server02 コンピューターに接続します。 この操作により、Server02 のノードがローカルコンピューターの WSMan: ドライブに追加され、Server02 コンピューターの WS-Management 設定を表示および変更できるようになります。 この `Set-Item` コマンドレットは、Server02 コンピューターのサービスノードの **CredSSP** 項目の値を **True** に変更します。 これによってリモート コンピューターのサービス設定が構成されます。 `Invoke-Command`コマンドレットは、 `Get-PSSessionConfiguration` Server02 コンピューターでコマンドを実行します。 このコマンドは、**Credential** パラメーターを使用します。また、**Authentication** パラメーターを **CredSSP** という値で使用します。 出力は、Server02 リモート コンピューター上のセッション構成を示しています。

### 例 8-セッション構成のリソース URI を取得する

この例は `$PSSessionConfigurationName` 、リソース URI を受け取るユーザー設定変数の値を設定する場合に便利です。

```powershell
(Get-PSSessionConfiguration -Name CustomShell).resourceURI
```

```Output
http://schemas.microsoft.com/powershell/microsoft.CustomShell
```

変数は、 `$PSSessionConfigurationName` セッションを作成するときに使用される既定の構成を指定します。 この変数は、ローカル コンピューターで設定されますが、リモート コンピューターの構成を指定します。 変数の詳細については `$PSSessionConfiguration` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

## PARAMETERS

### -Force

WinRM サービスが実行されていない場合に、WinRM サービスの再起動を求めるメッセージを表示しません。

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

### -Name

指定された名前または名前パターンのセッション構成のみを取得します。 1 つ以上のセッション構成名を入力します。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: All session configurations on the local computer
Accept pipeline input: False
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](./About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### Register-pssessionconfiguration # のコマンド # には、

## 注

- このコマンドレットを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

- コンピューター上のセッション構成を表示するには、そのコンピューターの Administrators グループのメンバーである必要があります。

- リモートコンピューターでコマンドを実行するには `Get-PSSessionConfiguration` 、ローカルコンピューターのクライアント設定で (コマンドレットを使用して) Credential Security Service Provider (CredSSP) 認証を有効にし、リモートコンピューターのサービス設定で有効にする必要があり `Enable-WSManCredSSP` ます。 また、リモートセッションを確立するときには、 **Authentication** パラメーターの **CredSSP** 値を使用する必要があります。 使用しない場合は、アクセスが拒否されます。

- が返すオブジェクトのメモプロパティは、 `Get-PSSessionConfiguration` 値がある場合にのみオブジェクトに表示されます。 セッション構成ファイルを使用して作成されたセッション構成にのみ、すべての定義済みプロパティがあります。

- セッション構成オブジェクトのプロパティは、セッション構成に設定されているオプションとその値によって異なります。 また、セッション構成ファイルを使用するセッション構成には追加のプロパティがあります。

- WSMan: ドライブでコマンドを使用して、セッション構成のプロパティを変更できます。
  ただし、powershell 2.0 で WSMan: ドライブを使用して、 **OutputBufferingMode** などの powershell 3.0 で導入されたセッション構成プロパティを変更することはできません。 PowerShell 2.0 のコマンドではエラーは生成されませんが、無効になります。 PowerShell 3.0 で導入されたプロパティを変更するには、PowerShell 3.0 で WSMan: ドライブを使用します。

## 関連リンク

[Disable-PSSessionConfiguration](Disable-PSSessionConfiguration.md)

[Enable-PSSessionConfiguration](Enable-PSSessionConfiguration.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSessionConfigurationFile](New-PSSessionConfigurationFile.md)

[New-PSSessionOption](New-PSSessionOption.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

[Test-PSSessionConfigurationFile](Test-PSSessionConfigurationFile.md)

[Unregister-PSSessionConfiguration](Unregister-PSSessionConfiguration.md)

[WSMan プロバイダー](../microsoft.wsman.management/about/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)

