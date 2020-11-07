---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 10/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/clear-recyclebin?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Clear-RecycleBin
ms.openlocfilehash: caae499b6d6b896acad94e2d83462676d0225165
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347450"
---
# Clear-RecycleBin

## 概要
ごみ箱の内容をクリアします。

## SYNTAX

### すべて

```
Clear-RecycleBin [[-DriveLetter] <String[]>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Clear-RecycleBin`コマンドレットは、コンピューターのごみ箱の内容を削除します。 この操作は、Windows の **空のごみ箱** の使用に似ています。

このコマンドレットは、PowerShell 7 で readded ました。

## 例

### 1: すべてのごみ箱をクリアします。

この例では、すべてのローカルコンピューターのごみ箱がクリアされています。

```powershell
Clear-RecycleBin
```

```Output
Confirm
Are you sure you want to perform this action?
Performing the operation "Clear-RecycleBin" on target "All of the contents of the Recycle Bin".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"):
```

`Clear-RecycleBin` ローカルコンピューター上のすべてのごみ箱をクリアするかどうかを確認するメッセージをユーザーに表示します。

### 2: 指定されたごみ箱をクリアします

この例では、指定されたドライブ文字のごみ箱をクリアします。

```powershell
Clear-RecycleBin -DriveLetter C
```

`Clear-RecycleBin`**ドライブ** 文字パラメーターを使用して、 **C** ボリュームにごみ箱を指定します。 コマンドを実行するかどうかを確認するメッセージがユーザーに表示されます。

### 3: 確認せずにすべてのごみ箱をクリアする

この例では、ローカルコンピューターのごみ箱をクリアするかどうかを確認するプロンプトは表示されません。

```powershell
Clear-RecycleBin -Force
```

`Clear-RecycleBin`**Force** パラメーターを使用し、ローカルコンピューター上のすべてのごみ箱をクリアするかどうかを確認するメッセージをユーザーに表示しません。

別の方法と `-Force` して、をに置き換え `-Confirm:$false` ます。

## PARAMETERS

### -ドライブ文字

1つのドライブ文字またはドライブ文字の配列に対してクリアするごみ箱を指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Force

ごみ箱をクリアするかどうかを確認するメッセージがユーザーに表示されないように指定します。

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

コマンドレットを実行する前に、ユーザーに確認を求めるメッセージが表示されます。 **Confirm** パラメーターが指定されていない場合でも、ユーザーに確認を求めるメッセージが表示されます。

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

が実行された場合に何が起こるか `Clear-RecycleBin` を示します。 コマンドレットは実行されません。

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

## 出力

### なし

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク
