---
title: PowerShell ドキュメントのスタイル ガイド
description: この記事では、PowerShell ドキュメントを記述するためのスタイル規則について説明します。
ms.date: 12/09/2020
ms.topic: conceptual
ms.openlocfilehash: 6f23f63cffc9fed9cbbcf84539875bfaf4247732
ms.sourcegitcommit: 61765d08321623743dc5db5367160f6982fe7857
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2020
ms.locfileid: "99599278"
---
# <a name="powershell-docs-style-guide"></a>PowerShell ドキュメントのスタイル ガイド

この記事では、PowerShell ドキュメントのコンテンツに固有のスタイル ガイダンスについて説明します。 [概要](overview.md#get-started-writing-docs)に記載されている情報に基づいています。

## <a name="product-terminology"></a>製品に関する用語

PowerShell にはいくつかのバリエーションがあります。

- **PowerShell** - これが既定値です。 PowerShell 7 以降を使用することをお勧めします。
- **PowerShell Core** - .NET Core 上で構築される PowerShell です。 **コア** という用語の使用は、Windows PowerShell と区別する必要がある場合に限定する必要があります。
- **Windows PowerShell** - .NET Framework 上で構築される PowerShell です。 Windows PowerShell は Windows にのみ付属しており、完全な Framework が必要です。

  一般に、ドキュメント内の "Windows PowerShell" への参照は _PowerShell_ に変更できます。
  _Windows powershell_ 固有の動作について説明する場合は、"windows powershell" を使用する必要があります。

関連製品

- **Visual Studio Code (VS Code)** -これは Microsoft の無料のオープンソースエディターです。 最初のメンションでは、完全な名前を使用する必要があります。 その後で、**VS Code** を使用できます。 "VSCode" は使用しないでください。
- **Visual Studio Code 用の PowerShell 拡張機能** - この拡張機能によって VS Code が PowerShell 用の推奨 IDE に切り替えられます。 最初のメンションでは、完全な名前を使用する必要があります。 その後で、**PowerShell 拡張機能** を使用できます。
- **Azure PowerShell** - これは、Azure サービスを管理するために使用される PowerShell モジュールのコレクションです。
- **Azure Stack PowerShell** - これは、Microsoft のハイブリッド クラウド ソリューションを管理するために使用される PowerShell モジュールのコレクションです。

## <a name="markdown-specifics"></a>Markdown の詳細

Microsoft のドキュメントを構築する Microsoft Open Publishing System (OPS) では、[markdig][] を使用して Markdown ドキュメントが処理されます。 markdig では、最新の [CommonMark][] 仕様の規則に基づいてドキュメントが解析されます。

新しい CommonMark 仕様では、いくつかの Markdown 要素の構築に関する規定が非常に厳密になっています。 このドキュメントの細部に、細心の注意を払ってください。

### <a name="blank-lines-spaces-and-tabs"></a>空白行、スペース、およびタブ

空白行は、Markdown 内のブロックが終わることも示します。 異なる種類の Markdown ブロックの間 (段落とリストの間や段落とヘッダーの間など) には、単一の空白行が必要です。

HTML では、連続した複数の空白行が1つの空白行として表示されます。 目的はありません。
コードブロック内では、連続する空白行によってコードブロックが分割されます。

行の末尾の余分なスペースを削除してください。

> [!NOTE]
> Markdown では、スペースが重要です。 常にハード タブではなくスペースを使用してください。 Markdown のレンダリング方法は、末尾のスペースによって変更できます。

### <a name="titles-and-headings"></a>タイトルと見出し

[ATX 見出し][atx]のみを使用します (`=` または `-` スタイルのヘッダーではなく # スタイルにします)。

- 文の先頭で大文字を使用する - 固有名詞、タイトル、ヘッダーの 1 文字目のみ大文字にします
- `#` と見出しの最初の文字の間には単一のスペースが必要
- 見出しの上下は単一の空白行にする
- ドキュメントごとの H1 は 1 つのみにする
- ヘッダーのレベルは 1 つずつ増分する。 レベルをスキップしない
- ヘッダーで太字またはその他のマークアップを使用しない

### <a name="limit-line-length-to-100-characters"></a>行の長さを 100 文字に制限する

これは、概念記事とコマンドレット リファレンスに適用されます。 行の長さを制限することで、git diff と履歴が読みやすくなります。 さらに、他の執筆者が投稿を簡単に行えるようにします。

Visual Studio Code の [Reflow Markdown][reflow] 拡張機能を使用すると、規定の行の長さに合うように段落が簡単にリフロ―されます。

About_topics の長さは 80 文字に制限されます。 詳細については、「 [About_ ファイルの書式設定](#formatting-about_-files)」を参照してください。

### <a name="lists"></a>リスト

リストに複数の文または段落が含まれている場合は、リストではなくサブレベルヘッダーを使用することを検討してください。

リストの上下は単一の空白行にします。

#### <a name="unordered-lists"></a>箇条書きリスト

複数の文が含まれている場合を除き、ピリオドでリスト項目を終了しないでください。 リスト項目の箇条書き記号には、ハイフン文字 (`-`) を使用します。 これにより、アスタリスク [`*`] を使用する、太字または斜体のマークアップとの混同を回避できます。 段落またはその他の要素を箇条書き項目の下に含めるには、改行を挿入し、インデントを箇条書き記号の後ろの最初の文字に揃えます。

次に例を示します。

```markdown
This is a list that contain child elements under a bullet item.

- First bullet item

  Sentence explaining the first bullet.

  - Child bullet item

    Sentence explaining the child bullet.

- Second bullet item
- Third bullet item
```

これは箇条書き項目の下にある子要素を含むリストです。

- 最初の箇条書き項目

  最初の箇条書きの内容文。

  - 子の箇条書き項目

    子の箇条書きを説明する文。

- 2 番目の箇条書き項目
- 3 番目の箇条書き項目

#### <a name="ordered-lists"></a>番号付きリスト

段落またはその他の要素を番号付き項目の下に含めるには、インデントを項目番号の後ろの最初の文字に揃えます。 番号付きリストのすべての項目では、項目ごとに増分される番号ではなく、番号 `1.` を使用する必要があります。 この値は、Markdown によって自動的に増分されて表示されます。 これにより、項目を簡単に並べ替えることができ、下位コンテンツのインデントが統一されます。

次に例を示します。

```markdown
1. For the first element, insert a single space after the 1. Long sentences should be
   wrapped to the next line and must line up with the first character after the numbered
   list marker.

   To include a second element (like this one), insert a line break after the first
   and align indentations. The indentation of the second element must line up with
   the first character after the numbered list marker. For single digit items, like
   this one, you indent to column 4. For double digits items, for example item number
   10, you indent to column 5.

1. The next numbered item starts here.
```

結果の Markdown は次のように表示されます。

1. 最初の要素では、1. の後ろに単一のスペースを挿入します。 長い文は次の行に折り返す必要があり、次の行の先頭は番号付きリスト マーカーの後ろの最初の文字と揃える必要があります。

   (この例のように) 2 つ目の要素を含めるには、1 つ目の要素の後に改行を挿入し、インデントを合わせます。 2 つ目の要素のインデントは、番号付きリスト マーカーの後ろの最初の文字と揃える必要があります。 このような 1 桁の番号の項目では、列 4 にインデントします。 2 桁の番号の項目 (項目番号 10 など) では、列 5 にインデントします。

1. 次の番号付き項目は、ここから始まります。

### <a name="images"></a>イメージ

画像を含める構文は次のとおりです。

```markdown
![[alt text]](<folderPath>)

Example:
![Introduction](./media/overview/Introduction.png)
```

`alt text` は画像の簡単な説明であり、`<folder path>` は画像の相対パスです。 代替テキストは、視覚障碍がある読者のために必要です。

画像は `media/<article-name>` 、アーティクルを含むフォルダー内のフォルダーに格納されている必要があります。
記事間で画像を共有しないでください。 `media` フォルダーの下に、記事のファイル名と一致するフォルダーを作成します。 その記事のイメージをその新しいフォルダーにコピーします。 複数の記事でイメージが使用されている場合は、各イメージ フォルダーにそのイメージ ファイルのコピーを含める必要があります。 これにより、ある記事でイメージを変更しても、別の記事に影響を与えることがなくなります。

サポートされているイメージファイルの種類は、、、、 `*.png` `*.gif` `*.jpeg` `*.jpg` 、です。 `*.svg`

### <a name="markdown-extensions-supported-by-open-publishing"></a>Open Publishing でサポートされる Markdown の拡張機能

[Microsoft Docs Authoring Pack](/contribute/how-to-write-docs-auth-pack)には、公開システム固有の機能をサポートするツールが含まれています。 アラートは、コンテンツの有意性を強調する色とアイコンを使用して docs.microsoft.com に表示するブロック引用符を作成するための Markdown 拡張機能です。 次の種類のアラートがサポートされています。

```markdown
> [!NOTE]
> Information the user should notice even if skimming.

> [!TIP]
> Optional information to help a user be more successful.

> [!IMPORTANT]
> Essential information required for user success.

> [!CAUTION]
> Negative potential consequences of an action.

> [!WARNING]
> Dangerous certain consequences of an action.
```

これらのアラートは docs.microsoft.com 上で次のように表示されます。

メモブロック

> [!NOTE]
> Information the user should notice even if skimming.

ヒントブロック

> [!TIP]
> Optional information to help a user be more successful.

重要なブロック

> [!IMPORTANT]
> Essential information required for user success.

注意ブロック

> [!CAUTION]
> Negative potential consequences of an action.

警告ブロック

> [!WARNING]
> Dangerous certain consequences of an action. (ある行動で起こるいくつかの危険な結果。)

### <a name="hyperlinks"></a>ハイパーリンク

- ハイパーリンクには Markdown 構文を使用する必要があり `[friendlyname](url-or-path)` ます。 [リンク参照][linkref] がサポートされています。
- 公開システムでは、次の3種類のリンクがサポートされています。
  - URL リンク
  - ファイルリンク
  - 相互参照リンク
- Docs.microsoft.com の他の記事へのリンクは、サイト相対パスである必要があります。
- 外部 web サイトへのすべての Url は、ターゲットサイトで有効でない場合を除き、HTTPS を使用する必要があります。
- リンクにはフレンドリ名が必要です。通常はリンクアーティクルのタイトルです
- 下部にある [ _関連リンク_ ] セクションのすべての項目はハイパーリンクにする必要があります。
- ハイパーリンクの角かっこ内には、バックティック、太字、またはその他のマークアップを使用しないでください。
- 特定の URI をドキュメント化するときに、ベア Url を使用できます。 URI は、バックティックで囲む必要があります。 次に例を示します。

  ```markdown
  By default, if you don't specify this parameter, the DMTF standard resource URI
  `http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/` is used and the class name is appended to it.
  ```

#### <a name="linking-to-other-content-on-docsmicrosoftcom"></a>Docs.microsoft.com の他のコンテンツへのリンク

- Docs.microsoft.com の他の記事への **URL リンク** は、サイト相対パスである必要があります。 相対リンクを作成する最も簡単な方法は、ブラウザーから URL をコピーし、 `https://docs.microsoft.com/en-us` markdown に貼り付ける値から削除することです。 Microsoft のプロパティの Url にロケールを含めないで `/en-us` ください (url から削除)。 URL から不要なクエリパラメーターを削除します。 削除する必要がある例を次に示します。

  - `?view=powershell-5.1` -PowerShell の特定のバージョンへのリンクに使用されます。
  - `?redirectedfrom=MSDN` -古い記事から新しい場所にリダイレクトされるときに URL に追加されます。

  ドキュメントの特定のバージョンにリンクする必要がある場合は、クエリ文字列にパラメーターを追加する必要があり `&preserve_view=true` ます。 例: `?view=powershell-5.1&preserve_view=true`

- **ファイルリンク** は、ある参照記事から別の参照記事へのリンク、またはある概念記事から別の参照アーティクルへのリンクに使用されます。 PowerShell の特定のバージョンに関する参照記事にリンクする必要がある場合は、URL リンクを使用する必要があります。

  - ファイルリンクには、相対ファイルパスが含まれています (例: `../folder/file.md` )。
  - すべてのファイルパスはスラッシュ () 文字を使用します `/`

- **相互参照リンク** は、公開システムによってサポートされる特別な機能です。 概念説明の記事で相互参照リンクを使用して、.NET API またはコマンドレットリファレンスにリンクすることができます。

  .NET API リファレンスへのリンクについては、中央の共同作成者ガイドの [ドキュメントでリンクを使用する方法](/contribute/how-to-write-links#xref-cross-reference-links) に関するページを参照してください。

  コマンドレットリファレンスへのリンクの形式は次のとおり `xref:<module-name>.<cmdlet-name>` です。 たとえば、を使用して、 `Get-Content` **管理** モジュールのコマンドレットにリンクします。

  `[Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)`

ディープリンクは、URL リンクとファイルリンクの両方で使用できます。 アンカーをターゲットパスの末尾に追加します。
次に例を示します。

- `[about_Splatting](about_Splatting.md#splatting-with-arrays)`
- `[custom key bindings](https://code.visualstudio.com/docs/getstarted/keybindings#_custom-keybindings-for-refactorings)`

詳細については、 [ドキュメントの「リンクを使用する](/contribute/how-to-write-links)」を参照してください。

## <a name="formatting-command-syntax-elements"></a>コマンド構文要素の書式設定

- コマンドレットとパラメーターには常に完全名を使用します。 別名を明示的に示す場合を除き、エイリアスの使用は避けてください。

- プロパティ、パラメーター、オブジェクト、型名、クラス名、クラスメソッドは **太字** にする必要があります。
  - プロパティとパラメーターの値は、バックティック () でラップする必要があり `` ` `` ます。
  - かっこで囲まれたスタイルを使用して型を参照する場合は、backticks を使用します。 例: `[System.Io.FileInfo]`

- 言語キーワード、コマンドレット名、関数、変数、ネイティブ Exe、ファイルパス、およびインライン構文の例は、バックティック () 文字でラップする必要があり `` ` `` ます。

  次に例を示します。

  ~~~markdown
  The following code uses `Get-ChildItem` to list the contents of `C:\Windows` and assigns
  the output to the `$files` variable.

  ```powershell
  $files = Get-ChildItem C:\Windows
  ```
  ~~~

  - 名前でパラメーターを参照する場合、名前は **太字** にする必要があります。 パラメーターをハイフン プレフィックスと共に使用する場合は、パラメーターをバッククォートで囲む必要があります。 次に例を示します。

    ```markdown
    The parameter's name is **Name**, but it's typed as `-Name` when used on the command
    line as a parameter.
    ```

  - 外部コマンドの使用例を示す場合は、例をバッククォートで囲む必要があります。
    ネイティブコマンドには、常にファイル拡張子を含めます。 次に例を示します。

    ```markdown
    To start the spooler service on a remote computer named DC01, you type `sc.exe \\DC01 start spooler`.
    ```

    ファイル拡張子を含めることで、PowerShell のコマンドの優先順位に従って正しいコマンドが実行されるようになります。

- 概念説明の記事 (リファレンス コンテンツではなく) を記述する場合は、コマンドレット名の最初のインスタンスをコマンドレットのドキュメントにハイパーリンクする必要があります。 ハイパーリンクの角かっこ内には、バックティック、太字、またはその他のマークアップを使用しないでください。

  次に例を示します。

  ```markdown
  This [Write-Host](/powershell/module/Microsoft.PowerShell.Utility/Write-Host) cmdlet
  uses the **Object** parameter to ...
  ```

  詳細については、この記事の「 [ハイパーリンク](#hyperlinks) 」セクションを参照してください。

## <a name="markdown-for-code-samples"></a>コードサンプルの Markdown

Markdown は、次の 2 つの異なるコード スタイルをサポートしています。

- **コードの span (inline)** -1 つのバックティック ( `` ` `` ) 文字でマークされます。 スタンドアロンブロックとしてではなく、段落内で使用されます。
- **コードブロック** -トリプルバックティック () 文字列で囲まれた複数行のブロック `` ``` `` 。 コードブロックには、バックティックの後に言語ラベルを付けることもできます。 言語ラベルを使用すると、コードブロックの内容に対して構文の強調表示が有効になります。

すべてのコード ブロックは、コード フェンス内に含めるようにします。 コードブロックにはインデントを使用しないでください。 Markdown ではこのパターンを使用できますが、問題になる可能性があるため、回避する必要があります。

コードブロックは、3つのバックティック () コードフェンスで囲まれた1行以上のコードです `` ``` `` 。
コード フェンス マーカーは、コード サンプルの前後にある独自の行に配置する必要があります。 コード ブロックの先頭にあるマーカーには、省略可能な言語ラベルを含めることができます。 Microsoft の Open Publishing System (OPS) では、言語ラベルを使用して、構文の強調表示機能をサポートしています。

サポートされている言語タグの完全な一覧については、共同作成者ガイドの「 [たジオフェンス型 code block](/contribute/code-in-docs#fenced-code-blocks) 」を参照してください。

また、OPS では、コード ブロックの内容をクリップボードにコピーする **[コピー]** ボタンも追加されます。 これにより、コードを簡単にスクリプトに貼り付けて、コードサンプルをテストすることができます。 ただし、このドキュメントに記載されているすべての例は、そのとおりに実行するためのものではありません。 一部のコードブロックは、PowerShell の概念を簡単に図解したものです。

ドキュメントでは、3種類のコードブロックが使用されています。

1. 構文ブロック
1. 説明用の例
1. 実行可能ファイルの例

### <a name="syntax-code-blocks"></a>構文コードブロック

構文コードブロックは、コマンドの構文構造を記述するために使用されます。 コードフェンスに言語タグを使用しないでください。 この例は、`Get-Command` コマンドレットのすべての使用できるパラメーターを示しています。

~~~markdown
```
Get-Command [-Verb <String[]>] [-Noun <String[]>] [-Module <String[]>]
  [-FullyQualifiedModule <ModuleSpecification[]>] [-TotalCount <Int32>] [-Syntax]
  [-ShowCommandInfo] [[-ArgumentList] <Object[]>] [-All] [-ListImported]
  [-ParameterName <String[]>] [-ParameterType <PSTypeName[]>] [<CommonParameters>]
```
~~~

この例では、`for` ステートメントを汎用的な用語で説明します。

~~~markdown
```
for (<init>; <condition>; <repeat>)
{<statement list>}
```
~~~

### <a name="illustrative-examples"></a>わかりやすい例

わかりやすい例は、PowerShell の概念を説明するために使用されます。 これらは、実行のためにクリップボードにコピーされることを意図していません。 これらは、簡単に型を簡単に理解できる単純な例に使用されます。 コードブロックには、PowerShell プロンプトと出力例を含めることができます。

次に、PowerShell の比較演算子の簡単な例を示します。 このケースでは、閲覧者がこの例をコピーして実行することは想定していません。

~~~markdown
```powershell
PS> 2 -eq 2
True

PS> 2 -eq 3
False

PS> 1,2,3 -eq 2
2

PS> "abc" -eq "abc"
True

PS> "abc" -eq "abc", "def"
False

PS> "abc", "def" -eq "abc"
abc
```
~~~

### <a name="executable-examples"></a>実行可能ファイルの例

複雑な例、またはコピーして実行することを意図した例では、次のブロックスタイルのマークアップを使用する必要があります。

~~~markdown
```powershell
<Your PowerShell code goes here>
```
~~~

PowerShell コマンドで表示される出力は、構文の強調表示を防ぐために **出力** コード ブロックで囲む必要があります。 次に例を示します。

~~~markdown
```powershell
Get-Command -Module Microsoft.PowerShell.Security
```

```Output
CommandType  Name                        Version    Source
-----------  ----                        -------    ------
Cmdlet       ConvertFrom-SecureString    3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       ConvertTo-SecureString      3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Get-Acl                     3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Get-AuthenticodeSignature   3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Get-CmsMessage              3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Get-Credential              3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Get-ExecutionPolicy         3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Get-PfxCertificate          3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       New-FileCatalog             3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Protect-CmsMessage          3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Set-Acl                     3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Set-AuthenticodeSignature   3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Set-ExecutionPolicy         3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Test-FileCatalog            3.0.0.0    Microsoft.PowerShell.Security
Cmdlet       Unprotect-CmsMessage        3.0.0.0    Microsoft.PowerShell.Security
```
~~~

**出力** コードラベルは、構文の強調表示システムでサポートされている公式の "言語" ではありません。
ただし、このラベルは、OPS によって web ページ上のコードボックスフレームに "出力" ラベルが追加されるため便利です。 "出力" コードボックスには構文の強調表示がありません。

## <a name="coding-style-rules"></a>コーディング スタイルの規則

### <a name="avoid-line-continuation-in-code-samples"></a>コード サンプルでの行の継続を避ける

PowerShell コード例では、行連結文字 (`` ` ``) を使用しないでください。 これらはわかりにくいため、行末に余分なスペースがあると問題が発生する可能性があります。

- PowerShell スプラッティングを使用して、複数のパラメーターを持つコマンドレットの行の長さを減らします。
- パイプ ( `|` ) 文字、左中かっこ ( `{` )、かっこ () `(` 、および角かっこ () のように、PowerShell の自然な改行の機会を活用し `[` ます。

### <a name="avoid-using-powershell-prompts-in-examples"></a>例で PowerShell プロンプトを使用しないようにする

プロンプト文字列の使用は推奨されていないため、コマンドラインの使用方法を示すシナリオに限定する必要があります。 これらの例の大部分では、プロンプト文字列をにする必要があり `PS>` ます。 このプロンプトは OS 固有のインジケーターに依存しません。

プロンプトを変更するコマンドや、表示されるパスがシナリオにとって重要であることを示すコマンドを示す例では、プロンプトが必要です。 次の例は、レジストリ プロバイダーの使用時にプロンプトがどのように変化するかを示しています。

```powershell
PS C:\> cd HKCU:\System\
PS HKCU:\System\> dir

    Hive: HKEY_CURRENT_USER\System

Name                   Property
----                   --------
CurrentControlSet
GameConfigStore        GameDVR_Enabled                       : 1
                       GameDVR_FSEBehaviorMode               : 2
                       Win32_AutoGameModeDefaultProfile      : {2, 0, 1, 0...}
                       Win32_GameModeRelatedProcesses        : {1, 0, 1, 0...}
                       GameDVR_HonorUserFSEBehaviorMode      : 0
                       GameDVR_DXGIHonorFSEWindowsCompatible : 0
```

### <a name="dont-use-aliases-in-examples"></a>例では別名を使用しない

エイリアスを特に記述している場合を除き、すべてのコマンドレットとパラメーターの完全な名前を使用します。
コマンドレットとパラメーター名には、 [Pascal][] 形式の正しい名前を使用する必要があります。

### <a name="using-parameters-in-examples"></a>例でのパラメーターの使用

位置指定パラメーターは使用しないでください。 一般に、パラメーターが位置指定の場合でも、パラメーター名は常に例に含める必要があります。 これにより、例で混乱を招く可能性が減ります。

## <a name="formatting-cmdlet-reference-articles"></a>コマンドレットのリファレンス記事の書式設定

コマンドレットのリファレンス記事には、特定の構造があります。 この構造は、[PlatyPS][] によって定義されています。
エラーでは、Markdown の PowerShell モジュールのコマンドレットヘルプが生成されます。 マークダウン ファイルを編集した後、PlatyPS を使用して、`Get-Help` コマンドレットによって使用される MAML ヘルプ ファイルを作成します。

PlatyPS には、コード内に記述されるコマンドレット リファレンス用のハードコーディングされたスキーマが含まれます。 [platyPS.schema.md][] ドキュメントでは、この構造が記述されています。 スキーマ違反があると、投稿を受け入れる前に修正する必要があるビルド エラーが発生します。

- ATX ヘッダー構造体は削除しないでください。 PlatyPS では、特定のヘッダーのセットが想定されています。
- **Input type** ヘッダーと **Output type** ヘッダーでは、型が必要です。 コマンドレットが入力を受け取らない場合、または値を返さない場合は、値を使用し `None` ます。
- インライン コード スパンは、どのパラグラフでも使用できます。
- たジオフェンス型コードブロックは、「 **例** 」のセクションでのみ許可されています。

エラースキーマでは、 **例** は H2 ヘッダーです。 各例は、H3 ヘッダーです。 例では、スキーマでコードブロックを段落で区切ることはできません。 スキーマでは、次の構造を使用できます。

```
### Example X - Title sentence

0 or more paragraphs
1 or more code blocks
0 or more paragraphs.
```

各例に番号を指定し、簡単なタイトルを追加します。

次に例を示します。

~~~markdown
### Example 1: Get cmdlets, functions, and aliases

This command gets the PowerShell cmdlets, functions, and aliases that are installed on the
computer.

```powershell
Get-Command
```

### Example 2: Get commands in the current session

```powershell
Get-Command -ListImported
```
~~~

## <a name="formatting-about_-files"></a>About_ ファイルの書式設定

`About_*` ファイルは Markdown で記述されますが、プレーンテキストファイルとして出荷されます。 [Pandoc][]を使用して、Markdown をプレーンテキストに変換します。 `About_*` ファイルは、すべてのバージョンの PowerShell と発行ツールで最適な互換性を持つように書式設定されています。

基本的な書式設定に関するガイドライン:

- 通常の行を80文字に制限する
- コードブロックの文字数は76文字に制限されています
- ブロックの引用符 (およびアラート) は78文字に制限されています
- これらの特殊なメタ文字 (、、および) を使用する場合は、次のように `\` `$` `<` なります。
  - ヘッダー内では、これらの文字は先頭文字を使用してエスケープする `\` か、バックティック () を使用してコード範囲で囲んでいる必要があります `` ` `` 。
  - 段落内では、これらの文字をコードの範囲内に配置する必要があります。 次に例を示します。

    ~~~markdown
    ### The purpose of the \$foo variable

    The `$foo` variable is used to store ...
    ~~~

- テーブルは76文字以内に収める必要があります
  - 複数行にまたがるセルの内容は手動で折り返します
  - 各行で開始と終了の `|` 文字を使用します
  - 次の例では、セル内の複数の行に折り返される情報を含むテーブルを適切に構築する方法を示します。

    ~~~markdown
    ```
    |Operator|Description                |Example                          |
    |--------|---------------------------|---------------------------------|
    |`-is`   |Returns TRUE when the input|`(get-date) -is [DateTime]`      |
    |        |is an instance of the      |`True`                           |
    |        |specified .NET type.       |                                 |
    |`-isNot`|Returns TRUE when the input|`(get-date) -isNot [DateTime]`   |
    |        |not an instance of the     |`False`                          |
    |        |specified.NET type.        |                                 |
    |`-as`   |Converts the input to the  |`"5/7/07" -as [DateTime]`        |
    |        |specified .NET type.       |`Monday, May 7, 2007 12:00:00 AM`|
    ```
    ~~~

    > [!NOTE]
    > 76列の制限は、トピックにのみ適用さ `About_*` れます。 ワイド列は、概念説明またはコマンドレットのリファレンス記事で使用できます。

## <a name="next-steps"></a>次のステップ

[編集のチェックリスト](editorial-checklist.md)

<!-- link references -->
[atx]: https://github.github.com/gfm/#atx-headings
[CommonMark]: https://spec.commonmark.org/
[markdig]: https://github.com/lunet-io/markdig
[Pandoc]: https://pandoc.org
[Pascal 形式の大文字と小文字の区別]: https://en.wikipedia.org/wiki/PascalCase
[platyPS.schema.md]: https://github.com/PowerShell/platyPS/blob/master/platyPS.schema.md
[PlatyPS]: https://github.com/powershell/platyps
[reflow]: https://marketplace.visualstudio.com/items?itemName=marvhen.reflow-markdown
[linkref]: https://spec.commonmark.org/0.29/#link-reference-definitions
