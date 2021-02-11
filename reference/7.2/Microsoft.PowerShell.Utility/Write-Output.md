---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Output
ms.openlocfilehash: 7e0c958201dbd1b42d1f4c4ee286b8aca1f8595a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603026"
---
# Write-Output

## 概要
指定されたオブジェクトをパイプラインの次のコマンドに送信します。 そのコマンドがパイプラインの最後のコマンドである場合、オブジェクトはコンソールに表示されます。

## SYNTAX

```
Write-Output [-InputObject] <PSObject[]> [-NoEnumerate] [<CommonParameters>]
```

## Description

`Write-Output`コマンドレットは、指定されたオブジェクトをパイプライン内の次のコマンドに送信します。
そのコマンドがパイプラインの最後のコマンドである場合、オブジェクトはコンソールに表示されます。

`Write-Output` オブジェクトをプライマリパイプラインに送信します。 "出力ストリーム" または "成功パイプライン" とも呼ばれます。 エラー オブジェクトをエラー パイプラインを送信するには、Write-Error を使用します。

このコマンドレットは通常、コンソールに文字列およびその他のオブジェクトを表示するためにスクリプトで使用されます。 の組み込みエイリアスの1つはで、を `Write-Output` `echo` 使用する他のシェルに似ていますが、 `echo` 既定の動作では、パイプラインの最後に出力が表示されます。 PowerShell では、通常、出力が既定で表示されるインスタンスではコマンドレットを使用する必要はありません。 たとえば、`Get-Process | Write-Output` は、`Get-Process` と同じです。 または、を `echo "Home directory: $HOME"` 書き込むことができ `"Home directory: $HOME"` ます。

既定では、は、 `Write-Output` コマンドレットに指定されたコレクションを列挙します。 ただし、を `Write-Output` 使用して、 **noenumerate** パラメーターを使用してコレクションを1つのオブジェクトとして渡すこともできます。

## 例

### 例 1: オブジェクトを取得し、コンソールに書き込む

```powershell
$P = Get-Process
Write-Output $P
```

最初のコマンドは、コンピューター上で実行されているプロセスを取得し、変数に格納し `$P` ます。

2番目と3番目のコマンドは、コンソールののプロセスオブジェクトを表示し `$P` ます。

### 例 2: 別のコマンドレットに出力を渡す

```powershell
Write-Output "test output" | Get-Member
```

このコマンドは、パイプを使用して文字列が `Get-Member` 渡されたことを示す system.string クラスのメンバーを表示する、"test output" という文字列をコマンドレットに渡します。

### 例 3: 出力の列挙を抑制する

```powershell
Write-Output 1,2,3 | Measure-Object
```

```Output
Count    : 3
...
```

```powershell
Write-Output 1,2,3 -NoEnumerate | Measure-Object
```

```Output
Count    : 1
...
```

このコマンドは、 **Noenumerate** パラメーターを追加して、コレクションまたは配列をパイプラインを介して1つのオブジェクトとして扱います。

## PARAMETERS

### -InputObject

パイプラインに送信するオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

```yaml
Type: System.Management.Automation.PSObject[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -NoEnumerate

既定では、 `Write-Output` コマンドレットは常に出力を列挙します。 **Noenumerate** パラメーターを指定すると、既定の動作が抑制され、が出力を列挙できなくなり `Write-Output` ます。 かっこによって列挙が強制されるため、 **Noenumerate** パラメーターは、コマンドがかっこで囲まれている場合には効果がありません。 たとえば、は `(Write-Output 1,2,3)` 引き続き配列を列挙します。

> [!NOTE]
> このスイッチは、PowerShell Core 6.2 以降でのみ正常に動作します。 以前のバージョンの PowerShell Core では、このスイッチを使用してもコレクションは引き続き列挙されます。

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

パイプを使用してオブジェクトをにパイプ処理でき `Write-Output` ます。

## 出力

### システム管理. PSObject

`Write-Output` 入力として送信されたオブジェクトを返します。

## 注

## 関連リンク

[about_Output_Streams](../Microsoft.PowerShell.Core/About/about_Output_Streams.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Tee-Object](Tee-Object.md)

[Write-Debug](Write-Debug.md)

[Write-Error](Write-Error.md)

[Write-Host](Write-Host.md)

[Write-Information](Write-Information.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)
