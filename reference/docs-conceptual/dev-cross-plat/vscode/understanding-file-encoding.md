---
title: VS Code と PowerShell でのファイルのエンコードの概要
description: VS Code と PowerShell でのファイルのエンコードの構成
ms.date: 02/28/2019
ms.openlocfilehash: afad189ff20a4e73d25f15c48d6c4982b18f29a3
ms.sourcegitcommit: 196c7f8cd24560cac70c88acc89909f17a86aea9
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2020
ms.locfileid: "93142550"
---
# <a name="understanding-file-encoding-in-vs-code-and-powershell"></a>VS Code と PowerShell でのファイルのエンコードの概要

VS Code を使用して PowerShell スクリプトを作成および編集するときは、正しい文字エンコード形式を使用してファイルを保存することが重要です。

## <a name="what-is-file-encoding-and-why-is-it-important"></a>ファイルのエンコードの概要とそれが重要な理由

VS Code により、人間が文字列をバッファーに入力することと、バイトのブロックをファイル システムに読み取りおよび書き込みすることの間のインターフェイスが管理されます。 VS Code では、ファイルを保存するとき、テキスト エンコードを使用して各文字がどのバイトになるかが判断されます。 詳細については、「[about_Character_Encoding](/powershell/module/microsoft.powershell.core/about/about_character_encoding)」をご覧ください。

同様に、PowerShell によりスクリプトが実行されるときは、ファイルを PowerShell プログラムに再構築するために、ファイル内のバイトを文字に変換する必要があります。 VS Code によりファイルが書き込まれ、PowerShell によりファイルが読み取られるため、これらでは同じエンコード システムを使用する必要があります。 PowerShell スクリプトを解析するこのプロセスは、 _バイト_ -> _文字_ -> _トークン_ -> _抽象構文木_ -> _実行_ の順に行われます。

VS Code と PowerShell は両方とも、実用的な既定のエンコード構成でインストールされています。 ただし、PowerShell により使用される既定のエンコードは、PowerShell Core (v6.x) のリリースで変わりました。 VS Code で PowerShell または PowerShell 拡張機能を使用しても確実に問題がないようにするには、VS Code と PowerShell の設定を正しく構成する必要があります。

## <a name="common-causes-of-encoding-issues"></a>エンコード問題の一般的な原因

エンコードの問題は、VS Code またはスクリプト ファイルのエンコードが、想定される PowerShell のエンコードと一致しない場合に発生します。 PowerShell でファイルのエンコードが自動的に決定される方法はありません。

