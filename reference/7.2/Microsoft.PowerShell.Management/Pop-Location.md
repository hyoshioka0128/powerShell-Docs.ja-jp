---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 02/04/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/pop-location?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Pop-Location
ms.openlocfilehash: 8a44849f27de80ece486b573f1adccafab51972a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600599"
---
# Pop-Location

## 概要
現在の場所を、最後にスタックにプッシュした場所に変更します。

## SYNTAX

```
Pop-Location [-PassThru] [-StackName <String>] [<CommonParameters>]
```

## Description

コマンドレットは、 `Pop-Location` コマンドレットを使用して、現在の場所を最後にスタックにプッシュした場所に変更し `Push-Location` ます。 既定のスタックから、またはコマンドを使用して作成したスタックから、場所をポップでき `Push-Location` ます。

## 例

### 例 1: 最新の場所に変更する

```
PS C:\> Pop-Location
```

このコマンドを実行すると、現在のスタックに最後に追加した場所に移動します。

### 例 2: 名前付きスタック内の最新の場所に変更する

```
PS C:\> Pop-Location -StackName "Stack2"
```

このコマンドを実行すると、場所スタック Stack2 に最後に追加した場所に移動します。

場所スタックの詳細については、「 [注](#notes)」を参照してください。

### 例 3: 異なるプロバイダーの場所を移動する

```
PS C:\> pushd HKLM:\Software\Microsoft\PowerShell
PS HKLM:\Software\Microsoft\PowerShell> pushd Cert:\LocalMachine\TrustedPublisher
PS cert:\LocalMachine\TrustedPublisher> popd
PS HKLM:\Software\Microsoft\PowerShell> popd
PS C:\>
```

これらのコマンドは、コマンドレットとコマンドレットを使用して、 `Push-Location` `Pop-Location` 異なる PowerShell プロバイダーでサポートされる場所間を移動します。 これらのコマンドは、 `pushd` の別名 `Push-Location` との別名を使用し `popd` `Pop-Location` ます。

最初のコマンドは、現在のファイルシステムの場所をスタックにプッシュし、PowerShell レジストリプロバイダーでサポートされている HKLM ドライブに移動します。

2番目のコマンドは、レジストリの場所をスタックにプッシュし、PowerShell 証明書プロバイダーでサポートされている場所に移動します。

最後の 2 つのコマンドにより、これらの場所がスタックからポップされます。 最初の `popd` コマンドはレジストリドライブに戻り、2番目のコマンドはファイルシステムドライブに戻ります。

## PARAMETERS

### -PassThru

位置を表すオブジェクトをパイプラインに渡します。 既定では、このコマンドレットによる出力はありません。

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

### -StackName

場所をポップする場所スタックを指定します。 場所スタック名を入力します。

このパラメーターを指定しないと、は `Pop-Location` 現在の場所スタックから場所をポップします。 既定では、現在の場所スタックは、PowerShell によって作成される名前のない既定の場所スタックです。 場所スタックを現在の場所スタックにするには、コマンドレットの **Stackname** パラメーターを使用し `Set-Location` ます。 場所スタックの詳細については、「 [注](#notes)」を参照してください。

`Pop-Location` 現在の場所スタックでない限り、名前のない既定のスタックから場所をポップすることはできません。

```yaml
Type: System.String
Parameter Sets: (All)
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

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### なし、システムの管理. PathInfo

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、場所を表す **system.string オブジェクトを** 生成します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

PowerShell では、プロセスごとに複数の実行空間がサポートされます。 各実行空間には、独自の _現在のディレクトリ_ があります。
これはと同じではありません `[System.Environment]::CurrentDirectory` 。 この動作は、明示的なディレクトリパスを指定せずに、.NET Api を呼び出したり、ネイティブアプリケーションを実行したりするときに問題になることがあります。

場所のコマンドレットでプロセス全体の現在のディレクトリが設定されていても、別の実行空間によっていつでも変更される可能性があるため、このディレクトリに依存することはできません。 現在の実行空間に固有の現在の作業ディレクトリを使用して、パスベースの操作を実行するには、location コマンドレットを使用する必要があります。

スタックとは、最後に追加された項目だけにアクセスできる、後入れ先出しのリストです。 使用した順序でスタックに項目を追加し、その後、逆の順序で使用するために項目を取得します。 PowerShell では、プロバイダーの場所を場所スタックに格納できます。

PowerShell では、名前のない既定の場所スタックが作成され、複数の名前付きロケーションスタックを作成できます。 スタック名を指定しない場合、PowerShell は現在の場所スタックを使用します。 既定では、名前のない既定の場所は現在の場所スタックですが、コマンドレットを使用して `Set-Location` 現在の場所スタックを変更できます。

場所スタックを管理するには、次のように PowerShell `*-Location` コマンドレットを使用します。

- 場所スタックに場所を追加するには、コマンドレットを使用し `Push-Location` ます。

- 場所スタックから場所を取得するには、コマンドレットを使用し `Pop-Location` ます。

- 現在の場所スタック内の場所を表示するには、コマンドレットの **stack** パラメーターを使用し `Get-Location` ます。

- 名前付きの場所スタック内の場所を表示するには、コマンドレットの **Stackname** パラメーターを使用し `Get-Location` ます。

- 新しい場所スタックを作成するには、コマンドレットの **Stackname** パラメーターを使用し `Push-Location` ます。 存在しないスタックを指定すると、によっ `Push-Location` てスタックが作成されます。

- 場所スタックを現在の場所スタックにするには、コマンドレットの **Stackname** パラメーターを使用し `Set-Location` ます。

名前のない既定の場所スタックは、現在の場所スタックになっている場合にだけ、完全にアクセスできます。
名前付きの場所スタックを現在の場所スタックにすると、またはコマンドレットを使用して `Push-Location` `Pop-Location` 既定のスタックから項目を追加または取得したり、コマンドレットを使用して名前のないスタック内の場所を表示したりすることができなくなり `Get-Location` ます。 名前のないスタックを現在のスタックにするには、コマンドレットの **Stackname** パラメーター `Set-Location` の値を `$Null` または空の文字列 () に設定し `""` ます。

また、組み込みエイリアスであるを参照することもでき `Pop-Location` `popd` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。

`Pop-Location` は、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[Get-Location](Get-Location.md)

[Push-Location](Push-Location.md)

[Set-Location](Set-Location.md)

[about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)
