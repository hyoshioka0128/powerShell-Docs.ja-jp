---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: Linux 用 DSC の nxArchive リソース
ms.openlocfilehash: 800954478f149e29c22d1a88304c3be9950f109a
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62078046"
---
# <a name="dsc-for-linux-nxarchive-resource"></a>Linux 用 DSC の nxArchive リソース

PowerShell Desired State Configuration (DSC) の **nxArchive** リソースは、Linux ノード上の特定のパスでアーカイブ (.tar、.zip) ファイルをアンパックするメカニズムを備えています。

## <a name="syntax"></a>構文

```
nxArchive <string> #ResourceName
{
    SourcePath = <string>
    DestinationPath = <string>
    [ Checksum = <string> { ctime | mtime | md5 }  ]
    [ Force = <bool> ]
    [ DependsOn = <string[]> ]
    [ Ensure = <string> { Absent | Present }  ]
}
```

## <a name="properties"></a>プロパティ

|  プロパティ |  説明 |
|---|---|
| SourcePath| アーカイブ ファイルのソース パスを指定します。 これは .tar、.zip、または .tar.gz ファイルである必要があります。 |
| DestinationPath| アーカイブ コンテンツが抽出されることを保証する場所を指定します。|
| チェックサム| ソース アーカイブが更新されたかどうかを確認するときに使用するタイプを定義します。 値は、"ctime"、"mtime"、または "md5" です。 既定値は "md5" です。|
| Force| 特定のファイル操作 (ファイルの上書き、空でないディレクトリの削除など) によって、エラーが発生します。 **Force** プロパティを使用すると、このようなエラーがオーバーライドされます。 既定値は **$false** です。|
| DependsOn | このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの **ID** が **ResourceName** で、そのタイプが **ResourceType** である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。|
| Ensure| アーカイブのコンテンツが **Destination** に存在するかどうかを決定します。 コンテンツが存在することを保証するには、このプロパティを "Present" に設定します。 コンテンツが存在しないことを保証するには、"Absent" に設定します。 既定値は "Present" です。|

## <a name="example"></a>例

次の例は、**nxArchive** リソースを使用して、`website.tar` というアーカイブ ファイルのコンテンツが指定した宛先に存在し、抽出されていることを保証する方法を示します。

```
Import-DSCResource -Module nx

nxFile SyncArchiveFromWeb
{
   Ensure = "Present"
   SourcePath = “http://release.contoso.com/releases/website.tar”
   DestinationPath = "/usr/release/staging/website.tar"
   Type = "File"
   Checksum = “mtime”
}

nxArchive SyncWebDir
{
   SourcePath = “/usr/release/staging/website.tar”
   DestinationPath = “/usr/local/apache2/htdocs/”
   Force = $false
   DependsOn = "[nxFile]SyncArchiveFromWeb"
}
```