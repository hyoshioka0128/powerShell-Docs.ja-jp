---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/format-table?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Format-Table
ms.openlocfilehash: 2db0a8abe8fbd87b077e40655a06a1db45482ebc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601395"
---
# Format-Table

## 概要
出力を表として書式設定します。

## SYNTAX

### すべて

```
Format-Table [-AutoSize] [-RepeatHeader] [-HideTableHeaders] [-Wrap] [[-Property] <Object[]>]
 [-GroupBy <Object>] [-View <String>] [-ShowError] [-DisplayError] [-Force] [-Expand <String>]
 [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

`Format-Table`コマンドレットは、各列のオブジェクトの選択されたプロパティを使用して、コマンドの出力をテーブルとして書式設定します。 各列に表示される既定のレイアウトとプロパティは、オブジェクトの種類によって決まります。 **プロパティ** パラメーターを使用して、表示するプロパティを選択できます。

PowerShell では、既定のフォーマッタを使用して、オブジェクトの種類の表示方法を定義します。 ファイルを使用すると、 `.ps1xml` 指定したプロパティを持つ出力テーブルを表示するカスタムビューを作成できます。 カスタムビューを作成した後、 **view** パラメーターを使用して、カスタムビューでテーブルを表示します。 ビューの詳細については、「 [about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)」を参照してください。

ハッシュテーブルを使用して、計算されたプロパティを表示する前にオブジェクトに追加したり、テーブルの列見出しを指定したりすることができます。 計算されたプロパティを追加するには、 **property** パラメーターまたは **GroupBy** パラメーターを使用します。 ハッシュ テーブルの詳細については、「[about_Hash_Tables (ハッシュ テーブルについて)](../Microsoft.PowerShell.Core/About/about_Hash_Tables.md)」をご覧ください。

## 例

### 例 1: PowerShell ホストの書式を設定する

この例では、PowerShell のホストプログラムに関する情報をテーブルに表示します。

```powershell
Get-Host | Format-Table -AutoSize
```

この `Get-Host` コマンドレットは、ホストを表す system.object **のホスト** オブジェクトを取得します。 オブジェクトは、パイプラインの下方向に送信され、 `Format-Table` テーブルに表示されます。 **AutoSize** パラメーターを指定すると、列の幅が調整され、切り捨てが最小限に抑えられます。

### 例 2: BasePriority でプロセスをフォーマットする

この例では、同じ **Basepriority** プロパティを持つグループにプロセスが表示されます。

```powershell
Get-Process | Sort-Object -Property BasePriority | Format-Table -GroupBy BasePriority -Wrap
```

`Get-Process`コマンドレットは、コンピューター上の各プロセスを表すオブジェクトを取得し、それをパイプラインの下に送信し `Sort-Object` ます。 オブジェクトは、 **Basepriority** プロパティの順序で並べ替えられます。

並べ替えられたオブジェクトは、パイプラインでに送信され `Format-Table` ます。 **GroupBy** パラメーターは、 **basepriority** プロパティの値に基づいてプロセスデータをグループに配置します。 **Wrap** パラメーターを指定すると、データが切り捨てられなくなります。

### 例 3: 開始日によるプロセスのフォーマット

この例では、コンピューター上で実行されているプロセスに関する情報を表示します。 オブジェクトが並べ替えられ、 `Format-Table` ビューを使用してオブジェクトが開始日でグループ化されます。

```powershell
Get-Process | Sort-Object StartTime | Format-Table -View StartTime
```

`Get-Process` コンピューター上で実行されているプロセスを表す **system.object オブジェクトを** 取得します。 オブジェクトは、パイプライン内でに送信され `Sort-Object` 、 **StartTime** プロパティに基づいて並べ替えられます。

並べ替えられたオブジェクトは、パイプラインでに送信され `Format-Table` ます。 **View** パラメーターは、PowerShell ファイルで定義されている、system.string オブジェクトの **StartTime** ビューを指定します。 `DotNetTypes.format.ps1xml`  **StartTime** ビューでは、各プロセスの開始時刻が短い日付に変換され、開始日によってプロセスがグループ化されます。

ファイルには、 `DotNetTypes.format.ps1xml` プロセスの **優先度** ビューが含まれています。 カスタマイズしたビューを使用して、独自のファイルを作成でき `format.ps1xml` ます。

### 例 4: テーブル出力にカスタムビューを使用する

この例では、カスタムビューにディレクトリの内容が表示されます。 カスタムビューでは、 **DirectoryInfo** およびによって作成された system.object オブジェクトのテーブル出力に **、[作成****時刻**] 列が追加されます `Get-ChildItem` 。

この例のカスタムビューは、PowerShell ソースコードで定義されているビューから作成されています。 ビューおよびこの例のビューを作成するために使用されるコードの詳細については、「 [about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md#sample-xml-for-a-format-table-custom-view)」を参照してください。

```powershell
Get-ChildItem  -Path C:\Test | Format-Table -View mygciview
```

```Output
    Directory: C:\Test

