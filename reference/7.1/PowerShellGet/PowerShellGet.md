---
Download Help Link: https://aka.ms/powershell71-help
Help Version: 7.1.0.0
keywords: powershell,コマンドレット
Locale: en-US
Module Guid: 1d73a601-4a6c-43c5-ba3f-619b18bbb404
Module Name: PowerShellGet
ms.date: 06/09/2017
schema: 2.0.0
title: PowerShellGet
ms.openlocfilehash: 577dc9a56da98d975b777e6cd48ecdcaafd3128d
ms.sourcegitcommit: 22c93550c87af30c4895fcb9e9dd65e30d60ada0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2020
ms.locfileid: "94892371"
---
# PowerShellGet モジュール

## 説明

PowerShellGet は、モジュール、DSC リソース、ロール機能、スクリプトなどの PowerShell アーティファクトを検出、インストール、更新、公開するためのコマンドを含むモジュールです。

> [!IMPORTANT]
> 2020年4月の時点で、PowerShell ギャラリーでは、トランスポート層セキュリティ (TLS) バージョン1.0 と1.1 がサポートされなくなりました。 TLS 1.2 以降を使用していない場合は、PowerShell ギャラリーにアクセスしようとするとエラーが発生します。 次のコマンドを使用して、TLS 1.2 を使用していることを確認します。
>
> `[Net.ServicePointManager]::SecurityProtocol = [Net.SecurityProtocolType]::Tls12`
>
> 詳細については、PowerShell ブログの [お知らせ](https://devblogs.microsoft.com/powershell/powershell-gallery-tls-support/) を参照してください。

## PowerShellGet コマンドレット

### [Find-Command](Find-Command.md)
モジュール内の PowerShell コマンドを検索します。

### [Find-DscResource](Find-DscResource.md)
Desired State Configuration (DSC) リソースを検索します。

### [Find-Module](Find-Module.md)
指定された条件に一致するリポジトリ内のモジュールを検索します。

### [Find-RoleCapability](Find-RoleCapability.md)
モジュール内のロール機能を検索します。

### [Find-Script](Find-Script.md)
スクリプトを検索します。

### [Get-InstalledModule](Get-InstalledModule.md)
PowerShellGet によってインストールされたコンピューター上のモジュールの一覧を取得します。

### [Get-InstalledScript](Get-InstalledScript.md)
インストールされているスクリプトを取得します。

### [Get-PSRepository](Get-PSRepository.md)
PowerShell リポジトリを取得します。

### [Install-Module](Install-Module.md)
1つ以上のモジュールをリポジトリからダウンロードし、ローカルコンピューターにインストールします。

### [Install-Script](Install-Script.md)
スクリプトをインストールします。

### [New-ScriptFileInfo](New-ScriptFileInfo.md)
メタデータを持つスクリプト ファイルを作成します。

### [Publish-Module](Publish-Module.md)
指定したモジュールをローカル コンピューターからオンライン ギャラリーに発行します。

### [Publish-Script](Publish-Script.md)
スクリプトを発行します。

### [Register-PSRepository](Register-PSRepository.md)
PowerShell リポジトリを登録します。

### [Save-Module](Save-Module.md)
モジュールとその依存関係をローカルコンピューターに保存しますが、モジュールはインストールしません。

### [Save-Script](Save-Script.md)
スクリプトを保存します。

### [Set-PSRepository](Set-PSRepository.md)
登録されているリポジトリの値を設定します。

### [Test-ScriptFileInfo](Test-ScriptFileInfo.md)
スクリプトのコメントブロックを検証します。

### [Uninstall-Module](Uninstall-Module.md)
モジュールをアンインストールします。

### [Uninstall-Script](Uninstall-Script.md)
スクリプトをアンインストールします。

### [Unregister-PSRepository](Unregister-PSRepository.md)
リポジトリの登録を解除します。

### [Update-Module](Update-Module.md)
指定したモジュールの最新バージョンをオンライン ギャラリーからダウンロードし、ローカル コンピューターにインストールします。

### [Update-ModuleManifest](Update-ModuleManifest.md)
モジュール マニフェスト ファイルを更新します。

### [Update-Script](Update-Script.md)
スクリプトを更新します。

### [Update-ScriptFileInfo](Update-ScriptFileInfo.md)
スクリプトの情報を更新します。
