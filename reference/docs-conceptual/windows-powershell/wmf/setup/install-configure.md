---
ms.date: 06/10/2020
title: WMF 5.1 のインストールと構成
description: この記事では、WMF 5.1 のインストール方法と、その前提要件について説明します。
ms.openlocfilehash: 0e076bfab684b6c83d62d236eea3bbd7ab2ad411
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92660848"
---
# <a name="install-and-configure-wmf-51"></a>WMF 5.1 のインストールと構成

> [!IMPORTANT]
> WMF 5.0 は、WMF 5.1 に置き換えられています。 WMF 5.0 を所有するユーザーがサポートを受けるには、WMF 5.1 にアップグレードする必要があります。
> **WMF 5.1 には .NET Framework 4.5.2** (またはそれ以上) が必要です。 .NET 4.5.2 (またはそれ以上) がインストールされていない場合、インストールは成功しますが、主な機能は失敗します。

## <a name="download-and-install-the-wmf-51-package"></a>WMF 5.1 パッケージのダウンロードとインストール

インストールするオペレーティング システムとアーキテクチャに合わせて WMF 5.1 パッケージをダウンロードします。

| オペレーティング システム       | 前提条件           | パッケージ リンク                          |
|------------------------|-------------------------|----------------------------------------|
| Windows Server 2012 R2 |                         | [Win8.1AndW2K12R2-KB3191564-x64.msu][] |
| Windows Server 2012    |                         | [W2K12-KB3191565-x64.msu][]            |
| Windows Server 2008 R2 | [.NET Framework 4.5.2][]| [Win7AndW2K8R2-KB3191566-x64.ZIP][]    |
| Windows 8.1            |                         | **x64:** [Win8.1AndW2K12R2-KB3191564-x64.msu][]</br>**x86:** [Win8.1-KB3191564-x86.msu][] |
| Windows 7 SP1          | [.NET Framework 4.5.2][]| **x64:** [Win7AndW2K8R2-KB3191566-x64.ZIP][]</br>**x86:** [Win7-KB3191566-x86.ZIP][] |

[.NET Framework 4.5.2]: https://www.microsoft.com/download/details.aspx?id=42642
[W2K12-KB3191565-x64.msu]: https://go.microsoft.com/fwlink/?linkid=839513
[Win7-KB3191566-x86.ZIP]: https://go.microsoft.com/fwlink/?linkid=839522
[Win7AndW2K8R2-KB3191566-x64.ZIP]: https://go.microsoft.com/fwlink/?linkid=839523
[Win8.1-KB3191564-x86.msu]: https://go.microsoft.com/fwlink/?linkid=839521
[Win8.1AndW2K12R2-KB3191564-x64.msu]: https://go.microsoft.com/fwlink/?linkid=839516

- WMF 5.1 RTM をインストールする前に、WMF 5.1 プレビューをアンインストールする必要があります。
- WMF 5.1 は、WMF 5.0 または WMF 4.0 の上に直接インストールできます。
- Windows 7 および Windows Server 2008 R2 の上に WMF 5.1 をインストールする前に、WMF 4.0 をインストールする **必要はありません** 。

## <a name="install-wmf-51-for-windows-server-2008-r2-and-windows-7"></a>Windows Server 2008 R2 および Windows 7 で WMF 5.1 をインストールする

> [!NOTE]
> Windows Server 2008 R2 および Windows 7 でのインストール手順は変更されているため、他のパッケージの手順と異なります。 Windows Server 2012 R2、Windows Server 2012、および Windows 8.1 のインストール手順は次のとおりです。

### <a name="wmf-51-prerequisites-for-windows-server-2008-r2-sp1-and-windows-7-sp1"></a>Windows Server 2008 R2 SP1 および Windows 7 SP1 での WMF 5.1 の前提条件

Windows Server 2008 R2 SP1 または Windows 7 SP1 に WMF 5.1 をインストールするには、次を行う必要があります。

