---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 5/20/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/register-argumentcompleter?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Register-ArgumentCompleter
ms.openlocfilehash: 5dc681e39a6d4991c4137d106879df8778d3c1fe
ms.sourcegitcommit: 9a86cac80402d8193147058d4ba50e07b26059dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97490637"
---
# Register-ArgumentCompleter

## 概要

カスタム引数 completer を登録します。

## SYNTAX

### このセット

```
Register-ArgumentCompleter -CommandName <String[]> -ScriptBlock <ScriptBlock> [-Native]
 [<CommonParameters>]
```

### PowerShellSet

```
Register-ArgumentCompleter [-CommandName <String[]>] -ParameterName <String>
 -ScriptBlock <ScriptBlock> [<CommonParameters>]
```

## Description

コマンドレットにより、 `Register-ArgumentCompleter` カスタム引数 completer が登録されます。 引数 completer を使用すると、指定した任意のコマンドに対して、実行時に動的にタブ補完を行うことができます。

## 例

### 例 1: カスタム引数 completer を登録する

次の例では、コマンドレットの **Id** パラメーターに completer 引数を登録し `Set-TimeZone` ます。

```powershell
$scriptBlock = {
    param($commandName, $parameterName, $wordToComplete, $commandAst, $fakeBoundParameters)

    (Get-TimeZone -ListAvailable).Id | Where-Object {
        $_ -like "$wordToComplete*"
    } | ForEach-Object {
          "'$_'"
    }
}
Register-ArgumentCompleter -CommandName Set-TimeZone -ParameterName Id -ScriptBlock $scriptBlock
```

最初のコマンドは、ユーザーが <kbd>Tab キー</kbd>を押したときに渡される必須パラメーターを受け取るスクリプトブロックを作成します。詳細については、 **ScriptBlock** パラメーターの説明を参照してください。

スクリプトブロック内では、 **Id** に使用できる値はコマンドレットを使用して取得され `Get-TimeZone` ます。 各タイムゾーンの **Id** プロパティは、パイプを使用してコマンドレットに渡され `Where-Object` ます。 `Where-Object`コマンドレットは、によって指定された値で始まらない id をフィルターで除外します `$wordToComplete` 。これは、 <kbd>Tab キー</kbd>を押す前にユーザーが入力したテキストを表します。フィルター選択された id は、 `ForEach-Object` 値にスペースが含まれている場合に、各値が引用符で囲まれたコマンドレットにパイプされます。

2番目のコマンドは、scriptblock、 **ParameterName** **Id** 、および **CommandName** を渡して、引数 completer を登録し `Set-TimeZone` ます。

### 例 2: タブ補完の値に詳細を追加する

次の例では、コマンドレットの **Name** パラメーターのタブ補完を上書き `Stop-Service` し、実行中のサービスだけを返します。

```powershell
$s = {
    param($commandName, $parameterName, $wordToComplete, $commandAst, $fakeBoundParameters)
    $services = Get-Service | Where-Object {$_.Status -eq "Running" -and $_.Name -like "$wordToComplete*"}
    $services | ForEach-Object {
        New-Object -Type System.Management.Automation.CompletionResult -ArgumentList $_.Name,
            $_.Name,
            "ParameterValue",
            $_.Name
    }
}
Register-ArgumentCompleter -CommandName Stop-Service -ParameterName Name -ScriptBlock $s
```

最初のコマンドは、ユーザーが <kbd>Tab キー</kbd>を押したときに渡される必須パラメーターを受け取るスクリプトブロックを作成します。詳細については、 **ScriptBlock** パラメーターの説明を参照してください。

スクリプトブロック内では、最初のコマンドは、コマンドレットを使用して実行中のすべてのサービスを取得し `Where-Object` ます。 サービスは、パイプを介してコマンドレットに渡され `ForEach-Object` ます。 `ForEach-Object`このコマンドレットは、新しい system.servicemodel オブジェクトを作成し、現在のサービスの名前 (パイプライン変数によって表されます) を設定し[ます。](/dotnet/api/system.management.automation.completionresult) `$_.Name`

入力 **可能オブジェクトを使用すると、** 返される各値に追加の詳細を指定できます。

- 入力候補 **テキスト**(String)-オートコンプリートの結果として使用されるテキスト。 これは、コマンドに送信される値です。
- **listitemtext** (String)-ユーザーが <kbd>Ctrl</kbd> + <kbd>Space</kbd>キーを押したときなど、一覧に表示されるテキスト。 これは表示のみに使用され、選択された場合はコマンドに渡されません。
- **resultType** ([入力](/dotnet/api/system.management.automation.completionresulttype)候補)-完了結果の型。
- **tooltip** (String)-オブジェクトについての詳細情報が表示されたツールヒントのテキスト。
  これは、ユーザーが<kbd>Ctrl</kbd>Space キーを押した後に項目を選択したときに表示され + <kbd></kbd>ます。

