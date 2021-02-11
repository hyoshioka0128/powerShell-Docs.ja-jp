---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 11/12/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-error?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Error
ms.openlocfilehash: 7e4f4adf60a4f6386c646d92ada0003f239f05f4
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605179"
---
# Get-Error

## 概要

現在のセッションからの最新のエラーメッセージを取得して表示します。

## SYNTAX

### 最新 (既定値)

```
Get-Error [[-Newest] <Int32>] [<CommonParameters>]
```

### エラー

```
Get-Error [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

`Get-Error`コマンドレットは、セッションで発生した最後のエラーからの現在のエラーの詳細を表す **Psexthe dederror** オブジェクトを取得します。

を使用すると、 `Get-Error` **最新** のパラメーターを使用して、現在のセッションで発生した指定された数のエラーを表示できます。

また、このコマンドレットは、などの `Get-Error` コレクションからエラーオブジェクトを受け取り `$Error` 、現在のセッションから複数のエラーを表示します。

## 例

### 例 1: 最新のエラーの詳細を取得する

この例では、は、 `Get-Error` 現在のセッションで発生した最新のエラーの詳細を表示します。

```powershell
Get-Childitem -path /NoRealDirectory
Get-Error
```

```
Get-ChildItem: Cannot find path 'C:\NoRealDirectory' because it does not exist.

Exception             :
    ErrorRecord          :
        Exception             :
            Message : Cannot find path 'C:\NoRealDirectory' because it does not exist.
            HResult : -2146233087
        TargetObject          : C:\NoRealDirectory
        CategoryInfo          : ObjectNotFound: (C:\NoRealDirectory:String) [], ParentContainsErrorRecordException
        FullyQualifiedErrorId : PathNotFound
    ItemName             : C:\NoRealDirectory
    SessionStateCategory : Drive
    TargetSite           :
        Name          : GetChildItems
        DeclaringType : System.Management.Automation.SessionStateInternal
        MemberType    : Method
        Module        : System.Management.Automation.dll
    StackTrace           :
   at System.Management.Automation.SessionStateInternal.GetChildItems(String path, Boolean recurse, UInt32 depth,
CmdletProviderContext context)
   at System.Management.Automation.ChildItemCmdletProviderIntrinsics.Get(String path, Boolean recurse, UInt32
depth, CmdletProviderContext context)
   at Microsoft.PowerShell.Commands.GetChildItemCommand.ProcessRecord()
    Message              : Cannot find path 'C:\NoRealDirectory' because it does not exist.
    Source               : System.Management.Automation
    HResult              : -2146233087
TargetObject          : C:\NoRealDirectory
CategoryInfo          : ObjectNotFound: (C:\NoRealDirectory:String) [Get-ChildItem], ItemNotFoundException
FullyQualifiedErrorId : PathNotFound,Microsoft.PowerShell.Commands.GetChildItemCommand
InvocationInfo        :
    MyCommand        : Get-ChildItem
    ScriptLineNumber : 1
    OffsetInLine     : 1
    HistoryId        : 57
    Line             : Get-Childitem -path c:\NoRealDirectory
    PositionMessage  : At line:1 char:1
                       + Get-Childitem -path c:\NoRealDirectory
                       + ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    InvocationName   : Get-Childitem
    CommandOrigin    : Internal
ScriptStackTrace      : at <ScriptBlock>, <No file>: line 1
PipelineIterationInfo :
```

### 例 2: 現在のセッションで発生した、指定された数のエラーメッセージを取得する

この例では、を `Get-Error` **最新** のパラメーターと共に使用する方法を示します。 この例では、[ **最新** ] は、このセッションで発生した3つの最新のエラーの詳細を返します。

```powershell
Get-Error -Newest 3
```

### 例 3: 詳細なメッセージを受信するためにエラーのコレクションを送信する

自動変数には、 `$Error` 現在のセッションのエラーオブジェクトの配列が含まれています。 オブジェクトの配列をにパイプ処理して、 `Get-Error` 詳細なエラーメッセージを受け取ることができます。

この例で `$Error` は、はパイプを使用してコマンドレットに渡され `Get-Error` ます。 結果は、例1の結果に似た詳細なエラーメッセージの一覧になります。

```powershell
$Error | Get-Error
```

## PARAMETERS

### -最新

現在のセッションで発生したエラーの数を指定します。

```yaml
Type: System.Int32
Parameter Sets: Newest
Aliases: Last

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

このパラメーターは、パイプライン入力に使用されます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: Error
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### PSObject

任意の **PSObject** からの入力をサポートしますが、結果は **Errorrecord** または **Exception** オブジェクトが指定されていない限り異なります。

## 出力

### システムの管理. ErrorRecord # Psexの Dederror

**Psexの Dederror** オブジェクトに出力します。

## 注

`Get-Error` パイプラインの入力を受け入れます。 たとえば、`$Error | Get-Error` のようになります。

## 関連リンク

[about_Try_Catch_Finally](../Microsoft.PowerShell.Core/About/about_Try_Catch_Finally.md)
