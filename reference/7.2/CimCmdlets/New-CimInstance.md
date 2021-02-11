---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/new-ciminstance?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-CimInstance
ms.openlocfilehash: d81117ad6885d660e31f031bd8134096db91b7da
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602174"
---
# New-CimInstance

## 概要
CIM インスタンスを作成します。

## SYNTAX

### ClassNameComputerSet (既定値)

```
New-CimInstance [-ClassName] <String> [-Key <String[]>] [[-Property] <IDictionary>]
 [-Namespace <String>] [-OperationTimeoutSec <UInt32>] [-ComputerName <String[]>] [-ClientOnly]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ClassNameSessionSet

```
New-CimInstance [-ClassName] <String> [-Key <String[]>] [[-Property] <IDictionary>]
 [-Namespace <String>] [-OperationTimeoutSec <UInt32>] -CimSession <CimSession[]> [-ClientOnly]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ResourceUriSessionSet

```
New-CimInstance -ResourceUri <Uri> [-Key <String[]>] [[-Property] <IDictionary>]
 [-Namespace <String>] [-OperationTimeoutSec <UInt32>] -CimSession <CimSession[]> [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### ResourceUriComputerSet

```
New-CimInstance -ResourceUri <Uri> [-Key <String[]>] [[-Property] <IDictionary>]
 [-Namespace <String>] [-OperationTimeoutSec <UInt32>] [-ComputerName <String[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### CimClassSessionSet

```
New-CimInstance [-CimClass] <CimClass> [[-Property] <IDictionary>] [-OperationTimeoutSec <UInt32>]
 -CimSession <CimSession[]> [-ClientOnly] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### CimClassComputerSet

```
New-CimInstance [-CimClass] <CimClass> [[-Property] <IDictionary>] [-OperationTimeoutSec <UInt32>]
 [-ComputerName <String[]>] [-ClientOnly] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

この `New-CimInstance` コマンドレットは、ローカルコンピューターまたはリモートコンピューターのクラス定義に基づいて、CIM クラスのインスタンスを作成します。 既定では、 `New-CimInstance` コマンドレットは、ローカルコンピューター上にインスタンスを作成します。

## 例

### 例 1: CIM クラスのインスタンスを作成する

この例では、コンピューターのルート/cimv2 名前空間に win32_environment という名前の CIM クラスのインスタンスを作成します。

```powershell
New-CimInstance -ClassName Win32_Environment -Property @{Name="testvar";VariableValue="testvalue";UserName="domain\user"}
```

クラスが存在しない場合、プロパティが間違っている場合、またはサーバーが呼び出しを拒否した場合、クライアント側の検証は実行されません。 インスタンスが正常に作成された場合は、新しく作成されたインスタンスがコマンドレットによって出力されます。

### 例 2: クラススキーマを使用して CIM クラスのインスタンスを作成する

この例では、CIM クラスオブジェクトを取得し、という名前の変数に格納し `$class` ます。 次に、変数の内容がコマンドレットに渡され `New-CimInstance` ます。

```powershell
$class = Get-CimClass -ClassName Win32_Environment
New-CimInstance -CimClass $class -Property @{Name="testvar";VariableValue="testvalue";UserName="Contoso\User1"}
```

### 例 3: クライアントで動的インスタンスを作成する

この例では、サーバーからインスタンスを取得せずに、 **Win32_Process** という名前の CIM クラスの動的インスタンスをクライアントコンピューターに作成します。 新しいインスタンスは変数に格納され `$a` ます。 この動的インスタンスは、このキーを持つインスタンスがサーバー上に存在する場合に、操作を実行するために使用できます。

```powershell
$a = New-CimInstance -ClassName Win32_Process -Property @{Handle=0} -Key Handle -ClientOnly
Get-CimInstance -CimInstance $a
Invoke-CimMethod -CimInstance $a -MethodName GetOwner
```

```Output
ProcessId Name                HandleCount WorkingSetSize VirtualSize
--------- ----                ----------- -------------- -----------
0         System Idle Process 0           8192           8192

Domain         :
ReturnValue    : 2
User           :
PSComputerName :
```

次に、 `Get-CimInstance` コマンドレットは、特定の単一のインスタンスを取得します。 コマンドレットでは、取得した `Invoke-CimMethod` インスタンスの **GetOwner** メソッドを呼び出します。

### 例 4: 特定の名前空間の CIM クラスのインスタンスを作成する

この例では、名前空間の **ルートまたは場所** にある **MSFT_Something** という名前の CIM クラスのインスタンスを取得し、という名前の変数に格納し `$class` ます。 変数は、 `New-CimInstance` 新しい CIM インスタンスを作成し、新しいインスタンスでクライアント側の検証を実行するために、コマンドレットに渡されます。

```powershell
$class = Get-CimClass -ClassName MSFT_Something -Namespace root/somewhere
New-CimInstance -CimClass $class -Property @{"Prop1"=1;"Prop2"="value"} -ClientOnly
```

この例では、 **ClassName** パラメーターの代わりに **CimClass** パラメーターを使用して、 **Prop1** と **Prop2** が実際に存在し、キーが正しくマークされていることを検証します。

**Clientonly** パラメーターと共に **ComputerName** パラメーターまたは **CimSession** パラメーターを使用することはできません。

## PARAMETERS

### -CimClass

インスタンスの型を表す CIM クラスオブジェクトを指定します。 コマンドレットを使用して、 `Get-CimClass` コンピューターからクラス宣言を取得します。 このパラメーターを使用すると、クライアント側のスキーマ検証の精度が向上します。

```yaml
Type: Microsoft.Management.Infrastructure.CimClass
Parameter Sets: CimClassSessionSet, CimClassComputerSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -CimSession

指定された CIM セッションを使用してコマンドを実行します。 CIM セッションを含む変数、またはやコマンドレットなどの CIM セッションを作成または取得するコマンドを入力し `New-CimSession` `Get-CimSession` ます。 詳細については、「 [about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)」を参照してください。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession[]
Parameter Sets: ClassNameSessionSet, ResourceUriSessionSet, CimClassSessionSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ClassName

操作によってインスタンスが作成される CIM クラスの名前を指定します。 メモ: クラス名の一覧を提供するために、PowerShell ではローカル WMI サーバーからクラスの一覧を取得するため、タブ補完を使用してクラスの一覧を参照できます。

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

### -ClientOnly

インスタンスが、CIM サーバーに移動せずに PowerShell でのみ作成されることを示します。 このパラメーターを使用すると、後続の PowerShell 操作で使用するために、メモリ内 CIM インスタンスを作成できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ClassNameComputerSet, ClassNameSessionSet, CimClassSessionSet, CimClassComputerSet
Aliases: Local

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ComputerName

CIM 操作を実行するコンピューターの名前を指定します。 完全修飾ドメイン名 (FQDN)、NetBIOS 名、または IP アドレスを指定できます。

このパラメーターを指定すると、コマンドレットは WSMan プロトコルを使用して、指定されたコンピューターへの一時的なセッションを作成します。

このパラメーターを指定しない場合、コマンドレットはコンポーネントオブジェクトモデル (COM) を使用してローカルコンピューター上で操作を実行します。

複数の操作が同じコンピューター上で実行されている場合、CIM セッションを使用して接続するとパフォーマンスが向上します。

```yaml
Type: System.String[]
Parameter Sets: ClassNameComputerSet, ResourceUriComputerSet, CimClassComputerSet
Aliases: CN, ServerName

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Key

キーとして使用されるプロパティを指定します。 **CimSession** と **ComputerName** は、 **Key** が指定されている場合は使用できません。

```yaml
Type: System.String[]
Parameter Sets: ClassNameComputerSet, ClassNameSessionSet, ResourceUriSessionSet, ResourceUriComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Namespace

新しいインスタンスのクラスの名前空間を指定します。 既定の名前空間は **root/cimv2** です。
タブ補完を使用して名前空間の一覧を参照することができます。これは、名前空間の一覧を提供するために、PowerShell がローカル WMI サーバーから名前空間の一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: ClassNameComputerSet, ClassNameSessionSet, ResourceUriSessionSet, ResourceUriComputerSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -OperationTimeoutSec

コマンドレットが CIM サーバーからの応答を待機する時間を指定します。 既定では、このパラメーターの値は0です。これは、コマンドレットがサーバーの既定のタイムアウト値を使用することを意味します。 **Operationtimeoutsec** パラメーターが、堅牢な接続再試行タイムアウトの3分未満の値に設定されている場合、 **operationtimeoutsec** パラメーターの値を超えるネットワークエラーは復旧できません。これは、クライアントが再接続する前に、サーバー上の操作がタイムアウトになるためです。

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

ハッシュテーブル (名前と値のペア) を使用して、CIM インスタンスのプロパティを指定します。

**CimClass** パラメーターを指定した場合、 `New-CimInstance` コマンドレットは、指定されたプロパティがサーバー上のクラス宣言と一致することを確認するために、クライアントでプロパティの検証を実行します。 **CimClass** パラメーターが指定されていない場合、プロパティの検証はサーバーで実行されます。

```yaml
Type: System.Collections.IDictionary
Parameter Sets: (All)
Aliases: Arguments

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ResourceUri

リソースクラスまたはインスタンスのリソース URI (uniform resource identifier) を指定します。 URI は、ディスクやプロセスなど、コンピューター上の特定の種類のリソースを特定するために使用されます。

URI は、プレフィックスとリソースのパスで構成されます。 次に例を示します。

`http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`

`http://intel.com/wbem/wscim/1/amt-schema/1/AMT_GeneralSettings`

既定では、このパラメーターを指定しない場合、DMTF 標準リソース URI `http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/` が使用され、クラス名が追加されます。

**ResourceURI** は、wsman プロトコルを使用して作成された cim セッションでのみ使用できます。また、wsman を使用して cim セッションを作成する **ComputerName** パラメーターを指定する場合にのみ使用できます。 **ComputerName** パラメーターを指定せずにこのパラメーターを指定した場合、または dcom プロトコルを使用して作成された CIM セッションを指定した場合は、dcom プロトコルで **ResourceURI** パラメーターがサポートされていないため、エラーが発生します。

**ResourceUri** パラメーターと **filter** パラメーターの両方が指定されている場合、 **filter** パラメーターは無視されます。

```yaml
Type: System.Uri
Parameter Sets: ResourceUriSessionSet, ResourceUriComputerSet
Aliases:

Required: True
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

### なし

このコマンドレットは、入力オブジェクトを受け入れません。

## 出力

### System.Object

このコマンドレットは、CIM インスタンス情報を格納しているオブジェクトを返します。

## 注

## 関連リンク

[Get-CimClass](get-cimclass.md)

[Get-CimInstance](get-ciminstance.md)

[Remove-CimInstance](remove-ciminstance.md)

[Set-CimInstance](Set-CimInstance.md)

