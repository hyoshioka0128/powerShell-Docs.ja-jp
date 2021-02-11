---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 02/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-location?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-Location
ms.openlocfilehash: ec6f69d1d4a0b382ceec26b9409d01501edb814f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602207"
---
# Set-Location

## 概要
現在の作業場所を、指定された場所に設定します。

## SYNTAX

### パス (既定値)

```
Set-Location [[-Path] <String>] [-PassThru] [<CommonParameters>]
```

### LiteralPath

```
Set-Location -LiteralPath <String> [-PassThru] [<CommonParameters>]
```

### スタック

```
Set-Location [-PassThru] [-StackName <String>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Set-Location` 作業場所を指定した場所に設定します。 この場所には、ディレクトリ、サブディレクトリ、レジストリの場所、または任意のプロバイダーパスを指定できます。

PowerShell 6.2 では、 `-` `+` **Path** パラメーターの値としておよびのサポートが追加されました。 PowerShell では、およびでアクセスできる最新の20個の場所の履歴が保持され `-` `+` ます。 このリストは、 **Stackname** パラメーターを使用してアクセスされる場所スタックからは独立しています。

## 例

### 例 1: 現在の場所を設定する

```
PS C:\> Set-Location -Path "HKLM:\"
PS HKLM:\>
```

このコマンドは、現在の場所をドライブのルートに設定し `HKLM:` ます。

### 例 2: 現在の場所を設定し、その場所を表示する

```
PS C:\> Set-Location -Path "Env:\" -PassThru
```

```Output
Path
----
Env:\

PS Env:\>
```

このコマンドは、現在の場所をドライブのルートに設定し `Env:` ます。 **PassThru** パラメーターを使用して、場所を表す **pathinfo** オブジェクトを返すように PowerShell に指示し `Env:\` ます。

### 例 3: 場所を C: ドライブの現在の場所に設定する

```powershell
PS C:\Windows\> Set-Location HKLM:\
PS HKLM:\> Set-Location C:
PS C:\Windows\>
```

最初のコマンドは、レジストリプロバイダーのドライブのルートに場所を設定し `HKLM:` ます。
2番目のコマンドは、場所を FileSystem プロバイダーのドライブの現在の場所に設定し `C:` ます。
(円記号を使用せずに) 形式でドライブ名を指定すると、コマンドレットによって `<DriveName>:` 場所が PSDrive 内の現在の場所に設定されます。
PSDrive use コマンドで現在の場所を取得し `Get-Location -PSDrive <DriveName>` ます。

### 例 4: 現在の場所を名前付きスタックに設定する

```
PS C:\> Push-Location -Path 'C:\Program Files\PowerShell\' -StackName "Paths"
PS C:\Program Files\PowerShell\> Set-Location -StackName "Paths"
PS C:\Program Files\PowerShell\> Get-Location -Stack
```

```Output
Path
----
C:\
```

最初のコマンドは、現在の場所をパススタックに追加します。
2番目のコマンドは、パスの場所スタックを現在の場所スタックにします。
3番目のコマンドは、現在の場所スタック内の場所を表示します。

`*-Location`コマンドレットは、コマンドで別の場所スタックが指定されていない限り、現在の場所スタックを使用します。 場所スタックの詳細については、「 [注](#notes)」を参照してください。

### 例 5: またはを使用して場所の履歴を移動する `+``-`

```
PS C:\> Set-Location -Path $env:SystemRoot
PS C:\Windows> Set-Location -Path Cert:\
PS Cert:\> Set-Location -Path HKLM:\
PS HKLM:\>

# Navigate back through the history using "-"
PS HKLM:\> Set-Location -Path -
PS Cert:\> Set-Location -Path -
PS C:\Windows>

# Navigate using the Set-Location alias "cd" and the implicit positional Path parameter
PS C:\Windows> cd -
PS C:\> cd +
PS C:\Windows> cd +
PS Cert:\>
```

エイリアスを使用 `cd -` すると、 `cd +` ターミナルでの場所の履歴を簡単に移動できます。 を使用した移動の詳細につい `-` / `+` ては、 **Path** パラメーターを参照してください。

## PARAMETERS

### -LiteralPath

場所のパスを指定します。 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

位置を表す **Pathinfo** オブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

新しい作業場所のパスを指定します。 パスが指定されていない場合、既定では `Set-Location` 現在のユーザーのホームディレクトリになります。 ワイルドカードを使用すると、コマンドレットは、ワイルドカードパターンに一致する最初のパスを選択します。

PowerShell では、最後に設定した20個の場所の履歴が保持されます。 **Path** パラメーターの値が文字の場合 `-` 、新しい作業場所は履歴内の前の作業場所 (存在する場合) になります。 同様に、値が文字の場合、 `+` 新しい作業場所は履歴内の次の作業場所 (存在する場合) になります。 これは、およびを使用する場合と似ていますが、履歴はスタックではなくリストであり、 `Pop-Location` `Push-Location` 暗黙的に追跡され、手動で制御される点が異なります。 現在、履歴一覧を表示する方法はありません。

```yaml
Type: System.String
Parameter Sets: Path
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -StackName

