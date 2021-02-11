---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/09/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/trace-command?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Trace-Command
ms.openlocfilehash: afc08b263d75f8a728ce6d64cc7ede0a639df196
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601410"
---
# Trace-Command

## 概要
指定された式またはコマンドのトレースを構成し、開始します。

## SYNTAX

### 式セット (既定値)

```
Trace-Command [-InputObject <PSObject>] [-Name] <String[]> [[-Option] <PSTraceSourceOptions>]
 [-Expression] <ScriptBlock> [-ListenerOption <TraceOptions>] [-FilePath <String>] [-Force] [-Debugger]
 [-PSHost] [<CommonParameters>]
```

### commandSet

```
Trace-Command [-InputObject <PSObject>] [-Name] <String[]> [[-Option] <PSTraceSourceOptions>]
 [-Command] <String> [-ArgumentList <Object[]>] [-ListenerOption <TraceOptions>] [-FilePath <String>] [-Force]
 [-Debugger] [-PSHost] [<CommonParameters>]
```

## Description
`Trace-Command`コマンドレットは、指定された式またはコマンドのトレースを構成し、開始します。
Set-TraceSource と同様に機能しますが、指定されたコマンドにのみ適用される点が異なります。

## 例

### 例 1: メタデータの処理、パラメーターのバインド、および式をトレースする

この例では、メタデータの処理、パラメーターのバインド、およびコマンドレットの作成と式の破棄のトレースを開始し `Get-Process Notepad` ます。

```powershell
Trace-Command -Name metadata,parameterbinding,cmdlet -Expression {Get-Process Notepad} -PSHost
```

**Name** パラメーターを使用してトレースソースを指定し、 **Expression** パラメーターでコマンドを指定し、 **PSHost** パラメーターを使用して出力をコンソールに送信します。 トレースオプションまたはリスナーオプションは指定されていないため、コマンドは既定値を使用します。

- トレースオプションのすべて
- リスナーオプションの場合は None

### 例 2: ParameterBinding 操作のアクションをトレースする

この例では、パイプラインからの入力を受け取る式を処理するときに、PowerShell の **Parameterbinding** 操作のアクションをトレース `Get-Alias` します。

```powershell
$A = "i*"
Trace-Command ParameterBinding {Get-Alias $Input} -PSHost -InputObject $A
```

では `Trace-Command` 、 **InputObject** パラメーターは、トレース中に処理される式にオブジェクトを渡します。

最初のコマンドは、変数に文字列を格納し `i*` `$A` ます。 2番目のコマンドは、 `Trace-Command` ParameterBinding トレースソースと共にコマンドレットを使用します。 **PSHost** パラメーターを指定すると、出力がコンソールに送信されます。

処理される式はです `Get-Alias $Input` 。ここで、 `$Input` 変数は **InputObject** パラメーターに関連付けられています。 **InputObject** パラメーターは、変数 `$A` を式に渡します。 実際には、トレース中に処理されるコマンドは `Get-Alias -InputObject $A" or "$A | Get-Alias` です。

## PARAMETERS

### -ArgumentList

トレースするコマンドのパラメーターとパラメーターの値を指定します。 **ArgumentList** のエイリアスは、**Args** です。 この機能は、動的パラメーターをデバッグする場合に特に便利です。

