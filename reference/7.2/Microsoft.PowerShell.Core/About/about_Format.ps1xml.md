---
description: PowerShell 6 以降では、オブジェクトの既定のビューが PowerShell ソースコードで定義されています。  独自 `Format.ps1xml` のファイルを作成して、オブジェクトの表示を変更したり、PowerShell で作成した新しいオブジェクトの種類の既定の表示を定義したりすることができます。
Locale: en-US
ms.date: 11/27/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_format.ps1xml?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Format.ps1xml
ms.openlocfilehash: 56bac204e33c39aa6192da9e6a899f00b0a4a743
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602401"
---
# <a name="about-formatps1xml"></a>Format.ps1xml について

## <a name="short-description"></a>簡単な説明

PowerShell 6 以降では、オブジェクトの既定のビューが PowerShell ソースコードで定義されています。

独自 `Format.ps1xml` のファイルを作成して、オブジェクトの表示を変更したり、PowerShell で作成した新しいオブジェクトの種類の既定の表示を定義したりすることができます。

## <a name="long-description"></a>長い説明

PowerShell 6 以降では、既定のビューが PowerShell ソースコードで定義されています。 Powershell `Format.ps1xml` 5.1 以前のバージョンのファイルは、powershell 6 以降のバージョンには存在しません。

Powershell ソースコードは、PowerShell コンソールでのオブジェクトの既定の表示を定義します。 独自 `Format.ps1xml` のファイルを作成して、オブジェクトの表示を変更したり、PowerShell で作成した新しいオブジェクトの種類の既定の表示を定義したりすることができます。

PowerShell がオブジェクトを表示すると、構造化された書式設定ファイルのデータを使用して、オブジェクトの既定の表示を決定します。 書式設定ファイルのデータは、オブジェクトがテーブルとリストのどちらで表示されるかを決定し、既定で表示されるプロパティを決定します。

書式設定は、表示のみに影響します。 パイプラインで渡されるオブジェクトのプロパティや、パイプラインが渡される方法には影響しません。 `Format.ps1xml` ファイルを使用して、ハッシュテーブルの出力形式をカスタマイズすることはできません。

フォーマットファイルでは、 `.ps1xml` 各オブジェクトの4つの異なるビューを定義できます。

- テーブル
- List
- Wide
- カスタム

たとえば、コマンドの出力を `Get-ChildItem` コマンドにパイプすると `Format-List` 、は `Format-List` ソースコードで定義されているリストビューを使用して、ファイルおよびフォルダーオブジェクトを一覧として表示する方法を決定します。

フォーマットファイルにオブジェクトの複数のビューが含まれている場合、PowerShell では、最初に見つかったビューが適用されます。

カスタムファイルで `Format.ps1xml` は、ビューの名前、適用先のオブジェクトの種類、列ヘッダー、およびビューの本文に表示されるプロパティを記述する XML タグのセットによって、ビューが定義されます。 ファイルの形式は、 `Format.ps1xml` データがユーザーに表示される直前に適用されます。

## <a name="creating-new-formatps1xml-files"></a>新しい Format.ps1xml ファイルの作成

既存のオブジェクトビューの表示形式を変更する、または新しいオブジェクトのビューを追加するには、独自の `Format.ps1xml` ファイルを作成して、PowerShell セッションに追加します。

カスタムビューを定義するファイルを作成するには、 `Format.ps1xml` [Get formatdata](xref:Microsoft.PowerShell.Utility.Get-FormatData) コマンドレットと [Export-formatdata](xref:Microsoft.PowerShell.Utility.Export-FormatData) コマンドレットを使用します。 ファイルを編集するには、テキストエディターを使用します。 ファイルは、PowerShell がアクセスできる任意のディレクトリ (のサブディレクトリなど) に保存でき `$HOME` ます。

現在のビューの書式を変更するには、書式設定ファイルでビューを探し、タグを使用してビューを変更します。 新しいオブジェクトの種類のビューを作成するには、新しいビューを作成するか、既存のビューをモデルとして使用します。 これらのタグについては、次のセクションで説明します。 その後、ファイル内の他のすべてのビューを削除して、ファイルを調べるすべてのユーザーに対して変更が明らかになるようにすることができます。

