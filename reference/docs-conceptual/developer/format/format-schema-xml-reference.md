---
ms.date: 09/13/2016
ms.topic: reference
title: 書式設定スキーマ XML リファレンス
description: 書式設定スキーマ XML リファレンス
ms.openlocfilehash: f59016df91fe458393655853b9eada0875a8dcb1
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667931"
---
# <a name="format-schema-xml-reference"></a>書式設定スキーマ XML リファレンス

このセクションのトピックでは、フォーマットファイル (Format.ps1xml ファイル) で使用される XML 要素について説明します。 書式設定ファイルは、.NET オブジェクトの表示方法を定義します。オブジェクト自体は変更されません。

## <a name="in-this-section"></a>このセクションの内容

[TableControl (Format) の TableColumnHeader の Alignment 要素](./alignment-element-for-tablecolumnheader-for-tablecontrol-format.md) 列ヘッダー内のデータを表示する方法を定義します。

[TableControl (Format) の TableColumnItem の Alignment 要素](./alignment-element-for-tablecolumnitem-for-tablecontrol-format.md) 行のデータをどのように表示するかを定義します。

[TableControl の AutoSize 要素 (Format)](./autosize-element-for-tablecontrol-format.md) データのサイズに基づいて列のサイズと列の数を調整するかどうかを指定します。

[WideControl の Autosize 要素 (形式)](./autosize-element-for-widecontrol-format.md) データのサイズに基づいて列のサイズと列の数を調整するかどうかを指定します。

[WideControl の Columnnumber 要素 (形式)](./columnnumber-element-for-widecontrol-format.md) ワイドビューに表示される列の数を指定します。

[Configuration 要素 (形式)](./configuration-element-format.md) 書式設定ファイルのトップレベルの要素を表します。

[Configuration のコントロールの Control 要素 (Format)](./control-element-for-controls-for-configuration-format.md) 書式設定ファイルのすべてのビューおよびコントロールを参照するために使用される名前によって使用できるコモンコントロールを定義します。

[ビューのコントロールの Control 要素 (Format)](./control-element-for-controls-for-view-format.md) ビューで使用できるコントロールと、コントロールを参照するために使用される名前を定義します。

[Configuration の Controls 要素 (Format)](./controls-element-for-configuration-format.md) 書式設定ファイルのすべてのビューで使用できる共通コントロールを定義します。

[View (Format) の Controls 要素](./controls-element-for-view-format.md) 特定のビューで使用できるビューコントロールを定義します。

[構成用のコントロールの CustomControl 要素 (形式)](./customcontrol-element-for-control-for-controls-for-configuration-format.md) コントロールを定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[View (Format) コントロールのコントロールの CustomControl 要素](./customcontrol-element-for-control-for-controls-for-view-format.md) ビューによって使用されるコントロールを定義します。

[GroupBy (Format) の CustomControl 要素](./customcontrol-element-for-groupby-format.md) 新しいグループを表示するカスタムコントロールを定義します。

[CustomControl 要素 (Format)](./customcontrol-element-for-groupby-format.md) ビューのカスタムコントロール形式を定義します。

[構成用のコントロールの式のバインドの CustomControlName 要素 (形式)](./customcontrolname-element-for-expressionbinding-for-controls-for-configuration-format.md) コモンコントロールの名前を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[View (Format) 用のコントロールのために Bindine 式の CustomControlName 要素](./customcontrolname-element-for-expressionbinding-for-controls-for-view-format.md) コモンコントロールまたはビューコントロールの名前を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[GroupBy (Format) の CustomControlName 要素](./customcontrolname-element-for-groupby-format.md) 新しいグループを表示するために使用されるカスタムコントロールの名前を指定します。 この要素は、テーブル、リスト、ワイド、またはカスタムコントロールビューを定義するときに使用されます。

[CustomControl の Customentry 要素 (構成用コントロール用) (形式)](./customentry-element-for-customcontrol-for-controls-for-configuration-format.md) コモンコントロールの定義を提供します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の Customentry 要素](./customentry-element-for-customentries-for-controls-for-view-format.md) コントロールの定義を提供します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[View (Format) の CustomEntries の Customentry 要素](./customentry-element-for-customentries-for-customcontrol-for-view-format.md) カスタムコントロールビューの定義を提供します。

