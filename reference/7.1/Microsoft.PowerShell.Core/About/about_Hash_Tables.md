---
description: PowerShell でハッシュテーブルを作成、使用、および並べ替える方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/28/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_hash_tables?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Hash_Tables
ms.openlocfilehash: 26d5aad01833bc564ad65f2dcd141c7f57042431
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391393"
---
# <a name="about-hash-tables"></a>ハッシュテーブルについて

## <a name="short-description"></a>概要
PowerShell でハッシュテーブルを作成、使用、および並べ替える方法について説明します。

## <a name="long-description"></a>詳細説明

ハッシュテーブルは、ディクショナリまたは連想配列とも呼ばれ、1つまたは複数のキーと値のペアを格納するコンパクトなデータ構造です。 たとえば、ハッシュテーブルには、一連の IP アドレスとコンピューター名が含まれている場合があります。 IP アドレスはキー、コンピューター名は値、またはその逆です。

PowerShell では、各ハッシュテーブルは Hashtable (system.string) オブジェクトです。 PowerShell では、Hashtable オブジェクトのプロパティとメソッドを使用できます。

PowerShell 3.0 以降では、PowerShell で [ordered] 属性を使用して、順序付けされた辞書 (OrderedDictionary) を作成できます。

順序付けされたディクショナリはハッシュテーブルとは異なり、キーは常に一覧表示される順序で表示されます。 ハッシュテーブル内のキーの順序は決定されません。

ハッシュテーブルのキーと値は、.NET オブジェクトでもあります。 多くの場合、文字列または整数ですが、任意のオブジェクト型を持つことができます。 また、入れ子になったハッシュテーブルを作成することもできます。この場合、キーの値は別のハッシュテーブルになります。

ハッシュテーブルは、データの検索と取得に非常に効率的であるため、頻繁に使用されます。 ハッシュテーブルを使用して、一覧を格納したり、PowerShell で計算されたプロパティを作成したりできます。 また、PowerShell には、文字列をハッシュテーブルに変換する Convertfrom-csv Convertfrom-stringdata というコマンドレットが用意されています。

### <a name="syntax"></a>構文

ハッシュテーブルの構文は次のとおりです。

```powershell
@{ <name> = <value>; [<name> = <value> ] ...}
```

順序付けられたディクショナリの構文は次のとおりです。

```powershell
[ordered]@{ <name> = <value>; [<name> = <value> ] ...}
```

PowerShell 3.0 では、[ordered] 属性が導入されました。

### <a name="creating-hash-tables"></a>ハッシュテーブルの作成

ハッシュテーブルを作成するには、次のガイドラインに従います。

- アットマーク (@) を使用してハッシュテーブルを開始します。
- ハッシュテーブルを中かっこ () で囲み {} ます。
- ハッシュテーブルのコンテンツの1つまたは複数のキーと値のペアを入力します。
- 各キーを値から区切るには、等号 (=) を使用します。
- セミコロン (;) を使用するまたは、キーと値のペアを区切るための改行。
- スペースが含まれているキーは、引用符で囲む必要があります。 値は有効な PowerShell 式である必要があります。 文字列は、スペースを含まない場合でも引用符で囲む必要があります。
- ハッシュテーブルを管理するには、それを変数に保存します。
- 順序付けされたハッシュテーブルを変数に割り当てる場合は、[ordered] 属性を "@" 記号の前に配置します。 変数名の前に配置すると、コマンドは失敗します。

$Hash の値に空のハッシュテーブルを作成するには、次のように入力します。

```powershell
$hash = @{}
```

ハッシュテーブルを作成するときに、キーと値を追加することもできます。 たとえば、次のステートメントでは、3つのキーを持つハッシュテーブルを作成します。

```powershell
$hash = @{ Number = 1; Shape = "Square"; Color = "Blue"}
```

### <a name="creating-ordered-dictionaries"></a>並べ替えられた辞書の作成

OrderedDictionary 型のオブジェクトを追加することによって、順序付けされたディクショナリを作成できますが、順序付けられたディクショナリを作成する最も簡単な方法は [Ordered] 属性を使用することです。

[Ordered] 属性は、PowerShell 3.0 で導入されました。