このコマンドレットによって現在の場所スタックが作成される既存の場所スタック名を指定します。 場所スタック名を入力します。 名前のない既定の場所スタックを示すに `$null` は、または空の文字列 () を入力 `""` します。

`*-Location` **Stackname** パラメーターを使用して別のスタックを指定しない限り、コマンドレットは現在のスタックに対して動作します。 場所スタックの詳細については、「 [注](#notes)」を参照してください。

```yaml
Type: System.String
Parameter Sets: Stack
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用してパスを含む文字列をこのコマンドレットに送ることができます。

## 出力

### なし、システムの管理.......................... PathInfoStack

**PassThru** パラメーターを指定しない限り、このコマンドレットは出力を生成しません。 **パススルー** と **Path** または **LiteralPath** を使用すると、新しい場所を表す **pathinfo** オブジェクトが生成されます。 **Stackname** と共に **PassThru** を使用すると、新しいスタックコンテキストを表す **pathinfostack** オブジェクトが生成されます。

## 注

PowerShell では、プロセスごとに複数の実行空間がサポートされます。 各実行空間には、独自の _現在のディレクトリ_ があります。
これはと同じではありません `[System.Environment]::CurrentDirectory` 。 この動作は、明示的なディレクトリパスを指定せずに、.NET Api を呼び出したり、ネイティブアプリケーションを実行したりするときに問題になることがあります。

場所のコマンドレットでプロセス全体の現在のディレクトリが設定されていても、別の実行空間によっていつでも変更される可能性があるため、このディレクトリに依存することはできません。 現在の実行空間に固有の現在の作業ディレクトリを使用して、パスベースの操作を実行するには、location コマンドレットを使用する必要があります。

`Set-Location`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/about/about_Providers.md)」を参照してください。

スタックとは、最後に追加された項目だけにアクセスできる、後入れ先出しのリストです。 使用した順序でスタックに項目を追加し、その後、逆の順序で使用するために項目を取得します。 PowerShell では、プロバイダーの場所を場所スタックに格納できます。 PowerShell によって、名前のない既定の場所スタックが作成されます。 複数の名前付きの場所スタックを作成できます。 スタック名を指定しない場合、PowerShell は現在の場所スタックを使用します。 既定では、名前のない既定の場所は現在の場所スタックですが、コマンドレットを使用して `Set-Location` 現在の場所スタックを変更できます。

場所スタックを管理するには、次のように `*-Location` コマンドレットを使用します。

- 場所スタックに場所を追加するには、コマンドレットを使用し `Push-Location` ます。

- 場所スタックから場所を取得するには、コマンドレットを使用し `Pop-Location` ます。

- 現在の場所スタック内の場所を表示するには、コマンドレットの **stack** パラメーターを使用し `Get-Location` ます。 名前付きの場所スタック内の場所を表示するには、の **Stackname** パラメーターを使用し `Get-Location` ます。

- 新しい場所スタックを作成するには、の **Stackname** パラメーターを使用し `Push-Location` ます。 存在しないスタックを指定すると、によっ `Push-Location` てスタックが作成されます。

- 場所スタックを現在の場所スタックにするには、の **Stackname** パラメーターを使用し `Set-Location` ます。

名前のない既定の場所スタックは、現在の場所スタックになっている場合にだけ、完全にアクセスできます。
名前付きの場所スタックを現在の場所スタックにすると、またはコマンドレットを使用して `Push-Location` `Pop-Location` 既定のスタックから項目を追加または取得したり、コマンドレットを使用して名前のないスタック内の場所を表示したりすることができなくなり `Get-Location` ます。 名前のないスタックを現在のスタックにするには、コマンドレットの **Stackname** パラメーター `Set-Location` の値を `$null` または空の文字列 () に設定し `""` ます。

## 関連リンク

[Get-Location](Get-Location.md)

[Pop-Location](Pop-Location.md)

[Push-Location](Push-Location.md)

