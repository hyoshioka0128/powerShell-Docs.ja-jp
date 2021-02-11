---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC リソースのデバッグ
description: この記事では、DSC 構成のデバッグを有効にする方法について説明します。
ms.openlocfilehash: 5dda217e8dc9cc4b8699c82153c1a588d405d99e
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92654124"
---
# <a name="debugging-dsc-resources"></a>DSC リソースのデバッグ

> 適用先:Windows PowerShell 5.0

PowerShell 5.0 では、構成が適用されているときに DSC リソースをデバッグできる新機能が Desired State Configuraiton (DSC) に導入されました。

## <a name="enabling-dsc-debugging"></a>DSC デバッグの有効化

リソースをデバッグする前に、[Enable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Enable-DscDebug) コマンドレットを呼び出すことによって、デバッグを有効にする必要があります。 このコマンドレットは、必須パラメーター **BreakAll** を取ります。

[Get-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Get-DscLocalConfigurationManager) への呼び出しの結果を参照して、デバッグが有効になっていることを確認できます。

次の PowerShell 出力は、デバッグを有効にした結果を示しています。

```powershell
PS C:\DebugTest> $LCM = Get-DscLocalConfigurationManager

PS C:\DebugTest> $LCM.DebugMode
NONE

PS C:\DebugTest> Enable-DscDebug -BreakAll

PS C:\DebugTest> $LCM = Get-DscLocalConfigurationManager

PS C:\DebugTest> $LCM.DebugMode
ForceModuleImport
ResourceScriptBreakAll

PS C:\DebugTest>
```

## <a name="starting-a-configuration-with-debug-enabled"></a>デバッグを有効にした構成の開始

DSC リソースをデバッグするには、そのリソースを呼び出す構成を開始します。 この例では、"WindowsPowerShellWebAccess" 機能がインストールされていることを確認するために **WindowsFeature** リソースを呼び出す単純な構成を見ていきます。

```powershell
Configuration PSWebAccess
    {
    Import-DscResource -ModuleName 'PsDesiredStateConfiguration'
    Node localhost
        {
        WindowsFeature PSWA
            {
            Name = 'WindowsPowerShellWebAccess'
            Ensure = 'Present'
            }
        }
    }
PSWebAccess
```

構成をコンパイルした後、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) を呼び出して開始します。 構成は、ローカル構成マネージャー (LCM) が構成の最初のリソースを呼び出したときに停止します。 `-Verbose` および `-Wait` パラメーターを使用した場合、デバッグを開始するために入力する必要がある行が出力に表示されます。

```powershell
Start-DscConfiguration .\PSWebAccess -Wait -Verbose
VERBOSE: Perform operation 'Invoke CimMethod' with following parameters, ''methodName' = SendConfigurationApply,'className' = MSFT_DSCLocalConfiguration
Manager,'namespaceName' = root/Microsoft/Windows/DesiredStateConfiguration'.
VERBOSE: An LCM method call arrived from computer TEST-SRV with user sid S-1-5-21-2127521184-1604012920-1887927527-108583.
VERBOSE: An LCM method call arrived from computer TEST-SRV with user sid S-1-5-21-2127521184-1604012920-1887927527-108583.
VERBOSE: [TEST-SRV]: LCM:  [ Start  Set      ]
WARNING: [TEST-SRV]:                            [DSCEngine] Warning LCM is in Debug 'ResourceScriptBreakAll' mode.  Resource script processing will
be stopped to wait for PowerShell script debugger to attach.
VERBOSE: [TEST-SRV]:                            [DSCEngine] Importing the module C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules\PSDesiredStateCo
nfiguration\DscResources\MSFT_RoleResource\MSFT_RoleResource.psm1 in force mode.
VERBOSE: [TEST-SRV]: LCM:  [ Start  Resource ]  [[WindowsFeature]PSWA]
VERBOSE: [TEST-SRV]: LCM:  [ Start  Test     ]  [[WindowsFeature]PSWA]
VERBOSE: [TEST-SRV]:                            [[WindowsFeature]PSWA] Importing the module MSFT_RoleResource in force mode.
WARNING: [TEST-SRV]:                            [[WindowsFeature]PSWA] Resource is waiting for PowerShell script debugger to attach.
Use the following commands to begin debugging this resource script:
Enter-PSSession -ComputerName TEST-SRV -Credential <credentials>
Enter-PSHostProcess -Id 9000 -AppDomainName DscPsPluginWkr_AppDomain
Debug-Runspace -Id 9
```

この時点で、LCM はリソースを呼び出し、最初のブレーク ポイントに到達しています。 出力の最後の 3 行は、プロセスに接続し、リソース スクリプトのデバッグを開始する方法を示しています。

## <a name="debugging-the-resource-script"></a>リソース スクリプトのデバッグ

PowerShell ISE の新しいインスタンスを開始します。 コンソール ウィンドウで、`Start-DscConfiguration` 出力から出力の最後の 3 行をコマンドとして入力し、`<credentials>` を有効なユーザー資格情報に置き換えます。 次のようなプロンプトが表示されます。

```powershell
[TEST-SRV]: [DBG]: [Process:9000]: [RemoteHost]: PS C:\DebugTest>>
```

スクリプト ウィンドウが開いてリソース スクリプトが表示され、 **Test-TargetResource** 関数の最初の行 (クラスベースのリソースの **Test()** メソッド) でデバッガーが停止します。 ISE でデバッグ コマンドを使うと、リソース スクリプトをステップ実行したり、変数の値を確認したり、呼び出し履歴を表示したりできます。 リソース スクリプト (またはクラス) のすべての行がブレークポイントとして設定されていることに注意してください。

## <a name="disabling-dsc-debugging"></a>DSC デバッグの無効化

[Enable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Enable-DscDebug)を呼び出した後では、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) を呼び出すたびに構成でデバッガー中断が行われるようになります。 構成を通常どおりに実行できるようにするには、[Disable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Disable-DscDebug) コマンドレットを呼び出してデバッグを無効化する必要があります。

> [!NOTE]
> 再起動をしても、LCM のデバッグ状態は変更されません。 デバッグが有効になっている場合、再起動後に構成を開始してもデバッグ中断が行われます。

## <a name="see-also"></a>参照

- [MOF を使用したカスタム DSC リソースの記述](../resources/authoringResourceMOF.md)
- [PowerShell クラスを使用したカスタム DSC リソースの記述](../resources/authoringResourceClass.md)
