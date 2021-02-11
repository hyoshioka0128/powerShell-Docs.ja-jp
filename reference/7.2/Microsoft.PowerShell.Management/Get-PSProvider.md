---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-psprovider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSProvider
ms.openlocfilehash: 20820d2d194f41d43048c9617b63b9acb3fbb4f8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602771"
---
# Get-PSProvider

## 概要
指定された PowerShell プロバイダーに関する情報を取得します。

## SYNTAX

```
Get-PSProvider [[-PSProvider] <String[]>] [<CommonParameters>]
```

## Description

`Get-PSProvider`コマンドレットは、現在のセッションの PowerShell プロバイダーを取得します。
セッション内の特定のドライブを取得することも、すべてのドライブを取得することもできます。

PowerShell プロバイダーを使用すると、ファイルシステムドライブのようにさまざまなデータストアにアクセスできます。
PowerShell プロバイダーの詳細については、「 [about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 例

### 例 1: 使用可能なすべてのプロバイダーの一覧を表示する

```powershell
Get-PSProvider
```

このコマンドは、使用可能なすべての PowerShell プロバイダーの一覧を表示します。

### 例 2: 指定した文字で始まるすべての PowerShell プロバイダーの一覧を表示する

```powershell
Get-PSProvider f*, r* | Format-List
```

このコマンドは、名前が f または r で始まるすべての PowerShell プロバイダーの一覧を表示します。

### 例 3: プロバイダーをセッションに追加したスナップインまたはモジュールを検索する

```powershell
Get-PSProvider | Format-Table name, module, pssnapin -auto
```

```Output
Name        Module       PSSnapIn
----        ------       --------
Test        TestModule
WSMan                    Microsoft.WSMan.Management
Alias                    Microsoft.PowerShell.Core
Environment              Microsoft.PowerShell.Core
FileSystem               Microsoft.PowerShell.Core
Function                 Microsoft.PowerShell.Core
Registry                 Microsoft.PowerShell.Core
Variable                 Microsoft.PowerShell.Core
Certificate              Microsoft.PowerShell.Security
```

```powershell
Get-PSProvider | Where {$_.pssnapin -eq "Microsoft.PowerShell.Security"}
```

```Output
Name            Capabilities      Drives
----            ------------      ------
Certificate     ShouldProcess     {cert}
```

これらのコマンドは、セッションにプロバイダーを追加した PowerShell スナップインまたはモジュールを検索します。
プロバイダーを含むすべての PowerShell 要素は、スナップインまたはモジュール内で発生します。

これらのコマンドは、を返す **Providerinfo** オブジェクトの Add-pssnapin および Module プロパティを使用し `Get-PSProvider` ます。
これらのプロパティの値には、プロバイダーを追加するスナップインまたはモジュールの名前が含まれます。

最初のコマンドは、セッションのすべてのプロバイダーを取得し、Name、Module、および PSSnapin プロパティの値を含め、表形式に書式設定します。

2番目のコマンドは、コマンドレットを使用して、 `Where-Object` **Microsoft. PowerShell** スナップインから取得されたプロバイダーを取得します。

### 例 4: ファイルシステムプロバイダーの Home プロパティのパスを解決する

```powershell
C:\> Resolve-Path ~
```

```Output
Path
----
C:\Users\User01
```

```powershell
PS C:\> (get-psprovider FileSystem).home
```

```Output
C:\Users\User01
```

この例では、チルダ記号 (~) が FileSystem プロバイダーの **Home** プロパティの値を表していることを示しています。
**Home** プロパティの値は省略可能ですが、 **FileSystem** プロバイダーでは、またはとして定義されてい `$env:homedrive\$env:homepath` `$home` ます。

## PARAMETERS

### -PSProvider

このコマンドレットによって情報が取得される PowerShell プロバイダーの名前を指定します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### String[]

パイプを使用して、1つまたは複数のプロバイダー名文字列をこのコマンドレットに渡します。

## 出力

### システムの管理. ProviderInfo

このコマンドレットは、セッション内の PowerShell プロバイダーを表すオブジェクトを返します。

## 注

## 関連リンク

