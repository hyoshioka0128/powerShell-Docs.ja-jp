---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl 要素 (書式)
description: ListControl 要素 (書式)
ms.openlocfilehash: cd5687ac8e94e2245d1ae2de8b2206185e5b8ca4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666588"
---
# <a name="listcontrol-element-format"></a>ListControl 要素 (書式)

ビューのリスト形式を定義します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl 要素 (形式)

## <a name="syntax"></a>構文

```xml
<ListControl>
  <ListEntries>...</ListEntries>
</ListControl>

```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ListControl` ます。 この要素には、1つの子要素のみを含める必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ListEntries 要素 (書式)](./listentries-element-for-listcontrol-format.md)|必須の要素です。<br /><br /> リストビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[View 要素 (書式)](./view-element-format.md)|1つ以上のオブジェクトのメンバーを表示するために使用されるビューを定義します。|

## <a name="remarks"></a>解説

リストビューの作成の詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

## <a name="example"></a>例

この例では、 [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトのリストビューを示します。

```
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <ListControl>
    <ListEntries>
      <ListEntry>...</ListEntry>
    </ListEntries>
  </ListControl>
</View>
```

## <a name="see-also"></a>参照

[View 要素 (書式)](./view-element-format.md)

[ListEntries 要素 (書式)](./listentries-element-for-listcontrol-format.md)

[リスト ビューを作成する](./creating-a-list-view.md)

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
