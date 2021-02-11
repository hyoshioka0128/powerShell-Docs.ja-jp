---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/get-cimassociatedinstance?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-CimAssociatedInstance
ms.openlocfilehash: e14ba6db4f5e93c6e2c1824ce845f82720616bc5
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601429"
---
# Get-CimAssociatedInstance

## 概要
アソシエーションによって特定の CIM インスタンスに接続されている CIM インスタンスを取得します。

## SYNTAX

### ComputerSet (既定)

```
Get-CimAssociatedInstance [[-Association] <String>] [-ResultClassName <String>]
 [-InputObject] <CimInstance> [-Namespace <String>] [-OperationTimeoutSec <UInt32>]
 [-ResourceUri <Uri>] [-ComputerName <String[]>] [-KeyOnly] [<CommonParameters>]
```

### SessionSet

```
Get-CimAssociatedInstance [[-Association] <String>] [-ResultClassName <String>]
 [-InputObject] <CimInstance> [-Namespace <String>] [-OperationTimeoutSec <UInt32>]
 [-ResourceUri <Uri>] -CimSession <CimSession[]> [-KeyOnly] [<CommonParameters>]
```

## Description

この `Get-CimAssociatedInstance` コマンドレットは、関連付けによって、ソースインスタンスと呼ばれる特定の cim インスタンスに接続されている cim インスタンスを取得します。

アソシエーションでは、各 CIM インスタンスに名前付きロールがあり、同じ CIM インスタンスを異なるロール内のアソシエーションに参加させることができます。

InputObject パラメーターが指定されていない場合、コマンドレットは次のいずれかの方法で動作します。

- **ComputerName** パラメーターも **CimSession** パラメーターも指定しない場合、このコマンドレットはコンポーネントオブジェクトモデル (COM) セッションを使用してローカル Windows Management Instrumentation (WMI) で動作します。
- **Computername** パラメーターまたは **CimSession** パラメーターを指定した場合、このコマンドレットは、 **computername** パラメーターまたは **CIMSESSION** パラメーターで指定された CIM サーバーに対して機能します。

## 例

### 例 1: 特定のインスタンスに関連付けられているすべてのインスタンスを取得する

```powershell
$disk = Get-CimInstance -ClassName Win32_LogicalDisk -KeyOnly
Get-CimAssociatedInstance -InputObject $disk[1]
```

この一連のコマンドは、Win32_LogicalDisk という名前のクラスのインスタンスを取得し、 `$disk` コマンドレットを使用して、という名前の変数に情報を格納し `Get-CimInstance` ます。 次に、変数内の最初の論理ディスクインスタンスをコマンドレットの入力オブジェクトとして使用して、 `Get-CimAssociatedInstance` 指定した cim インスタンスの関連付けられているすべての cim インスタンスを取得します。

### 例 2: 特定の型のすべての関連インスタンスを取得する

```powershell
$disk = Get-CimInstance -ClassName Win32_LogicalDisk -KeyOnly
Get-CimAssociatedInstance -InputObject $disk[1] -ResultClass Win32_DiskPartition
```

この一連のコマンドは、 **Win32_LogicalDisk** クラスのすべてのインスタンスを取得し、という名前の変数に格納し `$disk` ます。 次に、変数内の最初の論理ディスクインスタンスをコマンドレットの入力オブジェクトとして使用して、指定した `Get-CimAssociatedInstance` アソシエーションクラス **Win32_DiskPartition** に関連付けられているすべての関連するインスタンスを取得します。

### 例 3: 特定のクラスの修飾子を使用して、関連するすべてのインスタンスを取得する

```powershell
$s = Get-CimInstance -Query "Select * from Win32_Service where name like 'Winmgmt'"
Get-CimClass -ClassName *Service* -Qualifier "Association"
$c.CimClasName
```

```Output
Win32_LoadOrderGroupServiceDependencies
Win32_DependentService
Win32_SystemServices
Win32_LoadOrderGroupServiceMembers
Win32_ServiceSpecificationService
```

```powershell
Get-CimAssociatedInstance -InputObject $s -Association Win32_DependentService
```

この一連のコマンドは、WMI サービスに依存するサービスを取得し、という名前の変数に格納し `$s` ます。 **Win32_DependentService** のアソシエーションクラス名は、コマンドレットを使用して取得されます `Get-CimClass` 。**関連付け** を修飾子として指定し、コマンドレットに $s 渡して、取得した `Get-CimAssociatedInstance` association クラスのすべての関連インスタンスを取得します。

## PARAMETERS

