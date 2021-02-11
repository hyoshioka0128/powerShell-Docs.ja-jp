---
ms.date: 08/21/2019
ms.topic: reference
title: オブジェクトのプロパティを拡張する
description: オブジェクトのプロパティを拡張する
ms.openlocfilehash: 37803d9fd87319204565c2abde62f269744481b9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652879"
---
# <a name="extending-properties-for-objects"></a>オブジェクトのプロパティを拡張する

.NET Framework オブジェクトを拡張すると、エイリアスプロパティ、コードプロパティ、メモプロパティ、スクリプトプロパティ、およびプロパティセットをオブジェクトに追加できます。 以下のセクションでは、これらのプロパティを定義する XML について説明します。

> [!NOTE]
> 次のセクションの例は、 `Types.ps1xml` PowerShell インストールディレクトリ () の既定の種類のファイルに含まれてい `$PSHOME` ます。 詳細については、「 [About Types.ps1xml](/powershell/module/microsoft.powershell.core/about/about_types.ps1xml)」を参照してください。

## <a name="alias-properties"></a>別名のプロパティ

エイリアスプロパティは、既存のプロパティの新しい名前を定義します。

次の例では、 **Count** プロパティが [system.string 型に](/dotnet/api/System.Array) 追加されています。 Alias [プロパティ](/dotnet/api/system.management.automation.psaliasproperty) 要素は、拡張プロパティを別名プロパティとして定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name)要素は、新しい名前を指定します。 また、 [Referencedmembername](/dotnet/api/system.management.automation.psaliasproperty.referencedmembername) 要素は、エイリアスによって参照される既存のプロパティを指定します。 また、 `AliasProperty` 要素をメンバーメンバーに追加することも[](/dotnet/api/system.management.automation.psmemberset)できます。

```xml
<Type>
  <Name>System.Array</Name>
  <Members>
    <AliasProperty>
      <Name>Count</Name>
      <ReferencedMemberName>Length</ReferencedMemberName>
    </AliasProperty>
  </Members>
</Type>
```

## <a name="code-properties"></a>コードのプロパティ

コードプロパティは、.NET Framework オブジェクトの静的プロパティを参照します。

次の例では、 **Mode** プロパティが [DirectoryInfo](/dotnet/api/System.IO.DirectoryInfo) 型に追加されています。 [Codeproperty](/dotnet/api/system.management.automation.pscodeproperty)要素は、拡張プロパティをコードプロパティとして定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name)要素は、拡張プロパティの名前を指定します。 また、 [Getcodereference 参照](/dotnet/api/system.management.automation.pscodeproperty.gettercodereference) 要素は、拡張プロパティによって参照される静的メソッドを定義します。 また、 `CodeProperty` 要素をメンバーメンバーに追加することも[](/dotnet/api/system.management.automation.psmemberset)できます。

```xml
<Type>
  <Name>System.IO.DirectoryInfo</Name>
  <Members>
    <CodeProperty>
      <Name>Mode</Name>
      <GetCodeReference>
        <TypeName>Microsoft.PowerShell.Commands.FileSystemProvider</TypeName>
        <MethodName>Mode</MethodName>
      </GetCodeReference>
    </CodeProperty>
  </Members>
</Type>
```

## <a name="note-properties"></a>プロパティへの注記

Note プロパティは、静的な値を持つプロパティを定義します。

次の例では、 **Status** プロパティの値が常に **Success** であるが、 [DirectoryInfo](/dotnet/api/System.IO.DirectoryInfo) 型に追加されています。 Note [プロパティ要素は、拡張](/dotnet/api/system.management.automation.psnoteproperty) プロパティを note プロパティとして定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name)要素は、拡張プロパティの名前を指定します。 [Value](/dotnet/api/system.management.automation.psnoteproperty.value)要素は、拡張プロパティの静的な値を指定します。 要素は、 `NoteProperty` メンバーメンバーに追加することもでき[](/dotnet/api/system.management.automation.psmemberset)ます。

```xml
<Type>
  <Name>System.IO.DirectoryInfo</Name>
  <Members>
    <NoteProperty>
      <Name>Status</Name>
      <Value>Success</Value>
    </NoteProperty>
  </Members>
</Type>
```

## <a name="script-properties"></a>スクリプトのプロパティ

スクリプトプロパティは、スクリプトの出力を値として持つプロパティを定義します。

次の例では、 **VersionInfo** プロパティ [が、system.string 型に](/dotnet/api/System.IO.FileInfo) 追加されます。 [Scriptproperty](/dotnet/api/system.management.automation.psscriptproperty)要素は、拡張プロパティをスクリプトプロパティとして定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name)要素は、拡張プロパティの名前を指定します。 また、 [Getscriptblock](/dotnet/api/system.management.automation.psscriptproperty.getterscript) 要素は、プロパティ値を生成するスクリプトを指定します。 また、 `ScriptProperty` 要素をメンバーメンバーに追加することも[](/dotnet/api/system.management.automation.psmemberset)できます。

```xml
<Type>
  <Name>System.IO.FileInfo</Name>
  <Members>
    <ScriptProperty>
      <Name>VersionInfo</Name>
      <GetScriptBlock>
        [System.Diagnostics.FileVersionInfo]::GetVersionInfo($this.FullName)
      </GetScriptBlock>
    </ScriptProperty>
  </Members>
</Type>
```

## <a name="property-sets"></a>プロパティセット

プロパティセットは、セットの名前で参照できる拡張プロパティのグループを定義します。
たとえば、 [Format-Table](/powershell/module/Microsoft.PowerShell.Utility/Format-Table) 
 **プロパティ** パラメーターでは、表示する特定のプロパティセットを指定できます。 プロパティセットを指定すると、そのセットに属するプロパティだけが表示されます。

オブジェクトに対して定義できるプロパティセットの数に制限はありません。 ただし、オブジェクトの既定の表示プロパティを定義するために使用されるプロパティセットは、 **Psstandardmembers** メンバーセット内で指定する必要があります。 型ファイルでは、 `Types.ps1xml` 既定のプロパティセット名に **Defaultdisplayproperty**、 **DefaultDisplayPropertySet**、および **DefaultKeyPropertySet** が含まれます。 **Psstandardmembers** メンバーセットに追加した追加のプロパティセットはすべて無視されます。

次の例では、 **DefaultDisplayPropertySet** プロパティセットが [Servicecontroller](/dotnet/api/System.ServiceProcess.ServiceController)型の **psstandardmembers** メンバーセットに追加されます。 [PropertySet](/dotnet/api/system.management.automation.pspropertyset)要素は、プロパティのグループを定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name)要素は、プロパティセットの名前を指定します。 また、 [Referencedproperties](/dotnet/api/system.management.automation.pspropertyset.referencedpropertynames) 要素は、セットのプロパティを指定します。 `PropertySet`要素を[Type](/dotnet/api/system.management.automation.pstypename)要素のメンバーに追加することもできます。

```xml
<Type>
  <Name>System.ServiceProcess.ServiceController</Name>
  <Members>
    <MemberSet>
      <Name>PSStandardMembers</Name>
      <Members>
        <PropertySet>
           <Name>DefaultDisplayPropertySet</Name>
           <ReferencedProperties>
            <Name>Status</Name
            <Name>Name</Name>
            <Name>DisplayName</Name>
          </ReferencedProperties>
        </PropertySet>
      </Members>
    </MemberSet>
  </Members>
</Type>
```

## <a name="see-also"></a>関連項目

[Types.ps1xml について](/powershell/module/microsoft.powershell.core/about/about_types.ps1xml)

[System.Management.Automation](/dotnet/api/System.Management.Automation)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
