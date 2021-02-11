---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 10/06/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-history?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-History
ms.openlocfilehash: be47925211c57760ddbd0bd4c8e985e161d24593
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600792"
---
# Get-History

## 概要
現在のセッション中に入力したコマンドの一覧を取得します。

## SYNTAX

```
Get-History [[-Id] <Int64[]>] [[-Count] <Int32>] [<CommonParameters>]
```

## Description

この `Get-History` コマンドレットは、セッション履歴 (現在のセッション中に入力されたコマンドの一覧) を取得します。

PowerShell では、各セッションの履歴が自動的に保持されます。 セッション履歴内のエントリの数は、ユーザー設定変数の値によって決まり `$MaximumHistoryCount` ます。 Windows PowerShell 3.0 以降では、既定値は `4096` です。 既定では、履歴ファイルはホーム ディレクトリに保存されますが、任意の場所にファイルを保存することができます。 PowerShell の履歴機能の詳細については、「 [about_History](About/about_History.md)」を参照してください。

セッション履歴は、 **Psreadline** モジュールによって保持されている履歴とは別に管理されます。
どちらの履歴も、 **Psreadline** が読み込まれたセッションで使用できます。 このコマンドレットは、セッション履歴でのみ機能します。 詳細については、「」、「 [about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)」を参照してください。

## 例

### 例 1: セッションの履歴を取得する

この例では、セッション履歴のエントリを取得します。 既定の表示では、各コマンドとその ID が表示されます。これは、実行された順序を示します。

```powershell
Get-History
```

### 例 2: 文字列を含むエントリを取得する

この例では、文字列サービスを含むコマンド履歴のエントリを取得します。 最初のコマンドは、セッション履歴のすべてのエントリを取得します。 パイプライン演算子 ( `|` ) によって結果がコマンドレットに渡され、 `Where-Object` サービスを含むコマンドのみが選択されます。

```powershell
Get-History | Where-Object {$_.CommandLine -like "*Service*"}
```

### 例 3: 履歴エントリを特定の ID までエクスポートする

この例では、エントリ7で終わる最新の5つの履歴エントリを取得します。 パイプライン演算子は、結果をコマンドレットに渡します。これにより、 `Export-Csv` 履歴がコンマ区切りのテキストとして書式設定され、History.csv ファイルに保存されます。 このファイルには、履歴を一覧として書式設定するときに表示されるデータが含まれています。 これには、コマンドの状態、開始時刻、終了時刻が含まれます。

```powershell
Get-History -ID 7 -Count 5 | Export-Csv History.csv
```

### 例 4: 最新のコマンドを表示する

この例では、コマンド履歴の最後のコマンドを取得します。 最後のコマンドは、最後に入力されたコマンドです。 このコマンドは、 **Count** パラメーターを使用して、1つのコマンドだけを表示します。 既定では、は `Get-History` 最新のコマンドを取得します。 このコマンドは "h -c 1" と省略でき、上方向キーを押すことと同等です。

```powershell
Get-History -Count 1
```

### 例 5: 履歴内のエントリのすべてのプロパティを表示する

この例では、セッション履歴内のエントリのすべてのプロパティを表示します。 パイプライン演算子はコマンドの結果を `Get-History` コマンドレットに渡し `Format-List` ます。これにより、各履歴エントリのすべてのプロパティが表示されます。 これには、コマンドの ID、状態、開始時刻と終了時刻が含まれます。

```powershell
Get-History | Format-List -Property *
```

## PARAMETERS

### -カウント

このコマンドレットが取得する最新の履歴エントリの数を指定します。 既定では、 `Get-History` セッション履歴のすべてのエントリを取得します。 コマンドに **Count** と **Id** の両方のパラメーターを使用した場合、表示は **Id** パラメーターで指定されたコマンドで終了します。

Windows PowerShell 2.0 では、既定では、 `Get-History` 32 の最新のエントリが取得されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Id

セッション履歴内のエントリの Id の配列を指定します。 `Get-History` 指定されたエントリのみを取得します。 コマンドで **id** と **カウント** の両方のパラメーターを使用する場合、は、 `Get-History` **id** パラメーターで指定されたエントリで終わる最新のエントリを取得します。

```yaml
Type: System.Int64[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Int64

このコマンドレットには、履歴 ID をパイプすることができます。

## 出力

### Microsoft. PowerShell. コマンドの履歴情報

このコマンドレットは、取得した各履歴項目の履歴オブジェクトを返します。

## 注

セッション履歴は、セッション中に入力したコマンドの一覧です。 セッション履歴は、コマンドの実行順序、状態、および開始時刻と終了時刻を表します。 各コマンドを入力すると、PowerShell によってそのコマンドが履歴に追加され、再利用できるようになります。 コマンド履歴の詳細については、「 [about_History](About/about_History.md)」を参照してください。

Windows PowerShell 3.0 以降では、ユーザー設定変数の既定値 `$MaximumHistoryCount` は `4096` です。 Windows PowerShell 2.0 では、既定値は `64` です。 変数の詳細については `$MaximumHistoryCount` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

## 関連リンク

[Add-History](Add-History.md)

[Clear-History](Clear-History.md)

[Invoke-History](Invoke-History.md)

[about_History](About/about_History.md)
