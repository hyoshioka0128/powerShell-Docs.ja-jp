---
ms.date: 09/13/2016
ms.topic: reference
title: WideControl の WideItem の FormatString 要素 (書式)
description: WideControl の WideItem の FormatString 要素 (書式)
ms.openlocfilehash: f67a18e3ec4f1323e7f9be8904db518c679d53e5
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667880"
---
# <a name="formatstring-element-for-wideitem-for-widecontrol-format"></a>WideControl の WideItem の FormatString 要素 (書式)

プロパティまたはスクリプトの値をビューに表示する方法を定義する形式パターンを指定します。

Configuration 要素 (Format) ViewDefinitions 要素 (書式) ビュー要素 (Format) WideControl Element (format) WideEntries Element (format) WideEntry Element for WideItem (format) FormatString Element for WideControl (format) WideItem for WideControl (Format)

## <a name="syntax"></a>構文

```xml
<FormatString>PropertyPattern</FormatString>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `FormatString` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[WideControl の WideItem 要素 (書式)](./wideitem-element-for-widecontrol-format.md)|リストビューの行に表示される値を持つプロパティまたはスクリプトを定義します。|

## <a name="text-value"></a>テキスト値

データの書式設定に使用するパターンを指定します。 たとえば、このパターンを使用すると、型 system.string: {0: MMM} {0: dd} {0: HH}: {0: mm} のプロパティの値を書式設定[できます。](/dotnet/api/System.TimeSpan)

## <a name="remarks"></a>解説

書式指定文字列は、テーブルビュー、リストビュー、ワイドビュー、またはカスタムビューを作成するときに使用できます。 ビューに表示される値の書式設定の詳細については、「 [表示されるデータの書式設定](./formatting-displayed-data.md)」を参照してください。

ワイドビューでの書式指定文字列の使用の詳細については、「 [ワイドビューの作成](./creating-a-wide-view.md)」を参照してください。

## <a name="example"></a>例

次の例は、プロパティの値の書式設定文字列を定義する方法を示して `StartTime` います。

```xml
<WideItem>
  <PropertyName>StartTime</PropertyName>
  <FormatString>{0:MMM} (0:DD) (0:HH):(0:MM)</FormatString>
</WideItem>
```

## <a name="see-also"></a>参照

[ワイド ビューを作成する](./creating-a-wide-view.md)

[WideControl の WideItem 要素 (書式)](./wideitem-element-for-widecontrol-format.md)

[Windows PowerShell のフォーマットファイルと型ファイルの作成](./writing-a-powershell-formatting-file.md)
