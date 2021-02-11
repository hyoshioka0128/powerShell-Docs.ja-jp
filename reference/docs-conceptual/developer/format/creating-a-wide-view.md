---
ms.date: 09/13/2016
ms.topic: reference
title: ワイド ビューを作成する
description: ワイド ビューを作成する
ms.openlocfilehash: 4230ef91a3612e962b2773b12e8016df6f760eae
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655598"
---
# <a name="creating-a-wide-view"></a>ワイド ビューを作成する

ワイドビューでは、表示されているオブジェクトごとに1つの値が表示されます。 表示される値には、.NET オブジェクトプロパティの値、またはスクリプトの値を指定できます。 既定では、このビューにはラベルまたはヘッダーがありません。

## <a name="a-wide-view-display"></a>ワイドビューの表示

次の例は、Windows PowerShell の出力が[フォーマット全体](/powershell/module/Microsoft.PowerShell.Utility/Format-Wide)のコマンドレットにパイプされるときに、 [Get process](/powershell/module/Microsoft.PowerShell.Management/Get-Process)コマンドレットによって返さ[れる system.object オブジェクトを表示](/dotnet/api/System.Diagnostics.Process)する方法を示しています。 (既定では、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) コマンドレットはテーブルビューを返します)。この例では、2つの列を使用して、返された各オブジェクトのプロセスの名前を表示します。 オブジェクトのプロパティの名前は表示されず、プロパティの値のみが表示されます。

```
Get-Process | format-wide
AEADISRV                     agrsmsvc
Ati2evxx                     Ati2evxx
audiodg                      CCC
CcmExec                      communicator
Crypserv                     csrss
csrss                        DevDtct2
DM1Service                   dpupdchk
dwm                          DxStudio
EXCEL                        explorer
GoogleToolbarNotifier        GrooveMonitor
hpqwmiex                     hpservice
Idle                         InoRpc
InoRT                        InoTask
ipoint                       lsass
lsm                          MOM
MSASCui                      notepad
...                          ...

```

## <a name="defining-the-wide-view"></a>ワイドビューの定義

次の XML [は、system.string オブジェクトの](/dotnet/api/System.Diagnostics.Process) ワイドビュースキーマを示しています。

```xml
View>
  <Name>process</Name>
  <ViewSelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </ViewSelectedBy>
  <GroupBy>...</GroupBy>
  <Controls>...</Controls>
  <WideControl>
    <WideEntries>
      <WideEntry>
        <WideItem>
          <PropertyName>ProcessName</PropertyName>
        </WideItem>
      </WideEntry>
    </WideEntries>
  </WideControl>
</View>

```

ワイドビューを定義するには、次の XML 要素を使用します。

- [ビュー](./view-element-format.md)要素は、ワイドビューの親要素です。 (これは、テーブル、リスト、およびカスタムコントロールビューの親要素と同じです)。

- [Name](./name-element-for-view-format.md)要素は、ビューの名前を指定します。 この要素は、すべてのビューに必要です。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、ビューを使用するオブジェクトを定義します。 この要素は必須です。

- [GroupBy](./groupby-element-for-view-format.md)要素は、オブジェクトの新しいグループが表示されるタイミングを定義します。 特定のプロパティまたはスクリプトの値が変更されるたびに、新しいグループが開始されます。 この要素は省略可能です。

- [Controls](./controls-element-for-view-format.md)要素は、ワイドビューによって定義されるカスタムコントロールを定義します。 コントロールを使用すると、データの表示方法をさらに指定することができます。 この要素は省略可能です。 ビューでは、独自のカスタムコントロールを定義することも、書式設定ファイルの任意のビューで使用できるコモンコントロールを使用することもできます。 カスタムコントロールの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

- [WideControl](./widecontrol-element-format.md)要素とその子要素は、ビューに表示される内容を定義します。 前の例では、ビューは [Processname](/dotnet/api/System.Diagnostics.Process.ProcessName) プロパティを表示するように設計されています。

