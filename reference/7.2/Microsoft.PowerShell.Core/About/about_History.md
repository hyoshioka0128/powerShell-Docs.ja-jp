---
description: コマンド履歴のコマンドを取得して実行する方法について説明します。
Locale: en-US
ms.date: 05/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_history?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_History
ms.openlocfilehash: 44e03bd37b0b2c5928fb3aa850f3f5b554ccf123
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605458"
---
# <a name="about-history"></a>履歴について

## <a name="short-description"></a>簡単な説明
コマンド履歴のコマンドを取得して実行する方法について説明します。

## <a name="long-description"></a>長い説明

コマンドプロンプトでコマンドを入力すると、PowerShell によってコマンド履歴にコマンドが保存されます。 履歴内のコマンドは、作業のレコードとして使用できます。 また、コマンドの履歴からコマンドを再呼び出しし、実行することもできます。

PowerShell には、組み込み履歴と **Psreadline** モジュールによって管理される履歴という2つの異なる履歴プロバイダーがあります。 履歴は個別に管理されますが、両方の履歴は、 **Psreadline** が読み込まれるセッションで使用できます。

## <a name="using-the-psreadline-history"></a>PSReadLine 履歴の使用

PSReadLine 履歴は、すべての PowerShell セッションで使用されているコマンドを追跡します。
履歴は、ホストごとに中央のファイルに書き込まれます。 その履歴ファイルはすべてのセッションで使用でき、過去のすべての履歴が含まれています。 セッションが終了しても、履歴は削除されません。 また、この履歴をコマンドレットで管理することはできません `*-History` 。 詳細については、「 [about_PSReadLine](../../PSReadLine/About/about_PSReadLine.md)」を参照してください。

## <a name="using-the-built-in-session-history"></a>組み込みのセッション履歴を使用する

組み込みの履歴は、現在のセッションで使用されているコマンドのみを追跡します。 履歴は、他のセッションでは使用できず、セッションが終了すると削除されます。

### <a name="history-cmdlets"></a>History コマンドレット

PowerShell には、コマンドの履歴を管理するコマンドレットのセットが用意されています。

| コマンドレット           | エイリアス  | 説明                                |
| ---------------- | ------ | ------------------------------------------ |
| `Get-History`    | `h`    | コマンドの履歴を取得します。                  |
| `Invoke-History` | `r`    | コマンドの履歴でコマンドを実行します。     |
| `Add-History`    |        | コマンドの履歴にコマンドを追加します。     |
| `Clear-History`  | `clhy` | コマンド履歴からコマンドを削除します。 |

### <a name="keyboard-shortcuts-for-managing-history"></a>履歴を管理するためのキーボードショートカット

PowerShell コンソールでは、次のショートカットを使用してコマンドの履歴を管理できます。

- [ <kbd>Uparrow</kbd> ]: 前のコマンドを表示します。
- <kbd>Downarrow</kbd> -次のコマンドを表示します。
- <kbd>F7</kbd> -コマンドの履歴を表示します。
- <kbd>ESC</kbd> -履歴を非表示にします。
- <kbd>F8</kbd> -コマンドを検索します。 1つ以上の文字を入力してから、 <kbd>F8</kbd>キーを押します。 次のインスタンスにもう一度 <kbd>F8</kbd> キーを押します。
- <kbd>F9</kbd> -履歴 ID を使ってコマンドを検索します。 履歴 ID を入力し、 <kbd>F9</kbd>キーを押します。 <kbd>F7</kbd>キーを押して ID を検索します。
- <kbd>#</kbd>`<string>`</kbd><kbd>[] タブ</kbd> -の履歴を検索 `*<string>*` し、最新の一致を返します。 <kbd>Tab</kbd>キーを繰り返し押すと、履歴内の一致する項目が順番に表示されます。

> [!NOTE]
> これらのキーバインドは、コンソールホストアプリケーションによって実装されます。 Visual Studio Code や Windows ターミナルなどの他のアプリケーションでは、異なるキーバインドを持つことができます。 バインドは、PSReadLine モジュールでオーバーライドできます。 PSReadLine は、PowerShell セッションを開始すると自動的に読み込まれます。
> PSReadLine が読み込まれている場合、 <kbd>F7</kbd> と <kbd>F9</kbd> はどの関数にもバインドされていません。 PSReadLine では、同等の機能は提供されません。 詳細については、「 [about_PSReadLine](../../PSReadLine/About/about_PSReadLine.md)」を参照してください。

### <a name="maximumhistorycount"></a>Maximumhistorycount ユーザー

ユーザー `$MaximumHistoryCount` 設定変数は、PowerShell がコマンドの履歴に保存するコマンドの最大数を決定します。 既定値は
4096.

たとえば、次のコマンドは、 `$MaximumHistoryCount` を100コマンドに下げます。

```powershell
$MaximumHistoryCount = 100
```

設定を適用するには、PowerShell を再起動します。

すべての PowerShell セッションの新しい変数値を保存するには、割り当てステートメントを PowerShell プロファイルに追加します。 プロファイルの詳細については、「[about_Profiles](about_Profiles.md)」を参照してください。

ユーザー設定変数の詳細については `$MaximumHistoryCount` 、「 [about_Preference_Variables](about_Preference_Variables.md)」を参照してください。

### <a name="order-of-commands-in-the-history"></a>履歴内のコマンドの順序

コマンドの実行が完了すると、コマンドが入力されたときではなく、コマンドが履歴に追加されます。 コマンドが完了するまでに時間がかかる場合、またはコマンドが入れ子になったプロンプトで実行されている場合は、コマンドが履歴に表示されない可能性があります。 入れ子になったプロンプトで実行されているコマンドは、プロンプトレベルを終了した場合にのみ完了します。

## <a name="see-also"></a>参照

- [about_Line_Editing](about_Line_Editing.md)
- [about_Preference_Variables](about_Preference_Variables.md)
- [about_Profiles](about_Profiles.md)
- [about_Variables](about_Variables.md)
- [about_PSReadLine](../../PSReadLine/About/about_PSReadLine.md)

