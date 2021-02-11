---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 11/02/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/test-filecatalog?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-FileCatalog
ms.openlocfilehash: 2c3b9938e778888c277403ab9bb6923caa24c68c
ms.sourcegitcommit: 9a86cac80402d8193147058d4ba50e07b26059dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97490603"
---
# Test-FileCatalog

## 概要
`Test-FileCatalog` 信頼性を検証するために、カタログファイル (.cat) に含まれるハッシュが実際のファイルのハッシュと一致するかどうかを検証します。

このコマンドレットは、Windows でのみサポートされています。

## SYNTAX

```
Test-FileCatalog [-Detailed] [-FilesToSkip <String[]>] [-CatalogFilePath] <String> [[-Path] <String[]>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Test-FileCatalog` カタログファイル (.cat) のファイルハッシュとディスク上の実際のファイルのハッシュを比較することによって、ファイルの信頼性を検証します。 不一致が検出されると、ValidationFailed としてステータスが返されます。 -Detailed パラメーターを利用し、この情報をすべて取得できます。 また、カタログファイルでコマンドレットを呼び出した場合と同じ、署名プロパティにカタログの署名の状態が表示され `Get-AuthenticodeSignature` ます。 -FilesToSkip パラメーターを利用し、検証中にファイルをスキップすることもできます。

このコマンドレットは、Windows でのみサポートされています。

## 例

### 例 1: ファイルカタログを作成および検証する

```powershell
New-FileCatalog -Path $PSHOME\Modules\Microsoft.PowerShell.Utility -CatalogFilePath \temp\Microsoft.PowerShell.Utility.cat -CatalogVersion 2.0

Test-FileCatalog -CatalogFilePath \temp\Microsoft.PowerShell.Utility.cat -Path "$PSHome\Modules\Microsoft.PowerShell.Utility\"
```

```Output
Valid
```

### 例 2: 詳細な出力を使用してファイルカタログを検証する

```powershell
Test-FileCatalog -Detailed -CatalogFilePath \temp\Microsoft.PowerShell.Utility.cat -Path "$PSHome\Modules\Microsoft.PowerShell.Utility\"
```

```Output
Status        : Valid
HashAlgorithm : SHA256
CatalogItems  : {[Microsoft.PowerShell.Utility.psd1,
                A7028BD54018AE519381CDF5BF91F3B0417BD9345478086089ACBFAD05C899FC], [Microsoft.PowerShell.Utility.psm1,
                1127E8151FB86BCB683F932E8F6538552F7195816ED351A28AE07A753B8F20DE]}
PathItems     : {[Microsoft.PowerShell.Utility.psd1,
                A7028BD54018AE519381CDF5BF91F3B0417BD9345478086089ACBFAD05C899FC], [Microsoft.PowerShell.Utility.psm1,
                1127E8151FB86BCB683F932E8F6538552F7195816ED351A28AE07A753B8F20DE]}
Signature     : System.Management.Automation.Signature
```

## PARAMETERS

### -CatalogFilePath

検証に使用するハッシュを含むカタログファイル (.cat) へのパスです。

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

### -Detailed

詳細な情報を返し `CatalogInformation` ます。これは、テストされたファイルを含むより詳細なオブジェクト、予想される/実際のハッシュ、およびカタログファイルが署名されている場合の Authenticode 署名です。

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

### -FilesToSkip

検証の一部としてテストすべきではないパスの配列。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

カタログファイルに対して検証する必要のあるフォルダーまたはファイルの配列。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
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

### DirectoryInfo []、System.string []。

パイプラインは、 `DirectoryInfo` 検証する必要があるファイルへのパスを表す文字列またはオブジェクトの配列を受け入れます。

## 出力

### CatalogValidationStatus (システム管理)

またはのいずれかの値を格納している既定の戻り値の型 `Valid` `ValidationFailed` 。

### システム管理. CatalogInformation

を使用するときに返されるより詳細なオブジェクト。これを使用すると、 `-Detailed` 検証に合格した特定のファイル、予想されるハッシュ、検出されたハッシュ、およびカタログで使用されるアルゴリズムを分析できます。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク

[New-FileCatalog](New-FileCatalog.md)

[PowerShellGet](/powershell/module/PowerShellGet)
