---
external help file: Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Diagnostics
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.diagnostics/new-winevent?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-WinEvent
ms.openlocfilehash: 5b4b150a1c02e5d0689b44db9b2a984e297db766
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599553"
---
# New-WinEvent

## 概要
指定したイベント プロバイダーの新しい Windows イベントを作成します。

## SYNTAX

```
New-WinEvent [-ProviderName] <String> [-Id] <Int32> [-Version <Byte>] [[-Payload] <Object[]>]
 [<CommonParameters>]
```

## Description

**New-WinEvent** コマンドレットは、イベント プロバイダーの Windows イベント トレーシング (ETW) イベントを作成します。
このコマンドレットを使用すると、PowerShell から ETW チャネルにイベントを追加できます。

## 例

### 例 1

```powershell
PS C:\> New-WinEvent -ProviderName Microsoft-Windows-PowerShell -Id 45090 -Payload @("Workflow", "Running")
```

このコマンドは、**New-WinEvent** コマンドレットを使用して、Microsoft-Windows-PowerShell プロバイダーのイベント 45090 を作成します。

## PARAMETERS

### -Id

インストルメンテーション マニフェストで登録されたイベント ID を指定します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ペイロード

イベントのメッセージを指定します。 イベントがイベント ログに書き込まれると、ペイロードがイベント オブジェクトの **Message** プロパティに格納されます。

指定されたペイロードがイベント定義のペイロードと一致しない場合、PowerShell は警告を生成しますが、コマンドは成功します。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProviderName

"Microsoft-Windows-PowerShell" など、イベント ログにイベントを書き込むイベント プロバイダーを指定します。 ETW イベント プロバイダーは、イベントを ETW セッションに書き込む論理エンティティです。

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

### -Version

イベントのバージョン番号を指定します。 イベント番号を入力します。 この数値は、PowerShell によって必要なバイト型に変換されます。

このパラメーターを使用すると、同じイベントの異なるバージョンが定義されているときにイベントを指定できます。

```yaml
Type: System.Byte
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

### なし

このコマンドレットはパイプラインから入力を取得しません。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

* プロバイダーがを eventlog に書き込んでいる場合は、Get-WinEvent コマンドレットを使用してイベントログからイベントを取得できます。

## 関連リンク

[Get-WinEvent](Get-WinEvent.md)

