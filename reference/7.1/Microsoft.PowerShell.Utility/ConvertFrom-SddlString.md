---
external help file: Microsoft.PowerShell.Utility-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/convertfrom-sddlstring?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-SddlString
ms.openlocfilehash: 7ec2c3025f62a64cd24298c0749d40fa5eff4904
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346090"
---
# ConvertFrom-SddlString

## 概要
SDDL 文字列をカスタムオブジェクトに変換します。

## SYNTAX

### すべて

```
ConvertFrom-SddlString [-Sddl] <String> [-Type <AccessRightTypeNames>] [<CommonParameters>]
```

## Description

`ConvertFrom-SddlString`コマンドレットは、セキュリティ記述子定義言語の文字列を、Owner、Group、DiscretionaryAcl、SystemAcl、RawDescriptor の各プロパティを持つカスタム **Pscustomobject** オブジェクトに変換します。

Owner、Group、DiscretionaryAcl、および SystemAcl の各プロパティには、SDDL 文字列で指定されたアクセス権の読み取り可能なテキスト表現が含まれています。

このコマンドレットは、PowerShell 5.0 で導入されました。

## 例

### 例 1: ファイルシステムアクセス権 SDDL を PSCustomObject 変換する

```powershell
$acl = Get-Acl -Path C:\Windows
ConvertFrom-SddlString -Sddl $acl.Sddl
```

最初のコマンドは、 `Get-Acl` コマンドレットを使用して C:\Windows フォルダーのセキュリティ記述子を取得し、変数に保存します。

2番目のコマンドは、コマンドレットを使用して、 `ConvertFrom-SddlString` sddl 文字列のテキスト表現を取得します。これは、セキュリティ記述子を表すオブジェクトの sddl プロパティに含まれています。

### 例 2: レジストリアクセス権 SDDL を PSCustomObject 変換する

```powershell
$acl = Get-Acl HKLM:\SOFTWARE\Microsoft\
ConvertFrom-SddlString -Sddl $acl.Sddl -Type RegistryRights
```

最初のコマンドは、コマンドレットを使用して、 `Get-Acl` HKLM:\ SOFTWARE\Microsoft\ key のセキュリティ記述子を取得し、変数に保存します。

2番目のコマンドは、コマンドレットを使用して、 `ConvertFrom-SddlString` sddl 文字列のテキスト表現を取得します。これは、セキュリティ記述子を表すオブジェクトの sddl プロパティに含まれています。

この例では、パラメーターを使用して、 `-Type` SDDL 文字列がレジストリセキュリティ記述子を表すことを指定しています。

### 例 3: パラメーターを指定せずに ConvertFrom-SddlString を使用して、レジストリアクセス権 SDDL を PSCustomObject 変換する `-Type`

```powershell
$acl = Get-Acl -Path HKLM:\SOFTWARE\Microsoft\

ConvertFrom-SddlString -Sddl $acl.Sddl | Foreach-Object {$_.DiscretionaryAcl[0]}

BUILTIN\Administrators: AccessAllowed (ChangePermissions, CreateDirectories, Delete, ExecuteKey, FullControl, GenericExecute, GenericWrite, ListDirectory, ReadExtendedAttributes, ReadPermissions, TakeOwnership, Traverse, WriteData, WriteExtendedAttributes, WriteKey)

ConvertFrom-SddlString -Sddl $acl.Sddl -Type RegistryRights | Foreach-Object {$_.DiscretionaryAcl[0]}

BUILTIN\Administrators: AccessAllowed (ChangePermissions, CreateLink, CreateSubKey, Delete, EnumerateSubKeys, ExecuteKey, FullControl, GenericExecute, GenericWrite, Notify, QueryValues, ReadPermissions, SetValue, TakeOwnership, WriteKey)
```

最初のコマンドは、コマンドレットを使用して、 `Get-Acl` HKLM:\ SOFTWARE\Microsoft\ key のセキュリティ記述子を取得し、変数に保存します。

2番目のコマンドは、コマンドレットを使用して、 `ConvertFrom-SddlString` sddl 文字列のテキスト表現を取得します。これは、セキュリティ記述子を表すオブジェクトの sddl プロパティに含まれています。

パラメーターを使用しない `-Type` ので、表示されるアクセス権はファイルシステム用です。

3番目のコマンドは、パラメーターを指定 `ConvertFrom-SddlString` してコマンドレットを使用する `-Type` ため、返されるアクセス権はレジストリ用です。

## PARAMETERS

### -Sddl

SDDL 構文のセキュリティ記述子を表す文字列を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Type

SDDL 文字列が表す権限の種類を指定します。

このパラメーターの有効値は、次のとおりです。

- FileSystemRights
- RegistryRights
- ActiveDirectoryRights
- MutexRights
- SemaphoreRights
- CryptoKeyRights
- EventWaitHandleRights

既定では、コマンドレットはファイルシステム権限を使用します。

CryptoKeyRights と ActiveDirectoryRights は、PowerShell Core ではサポートされていません。

```yaml
Type: Microsoft.PowerShell.Commands.ConvertFromSddlStringCommand+AccessRightTypeNames
Parameter Sets: (All)
Aliases:
Accepted values: FileSystemRights, RegistryRights, ActiveDirectoryRights, MutexRights, SemaphoreRights, CryptoKeyRights, EventWaitHandleRights

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

SDDL 文字列をにパイプすることができ `ConvertFrom-SddlString` ます。

## 出力

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク

[セキュリティ記述子定義言語](/windows/win32/secauthz/security-descriptor-definition-language)
