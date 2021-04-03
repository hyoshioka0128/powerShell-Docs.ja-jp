---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/01/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/set-tracesource?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-TraceSource
ms.openlocfilehash: 42fd191f8f4b0bc4060f290cf906c71e9000a97c
ms.sourcegitcommit: 5b48fe7b2593581b7d4f7dd7c22206d8a45bb8af
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2021
ms.locfileid: "106184428"
---
# Set-TraceSource

## 構文
PowerShell コンポーネントのトレースを構成、開始、および停止します。

## 構文

### オプションセット (既定)

```
Set-TraceSource [-Name] <String[]> [[-Option] <PSTraceSourceOptions>] [-ListenerOption <TraceOptions>]
 [-FilePath <String>] [-Force] [-Debugger] [-PSHost] [-PassThru] [<CommonParameters>]
```

### removeAllListenersSet

```
Set-TraceSource [-Name] <String[]> [-RemoveListener <String[]>] [<CommonParameters>]
```

### Removefilelisten/Set

```
Set-TraceSource [-Name] <String[]> [-RemoveFileListener <String[]>] [<CommonParameters>]
```

## 説明

`Set-TraceSource`コマンドレットは、PowerShell コンポーネントのトレースを構成、開始、および停止します。 これを使用して、トレースするコンポーネントと、トレース出力の送信先を指定できます。

## 例

### 例 1: ParameterBinding コンポーネントをトレースする

```
Set-TraceSource -Name "ParameterBinding" -Option ExecutionFlow -PSHost -ListenerOption "ProcessId,TimeStamp"
```

このコマンドは、PowerShell の ParameterBinding コンポーネントのトレースを開始します。 ここでは、 **Name** パラメーターを使用してトレースソースを指定し、 **Option** パラメーターを使用してトレースイベントを選択し、PSHost パラメーターを使用して、 `ExecutionFlow` 出力をコンソールに送信する PowerShell ホストリスナーを選択します。  **ListenerOption** パラメーターは、 `ProcessID` トレースメッセージのプレフィックスにおよびの値を追加し `TimeStamp` ます。

### 例 2: トレースを停止する

```
Set-TraceSource -Name "ParameterBinding" -RemoveListener "Host"
```

このコマンドは、PowerShell の **Parameterbinding** コンポーネントのトレースを停止します。 この例では、 **Name** パラメーターを使用して、トレース対象のコンポーネントを識別し、 **removelistener** パラメーターを使用してトレースリスナーを識別します。

## パラメーター

### -デバッガー

コマンドレットがトレース出力をデバッガーに送信することを示します。 ユーザー モードまたはカーネル モードのデバッガー、あるいは Microsoft Visual Studio で出力を表示することができます。 このパラメーターは、既定のトレース リスナーも選択します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: optionsSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

このコマンドレットがトレース出力を送信するファイルを指定します。 このパラメーターは、ファイル トレース リスナーも選択します。 このパラメーターを使用してトレースを開始する場合は、 **Removefilelistener** パラメーターを使用してトレースを停止します。

```yaml
Type: System.String
Parameter Sets: optionsSet
Aliases: PSPath, Path

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

コマンドレットが読み取り専用ファイルを上書きすることを示します。 **FilePath** パラメーターと共に使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: optionsSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ListenerOption

出力の各トレースメッセージのプレフィックスにオプションのデータを指定します。 このパラメーターの有効値は、次のとおりです。

- `None`
- `LogicalOperationStack`
- `DateTime`
- `Timestamp`
- `ProcessId`
- `ThreadId`
- `Callstack`

`None` は既定値です。

これらの値はフラグベースの列挙体として定義されます。 このパラメーターを使用すると、複数の値を組み合わせて複数のフラグを設定できます。 値は、値の配列として **ListenerOption** パラメーターに渡すことも、それらの値のコンマ区切りの文字列として渡すこともできます。 コマンドレットでは、バイナリまたは演算を使用して値を結合します。 配列として値を渡すのが最も簡単なオプションであり、値に対してタブ補完を使用することもできます。

```yaml
Type: System.Diagnostics.TraceOptions
Parameter Sets: optionsSet
Aliases:
Accepted values: None, LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

トレースするコンポーネントを指定します。 各コンポーネントのトレース ソースの名前を入力します。
ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -Option

トレースされるイベントの種類を指定します。 このパラメーターの有効値は、次のとおりです。

- `None`
- `Constructor`
- `Dispose`
- `Finalizer`
- `Method`
- `Property`
- `Delegates`
- `Events`
- `Exception`
- `Lock`
- `Error`
- `Errors`
- `Warning`
- `Verbose`
- `WriteLine`
- `Data`
- `Scope`
- `ExecutionFlow`
- `Assert`
- `All`

