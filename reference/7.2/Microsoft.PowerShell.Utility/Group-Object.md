---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/group-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Group-Object
ms.openlocfilehash: 6198964f01a4c0dc70584cdb3e5c0e13f3965934
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599919"
---
# Group-Object

## 概要
指定したプロパティに対して同じ値を含むオブジェクトをグループ化します。

## SYNTAX

### テーブル

```
Group-Object [-NoElement] [-AsHashTable] [-AsString] [-InputObject <PSObject>]
 [[-Property] <Object[]>] [-Culture <String>] [-CaseSensitive] [<CommonParameters>]
```

## Description

`Group-Object`コマンドレットは、指定されたプロパティの値に基づいて、オブジェクトをグループ単位で表示します。
`Group-Object` プロパティ値ごとに1行のテーブルを返し、その値を持つアイテムの数を表示する列を返します。

複数のプロパティを指定すると、はまず `Group-Object` 最初のプロパティの値でそれらをグループ化し、次に各プロパティグループ内で次のプロパティの値によってグループ化します。

PowerShell 7 以降で `Group-Object` は、 **CaseSensitive** と **ashashtable** パラメーターを組み合わせて、大文字と小文字を区別するハッシュテーブルを作成できます。 ハッシュテーブルのキーは、大文字と小文字を区別する比較を使用し **て、system.string オブジェクトを** 出力します。

## 例

### 例 1: 拡張子別にファイルをグループ化する

この例では、の下にあるファイルを再帰的に取得 `$PSHOME` し、ファイル名拡張子別にグループ化します。 出力はコマンドレットに送信され `Sort-Object` 、指定された拡張子に対して検出されたファイルの数によって並べ替えられます。 空の **名前** は、ディレクトリを表します。

この例では、 **Noelement** パラメーターを使用して、グループのメンバーを除外します。

```powershell
$files = Get-ChildItem -Path $PSHOME -Recurse
$files | Group-Object -Property extension -NoElement | Sort-Object -Property Count -Descending
```

```Output
Count Name
----- ----
  365 .xml
  231 .cdxml
  197
  169 .ps1xml
  142 .txt
  114 .psd1
   63 .psm1
   49 .xsd
   36 .dll
   15 .mfl
   15 .mof
...
```

### 例 2: 整数を確率とほうでグループ化する

この例では、 **Property** パラメーターの値としてスクリプトブロックを使用する方法を示します。 このコマンドは、1 ~ 20 の整数を、確率と偶数でグループ化して表示します。

```powershell
1..20 | Group-Object -Property {$_ % 2}
```

```Output
Count Name                      Group
----- ----                      -----
   10 0                         {2, 4, 6, 8...}
   10 1                         {1, 3, 5, 7...}
```

### 例 3: EntryType でイベントログイベントをグループ化する

この例では、システムイベントログの最新エントリ1000を **Entrytype** でグループ化して表示します。

出力の [ **カウント** ] 列は、各グループのエントリ数を表します。 [ **名前** ] 列は、グループを定義する **EventType** 値を表します。 [ **グループ]** 列は、各グループのオブジェクトを表します。

```powershell
Get-WinEvent -LogName System -MaxEvents 1000 | Group-Object -Property LevelDisplayName
```

```Output
Count Name          Group
----- ----          -----
  153 Error         {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics...}
  722 Information   {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics...}
  125 Warning       {System.Diagnostics.Eventing.Reader.EventLogRecord, System.Diagnostics...}
```

### 例 4: 優先度クラスによってプロセスをグループ化する

この例は、 **Noelement** パラメーターの効果を示しています。 これらのコマンドは、コンピューター上のプロセスを優先度クラス別にグループ化します。

最初のコマンドは、 `Get-Process` コマンドレットを使用してコンピューター上のプロセスを取得し、そのオブジェクトをパイプラインの下に送信します。 `Group-Object`プロセスの優先度 **クラス** プロパティの値によってオブジェクトをグループ化します。

2番目の例では、 **Noelement** パラメーターを使用して、出力からグループのメンバーを除外します。 結果は、 **Count** および **Name** プロパティ値のみを含むテーブルになります。

次のサンプル出力に結果を示します。

```powershell
Get-Process | Group-Object -Property PriorityClass
```

