---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/export-modulemember?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-ModuleMember
ms.openlocfilehash: fcb9af654f9e95f44e49ea984294b80752aa3453
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600586"
---
# Export-ModuleMember

## 概要
エクスポートするモジュール メンバーを指定します。

## SYNTAX

```
Export-ModuleMember [[-Function] <String[]>] [-Cmdlet <String[]>] [-Variable <String[]>] [-Alias <String[]>]
 [<CommonParameters>]
```

## Description

**Export-modulemember** コマンドレットは、スクリプトモジュール (hbase-runner.psm1) ファイル、または New-Module コマンドレットを使用して作成された動的モジュールからエクスポートされるモジュールメンバーを指定します。
モジュールメンバーには、コマンドレット、関数、変数、およびエイリアスが含まれます。
このコマンドレットは、スクリプト モジュール ファイルまたは動的モジュールでのみ使用できます。

スクリプトモジュールに **export-modulemember** コマンドが含まれていない場合、スクリプトモジュール内の関数と別名はエクスポートされますが、変数はエクスポートされません。
スクリプトモジュールに **export-modulemember** コマンドが含まれている場合は、 **export-modulemember** コマンドで指定されたメンバーのみがエクスポートされます。
また、 **export-modulemember** を使用して、スクリプトモジュールが他のモジュールからインポートしたメンバーを抑制またはエクスポートすることもできます。

**Export-modulemember** コマンドは省略可能ですが、ベストプラクティスです。
コマンドが既定値を確認する場合でも、モジュールの作成者の意図は示されます。

## 例

### 例 1: スクリプトモジュールで関数とエイリアスをエクスポートする

```powershell
Export-ModuleMember -Function * -Alias *
```

このコマンドは、スクリプトモジュールで定義されているすべての関数とエイリアスをエクスポートします。

### 例 2: 特定のエイリアスと関数をエクスポートする

```powershell
Export-ModuleMember -Function Get-Test, New-Test, Start-Test -Alias gtt, ntt, stt
```

このコマンドは、スクリプト モジュールで定義されている 3 つのエイリアスと 3 つの関数をエクスポートします。

このコマンド形式を使用すると、モジュール メンバーの名前を指定できます。

### 例 3: メンバーをエクスポートしない

```powershell
Export-ModuleMember
```

このコマンドは、スクリプト モジュールで定義されているメンバーはエクスポートされないことを指定します。

このコマンドでは、モジュール メンバーをエクスポートできませんが、メンバーは非表示になりません。
ユーザーは、モジュール メンバーの読み取りとコピー、または、呼び出し演算子 (&) を使用してエクスポートされていないモジュール メンバーを起動することができます。

### 例 4: 特定の変数をエクスポートする

```powershell
Export-ModuleMember -Variable increment
```

このコマンドは、スクリプト モジュールから $increment 変数のみをエクスポートします。
他のメンバーはエクスポートされません。

変数をエクスポートする場合、モジュール内の関数をエクスポートするだけでなく、 **export-modulemember** コマンドには、すべての関数の名前と変数の名前を含める必要があります。

### 例 5: 複数のエクスポートコマンド

```powershell
# From TestModule.psm1
Function New-Test
{
    Write-Output 'I am New-Test function'
}
Export-ModuleMember -Function New-Test

function Validate-Test
{
    Write-Output 'I am Validate-Test function'
}
function Start-Test
{
    Write-Output 'I am Start-Test function'
}
Set-Alias stt Start-Test
Export-ModuleMember -Function Start-Test -Alias stt
```

これらのコマンドは、スクリプトモジュール (hbase-runner.psm1) ファイルで複数の **export-modulemember** コマンドがどのように解釈されるかを示しています。

これらのコマンドは、3 つの関数と 1 つのエイリアスを作成し、このうちの 2 つの関数とエイリアスをエクスポートします。

**Export-modulemember** コマンドを使用しない場合、3つの関数とエイリアスのすべてがエクスポートされます。
**Export-modulemember** コマンドでは、**新しい-TEST** **関数と** STT エイリアスだけがエクスポートされます。

### 例 6: 動的モジュールのメンバーをエクスポートする

```powershell
New-Module -Script {function SayHello {"Hello!"}; Set-Alias Hi SayHello; Export-ModuleMember -Alias Hi -Function SayHello}
```

このコマンドは、 **新しいモジュール** コマンドレットを使用して作成された動的モジュールで Export-ModuleMember を使用する方法を示しています。

この例では、 **export-modulemember** を使用して、動的モジュールの Hi エイリアスと **SayHello** 関数の両方をエクスポートします。

### 例 7: 1 つのコマンドで関数を宣言およびエクスポートする

```powershell
# From TestModule.psm1
function Export
{
  param (
    [Parameter(Mandatory=$true)]
    [ValidateSet("function","variable")]
    $Type,
    [Parameter(Mandatory=$true)]
    $Name,
    [Parameter(Mandatory=$true)]
    $Value
    )

    if ($Type -eq "function")
    {
        Set-item "function:script:$Name" $Value
        Export-ModuleMember $Name
    }
    else
    {
    Set-Variable -scope Script $Name $Value
    Export-ModuleMember -variable $Name
    }
}

Export function New-Test {Write-Output 'I am New-Test function'}
function helper {Write-Output 'I am helper function'}

Export variable Interval 0
$Interval = 2
```

この例には、関数を宣言したり変数を作成したりする **Export** という名前の関数が含まれています。その後、関数 `Export-ModuleMember` または変数に対してコマンドを記述します。
これにより、1 つのコマンドで関数または変数を宣言し、エクスポートできます。

**Export** 関数を使用するには、スクリプトモジュールにそれを含めます。
関数をエクスポートするには、 `Export` **function** キーワードの前に「」と入力します。

変数をエクスポートするには、次の形式を使用して変数を宣言し、その値を設定します。

`Export variable <variable-name> <value>`

例では、コマンドは正しい形式を示しています。
この例では、 **新しい-Test** 関数と $Interval 変数のみがエクスポートされます。

## PARAMETERS

### -エイリアス

スクリプト モジュール ファイルからエクスポートされるエイリアスを指定します。
エイリアス名を入力します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -コマンドレット

スクリプト モジュール ファイルからエクスポートされるコマンドレットを指定します。
コマンドレット名を入力します。
ワイルドカード文字を使用できます。

コマンドレットをスクリプト モジュール ファイルに作成することはできませんが、バイナリ モジュールからスクリプト モジュールにコマンドレットをインポートし、その後スクリプト モジュールからコマンドレットを再度エクスポートできます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -関数

スクリプト モジュール ファイルからエクスポートされる関数を指定します。
関数名を入力します。
ワイルドカード文字を使用できます。
パイプを使用して関数名の文字列を **export-modulemember** にパイプ処理することもできます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -Variable

スクリプト モジュール ファイルからエクスポートされる変数を指定します。
ドル記号を付けずに変数名を入力します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して関数名の文字列をこのコマンドレットに渡します。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

* エクスポートされたメンバーの一覧からメンバーを除外するには、 **export-modulemember** コマンドを追加します。このコマンドは、他のすべてのメンバーを一覧表示し、除外するメンバーを除外します。

## 関連リンク

[Get-Module](Get-Module.md)

[Import-Module](Import-Module.md)

[Remove-Module](Remove-Module.md)

