---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/get-cimsession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-CimSession
ms.openlocfilehash: 0e531b3cc072b7cc1efe0ef06fb741326bf3a72b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602181"
---
# Get-CimSession

## 概要
現在のセッションから CIM セッションオブジェクトを取得します。

## SYNTAX

### ComputerNameSet (既定値)

```
Get-CimSession [[-ComputerName] <String[]>] [<CommonParameters>]
```

### SessionIdSet

```
Get-CimSession [-Id] <UInt32[]> [<CommonParameters>]
```

### InstanceIdSet

```
Get-CimSession -InstanceId <Guid[]> [<CommonParameters>]
```

### NameSet

```
Get-CimSession -Name <String[]> [<CommonParameters>]
```

## Description

既定では、コマンドレットは、現在の PowerShell セッションで作成されたすべての CIM セッションを取得します。 のパラメーターを使用して、 `Get-CimSession` 特定のコンピューターのセッションを取得したり、名前や他の識別子でセッションを識別したりすることができます。 `Get-CimSession` は、他の PowerShell セッションで作成された、または他のコンピューターで作成された CIM セッションを取得しません。

CIM セッションの詳細については、「 [about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)」を参照してください。

## 例

### 例 1: 現在の PowerShell セッションから CIM セッションを取得する

この例では、 [CimSession](New-CimSession.md)を使用して cim セッションを作成し、を使用して cim セッションを取得し `Get-CimSession` ます。

```powershell
New-CimSession -ComputerName Server01,Server02
Get-CimSession
```

```Output
Id           : 1
Name         : CimSession1
InstanceId   : d1413bc3-162a-4cb8-9aec-4d2c61253d59
ComputerName : Server01
Protocol     : WSMAN

Id           : 2
Name         : CimSession2
InstanceId   : c0095981-52c5-4e7f-a5bb-c4c680541710
ComputerName : Server02
Protocol     : WSMAN
```

### 例 2: 特定のコンピューターへの CIM セッションを取得する

この例では、 **Server02** という名前のコンピューターに接続されている CIM セッションを取得します。

```powershell
Get-CimSession -ComputerName Server02
```

```Output
Id           : 2
Name         : CimSession2
InstanceId   : c0095981-52c5-4e7f-a5bb-c4c680541710
ComputerName : Server02
Protocol     : WSMAN
```

### 例 3: CIM セッションの一覧を取得し、一覧の書式を設定する

この例では、現在の PowerShell セッションのすべての CIM セッションを取得し、 **ComputerName** プロパティと **InstanceID** プロパティのみを含むテーブルを表示します。

```powershell
Get-CimSession | Format-Table -Property ComputerName,InstanceId
```

```Output
ComputerName InstanceId
------------ ----------
Server01     d1413bc3-162a-4cb8-9aec-4d2c61253d59
Server02     c0095981-52c5-4e7f-a5bb-c4c680541710
```

### 例 4: 特定の名前を持つすべての CIM セッションを取得する

この例では、名前が **serv** で始まるすべての CIM セッションを取得します。

```powershell
Get-CimSession -ComputerName Serv*
```

```Output
Id           : 1
Name         : CimSession1
InstanceId   : d1413bc-162a-4cb8-9aec-4d2c61253d59
ComputerName : Server01
Protocol     : WSMAN

Id           : 2
Name         : CimSession2
InstanceId   : c0095981-52c5-4e7f-a5bb-c4c680541710
ComputerName : Server02
Protocol     : WSMAN
```

### 例 5: 特定の CIM セッションを取得する

この例では、 **Id** が2の CIM セッションを取得します。

```powershell
Get-CimSession -ID 2
```

```Output
Id           : 2
Name         : CimSession2
InstanceId   : c0095981-52c5-4e7f-a5bb-c4c680541710
ComputerName : Server02
Protocol     : WSMAN
```

## PARAMETERS

### -ComputerName

CIM セッションの接続先となるコンピューターの名前を指定します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: ComputerNameSet
Aliases: CN, ServerName

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Id

取得する CIM セッションの識別子を指定します。 複数の Id については、コンマを使用して Id を区切るか、範囲演算子 () を使用して `..` id の範囲を指定します。 **Id** は、現在の PowerShell セッション内の CIM セッションを一意に識別する整数です。

範囲演算子の詳細については、「 [about_Operators](../Microsoft.PowerShell.Core/About/about_Operators.md)」を参照してください。

```yaml
Type: System.UInt32[]
Parameter Sets: SessionIdSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InstanceId

取得する CIM セッションのインスタンス Id を指定します。

**InstanceId** は、CIM セッションを一意に識別するグローバル一意識別子 (GUID) です。 **インスタンス** id は、PowerShell で複数のセッションが実行されている場合でも一意です。

**Instanceid** は、CIM セッションを表すオブジェクトの **instanceid** プロパティに格納されます。

```yaml
Type: System.Guid[]
Parameter Sets: InstanceIdSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

指定されたフレンドリ名を含む1つ以上の CIM セッションを取得します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: NameSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

## 出力

### Microsoft.Management.Infrastructure.CimSession

## 注

## 関連リンク

[Format-Table](../microsoft.powershell.utility/format-table.md)

[New-CimSession](New-CimSession.md)

[Remove-CimSession](remove-cimsession.md)

[about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)

