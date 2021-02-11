---
ms.date: 09/13/2016
ms.topic: reference
title: ListControl の ListEntries 要素 (書式)
description: ListControl の ListEntries 要素 (書式)
ms.openlocfilehash: d4d6625bb92ea27863fc30d5bf5625f9275e4f69
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666605"
---
# <a name="listentries-element-for-listcontrol-format"></a>ListControl の ListEntries 要素 (書式)

リストビューの定義を提供します。 リストビューでは、1つまたは複数の定義を指定する必要があります。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) ListControl Element (Format) ListEntries 要素 (Format)

## <a name="syntax"></a>構文

```xml
<ListEntries>
  <ListEntry>...</ListEntry>
</ListEntries>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `ListEntries` ます。 少なくとも1つの子要素を指定する必要があります。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[ListEntry 要素 (Format)](./listentry-element-for-listcontrol-format.md)|リストビューの定義を提供します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[ListControl 要素 (書式)](./listcontrol-element-format.md)|ビューのリスト形式を定義します。|

## <a name="remarks"></a>解説

リストビューの詳細については、「 [リストビュー](./creating-a-list-view.md)」を参照してください。

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

[ListControl 要素 (書式)](./listcontrol-element-format.md)

[ListEntry 要素 (Format)](./listentry-element-for-listcontrol-format.md)

[リストビュー](./creating-a-list-view.md)

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