単純なワイドビューを定義する完全な書式設定ファイルの例については、「 [Wide ビュー (基本)](./wide-view-basic.md)」を参照してください。

## <a name="providing-definitions-for-your-wide-view"></a>ワイドビューの定義を提供する

ワイドビューでは、 [WideControl](./widecontrol-element-format.md) 要素の子要素を使用して1つ以上の定義を指定できます。 通常、ビューの定義は1つだけです。 次の例では、ビューは [Processname](/dotnet/api/System.Diagnostics.Process.ProcessName) プロパティの値を表示する1つの定義を提供します。 ワイドビューでは、プロパティの値やスクリプトの値を表示できます (例には示されていません)。

```xml
<WideControl>
  <AutoSize/>
  <ColumnNumber></ColumnNumber>
  <WideEntries>
    <WideEntry>
      <WideItem>
        <PropertyName>ProcessName</PropertyName>
      </WideItem>
    </WideEntry>
  </WideEntries>
</WideControl>
```

次の XML 要素を使用して、ワイドビューの定義を指定できます。

- [WideControl](./widecontrol-element-format.md)要素とその子要素は、ビューに表示される内容を定義します。

- [AutoSize](./autosize-element-for-widecontrol-format.md)要素は、列のサイズと列の数をデータのサイズに基づいて調整するかどうかを指定します。 この要素は省略可能です。

- [Columnnumber](./columnnumber-element-for-widecontrol-format.md)要素は、ワイドビューに表示される列の数を指定します。 この要素は省略可能です。

- [WideEntries](./wideentries-element-for-widecontrol-format.md)要素は、ビューの定義を提供します。 ほとんどの場合、ビューには定義が1つだけ含まれます。 この要素は必須です。

- [WideEntry](./wideentry-element-for-widecontrol-format.md)要素は、ビューの定義を提供します。 少なくとも1つの [WideEntry](./wideentry-element-for-widecontrol-format.md) が必要です。ただし、追加できる要素の数に上限はありません。 ほとんどの場合、ビューには定義が1つだけ含まれます。

- [Entryselectedby](./entryselectedby-element-for-wideentry-format.md)要素は、特定の定義によって表示されるオブジェクトを指定します。 この要素は省略可能であり、異なるオブジェクトを表示する複数の [WideEntry](./wideentry-element-for-widecontrol-format.md) 要素を定義する場合にのみ必要です。

- [WideItem](./wideitem-element-for-widecontrol-format.md)要素は、ビューによって表示されるデータを指定します。 他の種類のビューとは異なり、ワイドコントロールで表示できる項目は1つだけです。

- [PropertyName](./propertyname-element-for-wideitem-for-widecontrol-format.md)要素は、ビューによって表示される値を持つプロパティを指定します。 プロパティまたはスクリプトのどちらかを指定する必要がありますが、両方を指定することはできません。

- [ScriptBlock](./scriptblock-element-for-wideitem-for-widecontrol-format.md)要素は、ビューによって値が表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

- [FormatString](./formatstring-element-for-wideitem-for-widecontrol-format.md)要素は、データの表示に使用されるパターンを指定します。 この要素は省略可能です。

ワイドビュー定義を定義する完全な書式設定ファイルの例については、「 [Wide ビュー (基本)](./wide-view-basic.md)」を参照してください。

## <a name="defining-the-objects-that-use-the-wide-view"></a>ワイドビューを使用するオブジェクトの定義

ワイドビューを使用する .NET オブジェクトを定義するには、次の2つの方法があります。 [Viewselectedby](./viewselectedby-element-format.md)要素を使用すると、ビューのすべての定義で表示できるオブジェクトを定義できます。また、 [entryselectedby](./entryselectedby-element-for-wideentry-format.md)要素を使用して、ビューの特定の定義によって表示されるオブジェクトを定義することもできます。 ほとんどの場合、ビューには定義が1つしかないため、オブジェクトは通常、 [Viewselectedby](./viewselectedby-element-format.md) 要素によって定義されます。

