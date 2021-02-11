---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: リソースをパッケージ化してプル サーバーにアップロードする
description: この記事では、プル サーバーにリソースをアップロードして、DSC によって管理されるノードの構成によってそれらをダウンロードできるようにする方法について説明します。
ms.openlocfilehash: a19d04346a0ae546cfcaf70701fde870d3839f65
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661697"
---
# <a name="package-and-upload-resources-to-a-pull-server"></a>リソースをパッケージ化してプル サーバーにアップロードする

以下のセクションでは、プル サーバーを既にセットアップしてあるものとします。 プル サーバーをセットアップしていない場合は、次のガイドを使用できます。

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)

各ターゲット ノードは、構成やリソースをダウンロードし、さらにその状態を報告するように構成できます。 この記事では、ダウンロードできるようにリソースをアップロードする方法、およびリソースを自動的にダウンロードするようにクライアントを構成する方法を示します。 ノードは、割り当てられた構成を **プル** または **プッシュ** (v5) によって受け取ると、構成で必要なすべてのリソースを LCM で指定された場所から自動的にダウンロードします。

## <a name="package-resource-modules"></a>リソース モジュールをパッケージ化する

クライアントでダウンロードできる各リソースは、`.zip` ファイルに格納する必要があります。 以下の例では、[xPSDesiredStateConfiguration](https://www.powershellgallery.com/packages/xPSDesiredStateConfiguration/8.4.0.0) リソースを使用して必要な手順を示します。

> [!NOTE]
> PowerShell 4.0 を使用しているクライアントがある場合は、リソース フォルダーの構造をフラット化し、すべてのバージョン フォルダーを削除する必要があります。 詳しくは、「[Multiple Resource Versions (複数のリソース バージョン)](../configurations/import-dscresource.md#multiple-resource-versions)」をご覧ください。

好みのユーティリティ、スクリプト、または方法を使って、リソース ディレクトリを圧縮することができます。 Windows の場合、`xPSDesiredStateConfiguration` ディレクトリを " _右クリック_ " して、 **[送信先]** 、 **[圧縮フォルダー]** の順に選択します。

![右クリック - [送信先] - [圧縮フォルダー]](media/package-upload-resources/right-click.gif)

### <a name="naming-the-resource-archive"></a>リソース アーカイブの名前を指定する

リソース アーカイブには、次の形式で名前を付ける必要があります。

```
{ModuleName}_{Version}.zip
```

上の例では、`xPSDesiredStateConfiguration.zip` の名前を `xPSDesiredStateConfiguration_8.4.4.0.zip` に変更する必要があります。

### <a name="create-checksums"></a>チェックサムを作成する

リソース モジュールを圧縮して名前を変更した後は、 **チェックサム** を作成する必要があります。 **チェックサム** は、クライアント上の LCM によって、リソースが変更されていて、再度ダウンロードする必要があるかどうかを判断するために使われます。 次の例で示すように、 **チェックサム** は [New-DSCCheckSum](/powershell/module/PSDesiredStateConfiguration/New-DSCCheckSum) コマンドレットで作成できます。

```powershell
New-DscChecksum -Path .\xPSDesiredStateConfiguration_8.4.4.0.zip
```

出力は示されませんが、"xPSDesiredStateConfiguration_8.4.4.0.zip.checksum" が表示されるようになるはずです。 また、`-Path` パラメーターを使用すると、ファイルのディレクトリに対して `New-DSCCheckSum` を実行することもできます。 チェックサムが既に存在する場合は、`-Force` パラメーターを使用して強制的に再作成できます。

### <a name="where-to-store-resource-archives"></a>リソース アーカイブを格納する場所

#### <a name="on-a-dsc-http-pull-server"></a>DSC HTTP プル サーバー上

HTTP プル サーバーをセットアップするときは、「 [DSC HTTP プル サーバーを設定する](pullServer.md)」で説明されているように、 **ModulePath** キーと **ConfigurationPath** キーに対するディレクトリを指定します。 **ConfigurationPath** キーは、".mof" ファイルを格納する必要がある場所を示します。 **ModulePath** は、DSC リソース モジュールを格納する必要がある場所を示します。

```powershell
    xDscWebService PSDSCPullServer
    {
    ...
        ModulePath              = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules"
        ConfigurationPath       = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration"
    ...
    }

```

#### <a name="on-an-smb-share"></a>SMB 共有上

**ResourceRepositoryShare** を指定した場合は、プル クライアントをセットアップするときに、 **ResourceRepositoryShare** ブロックの **SourcePath** ディレクトリに、アーカイブとチェックサムを格納します。

```powershell
ConfigurationRepositoryShare SMBPullServer
{
    SourcePath = '\\SMBPullServer\Configurations'
}

ResourceRepositoryShare SMBResourceServer
{
    SourcePath = '\\SMBPullServer\Resources'
}
```

**ConfigurationRepositoryShare** だけを指定した場合は、プル クライアントをセットアップするときに、 **ConfigurationRepositoryShare** ブロックの **SourcePath** ディレクトリに、アーカイブとチェックサムを格納します。

```powershell
ConfigurationRepositoryShare SMBPullServer
{
    SourcePath = '\\SMBPullServer\Pull'
}
```

#### <a name="updating-resources"></a>リソースの更新

アーカイブの名前のバージョン番号を変更するか、新しいチェックサムを作成することにより、強制的にノードにリソースを更新させることができます。 プル クライアントでは、LCM が更新されるときに、必要なリソースの新しいバージョンだけでなく更新されたチェックサムも確認されます。

## <a name="see-also"></a>参照

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)
