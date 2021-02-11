---
ms.date: 07/10/2019
keywords: JEA, PowerShell, セキュリティ
title: JEA の前提条件
description: この記事では、JEA の使用を開始するために満たす必要のある前提条件について説明します。
ms.openlocfilehash: 5cc70a06887a2d0a840cc83117f865d3148056e1
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92501730"
---
# <a name="prerequisites"></a>前提条件

Just Enough Administration は、PowerShell 5.0 以降に含まれる機能です。 この記事では、JEA の使用を開始するために満たす必要のある前提条件について説明します。

## <a name="check-which-version-of-powershell-is-installed"></a>インストールされている PowerShell のバージョンを確認する

システムにインストールされている PowerShell のバージョンを確認するには、Windows PowerShell プロンプトで `$PSVersionTable` 変数を調べます。

```powershell
$PSVersionTable.PSVersion
```

```Output
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      14393  1000
```

JEA は PowerShell 5.0 以降で利用できます。 完全な機能のためには、システムで使用できる最新バージョンの PowerShell をインストールすることをお勧めします。 次の表は、Windows Server での JEA の可用性を示します。

| サーバーのオペレーティング システム |                JEA の可用性                |
| ----------------------- | ---------------------------------------------- |
| Windows Server 2016 以降    | プレインストール済み                                   |
| Windows Server 2012 R2  | WMF 5.1 のすべての機能                |
| Windows Server 2012     | WMF 5.1 のすべての機能                |
| Windows Server 2008 R2  | WMF 5.1 の制限された機能<sup>1</sup> |

JEA は、自宅または会社のコンピューターでも使用できます。

| クライアントのオペレーティング システム |                   JEA の可用性                   |
| ----------------------- | ---------------------------------------------------- |
| Windows 10 1607 以降        | プレインストール済み                                         |
| Windows 10 1603、1511   | 制限された機能でプレインストール済み<sup>2</sup> |
| Windows 10 1507         | 使用不可                                        |
| Windows 8、8.1          | WMF 5.1 のすべての機能                      |
| Windows 7               | WMF 5.1 の制限された機能<sup>1</sup>       |

- <sup>1</sup> Windows Server 2008 R2 または Windows 7 で、グループが管理するサービス アカウントを使用するように JEA を構成することはできません。 仮想アカウントと他の JEA 機能はサポートされています *。*

- <sup>2</sup> 次の JEA 機能は、Windows 10 バージョン 1511 と 1603 ではサポートされていません。

  - グループ管理されたサービス アカウントとして実行する
  - セッション構成での条件付きアクセス規則
  - ユーザー ドライブ
  - ローカル ユーザー アカウントにアクセス許可を付与する

  これらの機能を使うには、Windows をバージョン 1607 (Anniversary Update) 以降に更新します。

### <a name="install-windows-management-framework"></a>Windows Management Framework をインストールする

より古いバージョンの PowerShell を実行している場合、最新の Windows Management Framework (WMF) 更新プログラムでご利用のシステムを更新する必要がある可能性があります。 詳細については、[WMF のドキュメント](/powershell/scripting/wmf/overview)を参照してください。

ご利用のすべてのサーバーをアップグレードする前に、WMF とのワークロードの互換性をテストすることをお勧めします。

Windows 10 のユーザーは、現在のバージョンの Windows PowerShell を取得するには、最新の機能更新をインストールする必要があります。

## <a name="enable-powershell-remoting"></a>PowerShell リモート処理を有効にする

PowerShell リモート処理は、JEA 構築の基盤を提供します。 JEA を使用する前に、PowerShell リモート処理を有効にし、適切にセキュリティで保護されていることを確実にする必要があります。 詳細については、[WinRM セキュリティ](/powershell/scripting/learn/remoting/winrmsecurity)に関するページを参照してください。

Windows Server 2012、2012 R2、2016 では、PowerShell リモート処理は既定で有効になります。 管理者特権の PowerShell ウィンドウで次のコマンドを実行することにより、PowerShell リモート処理を有効にできます。

```powershell
Enable-PSRemoting
```

## <a name="enable-powershell-module-and-script-block-logging-optional"></a>PowerShell モジュールおよびスクリプト ブロック ログを有効にする (省略可能)

次の手順で、システム上のすべての PowerShell 操作のログを有効にします。 PowerShell モジュール ログは JEA には必要ありませんが、ユーザーが実行するコマンドが確実に中央の場所にログ記録されるように、有効にすることをお勧めします。

グループ ポリシーを使って、PowerShell モジュール ログ ポリシーを構成できます。

1. ワークステーションでローカル グループ ポリシー エディターを開くか、Active Directory ドメイン コントローラーのグループ ポリシー管理コンソールでグループ ポリシー オブジェクトを開きます
2. **[コンピューターの構成]\\[管理用テンプレート]\\[Windows コンポーネント]\\[Windows PowerShell]** に移動します。
3. **[モジュール ログを有効にする]** をダブルクリックします。
4. **[有効]** をクリックします。
5. [オプション] セクションで、モジュール名の横にある **[表示]** をクリックします。
6. ポップアップ ウィンドウに「`*`」を入力して、すべてのモジュールからのコマンドをログ記録します。
7. **[OK]** をクリックしてポリシーを設定します。
8. **[PowerShell スクリプト ブロックのログ記録を有効にする]** をダブルクリックします。
9. **[有効]** をクリックします。
10. **[OK]** をクリックしてポリシーを設定します。
11. (ドメインに参加しているコンピューターのみ) `gpupdate` を実行するか、グループ ポリシーが更新されたポリシーを処理して設定を適用するのを待ちます

グループ ポリシーを通してシステム全体の PowerShell トランスクリプトを有効にすることもできます。

## <a name="next-steps"></a>次のステップ

[ロール機能ファイルを作成する](role-capabilities.md)

[セッション構成ファイルを作成する](session-configurations.md)

## <a name="see-also"></a>関連項目

[WinRM セキュリティ](/powershell/scripting/learn/remoting/winrmsecurity)

[ブルー チームを支援する PowerShell](https://devblogs.microsoft.com/powershell/powershell-the-blue-team/)
