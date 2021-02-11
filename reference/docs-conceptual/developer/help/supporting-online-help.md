---
ms.date: 09/13/2016
ms.topic: reference
title: オンライン ヘルプのサポート
description: オンライン ヘルプのサポート
ms.openlocfilehash: 0164b5e6c6c8d66a6ab2467a8adfb7efffe0fe17
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645414"
---
# <a name="supporting-online-help"></a>オンライン ヘルプのサポート

PowerShell 3.0 以降では、 `Get-Help` powershell コマンドのオンライン機能をサポートする方法が2つあります。 このトピックでは、さまざまなコマンドの種類に対してこの機能を実装する方法について説明します。

## <a name="about-online-help"></a>オンラインヘルプについて

オンラインヘルプは、常に PowerShell の重要な部分でした。 コマンドレットでは、 `Get-Help` コマンドプロンプトでヘルプトピックを表示しますが、多くのユーザーは、色のコーディング、ハイパーリンク、コミュニティコンテンツおよび wiki ベースのドキュメントでのアイデアの共有など、オンラインでの閲覧のエクスペリエンスを好むことができます。 最も重要なのは、更新可能なヘルプが登場する前に、オンラインヘルプで最新バージョンのヘルプファイルが提供されていたことです。

PowerShell 3.0 で更新可能なヘルプが登場したことにより、オンラインヘルプは依然として重要な役割を果たします。 柔軟なユーザーエクスペリエンスに加えて、オンラインヘルプでは、ヘルプトピックをダウンロードするために更新可能なヘルプを使用しない、または使用できないユーザーにヘルプを提供します。

## <a name="how-get-help--online-works"></a>Get-Help-オンラインのしくみ

コマンドのオンラインヘルプトピックをユーザーが検索できるように、コマンドに `Get-Help` はオンラインパラメーターがあり、ユーザーの既定のインターネットブラウザーでコマンドのヘルプトピックのオンラインバージョンを開くことができます。

たとえば、次のコマンドは、コマンドレットのオンラインヘルプトピックを開き `Invoke-Command` ます。

```powershell
Get-Help Invoke-Command -Online
```

を実装するために `Get-Help -Online` 、 `Get-Help` コマンドレットは次の場所にあるオンラインバージョンヘルプトピックの UNIFORM RESOURCE IDENTIFIER (URI) を検索します。

- コマンドのヘルプトピックの「 **関連リンク** 」セクションの最初のリンク。 ヘルプトピックは、ユーザーのコンピューターにインストールする必要があります。 この機能は、PowerShell 2.0 で導入されました。

- 任意 **のコマンドのプロパティ。** コマンドのヘルプトピックがユーザーのコンピューターにインストールされていない場合でも、**このプロパティにアクセスできます。** この機能は、PowerShell 3.0 で導入されました。

  `Get-Help` によって、[ **関連リンク** ] セクションの最初のエントリで URI が検索されてから **、"" というプロパティ** 値が取得されます。 プロパティ値が間違っているか変更されている場合は、最初の関連リンクに別の値を入力することで上書きできます。 ただし、最初の関連リンクは、ヘルプトピックがユーザーのコンピューターにインストールされている場合にのみ機能します。

## <a name="adding-a-uri-to-the-first-related-link-of-a-command-help-topic"></a>コマンドヘルプトピックの最初の関連リンクへの URI の追加

コマンドの `Get-Help -Online` XML ベースのヘルプトピックの「 **関連リンク** 」セクションの最初のエントリに有効な URI を追加することで、任意のコマンドをサポートできます。 このオプションは、XML ベースのヘルプトピックでのみ有効で、ヘルプトピックがユーザーのコンピューターにインストールされている場合にのみ機能します。 ヘルプトピックがインストールされていて、URI が設定されている場合 **、この** 値はコマンドのプロパティよりも優先されます。

この機能をサポートするには、要素内の最初の要素の下の要素に URI を記述する必要があり `maml:uri` `maml:relatedLinks/maml:navigationLink` `maml:relatedLinks` ます。

次の XML は、正しい URI の配置を示しています。 `Online version:`要素内のテキストはベストプラクティスですが、 `maml:linkText` 必須ではありません。

```xml
<maml:relatedLinks>
    <maml:navigationLink>
        <maml:linkText>Online version:</maml:linkText>
        <maml:uri>https://go.microsoft.com/fwlink/?LinkID=113279</maml:uri>
    </maml:navigationLink>
    <maml:navigationLink>
        <maml:linkText>about_History</maml:linkText>
        <maml:uri/>
    </maml:navigationLink>
</maml:relatedLinks>
```

## <a name="adding-the-helpuri-property-to-a-command"></a>コマンドへのプロパティの追加

このセクションでは、異なる種類のコマンドに、"コマンド" プロパティを追加する方法について説明します。

### <a name="adding-a-helpuri-property-to-a-cmdlet"></a>コマンドレットへのプロパティの追加

C# で記述さ **れたコマンド** レットの場合は、コマンドレットクラスに **コマンドレット** を追加します。 属性の値は、またはで始まる URI である必要があり `http` `https` ます。

次のコードは、コマンドレットクラスのすべて **の属性を** 示して `Get-History` います。

```csharp
[Cmdlet(VerbsCommon.Get, "History", HelpUri = "https://go.microsoft.com/fwlink/?LinkID=001122")]
```

### <a name="adding-a-helpuri-property-to-an-advanced-function"></a>高度な関数へのプロパティの追加

高度な関数の **場合は、** [属性] に [プロパティ] を **追加します** 。 プロパティの値は、"http" または "https" で始まる URI である必要があります。

次のコードは、関数のすべて **の属性を** 示しています。 `New-Calendar`

```powershell
function New-Calendar {
    [CmdletBinding(SupportsShouldProcess=$true,
    HelpURI="https://go.microsoft.com/fwlink/?LinkID=01122")]
```

### <a name="adding-a-helpuri-attribute-to-a-cim-command"></a>CIM コマンドへの属性の追加

CIM コマンドの **場合は、** CDXML ファイル内のコマンド **レットメタデータ** 要素に、"/" 属性を追加します。
属性の値は、またはで始まる URI である必要があり `http` `https` ます。

次のコードは、CIM コマンドのすべての属性を示しています。 `Start-Debug`

```xml
<CmdletMetadata Verb="Debug" HelpUri="https://go.microsoft.com/fwlink/?LinkID=001122"/>
```

### <a name="adding-a-helpuri-attribute-to-a-workflow"></a>ワークフローへの属性の追加

PowerShell 言語で記述されたワークフローの場合は、を追加 **します。** ワークフローコードに対する ExternalHelp コメントディレクティブ。 ディレクティブの値は、またはで始まる URI である必要があり `http` `https` ます。

> [!NOTE]
> このプロパティは、PowerShell の XAML ベースのワークフローではサポートされていません。

次のコードは、を示しています。ワークフローファイル内の ExternalHelp ディレクティブ。

```powershell
# .ExternalHelp "https://go.microsoft.com/fwlink/?LinkID=138338"
```
