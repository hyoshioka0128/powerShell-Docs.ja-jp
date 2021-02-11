---
ms.date: 06/12/2017
title: WMF 5.1 のコンソール機能強化
description: WMF 5.1 では、Windows PowerShell 5.1 のコンソール エクスペリエンスに新機能が追加されています。
ms.openlocfilehash: 9a86a2ed4787554e7255bedf1c2ae6e798fefa45
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92660761"
---
# <a name="console-improvements-in-wmf-51"></a>WMF 5.1 のコンソール機能強化

## <a name="powershell-console-improvements"></a>PowerShell コンソールの機能強化

コンソールのエクスペリエンスを改善するため、WMF 5.1 の powershell.exe が次のように変更されました。

### <a name="vt100-support"></a>VT100 のサポート

Windows 10 では、[VT100 エスケープ シーケンス](/windows/console/console-virtual-terminal-sequences)のサポートが追加されました。
PowerShell は、テーブルの幅を計算するとき、特定の VT100 書式指定エスケープ シーケンスを無視します。

また、PowerShell に追加された新しい API を書式指定コードで使用すると、VT100 がサポートされているかどうかを判定できます。 次に例を示します。

```powershell
if ($host.UI.SupportsVirtualTerminal)
{
    $esc = [char]0x1b
    "A yellow ${esc}[93mhello${esc}[0m"
}
else
{
    "A default hello"
}
```

これは、[ からの一致を強調表示するために使用できる完全な](https://gist.github.com/lzybkr/dcb973dccd54900b67783c48083c28f7)例`Select-String`です。 例を `MatchInfo.format.ps1xml` という名前のファイルに保存した後、使用するには、プロファイルまたはその他の場所で、`Update-FormatData -Prepend MatchInfo.format.ps1xml` を実行します。

VT100 エスケープ シーケンスは、Windows 10 Anniversary Update 以降でのみサポートされることに注意してください。
それより前のシステムではサポートされません。

### <a name="vi-mode-support-in-psreadline"></a>PSReadline での vi モードのサポート

[PSReadline](https://github.com/PowerShell/PSReadLine) は、vi モードのサポートを追加します。 vi モードを使用するには、`Set-PSReadlineOption -EditMode Vi` を実行します。

### <a name="redirected-stdin-with-interactive-input"></a>対話型の入力を使用する stdin のリダイレクト

以前のバージョンでは、stdin がリダイレクトされ、コマンドを対話的に入力するときは、PowerShell を `powershell -File -` で起動する必要がありました。

WMF 5.1 では、この見つけにくいオプションは不要になりました。 オプションを何も使わずに PowerShell を起動できます。

PSReadline ではリダイレクトされた stdin はサポートされておらず、リダイレクトされた stdin での組み込みコマンド ライン編集エクスペリエンスは、非常に限られたものであることに注意してください (たとえば、方向キーは機能しません)。 PSReadline の将来のリリースではこの問題が対処されるはずです。
