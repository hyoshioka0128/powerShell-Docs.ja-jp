---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/out-gridview?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-GridView
ms.openlocfilehash: 840aa38ff17ceaf7dc65ca838da020c3d8c27952
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94344390"
---
# Out-GridView

## 概要
別のウィンドウの対話型のテーブルに出力を送信します。

## SYNTAX

### PassThru (既定値)

```
Out-GridView [-InputObject <PSObject>] [-Title <String>] [-PassThru] [<CommonParameters>]
```

### Wait

```
Out-GridView [-InputObject <PSObject>] [-Title <String>] [-Wait] [<CommonParameters>]
```

### OutputMode

```
Out-GridView [-InputObject <PSObject>] [-Title <String>] [-OutputMode <OutputModeOption>]
 [<CommonParameters>]
```

## Description

`Out-GridView`コマンドレットは、コマンドからの出力をグリッドビューウィンドウに送信します。このウィンドウには、対話形式のテーブルに出力が表示されます。

このコマンドレットはユーザーインターフェイスを必要とするため、Windows Server Core または Windows Nano Server では機能しません。

テーブルの次の機能を使用して、データを確認することができます。

- 列の非表示、表示、および並べ替え
- 行の並べ替え
- クイック フィルター
- 条件フィルターの追加
- コピーと貼り付け

