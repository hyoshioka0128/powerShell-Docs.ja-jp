---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-pssessioncapability?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSSessionCapability
ms.openlocfilehash: e81302a442294a7eeab81c6e8e1c6b7fd0cfb5fe
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603049"
---
# Get-PSSessionCapability

## 概要
制約付きセッション構成で特定のユーザーの機能を取得します。

## SYNTAX

```
Get-PSSessionCapability [-ConfigurationName] <String> [-Username] <String> [-Full] [<CommonParameters>]
```

## Description

**Get PSSessionCapability** コマンドレットは、制約付きセッション構成で特定のユーザーの機能を取得します。
このコマンドレットを使用して、ユーザーのカスタマイズされたセッション構成を監査します。

Windows PowerShell 5.0 以降では、セッション構成 (.pssc) ファイルで **Roledefinitions** プロパティを使用できます。
このプロパティを使用すると、グループのメンバーシップに基づいて、1つの制約付きエンドポイントに対してユーザーのさまざまな機能を与えることができます。
**Get PSSessionCapability** コマンドレットを使用すると、ユーザーに付与されている正確な機能を決定できるため、これらのエンドポイントを監査する際の複雑さが軽減されます。

既定では、 **取得-PSSessionCapability ビリティ** コマンドレットは、指定されたユーザーが指定したエンドポイントで実行できるコマンドの一覧を返します。
これは、指定されたエンドポイントで **Get コマンド** を実行しているユーザーに相当します。
*Full* パラメーターを指定して実行すると、このコマンドレットは **InitialSessionState** オブジェクトを返します。
このオブジェクトには、指定したユーザーが指定したエンドポイントに対して操作する PowerShell 実行空間に関する詳細が含まれます。
これには、言語モード、実行ポリシー、および環境変数などの情報が含まれます。

## 例

### 例 1: ユーザーが使用できるコマンドを取得する

```powershell
Get-PSSessionCapability -ConfigurationName Endpoint1 -Username 'CONTOSO\User'
```

この例では、ローカルコンピューター上の Endpoint1 の制約付きエンドポイントに接続するときに、ユーザー CONTOSO\User が使用できるコマンドを返します。

### 例 2: ユーザーの実行空間に関する詳細を取得する

```powershell
Get-PSSessionCapability -ConfigurationName Endpoint1 -Username 'CONTOSO\User' -Full
```

この例では、Endpoint1 の制約付きエンドポイントに接続するときに、ユーザー CONTOSO\User が操作する実行空間に関する詳細を返します。

## PARAMETERS

### -ConfigurationName

検査する制約付きセッション構成 (エンドポイント) を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Full

このコマンドレットが、指定された制約付きエンドポイントで、指定したユーザーの初期セッション状態全体を返すことを示します。

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

### -Username

検査する機能を持つユーザーを指定します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

## 出力

### システム管理. エイリアス情報

### システム管理. FunctionInfo

### System.Management.Automation.Runspaces.InitialSessionState

## 注

## 関連リンク

[New-PSRoleCapabilityFile](New-PSRoleCapabilityFile.md)