```Output
Count Name         Group
----- ----         -----
   55 Normal       {System.Diagnostics.Process (AdtAgent), System.Diagnosti...
    1              {System.Diagnostics.Process (Idle)}
    3 High         {System.Diagnostics.Process (Newproc), System.Diagnostic...
    2 BelowNormal  {System.Diagnostics.Process (winperf),
```

```powershell
Get-Process | Group-Object -Property PriorityClass -NoElement
```

```Output
Count Name
----- ----
   55 Normal
    1
    3 High
    2 BelowNormal
```

### 例 5: プロセスを名前でグループ化する

次の例では、を使用し `Group-Object` て、ローカルコンピューター上で実行されているプロセスの複数のインスタンスをグループ化します。 `Where-Object` 複数のインスタンスを持つプロセスを表示します。

```powershell
Get-Process | Group-Object -Property Name -NoElement | Where-Object {$_.Count -gt 1}
```

```Output
Count Name
----- ----
2     csrss
5     svchost
2     winlogon
2     wmiprvse
```

### 例 6: ハッシュテーブル内のオブジェクトをグループ化する

この例では、 **ashashtable** パラメーターと **asstring** パラメーターを使用して、ハッシュテーブル内のグループをキーと値のペアのコレクションとして返します。

結果のハッシュ テーブルでは、各プロパティ値がキーで、グループの要素が値です。
各キーがハッシュ テーブルのオブジェクトのプロパティであるため、ドット表記を使用して値を表示します。

最初のコマンドは、 `Get` セッション内のコマンドレットとコマンドレットを取得し、 `Set` 動詞別にグループ化して、グループをハッシュテーブルとして返し、ハッシュテーブルを変数に保存し `$A` ます。

2番目のコマンドは、のハッシュテーブルを表示し `$A` ます。 2つのキーと値のペアがあります。1つはコマンドレット用で、もう1つは `Get` `Set` コマンドレット用です。

3番目のコマンドは、ドット表記を使用して、の `$A.Get` **Get** キーの値を表示し `$A` ます。 値 **は、[オブジェクト]** です。 **Asstring** パラメーターは、グループ内のオブジェクトを文字列に変換しません。

```powershell
$A = Get-Command Get-*, Set-* -CommandType cmdlet | Group-Object -Property Verb -AsHashTable -AsString
$A
```

```Output
Name     Value
----     -----
Get      {Get-Acl, Get-Alias, Get-AppLockerFileInformation, Get-AppLockerPolicy...}
Set      {Set-Acl, Set-Alias, Set-AppBackgroundTaskResourcePolicy, Set-AppLockerPolicy...}
```

```powershell
$A.Get
```

```Output
CommandType     Name                                Version    Source
-----------     ----                                -------    ------
Cmdlet          Get-Acl                             7.0.0.0    Microsoft.PowerShell.Security
Cmdlet          Get-Alias                           7.0.0.0    Microsoft.PowerShell.Utility
Cmdlet          Get-AppLockerFileInformation        2.0.0.0    AppLocker
Cmdlet          Get-AppLockerPolicy                 2.0.0.0    AppLocker
...
```

### 例 7: 大文字と小文字を区別するハッシュテーブルを作成する

この例では、 **CaseSensitive** パラメーターと **ashashtable** パラメーターを組み合わせて、大文字と小文字を区別するハッシュテーブルを作成します。 この例のファイルには、およびの拡張子があり `.txt` `.TXT` ます。

```powershell
$hash = Get-ChildItem -Path C:\Files | Group-Object -Property Extension -CaseSensitive -AsHashTable
$hash
```

```Output
Name           Value
----           -----
.TXT           {C:\Files\File7.TXT, C:\Files\File8.TXT, C:\Files\File9.TXT}
.txt           {C:\Files\file1.txt, C:\Files\file2.txt, C:\Files\file3.txt}
```

変数は、system.string `$hash` オブジェクトを格納します。 `Get-ChildItem` ディレクトリからファイル名を取得 `C:\Files` し、パイプラインの下位 **にある system.object** オブジェクトを送信します。 `Group-Object`**プロパティ** 値の **拡張** を使用してオブジェクトをグループ化します。 **CaseSensitive** および **ashashtable** パラメーターはハッシュテーブルを作成し、キーは大文字と小文字を区別するキーとを使用してグループ化され `.txt` `.TXT` ます。

## PARAMETERS

### -AsHashTable