"@" 記号の直前に属性を配置します。

```powershell
$hash = [ordered]@{ Number = 1; Shape = "Square"; Color = "Blue"}
```

順序付けされたディクショナリは、ハッシュテーブルを使用するのと同じ方法で使用できます。
どちらの種類も、ハッシュテーブルまたはディクショナリ (iDictionary) を受け取るパラメーターの値として使用できます。

[Ordered] 属性を使用してハッシュテーブルを変換またはキャストすることはできません。 順序付けされた属性を変数名の前に配置すると、コマンドは失敗し、次のエラーメッセージが表示されます。

```powershell
PS C:\> [ordered]$hash = @{}
At line:1 char:1
+ [ordered]$hash = @{}
+ [!INCLUDE[]()]
The ordered attribute can be specified only on a hash literal node.
+ CategoryInfo          : ParserError: (:) [], ParentContainsErrorRecordExc
eption
+ FullyQualifiedErrorId : OrderedAttributeOnlyOnHashLiteralNode
```

式を修正するには、[ordered] 属性を移動します。

```powershell
PS C:\> $hash = [ordered]@{}
```

順序付けされたディクショナリをハッシュテーブルにキャストできますが、変数をクリアして新しい値を入力しても、順序付けされた属性を回復することはできません。 順序を再設定するには、変数を削除して再作成する必要があります。

```
PS C:\> [hashtable]$hash = [ordered]@{
>> Number = 1; Shape = "Square"; Color = "Blue"}
PS C:\> $hash

Name                           Value
----                           -----
Color                          Blue
Shape                          Square
Number                         1
```

### <a name="displaying-hash-tables"></a>ハッシュテーブルの表示

変数に保存されているハッシュテーブルを表示するには、変数名を入力します。
既定では、ハッシュテーブルは、キーの列が1つ、値が1つのテーブルとして表示されます。

```powershell
C:\PS> $hash

Name                           Value
----                           -----
Shape                          Square
Color                          Blue
Number                         1
```

ハッシュテーブルには、キーと値のプロパティがあります。 すべてのキーまたはすべての値を表示するには、ドット表記を使用します。

```powershell
C:\PS> $hash.keys
Number
Shape
Color

C:\PS> $hash.values
1
Square
Blue
```

また、各キー名はハッシュテーブルのプロパティであり、その値はキー名プロパティの値です。 プロパティ値を表示するには、次の形式を使用します。

```powershell
$hashtable.<key>
<value>
```

次に例を示します。

```powershell
C:\PS> $hash.Number
1

C:\PS> $hash.Color
Blue
```

キー名がハッシュテーブルの型のいずれかのプロパティ名と競合する場合は、を使用して `PSBase` これらのプロパティにアクセスできます。 たとえば、キー名がで、キーのコレクションを取得する場合は、次の `keys` 構文を使用します。

```powershell
$hashtable.PSBase.Keys
```

ハッシュテーブルには、ハッシュテーブル内のキーと値のペアの数を示す Count プロパティがあります。

```powershell
C:\PS> $hash.count
3
```

ハッシュテーブルテーブルは配列ではないため、ハッシュテーブルのインデックスとして整数を使用することはできませんが、ハッシュテーブルにインデックスを作成するには、キー名を使用します。
キーが文字列値の場合は、キー名を引用符で囲みます。

次に例を示します。

```powershell
C:\PS> $hash["Number"]
1
```

### <a name="adding-and-removing-keys-and-values"></a>キーと値の追加と削除

ハッシュテーブルにキーと値を追加するには、次のコマンド形式を使用します。

```powershell
$hash["<key>"] = "<value>"
```

たとえば、"Now" という値を持つ "Time" キーをハッシュテーブルに追加するには、次のステートメント形式を使用します。

```powershell
$hash["Time"] = "Now"
```

また、配列の Add メソッドを使用して、キーと値をハッシュテーブルに追加することもできます。 Add メソッドには、次の構文があります。

```powershell
Add(Key, Value)
```

たとえば、"Now" という値を持つ "Time" キーをハッシュテーブルに追加するには、次のステートメント形式を使用します。

