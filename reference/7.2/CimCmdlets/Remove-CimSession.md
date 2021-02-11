---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/remove-cimsession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-CimSession
ms.openlocfilehash: a3ff2132c531df1ab19bf7149a55ea0df4a787a8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600573"
---
# Remove-CimSession

## 概要
1つ以上の CIM セッションを削除します。

## SYNTAX

### CimSessionSet (既定値)

```
Remove-CimSession [-CimSession] <CimSession[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ComputerNameSet

```
Remove-CimSession [-ComputerName] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### SessionIdSet

```
Remove-CimSession [-Id] <UInt32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceIdSet

```
Remove-CimSession -InstanceId <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### NameSet

```
Remove-CimSession -Name <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Remove-CimSession`コマンドレットでは、ローカルの PowerShell セッションから1つ以上の CIM セッションオブジェクトを削除します。

## 例

### 例 1: すべての CIM セッションを削除する

この例では、 [CimSession](Get-CimSession.md) コマンドレットを使用してローカルコンピューター上で使用可能なすべての CIM セッションを取得し、を使用してそれらを削除し `Remove-CimSession` ます。

```powershell
Get-CimSession | Remove-CimSession
```

### 例 2: 特定の CIM セッションを削除する

この例では、 **Id** 値が5の CIM セッションを削除します。

```powershell
Remove-CimSession -Id 5
```

### 例 3: WhatIf パラメーターを使用して削除する CIM セッションの一覧を表示する

この例では、共通パラメーター **WhatIf** を使用して、削除を実行しないように指定していますが、出力されるのは、完了した場合だけです。

```powershell
Remove-CimSession -Name a* -WhatIf
```

## PARAMETERS

### -CimSession

閉じる CIM セッションのセッションオブジェクトを指定します。

CIM セッションを含む変数、またはやコマンドレットなどの CIM セッションを作成または取得するコマンドを入力し [`New-CimSession`](New-CimSession.md) [`Get-CimSession`](Get-CimSession.md) ます。
詳細については、「 [about_CimSessions](../Microsoft.PowerShell.Core/About/about_CimSession.md)」を参照してください。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession[]
Parameter Sets: CimSessionSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -ComputerName

コンピューターの名前の配列を指定します。 指定したコンピューターに接続するセッションを削除します。 完全修飾ドメイン名 (FQDN) または NetBIOS 名を指定できます。

```yaml
Type: System.String[]
Parameter Sets: ComputerNameSet
Aliases: CN, ServerName

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Id

削除する CIM セッションの ID を指定します。 1つ以上の Id をコンマで区切って指定するか、範囲演算子 () を使用して `..` id の範囲を指定します。 **Id** は、現在の PowerShell セッションで CIM セッションを一意に識別する整数です。

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

削除する CIM セッションのインスタンス ID を指定します。 **InstanceId** は、CIM セッションを一意に識別するグローバル一意識別子 (GUID) です。 **インスタンス** id は、PowerShell で複数のセッションが実行されている場合でも一意です。

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

削除する CIM セッションのフレンドリ名を指定します。 このパラメーターには、ワイルドカード文字を使用できます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは、入力オブジェクトを受け入れません。

## 出力

### System.Object

このコマンドレットは、CIM セッション情報を格納するオブジェクトを返します。

## 注

## 関連リンク

[Get-CimSession](Get-CimSession.md)

[New-CimSession](New-CimSession.md)

[about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)

