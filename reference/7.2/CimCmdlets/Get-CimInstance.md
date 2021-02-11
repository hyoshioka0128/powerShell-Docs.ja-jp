---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 02/21/2019
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/get-ciminstance?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-CimInstance
ms.openlocfilehash: 0e4a148601f3ef2212f9c97128c54258906106d7
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601206"
---
# Get-CimInstance

## 概要
CIM サーバーからクラスの CIM インスタンスを取得します。

## SYNTAX

### ClassNameComputerSet (既定値)

```
Get-CimInstance [-ClassName] <String> [-ComputerName <String[]>] [-KeyOnly] [-Namespace <String>]
 [-OperationTimeoutSec <UInt32>] [-QueryDialect <String>] [-Shallow] [-Filter <String>]
 [-Property <String[]>] [<CommonParameters>]
```

### ResourceUriSessionSet

```
Get-CimInstance -CimSession <CimSession[]> -ResourceUri <Uri> [-KeyOnly] [-Namespace <String>]
 [-OperationTimeoutSec <UInt32>] [-Shallow] [-Filter <String>] [-Property <String[]>]
 [<CommonParameters>]
```

### QuerySessionSet

```
Get-CimInstance -CimSession <CimSession[]> [-ResourceUri <Uri>] [-Namespace <String>]
 [-OperationTimeoutSec <UInt32>] -Query <String> [-QueryDialect <String>] [-Shallow]
 [<CommonParameters>]
```

### ClassNameSessionSet

```
Get-CimInstance -CimSession <CimSession[]> [-ClassName] <String> [-KeyOnly] [-Namespace <String>]
 [-OperationTimeoutSec <UInt32>] [-QueryDialect <String>] [-Shallow] [-Filter <String>]
 [-Property <String[]>] [<CommonParameters>]
```

### CimInstanceSessionSet

```
Get-CimInstance -CimSession <CimSession[]> [-ResourceUri <Uri>] [-OperationTimeoutSec <UInt32>]
 [-InputObject] <CimInstance> [<CommonParameters>]
```

### CimInstanceComputerSet

```
Get-CimInstance [-ResourceUri <Uri>] [-ComputerName <String[]>] [-OperationTimeoutSec <UInt32>]
 [-InputObject] <CimInstance> [<CommonParameters>]
```

### ResourceUriComputerSet

```
Get-CimInstance -ResourceUri <Uri> [-ComputerName <String[]>] [-KeyOnly] [-Namespace <String>]
 [-OperationTimeoutSec <UInt32>] [-Shallow] [-Filter <String>] [-Property <String[]>]
 [<CommonParameters>]
```

### QueryComputerSet

```
Get-CimInstance [-ResourceUri <Uri>] [-ComputerName <String[]>] [-Namespace <String>]
 [-OperationTimeoutSec <UInt32>] -Query <String> [-QueryDialect <String>] [-Shallow]
 [<CommonParameters>]
```

## Description

コマンドレットでは、 `Get-CimInstance` cim サーバーからクラスの cim インスタンスを取得します。 クラス名またはこのコマンドレットのクエリのいずれかを指定できます。 このコマンドレットは、CIM サーバー上に存在する CIM インスタンスのスナップショットを表す1つ以上の CIM インスタンスオブジェクトを返します。

**InputObject** パラメーターが指定されていない場合、コマンドレットは次のいずれかの方法で動作します。

- **ComputerName** パラメーターも **CimSession** パラメーターも指定しない場合、このコマンドレットはコンポーネントオブジェクトモデル (COM) セッションを使用してローカル Windows Management Instrumentation (WMI) で動作します。
- **Computername** パラメーターまたは **CimSession** パラメーターを指定した場合、このコマンドレットは、 **computername** パラメーターまたは **CIMSESSION** パラメーターで指定された CIM サーバーに対して機能します。

**InputObject** パラメーターが指定されている場合、コマンドレットは次のいずれかの方法で動作します。

