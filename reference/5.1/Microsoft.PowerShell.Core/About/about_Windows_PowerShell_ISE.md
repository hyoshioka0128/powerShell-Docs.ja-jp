---
description: Windows PowerShell Integrated Scripting Environment (ISE) の機能とシステム要件について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_windows_powershell_ise?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Windows_PowerShell_ISE
ms.openlocfilehash: ff543024d7c62c70217eeaf3ded192a5a24c4757
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388843"
---
# <a name="about-windows-powershell-ise"></a>Windows PowerShell ISE について

## <a name="short-description"></a>概要

Windows PowerShell Integrated Scripting Environment (ISE) の機能とシステム要件について説明します。

## <a name="long-description"></a>詳細説明

Windows PowerShell ISE は、Windows PowerShell 用のグラフィカルホストアプリケーションです。
Windows PowerShell ISE では、1つの Windows ベースのグラフィカルユーザーインターフェイスでコマンドを実行し、スクリプトの記述、テスト、およびデバッグを行うことができます。 その機能には、Intellisense、複数行の編集、タブ補完、自動保存、構文の色分け、選択的な実行、状況依存のヘルプ、コマンドの表示 (ウィンドウでのコマンドの作成)、2バイト文字セットと右から左へ記述する言語のサポートが含まれます。

Windows PowerShell ISE は初心者向けの優れたツールです。 [コマンドウィンドウと新しいリモート PowerShell の表示] タブでは、最初の試行を成功させるためのタスクを実行できます。 スニペットとエラーインジケーターは、作業中に Windows PowerShell 言語を学習するのに役立ちます。

上級ユーザーは、洗練されたデバッグ機能、アドオン、および Windows PowerShell ISE オブジェクトモデルを利用できます。

Windows PowerShell 4.0 での Windows PowerShell ISE の新機能

Windows PowerShell ISE には、Windows PowerShell 4.0 の2つの新機能が導入されています。

- Windows PowerShell ISE で、Windows PowerShell ワークフローデバッグとリモートスクリプトデバッグの両方がサポートされるようになりました。 詳細については、「about_Debuggers」を参照してください。

- Windows PowerShell Desired State Configuration のプロバイダーおよび構成に IntelliSense サポートが追加されました。

## <a name="starting-windows-powershell-ise"></a>Windows PowerShell ISE の開始

サポートされているすべてのバージョンの Windows で、Windows PowerShell ISE がインストールされ、有効になっており、使用準備ができています。

- Windows 8.1、Windows 8、Windows Server 2012 R2、および Windows Server 2012 では、スタート画面で「PowerShell_ISE」と入力し、[PowerShell_ISE] または [Windows PowerShell ISE] をクリックします。

- Windows Server 2012 R2 および Windows Server 2012 のサーバーマネージャーで、[ツール] メニューの [Windows PowerShell ISE] をクリックします。

- 以前のバージョンの Windows では、[スタート]、[すべてのプログラム]、[アクセサリ]、[Windows PowerShell] の順にクリックし、[Windows PowerShell ISE] をクリックします。

- Windows PowerShell コンソール、Cmd.exe、または Windows の [実行] または [検索] ボックスに「PowerShell_ise.exe」と入力します。 NoProfile スイッチなどのコマンドラインパラメーターを使用することもできます。 詳細については、 [PowerShell_ISE.exe コンソールのヘルプ](about_powershell_ise_exe.md)を参照してください。

## <a name="running-interactive-commands"></a>対話型コマンドの実行

Windows PowerShell ISE では、任意の Windows PowerShell 式またはコマンドを実行できます。 Windows PowerShell コンソールで使用するのと同じように、コマンドレット、プロバイダー、スナップイン、およびモジュールを使用できます。

対話型コマンドは、コンソールウィンドウで入力または貼り付けできます。 コマンドを実行するには、ボタン、メニュー項目、およびキーボードショートカットを使用します。

複数行編集機能を使用すると、複数行のコードを一度に入力するか、コンソールウィンドウに貼り付けることができます。 上方向キーを押して前のコマンドを再実行すると、コマンド内のすべての行が再呼び出しされます。 コマンドを入力するときに、SHIFT + ENTER キーを押すと、新しい空白行が現在の行に表示されます。