Mode                LastWriteTime              CreationTime         Length Name
----                -------------              ------------         ------ ----
d-----        11/4/2019     15:54       9/24/2019     15:54                Archives
d-----        8/27/2019     14:22       8/27/2019     14:22                Drawings
d-----       10/23/2019     09:38       2/25/2019     09:38                Files
-a----        11/7/2019     11:07       11/7/2019     11:07          11345 Alias.txt
-a----        2/27/2019     15:15       2/27/2019     15:15            258 alias_out.txt
-a----        2/27/2019     15:16       2/27/2019     15:16            258 alias_out2.txt
```

`Get-ChildItem` 現在のディレクトリの内容を取得し `C:\Test` ます。 **DirectoryInfo** オブジェクトと system.object オブジェクトは、パイプラインを **介して送信** されます。
`Format-Table`**View** パラメーターを使用して、"作成 **後の時間**" 列を含むカスタムビュー **mygciview** を指定します。

の既定の出力には、"設定 `Format-Table` `Get-ChildItem` 後の **時間** " 列は含まれません。

### 例 5: テーブルの出力にプロパティを使用する

この例では、 **Property** パラメーターを使用して、プロパティ **名** と **dependentservices** を示す2列のテーブルにすべてのコンピューターのサービスを表示します。

```powershell
Get-Service | Format-Table -Property Name, DependentServices
```

`Get-Service` コンピューター上のすべてのサービスを取得し、 **ServiceController** オブジェクトをパイプラインの下に送信します。 `Format-Table`**Property** パラメーターを使用して、 **Name** プロパティと **dependentservices** プロパティをテーブルに表示するように指定します。

**Name** と **dependentservices** は、オブジェクト型のプロパティのうちの2つです。 すべてのプロパティを表示するには、「」を参照して `Get-Service | Get-Member -MemberType Properties` ください。

### 例 6: プロセスをフォーマットし、その実行時間を計算する

この例では、ローカルコンピューターの **メモ帳** プロセスのプロセス名と合計実行時間を含むテーブルを表示します。 合計実行時間は、現在の時刻から各プロセスの開始時刻を差し引いて求めます。

```powershell
Get-Process notepad |
  Format-Table ProcessName, @{Label="TotalRunningTime"; Expression={(Get-Date) - $_.StartTime}}
