---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: ノード上で構成を適用、取得、およびテストする
description: このガイドでは、ターゲット ノード上の構成を操作する方法を説明します。
ms.openlocfilehash: 6bc9262bc0e2ce8eea7b85bd46ecc0c0a691276d
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92651012"
---
# <a name="apply-get-and-test-configurations-on-a-node"></a>ノード上で構成を適用、取得、およびテストする

このガイドでは、ターゲット ノード上の構成を操作する方法を説明します。 このガイドは、次の手順に分かれています。

## <a name="apply-a-configuration"></a>構成を適用する

構成を適用したり管理したりするには、".mof" ファイルを生成する必要があります。 次のコードは、このガイド全体で使用する簡単な構成を表したものです。

```powershell
Configuration Sample
{
    # This will generate two .mof files, a localhost.mof, and a server02.mof
    Node localhost, server02
    {
        File SampleFile
        {
            DestinationPath = 'C:\Temp\temp.txt'
            Contents = 'This is a simple resource to show Configuration functionality on a Node.'
        }
    }
}

# The -OutputPath parameter is built into every Configuration automatically.
# The value of -OutputPath determines where the .mof file should be stored, once compiled.
Sample -OutputPath "C:\Temp\"
```

この構成をコンパイルすると、2 つの ".mof" ファイルが生成されます。

```Output
Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a----       11/27/2018   7:29 AM     2.13KB localhost.mof
-a----       11/27/2018   7:29 AM     2.13KB server02.mof
```