## <a name="viewing-output"></a>出力の表示

コマンドおよびスクリプトの結果がコンソールウィンドウに表示されます。 コンソールウィンドウで、キーボードショートカットまたはツールバーの [コピー] ボタンを使用して、結果を移動またはコピーできます。また、結果をスクリプトウィンドウまたはコンソールウィンドウまたはその他のプログラムに貼り付けることができます。 コンソールウィンドウをクリアするには、[出力ウィンドウのクリア] ボタンをクリックするか、次のいずれかのコマンドを入力します。

```
Clear-Host
cls
```

## <a name="writing-scripts-and-functions"></a>スクリプトと関数の記述

スクリプトウィンドウでは、スクリプトのオープン、作成、編集、および実行を行うことができます。 スクリプトウィンドウでは、ボタンとキーボードショートカットを使用してスクリプトを編集できます。 スクリプトウィンドウとコンソールウィンドウの間でテキストをコピー、切り取り、貼り付けすることもできます。

選択的実行機能を使用すると、スクリプトの全部または一部を実行できます。 スクリプトの一部を実行するには、実行するテキストを選択し、[選択項目の実行] ボタンをクリックするか、F8 キーを押します。 既定では、F8 は現在の行を実行します。

高度な編集機能には、かっこ一致、展開/折りたたみ、行番号、エラーインジケーター、ブロック編集とインデント、リッチコピー、および大文字小文字の変換が含まれます。

## <a name="getting-help"></a>ヘルプ情報の入手

Windows PowerShell ISE には、その使用方法を説明するヘルプトピックが含まれています。 また、インストールされているすべてのヘルプファイルには、[スクリプト] ウィンドウと [コマンド] ペインからアクセスできます。

Windows PowerShell ISE は、状況依存のヘルプもサポートしています。 特定のコマンドレット、プロバイダー、またはキーワードに関するヘルプを表示するには、項目の名前にカーソルを置き、F1 キーを押します。 ヘルプトピックを検索するには、F1 キーを押して検索語句を入力します。

コンピューターのヘルプトピックを更新するには、[ヘルプ] メニューの [Windows PowerShell の更新] ヘルプ項目を使用します。 この項目は、現在の UI カルチャで現在のセッションにあるモジュールのヘルプを更新します。 これは、パラメーターを指定せずに Update-Help コマンドレットを実行することと同じです。 Windows PowerShell に付属するコマンドレットのヘルプを更新するには、[管理者として実行] オプションを使用して Windows PowerShell ISE を開始します。

Windows PowerShell コンソールで使用する場合と同様に、Windows PowerShell ISE で Get-help、Save Help、および Update-Help コマンドレットを使用することもできます。 ただし、Windows PowerShell ISE では、ヘルプ関数は、一度に1ページではなく、ヘルプトピック全体を表示します。

## <a name="debugging-scripts"></a>スクリプトのデバッグ

Windows PowerShell ISE デバッガーを使用して、Windows PowerShell スクリプトまたは関数をデバッグすることができます。 スクリプトをデバッグするときに、メニュー項目とショートカットキーを使用して、Windows PowerShell コンソールで実行するのと同じタスクの多くを実行できます。 たとえば、スクリプトに行のブレークポイントを設定するには、コード行を右クリックし、[ブレークポイントの設定/解除] をクリックします。

デバッグ中にスクリプトをステップ実行すると、デバッグの蛍光ペンは、コマンドのどの部分が実行されているかを正確に示し、呼び出された関数とスクリプトを含むファイルを自動的に開きます。

既定では、[ブレークポイントの設定/解除] メニュー項目は、スクリプト内の行全体にブレークポイントを設定しますが、変数またはコマンド名にブレークポイントを設定できます。
また、行と列の番号によってコマンドにブレークポイントを設定して、長いパイプラインコマンドを簡単にデバッグできるようにすることもできます。

多くの場合、Windows PowerShell ISE でスクリプトファイルを開くだけで、スクリプト内の構文エラーをデバッグできます。 エラーインジケーターでは、構文エラーを識別し、アウトライン機能を使用して、スクリプトの一部を折りたたみ、問題点に焦点を当てることができます。

