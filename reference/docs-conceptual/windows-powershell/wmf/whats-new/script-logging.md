---
ms.date: 06/12/2017
title: スクリプトのトレースとログ
description: Windows PowerShell 5.x では、スクリプト ブロックの実行を監査できる新しいイベント ログが追加されています。
ms.openlocfilehash: d47fb6fdd1ee4b9372fab7b81e6dc94fb45b8880
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663117"
---
# <a name="script-tracing-and-logging"></a>スクリプトのトレースとログ

PowerShell には、コマンドレットの呼び出しをログに記録する **LogPipelineExecutionDetails** グループ ポリシーの設定が既にありますが、PowerShell のスクリプト言語には、ログ記録や監査の対象にしたい場合がある機能がいくつかあります。 新しい詳細スクリプト トレース機能では、システムでの PowerShell スクリプト アクティビティの詳細な追跡や分析が提供されます。 詳細なスクリプト トレースを有効にすると、PowerShell ではすべてのスクリプト ブロックが ETW イベント ログ **Microsoft-Windows-PowerShell/Operational** に記録されます。 スクリプト ブロックで別のスクリプト ブロックが作成された場合 (たとえば、`Invoke-Expression` を呼び出すことによって)、呼び出されたスクリプト ブロックも記録されます。

ログ記録は、 **[管理用テンプレート]**  ->  **[Windows コンポーネント]**  ->  **[Windows PowerShell]** の **[PowerShell スクリプト ブロックのログ記録を有効にする]** グループ ポリシー設定で有効にします。

イベントは次のとおりです。

| チャネル |                               運用時                               |
| ------- | ----------------------------------------------------------------------- |
| Level   | "詳細"                                                                 |
| オペコード  | 作成                                                                  |
| タスク    | CommandStart                                                            |
| Keyword | Runspace                                                                |
| EventId | Engine_ScriptBlockCompiled (0x1008 = 4104)                              |
| Message | Creating Scriptblock text (%1 of %2): </br> %3 </br> ScriptBlock ID: %4 |

メッセージに埋め込まれたテキストは、コンパイルされたスクリプト ブロックの範囲です。 ID は、スクリプト ブロックの有効期間に保持される GUID です。

詳細ログ記録の機能を有効にすると、次のように開始マーカーと終了マーカーが書き込まれます。

| チャネル |                                 運用時                                |
| ------- | -------------------------------------------------------------------------- |
| Level   | "詳細"                                                                    |
| オペコード  | Open / Close                                                               |
| タスク    | CommandStart / CommandStop                                                 |
| Keyword | Runspace                                                                   |
| EventId | ScriptBlock\_Invoke\_Start\_Detail (0x1009 = 4105) / </br> ScriptBlock\_Invoke\_Complete\_Detail (0x100A = 4106) |
| Message | Started / Completed invocation of ScriptBlock ID: %1 </br> Runspace ID: %2 |

ID は (イベント ID 0x1008 に関連付けることができる) スクリプト ブロックを表す GUIDで、Runspace ID はこのスクリプト ブロックが実行された実行空間を表します。

呼び出しメッセージ内のパーセント記号は、構造化 ETW プロパティを表します。 これらはメッセージ テキストで実際の値に置き換えられますが、Get-WinEvent コマンドレットを使用してメッセージを取得し、メッセージの **Properties** 配列を使用すると、より信頼性の高い方法でそれらにアクセスできます。

この機能を利用して、スクリプトを暗号化および難読化する悪意のある試みをラップ解除する方法の例を次に示します。

```powershell
## Malware
function SuperDecrypt
{
    param($script)
    $bytes = [Convert]::FromBase64String($script)

    ## XOR "encryption"
    $xorKey = 0x42
    for($counter = 0; $counter -lt $bytes.Length; $counter++)
    {
        $bytes[$counter] = $bytes[$counter] -bxor $xorKey
    }
    [System.Text.Encoding]::Unicode.GetString($bytes)
}

$decrypted = SuperDecrypt "FUIwQitCNkInQm9CCkItQjFCNkJiQmVCEkI1QixCJkJlQg=="
Invoke-Expression $decrypted
```

このことを実行すると、次のログ エントリが生成されます。

```Output
Compiling Scriptblock text (1 of 1):
function SuperDecrypt
{
    param($script)
    $bytes = [Convert]::FromBase64String($script)
    ## XOR "encryption"
    $xorKey = 0x42
    for($counter = 0; $counter -lt $bytes.Length; $counter++)
    {
        $bytes[$counter] = $bytes[$counter] -bxor $xorKey
    }
    [System.Text.Encoding]::Unicode.GetString($bytes)

}
ScriptBlock ID: ad8ae740-1f33-42aa-8dfc-1314411877e3

Compiling Scriptblock text (1 of 1):
$decrypted = SuperDecrypt "FUIwQitCNkInQm9CCkItQjFCNkJiQmVCEkI1QixCJkJlQg=="
ScriptBlock ID: ba11c155-d34c-4004-88e3-6502ecb50f52

Compiling Scriptblock text (1 of 1):
Invoke-Expression $decrypted
ScriptBlock ID: 856c01ca-85d7-4989-b47f-e6a09ee4eeb3

Compiling Scriptblock text (1 of 1):
Write-Host 'Pwnd'
ScriptBlock ID: 5e618414-4e77-48e3-8f65-9a863f54b4c8
```

スクリプト ブロックの長さが単一のイベントの容量を超える場合、PowerShell ではスクリプトが複数の部分に分割されます。 ログ メッセージからスクリプトを再結合するサンプル コードを次に示します。

```powershell
$created = Get-WinEvent -FilterHashtable @{ ProviderName="Microsoft-Windows-PowerShell"; Id = 4104 } |
  Where-Object { $_.<...> }
$sortedScripts = $created | sort { $_.Properties[0].Value }
$mergedScript = -join ($sortedScripts | % { $_.Properties[2].Value })
```

保持するバッファーが限られているすべてのログ システムと同様、このインフラストラクチャを攻撃する 1 つの方法は、正しくないイベントでログを溢れさせて以前の証拠を隠すことです。 この攻撃を防ぐには、Windows イベント転送に何らかの形式のイベント ログ コレクションを設定します。 詳細については、「[Spotting the Adversary with Windows Event Log Monitoring](https://apps.nsa.gov/iaarchive/library/reports/spotting-the-adversary-with-windows-event-log-monitoring.cfm)」 (Windows イベント ログ監視による敵対者の偵察) を参照してください。
