---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/clear-host?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Clear-Host
ms.openlocfilehash: de7dc6027653db063311bd34e1282e3cb5294e92
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600590"
---
# Clear-Host

## 概要

ホスト プログラムの表示を消去します。

## SYNTAX

```
Clear-Host [<CommonParameters>]
```

## Description

関数は、 `Clear-Host` 蓄積されたコマンドや出力を含む、現在の表示からすべてのテキストを削除します。 完了すると、コマンド プロンプトが表示されます。 関数名またはそのエイリアスであるを使用でき `cls` ます。

`Clear-Host` 現在のディスプレイのみに影響します。 保存した結果を削除したり、セッションから項目を削除したりすることはありません。 変数や関数など、セッション固有項目はこの関数で削除されません。

関数の動作は `Clear-Host` ホストプログラムによって決まるため、ホストプログラムによって動作が `Clear-Host` 異なる場合があります。

## 例

### 例 1

```
# Before

PS C:\> Get-Process

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    843      33    14428      22556    99    17.41   1688 CcmExec
     44       6     2196       4964    52     0.23    692 conhost
    646      12     2332       4896    49     1.12    388 csrss
    189      11     2860       7084   114     0.66   2896 csrss
     78      11     1876       4008    42     0.22   4000 csrss
     76       7     1848       5064    54     0.08   1028 dwm
    610      41    23952      44048   208     4.40   2080 explorer
      0       0        0         24     0               0 Idle
    182      32     7692      15980    91     0.23   3056 LogonUI
    186      25     7832      16068    91     0.27   3996 LogonUI
   1272      32    11512      20432    58    25.07    548 lsass
    267      10     3536       6736    34     0.80    556 lsm
    137      17     3520       7472    61     0.05   1220 msdtc
    447      31    70316      84476   201 1,429.67    836 MsMpEng
    265      18     7136      15628   134     2.20   3544 msseces
    248      16     6476       4076    76     0.22   1592 NisSrv
    368      25    61312      65508   614     1.78    848 powershell
    101       8     2304       6624    70     0.64   3648 rdpclip
    258      15     6804      12156    50     2.65    536 services
...

PS C:\> cls
#After

PS C:>
```

このコマンドは `cls` 、の別名 `Clear-Host` を使用して、現在の表示をクリアします。

## PARAMETERS

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

入力をにパイプすることはできません `Clear-Host` 。

## 出力

### なし

`Clear-Host` 出力を生成しません。

## 注

`Clear-Host` は、高度な関数ではなく、単純な関数です。 そのため、コマンドで **Debug** などの共通パラメーターを使用することはできません `Clear-Host` 。

## 関連リンク

[Get-Host](../Microsoft.PowerShell.Utility/Get-Host.md)

[Out-Host](Out-Host.md)

[Read-Host](../Microsoft.PowerShell.Utility/Read-Host.md)

[Write-Host](../Microsoft.PowerShell.Utility/Write-Host.md)

