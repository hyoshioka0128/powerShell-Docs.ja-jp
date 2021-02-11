---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/select-xml?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Select-Xml
ms.openlocfilehash: 7686ade747345b7c770772067007b8abf13aac3d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602391"
---
# Select-Xml

## 概要
XML 文字列または XML ドキュメント内のテキストを検索します。

## SYNTAX

### Xml (既定値)

```
Select-Xml [-Xml] <XmlNode[]> [-XPath] <String> [-Namespace <Hashtable>] [<CommonParameters>]
```

### パス

```
Select-Xml [-Path] <String[]> [-XPath] <String> [-Namespace <Hashtable>] [<CommonParameters>]
```

### LiteralPath

```
Select-Xml -LiteralPath <String[]> [-XPath] <String> [-Namespace <Hashtable>] [<CommonParameters>]
```

### コンテンツ

```
Select-Xml -Content <String[]> [-XPath] <String> [-Namespace <Hashtable>] [<CommonParameters>]
```

## Description

Xml **コマンドレット** を使用すると、XPath クエリを使用して、Xml 文字列とドキュメント内のテキストを検索できます。
XPath クエリを入力し、 *Content*、 *Path*、または *xml* パラメーターを使用して、検索する xml を指定します。

## 例

### 例 1: [エイリアス] プロパティノードを選択する

```
PS C:\> $Path = "$Pshome\Types.ps1xml"
PS C:\> $XPath = "/Types/Type/Members/AliasProperty"
PS C:\> Select-Xml -Path $Path -XPath $Xpath | Select-Object -ExpandProperty Node
Name                 ReferencedMemberName
----                 --------------------
Count                Length
Name                 Key
Name                 ServiceName
RequiredServices     ServicesDependedOn
ProcessName          Name
Handles              Handlecount
VM                   VirtualSize
WS                   WorkingSetSize
Name                 ProcessName
Handles              Handlecount
VM                   VirtualMemorySize
WS                   WorkingSet
PM                   PagedMemorySize
NPM                  NonpagedSystemMemorySize
Name                 __Class
Namespace            ModuleName
```

この例では、Types.ps1xml のエイリアス プロパティを取得します
(このファイルの詳細については、「about_Types.ps1xml」を参照してください)。

最初のコマンドは、Types.ps1xml ファイルのパスを $Path 変数に保存します。

2 番目のコマンドは、AliasProperty ノードの XML パスを $XPath 変数に保存します。

3 番目のコマンドは、**Select-Xml** コマンドレットを使用して、XPath ステートメントで特定された Types.ps1xml ファイルの AliasProperty ノードを取得します。
このコマンドは、パイプライン演算子を使用して、エイリアスプロパティノードを Select-Object コマンドレットに送信します。
*Expandproperty* パラメーターを指定すると、 **Node** オブジェクトが展開され、Name プロパティと referencedmembername プロパティが返されます。

結果には、Types.ps1xml ファイルの各エイリアス プロパティの Name と ReferencedMemberName が示されています。
たとえば、 **Length** プロパティのエイリアスである **Count** プロパティがあります。

### 例 2: XML ドキュメントを入力する

```
PS C:\> [xml]$Types = Get-Content $Pshome\Types.ps1xml
PS C:\> Select-Xml -Xml $Types -XPath "//MethodName"
```

この *例では、xml パラメーターを* 使用して、xml ドキュメントを xml **コマンドレット** に提供する方法を示します。

最初のコマンドは、Get-Content コマンドレットを使用して Types.ps1xml ファイルの内容を取得し、$Types 変数に保存します。
Xml は、 \[ \] 変数を xml オブジェクトとしてキャストします。

2 番目のコマンドは、**Select-Xml** コマンドレットを使用して、Types.ps1xml ファイルの MethodName ノードを取得します。
このコマンドは、*Xml* パラメーターを使用して $Types 変数内の XML コンテンツを指定し、*XPath* パラメーターを使用して MethodName ノードのパスを指定します。

### 例 3: PowerShell ヘルプファイルを検索する

```
PS C:\> $Namespace = @{command = "http://schemas.microsoft.com/maml/dev/command/2004/10"; maml = "http://schemas.microsoft.com/maml/2004/10"; dev = "http://schemas.microsoft.com/maml/dev/2004/10"}

The second command saves the path to the help files in the $Path variable.If there are no help files in this path on your computer, use the Update-Help cmdlet to download the help files. For more information about Updatable Help, see about_Updatable_Help (https://go.microsoft.com/fwlink/?LinkId=235801).
PS C:\> $Path = "$Pshome\en-us\*dll-Help.xml"

The third command uses the **Select-Xml** cmdlet to search the XML for cmdlet names by finding Command:Name element anywhere in the files. It saves the results in the $Xml variable.**Select-Xml** returns a **SelectXmlInfo** object that has a Node property, which is a **System.Xml.XmlElement** object. The Node property has an InnerXML property, which contains the actual XML that is retrieved.
PS C:\> $Xml = Select-Xml -Path $Path -Namespace $Namespace -XPath "//command:name"

The fourth command sends the XML in the $Xml variable to the Format-Table cmdlet. The **Format-Table** command uses a calculated property to get the Node.InnerXML property of each object in the $Xml variable, trim the white space before and after the text, and display it in the table, along with the path to the source file.
PS C:\> $Xml | Format-Table @{Label="Name"; Expression= {($_.node.innerxml).trim()}}, Path -AutoSize

Name                    Path
----                    ----
Export-Counter          C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Get-Counter             C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Get-WinEvent            C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Import-Counter          C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Diagnostics.dll-Help.xml
Add-Computer            C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Management.dll-Help.xml
Add-Content             C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Management.dll-Help.xml
Checkpoint-Computer     C:\Windows\system32\WindowsPowerShell\v1.0\en-us\Microsoft.PowerShell.Commands.Management.dll-Help.xml
...
```

