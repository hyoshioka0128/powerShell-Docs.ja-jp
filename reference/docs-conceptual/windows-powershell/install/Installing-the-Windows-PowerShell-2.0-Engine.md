---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: Windows PowerShell 2.0 エンジンのインストール
description: Windows PowerShell 2.0 エンジンは、Windows のオプション機能です。 この記事では、その機能をインストールする方法と、必要な要件について説明します。
ms.openlocfilehash: c82725c34f5c5864eba0c88eb33ecac9e43f86d3
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663967"
---
# <a name="installing-the-windows-powershell-20-engine"></a>Windows PowerShell 2.0 エンジンのインストール

このトピックでは、Windows PowerShell 2.0 エンジンのインストール方法について説明します。

Windows PowerShell 3.0 は、Windows PowerShell 2.0 との下位互換性を保つように設計されています。 Windows PowerShell 2.0 用に記述されたコマンドレット、プロバイダー、スナップイン、モジュール、スクリプトは、未変更のまま Windows PowerShell 3.0 と Windows PowerShell 4.0 で実行できます。 ただし、Microsoft .NET Framework 4 でランタイムのアクティブ化ポリシーが変更されたため、Windows PowerShell 2.0 用に記述され、共通言語ランタイム (CLR) 2.0 でコンパイルされた Windows PowerShell ホスト プログラムは、変更を加えなければ新しいリリースの Windows PowerShell (CLR 4.0 でコンパイルされたもの) で実行できません。

これらの変更の影響を受けるコマンドやホスト プログラムとの下位互換性を維持するため、Windows PowerShell 2.0、Windows PowerShell 3.0、および Windows PowerShell 4.0 のエンジンは、side-by-side で実行できるように設計されています。 また、Windows PowerShell 2.0 エンジンは Windows Server 2012 R2、Windows 8.1、Windows 8、Windows Server 2012、Windows Management Framework 3.0 にも組み込まれています。 Windows PowerShell 2.0 エンジンは、既存のスクリプトまたはホスト プログラムが Windows PowerShell 3.0、Windows PowerShell 4.0、Microsoft .NET Framework 4 との互換性がないために実行できない場合に限り使用することを意図しています。 そのようなケースはまれと考えられます。

Windows PowerShell 2.0 エンジンは、Windows Server 2012 R2、Windows 8.1、Windows&reg; 8、Windows Server&reg; 2012 のオプション機能です。 以前のバージョンの Windows に Windows Management Framework 3.0 をインストールすると、Windows PowerShell のインストール ディレクトリ内で Windows PowerShell 2.0 インストールが Windows PowerShell 3.0 のインストールによって完全に置き換えられます。 ただし、Windows PowerShell 2.0 エンジンは保持されます。

Windows PowerShell 2.0 エンジンの開始に関する情報については、「[Windows PowerShell 2.0 エンジンの開始](../Starting-the-Windows-PowerShell-2.0-Engine.md)」を参照してください。

## <a name="on-windows-81-and-windows-8"></a>Windows 8 および Windows 8.1

Windows 8.1 と Windows 8 では、Windows PowerShell 2.0 エンジンの機能が既定でオンになっています。
ただし、この機能を使うには、この機能に必要な Microsoft .NET Framework 3.5 オプションをオンにする必要があります。 このセクションでは、Windows PowerShell 2.0 エンジンの機能をオンまたはオフにする方法についても説明します。

#### <a name="to-turn-on-net-framework-35"></a>.NET Framework 3.5 をオンにするには

1. **[スタート]** 画面で、「 **Windows の機能** 」と入力します。
2. **アプリ** バーで、 **[設定]** をクリックしてから、 **[Windows の機能の有効化または無効化]** をクリックします。
3. **[Windows の機能]** ボックスで、 **[.NET Framework 3.5 (.NET 2.0 および 3.0 を含む)]** をオンにします。

   **[.NET Framework 3.5 (.NET 2.0 および 3.0 を含む)]** を選択すると、ボックスが塗りつぶされて、機能の一部のみが選択された状態になります。 Windows PowerShell 2.0 エンジンの場合はこの状態で十分です。

