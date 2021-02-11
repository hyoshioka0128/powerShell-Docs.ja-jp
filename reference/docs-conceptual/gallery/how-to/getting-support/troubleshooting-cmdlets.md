---
ms.date: 01/25/2021
title: コマンドレットのトラブルシューティング
description: この記事では、PowerShell ギャラリーを使用してエラーのトラブルシューティングを行うための情報と手順を示します
ms.openlocfilehash: 8139147683b655b5f8532c3068387db6df12a98f
ms.sourcegitcommit: 0f003644684422e425a59b7361121e05ac772e15
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/26/2021
ms.locfileid: "98771813"
---
# <a name="troubleshooting-cmdlets"></a>コマンドレットのトラブルシューティング

## <a name="how-to-resolve-warning-package-your-package-name-failed-to-download-issue"></a>"警告: パッケージ 'パッケージ名' をダウンロードできませんでした" の問題を解決する方法

一部のマシン上で `Install-Module` または `Update-Module` が失敗する場合があることがレポートされています。 調査の結果、ネットワーク接続の問題であることが判明しました。 最近、パッケージのダウンロードが安定するように、NuGet プロバイダーを更新しました。 下の指示に従い、最新版の NuGet プロバイダーをインストールし、モジュールをインストールするか、更新できます。 以下、'Azure' モジュールをサンプルとして使用します。

```powershell
Install-PackageProvider NuGet -MinimumVersion 2.8.5.206 -Force
Launch new PowerShell Console
Update-Module Azure -Verbose
```

## <a name="required-network-endpoints"></a>必要なネットワーク エンドポイント

Install および Update コマンドレットを使用する場合、PowerShell ギャラリーによって使用されるネットワーク エンドポイントに接続するためのインターネット アクセスが必要です。 お使いのネットワーク アクセス ポリシーで、次のエンドポイントへの接続が許可されていることを確認してください。

- `psg-prod-eastus.azureedge.net` - CDN ホスト名
- `az818661.vo.msecnd.net` - CDN ホスト名
- `devopsgallerystorage.blob.core.windows.net` - ストレージ アカウントのホスト名
- `*.powershellgallery.com` - Web サイト
- `go.microsoft.com` - リダイレクト サービス
- `onegetcdn.azureedge.net` - `PowerShellGet/PackageManagement` の NuGet プロバイダーのブートストラップ

> [!NOTE]
> PowerShell ギャラリー サービスが停止すると、PowerShell ギャラリーとやりとりするコマンドレットが予期しないエラーで失敗することがあります。 PowerShell ギャラリーの現在の状態を確認するには、GitHub の [PowerShell Gallery Status](https://github.com/PowerShell/PowerShellGallery/blob/master/psgallery_status.md) ページを参照してください。
