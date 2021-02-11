---
description: PowerShell が文字列データの入力と出力に文字エンコーディングを使用する方法について説明します。
Locale: en-US
ms.date: 10/21/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_character_encoding?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Character_Encoding
ms.openlocfilehash: f383b985baeae059565f61c700df7a76346497d8
ms.sourcegitcommit: cc72c40315fd2981d3009b335accbfa52d57640c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2020
ms.locfileid: "96349850"
---
# <a name="about_character_encoding"></a>about_Character_Encoding

## <a name="short-description"></a>簡単な説明
PowerShell が文字列データの入力と出力に文字エンコーディングを使用する方法について説明します。

## <a name="long-description"></a>長い説明

Unicode は世界中の文字エンコード標準です。 システムは、文字および文字列の操作に Unicode のみを使用します。 Unicode のすべての側面の詳細については、 [Unicode 標準](https://www.unicode.org/standard/standard.html)に関する説明を参照してください。

Windows では、Unicode および従来の文字セットがサポートされています。 Windows コードページなどの従来の文字セットでは、8ビット値または8ビット値の組み合わせを使用して、特定の言語または地理的地域の設定で使用される文字を表します。

PowerShell では、既定で Unicode 文字セットが使用されます。 ただし、いくつかのコマンドレットには、別の文字セットのエンコードを指定できる **encoding** パラメーターがあります。 このパラメーターを使用すると、他のシステムやアプリケーションとの相互運用性を確保するために必要な、特定の文字エンコードを選択できます。

次のコマンドレットには **Encoding** パラメーターがあります。

- Microsoft.PowerShell.Management
  - Add-Content
  - Get-Content
  - Set-Content
- Microsoft.PowerShell.Utility
  - Export-Clixml
  - Export-Csv
  - Export-PSSession
  - Format-Hex
  - Import-Csv
  - Out-File
  - Select-String
  - Send-MailMessage

## <a name="the-byte-order-mark"></a>バイト順マーク

バイト順マーク (BOM) は、ファイルまたはテキストストリームの最初の数バイトの _unicode 署名_ で、データに使用される unicode エンコーディングを示します。 詳細については、 [バイトオーダーマーク](/globalization/encoding/byte-order-mark) のドキュメントを参照してください。

Windows PowerShell では、以外のすべての Unicode エンコーディングでは、 `UTF7` 常に BOM が作成されます。 PowerShell Core では、 `utf8NoBOM` すべてのテキスト出力で既定値が使用されます。

全体的な互換性を最大限に高めるには、UTF-8 ファイルで Bom を使用しないようにしてください。 Windows プラットフォームでも使用されている unix プラットフォームおよび Unix の保護ユーティリティでは、Bom はサポートされていません。

同様に、 `UTF7` エンコードを避ける必要があります。 UTF-7 は標準の Unicode エンコードではなく、すべてのバージョンの PowerShell に BOM なしで記述されます。

Unix のようなプラットフォームで PowerShell スクリプトを作成したり、Windows のクロスプラットフォームエディター (Visual Studio Code など) を使用したりすると、を使用してエンコードされたファイルが生成さ `UTF8NoBOM` れます。 これらのファイルは PowerShell Core では正常に動作しますが、ファイルに Ascii 以外の文字が含まれていると、Windows PowerShell では機能しなくなる可能性があります。

スクリプトで Ascii 以外の文字を使用する必要がある場合は、BOM を使用して UTF-8 として保存します。 BOM がない場合、Windows PowerShell はスクリプトを従来の "ANSI" コードページでエンコードされたものとして misinterprets します。 逆に、UTF-8 BOM を持つファイルは、Unix のようなプラットフォームでは問題になる可能性があります。 、、、などの多くの Unix ツール `cat` は、 `sed` BOM を `awk` `gedit` 扱う方法がわからない場合があります。

## <a name="character-encoding-in-windows-powershell"></a>Windows PowerShell での文字エンコード

PowerShell 5.1 では、 **Encoding** パラメーターは次の値をサポートしています。

- `Ascii` Ascii (7 ビット) 文字セットを使用します。
- `BigEndianUnicode` は、ビッグエンディアンのバイト順で UTF-16 を使用します。
- `BigEndianUTF32` は、ビッグエンディアンのバイト順で 32 UTF-8 を使用します。
- `Byte` 文字のセットをバイトシーケンスにエンコードします。
- `Default` は、システムのアクティブなコードページ (通常は ANSI) に対応するエンコーディングを使用します。
- `Oem` は、システムの現在の OEM コードページに対応するエンコーディングを使用します。
- `String``Unicode` と同じです。
- `Unicode` は、リトルエンディアンのバイト順で UTF-16 を使用します。
- `Unknown``Unicode` と同じです。
- `UTF32` は、リトルエンディアンのバイト順で 32 UTF-8 を使用します。
- `UTF7` UTF-7 を使用します。
- `UTF8` UTF-8 (BOM 付き) を使用します。

一般に、Windows PowerShell では、既定で Unicode [utf-8](https://wikipedia.org/wiki/UTF-16) エンコードが使用されます。 ただし、Windows PowerShell のコマンドレットで使用される既定のエンコードには一貫性がありません。

> [!NOTE]
> 以外の Unicode エンコーディングを使用する場合は、 `UTF7` 常に BOM が作成されます。

出力をファイルに書き込むコマンドレットの場合:

- `Out-File` また、リダイレクト演算子 `>` とは `>>` 16LE を作成します。これは特に `Set-Content` およびとは異なり `Add-Content` ます。

- `New-ModuleManifest``Export-CliXml`また、16LE ファイルも作成します。

- ターゲットファイルが空であるか存在しない場合、 `Set-Content` および `Add-Content` エンコードを使用し `Default` ます。 `Default` は、アクティブシステムロケールの ANSI レガシコードページによって指定されたエンコーディングです。

- `Export-Csv` で `Ascii` は、 **Append** パラメーター (下記参照) を使用するときに、ファイルを作成しますが、別のエンコードを使用します。

- `Export-PSSession` 既定で、BOM を使用して UTF-8 ファイルを作成します。

- `New-Item -Type File -Value` BOM のない UTF-8 ファイルを作成します。

- `Send-MailMessage``Default`既定ではエンコードを使用します。

- `Start-Transcript``Utf8`BOM を使用してファイルを作成します。 **Append** パラメーターを使用する場合は、エンコードが異なる場合があります (下記参照)。

既存のファイルに追加するコマンドの場合:

- `Out-File -Append` また、 `>>` リダイレクト演算子は、既存のターゲットファイルのコンテンツのエンコードと一致しません。 代わりに、 **encoding** パラメーターが使用されない限り、既定のエンコーディングを使用します。 コンテンツを追加するときは、ファイルの元のエンコードを使用する必要があります。

- 明示的な **encoding** パラメーターがない場合、は `Add-Content` 既存のエンコーディングを検出し、新しいコンテンツに自動的に適用します。 既存のコンテンツに BOM がない場合は、 `Default` ANSI エンコーディングが使用されます。 の動作は、既定のエンコードがであることを除いて、 `Add-Content` PowerShell Core (v6 以降) で同じです `Utf8` 。

- `Export-Csv -Append` ターゲットファイルに BOM が含まれている場合に、既存のエンコードと一致します。 BOM がない場合、エンコードが使用さ `Utf8` れます。

- `Start-Transcript -Append` BOM を含むファイルの既存のエンコードと一致します。 BOM がない場合、既定でエンコードが設定さ `Ascii` れます。 このエンコードにより、トランスクリプト内のデータにマルチバイト文字が含まれている場合に、データの損失や文字の破損が発生する可能性があります。

BOM がない場合に文字列データを読み取るコマンドレットの場合は、次のようになります。

- `Get-Content` と `Import-PowerShellDataFile` は、 `Default` ANSI エンコーディングを使用します。 ANSI は、ファイルからソースコードを読み取るときに PowerShell エンジンが使用するものでもあります。

- `Import-Csv`、、およびは、BOM がないことを想定して `Import-CliXml` `Select-String` `Utf8` います。

## <a name="character-encoding-in-powershell-core"></a>PowerShell Core での文字エンコード

PowerShell Core (v6 以降) では、 **Encoding** パラメーターは次の値をサポートしています。

- `ascii`: ASCII (7 ビット) 文字セットのエンコーディングを使用します。
- `bigendianunicode`: ビッグエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `oem`: MS-DOS およびコンソールプログラムの既定のエンコードを使用します。
- `unicode`: リトルエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `utf7`: UTF-7 形式でエンコードします。
- `utf8`: UTF-8 形式 (BOM なし) でエンコードします。
- `utf8BOM`: バイト順マーク (BOM) を使用して UTF-8 形式でエンコードします。
- `utf8NoBOM`: バイトオーダーマーク (BOM) を使用せずに UTF-8 形式でエンコードします。
- `utf32`:32 UTF-8 形式でエンコードします。

PowerShell Core では、 `utf8NoBOM` すべての出力で既定値が使用されます。

PowerShell 6.2 以降では、 **Encoding** パラメーターを使用して、登録されているコードページの数値 id (など) `-Encoding 1251` や、登録されているコードページの文字列名 (など) を使用することもでき `-Encoding "windows-1251"` ます。 詳細については、 [コードページ](/dotnet/api/system.text.encoding.codepage)の .net ドキュメントを参照してください。

## <a name="changing-the-default-encoding"></a>既定のエンコードの変更

PowerShell には、既定のエンコーディング動作を変更するために使用できる既定の変数が2つあります。

- `$PSDefaultParameterValues`
- `$OutputEncoding`

詳細については、「 [about_Preference_Variables](about_Preference_Variables.md)」を参照してください。

PowerShell 5.1 以降では、リダイレクト演算子 ( `>` と) によって `>>` コマンドレットが呼び出され `Out-File` ます。 そのため、 `$PSDefaultParameterValues` 次の例に示すように、ユーザー設定変数を使用して、これらの既定のエンコーディングを設定できます。

```powershell
$PSDefaultParameterValues['Out-File:Encoding'] = 'utf8'
```

**Encoding** パラメーターを持つすべてのコマンドレットの既定のエンコードを変更するには、次のステートメントを使用します。

```powershell
$PSDefaultParameterValues['*:Encoding'] = 'utf8'
```

> [!IMPORTANT]
> このコマンドを PowerShell プロファイルに含めると、エンコードを明示的に指定していないすべてのコマンドとスクリプトに影響を与えるセッショングローバル設定が設定されます。
>
> 同様に、同じように動作させるスクリプトまたはモジュールにこのようなコマンドを含める必要があります。 これらのコマンドを使用すると、他のユーザー、別のコンピューター、または別のバージョンの PowerShell で実行されている場合でも、コマンドレットの動作が同じになります。

自動変数は、 `$OutputEncoding` PowerShell による外部プログラムとの通信に使用されるエンコーディングに影響します。 出力リダイレクト演算子と PowerShell コマンドレットがファイルに保存するために使用するエンコーディングには影響しません。

## <a name="see-also"></a>関連項目

- [.NET での文字エンコードの概要](/dotnet/standard/base-types/character-encoding-introduction)
- [コードページ-Win32 アプリ](/windows/win32/intl/code-pages)
- [Unicode 標準](https://www.unicode.org/standard/standard.html)
- [Encoding. コードページ](/dotnet/api/system.text.encoding.codepage)
- [UTF-16LE](https://wikipedia.org/wiki/UTF-16)
- [バイト順マーク](https://wikipedia.org/wiki/Byte_order_mark)
- [about_Preference_Variables](about_Preference_Variables.md)
