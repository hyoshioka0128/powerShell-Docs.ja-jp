---
description: ファイルを使用して `Types.ps1xml` 、PowerShell で使用されるオブジェクトの種類を拡張する方法について説明します。
Locale: en-US
ms.date: 04/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_types.ps1xml?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Types.ps1xml
ms.openlocfilehash: 9a87394631d3318f3fb3f4b2a0cb2ab8d25408d0
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600588"
---
# <a name="about-typesps1xml"></a>Types.ps1xml について

## <a name="short-description"></a>簡単な説明
ファイルを使用して `Types.ps1xml` 、PowerShell で使用されるオブジェクトの種類を拡張する方法について説明します。

## <a name="long-description"></a>長い説明

拡張型データは、PowerShell でオブジェクト型の追加のプロパティとメソッド ("メンバー") を定義します。 PowerShell セッションに拡張型データを追加するには、2つの方法があります。

- `Types.ps1xml` file: 拡張型データを定義する XML ファイル。
- `Update-TypeData`: ファイルを再読み込み `Types.ps1xml` し、現在のセッションの型の拡張データを定義するコマンドレット。

このトピックでは、ファイルについて説明し `Types.ps1xml` ます。 コマンドレットを使用して `Update-TypeData` 動的な拡張型データを現在のセッションに追加する方法の詳細については、「 [更新-typedata](xref:Microsoft.PowerShell.Utility.Update-TypeData)」を参照してください。

## <a name="about-extended-type-data"></a>拡張型データについて

拡張型データは、PowerShell でオブジェクト型の追加のプロパティとメソッド ("メンバー") を定義します。 PowerShell でサポートされている任意の型を拡張し、オブジェクトの種類で定義されているプロパティを使用するのと同じ方法で、追加されたプロパティとメソッドを使用できます。

たとえば、PowerShell では、  `System.DateTime` コマンドレットから返されるものなど、すべてのオブジェクトに DateTime プロパティが追加さ `Get-Date` れます。

```powershell
(Get-Date).DateTime
```

```Output
Sunday, January 29, 2012 9:43:57 AM
```

**Datetime** プロパティは、datetime 構造体の説明には [あり](/dotnet/api/system.datetime)ません。これは、powershell によってプロパティが追加され、powershell でのみ表示されるためです。

PowerShell は、内部的に拡張型の既定のセットを定義します。 この型情報は、起動時にすべての PowerShell セッションに読み込まれます。 **DateTime** プロパティは、この既定のセットの一部です。 PowerShell 6 より前の場合、型定義は `Types.ps1xml` powershell インストールディレクトリ () に格納されていました `$PSHOME` 。

## <a name="adding-extended-type-data-to-powershell"></a>PowerShell への拡張型データの追加

PowerShell セッションには、拡張型データの3つのソースがあります。

- PowerShell で定義されたとは、すべての PowerShell セッションに自動的に読み込まれます。 PowerShell 6 以降では、この情報は PowerShell にコンパイルされ、ファイルには付属しなくなりました `Types.ps1xml` 。

- `Types.ps1xml`モジュールが現在のセッションにインポートされるときに、モジュールによってエクスポートされるファイルが読み込まれます。

- コマンドレットを使用して定義されている拡張型データ `Update-TypeData` は、現在のセッションにのみ追加されます。 ファイルには保存されません。

セッションでは、3つのソースからの拡張型データが同じ方法でオブジェクトに適用され、指定された型のすべてのオブジェクトで使用できます。

## <a name="the-typedata-cmdlets"></a>TypeData コマンドレット

次のコマンドレットは、PowerShell 3.0 以降の **Microsoft Powershell ユーティリティ** モジュールに含まれています。

- `Get-TypeData`: 現在のセッションの拡張型データを取得します。
- `Update-TypeData`: `Types.ps1xml` ファイルを再読み込みします。 拡張型データを現在のセッションに追加します。
- `Remove-TypeData`: 現在のセッションから拡張型データを削除します。

これらのコマンドレットの詳細については、各コマンドレットのヘルプトピックを参照してください。

## <a name="built-in-typesps1xml-files"></a>組み込みの Types.ps1xml ファイル

