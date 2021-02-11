---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 02/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-Alias
ms.openlocfilehash: e9e80b4bf558dcf1e225868a1138979270ea304f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601788"
---
# Set-Alias

## 概要
現在の PowerShell セッションで、コマンドレットまたはその他のコマンドのエイリアスを作成または変更します。

## SYNTAX

### 既定値 (既定値)

```
Set-Alias [-Name] <string> [-Value] <string> [-Description <string>] [-Option <ScopedItemOptions>]
 [-PassThru] [-Scope <string>] [-Force] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットは、コマンド `Set-Alias` レットまたはコマンド (関数、スクリプト、ファイル、その他の実行可能ファイルなど) のエイリアスを作成または変更します。 エイリアスは、コマンドレットまたはコマンドを参照する代替名です。
たとえば、 `sal` はコマンドレットのエイリアスです `Set-Alias` 。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

コマンドレットは複数のエイリアスを持つことができますが、エイリアスは1つのコマンドレットにのみ関連付けることができます。 を使用すると、 `Set-Alias` 既存のエイリアスを別のコマンドレットに再割り当てしたり、説明などのエイリアスのプロパティを変更したりできます。

によって作成または変更されたエイリアス `Set-Alias` は永続的ではなく、現在の PowerShell セッション中にのみ使用できます。 PowerShell セッションが終了すると、エイリアスは削除されます。

## 例

### 例 1: コマンドレットのエイリアスを作成する

このコマンドは、現在の PowerShell セッションのコマンドレットのエイリアスを作成します。

```
PS> Set-Alias -Name list -Value Get-ChildItem

PS> Get-Alias -Name list

CommandType     Name
-----------     ----
Alias           list -> Get-ChildItem
```

コマンドレットにより、 `Set-Alias` 現在の PowerShell セッションにエイリアスが作成されます。 **Name** パラメーターは、エイリアスの名前を指定し `list` ます。 **値** パラメーターは、エイリアスが実行されるコマンドレットを指定します。

エイリアスを実行するには、PowerShell コマンドラインで「」と入力し `list` ます。

### 例 2: 既存のエイリアスを別のコマンドレットに再割り当てする

このコマンドは、既存のエイリアスを再割り当てして別のコマンドレットを実行します。

```
PS> Get-Alias -Name list

CommandType     Name
-----------     ----
Alias           list -> Get-ChildItem

PS> Set-Alias -Name list -Value Get-Location

PS> Get-Alias -Name list

CommandType     Name
-----------     ----
Alias           list -> Get-Location
```

コマンドレットでは、 `Get-Alias` **Name** パラメーターを使用して別名を表示し `list` ます。 `list`エイリアスは、コマンドレットに関連付けられてい `Get-ChildItem` ます。 エイリアスを実行すると、 `list` 現在のディレクトリ内の項目が表示されます。

コマンドレットでは、 `Set-Alias` **Name** パラメーターを使用してエイリアスを指定し `list` ます。 **Value** パラメーターによって、エイリアスがコマンドレットに関連付けられ `Get-Location` ます。

コマンドレットでは、 `Get-Alias` **Name** パラメーターを使用して別名を表示し `list` ます。 `list`エイリアスは、コマンドレットに関連付けられてい `Get-Location` ます。 エイリアスを実行すると、 `list` 現在のディレクトリの場所が表示されます。

### 例 3: 読み取り専用のエイリアスを作成して変更する

このコマンドは、読み取り専用のエイリアスを作成します。 読み取り専用オプションを指定すると、エイリアスが意図せず変更されるのを防ぐことができます。 読み取り専用エイリアスを変更または削除するには、 **Force** パラメーターを使用します。

```
PS> Set-Alias -Name loc -Value Get-Location -Option ReadOnly -PassThru | Format-List -Property *

DisplayName         : loc -> Get-Location
Definition          : Get-Location
Options             : ReadOnly
Description         :
Name                : loc
CommandType         : Alias

