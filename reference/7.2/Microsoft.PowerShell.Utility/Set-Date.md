---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/30/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/set-date?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-Date
ms.openlocfilehash: 0f35eea0dfca76b535aad3bdb663d55c224a11d2
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601194"
---
# Set-Date

## 概要
指定する時刻にコンピューターのシステム時刻を変更します。

## SYNTAX

### 日付 (既定値)

```
Set-Date [-Date] <DateTime> [-DisplayHint <DisplayHintType>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Adjust

```
Set-Date [-Adjust] <TimeSpan> [-DisplayHint <DisplayHintType>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

この `Set-Date` コマンドレットは、コンピューターのシステム日付と時刻を、指定した日付と時刻に変更します。
文字列を入力するか、 **DateTime** または **TimeSpan** オブジェクトをに渡して、新しい日付や時刻を指定でき `Set-Date` ます。 新しい日付または時刻を指定するには、 **date** パラメーターを使用します。
変更間隔を指定するには、[ **調整** ] パラメーターを使用します。

## 例

### 例 1: システム日付に3日を追加する

このコマンドは、現在のシステム日付に 3 日加算します。
時刻には影響しません。
このコマンド **は、date パラメーターを** 使用して日付を指定します。

この `Get-Date` コマンドレットは、現在の日付を **DateTime** オブジェクトとして返します。 **Datetime** オブジェクトの **AddDays** メソッドは、指定された日数 (3) を現在の **DateTime** オブジェクトに追加します。

```powershell
Set-Date -Date (Get-Date).AddDays(3)
```

### 例 2: システムクロックを10分前に設定する

この例では、現在のシステム時刻を10分後に設定します。

**調整** パラメーターを使用すると、ロケールの標準時形式で、変更の間隔 (10 分を差し引いた値) を指定できます。

**Displayhint** パラメーターは、時刻のみを表示するように PowerShell に指示しますが、を返す **DateTime** オブジェクトには影響しません `Set-Date` 。

```powershell
Set-Date -Adjust -0:10:0 -DisplayHint Time
```

### 例 3: 日付と時刻を変数値に設定する

これらのコマンドは、ローカルコンピューターのシステム日付と時刻を、変数に保存されている日付と時刻に変更し `$T` ます。 最初のコマンドは、日付を取得してに格納し `$T` ます。

2番目のコマンドは、 **Date** パラメーターを使用して、 **DateTime** オブジェクトを `$T` `Set-Date` コマンドレットに渡します。

```powershell
$T = Get-Date
Set-Date -Date $T
```

### 例 4: システムクロックに90分を加算する

これらのコマンドは、ローカル コンピューターのシステム時刻を 90 分進めます。

最初のコマンドは、コマンドレットを使用して、 `New-TimeSpan` 90 分の間隔で **TimeSpan** オブジェクトを作成し、変数に保存し `$90mins` ます。

2番目のコマンドは、の **調整** パラメーターを使用し `Set-Date` て、変数の **TimeSpan** オブジェクトの値で日付を調整し `$90mins` ます。

```powershell
$90mins = New-TimeSpan -Minutes 90
Set-Date -Adjust $90mins
```

## PARAMETERS

### -調整

このコマンドレットが現在の日付と時刻に対して加算または減算する値を指定します。
では、ロケールの標準の日付と時刻の形式で調整を入力するか、または **調整** パラメーターを使用して、 **TimeSpan** オブジェクトをからに渡すことができ `New-TimeSpan` `Set-Date` ます。

```yaml
Type: System.TimeSpan
Parameter Sets: Adjust
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -日付

指定した値に日付と時刻を変更します。
新しい日付をロケールに対応する短い日付形式で、時刻をロケールに対応する標準の時刻形式で入力できます。 または、 **DateTime** オブジェクトをから渡すことができ `Get-Date` ます。

日付を指定したのに時間を指定しなかった場合は、 `Set-Date` 指定した日付の午前0時に時刻が変更されます。 時刻だけを指定すると、日付は変わりません。

```yaml
Type: System.DateTime
Parameter Sets: Date
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -DisplayHint

日付と時刻の要素を表示するかどうかを指定します。このパラメーターに指定できる値は次のとおりです。

- **日付** -日付のみを表示します。
- **時間** -時刻のみを表示します。
- **DateTime** -日付と時刻を表示します。

このパラメーターは、表示のみに影響します。
を取得する **DateTime** オブジェクトには影響しません `Get-Date` 。

```yaml
Type: Microsoft.PowerShell.Commands.DisplayHintType
Parameter Sets: (All)
Aliases:
Accepted values: Date, Time, DateTime

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.DateTime

パイプを使用して日付をにパイプすることができ `Set-Date` ます。

## 出力

### System.DateTime

`Set-Date` 設定された日付を表すオブジェクトを返します。

## 注

- コンピューターの日付と時刻を変更する場合は、このコマンドレットを使用します。 変更によって、コンピューターが、日付または時刻に基づいてトリガーされるシステム全体のイベントと更新を受信できなくなる可能性があります。 エラーを回避するには、 **WhatIf** パラメーターと **Confirm** パラメーターを使用します。
-   `Set-Date` **AddDays**、 **addmonths**、 **Fromfiletime** など、で使用される DateTime および TimeSpan オブジェクトと共に、標準の .net メソッドを使用できます。 詳細については、「 [DateTime メソッド](/dotnet/api/system.datetime) 」と「」を参照してください。

  .NET SDK の[TimeSpan メソッド](/dotnet/api/system.timespan)。

## 関連リンク

[取得-日付](Get-Date.md)

[New-TimeSpan](New-TimeSpan.md)