詳しい手順については、この記事の「 [メモ](#notes) 」セクションを参照してください。

> [!NOTE]
> このコマンドレットは、PowerShell 7 で再導入されました。 このコマンドレットは、Windows デスクトップをサポートする Windows システムでのみ使用できます。 このコマンドレットのクロスプラットフォームバージョンについては、PowerShell ギャラリーの「 [Graphのツール](https://www.powershellgallery.com/packages/Microsoft.PowerShell.GraphicalTools) 」モジュールを参照してください。

## 例

### 例 1: グリッドビューにプロセスを出力する

この例では、ローカルコンピューター上で実行されているプロセスを取得し、それらをグリッドビューウィンドウに送信します。

```powershell
Get-Process | Out-GridView
```

### 例 2: 変数を使用してプロセスをグリッドビューに出力する

また、この例では、ローカルコンピューター上で実行されているプロセスを取得し、それらをグリッドビューウィンドウに送信します。

```powershell
$P = Get-Process
$P | Out-GridView
```

コマンドレットの出力 `Get-Process` は、変数に保存され `$P` ます。 次 `$P` に、はにパイプ処理され `Out-GridView` ます。

### 例 3: 選択したプロパティをグリッドビューに表示する

この例では、実行中のプロセスの選択したプロパティをグリッドビューで表示します。

```powershell
Get-Process | Select-Object -Property Name, WorkingSet, PeakWorkingSet |
  Sort-Object -Property WorkingSet -Descending | Out-GridView
```

の出力 `Get-Process` は、 `Select-Object` **Name** 、 **ワーキング** セット、 **peakworkingset** セットプロパティを選択するためにパイプ処理されます。 もう1つのパイプライン演算子は、フィルター処理されたオブジェクトをコマンドレットに送信し `Sort-Object` て、 **ワーキング** セットプロパティの値によって降順に並べ替えます。
次に、並べ替えられた結果をにパイプ処理し `Out-GridView` ます。 これで、グリッド ビューの機能を、検索、並べ替え、データのフィルター処理に使用できるようになりました。

### 例 4: 出力を変数に保存し、グリッドビューを出力する

この例では、コマンドレットの出力を変数に保存し、に送信し `Out-GridView` ます。

```powershell
($A = Get-ChildItem -Path $PSHOME -Recurse) | Out-GridView
```

`Get-ChildItem` 自動変数を使用して、PowerShell インストールディレクトリとそのサブディレクトリにあるすべてのファイルを取得し `$PSHOME` ます。 コマンド内のかっこは、演算の順序を確立します。 その結果、コマンドからの出力は、 `Get-ChildItem` に送信される前に変数に保存され `$A` `Out-GridView` ます。

### 例 5: 指定したコンピューターのグリッドビューへの出力プロセス

この例では、Server01 コンピューター上で実行されているプロセスをグリッドビューウィンドウに表示します。

```powershell
Get-Process -ComputerName "Server01" | ogv -Title "Processes - Server01"
```

例はを使用し `ogv` ます。これは、 `Out-GridView` コマンドレットのエイリアスです。 **Title** パラメーターは、ウィンドウタイトルを指定します。

### 例 6: リモートコンピューターからグリッドビューにデータを出力する

この例では、リモートコンピューターから収集されたデータをに送信する方法を示し `Out-GridView` ます。

```powershell
Invoke-Command -ComputerName S1, S2, S3 -ScriptBlock {Get-Culture} | Out-GridView
```

`Invoke-Command``Get-Culture`3 台のリモートコンピューター上で実行されます。 結果として得られるデータはにパイプ処理され `Out-GridView` ます。 リモートコンピューターで実行されるスクリプトブロックには、コマンドが含まれていないことに注意して `Out-GridView` ください。 含まれている場合、各リモート コンピュータでグリッド ビュー ウィンドウを開こうとすると、コマンドはエラーになります。

### 例 7: を使用して複数の項目を渡す `Out-GridView`

この例では、ウィンドウから複数のプロセスを選択でき `Out-GridView` ます。 選択したプロセスがコマンドに渡され、 `Export-Csv` ファイルに書き込まれ `ProcessLog.csv` ます。

```powershell
Get-Process | Out-GridView -PassThru | Export-Csv -Path .\ProcessLog.csv
```

の **PassThru** パラメーターを `Out-GridView` 使用すると、パイプラインで複数の項目を送信できます。 **PassThru**  パラメーターを使用すると、 **OutputMode** パラメーターの値を **Multiple** に設定した場合と同じ効果が得られます。

### 例 8: の Windows ショートカットを作成する `Out-GridView`

この例では、の **Wait** パラメーターを使用して、 `Out-GridView` ウィンドウへの Windows ショートカットを作成する方法を示し `Out-GridView` ます。

```powershell
pwsh -Command "Get-Service | Out-GridView -Wait"
```

このコマンドラインは、Windows ショートカットで使用できます。 **Wait** パラメーターを指定しないと、PowerShell はウィンドウが開いた直後に終了し `Out-GridView` ます。これにより、 `Out-GridView` ウィンドウがすぐに閉じられます。

## PARAMETERS

### -InputObject

コマンドレットが入力として受け入れるオブジェクトを指定し `Out-GridView` ます。

**InputObject** パラメーターを使用してオブジェクトのコレクションをに送信すると `Out-GridView` 、に `Out-GridView` よってコレクションが1つのコレクションオブジェクトとして扱われ、コレクションを表す1つの行が表示されます。 コレクション内の各オブジェクトを表示するには、パイプライン演算子 () を使用して、 `|` オブジェクトをに送信し `Out-GridView` ます。

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

### -OutputMode

対話型ウィンドウがパイプラインを他のコマンドに入力として送信する項目を指定します。
既定では、このコマンドレットによる出力はありません。 対話型のウィンドウからパイプラインを介して項目を送るには、項目をクリックして選択し、[OK] をクリックします。

このパラメーターの値が、パイプラインに送信できるアイテムの数を決定します。

- [なし] :  項目がありません。 これが既定値です。
- 単一: 項目数は 0 または 1 です。 次のコマンドが受け取ることができる入力オブジェクトが 1 つのみの場合、この値を使用します。
- 複数. 項目数は、0、1、または複数です。 次のコマンドが受け取ることができる入力オブジェクトが複数の場合、この値を使用します。 この値は、 **Passthru** パラメーターと同じです。

```yaml
Type: Microsoft.PowerShell.Commands.OutputModeOption
Parameter Sets: OutputMode
Aliases:
Accepted values: None, Single, Multiple

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

コマンドレットが、対話型ウィンドウからパイプラインを介して他のコマンドに入力として項目を送信することを示します。 既定では、このコマンドレットによる出力はありません。 このパラメーターを使用すると、 **OutputMode** パラメーターの値を **Multiple** に設定した場合と同じ効果が得られます。

対話型のウィンドウからパイプラインを介して項目を送るには、項目をクリックして選択し、[OK] をクリックします。 Shift キーや Ctrl キーを使用した複数選択がサポートされています。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PassThru
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Title

ウィンドウのタイトルバーに表示されるテキストを指定し `Out-GridView` ます。 既定では、を呼び出すコマンドがタイトルバーに表示され `Out-GridView` ます。

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

### -Wait

コマンドレットによってコマンドプロンプトが抑制され、ウィンドウが閉じられるまで Windows PowerShell が終了しないことを示し `Out-GridView` ます。 既定では、ウィンドウが開いたときにコマンドプロンプトが返され `Out-GridView` ます。

この機能を使用すると、 `Out-GridView` Windows ショートカットのコマンドレットを使用できます。 `Out-GridView` **Wait** パラメーターを指定せずにショートカットでを使用すると、 `Out-GridView` PowerShell が閉じる前にウィンドウが一時的に表示されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Wait
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

このコマンドレットには、任意のオブジェクトを送信できます。

## 出力

### なし

通常、 `Out-GridView` はオブジェクトを返しません。 **PassThru** パラメーターを使用すると、選択された行を表すオブジェクトがパイプラインに返されます。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

リモート コマンドを使用して、別のコンピューター上でグリッド ビュー ウィンドウを開くことはできません。

に送信するコマンドの出力は、コマンドレットやコマンドレットなどを `Out-GridView` 使用して書式設定することはできません `Format` `Format-Table` `Format-Wide` 。 プロパティを選択するには、コマンドレットを使用し `Select-Object` ます。

リモート コマンドから逆シリアル化された出力をグリッド ビュー ウィンドウで適切に書式設定することはできません。

**のキーボードショートカット**`Out-GridView`

|              使用するキー              |                                   実行する操作                                    |
| --------------------------------------- | -------------------------------------------------------------------------------------------- |
| <kbd>タブ</kbd>                          | **フィルター** ボックスから [ **条件の追加** ] メニューにカーソルを移動し、テーブルに戻ります。 |
| <kbd>↑</kbd>                      | 1行上に移動します。 最初のデータ行から列ヘッダーに移動します。                             |
| <kbd>下方向キー</kbd>                    | 1行下に移動します。                                                                           |
| <kbd>←</kbd>                    | 列ヘッダー行で、1列左に移動します。                                                  |
| <kbd>→</kbd>                   | 列ヘッダー行で、1列右に移動します。                                                 |
| <kbd>ContextMenuKey</kbd>               | 列ヘッダー行には、[列の選択] オプションが表示されます。                                    |
| <kbd>Enter</kbd> または <kbd>space</kbd> | 列ヘッダー行で、列データを並べ替えます (A ~ Z、Z-A を切り替えます)。                                    |

**グリッドビューウィンドウ機能を使用する方法**

**列を非表示または表示するには、次の手順に従います。**

1. 任意の列ヘッダーを右クリックし、[ **列の選択** ] をクリックします。
2. **[列の選択** ] ダイアログボックスで、方向キーを使用して、選択した列間の列を [使用できる列] ボックスに移動します。 **[列の選択** ] ボックス内の列のみがグリッドビューウィンドウに表示されます。

**列を並べ替えるには、次の手順に従います。**

列を目的の場所にドラッグアンドドロップすることができます。 または、次の手順を使用します。

1. 任意の列ヘッダーを右クリックし、[ **列の選択** ] をクリックします。
2. **[列の選択** ] ダイアログボックスで、[ **上へ移動** ] および [ **下へ移動** ] ボタンを使用して、列の順序を変更します。 一覧の上部にある列は、グリッド ビュー ウィンドウで、一覧の下部の列の左側に表示されます。

**テーブルのデータを並べ替える方法**

- データを並べ替えるには、列ヘッダーをクリックします。
- 並べ替え順序を変更するには、列ヘッダーをもう一度クリックします。 同じヘッダーをクリックするたびに、並べ替え順序が昇順と降順で切り替えられます。 現在の順序は、列ヘッダーにある三角形で示されます。

**テーブルのデータを選択する方法**

- 行を選択するには、行を選択するか、上矢印または下矢印を使用してその行に移動します。
- (ヘッダー行を除く) すべての行を選択するには、 <kbd>ctrl</kbd> + <kbd>A</kbd>キーを押します。
- 連続する行を選択するには、 <kbd>shift</kbd> キーを押しながら行をクリックするか、方向キーを使用します。
- 連続しない rows を選択するには、CTRL キーを押し <kbd>ながら</kbd> クリックして選択に行を追加します。
- 列を選択することや、列ヘッダー行全体を選択することはできません。

**行をコピーする方法**

- テーブルから1つ以上の行をコピーするには、行を選択し、CTRL + C キーを押します。

  任意のテキストやスプレッドシート プログラムにデータを貼り付けることができます。 列や行の一部をコピーすることはできません。また、列ヘッダー行をコピーすることはできません。

**テーブルで検索する方法 (クイックフィルター)**

テーブル内のデータを検索するには、[フィルター] ボックスを使用します。 ボックスに入力すると、指定したテキストを含む項目のみがテーブルに表示されます。

- テキストを検索します。 テーブル内のテキストを検索するには、[フィルター] ボックスに検索するテキストを入力します。
- 複数の単語を検索します。 テーブル内の複数の単語を検索するには、単語をスペースで区切って入力します。 `Out-GridView` すべての語 (論理 AND) を含む行を表示します。
- リテラル語句を検索します。 スペースや特殊文字を含む語句を検索するには、語句を引用符で囲みます。 `Out-GridView` 語句と完全に一致する行を表示します。
- 列を検索します。 1 列以上でテキストを検索するには、次の形式を使用します。

  `<column>:<text> [<column>:<text>] ...`

  たとえば、[ **DisplayName** ] 列で "Net" を検索するには、[ **フィルター** ] ボックスに次のように入力します。

  `displayname:net`

  [ **DisplayName** ] 列と [ **Name** ] 列に "Net" を含む行を検索するには、[ **フィルター** ] ボックスに次のように入力します。

  `displayname:net name:net`

- 検索をオフにします。 テーブル全体を再度表示するには、 **フィルター** ボックスの右上隅にある赤い [X] ボタンをクリックするか、[ **フィルター** ] ボックスからテキストを削除します。

**条件を使用したテーブルのフィルター処理**

ルールまたは条件を使用して、テーブルに表示するアイテムを決定できます。 項目は、設定したすべての条件を満たしている場合にのみ表示されます。 使用可能な基準は、グリッド ビュー ウィンドウに表示されるオブジェクトのプロパティと、これらのプロパティの .NET Framework の型によって決まります。

各条件は、次の形式で指定します。

`<column> <operator> <value>`

さまざまなプロパティの条件は **、と** によって接続されます。 同じプロパティの条件は、 **または** によって接続されます。 論理コネクタを変更することはできません。

条件は表示のみに影響します。 項目はテーブルから削除されません。

**条件を追加する方法**

1. [条件の **追加** ] メニューボタンを表示するには、ウィンドウの右上隅にある展開矢印をクリックします。
2. [ **条件の追加** ] メニューボタンをクリックします。
3. 列 (プロパティ) をクリックして選択します。 1 つまたは複数のプロパティを選択することができます。
4. プロパティの選択が完了したら、[ **追加** ] ボタンをクリックします。
5. 追加を取り消すには、[ **キャンセル** ] をクリックします。
6. 条件をさらに追加するには、[ **条件の追加** ] をもう一度クリックします。

**条件の編集方法**

- 演算子を変更するには、青の演算子の値をクリックし、ドロップダウンリストから別の演算子を選択します。
- 値を入力または変更するには、[値] ボックスに値を入力します。 無効な値を入力した場合、円に X が付けられたアイコンが表示されます。 削除するには、値を変更します。
- ステートメント **または** ステートメントを作成するには、同じプロパティを持つ条件を追加します。

**条件を削除する方法**

- 選択した条件を削除するには、各条件の横にある赤い X 印をクリックします。
- すべての条件を削除するには、[ **すべてクリア** ] ボタンをクリックします。

## 関連リンク

[Out-File](Out-File.md)

[Out-Printer](Out-Printer.md)

[Out-String](Out-String.md)