`Types.ps1xml`ディレクトリ内のファイル `$PSHOME` は、すべてのセッションに自動的に追加されます。

`Types.ps1xml`Powershell インストールディレクトリ () のファイル `$PSHOME` は XML ベースのテキストファイルで、powershell で使用されるオブジェクトにプロパティとメソッドを追加できます。 PowerShell には、 `Types.ps1xml` .net 型にいくつかの要素を追加する組み込みファイルがありますが、追加のファイルを作成して `Types.ps1xml` さらに型を拡張することもできます。

たとえば、既定では、配列オブジェクト () には、 `System.Array` 配列内のオブジェクトの数を一覧表示する **Length** プロパティがあります。 ただし、名前の **長さ** によってプロパティが明確に記述されていないため、PowerShell では、同じ値を表示する **Count** という名前のエイリアスプロパティが追加されます。 次の XML では、 **Count** プロパティを型に追加して `System.Array` います。

```xml
<Type>
  <Name>System.Array</Name>
  <Members>
    <AliasProperty>
      <Name>Count</Name>
      <ReferencedMemberName>
        Length
      </ReferencedMemberName>
    </AliasProperty>
  </Members>
</Type>
```

新しい **エイリアスプロパティ** を取得するには、 `Get-Member` 次の例に示すように、任意の配列に対してコマンドを使用します。

```powershell
Get-Member -InputObject (1,2,3,4)
```

コマンドを実行すると、次の結果が返されます。

```Output
Name       MemberType    Definition
----       ----------    ----------
Count      AliasProperty Count = Length
Address    Method        System.Object& Address(Int32)
Clone      Method        System.Object Clone()
CopyTo     Method        System.Void CopyTo(Array array, Int32 index):
Equals     Method        System.Boolean Equals(Object obj)
Get        Method        System.Object Get(Int32)
# ...
```

その結果、PowerShell では、 **Count** プロパティまたは配列の **Length** プロパティを使用できます。 次に例を示します。

```powershell
(1, 2, 3, 4).count
4
```

```powershell
(1, 2, 3, 4).length
4
```

## <a name="creating-new-typesps1xml-files"></a>新しい Types.ps1xml ファイルの作成

`.ps1xml`PowerShell を使用してインストールされるファイルは、書式設定にスクリプトブロックを含めることができるため、改ざんを防ぐためにデジタル署名されています。 そのため、.NET 型にプロパティまたはメソッドを追加するには、独自のファイルを作成 `Types.ps1xml` し、PowerShell セッションに追加します。

新しいファイルを作成するには、まず既存のファイルをコピーし `Types.ps1xml` ます。 新しいファイルには任意の名前を付けることができますが、ファイル名拡張子を持つ必要があり `.ps1xml` ます。 PowerShell にアクセスできる任意のディレクトリに新しいファイルを配置できますが、ファイルを PowerShell インストールディレクトリ ( `$PSHOME` ) またはインストールディレクトリのサブディレクトリに配置すると便利です。

新しいファイルを保存したら、コマンドレットを使用し `Update-TypeData` て PowerShell セッションに新しいファイルを追加します。 定義されている組み込み型よりも型を優先する場合は、コマンドレットの **PrependData** パラメーターを使用し `Update-TypeData` ます。 `Update-TypeData` 現在のセッションのみに影響します。 今後のすべてのセッションに変更を加えるには、コンソールをエクスポートするか、 `Update-TypeData` PowerShell プロファイルにコマンドを追加します。

## <a name="typesps1xml-and-add-member"></a>Types.ps1xml と Add-Member

ファイルは、 `Types.ps1xml` 影響を受ける PowerShell セッションで、指定された .net 型のオブジェクトのすべてのインスタンスにプロパティとメソッドを追加します。 ただし、プロパティまたはメソッドをオブジェクトの1つのインスタンスにのみ追加する必要がある場合は、コマンドレットを使用し `Add-Member` ます。

詳細については、「 [メンバーの追加](xref:Microsoft.PowerShell.Utility.Add-Member)」を参照してください。