この **例では、xml コマンドレット** を使用して、POWERSHELL の xml ベースのコマンドレットヘルプファイルを検索する方法を示します。
各ヘルプ ファイルのタイトルとなるコマンドレット名とヘルプ ファイルのパスを検索します。

最初のコマンドは、ヘルプ ファイルに使用されている XML 名前空間を表すハッシュ テーブルを作成し、$Namespace 変数に保存します。

### 例 4: XML を入力するさまざまな方法

```
PS C:\> $Xml = @"
<?xml version="1.0" encoding="utf-8"?>
<Book>
  <projects>
    <project name="Book1" date="2009-01-20">
      <editions>
        <edition language="English">En.Book1.com</edition>
        <edition language="German">Ge.Book1.Com</edition>
        <edition language="French">Fr.Book1.com</edition>
        <edition language="Polish">Pl.Book1.com</edition>
      </editions>
    </project>
  </projects>
</Book>
"@

The second command uses the *Content* parameter of **Select-Xml** to specify the XML in the $Xml variable.
PS C:\> Select-Xml -Content $Xml -XPath "//edition" | foreach {$_.node.InnerXML}

En.Book1.com
Ge.Book1.Com
Fr.Book1.com
Pl.Book1.com

The third command is equivalent to the second. It uses a pipeline operator (|) to send the XML in the $Xml variable to the **Select-Xml** cmdlet.
PS C:\> $Xml | Select-Xml -XPath "//edition" | foreach {$_.node.InnerXML}

En.Book1.com
Ge.Book1.Com
Fr.Book1.com
Pl.Book1.com
```

この例では、2つの異なる方法で XML を **選択して xml** コマンドレットに送信します。

最初のコマンドは、XML を含む文字列を $Xml 変数に保存します。
(ヒア文字列の詳細については、「about_Quoting_Rules」を参照してください)。

### 例 5: 既定の xmlns 名前空間を使用する

```
PS C:\> $SnippetNamespace = @{snip = "http://schemas.microsoft.com/PowerShell/Snippets"}

The second command uses the **Select-Xml** cmdlet to get the content of the Title element of each snippet. It uses the *Path* parameter to specify the Snippets directory and the *Namespace* parameter to specify the namespace in the $SnippetNamespace variable. The value of the *XPath* parameter is the "snip" namespace key, a colon (:), and the name of the Title element.The command uses a pipeline operator (|) to send each **Node** property that **Select-Xml** returns to the ForEach-Object cmdlet, which gets the title in the value of the **InnerXml** property of the node.
PS C:\> Select-Xml -Path $Home\Documents\WindowsPowerShell\Snippets -Namespace $SnippetNamespace -XPath "//snip:Title" | foreach {$_.Node.Innerxml}
```

この例では、既定の xmlns 名前空間を使用する XML ドキュメントで、 **xml** コマンドレットを使用する方法を示します。
例として、ユーザーが作成した Windows PowerShell ISE のスニペット ファイルのタイトルを取得します。
スニペットの詳細については、「New-IseSnippet」を参照してください。

最初のコマンドは、スニペット XML ファイルが使用する既定の名前空間のハッシュテーブルを作成し、$SnippetNamespace 変数に割り当てます。
ハッシュ テーブルの値は、スニペット XML の XMLNS スキーマ URI です。
ハッシュテーブルのキー名 (切り取り) は任意です。
予約されていない任意の名前を使用できますが、xmlns は使用できません。

## PARAMETERS

### -コンテンツ

検索対象の XML を含む文字列を指定します。
パイプを使用して文字列を **選択** することもできます。

```yaml
Type: System.String[]
Parameter Sets: Content
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

検索対象の XML ファイルのパスとファイル名を指定します。
*Path* と異なり、*LiteralPath* パラメーターの値は入力したとおりに使用されます。
ワイルドカードとして解釈される文字はありません。
パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。
単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Namespace

XML で使用されている名前空間のハッシュ テーブルを指定します。
@ {} の形式を使用し \<namespaceName\>  =  \<namespaceValue\> ます。

XML で既定の名前空間 (xmlns で始まる) を使用する場合は、名前空間名に任意のキーを使用します。
Xmlns は使用できません。
XPath ステートメントで、各ノード名の前に、名前空間の名前とコロン ((例: Node など) を付けます。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

検索対象の XML ファイルのパスとファイル名を指定します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Xml

1 つ以上の XML ノードを指定します。

XML ドキュメントは、XML ノードのコレクションとして処理されます。
パイプを使用 **し** て xml ドキュメントを xml にパイプする場合、各ドキュメントノードは、パイプラインを介して取得されるたびに個別に検索されます。

```yaml
Type: System.Xml.XmlNode[]
Parameter Sets: Xml
Aliases: Node

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -XPath

XPath 検索クエリを指定します。
クエリ言語では、大文字と小文字が区別されます。
このパラメーターは必須です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.string または System.Xml.Xmlノード

パイプを使用してパスまたは XML ノードをこのコマンドレットに渡します。

## 出力

### Microsoft. PowerShell. SelectXmlInfo

## 注

XPath は、XML ドキュメントの一部分を特定するために設計された標準言語です。 XPath 言語の詳細については、「 [Xpath リファレンス](/dotnet/standard/data/xml/select-nodes-using-xpath-navigation) 」および「イベントの [選択](/previous-versions//aa385231(v=vs.85))」の選択フィルターに関するセクションを参照してください。

## 関連リンク

[ConvertTo-Xml](ConvertTo-Xml.md)