PS> Set-Alias -Name loc -Value Get-Location -Option ReadOnly -Description 'Displays the current directory' -Force -PassThru | Format-List -Property *

DisplayName         : loc -> Get-Location
Definition          : Get-Location
Options             : ReadOnly
Description         : Displays the current directory
Name                : loc
CommandType         : Alias
```

コマンドレットにより、 `Set-Alias` 現在の PowerShell セッションにエイリアスが作成されます。 **Name** パラメーターは、エイリアスの名前を指定し `loc` ます。 **値** パラメーターは、 `Get-Location` エイリアスが実行されるコマンドレットを指定します。 **オプション** パラメーターは、 **ReadOnly** 値を指定します。 **PassThru** パラメーターは alias オブジェクトを表し、オブジェクトをパイプラインの下にあるオブジェクトをコマンドレットに送信し `Format-List` ます。 `Format-List`**プロパティパラメーターを** アスタリスク () と共に使用して `*` 、すべてのプロパティが表示されるようにします。 出力例では、これらのプロパティの部分的なリストを示しています。

`loc`2 つのパラメーターを追加することで、別名が変更されます。 **説明** エイリアスの目的を説明するテキストを追加します。 エイリアスは読み取り専用であるため、 **Force** パラメーターが必要です `loc` 。 **Force** パラメーターが使用されていない場合、変更は失敗します。

### 例 4: 実行可能ファイルのエイリアスを作成する

この例では、ローカルコンピューター上の実行可能ファイルのエイリアスを作成します。

```
PS> Set-Alias -Name np -Value C:\Windows\notepad.exe

PS> Get-Alias -Name np

CommandType     Name
-----------     ----
Alias           np -> notepad.exe
```

コマンドレットにより、 `Set-Alias` 現在の PowerShell セッションにエイリアスが作成されます。 **Name** パラメーターは、エイリアスの名前を指定し `np` ます。 **Value** パラメーターには **C:\Windows\notepad.exe** パスとアプリケーション名を指定します。 この `Get-Alias` コマンドレットでは、 **Name** パラメーターを使用して、 `np` エイリアスが **notepad.exe** に関連付けられていることを示しています。

エイリアスを実行するには、PowerShell コマンドラインで「」と入力して `np` **notepad.exe** を開きます。

### 例 5: パラメーターを使用してコマンドのエイリアスを作成する

この例では、パラメーターを使用して、コマンドにエイリアスを割り当てる方法を示します。

などのコマンドレットのエイリアスを作成でき `Set-Location` ます。 などのパラメーターと値を使用して、コマンドのエイリアスを作成することはできません `Set-Location -Path C:\Windows\System32` 。 コマンドのエイリアスを作成するには、コマンドを含む関数を作成し、その関数のエイリアスを作成します。 詳細については、「 [about_Functions](../Microsoft.PowerShell.Core/about/about_Functions.md)」を参照してください。

```
PS> Function CD32 {Set-Location -Path C:\Windows\System32}

