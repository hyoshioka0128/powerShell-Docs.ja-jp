---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 03/22/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/test-path?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-Path
ms.openlocfilehash: a79f12739643a873703b39d9d07e8b7db01dfbb4
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599923"
---
# Test-Path

## 概要
パスのすべての要素が存在するかどうかを確認します。

## SYNTAX

### パス (既定値)

```
Test-Path [-Path] <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>]
 [-PathType <TestPathType>] [-IsValid] [-Credential <PSCredential>]
 [-OlderThan <DateTime>] [-NewerThan <DateTime>] [<CommonParameters>]
```

### LiteralPath

```
Test-Path -LiteralPath <String[]> [-Filter <String>] [-Include <String[]>] [-Exclude <String[]>]
 [-PathType <TestPathType>] [-IsValid] [-Credential <PSCredential>]
 [-OlderThan <DateTime>] [-NewerThan <DateTime>] [<CommonParameters>]
```

## Description

この `Test-Path` コマンドレットは、パスのすべての要素が存在するかどうかを確認します。 すべての `$True` 要素が存在し、不足している場合はを返し `$False` ます。 また、パスの構文が有効かどうか、およびパスがコンテナーまたはターミナルまたはリーフの要素を指しているかどうかを判断することもできます。 が空白文字の場合は、 `Path` `$False` が返されます。 `Path`が、配列 `$null` の配列または空の配列の場合は、 `$null` 終了しないエラーが返されます。

## 例

### 例 1: パスをテストする

```powershell
Test-Path -Path "C:\Documents and Settings\DavidC"
```

```Output
True
```

このコマンドは、パス内のすべての要素 (つまり、C: ディレクトリ、Documents and Settings ディレクトリ、および DavidC ディレクトリ) が存在するかどうかを確認します。 不足している場合、コマンドレットはを返し `$False` ます。
それ以外の場合は `$True`を返します。

### 例 2: プロファイルのパスをテストする

```powershell
Test-Path -Path $profile
```

```Output
False
```

```powershell
Test-Path -Path $profile -IsValid
```

```Output
True
```

これらのコマンドは、PowerShell プロファイルのパスをテストします。

最初のコマンドは、パス中のすべての要素が存在するかどうかを判断します。 2 番目のコマンドは、パスの構文が正しいかどうかを確認します。 この場合、パスはですが、 `$False` 構文は正しい `$True` です。 これらのコマンドは、プロファイルが `$profile` 存在しない場合でも、プロファイルの場所を指す自動変数を使用します。

自動変数の詳細については、「about_Automatic_Variables」を参照してください。

### 例 3: 指定された種類以外のファイルがあるかどうかを確認する

```powershell
Test-Path -Path "C:\CAD\Commercial Buildings\*" -Exclude *.dwg
```

```Output
False
```

このコマンドは、ファイルが .dwg ファイル以外の商用施設ディレクトリにあるかどうかを確認します。

このコマンドは path **パラメーターを使用して** パスを指定します。 パスにはスペースが含まれているため、パスは引用符で囲みます。 パスの末尾のアスタリスクは、Commercial Building ディレクトリの内容を表します。 このような長いパスでは、パスの最初の数文字を入力し、TAB キーを使用してパスを完成させます。

このコマンドは、 **Exclude** パラメーターを指定して、評価から除外されるファイルを指定します。

この場合、ディレクトリには .dwg ファイルしか含まれていないため、結果はになり `$False` ます。

### 例 4: ファイルの確認

```powershell
Test-Path -Path $profile -PathType leaf
```

```Output
True
```

このコマンドは、変数に格納されているパスがファイルを指しているかどうかを確認し `$profile` ます。 この場合、PowerShell プロファイルがファイルであるため、 `.ps1` コマンドレットはを返し `$True` ます。

### 例 5: レジストリのパスを確認する

これらのコマンド `Test-Path` は、PowerShell レジストリプロバイダーと共にを使用します。

最初のコマンドは、 **Microsoft PowerShell** レジストリキーのレジストリパスがシステム上で正しいかどうかをテストします。 PowerShell が正しくインストールされている場合、コマンドレットはを返し `$True` ます。

> [!IMPORTANT]
> `Test-Path` は、すべての PowerShell プロバイダーで正しく機能しません。 たとえば、を使用してレジストリキーのパスをテストすることはできますが、レジストリエントリ `Test-Path` が存在する場合でも、それを使用してレジストリエントリのパスをテストすると、常にを返し `$False` ます。

```powershell
Test-Path -Path "HKLM:\Software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell"
```

```Output
True
```

```powershell
Test-Path -Path "HKLM:\Software\Microsoft\PowerShell\1\ShellIds\Microsoft.PowerShell\ExecutionPolicy"
```

```Output
False
```

### 例 6: ファイルが指定した日付より新しいかどうかをテストする

このコマンドは、 **Newerthan** dynamic パラメーターを使用して、コンピューター上の "PowerShell.exe" ファイルが2009年7月13日より新しいかどうかを判断します。