[7 ビット ASCII 文字セット](https://ascii.cl/)以外の文字を使用している場合は、エンコードの問題が発生する可能性が高くなります。 次に例を示します。

<!-- markdownlint-disable MD038 -->
- em ダッシュ (`—`)、改行なしスペース (` `)、左二重引用符 (`"`) などのアルファベット以外の拡張文字
- アクセント付きラテン文字 (`É`、`ü`)
- キリル文字などの非ラテン文字 (`Д`、`Ц`)
- CJK 文字 (`本`、`화`、`が`)

エンコードの問題の一般的な理由は次のとおりです。

- VS Code と PowerShell のエンコードが既定値から変更されていません。 PowerShell 5.1 以下では、既定のエンコードは VS Code とは異なります。
- 別のエディターでファイルが開かれ、新しいエンコードで上書きされました。 多くの場合、これは ISE で発生します。
- VS Code または PowerShell で想定されているエンコードとは異なるエンコードでファイルがソース管理にチェックインされています。 これは、共同作業者が異なるエンコード構成のエディターを使用している場合に発生する可能性があります。

### <a name="how-to-tell-when-you-have-encoding-issues"></a>エンコードの問題が発生したときの判断方法

多くの場合、エンコード エラーはスクリプト内の解析エラーとして現れます。 スクリプト内に通常とは異なる文字シーケンスが見つかった場合は、それが問題になる可能性があります。 以下の例では、半角ダッシュ (`–`) が `â&euro;"` という文字で表示されています。

```Output
Send-MailMessage : A positional parameter cannot be found that accepts argument 'Testing FuseMail SMTP...'.
At C:\Users\<User>\<OneDrive>\Development\PowerShell\Scripts\Send-EmailUsingSmtpRelay.ps1:6 char:1
+ Send-MailMessage â&euro;"From $from â&euro;"To $recipient1 â&euro;"Subject $subject  ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidArgument: (:) [Send-MailMessage], ParameterBindingException
    + FullyQualifiedErrorId : PositionalParameterNotFound,Microsoft.PowerShell.Commands.SendMailMessage
```

この問題は、VS Code により UTF-8 の文字 `–` がバイト `0xE2 0x80 0x93` としてエンコードされるために発生します。 このようなバイトが Windows-1252 としてデコードされると、`â&euro;"` という文字に解釈されます。

よく見られる通常とは異なる文字シーケンスの例を次に示します。

<!-- markdownlint-disable MD038 -->
- `–` の代わりに `â&euro;"`
- `—` の代わりに `â&euro;"`
- `Ä` の代わりに `Ã„2`
- ` ` の代わりに `Â` (改行なしスペース)
- `é` の代わりに `Ã&copy;`
<!-- markdownlint-enable MD038 -->

こちらの便利な[関連ドキュメント](https://www.i18nqa.com/debug/utf8-debug.html)には、UTF-8/Windows-1252 エンコードの問題を示す一般的なパターンの一覧が掲載されています。

## <a name="how-the-powershell-extension-in-vs-code-interacts-with-encodings"></a>VS Code の PowerShell 拡張機能とエンコードの相互作用

PowerShell 拡張機能は、さまざまな方法でスクリプトとやりとりします。

1. スクリプトが VS Code により編集されると、そのコンテンツは VS Code によって拡張機能に送信されます。 [言語サーバー プロトコル][]では、このコンテンツを UTF-8 で転送することを義務付けています。 そのため、拡張機能が不適切なエンコードが取得されることはありません。
1. 統合されたコンソールでスクリプトを直接実行されると、PowerShell によってファイルから直接読み取られます。 PowerShell のエンコードが VS Code のエンコードと異なる場合は、ここで何か問題が起こる可能性があります。
1. VS Code で開かれているスクリプトにより VS Code で開かれていない別のスクリプトが参照されている場合、拡張機能はフォール バックして、そのスクリプトのコンテンツをファイル システムから読み込みます。 PowerShell 拡張機能の既定は UTF-8 エンコードですが、[バイト オーダー マーク][] (BOM) 検出を使用して正しいエンコードが選択されます。

問題は、BOM なしの形式 (BOM なしの [UTF-8][] や [Windows-1252][] など) のエンコードを想定しているときに発生します。 PowerShell 拡張機能の既定値は UTF-8 です。 この拡張機能では VS Code のエンコード設定を変更できません。 詳細については、[問題番号 824](https://github.com/Microsoft/VSCode/issues/824) を参照してください。

## <a name="choosing-the-right-encoding"></a>適切なエンコードの選択

システムやアプリケーションごとに使用しているエンコードが異なる可能性があります。

- 現在、.NET Standard、Web、Linux の世界では、UTF-8 が主流のエンコードです。
- 多くの .NET Framework アプリケーションは [UTF-16][] を使用しています。 歴史的な理由から、これは "Unicode" と呼ばれることもあり、現在では UTF-8 と UTF-16 の両方を含む広範な[標準](https://en.wikipedia.org/wiki/Unicode)を指しています。
- Windows では、Unicode より前のネイティブ アプリケーションの多くが既定で Windows-1252 を使用し続けています。

Unicode エンコードには、バイト オーダー マーク (BOM) の概念もあります。 BOM はテキストの先頭にあり、これによりテキストが使用しているエンコードがデコーダーに通知されます。 マルチバイト エンコードの場合、BOM によりエンコードの[エンディアン](https://en.wikipedia.org/wiki/Endianness)が示されます。 BOM は、Unicode 以外のテキストにはまず出現しないバイトになるように設計されているため、BOM が存在する場合、そのテキストは Unicode であると合理的に推測できます。

BOM はオプションであり、Linux の世界ではそれほど採用されていません。UTF-8 の信頼性の高い規則があらゆるところで使用されているためです。 ほとんどの Linux アプリケーションでは、テキスト入力が UTF-8 でエンコードされていると想定されています。 多くの Linux アプリケーションは BOM を認識して正しく処理しますが、認識しないものもあります。そのため、そのようなアプリケーションで処理されたテキストにアーティファクトが生じます。

**したがって** :

- 主に Windows アプリケーションと Windows PowerShell を使用している場合は、BOM ありの UTF-8 または UTF-16 のようなエンコードをお勧めします。
- 複数のプラットフォームにまたがって作業する場合は、BOM ありの UTF-8 をお勧めします。
- 主に Linux 関連のコンテキストで作業する場合は、BOM なしの UTF-8 をお勧めします。
- Windows-1252 とラテン-1 は基本的にレガシ エンコードであり、できれば避けてください。
  ただし、一部の古い Windows アプリケーションではそれらに依存している可能性があります。
- スクリプトの署名は[エンコードに依存している](https://github.com/PowerShell/PowerShell/issues/3466)点にも注意してください。つまり、署名されたスクリプトのエンコードを変更するには再署名が必要です。

## <a name="configuring-vs-code"></a>VS Code の構成

VS Code の既定のエンコードは BOM なしの UTF-8 です。

[VS Code のエンコード][]を設定するには、VS Code の設定に移動し (<kbd>Ctrl</kbd>+<kbd>、</kbd>)、`"files.encoding"` 設定を指定します。

```json
"files.encoding": "utf8bom"
```

次のような値を指定できます。

- `utf8`:BOM なしの [UTF-8]
- `utf8bom`:BOM ありの [UTF-8]
- `utf16le`:リトル エンディアン [UTF-16]
- `utf16be`:ビッグ エンディアン [UTF-16]
- `windows1252`:[Windows-1252]

GUI ビューでこのドロップダウンを表示するか、JSON ビューですべてを確認できます。

また、可能であれば、以下を追加してエンコードを自動検出することもできます。

```json
"files.autoGuessEncoding": true
```

これらの設定がすべてのファイルの種類に影響しないようにする場合、VS Code では言語ごとに構成することもできます。 `[<language-name>]` フィールドに設定を指定して、言語固有の設定を作成します。 次に例を示します。

```json
"[powershell]": {
    "files.encoding": "utf8bom",
    "files.autoGuessEncoding": true
}
```

また、[Gremlins tracker][] for Visual Studio Code をインストールすることを検討してください。 この拡張機能により、目に見えないか他の通常の文字のように見えるため、文字化けしやすい特定の Unicode 文字が明らかになります。

## <a name="configuring-powershell"></a>PowerShell の構成

PowerShell の既定のエンコードはバージョンによって異なります。

- PowerShell 6 以降では、既定のエンコードはすべてのプラットフォームで BOM なしの UTF-8 です。
- Windows PowerShell では、既定のエンコードは通常 Windows-1252 (拡張子は [latin-1][]) であり、ISO 8859-1 とも呼ばれます。

PowerShell 5 以降では、以下のように既定のエンコードを確認できます。

```powershell
[psobject].Assembly.GetTypes() | Where-Object { $_.Name -eq 'ClrFacade'} |
  ForEach-Object {
    $_.GetMethod('GetDefaultEncoding', [System.Reflection.BindingFlags]'nonpublic,static').Invoke($null, @())
  }
```

次の[スクリプト](https://gist.github.com/rjmholt/3d8dd4849f718c914132ce3c5b278e0e)を使用して、PowerShell セッションが BOM なしのスクリプトに対して推測するエンコードを決定できます。

```powershell
$badBytes = [byte[]]@(0xC3, 0x80)
$utf8Str = [System.Text.Encoding]::UTF8.GetString($badBytes)
$bytes = [System.Text.Encoding]::ASCII.GetBytes('Write-Output "') + [byte[]]@(0xC3, 0x80) + [byte[]]@(0x22)
$path = Join-Path ([System.IO.Path]::GetTempPath()) 'encodingtest.ps1'

try
{
    [System.IO.File]::WriteAllBytes($path, $bytes)

    switch (& $path)
    {
        $utf8Str
        {
            return 'UTF-8'
            break
        }

        default
        {
            return 'Windows-1252'
            break
        }
    }
}
finally
{
    Remove-Item $path
}
```

より一般的にはプロファイル設定を使用して、特定のエンコードを使用するように PowerShell を構成することができます。
次の記事をご覧ください。

- [@mklement0] の [StackOverflow 上の PowerShell エンコードに関する回答](https://stackoverflow.com/a/40098904)。
- [@rkeithhill] の [PowerShell での BOM なしの UTF-8 入力の処理に関するブログ投稿](https://rkeithhill.wordpress.com/2010/05/26/handling-native-exe-output-encoding-in-utf8-with-no-bom/)。

PowerShell に特定の入力エンコードの使用を強制することはできません。 ロケールが en-US に設定された Windows で実行される PowerShell 5.1 以前では、BOM がない場合、既定値は Windows-1252 エンコードです。 他のロケール設定では、別のエンコードを使用できます。 相互運用性を確保するには、BOM が含まれる Unicode 形式でスクリプトを保存することをお勧めします。

> [!IMPORTANT]
> PowerShell スクリプトに触れる他のツールがある場合、そのツールがエンコードの選択の影響を受ける場合や、ツールによってスクリプトが別のエンコードに再エンコードされる場合があります。

### <a name="existing-scripts"></a>既存のスクリプト

既にファイル システム上にあるスクリプトは、必要に応じて新しく選択したエンコードに再エンコードします。 VS Code の下部のバーに、UTF-8 というラベルが表示されます。 クリックしてアクション バーを開き、 **[エンコード付きで保存]** を選択します。 ここで、そのファイル用に新しいエンコードを選択できます。 完全な手順については、[VS Code のエンコード][]に関するページを参照してください。

複数のファイルを再エンコードする必要がある場合は、次のスクリプトを使用できます。

```powershell
Get-ChildItem *.ps1 -Recurse | ForEach-Object {
    $content = Get-Content -Path $_
    Set-Content -Path $_.Fullname -Value $content -Encoding UTF8 -PassThru -Force
}
```

### <a name="the-powershell-integrated-scripting-environment-ise"></a>PowerShell Integrated Scripting Environment (ISE)

PowerShell ISE を使用してスクリプトも編集する場合は、そこでエンコード設定を同期する必要があります。

ISE で BOM を尊重する必要がありますが、リフレクションを使用して[エンコードを設定](https://bensonxion.wordpress.com/2012/04/25/powershell-ise-default-saveas-encoding/)することもできます。
これはスタートアップ後には保持されないことに注意してください。

### <a name="source-control-software"></a>ソース管理ソフトウェア

git などの一部のソース管理ツールはエンコードを無視します (git は単にバイトを追跡します)。 他のもの (Azure DevOps や Mercurial など) はそうではない場合があります。 一部の git ベースのツールでさえ、テキストのデコードに依存しています。

その場合は、次のことを確認してください。

- VS Code の構成と一致するように、ソース管理でテキスト エンコードを構成します。
- すべてのファイルが適切なエンコードでソース管理に確実にチェックインされているようにします。
- ソース管理を介して受信したエンコードの変化に注意します。 これを示す主な兆候は変化を示す差異ですが、何も変わっていないように見えます (バイトは変わっていますが、文字は変わっていないため)。

### <a name="collaborators-environments"></a>共同作業者の環境

ソース管理の構成に加え、共有するファイルの共同作業者が PowerShell ファイルを再エンコードしてエンコードをオーバーライドする設定を確実に保持していないようにします。

### <a name="other-programs"></a>その他のプログラム

PowerShell スクリプトの読み取りまたは書き込みを行う他のプログラムが、再エンコードする可能性があります。

いくつかの例を次に示します。

- クリップボードを使用してスクリプトをコピーして貼り付ける。 これは次のようなシナリオで一般的です。
  - スクリプトを VM にコピーする
  - 電子メールまたは Web ページからスクリプトをコピーする
  - Microsoft Word または PowerPoint ドキュメントとの間でスクリプトをコピーする
- 次のようなその他のテキスト エディター:
  - メモ帳
  - vim
  - その他の PowerShell スクリプト エディター
- 次のようなテキスト編集ユーティリティ:
  - `Get-Content`/`Set-Content`/`Out-File`
  - PowerShell リダイレクト演算子 (`>`、`>>` など)
  - `sed`/`awk`
- 次のようなファイル転送プログラム:
  - Web ブラウザー (スクリプトのダウンロード時)
  - ファイル共有

このようなツールの一部はテキストではなくバイトで処理しますが、エンコードの構成機能が用意されているツールもあります。 エンコードを構成する必要がある場合は、問題を防ぐために、それをエディターのエンコードと同じにする必要があります。

## <a name="other-resources-on-encoding-in-powershell"></a>PowerShell でのエンコードに関するその他のリソース

PowerShell でのエンコードとエンコードの構成に関するお勧めの投稿が他にもいくつかあります。

- [about_Character_Encoding](/powershell/module/microsoft.powershell.core/about/about_character_encoding)
- [@mklement0] の [StackOverflow 上の PowerShell エンコードの概要](https://stackoverflow.com/questions/40098771/changing-powershells-default-output-encoding-to-utf-8)
- エンコードの問題について VS Code-PowerShell で開かれた以前の問題:
  - [#1308](https://github.com/PowerShell/VSCode-powershell/issues/1308)
  - [#1628](https://github.com/PowerShell/VSCode-powershell/issues/1628)
  - [#1680](https://github.com/PowerShell/VSCode-powershell/issues/1680)
  - [#1744](https://github.com/PowerShell/VSCode-powershell/issues/1744)
  - [#1751](https://github.com/PowerShell/VSCode-powershell/issues/1751)
- [_Joel on Software_ が Unicode について書いた以前の投稿](https://www.joelonsoftware.com/2003/10/08/the-absolute-minimum-every-software-developer-absolutely-positively-must-know-about-unicode-and-character-sets-no-excuses/)
- [.NET Standard のエンコード](https://github.com/dotnet/standard/issues/260#issuecomment-289549508)

[@mklement0]: https://github.com/mklement0
[@rkeithhill]: https://github.com/rkeithhill
[Windows-1252]: https://wikipedia.org/wiki/Windows-1252
[latin-1]: https://wikipedia.org/wiki/ISO/IEC_8859-1
[UTF-8]: https://wikipedia.org/wiki/UTF-8
[バイト オーダー マーク]: https://wikipedia.org/wiki/Byte_order_mark
[UTF-16]: https://wikipedia.org/wiki/UTF-16
[言語サーバー プロトコル]: https://microsoft.github.io/language-server-protocol/
[VS Code のエンコード]: https://code.visualstudio.com/docs/editor/codebasics#_file-encoding-support
[Gremlins tracker]: https://marketplace.visualstudio.com/items?itemName=nhoizey.gremlins
