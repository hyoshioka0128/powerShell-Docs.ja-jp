---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 05/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/add-history?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Add-History
ms.openlocfilehash: d2c0abb0e6742de3e316fe964d5945375591ef4d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604938"
---
# Add-History

## 概要
セッションの履歴にエントリを追加します。

## SYNTAX

```
Add-History [[-InputObject] <PSObject[]>] [-Passthru] [<CommonParameters>]
```

## Description

この `Add-History` コマンドレットは、セッション履歴の末尾 (現在のセッション中に入力されたコマンドの一覧) にエントリを追加します。

セッション履歴は、セッション中に入力したコマンドの一覧です。 セッション履歴は、実行の順序、状態、コマンドの開始時刻と終了時刻を表します。 各コマンドを入力すると、PowerShell によってそのコマンドが履歴に追加され、再利用できるようになります。 セッションの履歴の詳細については、「 [about_History](About/about_History.md)」を参照してください。

セッション履歴は、 **Psreadline** モジュールによって保持されている履歴とは別に管理されます。
どちらの履歴も、 **Psreadline** が読み込まれたセッションで使用できます。 このコマンドレットは、セッション履歴でのみ機能します。 詳細については、「」、「 [about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)」を参照してください。

コマンドレットを使用する `Get-History` と、コマンドを取得してに渡すことができます `Add-History` 。また、コマンドを CSV または XML ファイルにエクスポートしてから、コマンドをインポートして、インポートしたファイルをに渡すこともできます `Add-History` 。 このコマンドレットを使用して、履歴に特定のコマンドを追加するか、または複数のセッションからのコマンドを含む 1 つの履歴ファイルを作成できます。

## 例

### 例 1: 別のセッションの履歴にコマンドを追加する

この例では、1つの PowerShell セッションで入力したコマンドを別の PowerShell セッションの履歴に追加します。

```powershell
Get-History | Export-Csv c:\testing\history.csv -IncludeTypeInformation
Import-Csv c:\testing\history.csv | Add-History
```

最初のコマンドは、履歴内のコマンドを表すオブジェクトを取得し、それらをファイルにエクスポートし `History.csv` ます。

2 番目のコマンドは、別のセッションのコマンド ラインで入力します。 コマンドレットを使用して、 `Import-Csv` ファイル内のオブジェクトをインポートし `History.csv` ます。 パイプライン演算子 () は、 `|` オブジェクトをコマンドレットに渡します。これにより、 `Add-History` ファイル内のコマンドを表すオブジェクトが `History.csv` 現在のセッション履歴に追加されます。

### 例 2: コマンドをインポートして実行する

この例では、ファイルからコマンドをインポートし `History.xml` 、それを現在のセッション履歴に追加して、結合された履歴のコマンドを実行します。

```powershell
Import-Clixml c:\temp\history.xml | Add-History -PassThru | ForEach-Object -Process {Invoke-History}
```

最初のコマンドは、コマンドレットを使用して、 `Import-Clixml` ファイルにエクスポートされたコマンドの履歴をインポートし `History.xml` ます。 パイプライン演算子はコマンドレットにコマンドを渡し `Add-History` 、現在のセッション履歴にコマンドを追加します。 **PassThru** パラメーターは、追加されたコマンドを表すオブジェクトをパイプラインに渡します。

次に、コマンドレットを使用して、結合され `ForEach-Object` `Invoke-History` た履歴の各コマンドにコマンドを適用します。 コマンドは、コマンド `Invoke-History` `{}` レットの **Process** パラメーターによって求められるように、かっこ () で囲まれたスクリプトブロックとして書式設定され `ForEach-Object` ます。

### 例 3: 履歴のコマンドを履歴の最後に追加する

この例では、履歴の最初の5つのコマンドを履歴リストの最後に追加します。

```powershell
Get-History -Id 5 -Count 5 | Add-History
```

`Get-History`コマンドレットは、コマンド5で終了する5つのコマンドを取得します。 パイプライン演算子は、それらをコマンドレットに渡して、 `Add-History` 現在の履歴に追加します。 コマンドには `Add-History` パラメーターが含まれていませんが、PowerShell は、パイプラインを通じて渡されたオブジェクトをの **InputObject** パラメーターと関連付け `Add-History` ます。

### 例 4: 現在の履歴に .csv ファイルのコマンドを追加する

この例では、ファイル内のコマンドを `History.csv` 現在のセッション履歴に追加します。

