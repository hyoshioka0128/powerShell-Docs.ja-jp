---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 05/15/2019
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/set-ciminstance?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-CimInstance
ms.openlocfilehash: 041ef2d5b5541c250b98003099fe58018df155c2
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605168"
---
# Set-CimInstance

## 概要
Cim クラスの ModifyInstance メソッドを呼び出すことによって、cim サーバー上の CIM インスタンスを変更します。

## SYNTAX

### CimInstanceComputerSet (既定値)

```
Set-CimInstance [-ComputerName <String[]>] [-ResourceUri <Uri>] [-OperationTimeoutSec <UInt32>]
 [-InputObject] <CimInstance> [-Property <IDictionary>] [-PassThru] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### CimInstanceSessionSet

```
Set-CimInstance -CimSession <CimSession[]> [-ResourceUri <Uri>] [-OperationTimeoutSec <UInt32>]
 [-InputObject] <CimInstance> [-Property <IDictionary>] [-PassThru] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### QuerySessionSet

```
Set-CimInstance -CimSession <CimSession[]> [-Namespace <String>] [-OperationTimeoutSec <UInt32>]
 [-Query] <String> [-QueryDialect <String>] -Property <IDictionary> [-PassThru] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### QueryComputerSet

```
Set-CimInstance [-ComputerName <String[]>] [-Namespace <String>] [-OperationTimeoutSec <UInt32>]
 [-Query] <String> [-QueryDialect <String>] -Property <IDictionary> [-PassThru] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

このコマンドレットは、CIM サーバー上の CIM インスタンスを変更します。

**InputObject** パラメーターが指定されていない場合、コマンドレットは次のいずれかの方法で動作します。

- **ComputerName** パラメーターも **CimSession** パラメーターも指定しない場合、このコマンドレットはコンポーネントオブジェクトモデル (COM) セッションを使用してローカル Windows Management Instrumentation (WMI) で動作します。
- **Computername** パラメーターまたは **CimSession** パラメーターを指定した場合、このコマンドレットは、 **computername** パラメーターまたは **CIMSESSION** パラメーターで指定された CIM サーバーに対して機能します。

**InputObject** パラメーターが指定されている場合、コマンドレットは次のいずれかの方法で動作します。

- **ComputerName** パラメーターも **CimSession** パラメーターも指定しない場合、このコマンドレットは入力オブジェクトの CIM セッションまたはコンピューター名を使用します。
- **Computername** パラメーターまたは **CimSession** パラメーターのいずれかが指定されている場合、このコマンドレットは **CimSession** パラメーター値または **computername** パラメーター値を使用します。 これはあまり一般的ではありません。

## 例

### 例 1: CIM インスタンスを設定する

