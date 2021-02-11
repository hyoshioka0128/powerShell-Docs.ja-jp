---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 03/12/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-unique?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Unique
ms.openlocfilehash: de827d956e6e813e84d87bb1578ee7d596fe2f15
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602413"
---
# Get-Unique

## 概要
並べ替えられたリストから一意の項目を返します。

## SYNTAX

### AsString (既定値)

```
Get-Unique [-InputObject <PSObject>] [-AsString] [<CommonParameters>]
```

### UniqueByType

```
Get-Unique [-InputObject <PSObject>] [-OnType] [<CommonParameters>]
```

## Description

`Get-Unique`コマンドレットは、並べ替えられたリスト内の各項目を次の項目と比較し、重複を除去して、各項目の1つのインスタンスのみを返します。 コマンドレットが適切に機能するためにはリストを並べ替える必要があります。

`Get-Unique` では、大文字と小文字が区別されます。 その結果、大文字と小文字のみが異なる文字列は一意であると見なされます。

## 例

### 例 1: テキストファイル内の一意の単語を取得する

これらのコマンドは、テキスト ファイルの一意の単語の数を調べます。

```powershell
$A = $( foreach ($line in Get-Content C:\Test1\File1.txt) {
    $line.tolower().split(" ")
  }) | Sort-Object | Get-Unique
$A.count
```

最初のコマンドは、File.txt ファイルの内容を取得します。 各行のテキストが小文字に変換された後、スペース (" ") を区切りとしてそれぞれの単語が別個の行に配置されます。 次に、結果の一覧をアルファベット順に並べ替え (既定)、コマンドレットを使用して `Get-Unique` 重複する単語を削除します。 結果は変数に格納され `$A` ます。

2番目のコマンドは、内の文字列のコレクションの **Count** プロパティを使用して、 `$A` に含まれる項目の数を確認し `$A` ます。

### 例 2: 配列内の一意の整数を取得する

このコマンドは、整数のセットの一意のメンバーを取得します。

```powershell
1,1,1,1,12,23,4,5,4643,5,3,3,3,3,3,3,3 | Sort-Object | Get-Unique
```

```Output
1
3
4
5
12
23
4643
```

最初のコマンドは、コマンドラインで入力された整数の配列を受け取り、それらをコマンドレットにパイプ `Sort-Object` して並べ替え、にパイプ処理し `Get-Unique` ます。これにより、エントリが重複しなくなります。

### 例 3: ディレクトリ内の一意のオブジェクトの種類を取得する

このコマンドは、 `Get-ChildItem` コマンドレットを使用して、ファイルとディレクトリを含むローカルディレクトリの内容を取得します。

```powershell
Get-ChildItem | Sort-Object {$_.GetType()} | Get-Unique -OnType
```

パイプライン演算子 (|) によって結果がコマンドレットに送信され `Sort-Object` ます。 この `$_.GetType()` ステートメントは、 **GetType** メソッドを各ファイルまたはディレクトリに適用します。 次に、は `Sort-Object` 型で項目を並べ替えます。 もう1つのパイプライン演算子は、結果をに送信 `Get-Unique` します。 **Ontype** パラメーターは、 `Get-Unique` 各型のオブジェクトを1つだけ返すように指示します。

### 例 4: 一意のプロセスを取得する

このコマンドは、コンピューター上で実行されているプロセスの名前を取得し、重複を削除してその結果を返します。

```powershell
Get-Process | Sort-Object | Select-Object processname | Get-Unique -AsString
```

コマンドは、 `Get-Process` コンピューター上のすべてのプロセスを取得します。 パイプライン演算子 (|) は、結果をに渡し `Sort-Object` ます。既定では、ProcessName によってプロセスがアルファベット順に並べ替えられます。 結果はパイプを使用してコマンドレットに渡され、 `Select-Object` 各オブジェクトの ProcessName プロパティの値のみが選択されます。 結果は、にパイプ処理されて、重複が除去され `Get-Unique` ます。

**Asstring** パラメーターは、 `Get-Unique` **ProcessName** 値を文字列として扱うように指示します。
このパラメーターを指定しない場合、は `Get-Unique` **ProcessName** 値をオブジェクトとして扱い、オブジェクトの1つのインスタンス (つまりリスト内の最初のプロセス名) のみを返します。

## PARAMETERS

### -AsString

このコマンドレットがデータを文字列として使用することを示します。 このパラメーターを指定しないと、データはオブジェクトとして扱われるため、ファイルのコレクションなど、同じ型のオブジェクトのコレクションをに送信すると、 `Get-Unique` 1 つだけが返されます (最初の)。 このパラメーターを使用すると、ファイル名などのオブジェクトのプロパティの一意の値を検索できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AsString
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

の入力を指定し `Get-Unique` ます。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

このコマンドレットは、 **InputObject** を使用して送信された入力をコレクションとして扱います。 コレクション内の個々の項目は列挙されません。 コレクションは1つの項目であるため、 **InputObject** を使用して送信された入力は常に変更されずに返されます。

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

### -OnType

このコマンドレットが各型のオブジェクトを1つだけ返すことを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: UniqueByType
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意の型のオブジェクトをにパイプすることができ `Get-Unique` ます。

## 出力

### システム管理. PSObject

が返すオブジェクトの型 `Get-Unique` は、入力によって決定されます。

## 注

また、組み込みエイリアスであるを参照することもでき `Get-Unique` `gu` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

リストを並べ替えるには、Sort-Object を使用します。 また、の **unique** パラメーターを使用して、 `Sort-Object` リスト内の一意の項目を検索することもできます。

## 関連リンク

[Select-Object](Select-Object.md)

[Sort-Object](Sort-Object.md)

