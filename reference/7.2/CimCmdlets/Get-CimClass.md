---
external help file: Microsoft.Management.Infrastructure.CimCmdlets.dll-Help.xml
Locale: en-US
Module Name: CimCmdlets
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/cimcmdlets/get-cimclass?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-CimClass
ms.openlocfilehash: 483b4b5b940a9effca99e942b86a967de5d26d68
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600581"
---
# Get-CimClass

## 概要
特定の名前空間にある CIM クラスのリストを取得します。

## SYNTAX

### ComputerSet (既定)

```
Get-CimClass [[-ClassName] <String>] [[-Namespace] <String>] [-OperationTimeoutSec <UInt32>]
 [-ComputerName <String[]>] [-MethodName <String>] [-PropertyName <String>]
 [-QualifierName <String>] [<CommonParameters>]
```

### SessionSet

```
Get-CimClass [[-ClassName] <String>] [[-Namespace] <String>] [-OperationTimeoutSec <UInt32>]
 -CimSession <CimSession[]> [-MethodName <String>] [-PropertyName <String>]
 [-QualifierName <String>] [<CommonParameters>]
```

## Description

`Get-CimClass`コマンドレットは、特定の名前空間にある CIM クラスのリストを取得します。 クラス名が指定されていない場合、コマンドレットは名前空間内のすべてのクラスを返します。 Cim インスタンスとは異なり、CIM クラスには、取得元の cim セッションまたはコンピューター名は含まれません。

## 例

### 例 1: すべてのクラス定義を取得する

この例では、名前空間 **root/cimv2** の下にあるすべてのクラス定義を取得します。

```powershell
Get-CimClass
```

### 例 2: 特定の名前を持つクラスを取得する

この例では、名前に **disk** という単語を含むクラスを取得します。

```powershell
Get-CimClass -ClassName *disk*
```

### 例 3: 特定のメソッド名を持つクラスを取得する

この例では、 **Win32** という名前で始まり、 **Term** で始まるメソッド名を持つクラスを取得します。

```powershell
Get-CimClass -ClassName Win32* -MethodName Term*
```

### 例 4: 特定のプロパティ名を持つクラスを取得する

この例では、 **Win32** という名前で始まり、 **Handle** という名前のプロパティを持つクラスを取得します。

```powershell
Get-CimClass -ClassName Win32* -PropertyName Handle
```

### 例 5: 特定の修飾子名を持つクラスを取得する

この例では、 **Win32** という名前で始まるクラスを取得し、名前に **Disk** という単語を含め、指定された修飾子の **関連付け** を使用します。

```powershell
Get-CimClass -ClassName Win32*Disk* -QualifierName Association
```

### 例 6: 特定の名前空間からクラス定義を取得する

この例では、指定された名前空間 **root/standardCimv2** から、名前に **Net** という単語を含むクラス定義を取得します。

```powershell
Get-CimClass -Namespace root/standardCimv2 -ClassName *Net*
```

### 例 7: リモートサーバーからクラス定義を取得する

この例では、指定されたリモートサーバー **Server01** と **Server02** から、名前に **disk** という単語が含まれているクラス定義を取得します。

```powershell
Get-CimClass -ClassName *disk* -ComputerName Server01, Server02
```

### 例 8: CIM セッションを使用してクラスを取得する

```powershell
$s = New-CimSession -ComputerName Server01, Server02
Get-CimClass -ClassName *disk* -CimSession $s
```

この一連のコマンドは、複数のコンピューターを含むセッションを作成し、コマンドレットを使用して変数に格納した `$s` `New-CimSession` 後、コマンドレットを使用してクラスを取得し `Get-CimClass` ます。

## PARAMETERS

### -CimSession

リモート セッションまたはリモート コンピューターでコマンドレットを実行します。 またはコマンドレットの出力など、コンピューター名またはセッションオブジェクトを入力し `New-CimSession` `Get-CimSession` ます。 既定値は、ローカル コンピューターで実行中の現在のセッションです。

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

### -ClassName

操作を実行する CIM クラスの名前を指定します。 タブ補完を使用してクラスの一覧を参照することができます。これは、クラス名の一覧を提供するために、PowerShell がローカル WMI サーバーからクラスの一覧を取得するためです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -ComputerName

CIM 操作を実行するコンピューターを指定します。 完全修飾ドメイン名 (FQDN)、NetBIOS 名、または IP アドレスを指定できます。

このパラメーターを指定すると、コマンドレットは WsMan プロトコルを使用して、指定されたコンピューターへの一時的なセッションを作成します。

このパラメーターを指定しない場合、コマンドレットはコンポーネントオブジェクトモデル (COM) を使用してローカルコンピューター上で操作を実行します。

複数の操作が同じコンピューター上で実行されている場合、CIM セッションを使用するとパフォーマンスが向上します。

```yaml
Type: System.String[]
Parameter Sets: ComputerSet
Aliases: CN, ServerName

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MethodName

この名前と一致するメソッドを持つクラスを検索します。 このパラメーターには、ワイルドカード文字を使用できます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -Namespace

CIM 操作の名前空間を指定します。 既定の名前空間は **root/cimv2** です。 タブ補完を使用して名前空間の一覧を参照することができます。これは、名前空間の一覧を提供するために、PowerShell がローカル WMI サーバーから名前空間の一覧を取得するためです。

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

### -PropertyName

この名前と一致するプロパティを持つクラスを検索します。 このパラメーターには、ワイルドカード文字を使用できます。

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

### -QualifierName

クラスをクラスレベルの修飾子名でフィルター処理します。 このパラメーターには、ワイルドカード文字を使用できます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットは、入力オブジェクトを受け入れません。

## 出力

### CimClass (Microsoft 管理)

このコマンドレットは、CIM クラスオブジェクトを返します。

## 注

## 関連リンク

[New-CimSession](New-CimSession.md)