変更を保存したら、 [更新プログラムの FormatData](xref:Microsoft.PowerShell.Utility.Update-FormatData) を使用して、PowerShell セッションに新しいファイルを追加します。 組み込みファイルで定義されているビューよりもビューを優先する場合は、 **PrependPath** パラメーターを使用します。 `Update-FormatData` 現在のセッションのみに影響します。 今後のすべてのセッションに変更を加えるには、 `Update-FormatData` PowerShell プロファイルにコマンドを追加します。

### <a name="example-add-calendar-data-to-culture-objects"></a>例: カルチャオブジェクトへのカレンダーデータの追加

この例では、現在の PowerShell セッションで、コマンドレットによって生成 **さ** れるカルチャオブジェクトの書式設定を変更する方法を示し `Get-Culture` ます。 この例のコマンドでは、カルチャオブジェクトの既定のテーブルビュー表示に **Calendar** プロパティを追加します。

まず、ソースコードファイルから書式データを取得し、 `Format.ps1xml` カルチャオブジェクトの現在のビューを含むファイルを作成します。

```powershell
Get-FormatData -TypeName System.Globalization.CultureInfo |
  Export-FormatData -Path $HOME\Format\CultureInfo.Format.ps1xml
```

`CultureInfo.Format.ps1xml`Visual Studio Code など、任意の XML エディターまたはテキストエディターでファイルを開きます。 次の XML は、 **CultureInfo** オブジェクトのビューを定義します。

`CultureInfo.Format.ps1xml`ファイルは次の例のようになります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Configuration>
  <ViewDefinitions>
    <View>
      <Name>System.Globalization.CultureInfo</Name>
      <ViewSelectedBy>
        <TypeName>System.Globalization.CultureInfo</TypeName>
      </ViewSelectedBy>
      <TableControl>
        <TableHeaders>
          <TableColumnHeader>
            <Width>16</Width>
          </TableColumnHeader>
          <TableColumnHeader>
            <Width>16</Width>
          </TableColumnHeader>
          <TableColumnHeader />
        </TableHeaders>
        <TableRowEntries>
          <TableRowEntry>
            <TableColumnItems>
              <TableColumnItem>
                <PropertyName>LCID</PropertyName>
              </TableColumnItem>
              <TableColumnItem>
                <PropertyName>Name</PropertyName>
              </TableColumnItem>
              <TableColumnItem>
                <PropertyName>DisplayName</PropertyName>
              </TableColumnItem>
            </TableColumnItems>
          </TableRowEntry>
        </TableRowEntries>
      </TableControl>
    </View>
  </ViewDefinitions>
</Configuration>
```

新しいタグのセットを追加して、 **Calendar** プロパティの新しい列を作成し `<TableColumnHeader>` ます。 **Calendar** プロパティの値は long であるため、45文字の値をとして指定し `<Width>` ます。

```xml
<TableHeaders>
  <TableColumnHeader>
    <Width>16</Width>
  </TableColumnHeader>
  <TableColumnHeader>
    <Width>16</Width>
  </TableColumnHeader>
  <TableColumnHeader>
    <Width>45</Width>
  </TableColumnHeader>
  <TableColumnHeader/>
</TableHeaders>
```

タグとタグを使用して、テーブルの行に **Calendar** の新しい列項目を追加し `<TableColumnItem>` `<PropertyName` ます。

```xml
<TableRowEntries>
  <TableRowEntry>
    <TableColumnItems>
      <TableColumnItem>
        <PropertyName>LCID</PropertyName>
      </TableColumnItem>
      <TableColumnItem>
        <PropertyName>Name</PropertyName>
      </TableColumnItem>
      <TableColumnItem>
        <PropertyName>Calendar</PropertyName>
      </TableColumnItem>
      <TableColumnItem>
        <PropertyName>DisplayName</PropertyName>
      </TableColumnItem>
    </TableColumnItems>
  </TableRowEntry>