## <a name="example-adding-an-age-member-to-fileinfo-objects"></a>例: FileInfo オブジェクトへの Age メンバーの追加

この例では、 **Age** プロパティを system.object オブジェクトに追加する **方法を示し** ます。 ファイルの有効期間は、その作成時刻と現在の時刻の差です (日数)。

**Age** プロパティは、スクリプトブロックを使用して計算されるので、 `<ScriptProperty>` 新しい **age** プロパティのモデルとして使用するタグを見つけます。

次の XML コードをファイルに保存し `$PSHOME\MyTypes.ps1xml` ます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Types>
  <Type>
    <Name>System.IO.FileInfo</Name>
    <Members>
      <ScriptProperty>
        <Name>Age</Name>
        <GetScriptBlock>
          ((Get-Date) - ($this.CreationTime)).Days
        </GetScriptBlock>
      </ScriptProperty>
    </Members>
  </Type>
</Types>
```

を実行して、 `Update-TypeData` 現在のセッションに新しいファイルを追加し `Types.ps1xml` ます。 このコマンドは、 **PrependData** パラメーターを使用して、新しいファイルを元の定義よりも上位の優先順位に配置します。

の詳細について `Update-TypeData` は、「 [更新-typedata](xref:Microsoft.PowerShell.Utility.Update-TypeData)」を参照してください。

```powershell
Update-Typedata -PrependPath $PSHOME\MyTypes.ps1xml
```

変更をテストするには、コマンドを実行して `Get-ChildItem` ディレクトリ内の PowerShell.exe ファイルを取得し、そのファイルを `$PSHOME` コマンドレットにパイプして、 `Format-List` ファイルのすべてのプロパティを一覧表示します。 変更の結果として、 **Age** プロパティが一覧に表示されます。

```powershell
Get-ChildItem $PSHOME\pwsh.exe | Select-Object Age
```

```Output
142
```

## <a name="the-xml-in-typesps1xml-files"></a>Types.ps1xml ファイル内の XML

完全なスキーマ定義は、GitHub の PowerShell ソースコードリポジトリにある [Types .xsd](https://github.com/PowerShell/PowerShell/blob/master/src/Schemas/Types.xsd) にあります。

タグは、 `<Types>` ファイルで定義されているすべての型を囲みます。 タグは1つだけにする必要があり `<Types>` ます。

ファイルに記述されている各 .NET 型は、タグで表す必要があり `<Type>` ます。

型タグには、次のタグが含まれている必要があります。

`<Name>`: 影響を受ける .NET 型の名前を囲みます。

`<Members>`: .NET 型に対して定義されている新しいプロパティとメソッドのタグを囲みます。

タグ内には、次のいずれかのメンバータグを含めることができ `<Members>` ます。

`<AliasProperty>`: 既存のプロパティの新しい名前を定義します。

タグには、 `<AliasProperty>` `<Name>` 新しいプロパティの名前と既存のプロパティを指定するタグを指定するタグが必要 `<ReferencedMemberName>` です。

たとえば、 **Count** エイリアスプロパティは、配列オブジェクトの **Length** プロパティのエイリアスです。

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

`<CodeMethod>`: .NET クラスの静的メソッドを参照します。

タグには、 `<CodeMethod>` `<Name>` 新しいメソッドの名前を指定するタグと `<GetCodeReference>` 、メソッドが定義されているコードを指定するタグが必要です。

たとえば、オブジェクトの **Mode** プロパティ `System.IO.DirectoryInfo` は、PowerShell FileSystem プロバイダーで定義されているコードプロパティです。

```xml
<Type>
  <Name>System.IO.DirectoryInfo</Name>
  <Members>
    <CodeProperty>
      <Name>Mode</Name>
      <GetCodeReference>
        <TypeName>
          Microsoft.PowerShell.Commands.FileSystemProvider
        </TypeName>
        <MethodName>Mode</MethodName>
      </GetCodeReference>
    </CodeProperty>
  </Members>
