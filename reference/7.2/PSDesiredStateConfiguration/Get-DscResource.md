---
external help file: PSDesiredStateConfiguration-help.xml
Locale: en-US
Module Name: PSDesiredStateConfiguration
ms.date: 07/23/2020
online version: https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/get-dscresource?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-DscResource
ms.openlocfilehash: dbc036562da0172dc4406f169891d2cd1c5e4098
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602402"
---
# Get-DscResource

## 概要
コンピューターに存在する Desired State Configuration (DSC) リソースを取得します。

## SYNTAX

```
Get-DscResource [[-Name] <String[]>] [[-Module] <Object>] [-Syntax] [<CommonParameters>]
```

## Description

この `Get-DscResource` コマンドレットは、コンピューター上に存在する POWERSHELL DSC リソースを取得します。 このコマンドレットは、PSModulePath にインストールされているリソースのみを検出します。 これには、ユーザーが作成した組み込みプロバイダーとカスタムプロバイダーの詳細が表示されます。 このコマンドレットは、モジュールとしてパッケージ化されているか、セッションで実行時に作成された他の構成である複合リソースの詳細も表示します。

## 例

### 例 1: ローカルコンピューター上のすべてのリソースを取得する

```powershell
Get-DscResource
```

以下のコマンドは、ローカル コンピューター上のすべてのリソースを取得します。

### 例 2: 名前を指定してリソースを取得する

```powershell
Get-DscResource -Name "WindowsFeature"
```

以下のコマンドは、WindowsFeature リソースを取得します。

### 例 3: モジュールからすべてのリソースを取得する

```powershell
Get-DscResource -Module "xHyper-V"
```

このコマンドは、xHyper モジュールからすべてのリソースを取得します。

### 例 4: ワイルドカード文字を使用してリソースを取得する

```powershell
Get-DscResource -Name P*,r*
```

このコマンドは、 **Name** パラメーターで指定されたワイルドカードパターンに一致するすべてのリソースを取得します。

### 例 5: リソースの構文を取得する

```powershell
Get-DscResource -Name "WindowsFeature" -Syntax
```

以下のコマンドは、WindowsFeature リソースを取得し、リソースの構文を表示します。

### 例 6: リソースのすべてのプロパティを取得する

```powershell
Get-DscResource -Name "User" | Select-Object -ExpandProperty Properties
```

以下のコマンドは、User リソースを取得し、次にパイプライン演算子を使用して User リソースのすべてのプロパティを返します。

### 例 7: 指定したバージョンの指定したモジュールからすべてのリソースを取得する

```powershell
Get-DscResource -Module @{ModuleName='xHyper-V';RequiredVersion='3.0.0.0'}
```

このコマンドは、xHyper モジュールのバージョン3.0.0.0 ですべてのリソースを取得します。

## PARAMETERS

### -モジュール

DSC リソースを表示するモジュールの名前または完全修飾名を指定します。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Name

表示する DSC リソースの名前の配列を指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -構文

コマンドレットが、指定された DSC リソースの構文ビューを返すことを示します。 返された構文は、PowerShell スクリプトでリソースを使用する方法を示しています。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String[]

### System.Object

## 出力

### DesiredStateConfiguration []. DscResourceInfo []

### string[]

## 注

## 関連リンク

[PowerShell Desired State Configuration の概要](/powershell/scripting/dsc/overview/overview)

[Invoke-DscResource](/powershell/module/PSDesiredStateConfiguration/Invoke-DscResource)

