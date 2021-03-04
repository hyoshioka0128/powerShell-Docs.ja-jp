---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 03/02/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/read-host?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Read-Host
ms.openlocfilehash: 1c799a5b0f9041d285ce0e83a98582d6888c607e
ms.sourcegitcommit: 1dfd5554b70c7e8f4e3df19e29c384a9c0a4b227
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/03/2021
ms.locfileid: "101685293"
---
# Read-Host

## 概要
コンソールからの入力行を読み取ります。

## SYNTAX

### AsString (既定値)

```
Read-Host [[-Prompt] <Object>] [-MaskInput] [<CommonParameters>]
```

### AsSecureString

```
Read-Host [[-Prompt] <Object>] [-AsSecureString] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Read-Host` コンソールから入力行を読み取ります。 ユーザーに入力を求めるために使用できます。 セキュリティで保護された文字列として入力を保存できるため、このコマンドレットを使用して、パスワードなどのセキュリティで保護されたデータと共有データなどの入力をユーザーに求めることができます。

> [!NOTE]
> `Read-Host` では、ユーザーからの入力として許容できる文字数は1022文字に制限されています。

## 例

### 例 1: コンソールの入力を変数に保存する

この例では、プロンプトとして "age:" という文字列が表示されます。 値を入力し、Enter キーを押すと、値は変数に格納され `$Age` ます。

```powershell
$Age = Read-Host "Please enter your age"
```

### 例 2: コンソール入力をセキュリティで保護された文字列として保存する

この例では、プロンプトとして "Enter a Password:" という文字列が表示されます。 値が入力されると、 `*` 入力の代わりにアスタリスク () がコンソールに表示されます。 Enter キーを押すと、値は変数に **SecureString** オブジェクトとして格納され `$pwd_secure_string` ます。

```powershell
$pwd_secure_string = Read-Host "Enter a Password" -AsSecureString
```

### 例 3: 入力をプレーンテキスト文字列としてマスクする

この例では、プロンプトとして "Enter a Password:" という文字列が表示されます。 値が入力されると、 `*` 入力の代わりにアスタリスク () がコンソールに表示されます。 Enter キーを押すと、値はプレーンテキスト **文字列** オブジェクトとして変数に格納され `$pwd_string` ます。

```powershell
$pwd_string = Read-Host "Enter a Password" -MaskInput
```

## PARAMETERS

### -AsSecureString

コマンドレットによって、 `*` ユーザーが入力として入力した文字の代わりにアスタリスク () が表示されることを示します。 このパラメーターを使用すると、 `Read-Host` コマンドレットの出力は **SecureString** オブジェクト (**SecureString**) になります。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AsSecureString
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaskInput

コマンドレットによって、 `*` ユーザーが入力として入力した文字の代わりにアスタリスク () が表示されることを示します。 このパラメーターを使用すると、 `Read-Host` コマンドレットの出力は **String** オブジェクトになります。
これにより、 **SecureString** の代わりにプレーンテキストとして返されるパスワードの入力を安全に求めることができます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AsString
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Prompt

プロンプトのテキストを指定します。 文字列を入力します。 文字列にスペースが含まれる場合は、二重引用符で囲みます。 入力した `:` テキストに、PowerShell によってコロン () が追加されます。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### System.string、または SecureString

**AsSecureString** パラメーターが使用されている場合、は `Read-Host` **SecureString** を返します。 それ以外の場合、文字列を返します。

## 注

## 関連リンク

[Clear-Host](../microsoft.powershell.core/clear-host.md)

[Get-Host](Get-Host.md)

[Write-Host](Write-Host.md)

[ConvertFrom-SecureString](../Microsoft.PowerShell.Security/ConvertFrom-SecureString.md)
