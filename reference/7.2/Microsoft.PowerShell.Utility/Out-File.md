---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/21/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/out-file?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-File
ms.openlocfilehash: e3a066957ab1e6935049003f8ccf55aee8bb7c94
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600797"
---
# Out-File

## 概要
ファイルに出力を送信します。

## SYNTAX

### ByPath (既定値)

```
Out-File [-FilePath] <string> [[-Encoding] <Encoding>] [-Append] [-Force] [-NoClobber]
 [-Width <int>] [-NoNewline] [-InputObject <psobject>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Out-File [[-Encoding] <Encoding>] -LiteralPath <string> [-Append] [-Force] [-NoClobber]
 [-Width <int>] [-NoNewline] [-InputObject <psobject>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Out-File`コマンドレットは、出力をファイルに送信します。 ファイルへの書き込みには、PowerShell の書式設定システムを暗黙的に使用します。 このファイルは、ターミナルと同じ表示表現を受け取ります。 これは、すべての入力オブジェクトが文字列でない限り、プログラムによる処理には適していない可能性があることを意味します。
出力のパラメーターを指定する必要がある場合は、 `Out-File` リダイレクト演算子 () ではなくを使用し `>` ます。 リダイレクトの詳細については、「 [about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)」を参照してください。

## 例

### 例 1: 出力を送信し、ファイルを作成する

この例では、ローカルコンピューターのプロセスの一覧をファイルに送信する方法を示します。 ファイルが存在しない場合は、 `Out-File` 指定されたパスにファイルを作成します。

```powershell
Get-Process | Out-File -FilePath .\Process.txt
Get-Content -Path .\Process.txt
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     29    22.39      35.40      10.98   42764   9 Application
     53    99.04     113.96       0.00   32664   0 CcmExec
     27    96.62     112.43     113.00   17720   9 Code
```

`Get-Process`コマンドレットは、ローカルコンピューター上で実行されているプロセスの一覧を取得します。 **プロセス** オブジェクトは、パイプラインを介してコマンドレットに送信され `Out-File` ます。 `Out-File`**FilePath** パラメーターを使用して、 **Process.txt** という名前の現在のディレクトリにファイルを作成します。 `Get-Content`このコマンドは、ファイルからコンテンツを取得し、PowerShell コンソールに表示します。

### 例 2: 既存のファイルが上書きされないようにする

この例では、既存のファイルが上書きされないようにします。 既定では、は `Out-File` 既存のファイルを上書きします。

```powershell
Get-Process | Out-File -FilePath .\Process.txt -NoClobber
```

```Output
Out-File : The file 'C:\Test\Process.txt' already exists.
At line:1 char:15
+ Get-Process | Out-File -FilePath .\Process.txt -NoClobber
+               ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

`Get-Process`コマンドレットは、ローカルコンピューター上で実行されているプロセスの一覧を取得します。 **プロセス** オブジェクトは、パイプラインを介してコマンドレットに送信され `Out-File` ます。 `Out-File`**FilePath** パラメーターを使用し、 **Process.txt** という名前の現在のディレクトリ内のファイルへの書き込みを試みます。 **NoClobber** パラメーターを指定すると、ファイルが上書きされなくなり、ファイルが既に存在することを示すメッセージが表示されます。

### 例 3: 出力を ASCII 形式でファイルに送信する

この例では、特定のエンコードの種類を使用して出力をエンコードする方法を示します。

```powershell
$Procs = Get-Process
Out-File -FilePath .\Process.txt -InputObject $Procs -Encoding ASCII -Width 50
```

`Get-Process`コマンドレットは、ローカルコンピューター上で実行されているプロセスの一覧を取得します。 **プロセス** オブジェクトは変数に格納され `$Procs` ます。 `Out-File`**FilePath** パラメーターを使用して、 **Process.txt** という名前の現在のディレクトリにファイルを作成します。 **InputObject** パラメーターは、プロセスオブジェクトを `$Procs` **Process.txt** ファイルに渡します。 **Encoding** パラメーターは、出力を **ASCII** 形式に変換します。 **Width** パラメーターを指定すると、ファイル内の各行が50文字に制限されるため、一部のデータが切り捨てられる可能性があります。

### 例 4: プロバイダーを使用し、ファイルに出力を送信する

この例では、 `Out-File` **ファイルシステム** プロバイダードライブにない場合にコマンドレットを使用する方法を示します。 コマンドレットを使用して、 `Get-PSProvider` ローカルコンピューター上のプロバイダーを表示します。 詳細については、「[about_Providers](../Microsoft.Powershell.Core/About/about_Providers.md)」を参照してください。

```
PS> Set-Location -Path Alias:

PS> Get-Location

Path
----
Alias:\

PS> Get-ChildItem | Out-File -FilePath C:\TestDir\AliasNames.txt

PS> Get-Content -Path C:\TestDir\AliasNames.txt

CommandType     Name
-----------     ----
Alias           % -> ForEach-Object
Alias           ? -> Where-Object
Alias           ac -> Add-Content
Alias           cat -> Get-Content
```

この `Set-Location` コマンドは **Path** パラメーターを使用して、現在の場所をレジストリプロバイダーに設定し `Alias:` ます。 コマンドレットにより、 `Get-Location` の完全なパスが表示され `Alias:` ます。
`Get-ChildItem` オブジェクトをパイプラインからコマンドレットに送信し `Out-File` ます。 `Out-File`**FilePath** パラメーターを使用して、出力の完全なパスとファイル名 ( **C:\TestDir\AliasNames.txt**) を指定します。 コマンドレットでは、 `Get-Content` **Path** パラメーターを使用して、ファイルの内容を PowerShell コンソールに表示します。

## PARAMETERS

### -追加

既存のファイルの末尾に出力を追加します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
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
Position: 1
Default value: UTF8NoBOM
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

出力ファイルのパスを指定します。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases: Path

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

読み取り専用の属性をオーバーライドし、既存の読み取り専用ファイルを上書きします。 **Force** パラメーターは、セキュリティ制限をオーバーライドしません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

ファイルに書き込むオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

出力ファイルのパスを指定します。 **LiteralPath** パラメーターは、入力されたとおりに使用されます。
ワイルドカード文字は使用できません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。 詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NoClobber

**NoClobber** を実行すると、既存のファイルが上書きされず、ファイルが既に存在することを示すメッセージが表示されます。 既定では、指定されたパスにファイルが存在する場合、は `Out-File` 警告なしにファイルを上書きします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NoOverwrite

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -なし Wline

ファイルに書き込まれる内容が改行文字で終了しないことを指定します。 入力オブジェクトの文字列形式が連結され、出力が形成されます。 出力文字列間にスペースや改行は挿入されません。 最後の出力文字列の後に改行は追加されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Width

出力の各行の文字数を指定します。 これを超過する文字は、折り返されることなく切り捨てられます。 このパラメーターを使用しない場合、幅はホストの特性によって決まります。 PowerShell コンソールの既定値は80文字です。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

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

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

パイプを使用して任意のオブジェクトをにパイプすることができ `Out-File` ます。

## 出力

### なし

`Out-File` は出力を生成しません。

## 注

入力オブジェクトは、ターミナルのように自動的に書式設定されますが、コマンドレットを使用して、 `Format-*` 出力の形式をファイルに明示的に制御できます。 たとえば、`Get-Date | Format-List | Out-File out.txt`

PowerShell コマンドの出力をコマンドレットに送信するには、 `Out-File` パイプラインを使用します。 または、変数にデータを格納し、 **InputObject** パラメーターを使用してデータをコマンドレットに渡すこともでき `Out-File` ます。

`Out-File` データをファイルに保存しますが、パイプラインに出力オブジェクトを生成しません。

## 関連リンク

[about_Providers](../Microsoft.Powershell.Core/About/about_Providers.md)

[about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)

[Out-Default](../Microsoft.PowerShell.Core/Out-Default.md)

[Out-Host](../Microsoft.PowerShell.Core/Out-Host.md)

[Out-Null](../Microsoft.PowerShell.Core/Out-Null.md)

[Out-String](Out-String.md)

[Tee-Object](Tee-Object.md)

