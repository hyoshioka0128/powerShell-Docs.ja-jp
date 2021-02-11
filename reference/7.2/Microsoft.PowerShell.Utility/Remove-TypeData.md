---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/remove-typedata?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Remove-TypeData
ms.openlocfilehash: 8ede1375faa1287b70eeb49689ec7fe1bb800d55
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605450"
---
# Remove-TypeData

## 構文
現在のセッションから拡張型を削除します。

## 構文

### RemoveTypeDataSet (既定値)

```
Remove-TypeData -TypeData <TypeData> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### RemoveTypeSet

```
Remove-TypeData [-TypeName] <String> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### RemoveFileSet 場合

```
Remove-TypeData -Path <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Remove-TypeData` 現在のセッションから拡張型データを削除します。 このコマンドレットは、現在のセッションと、現在のセッションで作成されたセッションにのみ作用します。

コマンドとファイルを定義することで、PowerShell でオブジェクトにプロパティとメソッドを追加でき `Update-TypeData` `Types.ps1xml` ます。 `Remove-TypeData` これらの拡張プロパティとメソッドを現在のセッションから削除します。 `Remove-TypeData` ファイルを削除し `Types.ps1xml` たり、ファイルから拡張型定義を削除したりすることはありません `Types.ps1xml` 。 ファイルの詳細について `Types.ps1xml` は、「 [about_Types.ps1xml](../Microsoft.PowerShell.Core/about/about_Types.ps1xml.md)」を参照してください。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: 指定された型の型データを削除する

この例では、ファイルによって追加された型データ `Types.ps1xml` と、コマンドレットを使用してセッションに追加された動的な型データを含む、配列型のすべての型データをセッションから削除します。 `Update-TypeData`

```powershell
Remove-TypeData -TypeName System.Array
```

### 例 2: セッションから拡張データ型を削除する

この例は、セッションから拡張型データを削除した場合の効果を示しています。 最初のは、system.string `Get-TypeData` 型の拡張型データを取得します。 出力には、PowerShell のすべての system.string オブジェクトに **datetime** プロパティが追加されていることが示されて **います。** この `Get-Date` コマンドレットは **、system.string** オブジェクトを返します。 このコマンドは、ドット表記を使用して、を返す **system.string オブジェクトの** **datetime** プロパティの値を取得し `Get-Date` ます。

```powershell
Get-TypeData System.DateTime
(Get-Date).DateTime
Get-TypeData System.DateTime | Remove-TypeData
(Get-Date).DateTime
```

```Output
TypeName        Members
--------        -------
System.DateTime {[DateTime, System.Management.Automation.Runspaces.ScriptPropertyData]}

Friday, January 20, 2012 9:01:00 PM
```

次の `Get-TypeData` コマンドレットは、system.string 型のすべての拡張型データを取得し、コマンドレットにパイプを使用して `Remove-TypeData` 拡張型データを削除します。 最後の `Get-Date` コマンドレットは、 **DateTime** 型の拡張型データを削除した場合の影響を示しています。 System.string プロパティ **は存在しなく** なったため、その値を取得するコマンドは nothing を返します。

### 例 3: モジュールの拡張型を削除する

この例では、モジュールオブジェクトのすべての拡張型データを削除します。 パイプを使用してオブジェクトをにパイプすると `Remove-TypeData` 、は `Remove-TypeData` オブジェクトの種類の名前を取得し、その型のすべてのオブジェクトのすべての型データを削除します。

```powershell
Get-Module | Remove-TypeData
```

### 例 4: 指定したモジュールから拡張型を削除する

この例では、コマンドレットの **Path** パラメーターを使用して、 `Remove-TypeData` `Types.ps1xml` **Psscheduledjob** モジュールおよび **psworkflow** モジュールによって追加されたファイルで定義されている拡張型を削除します。 このコマンドは、コマンドレットを使用して追加された動的な型データには影響しません `Update-TypeData` 。 このコマンドは、モジュールが現在のセッションにインポートされている場合にのみ成功します。

```powershell
Remove-TypeData -Path "$PSHOME\Modules\PSScheduledJob", "$PSHOME\Modules\PSWorkflow\PSWorkflow.types.ps1xml"
```

モジュールの詳細については、「 [about_Modules](../Microsoft.PowerShell.Core/About/about_Modules.md)」を参照してください。

### 例 5: リモートセッションから拡張型を削除する

この例では、リモートセッションから拡張型を削除します。 このコマンドは、コマンドレットを使用して、 `Invoke-Command` 変数内のセッションのすべての CIM 型の拡張型データを削除し `$S` ます。

```powershell
Invoke-Command -Session $S {Get-TypeData -TypeName *CIM* | Remove-TypeData}
```

## パラメーター

### -Path

このコマンドレットがセッション拡張型データから削除するファイルの配列を指定します。 このパラメーターは必須です。

1つ以上のファイルのパスとファイル名を入力し `Types.ps1xml` ます。 ワイルドカードはサポートされていません。 パスを省略した場合、現在のディレクトリが既定の場所になります。

```yaml
Type: System.String[]
Parameter Sets: RemoveFileSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeData

このコマンドレットがセッションから削除する型データを指定します。 このパラメーターは必須です。 **Typedata** オブジェクト (System.string **. typedata**) を含む変数、またはコマンドなどの **typedata** オブジェクトを取得するコマンドを入力します `Get-TypeData` 。 パイプを使用して **Typedata** オブジェクトをにパイプすることもでき `Remove-TypeData` ます。

```yaml
Type: System.Management.Automation.Runspaces.TypeData
Parameter Sets: RemoveTypeDataSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -TypeName

このコマンドレットがすべての拡張型データを削除する型を指定します。 System 名前空間の型に対しては、短い名前を入力します。 それ以外の場合は、完全な型名が必要です。 ワイルドカードはサポートされていません。

パイプを使用して型名をにすることができ `Remove-TypeData` ます。 パイプを使用してオブジェクトをにパイプすると `Remove-TypeData` 、は `Remove-TypeData` オブジェクトの型名を取得し、そのオブジェクト型のすべての型データを削除します。

```yaml
Type: System.String
Parameter Sets: RemoveTypeSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システムの管理.... TypeData

パイプを使用して、コマンドレットが返すような **Typedata** オブジェクトをにすることができ `Get-TypeData` `Remove-TypeData` ます。

### System.String

型名をにパイプすることができ `Remove-TypeData` ます。 パイプを使用してオブジェクトをにパイプすると `Remove-TypeData` 、は `Remove-TypeData` オブジェクトの型名を取得し、そのオブジェクト型のすべての型データを削除します。

## 出力

### なし

このコマンドレットは出力を生成しません。

## メモ

`Remove-TypeData` では、現在のセッションの拡張型データのみを削除できます。 このコマンドレットは、モジュールに定義されている一方で現在のセッションにインポートされていない拡張型のように、コンピューター上に存在していて現在のセッションに追加されていない拡張型データは削除できません。

## 関連リンク

[Get-TypeData](Get-TypeData.md)

[Update-TypeData](Update-TypeData.md)