このコマンドレットがグループをハッシュテーブルとして返すことを示します。 ハッシュ テーブルのキーは、オブジェクトをグループ化するためのプロパティの値です。 ハッシュ テーブルの値は、そのプロパティ値を含むオブジェクトです。

それ自体では、 **ashashtable** パラメーターは、各キーがグループ化されたオブジェクトのインスタンスである各ハッシュテーブルを返します。 **Asstring** パラメーターと共に使用する場合、ハッシュテーブル内のキーは文字列になります。

PowerShell 7 以降では、大文字と小文字を区別するハッシュテーブルを作成するには、コマンドに **CaseSensitive** と **ashashtable** を含めます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: AHT

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsString

このコマンドレットがハッシュテーブルキーを文字列に変換することを示します。 既定では、ハッシュ テーブルのキーは、グループ化されたオブジェクトのインスタンスです。 このパラメーターは、 **Ashashtable** パラメーターと共に使用する場合にのみ有効です。

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

### -CaseSensitive

このコマンドレットによってグループ化で大文字と小文字が区別されることを示します。 このパラメーターを指定しない場合、グループ内のオブジェクトのプロパティ値は大文字と小文字が区別されません。

PowerShell 7 以降では、大文字と小文字を区別するハッシュテーブルを作成するには、コマンドに **CaseSensitive** と **ashashtable** を含めます。

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

### -Culture

文字列を比較するときに使用するカルチャを指定します。

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

### -InputObject

オブジェクトをグループに指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

**InputObject** パラメーターを使用してオブジェクトのコレクションをに送信すると `Group-Object` 、は `Group-Object` コレクションを表す1つのオブジェクトを受け取ります。 その結果、そのオブジェクトをメンバーとする単一のグループを作成します。

コレクション内のオブジェクトをグループ化するには、オブジェクトをにパイプ処理し `Group-Object` ます。

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

### -NoElement

このコマンドレットが結果からグループのメンバーを除外することを示します。

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

### -プロパティ

グループ化のためのプロパティを指定します。 オブジェクトは、指定したプロパティの値に基づいてグループに配置されます。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 式 `<string>` または `<script block>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをにパイプすることができ `Group-Object` ます。

## 出力

### GroupInfo または System. Collections. Hashtable

**Ashashtable** パラメーターを使用すると、は `Group-Object` **Hashtable** オブジェクトを返します。
それ以外の場合は、 **GroupInfo** オブジェクトを返します。

## 注

やなどの書式設定コマンドレットの **GroupBy** パラメーターを使用して、 `Format-Table` `Format-List` オブジェクトをグループ化できます。 `Group-Object`プロパティ値ごとに1行のテーブルを作成するとは異なり、 **GroupBy** パラメーターは、プロパティ値を持つ項目ごとに1行のテーブルをプロパティ値ごとに作成します。

`Group-Object` では、グループ化されるオブジェクトが Microsoft .NET コア型と同じである必要はありません。 異なる .NET Core 型のオブジェクトをグループ化する場合、で `Group-Object` は次の規則が使用されます。

- 同じプロパティ名と型。

  オブジェクトに指定された名前のプロパティがあり、プロパティ値の .NET Core 型が同じである場合、プロパティ値は同じ型のオブジェクトに使用されるのと同じ規則を使用してグループ化されます。

- 同じプロパティ名、異なる型。

  オブジェクトに指定された名前のプロパティがあり、プロパティ値が異なるオブジェクトに異なる .NET Core 型を持っている場合、は、 `Group-Object` そのプロパティグループの .net core 型として最初に見つかったプロパティの .Net core 型を使用します。 オブジェクトに異なる型のプロパティがある場合、プロパティ値は、そのグループの型に変換されます。 型変換に失敗した場合、オブジェクトはグループに含まれません。

- プロパティがありません。

  指定されたプロパティを持たないオブジェクトはグループ化できません。 グループ化されていないオブジェクトは、という名前のグループの最終的な **GroupInfo** オブジェクト出力に表示され `AutomationNull.Value` ます。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[about_Hash_Tables](../Microsoft.PowerShell.Core/About/about_Hash_Tables.md)

[Compare-Object](Compare-Object.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Measure-Object](Measure-Object.md)

[New-Object](New-Object.md)

[Select-Object](Select-Object.md)

[Sort-Object](Sort-Object.md)

[Tee-Object](Tee-Object.md)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)
