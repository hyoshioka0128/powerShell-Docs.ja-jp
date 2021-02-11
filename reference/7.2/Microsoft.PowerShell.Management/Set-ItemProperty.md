---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 05/14/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-itemproperty?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-ItemProperty
ms.openlocfilehash: 18383dc2b1e018e56864e412dfe75eca2b578a08
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600117"
---
# Set-ItemProperty

## 概要
項目のプロパティの値を作成または変更します。

## SYNTAX

### propertyValuePathSet (既定値)

```
Set-ItemProperty [-Path] <String[]> [-Name] <String> [-Value] <Object> [-PassThru] [-Force] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [-Type <RegistryValueKind>] [<CommonParameters>]
```

### propertyPSObjectPathSet

```
Set-ItemProperty [-Path] <String[]> -InputObject <PSObject> [-PassThru] [-Force] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [-Type <RegistryValueKind>] [<CommonParameters>]
```

### Propertyvaluelろ Alpathset

```
Set-ItemProperty -LiteralPath <String[]> [-Name] <String> [-Value] <Object> [-PassThru] [-Force]
 [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [-Type <RegistryValueKind>] [<CommonParameters>]
```

### propertyPSObjectLiteralPathSet

```
Set-ItemProperty -LiteralPath <String[]> -InputObject <PSObject> [-PassThru] [-Force] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [-Type <RegistryValueKind>] [<CommonParameters>]
```

## Description

コマンドレットにより、 `Set-ItemProperty` 指定した項目のプロパティの値が変更されます。
コマンドレットを使用して、項目のプロパティを作成または変更できます。
たとえば、を使用して、 `Set-ItemProperty` ファイルオブジェクトの **IsReadOnly** プロパティの値をに設定でき `$True` ます。

また、を使用し `Set-ItemProperty` て、レジストリ値とデータを作成および変更します。
たとえば、新しいレジストリ エントリをキーに追加し、その値を設定または変更できます。

## 例

### 例 1: ファイルのプロパティを設定する

このコマンドは、"final.doc" ファイルの **IsReadOnly** プロパティの値を "true" に設定します。
**パス** を使用して、**ファイルを指定し、プロパティ** の名前を指定し、 **value** パラメーターを使用して新しい値を指定します。

ファイルは **IsReadOnly** オブジェクトであり、そのプロパティの1つにすぎ **ません。**
すべてのプロパティを表示するには、「」と入力 `Get-Item C:\GroupFiles\final.doc | Get-Member -MemberType
Property` します。

`$true`自動変数は、値 "TRUE" を表します。
詳細については、「 [about_Automatic_Variables](../Microsoft.PowerShell.Core/About/about_Automatic_Variables.md)」を参照してください。

```powershell
Set-ItemProperty -Path C:\GroupFiles\final.doc -Name IsReadOnly -Value $true
```

### 例 2: レジストリエントリと値を作成する

この例では、を使用し `Set-ItemProperty` て新しいレジストリエントリを作成し、エントリに値を割り当てる方法を示します。 キーの "ContosoCompany" キーに "NoOfEmployees" エントリを作成 `HKLM\Software` し、その値を823に設定します。

レジストリエントリはレジストリキーのプロパティ (項目) と見なされるため、を使用して `Set-ItemProperty` レジストリエントリを作成し、値を設定して変更します。

```powershell
Set-ItemProperty -Path "HKLM:\Software\ContosoCompany" -Name "NoOfEmployees" -Value 823
Get-ItemProperty -Path "HKLM:\Software\ContosoCompany"
```

```Output
PSPath        : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\software\contosocompany
PSParentPath  : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\software
PSChildName   : contosocompany
PSDrive       : HKLM
PSProvider    : Microsoft.PowerShell.Core\Registry
NoOfLocations : 2
NoOfEmployees : 823
```

```powershell
Set-ItemProperty -Path "HKLM:\Software\ContosoCompany" -Name "NoOfEmployees" -Value 824
Get-ItemProperty -Path "HKLM:\Software\ContosoCompany"
```

```Output
PSPath        : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\software\contosocompany
PSParentPath  : Microsoft.PowerShell.Core\Registry::HKEY_LOCAL_MACHINE\software
PSChildName   : contosocompany
PSDrive       : HKLM
PSProvider    : Microsoft.PowerShell.Core\Registry
NoOfLocations : 2
NoOfEmployees : 824
```

最初のコマンドは、レジストリエントリを作成します。
**パス** を使用して、ドライブのパス `HKLM:` と "Software\MyCompany" キーを指定します。
このコマンドでは、 **名前** を使用してエントリ名と値を指定し、 **値** を指定します。

2番目のコマンドは、 `Get-ItemProperty` コマンドレットを使用して、新しいレジストリエントリを確認します。
またはコマンドレットを使用した場合、エントリは、 `Get-Item` `Get-ChildItem` 項目や子項目ではなく、キーのプロパティであるため、表示されません。

3番目のコマンドは、 **Noofemployees** エントリの値を824に変更します。

