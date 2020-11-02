---
ms.date: 06/12/2017
title: WMF 5.0 の既知の問題
description: WMF 5.0 の既知の問題
ms.openlocfilehash: 3f8dcf0f7aab27ff9d3c3a17377959988844a430
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663368"
---
# <a name="known-issues-in-wmf-50"></a>WMF 5.0 の既知の問題

## <a name="powershell-shortcuts-are-broken-when-used-for-the-first-time"></a>初回使用時に PowerShell のショートカットが壊れている

**解決策:** 次の操作のいずれかを実行します。

1. PowerShell のショートカットを右クリックします。 [Windows PowerShell] を選択し、管理者特権以外のモードで起動します。
2. PowerShell のショートカットを右クリックします。 [Windows PowerShell] を右クリックし、[管理者として実行] を選択して管理者特権モードで起動します。

上記の操作のいずれかを実行すると、PowerShell のショートカットが機能します。 これらの操作は、一度だけ実行する必要があります。

## <a name="powershell-modules-and-dsc-resources-report-errors-about-executionpolicy-on-windows-7"></a>Windows 7 で PowerShell モジュールと DSC リソースにより ExecutionPolicy についてエラーがレポートされる

Windows 7 で PowerShell モジュールと DSC リソースを使用すると、ExecutionPolicy のエラーが通知される場合があります。

**解決策:** 管理者特権の PowerShell セッション (管理者として実行) で、次のコマンドを実行して ExecutionPolicy を **RemoteSigned** に設定します。

```powershell
Set-ExecutionPolicy RemoteSigned
```

## <a name="connecting-to-an-old-remote-exchange-endpoint-causes-a-crash"></a>古いリモート Exchange のエンドポイントに接続するとクラッシュが発生する

古い Exchange のエンドポイントは、新しいエンドポイントにリダイレクトします。 リダイレクト ロジックに含まれたバグにより、クラッシュが発生します。

**解決策:** 新しいエンドポイントに直接接続します。

## <a name="software-inventory-logging-feature-is-erroneously-stopped-after-wmf-50-installation-on-windows-server-2012-r2"></a>Windows Server 2012 R2 に WMF 5.0 をインストールした後、ソフトウェア インベントリ ログ機能が誤って停止される

SIL を既に実行している Windows Server 2012 R2 に WMF 5.0 をインストールする場合、インストール後ソフトウェア インベントリ ログ機能が誤って停止されます。

**解決策:** インストール プロセスでソフトウェア インベントリ ログ機能が誤って停止されるため、WMF のインストール後に `Start-SilLogging` コマンドレットを 1 回実行します。

## <a name="get-childitem-does-not-work-if--literalpath-and--recurse-are-used-together"></a>-LiteralPath と -Recurse を一緒に使用すると `Get-ChildItem` が機能しない

ディレクトリ名に無効なワイルドカードが含まれている場合、-LiteralPath と -Recurse を一緒に使用すると、`Get-ChildItem` によって期待どおりの結果が生成されません。

**解決策:** 理想的ではありませんが、現在の対応策では、コマンドレットに依存するのではなく、スクリプトに再帰を実装します。

## <a name="sysprep-fails-after-wmf-50-installation"></a>WMF 5.0 のインストール後に Sysprep が失敗する

実行中の Windows Server のバージョンによって、この問題の解決策は 2 つあります。

**解決策:**

- **Windows Server 2008 R2** を実行するシステムの場合
  1. 管理者として PowerShell を開きます。
  2. 次のコマンドを実行します。

     ```powershell
     Set-SilLogging -TargetUri https://BlankTarget -CertificateThumbprint 0123456789
     ```

  3. 予想されるとおりにコマンドを実行してエラーを無視します。

     ```powershell
     Publish-SilData
     ```

  4. \Windows\System32\Logfiles\SIL\ ディレクトリ内のファイルを削除します。

     ```powershell
     Remove-Item -Recurse $env:SystemRoot\System32\Logfiles\SIL\
     ```

  5. 使用可能な重要な Windows 更新プログラムをすべてをインストールし、通常どおりに Sysyprep の操作を開始します。

- **Windows Server 2012** を実行するシステムの場合
  1. Sysprep されるようにサーバーに WMF 5.0 をインストールした後、管理者としてログインします。
  2. `\Windows\System32\Sysprep\ActionFiles\` ディレクトリから Windows ディレクトリ以外の場所 (例: `C:\`) に Generize.xml をコピーします。
  3. メモ帳で Generalize.xml のコピーを開きます。
  4. 次のテキストを検索して削除します。テキストごとに 1 つのインスタンスを削除する必要があります (これらはドキュメントの末尾付近にあります)。

     ```xml
     <sysprepOrder order="0x3200"></sysprepOrder>
     <sysprepOrder order="0x3300"></sysprepOrder>
     ```

  5. 編集した Generalize.xml のコピーを保存してファイルを閉じます。
  6. 管理者としてコマンド プロンプトを開きます。
  7. 次のコマンドを実行し、System32 フォルダーで Generalize.xml ファイルの所有権を取得します。

     ```powershell
     Takeown /f C:\Windows\System32\Sysprep\ActionFiles\Generalize.xml
     ```

  8. 次のコマンドを実行し、ファイルの適切なアクセス許可を設定します。

     ```powershell
     Cacls C:\Windows\System32\ Sysprep\ActionFiles\Generalize.xml /G `<AdministratorUserName>`:F
     ```

     - 確認のプロンプトで [はい] と回答します。
     - なお `<AdministratorUserName>` をコンピューター管理者のユーザー名に置き換える必要があります。 たとえば、"Administrator" にします。

  9. 次のコマンドを使用して、Sysprep ディレクトリに保存した編集済みファイルをコピーします。

     ```powershell
     xcopy C:\Generalize.xml C:\Windows\System32\Sysprep\ActionFiles\Generalize.xml
     ```

     - [はい] と回答して上書きします (上書きの確認メッセージが表示されない場合は、入力されたパスを再確認してください)。
     - 編集した Generalize.xml のコピーは C:\ にコピーされたことを想定しています。

  10. この解決策により Generalize.xml が更新されます。 Sysprep を実行して汎用化オプションを有効にしてください。
