---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/15/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/import-clixml?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Import-Clixml
ms.openlocfilehash: 3658ea5eded0fed95a1c9a1ea6e7d84a557e1e36
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602216"
---
# Import-Clixml

## 概要
EXPORT-CLIXML ファイルをインポートし、対応するオブジェクトを PowerShell に作成します。

## SYNTAX

### ByPath (既定値)

```
Import-Clixml [-Path] <String[]> [-IncludeTotalCount] [-Skip <UInt64>] [-First <UInt64>]
 [<CommonParameters>]
```

### ByLiteralPath

```
Import-Clixml -LiteralPath <String[]> [-IncludeTotalCount] [-Skip <UInt64>] [-First <UInt64>]
 [<CommonParameters>]
```

## Description

この `Import-Clixml` コマンドレットは、Microsoft .NET Framework オブジェクトを表すデータを含む共通言語基盤 (CLI) XML ファイルをインポートし、PowerShell オブジェクトを作成します。 CLI の詳細については、「 [言語に依存](/dotnet/standard/language-independence)しない」を参照してください。

Windows コンピューターでを使用する場合 `Import-Clixml` は、資格情報をインポートし、を使用してセキュリティで保護された XML としてエクスポートした文字列をセキュリティで保護することが重要です `Export-Clixml` 。 例については、例2を参照してください。

`Import-Clixml` バイト順マーク (BOM) を使用して、ファイルのエンコード形式を検出します。 ファイルに BOM がない場合は、エンコードが UTF8 であると想定されます。

## 例

### 例 1: シリアル化されたファイルをインポートしてオブジェクトを再作成する

この例では、 `Export-Clixml` コマンドレットを使用して、によって返されるプロセス情報のシリアル化されたコピーを保存し `Get-Process` ます。 `Import-Clixml` シリアル化されたファイルの内容を取得し、変数に格納されているオブジェクトを再作成し `$Processes` ます。

```powershell
Get-Process | Export-Clixml -Path .\pi.xml
$Processes = Import-Clixml -Path .\pi.xml
```

### 例 2: セキュリティで保護された資格情報オブジェクトをインポートする

この例では、コマンドレットを実行して変数に格納した資格情報を指定した場合、 `$Credential` `Get-Credential` コマンドレットを実行して `Export-Clixml` 資格情報をディスクに保存できます。

> [!IMPORTANT]
> `Export-Clixml` 暗号化された資格情報のみを Windows でエクスポートします。 MacOS や Linux などの Windows 以外のオペレーティングシステムでは、資格情報はプレーンテキストでエクスポートされます。

```powershell
$Credxmlpath = Join-Path (Split-Path $Profile) TestScript.ps1.credential
$Credential | Export-Clixml $Credxmlpath
$Credxmlpath = Join-Path (Split-Path $Profile) TestScript.ps1.credential
$Credential = Import-Clixml $Credxmlpath
```

コマンドレットでは、 `Export-Clixml` Windows [データ保護 API](/previous-versions/windows/apps/hh464970(v=win.10))を使用して資格情報オブジェクトを暗号化します。
暗号化により、ユーザーアカウントのみが資格情報オブジェクトの内容を復号化できるようになります。 エクスポートされたファイルは、 `CLIXML` 別のコンピューターまたは別のユーザーが使用することはできません。

この例では、資格情報が格納されているファイルがによって表され `TestScript.ps1.credential` ます。 **Testscript** を、資格情報の読み込みに使用するスクリプトの名前に置き換えます。

資格情報オブジェクトをパイプラインの下に送信 `Export-Clixml` し、 `$Credxmlpath` 最初のコマンドで指定したパスに保存します。

スクリプトに資格情報を自動的にインポートするには、最後の2つのコマンドを実行します。 を実行し `Import-Clixml` て、セキュリティで保護された資格情報オブジェクトをスクリプトにインポートします。 このインポートにより、スクリプトにプレーンテキストのパスワードが公開されるリスクがなくなります。

## PARAMETERS

### -最初

指定された数のオブジェクトのみを取得します。 取得するオブジェクトの数を入力します。

```yaml
Type: System.UInt64
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -IncludeTotalCount

データセット内のオブジェクトのうち、選択したオブジェクトの合計数を報告します。 コマンドレットが合計数を判別できない場合は、[ **不明な合計数**] と表示されます。 整数には、合計数の値の信頼性を示す " **精度** " プロパティがあります。 **精度** の範囲の値がからの場合は、 `0.0` `1.0` `0.0` コマンドレットがオブジェクトをカウントできなかったことを意味します。は、この `1.0` カウントが正確であることを意味し、との間の値は、より信頼性の高い推定値であること `0.0` `1.0` を示します。

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

### -LiteralPath

XML ファイルへのパスを指定します。 **Path** と異なり、 **LiteralPath** パラメーターの値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

XML ファイルへのパスを指定します。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Skip

指定された数のオブジェクトを無視してから、残りのオブジェクトを取得します。 スキップするオブジェクトの数を入力します。

```yaml
Type: System.UInt64
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

へのパスを含む文字列をパイプラインでき `Import-Clixml` ます。

## 出力

### PSObject

`Import-Clixml` 格納されている XML ファイルから逆シリアル化されたオブジェクトを返します。

## 注

パラメーターの複数の値を指定する場合は、コンマを使用して値を区切ります。 たとえば、「 `<parameter-name> <value1>, <value2>` 」のように入力します。

## 関連リンク

[Export-Clixml](Export-Clixml.md)

[XML シリアル化の概要](/dotnet/standard/serialization/introducing-xml-serialization)

[Join-Path](../Microsoft.PowerShell.Management/Join-Path.md)

[ディスクへの資格情報の安全な保存](https://powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk)

[PowerShell を使用してレガシシステムに資格情報を渡す](https://devblogs.microsoft.com/scripting/use-powershell-to-pass-credentials-to-legacy-systems/)

