---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 12/19/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/format-list?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Format-List
ms.openlocfilehash: d8410fbc2d3f29f0726f84ab151993a60ce95434
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599720"
---
# Format-List

## 概要
出力の各プロパティは、新しい行に表示されるプロパティの一覧として書式設定されます。

## SYNTAX

```
Format-List [[-Property] <Object[]>] [-GroupBy <Object>] [-View <string>] [-ShowError]
[-DisplayError] [-Force] [-Expand <string>] [-InputObject <psobject>] [<CommonParameters>]
```

## Description

`Format-List`コマンドレットは、各プロパティが個別の行に表示されるプロパティの一覧として、コマンドの出力を書式設定します。 を使用すると、 `Format-List` オブジェクトのすべてのプロパティまたは選択したプロパティを一覧として書式設定および表示できます (リスト *)。

リスト内の各項目に対して、テーブルよりも多くの領域が使用可能であるため、PowerShell では、一覧に表示されるオブジェクトのプロパティが多くなり、プロパティ値が切り捨てられる可能性は低くなります。

## 例

### 例 1: コンピューターサービスの書式を設定する

```powershell
Get-Service | Format-List
```

このコマンドは、コンピューターのサービスに関する情報を一覧として書式設定します。 既定では、サービスは表として書式設定されます。 `Get-Service`コマンドレットは、コンピューター上のサービスを表すオブジェクトを取得します。 パイプライン演算子 (|) は、パイプラインを介して結果をに渡し `Format-List` ます。
次に、 `Format-List` 一覧のサービス情報を書式設定し、表示するために既定の出力コマンドレットに送信します。

### 例 2: TYPES.PS1XML ファイルの書式を設定する

これらのコマンドは、PowerShell ディレクトリ内の TYPES.PS1XML ファイルに関する情報を一覧として表示します。

```powershell
$A = Get-ChildItem $pshome\*.ps1xml
Format-List -InputObject $A
```

最初のコマンドは、ファイルを表すオブジェクトを取得し、変数に格納し `$A` ます。

2番目のコマンドは、を使用し `Format-List` て、に格納されたオブジェクトに関する情報を書式設定し `$A` ます。 このコマンドは、 **InputObject** パラメーターを使用して変数をに渡します。これにより `Format-List` 、書式設定された出力が表示のために既定の出力コマンドレットに送信されます。

### 例 3: プロセスのプロパティを名前で書式設定する

このコマンドは、コンピューター上の各プロセスの名前、基本優先度、および優先度クラスを表示します。

```powershell
Get-Process | Format-List -Property name, basepriority, priorityclass
```

コマンドレットを使用して、 `Get-Process` 各プロセスを表すオブジェクトを取得します。 パイプライン演算子 (|) は、パイプラインを介してプロセスオブジェクトをに渡し `Format-List` ます。 `Format-List` 指定されたプロパティの一覧としてプロセスを書式設定します。 *プロパティ* パラメーター名は省略可能であるため、省略できます。

### 例 4: プロセスのすべてのプロパティを書式設定する

このコマンドは、Winlogon プロセスのすべてのプロパティを表示します。

```powershell
Get-Process winlogon | Format-List -Property *
```

Get-Process コマンドレットを使用して、Winlogon プロセスを表すオブジェクトを取得します。 パイプライン演算子 (|) は、パイプラインを介して Winlogon プロセスオブジェクトをに渡し `Format-List` ます。 このコマンドは、 *Property* パラメーターを使用してプロパティを指定し、を使用して \* すべてのプロパティを示します。
*Property* パラメーターの名前は省略可能なので、省略してコマンドをとして入力でき `Format-List *` ます。 `Format-List` は、結果を既定の出力コマンドレットに自動的に送信して表示します。

### 例 5: 形式エラーのトラブルシューティング

次の例では、 **displayerror** パラメーターまたは **showerror** パラメーターを式に追加した結果を示します。

