---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 04/26/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/out-default?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-Default
ms.openlocfilehash: d650a9280982703e09ec22698c3848a616abb067
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603218"
---
# Out-Default

## 概要
既定のフォーマッタおよび既定の出力コマンドレットに出力を送信します。

## SYNTAX

```
Out-Default [-Transcript] [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

PowerShell `Out-Default` は、すべてのパイプラインの末尾にを自動的に追加します。 `Out-Default` オブジェクトストリームを書式設定し、出力する方法を決定します。 オブジェクトストリームが文字列のストリームである場合は、 `Out-Default` これらのを直接パイプし `Out-Host` て、ホストによって提供される適切な api を呼び出します。 オブジェクトストリームに文字列が含まれていない場合は、 `Out-Default` オブジェクトを調べて何を行うかを決定します。
まず、オブジェクトの種類を確認し、このオブジェクトの種類に対して登録済みの _ビュー_ があるかどうかを判断します。

PowerShell では、XML スキーマと、 `Update-FormatData` 任意のユーザーがオブジェクトの種類のビューを登録できるメカニズム (コマンドレット) が定義されています。 任意のオブジェクトの種類に対して、 **横**、 **一覧**、 **テーブル**、または **カスタム** ビューを指定できます。 ビューでは、表示するプロパティと表示方法を指定します。 ビューが登録されている場合は、使用するフォーマッタを定義します。 そのため、登録されたビューが **テーブル** ビューの場合、は `Out-Default` オブジェクトをにストリームし `Format-Table | Out-Host` ます。 `Format-Table` オブジェクトを (ビュー定義のデータによって駆動される) 書式設定レコードのストリームに変換し、その `Out-Host` 書式設定レコードをホストインターフェイスの呼び出しに変換します。

## 例

### 例 1

このコマンドレットはエンドユーザーが直接実行することを意図していませんが、にすることもできます。

```powershell
Get-Process | Select-Object -First 5 | Out-Default
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     12     2.56       5.20       0.00    7376   0 aesm_service
     48    34.32      18.10      26.64    9320  13 AlertusDesktopAlert
     24    13.97      12.74       0.77   12656  13 ApplicationFrameHost
      8     1.79       4.41       0.00    8180   0 AppVShNotify
      9     1.99       5.07       0.19   19320  13 AppVShNotify
```

## PARAMETERS

### -InputObject

コマンドレットへの入力を受け入れます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -トランスクリプト

出力を PowerShell の議事録サービスに送信するかどうかを決定します。

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

## 出力

## 注

## 関連リンク

[Format-Custom](../Microsoft.PowerShell.Utility/Format-Custom.md)

[Format-List](../Microsoft.PowerShell.Utility/Format-List.md)

[Format-Table](../Microsoft.PowerShell.Utility/Format-Table.md)

[Format-Wide](../Microsoft.PowerShell.Utility/Format-Wide.md)

[about_Format.ps1xml](About/about_Format.ps1xml.md)

[PowerShell の書式設定と出力が実際に動作するしくみ](https://devblogs.microsoft.com/powershell/how-powershell-formatting-and-outputting-really-works/)

