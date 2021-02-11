---
ms.date: 09/13/2016
ms.topic: reference
title: GroupBy の EntrySelectedBy の TypeName 要素 (書式)
description: GroupBy の EntrySelectedBy の TypeName 要素 (書式)
ms.openlocfilehash: 07cc92e9c501aa0eb2d219e416851be0fcd80f47
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645703"
---
# <a name="typename-element-for-entryselectedby-for-groupby-format"></a>GroupBy の EntrySelectedBy の TypeName 要素 (書式)

カスタムコントロールのこの定義を使用する .NET 型を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) の GroupBy 要素を使用して、groupby (形式) の CustomControl 要素に対して、groupby (形式) の CustomEntries 要素の CustomControl の CustomControl の CustomEntry 要素を指定します。 groupby (format) の EntrySelectedBy を指定します。 groupby (形式)

## <a name="syntax"></a>構文

```xml
<TypeName>Nameof.NetType</TypeName>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `TypeName` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-groupby-format.md)|このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

.NET 型の完全修飾名 (など) を指定し `System.IO.DirectoryInfo` ます。

## <a name="remarks"></a>解説

各コントロール定義には、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

カスタムコントロールビューのコンポーネントの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

## <a name="see-also"></a>参照

[カスタム コントロールを作成する](./creating-custom-controls.md)

[GroupBy の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-groupby-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