```powershell
$hash.Add("Time", "Now")
```

また、加算演算子 (+) を使用してハッシュテーブルを既存のハッシュテーブルに追加することで、ハッシュテーブルにキーと値を追加できます。 たとえば、次のステートメントは、値が "Now" の "Time" キーを $hash 変数のハッシュテーブルに追加します。

```powershell
$hash = $hash + @{Time="Now"}
```

変数に格納されている値を追加することもできます。

```powershell
$t = "Today"
$now = (Get-Date)

$hash.Add($t, $now)
```

減算演算子を使用してハッシュテーブルからキーと値のペアを削除することはできませんが、Hashtable オブジェクトの Remove メソッドを使用することはできます。 Remove メソッドでは、キーが値として取得されます。

Remove メソッドの構文は次のとおりです。

```
Remove(Key)
```

たとえば、$hash 変数の値のハッシュテーブルから Time = Now のキーと値のペアを削除するには、次のように入力します。

```powershell
$hash.Remove("Time")
```

PowerShell では、Hashtable オブジェクトのすべてのプロパティとメソッド (Contains、Clear、Clone、CopyTo など) を使用できます。 Hashtable オブジェクトの詳細については [、「system.string](/dotnet/api/system.collections.hashtable)」を参照してください。

### <a name="object-types-in-hashtables"></a>ハッシュテーブルのオブジェクトの種類

ハッシュテーブル内のキーと値は任意の .NET オブジェクト型を持つことができ、1つのハッシュテーブルには複数の型のキーと値を含めることができます。

次のステートメントでは、プロセス名の文字列のハッシュテーブルを作成し、オブジェクトの値を処理して、変数に保存し `$p` ます。

```powershell
$p = @{"PowerShell" = (Get-Process PowerShell);
"Notepad" = (Get-Process notepad)}
```

でハッシュテーブルを表示し、キー名プロパティを使用して値を表示することができ `$p` ます。

```powershell
C:\PS> $p

Name                           Value
----                           -----
PowerShell                     System.Diagnostics.Process (PowerShell)
Notepad                        System.Diagnostics.Process (notepad)

C:\PS> $p.PowerShell

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    441      24    54196      54012   571     5.10   1788 PowerShell

C:\PS> $p.keys | foreach {$p.$_.handles}
441
251
```

ハッシュテーブル内のキーは、任意の .NET 型にすることもできます。 次のステートメントは、変数のハッシュテーブルにキーと値のペアを追加し `$p` ます。 キーは、WinRM サービスを表すサービスオブジェクトであり、値はサービスの現在の状態です。

```powershell
C:\PS> $p = $p + @{(Get-Service WinRM) = ((Get-Service WinRM).Status)}
```

新しいキーと値のペアを表示してアクセスするには、ハッシュテーブル内の他のペアに使用するのと同じメソッドを使用します。

```powershell
C:\PS> $p

Name                           Value
----                           -----
PowerShell                     System.Diagnostics.Process (PowerShell)
Notepad                        System.Diagnostics.Process (notepad)
System.ServiceProcess.Servi... Running

C:\PS> $p.keys
PowerShell
Notepad

Status   Name               DisplayName
------   ----               -----------
Running  winrm              Windows Remote Management (WS-Manag...

C:\PS> $p.keys | foreach {$_.name}
winrm
```

ハッシュテーブル内のキーと値は、Hashtable オブジェクトにすることもできます。 次のステートメントでは、キーが文字列である変数のハッシュテーブルにキーと値のペアを追加し、 `$p` 値は、3つのキーと値のペアを持つハッシュテーブルに Hash2 ます。

```powershell
C:\PS> $p = $p + @{"Hash2"= @{a=1; b=2; c=3}}
```

同じメソッドを使用して、新しい値を表示したり、アクセスしたりすることができます。

```powershell
C:\PS> $p

Name                           Value
----                           -----
PowerShell                     System.Diagnostics.Process (PowerShell)
Notepad                        System.Diagnostics.Process (notepad)
System.ServiceProcess.Servi... Running
Hash2                          {a, b, c}

C:\PS> $p.Hash2

Name                           Value
----                           -----
a                              1
b                              2
c                              3

C:\PS> $p.Hash2.b
2
```