</Type>
```

`<CodeProperty>`: .NET クラスの静的メソッドを参照します。

タグには、 `<CodeProperty>` `<Name>` 新しいプロパティの名前を指定するタグと、プロパティが定義されているコードを指定するタグが必要 `<GetCodeReference>` です。

たとえば、オブジェクトの **Mode** プロパティ `System.IO.DirectoryInfo` は、PowerShell FileSystem プロバイダーで定義されているコードプロパティです。

```xml
<Type>
  <Name>System.IO.DirectoryInfo</Name>
  <Members>
    <CodeProperty>
      <Name>Mode</Name>
      <GetCodeReference>
        <TypeName>
          Microsoft.PowerShell.Commands.FileSystemProvider
        </TypeName>
        <MethodName>Mode</MethodName>
      </GetCodeReference>
    </CodeProperty>
  </Members>
</Type>
```

`<MemberSet>`: メンバーのコレクション (プロパティとメソッド) を定義します。

タグは、 `<MemberSet>` プライマリタグ内に表示され `<Members>` ます。 タグは、 `<Name>` メンバーセットの名前を囲むタグと `<Members>` 、セット内のメンバー (プロパティとメソッド) を囲むセカンダリタグで囲む必要があります。 プロパティ (やなど) またはメソッド (やなど) を作成するすべてのタグは `<NoteProperty>` `<ScriptProperty>` `<Method>` `<ScriptMethod>` 、セットのメンバーになることができます。

ファイルでは、タグを使用して `Types.ps1xml` `<MemberSet>` PowerShell で .net オブジェクトの既定のビューを定義します。 この場合、メンバーセットの名前 (タグ内の値 `<Name>` ) は常に **Psstandardmembers** で、プロパティの名前 (タグの値 `<Name>` ) は次のいずれかになります。

- `DefaultDisplayProperty`: オブジェクトの1つのプロパティ。

- `DefaultDisplayPropertySet`: オブジェクトの1つ以上のプロパティ。

- `DefaultKeyPropertySet`: オブジェクトの1つまたは複数のキープロパティ。 キープロパティは、セッション履歴内の項目の ID 番号など、プロパティ値のインスタンスを識別します。

たとえば、次の XML では、 `System.ServiceProcess.ServiceController` コマンドレットによって返されるサービス (オブジェクト) の既定の表示を定義して `Get-Service` います。 **Psstandardmembers** という名前のメンバーセットを定義します。これは、 **Status**、 **Name**、 **DisplayName** の各プロパティが設定された既定のプロパティで構成されます。

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
            <Name>Status</Name>
            <Name>Name</Name>
            <Name>DisplayName</Name>
          </ReferencedProperties>
        </PropertySet>
      </Members>
    </MemberSet>
  </Members>
</Type>
```

`<Method>`: 基になるオブジェクトのネイティブメソッドを参照します。

`<Methods>`: オブジェクトのメソッドのコレクション。

`<NoteProperty>`: 静的な値を持つプロパティを定義します。

タグには、 `<NoteProperty>` `<Name>` 新しいプロパティの名前を指定するタグと、 `<Value>` プロパティの値を指定するタグが必要です。

たとえば、次の XML では、 **DirectoryInfo** オブジェクトの **Status** プロパティが作成されます。 **Status** プロパティの値は常に **Success** です。

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

`<ParameterizedProperty>`: 引数を受け取り、値を返すプロパティ。

`<Properties>`: オブジェクトのプロパティのコレクション。

`<Property>`: ベースオブジェクトのプロパティ。

`<PropertySet>`: オブジェクトのプロパティのコレクションを定義します。

タグには、 `<PropertySet>` `<Name>` プロパティセットの名前と、プロパティを指定するタグを指定するタグが必要 `<ReferencedProperty>` です。 プロパティの名前はタグで囲まれ `<Name>` ます。

で `Types.ps1xml` は、タグを使用して、 `<PropertySet>` オブジェクトの既定の表示のプロパティのセットを定義します。 既定の表示は、タグのタグの **Psstandardmembers** 値によって識別でき `<Name>` `<MemberSet>` ます。

たとえば、次の XML では、オブジェクトの **Status** プロパティが作成され `System.IO.DirectoryInfo` ます。 **Status** プロパティの値は常に **Success** です。

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
            <Name>Status</Name>
            <Name>Name</Name>
            <Name>DisplayName</Name>
          </ReferencedProperties>
        </PropertySet>
      </Members>
    </MemberSet>
  </Members>
