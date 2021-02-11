---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-pscallstack?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSCallStack
ms.openlocfilehash: 607e3999a1dbe9a4f22a751f11ac93ee3f9d9409
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599910"
---
# Get-PSCallStack

## 概要
現在の呼び出し履歴を表示します。

## SYNTAX

```
Get-PSCallStack [<CommonParameters>]
```

## Description

**Get PSCallStack** コマンドレットは、現在の呼び出し履歴を表示します。

PowerShell デバッガーで使用するように設計されていますが、このコマンドレットを使用すると、デバッガーの外部にあるスクリプトまたは関数で呼び出し履歴を表示できます。

デバッガーで **Get PSCallStack** コマンドを実行するには、またはを入力し `k` `Get-PSCallStack` ます。

## 例

### 例 1: 関数の呼び出し履歴を取得する

```
PS C:\> function my-alias {
$p = $args[0]
Get-Alias | where {$_.definition -like "*$p"} | format-table definition, name -auto
}
PS C:\ps-test> Set-PSBreakpoint -Command my-alias
Command    : my-alias
Action     :
Enabled    : True
HitCount   : 0
Id         : 0
Script     : prompt PS C:\> my-alias Get-Content

Entering debug mode. Use h or ? for help.
Hit Command breakpoint on 'prompt:my-alias'
my-alias get-content
[DBG]: PS C:\ps-test> s
$p = $args[0]
DEBUG: Stepped to ':    $p = $args[0]    '
[DBG]: PS C:\ps-test> s
get-alias | Where {$_.Definition -like "*$p*"} | format-table Definition,
[DBG]: PS C:\ps-test>get-pscallstack

Name        CommandLineParameters         UnboundArguments              Location
----        ---------------------         ----------------              --------
prompt      {}                            {}                            prompt
my-alias    {}                            {get-content}                 prompt
prompt      {}                            {}                            prompt PS C:\> [DBG]: PS C:\ps-test> o
Definition  Name
----------  ----
Get-Content gc
Get-Content cat
Get-Content type
```

このコマンドは、 **Get PSCallStack** コマンドレットを使用して、My エイリアスの呼び出し履歴を表示します。これは、コマンドレット名のエイリアスを取得する単純な関数です。

最初のコマンドは、PowerShell プロンプトで関数を入力します。
2 番目のコマンドは、Set-PSBreakpoint コマンドレットを使用して、My-Alias 関数にブレークポイントを設定します。
3 番目のコマンドは、My-Alias 関数を使用して、Get-Content コマンドレットに対して現在のセッションのすべてのエイリアスを取得します。

デバッガーは関数呼び出しに割り込みます。
2 つの連続するステップイン (s) コマンドは、行ごとに関数を実行します。
次に、 **Get PSCallStack** コマンドを使用して、呼び出し履歴を取得します。

最後のコマンドは、デバッガーを終了し、完了まで実行を継続する Step-Out コマンド (o) です。

## PARAMETERS

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### CallStackFrame (システム管理)

**Get-PSCallStack** は、呼び出し履歴内の項目を表すオブジェクトを返します。

## 注

## 関連リンク

[Disable-PSBreakpoint](Disable-PSBreakpoint.md)

[Enable-PSBreakpoint](Enable-PSBreakpoint.md)

[Format-Table](Format-Table.md)

[Get-PSBreakpoint](Get-PSBreakpoint.md)

[Remove-PSBreakpoint](Remove-PSBreakpoint.md)

[Set-PSBreakpoint](Set-PSBreakpoint.md)