また、 `New-ItemProperty` コマンドレットを使用してレジストリエントリとその値を作成し、を使用して値を変更することもでき `Set-ItemProperty` ます。

ドライブの詳細については `HKLM:` 、「」と入力 `Get-Help Get-PSDrive` してください。
PowerShell を使用してレジストリを管理する方法の詳細については、「」と入力 `Get-Help Registry` してください。

### 例 3: パイプラインを使用して項目を変更する

これらのコマンドは、パイプライン演算子 () を使用してに項目を送信する方法を示して `|` `Set-ItemProperty` います。

コマンドの最初の部分では、を使用して、 `Get-ChildItem` "Weekly.txt" ファイルを表すオブジェクトを取得します。 このコマンドは、パイプライン演算子を使用して、ファイルオブジェクトをに送信し `Set-ItemProperty` ます。
この `Set-ItemProperty` コマンドは、 **Name** パラメーターと **value** パラメーターを使用して、プロパティとその新しい値を指定します。

このコマンドは、 **InputObject** パラメーターを使用して、が取得するオブジェクトを指定することと同じです `Get-ChildItem` 。

```powershell
Get-ChildItem weekly.txt | Set-ItemProperty -Name IsReadOnly -Value $True
```

## PARAMETERS

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。
> 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、 [Invoke コマンド](../Microsoft.PowerShell.Core/Invoke-Command.md)を使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -除外

このコマンドレットによって操作で除外される項目を文字列配列として指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカード文字を使用できます。 **Exclude** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Filter

**パス** パラメーターを修飾するフィルターを指定します。 [ファイルシステム](../Microsoft.PowerShell.Core/About/about_FileSystem_Provider.md)プロバイダーは、フィルターの使用をサポートする唯一のインストール済み PowerShell プロバイダーです。 **ファイルシステム** フィルター言語の構文については、 [about_Wildcards](../Microsoft.PowerShell.Core/About/about_Wildcards.md)を参照してください。
フィルターは他のパラメーターよりも効率的です。これは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、コマンドレットがオブジェクトを取得するときに、フィルターが適用されるためです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Force

ユーザーがアクセスできない項目のプロパティをコマンドレットに強制的に設定します。
実装はプロバイダーごとに異なります。
詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

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

### -Include

文字列配列として、このコマンドレットによって操作に含まれる項目を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `"*.txt"` ます。 ワイルドカード文字を使用できます。 **Include** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -InputObject

このコマンドレットによって変更されるプロパティを持つオブジェクトを指定します。
オブジェクトが格納されている変数、またはオブジェクトを取得するコマンドを入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: propertyPSObjectPathSet, propertyPSObjectLiteralPathSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: propertyValueLiteralPathSet, propertyPSObjectLiteralPathSet
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

プロパティの名前を指定します。

```yaml
Type: System.String
Parameter Sets: propertyValuePathSet, propertyValueLiteralPathSet
Aliases: PSProperty

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

項目プロパティを表すオブジェクトを返します。
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

### -Path

変更するプロパティを持つ項目のパスを指定します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: propertyValuePathSet, propertyPSObjectPathSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Type

**Type** は、レジストリプロバイダーによってコマンドレットに追加される動的パラメーターです `Set-ItemProperty` 。
このパラメーターは、レジストリドライブでのみ機能します。

このコマンドレットによって追加されるプロパティの型を指定します。
このパラメーターの有効値は、次のとおりです。

- **String**: null で終わる文字列を指定します。 **REG_SZ** と同じです。
- **Expandstring**: 値の取得時に展開される環境変数への展開されていない参照を含む、null で終わる文字列を指定します。 **REG_EXPAND_SZ** と同じです。
- **Binary**: 任意の形式のバイナリデータを指定します。 **REG_BINARY** と同じです。
- **DWord**:32 ビットのバイナリ数値を指定します。 **REG_DWORD** と同じです。
- Strings: 2 つの null 文字で終了する null で終わる文字列の配列を指定 **します。**
  **REG_MULTI_SZ** と同じです。
- **Qword**:64 ビットのバイナリ数値を指定します。 **REG_QWORD** と同じです。
- **不明**: **REG_RESOURCE_LIST** など、サポートされていないレジストリデータ型を示します。

```yaml
Type: Microsoft.Win32.RegistryValueKind
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Value

プロパティの値を指定します。

```yaml
Type: System.Object
Parameter Sets: propertyValuePathSet, propertyValueLiteralPathSet
Aliases:

Required: True
Position: 2
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

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用してオブジェクトをこのコマンドレットに渡します。

## 出力

### なし、システム管理. PSCustomObject

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、変更された項目を表す **pscustomobject** とその新しいプロパティ値を生成します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

`Set-ItemProperty` は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Clear-ItemProperty](Clear-ItemProperty.md)

[Copy-ItemProperty](Copy-ItemProperty.md)

[Get-ItemProperty](Get-ItemProperty.md)

[Move-ItemProperty](Move-ItemProperty.md)

[New-ItemProperty](New-ItemProperty.md)

[Remove-ItemProperty](Remove-ItemProperty.md)

[Rename-ItemProperty](Rename-ItemProperty.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

