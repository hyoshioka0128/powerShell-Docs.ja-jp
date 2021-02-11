---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/disable-psbreakpoint?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disable-PSBreakpoint
ms.openlocfilehash: 2bd1a48d2075950cdb337063a100bf31534ac6fe
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602627"
---
# Disable-PSBreakpoint

## 概要
現在のコンソール内のブレークポイントを無効にします。

## SYNTAX

### ブレークポイント (既定値)

```
Disable-PSBreakpoint [-PassThru] [-Breakpoint] <Breakpoint[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Id

```
Disable-PSBreakpoint [-PassThru] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

**-PSBreakpoint** ポイントコマンドレットは、ブレークポイントを無効にします。これにより、スクリプトの実行時にヒットしないことが保証されます。
これを使用してすべてのブレークポイントを無効にするか、ブレークポイント オブジェクト ID またはブレークポイント ID を送信してブレークポイントを指定することができます。

技術的には、このコマンドレットを使用して、ブレークポイントのオブジェクトの Enabled プロパティの値を False に変更します。
ブレークポイントを再び有効にするには、Enable-PSBreakpoint コマンドレットを使用します。
ブレークポイントは、Set-PSBreakpoint コマンドレットを使用して作成したときに既定で有効になっています。

ブレークポイントは、スクリプト内の指示を調べることができるように、実行を一時的に停止するスクリプト内のポイントです。
**Disable-PSBreakpoint ポイントは、** PowerShell スクリプトのデバッグ用に設計されたいくつかのコマンドレットの1つです。
PowerShell デバッガーの詳細については、「about_Debuggers」を参照してください。

## 例

### 例 1: ブレークポイントを設定して無効にする

```
PS C:\> $B = Set-PSBreakpoint -Script "sample.ps1" -Variable "name"
PS C:\> $B | Disable-PSBreakpoint
```

これらのコマンドは、新しく作成したブレークポイントを無効にします。

最初のコマンドは、Set-PSBreakpoint コマンドレットを使用して、Sample.ps1 スクリプト内の *Name* 変数にブレークポイントを作成します。
次に、ブレークポイントオブジェクトを $B 変数に保存します。

2番目のコマンドは、 **disable-PSBreakpoint ポイント** コマンドレットを使用して、新しいブレークポイントを無効にします。
パイプライン演算子 (|) を使用して、$B 内のブレークポイントオブジェクトを **無効-PSBreakpoint ポイント** コマンドレットに送信します。

このコマンドの結果として、$B 内のブレークポイントオブジェクトの Enabled プロパティの値は False になります。

### 例 2: ブレークポイントを無効にする

```
PS C:\> Disable-PSBreakpoint -Id 0
```

このコマンドは、ブレークポイント ID が 0 のブレークポイントを無効にします。

### 例 3: 無効になっているブレークポイントを作成する

```
PS C:\> Disable-PSBreakpoint -Breakpoint ($B = Set-PSBreakpoint -Script "sample.ps1" -Line 5)
PS C:\> $B
```

このコマンドは、有効に設定するまで無効である新しいブレークポイントを作成します。

このコマンドは、 **disable-PSBreakpoint** ポイントコマンドレットを使用して、ブレークポイントを無効にします。
*ブレークポイント* パラメーターの値は、新しいブレークポイントを設定し、ブレークポイントオブジェクトを生成し、そのオブジェクトを $B 変数に保存する Set-PSBreakpoint コマンドです。

オブジェクトを値として受け取るコマンドレットのパラメーターは、オブジェクトを含む変数や、オブジェクトを取得または生成するコマンドを受け取ることができます。
この場合、 **設定-PSBreakpoint** ポイントはブレークポイントオブジェクトを生成するため、 *ブレーク* ポイントパラメーターの値として使用できます。

2番目のコマンドは、$B 変数の値にブレークポイントオブジェクトを表示します。

### 例 4: 現在のコンソールのすべてのブレークポイントを無効にする

```
PS C:\> Get-PSBreakpoint | Disable-PSBreakpoint
```

このコマンドは、現在のコンソールのすべてのブレークポイントを無効にします。
次のように、このコマンドを省略することができます。 "gbp |.dbp "。

## PARAMETERS

### -ブレークポイント

無効にするブレークポイントを指定します。
ブレークポイント オブジェクトを含んでいる変数または Get-PSBreakpoint コマンドのようなブレークポイント オブジェクトを取得するコマンドを入力します。
また、パイプを使用してブレークポイントオブジェクトを **無効** にすることもできます。

```yaml
Type: System.Management.Automation.Breakpoint[]
Parameter Sets: Breakpoint
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Id

指定したブレークポイント ID を持つブレークポイントを無効にします。
ID または ID が含まれる変数を入力します
パイプを使用して Id を無効にすることはできません **-PSBreakpoint ポイント**。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

有効になっているブレークポイントを表すオブジェクトが返されます。
既定では、このコマンドレットによる出力はありません。

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

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム.... ブレークポイント

パイプを使用して、ブレークポイントオブジェクトを無効にし、 **-PSBreakpoint ポイントを無効** にすることができます。

## 出力

### None または system.string。

*PassThru* パラメーターを使用すると、**無効** にしたブレークポイントを表すオブジェクトが返されます。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

## 関連リンク

[Enable-PSBreakpoint](Enable-PSBreakpoint.md)

[Get-PSBreakpoint](Get-PSBreakpoint.md)

[Get-PSCallStack](Get-PSCallStack.md)

[Remove-PSBreakpoint](Remove-PSBreakpoint.md)

[Set-PSBreakpoint](Set-PSBreakpoint.md)

