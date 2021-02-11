---
ms.date: 09/13/2016
ms.topic: reference
title: リスト ビュー (グループ別)
description: リスト ビュー (グループ別)
ms.openlocfilehash: e039c38d1e4e93f65a508fe60aaaf35c64ebe2ed
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666622"
---
# <a name="list-view-groupby"></a>リスト ビュー (グループ別)

この例では、リストの行をグループに分割するリストビューを実装する方法を示します。 このリストビューには、Servicecontroller のプロパティが表示されます。 [Displayproperty =](/dotnet/api/System.ServiceProcess.ServiceController) [Get Service](/powershell/module/Microsoft.PowerShell.Management/Get-Service) コマンドレットによって返される Fullname オブジェクト。 リストビューのコンポーネントの詳細については、「 [リストビューの作成](./creating-a-list-view.md)」を参照してください。

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

- オブジェクトの新しいグループをどのように表示するかを定義する [GroupBy](./viewselectedby-element-format.md) 要素。

- ビューによって表示されるプロパティを定義する [ListControl](./listcontrol-element-format.md) 要素。

- リストビューの行に表示される内容を定義する [ListItem](./listitem-element-for-listitems-for-listcontrol-format.md) 要素。

- 表示されるプロパティを定義する [PropertyName](./propertyname-element-for-listitem-for-listcontrol-format.md) 要素。

## <a name="example"></a>例

次の XML は、 [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController.Status) プロパティの値が変更されるたびに新しいグループを開始するリストビューを定義します。 各グループが開始されると、プロパティの新しい値を含むカスタムラベルが表示されます。

```xml
<Configuration>
  <ViewDefinitions>
    <View>
      <Name>System.ServiceProcess.ServiceController</Name>
      <ViewSelectedBy>
        <TypeName>System.ServiceProcess.ServiceController</TypeName>
      </ViewSelectedBy>
      <GroupBy>
        <PropertyName>Status</PropertyName>
        <Label>New Service Status</Label>
      </GroupBy>
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

次の例は、Windows PowerShell で Servicecontroller を表示する方法を示しています。 [Displayproperty = Fullname](/dotnet/api/System.ServiceProcess.ServiceController) オブジェクトこのフォーマットファイルが読み込まれた後。 グループラベルの前後に追加された空白行は、Windows PowerShell によって自動的に追加されます。

```powershell
Get-Service f*
```

```output
   New Service Status: Stopped

Name        : Fax
DisplayName : Fax
ServiceType : Win32OwnProcess

   New Service Status: Running

Name        : FCSAM
DisplayName : Microsoft Antimalware Service
ServiceType : Win32OwnProcess

   New Service Status: Stopped

Name        : fdPHost
DisplayName : Function Discovery Provider Host
ServiceType : Win32ShareProcess

   New Service Status: Running

Name        : FDResPub
DisplayName : Function Discovery Resource Publication
ServiceType : Win32ShareProcess

Name        : FontCache
DisplayName : Windows Font Cache Service
ServiceType : Win32ShareProcess

   New Service Status: Stopped

Name        : FontCache3.0.0.0
DisplayName : Windows Presentation Foundation Font Cache 3.0.0.0
ServiceType : Win32OwnProcess

   New Service Status: Running

Name        : FSysAgent
DisplayName : Microsoft Forefront System Agent
ServiceType : Win32OwnProcess

Name        : FwcAgent
DisplayName : Firewall Client Agent
ServiceType : Win32OwnProcess
```

## <a name="see-also"></a>参照

[書式設定ファイルの例](./examples-of-formatting-files.md)

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