**ArgumentList** の動作の詳細については、「 [about_Splatting](../Microsoft.PowerShell.Core/About/about_Splatting.md#splatting-with-arrays)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: commandSet
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Command

トレース中に処理するコマンドを指定します。

```yaml
Type: System.String
Parameter Sets: commandSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -デバッガー

コマンドレットがトレース出力をデバッガーに送信することを示します。 出力はユーザー モードまたはカーネル モードのデバッガーや、Visual Studio で表示することができます。 このパラメーターは、既定のトレース リスナーも選択します。

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

### -式

トレース中に処理する式を指定します。 式を中かっこ () で囲み `{}` ます。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: expressionSet
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

コマンドレットがトレース出力を送信するファイルを指定します。 このパラメーターは、ファイル トレース リスナーも選択します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: PSPath, Path

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。 **FilePath** パラメーターと共に使用します。 **Force** パラメーターを使用しても、コマンドレットはセキュリティ制限を上書きできません。

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

### -InputObject

トレース中に処理される式への入力を指定します。 式が受け取る入力を表した変数を入力するか、パイプラインを通じてオブジェクトを渡すことができます。

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

### -ListenerOption

出力の各トレースメッセージのプレフィックスにオプションのデータを指定します。 このパラメーターの有効値は、次のとおりです。

- なし
- LogicalOperationStack
- DateTime
- Timestamp
- ProcessId
- スレッド Id
- 呼び出し履歴

**[なし]** が既定値です。

複数のオプションを指定するには、スペースなしで、コンマで区切り、"ProcessID,ThreadID" のように引用符で囲みます。

```yaml
Type: System.Diagnostics.TraceOptions
Parameter Sets: (All)
Aliases:
Accepted values: None, LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

トレースされる PowerShell コンポーネントの配列を指定します。 各コンポーネントのトレース ソースの名前を入力します。 ワイルドカードを使用できます。 コンピューター上のトレースソースを検索するには、「」と入力 `Get-TraceSource` します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Option

トレース対象のイベントの種類を決定します。 このパラメーターの有効値は、次のとおりです。

- なし
- コンストラクター
- Dispose
- ファイナライザー
- メソッド
- プロパティ
- デリゲート
- イベント
- 例外
- ロック
- エラー
- エラー
- 警告
- "詳細"
- WriteLine
- データ
- Scope
- ExecutionFlow
- Assert
- すべて

ALL が既定値です。

次の値はその他の値の組み合わせです。

- ExecutionFlow: (コンストラクター、Dispose、ファイナライザー、メソッド、デリゲート、イベント、およびスコープ)
- データ: (コンストラクター、Dispose、ファイナライザー、プロパティ、Verbose、および WriteLine)
- エラー: (エラーと例外)。

複数のオプションを指定するには、スペースなしで、コンマで区切り、"Constructor,Dispose" のように引用符で囲みます。

```yaml
Type: System.Management.Automation.PSTraceSourceOptions
Parameter Sets: (All)
Aliases:
Accepted values: None, Constructor, Dispose, Finalizer, Method, Property, Delegates, Events, Exception, Lock, Error, Errors, Warning, Verbose, WriteLine, Data, Scope, ExecutionFlow, Assert, All

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PSHost

コマンドレットがトレース出力を PowerShell ホストに送信することを示します。 このパラメーターは、PSHost トレース リスナーも選択します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して、入力を表すオブジェクトをに渡し `Trace-Command` ます。

## 出力

### システム管理. PSObject

デバッグ ストリーム内にコマンド トレースを返します。

## 注

- トレースは、開発者がプログラムをデバッグし、調整するために使用するメソッドです。 トレース時に、プログラムは、内部処理の各手順について詳細なメッセージを生成します。

- PowerShell トレースコマンドレットは、PowerShell 開発者を支援するように設計されていますが、すべてのユーザーが使用できます。 シェルの機能のほとんどすべての側面を監視します。

- トレースが有効になっている PowerShell コンポーネントを検索するには、「」と入力 `Get-Help Get-TraceSource` します。

  トレースソースは、トレースを管理し、コンポーネントのトレースメッセージを生成する各 PowerShell コンポーネントの一部です。 コンポーネントをトレースするには、トレース ソースを特定します。

  トレースリスナーは、トレースの出力を受信し、ユーザーに表示します。 トレースデータは、ユーザーモードまたはカーネルモードのデバッガー、ホストまたはコンソール、ファイル、または **TraceListener** クラスから派生したカスタムリスナーに送信することを選択できます。

- CommandSet パラメーターセットを使用すると、PowerShell はパイプラインで処理されるのと同じようにコマンドを処理します。 たとえば、コマンドの検出は、受信した各オブジェクトごとに繰り返されません。

- **Name**、 **Expression**、 **Option**、および **Command** パラメーターの名前は省略可能です。 パラメーター名を省略した場合、 **名前**、 **式**、 **オプション** 、または **名前**、 **コマンド**、 **オプション** の順に名前のないパラメーター値が表示されます。 パラメーター名を指定する場合は、パラメーターの順序に決まりはありません。

## 関連リンク

[Get-TraceSource](Get-TraceSource.md)

[Measure-Command](Measure-Command.md)

[Set-TraceSource](Set-TraceSource.md)

