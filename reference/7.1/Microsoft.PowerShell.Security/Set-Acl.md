---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-acl?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-Acl
ms.openlocfilehash: 314734957eea971338ac886ad82e9ce9eeb6a242
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346566"
---
# Set-Acl

## 概要
ファイルやレジストリ キーなど、指定した項目のセキュリティ記述子を変更します。

## SYNTAX

### ByPath (既定値)

```
Set-Acl [-Path] <String[]> [-AclObject] <Object> [-ClearCentralAccessPolicy] [-Passthru] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByInputObject

```
Set-Acl [-InputObject] <PSObject> [-AclObject] <Object> [-Passthru] [-Filter <String>] [-Include <String[]>]
 [-Exclude <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Set-Acl -LiteralPath <String[]> [-AclObject] <Object> [-ClearCentralAccessPolicy] [-Passthru]
 [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Set-Acl`このコマンドレットは、指定した項目 (ファイルやレジストリキーなど) のセキュリティ記述子を、指定したセキュリティ記述子の値と一致するように変更します。

を使用するに `Set-Acl` は、 **Path** または **InputObject** パラメーターを使用して、セキュリティ記述子を変更する項目を識別します。 次に、 **AclObject** または **SecurityDescriptor** パラメーターを使用して、適用する値が含まれるセキュリティ記述子を指定します。 `Set-Acl` 指定されたセキュリティ記述子を適用します。 **AclObject** パラメーターの値をモデルとして使用し、項目のセキュリティ記述子の値を **AclObject** パラメーターの値と一致するように変更します。

## 例

### 例 1: あるファイルから別のファイルにセキュリティ記述子をコピーする

```powershell
$DogACL = Get-Acl -Path "C:\Dog.txt"
Set-Acl -Path "C:\Cat.txt" -AclObject $DogACL
```

これらのコマンドは、Dog.txt ファイルのセキュリティ記述子から Cat.txt ファイルのセキュリティ記述子に値をコピーします。 コマンドが完了すると、Dog.txt ファイルと Cat.txt ファイルのセキュリティ記述子は同一となります。

最初のコマンドは、 `Get-Acl` コマンドレットを使用して、Dog.txt ファイルのセキュリティ記述子を取得します。
代入演算子 () は、 `=` $DogACL 変数の値にセキュリティ記述子を格納します。

2番目のコマンドは、を使用して `Set-Acl` Cat.txt の ACL の値をの値に変更し `$DogACL` ます。

**Path** パラメーターの値は、Cat.txt ファイルへのパスです。 **Aclobject** パラメーターの値は、モデル acl (この場合は、変数に保存されている Dog.txt の acl) です `$DogACL` 。

### 例 2: パイプライン演算子を使用して記述子を渡す

```powershell
Get-Acl -Path "C:\Dog.txt" | Set-Acl -Path "C:\Cat.txt"
```

このコマンドは、前の例のコマンドとほぼ同じですが、パイプライン演算子 () を使用してコマンドからコマンドにセキュリティ記述子を送信する点が異なり `|` `Get-Acl` `Set-Acl` ます。

最初のコマンドは、 `Get-Acl` コマンドレットを使用して、Dog.txt ファイルのセキュリティ記述子を取得します。 パイプライン演算子 () は、 `|` Dog.txt セキュリティ記述子を表すオブジェクトを `Set-Acl` コマンドレットに渡します。

2番目のコマンドは、を使用して `Set-Acl` Dog.txt のセキュリティ記述子を Cat.txt に適用します。
コマンドが完了すると、Dog.txt ファイルと Cat.txt ファイルの ACL は同一となります。

### 例 3: 複数のファイルにセキュリティ記述子を適用する

```powershell
$NewAcl = Get-Acl File0.txt
Get-ChildItem -Path "C:\temp" -Recurse -Include "*.txt" -Force | Set-Acl -AclObject $NewAcl
```

これらのコマンドは、File0.txt ファイル内のセキュリティ記述子を、 `C:\Temp` ディレクトリとそのすべてのサブディレクトリ内のすべてのテキストファイルに適用します。

最初のコマンドは、現在のディレクトリ内の File0.txt ファイルのセキュリティ記述子を取得し、代入演算子 () を使用し `=` て変数に格納し `$NewACL` ます。

パイプラインの最初のコマンドは、Get-ChildItem コマンドレットを使用して、ディレクトリ内のすべてのテキストファイルを取得し `C:\Temp` ます。 **再帰** パラメーターは、のすべてのサブディレクトリに対してコマンドを拡張し `C:\temp` ます。 **Include** パラメーターは、ファイル名拡張子を持つファイルに取得されるファイルを制限し `.txt` ます。 **Force** パラメーターは、隠しファイルを取得します。このパラメーターが指定されない場合、隠しファイルは除外されます (を使用することはできません `c:\temp\*.txt` 。これは、 **再帰** パラメーターは、ファイルではなくディレクトリで動作するためです)。

パイプライン演算子 () は、 `|` 取得したファイルを表すオブジェクトをコマンドレットに送信します。これにより、 `Set-Acl` **aclobject** パラメーターのセキュリティ記述子がパイプライン内のすべてのファイルに適用されます。

実際には、複数の項目に影響を与える可能性のあるすべてのコマンドで **WhatIf** パラメーターを使用することをお勧めし `Set-Acl` ます。 この場合、パイプラインの2番目のコマンドはになり `Set-Acl -AclObject $NewAcl -WhatIf` ます。 このコマンドは、コマンドによって影響を受けるファイルを一覧表示します。 結果を確認した後、 **WhatIf** パラメーターを指定せずにコマンドを再度実行できます。

### 例 4: 継承を無効にし、継承されたアクセス規則を保持する

```powershell
$NewAcl = Get-Acl -Path "C:\Pets\Dog.txt"
$isProtected = $true
$preserveInheritance = $true
$NewAcl.SetAccessRuleProtection($isProtected, $preserveInheritance)
Set-Acl -Path "C:\Pets\Dog.txt" -AclObject $NewAcl
```

これらのコマンドは、既存の継承されたアクセス規則を保持したまま、親フォルダーからのアクセス継承を無効にします。

最初のコマンドは、 `Get-Acl` コマンドレットを使用して、Dog.txt ファイルのセキュリティ記述子を取得します。

次に、継承されたアクセス規則を明示的なアクセス規則に変換するための変数が作成されます。 このに関連付けられているアクセス規則を継承から保護するには、 `$isProtected` 変数をに設定 `$true` します。継承を許可するには、 `$isProtected` をに設定し `$false` ます。 詳細については、「 [アクセス規則の保護の設定](/dotnet/api/system.security.accesscontrol.objectsecurity.setaccessruleprotection)」を参照してください。
継承された `$preserveInheritance` アクセス規則を保持するためにに設定された変数 `$true` 。継承されたアクセス規則を削除する場合は false。 次に、 **Setaccessruleprotection ()** メソッドを使用して、アクセス規則の保護を更新します。

最後のコマンドは、を使用して、 `Set-Acl` のセキュリティ記述子を Dog.txt に適用します。 コマンドが完了すると、ペットフォルダーから継承された Dog.txt の Acl が Dog.txt に直接適用され、ペットに追加された新しいアクセスポリシーによって Dog.txt へのアクセスが変更されることはありません。

### 例 5: 管理者にファイルのフルコントロールを付与する

```powershell
$NewAcl = Get-Acl -Path "C:\Pets\Dog.txt"
# Set properties
$identity = "BUILTIN\Administrators"
$fileSystemRights = "FullControl"
$type = "Allow"
# Create new rule
$fileSystemAccessRuleArgumentList = $identity, $fileSystemRights, $type
$fileSystemAccessRule = New-Object -TypeName System.Security.AccessControl.FileSystemAccessRule -ArgumentList $fileSystemAccessRuleArgumentList
# Apply new rule
$NewAcl.SetAccessRule($fileSystemAccessRule)
Set-Acl -Path "C:\Pets\Dog.txt" -AclObject $NewAcl
```

このコマンドを実行すると、 **BUILTIN\Administrators** グループに Dog.txt ファイルのフルコントロールが与えられます。

最初のコマンドは、 `Get-Acl` コマンドレットを使用して、Dog.txt ファイルのセキュリティ記述子を取得します。

次の変数が作成され、Dog.txt ファイルのフルコントロールを **BUILTIN\Administrators** グループに付与します。 `$identity`[ユーザーアカウント](/dotnet/api/system.security.accesscontrol.filesystemaccessrule.-ctor)の名前に設定された変数。 `$fileSystemRights`変数が FullControl に設定されています。また、アクセス規則に関連付けられている操作の種類を指定する[filesystemrights](/dotnet/api/system.security.accesscontrol.filesystemrights)値のいずれかを指定することもできます。 `$type`操作を許可するか拒否するかを指定する変数が "allow" に設定されています。 `$fileSystemAccessRuleArgumentList`変数は、新しい **Filesystemaccessrule** オブジェクトを作成するときに渡される引数リストです。 次に、新しい **Filesystemaccessrule** オブジェクトが作成され、 **Filesystemaccessrule** オブジェクトが **setaccessrule ()** メソッドに渡され、新しいアクセス規則が追加されます。

最後のコマンドは、を使用して、 `Set-Acl` のセキュリティ記述子を Dog.txt に適用します。 コマンドが完了すると、 **BUILTIN\Administrators** グループは Dog.txt を完全に制御できるようになります。

## PARAMETERS

### -AclObject

目的のプロパティの値を備えた　ACL を指定します。 `Set-Acl` 指定したセキュリティオブジェクトの値と一致するように、 **Path** または **InputObject** パラメーターで指定された項目の ACL を変更します。

コマンドの出力を `Get-Acl` 変数に保存し、 **Aclobject** パラメーターを使用して変数を渡すことも、コマンドを入力することもでき `Get-Acl` ます。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ClearCentralAccessPolicy

指定された項目から、集約型アクセス ポリシーを削除します。

Windows Server 2012 以降では、管理者は Active Directory とグループポリシーを使用して、ユーザーとグループの集約型アクセスポリシーを設定できます。 詳細については、「 [Dynamic Access Control: シナリオの概要](/windows-server/identity/solution-guides/dynamic-access-control--scenario-overview)」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ByPath, ByLiteralPath
Aliases:

Required: False
Position: Named
Default value: False
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

プロバイダーの形式や言語でフィルターを指定します。 このパラメーターの値は、 **Path** パラメーターを修飾します。 ワイルドカードを使用できるかどうかなど、フィルターの構文はプロバイダーによって異なります。 フィルターは他のパラメーターよりも効率的です。これは、オブジェクトを取得した後に PowerShell がオブジェクトをフィルター処理するのではなく、オブジェクトを取得するときにプロバイダーが適用するためです。

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

指定した項目だけを変更します。 このパラメーターの値は、 **Path** パラメーターを修飾します。
パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカードを使用できます。

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

指定されたオブジェクトのセキュリティ記述子を変更します。 オブジェクトが格納されている変数、またはオブジェクトを取得するコマンドを入力します。

パイプを使用して変更するオブジェクトをにパイプすることはできません `Set-Acl` 。 代わりに、 **InputObject** パラメーターをコマンド内で明示的に使用します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: ByInputObject
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -LiteralPath

指定した項目のセキュリティ記述子を変更します。 **Path** と異なり、 **LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符 () で囲み `'` ます。
単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: ByLiteralPath
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Passthru

変更されたセキュリティ記述子を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

指定した項目のセキュリティ記述子を変更します。 ファイルまたはレジストリ キーへのパスなどの項目へのパスを入力します。 ワイルドカードを使用できます。

`Set-Acl`( **Aclobject** または **SecurityDescriptor** パラメーターを使用するか、セキュリティオブジェクトを Get-Acl からに渡すことによって) セキュリティオブジェクトをに渡し `Set-Acl` 、 **path** パラメーター (name と value) を省略すると、は `Set-Acl` セキュリティオブジェクトに含まれているパスを使用します。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
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

### Accesscontrol-namespace、Accesscontrol-namespace、...... CommonSecurityDescriptor

パイプを使用して、ACL オブジェクトまたはセキュリティ記述子をにパイプすることができ `Set-Acl` ます。

## 出力

### Accesscontrol-namespace. System.security.accesscontrol.filesecurity

既定で `Set-Acl` は、は出力を生成しません。 ただし、 **Passthru** パラメーターを使用すると、セキュリティ オブジェクトを生成します。 セキュリティ オブジェクトの種類は、項目の種類に応じて異なります。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

`Set-Acl`コマンドレットは、PowerShell ファイルシステムとレジストリプロバイダーによってサポートされています。 そのため、そのコマンドレットを使用して、ファイル、ディレクトリ、およびレジストリ キーのセキュリティ記述子を変更できます。

## 関連リンク

[Get-Acl](Get-Acl.md)

[FileSystemAccessRule](/dotnet/api/system.security.accesscontrol.filesystemaccessrule.-ctor)

[ObjectSecurity. SetAccessRuleProtection](/dotnet/api/system.security.accesscontrol.objectsecurity.setaccessruleprotection)

[FileSystemRights](/dotnet/api/system.security.accesscontrol.filesystemrights)
