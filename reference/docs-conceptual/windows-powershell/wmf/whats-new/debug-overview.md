---
ms.date: 06/12/2017
keywords: WMF, PowerShell, セットアップ
title: PowerShell スクリプト デバッグの強化
description: WMF 5.0 では、Windows PoowerShell に新しいデバッグ機能が追加されています。
ms.openlocfilehash: 5703343e1b85024931638e8b04a09f7208ea123c
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92646730"
---
# <a name="improvements-in-powershell-script-debugging"></a>PowerShell スクリプト デバッグの強化

PowerShell 5.0 には、デバッグ エクスペリエンスを強化するいくつかの機能強化が含まれています。

## <a name="break-all"></a>すべて中断

PowerShell コンソールと PowerShell ISE では、実行中のスクリプトにデバッガーで割り込むことができるようになりました。 これは、ローカルとリモートの両方のセッションに適用されます。

コンソールでは、<kbd>Ctrl</kbd>+<kbd>Break</kbd> キーを押します。

ISE では、 <kbd>Ctrl</kbd>+<kbd>B</kbd> キーを押すか、 **[デバッグ] -> [すべて中断]** メニュー コマンドを使います。

## <a name="remote-debugging-and-remote-file-editing-in-powershell-ise"></a>PowerShell ISE でのリモートによるデバッグとファイル編集

PowerShell ISE では、PSEdit コマンドを実行することにより、リモート セッションでファイルを開いて編集できるようになりました。
たとえば、次のように、リモート セッションでコマンド ラインからファイルを開いて編集できます。

```powershell
[RemoteComputer1]: PS C:\> PSEdit C:\DebugDemoScripts\Test-GetMutex.ps1
```

また、ブレークポイントにヒットしたときに、PowerShell ISE で自動的に開くリモート ファイルを編集して変更を保存できるようになりました。 今後は、リモート コンピューターで実行しているスクリプト ファイルをデバッグし、エラー修正のためにファイルを編集し、変更したスクリプトを再実行することができます。

## <a name="advanced-script-debugging"></a>高度なスクリプトのデバッグ

新しい高度なデバッグ機能では、PowerShell が読み込まれたローカル コンピューターのプロセスにアタッチし、そのプロセス内の任意の実行空間をデバッグできます。

### <a name="runspace-debugging"></a>実行空間のデバッグ

プロセス内の現在の実行空間を一覧表示し、その実行空間に PowerShell コンソールや PowerShell ISE デバッガーをアタッチしてスクリプト デバッグを実行するための、新しいコマンドレットが追加されました。

- Get-Runspace
- Debug-Runspace
- Enable-RunspaceDebug
- Disable-RunspaceDebug
- Get-RunspaceDebug

### <a name="attach-to-process-hosting-powershell"></a>PowerShell をホストしているプロセスにアタッチする

PowerShell が読み込まれているすべてのコンピューター プロセスにアタッチできるようになりました。 それを行うには、ホスト プロセスで対話型セッションに入ります。 詳細については、次をご覧ください。

- [Enter-PSHostProcess](/powershell/module/Microsoft.PowerShell.Core/Enter-PSHostProcess)
- [Exit-PSHostProcess](/powershell/module/Microsoft.PowerShell.Core/Exit-PSHostProcess)
