---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 03/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/test-pssessionconfigurationfile?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-PSSessionConfigurationFile
ms.openlocfilehash: 6786210fb2b89102c6193c755168af4264fae58c
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94345801"
---
# Test-PSSessionConfigurationFile

## 概要
セッション構成ファイル内のキーと値を確認します。

## SYNTAX

```
Test-PSSessionConfigurationFile [-Path] <String> [<CommonParameters>]
```

## Description

このコマンドレットは、セッション構成ファイルに有効なキーが含まれていること、および値が正しい型であることを確認します。 列挙値の場合、コマンドレットは、指定した値が有効であることを検証します。

このコマンドレットは、 `$True` ファイルがすべてのテストに合格している場合はを返し、 `$False` そうでない場合はを返します。 エラーを検索するには、 **Verbose** パラメーターを使用します。

`Test-PSSessionConfigurationFile` コマンドレットによって作成されたものなど、セッション構成ファイルを検証し `New-PSSessionConfigurationFile` ます。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。 セッション構成ファイルの詳細については、「 [about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)」を参照してください。

このコマンドレットは、PowerShell 3.0 で導入されました。

## 例

### 例 1: セッション構成ファイルをテストする

```powershell
Test-PSSessionConfigurationFile -Path "FullLanguage.pssc"
```

```Output
True
```

### 例 2: セッション構成のセッション構成ファイルをテストする

この例では、 **制限** されたセッション構成で使用される構成ファイルをテストします。
**Path** パラメーターの値は、制限された `Get-PSSessionConfiguration` セッション構成を取得する **Restricted** コマンドの結果です。 セッション構成ファイルのパスは、セッション構成の **Configfilepath** プロパティの値に格納されます。

```powershell
Test-PSSessionConfigurationFile -Path (Get-PSSessionConfiguration -Name Restricted).ConfigFilePath
```

### 例 3: すべてのセッション構成ファイルをテストする

この例の関数は、ローカルコンピューター上のすべてのセッション構成ファイルをテストします。 関数は、コマンドレットを使用して `Get-PSSessionConfiguration` すべてのセッション構成を取得します。 ループ内のコードは、 `ForEach-Object` ファイルパスを表示し、各セッション構成をテストします。

```powershell
function Test-AllConfigFiles
{
    Get-PSSessionConfiguration | ForEach-Object {
        if ($_.ConfigFilePath) {
            $_.ConfigFilePath
            Test-PSSessionConfigurationFile -Verbose -Path $_.ConfigFilePath
        }
    }
}
Test-AllConfigFiles
```

```Output
C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\Empty_6fd77bf6-e084-4372-bd8a-af3e207354d3.pssc
True
C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\Full_1e9cb265-dae0-4bd3-89a9-8338a47698a1.pssc
VERBOSE: The member 'AliasDefinitions' must contain the required key 'Description'. Add the require key
to the fileC:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\Full_1e9cb265-dae0-4bd3-89a9-8338a47698a1.pssc.
False
C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\NoLanguage_0c115179-ff2a-4f66-a5eb-e56e5692ba22.pssc
True
C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\RestrictedLang_b6bd9474-0a6c-4e06-8722-c2c95bb10d3e.pssc
True
C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\RRS_3fb29420-2c87-46e5-a402-e21436331efc.pssc
True
```

セッション構成の **Configfilepath** プロパティには、セッション構成で使用されるセッション構成ファイルのパス (存在する場合) が含まれています。

**ConfigFilePath** プロパティの値が入力されている場合 (true の場合)、コマンドは **ConfigFilePath** プロパティの値を取得 (出力) します。 次に、コマンドレットを使用して、 `Test-PSSessionConfigurationFile` **configfilepath** 値内のファイルをテストします。 **Verbose** パラメーターは、ファイルがテストに合格しなかった場合に、ファイル エラーを返します。

## PARAMETERS

### -Path

セッション構成ファイル (.pssc) のパスとファイル名を指定します。 パスを省略した場合、既定値は現在のフォルダーになります。 ワイルドカード文字はサポートされていますが、1つのファイルに解決する必要があります。 パイプを使用して、セッション構成ファイルのパスをにパイプすることもでき `Test-PSSessionConfigurationFile` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、セッション構成ファイルのパスをにパイプすることができ `Test-PSSessionConfigurationFile` ます。

## 出力

### System.Boolean

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

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

[WSMan プロバイダー](../Microsoft.WsMan.Management/About/about_WSMan_Provider.md)

[about_Session_Configurations](About/about_Session_Configurations.md)

[about_Session_Configuration_Files](About/about_Session_Configuration_Files.md)
