---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/tee-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Tee-Object
ms.openlocfilehash: 5fd1ff9760cbddeb0c5c29052caf0f36d6d760d0
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603224"
---
# Tee-Object

## 概要
ファイルまたは変数にコマンドの出力を保存し、またそれをパイプラインに送信します。

## SYNTAX

### File (既定値)

```
Tee-Object [-InputObject <PSObject>] [-FilePath] <String> [-Append] [<CommonParameters>]
```

### LiteralFile

```
Tee-Object [-InputObject <PSObject>] -LiteralPath <String> [<CommonParameters>]
```

### 変数

```
Tee-Object [-InputObject <PSObject>] -Variable <String> [<CommonParameters>]
```

## Description

`Tee-Object`コマンドレットは、出力をリダイレクトします。つまり、コマンドの出力を2方向 (文字 T など) で送信します。 出力は、ファイルまたは変数に保存され、パイプラインにも送信されます。 がパイプラインの最後のコマンドである場合は、 `Tee-Object` コマンドの出力がプロンプトに表示されます。

## 例

### 例 1: ファイルとコンソールにプロセスを出力する

この例では、コンピューター上で実行されているプロセスの一覧を取得し、その結果をファイルに送信します。
2 番目のパスを指定していないため、プロセスはコンソールに表示されます。

```powershell
Get-Process | Tee-Object -FilePath "C:\Test1\testfile2.txt"
```

```Output
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)    Id ProcessName
-------  ------    -----      ----- -----   ------    -- -----------
83       4     2300       4520    39     0.30    4032 00THotkey
272      6     1400       3944    34     0.06    3088 alg
81       3      804       3284    21     2.45     148 ApntEx
81       4     2008       5808    38     0.75    3684 Apoint
...
```

### 例 2: 変数にプロセスを出力する `Select-Object`

この例では、コンピューター上で実行されているプロセスの一覧を取得し、変数に保存 `$proc` して、にパイプ処理し `Select-Object` ます。

```powershell
Get-Process notepad | Tee-Object -Variable proc | Select-Object processname,handles
```

```Output
ProcessName                              Handles
-----------                              -------
notepad                                  43
notepad                                  37
notepad                                  38
notepad                                  38
```

`Select-Object`コマンドレットでは、 **ProcessName** を選択し、プロパティを **処理** します。 変数には、 `$proc` によって返される既定の情報が含まれていることに注意して `Get-Process` ください。

### 例 3: 2 つのログファイルにシステムファイルを出力する

この例では、システムファイルの一覧を、累積ファイルと現在のファイルの2つのログファイルに保存します。

```powershell
Get-ChildItem -Path D: -File -System -Recurse |
  Tee-Object -FilePath "c:\test\AllSystemFiles.txt" -Append |
    Out-File c:\test\NewSystemFiles.txt
```

このコマンドは、コマンドレットを使用して、 `Get-ChildItem` D: ドライブ上のシステムファイルを再帰的に検索します。 パイプライン演算子 (|) によってにリストが送信され `Tee-Object` 、AllSystemFiles.txt ファイルにリストが追加され、コマンドレットに一覧が渡されます。この一覧は、 `Out-File` NewSystemFiles.txt ファイルに保存されます。

## PARAMETERS

### -追加

コマンドレットによって、指定したファイルに出力が追加されることを示します。 このパラメーターを指定しない場合、ファイル内の既存の内容が警告なしで新しい内容に置き換えられます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: File
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

このコマンドレットがオブジェクトをワイルドカード文字に保存するファイルを指定しますが、1つのファイルに解決する必要があります。

```yaml
Type: System.String
Parameter Sets: File
Aliases: Path

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -InputObject

保存されて表示されるオブジェクトを指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。 パイプを使用してオブジェクトをにパイプすることもでき `Tee-Object` ます。

で **inputobject** パラメーターを使用すると `Tee-Object` 、コマンドの結果をにパイプするのではなく、 `Tee-Object` 値がコレクションの場合でも、 **inputobject** 値は単一のオブジェクトとして扱われます。

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

### -LiteralPath

このコマンドレットでオブジェクトを保存するファイルを指定します。 **FilePath** とは異なり、 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: LiteralFile
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Variable

コマンドレットによってオブジェクトが保存される変数を指定します。 前のドル記号 () を付けずに変数名を入力し `$` ます。

```yaml
Type: System.String
Parameter Sets: Variable
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用してオブジェクトをにパイプ処理でき `Tee-Object` ます。

## 出力

### システム管理. PSObject

`Tee-Object` リダイレクトするオブジェクトを返します。

## 注

コマンドレットまたはリダイレクト演算子を使用することもできます `Out-File` 。どちらも、出力をファイルに保存しますが、パイプラインには送信しません。

PowerShell 6 以降、で `Tee-Object` は、ファイルへの書き込み時に、BOM のない utf-8 エンコードが使用されます。 別のエンコーディングが必要な場合は、 `Out-File` **encoding** パラメーターを指定してコマンドレットを使用します。

## 関連リンク

[Compare-Object](Compare-Object.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Group-Object](Group-Object.md)

[Measure-Object](Measure-Object.md)

[New-Object](New-Object.md)

[Select-Object](Select-Object.md)

[Sort-Object](Sort-Object.md)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

