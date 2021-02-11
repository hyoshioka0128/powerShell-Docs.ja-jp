---
ms.date: 12/05/2019
keywords: powershell,コマンドレット
title: Windows PowerShell の開始
description: この記事では、さまざまなバージョンの PowerShell を開始する方法について説明します。
ms.openlocfilehash: 47da7d85c9f7e6966a7f7e809c77979cd24cf129
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92664013"
---
# <a name="starting-windows-powershell"></a>Windows PowerShell の開始

Windows PowerShell は、複数のホストに埋め込まれているスクリプト エンジン `.DLL` です。 開始するホストとして最も一般的なものは、対話型コマンド ラインの `powershell.exe` および対話型スクリプト環境の `powershell_ise.exe` です。

Windows Server&reg; 2012 R2、Windows&reg; 8.1、Windows Server 2012、Windows 8 で Windows PowerShell&reg; を開始するには、「[Windows の一般的な管理タスクとナビゲーション](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831491(v=ws.11))」をご覧ください。

## <a name="powershell-core-has-renamed-binary"></a>PowerShell Core でバイナリ名が変更されました

PowerShell とも呼ばれている PowerShell Core はオープン ソースのバージョン 6 以降であり、.NET Core が使用されます。 サポートされているバージョンは、Windows、macOS、Linux で使用できます。