```

```Output
ProcessName TotalRunningTime
----------- ----------------
notepad     03:20:00.2751767
notepad     00:00:16.7710520
```

`Get-Process` ローカルコンピューターの **メモ帳** のすべてのプロセスを取得し、そのオブジェクトをパイプラインの下に送信します。 `Format-Table`**ProcessName**、プロパティ、および Totalrunningtime という2つの列を含むテーブルを表示し `Get-Process` ます。集計プロパティです。 

**Totalrunningtime** プロパティは、**ラベル** と **式** の2つのキーを持つハッシュテーブルによって指定されます。 **Label** キーは、プロパティ名を指定します。 **式** キーは計算を指定します。 この式は、各プロセスオブジェクトの **StartTime** プロパティを取得し、 `Get-Date` 現在の日付と時刻を取得するコマンドの結果から減算します。

### 例 7: メモ帳のプロセスをフォーマットする

この例では `Get-CimInstance` 、を使用して、ローカルコンピューター上のすべての **メモ帳** プロセスの実行時間を取得します。 を `Get-CimInstance` **ComputerName** パラメーターと共に使用すると、リモートコンピューターから情報を取得できます。

```powershell
$Processes = Get-CimInstance -Class win32_process -Filter "name='notepad.exe'"
$Processes | Format-Table ProcessName, @{ Label = "Total Running Time"; Expression={(Get-Date) - $_.CreationDate}}
```

```Output
ProcessName Total Running Time
----------- ------------------
notepad.exe 03:39:39.6260693
notepad.exe 00:19:56.1376922
```

`Get-CimInstance`**notepad.exe** という名前のすべてのローカルコンピューターのプロセスを記述する WMI **Win32_Process** クラスのインスタンスを取得します。 プロセスオブジェクトは変数に格納され `$Processes` ます。

変数内のプロセスオブジェクト `$Processes` は、パイプラインの下に送信されます。このオブジェクトには `Format-Table` 、 **ProcessName** プロパティと新しい計算されるプロパティ ( **合計実行時間**) が表示されます。

このコマンドは、新しい計算されるプロパティの名前 ( **合計実行時間**) を **ラベル** キーに割り当てます。 **式** キーのスクリプトブロックは、現在の日付からプロセスの作成日を差し引くことによって、プロセスが実行されている時間を計算します。 `Get-Date`コマンドレットは、現在の日付を取得します。 作成日は現在の日付から差し引かれます。 結果は、 **合計実行時間** の値になります。

### 例 8: 形式エラーのトラブルシューティング

次の例は、 **displayerror** パラメーターまたは **showerror** パラメーターを式と共に追加した結果を示しています。

```powershell
Get-Date | Format-Table DayOfWeek,{ $_ / $null } -DisplayError
```

```Output
DayOfWeek  $_ / $null
--------- ------------
Wednesday #ERR
```

```powershell
Get-Date | Format-Table DayOfWeek,{ $_ / $null } -ShowError
```

```Output
DayOfWeek  $_ / $null
--------- ------------
Wednesday

