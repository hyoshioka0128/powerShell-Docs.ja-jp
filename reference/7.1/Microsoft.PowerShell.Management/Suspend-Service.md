---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/suspend-service?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Suspend-Service
ms.openlocfilehash: c68b9f5d145c190cc786ee5da7a98e0fc6170ead
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347552"
---
# Suspend-Service

## 概要
実行中の 1 つ以上のサービスを中断 (一時停止) します。

## SYNTAX

### InputObject (既定値)

```
Suspend-Service [-InputObject] <ServiceController[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Default

```
Suspend-Service [-Name] <String[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### DisplayName

```
Suspend-Service [-PassThru] -DisplayName <String[]> [-Include <String[]>] [-Exclude <String[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

## Description

`Suspend-Service`コマンドレットは、指定された各サービスの Windows サービスコントローラーに中断メッセージを送信します。 中断している間も、サービスは実行されていますが、コマンドレットを実行するなどして、再開されるまでそのアクションは停止し `Resume-Service` ます。 サービスは、サービス名または表示名で指定できます。また、 **InputObject** パラメーターを使用して、中断するサービスを表すサービスオブジェクトを渡すこともできます。

## 例

### 例 1: サービスを中断する

```
PS C:\> Suspend-Service -DisplayName "Telnet"
```

このコマンドを実行すると、ローカル コンピューター上の Telnet サービス (Tlntsvr) が中断されます。

### 例 2: サービスを中断した場合の動作を表示する

```
PS C:\> Suspend-Service -Name lanman* -WhatIf
```

このコマンドは、サービス名が lanman で始まるサービスを中断した場合に何が起こるかを示します。 サービスを中断するには、 **WhatIf** パラメーターを指定せずにコマンドを再実行します。

### 例 3: サービスを取得して中断する

```
PS C:\> Get-Service schedule | Suspend-Service
```

このコマンドは、 `Get-Service` コマンドレットを使用して、コンピューター上のタスクスケジューラ (スケジュール) サービスを表すオブジェクトを取得します。 パイプライン演算子 () は、 `|` 結果をに渡します `Suspend-Service` 。これにより、サービスが中断されます。

### 例 4: 中断可能なすべてのサービスを中断する

```
PS C:\> Get-Service | Where-Object {$_.CanPauseAndContinue -eq "True"} | Suspend-Service -Confirm
```

このコマンドを実行すると、コンピューター上の中断可能なすべてのサービスが中断されます。 を使用して、 `Get-Service` コンピューター上のサービスを表すオブジェクトを取得します。 パイプライン演算子は、結果を `Where-Object` コマンドレットに渡します。このコマンドレットは、CanPauseAndContinue プロパティに値が設定されているサービスのみを選択し `$True` ます。 **CanPauseAndContinue** もう1つのパイプライン演算子は、結果をに渡し `Suspend-Service` ます。 **Confirm** パラメータは、各サービスを中断する前に確認メッセージを表示します。

## PARAMETERS

### -DisplayName

中断するサービスの表示名を指定します。 ワイルドカード文字を使用できます。

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

指定されたサービスから除外するサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 「s*」などの名前要素またはパターンを入力します。 ワイルドカード文字を使用できます。

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

中断するサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 「s*」などの名前要素またはパターンを入力します。 ワイルドカード文字を使用できます。

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

中断するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

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

中断するサービスのサービス名を指定します。 ワイルドカード文字を使用できます。

パラメーター名は省略可能です。 **名前** またはそのエイリアスである **ServiceName** を使用することも、パラメーター名を省略することもできます。

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

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、サービスを表す **ServiceController** オブジェクトを生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- `Suspend-Service` 現在のユーザーがこの操作を行うためのアクセス許可を持っている場合にのみ、サービスを制御できます。 コマンドが正常に機能しない場合は、必要なアクセス許可が与えられていない可能性があります。
- `Suspend-Service` 中断および再開がサポートされているサービスのみを中断できます。 特定のサービスを中断できるかどうかを判断するには、 `Get-Service` **CanPauseAndContinue** プロパティと共にコマンドレットを使用します。 たとえば、「 `Get-Service wmi | Format-List Name, CanPauseAndContinue` 」のように入力します。 コンピューター上の中断可能なすべてのサービスを検索するには、「」と入力 `Get-Service | Where-Object {$_.CanPauseAndContinue -eq $true}` します。
- システム上のサービスのサービス名と表示名を確認するには、「」と入力 `Get-Service` します。
  サービス名は [ **名前** ] 列に表示され、表示名は [ **DisplayName** ] 列に表示されます。

## 関連リンク

[Get-Service](Get-Service.md)

[New-Service](New-Service.md)

[Restart-Service](Restart-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Remove-Service](Remove-Service.md)
