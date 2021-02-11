---
description: スクリプトがユーザーインターフェイス (UI) 言語でユーザーにメッセージや指示を簡単に表示できるようにする、スクリプト国際化機能について説明します。
Locale: en-US
ms.date: 03/20/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_script_internationalization?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Script_Internationalization
ms.openlocfilehash: 283ea6788e76b481c912fc5672350efd2e8bafaa
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605201"
---
# <a name="about-script-internationalization"></a>スクリプト国際化について

## <a name="short-description"></a>簡単な説明
スクリプトがユーザーインターフェイス (UI) 言語でユーザーにメッセージや指示を簡単に表示できるようにする、スクリプト国際化機能について説明します。

## <a name="long-description"></a>長い説明

PowerShell スクリプトの国際化機能を使用すると、ユーザーの言語でヘルプとユーザーのメッセージを表示することで、世界中のユーザーにより優れたサービスを提供できます。

スクリプトの国際化機能は、実行中にオペレーティングシステムの UI カルチャを照会し、適切に翻訳されたテキスト文字列をインポートして、ユーザーに表示します。 Data セクションでは、コードとは別にテキスト文字列を格納できるため、簡単に識別して抽出することができます。 新しいコマンドレットでは、 `ConvertFrom-StringData` テキスト文字列を辞書に似たハッシュテーブルに変換し、翻訳を容易にします。

インターナショナルヘルプテキストをサポートするために、PowerShell には次の機能が含まれています。

- テキスト文字列をコードの命令から分離するデータセクション。 Data セクションの詳細については、「 [about_Data_Sections](about_Data_Sections.md)」を参照してください。

- 新しい自動変数、 `$PSCulture` および `$PSUICulture` 。 `$PSCulture` 日付、時刻、通貨などの要素に対してシステムで使用される UI 言語の名前を格納します。 変数は、 `$PSUICulture` メニューやテキスト文字列などのユーザーインターフェイス要素にシステムで使用される UI 言語の名前を格納します。

- `ConvertFrom-StringData`変換を容易にするためにテキスト文字列をディクショナリに似たハッシュテーブルに変換するコマンドレット。 詳細については、「 [convertfrom-csv-convertfrom-stringdata](xref:Microsoft.PowerShell.Utility.ConvertFrom-StringData)」を参照してください。

- `.psd1`翻訳されたテキスト文字列を格納する新しいファイルの種類。 これらの `.psd1` ファイルは、スクリプトディレクトリの言語固有のサブディレクトリに格納されます。

- `Import-LocalizedData`実行時に、指定した言語の翻訳されたテキスト文字列をスクリプトにインポートするコマンドレット。 このコマンドレットは、Windows でサポートされている言語の文字列を認識し、インポートします。 詳細については [、「import-localizeddata](xref:Microsoft.PowerShell.Utility.Import-LocalizedData)」を参照してください。

### <a name="the-data-section-storing-default-strings"></a>Data セクション: 既定の文字列の格納

スクリプト内の Data セクションを使用して、テキスト文字列を既定の言語で格納します。 キーと値のペアの文字列を、ここでは文字列に配置します。 各キーと値のペアは、別々の行に配置する必要があります。 コメントを含める場合は、コメントを別の行に記述する必要があります。

`ConvertFrom-StringData`このコマンドレットは、ここで指定した文字列のキーと値のペアを、Data section 変数の値に格納されているディクショナリに似たハッシュテーブルに変換します。

次の例では、スクリプトの Data セクションに、 `World.ps1` スクリプトのプロンプトメッセージの English-United States (en-us) のセットが含まれています。 `ConvertFrom-StringData`コマンドレットは、文字列をハッシュテーブルに変換し、変数に格納し `$msgtable` ます。

```powershell
$msgTable = Data {
    #culture="en-US"
    ConvertFrom-StringData @'
    helloWorld = Hello, World.
    errorMsg1 = You cannot leave the user name field blank.
    promptMsg = Please enter your user name.
'@
}
```

ここで説明する文字列の詳細については、「 [about_Quoting_Rules](about_Quoting_Rules.md)」を参照してください。

### <a name="psd1-files-storing-translated-strings"></a>.PSD1 Files: 翻訳された文字列の格納

各 UI 言語のスクリプトメッセージを、スクリプトとファイル名拡張子が同じ名前の別のテキストファイルに保存し `.psd1` ます。 次の形式のカルチャ名を使用して、スクリプトディレクトリのサブディレクトリにファイルを格納します。

`<language>-<region>`

例: de、ar-SA、zh-tw-Hans

たとえば、 `World.ps1` スクリプトがディレクトリに格納されている場合は、 `C:\Scripts` 次のようなファイルディレクトリ構造を作成します。

```
C:\Scripts
C:\Scripts\World.ps1
C:\Scripts\de-DE\World.psd1
C:\Scripts\ar-SA\World.psd1
C:\Scripts\zh-CN\World.psd1
...
```

`World.psd1`スクリプトディレクトリのデ de サブディレクトリのファイルには、次のステートメントが含まれている場合があります。

```powershell
ConvertFrom-StringData -StringData @'
helloWorld = Hallo, Welt.
errorMsg1 = Das Feld Benutzername darf nicht leer sein.
promptMsg = Geben Sie Ihren Benutzernamen ein.
'@
```