NewerThan パラメーターはファイル システム ドライブでのみ有効です。

```powershell
Test-Path $pshome\PowerShell.exe -NewerThan "July 13, 2009"
```

```Output
True
```

### 例 7: 値として null でパスをテストする

配列の配列または空の配列に対して返されたエラーは、 `null` `null` 終了しないエラーです。 を使用して抑制することができ `-ErrorAction SilentlyContinue` ます。 次の例は、エラーを返すすべてのケースを示して `NullPathNotPermitted` います。

```powershell
Test-Path $null
Test-Path $null, $null
Test-Path @()
```

```Output
Test-Path : Cannot bind argument to parameter 'Path' because it is null.
At line:1 char:11
+ Test-Path $null
+           ~~~~~
    + CategoryInfo          : InvalidData: (:) [Test-Path], ParameterBindingValidationException
    + FullyQualifiedErrorId : ParameterArgumentValidationErrorNullNotAllowed,Microsoft.PowerShell.Commands.TestPathCommand
```

### 例 8: 値として空白を含むパスをテストする

パラメーターに空白または空の文字列を指定すると `-Path` 、 **False** が返されます。
次の例では、空白と空の文字列を示します。

```powershell
Test-Path ' '
Test-Path ''
```

```Output
False
False
```

## PARAMETERS

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、 [Invoke コマンド](../Microsoft.PowerShell.Core/Invoke-Command.md)を使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -除外

このコマンドレットで省略される項目を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 「*.txt」などのパス要素またはパターンを入力します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Filter

プロバイダーの形式または言語でフィルターを指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 ワイルドカード文字の使用など、フィルターの構文はプロバイダーによって異なります。 フィルターは他のパラメーターよりも効率的です。これは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、オブジェクトを取得するときにプロバイダーによって適用されるためです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Include

このコマンドレットによってテストされるパスを指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 「*.txt」などのパス要素またはパターンを入力します。 ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -IsValid

パスの要素が存在するかどうかに関係なく、このコマンドレットがパスの構文をテストすることを示します。 このコマンドレットは `$True` 、パスの構文が有効である場合はを返し、そうでない場合はを返し `$False` ます。

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

### -LiteralPath

テストするパスを指定します。 **Path** と異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 PowerShell でエスケープシーケンスとして解釈される可能性のある文字がパスに含まれている場合は、そのパスを単一引用符で囲み、解釈されないようにする必要があります。

```yaml
Type: System.String[]
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NewerThan

時刻を **DateTime** オブジェクトとして指定します。

```yaml
Type: System.Nullable`1[System.DateTime]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OlderThan

時刻を **DateTime** オブジェクトとして指定します。

```yaml
Type: System.Nullable`1[System.DateTime]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

テストするパスを指定します。 ワイルドカード文字を使用できます。 パスにスペースが含まれる場合は、二重引用符で囲みます。

```yaml
Type: System.String[]
Parameter Sets: Path
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -PathType

パスの最後の要素の型を指定します。 このコマンドレットは、指定された型の要素の場合はを返し、そうでない場合はを返し `$True` `$False` ます。 このパラメーターの有効値は、次のとおりです。

- コンテナー。
  ディレクトリやレジストリ キーのように、他の要素を含む要素です。
- リーフ.
  ファイルのように、他の要素を含まない要素です。
- いつ.
  コンテナーまたはリーフのどちらかです。

パスの最終要素が特定の種類かどうかを示します。

> [!CAUTION]
>
> PowerShell バージョン6.1.2 までは、 **IsValid** と **pathtype** スイッチが一緒に指定されている場合、 `Test-Path` コマンドレットは **pathtype** スイッチを無視し、パスの種類を検証せずに構文パスのみを検証します。
>
> [問題 #8607](https://github.com/PowerShell/PowerShell/issues/8607)によれば、この動作を修正することは将来のバージョンでは互換性に影響する変更になる可能性があります。これは、 **IsValid** および **pathtype** スイッチが個別のパラメーターセットに属しているため、この混乱を避けるために一緒に使用することはできません。

```yaml
Type: Microsoft.PowerShell.Commands.TestPathType
Parameter Sets: (All)
Aliases: Type
Accepted values: Any, Container, Leaf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### System.String

パイプを使用してパスを含む文字列をこのコマンドレットに送ることができます。

## 出力

### System.Boolean

コマンドレットでは、 **ブール** 値を返します。

## 注

**パスの名詞を** 含むコマンドレット ( **path** コマンドレット) は、パス名と共に使用され、すべての PowerShell プロバイダーが解釈できる簡潔な形式の名前を返します。 プログラムやスクリプトで、パス名の全部または一部を特定の形式で表示するために使用することを目的としています。
これは、 **Dirname**、 **Normpath**、 **realpath**、 **Join**、またはその他のパスマニピュレーターを使用する場合と同じように使用します。

`Test-Path`は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Convert-Path](Convert-Path.md)

[Join-Path](Join-Path.md)

[Resolve-Path](Resolve-Path.md)

[Split-Path](Split-Path.md)
