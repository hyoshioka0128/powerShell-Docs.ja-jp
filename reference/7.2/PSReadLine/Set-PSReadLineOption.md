---
external help file: Microsoft.PowerShell.PSReadLine2.dll-Help.xml
Locale: en-US
Module Name: PSReadLine
ms.date: 11/23/2020
online version: https://docs.microsoft.com/powershell/module/psreadline/set-psreadlineoption?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-PSReadLineOption
ms.openlocfilehash: cac2c2bb8ce1f3a28c0d91c7503b3ac4d038ccad
ms.sourcegitcommit: 077488408c820c860131382324bdd576d0edf52a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/24/2020
ms.locfileid: "99601800"
---
# Set-PSReadLineOption

## 構文
**Psreadline** でのコマンドライン編集の動作をカスタマイズします。

## 構文

```
Set-PSReadLineOption [-EditMode <EditMode>] [-ContinuationPrompt <String>] [-HistoryNoDuplicates]
 [-AddToHistoryHandler <System.Func`2[System.String,System.Object]>]
 [-CommandValidationHandler <System.Action`1[System.Management.Automation.Language.CommandAst]>]
 [-HistorySearchCursorMovesToEnd] [-MaximumHistoryCount <Int32>] [-MaximumKillRingCount <Int32>]
 [-ShowToolTips] [-ExtraPromptLineCount <Int32>] [-DingTone <Int32>] [-DingDuration <Int32>]
 [-BellStyle <BellStyle>] [-CompletionQueryItems <Int32>] [-WordDelimiters <String>]
 [-HistorySearchCaseSensitive] [-HistorySaveStyle <HistorySaveStyle>] [-HistorySavePath <String>]
 [-AnsiEscapeTimeout <Int32>] [-PromptText <String[]>] [-ViModeIndicator <ViModeStyle>]
 [-ViModeChangeHandler <ScriptBlock>] [-PredictionSource <PredictionSource>]
 [-PredictionViewStyle <PredictionViewStyle>] [-Colors <Hashtable>] [<CommonParameters>]
```

## 説明

コマンドラインを編集しているときに、コマンドレットによって `Set-PSReadLineOption` **psreadline** モジュールの動作がカスタマイズされます。 **Psreadline** の設定を表示するには、を使用し `Get-PSReadLineOption` ます。

## 例

### 例 1: 前景色と背景色を設定する

この例では、 **Psreadline** を設定して、 **コメント** トークンを緑色の前景テキストがグレーの背景に表示されるようにします。 この例で使用されているエスケープシーケンスでは、 **32** は前景色を表し、 **47** は背景色を表します。

```powershell
Set-PSReadLineOption -Colors @{ "Comment"="`e[32;47m" }
```

前景テキストの色のみを設定するように選択できます。 たとえば、 **コメント** トークンの背景色が明るい緑で表示さ ``"Comment"="`e[92m"`` れます。

### 例 2: ベルのスタイルを設定する

この例では、 **Psreadline** は、ユーザーに注意する必要があるエラーまたは条件に応答します。
**ベル Lstyle** は、1221 Hz の60ミリ秒で可聴ビープ音を発するように設定されています。

```powershell
Set-PSReadLineOption -BellStyle Audible -DingTone 1221 -DingDuration 60
```

> [!NOTE]
> この機能は、プラットフォーム上のすべてのホストで動作しない可能性があります。

### 例 3: 複数のオプションを設定する

`Set-PSReadLineOption` ハッシュテーブルを使用して複数のオプションを設定できます。

```powershell
$PSReadLineOptions = @{
    EditMode = "Emacs"
    HistoryNoDuplicates = $true
    HistorySearchCursorMovesToEnd = $true
    Colors = @{
        "Command" = "#8181f7"
    }
}
Set-PSReadLineOption @PSReadLineOptions
```

ハッシュテーブルによって `$PSReadLineOptions` 、キーと値が設定されます。 `Set-PSReadLineOption` では、キーと値を使用して `@PSReadLineOptions` **psreadline** オプションを更新します。

ハッシュテーブル名を入力したキーと値は、 `$PSReadLineOptions` PowerShell コマンドラインで表示できます。

### 例 4: 複数の色のオプションを設定する

この例では、1つのコマンドで複数の color 値を設定する方法を示します。

```powershell
Set-PSReadLineOption -Colors @{
  Command            = 'Magenta'
  Number             = 'DarkGray'
  Member             = 'DarkGray'
  Operator           = 'DarkGray'
  Type               = 'DarkGray'
  Variable           = 'DarkGreen'
  Parameter          = 'DarkGreen'
  ContinuationPrompt = 'DarkGray'
  Default            = 'DarkGray'
}
```

### 例 5: 複数の型の色の値を設定する

この例では、 **Psreadline** に表示されるトークンの色を設定する方法について、3種類の方法を示します。

```powershell
Set-PSReadLineOption -Colors @{
 # Use a ConsoleColor enum
 "Error" = [ConsoleColor]::DarkRed

 # 24 bit color escape sequence
 "String" = "$([char]0x1b)[38;5;100m"

 # RGB value
 "Command" = "#8181f7"
}
```

### 例 6: ViModeChangeHandler を使用して Vi モードの変更を表示する

この例では、 **Vi** モードの変更に応じて、VT escape のカーソル変更を出力します。

```powershell
function OnViModeChange {
    if ($args[0] -eq 'Command') {
        # Set the cursor to a blinking block.
        Write-Host -NoNewLine "`e[1 q"
    } else {
        # Set the cursor to a blinking line.
        Write-Host -NoNewLine "`e[5 q"
    }
}
Set-PSReadLineOption -ViModeIndicator Script -ViModeChangeHandler $Function:OnViModeChange
```

**Onvimodechange** 関数は、 **Vi** モード (insert および command) のカーソルオプションを設定します。
**ViModeChangeHandler** は、プロバイダーを使用し `Function:` て、スクリプトブロックオブジェクトとして **onvimodechange** を参照します。

詳細については、「[about_Providers](/powershell/module/microsoft.powershell.core/about/about_providers)」を参照してください。

## パラメーター

### -Addtohistory ハンドラー

**Psreadline** 履歴に追加するコマンドを制御する **ScriptBlock** を指定します。

**ScriptBlock** は、コマンドラインを入力として受け取ります。 **ScriptBlock** がを返した場合は、 `$True` コマンドラインが履歴に追加されます。

```yaml
Type: System.Func`2[System.String,System.Object]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -AnsiEscapeTimeout

このオプションは、入力がリダイレクトされるときのウィンドウに固有です。たとえば、またはで実行されている場合です `tmux` `screen` 。

Windows でのリダイレクト入力では、多くのキーがエスケープ文字で始まる文字のシーケンスとして送信されます。 1つのエスケープ文字の後により多くの文字と有効なエスケープシーケンスを区別することはできません。

ターミナルは、ユーザーの種類よりも高速に文字を送信できることを前提としています。 **Psreadline** は、このタイムアウトを待機してから、完全なエスケープシーケンスを受け取っています。

入力時にランダムまたは予期しない文字が表示する場合は、このタイムアウトを調整できます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 100
Accept pipeline input: False
Accept wildcard characters: False
```

### -ベル Lstyle

**Psreadline** がさまざまなエラーやあいまいな状況にどのように応答するかを指定します。

有効な値は次のとおりです。

- **可聴**: 短いビープ音。
- **ビジュアル**: テキストが短時間点滅します。
- **None**: フィードバックがありません。

```yaml
Type: Microsoft.PowerShell.BellStyle
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Audible
Accept pipeline input: False
Accept wildcard characters: False
```

### -色

**Colors** パラメーターは、 **psreadline** によって使用されるさまざまな色を指定します。

引数は、キーが色を指定する要素と値を指定するハッシュテーブルです。
詳細については、「 [about_Hash_Tables](/powershell/module/microsoft.powershell.core/about/about_hash_tables)」を参照してください。

色には、 **ConsoleColor** の値か、 `[ConsoleColor]::Red` 有効な ANSI エスケープシーケンスを指定できます。 有効なエスケープシーケンスは、ターミナルによって異なります。 PowerShell 5.0 では、赤いテキストのエスケープシーケンスの例はです `$([char]0x1b)[91m` 。 PowerShell 6 以降では、同じエスケープシーケンスは `` `e[91m`` です。 次の型を含むその他のエスケープシーケンスを指定できます。

PSReadLine 2.2.0 でののカスタマイズをサポートするために、2色の設定が追加されました `ListView` 。

- **ListPredictionColor** -先頭の `>` 文字と末尾のソース名 (など) の色を設定し `[History]` ます。 既定では、が `DarkYellow` 前景色として使用されます。
- **ListPredictionSelectedColor** -リスト項目が選択されていることを示す色を設定します。 既定では、は `DarkBlack` 背景色としてを使用します。

- 256色
- 24ビットカラー
- 前景、背景、または両方
- 反転、太字

ANSI カラーコードの詳細については、Wikipedia の「 [ansi escape code](https://wikipedia.org/wiki/ANSI_escape_code#Colors_) 」を参照してください。

有効なキーは次のとおりです。

- **続行 ationprompt**: 継続プロンプトの色。
- **強調**: 強調色。 たとえば、履歴を検索するときに一致するテキストです。
- **エラー**: エラーの色。 たとえば、プロンプトでを入力します。
- **Selection**: メニューの選択または選択されたテキストを強調表示する色です。
- **既定** 値: 既定のトークンの色。
- **Comment**: コメントトークンの色。
- **キーワード**: キーワードトークンの色。
- **String**: 文字列トークンの色。
- **Operator**: 演算子のトークンの色。
- **Variable**: 変数のトークンの色。
- **Command**: コマンドトークンの色。
- **パラメーター**: パラメーターのトークンの色。
- **Type**: 型トークンの色。
- **Number**: 数値トークンの色。
- **メンバー**: メンバー名トークンの色。
- **InlinePrediction**: 予測提案のインラインビューの色です。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CommandValidationHandler

**Validateandacceptline** から呼び出される **ScriptBlock** を指定します。 例外がスローされた場合、検証は失敗し、エラーが報告されます。

例外をスローする前に、検証ハンドラーはエラーの発生時にカーソルを置いて、簡単に修正できるようにします。 検証ハンドラーは、などのコマンドラインを変更して、一般的な入力ミスを修正することもできます。

**Validateandacceptline** を使用すると、動作しないコマンドで履歴が乱雑にならないようにすることができます。

```yaml
Type: System.Action`1[System.Management.Automation.Language.CommandAst]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -補完 Queryitems

プロンプトを表示せずに表示される完了項目の最大数を指定します。

表示する項目の数がこの値よりも大きい場合、 **Psreadline** は、完了項目を表示する前に、 **[はい/いいえ]** を表示します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 100
Accept pipeline input: False
Accept wildcard characters: False
```

### -続行 Ationprompt

複数行の入力が入力された場合に、後続の行の先頭に表示される文字列を指定します。 既定値は2倍大きい記号 ( `>>` ) です。 空の文字列は有効です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: >>
Accept pipeline input: False
Accept wildcard characters: False
```

### -有効期間

**ベル Lstyle** が **可聴** に設定されている場合のビープ音の期間を指定します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 50ms
Accept pipeline input: False
Accept wildcard characters: False
```

### -外調

**ベル Lstyle** が **可聴** に設定されている場合のビープ音のトーンをヘルツ (hz) で指定します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 1221
Accept pipeline input: False
Accept wildcard characters: False
```

### -EditMode

コマンドライン編集モードを指定します。 このパラメーターを使用すると、によって設定されたキーバインドがリセットさ `Set-PSReadLineKeyHandler` れます。

有効な値は次のとおりです。

- **Windows**: キーバインドは、PowerShell、cmd、および Visual Studio をエミュレートします。
- **Emacs**: キーバインドは Bash または Emacs をエミュレートします。
- **Vi**: キーバインドは vi をエミュレートします。

`Get-PSReadLineKeyHandler`現在構成されている **EditMode** のキーバインドを表示するには、を使用します。

```yaml
Type: Microsoft.PowerShell.EditMode
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Windows
Accept pipeline input: False
Accept wildcard characters: False
```

### -ExtraPromptLineCount

余分な行の数を指定します。

プロンプトが複数行にわたっている場合は、このパラメーターの値を指定します。 このオプションは、出力を表示した後に **Psreadline** がプロンプトを表示するときに余分な行を使用できるようにする場合に使用します。 たとえば、 **Psreadline** は入力候補の一覧を返します。

このオプションは、以前のバージョンの **Psreadline** の場合よりも小さくする必要がありますが、関数を使用するときに便利です `InvokePrompt` 。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 0
Accept pipeline input: False
Accept wildcard characters: False
```

### -履歴の重複

このオプションは、再呼び出しの動作を制御します。 重複するコマンドは、履歴ファイルにまだ追加されています。
このオプションを設定すると、コマンドの呼び出し時に最新の呼び出しのみが表示されます。 再呼び出し中に順序を維持するために、繰り返しコマンドが履歴に追加されます。 ただし、通常は、履歴をリコールまたは検索するときにコマンドを複数回表示したくはありません。

既定では、グローバル **PSConsoleReadLineOptions** オブジェクトの **履歴の no重複** プロパティはに設定され `True` ます。 この **Switchparameter** を使用して、プロパティ値をに設定し `True` ます。 プロパティ値を変更するには、次のように **Switchparameter** の値を指定する必要があります `-HistoryNoDuplicates:$False` 。

次のコマンドを使用して、プロパティ値を直接設定できます。

`(Get-PSReadLineOption).HistoryNoDuplicates = $False`

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

### -履歴の Savepath

履歴を保存するファイルへのパスを指定します。 Windows または Windows 以外のプラットフォームを実行しているコンピューターでは、別の場所にファイルが保存されます。 ファイル名は変数に格納され `$($host.Name)_history.txt` ます。たとえば、のように `ConsoleHost_history.txt` なります。

このパラメーターを使用しない場合、既定のパスは次のようになります。

**Windows**

`$env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine\$($host.Name)_history.txt`

**Windows 以外**

`$env:XDG_DATA_HOME/powershell/PSReadLine\$($host.Name)_history.txt`

`$env:HOME/.local/share/powershell/PSReadLine\$($host.Name)_history.txt`

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: A file named $($host.Name)_history.txt in $env:APPDATA\Microsoft\Windows\PowerShell\PSReadLine on Windows and $env:XDG_DATA_HOME/powershell/PSReadLine or $env:HOME/.local/share/powershell/PSReadLine on non-Windows platforms
Accept pipeline input: False
Accept wildcard characters: False
```

### -履歴の Savestyle

**Psreadline** が履歴を保存する方法を指定します。

有効な値は次のとおりです。

- **Saveincrementally**: 各コマンドが実行された後に履歴を保存し、PowerShell の複数のインスタンス間で共有します。
- **Saveatexit**: PowerShell の終了時に履歴ファイルを追加します。
- **Savは**、履歴ファイルを使用しないようにします。

```yaml
Type: Microsoft.PowerShell.HistorySaveStyle
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: SaveIncrementally
Accept pipeline input: False
Accept wildcard characters: False
```

### -HistorySearchCaseSensitive

**ReverseSearchHistory** や **history search後方** などの関数で、履歴検索で大文字と小文字を区別することを指定します。

既定では、global **PSConsoleReadLineOptions** オブジェクトの **HistorySearchCaseSensitive** プロパティはに設定されて `False` います。 この **Switchparameter** を使用して、プロパティ値をに設定し `True` ます。 プロパティの値を変更するには、次のように **Switchparameter** の値を指定する必要があります `-HistorySearchCaseSensitive:$False` 。

次のコマンドを使用して、プロパティ値を直接設定できます。

`(Get-PSReadLineOption).HistorySearchCaseSensitive = $False`

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

### -HistorySearchCursorMovesToEnd

は、検索を使用して、履歴から読み込まれたコマンドの最後までカーソルが移動することを示します。
このパラメーターをに設定すると `$False` 、上矢印または下矢印を押したときの位置にカーソルが置かれたままになります。

既定では、global **PSConsoleReadLineOptions** オブジェクトの **HistorySearchCursorMovesToEnd** プロパティはに設定されて `False` います。 この **Switchparameter** を使用して、プロパティ値をに設定し `True` ます。 プロパティの値を変更するには、次のように **Switchparameter** の値を指定する必要があります `-HistorySearchCursorMovesToEnd:$False` 。

次のコマンドを使用して、プロパティ値を直接設定できます。

`(Get-PSReadLineOption).HistorySearchCursorMovesToEnd = $False`

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

### -Maximumhistory Count

**Psreadline** 履歴に保存するコマンドの最大数を指定します。

**Psreadline** 履歴は、PowerShell 履歴とは別のものです。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumKillRingCount

Kill ring に格納される項目の最大数を指定します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 10
Accept pipeline input: False
Accept wildcard characters: False
```

### -PromptText

解析エラーが発生した場合、 **Psreadline** はプロンプトの一部を赤で変更します。 **Psreadline** は、プロンプトの一部の色だけを変更する方法を決定するために、prompt 関数を分析します。 この分析は100% の信頼度ではありません。

**Psreadline** が予期しない方法でプロンプトを変更する場合は、このオプションを使用します。 末尾の空白を含めます。

たとえば、プロンプト関数が次の例のようになります。

`function prompt { Write-Host -NoNewLine -ForegroundColor Yellow "$pwd"; return "# " }`

その後、次のように設定します。

`Set-PSReadLineOption -PromptText "# "`

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: >
Accept pipeline input: False
Accept wildcard characters: False
```

### -ShowToolTips ヒント

候補の候補を表示すると、入力候補の一覧にツールヒントが表示されます。

既定では、このオプションは有効になっています。 以前のバージョンの **Psreadline** では、このオプションは既定では有効になっていません。 無効にするには、このオプションをに設定 `$False` します。

既定では、グローバル **PSConsoleReadLineOptions** オブジェクトの **showtooltips ヒント** プロパティはに設定されてい `True` ます。 この **Switchparameter** を使用して、プロパティ値をに設定し `True` ます。 プロパティ値を変更するには、次のように **Switchparameter** の値を指定する必要があります `-ShowToolTips:$False` 。

次のコマンドを使用して、プロパティ値を直接設定できます。

`(Get-PSReadLineOption).ShowToolTips = $False`

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

### -ViModeChangeHandler

**Vimodeindicator** がに設定されている場合、指定されたスクリプトブロックは、モードが変更されるたびに `Script` 呼び出されます。 スクリプトブロックには、型の引数が1つ用意されてい `ViMode` ます。

このパラメーターは、PowerShell 7 で導入されました。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ViModeIndicator

このオプションは、現在の **Vi** モードの視覚的な表示を設定します。 挿入モードまたはコマンドモード。

有効な値は次のとおりです。

- **None**: 何も表示されません。
- **プロンプト**: プロンプトの色が変更されます。
- **カーソル**: カーソルのサイズが変更されます。
- **スクリプト**: ユーザーが指定したテキストが印刷されます。

```yaml
Type: Microsoft.PowerShell.ViModeStyle
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WordDelimiters

**Forwardword** や、入力 **候補** などの関数の単語を区切る文字を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: ;:,.[]{}()/\|^&*-=+'"---
Accept pipeline input: False
Accept wildcard characters: False
```

### -PredictionSource

予測候補を取得するための PSReadLine のソースを指定します。

有効な値は次のとおりです。

- **None** -予測 IntelliSense 機能を無効にします (既定)。
-`**履歴** -予測 IntelliSense 機能を有効にし、唯一のソースとして psreadline 履歴を使用します。
- **プラグイン** -予測 IntelliSense 機能を有効にし、唯一の `CommandPrediction` ソースとしてプラグイン () を使用します。 この値は PSReadLine 2.2.0 で追加されました
- 履歴と **プラグイン**-予測 IntelliSense 機能を有効にし、ソースとして履歴とプラグインの両方を使用します。 この値は PSReadLine 2.2.0 で追加されました

```yaml
Type: Microsoft.PowerShell.PredictionSource
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PredictionViewStyle

予測テキストの表示スタイルを設定します。 既定値は **InlineView** です。

- **InlineView** -魚と zsh のような、今日の既存のスタイルです。 (既定値)。
- **ListView** -提案はドロップダウンリストに表示され、ユーザーは <kbd>Uparrow</kbd> と <kbd>downarrow</kbd>を使用して選択できます。

このパラメーターは PSReadLine 2.2.0 で追加されました。

```yaml
Type: Microsoft.PowerShell.PredictionViewStyle
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: InlineView
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](http://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してオブジェクトを `Set-PSReadLineOption.`

## 出力

### なし

このコマンドレットは出力を生成しません。

## メモ

## 関連リンク

[about_PSReadLine](./About/about_PSReadLine.md)

[Get-PSReadLineKeyHandler](Get-PSReadLineKeyHandler.md)

[Get-PSReadLineOption](Get-PSReadLineOption.md)

[-PSReadLineKeyHandler を削除します。](Remove-PSReadLineKeyHandler.md)

[設定-PSReadLineKeyHandler](Set-PSReadLineKeyHandler.md)