[GroupBy (Format) の CustomControl の Customentry 要素](./customentry-element-for-customcontrol-for-groupby-format.md) コントロールの定義を提供します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[Configuration の CustomControl の Customentries 要素 (形式)](./customentries-element-for-customcontrol-for-controls-for-configuration-format.md) コモンコントロールの定義を提供します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[View (Format) 用のコントロールの CustomControl の Customentries 要素](./customentries-element-for-customcontrol-for-controls-for-view-format.md) コントロールの定義を提供します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[GroupBy (Format) の CustomControl の Customentries 要素](./customentries-element-for-customcontrol-for-groupby-format.md) コントロールの定義を提供します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[CustomControl For View (Format) の Customentries 要素](./customentries-element-for-customcontrol-for-view-format.md) カスタムコントロールビューの定義を提供します。 カスタムコントロールビューでは、1つまたは複数の定義を指定する必要があります。

[構成のための CustomEntry コントロールの Customitem 要素](./customitem-element-for-customentry-for-controls-for-configuration-format.md) コントロールによって表示されるデータとその表示方法を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビュー用のコントロール用の Custommentry の Customitem 要素 (形式)](./customitem-element-for-customentry-for-controls-for-view-format.md) コントロールによって表示されるデータとその表示方法を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomEntry For ビューの Customitem 要素](./customitem-element-for-customentry-for-customcontrol-for-view-format.md) カスタムコントロールビューとその表示方法によって表示されるデータを定義します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy の CustomEntry の Customitem 要素 (形式)](./customitem-element-for-customentry-for-groupby-format.md) カスタムコントロールビューとその表示方法によって表示されるデータを定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[DefaultSettings 要素 (Format)](./defaultsettings-element-format.md) 書式設定ファイルのすべてのビューに適用される共通設定を定義します。 一般的な設定には、エラーの表示、テーブル内のテキストの折り返し、コレクションの展開方法の定義などがあります。

[Displayerror 要素 (Format)](./displayerror-element-format.md) データの表示中にエラーが発生した場合に、文字列 #ERR が表示されることを指定します。

[Configuration 用のコントロール用の CustomEntry 要素 (形式)](./entryselectedby-element-for-customentry-for-controls-for-configuration-format.md) コモンコントロールの定義、またはこのコントロールを使用するために存在する必要がある条件を使用する .NET 型を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールに対する CustomEntry 要素 (形式)](./entryselectedby-element-for-customentry-for-controls-for-view-format.md) このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomEntry For ビューの Entryselectedby 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md) このカスタムエントリを使用する .NET 型、またはこのエントリが使用されるために必要な条件を定義します。

[列挙 Able展開 (形式) の Entryselectedby 要素](./entryselectedby-element-for-enumerableexpansion-format.md) この定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。

[GroupBy の CustomEntry の Entryselectedby 要素 (形式)](./entryselectedby-element-for-customentry-for-groupby-format.md) このコントロール定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListEntry For ListControl (Format) の Entryselectedby 要素](./entryselectedby-element-for-listentry-for-listcontrol-format.md) このリストビュー定義を使用する .NET 型、またはこの定義を使用するために存在する必要がある条件を定義します。 ほとんどの場合、リストビューに必要な定義は1つだけです。 ただし、同じリストビューを使用して異なるオブジェクトの異なるデータを表示する場合は、リストビューに複数の定義を指定できます。

[TableRowEntry の Entryselectedby 要素 (形式)](./entryselectedby-element-for-tablerowentry-for-tablecontrol-format.md) プロパティ値が行内に表示される .NET 型を定義します。

[WideEntry の Entryselectedby 要素 (形式)](./entryselectedby-element-for-wideentry-format.md) ワイドビューのこの定義を使用する .NET 型、またはこの定義を使用するために必要な条件を定義します。

[列挙 Able展開要素 (形式)](./enumerableexpansion-element-format.md) 特定の .NET コレクションオブジェクトをビューに表示するときに展開する方法を定義します。

[列挙 Able展開要素 (形式)](./enumerableexpansions-element-format.md) .NET コレクションオブジェクトがビューに表示されるときの展開方法を定義します。

[構成対象のコントロールの式のバインドの列挙 Atecollection 要素 (形式)](./enumeratecollection-element-for-expressionbinding-for-controls-for-configuration-format.md) コレクションの要素がコントロールによって表示されることを指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の式のバインドの列挙 Atecollection 要素](./enumeratecollection-element-for-expressionbinding-for-controls-for-view-format.md) コレクションの要素が表示されることを指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For View (Format) の式バインドの列挙 Atecollection 要素](./enumeratecollection-element-for-expressionbinding-for-customcontrol-for-view-format.md) コレクションの要素を表示することを指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の式のバインドの列挙 Atecollection 要素](./enumeratecollection-element-for-expressionbinding-for-groupby-format.md) コレクションの要素を表示することを指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[要素の展開 (形式)](./expand-element-format.md) この定義に対してコレクションオブジェクトを展開する方法を指定します。

