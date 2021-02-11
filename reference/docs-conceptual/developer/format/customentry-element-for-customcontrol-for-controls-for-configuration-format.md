---
ms.date: 09/13/2016
ms.topic: reference
title: Configuration の Controls の CustomControl の CustomEntry 要素 (書式)
description: Configuration の Controls の CustomControl の CustomEntry 要素 (書式)
ms.openlocfilehash: 3967be86a1d6c12c7215ef19d50bac9fafd5ad6d
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648273"
---
# <a name="customentry-element-for-customcontrol-for-controls-for-configuration-format"></a>Configuration の Controls の CustomControl の CustomEntry 要素 (書式)

コモンコントロールの定義を提供します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

Configuration 要素 (Format) コントロールの configuration (format) コントロールの要素の構成 (形式) の CustomControl 要素の構成のためのコントロールの要素の構成 (書式) の CustomControl の構成 (形式) の CustomEntries 要素の構成のためのコントロールの CustomControl のための構成 (形式)

## <a name="syntax"></a>構文

```xml
<CustomEntry>
  <EntrySelectedBy>...</EntrySelectedBy>
  <CustomItem>...</CustomItem>
</CustomEntry>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `CustomEntry` ます。 定義によって表示される項目を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[Configuration の Controls の CustomEntry の EntrySelectedBy 要素 (書式)](./entryselectedby-element-for-customentry-for-controls-for-configuration-format.md)|省略可能な要素です。<br /><br /> コモンコントロールの定義、またはこのコントロールを使用するために存在する必要がある条件を使用する .NET 型を定義します。|
|[構成のための CustomEntry コントロールの CustomItem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md)|必須の要素です。<br /><br /> コントロールによって表示されるデータとその表示方法を定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration の CustomControl の CustomEntries 要素 (形式)](./customentries-element-for-customcontrol-for-controls-for-configuration-format.md)|コモンコントロールの定義を提供します。|

## <a name="remarks"></a>解説

ほとんどの場合、共通のカスタムコントロールごとに1つの定義のみが必要ですが、同じコントロールを使用して異なる .NET オブジェクトを表示する場合は、複数の定義を持つことができます。 このような場合は、オブジェクトまたはオブジェクトのセットごとに個別の定義を指定できます。

## <a name="see-also"></a>参照

[Configuration の CustomControl の CustomEntries 要素 (形式)](./customentries-element-for-customcontrol-for-controls-for-configuration-format.md)

[構成のための CustomEntry コントロールの CustomItem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
