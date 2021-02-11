---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/29/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/show-command?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Show-Command
ms.openlocfilehash: e6995da1e9ff2a1678c5703b79223481533bc9a5
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346158"
---
# Show-Command

## 概要
グラフィカルウィンドウに PowerShell コマンド情報を表示します。

## SYNTAX

```
Show-Command [[-Name] <String>] [-Height <Double>] [-Width <Double>] [-NoCommonParameter]
 [-ErrorPopup] [-PassThru] [<CommonParameters>]
```

## Description

`Show-Command`コマンドレットを使用すると、コマンドウィンドウで PowerShell コマンドを作成できます。 コマンド ウィンドウの機能を使用して、コマンドを実行することやコマンドを返すことができます。

`Show-Command` は、非常に便利な教育および学習ツールです。 `Show-Command` コマンドレット、関数、ワークフロー、CIM コマンドなど、すべてのコマンドの種類に対して機能します。

パラメーターを指定しない場合、 `Show-Command` インストールされているすべてのモジュールで使用可能なすべてのコマンドを一覧表示するコマンドウィンドウが表示されます。 モジュール内のコマンドを検索するには、[モジュール] ボックスの一覧からモジュールを選択します。 コマンドを選択するには、コマンド名をクリックします。

コマンドウィンドウを使用するには、名前を使用する **か、コマンド一覧の** コマンド名をクリックして、コマンドを選択します。 各パラメーターセットは、別のタブに表示されます。アスタリスクは、必須のパラメーターを示します。 パラメーターに値を入力するには、テキスト ボックスに値を入力するか、ドロップダウン ボックスから値を選択します。 スイッチ パラメーターを追加するには、パラメーター チェック ボックスをオンにします。

準備ができたら、[ **Copy** ] をクリックして、作成したコマンドをクリップボードにコピーするか、[ **Run** ] をクリックしてコマンドを実行します。 また、 **PassThru** パラメーターを使用して、PowerShell コンソールなどのホストプログラムにコマンドを返すこともできます。 コマンドの選択をキャンセルし、すべてのコマンドを表示するビューに戻るには、Ctrl キーを押しながら、選択したコマンドをクリックします。

PowerShell Integrated Scripting Environment (ISE) では、ウィンドウのバリエーション `Show-Command` が既定で表示されます。 このコマンドウィンドウの使用方法の詳細については、PowerShell ISE のヘルプトピックを参照してください。

このコマンドレットは、PowerShell 7 で再導入されました。

このコマンドレットはユーザーインターフェイスを必要とするため、Windows Server Core または Windows Nano Server では機能しません。 このコマンドレットは、Windows デスクトップをサポートする Windows システムでのみ使用できます。

## 例

### 例 1: [コマンド] ウィンドウを開く

この例では、ウィンドウの既定のビューを表示し `Show-Command` ます。 [ **コマンド** ] ウィンドウには、コンピューターにインストールされているすべてのモジュールのすべてのコマンドの一覧が表示されます。

```powershell
Show-Command
```

### 例 2: [コマンド] ウィンドウでコマンドレットを開く

この例では `Invoke-Command` 、コマンドレットを **コマンド** ウィンドウに表示します。 この表示を使用して、コマンドを実行でき `Invoke-Command` ます。

```powershell
Show-Command -Name "Invoke-Command"
```

### 例 3: 指定されたパラメーターを使用してコマンドレットを開く

このコマンドを実行すると `Show-Command` 、コマンドレットのウィンドウが開き `Connect-PSSession` ます。

```powershell
Show-Command -Name "Connect-PSSession" -Height 700 -Width 1000 -ErrorPopup
```

**高さ** と **幅** のパラメーターでは、コマンドウィンドウの次元を指定します。 **Errorpopup** パラメーターを指定すると、エラーコマンドウィンドウが表示されます。

[ **実行** ] をクリックすると、コマンドラインでコマンドを入力した `Connect-PSSession` 場合と同様に、コマンドが実行され `Connect-PSSession` ます。

### 例 4: コマンドレットの新しい既定のパラメーター値を指定する

この例では、自動変数を使用して、 `$PSDefaultParameterValues` コマンドレットの **Height** 、 **Width** 、および **errorpopup** パラメーターに新しい既定値を設定し `Show-Command` ます。

```powershell
$PSDefaultParameterValues = @{
    "Show-Command:Height" = 700
    "Show-Command:Width" = 1000
    "Show-Command:ErrorPopup" = $True
}
```

コマンドを実行すると `Show-Command` 、新しい既定値が自動的に適用されます。 すべての PowerShell セッションでこれらの既定値を使用するには、 `$PSDefaultParameterValues` powershell プロファイルに変数を追加します。 詳細については、「 [about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md) 」および「 [about_Parameters_Default_Values](../Microsoft.PowerShell.Core/About/about_Parameters_Default_Values.md)」を参照してください。

### 例 5: 出力をグリッドビューに送信する

このコマンドは、コマンドレットとコマンドレットを一緒に使用する方法を示し `Show-Command` て `Out-GridView` います。

```powershell
Show-Command Get-ChildItem | Out-GridView
```

コマンドレットを使用してコマンド `Show-Command` レットのコマンドウィンドウを開き `Get-ChildItem` ます。
[ **実行** ] ボタンをクリックすると、コマンドが実行され、 `Get-ChildItem` 出力が生成されます。 パイプライン演算子 (|) は、コマンドの出力を `Get-ChildItem` コマンドレットに送信します。これにより、 `Out-GridView` `Get-ChildItem` 対話型ウィンドウに出力が表示されます。

### 例 6: [コマンド] ウィンドウで作成したコマンドを表示する

