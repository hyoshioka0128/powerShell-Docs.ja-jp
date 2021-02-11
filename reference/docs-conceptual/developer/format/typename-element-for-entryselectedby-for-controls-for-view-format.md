---
ms.date: 09/13/2016
ms.topic: reference
title: View の Controls の EntrySelectedBy の TypeName 要素 (書式)
description: View の Controls の EntrySelectedBy の TypeName 要素 (書式)
ms.openlocfilehash: 03a70b0c6c71910932f1d032c94c602cef8ab964
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647895"
---
# <a name="typename-element-for-entryselectedby-for-controls-for-view-format"></a>View の Controls の EntrySelectedBy の TypeName 要素 (書式)

コントロールのこの定義を使用する .NET 型を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (書式) コントロールの要素 (書式) コントロールのコントロール要素 (format) を表示するコントロールのコントロール要素 (書式) のコントロール要素 CustomControl の CustomEntries 要素ビューのコントロール (format) のコントロールのために、ビュー (format) の CustomEntry 要素を使用して、ビュー (形式) のコントロールに対して EntrySelectedBy のビュー (format) の TypeName 要素を指定します。

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
|[View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md)|このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。|

## <a name="text-value"></a>テキスト値

.NET 型の完全修飾名 (など) を指定し `System.IO.DirectoryInfo` ます。

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[View の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