</Type>
```

`<ScriptMethod>`: スクリプトの出力を値として持つメソッドを定義します。

タグには、 `<ScriptMethod>` `<Name>` 新しいメソッドの名前と、 `<Script>` メソッドの結果を返すスクリプトブロックを囲むタグを指定するタグが必要です。

たとえば、 `ConvertToDateTime` `ConvertFromDateTime` 管理オブジェクト () のメソッドとメソッド `System.System.Management.ManagementObject` は、 `ToDateTime` `ToDmtfDateTime` クラスのおよび静的メソッドを使用するスクリプトメソッドです `System.Management.ManagementDateTimeConverter` 。

```xml
<Type>
 <Name>System.Management.ManagementObject</Name>
 <Members>
 <ScriptMethod>
   <Name>ConvertToDateTime</Name>
   <Script>
   [System.Management.ManagementDateTimeConverter]::ToDateTime($args[0])
   </Script>
 </ScriptMethod>
 <ScriptMethod>
   <Name>ConvertFromDateTime</Name>
   <Script>
   [System.Management.ManagementDateTimeConverter]::ToDmtfDateTime($args[0])
   </Script>
 </ScriptMethod>
 </Members>
</Type>
```

`<ScriptProperty>`: スクリプトの出力を値として持つプロパティを定義します。

タグには、 `<ScriptProperty>` `<Name>` 新しいプロパティの名前を指定するタグと、 `<GetScriptBlock>` プロパティ値を返すスクリプトブロックを囲むタグが必要です。

たとえば、 **VersionInfo** オブジェクトの GetVersionInfo プロパティは、 **FileVersionInfo** オブジェクトの静的メソッドの **FullName** プロパティを使用した結果として得られるスクリプトプロパティ **です。**

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

詳細については、「 [Windows PowerShell Software Development Kit (SDK)](/powershell/scripting/developer/windows-powershell)」を参照してください。

## <a name="update-typedata"></a>Update-TypeData

`Types.ps1xml`PowerShell セッションにファイルを読み込むには、コマンドレットを実行し `Update-TypeData` ます。 ファイル内の型を組み込みファイル内の型よりも優先する場合は `Types.ps1xml` 、の **PrependData** パラメーターを追加し `Update-TypeData` ます。 `Update-TypeData` 現在のセッションのみに影響します。 今後のすべてのセッションに変更を加えるには、セッションをエクスポートするか、 `Update-TypeData` PowerShell プロファイルにコマンドを追加します。

プロパティで発生した例外、またはプロパティをコマンドに追加したときに発生した例外は `Update-TypeData` 、にエラーを報告しません `StdErr` 。 これは、書式設定および出力の際に、多くの一般的な型で発生する例外を抑制します。 .NET のプロパティを取得する場合は、次の例に示すように、代わりにメソッド構文を使用して例外の抑制を回避できます。

```powershell
"hello".get_Length()
```

メソッド構文は .NET プロパティでのみ使用できます。 コマンドレットの実行によって追加されたプロパティ `Update-TypeData` は、メソッド構文を使用できません。

## <a name="signing-a-typesps1xml-file"></a>Types.ps1xml ファイルへの署名

ファイルのユーザーを保護するために `Types.ps1xml` 、デジタル署名を使用してファイルに署名することができます。 詳細については、「 [about_Signing](about_Signing.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Signing](about_Signing.md)

[Copy-Item](xref:Microsoft.PowerShell.Management.Copy-Item)

[Copy-ItemProperty](xref:Microsoft.PowerShell.Management.Copy-ItemProperty)

[Get-Member](xref:Microsoft.PowerShell.Utility.Get-Member)

[Get-TypeData](xref:Microsoft.PowerShell.Utility.Get-TypeData)

[Remove-TypeData](xref:Microsoft.PowerShell.Utility.Remove-TypeData)

[Update-TypeData](xref:Microsoft.PowerShell.Utility.Update-TypeData)

