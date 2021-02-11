---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/wait-process?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Wait-Process
ms.openlocfilehash: 6fe942f98183a3b185adf5781699bf41d03db920
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94343363"
---
# Wait-Process

## 概要
プロセスが停止するまで、次の入力を受け取るのを待ちます。

## SYNTAX

### 名前 (既定値)

```
Wait-Process [-Name] <String[]> [[-Timeout] <Int32>] [<CommonParameters>]
```

### Id

```
Wait-Process [-Id] <Int32[]> [[-Timeout] <Int32>] [<CommonParameters>]
```

### InputObject

```
Wait-Process [[-Timeout] <Int32>] -InputObject <Process[]> [<CommonParameters>]
```

## Description

`Wait-Process`コマンドレットは、実行中の1つ以上のプロセスが、入力を受け入れる前に停止するのを待機します。 PowerShell コンソールでは、このコマンドレットにより、プロセスが停止するまでコマンドプロンプトが表示されなくなります。 プロセスは、プロセス名またはプロセス ID (PID) で指定することも、パイプを使用してに処理することもでき `Wait-Process` ます。

`Wait-Process` ローカルコンピューター上で実行されているプロセスでのみ機能します。

## 例

### 例 1: プロセスを停止して待機する

```
PS C:\> $nid = (Get-Process notepad).id
PS C:\> Stop-Process -Id $nid
PS C:\> Wait-Process -Id $nid
```

この例では、メモ帳のプロセスを停止し、プロセスが停止するまで待機してから、次のコマンドに進みます。

最初のコマンドは、 `Get-Process` コマンドレットを使用して、メモ帳のプロセスの ID を取得します。 ID は変数に格納され `$nid` ます。

2番目のコマンドは、コマンドレットを使用して、 `Stop-Process` に格納されている ID を使用してプロセスを停止し `$nid` ます。

3番目のコマンドは、を使用して `Wait-Process` 、Notepad プロセスが停止するまで待機します。 の **Id** パラメーターを使用して `Wait-Process` プロセスを識別します。

### 例 2: プロセスを指定する

```
PS C:\> $p = Get-Process notepad
PS C:\> Wait-Process -Id $p.id
PS C:\> Wait-Process -Name "notepad"
PS C:\> Wait-Process -InputObject $p
```

これらのコマンドは、にプロセスを指定する3つの異なる方法を示して `Wait-Process` います。 最初のコマンドは、メモ帳のプロセスを取得し、変数に格納し `$p` ます。

2番目のコマンドは **Id** パラメーターを使用し、3番目のコマンドは **Name** パラメーターを使用し、4番目のコマンドは **InputObject** パラメーターを使用します。

これらのコマンドの結果はすべて同じであり、置き換えて使用することもできます。

### 例 3: 指定した時間だけプロセスを待機する

```
PS C:\> Wait-Process -Name outlook, winword -Timeout 30
```

このコマンドは、Outlook と Winword の各プロセスが停止するまで 30 秒間待ちます。 プロセスがいずれも停止しない場合、コマンドレットによって未終了エラーが表示され、コマンド プロンプトが表示されます。

## PARAMETERS

### -Id

プロセスのプロセス ID を指定します。 複数の ID を指定するには、ID をコンマで区切ります。
プロセスの PID を検索するには、「」と入力 `Get-Process` します。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases: PID, ProcessId

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InputObject

プロセス オブジェクトを送信して、プロセスを指定します。 プロセスオブジェクトが格納されている変数を入力するか、コマンドレットなどのプロセスオブジェクトを取得するコマンドまたは式を入力し `Get-Process` ます。

```yaml
Type: System.Diagnostics.Process[]
Parameter Sets: InputObject
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

プロセスのプロセス名を指定します。 複数の名前を指定するには、名前をコンマで区切ります。 ワイルドカード文字はサポートされていません。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases: ProcessName

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -タイムアウト

このコマンドレットが、指定されたプロセスを停止するまで待機する最大時間を秒単位で指定します。
この時間が経過すると、まだ実行中のプロセスの一覧を示す未終了エラーが表示され、待機動作が終了します。 既定では、タイムアウトはありません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: TimeoutSec

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Diagnostics.Process

パイプを使用してプロセスオブジェクトをこのコマンドレットに渡します。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

このコマンドレットは、 **system.servicemodel クラスの** **waitforexit** メソッドを使用します。

## 関連リンク

[Debug-Process](Debug-Process.md)

[Get-Process](Get-Process.md)

[Start-Process](Start-Process.md)

[Stop-Process](Stop-Process.md)

[Wait-Process](Wait-Process.md)
