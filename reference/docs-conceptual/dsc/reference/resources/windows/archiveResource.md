---
ms.date: 07/16/2020
ms.topic: reference
title: DSC Archive リソース
description: DSC Archive リソース
ms.openlocfilehash: 28e2436683d7cb3b69f894ac75bb1a58b8eb1e8a
ms.sourcegitcommit: 196c7f8cd24560cac70c88acc89909f17a86aea9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2020
ms.locfileid: "93142420"
---
# <a name="dsc-archive-resource"></a>DSC Archive リソース

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.x

Windows PowerShell Desired State Configuration (DSC) の Archive リソースは、特定のパスでアーカイブ (.zip) ファイルをアンパックするメカニズムを備えています。

[!INCLUDE [Updated DSC Resources](../../../../../includes/dsc-resources.md)]

## <a name="syntax"></a>構文

```Syntax
Archive [string] #ResourceName
{
    Destination = [string]
    Path = [string]
    [ Checksum = [string] { CreatedDate | ModifiedDate | SHA-1 | SHA-256 | SHA-512 } ]
    [ Credential = [PSCredential] ]
    [ Force = [bool] ]
    [ Validate = [bool] ]
    [ Ensure = [string] { Absent | Present } ]
    [ DependsOn = [string[]] ]
    [ PsDscRunAsCredential = [PSCredential] ]
}
```

## <a name="properties"></a>Properties

|プロパティ |説明 |
|---|---|
| 宛先 | アーカイブ コンテンツが抽出されることを保証する場所を指定します。 |
| Path | アーカイブ ファイルのソース パスを指定します。 |
| Checksum | 2 つのファイルが同じであるかどうかを決定するときに使用するタイプを定義します。 **Checksum** が指定されていない場合、ファイルまたはディレクトリ名のみが比較に使用されます。 有効な値は、次のとおりです。 **SHA-1** 、 **SHA-256** 、 **SHA-512** 、 **createdDate** 、 **modifiedDate** 。 **Validate** を指定せずに **Checksum** を指定した場合、構成は失敗します。 |
| 資格情報 | 必要に応じて、指定されたアーカイブ パスと宛先にアクセスするためのアクセス許可を持つユーザー アカウントの資格情報。 |
| Force | 特定のファイル操作 (ファイルの上書き、空でないディレクトリの削除など) によって、エラーが発生します。 **Force** プロパティを使用すると、このようなエラーがオーバーライドされます。 既定値は **False** です。 |
| 検証| **Checksum** プロパティを使用して、アーカイブが署名と一致するかどうかを確認します。 **Validate** を指定せずに **Checksum** を指定した場合、構成は失敗します。 **Checksum** を指定せずに **Validate** を指定した場合、 _SHA-256_ **チェックサム** が既定で使用されます。 |

## <a name="common-properties"></a>共通プロパティ

|プロパティ |説明 |
|---|---|
|DependsOn |このリソースを構成する前に、他のリソースの構成を実行する必要があることを示します。 たとえば、最初に実行するリソース構成スクリプト ブロックの ID が ResourceName で、そのタイプが ResourceType である場合、このプロパティを使用する構文は `DependsOn = "[ResourceType]ResourceName"` になります。 |
|Ensure |アーカイブのコンテンツが **Destination** に存在するかどうかを決定します。 コンテンツが存在することを保証するには、このプロパティを **Present** に設定します。 コンテンツが存在しないことを保証するには、 **Absent** に設定します。 既定値は **Present** です。 |
|PsDscRunAsCredential |リソース全体を実行するための資格情報を設定します。 |

> [!NOTE]
> **PsDscRunAsCredential** という共通プロパティは、他の資格情報という文脈の中であらゆる DSC リソースを実行するために WMF 5.0 で追加されました。 詳細については、「[DSC リソースに対して資格情報を使用する](../../../configurations/runasuser.md)」を参照してください。

## <a name="example"></a>例

次の例は、使用されている承認済みの指定した宛先に、`Test.zip` という名前のアーカイブ ファイルのコンテンツが存在し、抽出され、承認されていることを、Archive リソースを使用して確実にする方法を示しています。

```powershell
Archive ArchiveExample {
    Ensure = "Present"
    Path = "C:\Users\Public\Documents\Test.zip"
    Destination = "C:\Users\Public\Documents\ExtractionPath"
}
```
