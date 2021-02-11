---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/out-null?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-Null
ms.openlocfilehash: fe28f52088a82b93799724de7a7a3f20ce42e36b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599928"
---
# Out-Null

## 概要
パイプラインで送信するか、表示するのではなく、出力を非表示にします。

## SYNTAX

```
Out-Null [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

**Out null** コマンドレットは、出力を null に送信します。実際には、パイプラインから削除し、出力が画面に表示されないようにします。

## 例

### 例 1: 出力を削除する

```powershell
Get-ChildItem | Out-Null
```

このコマンドは、現在の場所/ディレクトリ内の項目を取得しますが、その出力はパイプラインから渡されず、コマンドラインにも表示されません。
これは、不要な出力を隠す場合に便利です。

## PARAMETERS

### -InputObject

(パイプラインから削除された) NULL に送信するオブジェクトを指定します。
オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをこのコマンドレットに渡します。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

* **Out** 動詞 ( **out** コマンドレット) を含むコマンドレットには、名前またはファイルパスのパラメーターはありません。 **Out** コマンドレットにデータを送信するには、パイプライン演算子 (|) を使用して、PowerShell コマンドの出力をコマンドレットに送信します。 変数にデータを格納し、*InputObject* パラメーターを使用してコマンドレットにデータを渡すこともできます。 詳細については、例を参照してください。
* **Out-Null** は出力オブジェクトを返しません。 パイプを使用して **Out-Null** の出力を Get-Member コマンドレットに渡した場合、 **Get Member** はオブジェクトが指定されていないことを報告します。

## 関連リンク

[Out-Default](Out-Default.md)

[Out-Host](Out-Host.md)