- **ComputerName** パラメーターも **CimSession** パラメーターも指定しない場合、このコマンドレットは入力オブジェクトの CIM セッションまたはコンピューター名を使用します。
- **Computername** パラメーターまたは **CimSession** パラメーターのいずれかが指定されている場合、このコマンドレットは CimSession パラメーター値または **computername** パラメーター値を使用します。

## 例

### 例 1: 指定したクラスの CIM インスタンスを取得する

この例では、 **Win32_Process** という名前のクラスの CIM インスタンスを取得します。

```powershell
Get-CimInstance -ClassName Win32_Process
```

### 例 2: WMI サーバーから名前空間の一覧を取得する

この例では、WMI サーバーの **ルート** 名前空間の下にある名前空間の一覧を取得します。

```powershell
Get-CimInstance -Namespace root -ClassName __Namespace
```

### 例 3: クエリを使用してフィルター処理されたクラスのインスタンスを取得する

この例では、**クエリ** パラメーターによって指定されたクエリを使用して、 **Win32_Process** という名前のクラスの文字 **P** で始まるすべての CIM インスタンスを取得します。

```powershell
Get-CimInstance -Query "SELECT * from Win32_Process WHERE name LIKE 'P%'"
```

### 例 4: クラス名とフィルター式を使用してフィルター処理されたクラスのインスタンスを取得する

この例では、Filter パラメーターを使用して、 **Win32_Process** という名前のクラスの文字 **P** で始まるすべての CIM インスタンスを取得します。

```powershell
Get-CimInstance -ClassName Win32_Process -Filter "Name like 'P%'"
```

### 例 5: キープロパティが入力されている CIM インスタンスを取得する

この例では、キープロパティを使用して **Win32_Process** という名前のクラスの新しい CIM インスタンスをメモリに作成し、と `@{ "Handle"=0 }` いう名前の変数に格納し `$x` ます。 変数は、 `Get-CimInstance` 特定のインスタンスを取得するために、CIM インスタンスとしてコマンドレットに渡されます。

```powershell
$x = New-CimInstance -ClassName Win32_Process -Namespace root\cimv2 -Property @{ "Handle"=0 } -Key Handle -ClientOnly
Get-CimInstance -CimInstance $x
```

### 例 6: CIM インスタンスを取得して再利用する

この例では、 **Win32_Process** という名前のクラスの CIM インスタンスを取得し、それらを変数およびに格納し `$x` `$y` ます。 この変数は、 `$x` **名前** と値が **AutoSize** に設定され **ている** テーブルでフォーマットされます。

```powershell
$x,$y = Get-CimInstance -ClassName Win32_Process
$x | Format-Table -Property Name,KernelModeTime -AutoSize
```

```Output
Name                 KernelModeTime
----                 --------------
System Idle Process 157238797968750
```

### 例 7: リモートコンピューターから CIM インスタンスを取得する

この例では、 **Server01** と **Server02** という名前のリモートコンピューターから、 **Win32_ComputerSystem** という名前のクラスの CIM インスタンスを取得します。

```powershell
Get-CimInstance -ClassName Win32_ComputerSystem -ComputerName Server01,Server02
```

### 例 8: すべてのプロパティではなく、キープロパティのみを取得する

この例では、キープロパティのみを取得します。これにより、オブジェクトとネットワークトラフィックのサイズが縮小されます。

```powershell
$x = Get-CimInstance -Class Win32_Process -KeyOnly
$x | Invoke-CimMethod -MethodName GetOwner
```

### 例 9: すべてのプロパティではなく、プロパティのサブセットのみを取得する

この例では、プロパティのサブセットのみを取得するため、オブジェクトとネットワークトラフィックのサイズが縮小されます。

```powershell
Get-CimInstance -Class Win32_Process -Property Name,KernelModeTime
$x = Get-CimInstance -Class Win32_Process -Property Name,KernelModeTime
$x | Invoke-CimMethod -MethodName GetOwner
```

**プロパティ** パラメーターを使用して取得したインスタンスは、他の CIM 操作 (やなど) を実行するために使用でき `Set-CimInstance` `Invoke-CimMethod` ます。

### 例 10: CIM セッションを使用して CIM インスタンスを取得する

