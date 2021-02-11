---
ms.date: 09/13/2016
ms.topic: reference
title: リスト ビューを作成する
description: リスト ビューを作成する
ms.openlocfilehash: c34c4fddc27d4fd016fba37fc465924d693756a5
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655638"
---
# <a name="creating-a-list-view"></a>リスト ビューを作成する

リストビューでは、1つの列にデータが順番に表示されます。 一覧に表示されるデータは、.NET プロパティの値、またはスクリプトの値にすることができます。

## <a name="a-list-view-display"></a>リストビューの表示

次の出力は、Windows PowerShell が Servicecontroller のプロパティをどのように表示するかを示しています。 [Displayproperty =](/dotnet/api/System.ServiceProcess.ServiceController) [Get Service](/powershell/module/microsoft.powershell.management/get-service) コマンドレットによって返される Fullname オブジェクト。 この例では、3つのオブジェクトが返されました。各オブジェクトは、前のオブジェクトから空白行に区切られています。

```powershell
Get-Service | format-list
```

```output
Name                : AEADIFilters
DisplayName         : Andrea ADI Filters Service
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess

Name                : AeLookupSvc
DisplayName         : Application Experience
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32ShareProcess

Name                : AgereModemAudio
DisplayName         : Agere Modem Call Progress Audio
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : False
CanStop             : True
ServiceType         : Win32OwnProcess
...
```

## <a name="defining-the-list-view"></a>リストビューの定義

