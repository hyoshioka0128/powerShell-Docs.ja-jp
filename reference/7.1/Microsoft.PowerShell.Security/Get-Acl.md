---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 03/25/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-acl?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Acl
ms.openlocfilehash: 118c3e563743cee03dc7a75ca68e0979c1522f07
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347076"
---
# Get-Acl

## 概要
ファイル、レジストリ キーなどのリソースのセキュリティ記述子を取得します。

## SYNTAX

### ByPath (既定値)

```
Get-Acl [[-Path] <String[]>] [-Audit] [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>]
 [<CommonParameters>]
```

### ByInputObject

```
Get-Acl -InputObject <PSObject> [-Audit] [-Filter <String>] [-Include <String[]>]
 [-Exclude <String[]>] [<CommonParameters>]
```

### ByLiteralPath

```
Get-Acl [-LiteralPath <String[]>] [-Audit] [-Filter <String>] [-Include <String[]>]
 [-Exclude <String[]>] [<CommonParameters>]
```

## Description

`Get-Acl`コマンドレットは、ファイルまたはリソースのセキュリティ記述子を表すオブジェクトを取得します。 セキュリティ記述子には、リソースのアクセス制御リスト (ACL) が含まれます。 ACL は、リソースにアクセスするユーザーおよびユーザー グループに割り当てられるアクセス許可を指定します。

Windows PowerShell 3.0 以降では、の **InputObject** パラメーターを使用して、 `Get-Acl` パスを持たないオブジェクトのセキュリティ記述子を取得できます。

## 例

### 例 1-フォルダーの ACL を取得する

この例では、ディレクトリのセキュリティ記述子を取得し `C:\Windows` ます。

```powershell
Get-Acl C:\Windows
```

### 例 2-ワイルドカードを使用してフォルダーの ACL を取得する

この例では、 `.log` `C:\Windows` 名前がで始まるディレクトリ内のすべてのファイルの PowerShell パスと SDDL を取得し `s` ます。

```powershell
Get-Acl C:\Windows\s*.log | Format-List -Property PSPath, Sddl
```

このコマンドは、コマンドレットを使用して、 `Get-Acl` 各ログファイルのセキュリティ記述子を表すオブジェクトを取得します。 パイプライン演算子 () を使用して、 `|` 結果をコマンドレットに送信し `Format-List` ます。 このコマンドは、の **Property** パラメーターを使用して `Format-List` 、各セキュリティ記述子オブジェクトの **Pspath** プロパティと **SDDL** プロパティのみを表示します。

テーブルでは長い値が切り捨てられるため、多くの場合、一覧は PowerShell で使用されます。

**SDDL** 値は、システム管理者に有用です。それは、これらの値がセキュリティ記述子内にすべての情報を含む単純なテキスト文字列であるためです。 そのため、これらの値は簡単に渡して保存でき、必要なときに解析できます。

### 例 3-ACL の監査エントリの数を取得する

この例では、 `.log` `C:\Windows` 名前がで始まるディレクトリ内のファイルのセキュリティ記述子を取得し `s` ます。

```powershell
Get-Acl C:\Windows\s*.log -Audit | ForEach-Object { $_.Audit.Count }
```

ここでは、 **Audit** パラメーターを使用して、セキュリティ記述子の SACL から監査レコードを取得します。
次に、コマンドレットを使用して、 `ForEach-Object` 各ファイルに関連付けられている監査レコードの数をカウントします。 結果として、各ログ ファイルの監査レコードの数を表す番号のリストが得られます。

### 例 4-レジストリキーの ACL を取得する

この例では、コマンドレットを使用して、 `Get-Acl` レジストリの Control サブキー () のセキュリティ記述子を取得し `HKLM:\SYSTEM\CurrentControlSet\Control` ます。

```powershell
Get-Acl -Path HKLM:\System\CurrentControlSet\Control | Format-List
```