この例は、ウィンドウで作成したコマンドを示して `Show-Command` います。 このコマンドは、 **PassThru** パラメーターを使用します。これにより、結果が文字列として返され `Show-Command` ます。

```powershell
Show-Command -PassThru
```

```Output
Get-EventLog -LogName "Windows PowerShell" -Newest 5
```

たとえば、 `Show-Command` `Get-EventLog` Windows PowerShell イベントログ内の最新の5つのイベントを取得するコマンドをウィンドウで作成し、[ **OK** ] をクリックすると、上記の出力が返されます。 コマンド文字列を表示すると、PowerShell の学習に役立ちます。

### 例 7: コマンドを変数に保存する

この例では、コマンドレットの **PassThru** パラメーターを使用したときに取得したコマンド文字列を実行する方法を示し `Show-Command` ます。 この方法により、コマンドを表示して使用することができます。

```powershell
$C = Show-Command -PassThru
$C
Invoke-Expression $C
```

```Output
Get-EventLog -LogName "PowerShell" -Newest 5

Index Time          EntryType   Source                 InstanceID Message
----- ----          ---------   ------                 ---------- -------
11520 Dec 16 16:37  Information Windows PowerShell            400 Engine state is changed from None to Available...
11519 Dec 16 16:37  Information Windows PowerShell            600 Provider "Variable" is Started. ...
11518 Dec 16 16:37  Information Windows PowerShell            600 Provider "Registry" is Started. ...
11517 Dec 16 16:37  Information Windows PowerShell            600 Provider "Function" is Started. ...
11516 Dec 16 16:37  Information Windows PowerShell            600 Provider "FileSystem" is Started. ...
```

最初のコマンドは、コマンドレットの **PassThru** パラメーター `Show-Command` を使用して、コマンドの結果を変数に保存し `$C` ます。 この場合は、ウィンドウを使用して、 `Show-Command` `Get-EventLog` Windows PowerShell イベントログ内の最新の5つのイベントを取得するコマンドを作成します。 [ **OK** ] をクリックすると、 `Show-Command` コマンド文字列が返されます。この文字列は変数に保存されてい `$C` ます。

### 例 8: コマンドの出力を変数に保存する

この例では、 **Errorpopup** パラメーターを使用して、コマンドの出力を変数に保存します。

```powershell
$P = Show-Command Get-Process -ErrorPopup
$P
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    473      33    94096     112532   709     2.06   4492 powershell
```

**ErrorPopup** は、ウィンドウにエラーを表示するだけでなく、新しいコマンドを作成する代わりにコマンドの出力を現在のコマンドに返します。 このコマンドを実行すると、 `Show-Command` ウィンドウが開きます。 ウィンドウの機能を使用して、パラメーターの値を設定することができます。 コマンドを実行するには、ウィンドウの [ **実行** ] ボタンをクリックし `Show-Command` ます。

## PARAMETERS

### -ErrorPopup

コマンドレットによってエラーがコマンドラインに表示されるだけでなく、ポップアップウィンドウに表示されることを示します。 既定では、ウィンドウで実行されるコマンドが `Show-Command` エラーを生成すると、そのエラーはコマンドラインでのみ表示されます。

また、(ウィンドウの [ **実行** ] ボタンを使用して) コマンドを実行すると、コマンドを `Show-Command` 実行して出力を新しいコマンドに返すのではなく、 **errorpopup** パラメーターによってコマンドの結果が現在のコマンドに返されます。 この機能を使用すると、コマンドの結果を変数に保存できます。

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

### -高さ

ウィンドウの高さをピクセル単位で指定し `Show-Command` ます。 300 以上で画面の解像度以下の値を入力します (ピクセル単位)。 値が大きすぎて画面にコマンドウィンドウを表示できない場合は、に `Show-Command` よってエラーが生成されます。 既定の高さは 600 ピクセルです。 `Show-Command` **Name** パラメーターを含むコマンドの場合、既定の高さは300ピクセルになります。

```yaml
Type: System.Double
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

指定されたコマンドについて、コマンド ウィンドウが表示されます。 コマンドレット、関数、または CIM コマンドの名前など、1つのコマンドの名前を入力します。 このパラメーターを省略した場合、には、 `Show-Command` コンピューターにインストールされているすべてのモジュールのすべての PowerShell コマンドを一覧表示するコマンドウィンドウが表示されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: CommandName

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoCommonParameter

このコマンドレットがコマンド表示の共通パラメーターセクションを省略することを示します。 既定では、共通パラメーターはコマンド ウィンドウの下部の展開可能なセクションに表示されます。

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

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。 コマンド文字列を実行するには、コマンドプロンプトでコピーして貼り付けます。または、変数に保存し、コマンドレットを使用して `Invoke-Expression` 変数内の文字列を実行します。

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

### -Width

ウィンドウの幅をピクセル単位で指定し `Show-Command` ます。 300 以上で画面の解像度以下の値を入力します (ピクセル単位)。 値が大きすぎて画面にコマンドウィンドウを表示できない場合は、に `Show-Command` よってエラーが生成されます。 既定の幅は 300 ピクセルです。

```yaml
Type: System.Double
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

### なし

入力をにパイプすることはできません `Show-Command` 。

## 出力

### None、System.string、System.object

**PassThru** パラメーターを使用すると、は `Show-Command` コマンド文字列を返します。 **Errorpopup** パラメーターを使用すると、は `Show-Command` コマンド出力 (すべてのオブジェクト) を返します。 それ以外の場合、 `Show-Command` は出力を生成しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

`Show-Command` リモートセッションでは機能しません。

## 関連リンク