InvalidArgument: Failed to evaluate expression " $_ / $null ".
```

## PARAMETERS

### -AutoSize

コマンドレットによって、データの幅に基づいて列のサイズと列数が調整されることを示します。 既定では、列のサイズと数は、ビューによって決まります。

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

### -DisplayError

コマンドレットによってコマンドラインにエラーが表示されることを示します。 このパラメーターは、コマンドで式を書式設定 `Format-Table` し、式のトラブルシューティングを行う必要がある場合に、デバッグのために使用できます。

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

### -展開

コレクションオブジェクトの形式とコレクション内のオブジェクトを指定します。 このパラメーターは、 [ICollection](/dotnet/api/system.collections.icollection) (system.string) インターフェイスをサポートするオブジェクトを書式設定するように設計されてい[ます。](/dotnet/api/system.collections) 既定値は **Enumonly** です。
このパラメーターに指定できる値は次のとおりです。

- **Enumonly**: コレクション内のオブジェクトのプロパティを表示します。
- **Coreonly**: コレクションオブジェクトのプロパティを表示します。
- **Both**: コレクションオブジェクトのプロパティと、コレクション内のオブジェクトのプロパティを表示します。

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

コマンドレットがすべてのエラー情報を表示するようにコマンドレットに指示することを示します。 **Displayerror** パラメーターまたは **showerror** パラメーターと共に使用します。 既定では、エラー オブジェクトがエラーまたは表示ストリームに書き込まれるとき、エラー情報の一部のみが表示されます。

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

### -GroupBy

プロパティ値に基づいて個別のテーブルに並べ替えられた出力を指定します。 たとえば、 **GroupBy** を使用して、サービスの状態に基づいて別のテーブルのサービスを一覧表示できます。

式またはプロパティを入力してください。 **GroupBy** パラメーターは、オブジェクトが並べ替えられることを想定しています。
を使用し `Sort-Object` て `Format-Table` オブジェクトをグループ化する前に、コマンドレットを使用します。

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

### -HideTableHeaders

表から列見出しを削除します。

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

表示するオブジェクト プロパティと、その表示順序を指定します。 1つまたは複数のプロパティ名をコンマで区切って入力するか、ハッシュテーブルを使用して計算されたプロパティを表示します。 ワイルドカードを使用できます。

このパラメーターを省略した場合、表示に表示されるプロパティは、最初のオブジェクトのプロパティによって異なります。 たとえば、最初のオブジェクトが **propertya** と **propertya** を持ち、後続のオブジェクトが **propertya**、 **propertya**、および **Propertya** を持つ場合、 **propertya** と **propertya** のヘッダーのみが表示されます。

**Property** パラメーターは省略可能です。 **プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

**Property** パラメーターの値には、新しい集計プロパティを指定できます。 計算されるプロパティには、スクリプトブロックまたはハッシュテーブルを指定できます。 有効なキーと値のペアは次のとおりです。

- 名前 (またはラベル) `<string>`
- 式 `<string>` または `<script block>`
- FormatString `<string>`
- 幅- `<int32>` -より大きい必要があります `0`
- Alignment-値には `Left` 、、 `Center` 、またはを指定できます。 `Right`

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

### -RepeatHeader

すべての画面がいっぱいになった後にテーブルのヘッダーを表示します。 繰り返しヘッダーは、出力が、、 `less` また `more` はスクリーンリーダーとのページングなどのページャーにパイプ処理される場合に便利です。

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

### -ShowError

このパラメーターは、パイプラインを介してエラーを送信します。 このパラメーターは、コマンドで式を書式設定 `Format-Table` し、式のトラブルシューティングを行う必要がある場合に、デバッグのために使用できます。

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

### -ビュー

PowerShell 6 以降では、既定のビューが PowerShell ソースコードで定義されてい `C#` ます。 Powershell `*.format.ps1xml` 5.1 以前のバージョンのファイルは、powershell 6 以降のバージョンには存在しません。

**View** パラメーターを使用すると、テーブルの代替形式またはカスタムビューを指定できます。 既定の PowerShell ビューを使用することも、カスタムビューを作成することもできます。 カスタムビューを作成する方法の詳細については、「 [about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)」を参照してください。

**View** パラメーターの代替ビューおよびカスタムビューでは、テーブル形式を使用する必要があります。そうしないと、は `Format-Table` 失敗します。 代替ビューが一覧の場合は、コマンドレットを使用し `Format-List` ます。 代替ビューがリストまたはテーブルではない場合は、 `Format-Custom` コマンドレットを使用します。

**プロパティ** と **ビュー** のパラメーターを同じコマンドで使用することはできません。

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

### -Wrap

列幅を超えるテキストを次の行に表示します。 既定では、列幅を超えるテキストは切り捨てられます。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプライン内の任意のオブジェクトをに送信でき `Format-Table` ます。

## 出力

### Microsoft. PowerShell. 内部形式

`Format-Table` テーブルを表す書式オブジェクトを返します。

## 注

## 関連リンク

[about_Calculated_Properties](../Microsoft.PowerShell.Core/About/about_Calculated_Properties.md)

[about_Format.ps1xml](../Microsoft.PowerShell.Core/About/about_Format.ps1xml.md)

[about_Hash_Tables](../Microsoft.PowerShell.Core/About/about_Hash_Tables.md)

[Export-FormatData](Export-FormatData.md)

[Format-Custom](Format-Custom.md)

[Format-Hex](Format-Hex.md)

[Format-List](Format-List.md)

[Format-Wide](Format-Wide.md)

[Get-FormatData](Get-FormatData.md)

[Get-Member](Get-Member.md)

[Get-CimInstance](../CimCmdlets/Get-CimInstance.md)

[Update-FormatData](Update-FormatData.md)