[構成用のコントロールに対する CustomItem の式のバインド要素 (形式)](./expressionbinding-element-for-customitem-for-controls-for-configuration-format.md) コントロールによって表示されるデータを定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールに対する CustomItem の式のバインド要素 (形式)](./expressionbinding-element-for-customitem-for-controls-for-view-format.md) コントロールによって表示されるデータを定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) に対する CustomItem の式のバインド要素](./expressionbinding-element-for-customitem-for-customcontrol-for-view-format.md) コントロールによって表示されるデータを定義します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の CustomItem の式のバインド要素](./expressionbinding-element-for-customitem-for-groupby-format.md) コントロールによって表示されるデータを定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[構成対象のコントロールのフレームの Firstlinehanging 要素 (形式)](./firstlinehanging-element-for-frame-for-controls-for-configuration-format.md) データの最初の行を左にシフトする文字数を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールのフレームの Firstlinehanging 要素 (形式)](./firstlinehanging-element-for-frame-for-controls-for-view-format.md) データの最初の行を左にシフトする文字数を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For View (Format) のフレームの Firstlinehanging 要素](./firstlinehanging-element-for-frame-for-customcontrol-for-view-format.md) データの最初の行を左にシフトする文字数を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の Frame の Firstlinehanging 要素](./firstlinehanging-element-for-frame-for-groupby-format.md) データの最初の行を左にシフトする文字数を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[構成対象のコントロールのフレームの Firstlineindent 要素 (形式)](./firstlineindent-element-for-frame-for-controls-for-configuration-format.md) データの最初の行を右にシフトする文字数を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールのフレームの Firstlineindent 要素 (Format)](./firstlineindent-element-for-frame-for-controls-for-view-format.md) データの最初の行を右にシフトする文字数を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[Firstlineindent 要素](./firstlineindent-element-for-frame-for-customcontrol-for-view-format.md) データの最初の行を右にシフトする文字数を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の Frame の Firstlineindent 要素](./firstlineindent-element-for-frame-for-groupby-format.md) データの最初の行を右にシフトする文字数を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListItem の FormatString 要素 (形式)](./formatstring-element-for-listitem-for-listcontrol-format.md) プロパティまたはスクリプトの値の表示方法を定義する形式パターンを指定します。

[TableColumnItem の FormatString 要素 (Format)](./formatstring-element-for-tablecolumnitem-for-tablecontrol-format.md) テーブルのプロパティまたはスクリプトの値の表示方法を定義する形式パターンを指定します。

[WideControl の WideItem の FormatString 要素 (形式)](./formatstring-element-for-wideitem-for-widecontrol-format.md) プロパティまたはスクリプトの値をビューに表示する方法を定義する形式パターンを指定します。

[構成用のコントロールの CustomItem の Frame 要素 (形式)](./frame-element-for-customitem-for-controls-for-configuration-format.md) データを左右に移動するなど、データの表示方法を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールの CustomItem の Frame 要素 (Format)](./frame-element-for-customitem-for-controls-for-view-format.md) データを左右に移動するなど、データの表示方法を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の CustomItem の Frame 要素](./frame-element-for-customitem-for-customcontrol-for-view-format.md) データを左右に移動するなど、データの表示方法を定義します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の CustomItem の Frame 要素](./frame-element-for-customitem-for-groupby-format.md) データを左右に移動するなど、データの表示方法を定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ビューの GroupBy 要素 (Format)](./groupby-element-for-view-format.md) Windows PowerShell がオブジェクトの新しいグループを表示する方法を定義します。

[Hidetableheaders 要素 (形式)](./hidetableheaders-element-format.md) テーブルのヘッダーが表示されないことを指定します。

[構成用のコントロールの式のバインドの Itemselectioncondition 要素 (形式)](./itemselectioncondition-element-for-expressionbinding-for-controls-for-configuration-format.md) このコントロールを使用するために必要な条件を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の式のバインドの Itemselectioncondition 要素](./itemselectioncondition-element-for-expressionbinding-for-controls-for-view-format.md) このコントロールを使用するために必要な条件を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For View (Format) の式のバインドの Itemselectioncondition 要素](./itemselectioncondition-element-for-expressionbinding-for-customcontrol-format.md) このコントロールを使用するために必要な条件を定義します。 コントロール項目に指定できる選択条件の数に制限はありません。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の式のバインドの Itemselectioncondition 要素](./itemselectioncondition-element-for-expressionbinding-for-groupby-format.md) このコントロールを使用するために必要な条件を定義します。 コントロール項目に指定できる選択条件の数に制限はありません。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListItem の Itemselectioncondition 要素 (形式)](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md) このリスト項目を使用するために必要な条件を定義します。