### -関連付け

アソシエーションクラスの名前を指定します。 このパラメーターを指定しない場合、コマンドレットは任意の型のすべての既存の関連付けオブジェクトを返します。

たとえば、AB1 と AB2 の2つのアソシエーションを通じてクラス A がクラス B に関連付けられている場合、このパラメーターを使用して、AB1 または AB2 のいずれかの関連付けの種類を指定できます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -CimSession

指定された CIM セッションを使用してコマンドを実行します。 CIM セッションを含む変数、またはやなどの CIM セッションを作成または取得するコマンドを入力 `New-CimSession` し `Get-CimSession` ます。 詳細については、「 [about_CimSession](../Microsoft.PowerShell.Core/About/about_CimSession.md)」を参照してください。

```yaml
Type: Microsoft.Management.Infrastructure.CimSession[]
Parameter Sets: SessionSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ComputerName

CIM 操作を実行するコンピューターの名前を指定します。 完全修飾ドメイン名 (FQDN) または NetBIOS 名を指定できます。

このパラメーターを指定すると、コマンドレットは WsMan プロトコルを使用して、指定されたコンピューターへの一時的なセッションを作成します。

このパラメーターを指定しない場合、コマンドレットはコンポーネントオブジェクトモデル (COM) を使用してローカルコンピューター上で操作を実行します。

複数の操作が同じコンピューター上で実行されている場合、CIM セッションを使用して接続するとパフォーマンスが向上します。

```yaml
Type: System.String[]
Parameter Sets: ComputerSet
Aliases: CN, ServerName

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

このコマンドレットへの入力を指定します。 このパラメーターを使用するか、またはこのコマンドレットへの入力をパイプで渡すことができます。

```yaml
Type: Microsoft.Management.Infrastructure.CimInstance
Parameter Sets: (All)
Aliases: CimInstance

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -KeyOnly

キープロパティのみが設定されたオブジェクトを返します。 これにより、ネットワーク経由で転送されるデータの量が減少します。

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

### -Namespace

CIM 操作の名前空間を指定します。 既定の名前空間は root/cimv2 です。

> [!NOTE]
> タブ補完を使用して名前空間の一覧を参照することができます。これは、名前空間の一覧を提供するために、PowerShell がローカル WMI サーバーから名前空間の一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: (All)
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
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ResourceUri

リソースクラスまたはインスタンスのリソース URI (uniform resource identifier) を指定します。 URI は、ディスクやプロセスなど、コンピューター上の特定の種類のリソースを特定するために使用されます。

URI は、プレフィックスとリソースのパスで構成されます。 次に例を示します。

- `http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`
- `http://intel.com/wbem/wscim/1/amt-schema/1/AMT_GeneralSettings`

既定では、このパラメーターを指定しない場合、DMTF 標準リソース URI `http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/` が使用され、クラス名が追加されます。

**ResourceURI** は、wsman プロトコルを使用して作成された cim セッションでのみ使用できます。また、wsman を使用して cim セッションを作成する **ComputerName** パラメーターを指定する場合にのみ使用できます。 **ComputerName** パラメーターを指定せずにこのパラメーターを指定した場合、または dcom プロトコルを使用して作成された CIM セッションを指定した場合、dcom プロトコルでは **ResourceURI** パラメーターがサポートされないため、エラーが発生します。

**ResourceUri** パラメーターと **filter** パラメーターの両方が指定されている場合、 **filter** パラメーターは無視されます。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResultClassName

関連付けられているインスタンスのクラス名を指定します。 CIM インスタンスは、1つまたは複数の CIM インスタンスに関連付けることができます。 結果クラス名を指定しない場合は、関連付けられているすべての CIM インスタンスが返されます。

既定では、このパラメーターの値は null で、関連付けられているすべての CIM インスタンスが返されます。

関連付けの結果をフィルター処理して、特定のクラス名に一致させることができます。 フィルター処理はサーバー上で行われます。 このパラメーターが指定されていない場合、は `Get-CIMAssociatedInstance` 既存のすべての関連付けを返します。 たとえば、クラス A がクラス B、C、および D に関連付けられている場合、このパラメーターを使用して、出力を特定の型 (B、C、または D) に制限できます。

```yaml
Type: System.String
Parameter Sets: (All)
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

### なし

このコマンドレットは、入力オブジェクトを受け入れません。

## 出力

### System.Object

このコマンドレットは、オブジェクトを返します。

## 注

## 関連リンク

[Get-CimClass](get-cimclass.md)

[Get-CimInstance](get-ciminstance.md)