最後のコマンドは、停止したサービスをコマンドレットに手動で渡すことができることを示して `Stop-Service` います。 影響を受けるのは、タブ補完だけです。

### 例 3: カスタムネイティブ引数 completer を登録する

**ネイティブパラメーターを** 使用すると、ネイティブコマンドのタブ補完を行うことができます。 次の例では、 `dotnet` コマンドラインインターフェイス (CLI) のタブ補完を追加します。

> [!NOTE]
> `dotnet complete`コマンドは、バージョン2.0 以降の dotnet cli でのみ使用できます。

```powershell
$scriptblock = {
    param($wordToComplete, $commandAst, $cursorPosition)
        dotnet complete --position $cursorPosition $commandAst.ToString() | ForEach-Object {
            [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
        }
}
Register-ArgumentCompleter -Native -CommandName dotnet -ScriptBlock $scriptblock
```

最初のコマンドは、ユーザーが <kbd>Tab キー</kbd>を押したときに渡される必須パラメーターを受け取るスクリプトブロックを作成します。詳細については、 **ScriptBlock** パラメーターの説明を参照してください。

スクリプトブロック内では、 `dotnet complete` タブ補完を実行するためにコマンドが使用されます。
結果はパイプを使用してコマンドレットに渡されます。この `ForEach-Object` コマンドレット [は、](/dotnet/api/system.management.automation.completionresult)値ごとに新しい "**補完結果**" オブジェクトを作成するために、このクラスの **新しい** 静的メソッドを使用します。

## PARAMETERS

### -CommandName

コマンドの名前を配列として指定します。

```yaml
Type: System.String[]
Parameter Sets: NativeSet, PowerShellSet
Aliases:

Required: True (NativeSet), False (PowerShellSet)
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ネイティブ

引数 completer が、PowerShell がパラメーター名を完了できないネイティブコマンド用であることを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: NativeSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ParameterName

引数を完了するパラメーターの名前を指定します。 指定されたパラメーター名に、コマンドレットの **ForegroundColor** パラメーターなどの列挙値を指定することはできません `Write-Host` 。

列挙型の詳細については、「 [about_Enum](./About/about_Enum.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: PowerShellSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptBlock

タブ補完を実行するために実行するコマンドを指定します。 指定したスクリプトブロックは、入力を完了する値を返す必要があります。 スクリプトブロックは、パイプライン ( `ForEach-Object` 、など `Where-Object` )、または別の適切なメソッドを使用して、値のロールアウトを行う必要があります。 値の配列を返すと、PowerShell は配列全体を **1 つ** のタブ補完値として扱います。

スクリプトブロックでは、以下のパラメーターを以下の順序で指定する必要があります。 PowerShell では位置によって値が渡されるため、パラメーターの名前は重要ではありません。

- `$commandName` (位置 0)-このパラメーターには、スクリプトブロックがタブ補完を提供するコマンドの名前を設定します。
- `$parameterName` (位置 1)-このパラメーターは、値にタブ補完が必要なパラメーターに設定されています。
- `$wordToComplete` (位置 2)-このパラメーターは、 <kbd>Tab キー</kbd>を押す前にユーザーが指定した値に設定されます。スクリプトブロックは、この値を使用してタブ補完の値を決定する必要があります。
- `$commandAst` (位置 3)-このパラメーターは、現在の入力行の抽象構文ツリー (AST) に設定されます。 詳細については、「 [Ast クラス](/dotnet/api/system.management.automation.language.ast)」を参照してください。
- `$fakeBoundParameters` (位置 4)-このパラメーターは、 `$PSBoundParameters` ユーザーが <kbd>Tab キー</kbd>を押した前に、コマンドレットのを含むハッシュテーブルに設定されます。詳細については、「 [about_Automatic_Variables](./About/about_Automatic_Variables.md)」を参照してください。

**ネイティブ** パラメーターを指定する場合、スクリプトブロックは、指定された順序で次のパラメーターを受け取る必要があります。 PowerShell では位置によって値が渡されるため、パラメーターの名前は重要ではありません。

- `$wordToComplete` (位置 0)-このパラメーターは、 <kbd>Tab キー</kbd>を押す前にユーザーが指定した値に設定されます。スクリプトブロックは、この値を使用してタブ補完の値を決定する必要があります。
- `$commandAst` (Position 1)-このパラメーターは、現在の入力行の抽象構文ツリー (AST) に設定されます。 詳細については、「 [Ast クラス](/dotnet/api/system.management.automation.language.ast)」を参照してください。
- `$cursorPosition` (位置 2)-このパラメーターは、ユーザーが <kbd>Tab キー</kbd>を押したときのカーソルの位置に設定されます。

パラメーター属性として **ArgumentCompleter** を指定することもできます。 詳細については、「 [about_Functions_Advanced_Parameters](./About/about_Functions_Advanced_Parameters.md)」を参照してください。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](./About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### なし

このコマンドレットは、出力を返しません。

## 注

## 関連リンク

