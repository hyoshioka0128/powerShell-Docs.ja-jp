---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/19/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-clixml?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-Clixml
ms.openlocfilehash: 7a0c25197ba00fb05089f855592e7744ef10e9b9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604935"
---
# Export-Clixml

## 概要
1 つまたは複数のオブジェクトの XML ベースの表現を作成し、ファイルに格納します。

## SYNTAX

### ByPath (既定値)

```
Export-Clixml [-Depth <Int32>] [-Path] <String> -InputObject <PSObject> [-Force] [-NoClobber]
 [-Encoding <Encoding>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Export-Clixml [-Depth <Int32>] -LiteralPath <String> -InputObject <PSObject> [-Force] [-NoClobber]
 [-Encoding <Encoding>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Export-Clixml` 共通言語基盤 (CLI) の XML ベースの表現をオブジェクトまたはオブジェクトで作成し、ファイルに格納します。 その後、コマンドレットを使用して、 `Import-Clixml` そのファイルの内容に基づいて保存されたオブジェクトを再作成できます。
CLI の詳細については、「 [言語に依存](/dotnet/standard/language-independence)しない」を参照してください。

このコマンドレットはに似ていますが、では `ConvertTo-Xml` 、結果の XML がファイルに格納される点が異なり `Export-Clixml` ます。 `ConvertTo-XML` は XML を返します。そのため、PowerShell で処理を続行できます。

Windows コンピューターでのの重要な使用方法 `Export-Clixml` は、資格情報をエクスポートし、文字列を安全に XML としてセキュリティで保護することです。 例については、例3を参照してください。

## 例

### 例 1: XML ファイルに文字列をエクスポートする

この例では、現在のディレクトリに格納されている XML ファイルを作成します。 **これは**、という文字列の表現です。

```powershell
"This is a test" | Export-Clixml -Path .\sample.xml
```

文字列 `This is a test` はパイプラインで送信されます。 `Export-Clixml`**Path** パラメーターを使用して、現在のディレクトリにという名前の XML ファイルを作成し `sample.xml` ます。

### 例 2: オブジェクトを XML ファイルにエクスポートする

この例では、オブジェクトを XML ファイルにエクスポートし、エクスポートしたファイルから XML をインポートしてオブジェクトを作成する方法を示します。

```powershell
Get-Acl C:\test.txt | Export-Clixml -Path .\FileACL.xml
$fileacl = Import-Clixml -Path .\FileACL.xml
```

コマンドレットでは、 `Get-Acl` ファイルのセキュリティ記述子を取得し `Test.txt` ます。 オブジェクトをパイプラインの下に送信して、セキュリティ記述子をに渡し `Export-Clixml` ます。 オブジェクトの XML ベースの表現は、という名前のファイルに格納され `FileACL.xml` ます。

`Import-Clixml`コマンドレットは、ファイル内の XML からオブジェクトを作成し `FileACL.xml` ます。 次に、変数にオブジェクトを保存し `$fileacl` ます。

### 例 3: Windows でエクスポートされた資格情報オブジェクトを暗号化する

この例では、コマンドレットを実行して変数に格納した資格情報を指定した場合、 `$Credential` `Get-Credential` コマンドレットを実行して `Export-Clixml` 資格情報をディスクに保存できます。

> [!IMPORTANT]
> `Export-Clixml` 暗号化された資格情報のみを Windows でエクスポートします。 MacOS や Linux などの Windows 以外のオペレーティングシステムでは、資格情報は Unicode 文字配列として格納されたプレーンテキストとしてエクスポートされます。 これにより、一部の難読化が可能になりますが、暗号化は提供されません。

```powershell
$Credxmlpath = Join-Path (Split-Path $Profile) TestScript.ps1.credential
$Credential | Export-Clixml $Credxmlpath
$Credxmlpath = Join-Path (Split-Path $Profile) TestScript.ps1.credential
$Credential = Import-Clixml $Credxmlpath
```

コマンドレットでは、 `Export-Clixml` Windows [データ保護 API](/previous-versions/windows/apps/hh464970(v=win.10))を使用して資格情報オブジェクトを暗号化します。 暗号化により、そのコンピューター上のユーザーアカウントのみが資格情報オブジェクトの内容を復号化できるようになります。
エクスポートされたファイルは、 `CLIXML` 別のコンピューターまたは別のユーザーが使用することはできません。

この例では、資格情報が格納されているファイルがによって表され `TestScript.ps1.credential` ます。 **Testscript** を、資格情報の読み込みに使用するスクリプトの名前に置き換えます。

資格情報オブジェクトをパイプラインの下に送信 `Export-Clixml` し、 `$Credxmlpath` 最初のコマンドで指定したパスに保存します。

スクリプトに資格情報を自動的にインポートするには、最後の2つのコマンドを実行します。 を実行し `Import-Clixml` て、セキュリティで保護された資格情報オブジェクトをスクリプトにインポートします。 このインポートにより、スクリプトにプレーンテキストのパスワードが公開されるリスクがなくなります。

### 例 4: Linux または macOS で資格情報オブジェクトをエクスポートする

この例では、  `$Credential` コマンドレットを使用して、変数に PSCredential を作成し `Get-Credential` ます。 次 `Export-Clixml` に、を使用して、資格情報をディスクに保存します。

