---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/import-localizeddata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-LocalizedData
ms.openlocfilehash: fa5de857b70ec05d30c42fbf8e51a9411d823018
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601210"
---
# Import-LocalizedData

## 概要
オペレーティング システム用に選択されている UI カルチャに基づき、言語固有のデータをスクリプトと関数にインポートします。

## SYNTAX

```
Import-LocalizedData [[-BindingVariable] <String>] [[-UICulture] <String>] [-BaseDirectory <String>]
 [-FileName <String>] [-SupportedCommand <String[]>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Import-LocalizedData` オペレーティングシステムの現在のユーザーに設定されている UI 言語と一致する名前を持つサブディレクトリから文字列を動的に取得します。 スクリプトにより、現在のユーザーが選択している UI 言語でユーザー メッセージを表示できるように設計されています。

`Import-LocalizedData``.psd1`スクリプトディレクトリの言語固有のサブディレクトリにあるファイルからデータをインポートし、コマンドで指定されたローカル変数に保存します。 コマンドレットでは、自動変数の値に基づいてサブディレクトリとファイルを選択し `$PSUICulture` ます。 スクリプトのローカル変数を使用してユーザー メッセージを表示すると、ユーザーの UI 言語でメッセージが表示されます。

のパラメーターを使用し `Import-LocalizedData` て、代替 UI カルチャ、パス、およびファイル名を指定し、サポートされているコマンドを追加して、ファイルが見つからない場合に表示されるエラーメッセージを非表示にすることができ `.psd1` ます。

この `Import-LocalizedData` コマンドレットは、Windows PowerShell 2.0 で導入されたスクリプト国際化イニシアチブをサポートしています。 このイニシアチブは、スクリプトを使用して現在のユーザーの UI 言語でユーザー メッセージを容易に表示できるようにして、世界中のユーザーのサービス向上を目的としています。 この詳細およびファイルの形式の詳細について `.psd1` は、「 [about_Script_Internationalization](../Microsoft.PowerShell.Core/About/about_Script_Internationalization.md)」を参照してください。

## 例

### 例 1: テキスト文字列をインポートする

この例では、テキスト文字列を変数にインポート `$Messages` します。 その他のすべてのコマンドレットのパラメーターの既定値を使用します。

```powershell
Import-LocalizedData -BindingVariable "Messages"
```

コマンドがディレクトリ内の Archives.ps1 スクリプトに含まれていて、 `C:\Test` 自動変数の値 `$PsUICulture` が zh-tw である場合、は `Import-LocalizedData` `Archives.psd1` ディレクトリ内のファイルを `C:\test\zh-CN` 変数にインポートし `$Messages` ます。

### 例 2: ローカライズされたデータ文字列をインポートする

この例は、スクリプトではなく、コマンドラインで実行されます。 Test.psd1 ファイルからローカライズされたデータの文字列を取得し、コマンドラインに表示します。 コマンドはスクリプトでは使用されないため、**FileName** パラメーターが必要になります。 このコマンドは、 **UICulture** パラメーターを使用して、en-us カルチャを指定します。

```powershell
Import-LocalizedData -FileName "Test.psd1" -UICulture "en-US"
```

```Output
Name                           Value
----                           -----
Msg3                           "Use $_ to represent the object that is being processed."
Msg2                           "This command requires the credentials of a member of the Administrators group on the...
Msg1                           "The Name parameter is missing from the command."
```

`Import-LocalizedData` ローカライズされたデータ文字列を含むハッシュテーブルを返します。

### 例 3: UI カルチャ文字列をインポートする

```powershell
Import-LocalizedData -BindingVariable "MsgTbl" -UICulture "ar-SA" -FileName "Simple" -BaseDirectory "C:\Data\Localized"
```

このコマンドは、テキスト文字列を `$MsgTbl` スクリプトの変数にインポートします。

