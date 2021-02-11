---
Download Help Link: https://go.microsoft.com/fwlink/?linkid=392040
Help Version: 5.2.0.0
keywords: powershell,コマンドレット
Locale: en-US
Module Guid: 4ae9fd46-338a-459c-8186-07f910774cb8
Module Name: PackageManagement
ms.date: 06/09/2017
schema: 2.0.0
title: PackageManagement
ms.openlocfilehash: 208545677771270ad8f2e9d76ba01046b07e86e2
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94892782"
---
# PackageManagement モジュール

## 説明

このトピックでは、Package Management コマンドレットのヘルプトピックを示します。

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## PackageManagement コマンドレット

### [Find-Package](Find-Package.md)
使用可能なパッケージソース内のソフトウェアパッケージを検索します。

### [Find-PackageProvider](Find-PackageProvider.md)
インストールに使用できる Package Management パッケージプロバイダーの一覧を返します。

### [Get-Package](Get-Package.md)
**PackageManagement** と共にインストールされたすべてのソフトウェアパッケージの一覧を返します。

### [Get-PackageProvider](Get-PackageProvider.md)
Package Management に接続されているパッケージプロバイダーの一覧を返します。

### [Get-PackageSource](Get-PackageSource.md)
パッケージプロバイダーに登録されているパッケージソースの一覧を取得します。

### [Import-PackageProvider](Import-PackageProvider.md)
Package Management パッケージプロバイダーを現在のセッションに追加します。

### [Install-Package](Install-Package.md)
1つまたは複数のソフトウェアパッケージをインストールします。

### [Install-PackageProvider](Install-PackageProvider.md)
1つまたは複数の Package Management パッケージプロバイダーをインストールします。

### [Register-PackageSource](Register-PackageSource.md)
指定したパッケージプロバイダーのパッケージソースを追加します。

### [Save-Package](Save-Package.md)
パッケージをインストールせずにローカルコンピューターに保存します。

### [Set-PackageSource](Set-PackageSource.md)
指定したパッケージプロバイダーのパッケージソースを置き換えます。

### [Uninstall-Package](Uninstall-Package.md)
1つまたは複数のソフトウェアパッケージをアンインストールします。

### [Unregister-PackageSource](Unregister-PackageSource.md)
登録されているパッケージソースを削除します。