同様に、 `World.psd1` スクリプトディレクトリの ar-SA サブディレクトリのファイルには、次のステートメントが含まれている場合があります。

```powershell
ConvertFrom-StringData -StringData @'
helloWorld = مرحبًا أيها العالَم
errorMsg1 = لا يمكنك ترك حقل اسم المستخدم فارغًا
promptMsg = يرجى إدخال اسم المستخدم الخاص بك
'@
```

### <a name="import-localizeddata-dynamic-retrieval-of-translated-strings"></a>Import-localizeddata: 翻訳された文字列の動的な取得

現在のユーザーの UI 言語で文字列を取得するには、コマンドレットを使用し `Import-LocalizedData` ます。

`Import-LocalizedData` 自動変数の値を検索 `$PSUICulture` し、 `<script-name>.psd1` その値に一致するサブディレクトリ内のファイルの内容をインポートし `$PSUICulture` ます。 次に、 **Bindingvariable** パラメーターの値で指定した変数に、インポートされたコンテンツを保存します。

```powershell
Import-LocalizedData -BindingVariable msgTable
```

たとえば、 `Import-LocalizedData` スクリプトにコマンドが表示され、 `C:\Scripts\World.ps1` の値 `$PSUICulture` が "ar-SA" の場合、は `Import-LocalizedData` 次のファイルを検索します。

`C:\Scripts\ar-SA\World.psd1`

次に、ファイルからアラビア語のテキスト文字列を変数にインポートして `$msgTable` 、スクリプトの Data セクションに定義されている可能性のある既定の文字列を置き換え `World.ps1` ます。

その結果、スクリプトが変数を使用してユーザーメッセージを表示すると、 `$msgTable` メッセージはアラビア語で表示されます。

たとえば、次のスクリプトでは、アラビア語の "ユーザー名を入力してください" というメッセージが表示されます。

```powershell
if (!($username)) { $msgTable.promptMsg }
```

が `Import-LocalizedData` `.psd1` の値と一致するファイルを見つけることができない場合、 `$PSUIculture` の値 `$msgTable` は置き換えられず、への呼び出しによって en-us の `$msgTable.promptMsg` フォールバック文字列が表示されます。

## <a name="examples"></a>例

この例では、スクリプトでスクリプトの国際化機能を使用して、コンピューターに設定されている言語でユーザーに曜日を表示する方法を示します。

Sample1.ps1 スクリプトファイルの完全な一覧を次に示します。

スクリプトは、コマンドを含む Day ($Day) という名前のデータセクションから始まり `ConvertFrom-StringData` ます。 に送信される式 `ConvertFrom-StringData` は、キーと値のペアの既定の UI カルチャ en-us の曜日名を含む、ここで指定した文字列です。 コマンドレットは、ここで指定した `ConvertFrom-StringData` 文字列のキーと値のペアをハッシュテーブルに変換し、変数の値として保存し `$Day` ます。

`Import-LocalizedData`このコマンドは、 `.psd1` 自動変数の値に一致するディレクトリ内のファイルの内容をインポート `$PSUICulture` し、変数に保存します。これにより、 `$Day` Data セクションで定義されているの値が置き換え `$Day` られます。

残りのコマンドは、文字列を配列に読み込んで表示します。

```powershell
$Day = Data {
#culture="en-US"
ConvertFrom-StringData -StringData @'
    messageDate = Today is
    d0 = Sunday
    d1 = Monday
    d2 = Tuesday
    d3 = Wednesday
    d4 = Thursday
    d5 = Friday
    d6 = Saturday
'@
}

Import-LocalizedData -BindingVariable Day

#Build an array of weekdays.
$a = $Day.d0, $Day.d1, $Day.d2, $Day.d3, $Day.d4, $Day.d5, $Day.d6

# Get the day of the week as a number (Monday = 1).
# Index into $a to get the name of the day.
# Use string formatting to build a sentence.

"{0} {1}" -f $Day.messageDate, $a[(Get-Date -UFormat %u)] | Out-Host
```

`.psd1`スクリプトをサポートするファイルは、の値と一致する名前のスクリプトディレクトリのサブディレクトリに保存され `$PSUICulture` ます。

次に、の完全な一覧を示し `.\de-DE\sample1.psd1` ます。

```powershell
# culture="de-DE"
ConvertFrom-StringData @'
    messageDate = Heute ist
    d0 = Sonntag
    d1 = Montag
    d2 = Dienstag
    d3 = Mittwoch
    d4 = Donnerstag
    d5 = Freitag
    d6 = Samstag
'@
```

その結果、の値が de になっていないシステムで Sample.ps1 を実行すると、 `$PSUICulture` スクリプトの出力は次のようになります。

```Output
Heute ist Freitag
```

## <a name="see-also"></a>関連項目

- [about_Data_Sections](about_Data_Sections.md)
- [about_Automatic_Variables](about_Automatic_Variables.md)
- [about_Hash_Tables](about_Hash_Tables.md)
- [about_Quoting_Rules](about_Quoting_Rules.md)
- [ConvertFrom-StringData](xref:Microsoft.PowerShell.Utility.ConvertFrom-StringData)
- [Import-LocalizedData](xref:Microsoft.PowerShell.Utility.Import-LocalizedData)

