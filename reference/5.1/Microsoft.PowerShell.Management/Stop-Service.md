---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/stop-service?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Stop-Service
ms.openlocfilehash: 77ce507d0eaf476e2c24f41e433bd69fdcb416bb
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94342571"
---
# Stop-Service

## 概要
実行中のサービスを 1 つ以上停止します。

## SYNTAX

### InputObject (既定値)

```
Stop-Service [-Force] [-NoWait] [-InputObject] <ServiceController[]> [-PassThru] [-Include <String[]>]
 [-Exclude <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Default

```
Stop-Service [-Force] [-NoWait] [-Name] <String[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### DisplayName

```
Stop-Service [-Force] [-NoWait] [-PassThru] -DisplayName <String[]> [-Include <String[]>] [-Exclude <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Stop-Service`コマンドレットは、指定された各サービスについて、Windows サービスコントローラーに停止メッセージを送信します。 サービスは、サービス名または表示名で指定できます。また、 **InputObject** パラメーターを使用して、停止するサービスを表すサービスオブジェクトを渡すこともできます。

## 例

### 例 1: ローカルコンピューター上のサービスを停止する

```
PS C:\> Stop-Service -Name "sysmonlog"
```

このコマンドを実行すると、ローカル コンピューター上のパフォーマンス ログと警告 (SysmonLog) サービスが停止されます。

### 例 2: 表示名を使用してサービスを停止する

```
PS C:\> Get-Service -DisplayName "telnet" | Stop-Service
```

このコマンドを実行すると、ローカル コンピューター上の Telnet サービスが停止されます。 このコマンドは、を使用して、 `Get-Service` Telnet サービスを表すオブジェクトを取得します。 パイプライン演算子 () は、 `|` オブジェクトをにパイプ処理して `Stop-Service` 、サービスを停止します。

### 例 3: 依存サービスを持つサービスを停止する

```
PS C:\> Get-Service -Name "iisadmin" | Format-List -Property Name, DependentServices
PS C:\> Stop-Service -Name "iisadmin" -Force -Confirm
```

この例では、ローカルコンピューター上の IISAdmin サービスを停止します。 このサービスを停止すると、IISAdmin サービスに依存するサービスも停止されるため、その前に、 `Stop-Service` iisadmin サービスに依存するサービスを一覧表示するコマンドを使用することをお勧めします。

最初のコマンドを実行すると、IISAdmin に依存するサービスの一覧が表示されます。 を使用して、 `Get-Service` IISAdmin サービスを表すオブジェクトを取得します。 パイプライン演算子 () は、 `|` 結果を `Format-List` コマンドレットに渡します。 このコマンドは、の **Property** パラメーターを使用して、 `Format-List` サービスの **Name** プロパティと **dependentservices** プロパティのみを一覧表示します。

2 番目のコマンドを実行すると、IISAdmin サービスが停止します。 **Force** パラメーターは、依存サービスを持つサービスを停止するために必要です。 このコマンドでは、 **Confirm** パラメーターを使用して、各サービスを停止する前にユーザーに確認を要求します。

## PARAMETERS

### -DisplayName

停止するサービスの表示名を指定します。
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

### -Force

サービスに依存するサービスがある場合でも、強制的にサービスを停止するようにコマンドレットを実行します。

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

### -Include

このコマンドレットが停止するサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 「S *」のように、名前要素またはパターンを入力します。 ワイルドカード文字を使用できます。

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

停止するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

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

停止するサービスのサービス名を指定します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases: ServiceName

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -NoWait

このコマンドレットが no wait オプションを使用することを示します。

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

このコマンドレットには、サービスオブジェクトまたはサービス名を含む文字列をパイプすることができます。

## 出力

### None、ServiceController。

このコマンドレットは、 **PassThru** パラメーターを使用する場合に、サービスを表す **ServiceController** オブジェクトを生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

また、 `Stop-Service` 組み込みエイリアスである **spsv** でを参照することもできます。 詳細については、「about_Aliases」を参照してください。

`Stop-Service` 現在のユーザーがこの操作を行うためのアクセス許可を持っている場合にのみ、サービスを制御できます。 コマンドが正常に機能しない場合は、必要なアクセス許可が与えられていない可能性があります。

システム上のサービスのサービス名と表示名を確認するには、「」と入力 `Get-Service` します。 サービス名は [ **名前** ] 列に表示され、表示名は [ **DisplayName** ] 列に表示されます。

## 関連リンク

[Get-Service](Get-Service.md)

[New-Service](New-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Suspend-Service](Suspend-Service.md)
