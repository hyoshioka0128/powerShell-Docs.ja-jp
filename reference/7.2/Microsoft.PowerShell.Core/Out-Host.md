---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 08/07/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/out-host?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Out-Host
ms.openlocfilehash: 4fefb133416b3db6c19c71a916d73fe00f86f3a4
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599713"
---
# Out-Host

## 概要
コマンド ラインに出力を送信します。

## SYNTAX

### すべて

```
Out-Host [-Paging] [-InputObject <PSObject>] [<CommonParameters>]
```

## Description

`Out-Host`コマンドレットは、出力を PowerShell ホストに送信して表示します。 ホストは、コマンド ラインに出力を表示します。 `Out-Host`が既定値であるため、パラメーターを使用する場合を除き、指定する必要はありません。

`Out-Host` は、実行されるすべてのコマンドに自動的に追加されます。 パイプラインの出力を、コマンドを実行しているホストに渡します。 `Out-Host` は、ANSI エスケープシーケンスを無視します。 エスケープシーケンスはホストによって処理されます。 `Out-Host` は、ANSI エスケープシーケンスを解釈または変更せずにホストに渡します。

## 例

### 例 1: 出力を1ページずつ表示する

この例では、システムによって一度に1ページずつ処理されます。

```powershell
Get-Process | Out-Host -Paging
```

```Output
NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     30    24.12      36.95      15.86   21004  14 ApplicationFrameHost
     55    24.33      60.48      10.80   12904  14 BCompare
<SPACE> next page; <CR> next line; Q quit
      9     4.71       8.94       0.00   16864  14 explorer
<SPACE> next page; <CR> next line; Q quit
```

`Get-Process` システムプロセスを取得し、オブジェクトをパイプラインの下に送信します。 `Out-Host`**ページング** パラメーターを使用して、一度に1ページのデータを表示します。

### 例 2: 入力として変数を使用する

この例では、変数に格納されているオブジェクトをの入力として使用 `Out-Host` します。

```powershell
$io = Get-History
Out-Host -InputObject $io
```

`Get-History` PowerShell セッションの履歴を取得し、そのオブジェクトを変数に格納し `$io` ます。
`Out-Host`**InputObject** パラメーターを使用して変数を指定 `$io` し、履歴を表示します。

## PARAMETERS

### -InputObject

コンソールに書き込まれるオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -ページング

は `Out-Host` 、一度に1ページの出力を表示し、残りのページが表示される前にユーザー入力を待機することを示します。 既定では、すべての出力が1ページに表示されます。 ページ サイズは、ホストの特性によって決まります。

<kbd>スペース</kbd>バーを押すと、次の出力ページが表示されます。 <kbd>enter</kbd>キーを押すと、次の出力行が表示されます。 <kbd>Q</kbd>キーを押して終了します。

**ページング** は、 **more** コマンドに似ています。

> [!NOTE]
> **ページング** パラメーターは、PowerShell ISE ホストではサポートされていません。

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

### システム管理. PSObject

オブジェクトをパイプラインからに送信でき `Out-Host` ます。

## 出力

### なし

`Out-Host` 出力を生成しません。 表示するために、オブジェクトをホストに送信します。

## 注

**ページング** パラメーターは、すべての PowerShell ホストではサポートされていません。 たとえば、PowerShell ISE で **Paging** パラメーターを使用すると、次のエラーが表示されます。 `out-lineoutput : The method or operation is not implemented.`

**Out** 動詞を含むコマンドレットは、 `Out-` オブジェクトを書式設定しません。 オブジェクトをレンダリングし、指定された表示先に送信します。 書式設定されていないオブジェクトをコマンドレットに送信すると、コマンドレットによって、 `Out-` レンダリングされる前に書式設定コマンドレットに送信されます。

`Out-`コマンドレットには、名前またはファイルパスのパラメーターがありません。 コマンドレットにデータを送信するには、パイプラインを使用して `Out-` PowerShell コマンドの出力をコマンドレットに送信します。 または、変数にデータを格納し、 **InputObject** パラメーターを使用してデータをコマンドレットに渡すことができます。

`Out-Host` データを送信しますが、出力オブジェクトは生成しません。 の出力を `Out-Host` コマンドレットにパイプラインすると `Get-Member` 、は `Get-Member` オブジェクトが指定されていないことを報告します。

## 関連リンク

[Clear-Host](Clear-Host.md)

[Out-Default](Out-Default.md)

[Out-File](../Microsoft.PowerShell.Utility/Out-File.md)

[Out-Null](Out-Null.md)

[Out-Printer](../Microsoft.PowerShell.Utility/Out-Printer.md)

[Out-String](../Microsoft.PowerShell.Utility/Out-String.md)

[Write-Host](../Microsoft.PowerShell.Utility/Write-Host.md)