- 最新の Service Pack をインストールする必要があります。
- WMF 3.0 をインストール **しないでください** 。 WMF 3.0 経由で WMF 5.1 をインストールすると、 **PSModulePath** (`$env:PSModulePath`) が失われ、これによって他のアプリケーションで障害が発生する場合があります。 WMF 5.1 をインストールする前に WMF 3.0 をアンインストールするか、 **PSModulePath** を保存して、WMF 5.1 のインストールが完了した後に手動で復元する必要があります。
- WMF 5.1には少なくとも [.NET Framework 4.5.2 ](https://www.microsoft.com/download/details.aspx?id=42642)が必要です。 Microsoft .NET Framework 4.5.2 をインストールするには、ダウンロード場所で提供されている手順に従ってください。

### <a name="installing-wmf-51-on-windows-server-2008-r2-and-windows-7"></a>Windows Server 2008 R2 および Windows 7 で WMF 5.1 をインストールする

1. ZIP ファイルをダウンロードしたフォルダーに移動します。

1. ZIP ファイルを右クリックし、 **[すべて展開...]** を選択します。ZIP ファイルには、2 つのファイルが含まれています (MSU と `Install-WMF5.1.ps1` スクリプト ファイル)。 ZIP ファイルを展開したら、Windows 7 または Windows Server 2008 R2 を実行する任意のコンピューターにコンテンツをコピーすることができます。

1. ZIP ファイルを展開した後、PowerShell を管理者として開き、その ZIP ファイルの内容を含むフォルダーに移動します。

1. そのフォルダーの `Install-WMF5.1.ps1` スクリプトを実行して、指示に従います。 このスクリプトはローカル コンピューターの前提条件を確認し、前提条件を満たしている場合に WMF 5.1 をインストールします。 前提条件は次のとおりです。

   `Install-WMF5.1.ps1` では、Windows Server 2008 R2 および Windows 7 でのインストールの自動化を容易にするため、次のパラメーターが使用されます。

   - **AcceptEula** :このパラメーターが含まれる場合、使用許諾契約書は自動的に受け入れられ、表示はされません。
   - **AllowRestart** :このパラメーターは、AcceptEula が指定されている場合にのみ使用できます。 このパラメーターが含まれていて、WMF 5.1 のインストール後に再起動が必要な場合は、インストールが完了した後すぐに、メッセージが表示されることなく再起動が実行されます。

## <a name="winrm-dependency"></a>WinRM の依存関係

Windows PowerShell Desired State Configuration (DSC) は、WinRM に依存します。 WinRM は、Windows Server 2008 R2 および Windows 7 では既定で無効になっています。 WinRM を有効にするには、Windows PowerShell 管理者特権セッションで `Set-WSManQuickConfig` を実行します。

## <a name="install-wmf-51-for-windows-server-2012-r2-windows-server-2012-and-windows-81"></a>Windows Server 2012 R2、Windows Server 2012、および Windows 8.1 で WMF 5.1 をインストールする

### <a name="install-from-windows-file-explorer"></a>エクスプローラーからインストールする

1. MSU ファイルをダウンロードしたフォルダーに移動します。
1. MSU をダブルクリックして実行します。

### <a name="installing-from-the-command-prompt"></a>コマンド プロンプトからインストールする

1. コンピューターのアーキテクチャに適切なパッケージをダウンロードした後、管理者特権 (管理者として実行) を使ってコマンド プロンプト ウィンドウを開きます。 Windows Server 2012 R2、Windows Server 2012、Windows Server 2008 R2 SP1 の Server Core インストール オプションで、既定で管理者特権でコマンド プロンプトが開きます。
1. WMF 5.1 のインストール パッケージをダウンロードまたはコピーしたフォルダーにディレクトリを変更します。
1. 次のいずれかのコマンドを実行します。
   - Windows Server 2012 R2 または Windows 8.1 x64 を実行しているコンピューターの場合は、`Win8.1AndW2K12R2-KB3191564-x64.msu /quiet /norestart` を実行します。
   - Windows Server 2012 を実行しているコンピューターの場合は、`W2K12-KB3191565-x64.msu /quiet /norestart` を実行します。
   - Windows 8.1 x86 を実行しているコンピューターの場合は、`Win8.1-KB3191564-x86.msu /quiet /norestart` を実行します。

   > [!NOTE]
   > WMF 5.1 をインストールするには、再起動が必要です。 `/quiet` オプションを使用すると、システムは警告なしで再起動します。 再起動しないようにするには、`/norestart` オプションを使用します。 ただし、再起動が完了するまで、WMF 5.1 はインストールされません。