#### <a name="to-turn-the-windows-powershell-20-engine-on-and-off"></a>Windows PowerShell 2.0 エンジンをオンまたはオフにするには

1. **[スタート]** 画面で、「 **Windows の機能** 」と入力します。

2. **アプリ** バーで、 **[設定]** をクリックしてから、 **[Windows の機能の有効化または無効化]** をクリックします。

3. **[Windows の機能]** ボックスで、[ **Windows PowerShell 2.0** ] ノードを展開し、[ **Windows PowerShell 2.0** エンジン] ボックスをクリックして、オンまたはオフにします。

## <a name="on-windows-server-2012-r2-and-windows-server-2012"></a>Windows Server 2012 R2 または Windows Server 2012

次の手順を実行して、Windows PowerShell 2.0 エンジンと Microsoft .NET Framework 3.5 の機能を追加します。 Windows PowerShell 2.0 エンジンには最低でも Microsoft .NET Framework 2.0.50727 が必要です。 この要件は Microsoft .NET Framework 3.5 で満たされます。

#### <a name="to-add-the-net-framework-35-feature"></a>.NET Framework 3.5 の機能を追加するには

1. **サーバー マネージャー** で、 **[管理]** メニューから **[役割と機能の追加]** を選びます。

    または、 **サーバー マネージャー** で、 **[すべてのサーバー]** をクリックし、サーバー名を右クリックしてから、 **[役割と機能の追加]** を選択します。

2. **[インストールの種類]** ページで **[役割ベースまたは機能ベースのインストール]** を選びます。

3. **[機能]** ページで、 **[.NET 3.5 Framework 機能]** ノードを展開し、 **[.NET Framework 3.5 (.NET 2.0 および 3.0 を含む)]** を選びます。

   このノードの下にあるその他のオプションは、Windows PowerShell 2.0 エンジンには必要ありません。

#### <a name="to-add-the-windows-powershell-20-engine-feature"></a>Windows PowerShell 2.0 エンジンの機能を追加するには

- **サーバー マネージャー** で、 **[管理]** メニューから **[役割と機能の追加]** を選びます。

  または、 **サーバー マネージャー** で、 **[すべてのサーバー]** をクリックし、サーバー名を右クリックしてから、 **[役割と機能の追加]** を選びます。

- **[インストールの種類]** ページで **[役割ベースまたは機能ベースのインストール]** を選びます。

- **[機能]** ページで、 **[Windows PowerShell (インストール済み)]** ノードを展開し、 **[Windows PowerShell 2.0 エンジン]** を選択します。

Windows PowerShell 2.0 エンジンの開始に関する情報については、「[Windows PowerShell 2.0 エンジンの開始](../Starting-the-Windows-PowerShell-2.0-Engine.md)」を参照してください。

## <a name="on-earlier-systems"></a>以前のシステムの場合

Windows 7、Windows Server 2008 R2、および Windows Server 2012 に Windows PowerShell 4.0 をインストールする [Windows Management Framework 4.0](https://go.microsoft.com/fwlink/?LinkID=293881) パッケージには、Windows PowerShell 2.0 エンジンが組み込まれています。 Windows PowerShell 2.0 をエンジンは有効になっており、使用準備が整っていますので、追加のインストールや、セットアップ、構成は必要ありません。

Windows 7、Windows Server 2008 R2、および Windows Server 2008 に Windows PowerShell 3.0 をインストールする Windows Management Framework 3.0 パッケージには、Windows PowerShell 2.0 エンジンが組み込まれています。 Windows PowerShell 2.0 をエンジンは有効になっており、使用準備が整っていますので、追加のインストールや、セットアップ、構成は必要ありません。

## <a name="see-also"></a>参照

- [Windows PowerShell のシステム要件](Windows-PowerShell-System-Requirements.md)
- [Windows PowerShell のインストール](Installing-Windows-PowerShell.md)
- [Windows PowerShell の開始](/previous-versions/ms714415(v=vs.85))
- [Windows PowerShell 2.0 エンジンの開始](../Starting-the-Windows-PowerShell-2.0-Engine.md)