```powershell
$a = Import-Csv c:\testing\history.csv
Add-History -InputObject $a -PassThru
```

コマンド `Import-Csv` レットは、ファイル内のコマンドをインポート `History.csv` し、その内容を変数に格納し `$a` ます。

2番目のコマンドは、コマンドレットを使用して、 `Add-History` から現在のセッション履歴にコマンドを追加し `History.csv` ます。 **InputObject** パラメーターを使用して変数を指定し、PassThru パラメーターを使用して `$a` コマンドラインに表示するオブジェクトを生成します。  **PassThru** パラメーターを指定しない場合、 `Add-History` コマンドレットは出力を生成しません。

### 例 5: 現在の履歴に .xml ファイルのコマンドを追加する

この例では、ファイル内のコマンドを `history.xml` 現在のセッション履歴に追加します。

```powershell
Add-History -InputObject (Import-Clixml c:\temp\history.xml)
```

**InputObject** パラメーターを指定すると、コマンドの結果がかっこで囲まれてコマンドレットに渡され `Add-History` ます。 最初に実行されるかっこ内のコマンドは、 `history.xml` ファイルを PowerShell にインポートします。 次に、コマンドレットは、 `Add-History` セッション履歴にファイル内のコマンドを追加します。

## PARAMETERS

### -InputObject

履歴 **情報** オブジェクトとしてセッション履歴に追加するエントリの配列を指定します。
このパラメーターを使用して `Get-History` 、、 `Import-Clixml` 、またはコマンドレットによって返された履歴情報オブジェクトをに送信でき `Import-Csv` `Add-History` ます。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Passthru

このコマンドレットが各履歴エントリの履歴 **情報** オブジェクトを返すことを示します。 既定では、このコマンドレットによる出力はありません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### Microsoft. PowerShell. コマンドの履歴情報

このコマンドレットには、 **履歴情報** オブジェクトをパイプすることができます。

## 出力

### None または Microsoft. PowerShell. History Info

**PassThru** パラメーターを指定した場合、このコマンドレットは **履歴情報** オブジェクトを返します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

セッション履歴は、セッション中に ID と共に入力されたコマンドの一覧です。 セッション履歴は、実行の順序、状態、コマンドの開始時刻と終了時刻を表します。 各コマンドを入力すると、PowerShell によってそのコマンドが履歴に追加され、再利用できるようになります。 セッションの履歴の詳細については、「 [about_History](About/about_History.md)」を参照してください。

履歴に追加するコマンドを指定するには、**InputObject** パラメーターを使用します。 コマンドは、コマンド `Add-History` レットによって各コマンドに対して返された **履歴情報** オブジェクトなどを受け入れ `Get-History` ます。 パスとファイル名、またはコマンドの一覧を渡すことはできません。

**InputObject** パラメーターを使用して、**履歴情報** オブジェクトのファイルをに渡すことができ `Add-History` ます。 これを行うには、またはコマンドレットを使用してコマンドの結果を `Get-History` ファイルにエクスポート `Export-Csv` してから、 `Export-Clixml` またはコマンドレットを使用してファイルをインポートし `Import-Csv` `Import-Clixml` ます。 その後、インポートされた **履歴情報** オブジェクトのファイルを `Add-History` パイプラインまたは変数でに渡すことができます。 詳細については、例を参照してください。

コマンドレットに渡す **履歴情報** オブジェクトのファイルには、 `Add-History` 型情報、列見出し、および **履歴情報** オブジェクトのすべてのプロパティが含まれている必要があります。 オブジェクトをに再び渡す場合は `Add-History` 、コマンドレットの **Notypeinformation** パラメーターを使用せず、 `Export-Csv` ファイル内の型情報、列見出し、またはフィールドを削除しないでください。

セッションの履歴を変更するには、CSV ファイルまたは XML ファイルにセッションをエクスポートし、ファイルを変更して、ファイルをインポートし、を使用し `Add-History` て現在のセッション履歴に追加します。

## 関連リンク

[Clear-History](Clear-History.md)

[Get-History](Get-History.md)

[Invoke-History](Invoke-History.md)

[about_PSReadLine](../PSReadLine/About/about_PSReadLine.md)

[Get-PSReadLineOption](/powershell/module/psreadline/get-psreadlineoption)

[設定-PSReadLineOption](/powershell/module/psreadline/set-psreadlineoption)

