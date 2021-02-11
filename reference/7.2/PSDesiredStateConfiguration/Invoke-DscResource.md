---
external help file: PSDesiredStateConfiguration-help.xml
Locale: en-US
Module Name: PSDesiredStateConfiguration
ms.date: 08/11/2020
online version: https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/invoke-dscresource?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-DscResource
ms.openlocfilehash: 732db5a049a68e059db6062d2f6c3f850d579adc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600999"
---
# Invoke-DscResource

## 概要
指定された PowerShell Desired State Configuration (DSC) リソースのメソッドを実行します。

## SYNTAX

```
Invoke-DscResource [-Name] <String> [[-ModuleName] <ModuleSpecification>] [-Method] <String>
 [-Property] <Hashtable> [<CommonParameters>]
```

## Description

`Invoke-DscResource` コマンドレットは、指定された PowerShell Desired State Configuration (DSC) リソースのメソッドを実行します。

このコマンドレットは、構成ドキュメントを作成せずに、DSC リソースを直接呼び出します。 このコマンドレットを使用すると、構成管理製品は DSC リソースを使用して windows または Linux を管理できます。 DSC エンジンの実行中、デバッグが有効になっている場合は、このコマンドレットでリソースのデバッグを有効にすることもできます。

> [!NOTE]
> `Invoke-DscResource` は、PowerShell 7 の試験的な機能です。 コマンドレットを使用するには、次のコマンドを使用して有効にする必要があります。
>
> `Enable-ExperimentalFeature PSDesiredStateConfiguration.InvokeDscResource`

## 例

### 例 1: 必須プロパティを指定してリソースの Set メソッドを呼び出す

この例では、 **Windowsprocess** という名前のリソースの **Set** メソッドを呼び出し、指定された Windows プロセスを開始するための必須の **パス** と **引数** のプロパティを提供します。

```powershell
Invoke-DscResource -Name WindowsProcess -Method Set -ModuleName PSDesiredStateConfiguration -Property @{
  Path = 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe'
  Arguments = ''
}
```

### 例 2: 指定したモジュールのリソースのテストメソッドを呼び出す

この例では、 **Windowsprocess** という名前のリソースの **Test** メソッドを呼び出します。これは **PSDesiredStateConfiguration** という名前のモジュールにあります。

```powershell
$SplatParam = @{
  Name = 'WindowsProcess'
  ModuleName = 'PSDesiredStateConfiguration'
  Property = @{Path = 'C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe'; Arguments = ''}
  Method = Test
}

Invoke-DscResource @SplatParam
```

## PARAMETERS

### -メソッド

このコマンドレットが呼び出すリソースのメソッドを指定します。 このパラメーターに指定できる値は、 **Get**、 **Set**、および **Test** です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Get, Set, Test

Required: True
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ModuleName

このコマンドレットが指定されたリソースを呼び出すモジュールの名前を指定します。

```yaml
Type: Microsoft.PowerShell.Commands.ModuleSpecification
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Name

開始する DSC リソースの名前を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -プロパティ

ハッシュ テーブルで、リソースのプロパティ名と値を、それぞれキーと値として指定します。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: True
Position: 3
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

### Microsoft. PowerShell. コマンドの通知

## 出力

### System.Object

## 注

- 以前は、Windows PowerShell 5.1 のリソースは、 **PsDscRunAsCredential** キーを使用してユーザーコンテキストで指定されていない限り、システムコンテキストで実行されていました。 PowerShell 7.0 では、リソースはユーザーのコンテキストで実行され、 **PsDscRunAsCredential** はサポートされなくなりました。 このキーを使用する以前の構成では、例外がスローされます。

- PowerShell 7 の時点で、は `Invoke-DscResource` WMI DSC リソースの呼び出しをサポートしていません。 これには、**PSDesiredStateConfiguration** の **ファイル** リソースと **ログ** リソースが含まれます。

## 関連リンク

[Windows PowerShell Desired State Configuration の概要](/powershell/scripting/dsc/overview/dscforengineers)

[Get-DscResource](Get-DscResource.md)
