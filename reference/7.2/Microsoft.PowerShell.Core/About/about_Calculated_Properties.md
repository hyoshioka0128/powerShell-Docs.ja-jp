---
description: PowerShell では、新しいプロパティを動的に追加したり、パイプラインに出力されるオブジェクトの書式を変更したりできます。
Locale: en-US
ms.date: 08/07/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_calculated_properties?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Calculated_Properties
ms.openlocfilehash: 1f7470a47a24b7c2b8265d180aac49cfec9031f6
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602421"
---
# <a name="about-calculated-properties"></a>計算されるプロパティについて

## <a name="short-description"></a>簡単な説明

PowerShell では、新しいプロパティを動的に追加したり、パイプラインに出力されるオブジェクトの書式を変更したりできます。

## <a name="long-description"></a>長い説明

いくつかの PowerShell コマンドレットは、出力オブジェクトへの新しいプロパティの追加を可能にするパラメーターを使用して、入力オブジェクトを出力オブジェクトに変換、集計、または処理します。 これらのパラメーターを使用すると、入力オブジェクトの値に基づいて、出力オブジェクトに対して新しい計算されるプロパティを生成できます。
計算プロパティは、新しいプロパティの名前、値を計算する式、および省略可能な書式情報を指定するキーと値のペアを含む [ハッシュテーブル](about_hash_tables.md) によって定義されます。

## <a name="supported-cmdlets"></a>サポートされているコマンドレット

次のコマンドレットは、 **property** パラメーターの計算されたプロパティ値をサポートしています。 コマンドレットでは、 `Format-*` **GroupBy** パラメーターの計算値もサポートされています。

次の一覧は、計算されたプロパティをサポートするコマンドレットと、各コマンドレットでサポートされているキーと値のペアを示しています。

- `Compare-Object`
  - `expression`

- `ConvertTo-Html`
  - `name`/`label` -省略可能 (PowerShell 6.x で追加)
  - `expression`
  - `width` -省略可能
  - `alignment` -省略可能

- `Format-Custom`
  - `expression`
  - `depth` -省略可能

- `Format-List`
  - `name`/`label` -省略可能
  - `expression`
  - `formatstring` -省略可能

  この同じキーと値のペアのセットは、すべてのコマンドレットの **GroupBy** パラメーターに渡される計算されるプロパティ値にも適用さ `Format-*` れます。

- `Format-Table`
  - `name`/`label` -省略可能
  - `expression`
  - `formatstring` -省略可能
  - `width` -省略可能
  - `alignment` -省略可能

- `Format-Wide`
  - `expression`
  - `formatstring` -省略可能

- `Group-Object`
  - `expression`

- `Measure-Object`
  - では、ハッシュテーブルではなく、式のスクリプトブロックのみがサポートされます。
  - PowerShell 5.1 以前ではサポートされていません。

- `Select-Object`
  - `name`/`label` -省略可能
  - `expression`

- `Sort-Object`
  - `expression`
  - `ascending`/`descending` -省略可能

