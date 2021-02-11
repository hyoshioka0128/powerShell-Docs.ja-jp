---
ms.date: 09/13/2016
ms.topic: reference
title: 出力オブジェクトを拡張する
description: 出力オブジェクトを拡張する
ms.openlocfilehash: 9fea476e3032002bd206609313581cc6373dfddc
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652895"
---
# <a name="extending-output-objects"></a>出力オブジェクトを拡張する

型ファイル (types.ps1xml) を使用して、コマンドレット、関数、およびスクリプトによって返される .NET Framework オブジェクトを拡張できます。 型ファイルは、既存のオブジェクトにプロパティとメソッドを追加できる XML ベースのファイルです。 たとえば、Windows PowerShell には Types.ps1xml ファイルが用意されています。これにより、既存のいくつかの .NET Framework オブジェクトに要素が追加されます。 Types.ps1xml ファイルは、Windows PowerShell インストールディレクトリ () にあり `$pshome` ます。 独自の型ファイルを作成して、それらのオブジェクトをさらに拡張したり、他のオブジェクトを拡張したりすることができます。 型ファイルを使用してオブジェクトを拡張すると、そのオブジェクトのすべてのインスタンスが新しい要素で拡張されます。

## <a name="extending-the-systemarray-object"></a>System.string オブジェクトの拡張

次の例は、Windows PowerShell が Types.ps1xml ファイル内の [system.string オブジェクトを](/dotnet/api/System.Array) 拡張する方法を示しています。 既定では、 [system.string オブジェクトに](/dotnet/api/System.Array) は、 `Length` 配列内のオブジェクトの数を一覧表示するプロパティがあります。 ただし、名前 "length" はプロパティを明確に記述していないため、Windows PowerShell は `Count` alias プロパティを追加します。このプロパティには、プロパティと同じ値が表示され `Length` ます。 次の XML は、 `Count` プロパティを system.string 型に追加 [します](/dotnet/api/System.Array) 。

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

この新しいエイリアスプロパティを表示するには、次の例に示すように、任意の配列で [Get メンバー](/powershell/module/Microsoft.PowerShell.Utility/Get-Member) コマンドを使用します。

```powershell
Get-Member -InputObject (1,2,3,4)
```

コマンドを実行すると、次の結果が返されます。

```output
Name           MemberType    Definition
----           ----------    ----------
Count          AliasProperty Count = Length
Address        Method        System.Object& Address(Int32 )
Clone          Method        System.Object Clone()
CopyTo         Method        System.Void CopyTo(Array array, Int32 index):
Equals         Method        System.Boolean Equals(Object obj)
Get            Method        System.Object Get(Int32 )
...
Length         Property      System.Int32 Length {get;}
```

プロパティまたはプロパティのいずれかを使用して、 `Count` `Length` 配列内のオブジェクトの数を確認できます。 次に例を示します。

```powershell
PS> (1, 2, 3, 4).Count
```

```output
4
```

```powershell
PS> (1, 2, 3, 4).Length
```

```output
4
```

## <a name="custom-types-files"></a>カスタム型ファイル

カスタム型ファイルを作成するには、まず既存の型ファイルをコピーします。 新しいファイルには任意の名前を付けることができますが、ファイル名の拡張子は types.ps1xml である必要があります。 ファイルをコピーすると、Windows PowerShell にアクセスできる任意のディレクトリに新しいファイルを配置できますが、ファイルを Windows PowerShell インストールディレクトリ ( `$pshome` ) またはインストールディレクトリのサブディレクトリに配置すると便利です。

独自の拡張型をファイルに追加するには、拡張する各オブジェクトの types 要素を追加します。 次のトピックでは例を紹介します。

- プロパティとプロパティセットの追加の詳細については、「[拡張プロパティ](./extending-properties-for-objects.md)」を参照してください。

- メソッドの追加の詳細については、「 [拡張メソッド](./defining-default-methods-for-objects.md)」を参照してください。

- メンバーセットの追加の詳細については、「 [拡張メンバーセット](./defining-default-member-sets-for-objects.md)」を参照してください。

独自の拡張型を定義したら、次のいずれかの方法を使用して拡張オブジェクトを使用できるようにします。

- 拡張型ファイルを現在のセッションで使用できるようにするには、 [更新-TypeData](/powershell/module/Microsoft.PowerShell.Utility/Update-TypeData) コマンドレットを使用して、新しいファイルを追加します。 他の型ファイル (Types.ps1xml ファイルを含む) で定義されている型よりも型を優先する場合は、 `PrependData` [更新-typedata](/powershell/module/Microsoft.PowerShell.Utility/Update-TypeData) コマンドレットのパラメーターを使用します。
- 拡張型ファイルを今後のすべてのセッションで使用できるようにするには、モジュールに型ファイルを追加するか、現在のセッションをエクスポートするか、または [更新-TypeData](/powershell/module/Microsoft.PowerShell.Utility/Update-TypeData) コマンドを Windows PowerShell プロファイルに追加します。

## <a name="signing-types-files"></a>署名の種類ファイル

XML にはスクリプトブロックを含めることができるため、改ざんを防ぐために型ファイルをデジタル署名する必要があります。 デジタル署名の追加の詳細については、「」を参照してください [about_Signing](/powershell/module/microsoft.powershell.core/about/about_signing)

## <a name="see-also"></a>参照

[オブジェクトの既定のプロパティの定義](./extending-properties-for-objects.md)

[オブジェクトの既定のメソッドを定義する](./defining-default-methods-for-objects.md)

[オブジェクトの既定のメンバー セットを定義する](./defining-default-member-sets-for-objects.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
