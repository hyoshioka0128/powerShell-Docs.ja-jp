---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-service?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Restart-Service
ms.openlocfilehash: fee113e20b178c2b86b8f95cd3147f991aed8efe
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346634"
---
# Restart-Service

## 概要
1 つ以上のサービスを停止し、再起動します。

## SYNTAX

### InputObject (既定値)

```
Restart-Service [-Force] [-InputObject] <ServiceController[]> [-PassThru] [-Include <String[]>]
 [-Exclude <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Default

```
Restart-Service [-Force] [-Name] <String[]> [-PassThru] [-Include <String[]>] [-Exclude <String[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### DisplayName

```
Restart-Service [-Force] [-PassThru] -DisplayName <String[]> [-Include <String[]>] [-Exclude <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Restart-Service`コマンドレットは、指定されたサービスの Windows サービスコントローラーに停止メッセージを送信した後、開始メッセージを送信します。 サービスが既に停止している場合は、エラーを通知せずに起動されます。 サービスは、サービス名または表示名で指定できます。また、 **InputObject** パラメーターを使用して、再起動する各サービスを表すオブジェクトを渡すこともできます。

## 例

### 例 1: ローカルコンピューター上のサービスを再起動する

```powershell
PS C:\> Restart-Service -Name winmgmt
```

このコマンドを実行すると、ローカル コンピューター上の Windows Management Instrumentation サービス (WinMgmt) が再起動されます。

### 例 2: サービスを除外する

```powershell
PS C:\> Restart-Service -DisplayName "net*" -Exclude "net logon"
```

このコマンドは、net Logon サービスを除き、Net で始まる表示名を持つサービスを再起動します。

### 例 3: 停止したすべてのネットワークサービスを開始する

```powershell
PS C:\> Get-Service -Name "net*" | Where-Object {$_.Status -eq "Stopped"} | Restart-Service
```

このコマンドを実行すると、コンピューター上で停止しているネットワーク サービスがすべて開始されます。

このコマンドは、 `Get-Service` コマンドレットを使用して、サービス名が net で始まるサービスを表すオブジェクトを取得します。 パイプライン演算子 () は、 `|` サービスオブジェクトをコマンドレットに送信し `Where-Object` ます。これにより、status が stopped のサービスだけが選択されます。 もう1つのパイプライン演算子は、選択したサービスをに送信し `Restart-Service` ます。

実際には、 **WhatIf** パラメーターを使用して、コマンドを実行する前にその効果を判断します。

## PARAMETERS

### -DisplayName

再起動するサービスの表示名を指定します。 ワイルドカード文字を使用できます。

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

ユーザーに確認せずに、直ちにコマンドを実行します。

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

このコマンドレットによって再起動されるサービスを指定します。 このパラメーターの値は、 **Name** パラメーターを修飾します。 「S *」のように、名前要素またはパターンを入力します。 ワイルドカード文字を使用できます。

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

再起動するサービスを表す **ServiceController** オブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

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

再起動するサービスのサービス名を指定します。

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

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、再起動されたサービスを表す **ServiceController** オブジェクトを生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- `Restart-Service` 現在のユーザーがこの操作を行うためのアクセス許可を持っている場合にのみ、サービスを制御できます。 コマンドが正常に機能しない場合は、必要なアクセス許可が与えられていない可能性があります。
- システム上のサービスのサービス名と表示名を確認するには、 `Get-Service` 「」と入力します。
  サービス名は [ **名前** ] 列に表示され、表示名は [ **DisplayName** ] 列に表示されます。

## 関連リンク

[Get-Service](Get-Service.md)

[New-Service](New-Service.md)

[Resume-Service](Resume-Service.md)

[Set-Service](Set-Service.md)

[Start-Service](Start-Service.md)

[Stop-Service](Stop-Service.md)

[Suspend-Service](Suspend-Service.md)

[Remove-Service](Remove-Service.md)
