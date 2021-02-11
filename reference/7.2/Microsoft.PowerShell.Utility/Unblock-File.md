---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/unblock-file?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Unblock-File
ms.openlocfilehash: 8bbfe761a71b38b541f23730d84eb0023a059318
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602621"
---
# Unblock-File

## 概要
インターネットからダウンロードされたファイルのブロックを解除します。

## SYNTAX

### ByPath (既定値)

```
Unblock-File [-Path] <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Unblock-File -LiteralPath <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Unblock-File`コマンドレットを使用すると、インターネットからダウンロードされたファイルを開くことができます。 これにより、powershell の実行ポリシーが **RemoteSigned** の場合でも、インターネットからダウンロードされた powershell スクリプトファイルのブロックが解除され、実行できるようになります。 既定では、信頼されていないファイルからコンピューターを保護するために、これらのファイルはブロックされています。

コマンドレットを使用する前に、 `Unblock-File` ファイルとそのソースを確認し、開いても安全であることを確認します。

内部的には、 `Unblock-File` コマンドレットは、インターネットからダウンロードされたことを示す値が "3" である、ゾーン識別子の代替データストリームを削除します。

PowerShell 実行ポリシーの詳細については、「 [about_Execution_Policies](../Microsoft.PowerShell.Core/about/about_Execution_Policies.md)」を参照してください。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: ファイルのブロックを解除する

このコマンドは、PowerShellTips.chm ファイルをブロック解除します。

```
PS C:\> Unblock-File -Path C:\Users\User01\Documents\Downloads\PowerShellTips.chm
```

### 例 2: 複数のファイルのブロックを解除する

このコマンドは、 `C:\Downloads` 名前に "PowerShell" が含まれているディレクトリ内のすべてのファイルのブロックを解除します。 すべてのファイルが安全なことを確認してから、コマンドを実行してください。

```
PS C:\> dir C:\Downloads\*PowerShell* | Unblock-File
```

### 例 3: スクリプトを検索してブロックを解除する

このコマンドは、PowerShell スクリプトを検索してブロックを解除する方法を示しています。

最初のコマンドは、 *Get Item* コマンドレット get Files の **stream** パラメーターを、ゾーン識別子ストリームと共に使用します。

2番目のコマンドは、実行ポリシーが **RemoteSigned** されている PowerShell セッションでブロックされているスクリプトを実行するとどうなるかを示しています。 RemoteSigned ポリシーにより、デジタル署名されている場合を除き、インターネットからダウンロードされたスクリプトの実行は防止されます。

3番目のコマンドは、コマンドレットを使用して、 `Unblock-File` セッションで実行できるようにスクリプトのブロックを解除します。

```
PS C:\> Get-Item * -Stream "Zone.Identifier" -ErrorAction SilentlyContinue
   FileName: C:\ps-test\Start-ActivityTracker.ps1

Stream                   Length
------                   ------
Zone.Identifier              26

PS C:\> C:\ps-test\Start-ActivityTracker.ps1
c:\ps-test\Start-ActivityTracker.ps1 : File c:\ps-test\Start-ActivityTracker.ps1 cannot
be loaded. The file c:\ps-test\Start-ActivityTracker.ps1 is not digitally signed. The script
will not execute on the system. For more information, see about_Execution_Policies.

At line:1 char:1
+ c:\ps-test\Start-ActivityTracker.ps1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess

PS C:\> Get-Item C:\ps-test\Start-ActivityTracker.ps1 | Unblock-File
```

## PARAMETERS

### -LiteralPath

ブロックを解除するファイルを指定します。 **Path** と異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Path

ブロックを解除するファイルを指定します。 ワイルドカード文字がサポートされています。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
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

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

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

### System.String

パイプを使用して、ファイルパスをにパイプすることができ `Unblock-File` ます。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

- PowerShell 7 で macOS のサポートが追加されました。
- `Unblock-File`コマンドレットは、ファイルシステムドライブでのみ機能します。
- `Unblock-File`ファイルエクスプローラーの [**プロパティ**] ダイアログボックスの [**ブロック解除**] ボタンと同じ操作を実行します。
- ブロックされていないファイルに対してコマンドレットを使用しても、コマンドはブロックされていない `Unblock-File` ファイルには影響せず、コマンドレットはエラーを生成しません。

## 関連リンク

[about_Execution_Policies](../Microsoft.PowerShell.Core/About/about_Execution_Policies.md)

[Get-Item](../Microsoft.PowerShell.Management/Get-Item.md)

[Out-File](Out-File.md)

[FileSystem プロバイダー](../Microsoft.PowerShell.Core/about/about_FileSystem_Provider.md)