PowerShell 6 以降、PowerShell バイナリの名前が Windows で `pwsh.exe` に、macOS と Linux で `pwsh` に変更されました。 `pwsh-preview` を使用し、PowerShell プレビュー バージョンを開始できます。 詳細については、「[PowerShell Core 6.0 の新機能](/powershell/scripting/whats-new/what-s-new-in-powershell-core-60#renamed-powershellexe-to-pwshexe)」と「[pwsh について](/powershell/module/microsoft.powershell.core/about/about_pwsh)」を参照してください。

PowerShell 7 のコマンド リファレンスとインストール ドキュメントは次のリンクから見つかります。

| ドキュメント | Link |
| ----- | ----- |
| コマンドレット リファレンス | [PowerShell モジュール ブラウザー](/powershell/module/) |
| Windows インストール | [Windows に PowerShell Core をインストールする](/powershell/scripting/install/installing-powershell-core-on-windows) |
| macOS インストール | [macOS に PowerShell Core をインストールする](/powershell/scripting/install/installing-powershell-core-on-macos) |
| Linux インストール | [Linux に PowerShell Core をインストールする](/powershell/scripting/install/installing-powershell-core-on-linux) |

他の PowerShell バージョンのコンテンツを表示するには、「[PowerShell ドキュメントの使用方法](../how-to-use-docs.md)」を参照してください。

## <a name="how-to-start-windows-powershell-on-earlier-versions-of-windows"></a>以前のバージョンの Windows で Windows PowerShell を開始する方法

このセクションでは、Windows&reg; 7、Windows Server&reg; 2008 R2、Windows Server&reg; 2008 で、Windows PowerShell と Windows PowerShell Integrated Scripting Environment (ISE) を開始する方法について説明します。 Windows Server&reg; 2008 R2 と Windows Server&reg; 2008 で、Windows PowerShell 2.0 の Windows PowerShell ISE のオプション機能を有効にする方法についても説明します。

次のいずれかの方法で、必要に応じて、Windows PowerShell 3.0 または Windows PowerShell 4.0 のインストールされたバージョンを開始します。

#### <a name="from-the-start-menu"></a>[スタート] メニューからの場合

- **[スタート]** をクリックし、「 **PowerShell** 」と入力して **[Windows PowerShell]** をクリックします。
- **[スタート]** メニューから、 **[スタート]** 、 **[すべてのプログラム]** 、 **[アクセサリ]** 、 **[Windows PowerShell]** フォルダー、 **[Windows PowerShell]** の順にクリックします。

#### <a name="at-the-command-prompt"></a>コマンド プロンプトでの場合

**cmd.exe** 、Windows PowerShell、または Windows PowerShell ISE で Windows PowerShell を開始するには、次のように入力します。

```
PowerShell
```

また、`powershell.exe` プログラムのパラメーターを使って、セッションをカスタマイズできます。 詳しくは、「[PowerShell.exe コマンドラインのヘルプ](/powershell/module/Microsoft.PowerShell.Core/About/about_PowerShell_exe)」をご覧ください。

#### <a name="with-administrative-privileges-run-as-administrator"></a>管理特権を使う場合 (管理者として実行)

**[スタート]** をクリックし、「 **PowerShell** 」と入力し、 **[Windows PowerShell]** を右クリックして、 **[管理者として実行]** をクリックします。

## <a name="how-to-start-windows-powershell-ise-on-earlier-releases-of-windows"></a>以前のリリースの Windows で Windows PowerShell ISE を開始する方法

次のいずれかの方法で Windows PowerShell ISE を開始します。

#### <a name="from-the-start-menu"></a>[スタート] メニューからの場合

- **[スタート]** をクリックし、「 **ISE** 」と入力して **[Windows PowerShell ISE]** をクリックします。
- **[スタート]** メニューから、 **[スタート]** 、 **[すべてのプログラム]** 、 **[アクセサリ]** 、 **[Windows PowerShell]** フォルダー、 **[Windows PowerShell ISE]** の順にクリックします。

#### <a name="at-the-command-prompt"></a>コマンド プロンプトでの場合

`cmd.exe`、Windows PowerShell、または Windows PowerShell ISE で Windows PowerShell を開始するには、次のように入力します。

```
PowerShell_ISE
```

または

```
ISE
```

#### <a name="with-administrative-privileges-run-as-administrator"></a>管理特権を使う場合 (管理者として実行)

**[スタート]** をクリックし、「 **ISE** 」と入力し、 **[Windows PowerShell ISE]** を右クリックして、 **[管理者として実行]** をクリックします。

## <a name="how-to-enable-windows-powershell-ise-on-earlier-releases-of-windows"></a>以前のリリースの Windows で Windows PowerShell ISE を有効にする方法

Windows PowerShell 4.0 および Windows PowerShell 3.0 では、Windows PowerShell ISE は既定ではすべてのバージョンの Windows で有効になっています。 まだ有効になっていない場合、Windows Management Framework 4.0 または Windows Management Framework 3.0 により有効になります。

Windows PowerShell 2.0 では、Windows PowerShell ISE は既定では Windows 7 で有効になっています。 ただし、Windows Server 2008 R2 および Windows Server 2008 では省略可能な機能です。

Windows Server 2008 R2 または Windows Server 2008 で Windows PowerShell 2.0 の Windows PowerShell ISE を有効にするには、次の手順に従います。

#### <a name="to-enable-windows-powershell-integrated-scripting-environment-ise"></a>Windows PowerShell Integrated Scripting Environment (ISE) を有効にするには

1. Server Manager を起動します。
2. **[機能]** 、 **[機能の追加]** の順にクリックします。
3. [機能の選択]で、[Windows PowerShell Integrated Scripting Environment (ISE)] をクリックします。

## <a name="starting-the-32-bit-version-of-windows-powershell"></a>32 ビット版の Windows PowerShell を起動する

64 ビット コンピューターに Windows PowerShell をインストールする場合、64 ビット版に加えて、 **Windows PowerShell (x86)** 、つまり Windows PowerShell の 32 ビット版もインストールされます。 Windows PowerShell を実行すると、既定では 64 ビット版が実行されます。

ただし、32 ビット版を必要とするモジュールを使用するときや、32 ビットのコンピューターにリモートで接続しているときなど、 **Windows PowerShell (x86)** の実行を必要とすることもときどきあります。

Windows PowerShell の 32 ビット版を起動するには、次の手順のいずれかを使用します。

#### <a name="in-windows-serverreg-2012-r2"></a>Windows Server&reg; 2012 R2

- **[スタート]** 画面で、「 **Windows PowerShell (x86)** 」と入力します。 **[Windows PowerShell x86]** タイルをクリックします。
- **サーバー マネージャー** の **[ツール]** メニューから、 **[Windows PowerShell (x86)]** を選択します。
- デスクトップで、右上隅にカーソルを移動し、 **[検索]** をクリックする。「 **PowerShell x86** 」と入力し、 **[Windows PowerShell (x86)]** をクリックします。
- コマンド ラインで、次のように入力します。`%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### <a name="in-windows-serverreg-2012"></a>Windows Server&reg; 2012

- **[スタート]** 画面で、「 **PowerShell** 」と入力して **[Windows PowerShell (x86)]** をクリックします。
- **サーバー マネージャー** の **[ツール]** メニューから、 **[Windows PowerShell (x86)]** を選択します。
- デスクトップで、右上隅にカーソルを移動し、 **[検索]** をクリックする。「 **PowerShell** 」と入力し、 **[Windows PowerShell (x86)]** をクリックします。
- コマンド ラインで、次のように入力します。`%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### <a name="in-windowsreg-81"></a>Windows&reg; 8.1

- **[スタート]** 画面で、「 **Windows PowerShell (x86)** 」と入力します。 **[Windows PowerShell x86]** タイルをクリックします。
- Windows 8.1 の [リモート サーバー管理ツール](https://go.microsoft.com/fwlink/?LinkID=304145)を実行している場合、 **[サーバー管理ツール]** メニューから [Windows PowerShell x86] を開くこともできます。 **[Windows PowerShell (x86)]** を選択します。
- デスクトップで、右上隅にカーソルを移動し、 **[検索]** をクリックする。「 **PowerShell x86** 」と入力し、 **[Windows PowerShell (x86)]** をクリックします。
- コマンド ラインで、次のように入力します。`%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`

#### <a name="in-windowsreg-8"></a>Windows&reg; 8

- **[スタート]** 画面で、右上隅にカーソルを移動し、 **[設定]** をクリックし、 **[タイル]** をクリックします。そして、 **[管理ツールを表示する]** スライダーを **[はい]** に移動させます。 次に「 **PowerShell** 」と入力し、 **[Windows PowerShell(x86)]** をクリックします。
- Windows 8 の [リモート サーバー管理ツール](https://www.microsoft.com/download/details.aspx?id=28972)を実行している場合、 **[サーバー管理ツール]** メニューから [Windows PowerShell x86] を開くこともできます。 **[Windows PowerShell (x86)]** を選択します。
- **[スタート]** 画面またはデスクトップで、「 **PowerShell (x86)** 」と入力し、 **[Windows PowerShell (x86)]** をクリックします。
- コマンド ラインで、次のように入力します。`%SystemRoot%\SysWOW64\WindowsPowerShell\v1.0\powershell.exe`