次の XML は、Servicecontroller のいくつかのプロパティを表示するためのリストビュースキーマを示して [います。Displayproperty = Fullname](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクト。 リストビューに表示する各プロパティを指定する必要があります。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.ServiceProcess.ServiceController</TypeName>
  </ViewSelectedBy>
  <ListControl>
    <ListEntries>
      <ListEntry>
        <ListItems>
          <ListItem>
            <PropertyName>Name</PropertyName>
          </ListItem>
          <ListItem>
            <PropertyName>DisplayName</PropertyName>
          </ListItem>
          <ListItem>
            <PropertyName>Status</PropertyName>
          </ListItem>
          <ListItem>
            <PropertyName>ServiceType</PropertyName>
          </ListItem>
        </ListItems>
      </ListEntry>
    </ListEntries>
  </ListControl>
</View>
```

リストビューを定義するには、次の XML 要素を使用します。

- [ビュー](./view-element-format.md)要素は、リストビューの親要素です。 (これは、テーブル、ワイド、およびカスタムコントロールビューの親要素と同じです)。

- [Name](./name-element-for-view-format.md)要素は、ビューの名前を指定します。 この要素は、すべてのビューに必要です。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、ビューを使用するオブジェクトを定義します。 この要素は必須です。

- [GroupBy](./groupby-element-for-view-format.md)要素は、オブジェクトの新しいグループが表示されるタイミングを定義します。 特定のプロパティまたはスクリプトの値が変更されるたびに、新しいグループが開始されます。 この要素は省略可能です。

- [Controls](./controls-element-for-view-format.md)要素は、リストビューによって定義されるカスタムコントロールを定義します。 コントロールを使用すると、データの表示方法をさらに指定することができます。 この要素は省略可能です。 ビューでは、独自のカスタムコントロールを定義することも、書式設定ファイルの任意のビューで使用できるコモンコントロールを使用することもできます。 カスタムコントロールの詳細については、「 [カスタムコントロールの作成](./creating-custom-controls.md)」を参照してください。

- [ListControl](./listcontrol-element-format.md)要素は、ビューに表示される内容と書式設定を定義します。 他のすべてのビューと同様に、リストビューでは、オブジェクトのプロパティまたはスクリプトによって生成される値の値を表示できます。

単純なリストビューを定義する完全な書式設定ファイルの例については、「 [リストビュー (基本)](./list-view-basic.md)」を参照してください。

## <a name="providing-definitions-for-your-list-view"></a>リストビューの定義を指定する

リストビューでは、 [ListControl](./listcontrol-element-format.md) 要素の子要素を使用して1つまたは複数の定義を指定できます。 通常、ビューの定義は1つだけです。 次の例では、ビューには、システムの複数のプロパティを表示する1つの定義が用意されています。 [Displayproperty = Fullname](/dotnet/api/System.Diagnostics.Process) オブジェクト。 リストビューでは、プロパティの値またはスクリプトの値を表示できます (例には示されていません)。

```xml
<ListControl>
    <ListEntries>
      <ListEntry>
        <ListItems>
          <ListItem>
            <PropertyName>Name</PropertyName>
          </ListItem>
          <ListItem>
            <PropertyName>DisplayName</PropertyName>
          </ListItem>
          <ListItem>
            <PropertyName>Status</PropertyName>
          </ListItem>
          <ListItem>
            <PropertyName>ServiceType</PropertyName>
          </ListItem>
        </ListItems>
      </ListEntry>
    </ListEntries>
  </ListControl>

```

次の XML 要素を使用して、リストビューの定義を指定できます。

- [ListControl](./listcontrol-element-format.md)要素とその子要素は、ビューに表示される内容を定義します。

- [Listentries](./listentries-element-for-listcontrol-format.md)要素は、ビューの定義を提供します。 ほとんどの場合、ビューには定義が1つだけ含まれます。 この要素は必須です。

- [ListEntry](./listentry-element-for-listcontrol-format.md)要素は、ビューの定義を提供します。 少なくとも1つの [ListEntry](./listentry-element-for-listcontrol-format.md) が必要です。ただし、追加できる要素の数に上限はありません。 ほとんどの場合、ビューには定義が1つだけ含まれます。

- [Entryselectedby](./entryselectedby-element-for-listentry-for-listcontrol-format.md)要素は、特定の定義によって表示されるオブジェクトを指定します。 この要素は省略可能であり、異なるオブジェクトを表示する複数の [ListEntry](./listentry-element-for-listcontrol-format.md) 要素を定義する場合にのみ必要です。

- [ListItems](./listitems-element-for-listentry-for-listcontrol-format.md)要素は、リストビューの行に値が表示されるプロパティとスクリプトを指定します。

- [ListItem](./listitems-element-for-listentry-for-listcontrol-format.md)要素は、リストビューの行に表示される値を持つプロパティまたはスクリプトを指定します。 リストビューでは、少なくとも1つのプロパティまたはスクリプトを指定する必要があります。 指定できる行の数に上限はありません。

- [PropertyName](./propertyname-element-for-listitem-for-listcontrol-format.md)要素は、値が行に表示されるプロパティを指定します。 プロパティまたはスクリプトのどちらかを指定する必要がありますが、両方を指定することはできません。

- [ScriptBlock](./scriptblock-element-for-listitem-for-listcontrol-format.md)要素は、値が行に表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

- [Label](./label-element-for-listitem-for-listcontrol-format.md)要素は、行のプロパティまたはスクリプト値の左側に表示されるラベルを指定します。 この要素は省略可能です。 ラベルが指定されていない場合は、プロパティまたはスクリプトの名前が表示されます。 完全な例については、「 [リストビュー (ラベル)](./list-view-labels.md)」を参照してください。

- [Itemselectioncondition](./itemselectioncondition-element-for-listitem-for-listcontrol-format.md)要素は、表示される行に対して存在する必要がある条件を指定します。 リストビューに条件を追加する方法の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。 この要素は省略可能です。

- [FormatString](./formatstring-element-for-listitem-for-listcontrol-format.md)要素は、プロパティまたはスクリプトの値を表示するために使用されるパターンを指定します。 この要素は省略可能です。

単純なリストビューを定義する完全な書式設定ファイルの例については、「 [リストビュー (基本)](./list-view-basic.md)」を参照してください。

## <a name="defining-the-objects-that-use-the-list-view"></a>リストビューを使用するオブジェクトの定義

リストビューを使用する .NET オブジェクトを定義するには、次の2つの方法があります。 [Viewselectedby](./viewselectedby-element-format.md)要素を使用すると、ビューのすべての定義で表示できるオブジェクトを定義できます。また、 [entryselectedby](./entryselectedby-element-for-listentry-for-listcontrol-format.md)要素を使用して、ビューの特定の定義によって表示されるオブジェクトを定義することもできます。 ほとんどの場合、ビューには定義が1つしかないため、オブジェクトは通常、 [Viewselectedby](./viewselectedby-element-format.md) 要素によって定義されます。

次の例は、 [Viewselectedby](./viewselectedby-element-format.md) および [TypeName](./typename-element-for-viewselectedby-format.md) 要素を使用してリストビューに表示されるオブジェクトを定義する方法を示しています。 指定できる [TypeName](./typename-element-for-viewselectedby-format.md) 要素の数に制限はありません。これらの要素の順序は重要ではありません。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <TypeName>System.Diagnostics.Process</TypeName>
  </ViewSelectedBy>
  <ListControl>...</ListControl>
</View>
```

次の XML 要素を使用して、リストビューで使用されるオブジェクトを指定できます。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、リストビューによって表示されるオブジェクトを定義します。

- [TypeName](./typename-element-for-viewselectedby-format.md)要素は、ビューによって表示される .net オブジェクトを指定します。 完全修飾 .NET 型名が必要です。 ビューには少なくとも1つの種類または選択セットを指定する必要がありますが、指定できる要素の最大数はありません。

完全な書式設定ファイルの例については、「 [リストビュー (基本)](./list-view-basic.md)」を参照してください。

次の例では、 [Viewselectedby](./viewselectedby-element-format.md) 要素と [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md) 要素を使用します。 複数のビューを使用して表示される関連オブジェクトのセットがある場合、たとえば、同じオブジェクトのリストビューやテーブルビューを定義するときに、選択セットを使用します。 選択セットを作成する方法の詳細については、「 [選択セットの定義](./defining-selection-sets.md)」を参照してください。

```xml
<View>
  <Name>System.ServiceProcess.ServiceController</Name>
  <ViewSelectedBy>
    <SelectionSetName>.NET Type Set</SelectionSetName>
  </ViewSelectedBy>
  <ListControl>...</ListControl>
</View>
```

次の XML 要素を使用して、リストビューで使用されるオブジェクトを指定できます。

- [Viewselectedby](./viewselectedby-element-format.md)要素は、リストビューによって表示されるオブジェクトを定義します。

- [SelectionSetName](./selectionsetname-element-for-viewselectedby-format.md)要素は、ビューで表示できるオブジェクトのセットを指定します。 ビューには少なくとも1つの選択セットまたは型を指定する必要がありますが、指定できる要素の最大数はありません。

次の例では、 [Entryselectedby](./entryselectedby-element-for-listentry-for-listcontrol-format.md) 要素を使用して、リストビューの特定の定義によって表示されるオブジェクトを定義する方法を示します。 この要素を使用して、オブジェクトの .NET 型名、選択したオブジェクトのセット、または定義を使用するタイミングを指定する選択条件を指定できます。 選択条件を作成する方法の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

```xml
<ListEntry>
  <EntrySelectedBy>
    <TypeName>.NET Type</TypeName>
  </EntrySelectedBy>
</ListEntry>
```

次の XML 要素を使用して、リストビューの特定の定義によって使用されるオブジェクトを指定できます。

- [Entryselectedby](./entryselectedby-element-for-listentry-for-listcontrol-format.md)要素は、定義によって表示されるオブジェクトを定義します。

- [TypeName](./typename-element-for-entryselectedby-for-listcontrol-format.md)要素は、定義によって表示される .net オブジェクトを指定します。 この要素を使用する場合は、完全修飾された .NET 型名が必要です。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。

- [SelectionSetName](./selectionsetname-element-for-entryselectedby-for-listcontrol-format.md)要素 (表示されません) は、この定義で表示できるオブジェクトのセットを指定します。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。

- [Selectioncondition](./selectioncondition-element-for-entryselectedby-for-listcontrol-format.md)要素 (表示されません) は、この定義を使用するために存在する必要がある条件を指定します。 定義には、少なくとも1つの種類、選択セット、または選択条件を指定する必要がありますが、指定できる要素の最大数はありません。 選択条件の定義の詳細については、「 [データを表示するための条件の定義](./defining-conditions-for-displaying-data.md)」を参照してください。

## <a name="displaying-groups-of-objects-in-a-list-view"></a>リストビューでのオブジェクトのグループの表示

リストビューによって表示されるオブジェクトをグループに分けることができます。 これは、グループを定義することではなく、特定のプロパティまたはスクリプトの値が変更されるたびに、Windows PowerShell によって新しいグループが開始されることを意味します。 次の例では、 [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController.ServiceType) プロパティの値が変更されるたびに、新しいグループが開始されます。

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

グループを定義する完全な書式設定ファイルの例については、「 [リストビュー (GroupBy)](./list-view-groupby.md)」を参照してください。

## <a name="using-format-strings"></a>書式指定文字列の使用

書式設定文字列をビューに追加して、データの表示方法をさらに定義できます。 次の例は、プロパティの値の書式設定文字列を定義する方法を示して `StartTime` います。

```xml
<ListItem>
  <PropertyName>StartTime</PropertyName>
  <FormatString>{0:MMM} (0:DD) (0:HH):(0:MM)</FormatString>
</ListItem>
```

次の XML 要素を使用して、書式パターンを指定できます。

- [ListItem](./listitem-element-for-listitems-for-listcontrol-format.md)要素は、ビューによって表示されるデータを指定します。

- [PropertyName](./propertyname-element-for-listitem-for-listcontrol-format.md)要素は、ビューによって表示される値を持つプロパティを指定します。 プロパティまたはスクリプトのどちらかを指定する必要がありますが、両方を指定することはできません。

- [FormatString](./formatstring-element-for-listitem-for-listcontrol-format.md)要素は、プロパティまたはスクリプトの値をビューに表示する方法を定義する形式パターンを指定します。

- [ScriptBlock](./scriptblock-element-for-listitem-for-listcontrol-format.md)要素 (表示されません) は、ビューによって値が表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

次の例では、 `ToString` スクリプトの値の書式を設定するためにメソッドが呼び出されています。 スクリプトは、オブジェクトの任意のメソッドを呼び出すことができます。 したがって、オブジェクトに、書式設定パラメーターを持つなどのメソッドがある場合、 `ToString` スクリプトはそのメソッドを呼び出してスクリプトの出力値を書式設定できます。

```xml
<ListItem>
  <ScriptBlock>
    [String}::Format("(0,10) (1,8)", .LastWriteTime.ToString("d"),
    $_.LastWriteTime.Totring("t"))
  </ScriptBlock>
</ListItem>
```

次の XML 要素を使用して、メソッドを呼び出すことができ `ToString` ます。

- [ListItem](./listitem-element-for-listitems-for-listcontrol-format.md)要素は、ビューによって表示されるデータを指定します。

- [ScriptBlock](./scriptblock-element-for-listitem-for-listcontrol-format.md)要素 (表示されません) は、ビューによって値が表示されるスクリプトを指定します。 スクリプトまたはプロパティのいずれかを指定する必要がありますが、両方を指定することはできません。

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](../cmdlet/writing-a-windows-powershell-cmdlet.md)