コンソールで使用するのと同じように、コマンドウィンドウで Windows PowerShell デバッガーコマンドレットを使用することもできます。

## <a name="running-remote-commands"></a>リモート コマンドの実行

新しいリモート PowerShell タブ機能を使用すると、ユーザーが管理する永続的な Windows PowerShell セッション ("PSSession") をローカルコンピューターまたはリモートコンピューターに簡単に確立できます。 コマンドを実行すると、ポップアップウィンドウが開き、リモートコンピューターでコマンドを実行するアクセス許可を持つコンピューター名とユーザーアカウントを入力するように求められます。

## <a name="customizing-the-view"></a>ビューのカスタマイズ

Windows PowerShell ISE の機能を使用して、コンソールウィンドウとスクリプトウィンドウの移動やサイズ変更を行うことができます。 いずれかのペインの表示と非表示を切り替えることができ、すべてのウィンドウでテキストのサイズを変更できます。

また、[オプション] ウィンドウを使用して、Windows PowerShell ISE の外観と操作をカスタマイズすることもできます。 さらに、Windows PowerShell ISE には、メニューやメニュー項目の追加などの Windows PowerShell ISE をカスタマイズするために使用できる、$psISE カスタムホスト変数が用意されています。

## <a name="windows-powershell-ise-profile"></a>プロファイルの Windows PowerShell ISE

Windows PowerShell ISE には、独自の Windows PowerShell プロファイル、Microsoft.PowerShellISE_profile.ps1 があります。 このプロファイルには、Windows PowerShell ISE で使用する関数、エイリアス、変数、およびコマンドを格納できます。

Windows PowerShell AllHosts プロファイル (CurrentUser \\ allhosts および AllUsers \\ allhosts) の項目は、windows powershell ホストプログラムと同様に Windows PowerShell ISE でも使用できます。 ただし、Windows PowerShell コンソールプロファイルの項目は Windows PowerShell ISE では使用できません。

プロファイルの移動と再構成の手順については、Windows PowerShell ISE ヘルプおよび about_Profiles を参照してください。

## <a name="notes"></a>注

Windows PowerShell ISE は、Windows のクライアントバージョンとサーバーバージョンで既定で有効になっているオプションの Windows 機能です。 クライアントバージョンの Windows で Windows PowerShell ISE を有効または無効にするには、コントロールパネルの [Windows の機能の有効化または無効化] を使用します。 Windows のサーバーバージョンで Windows PowerShell ISE を有効または無効にするにはサーバーマネージャーの役割と機能の追加ウィザードを使用します。

Windows PowerShell ISE にはユーザーインターフェイスが必要なため、Windows Server の Server Core インストールでは機能しません。 ただし、Windows PowerShell ISE 機能を追加すると、インストールは自動的に GUI を使用してサーバーに変換されます。

Windows PowerShell ISE は、Windows Presentation Foundation (WPF) に基づいて構築されています。
Windows PowerShell ISE のグラフィカル要素がシステム上で正しくレンダリングされない場合は、システム上で "WPF ハードウェアアクセラレータを無効にする" グラフィックスレンダリング設定を追加または調整することによって、問題を解決することができます。 詳細については、「[グラフィックス レンダリングのレジストリ設定](/dotnet/framework/wpf/graphics-multimedia/graphics-rendering-registry-settings)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Debuggers](about_Debuggers.md)

[about_Profiles](about_Profiles.md)

[about_Updatable_Help](about_Updatable_Help.md)

[Get-Help](xref:Microsoft.PowerShell.Core.Get-Help)

[Get-IseSnippet](xref:ISE.Get-IseSnippet)

[Import-IseSnippet](xref:ISE.Import-IseSnippet)

[New-IseSnippet](xref:ISE.New-IseSnippet)

[Save-Help](xref:Microsoft.PowerShell.Core.Save-Help)

[Show-Command](xref:Microsoft.PowerShell.Utility.Show-Command)

[Update-Help](xref:Microsoft.PowerShell.Core.Update-Help)