### <a name="sorting-keys-and-values"></a>キーと値の並べ替え

ハッシュテーブル内の項目は、本質的に順序付けられていません。 キーと値のペアは、表示するたびに異なる順序で表示される場合があります。

ハッシュテーブルを並べ替えることはできませんが、ハッシュテーブルの GetEnumerator メソッドを使用してキーと値を列挙し、Sort-Object コマンドレットを使用して列挙値を並べ替えて表示することができます。

たとえば、次のコマンドは、変数内のハッシュテーブル内のキーと値を列挙 `$p` し、キーをアルファベット順に並べ替えます。

```powershell
C:\PS> $p.GetEnumerator() | Sort-Object -Property key

Name                           Value
----                           -----
Notepad                        System.Diagnostics.Process (notepad)
PowerShell                     System.Diagnostics.Process (PowerShell)
System.ServiceProcess.Servi... Running
```

次のコマンドでは、同じ手順を使用して、ハッシュ値を降順で並べ替えます。

```powershell
C:\PS> $p.getenumerator() | Sort-Object -Property Value -Descending

Name                           Value
----                           -----
PowerShell                     System.Diagnostics.Process (PowerShell)
Notepad                        System.Diagnostics.Process (notepad)
System.ServiceProcess.Servi... Running
```

### <a name="creating-objects-from-hash-tables"></a>ハッシュテーブルからのオブジェクトの作成

PowerShell 3.0 以降では、プロパティとプロパティ値のハッシュテーブルからオブジェクトを作成できます。

構文は次のとおりです。

```powershell
[<class-name>]@{
  <property-name>=<property-value>
  <property-name>=<property-value>
}
```

このメソッドは、null コンストラクターを持つクラス (つまり、パラメーターを持たないコンストラクター) に対してのみ機能します。 オブジェクトのプロパティは、パブリックで設定可能である必要があります。

詳細については、「 [about_Object_Creation](about_Object_Creation.md)」を参照してください。

### <a name="convertfrom-stringdata"></a>ConvertFrom-StringData

`ConvertFrom-StringData`コマンドレットは、文字列またはキーと値のペアのここから文字列をハッシュテーブルに変換します。 スクリプトのデータセクションでコマンドレットを安全に使用できます。また、コマンドレットを使用して、 `ConvertFrom-StringData` `Import-LocalizedData` 現在のユーザーのユーザーインターフェイス (UI) カルチャにユーザーメッセージを表示することもできます。

ここでは、ハッシュテーブルの値に引用符が含まれている場合に特に便利です。 ここで説明する文字列の詳細については、「 [about_Quoting_Rules](about_Quoting_Rules.md)」を参照してください。

次の例では、前の例でユーザーメッセージのここから文字列を作成する方法と、を使用して `ConvertFrom-StringData` 文字列からハッシュテーブルに変換する方法を示します。

次のコマンドは、キーと値のペアのここに文字列を作成し、文字列変数に保存し \$ ます。

```powershell
C:\PS> $string = @"
Msg1 = Type "Windows".
Msg2 = She said, "Hello, World."
Msg3 = Enter an alias (or "nickname").
"@
```

このコマンドは、ConvertFrom-StringData コマンドレットを使用して、この文字列をハッシュテーブルに変換します。

```powershell
C:\PS> ConvertFrom-StringData $string

Name                           Value
----                           -----
Msg3                           Enter an alias (or "nickname").
Msg2                           She said, "Hello, World."
Msg1                           Type "Windows".
```

ここで説明する文字列の詳細については、「 [about_Quoting_Rules](about_Quoting_Rules.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Arrays](about_Arrays.md)

[about_Object_Creation](about_Object_Creation.md)

[about_Quoting_Rules](about_Quoting_Rules.md)

[about_Script_Internationalization](about_Script_Internationalization.md)

[ConvertFrom-StringData](xref:Microsoft.PowerShell.Utility.ConvertFrom-StringData)

[Import-LocalizedData](xref:Microsoft.PowerShell.Utility.Import-LocalizedData)

[System.Collections.Hashtable](/dotnet/api/system.collections.hashtable)