</TableRowEntries>
```

ファイルを保存して閉じます。 `Update-FormatData`新しいフォーマットファイルを現在の PowerShell セッションに追加するには、を使用します。

この例では、 **PrependPath** パラメーターを使用して、新しいファイルを元のファイルよりも優先順位の高い順に配置します。 詳細については、「 [更新-FormatData](xref:Microsoft.PowerShell.Utility.Update-FormatData)」を参照してください。

```powershell
Update-FormatData -PrependPath $HOME\Format\CultureInfo.Format.ps1xml
```

変更をテストするには、 `Get-Culture` **Calendar** プロパティを含む出力を入力して確認します。

```powershell
Get-Culture
```

```Output
LCID  Name   Calendar                                DisplayName
----  ----   --------                                -----------
1033  en-US  System.Globalization.GregorianCalendar  English (United States)
```

## <a name="the-xml-in-formatps1xml-files"></a>Format.ps1xml ファイル内の XML

完全なスキーマ定義は、GitHub の PowerShell ソースコードリポジトリの [.xsd](https://github.com/PowerShell/PowerShell/blob/master/src/Schemas/Format.xsd) にあります。

各ファイルの **Viewdefinitions** セクションには `Format.ps1xml` 、 `<View>` 各ビューを定義するタグが含まれています。 一般的な `<View>` タグには、次のタグが含まれます。

- `<Name>` ビューの名前を識別します。
- `<ViewSelectedBy>` ビューが適用されるオブジェクトの種類を指定します。
- `<GroupBy>` ビュー内の項目をグループにまとめる方法を指定します。
- `<TableControl>`、、、およびには、 `<ListControl>` `<WideControl>` `<CustomControl>` 各項目の表示方法を指定するタグが含まれています。

### <a name="viewselectedby-tag"></a>タグで viewselected

タグには `<ViewSelectedBy>` `<TypeName>` 、ビューが適用されるオブジェクトの種類ごとにタグを含めることができます。 または、タグを使用して、他の場所で定義されている `<SelectionSetName>` 選択セットを参照するタグを含めることもでき `<SelectionSet>` ます。

### <a name="groupby-tag"></a>GroupBy タグ

タグには、アイテムをグループ化するために使用する `<GroupBy>` オブジェクトプロパティを指定するタグが含まれてい `<PropertyName>` ます。 また、 `<Label>` 各グループのラベルとして使用する文字列を指定するタグ、またはタグを `<CustomControlName>` 使用して他の場所で定義されたカスタムコントロールを参照するタグのいずれかも含まれ `<Control>` ます。 タグにはタグ `<Control>` とタグが含まれてい `<Name>` `<CustomControl>` ます。

### <a name="tablecontroltag"></a>TableControlTag

`<TableControl>`タグには、通常、 `<TableHeaders>` `<TableRowEntries>` テーブルのヘッドと行の書式を定義するタグとタグが含まれています。 `<TableHeaders>`タグには `<TableColumnHeader>` 、通常 `<Label>` 、、、タグを含むタグが含まれてい `<Width>` `<Alignment>` ます。 `<TableRowEntries>`タグには `<TableRowEntry>` 、テーブルの各行のタグが含まれています。 タグには、 `<TableRowEntry>` `<TableColumnItems>` 行の `<TableColumnItem>` 各列のタグを含むタグが含まれています。 通常、タグには、定義された `<TableColumnItem>` `<PropertyName>` 場所に表示されるオブジェクトプロパティを識別するタグ、または `<ScriptBlock>` 場所に表示される結果を計算するスクリプトコードを含むタグのいずれかが含まれます。

> [!NOTE]
> スクリプトブロックは、計算された結果が役に立つ場所の他の場所でも使用できます。

タグには、 `<TableColumnItem>` `<FormatString>` プロパティまたは計算された結果の表示方法を指定するタグを含めることもできます。

### <a name="listcontrol-tag"></a>ListControl タグ

`<ListControl>`タグには、通常、タグが含まれてい `<ListEntries>` ます。 タグには `<ListEntries>` タグが含まれてい `<ListEntry>` ます。 タグには `<ListEntry>` タグが含まれてい `<ListItems>` ます。 タグにはタグが含ま `<ListItems>` れてい `<ListItem>` `<PropertyName>` ます。 タグは、 `<PropertyName>` 一覧の指定した位置に表示されるオブジェクトプロパティを指定します。 選択セットを使用してビューの選択が定義されている場合は、タグ `<ListControl>` と `<ListEntry>` タグに1つ以上のタグを含むタグを含めることもでき `<EntrySelectedBy>` `<TypeName>` ます。 これらの `<TypeName>` タグは、タグの表示対象となるオブジェクトの種類を指定し `<ListControl>` ます。

### <a name="widecontrol-tag"></a>WideControl タグ

`<WideControl>`タグには、通常、タグが含まれてい `<WideEntries>` ます。 `<WideEntries>`タグに1つ以上のタグが含まれてい `<WideEntry>` ます。 通常、タグには、 `<WideEntry>` `<PropertyName>` ビュー内の指定した位置に表示されるプロパティを指定するタグが含まれます。 タグには、 `<PropertyName>` `<FormatString>` プロパティを表示する方法を指定するタグを含めることができます。

### <a name="customcontrol-tag"></a>CustomControl タグ

`<CustomControl>`タグを使用すると、スクリプトブロックを使用して形式を定義できます。 `<CustomControl>`タグには、通常、複数のタグを含むタグが含まれてい `<CustomEntries>` `<CustomEntry>` ます。 各 `<CustomEntry>` タグには `<CustomItem>` 、、、、タグなど、ビュー内の指定された場所の内容と書式を指定するさまざまなタグを含めることができるタグが含まれてい `<Text>` `<Indentation>` `<ExpressionBinding>` `<NewLine>` ます。

## <a name="tracing-formatps1xml-file-use"></a>Xml ファイルの使用 Format.ps1のトレース

ファイルの読み込みまたはアプリケーションで発生したエラーを検出するには、 `Format.ps1xml` `Trace-Command` **Name** パラメーターの値として、次のいずれかの形式コンポーネントを指定したコマンドレットを使用します。

- FormatFileLoading
- FormatViewBinding

詳細については、「 [Trace-Command](xref:Microsoft.PowerShell.Utility.Trace-Command) と [Get tracesource](xref:Microsoft.PowerShell.Utility.Get-TraceSource)」を参照してください。

## <a name="signing-a-formatps1xml-file"></a>Format.ps1xml ファイルへの署名

ファイルのユーザーを保護するには `Format.ps1xml` 、デジタル署名を使用してファイルに署名します。 詳細については、「 [about_Signing](about_Signing.md)」を参照してください。

## <a name="sample-xml-for-a-format-table-custom-view"></a>Format-Table カスタムビューのサンプル XML

次の XML サンプルでは、 `Format-Table` によって作成される **DirectoryInfo** および system.object オブジェクトのカスタムビュー **を作成** し `Get-ChildItem` ます。 カスタムビューには **mygciview** という名前が付けられ、テーブルに "作成 **後の時間** " 列が追加されます。

カスタムビューを作成するには、 `Get-FormatData` コマンドレットとコマンドレットを使用して `Export-FormatData` ファイルを生成し `.ps1xml` ます。 次に、ファイルを編集して、 `.ps1xml` カスタムビューのコードを作成します。 ファイルは、 `.ps1xml` PowerShell がアクセスできる任意のディレクトリに格納できます。 たとえば、のサブディレクトリ `$HOME` です。

`.ps1xml`ファイルが作成されたら、コマンドレットを使用して、 `Update-FormatData` 現在の PowerShell セッションにビューを含めます。 または、すべての PowerShell セッションでビューを使用できるようにする必要がある場合は、PowerShell プロファイルに update コマンドを追加します。

この例では、カスタムビューでテーブル形式を使用する必要があります。そうしないと、は `Format-Table` 失敗します。

`Format-Table` **View** パラメーターと共にを使用してカスタムビューの名前を指定し、 **mygciview** を使用して、テーブルの出力の形式を作成 **時** の列に設定します。 コマンドの実行方法の例については、「 [Format-Table](xref:Microsoft.PowerShell.Utility.Format-Table)」を参照してください。

> [!NOTE]
> カスタムビューを作成するためにソースコードから書式設定 XML を取得できますが、必要な結果を得るためにより多くの開発が必要になる場合があります。

次のコマンドでは `Get-FormatData` 、 **powershellversion** パラメーターの代わりに、すべてのローカル書式情報が返されるようにしています。 `-PowerShellVersion $PSVersionTable.PSVersion`特定の PowerShell バージョンではなくを使用します。

```powershell
Get-FormatData -PowerShellVersion 5.1 -TypeName System.IO.DirectoryInfo |
   Export-FormatData -Path ./Mygciview.Format.ps1xml