[ListControl の ListItem の Label 要素 (形式)](./label-element-for-listitem-for-listcontrol-format.md) 行内のプロパティまたはスクリプト値のラベルを指定します。

[GroupBy (Format) の Label 要素](./label-element-for-groupby-format.md) 新しいグループが検出されたときに表示されるラベルを指定します。

[TableColumnHeader の Label 要素 (形式)](./label-element-for-tablecolumnheader-for-tablecontrol-format.md) 列の上部に表示されるラベルを定義します。

[構成対象のコントロールのフレームの左インデント要素 (形式)](./leftindent-element-for-frame-for-controls-for-configuration-format.md) データを左余白から移動する文字数を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールのフレームの左インデント要素 (書式)](./leftindent-element-for-frame-for-controls-for-view-format.md) データを左余白から移動する文字数を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (形式) のフレームの左インデント要素](./leftindent-element-for-frame-for-customcontrol-for-view-format.md) データを左余白から移動する文字数を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の Frame の左インデント要素](./leftindent-element-for-frame-for-groupby-format.md) データを左余白から移動する文字数を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListControl 要素 (Format)](./listcontrol-element-format.md) ビューのリスト形式を定義します。

[ListEntry 要素 (Format)](./listentry-element-for-listcontrol-format.md) リストビューの定義を提供します。

[Listentries 要素 (書式)](./listentries-element-for-listcontrol-format.md) リストビューの行をどのように表示するかを定義します。

[ListItem 要素 (形式)](./listitem-element-for-listitems-for-listcontrol-format.md) リストビューの行に表示される値を持つプロパティまたはスクリプトを定義します。

[ListItems 要素 (Format)](./listitems-element-for-listentry-for-listcontrol-format.md) リストビューに表示されるプロパティとスクリプトを定義します。

[構成用コントロールのコントロールの Name 要素 (Format)](./name-element-for-control-for-controls-for-configuration-format.md) コントロールの名前を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[SelectionSet の Name 要素 (Format)](./name-element-for-selectionset-format.md) 選択セットを参照するために使用する名前を指定します。

[ビューの Name 要素 (Format)](./name-element-for-view-format.md) ビューを識別するために使用される名前を指定します。

[構成用のコントロールの CustomItem の改行要素 (形式)](./newline-element-for-customitem-for-controls-for-configuration-format.md) コントロールの表示に空白行を追加します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールの CustomItem の改行要素 (Format)](./newline-element-for-customitem-for-controls-for-view-format.md) コントロールの表示に空白行を追加します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の CustomItem の改行要素](./newline-element-for-customitem-for-customcontrol-for-view-format.md) コントロールの表示に空白行を追加します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の CustomItem の改行要素](./newline-element-for-customitem-for-groupby-format.md) コントロールの表示に空白行を追加します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[構成用のコントロールの式のバインドの PropertyName 要素 (Format)](./propertyname-element-for-expressionbinding-for-controls-for-configuration-format.md) コモンコントロールによって表示される値を持つ .NET プロパティを指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の式のバインドの PropertyName 要素](./propertyname-element-for-expressionbinding-for-controls-for-view-format.md) コントロールによって表示される値を持つ .NET プロパティを指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For View (Format) の式のバインドの PropertyName 要素](./propertyname-element-for-expressionbinding-for-customcontrol-for-view-format.md) コントロールによって表示される値を持つ .NET プロパティを指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の式のバインドの PropertyName 要素](./propertyname-element-for-expressionbinding-for-groupby-format.md) コントロールによって表示される値を持つ .NET プロパティを指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[GroupBy (Format) の PropertyName 要素](./propertyname-element-for-groupby-format.md) 値が変更されるたびに新しいグループを開始する .NET プロパティを指定します。

[ItemSeclectionCondition の PropertyName 要素 (構成用コントロール用) (形式)](./propertyname-element-for-itemseclectioncondition-for-controls-for-configuration-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、コントロールが使用されます。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の ItemSelectionCondition の PropertyName 要素](./propertyname-element-for-itemselectioncondition-for-controls-for-view-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、コントロールが使用されます。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl の ItemSelectionCondition の PropertyName 要素](./propertyname-element-for-itemselectioncondition-for-customcontrol-for-view-format.md) は、条件をトリガーする .net プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、コントロールが使用されます。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の ItemSelectionCondition の PropertyName 要素](./propertyname-element-for-itemselectioncondition-for-groupby-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、コントロールが使用されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListItem の ItemSelectionCondition の PropertyName 要素 (形式)](./propertyname-element-for-itemselectioncondition-for-listcontrol-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、ビューが使用されます。 この要素は、リストビューを定義するときに使用されます。

[ListControl の ListItem の PropertyName 要素 (形式)](./propertyname-element-for-listitem-for-listcontrol-format.md) 値が一覧に表示される .NET プロパティを指定します。

[ListEntry (Format) の EntrySelectedBy の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-controls-for-configuration-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、エントリが使用されます。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールの SelectionCondition の PropertyName 要素 (Format)](./propertyname-element-for-selectioncondition-for-controls-for-view-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、エントリが使用されます。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-customcontrol-for-view-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、定義が使用されます。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[列挙 Able膨張 (形式) の EntrySelectedBy の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、定義が使用されます。

[GroupBy (Format) の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-groupby-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、定義が使用されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListEntry (Format) の EntrySelectedBy の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-entryselectedby-for-listcontrol-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、リストエントリが使用されます。

[TableRowEntry (Format) の EntrySelectedBy の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-entryselectedby-for-tablerowentry-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、テーブルエントリが使用されます。

[WideEntry (Format) の EntrySelectedBy の SelectionCondition の PropertyName 要素](./propertyname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md) 条件をトリガーする .NET プロパティを指定します。 このプロパティが存在する場合、またはと評価された場合 `true` 、条件が満たされ、定義が使用されます。

[TableColumnItem の PropertyName 要素 (形式)](./propertyname-element-for-tablecolumnitem-for-tablecontrol-format.md) 行の列に値を表示するプロパティを指定します。

[WideItem の PropertyName 要素 (形式)](./propertyname-element-for-wideitem-for-widecontrol-format.md) ワイドビューに表示される値を持つオブジェクトのプロパティを指定します。

[構成対象のコントロールのフレームの右インデント要素 (形式)](./rightindent-element-for-frame-for-controls-for-configuration-format.md) データを右余白から移動する文字数を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールのフレームの右インデント要素 (書式)](./rightindent-element-for-frame-for-controls-for-view-format.md) データを右余白から移動する文字数を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[右インデント要素](./rightindent-element-for-frame-for-customcontrol-for-view-format.md) データを右余白から移動する文字数を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の Frame の右インデント要素](./rightindent-element-for-frame-for-groupby-format.md) データを右余白から移動する文字数を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[構成用のコントロールの式のバインドの ScriptBlock 要素 (形式)](./scriptblock-element-for-expressionbinding-for-controls-for-configuration-format.md) 共通コントロールによって値が表示されるスクリプトを指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の式のバインドの ScriptBlock 要素](./scriptblock-element-for-expressionbinding-for-controls-for-view-format.md) コントロールによって値が表示されるスクリプトを指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomCustomControl For View (Format) の式のバインドの ScriptBlock 要素](./scriptblock-element-for-expressionbinding-for-customcontrol-for-view-format.md) コントロールによって値が表示されるスクリプトを指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の式のバインドの ScriptBlock 要素](./scriptblock-element-for-expressionbinding-for-groupby-format.md) コントロールによって値が表示されるスクリプトを指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[GroupBy (Format) の ScriptBlock 要素](./scriptblock-element-for-groupby-format.md) 値が変更されるたびに新しいグループを開始するスクリプトを指定します。

