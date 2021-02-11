---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 02/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/push-location?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Push-Location
ms.openlocfilehash: eaa7622e29680de4228579f8b77a6c829a586bf8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601223"
---
# Push-Location

## 概要
現在の場所を、場所スタックの最上部に追加します。

## SYNTAX

### パス (既定値)

```
Push-Location [[-Path] <String>] [-PassThru] [-StackName <String>] [<CommonParameters>]
```

### LiteralPath

```
Push-Location [-LiteralPath <String>] [-PassThru] [-StackName <String>] [<CommonParameters>]
```

## Description

`Push-Location`コマンドレットは、現在の場所を場所スタックに追加 ("プッシュ") します。 パスを指定すると、は `Push-Location` 現在の場所を場所スタックにプッシュし、現在の場所をパスで指定された場所に変更します。 コマンドレットを使用して `Pop-Location` 、場所スタックから場所を取得できます。

既定では、コマンドレットは現在の場所を `Push-Location` 現在の場所スタックにプッシュしますが、 **stackname** パラメーターを使用して別の場所スタックを指定することもできます。 スタックが存在しない場合は、に `Push-Location` よって作成されます。

場所スタックの詳細については、「 [注](#notes)」を参照してください。

## 例

### 例 1

この例では、現在の場所を既定の場所スタックにプッシュし、その場所をに変更し `C:\Windows` ます。

```
PS C:\> Push-Location C:\Windows
```

### 例 2

この例では、現在の場所を RegFunction スタックにプッシュし、現在の場所を場所に変更し `HKLM:\Software\Policies` ます。

```
PS C:\> Push-Location HKLM:\Software\Policies -StackName RegFunction
```

Location コマンドレットは、任意の PowerShell ドライブ (PSDrive) で使用できます。

### 例 3

このコマンドを実行すると、現在の場所が既定のスタックにプッシュされます。 場所は変更されません。

```
PS C:\> Push-Location
```

### 例 4-名前付きスタックを作成して使用する

これらのコマンドは、名前付き場所スタックを作成および使用する方法を示しています。

```
PS C:\> Push-Location ~ -StackName Stack2
PS C:\Users\User01> Pop-Location -StackName Stack2
PS C:\>
```

最初のコマンドは、現在の場所を Stack2 という名前の新しいスタックにプッシュした後、現在の場所を、コマンドでチルダ記号 () によって表されるホームディレクトリに変更し `~` ます。このディレクトリは、ファイルシステムプロバイダーのドライブで使用された場合、 `$HOME` とと同等です `$env:USERPROFILE` 。

Stack2 がセッションに存在しない場合は、に `Push-Location` よって作成されます。 2番目のコマンドは、コマンドレットを使用して、 `Pop-Location` Stack2 スタックから元の場所 () をポップし `C:\` ます。 **Stackname** パラメーターを指定しないと、は名前のない `Pop-Location` 既定のスタックから場所をポップします。

場所スタックの詳細については、「 [注](#notes)」を参照してください。

## PARAMETERS

### -LiteralPath

新しい場所のパスを指定します。 **Path** パラメーターと異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: LiteralPath
Aliases: PSPath, LP

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

場所を表すオブジェクトをパイプラインに渡します。 既定では、このコマンドレットによる出力はありません。

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

現在の場所をスタックの最上部に追加 (プッシュ) してから、このパスが示す場所に移動します。 このコマンドレットをサポートするプロバイダーの任意のパスを入力します。 ワイルドカードを使用できます。 パラメーター名は省略可能です。

```yaml
Type: System.String
Parameter Sets: Path
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -StackName

現在の場所を追加する場所スタックを指定します。 場所スタック名を入力します。
スタックが存在しない場合は、に `Push-Location` よって作成されます。

このパラメーターを指定しない場合、は `Push-Location` 現在の場所スタックに場所を追加します。 既定では、現在の場所スタックは、PowerShell によって作成される名前のない既定の場所スタックです。
場所スタックを現在の場所スタックにするには、コマンドレットの **Stackname** パラメーターを使用し `Set-Location` ます。 場所スタックの詳細については、「 [注](#notes)」を参照してください。

> [!NOTE]
> `Push-Location` 現在の場所スタックでない限り、名前のない既定のスタックに場所を追加することはできません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Default stack
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用してパス (リテラルパスではない) を含む文字列をにパイプすることができ `Push-Location` ます。

## 出力

### なしまたはシステムの管理. PathInfo

**PassThru** パラメーターを使用すると、によって、 `Push-Location` 場所を表す system.string オブジェクトが生成さ **れます。** それ以外の場合、このコマンドレットによる出力はありません。

## 注

PowerShell では、プロセスごとに複数の実行空間がサポートされます。 各実行空間には、独自の _現在のディレクトリ_ があります。
これはと同じではありません `[System.Environment]::CurrentDirectory` 。 この動作は、明示的なディレクトリパスを指定せずに、.NET Api を呼び出したり、ネイティブアプリケーションを実行したりするときに問題になることがあります。

場所のコマンドレットでプロセス全体の現在のディレクトリが設定されていても、別の実行空間によっていつでも変更される可能性があるため、このディレクトリに依存することはできません。 現在の実行空間に固有の現在の作業ディレクトリを使用して、パスベースの操作を実行するには、location コマンドレットを使用する必要があります。

スタックとは、最後に追加された項目のみがアクセス可能な後入れ先出しリストです。
使用した順序でスタックに項目を追加し、その後、逆の順序で使用するために項目を取得します。 PowerShell では、プロバイダーの場所を場所スタックに格納できます。

PowerShell では、名前のない既定の場所スタックが作成され、複数の名前付きロケーションスタックを作成できます。 スタック名を指定しない場合、PowerShell は現在の場所スタックを使用します。 既定では、名前のない既定の場所は現在の場所スタックですが、コマンドレットを使用して `Set-Location` 現在の場所スタックを変更できます。

場所スタックを管理するには、次のように PowerShell Location コマンドレットを使用します。

- 場所スタックに場所を追加するには、コマンドレットを使用し `Push-Location` ます。

- 場所スタックから場所を取得するには、コマンドレットを使用し `Pop-Location` ます。

- 現在の場所スタック内の場所を表示するには、コマンドレットの **stack** パラメーターを使用し `Get-Location` ます。

- 名前付きの場所スタック内の場所を表示するには、コマンドレットの **Stackname** パラメーターを使用し `Get-Location` ます。

- 新しい場所スタックを作成するには、コマンドレットの StackName パラメーターを使用し `Push-Location` ます。 存在しないスタックを指定すると、によっ `Push-Location` てスタックが作成されます。

- 場所スタックを現在の場所スタックにするには、コマンドレットの StackName パラメーターを使用し `Set-Location` ます。

名前のない既定の場所スタックは、現在の場所スタックになっている場合にだけ、完全にアクセスできます。
名前付きの場所スタックを現在の場所スタックにすると、またはコマンドレットを使用して `Push-Location` `Pop-Location` 既定のスタックから項目を追加または取得したり、コマンドレットを使用して名前のないスタック内の場所を表示したりすることができなくなり `Get-Location` ます。 名前のないスタックを現在のスタックにするには、コマンドレットの **Stackname** パラメーター `Set-Location` の値を `$null` または空の文字列 () に設定し `""` ます。

また、組み込みエイリアスであるを参照することもでき `Push-Location` `pushd` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

`Push-Location`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Get-Location](Get-Location.md)

[Pop-Location](Pop-Location.md)

[Set-Location](Set-Location.md)

[about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)
