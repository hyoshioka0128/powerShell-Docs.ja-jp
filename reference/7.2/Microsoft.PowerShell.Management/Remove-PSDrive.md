---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/remove-psdrive?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-PSDrive
ms.openlocfilehash: d20a348f3f5ba193eb85e0f9d0e2cc11ad083b47
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601016"
---
# Remove-PSDrive

## 概要
一時 PowerShell ドライブを削除し、マップされたネットワークドライブを切断します。

## SYNTAX

### 名前 (既定値)

```
Remove-PSDrive [-Name] <String[]> [-PSProvider <String[]>] [-Scope <String>] [-Force]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### LiteralName

```
Remove-PSDrive [-LiteralName] <String[]> [-PSProvider <String[]>] [-Scope <String>] [-Force]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは `Remove-PSDrive` 、コマンドレットを使用して作成された一時 PowerShell ドライブを削除し `New-PSDrive` ます。

Windows PowerShell 3.0 以降では、は、 `Remove-PSDrive` のパラメーターを使用して作成されたドライブも含めて、マップされたネットワークドライブを切断し `Persist` `New-PSDrive` ます。

`Remove-PSDrive` Windows 物理ドライブまたは論理ドライブを削除できません。

Windows PowerShell 3.0 以降では、外部ドライブがコンピューターに接続されている場合、PowerShell は新しいドライブを表す PSDrive をファイルシステムに自動的に追加します。
PowerShell を再起動する必要はありません。
同様に、外部ドライブがコンピューターから切断されると、PowerShell は、削除されたドライブを表す PSDrive を自動的に削除します。

## 例

### 例 1: ファイルシステムドライブを削除する

このコマンドは、"smp" という名前の一時ファイル システム ドライブを削除します。

```powershell
Remove-PSDrive -Name smp
```

### 例 2: マップされたネットワークドライブを削除する

このコマンドは `Remove-PSDrive` 、を使用して、マップされた X: および S: ネットワークドライブを切断します。

```powershell
Get-PSDrive X, S | Remove-PSDrive
```

## PARAMETERS

### -Force

現在の PowerShell ドライブを削除します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralName

ドライブの名前を指定します。

**LiteralName** の値は、入力した内容のまま使用されます。
ワイルドカードとして解釈される文字はありません。
名前にエスケープ文字が含まれている場合は、単一引用符 (') で囲みます。
単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: LiteralName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

削除するドライブの名前を指定します。
ドライブ名の後にコロン (:) を入力しないでください。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -PSProvider

**Psprovider** オブジェクトの配列を指定します。
このコマンドレットは、指定された PowerShell プロバイダーに関連付けられているすべてのドライブを削除し、切断します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -スコープ

ドライブのスコープを指定します。
このパラメーターに指定できる値は、Global、Local、および Script、または現在のスコープを基準とする数値です。 スコープは、スコープの数から0までの範囲です。 現在のスコープ番号は0で、その親は1です。
詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Local
Accept pipeline input: True (ByPropertyName)
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.Management.Automation.PSDriveInfo

パイプを使用して、コマンドレットなどのドライブオブジェクトを `Get-PSDrive` `Remove-PSDrive` コマンドレットに渡します。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## 注

- `Remove-PSDrive`コマンドレットは、PowerShell プロバイダーによって公開されるデータを使用するように設計されています。 セッションのプロバイダーの一覧を表示するには、`Get-PSProvider` コマンドレットを使用します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Get-PSDrive](Get-PSDrive.md)

[New-PSDrive](New-PSDrive.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

