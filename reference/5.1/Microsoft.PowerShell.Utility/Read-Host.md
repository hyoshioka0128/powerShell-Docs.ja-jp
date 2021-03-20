---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 03/18/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/read-host?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Read-Host
ms.openlocfilehash: 7f8c8a71657d1d00beb4c6e22159fb2b4ad5dce4
ms.sourcegitcommit: 16a02ae47d1a85b01692101aa0aa6e91e1ba398e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/20/2021
ms.locfileid: "104726368"
---
# Read-Host

## 概要
コンソールからの入力行を読み取ります。

## SYNTAX

```
Read-Host [[-Prompt] <Object>] [-AsSecureString] [<CommonParameters>]
```

## Description

`Read-Host`コマンドレットは、コンソール (stdin) からの入力行を読み取ります。 ユーザーに入力を求めるために使用できます。 セキュリティで保護された文字列として入力を保存できるため、このコマンドレットを使用して、パスワードなどのセキュリティで保護されたデータをユーザーに要求することができます。

> [!NOTE]
> `Read-Host` では、ユーザーからの入力として許容できる文字数は8190文字に制限されています。

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

## PARAMETERS

### -AsSecureString

コマンドレットによって、 `*` ユーザーが入力として入力した文字の代わりにアスタリスク () が表示されることを示します。 このパラメーターを使用すると、 `Read-Host` コマンドレットの出力は **SecureString** オブジェクト (**SecureString**) になります。

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

このコマンドレットは、PowerShell パイプラインからの入力を受け入れません。

## 出力

### System.string、または SecureString

**AsSecureString** パラメーターが使用されている場合、は `Read-Host` **SecureString** を返します。 それ以外の場合、文字列を返します。

## 注

このコマンドレットは、ホストプロセスの stdin ストリームからの読み取りのみを行います。 通常、stdin ストリームは、ホストコンソールのキーボードに接続されています。

## 関連リンク

[Clear-Host](../microsoft.powershell.core/clear-host.md)

[Get-Host](Get-Host.md)

[Write-Host](Write-Host.md)

[ConvertFrom-SecureString](../Microsoft.PowerShell.Security/ConvertFrom-SecureString.md)
