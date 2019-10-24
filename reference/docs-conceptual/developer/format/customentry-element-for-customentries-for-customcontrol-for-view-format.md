---
title: CustomControl for ビュー (Format) の CustomEntries の CustomEntry 要素Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: ac3c0a25-f2ca-4e28-b3dc-9cb06a76d92a
caps.latest.revision: 11
ms.openlocfilehash: 7804155bffeb1f0df8339f797bf59f8def56a3fc
ms.sourcegitcommit: 52a67bcd9d7bf3e8600ea4302d1fa8970ff9c998
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72364021"
---
# <a name="customentry-element-for-customentries-for-customcontrol-for-view-format"></a>View の CustomControl の CustomEntries の CustomEntry 要素 (書式)

カスタムコントロールビューの定義を提供します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) CustomControl 要素 (形式) の CustomEntries 要素の CustomControl for view (Format) ビューの Custommentry 要素 for View (Format)

## <a name="syntax"></a>構文

```xml
<CustomEntry>
  <EntrySelectedBy>...</EntrySelectedBy>
  <CustomItem>...</CustomItem>
</CustomEntry>
```

## <a name="attributes-and-elements"></a>属性と要素

次のセクションでは、属性、子要素、`CustomEntry` 要素の親要素について説明します。 定義によって表示される項目を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|[説明]|
|-------------|-----------------|
|[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)|省略可能な要素。<br /><br /> カスタムコントロールビューの定義、またはこの定義を使用するために存在する必要がある条件を使用する .NET 型を定義します。|
|[CustomEntry for ビューの CustomItem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md)|カスタムコントロール定義のコントロールを定義します。|

### <a name="parent-elements"></a>親要素

|要素|[説明]|
|-------------|-----------------|
|[CustomControl for View (Format) の CustomEntries 要素](./customentries-element-for-customcontrol-for-view-format.md)|カスタムコントロールビューの定義を提供します。 カスタムコントロールビューでは、1つまたは複数の定義を指定する必要があります。|

## <a name="remarks"></a>コメント

ほとんどの場合、カスタムコントロールビューごとに1つの定義のみが必要ですが、同じビューを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を指定できます。 このような場合は、オブジェクトまたはオブジェクトのセットごとに個別の定義を指定できます。

## <a name="see-also"></a>参照

[View (Format) の CustomControl 要素](./customcontrol-element-for-view-format.md)

[CustomEntry for ビューの CustomItem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md)

[CustomEntry for ビューの EntrySelectedBy 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md)

[PowerShell フォーマットファイルの作成](./writing-a-powershell-formatting-file.md)