**UICulture** パラメーターを使用して、 `Simple.psd1` のサブディレクトリにあるファイルからデータをインポートするようにコマンドレットに指示し `ar-SA` `C:\Data\Localized` ます。

### 例 4: ローカライズされたデータをスクリプトにインポートする

この例は、単純なスクリプト内のローカライズされたデータを表示する方法を示します。

```powershell
PS C:\> # In C:\Test\en-US\Test.psd1:

ConvertFrom-StringData @'

# English strings

Msg1 = "The Name parameter is missing from the command."
Msg2 = "This command requires the credentials of a member of the Administrators group on the computer."
Msg3 = "Use $_ to represent the object that is being processed."
'@

# In C:\Test\Test.ps1

Import-LocalizedData -BindingVariable "Messages"
Write-Host $Messages.Msg2

# In Windows PowerShell

PS C:\> .\Test.ps1

This command requires the credentials of a member of the Administrators group on the computer.
```

この例の最初の部分では、ファイルの内容を示し `Test.psd1` ます。 これには、 `ConvertFrom-StringData` 一連の名前付きテキスト文字列をハッシュテーブルに変換するコマンドが含まれています。 この `Test.psd1` ファイルは、 `C:\Test` スクリプトが格納されているディレクトリの en-us サブディレクトリにあります。

この例の2番目の部分は、スクリプトの内容を示して `Test.ps1` います。 これには、 `Import-LocalizedData` 一致するファイルから変数にデータをインポートするコマンド `.psd1` と、 `$Messages` `Write-Host` 変数内のいずれかのメッセージを `$Messages` ホストプログラムに書き込むコマンドが含まれています。

例の最後の部分で、スクリプトを実行します。 出力は、オペレーティング システムの現在のユーザーのために設定された UI の言語で正しいユーザー メッセージが表示されることを示しています。

### 例 5: スクリプト内の既定のテキスト文字列を置き換える

この例では、を使用し `Import-LocalizedData` て、スクリプトの DATA セクションに定義されている既定のテキスト文字列を置換する方法を示します。

```powershell
PS C:\> # In TestScript.ps1
$UserMessages = DATA

{    ConvertFrom-StringData @'

    # English strings

        Msg1 = "Enter a name."
        Msg2 = "Enter your employee ID."
        Msg3 = "Enter your building number."
'@
}

Import-LocalizedData -BindingVariable "UserMessages"
$UserMessages.Msg1...
```

この例では、TestScript.ps1 スクリプトの DATA セクションに、 `ConvertFrom-StringData` データセクションの内容をハッシュテーブルに変換し、変数の値に格納するコマンドが含まれてい `$UserMessages` ます。

このスクリプトには、 `Import-LocalizedData` 変数の値で指定されたサブディレクトリ内の TestScript.psd1 ファイルから翻訳済みのテキスト文字列のハッシュテーブルをインポートするコマンドも含まれてい `$PsUICulture` ます。 コマンドがファイルを検索した場合 `.psd1` 、ファイルから翻訳された文字列を同じ変数の値に保存し `$UserMessages` 、データセクションロジックによって保存されたハッシュテーブルを上書きします。

3番目のコマンドは、変数内の最初のメッセージを表示し `$UserMessages` ます。

コマンドが `Import-LocalizedData` 言語のファイルを検索した場合 `.psd1` `$PsUICulture` 、変数の値には `$UserMessages` 翻訳されたテキスト文字列が含まれます。 コマンドが何らかの理由で失敗した場合、スクリプトの DATA セクションに定義されている既定のテキスト文字列が表示されます。

### 例 6: UI カルチャが見つからない場合にエラーメッセージを表示しない

この例で `Import-LocalizedData` は、ユーザーの UI カルチャに一致するディレクトリが見つからない場合、またはディレクトリ内のスクリプトのファイルが見つからない場合に表示されるエラーメッセージを非表示にする方法を示し `.psd1` ます。

