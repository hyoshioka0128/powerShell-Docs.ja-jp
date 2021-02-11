---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/07/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/get-verb?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Verb
ms.openlocfilehash: 6aba1c87a5d711cbfe84f9f6f6d1051acbcd524a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600412"
---
# Get-Verb

## 概要
承認された PowerShell 動詞を取得します。

## SYNTAX

```
Get-Verb [[-Verb] <String[]>] [[-Group] <String[]>] [<CommonParameters>]
```

## Description

関数は、 `Get-Verb` PowerShell コマンドでの使用が承認されている動詞を取得します。

PowerShell コマンドレットと関数名の形式を指定し、承認された動詞を含めることをお勧めし `Verb-Noun` ます。 この方法では、コマンド名の一貫性と予測可能性が向上し、使いやすくなります。

未承認の動詞を使用するコマンドは、PowerShell でも実行できます。 ただし、承認されていない動詞を含むモジュールを名前にインポートすると、コマンドによって `Import-Module` 警告メッセージが表示されます。

> [!NOTE]
> が返す動詞リストが `Get-Verb` 完全ではない可能性があります。 承認済みの PowerShell 動詞とその説明を更新した一覧については、「Microsoft Docs の承認された [動詞](../../docs-conceptual/developer/cmdlet/approved-verbs-for-windows-powershell-commands.md) 」を参照してください。

## 例

### 例 1-すべての動詞の一覧を取得する

```powershell
Get-Verb
```

### 例 2-"un" で始まる承認された動詞の一覧を取得する

```powershell
Get-Verb un*
```

```Output
Verb       AliasPrefix Group     Description
----       ----------- -----     -----------
Undo       un          Common    Sets a resource to its previous state
Unlock     uk          Common    Releases a resource that was locked
Unpublish  ub          Data      Makes a resource unavailable to others
Uninstall  us          Lifecycle Removes a resource from an indicated location
Unregister ur          Lifecycle Removes the entry for a resource from a repository
Unblock    ul          Security  Removes restrictions to a resource
Unprotect  up          Security  Removes safeguards from a resource that were added to prevent it from attack or loss
```

### 例 3-セキュリティグループ内のすべての承認された動詞を取得する

```powershell
Get-Verb -Group Security
```

```Output
Verb      AliasPrefix Group    Description
----      ----------- -----    -----------
Block     bl          Security Restricts access to a resource
Grant     gr          Security Allows access to a resource
Protect   pt          Security Safeguards a resource from attack or loss
Revoke    rk          Security Specifies an action that does not allow access to a resource
Unblock   ul          Security Removes restrictions to a resource
Unprotect up          Security Removes safeguards from a resource that were added to prevent it from attack or loss
```

### 例 4-許可されていない動詞を持つモジュール内のすべてのコマンドを検索する

```powershell
Get-Command -Module Microsoft.PowerShell.Utility | Where-Object Verb -NotIn (Get-Verb).Verb
```

```Output
CommandType     Name            Version    Source
-----------     ----            -------    ------
Cmdlet          Sort-Object     3.1.0.0    Microsoft.PowerShell.Utility
Cmdlet          Tee-Object      3.1.0.0    Microsoft.PowerShell.Utility
```

## PARAMETERS

### -動詞

指定した動詞のみを取得します。 動詞の名前または名前パターンを入力します。 ワイルドカードを指定できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:
Accepted values: Common, Communications, Data, Diagnostic, Lifecycle, Other, Security

Required: False
Position: 1
Default value: All groups
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -Group

指定されたグループのみを取得します。 グループの名前を入力します。 ワイルドカードは使用できません。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: All verbs
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

## 出力

### VerbInfo (システム管理)

## 注

PowerShell 動詞は、最も一般的な使用方法に基づいてグループに割り当てられます。 グループは動詞の検索と比較を簡単にするように設計されています。使用を制限するためではありません。 あらゆる種類のコマンドにあらゆる承認された動詞を使用できます。

各 PowerShell 動詞は、次のいずれかのグループに割り当てられます。

- 共通: Add など、ほぼすべてのコマンドレットに適用できる汎用アクションを定義します。
- 通信: 接続などの通信に適用されるアクションを定義します。
- データ: バックアップなどのデータ処理に適用されるアクションを定義します。
- 診断: デバッグなどの診断に適用されるアクションを定義します。
- ライフサイクル: 完了など、コマンドレットのライフサイクルに適用されるアクションを定義します。
- セキュリティ: Revoke など、セキュリティに適用されるアクションを定義します。
- その他: 他の種類のアクションを定義します。

やなど、PowerShell と共にインストールされるコマンドレットの中には、 `Tee-Object` 未承認の動詞を使用するものがあり `Where-Object` ます。 これらのコマンドレットは履歴例外であり、その動詞は **予約済み** として分類されます。

## 関連リンク

[Import-Module](../microsoft.powershell.core/import-module.md)