この例では、コマンドレットを使用して **Server01** と **Server02** という名前のコンピューターに CIM セッションを作成 `New-CimSession` し、という名前の変数にセッション情報を格納し `$s` ます。 次に、CimSession パラメーターを使用して変数の内容をに渡し、 `Get-CimInstance` **Win32_ComputerSystem** という名前のクラスの CIM インスタンスを取得します。 

```powershell
$s = New-CimSession -ComputerName Server01,Server02
Get-CimInstance -ClassName Win32_ComputerSystem -CimSession $s
```

## PARAMETERS

### -CimSession

このコマンドレットに使用する CIM セッションを指定します。 Cim セッションを含む変数、またはやコマンドレットなどの CIM セッションを作成または取得するコマンドを入力し `New-CimSession` `Get-CimSession` ます。 詳細については、「 [about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)」を参照してください。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession[]
Parameter Sets: ResourceUriSessionSet, QuerySessionSet, ClassNameSessionSet, CimInstanceSessionSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ClassName

CIM インスタンスを取得する CIM クラスの名前を指定します。 タブ補完を使用してクラスの一覧を参照することができます。これは、クラス名の一覧を提供するために、PowerShell がローカル WMI サーバーからクラスの一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, ClassNameSessionSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ComputerName

CIM 操作を実行するコンピューターを指定します。 完全修飾ドメイン名 (FQDN)、NetBIOS 名、または IP アドレスを指定できます。 このパラメーターを指定しない場合、コマンドレットはコンポーネントオブジェクトモデル (COM) を使用してローカルコンピューター上で操作を実行します。

このパラメーターを指定すると、コマンドレットは WsMan プロトコルを使用して、指定されたコンピューターへの一時的なセッションを作成します。

同じコンピューターで複数の操作を実行する場合は、パフォーマンスを向上させるために CIM セッションを使用して接続します。

```yaml
Type: System.String[]
Parameter Sets: ClassNameComputerSet, CimInstanceComputerSet, ResourceUriComputerSet, QueryComputerSet
Aliases: CN, ServerName

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Filter

フィルターとして使用する where 句を指定します。 **WQL** または **cql** クエリ言語で句を指定します。 `WHERE`パラメーターの値にキーワードを含めないでください。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, ResourceUriSessionSet, ClassNameSessionSet, ResourceUriComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -InputObject

入力として使用する CIM インスタンスオブジェクトを指定します。

Cim インスタンスオブジェクトを既に使用している場合は、cim サーバーから最新のスナップショットを取得するために、このパラメーターを使用して CIM インスタンスオブジェクトを渡すことができます。 CIM インスタンスオブジェクトを入力として渡すと、は、 `Get-CimInstance` 列挙またはクエリ操作ではなく、GET cim 操作を使用してサーバーからオブジェクトを返します。 Get CIM 操作の使用は、すべてのインスタンスを取得してからフィルター処理するよりも効率的です。

CIM クラスで get 操作が実装されていない場合、 **InputObject** パラメーターを指定するとエラーが返されます。

```yaml
Type: Microsoft.Management.Infrastructure.CimInstance
Parameter Sets: CimInstanceSessionSet, CimInstanceComputerSet
Aliases: CimInstance

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -KeyOnly

キープロパティが設定されたオブジェクトのみが返されることを示します。 **Keyonly** パラメーターを指定すると、ネットワーク経由で転送されるデータの量が減少します。

**Keyonly** パラメーターを使用して、オブジェクトのごく一部のみを返します。これは、やコマンドレットなど、他の操作に使用でき `Set-CimInstance` `Get-CimAssociatedInstance` ます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ClassNameComputerSet, ResourceUriSessionSet, ClassNameSessionSet, ResourceUriComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Namespace

CIM クラスの名前空間を指定します。

既定の名前空間は **root/cimv2** です。 タブ補完を使用して名前空間の一覧を参照することができます。これは、名前空間の一覧を提供するために、PowerShell がローカル WMI サーバーから名前空間の一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, ResourceUriSessionSet, QuerySessionSet, ClassNameSessionSet, ResourceUriComputerSet, QueryComputerSet
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

