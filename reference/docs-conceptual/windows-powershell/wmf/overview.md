---
ms.date: 04/19/2019
keywords: WMF, PowerShell, セットアップ
title: Windows Management Framework (WMF)
description: WMF は Windows PowerShell の前提要件です。 この記事では、WMF バージョンの履歴を示すと共に、WMF を検索してインストールする方法について説明します。
ms.openlocfilehash: 339b140325befea0b28aa470d4249170937f2c37
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92654048"
---
# <a name="windows-management-framework"></a>Windows Management Framework

Windows Management Framework (WMF) では、Windows の一貫した管理インターフェイスが提供されます。 WMF では、さまざまなバージョンの Windows クライアントおよび Windows Server を管理するためのシームレスな方法が提供されます。 WMF インストーラー パッケージには、管理機能の更新プログラムが含まれており、以前のバージョンの Windows で使用できます。

WMF をインストールすると、次の機能が追加または更新されます。

- Windows PowerShell
- Windows PowerShell Desired State Configuration (DSC)
- Windows PowerShell Integrated Script Environment (ISE)
- Windows リモート管理 (WinRM)
- Windows Management Instrumentation (WMI)
- Windows PowerShell Web サービス (Management OData IIS 拡張機能)
- ソフトウェア インベントリ ログ (SIL)
- Server Manager CIM Provider

## <a name="wmf-release-notes"></a>WMF リリース ノート

PowerShell と特定の WMF のその他のコンポーネントのさまざまな拡張に関する詳細については、下のリンクにアクセスし、リリース ノートを参照してください。

- [WMF 5.1](whats-new/release-notes.md#wmf-51-changes)
- [WMF 5.0](whats-new/release-notes.md#wmf-50-changes)
- [WMF 4.0](https://download.microsoft.com/download/3/D/6/3D61D262-8549-4769-A660-230B67E15B25/Windows%20Management%20Framework%204%200%20Release%20Notes.docx)
- [WMF 3.0](https://download.microsoft.com/download/E/7/6/E76850B8-DA6E-4FF5-8CCE-A24FC513FD16/WMF%203%20Release%20Notes.docx)

## <a name="wmf-availability-across-windows-operating-systems"></a>Windows オペレーティング システムに対する WMF の対応状況

|        オペレーティング システムのバージョン         | [WMF 5.1][]  | WMF 5.0<br>*サポート対象外* | [WMF 4.0][]  | [WMF 3.0][]  | [WMF 2.0][]  |
| --------------------------------------- | ------------ | --------------------------- | ------------ | ------------ | ------------ |
| Windows Server 2019                     | 出荷時にインストール済み |                             |              |              |              |
| Windows Server 2016                     | 出荷時にインストール済み |                             |              |              |              |
| Windows 10                              | 出荷時にインストール済み | 出荷時にインストール済み                |              |              |              |
| Windows Server 2012 R2                  | はい          | はい                         | 出荷時にインストール済み |              |              |
| Windows 8.1                             | はい          | はい                         | 出荷時にインストール済み |              |              |
| Windows Server 2012                     | はい          | はい                         | はい          | 出荷時にインストール済み |              |
| Windows 8<br>*サポート対象外*           |              |                             |              | 出荷時にインストール済み |              |
| Windows Server 2008 R2 SP1              | はい          | はい                         | はい          | はい          | 出荷時にインストール済み |
| Windows 7 SP1                           | はい          | はい                         | はい          | はい          | 出荷時にインストール済み |
| Windows Server 2008 SP2                 |              |                             |              | はい          | はい          |
| Windows Vista<br>*サポート対象外*       |              |                             |              |              | はい          |
| Windows Server 2003<br>*サポート対象外* |              |                             |              |              | はい          |
| Windows XP<br>*サポート対象外*          |              |                             |              | はい          | はい          |

- **出荷時にインストール済み** :指定されたバージョンの WMF の機能は、示されているバージョンの Windows クライアントおよび Windows Server に付属しています。
- **サポート対象外** :これらの製品は Microsoft でサポートされなくなりました。 サポートされている新しいバージョンにアップグレードする必要があります。 詳細については、「[Microsoft ライフサイクル ポリシー][]」ページを参照してください。

> [!NOTE]
> WMF 5.0 のインストーラーは利用できなくなりました。また、サポート対象外になりました。 WMF 5.1 に置き換えられました。

[Microsoft ライフサイクル ポリシー]: https://support.microsoft.com/lifecycle
[WMF 5.1]: https://aka.ms/wmf51download
[WMF 4.0]: https://aka.ms/wmf4download
[WMF 3.0]: https://aka.ms/wmf3download
[WMF 2.0]: https://aka.ms/wmf2download
