---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListEntry 要素 (書式)
description: ListControl の ListEntry 要素 (書式)
ms.openlocfilehash: 96ae5fcdd837d2491d6c7ff6a375fef1d83ae3e9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666571"
---
# <a name="listentry-element-for-listcontrol-format"></a>ListControl の ListEntry 要素 (書式)

リストビューの定義を提供します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (Format) ListEntries 要素 (format) ListEntry 要素 (形式)

## <a name="syntax"></a>構文

```xml
<ListEntry>
  <EntrySelectedBy>...</EntrySelectedBy>
  <ListItems>...</ListItems>
</ListEntry>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ListEntry` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ListEntry の EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-listentry-for-listcontrol-format.md)|省略可能な要素です。<br /><br /> このリストビュー定義を使用する .NET オブジェクト、またはこの定義を使用するために必要な条件を定義します。|
|[ListItems 要素 (Format)](./listitems-element-for-listentry-for-listcontrol-format.md)|必須の要素です。<br /><br /> リストビューによって値が表示されるプロパティとスクリプトを定義します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ListEntries 要素 (書式)](./listentries-element-for-listcontrol-format.md)|リストビューの定義を提供します。|

## <a name="remarks"></a>解説

リストビューは、各オブジェクトのプロパティ値またはスクリプト値を表示するリスト形式です。 リストビューの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

この例は、 [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトのリストビューを定義する XML 要素を示しています。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <ListControl>
    <ListEntries>
      <ListEntry>
        <ListItems>...</ListItems>
      </ListEntry>
    </ListEntries>
  </ListControl>
</View>
```

## <a name="see-also"></a>参照

[リスト ビューを作成する](./creating-a-list-view.md)

[ListEntry の EntrySelectedBy 要素 (形式)](./entryselectedby-element-for-listentry-for-listcontrol-format.md)

[ListEntries 要素 (書式)](./listentries-element-for-listcontrol-format.md)

[ListItems 要素 (Format)](./listitems-element-for-listentry-for-listcontrol-format.md)

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