```powershell
PC /> Get-Date | Format-List DayOfWeek,{ $_ / $null } -DisplayError

DayOfWeek    : Friday
 $_ / $null  : #ERR

PC /> Get-Date | Format-List DayOfWeek,{ $_ / $null } -ShowError

DayOfWeek    : Friday
 $_ / $null  :

Failed to evaluate expression " $_ / $null ".
+ CategoryInfo          : InvalidArgument: (12/21/2018 7:59:23 AM:PSObject) [], RuntimeException
+ FullyQualifiedErrorId : PSPropertyExpressionError
```

## PARAMETERS

### -DisplayError

このコマンドレットがコマンドラインでエラーを表示することを示します。 このパラメーターはほとんど使用されませんが、コマンドで式を書式設定するときに、 `Format-List` 式が動作していないと思われる場合は、デバッグのために使用できます。

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

### -展開

書式設定されたコレクションオブジェクト、およびコレクション内のオブジェクトを指定します。 このパラメーターは、ICollection (System.Collections) インターフェイスをサポートするオブジェクトを書式設定するために用意されています。 既定値は EnumOnly です。 このパラメーターの有効値は、次のとおりです。

- Enumonly です. コレクション内のオブジェクトのプロパティを表示します。
- CoreOnly。 コレクション オブジェクトのプロパティを表示します。
- Both。 コレクション オブジェクトのプロパティと、コレクション内のオブジェクトのプロパティを表示します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: CoreOnly, EnumOnly, Both

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

このコマンドレットによってすべてのエラー情報が表示されることを示します。 **Displayerror** パラメーターまたは **showerror** パラメーターと共に使用します。 既定では、エラー オブジェクトがエラーまたは表示ストリームに書き込まれるとき、エラー情報の一部のみが表示されます。

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

### -GroupBy

共有プロパティまたは値に基づいて、グループ内の出力を指定します。 式または出力のプロパティを入力します。

**GroupBy** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 名前 (またはラベル)- `<string>`
- 式 `<string>` または `<script block>`
- FormatString `<string>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

書式設定するオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

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

### -プロパティ

表示するオブジェクト プロパティと、その表示順序を指定します。
ワイルドカードを使用できます。

このパラメーターを省略した場合、表示されるプロパティは、表示されるオブジェクトに依存します。 パラメーター名 "Property" は省略可能です。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 名前 (またはラベル)- `<string>`
- 式 `<string>` または `<script block>`
- FormatString `<string>`

詳細については、「 [about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -ShowError

コマンドレットがパイプラインを介してエラーを送信することを示します。 このパラメーターはほとんど使用されませんが、コマンドで式を書式設定するときに、 `Format-List` 式が動作していないと思われる場合は、デバッグのために使用できます。

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

### -ビュー

代替リスト形式またはビューの名前を指定します。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをにパイプすることができ `Format-List` ます。

## 出力

### Microsoft. PowerShell. 内部形式

`Format-List` リストを表す書式オブジェクトを返します。

## 注

Format-List は、その組み込みエイリアスである FL を使用して参照することもできます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

形式のコマンドレット (など) では、 `Format-List` 表示されるデータは配置されますが、表示されません。
データは、PowerShell の出力機能と、Out 動詞 (Out コマンドレット) を含むコマンドレット (やなど) によって表示され `Out-Host` `Out-File` ます。

Format コマンドレットを使用しない場合、PowerShell では、表示される各オブジェクトに既定の形式が適用されます。

**GroupBy** パラメーターは、オブジェクトが並べ替えられていることを前提としています。 を使用してオブジェクトをグループ化する前に、Sort-Object を使用し `Format-List` ます。

**View** パラメーターを使用すると、テーブルの代替形式を指定できます。 PowerShell ディレクトリ内のファイルで定義されているビューを使用することも `*.format.PS1XML` 、新しい types.ps1xml ファイルに独自のビューを作成して、Update-FormatData コマンドレットを使用してそれらを powershell に含めることもできます。

**View** パラメーターの代替ビューでは、リスト形式を使用する必要があります。そうでない場合、コマンドは失敗します。 代替ビューがテーブルの場合は、を使用 `Format-Table` します。 代替ビューが一覧またはテーブルではない場合は、を使用し `Format-Custom` ます。

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[Format-Custom](Format-Custom.md)

[Format-Hex](Format-Hex.md)

[Format-Table](Format-Table.md)

[Format-Wide](Format-Wide.md)