次の例は、 [Viewselectedby](./viewselectedby-element-format.md) および [TypeName](./typename-element-for-viewselectedby-format.md) 要素を使用して、ワイドビューによって表示されるオブジェクトを定義する方法を示しています。 指定できる [TypeName](./typename-element-for-viewselectedby-format.md) 要素の数に制限はありません。これらの要素の順序は重要ではありません。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </ViewSelectedBy>
  <WideControl>...</WideControl>
</View>
```

次の XML 要素を使用して、ワイドビューで使用されるオブジェクトを指定できます。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、ワイドビューによって表示されるオブジェクトを定義します。

- [TypeName](./typename-element-for-viewselectedby-format.md)要素は、ビューによって表示される .net を指定します。 完全修飾 .NET 型名が必要です。 ビューには少なくとも1つの種類または選択セットを指定する必要がありますが、指定できる要素の最大数はありません。

完全な書式設定ファイルの例については、「 [ワイドビュー (Basic)](./wide-view-basic.md)」を参照してください。

次の例では、 [Viewselectedby](./viewselectedby-element-format.md) 要素と [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md) 要素を使用します。 複数のビューを使用して表示される関連オブジェクトのセットを持つ選択セットを使用します。たとえば、同じオブジェクトに対してワイドビューやテーブルビューを定義する場合などです。 選択セットを作成する方法の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <SelectionSetName>.NET Type Set</SelectionSetName>
  </ViewSelectedBy>
  <WideControl>...</WideControl>
</View>
```

次の XML 要素を使用して、ワイドビューで使用されるオブジェクトを指定できます。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、ワイドビューによって表示されるオブジェクトを定義します。

- [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md)要素は、ビューで表示できるオブジェクトのセットを指定します。 ビューには少なくとも1つの選択セットまたは型を指定する必要がありますが、指定できる要素の最大数はありません。

次の例では、 [Entryselectedby](./entryselectedby-element-for-wideentry-format.md) 要素を使用して、ワイドビューの特定の定義によって表示されるオブジェクトを定義する方法を示します。 この要素を使用して、オブジェクトの .NET 型名、選択したオブジェクトのセット、または定義を使用するタイミングを指定する選択条件を指定できます。 選択条件を作成する方法の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

```xml
<WideEntry>
  <EntrySelectedBy>
    <TypeName>.NET Type</TypeName>
  </EntrySelectedBy>
</WideEntry>
```

次の XML 要素を使用して、ワイドビューの特定の定義で使用されるオブジェクトを指定できます。

- [Entryselectedby](./entryselectedby-element-for-wideentry-format.md)要素は、定義によって表示されるオブジェクトを定義します。

- [TypeName](./typename-element-for-viewselectedby-format.md)要素は、定義によって表示される .net を指定します。 この要素を使用する場合は、完全修飾 .NET 型名が必要です。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。

- [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md)要素 (表示されません) は、この定義で表示できるオブジェクトのセットを指定します。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。

- [Selectioncondition](./selectioncondition-element-for-entryselectedby-for-widecontrol-format.md)要素 (表示されません) は、この定義を使用するために存在する必要がある条件を指定します。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。 選択条件の定義の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="displaying-groups-of-objects-in-a-wide-view"></a>ワイドビューでのオブジェクトのグループの表示

ワイドビューで表示されるオブジェクトは、グループに分けることができます。 これは、グループを定義することではなく、特定のプロパティまたはスクリプトの値が変更されるたびに、Windows PowerShell によって新しいグループが開始されることを意味します。 次の例では、 [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController.ServiceType) プロパティの値が変更されるたびに、新しいグループが開始されます。

```xml
<GroupBy>
  <Label>Service Type</Label>
  <PropertyName>ServiceType</PropertyName>
</GroupBy>

```

