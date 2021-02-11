---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/resume-service?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Resume-Service
ms.openlocfilehash: 8ce2182117167d93c8f7abd86eeb2c79abdf09c8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602777"
---
# Resume-Service

## 概要
中断 (一時停止) 中のサービスを 1 つ以上再開します。

## SYNTAX

### InputObject (既定値)

```
Resume-Service [-InputObject] <ServiceController[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Default

```
Resume-Service [-Name] <String[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### DisplayName

```
Resume-Service [-PassThru] -DisplayName <String[]> [-Include <String[]>] [-Exclude <String[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Description

`Resume-Service`コマンドレットは、指定された各サービスの Windows サービスコントローラーに再開メッセージを送信します。 サービスが中断されている場合は、再開されます。 現在実行中の場合、メッセージは無視されます。 サービスは、サービス名または表示名で指定できます。また、 **InputObject** パラメーターを使用して、再開するサービスを表すサービスオブジェクトを渡すこともできます。

## 例

### 例 1: ローカルコンピューター上のサービスを再開する

```
PS C:\> Resume-Service "sens"
```

このコマンドは、ローカルコンピューター上のシステムイベント通知サービスを再開します。 サービス名は、コマンドでは、sens によって表されます。 このコマンドは、 **name** パラメーターを使用してサービスのサービス名を指定しますが、パラメーター名は省略可能であるため、コマンドはパラメーター名を省略します。

### 例 2: すべての中断されたサービスを再開する

```
PS C:\> Get-Service | Where-Object {$_.Status -eq "Paused"} | Resume-Service
```

このコマンドは、コンピューター上のすべての中断されたサービスを再開します。 コマンドレットコマンドを実行すると、 `Get-Service` コンピューター上のすべてのサービスが取得されます。 パイプライン演算子 () は、 `|` 結果を `Where-Object` コマンドレットに渡します。このコマンドレットは、 **Status** プロパティが一時停止になっているサービスを選択します。 次のパイプライン演算子は、結果をに送信します `Resume-Service` 。これにより、一時停止したサービスが再開されます。

実際には、 **WhatIf** パラメーターを使用して、コマンドを実行する前にその効果を判断します。

## PARAMETERS

### -DisplayName

再開するサービスの表示名を指定します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: DisplayName
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -除外

このコマンドレットで省略されるサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 「S *」のように、名前要素またはパターンを入力します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Include

再開するサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 「S *」のように、名前要素またはパターンを入力します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -InputObject

再開するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.ServiceProcess.ServiceController[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

再開するサービスのサービス名を指定します。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases: ServiceName

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -PassThru

サービスを表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### ServiceController (System.string)、System.string

このコマンドレットには、サービス名を含むサービスオブジェクトまたは文字列をパイプすることができます。

## 出力

### None、ServiceController。

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、再開されたサービスを表す **ServiceController** オブジェクトを生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- 中断されたサービスの状態は一時停止されます。 サービスが再開されると、その状態は [実行中] になります。
- `Resume-Service` 現在のユーザーがこの操作を行うためのアクセス許可を持っている場合にのみ、サービスを制御できます。 コマンドが正常に機能しない場合は、必要なアクセス許可が与えられていない可能性があります。
- システム上のサービスのサービス名と表示名を確認するには、「」と入力 `Get-Service` します。
  サービス名は [ **名前** ] 列に表示され、表示名は [ **DisplayName** ] 列に表示されます。

## 関連リンク

[Get-Service](Get-Service.md)

[New-Service](New-Service.md)

[Restart-Service](Restart-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)

[Remove-Service](Remove-Service.md)
