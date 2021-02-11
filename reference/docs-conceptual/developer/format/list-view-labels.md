---
ms.date: 09/13/2016
ms.topic: reference
title: リスト ビュー (ラベル)
description: リスト ビュー (ラベル)
ms.openlocfilehash: 2d341ae95d025e0f95b5d88b96afb846b62b092f
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666690"
---
# <a name="list-view-labels"></a>リスト ビュー (ラベル)

この例では、リストの各行にカスタムラベルを表示するリストビューを実装する方法を示します。 このリストビューには、Servicecontroller のプロパティが表示されます。 [Displayproperty =](/dotnet/api/System.ServiceProcess.ServiceController) [Get Service](/powershell/module/Microsoft.PowerShell.Management/Get-Service) コマンドレットによって返される Fullname オブジェクト。 リストビューのコンポーネントの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

### <a name="to-load-this-formatting-file"></a>この書式設定ファイルを読み込むには

1. このトピックの「例」のセクションにある XML をテキストファイルにコピーします。

2. テキスト ファイルを保存します。 ファイルに拡張子を追加して、 `format.ps1xml` 書式設定ファイルとして識別するようにしてください。

3. Windows PowerShell を開き、次のコマンドを実行して、書式設定ファイルを現在のセッションに読み込み `Update-formatdata -prependpath PathToFormattingFile` ます。

   > [!WARNING]
   > この書式設定ファイルは、Windows PowerShell の書式設定ファイルによって既に定義されているオブジェクトの表示を定義します。 `prependPath`コマンドレットを実行するときにパラメーターを使用する必要があります。このフォーマットファイルをモジュールとして読み込むことはできません。

## <a name="demonstrates"></a>対象

この書式設定ファイルは、次の XML 要素を示しています。

- ビューの [Name](./name-element-for-view-format.md) 要素。

- ビューによって表示されるオブジェクトを定義する [Viewselectedby](./viewselectedby-element-format.md) 要素。

- ビューによって表示されるプロパティを定義する [ListControl](./listcontrol-element-format.md) 要素。

- リストビューの行に表示される内容を定義する [ListItem](./listitem-element-for-listitems-for-listcontrol-format.md) 要素。

- リストビューの行に表示される内容を定義する [Label](./label-element-for-listitem-for-listcontrol-format.md) 要素。

- 表示されるプロパティを定義する [PropertyName](./propertyname-element-for-listitem-for-listcontrol-format.md) 要素。

## <a name="example"></a>例

次の XML は、各行にカスタムラベルを表示するリストビューを定義しています。 この場合、ラベルには、各文字を大文字にしたプロパティ名と "property" という語が含まれます。 各行には、プロパティの名前の後にプロパティの値が表示されます。

```xml
<Configuration>
  <ViewDefinitions>
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
            <Label>NAME property</Label>
            <PropertyName>Name</PropertyName>
          </ListItem>
          <ListItem>
            <Label>DISPLAYNAME property</Label>
            <PropertyName>DisplayName</PropertyName>
          </ListItem>
          <ListItem>
            <Label>STATUS property</Label>
            <PropertyName>Status</PropertyName>
          </ListItem>
          <ListItem>
            <Label>SERVICETYPE property</Label>
            <PropertyName>ServiceType</PropertyName>
          </ListItem>
        </ListItems>
      </ListEntry>
    </ListEntries>
  </ListControl>
</View>

  </ViewDefinitions>
</Configuration>
```

次の例は、Windows PowerShell で Servicecontroller を表示する方法を示しています。 [Displayproperty = Fullname](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトこのフォーマットファイルが読み込まれた後。

```powershell
Get-Service f*
```

```output
NAME property        : Fax
DISPLAYNAME property : Fax
STATUS property      : Stopped
SERVICETYPE property : Win32OwnProcess

NAME property        : FCSAM
DISPLAYNAME property : Microsoft Antimalware Service
STATUS property      : Running
SERVICETYPE property : Win32OwnProcess

NAME property        : fdPHost
DISPLAYNAME property : Function Discovery Provider Host
STATUS property      : Stopped
SERVICETYPE property : Win32ShareProcess

NAME property        : FDResPub
DISPLAYNAME property : Function Discovery Resource Publication
STATUS property      : Running
SERVICETYPE property : Win32ShareProcess

NAME property        : FontCache
DISPLAYNAME property : Windows Font Cache Service
STATUS property      : Running
SERVICETYPE property : Win32ShareProcess

NAME property        : FontCache3.0.0.0
DISPLAYNAME property : Windows Presentation Foundation Font Cache 3.0.0.0
STATUS property      : Stopped
SERVICETYPE property : Win32OwnProcess

NAME property        : FSysAgent
DISPLAYNAME property : Microsoft Forefront System Agent
STATUS property      : Running
SERVICETYPE property : Win32OwnProcess

NAME property        : FwcAgent
DISPLAYNAME property : Firewall Client Agent
STATUS property      : Running
SERVICETYPE property : Win32OwnProcess
```

## <a name="see-also"></a>参照

[書式設定ファイルの例](./examples-of-formatting-files.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
