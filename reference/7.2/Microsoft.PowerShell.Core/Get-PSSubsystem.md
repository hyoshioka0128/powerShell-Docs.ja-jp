---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 10/15/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-pssubsystem?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSSubsystem
ms.openlocfilehash: 19129eb8a0b478d10fdbf1e35f15b7f86969b5fb
ms.sourcegitcommit: 71173a89c4f05b5283ccd1e885a780773c13fa47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2021
ms.locfileid: "103194983"
---
# Get-PSSubsystem

## 概要
PowerShell に登録されているサブシステムに関する情報を取得します。

## SYNTAX

### GetAllSet (既定値)

```
Get-PSSubsystem [<CommonParameters>]
```

### GetByKindSet

```
Get-PSSubsystem -Kind <SubsystemKind> [<CommonParameters>]
```

### GetByTypeSet

```
Get-PSSubsystem -SubsystemType <Type> [<CommonParameters>]
```

## Description

PowerShell に登録されているサブシステムに関する情報を取得します。

> [!NOTE]
> これは試験段階の機能です。 このコマンドレットは、機能が有効になっている場合にのみ使用でき `PSSubsystemPluginModel` ます。 詳細については、「[試験的な機能の使用](/powershell/scripting/learn/experimental-features)」を参照してください。

この機能により、`System.Management.Automation.dll` のコンポーネントを、独自のアセンブリに存在する個々のサブシステムに分けることができます。 この分割により、コア PowerShell エンジンのディスク占有領域が削減され、これらのコンポーネントを最小限の PowerShell インストールに対するオプション機能にすることができます。

現時点では、**CommandPredictor** サブシステムのみがサポートされています。 このサブシステムは、カスタム予測プラグインを提供するために、PSReadLine モジュールと共に使用されます。 今後、**Job**、**CommandCompleter**、**Remoting** などのコンポーネントは、`System.Management.Automation.dll` 外のサブシステム アセンブリに分割される可能性があります。

## 例

### 例 1-使用可能なすべてのサブシステムを表示する

```powershell
Get-PSSubsystem
```

```Output
Kind              SubsystemType     IsRegistered Implementations
----              -------------     ------------ ---------------
CommandPredictor  ICommandPredictor        False {}
```

### 例 2-特定の種類の使用可能なすべてのサブシステムを表示する

```powershell
PS> Get-PSSubsystem -Kind CommandPredictor | Format-List
```

```Output
Kind                      : CommandPredictor
SubsystemType             : System.Management.Automation.Subsystem.ICommandPredictor
AllowUnregistration       : True
AllowMultipleRegistration : True
RequiredCmdlets           : {}
RequiredFunctions         : {}
IsRegistered              : False
Implementations           : {}
```

## PARAMETERS

### -Kind


返されるサブシステムの種類を指定します。 有効な値は `CommandPredictor` です。

```yaml
Type: System.Management.Automation.Subsystem.SubsystemKind
Parameter Sets: GetByKindSet
Aliases:
Accepted values: CommandPredictor

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -SubsystemType

返されるサブシステムの種類を指定します。

```yaml
Type: System.Type
Parameter Sets: GetByTypeSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム.... サブシステムの種類

### System.Type

## 出力

### システム... Subsystem. SubsystemInfo

## 注

## 関連リンク

[about_experimental_features](about/about_experimental_features.md)

[Disable-ExperimentalFeature](Disable-ExperimentalFeature.md)

[Enable-ExperimentalFeature](Get-ExperimentalFeature.md)

[Get-ExperimentalFeature](Get-ExperimentalFeature.md)

[試験的機能の使用](/powershell/scripting/learn/experimental-features)