`All` は既定値です。

次の値はその他の値の組み合わせです。

- `ExecutionFlow`: `Constructor`, `Dispose`, `Finalizer`, `Method`, `Delegates`, `Events`, `Scope`
- `Data`: `Constructor`, `Dispose`, `Finalizer`, `Property`, `Verbose`, `WriteLine`
- `Errors`: `Error`, `Exception`

これらの値はフラグベースの列挙体として定義されます。 このパラメーターを使用すると、複数の値を組み合わせて複数のフラグを設定できます。 値は、値の配列として **オプション** パラメーターに渡すことも、その値のコンマ区切りの文字列として渡すこともできます。 コマンドレットでは、バイナリまたは演算を使用して値を結合します。 配列として値を渡すのが最も簡単なオプションであり、値に対してタブ補完を使用することもできます。

```yaml
Type: System.Management.Automation.PSTraceSourceOptions
Parameter Sets: optionsSet
Aliases:
Accepted values: None, Constructor, Dispose, Finalizer, Method, Property, Delegates, Events, Exception, Lock, Error, Errors, Warning, Verbose, WriteLine, Data, Scope, ExecutionFlow, Assert, All

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: optionsSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PSHost

このコマンドレットが、トレース出力を PowerShell ホストに送信することを示します。 このパラメーターは、PSHost トレース リスナーも選択します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: optionsSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RemoveFileListener

指定したファイルに関連付けられているファイル トレース リスナーを削除することで、トレースを停止します。 トレース出力ファイルのパスとファイル名を入力します。

```yaml
Type: System.String[]
Parameter Sets: removeFileListenersSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RemoveListener

トレース リスナーを削除することで、トレースを停止します。

**Removelistener** には次の値を使用します。

- PSHost (コンソール) を削除するには、「」と入力 `Host` します。
- デバッガーを削除するには、「」と入力 `Debug` します。
- すべてのトレースリスナーを削除するには、「」と入力 `*` します。

ファイルトレースリスナーを削除するには、 **Removefilelistener** パラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: removeAllListenersSet
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

### System.String

パイプを使用して、名前を含む文字列をにパイプすることができ `Set-TraceSource` ます。

## 出力

### None または System.management.automation.pstracesource。

**PassThru** パラメーターを使用すると、に `Set-TraceSource` よってトレースセッションを表す **system.management.automation.pstracesource** オブジェクトが生成されます。 それ以外の場合、このコマンドレットによる出力はありません。

## Notes

- トレースは、開発者がプログラムをデバッグし、調整するために使用するメソッドです。 トレース時に、プログラムは、内部処理の各手順について詳細なメッセージを生成します。

  PowerShell トレースコマンドレットは、PowerShell 開発者を支援するように設計されていますが、すべてのユーザーが使用できます。 これらを使用すると、PowerShell の機能のほぼすべての側面を監視できます。

  トレースソースは、トレースを管理し、コンポーネントのトレースメッセージを生成する各 PowerShell コンポーネントの一部です。 コンポーネントをトレースするには、トレース ソースを特定します。

  トレースリスナーは、トレースの出力を受信し、ユーザーに表示します。 トレースデータは、ユーザーモードまたはカーネルモードのデバッガー、コンソール、ファイル、または **TraceListener** クラスから派生したカスタムリスナーに送信することを選択できます。

- トレースを開始するには、 **Name** パラメーターを使用してトレースソースを指定し、 **FilePath**、 **Debugger**、または **PSHost** パラメーターを使用してリスナー (出力先) を指定します。 **Options** パラメーターを使用して、トレースされるイベントの種類を決定し、 **ListenerOption** パラメーターを使用してトレース出力を構成します。
- トレースの構成を変更するには、 `Set-TraceSource` トレースを開始するときと同じように、コマンドを入力します。 PowerShell は、トレースソースが既にトレースされていることを認識します。 トレースを停止し、新しい構成を追加して、トレースを開始または再開します。
- トレースを停止するには、 **Removelistener** パラメーターを使用します。 ファイルリスナーを使用するトレース ( **FilePath** パラメーターを使用して開始されたトレース) を停止するには、 **removefilelistener** パラメーターを使用します。
  リスナーを削除すると、トレースは停止します。
- トレース可能なコンポーネントを判別するには、Get TraceSource を使用します。 各モジュールのトレースソースは、コンポーネントが使用されているときに自動的に読み込まれ、の出力に表示され `Get-TraceSource` ます。

## 関連リンク

[Get-TraceSource](Get-TraceSource.md)

[Trace-Command](Trace-Command.md)
