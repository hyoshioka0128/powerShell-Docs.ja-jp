---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 05/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/clear-history?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Clear-History
ms.openlocfilehash: 1b465779f56c6bd71d7adfa7ffb5b391f5402656
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603054"
---
# Clear-History

## 概要
PowerShell セッションのコマンド履歴からエントリを削除します。

## SYNTAX

### IDParameter (既定値)

```
Clear-History [[-Id] <int[]>] [[-Count] <int>] [-Newest] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### CommandLineParameter

```
Clear-History [[-Count] <int>] [-CommandLine <string[]>] [-Newest] [-WhatIf] [-Confirm]
[<CommonParameters>]
```

## Description

`Clear-History` PowerShell セッションからコマンド履歴を削除します。 各 PowerShell セッションには、独自のコマンド履歴があります。 コマンドの履歴を表示するには、コマンドレットを使用し `Get-History` ます。

既定では、は `Clear-History` PowerShell セッションからコマンド履歴全体を削除します。 でパラメーターを使用すると、 `Clear-History` 選択したコマンドを削除できます。

`Clear-History` では、コマンド履歴ファイルはクリアされません `PSReadLine` 。 モジュールは、 `PSReadLine` すべての powershell セッションからのすべての powershell コマンドを含む履歴ファイルを格納します。 PowerShell プロンプトで、キーボードの上矢印と下矢印を使用して、コマンドの履歴をスクロールします。 コマンド履歴の構成を表示するには `PSReadLine` 、を使用 `Get-PSReadLineOption` します。
`PSReadLine` PowerShell 5.0 以降に付属しています。 詳細については、「 [about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)」を参照してください。

## 例

### 例 1: PowerShell セッションからコマンド履歴を削除する

このコマンドは、PowerShell セッションの履歴からすべてのコマンドを削除します。

```powershell
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location .\Test
   2 Update-Help
   3 Set-Location C:\Test\Logs
   4 Get-Location
```

```powershell
Clear-History
Get-History
```

```Output
  Id CommandLine
  -- -----------
   5 Clear-History
```

コマンドレットにより、 `Get-History` PowerShell セッションの履歴が表示されます。 `Clear-History` コマンド履歴全体を削除します。 `Get-History` 更新されたコマンドの履歴を表示し、以前の履歴が削除されたことを確認します。

### 例 2: 最新のコマンドを削除する

このコマンドは、 **Count** と **最新** のパラメーターを使用して、PowerShell セッションの履歴から最新のコマンドを削除します。

```powershell
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-Command Clear-History
   3 Get-Command Clear-History -Syntax
   4 Get-Command Clear-History -ShowCommandInfo
   5 Get-Help Get-Alias
   6 Get-Command Get-ChildItem -Syntax
   7 Get-Help Clear-History
   8 Set-Location C:\Test\Logs
   9 Get-Help Get-Variable
  10 Get-Help Get-ChildItem
```

```powershell
Clear-History -Count 5 -Newest
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-Command Clear-History
   3 Get-Command Clear-History -Syntax
   4 Get-Command Clear-History -ShowCommandInfo
   5 Get-Help Get-Alias
  11 Clear-History -Count 5 -Newest
```

コマンドレットにより、 `Get-History` PowerShell セッションの履歴が表示されます。 `Clear-History` コマンド履歴を削除するには、を使用します。 **Count** パラメーターは、指定した **Id** を含む、削除するコマンドの数を指定します。**最新** のパラメーターは、最新のコマンドが履歴から消去されることを指定します。 `Get-History`更新されたコマンドの履歴を表示し、5つの最新のコマンドが削除されたことを確認します。 **id 6**  -  **id 10**。

### 例 3: 特定の条件に一致するコマンドを削除する

このコマンドは、 **CommandLine** パラメーターによって定義された特定の条件に一致するコマンドを削除します。

```powershell
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-Command Clear-History
   3 Get-Command Clear-History -Syntax
   4 Get-Command Clear-History -ShowCommandInfo
   5 Get-Help Get-Alias
   6 Get-Command Get-ChildItem -Syntax
   7 Get-Help Clear-History
```

```powershell
Clear-History -CommandLine *Help*, *Syntax
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-Command Clear-History
   4 Get-Command Clear-History -ShowCommandInfo
   8 Clear-History -CommandLine *Help*, *Syntax
```

コマンドレットにより、 `Get-History` PowerShell セッションの履歴が表示されます。 `Clear-History` コマンド履歴を削除します。 **CommandLine** パラメーターは、 **Help** または end with **構文** を含むコマンドを指定します。 `Get-History` 更新されたコマンドの履歴を表示し、コマンド **id 3**、 **id 5**、 **id 6**、および **id 7** が削除されたことを確認します。

### 例 4: Id 番号を指定してコマンドを削除する

このコマンドは、 **Id** を使用して特定の履歴項目を削除します。複数のコマンドを削除するには、 **Id** 番号のコンマ区切りリストを送信します。

```powershell
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-History
   3 Get-Help Get-Alias
   4 Get-Command Clear-History
   5 Get-Command Clear-History -Syntax
   6 Get-Command Clear-History -ShowCommandInfo
```

```powershell
Clear-History -Id 3, 5
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-History
   4 Get-Command Clear-History
   6 Get-Command Clear-History -ShowCommandInfo
   7 Get-History
   8 Clear-History -Id 3, 5
