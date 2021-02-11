---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-formatdata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-FormatData
ms.openlocfilehash: d42b108d469ed8989628a6c0b4214af4afc0db00
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599292"
---
# Export-FormatData

## 概要
現在のセッションの書式設定データを書式設定ファイルに保存します。

## SYNTAX

### ByPath (既定値)

```
Export-FormatData -InputObject <ExtendedTypeDefinition[]> -Path <String> [-Force] [-NoClobber]
 [-IncludeScriptBlock] [<CommonParameters>]
```

### ByLiteralPath

```
Export-FormatData -InputObject <ExtendedTypeDefinition[]> -LiteralPath <String> [-Force] [-NoClobber]
 [-IncludeScriptBlock] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Export-FormatData` 現在のセッションの書式設定オブジェクトから PowerShell 書式設定ファイル (format.ps1xml) を作成します。 これは、を返して XML 形式のファイルに保存する **Extendedtypedefinition** オブジェクトを取得 `Get-FormatData` します。

PowerShell は、書式設定ファイルのデータ (format.ps1xml) を使用して、セッション内の Microsoft .NET Framework オブジェクトの既定の表示を生成します。 書式設定ファイルは表示および編集することができます。書式設定データをセッションに追加するには、Update-FormatData コマンドレットを使用します。

PowerShell でのファイルの書式設定の詳細については、「 [about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)」を参照してください。

## 例

### 例 1: セッション形式のデータをエクスポートする

```powershell
Get-FormatData -TypeName "*" | Export-FormatData -Path "allformat.ps1xml" -IncludeScriptBlock
```

このコマンドは、セッション内のすべての書式設定データを AllFormat.ps1xml ファイルにエクスポートします。

コマンドは、コマンドレットを使用して、 `Get-FormatData` セッションの形式データを取得します。 `*` **TypeName** パラメーターの値 (all) は、セッション内のすべてのデータを取得するようにコマンドレットに指示します。

このコマンドは、パイプライン演算子 () を使用して、 `|` コマンドからの形式データを `Get-FormatData` コマンドレットに送信し `Export-FormatData` ます。これにより、フォーマットデータが AllFormat.ps1 ファイルにエクスポートされます。

この `Export-FormatData` コマンドは、 **IncludeScriptBlock** パラメーターを使用して、ファイルの形式データにスクリプトブロックを含めます。

### 例 2: 型の書式データをエクスポートする

```powershell
$F = Get-FormatData -TypeName "helpinfoshort"
Export-FormatData -InputObject $F -Path "c:\test\help.format.ps1xml" -IncludeScriptBlock
```

これらのコマンドは、 **Helpinfoshort** 型の形式データを Help.format.ps1xml ファイルにエクスポートします。

最初のコマンドは、コマンドレットを使用して `Get-FormatData` **Helpinfoshort** 型の書式データを取得し、変数に保存し `$F` ます。

2番目のコマンドは、コマンドレットの **InputObject** パラメーターを使用し `Export-FormatData` て、変数に保存された形式のデータを入力し `$F` ます。 また、 **IncludeScriptBlock** パラメーターを使用して、出力にスクリプトブロックを含めます。

### 例 3: スクリプトブロックを使用せずに書式設定データをエクスポートする

```powershell
Get-FormatData -TypeName "System.Diagnostics.Process" | Export-FormatData -Path process.format.ps1xml
Update-FormatData -PrependPath ".\process.format.ps1xml"
Get-Process p*
```

```Output
Handles  NPM(K)  PM(K)  WS(K) VM(M)   CPU(s)    Id ProcessName
-------  ------  -----  ----- -----   ------    -- -----------
323                                       5600 powershell
336                                       3900 powershell_ise
138                                       4076 PresentationFontCache
```

この例は、コマンドから **IncludeScriptBlock** パラメーターを省略した場合の効果を示して `Export-FormatData` います。

最初のコマンドは、コマンドレットを使用して、 `Get-FormatData` Get-Process コマンドレットが返す **システム** の形式のデータを取得します。 このコマンドは、パイプライン演算子 () を使用して、 `|` 書式設定データをコマンドレットに送信します。これにより、 `Export-FormatData` 現在のディレクトリ内の Process.format.ps1xml ファイルにエクスポートされます。

この場合、コマンドでは `Export-FormatData` **IncludeScriptBlock** パラメーターが使用されません。

2番目のコマンドは、コマンドレットを使用して、 `Update-FormatData` 現在のセッションに Process.format.ps1xml ファイルを追加します。 このコマンドは、 **PrependPath** パラメーターを使用して、プロセスオブジェクトの標準の書式設定データの前に Process.format.ps1xml ファイル内のプロセスオブジェクトの書式設定データが確実に見つかるようにします。

3 番目のコマンドにより、この変更の効果が示されています。 コマンドは、コマンドレットを使用して、 `Get-Process` P で始まる名前を持つプロセスを取得します。出力には、スクリプトブロックを使用して計算されたプロパティ値が表示されないことが示されています。

## PARAMETERS

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

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

### -IncludeScriptBlock

書式設定データのスクリプトブロックをエクスポートするかどうかを示します。

中に含まれるコードが悪用される可能性があるため、既定ではスクリプト ブロックはエクスポートされません。

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

エクスポートする書式設定データ オブジェクトを指定します。 オブジェクトを含む変数、またはコマンドなどのオブジェクトを取得するコマンドを入力し `Get-FormatData` ます。 オブジェクトをからにパイプすることもでき `Get-FormatData` `Export-FormatData` ます。

```yaml
Type: System.Management.Automation.ExtendedTypeDefinition[]
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

出力ファイルの場所を指定します。 **Path** パラメーターと異なり、**LiteralPath** の値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

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

コマンドレットが既存のファイルを上書きしないことを示します。 既定では、 `Export-FormatData` ファイルに読み取り専用属性が含まれていない限り、は警告なしにファイルを上書きします。

読み取り専用ファイルを上書きするように指定するには `Export-FormatData` 、 **Force** パラメーターを使用します。

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

### -Path

出力ファイルの場所を指定します。
パス (省略可能) と format.ps1xml ファイル名拡張子が付いたファイル名を入力します。
パスを省略した場合、は `Export-FormatData` 現在のディレクトリにファイルを作成します。

Types.ps1xml 以外のファイル名拡張子を使用する場合、この `Update-FormatData` コマンドレットはファイルを認識しません。

既存のファイルを指定すると、ファイルに `Export-FormatData` 読み取り専用属性が含まれていない限り、は警告なしにファイルを上書きします。 読み取り専用ファイルを上書きするには、 **Force** パラメーターを使用します。 ファイルが上書きされないようにするには、 **NoClobber** パラメーターを使用します。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases: FilePath

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システムの管理. ExtendedTypeDefinition

パイプを使用して、 **Extendedtypedefinition** オブジェクトをからにパイプすることができ `Get-FormatData` `Export-FormatData` ます。

## 出力

### なし

`Export-FormatData` はオブジェクトを返しません。
ファイルを生成し、指定されたパス内に保存します。

## 注

- エクスポートされた書式設定ファイルを含む任意の書式設定ファイルを使用するには、セッションの実行ポリシーでスクリプトと構成ファイルの実行が許可されている必要があります。 詳細については、「[about_Execution_Policies](../Microsoft.PowerShell.Core/About/about_Execution_Policies.md)」を参照してください。

## 関連リンク

[Get-FormatData](Get-FormatData.md)

[Update-FormatData](Update-FormatData.md)