### -プロパティ

取得するインスタンスプロパティのセットを指定します。 メモリまたはネットワーク上で返されるオブジェクトのサイズを小さくする必要がある場合は、このパラメーターを使用します。 返されたオブジェクトには、 **プロパティ** パラメーターを使用して一覧表示していない場合でも、キープロパティが含まれます。 クラスのその他のプロパティは存在しますが、値は設定されません。

```yaml
Type: System.String[]
Parameter Sets: ClassNameComputerSet, ResourceUriSessionSet, ClassNameSessionSet, ResourceUriComputerSet
Aliases: SelectProperties

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Query

CIM サーバーで実行するクエリを指定します。 指定した値に二重引用符 `"` 、単一引用符、または円記号が含まれている場合は `'` `\` 、円記号を付けることによってこれらの文字をエスケープする必要があります。 指定された値が WQL **LIKE** 演算子を使用する場合は、次の文字を角かっこで囲むことによってエスケープする必要があります `[]` : パーセント `%` 、アンダースコア `_` 、または左角かっこ `[` 。

メタデータクエリを使用して、クラスの一覧またはイベントクエリを取得することはできません。 クラスの一覧を取得するには、 `Get-CimClass` コマンドレットを使用します。 イベントクエリを取得するには、 `Register-CimIndicationEvent` コマンドレットを使用します。

**Querydialect** パラメーターを使用して、クエリ言語を指定できます。

```yaml
Type: System.String
Parameter Sets: QuerySessionSet, QueryComputerSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -QueryDialect

クエリパラメーターに使用するクエリ言語を指定します。 このパラメーターに指定できる値は、 **WQL** または **cql** です。 既定値は **WQL** です。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, QuerySessionSet, ClassNameSessionSet, QueryComputerSet
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

**ResourceURI** は、wsman プロトコルを使用して作成された cim セッションでのみ使用できます。また、wsman を使用して cim セッションを作成する **ComputerName** パラメーターを指定する場合にのみ使用できます。 **ComputerName** パラメーターを指定せずにこのパラメーターを指定した場合、または dcom プロトコルを使用して作成された CIM セッションを指定した場合は、dcom プロトコルで **ResourceURI** パラメーターがサポートされていないため、エラーが発生します。

**ResourceUri** パラメーターと **filter** パラメーターの両方が指定されている場合、 **filter** パラメーターは無視されます。

```yaml
Type: System.Uri
Parameter Sets: ResourceUriSessionSet, ResourceUriComputerSet, QuerySessionSet, QueryComputerSet, CimInstanceSessionSet, CimInstanceComputerSet
Aliases:

Required: True (ResourceUriSessionSet, ResourceUriComputerSet), False (QuerySessionSet, QueryComputerSet, CimInstanceSessionSet, CimInstanceComputerSet)
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -浅い

子クラスのインスタンスを含めずに、クラスのインスタンスが返されることを示します。 既定では、コマンドレットは、クラスとその子クラスのインスタンスを返します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ClassNameComputerSet, ResourceUriSessionSet, QuerySessionSet, ClassNameSessionSet, ResourceUriComputerSet, QueryComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### CIM インスタンス

このコマンドレットは、InputObject パラメーターで指定された入力オブジェクトを受け入れます。

## 出力

### CIM インスタンス

このコマンドレットは、cim サーバー上の CIM インスタンスのスナップショットを表す1つ以上の CIM インスタンスオブジェクトを返します。

## 注

## 関連リンク

[Format-Table](../microsoft.powershell.utility/format-table.md)

[Get-CimAssociatedInstance](Get-CimAssociatedInstance.md)

[Get-CimClass](Get-CimClass.md)

[Invoke-CimMethod](invoke-cimmethod.md)

[New-CimInstance](New-CimInstance.md)

[Register-CimIndicationEvent](Register-CimIndicationEvent.md)

[Remove-CimInstance](remove-ciminstance.md)

[Set-CimInstance](Set-CimInstance.md)

