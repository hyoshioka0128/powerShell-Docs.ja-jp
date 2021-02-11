---
ms.date: 09/13/2016
ms.topic: reference
title: View の CustomEntry の EntrySelectedBy の TypeName 要素 (書式)
description: View の CustomEntry の EntrySelectedBy の TypeName 要素 (書式)
ms.openlocfilehash: 72bb88bccc2bbd62f7ed160b820cf9169cb69341
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645750"
---
# <a name="typename-element-for-entryselectedby-for-customentry-for-view-format"></a>View の CustomEntry の EntrySelectedBy の TypeName 要素 (書式)

カスタムコントロールビューのこの定義を使用する .NET 型を指定します。 定義に指定できる型の数に制限はありません。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (形式) CustomControl 要素 (形式) の CustomEntries for view (format) CustomControl for view (format) を使用して、customentry for ビューの entryselectedby 要素を指定します。この要素は、customentry for View 用の EntrySelectedBy の TypeName 要素を指定します。

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
|[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)|このカスタムコントロールビュー定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

.NET 型の完全修飾名 (など) を指定し `System.IO.DirectoryInfo` ます。

## <a name="remarks"></a>解説

各カスタムコントロールビュー定義には、少なくとも1つの型名、選択セット、または選択条件が定義されている必要があります。

カスタムコントロールビューのコンポーネントの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

## <a name="see-also"></a>参照

[カスタム コントロールを作成する](./creating-custom-controls.md)

[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
