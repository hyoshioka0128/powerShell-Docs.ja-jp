---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/set-tracesource?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-TraceSource
ms.openlocfilehash: 6e7fd1c6eb3551906b4dd9d947b5e551cd05d30a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602027"
---
# Set-TraceSource

## 概要
PowerShell コンポーネントのトレースを構成、開始、および停止します。

## SYNTAX

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

## Description

**Set TraceSource** コマンドレットは、PowerShell コンポーネントのトレースを構成、開始、および停止します。
これを使用して、トレースするコンポーネントと、トレース出力の送信先を指定できます。

## 例

### 例 1: ParameterBinding コンポーネントをトレースする

```
PS C:\> Set-TraceSource -Name "ParameterBinding" -Option ExecutionFlow -PSHost -ListenerOption "ProcessId,TimeStamp"
```

このコマンドは、PowerShell の ParameterBinding コンポーネントのトレースを開始します。
*Name* パラメーターを使用してトレースソースを指定し、 *Option* パラメーターを使用して executionflow トレースイベントを選択します。また、 *PSHost* パラメーターを使用して、出力をコンソールに送信する PowerShell ホストリスナーを選択します。
*ListenerOption* パラメーターを指定すると、ProcessID と TimeStamp の値がトレースメッセージのプレフィックスに追加されます。

### 例 2: トレースを停止する

```
PS C:\> Set-TraceSource -Name "ParameterBinding" -RemoveListener "Host"
```

このコマンドは、PowerShell の ParameterBinding コンポーネントのトレースを停止します。
この例では、 *Name* パラメーターを使用して、トレース対象のコンポーネントを識別し、 *removelistener* パラメーターを使用してトレースリスナーを識別します。

## PARAMETERS

### -デバッガー

コマンドレットがトレース出力をデバッガーに送信することを示します。
ユーザー モードまたはカーネル モードのデバッガー、あるいは Microsoft Visual Studio で出力を表示することができます。
このパラメーターは、既定のトレース リスナーも選択します。

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

このコマンドレットがトレース出力を送信するファイルを指定します。
このパラメーターは、ファイル トレース リスナーも選択します。
このパラメーターを使用してトレースを開始する場合は、 *Removefilelistener* パラメーターを使用してトレースを停止します。

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

コマンドレットが読み取り専用ファイルを上書きすることを示します。
*FilePath* パラメーターと共に使用します。

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

出力の各トレースメッセージのプレフィックスにオプションのデータを指定します。
このパラメーターの有効値は、次のとおりです。

- なし
- LogicalOperationStack
- DateTime
- Timestamp
- ProcessId
- スレッド Id
- 呼び出し履歴

[なし] が既定値です。

複数のオプションを指定するには、スペースなしで、コンマで区切り、"ProcessID,ThreadID" のように引用符で囲みます。

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

トレースするコンポーネントを指定します。
各コンポーネントのトレース ソースの名前を入力します。
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

トレースされるイベントの種類を指定します。
このパラメーターの有効値は、次のとおりです。

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

作業中の項目を表すオブジェクトを返します。
既定では、このコマンドレットによる出力はありません。

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

ndicates は、このコマンドレットがトレース出力を PowerShell ホストに送信することを通知します。
このパラメーターは、PSHost トレース リスナーも選択します。

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

指定したファイルに関連付けられているファイル トレース リスナーを削除することで、トレースを停止します。
トレース出力ファイルのパスとファイル名を入力します。

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

*Removelistener* には次の値を使用します。

- PSHost (コンソール) を削除するには、「」と入力 `Host` します。
- デバッガーを削除するには、「」と入力 `Debug` します。
- すべてのトレースリスナーを削除するには、「」と入力 `*` します。

ファイルトレースリスナーを削除するには、 *Removefilelistener* パラメーターを使用します。

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

パイプを使用して、名前を含む文字列を **設定-TraceSource** にすることができます。

## 出力

### None または System.management.automation.pstracesource。

*PassThru* パラメーターを使用すると、 **system.management.automation.pstracesource** オブジェクトがトレースセッションを表すように **設定** されます。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

* トレースは、開発者がプログラムをデバッグし、調整するために使用するメソッドです。 トレース時に、プログラムは、内部処理の各手順について詳細なメッセージを生成します。

  PowerShell トレースコマンドレットは、PowerShell 開発者を支援するように設計されていますが、すべてのユーザーが使用できます。
これらを使用すると、PowerShell の機能のほぼすべての側面を監視できます。

  トレースソースは、トレースを管理し、コンポーネントのトレースメッセージを生成する各 PowerShell コンポーネントの一部です。
コンポーネントをトレースするには、トレース ソースを特定します。

  トレースリスナーは、トレースの出力を受信し、ユーザーに表示します。
トレースデータは、ユーザーモードまたはカーネルモードのデバッガー、コンソール、ファイル、または **TraceListener** クラスから派生したカスタムリスナーに送信することを選択できます。

* トレースを開始するには、 *Name* パラメーターを使用してトレースソースを指定し、 *FilePath*、 *Debugger*、または *PSHost* パラメーターを使用してリスナー (出力先) を指定します。 *Options* パラメーターを使用して、トレースされるイベントの種類を決定し、 *ListenerOption* パラメーターを使用してトレース出力を構成します。
* トレースの構成を変更するには、トレースを開始するときと同じように、 **Set TraceSource** コマンドを入力します。 PowerShell は、トレースソースが既にトレースされていることを認識します。 トレースを停止し、新しい構成を追加して、トレースを開始または再開します。
* トレースを停止するには、 *Removelistener* パラメーターを使用します。 ファイルリスナーを使用するトレース ( *FilePath* パラメーターを使用して開始されたトレース) を停止するには、 *removefilelistener* パラメーターを使用します。 リスナーを削除すると、トレースは停止します。
* トレース可能なコンポーネントを判別するには、Get TraceSource を使用します。 各モジュールのトレースソースは、コンポーネントが使用されているときに自動的に読み込まれ、 **Get TraceSource** の出力に表示されます。

## 関連リンク

[Get-TraceSource](Get-TraceSource.md)

[Trace-Command](Trace-Command.md)