構成を適用するには、[Start-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを使います。 `-Path` パラメーターでは、".mof" ファイルが存在するディレクトリを指定します。 `-Computername` を指定しないと、`Start-DSCConfiguration` では、".mof" ファイルの名前によって指定されているコンピューター名 (`<computername>.mof`) に対して、各構成の適用が試みられます。 詳細な出力を表示するには、`Start-DSCConfiguration` に対して `-Verbose` を指定します。

```powershell
Start-DSCConfiguration -Path C:\Temp\ -Verbose
```

`-Wait` を指定しない場合、1 つのジョブが作成されます。 作成されたジョブには、`Start-DSCConfiguration` によって処理される ".mof" ファイルごとに 1 つの **ChildJob** があります。

```Output
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
45     Job45           Configuratio... Running       True            localhost,server02   Start-DSCConfiguration...
```

構成に長い時間がかかっていて停止したい場合は、[Stop-DSCConfiguration](/powershell/module/PSDesiredStateConfiguration/Stop-DscConfiguration) を使ってローカル ノードでの適用を停止できます。

```powershell
Stop-DSCConfiguration -Force
```

完了した後は、[Get-Job](/powershell/module/microsoft.powershell.core/get-job) によって返されるジョブ オブジェクトにより、ジョブの状態を確認できます。

```powershell
$job = Get-Job
$job.ChildJobs
```

```Output
Id     Name            PSJobTypeName   State         HasMoreData     Location             Command
--     ----            -------------   -----         -----------     --------             -------
49     Job49           Configuratio... Completed     True            localhost            Start-DSCConfiguration...
50     Job50           Configuratio... Completed     True            server02             Start-DSCConfiguration...
```

**詳細** な出力を見るには、次のコマンドを使って、各 **ChildJob** の **詳細** ストリームを表示します。 PowerShell のジョブについて詳しくは、「[About Jobs (ジョブについて)](/powershell/module/microsoft.powershell.core/about/about_jobs)」をご覧ください。

```powershell
# View the verbose output of the localhost job using array indexing.
$job.ChildJobs[0].Verbose
```

```Output
Perform operation 'Invoke CimMethod' with following parameters, ''methodName' = SendConfigurationApply,
'className' = MSFT_DSCLocalConfigurationManager,'namespaceName' = root/Microsoft/Windows/DesiredStateConfiguration'.
An LCM method call arrived from computer SERVER01 with user sid S-1-5-21-124525095-708259637-1543119021-1282804.
[SERVER01]: LCM:  [ Start  Set      ]
[SERVER01]: LCM:  [ Start  Resource ]  [[File]SampleFile]
[SERVER01]: LCM:  [ Start  Test     ]  [[File]SampleFile]
[SERVER01]:                            [[File]SampleFile] The destination object was found and no action is required.
[SERVER01]: LCM:  [ End    Test     ]  [[File]SampleFile]  in 0.0370 seconds.
[SERVER01]: LCM:  [ Skip   Set      ]  [[File]SampleFile]
[SERVER01]: LCM:  [ End    Resource ]  [[File]SampleFile]
[SERVER01]: LCM:  [ End    Set      ]
[SERVER01]: LCM:  [ End    Set      ]    in  0.2400 seconds.
Operation 'Invoke CimMethod' complete.
```

PowerShell 5.0 以降では、`Start-DSCConfiguration` に `-UseExisting` パラメーターが追加されています。 `-UseExisting` を指定することにより、`-Path` パラメーターで指定したものではなく、既存適用されている構成を使うようコマンドレットに指示します。

```powershell
Start-DSCConfiguration -UseExisting -Verbose -Wait
```

## <a name="test-a-configuration"></a>構成をテストする

[Test-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration) を使って、現在適用されている構成をテストできます。
`Test-DSCConfiguration` では、ノードが準拠している場合は `True` が返され、そうでない場合は `False` が返されます。

```powershell
Test-DSCConfiguration
```

PowerShell 5.0 で初めて追加された`-Detailed` パラメーターでは、 **ResourcesInDesiredState** および **ResourcesNotInDesiredState** のコレクションを含むオブジェクトが返されます

```powershell
Test-DSCConfiguration -Detailed
```

PowerShell 5.0 以降では、適用しないで構成をテストできます。 `-ReferenceConfiguration` パラメーターは、ノードのテスト対象の ".mof" ファイルのパスを受け取ります。 ノードに対して **設定** アクションは実行されません。 PowerShell 4.0 では、適用しないで構成をテストする回避策がありますが、ここでは説明しません。

## <a name="get-configuration-values"></a>構成の値を取得する

[Get-DSCConfiguration](/powershell/module/PSDesiredStateConfiguration/Get-DscConfiguration) コマンドレットでは、現在適用されている構成で構成されるリソースの現在の値が返されます。

```powershell
Get-DSCConfiguration
```

正しく適用されている場合、サンプルの構成からの出力は次のようになります。

```Output
ConfigurationName    : Sample
DependsOn            :
ModuleName           : PSDesiredStateConfiguration
ModuleVersion        :
PsDscRunAsCredential :
ResourceId           : [File]SampleFile
SourceInfo           :
Attributes           : {archive}
Checksum             :
Contents             :
CreatedDate          : 11/27/2018 7:16:06 AM
Credential           :
DestinationPath      : C:\Temp\temp.txt
Ensure               : present
Force                :
MatchSource          :
ModifiedDate         : 11/27/2018 7:16:06 AM
Recurse              :
Size                 : 75
SourcePath           :
SubItems             :
Type                 : file
PSComputerName       :
CimClassName         : MSFT_FileDirectoryConfiguration
```

## <a name="get-configuration-status"></a>構成の状態を取得する

PowerShell 5.0 以降の [Get-DSCConfigurationStatus](/powershell/module/PSDesiredStateConfiguration/Get-DscConfigurationStatus) コマンドレットでは、ノードに適用された構成の履歴を参照できます。 PowerShell DSC では、 **プッシュ** または **プル** モードで適用された直近 {{N}} 個の構成が追跡されています。 これには、LCM によって実行された " *整合性* " チェックが含まれます。 既定の `Get-DSCConfigurationStatus` では、最後の履歴エントリだけが表示されます。

```powershell
Get-DSCConfigurationStatus
```

```Output
Status     StartDate                 Type            Mode  RebootRequested      NumberOfResources
------     ---------                 ----            ----  ---------------      -----------------
Success    11/27/2018 7:18:40 AM     Consistency     PUSH  False                1
```

すべての構成状態の履歴を表示するには、`-All` パラメーターを使います。

> [!NOTE]
> 簡潔にするため出力は切り捨てられます。

```powershell
Get-DSCConfigurationStatus -All
```

```Output
Status     StartDate                 Type            Mode  RebootRequested      NumberOfResources
------     ---------                 ----            ----  ---------------      -----------------
Success    11/27/2018 7:18:40 AM     Consistency     PUSH  False                1
Success    11/27/2018 7:16:06 AM     Initial         PUSH  False                1
Success    11/27/2018 7:04:53 AM     Initial         PUSH  False                1
Success    11/27/2018 7:03:45 AM     Consistency     PUSH  False                2
Success    11/27/2018 7:03:40 AM     Consistency     PUSH  False                2
Success    11/27/2018 6:48:40 AM     Consistency     PUSH  False                2
Success    11/27/2018 6:33:44 AM     Consistency     PUSH  False                2
Success    11/27/2018 6:33:40 AM     Consistency     PUSH  False                2
Success    11/27/2018 6:18:40 AM     Consistency     PUSH  False                2
Success    11/27/2018 6:03:44 AM     Consistency     PUSH  False                2
```

## <a name="manage-configuration-documents"></a>構成ドキュメントを管理する

LCM では、 **構成ドキュメント** を処理することによって、ノードの構成が管理されます。 これらの ".mof" ファイルは、`C:\Windows\System32\Configuration` ディレクトリにあります。

PowerShell 5.0 以降では、[Remove-DSCConfigurationDocument](/powershell/module/PSDesiredStateConfiguration/Remove-DscConfigurationDocument) を使って ".mof" ファイルを削除することで、将来の整合性チェックを停止したり、適用されるときにエラーがある構成を削除したりできます。 `-Stage` パラメーターで、削除する ".mof" ファイルを指定できます。

```powershell
Remove-DSCConfigurationDocument -Stage Current
```

> [!NOTE]
> PowerShell 4.0 ではまだ、[Remove-Item](/powershell/module/microsoft.powershell.management/remove-item) を使って直接これらの ".mof" ファイルを削除することができます。

## <a name="publish-configurations"></a>構成を発行する

PowerShell 5.0 以降では、[Publish-DSCConfiguration](/powershell/module/PSDesiredStateConfiguration/Publish-DscConfiguration) コマンドレットが追加されています。 このコマンドレットを使うと、適用することなく、リモート コンピューターに ".mof" ファイルを発行できます。

```powershell
Publish-DscConfiguration -Path '$home\WebServer' -ComputerName "ContosoWebServer" -Credential (get-credential Contoso\webadministrator)
```

## <a name="see-also"></a>関連項目

- [取得、テスト、および設定](../resources/get-test-set.md)
