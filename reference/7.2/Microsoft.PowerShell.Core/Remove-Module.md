---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/remove-module?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-Module
ms.openlocfilehash: 2954bbec68b837a73e5365ab6a1e8ecb501d4898
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605193"
---
# Remove-Module

## 概要
現在のセッションからモジュールを削除します。

## SYNTAX

### name

```
Remove-Module [-Name] <String[]> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### FullyQualifiedName

```
Remove-Module [-FullyQualifiedName] <ModuleSpecification[]> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ModuleInfo

```
Remove-Module [-ModuleInfo] <PSModuleInfo[]> [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

**Remove-Module** コマンドレットは、現在のセッションから、コマンドレットと関数などのモジュールのメンバーを削除します。

モジュールにアセンブリ (.dll) が含まれる場合は、アセンブリによって実装されているすべてのメンバーが削除されますが、アセンブリはアンロードされません。

このコマンドレットは、コンピューターからモジュールをアンインストール、または削除しません。
現在の PowerShell セッションにのみ影響します。

## 例

### 例 1: モジュールを削除する

```powershell
Remove-Module -Name "BitsTransfer"
```

このコマンドは、現在のセッションから BitsTransfer モジュールを削除します。

### 例 2: すべてのモジュールを削除する

```powershell
Get-Module | Remove-Module
```

このコマンドは、現在のセッションからすべてのモジュールを削除します。

### 例 3: パイプラインを使用してモジュールを削除する

```powershell
"FileTransfer", "PSDiagnostics" | Remove-Module -Verbose
```

```Output
VERBOSE: Performing operation "Remove-Module" on Target "filetransfer (Path: 'C:\Windows\system32\WindowsPowerShell\v1.0\Modules\filetransfer\filetransfer.psd1')".
VERBOSE: Performing operation "Remove-Module" on Target "Microsoft.BackgroundIntelligentTransfer.Management (Path: 'C:\Windows\assembly\GAC_MSIL\Microsoft.BackgroundIntelligentTransfer.Management\1.0.0.0__31bf3856ad364e35\Microsoft.BackgroundIntelligentTransfe
r.Management.dll')".
VERBOSE: Performing operation "Remove-Module" on Target "psdiagnostics (Path: 'C:\Windows\system32\WindowsPowerShell\v1.0\Modules\psdiagnostics\psdiagnostics.psd1')".
VERBOSE: Removing imported function 'Start-Trace'.
VERBOSE: Removing imported function 'Stop-Trace'.
VERBOSE: Removing imported function 'Enable-WSManTrace'.
VERBOSE: Removing imported function 'Disable-WSManTrace'.
VERBOSE: Removing imported function 'Enable-PSWSManCombinedTrace'.
VERBOSE: Removing imported function 'Disable-PSWSManCombinedTrace'.
VERBOSE: Removing imported function 'Set-LogProperties'.
VERBOSE: Removing imported function 'Get-LogProperties'.
VERBOSE: Removing imported function 'Enable-PSTrace'.
VERBOSE: Removing imported function 'Disable-PSTrace'.
VERBOSE: Performing operation "Remove-Module" on Target "PSDiagnostics (Path: 'C:\Windows\system32\WindowsPowerShell\v1.0\Modules\psdiagnostics\PSDiagnostics.psm1')".
```

このコマンドは、現在のセッションから BitsTransfer モジュールと PSDiagnostics モジュールを削除します。

このコマンドは、パイプライン演算子 (|) を使用してモジュール名を **Remove-Module** に送信します。
*Verbose* 共通パラメーターを使用して、削除されるメンバーに関する詳細な情報を取得します。

*詳細* メッセージには、削除された項目が表示されます。
BitsTransfer モジュールには、コマンドレットを実装するアセンブリ、および独自のアセンブリを含む入れ子になったモジュールが含まれるため、これらのメッセージは異なります。
PSDiagnostics モジュールには、関数をエクスポートするモジュール スクリプト ファイル (.psm1) が含まれます。

### 例 4: ModuleInfo を使用してモジュールを削除する

```powershell
$a = Get-Module BitsTransfer
Remove-Module -ModuleInfo $a
```

このコマンドは、 *Moduleinfo* パラメーターを使用して BitsTransfer モジュールを削除します。

## PARAMETERS

### -Force

このコマンドレットが読み取り専用モジュールを削除することを示します。
既定では、**Remove-Module** は、読み取り/書き込みモジュールのみ削除します。

**ReadOnly** 値と **ReadWrite** 値は、モジュールの **AccessMode** プロパティに格納されます。

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

### -FullyQualifiedName

削除するモジュールの完全修飾名を指定します。

```yaml
Type: Microsoft.PowerShell.Commands.ModuleSpecification[]
Parameter Sets: FullyQualifiedName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ModuleInfo

削除するモジュール オブジェクトを指定します。
モジュールオブジェクト (**PSModuleInfo**) を含む変数、または Get-Module コマンドなどのモジュールオブジェクトを取得するコマンドを入力します。
パイプを使用してモジュール オブジェクトを **Remove-Module** に渡すこともできます。

```yaml
Type: System.Management.Automation.PSModuleInfo[]
Parameter Sets: ModuleInfo
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Name

削除するモジュールの名前を指定します。
ワイルドカード文字を使用できます。
パイプを使用して名前の文字列を **Remove-Module** に渡すこともできます。

```yaml
Type: System.String[]
Parameter Sets: name
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
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

### System.string, PSModuleInfo のようになります。

パイプを使用してモジュール名とモジュールオブジェクトを削除し、モジュールを **削除** することができます。

## 出力

### なし

このコマンドレットは出力を生成しません。

## 注

モジュールを削除する場合、モジュールにはを実行するイベントがあります。
このイベントにより、モジュールは削除されたことに反応し、リソースの解放などのクリーンアップを実行できます。 例:

$OnRemoveScript = {

  \# クリーンアップの実行

  $cachedSessions |Remove-PSSession

}

$ExecutionContext.. SessionState + = $OnRemoveScript

完全な一貫性を確保するために、PowerShell プロセスの終了に対応することも役立ちます。

Register-EngineEvent-SourceIdentifier ([PsEngineEvent]:: 終了)-アクションの $OnRemoveScript

## 関連リンク

[Get-Module](Get-Module.md)

[Import-Module](Import-Module.md)