[構成用のコントロールの ItemSelectionCondition の ScriptBlock 要素 (形式)](./scriptblock-element-for-itemseclectioncondition-for-controls-for-configuration-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、コントロールが使用されます。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の ItemSelectionCondition の ScriptBlock 要素](./scriptblock-element-for-itemselectioncondition-for-controls-for-view-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、コントロールが使用されます。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の ItemSelectionCondition の ScriptBlock 要素](./scriptblock-element-for-itemselectioncondition-for-customcontrol-for-view-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、コントロールが使用されます。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の ItemSelectionCondition の ScriptBlock 要素](./scriptblock-element-for-itemselectioncondition-for-groupby-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、コントロールが使用されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListControl の ItemSelectionCondition の ScriptBlock 要素 (形式)](./scriptblock-element-for-itemselectioncondition-for-listcontrol-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、リスト項目が使用されます。 この要素は、リストビューを定義するときに使用されます。

[ListItem の ScriptBlock 要素 (形式)](./scriptblock-element-for-listitem-for-listcontrol-format.md) 一覧の行に値が表示されるスクリプトを指定します。

[構成用のコントロールの SelectionCondition の ScriptBlock 要素 (形式)](./scriptblock-element-for-selectioncondition-for-controls-for-configuration-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、定義が使用されます。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールの SelectionCondition の ScriptBlock 要素 (Format)](./scriptblock-element-for-selectioncondition-for-controls-for-view-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、定義が使用されます。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-customcontrol-for-view-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、定義が使用されます。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[列挙 Able展開の EntrySelectedBy の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md) 条件をトリガーするスクリプトを指定します。

[GroupBy (Format) の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-groupby-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、定義が使用されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListEntry (Format) の EntrySelectedBy の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-listcontrol-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、リストエントリが使用されます。

[TableRowEntry (Format) の EntrySelectedBy の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-tablecontrol-format.md) 条件をトリガーするスクリプトブロックを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、テーブルエントリが使用されます。

[WideEntry (Format) の EntrySelectedBy の SelectionCondition の ScriptBlock 要素](./scriptblock-element-for-selectioncondition-for-entryselectedby-for-widecontrol-format.md) 条件をトリガーするスクリプトを指定します。 このスクリプトがに評価されると、 `true` 条件が満たされ、ワイドエントリの定義が使用されます。

[TableColumnItem の ScriptBlock 要素 (形式)](./scriptblock-element-for-tablecolumnitem-for-tablecontrol-format.md) 行の列に値が表示されるスクリプトを指定します。

[WideItem の ScriptBlock 要素 (形式)](./scriptblock-element-for-wideitem-for-widecontrol-format.md) ワイドビューに値が表示されるスクリプトを指定します。

[CustomEntry For 構成 (形式) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-controls-for-configuration-format.md) 共通のコントロール定義を使用するために必要な条件を定義します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) に対する EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-controls-for-view-format.md) コントロール定義を使用するために必要な条件を定義します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-customcontrol-format.md) コントロール定義を使用するために存在する必要がある条件を定義します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[列挙 Able膨張 (形式) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-enumerableexpansion-format.md) この定義のコレクションオブジェクトを展開するために必要な条件を定義します。

[GroupBy (Format) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-groupby-format.md) コントロール定義を使用するために存在する必要がある条件を定義します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListEntry (Format) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-listcontrol-format.md) リストビューのこの定義を使用するために必要な条件を定義します。 リスト定義に指定できる選択条件の数に制限はありません。

[TableRowEntry (Format) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-tablecontrol-format.md) テーブルビューのこの定義に使用する必要がある条件を定義します。 テーブル定義に指定できる選択条件の数に制限はありません。

[WideEntry (Format) の EntrySelectedBy の Selectioncondition 要素](./selectioncondition-element-for-entryselectedby-for-widecontrol-format.md) この定義を使用するために必要な条件を定義します。 ワイドエントリの定義に指定できる選択条件の数に制限はありません。

[Selectionset 要素 (形式)](./selectionset-element-format.md) セットの名前で参照できる .NET オブジェクトのセットを定義します。

[構成用のコントロール (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md) コントロールのこの定義を使用する .NET 型のセットを指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-controls-for-view-format.md) コントロールのこの定義を使用する .NET 型のセットを指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomEntry (形式) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-customcontrol-for-view-format.md) リストエントリの一連の .NET オブジェクトを指定します。 エントリに指定できる選択セットの数に制限はありません。

[列挙 Able膨張 (形式) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-enumerableexpansion-format.md) この定義によって展開される .NET 型のセットを指定します。