```

コマンドレットにより、 `Get-History` PowerShell セッションの履歴が表示されます。 `Clear-History` コマンド履歴を削除します。 **Id** パラメーターは、削除するコマンドを指定します。 `Get-History` 更新されたコマンドの履歴を表示し、 **id 3** と **id 5** が削除されたことを確認します。

### 例 5: Id 番号と数でコマンドを削除する

このコマンドは、 **Id** パラメーターと **Count** パラメーターを使用してコマンド履歴を削除します。 コマンドは、指定された **Id** から逆順に削除されます。

```powershell
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-Command Clear-History
   3 Get-Command Clear-History -Syntax
   4 Get-Command Clear-History -ShowCommandInfo
   5 Get-Help Get-Alias
   6 Get-Command Get-ChildItem -Syntax
   7 Get-Help Clear-History
   8 Set-Location C:\Test\Logs
   9 Get-Help Get-Variable
  10 Get-Help Get-ChildItem
```

```powershell
Clear-History -Id 7 -Count 5
Get-History
```

```Output
  Id CommandLine
  -- -----------
   1 Set-Location C:\Test\
   2 Get-Command Clear-History
   8 Set-Location C:\Test\Logs
   9 Get-Help Get-Variable
  10 Get-Help Get-ChildItem
  11 Clear-History -Id 7 -Count 5
```

コマンドレットにより、 `Get-History` PowerShell セッションの履歴が表示されます。 `Clear-History` コマンド履歴を削除します。 **Id** パラメーターは、 **id 7** で始まることを指定します。 **Count** パラメーターは、指定された **Id** を含む5つのコマンドを削除するように指定します。更新された `Get-History` コマンド履歴を表示し、5つのコマンドが削除されたことを確認します。 **id 3**  -  **id 7**。

## PARAMETERS

### -CommandLine

PowerShell セッションからコマンドの履歴を削除します。 文字列は完全に一致する必要があります。または、によって表示される PowerShell セッション履歴のコマンドと一致させるには、ワイルドカードを使用し `Get-History` ます。 複数の文字列を入力すると、によって、 `Clear-History` 任意の文字列に一致するコマンドが削除されます。 **CommandLine** パラメーターは **Count** と共に使用できます。

スペースを含む文字列の場合は、単一引用符を使用します。 詳細については、「 [about_Quoting_Rules](About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: CommandLineParameter
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -カウント

が削除する履歴エントリの数を指定し `Clear-History` ます。 コマンドは、履歴内の最も古いエントリから順に削除されます。

**Count** パラメーターと **Id** パラメーターは一緒に使用できます。 **Count** パラメーターは、指定した **Id** を含む、削除するコマンドの数を指定します。指定された **Id** から開始すると、コマンドは順番に逆順に削除されます。 たとえば、 **Id** が30で **カウント** が10の場合、に `Clear-History` よって21から30までの項目が削除されます。

**Count** パラメーターと **CommandLine** パラメーターは一緒に使用できます。 **カウント** は、コマンド **ライン** パラメーターの値と一致する、削除するコマンドの数を指定します。 コマンドは順番に削除されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

が削除するコマンド履歴 **Id** を指定し `Clear-History` ます。 **Id** 番号を表示するには、コマンドレットを使用し `Get-History` ます。 **Id** 番号は順次で、コマンドは PowerShell セッション全体で **id** 番号を保持します。 **Id** パラメーターは、 **Count** **および all** と共に使用できます。

```yaml
Type: System.Int32[]
Parameter Sets: IDParameter
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -最新

**最新** のパラメーターを使用すると、によって `Clear-History` 履歴内の最新のエントリが削除されます。 既定では、は `Clear-History` 履歴内の最も古いエントリを削除します。

**最新** のパラメーターは、 **Id** と **カウント** と共に使用できます。 **Count** パラメーターは、指定した **Id** を含む、削除するコマンドの数を指定します。指定された **Id** から開始すると、コマンドは順番に削除されます。 たとえば、 **Id** が30で **カウント** が10の場合、では `Clear-History` 30 ~ 39 の項目が削除されます。

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

コマンドレットを実行する前に確認メッセージを表示 `Clear-History` します。

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

コマンドレットを実行した場合の動作 `Clear-History` を示します。 このコマンドレットは実行されません。

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

### なし

パイプを使用してオブジェクトをにパイプすることはできません `Clear-History` 。

## 出力

### なし

`Clear-History` は出力を生成しません。

## 注

PowerShell セッションの履歴は、PowerShell セッション中に入力されたコマンドの一覧です。 履歴を表示し、コマンドを追加、削除できます。また履歴からコマンドを実行できます。 詳細については、「 [about_History](About/about_History.md)」を参照してください。

セッション履歴は、 **Psreadline** モジュールによって保持されている履歴とは別に管理されます。
どちらの履歴も、 **Psreadline** が読み込まれたセッションで使用できます。 このコマンドレットは、セッション履歴でのみ機能します。 詳細については、「」、「 [about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)」を参照してください。

## 関連リンク

[about_History](About/about_History.md)

[about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)

[about_Quoting_Rules](About/about_Quoting_Rules.md)

[Add-History](Add-History.md)

[Get-History](Get-History.md)

[Invoke-History](Invoke-History.md)

[Get-PSReadLineOption](/powershell/module/psreadline/get-psreadlineoption)

[設定-PSReadLineOption](/powershell/module/psreadline/set-psreadlineoption)