> [!NOTE]
> の値は、 `expression` ハッシュテーブルではなく、スクリプトブロックにすることができます。 詳細については、「 [メモ](#notes) 」を参照してください。

## <a name="hashtable-key-definitions"></a>Hashtable キーの定義

- `name`/`label` -作成するプロパティの名前を指定します。 `name`またはそのエイリアスであるを使用でき `label` ます。
- `expression` -新しいプロパティの値を計算するために使用されるスクリプトブロック。
- `alignment` -列に値を表示する方法を定義するために表形式の出力を生成するコマンドレットによって使用されます。 値は、、 `'left'` 、またはである必要があり `'center'` `'right'` ます。
- `formatstring` -出力の値の書式設定方法を定義する書式指定文字列を指定します。 書式指定文字列の詳細については、「 [.net での書式設定](/dotnet/standard/base-types/formatting-types)」を参照してください。
- `width` -テーブル内の値が表示されるときの最大幅列を指定します。 値はよりも大きくする必要があり `0` ます。
- `depth` -の **depth** パラメーターは、 `Format-Custom` すべてのプロパティの展開の深さを指定します。 `depth`キーを使用すると、プロパティごとの展開の深さを指定できます。
- `ascending` / `descending` -1 つまたは複数のプロパティの並べ替え順序を指定できます。 これらはブール値です。

指定された名前プレフィックスが明確である限り、ハッシュテーブルキーのスペルを確認する必要はありません。 たとえば、は、 `n` の代わりに使用でき `Name` 、 `e` の代わりに使用でき `Expression` ます。

## <a name="examples"></a>例

### <a name="compare-object"></a>Compare-Object

計算されたプロパティを使用すると、入力オブジェクトのプロパティの比較方法を制御できます。 この例では、値を直接比較するのではなく、値を算術演算の結果と比較します (2 の剰余)。

```powershell
Compare-Object @{p=1} @{p=2} -property @{ Expression = { $_.p % 2 } }
```

```Output
 $_.p % 2  SideIndicator
---------- -------------
         0 =>
         1 <=
```

### <a name="convertto-html"></a>ConvertTo-Html

`ConvertTo-Html` オブジェクトのコレクションを HTML テーブルに変換できます。
計算されたプロパティを使用すると、テーブルの表示方法を制御できます。

```powershell
Get-Alias |
  ConvertTo-Html Name,
                 Definition,
                 @{
                    name='ParameterCount'
                    expr={$_.Parameters.Keys.Count}
                    align='center'
                 } |
    Out-File .\aliases.htm -Force
```

この例では、PowerShell エイリアスの一覧と、各エイリアス化されたコマンドの number パラメーターを含む HTML テーブルを作成します。 **Parametercount** 列の値は中央揃えになっています。

### <a name="format-custom"></a>Format-Custom

`Format-Custom` クラス定義と同様の形式で、オブジェクトのカスタムビューを提供します。 より複雑なオブジェクトには、複合型と深い入れ子になっているメンバーを含めることができます。 の **depth** パラメーターは、 `Format-Custom` すべてのプロパティの展開の深さを指定します。 `depth`キーを使用すると、プロパティごとの展開の深さを指定できます。

この例では、キーを使用すると、 `depth` コマンドレットのカスタム出力が簡略化され `Get-Date` ます。 `Get-Date`**DateTime** オブジェクトを返します。 このオブジェクトの **Date** プロパティも **DateTime** オブジェクトであるため、オブジェクトは入れ子になっています。

```powershell
Get-Date | Format-Custom @{expr={$_.Date};depth=1},TimeOfDay
```

```Output
class DateTime
{
  $_.Date =
    class DateTime
    {
      Date = 8/7/2020 12:00:00 AM
      Day = 7
      DayOfWeek = Friday
      DayOfYear = 220
      Hour = 0
      Kind = Local
      Millisecond = 0
      Minute = 0
      Month = 8
      Second = 0
      Ticks = 637323552000000000
      TimeOfDay = 00:00:00
      Year = 2020
      DateTime = Friday, August 07, 2020 12:00:00 AM
    }
  TimeOfDay =
    class TimeSpan
    {
      Ticks = 435031592302
      Days = 0
      Hours = 12
      Milliseconds = 159
      Minutes = 5
      Seconds = 3
      TotalDays = 0.503508787386574
      TotalHours = 12.0842108972778
      TotalMilliseconds = 43503159.2302
      TotalMinutes = 725.052653836667
      TotalSeconds = 43503.1592302
    }
}
```

### <a name="format-list"></a>Format-List

この例では、計算されたプロパティを使用して、出力の名前と形式をから変更し `Get-ChildItem` ます。

```powershell
Get-ChildItem *.json -File |
  Format-List Fullname,
              @{
                 name='Modified'
                 expression={$_.LastWriteTime}
                 formatstring='O'
              },
              @{
                 name='Size'
                 expression={$_.Length/1KB}
                 formatstring='N2'
              }
```

```Output
FullName : C:\Git\PS-Docs\PowerShell-Docs\.markdownlint.json
Modified : 2020-07-23T10:26:28.4092457-07:00
Size     : 2.40

FullName : C:\Git\PS-Docs\PowerShell-Docs\.openpublishing.publish.config.json
Modified : 2020-07-23T10:26:28.4092457-07:00
Size     : 2.25

FullName : C:\Git\PS-Docs\PowerShell-Docs\.openpublishing.redirection.json
Modified : 2020-07-27T13:05:24.3887629-07:00
Size     : 324.60
```

### <a name="format-table"></a>Format-Table

この例では、計算されるプロパティによって、コンテンツの種類によってファイルを分類するために使用される **type** プロパティが追加されます。

```powershell
Get-ChildItem -File |
  Sort-Object extension |
    Format-Table Name, Length -GroupBy @{
      name='Type'
      expression={
        switch ($_.extension) {
          '.md'   {'Content'}
          ''      {'Metacontent'}
          '.ps1'  {'Automation'}
          '.yml'  {'Automation'}
          default {'Configuration'}
        }
      }
    }
```

```Output
   Type: Metacontent

Name              Length
----              ------
ThirdPartyNotices   1229
LICENSE-CODE        1106
LICENSE            19047

   Type: Configuration

Name                                Length
----                                ------
.editorconfig                          183
.gitattributes                         419
.gitignore                             228
.markdownlint.json                    2456
.openpublishing.publish.config.json   2306
.openpublishing.redirection.json    332394
.localization-config                   232

   Type: Content

Name            Length
----            ------
README.md         3355
CONTRIBUTING.md    247

   Type: Automation

Name                      Length
----                      ------
.openpublishing.build.ps1    796
build.ps1                   7495
ci.yml                       645
ci-steps.yml                2035
daily.yml                   1271
```

### <a name="format-wide"></a>Format-Wide

コマンドレットを使用すると、 `Format-Wide` コレクション内のオブジェクトの1つのプロパティの値を複数列リストとして表示できます。

この例では、ファイル名とサイズ (kb 単位) をワイドリストとして表示します。 `Format-Wide`には複数のプロパティが表示されないため、計算されるプロパティを使用して、2つのプロパティの値を1つの値に結合します。

```powershell
Get-ChildItem -File |
  Format-Wide -Property @{e={'{0} ({1:N2}kb)' -f $_.name,($_.length/1kb)}}
```

```Output
.editorconfig (0.18kb)                          .gitattributes (0.41kb)
.gitignore (0.22kb)                             .localization-config (0.23kb)
.markdownlint.json (2.40kb)                     .openpublishing.build.ps1 (0.78kb)
.openpublishing.publish.config.json (2.25kb)    .openpublishing.redirection.json (324.60kb)
build.ps1 (7.32kb)                              ci.yml (0.63kb)
ci-steps.yml (1.99kb)                           CONTRIBUTING.md (0.24kb)
daily.yml (1.24kb)                              LICENSE (18.60kb)
LICENSE-CODE (1.08kb)                           README.md (3.28kb)
ThirdPartyNotices (1.20kb)
```

### <a name="group-object"></a>Group-Object

`Group-Object`コマンドレットは、指定されたプロパティの値に基づいて、オブジェクトをグループ単位で表示します。 この例では、計算されるプロパティは、各コンテンツタイプのファイル数をカウントします。

```powershell
Get-ChildItem -File |
  Sort-Object extension |
    Group-Object -NoElement -Property @{
      expression={
        switch ($_.extension) {
          '.md'   {'Content'}
          ''      {'Metacontent'}
          '.ps1'  {'Automation'}
          '.yml'  {'Automation'}
          default {'Configuration'}
        }
      }
    }
```

```Output
Count Name
----- ----
    5 Automation
    7 Configuration
    2 Content
    3 Metacontent
```

### <a name="measure-object"></a>Measure-Object

`Measure-Object`コマンドレットでは、オブジェクトの数値プロパティを計算します。 この例では、計算されたプロパティを使用して、3で均等に割り切れた、1 ~ 10 の数値のカウント (**合計**) を取得します。

```powershell
1..10 | Measure-Object -Property {($_ % 3) -eq 0} -Sum
```

```Output
Count             : 10
Average           :
Sum               : 3
Maximum           :
Minimum           :
StandardDeviation :
Property          : ($_ % 3) -eq 0
```

> [!NOTE]
> 他のコマンドレットとは異なり、で `Measure-Object` は、計算されるプロパティのハッシュテーブルは受け入れられません。 スクリプトブロックを使用する必要があります。

### <a name="select-object"></a>Select-Object

計算されたプロパティを使用して、コマンドレットを使用して出力オブジェクトにメンバーを追加でき `Select-Object` ます。 この例では、文字で始まる PowerShell エイリアスを一覧表示し `C` ます。 を使用して、 `Select-Object` エイリアス、マップ先のコマンドレット、およびコマンドレットに対して定義されているパラメーターの数を出力します。 計算されたプロパティを使用して、 **Parametercount** プロパティを作成できます。

```powershell
$aliases = Get-Alias c* |
  Select-Object Name,
                Definition,
                @{
                    name='ParameterCount'
                    expr={$_.Parameters.Keys.Count}
                }
$aliases | Get-Member
$aliases
```

```Output
   TypeName: Selected.System.Management.Automation.AliasInfo

Name           MemberType   Definition
----           ----------   ----------
Equals         Method       bool Equals(System.Object obj)
GetHashCode    Method       int GetHashCode()
GetType        Method       type GetType()
ToString       Method       string ToString()
Definition     NoteProperty string Definition=Get-Content
Name           NoteProperty string Name=cat
ParameterCount NoteProperty System.Int32 ParameterCount=21

Name    Definition         ParameterCount
----    ----------         --------------
cat     Get-Content                    21
cd      Set-Location                   15
cdd     Push-MyLocation                 1
chdir   Set-Location                   15
clc     Clear-Content                  20
clear   Clear-Host                      0
clhy    Clear-History                  17
cli     Clear-Item                     20
clp     Clear-ItemProperty             22
cls     Clear-Host                      0
clv     Clear-Variable                 19
cnsn    Connect-PSSession              29
compare Compare-Object                 20
copy    Copy-Item                      24
cp      Copy-Item                      24
cpi     Copy-Item                      24
cpp     Copy-ItemProperty              23
cvpa    Convert-Path                   13
```

### <a name="sort-object"></a>Sort-Object

計算されたプロパティを使用すると、プロパティごとに異なる順序でデータを並べ替えることができます。 この例では、CSV ファイルのデータを **日付** 順に昇順に並べ替えます。 しかし、各日付内では、 **販売** された単位で行を降順に並べ替えます。

```powershell
Import-Csv C:\temp\sales-data.csv |
  Sort-Object Date, @{expr={$_.UnitsSold}; desc=$true}, Salesperson  |
    Select-Object Date, Salesperson, UnitsSold
```

```Output
Date       Salesperson UnitsSold
----       ----------- ---------
2020-08-01 Sally       3
2020-08-01 Anne        2
2020-08-01 Fred        1
2020-08-02 Anne        6
2020-08-02 Fred        2
2020-08-02 Sally       0
2020-08-03 Anne        5
2020-08-03 Sally       3
2020-08-03 Fred        1
2020-08-04 Anne        2
2020-08-04 Fred        2
2020-08-04 Sally       2
```

## <a name="notes"></a>メモ

- 式スクリプトブロックは、ハッシュテーブルのエントリとして指定するのではなく、引数として _直接_ 指定することができ `Expression` ます。 次に例を示します。

  ```powershell
  '1', '10', '2' | Sort-Object { [int] $_ }
  ```

  この例は、、、など、キーを介してプロパティに名前を付ける (またはサポートする) 必要がないコマンドレットに便利です `Name` `Sort-Object` `Group-Object` `Measure-Object` 。

  プロパティの名前付けをサポートするコマンドレットの場合、スクリプトブロックは文字列に変換され、出力でプロパティの名前として使用されます。

- `Expression` スクリプトブロックは _子_ スコープで実行されます。つまり、呼び出し元の変数を直接変更することはできません。

- パイプラインロジックは、スクリプトブロックからの出力に適用され `Expression` ます。 これは、単一要素の配列を出力すると、その配列がラップ解除されることを意味します。

- ほとんどのコマンドレットでは、式スクリプトブロック内のエラーは自動的に無視されます。
  `Sort-Object`では、ステートメントの終了エラーとスクリプト終了エラーは _出力_ されますが、ステートメントは終了しません。

## <a name="see-also"></a>参照

[about_Hash_Tables](about_hash_tables.md)

[Compare-Object](xref:Microsoft.PowerShell.Utility.Compare-Object)

[ConvertTo-Html](xref:Microsoft.PowerShell.Utility.ConvertTo-Html)

[Format-Custom](xref:Microsoft.PowerShell.Utility.Format-Custom)

[Format-List](xref:Microsoft.PowerShell.Utility.Format-List)

[Format-Table](xref:Microsoft.PowerShell.Utility.Format-Table)

[Format-Wide](xref:Microsoft.PowerShell.Utility.Format-Wide)

[Group-Object](xref:Microsoft.PowerShell.Utility.Group-Object)

[Measure-Object](xref:Microsoft.PowerShell.Utility.Measure-Object)

[Select-Object](xref:Microsoft.PowerShell.Utility.Select-Object)

[Sort-Object](xref:Microsoft.PowerShell.Utility.Sort-Object)

[.NET での型の書式設定](/dotnet/standard/base-types/formatting-types)