PS> Set-Alias -Name Go -Value CD32
```

という名前 `CD32` の関数が作成されます。 関数は、 `Set-Location` **Path** パラメーターを指定したコマンドレットを使用して、 **C:\Windows\System32** ディレクトリを指定します。

コマンドレットにより、 `Set-Alias` 現在の PowerShell セッションの関数のエイリアスが作成されます。 **Name** パラメーターは、エイリアスの名前を指定し `Go` ます。 **Value** パラメーターは、関数の名前を指定し `CD32` ます。

エイリアスを実行するには、PowerShell コマンドラインで「」と入力し `Go` ます。 関数が実行され、 `CD32` ディレクトリ **C:\Windows\System32** に変更されます。

## PARAMETERS

### -Description

エイリアスの説明を指定します。 任意の文字列を入力できます。 説明にスペースが含まれている場合は、単一引用符で囲みます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

**Force** パラメーターを使用して、**オプション** パラメーターが **ReadOnly** に設定されているエイリアスを変更または削除します。

**Force** パラメーターは、 **Option** パラメーターを **Constant** に設定してエイリアスを変更または削除することはできません。

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

### -Name

新しいエイリアスの名前を指定します。 エイリアス名には、英数字とハイフンを使用できます。 エイリアス名には、123などの数値を指定できません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Option

エイリアスの **オプション** プロパティの値を設定します。 **ReadOnly** や **Constant** などの値は、意図しない変更からエイリアスを保護します。 セッション内のすべてのエイリアスの **Option** プロパティを表示するには、「」と入力 `Get-Alias | Format-Table -Property Name, Options -Autosize` します。

このパラメーターに指定できる値は次のとおりです。

- **Allscope** エイリアスは、作成された新しいスコープにコピーされます。
- **定数** 変更または削除することはできません。
- **なし** オプションを設定せず、既定値です。
- **プライベート** エイリアスは、現在のスコープでのみ使用できます。
- **読み取り専用****Force** パラメーターを使用しない限り、変更または削除することはできません。
- **未指定**

```yaml
Type: System.Management.Automation.ScopedItemOptions
Parameter Sets: (All)
Aliases:
Accepted values: AllScope, Constant, None, Private, ReadOnly, Unspecified

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

エイリアスを表すオブジェクトを返します。 オブジェクトを表示するには、などの書式設定コマンドレットを使用し `Format-List` ます。 既定で `Set-Alias` は、は出力を生成しません。

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

### -スコープ

このエイリアスが有効なスコープを指定します。 既定値は **Local** です。 詳細については、「 [about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)」を参照してください。

使用できる値は次のとおりです。

- グローバル
- ローカル
- プライベート
- 番号付きスコープ
- スクリプト

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Global, Local, Private, Numbered scopes, Script

Required: False
Position: Named
Default value: Local
Accept pipeline input: False
Accept wildcard characters: False
```

### -Value

エイリアスが実行されるコマンドレットまたはコマンドの名前を指定します。 **値** パラメーターは、エイリアスの **定義** プロパティです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
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

### なし

`Set-Alias` は、パイプラインからの入力を受け入れません。

## 出力

### None または system.servicemodel. エイリアス情報

**PassThru** パラメーターを使用すると、に `Set-Alias` よって、エイリアスを表す system.string オブジェクトが生成さ **れます。** それ以外の場合、 `Set-Alias` は出力を生成しません。

## 注

PowerShell には、各 PowerShell セッションで使用できる組み込みエイリアスが含まれています。 `Get-Alias`コマンドレットは、PowerShell セッションで使用できるエイリアスを表示します。

エイリアスを作成するには、コマンドレットまたはを使用し `Set-Alias` `New-Alias` ます。 PowerShell 6 では、エイリアスを削除するには、コマンドレットを使用し `Remove-Alias` ます。 `Remove-Item` は、以前のバージョンの PowerShell で作成されたスクリプトの場合など、下位互換性のために受け入れられます。 などのコマンドを使用し `Remove-Item -Path Alias:aliasname` ます。

各 PowerShell セッションで使用できるエイリアスを作成するには、それを PowerShell プロファイルに追加します。
詳細については、「 [about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md)」を参照してください。

エイリアスは、エクスポートとインポートを実行することで、別の PowerShell セッションで保存して再利用できます。 エイリアスをファイルに保存するには、を使用 `Export-Alias` します。 新しい PowerShell セッションに保存されたエイリアスを追加するには、を使用 `Import-Alias` します。

## 関連リンク

[about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)

[about_Functions](../Microsoft.PowerShell.Core/about/about_Functions.md)

[about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md)

[about_Scopes](../Microsoft.PowerShell.Core/About/about_Scopes.md)

[Export-Alias](Export-Alias.md)

[Get-Alias](Get-Alias.md)

[Import-Alias](Import-Alias.md)

[New-Alias](New-Alias.md)

[Remove-Alias](Remove-Alias.md)

[Remove-Item](../Microsoft.PowerShell.Management/Remove-Item.md)

