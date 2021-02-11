---
ms.date: 09/13/2016
ms.topic: reference
title: DefaultSettings 要素 (書式)
description: DefaultSettings 要素 (書式)
ms.openlocfilehash: 1c2055b38a416fe2d75fa20c6c87e92d9eed4285
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666724"
---
# <a name="defaultsettings-element-format"></a>DefaultSettings 要素 (書式)

書式設定ファイルのすべてのビューに適用される共通設定を定義します。 一般的な設定には、エラーの表示、テーブル内のテキストの折り返し、コレクションの展開方法の定義などがあります。

Configuration 要素 (Format) DefaultSettings 要素 (形式)

## <a name="syntax"></a>構文

```xml
<DefaultSettings>
  <ShowError/>
  <DisplayError/>
 <PropertyCountForTable>NumberOfProperties</PropertyCountFortable>
  <WrapTables/>
  <EnumerableExpansions>...</EnumerableExpansions>
</DefaultSettings>
```

## <a name="attributes-and-elements"></a>属性および要素

次のセクションでは、要素の属性、子要素、および親要素について説明し `DefaultSettings` ます。

### <a name="attributes"></a>属性

なし。

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[DisplayError 要素 (書式)](./displayerror-element-format.md)|省略可能な要素です。<br /><br /> データの表示中にエラーが発生した場合に、文字列 #ERR が表示されることを指定します。|
|[EnumerableExpansions 要素 (書式)](./enumerableexpansions-element-format.md)|省略可能な要素です。<br /><br /> .NET オブジェクトをビューに表示するときに展開するさまざまな方法を定義します。|
|[PropertyCountForTable (形式)](./propertycountfortable-element-format.md)|省略可能な要素です。<br /><br /> オブジェクトをテーブルビューに表示するために必要なプロパティの最小数を指定します。|
|[ShowError 要素 (書式)](./showerror-element-format.md)|省略可能な要素です。<br /><br /> データの一部を表示しているときにエラーが発生した場合に、完全なエラーレコードが表示されることを指定します。|
|[WrapTables 要素 (書式)](./wraptables-element-format.md)|省略可能な要素です。<br /><br /> 列の幅に合わない場合に、テーブル内のデータを次の行に移動するように指定します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[Configuration 要素](./configuration-element-format.md)|書式設定ファイルのトップレベルの要素を表します。|

## <a name="remarks"></a>解説

## <a name="see-also"></a>参照

[Configuration 要素](./configuration-element-format.md)

[DisplayError 要素 (書式)](./displayerror-element-format.md)

[EnumerableExpansions 要素 (書式)](./enumerableexpansions-element-format.md)

[PropertyCountForTable (形式)](./propertycountfortable-element-format.md)

[ShowError 要素 (書式)](./showerror-element-format.md)

[WrapTables 要素 (書式)](./wraptables-element-format.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
