---
ms.date: 08/09/2017
title: Windows PowerShell のインストール
description: この記事では、さまざまなバージョンの Windows に Windows PowerShell をインストールする方法について説明します。
ms.openlocfilehash: 04e6d791e6895dd50825c58c905ff9cf8fa86ca8
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663980"
---
# <a name="installing-windows-powershell"></a>Windows PowerShell のインストール

既定では、Windows PowerShell は、Windows 7 SP1 および Windows Server 2008 R2 SP1 以降のすべての Windows にインストールされています。

PowerShell 6 以降を使う場合は、Windows PowerShell ではなく PowerShell Core をインストールする必要があります。 詳細については、「[Windows への PowerShell Core のインストール](../../install/Installing-PowerShell-Core-on-Windows.md)」を参照してください。

## <a name="finding-powershell-in-windows-10-81-80-and-7"></a>Windows 10、8.1、8.0、7 で PowerShell を見つける

Windows で PowerShell コンソールや Integrated Scripting Environment (ISE) を見つけるのは、その場所が Windows のあるバージョンから次のバージョンへ移動するため、難しい場合があります。

次の表は、使用している Windows のバージョンで PowerShell を見つけるのに役立ちます。 ここに一覧されているバージョンはすべて、リリース時の元のバージョンです。更新プログラムは含まれていません。

### <a name="for-console"></a>コンソールの場合

|     Version      |                                                            Location                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Windows 10       | 左下隅にある Windows アイコンをクリックして、「PowerShell」と入力し始めます                                                                  |
| Windows 8.1、8.0 | スタート画面で、「PowerShell」と入力し始めます。<br/>デスクトップ上の場合は、左下隅にある Windows アイコンをクリックして、「PowerShell」と入力し始めます |
| Windows 7 SP1    | 左下隅にある Windows アイコンをクリックして、検索ボックスに「PowerShell」と入力し始めます                                                |

### <a name="for-ise"></a>ISE の場合

|     Version      |                                                            Location                                                            |
| ---------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| Windows 10       | 左下隅にある Windows アイコンをクリックして、「ISE」と入力し始めます                                                                         |
| Windows 8.1、8.0 | スタート画面で、「 **PowerShell ISE** 」と入力します。<br/>デスクトップで、左下隅にある Windows アイコンをクリックして、「 **PowerShell ISE** 」と入力します |
| Windows 7 SP1    | 左下隅にある Windows アイコンをクリックして、検索ボックスに「PowerShell」と入力し始めます                                                |

## <a name="finding-powershell-in-windows-server-versions"></a>Windows Server バージョンで PowerShell を見つける

Windows Server 2008 R2 以降では、グラフィカル ユーザー インターフェイス (GUI) を含めずに、Windows オペレーティング システムをインストールできます。 GUI なしの Windows Server のエディションは **Core** エディションという名前で、GUI ありのエディションは **Desktop** という名前です。

### <a name="windows-server-core-editions"></a>Windows Server Core エディション

すべての Core エディションで、サーバーにログインすると、Windows コマンド プロンプト ウィンドウが表示されます。

「`powershell`」と入力して **Enter** キーを押し、コマンド プロンプト セッション内で PowerShell を開始します。 「`exit`」と入力して PowerShell セッションを終了し、コマンド プロンプトに戻ります。

### <a name="windows-server-desktop-editions"></a>Windows Server Desktop エディション

すべての Desktop エディションで、左下隅にある Windows アイコンをクリックして、「PowerShell」と入力し始めます。 コンソールと ISE のオプションが両方表示されます。

上記のルールの例外は、Windows Server 2008 R2 SP1 の ISE のみです。この場合は、左下隅にある Windows アイコンをクリックして、「PowerShell ISE」と入力します。

## <a name="how-to-check-the-version-of-powershell"></a>PowerShell のバージョンを確認する方法

インストールした PowerShell のバージョンを確認するには、PowerShell コンソール (または ISE) を起動し、「`$PSVersionTable`」と入力して **Enter** キーを押します。 `PSVersion` の値を探します。

## <a name="upgrading-existing-windows-powershell"></a>既存の Windows PowerShell をアップグレードする

PowerShell のインストール パッケージは、WMF インストーラー内に含まれています。 WMF インストーラーのバージョンは、PowerShell のバージョンに一致します。Windows PowerShell に、スタンドアロンのインストーラーはありません。

PowerShell の既存のバージョンを更新する場合、Windows では、以下の表を使って、更新する PowerShell のバージョン用のインストーラーを見つけます。

|                    Windows                     |                                  PS 3.0                                   |                                  PS 4.0                                   |                                  PS 5.0                                   |                                  PS 5.1                                   |
| ---------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Windows 10 (注 1 を参照してください)<br/>Windows Server 2016 | -                                                                         | -                                                                         | -                                                                         | インストール済み                                                                 |
| Windows 8.1<br/>Windows Server 2012 R2         | -                                                                         | インストール済み                                                                 | [WMF 5.0](https://www.microsoft.com/download/details.aspx?id=50395) | [WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616) |
| Windows 8<br/>Windows Server 2012              | インストール済み                                                                 | [WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855) | [WMF 5.0](https://www.microsoft.com/download/details.aspx?id=50395) | [WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616) |
| Windows 7 SP1<br/>Windows Server 2008 R2 SP1   | [WMF 3.0](https://www.microsoft.com/download/details.aspx?id=34595) | [WMF 4.0](https://www.microsoft.com/download/details.aspx?id=40855) | [WMF 5.0](https://www.microsoft.com/download/details.aspx?id=50395) | [WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616) |

> [!NOTE]
> 自動更新を有効にしている Windows 10 の最初のリリースでは、PowerShell はバージョン 5.0 から 5.1 に更新されます。 元のバージョンの Windows 10 が Windows Update によって更新されない場合、PowerShell のバージョンは 5.0 です。

## <a name="need-azure-powershell"></a>Azure PowerShell が必要な場合

**Azure PowerShell** を探している場合は、「 [Overview of Azure PowerShell](/powershell/azure/overview)」 (Azure PowerShell の概要) から開始することができます。

その他にも、[Azure PowerShell のインストールと構成](/powershell/azure/install-az-ps)が必要な場合があります

## <a name="see-also"></a>参照

[Windows PowerShell のシステム要件](Windows-PowerShell-System-Requirements.md)

[Windows PowerShell の開始](../Starting-Windows-PowerShell.md)