[GroupBy (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-groupby-format.md) リストエントリの一連の .NET オブジェクトを指定します。 エントリに指定できる選択セットの数に制限はありません。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListEntry (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md) リストエントリの一連の .NET オブジェクトを指定します。 エントリに指定できる選択セットの数に制限はありません。

[TableRowEntry (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-tablecontrol-format.md) テーブルビューのこのエントリを使用する .NET 型のセットを指定します。 エントリに指定できる選択セットの数に制限はありません。

[WideEntry (Format) の EntrySelectedBy の SelectionSetName 要素](./selectionsetname-element-for-entryselectedby-for-widecontrol-format.md) 定義の .NET オブジェクトのセットを指定します。 これらのオブジェクトのいずれかが表示されるたびに、定義が使用されます。

[構成用のコントロールの SelectionCondition の SelectionSetName 要素 (形式)](./selectionsetname-element-for-selectioncondition-for-controls-for-configuration-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、このコントロールを使用してオブジェクトが表示されます。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の SelectionCondition の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-controls-for-view-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、このコントロールを使用してオブジェクトが表示されます。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomEntry For ビューの Entryselectedby 要素](./entryselectedby-element-for-customentry-for-customcontrol-for-view-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、このコントロールを使用してオブジェクトが表示されます。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[列挙 Able膨張 (Format) の EntrySelectedBy の SelectionCondition の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされます。

[GroupBy (Format) の SelectionCondition の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-groupby-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、このコントロールを使用してオブジェクトが表示されます。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[EntrySelectedBy For ListEntry (Format) の SelectionCondition の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-listentry-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、リストビューのこの定義を使用してオブジェクトが表示されます。

[EntrySelectedBy For TableRowEntry (Format) の SelectionCondition の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-tablecontrol-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、テーブルビューのこの定義を使用してオブジェクトが表示されます。

[EntrySelectedBy For WideEntry (Format) の SelectionCondition の SelectionSetName 要素](./selectionsetname-element-for-selectioncondition-for-entryselectedby-for-wideentry-format.md) 条件をトリガーする .NET 型のセットを指定します。 このセット内のいずれかの型が存在する場合、条件が満たされ、このワイドビューの定義を使用してオブジェクトが表示されます。

[ViewSelectedBy (Format) の SelectionSetName 要素](./selectionsetname-element-for-viewselectedby-format.md) ビューに表示される一連の .NET オブジェクトを指定します。

[Selectionsets 要素 (Format)](./selectionsets-element-format.md) 個々の形式ビューで使用できる .NET オブジェクトのセットを定義します。

[Showerror 要素 (形式)](./showerror-element-format.md) データの一部を表示しているときにエラーが発生した場合に、完全なエラーレコードが表示されることを指定します。

[TableControl の TableHeaders の TableColumnHeader 要素 (Format)](./tablecolumnheader-element-format.md) ラベル、列の幅、およびテーブルの列のラベルの配置を定義します。

[TableColumnItem 要素 (Format)](./tablecolumnitem-element-for-tablecolumnitems-for-tablecontrol-format.md) 行の列に値が表示されるプロパティまたはスクリプトを定義します。

[TableColumnItems 要素 (Format)](./tablecolumnitems-element-for-tablerowentry-for-tablecontrol-format.md) 値が行内に表示されるプロパティまたはスクリプトを定義します。

[Tablecontrol 要素 (形式)](./tablecontrol-element-format.md) ビューのテーブル形式を定義します。

[Tableheaders 要素 (書式)](./tableheaders-element-format.md) テーブルの列のヘッダーを定義します。

[TableRowEntries 要素 (Format)](./tablerowentries-element-for-tablecontrol-format.md) テーブルの行を定義します。

[TableRowEntry 要素 (Format)](./tablerowentry-element-for-tablerowentries-for-tablecontrol-format.md) テーブルの行に表示されるデータを定義します。

[構成用のコントロールの CustomItem のテキスト要素 (形式)](./text-element-for-customitem-for-controls-for-configuration-format.md) ラベル、データを囲む角かっこ、データをインデントするスペースなど、コントロールによって表示されるデータに追加されるテキストを指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールの CustomItem のテキスト要素 (書式)](./text-element-for-customitem-for-controls-for-view-format.md) ラベル、データを囲む角かっこ、データをインデントするスペースなど、コントロールによって表示されるデータに追加されるテキストを指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomItem の Text 要素 (Format)](./text-element-for-customitem-for-customview-for-view-format.md) ラベル、データを囲む角かっこ、データをインデントするスペースなど、コントロールによって表示されるデータに追加されるテキストを指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[GroupBy (Format) の CustomItem のテキスト要素](./text-element-for-customitem-for-groupby-format.md) ラベル、データを囲む角かっこ、データをインデントするスペースなど、コントロールによって表示されるデータに追加されるテキストを指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[構成用のコントロール (Format) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-controls-for-configuration-format.md) コントロールのこの定義を使用する .NET 型を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロール (Format) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-controls-for-view-format.md) コントロールのこの定義を使用する .NET 型を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomEntry For ビュー (形式) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-customentry-for-view-format.md) カスタムコントロールビューのこの定義を使用する .NET 型を指定します。 定義に指定できる型の数に制限はありません。

[列挙型展開の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-enumerableexpansion-format.md) この定義によって展開される .NET 型を指定します。 この要素は、既定の設定を定義するときに使用されます。

[GroupBy (Format) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-groupby-format.md) カスタムコントロールのこの定義を使用する .NET 型を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListControl (Format) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-listcontrol-format.md) リストビューのこのエントリを使用する .NET 型を指定します。 リストエントリに指定できる型の数に制限はありません。

[TableRowEntry (Format) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-tablecontrol-format.md) テーブルビューのこのエントリを使用する .NET 型を指定します。 テーブルエントリに指定できる型の数に制限はありません。

[WideEntry (Format) の EntrySelectedBy の TypeName 要素](./typename-element-for-entryselectedby-for-wideentry-format.md) 定義の .NET 型を指定します。 このオブジェクトが表示されるたびに、定義が使用されます。

[構成用のコントロールの SelectionCondition の TypeName 要素 (Format)](./typename-element-for-selectioncondition-for-controls-for-configuration-format.md) 条件をトリガーする .NET 型を指定します。 この要素は、書式設定ファイル内のすべてのビューで使用できるコモンコントロールを定義するときに使用されます。

[ビューのコントロールの SelectionCondition の TypeName 要素 (Format)](./typename-element-for-selectioncondition-for-controls-for-view-format.md) 条件をトリガーする .NET 型を指定します。 この要素は、ビューで使用できるコントロールを定義するときに使用されます。

[CustomControl For ビュー (Format) の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-customcontrol-for-view-format.md) 条件をトリガーする .NET 型を指定します。 この要素は、カスタムコントロールビューを定義するときに使用されます。

[列挙型拡張の EntrySelectedBy の SelectionCondition の TypeName 要素 (Format)](./typename-element-for-selectioncondition-for-entryselectedby-for-enumerableexpansion-format.md) 条件をトリガーする .NET 型を指定します。

[GroupBy (Format) の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-groupby-format.md) 条件をトリガーする .NET 型を指定します。 この要素は、新しいオブジェクトのグループをどのように表示するかを定義するときに使用されます。

[ListControl (Format) の EntrySelectedBy の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-entryselectedby-for-listcontrol-format.md) 条件をトリガーする .NET 型を指定します。 この型が存在する場合は、リストエントリが使用されます。

[TableRowEntry (Format) の EntrySelectedBy の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-entryselectedby-for-tablecontrol-format.md) 条件をトリガーする .NET 型を指定します。 この型が存在する場合、条件が満たされ、テーブルの行が使用されます。

[WideEntry (Format) の EntrySelectedBy の SelectionCondition の TypeName 要素](./typename-element-for-selectioncondition-for-entryselectedby-for-widecontrol-format.md) 条件をトリガーする .NET 型を指定します。 この型が存在する場合は、定義が使用されます。

[型の TypeName 要素 (Format)](./typename-element-for-types-format.md) 選択セットに属するオブジェクトの .NET 型を指定します。

[ViewSelectedBy (Format) の TypeName 要素](./typename-element-for-viewselectedby-format.md) ビューに表示される .NET オブジェクトを指定します。

[Types 要素 (Format)](./types-element-for-selectionset-format.md) 選択セット内の .NET オブジェクトを定義します。

[View 要素 (Format)](./view-element-format.md) 1つ以上の .NET オブジェクトを表示するために使用されるビューを定義します。

[Viewdefinitions 要素 (形式)](./viewdefinitions-element-format.md) オブジェクトを表示するために使用するビューを定義します。

[Viewselectedby 要素 (形式)](./viewselectedby-element-format.md) ビューに表示される .NET オブジェクトを定義します。

[WideControl 要素 (Format)](./widecontrol-element-format.md) ビューのワイド (単一値) リスト形式を定義します。 このビューには、各オブジェクトの1つのプロパティ値またはスクリプト値が表示されます。

[WideEntries 要素 (Format)](./wideentries-element-for-widecontrol-format.md) ワイドビューの定義を提供します。 ワイドビューでは、1つまたは複数の定義を指定する必要があります。

[WideEntry 要素 (Format)](./wideentry-element-for-widecontrol-format.md) ワイドビューの定義を提供します。

[WideItem 要素 (Format)](./wideitem-element-for-widecontrol-format.md) 値が表示されるプロパティまたはスクリプトを定義します。

[Width 要素 (書式)](./width-element-for-tablecolumnheader-for-tablecontrol-format.md) 列の幅 (文字数) を定義します。

[Wrap 要素 (書式)](./wrap-element-for-tablerowentry-for-tablecontrol-format.md) 列幅を超えるテキストを次の行に表示するように指定します。

[WrapTables 要素 (Format)](./wraptables-element-format.md) データが列の幅よりも長い場合に、テーブルセル内のデータを次の行に移動するように指定します。

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
