---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 05/14/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/remove-itemproperty?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-ItemProperty
ms.openlocfilehash: 870d8e9797ff2cfce14034706f704c0e1c954fc2
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602414"
---
# Remove-ItemProperty

## 概要
項目からプロパティとその値を削除します。

## SYNTAX

### パス (既定値)

```
Remove-ItemProperty [-Path] <String[]> [-Name] <String[]> [-Force] [-Filter <String>] [-Include <String[]>]
 [-Exclude <String[]>] [-Credential <PSCredential>] [-InformationAction <ActionPreference>]
 [-InformationVariable <String>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### LiteralPath

```
Remove-ItemProperty -LiteralPath <String[]> [-Name] <String[]> [-Force] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Remove-ItemProperty`コマンドレットは、項目からプロパティとその値を削除します。
これを使用して、レジストリ値とその中に格納されているデータを削除することができます。

## 例

### 例 1: レジストリ値を削除する

このコマンドは、レジストリキーの "Smpproperty" サブキーから、"SmpProperty" レジストリ値とそのデータを削除し `HKEY_LOCAL_MACHINE\Software` ます。

```powershell
Remove-ItemProperty -Path "HKLM:\Software\SmpApplication" -Name "SmpProperty"
```

コマンドはファイルシステムドライブ () から発行されるので、 `PS C:\>` ドライブ、 `HKLM:` 、および "Software" キーを含む "SmpApplication" サブキーの完全修飾パスが含まれます。

### 例 2: HKCU の場所からレジストリ値を削除する

これらのコマンドは、"HKEY_CURRENT_USER\Software\MyCompany" の "MyApp" サブキーから、"Options" レジストリ値とそのデータを削除します。

```
PS C:\> Set-Location HKCU:\Software\MyCompany\MyApp
PS HKCU:\Software\MyCompany\MyApp> Remove-ItemProperty -Path . -Name "Options" -Confirm
```

最初のコマンドは、コマンドレットを使用して、 `Set-Location` 現在の場所を **HKEY_CURRENT_USER** ドライブ ( `HKCU:` ) とサブキーに変更し `Software\MyCompany\MyApp` ます。

2番目のコマンドは、を使用して、" `Remove-ItemProperty` MyApp" サブキーから "Options" レジストリ値とそのデータを削除します。 **パス** が必要であるため、コマンドはドット () を使用し `.` て現在の場所を示します。 **Confirm** パラメーターは、値を削除する前にユーザープロンプトを要求します。

### 例 3: パイプラインを使用してレジストリ値を削除する

このコマンドは、レジストリキーから "NoOfEmployees" レジストリ値とそのデータを削除し `HKLM\Software\MyCompany` ます。

```powershell
Get-Item -Path HKLM:\Software\MyCompany | Remove-ItemProperty -Name NoOfEmployees
```

コマンドは、コマンドレットを使用して、 `Get-Item` レジストリキーを表す項目を取得します。
パイプライン演算子 () を使用して `|` 、オブジェクトをに送信 `Remove-ItemProperty` します。
次に、の **name** パラメーターを使用して、 `Remove-ItemProperty` レジストリ値の名前を指定します。

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

ユーザーがアクセスできないオブジェクトのプロパティをコマンドレットに強制的に削除します。
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

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

削除するプロパティの名前を指定します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PSProperty

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Path

プロパティを削除する項目のパスを指定します。
ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
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

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用してパスを含む文字列をこのコマンドレットに送ることができます。

## 出力

### なし

このコマンドレットによる戻り値はありません。

## 注

- PowerShell レジストリプロバイダーでは、レジストリ値はレジストリキーまたはサブキーのプロパティと見なされます。 これらの値は、 **get-itemproperty** コマンドレットを使用して管理できます。
- `Remove-ItemProperty` は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Get-Item](Get-Item.md)

[Clear-ItemProperty](Clear-ItemProperty.md)

[Copy-ItemProperty](Copy-ItemProperty.md)

[Get-ItemProperty](Get-ItemProperty.md)

[Move-ItemProperty](Move-ItemProperty.md)

[New-ItemProperty](New-ItemProperty.md)

[Remove-Item](Remove-Item.md)

[Rename-ItemProperty](Rename-ItemProperty.md)

[Set-ItemProperty](Set-ItemProperty.md)

[Set-Location](Set-Location.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