Update-FormatData -AppendPath ./Mygciview.Format.ps1xml
```

```xml
<?xml version="1.0" encoding="utf-8"?>
<Configuration>
  <ViewDefinitions>
    <View>
      <Name>mygciview</Name>
      <ViewSelectedBy>
        <TypeName>System.IO.DirectoryInfo</TypeName>
        <TypeName>System.IO.FileInfo</TypeName>
      </ViewSelectedBy>
      <GroupBy>
        <PropertyName>PSParentPath</PropertyName>
      </GroupBy>
      <TableControl>
        <TableHeaders>
          <TableColumnHeader>
            <Label>Mode</Label>
            <Width>7</Width>
            <Alignment>Left</Alignment>
          </TableColumnHeader>
          <TableColumnHeader>
            <Label>LastWriteTime</Label>
            <Width>26</Width>
            <Alignment>Right</Alignment>
          </TableColumnHeader>
          <TableColumnHeader>
            <Label>CreationTime</Label>
            <Width>26</Width>
            <Alignment>Right</Alignment>
          </TableColumnHeader>
          <TableColumnHeader>
            <Label>Length</Label>
            <Width>14</Width>
            <Alignment>Right</Alignment>
          </TableColumnHeader>
          <TableColumnHeader>
            <Label>Name</Label>
            <Alignment>Left</Alignment>
          </TableColumnHeader>
        </TableHeaders>
        <TableRowEntries>
          <TableRowEntry>
            <Wrap />
            <TableColumnItems>
              <TableColumnItem>
                <PropertyName>ModeWithoutHardLink</PropertyName>
              </TableColumnItem>
              <TableColumnItem>
                <PropertyName>LastWriteTime</PropertyName>
              </TableColumnItem>
              <TableColumnItem>
                <PropertyName>CreationTime</PropertyName>
              </TableColumnItem>
              <TableColumnItem>
                <PropertyName>Length</PropertyName>
              </TableColumnItem>
              <TableColumnItem>
                <PropertyName>Name</PropertyName>
              </TableColumnItem>
            </TableColumnItems>
          </TableRowEntry>
        </TableRowEntries>
      </TableControl>
    </View>
  </ViewDefinitions>
</Configuration>
```

## <a name="see-also"></a>関連項目

[Export-FormatData](xref:Microsoft.PowerShell.Utility.Export-FormatData)

[Get-FormatData](xref:Microsoft.PowerShell.Utility.Get-FormatData)

[Get-TraceSource](xref:Microsoft.PowerShell.Utility.Get-TraceSource)

[書式設定スキーマ XML リファレンス](/powershell/scripting/developer/format/format-schema-xml-reference)

[Trace-Command](xref:Microsoft.PowerShell.Utility.Trace-Command)

[Update-FormatData](xref:Microsoft.PowerShell.Utility.Update-FormatData)

[PowerShell 書式設定ファイルを記述する](/powershell/scripting/developer/format/writing-a-powershell-formatting-file)