**Path** パラメーターは、Control サブキーを指定します。 パイプライン演算子 () は、 `|` コマンドに対して取得するセキュリティ記述子を渡し `Get-Acl` ます。これにより、 `Format-List` セキュリティ記述子のプロパティがリストとして書式設定され、読みやすくなります。

### 例 5- **InputObject** を使用して ACL を取得する

この例では、の **InputObject** パラメーターを使用して、 `Get-Acl` ストレージサブシステムオブジェクトのセキュリティ記述子を取得します。

```powershell
Get-Acl -InputObject (Get-StorageSubSystem -Name S087)
```

## PARAMETERS

### -Audit

システム アクセス制御リスト (SACL) からセキュリティ記述子の監査データを取得します。

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

### -除外

指定した項目を除外します。 このパラメーターの値は、 **Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカードを使用できます。

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

プロバイダーの形式や言語でフィルターを指定します。 このパラメーターの値は、 **Path** パラメーターを修飾します。 ワイルドカードを使用できるかどうかなど、フィルターの構文はプロバイダーによって異なります。 フィルターは他のパラメーターよりも効率的です。これは、オブジェクトを取得した後に PowerShell がオブジェクトをフィルター処理するのではなく、オブジェクトの取得時にプロバイダーによって適用されるためです。

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

### -Include

指定された項目だけを取得します。 このパラメーターの値は、 **Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカードを使用できます。

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

### -Path

リソースへのパスを指定します。 `Get-Acl` パスによって示されるリソースのセキュリティ記述子を取得します。 ワイルドカードを使用できます。 **Path** パラメーターを省略した場合、は `Get-Acl` 現在のディレクトリのセキュリティ記述子を取得します。

パラメーター名 ("Path") は省略可能です。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -InputObject

指定したオブジェクトのセキュリティ記述子を取得します。 オブジェクトが格納されている変数、またはオブジェクトを取得するコマンドを入力します。

パイプを使用してパス以外のオブジェクトをにパイプすることはできません `Get-Acl` 。 代わりに、 **InputObject** パラメーターをコマンド内で明示的に使用します。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: ByInputObject
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath

リソースへのパスを指定します。 **Path** と異なり、 **LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: ByLiteralPath
Aliases: PSPath

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、パスを含む文字列をにパイプすることができ `Get-Acl` ます。

## 出力

### Accesscontrol-namespace、System.security.accesscontrol.filesecurity、Accesscontrol-namespace、Accesscontrol-namespace、およびの各セキュリティについてのセキュリティを強化します。

`Get-Acl` 取得する Acl を表すオブジェクトを返します。 オブジェクトの種類は、ACL の種類に依存します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

既定では、リソース `Get-Acl` () への PowerShell パス、 `<provider>::<resource-path>` リソースの所有者、およびリソースの随意アクセス制御リスト (DACL) のアクセス制御エントリのリスト (配列) が表示されます。 DACL リストは、リソースの所有者によって制御されます。

結果をリストとして書式設定すると ( `Get-Acl | Format-List` )、パス、所有者、アクセスの一覧に加えて、PowerShell では次のプロパティとプロパティ値が表示されます。

- **グループ** : 所有者のセキュリティグループ。
- **Audit** : システムアクセス制御リスト (SACL) 内のエントリのリスト (配列)。 SACL は、Windows が監査レコードを生成するアクセス試行の種類を指定します。
- **Sddl** : セキュリティ記述子定義言語形式の単一のテキスト文字列に表示されるリソースのセキュリティ記述子。 PowerShell では、セキュリティ記述子の **Getsddlform** メソッドを使用して、このデータを取得します。

`Get-Acl`はファイルシステムとレジストリプロバイダーによってサポートされているため、を使用して、ファイル `Get-Acl` やディレクトリなどのファイルシステムオブジェクトの ACL と、レジストリキーやエントリなどのレジストリオブジェクトを表示できます。

## 関連リンク

[Set-Acl](Set-Acl.md)