```powershell
PS C:\> # In Day1.ps1

Import-LocalizedData -BindingVariable "Day"

# In Day2.ps1

Import-LocalizedData -BindingVariable "Day" -ErrorAction:SilentlyContinue

PS C:\> .\Day1.ps1
Import-LocalizedData : Cannot find PowerShell data file 'Day1.psd1' in directory 'C:\ps-test\fr-BE\' or any parent culture directories.
At C:\ps-test\Day1.ps1:17 char:21+ Import-LocalizedData <<<<  Day
Today is Tuesday

PS C:\> .\Day2.ps1
Today is Tuesday
```

**SilentlyContinue** の値を持つ ErrorAction 共通パラメーターを使用して、エラー メッセージを非表示にすることができます。 これは、既定の言語またはフォールバック言語でユーザーメッセージを提供し、エラーメッセージが必要ない場合に特に便利です。

この例では、コマンドを含む2つのスクリプト `Day1.ps1` と Day2.ps1 を比較し `Import-LocalizedData` ます。 スクリプトは同じですが、Day2.ps1 では値がで **Erroraction** 共通パラメーターを使用する点が異なり `SilentlyContinue` ます。

このサンプル出力は、UI カルチャがに設定され、 `fr-BE` その ui カルチャに一致するファイルまたはディレクトリがない場合に、両方のスクリプトを実行した結果を示しています。 `Day1.ps1` エラーメッセージと英語の出力を表示します。 `Day2.ps1` 英語の出力のみが表示されます。

## PARAMETERS

### -BaseDirectory

ファイルが配置されているベースディレクトリを指定し `.psd1` ます。 既定では、スクリプトが配置されているディレクトリです。 `Import-LocalizedData``.psd1`ベースディレクトリの言語固有のサブディレクトリで、スクリプトのファイルを検索します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -BindingVariable

テキスト文字列のインポート先の変数を指定します。 ドル記号を付けずに変数名を入力してください ( `$` )。

Windows PowerShell 2.0 ではこのパラメーターは必須です。 Windows PowerShell 3.0 ではこのパラメーターは省略可能です。 このパラメーターを省略すると、は `Import-LocalizedData` テキスト文字列のハッシュテーブルを返します。 ハッシュ テーブルは、パイプラインに渡されるか、コマンドラインに表示されます。

を使用して、 `Import-LocalizedData` スクリプトの data セクションに指定されている既定のテキスト文字列を置き換える場合は、データセクションを変数に割り当て、 **bindingvariable** パラメーターの値に data section 変数の名前を入力します。 次に、 `Import-LocalizedData` インポートしたコンテンツを **bindingvariable** に保存するときに、インポートされたデータによって既定のテキスト文字列が置き換えられます。 既定のテキスト文字列を指定しない場合は、任意の変数名を選択することができます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: Variable

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FileName

インポートするデータファイルの名前を指定し `.psd1)` ます。 ファイル名を入力します。 ファイル名拡張子を含まないファイル名を指定することも、ファイル名拡張子を含むファイル名を指定することもでき `.psd1` `.psd1` ます。 データファイルは Unicode または UTF-8 として保存する必要があります。

 `Import-LocalizedData` がスクリプトで使用されていない場合は、FileName パラメーターが必要です。
それ以外の場合、このパラメーターは省略可能であり、既定値はスクリプトのベース名です。 このパラメーターを使用すると `Import-LocalizedData` 、別のファイルを検索するように指示でき `.psd1` ます。

たとえば、 **ファイル名** が省略されていて、スクリプト名が FindFiles.ps1 である場合、は `Import-LocalizedData` FindFiles.psd1 のデータファイルを検索します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SupportedCommand

データのみを生成するコマンドレットと関数を指定します。

