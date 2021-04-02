---
external help file: Microsoft.Powershell.LocalAccounts.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.LocalAccounts
ms.date: 02/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.localaccounts/get-localuser?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-LocalUser
ms.openlocfilehash: 2b1831a854c4c61d4c4631ae475a59d8240a926b
ms.sourcegitcommit: 4d6ed6f7d747a9bbb3fcfcf6c981c5aa8a973a08
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2021
ms.locfileid: "106072278"
---
# Get-LocalUser

## 概要
ローカルユーザーアカウントを取得します。

## SYNTAX

### 既定値 (既定値)

```
Get-LocalUser [[-Name] <String[]>] [<CommonParameters>]
```

### SecurityIdentifier

```
Get-LocalUser [[-SID] <SecurityIdentifier[]>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Get-LocalUser` ローカルユーザーアカウントを取得します。 このコマンドレットは、既定の組み込みユーザーアカウント、作成したローカルユーザーアカウント、および Microsoft アカウントに接続したローカルアカウントを取得します。

> [!NOTE]
> 64ビットシステムでは、32ビットの PowerShell では、Microsoft. PowerShell. LocalAccounts モジュールは使用できません。

## 例

### 例 1: 名前を使用してアカウントを取得する

この例では、AdminContoso02 という名前のユーザーアカウントを取得します。

```powershell
Get-LocalUser -Name "AdminContoso02"
```

```Output
Name             Enabled Description
----             ------- -----------
AdminContoso02   True    Description of this account.
```

### 例 2: Microsoft アカウントに接続されているアカウントを取得する

この例では、Microsoft アカウントに接続されているユーザーアカウントを取得します。 この例では、Outlook.com のアカウントのユーザー名にプレースホルダー値を使用します。

```powershell
Get-LocalUser -Name "MicrosoftAccount\username@Outlook.com"
```

```Output
Name                                    Enabled  Description
----                                    -------  -----------
MicrosoftAccount\username@outlook.com  True     Description of this account.
```

### 例 3: 指定された SID を持つアカウントを取得する

この例では、指定された SID を持つローカルユーザーアカウントを取得します。

```powershell
Get-LocalUser -SID S-1-5-21-9526073513-1762370368-3942940353-500
```

```Output
Name          Enabled Description
----          ------- -----------
Administrator True    Built-in account for administering the computer/domain
```

## PARAMETERS

### -Name

このコマンドレットが取得するユーザーアカウントの名前の配列を指定します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -SID

このコマンドレットが取得するユーザーアカウントのセキュリティ Id (Sid) の配列を指定します。

```yaml
Type: System.Security.Principal.SecurityIdentifier[]
Parameter Sets: SecurityIdentifier
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.string, SecurityIdentifier (システムのセキュリティ)

このコマンドレットには、文字列または SID をパイプすることができます。

## 出力

### SecurityAccountsManager. LocalUser [] です。

このコマンドレットは、ローカルユーザーアカウントを返します。

## 注

**LocalUser**、 **LocalGroup**、および **localprincipal** オブジェクトの **principalsource** プロパティは、オブジェクトのソースを記述します。 考えられるソースは次のとおりです。

- ローカル
- Active Directory
- Azure Active Directory グループ
- Microsoft アカウント

**Principalsource** は、windows 10、windows Server 2016、およびそれ以降のバージョンの windows オペレーティングシステムでのみサポートされています。 以前のバージョンの場合、プロパティは空白になります。

## 関連リンク

[Disable-LocalUser](Disable-LocalUser.md)

[Enable-LocalUser](Enable-LocalUser.md)

[New-LocalUser](New-LocalUser.md)

[Remove-LocalUser](Remove-LocalUser.md)

[Rename-LocalUser](Rename-LocalUser.md)

[Set-LocalUser](Set-LocalUser.md)