グループがいつ開始されるかを定義するには、次の XML 要素を使用します。

- [GroupBy](./groupby-element-for-view-format.md)要素は、新しいグループを開始するプロパティまたはスクリプトを定義し、グループの表示方法を定義します。

- [PropertyName](./propertyname-element-for-groupby-format.md)要素は、値が変更されるたびに新しいグループを開始するプロパティを指定します。 グループを開始するには、プロパティまたはスクリプトを指定する必要がありますが、両方を指定することはできません。

- [ScriptBlock](./scriptblock-element-for-groupby-format.md)要素は、値が変更されるたびに新しいグループを開始するスクリプトを指定します。 グループを開始するには、スクリプトまたはプロパティを指定する必要がありますが、両方を指定することはできません。

- [Label](./label-element-for-groupby-format.md)要素は、各グループの先頭に表示されるラベルを定義します。 Windows PowerShell は、この要素で指定されたテキストに加えて、新しいグループをトリガーした値を表示し、ラベルの前後に空白行を追加します。 この要素は省略可能です。

- [CustomControl](./customcontrol-element-for-groupby-format.md)要素は、データの表示に使用されるコントロールを定義します。 この要素は省略可能です。

- [CustomControlName](./customcontrolname-element-for-groupby-format.md)要素は、データの表示に使用されるコモンコントロールまたはビューコントロールを指定します。 この要素は省略可能です。

グループを定義する完全な書式設定ファイルの例については、「 [Wide ビュー (GroupBy)](./wide-view-groupby.md)」を参照してください。

## <a name="using-format-strings"></a>書式指定文字列の使用

書式設定文字列をワイドビューに追加して、データの表示方法をさらに詳細に定義できます。 次の例は、プロパティの値の書式設定文字列を定義する方法を示して `StartTime` います。

```xml
<WideItem>
  <PropertyName>StartTime</PropertyName>
  <FormatString>{0:MMM} (0:DD) (0:HH):(0:MM)</FormatString>
</WideItem>
```

次の XML 要素を使用して、書式パターンを指定できます。

- [WideItem](./wideitem-element-for-widecontrol-format.md)要素は、ビューによって表示されるデータを指定します。

- [PropertyName](./propertyname-element-for-wideitem-for-widecontrol-format.md)要素は、ビューによって表示される値を持つプロパティを指定します。 プロパティまたはスクリプトのどちらかを指定する必要がありますが、両方を指定することはできません。

- [FormatString](./formatstring-element-for-wideitem-for-widecontrol-format.md)要素は、プロパティまたはスクリプトの値をビューに表示する方法を定義する形式パターンを指定します。

- [ScriptBlock](./scriptblock-element-for-wideitem-for-widecontrol-format.md)要素 (表示されません) は、ビューによって値が表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

次の例では、 `ToString` スクリプトの値の書式を設定するためにメソッドが呼び出されています。 スクリプトは、オブジェクトの任意のメソッドを呼び出すことができます。 したがって、オブジェクトに、書式設定パラメーターを持つなどのメソッドがある場合、 `ToString` スクリプトはそのメソッドを呼び出してスクリプトの出力値を書式設定できます。

```xml
<WideItem>
  <ScriptBlock>
    [String}::Format("(0,10) (1,8)", .LastWriteTime.ToString("d"),
    $_.LastWriteTime.Totring("t"))
  </ScriptBlock>
</WideItem>
```

次の XML 要素を使用して、メソッドを呼び出すことができ `ToString` ます。

- [WideItem](./wideitem-element-for-widecontrol-format.md)要素は、ビューによって表示されるデータを指定します。

- [ScriptBlock](./scriptblock-element-for-wideitem-for-widecontrol-format.md)要素 (表示されません) は、ビューによって値が表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

## <a name="see-also"></a>参照

[ワイド ビュー (基本)](./wide-view-basic.md)

[ワイド ビュー (グループ別)](./wide-view-groupby.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
