---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/09/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/enable-psbreakpoint?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Enable-PSBreakpoint
ms.openlocfilehash: 28cda7a2fa4435092b647e43a250222a852114b0
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602195"
---
# Enable-PSBreakpoint

## 概要
現在のコンソール内のブレークポイントを有効にします。

## SYNTAX

### Id (既定値)

```
Enable-PSBreakpoint [-PassThru] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ブレークポイント

```
Enable-PSBreakpoint [-PassThru] [-Breakpoint] <Breakpoint[]> [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

コマンドレットでは、 `Enable-PSBreakpoint` 無効になっているブレークポイントを再度有効にします。 これを使用すると、ブレークポイントオブジェクトまたは Id を指定することによって、すべてのブレークポイントまたは特定のブレークポイントを有効にできます。

ブレークポイントは、スクリプトの状態を確認できるように、実行が一時的に停止するスクリプト内のポイントです。 新しく作成されたブレークポイントは自動的に有効になりますが、を使用して無効にすることができ `Disable-PSBreakpoint` ます。

技術的には、このコマンドレットは、ブレークポイントオブジェクトの **Enabled** プロパティの値を **True** に変更します。

`Enable-PSBreakpoint` は、PowerShell スクリプトのデバッグ用に設計されたいくつかのコマンドレットの1つです。 PowerShell デバッガーの詳細については、「 [about_Debuggers](../Microsoft.PowerShell.Core/About/about_Debuggers.md)」を参照してください。

## 例

### 例 1: すべてのブレークポイントを有効にする

この例では、現在のセッションのすべてのブレークポイントを有効にします。

```powershell
Get-PSBreakpoint | Enable-PSBreakpoint
```

エイリアスを使用して、この例をとして省略でき `gbp | ebp` ます。

### 例 2: ID によってブレークポイントを有効にする

この例では、ブレークポイント Id を使用して複数のブレークポイントを有効にします。

```powershell
Enable-PSBreakpoint -Id 0, 1, 5
```

### 例 3: 無効になっているブレークポイントを有効にする

この例では、無効になっているブレークポイントを再度有効にします。

```powershell
$B = Set-PSBreakpoint -Script "sample.ps1" -Variable Name -PassThru
$B | Enable-PSBreakpoint -PassThru
```

```Output
AccessMode : Write
Variable   : Name
Action     :
Enabled    : False
HitCount   : 0
Id         : 0
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1

AccessMode : Write
Variable   : Name
Action     :
Enabled    : True
HitCount   : 0
Id         : 0
Script     : C:\ps-test\sample.ps1
ScriptName : C:\ps-test\sample.ps1
```

`Set-PSBreakpoint` スクリプト内の **名前** 変数にブレークポイントを作成し、 `Sample.ps1` 変数内のブレークポイントオブジェクトを保存し `$B` ます。 **PassThru** パラメーターは、ブレークポイントの **Enabled** プロパティの値が **False** であることを示します。

`Enable-PSBreakpoint` ブレークポイントを再度有効にします。 ここでも、 **PassThru** パラメーターを使用すると、 **Enabled** プロパティの値が **True** であることがわかります。

### 例 4: 変数を使用してブレークポイントを有効にする

この例では、ブレークポイントオブジェクトを使用して、一連のブレークポイントを有効にします。

```powershell
$B = Get-PSBreakpoint -Id 3, 5
Enable-PSBreakpoint -Breakpoint $B
```

`Get-PSBreakpoint` ブレークポイントを取得し、変数に保存し `$B` ます。 **ブレーク** ポイントパラメーターを使用すると、 `Enable-PSBreakpoint` ブレークポイントを有効にできます。

この例は、を実行することと同じです `Enable-PSBreakpoint -Id 3, 5` 。

## PARAMETERS

### -ブレークポイント

有効にするブレークポイントを指定します。 ブレークポイントを含む変数、またはブレークポイントオブジェクトを取得するコマンド (など) を指定し `Get-PSBreakpoint` ます。 パイプを使用して、ブレークポイントオブジェクトをにパイプすることもでき `Enable-PSBreakpoint` ます。

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

有効にするブレークポイントの **Id** 番号を指定します。 既定値はすべてのブレークポイントです。
**Id** を数値または変数で指定します。 **Id** 番号をにパイプすることはできません `Enable-PSBreakpoint` 。 ブレークポイントの **Id** を確認するには、 `Get-PSBreakpoint` コマンドレットを使用します。

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

有効になっているブレークポイントを表すオブジェクトを返します。 既定では、このコマンドレットは出力を生成しません。

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

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

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

パイプを使用して、ブレークポイントオブジェクトをに渡し `Enable-PSBreakpoint` ます。

## 出力

### None または system.string。

**PassThru** パラメーターを使用すると、 `Enable-PSBreakpoint` 有効にされたブレークポイントを表すブレークポイントオブジェクトが返されます。 それ以外の場合、このコマンドレットは出力を生成しません。

## 注

- 既に有効になっている `Enable-PSBreakpoint` ブレークポイントを有効にしようとすると、コマンドレットはエラーを生成しません。 そのため、一部のブレークポイントのみが無効な場合でも、エラーを生成せずにすべてのブレークポイントを有効にできます。

- ブレークポイントは、コマンドレットを使用して作成するときに有効になり `Set-PSBreakpoint` ます。 新しく作成されたブレークポイントを有効にする必要はありません。

## 関連リンク

[Disable-PSBreakpoint](Disable-PSBreakpoint.md)

[Get-PSBreakpoint](Get-PSBreakpoint.md)

[Get-PSCallStack](Get-PSCallStack.md)

[Remove-PSBreakpoint](Remove-PSBreakpoint.md)

[Set-PSBreakpoint](Set-PSBreakpoint.md)