> [!IMPORTANT]
> `Export-Clixml` 暗号化された資格情報のみを Windows でエクスポートします。 MacOS や Linux などの Windows 以外のオペレーティングシステムでは、資格情報は Unicode 文字配列として格納されたプレーンテキストとしてエクスポートされます。 これにより、一部の難読化が可能になりますが、暗号化は提供されません。

```powershell
PS> $Credential = Get-Credential

PowerShell credential request
Enter your credentials.
User: User1
Password for user User1: ********

PS> $Credential | Export-Clixml ./cred2.xml
PS> Get-Content ./cred2.xml

...
    <Props>
      <S N="UserName">User1</S>
      <SS N="Password">700061007300730077006f0072006400</SS>
    </Props>
...

PS> 'password' | Format-Hex -Encoding unicode

   Label: String (System.String) <52D60C91>

          Offset Bytes                                           Ascii
                 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F
          ------ ----------------------------------------------- -----
0000000000000000 70 00 61 00 73 00 73 00 77 00 6F 00 72 00 64 00 p a s s w o r d
```

この例のの出力は、 `Get-Content` XML ファイル内の資格情報に焦点を合わせて切り捨てられています。 パスワードのプレーンテキスト値は、によって証明される Unicode 文字配列として XML ファイルに格納されることに注意して `Format-Hex` ください。 そのため、値はエンコードされますが、暗号化されません。

## PARAMETERS

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -深さ

XML 表現に含める子オブジェクトのレベルを指定します。 既定値は `2` です。

ファイルのオブジェクトの種類に対して既定値をオーバーライドでき `Types.ps1xml` ます。 詳細については、「 [about_Types.ps1xml](../Microsoft.PowerShell.Core/About/about_Types.ps1xml.md)」を参照してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 2
Accept pipeline input: False
Accept wildcard characters: False
```

### -Encoding

ターゲット ファイルのエンコードの種類を指定します。 既定値は `utf8NoBOM` です。

このパラメーターに指定できる値は次のとおりです。

- `ascii`: ASCII (7 ビット) 文字セットのエンコーディングを使用します。
- `bigendianunicode`: ビッグエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `bigendianutf32`: ビッグエンディアンのバイト順を使用して、32 UTF-8 形式でエンコードします。
- `oem`: MS-DOS およびコンソールプログラムの既定のエンコードを使用します。
- `unicode`: リトルエンディアンのバイト順を使用して UTF-16 形式でエンコードします。
- `utf7`: UTF-7 形式でエンコードします。
- `utf8`: UTF-8 形式でエンコードします。
- `utf8BOM`: バイト順マーク (BOM) を使用して UTF-8 形式でエンコードします。
- `utf8NoBOM`: バイトオーダーマーク (BOM) を使用せずに UTF-8 形式でエンコードします。
- `utf32`:32 UTF-8 形式でエンコードします。

PowerShell 6.2 以降では、 **Encoding** パラメーターを使用して、登録されているコードページの数値 id (など) `-Encoding 1251` や、登録されているコードページの文字列名 (など) を使用することもでき `-Encoding "windows-1251"` ます。 詳細については、 [コードページ](/dotnet/api/system.text.encoding.codepage?view=netcore-2.2)の .net ドキュメントを参照してください。

> [!NOTE]
> **Utf-7** _ の使用は推奨されなくなりました。 PowerShell 7.1 では、 `utf7` _ *Encoding** パラメーターにを指定すると、警告が書き込まれます。

```yaml
Type: System.Text.Encoding
Parameter Sets: (All)
Aliases:
Accepted values: ASCII, BigEndianUnicode, BigEndianUTF32, OEM, Unicode, UTF7, UTF8, UTF8BOM, UTF8NoBOM, UTF32

Required: False
Position: Named
Default value: UTF8NoBOM
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

必要に応じて、このコマンドレットは出力ファイルの読み取り専用の属性をクリアします。 コマンドが完了すると、このコマンドレットが読み取り専用の属性をリセットしようとします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

変換するオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。 パイプを使用してオブジェクトをにパイプ処理することもでき `Export-Clixml` ます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

オブジェクトの XML 表現が格納されるファイルへのパスを指定します。 **Path** と異なり、 **LiteralPath** パラメーターの値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoClobber

コマンドレットが既存のファイルの内容を上書きしないことを示します。 既定では、指定されたパスにファイルが存在する場合、は `Export-Clixml` 警告なしにファイルを上書きします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NoOverwrite

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

オブジェクトの XML 表現が格納されるファイルへのパスを指定します。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

任意のオブジェクトをにパイプラインでき `Export-Clixml` ます。

## 出力

### システム. IO. FileInfo

`Export-Clixml` XML を含むファイルを作成します。

## 注

## 関連リンク

[ConvertTo-Html](ConvertTo-Html.md)

[ConvertTo-Xml](ConvertTo-Xml.md)

[エクスポート-Csv](Export-Csv.md)

[Import-Clixml](Import-Clixml.md)

[Join-Path](../Microsoft.PowerShell.Management/Join-Path.md)

[ディスクへの資格情報の安全な保存](https://powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)

[PowerShell を使用してレガシシステムに資格情報を渡す](https://devblogs.microsoft.com/scripting/use-powershell-to-pass-credentials-to-legacy-systems/)

[Windows.Security.Cryptography.DataProtection](/uwp/api/windows.security.cryptography.dataprotection)
