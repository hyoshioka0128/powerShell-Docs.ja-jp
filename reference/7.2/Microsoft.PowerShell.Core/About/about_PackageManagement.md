---
description: PackageManagement は、ソフトウェアパッケージマネージャーのアグリゲーターです。
Locale: en-US
ms.date: 03/30/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_packagemanagement?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PackageManagement
ms.openlocfilehash: 22f47d01063778fca4f51a534b15d485b3beee28
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602170"
---
# <a name="about-packagemanagement"></a>PackageManagement について

## <a name="short-description"></a>概要
PackageManagement は、ソフトウェアパッケージマネージャーのアグリゲーターです。

## <a name="long-description"></a>詳細説明

PackageManagement 機能は、Windows PowerShell 5.0 で導入されました。

PackageManagement は、ソフトウェアパッケージ管理システムのための統一されたインターフェイスです。PackageManagement コマンドレットを実行して、ソフトウェアの検出、インストール、およびインベントリ (SDII) のタスクを実行できます。 基になるインストールテクノロジに関係なく、PackageManagement モジュールで共通のコマンドレットを実行して、パッケージの検索、インストール、またはアンインストールを行うことができます。パッケージリポジトリを追加、削除、および照会します。コンピューターに対してクエリを実行し、インストールされているソフトウェアパッケージを確認します。

PackageManagement では、他のソフトウェアパッケージ管理システムのサポートを有効にする柔軟なプラグインモデルがサポートされています。

PackageManagement モジュールは、PowerShell の Windows PowerShell 5.0 以降のリリースに含まれており、パッケージプロバイダー、パッケージソース、パッケージ自体という3つのレベルのパッケージ管理構造で機能します。 いくつかの用語を定義してみましょう。

- パッケージマネージャー: ソフトウェアパッケージ管理システム。 PackageManagement の用語では、これはパッケージプロバイダーです。
- パッケージプロバイダー: パッケージマネージャーの PackageManagement 用語。 例として、Windows インストーラー、Chocolatey などを含めることができます。
- パッケージソース: パッケージプロバイダーをリポジトリとして使用するように構成する、URL、ローカルフォルダー、またはネットワーク共有フォルダー。
- パッケージ: パッケージプロバイダーが管理し、特定のパッケージソースに格納されているソフトウェア。

PackageManagement モジュールには、次のコマンドレットが含まれています。 詳細については、 [PackageManagement](/powershell/module/packagemanagement) のヘルプを参照してください。

- `Get-PackageProvider`: PackageManagement に接続されているパッケージプロバイダーの一覧を返します。
- `Get-PackageSource`: パッケージプロバイダーに登録されているパッケージソースの一覧を取得します。
- `Register-PackageSource`: 指定したパッケージプロバイダーのパッケージソースを追加します。
- `Set-PackageSource`: 既存のパッケージソースのプロパティを設定します。
- `Unregister-PackageSource`: 登録されているパッケージソースを削除します。
- `Get-Package`: インストールされているソフトウェアパッケージの一覧を返します。
- `Find-Package`: 使用可能なパッケージソース内のソフトウェアパッケージを検索します。
- `Install-Package`: 1 つまたは複数のソフトウェアパッケージをインストールします。
- `Save-Package`: パッケージをインストールせずにローカルコンピューターに保存します。
- `Uninstall-Package`: 1 つまたは複数のソフトウェアパッケージをアンインストールします。

### <a name="package-provider-bootstrapping-and-dynamic-cmdlet-parameters"></a>パッケージプロバイダーブートストラップと動的コマンドレットパラメーター

既定では、PackageManagement はコアブートストラッププロバイダーに付属しています。 プロバイダーをブートストラップすることで、必要に応じて追加のパッケージプロバイダーをインストールできます。つまり、web サービスからプロバイダーを自動的にインストールするように求めるプロンプトに応答します。 任意の PackageManagement コマンドレットを使用してパッケージプロバイダーを指定できます。指定したプロバイダーが使用できない場合は、プロバイダーをブートストラップする (または自動的にインストールする) ことを確認するメッセージが表示されます。 次の例では、Chocolatey プロバイダーがまだインストールされていない場合、PackageManagement ブートストラップによってプロバイダーがインストールされます。

```powershell
Find-Package -Provider Chocolatey <PackageName>
```

Chocolatey プロバイダーがまだインストールされていない場合は、上記のコマンドを実行すると、インストールを求めるメッセージが表示されます。

```powershell
Install-Package <Chocolatey package Name> -ForceBootstrap
```

Chocolatey プロバイダーがまだインストールされていない場合は、上記のコマンドを実行すると、プロバイダーがインストールされます。ただし、ForceBootstrap パラメーターがコマンドに追加されているため、インストールするように求められることはありません。プロバイダーとパッケージの両方が自動的にインストールされます。

パッケージをインストールしようとしたときに、サポートプロバイダーがまだインストールされていない場合に、ForceBootstrap パラメーターをコマンドに追加しないと、PackageManagement によってプロバイダーをインストールするように求めるメッセージが表示されます。

PackageManagement コマンドでパッケージプロバイダーを指定すると、そのパッケージプロバイダーに固有の動的パラメーターを使用できるようになります。 特定の PackageManagement コマンドレットに対して Get-Help を実行すると、パラメーターセットの一覧が返され、使用可能なパッケージプロバイダーの動的パラメーターが個別のパラメーターセットにグループ化されます。

PackageManagement プロジェクトに関する詳細情報

Packagemanagement パッケージプロバイダーの作成方法を含め、PackageManagement open development プロジェクトの詳細については、GitHub の PackageManagement プロジェクトを参照してください https://oneget.org 。

## <a name="see-also"></a>関連項目

[Get-PackageProvider](xref:PackageManagement.Get-PackageProvider)

[Get-PackageSource](xref:PackageManagement.Get-PackageSource)

[Register-PackageSource](xref:PackageManagement.Register-PackageSource)

[Set-PackageSource](xref:PackageManagement.Set-PackageSource)

[Unregister-PackageSource](xref:PackageManagement.Unregister-PackageSource)

[Get-Package](xref:PackageManagement.Get-Package)

[Find-Package](xref:PackageManagement.Find-Package)

[Install-Package](xref:PackageManagement.Install-Package)

[Save-Package](xref:PackageManagement.Save-Package)

[Uninstall-Package](xref:PackageManagement.Uninstall-Package)