このパラメーターを使用して、記述またはテストしたコマンドレットと関数を指定します。 詳細については、「 [about_Script_Internationalization](../Microsoft.PowerShell.Core/About/about_Script_Internationalization.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UICulture

代替の UI カルチャを指定します。
既定値は、自動変数の値です `$PsUICulture` 。
、、などの形式で UI カルチャを入力し `<language>-<region>` `en-US` `de-DE` `ar-SA` ます。

**UICulture** パラメーターの値によって、から `Import-LocalizedData` スクリプトのファイルを取得する、言語固有のサブディレクトリ (ベースディレクトリ内) が決定され `.psd1` ます。

このコマンドレットは、 **UICulture** パラメーターの値と同じ名前のサブディレクトリ、 `$PsUICulture` またはやなどの自動変数を検索し `de-DE` `ar-SA` ます。 ディレクトリが見つからない場合、またはディレクトリにスクリプトのファイルが含まれていない場合は、 `.psd1` de や ar などの言語コードの名前を持つサブディレクトリが検索されます。 サブディレクトリまたはファイルが見つからない場合 `.psd1` 、コマンドは失敗し、スクリプトで指定された既定の言語でデータが表示されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### System.Collections.Hashtable

`Import-LocalizedData`**bindingvariable** パラメーターの値で指定された変数にハッシュテーブルを保存します。

## 注

- を使用する前に `Import-LocalizedData` 、ユーザーメッセージをローカライズします。 キーと値のペアのハッシュテーブルで各ロケール (UI カルチャ) のメッセージの書式を設定し、スクリプトとファイル名の拡張子が同じであるファイルにハッシュテーブルを保存し `.psd1` ます。 サポートされている各 UI カルチャのスクリプトディレクトリの下にディレクトリを作成し、ui カルチャ名を使用して、 `.psd1` 各 ui カルチャのファイルをディレクトリに保存します。

  たとえば、de-DE ロケールのユーザー メッセージをローカライズし、ハッシュ テーブルで書式を設定します。
  ハッシュテーブルをファイルに保存 `<ScriptName>.psd1` します。 次に、 `de-DE` スクリプトディレクトリの下にサブディレクトリを作成し、ドイツ語 `<ScriptName\>.psd1` ファイルをサブディレクトリに保存し `de-DE` ます。
  サポートしているロケールごとにこのメソッドを繰り返します。

- `Import-LocalizedData` スクリプトのローカライズされたユーザーメッセージの構造化検索を実行します。

  `Import-LocalizedData` スクリプトファイルが格納されているディレクトリ (または **Basedirectory** パラメーターの値) の検索を開始します。 次に、ベースディレクトリ内で、変数の値と同じ名前のサブディレクトリ `$PsUICulture` (または、 **UICulture** パラメーターの値) を検索し `de-DE` `ar-SA` ます。 次に、そのサブディレクトリで、 `.psd1` スクリプトと同じ名前のファイル (または **FileName** パラメーターの値) を検索します。

  が `Import-LocalizedData` UI カルチャの名前を持つサブディレクトリを見つけられない場合、またはサブディレクトリにスクリプト用のファイルが含まれていない場合は `.psd1` 、 `.psd1` de や ar などの言語コードの名前を持つサブディレクトリで、スクリプトのファイルが検索されます。 サブディレクトリまたはファイルが見つからない場合、 `.psd1` コマンドは失敗し、スクリプト内の既定の言語でデータが表示され、データをインポートできなかったことを示すエラーメッセージが表示されます。 メッセージを非表示にして適切に失敗させるには、SilentlyContinue の値を指定した **Erroraction** 共通パラメーターを使用します。

  が `Import-LocalizedData` サブディレクトリとファイルを検出すると `.psd1` 、ユーザーメッセージのハッシュテーブルをコマンドの **bindingvariable** パラメーターの値にインポートします。 次に、変数のハッシュ テーブルからメッセージを表示すると、ローカライズされたメッセージが表示されます。

  詳細については、「 [about_Script_Internationalization](../Microsoft.Powershell.Core/About/about_Script_Internationalization.md)」を参照してください。

## 関連リンク

[Write-Host](Write-Host.md)

[Import-PowerShellDataFile](Import-PowerShellDataFile.md)