この例では、**クエリ** パラメーターを使用して、[**変数の値** のプロパティの値を **abcd** に設定します。 WQL (Windows Management Instrumentation Query Language) クエリと一致するインスタンスを変更できます。

```powershell
Set-CimInstance -Query 'Select * from Win32_Environment where name LIKE "testvar%"' -Property @{VariableValue="abcd"}
```

### 例 2: パイプラインを使用して CIM インスタンスのプロパティを設定する

この例では、コマンドレットを使用して、 **クエリ** パラメーターによってフィルター処理された CIM インスタンスオブジェクトを取得し `Get-CimInstance` ます。 `Set-CimInstance`コマンドレットでは、[**変数** の値の値プロパティの値を **abcd** に変更します。

```powershell
Get-CimInstance -Query 'Select * from Win32_Environment where name LIKE "testvar%"' |
  Set-CimInstance -Property @{VariableValue="abcd"}
```

### 例 3: 入力オブジェクトを使用して CIM インスタンスのプロパティを設定する

```powershell
$x = Get-CimInstance -Query 'Select * from Win32_Environment where Name="testvar"'
Set-CimInstance -InputObject $x -Property @{VariableValue="somevalue"} -PassThru
```

この例では、クエリパラメーターによってフィルター処理された CIM インスタンスオブジェクトを、を使用して変数に取得し、その `$x` `Get-CimInstance` 変数の内容を `Set-CimInstance` コマンドレットに渡します。 `Set-CimInstance` 次に、[ **変数の値** ] プロパティを **値** に変更します。 **Passthru** パラメーターが使用されているため、この例では変更された CIM インスタンスオブジェクトを返します。

### 例 4: CIM インスタンスのプロパティを設定する

この例では、コマンドレットを使用して、 **クエリ** パラメーターに指定されている CIM インスタンスオブジェクトを変数に取得 `$x` `Get-CimInstance` し、変更するオブジェクトの変数 **値** プロパティの値を変更します。 次に、コマンドレットを使用して CIM インスタンスオブジェクトを保存し `Set-CimInstance` ます。
**Passthru** パラメーターが使用されているため、この例では変更された CIM インスタンスオブジェクトを返します。

```powershell
$x = Get-CimInstance -Query 'Select * from Win32_Environment where name="testvar"'
$x.VariableValue = "Change"
Set-CimInstance -CimInstance $x -PassThru
```

### 例 5: WhatIf を使用して変更する CIM インスタンスの一覧を表示する

この例では、一般的なパラメーター **WhatIf** を使用して、変更を実行しないように指定していますが、実行された場合に発生することを出力します。

```powershell
Set-CimInstance -Query 'Select * from Win32_Environment where name LIKE "testvar%"' -Property @{VariableValue="abcd"} -WhatIf
```

### 例 6: ユーザーからの確認後に CIM インスタンスを設定する

この例では、共通パラメーター **Confirm** を使用して、ユーザーからの確認後にのみ変更を行うように指定しています。

```powershell
Set-CimInstance -Query 'Select * from Win32_Environment where name LIKE "testvar%"' -Property @{VariableValue="abcd"} -Confirm
```

### 例 7: 作成した CIM インスタンスを設定する

この例では、コマンドレットを使用して、指定したプロパティを持つ CIM インスタンスを作成 `New-CimInstance` し、その内容を変数に取得し `$x` ます。 次に、変数がコマンドレットに渡されます。これにより、変数 variable `Set-CimInstance` **value** プロパティの値が任意の **値** に変更されます。
**Passthru** パラメーターが使用されているため、この例では変更された CIM インスタンスオブジェクトを返します。

```powershell
$x = New-CimInstance -ClassName Win32_Environment -Property @{Name="testvar";UserName="domain\user"} -Keys Name,UserName -ClientOnly
Set-CimInstance -CimInstance $x -Property @{VariableValue="somevalue"} -PassThru
```

## PARAMETERS

### -CimSession

リモートコンピューターでコマンドレットを実行します。 またはコマンドレットの出力など、コンピューター名またはセッションオブジェクトを入力し `New-CimSession` `Get-CimSession` ます。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession[]
Parameter Sets: CimInstanceSessionSet, QuerySessionSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ComputerName

CIM 操作を実行するコンピューターの名前を指定します。 完全修飾ドメイン名 (FQDN) または NetBIOS 名を指定できます。

このパラメーターを指定しない場合、コマンドレットはコンポーネントオブジェクトモデル (COM) を使用してローカルコンピューター上で操作を実行します。

このパラメーターを指定すると、コマンドレットは WsMan プロトコルを使用して、指定されたコンピューターへの一時的なセッションを作成します。

複数の操作が同じコンピューター上で実行されている場合、CIM セッションを使用して接続するとパフォーマンスが向上します。

```yaml
Type: System.String[]
Parameter Sets: CimInstanceComputerSet, QueryComputerSet
Aliases: CN, ServerName

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

入力として使用する CIM インスタンスオブジェクトを指定します。

```yaml
Type: Microsoft.Management.Infrastructure.CimInstance
Parameter Sets: CimInstanceComputerSet, CimInstanceSessionSet
Aliases: CimInstance

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Namespace

CIM 操作の名前空間を指定します。 既定の名前空間は root/cimv2 です。 タブ補完を使用して名前空間の一覧を参照することができます。これは、名前空間の一覧を提供するために、PowerShell がローカル WMI サーバーから名前空間の一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: QuerySessionSet, QueryComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -OperationTimeoutSec

コマンドレットがコンピューターからの応答を待機する時間を指定します。 既定では、このパラメーターの値は0です。これは、コマンドレットがサーバーの既定のタイムアウト値を使用することを意味します。

**Operationtimeoutsec** パラメーターが、堅牢な接続再試行タイムアウトの3分未満の値に設定されている場合、 **operationtimeoutsec** パラメーターの値を超えるネットワークエラーは復旧できません。これは、クライアントが再接続する前に、サーバー上の操作がタイムアウトになるためです。

```yaml
Type: System.UInt32
Parameter Sets: (All)
Aliases: OT

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -プロパティ

(名前と値のペアを使用して) CIM インスタンスのプロパティをハッシュテーブルとして指定します。 このパラメーターを使用して指定されたプロパティのみが変更されます。 CIM インスタンスのその他のプロパティは変更されません。

```yaml
Type: System.Collections.IDictionary
Parameter Sets: CimInstanceComputerSet, CimInstanceSessionSet, QuerySessionSet, QueryComputerSet
Aliases: Arguments

Required: True (QuerySessionSet, QueryComputerSet), False (CimInstanceComputerSet, CimInstanceSessionSet)
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Query

コマンドレットを実行する CIM インスタンスを取得するために CIM サーバーで実行するクエリを指定します。 QueryDialect パラメーターを使用して、クエリ言語を指定できます。

指定した値に二重引用符 ( `"` )、単一引用符 ( `'` )、または円記号 () が含まれている場合は `\` 、円記号 () を付けることにより、これらの文字をエスケープする必要があり `\` ます。 指定された値が WQL **LIKE** 演算子を使用する場合は、次の文字を角かっこ () で囲むことによってエスケープする必要があります `[]` : パーセント () `%` 、アンダースコア ( `_` )、または左角かっこ ( `[` )。

```yaml
Type: System.String
Parameter Sets: QuerySessionSet, QueryComputerSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -QueryDialect

クエリパラメーターに使用するクエリ言語を指定します。 このパラメーターに指定できる値は、 **WQL** または **cql** です。 既定値は **WQL** です。

```yaml
Type: System.String
Parameter Sets: QuerySessionSet, QueryComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ResourceUri

リソースクラスまたはインスタンスのリソース URI (uniform resource identifier) を指定します。 URI は、ディスクやプロセスなど、コンピューター上の特定の種類のリソースを特定するために使用されます。

URI は、プレフィックスとリソースのパスで構成されます。 次に例を示します。

- `http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`
- `http://intel.com/wbem/wscim/1/amt-schema/1/AMT_GeneralSettings`

既定では、このパラメーターを指定しない場合、DMTF 標準リソース URI `http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/` が使用され、クラス名が追加されます。

ResourceURI は、WSMan プロトコルを使用して作成された CIM セッションでのみ使用できます。また、WSMan を使用して CIM セッションを作成する ComputerName パラメーターを指定する場合にのみ使用できます。 ComputerName パラメーターを指定せずにこのパラメーターを指定した場合、または DCOM プロトコルを使用して作成された CIM セッションを指定した場合は、DCOM プロトコルで ResourceURI パラメーターがサポートされていないため、エラーが発生します。

**ResourceUri** パラメーターと **filter** パラメーターの両方が指定されている場合、 **filter** パラメーターは無視されます。

```yaml
Type: System.Uri
Parameter Sets: CimInstanceComputerSet, CimInstanceSessionSet
Aliases:

Required: False
Position: Named
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### Microsoft.Management.Infrastructure.CimInstance

## 出力

### Microsoft.Management.Infrastructure.CimInstance

**Passthru** パラメーターを指定すると、このコマンドレットは、変更された CIM インスタンスオブジェクトを返します。

## 注

## 関連リンク

[Get-CimInstance](get-ciminstance.md)

[New-CimInstance](New-CimInstance.md)

[Remove-CimInstance](remove-ciminstance.md)

