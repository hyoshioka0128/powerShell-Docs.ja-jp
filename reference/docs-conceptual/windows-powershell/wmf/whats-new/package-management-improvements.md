---
ms.date: 06/12/2017
title: WMF 5.1 のパッケージ管理の機能強化
description: Windows PowerShell 5.1 には、更新された Package Management コマンドレットが含まれています。
ms.openlocfilehash: 572ebd1f0aa3cf09579e13c3ea52ae3e979e421f
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663189"
---
# <a name="improvements-to-package-management-in-wmf-51"></a>WMF 5.1 のパッケージ管理の機能強化

WMF 5.1 で行われた修正:

## <a name="version-alias"></a>バージョン エイリアス

**シナリオ** : パッケージ P1 のバージョン 1.0 および 2.0 がシステムにインストールされていて、バージョン 1.0 をアンインストールしたい場合、`Uninstall-Package -Name P1 -Version 1.0` を実行し、このコマンドレットを実行するとバージョン 1.0 がアンインストールされるものと期待します。 しかし、結果はバージョン 2.0 がアンインストールされます。

このようになるのは、`-Version` パラメーターが `-MinimumVersion` パラメーターのエイリアスであるためです。 PackageManagement は、最小バージョンが 1.0 という条件を満たすパッケージを探して、最新のバージョンを返します。 たいていの場合は最新バージョンを探すのが目的の結果なので、この動作は通常であれば想定したものです。 ただし、`Uninstall-Package` の場合には当てはまりません。

**解決策** : PackageManagement (別名 OneGet) および PowerShellGet で `-Version` エイリアス全体を削除しました 。

## <a name="multiple-prompts-for-bootstrapping-the-nuget-provider"></a>NuGet プロバイダーのブートストラップに対する複数のプロンプト

**シナリオ** : `Find-Module`、`Install-Module` または他の PackageManagement コマンドレットをコンピューターで初めて実行すると、PackageManagement は NuGet プロバイダーをブートストラップしようとします。 これは、PowerShellGet プロバイダーは PowerShell モジュールをダウンロードするために NuGet プロバイダーも使用するためです。
そのとき、PackageManagement は NuGet プロバイダーをインストールする許可をユーザーに求めます。 ユーザーがブートストラップに対して "はい" を選択すると、最新バージョンの NuGet プロバイダーがインストールされます。

ただし、古いバージョンの NuGet プロバイダーがコンピューターにインストールされている場合があり、古いバージョンの NuGet が PowerShell セッションに最初に読み込まれることがあります (PackageManagement での競合状態)。 ただし、PowerShellGet が動作するには新しいバージョンの NuGet プロバイダーが必要なので、PowerShellGet は PackageManagement に NuGet プロバイダーのブートストラップを再び要求します。
これにより、NuGet プロバイダーのブートストラップで複数のプロンプトが表示されます。

**解決策** : WMF 5.1 では、PackageManagement は、NuGet プロバイダーのブートストラップに対して複数のプロンプトが表示されるのを避けるため、NuGet プロバイダーの最新バージョンを読み込むようになっています。

この問題を回避策することもできます。古いバージョンの NuGet プロバイダー (NuGet-Anycpu.exe) が存在する場合は、$env:ProgramFiles\PackageManagement\ProviderAssemblies または $env:LOCALAPPDATA\PackageManagement\ProviderAssemblies から手動で削除します。

## <a name="support-for-packagemanagement-on-computers-with-intranet-access-only"></a>イントラネット アクセスのみのコンピューターでの PackageManagement のサポート

**シナリオ** : エンタープライズのシナリオで、ユーザーはイントラネットのみでインターネット アクセスのない環境で作業しています。 PackageManagement は WMF 5.0 でこのケースをサポートしていませんでした。

**シナリオ** : WMF 5.0 の PackageManagement は、イントラネット アクセスしかない (インターネットにアクセスできない) コンピューターをサポートしませんでした。

**解決策** : WMF 5.1 では、以下のようにして、イントラネット接続のみのコンピューターで PackageManagement を使用できます。

1. インターネットに接続できる別のコンピューターで、`Install-PackageProvider -Name NuGet` を使用して、NuGet プロバイダーをダウンロードします。

2. `$env:ProgramFiles\PackageManagement\ProviderAssemblies\nuget` または `$env:LOCALAPPDATA\PackageManagement\ProviderAssemblies\nuget` のいずれかで NuGet プロバイダーを見つけます。

3. イントラネット コンピューターがアクセスできるフォルダーまたはネットワーク共有の場所にバイナリをコピーし、`Install-PackageProvider -Name NuGet -Source <Path to folder>` を使用して NuGet プロバイダーをインストールします。

## <a name="event-logging-improvements"></a>イベント ログの機能強化

パッケージをインストールすると、コンピューターの状態が変化します。 WMF 5.1 の PackageManagement は、`Install-Package`、`Uninstall-Package`、`Save-Package` アクティビティのイベントを Windows イベント ログに記録するようになりました。 イベント ログは PowerShell の場合と同じで、`Microsoft-Windows-PowerShell, Operational` です。

## <a name="support-for-basic-authentication"></a>基本認証のサポート

WMF 5.1 の PackageManagement は、基本認証を必要とするリポジトリからのパッケージの検索とインストールをサポートします。 `Find-Package` および `Install-Package` コマンドレットに資格情報を渡すことができます。 次に例を示します。

```powershell
Find-Package -Source <SourceWithCredential> -Credential (Get-Credential)
```

## <a name="support-for-using-packagemanagement-behind-a-proxy"></a>プロキシの背後での PackageManagement の使用のサポート

WMF 5.1 の PackageManagement は、新しいプロキシ パラメーター `-ProxyCredential` と `-Proxy` を受け取るようになりました。 これらのパラメーターを使用すると、プロキシの URL と資格情報を PackageManagement コマンドレットに対して指定できます。 既定では、システムのプロキシ設定が使用されます。 次に例を示します。

```powershell
Find-Package -Source https://www.nuget.org/api/v2/ -Proxy http://www.myproxyserver.com -ProxyCredential (Get-Credential)
```
