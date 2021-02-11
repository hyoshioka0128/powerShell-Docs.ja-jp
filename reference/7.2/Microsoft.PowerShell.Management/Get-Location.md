---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 03/12/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-location?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-Location
ms.openlocfilehash: 57535b4f566a3bdd541d2035b61c8162e15b35f3
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599698"
---
# Get-Location

## 概要
現在の作業場所または場所スタックに関する情報を取得します。

## SYNTAX

### Location (既定値)

```
Get-Location [-PSProvider <String[]>] [-PSDrive <String[]>] [<CommonParameters>]
```

### スタック

```
Get-Location [-Stack] [-StackName <String[]>] [<CommonParameters>]
```

## Description

`Get-Location`コマンドレットは、print working directory (pwd) コマンドと同様に、現在のディレクトリを表すオブジェクトを取得します。

PowerShell ドライブ間を移動すると、PowerShell は各ドライブ内の場所を保持します。 このコマンドレットを使用すると、各ドライブ内の場所を見つけることができます。

このコマンドレットを使用すると、実行時に現在のディレクトリを取得し、それを関数やスクリプト (PowerShell プロンプトで現在のディレクトリを表示する関数など) で使用できます。

また、このコマンドレットを使用して、場所スタック内の場所を表示することもできます。 詳細については、 **Stack** および **Stackname** パラメーターのメモと説明を参照してください。

## 例

### 例 1: 現在のドライブの場所を表示する

このコマンドを実行すると、現在の PowerShell ドライブ内の場所が表示されます。

```powershell
PS C:\Windows> Get-Location
```

```Output
Path
----
C:\Windows
```

たとえば、ドライブのディレクトリにいる場合は、その `Windows` `C:` ディレクトリへのパスが表示されます。

### 例 2: 異なるドライブの現在の場所を表示する

この例では、を使用し `Get-Location` て、さまざまな PowerShell ドライブに現在の場所を表示する方法を示します。 `Set-Location` は、異なる PSDrives 上の複数の異なるパスに場所を変更するために使用されます。

```powershell
PS C:\> Set-Location C:\Windows
PS C:\Windows> Set-Location HKLM:\Software\Microsoft
PS HKLM:\Software\Microsoft> Set-Location "HKCU:\Control Panel\Input Method"
PS HKCU:\Control Panel\Input Method> Get-Location -PSDrive C

Path
----
C:\Windows

PS HKCU:\Control Panel\Input Method> Get-Location -PSDrive HKLM

Path
----
HKLM:\Software\Microsoft

PS HKCU:\Control Panel\Input Method> Set-Location C:
PS C:\Windows> Get-Location -PSProvider Registry

Path
----
HKCU:\Control Panel\Input Method
```

### 例 3: スタックを使用して場所を取得する

この例では、の **stack** および **stackname** パラメーターを使用して、 `Get-Location` 現在の場所スタックおよび代替位置スタック内の場所を一覧表示する方法を示します。

`Push-Location`コマンドレットは、3つの異なる場所に変更するために使用されます。 3番目のプッシュでは、別のスタック名が使用されます。 の **stack** パラメーターには、 `Get-Location` 既定のスタックの内容が表示されます。 の **Stackname** パラメーターは、 `Get-Location` という名前のスタックの内容を表示し `Stack2` ます。

```powershell
PS C:\> Push-Location C:\Windows
PS C:\Windows>Push-Location System32
PS C:\Windows\System32>Push-Location WindowsPowerShell -StackName Stack2
C:\Windows\System32\WindowsPowerShell>Get-Location -Stack

Path
----
C:\Windows
C:\

C:\Windows\System32\WindowsPowerShell>Get-Location -StackName Stack2

Path
----
C:\Windows\System32
```

### 例 4: PowerShell プロンプトをカスタマイズする

この例では、PowerShell プロンプトをカスタマイズする方法を示します。

```powershell
PS C:\>
function prompt { 'PowerShell: ' + (Get-Location) + '> '}
PowerShell: C:\>
```

プロンプトを定義する関数には、コマンドが含まれてい `Get-Location` ます。このコマンドは、コンソールにプロンプトが表示されるたびに実行されます。

既定の PowerShell プロンプトの形式は、という特殊な関数によって定義され `prompt` ます。 という名前の新しい関数を作成することにより、コンソールでプロンプトを変更でき `prompt` ます。

現在の prompt 関数を表示するには、次のコマンドを入力します。 `Get-Content Function:\prompt`

## PARAMETERS

### -PSDrive

指定した PowerShell ドライブ内の現在の場所を取得します。

たとえば、ドライブにいる場合は、 `Cert:` このパラメーターを使用してドライブ内の現在の場所を見つけることができ `C:` ます。

```yaml
Type: System.String[]
Parameter Sets: Location
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PSProvider

指定した PowerShell プロバイダーによってサポートされるドライブ内の現在の場所を取得します。
指定したプロバイダーが複数のドライブをサポートしている場合、このコマンドレットは、最後にアクセスしたドライブ上の場所を返します。

たとえば、ドライブにいる場合は、 `C:` このパラメーターを使用して、PowerShell **レジストリ** プロバイダーのドライブ内の現在の場所を見つけることができます。

```yaml
Type: System.String[]
Parameter Sets: Location
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Stack

このコマンドレットによって、現在の場所スタックに追加された場所が表示されることを示します。 コマンドレットを使用して、スタックに場所を追加でき `Push-Location` ます。

別の場所スタック内の場所を表示するには、 **Stackname** パラメーターを使用します。 場所スタックの詳細については、「 [注](#notes)」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Stack
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -StackName

名前付きの場所スタックを文字列配列として指定します。 1 つ以上の場所スタック名を入力します。

現在の場所スタック内の場所を表示するには、 **stack** パラメーターを使用します。 場所スタックを現在の場所スタックにするには、 `Set-Location` コマンドレットを使用します。

このコマンドレットでは、現在のスタックでない限り、名前のない既定のスタック内の場所を表示することはできません。

```yaml
Type: System.String[]
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

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### ..................... PathInfo スタック

**Stack** パラメーターまたは **stackname** パラメーターを使用すると、このコマンドレットは **pathinfostack** オブジェクトを返します。 それ以外の場合は、 **Pathinfo** オブジェクトを返します。

## 注

PowerShell では、プロセスごとに複数の実行空間がサポートされます。 各実行空間には、独自の _現在のディレクトリ_ があります。
これはと同じではありません `[System.Environment]::CurrentDirectory` 。 この動作は、明示的なディレクトリパスを指定せずに、.NET Api を呼び出したり、ネイティブアプリケーションを実行したりするときに問題になることがあります。
コマンドレットにより、 `Get-Location` 現在の PowerShell 実行空間の現在のディレクトリが返されます。

このコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションのプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

**Psprovider**、 **PSDrive**、 **Stack**、 **stackname** の各パラメーターがどのように対話するかは、プロバイダーによって異なります。 ドライブとそのドライブを公開していないプロバイダーを指定した場合など、エラーになる組み合わせもあります。 パラメーターが指定されていない場合、このコマンドレットは、現在の作業場所を含むプロバイダーの **Pathinfo** オブジェクトを返します。

スタックとは、最後に追加された項目のみがアクセス可能な後入れ先出しリストです。 使用した順序でスタックに項目を追加し、その後、逆の順序で使用するために項目を取得します。 PowerShell では、プロバイダーの場所を場所スタックに格納できます。 PowerShell では、名前のない既定の場所スタックが作成され、複数の名前付きロケーションスタックを作成できます。 スタック名を指定しない場合、PowerShell は現在の場所スタックを使用します。 既定では、名前のない既定の場所は現在の場所スタックですが、コマンドレットを使用して `Set-Location` 現在の場所スタックを変更できます。

場所スタックを管理するには、次のように PowerShell `*-Location` コマンドレットを使用します。

- 場所スタックに場所を追加するには、コマンドレットを使用し `Push-Location` ます。

- 場所スタックから場所を取得するには、コマンドレットを使用し `Pop-Location` ます。

- 現在の場所スタック内の場所を表示するには、コマンドレットの **stack** パラメーターを使用し `Get-Location` ます。 名前付きの場所スタック内の場所を表示するには、コマンドレットの **Stackname** パラメーターを使用し `Get-Location` ます。

- 新しい場所スタックを作成するには、コマンドレットの **Stackname** パラメーターを使用し `Push-Location` ます。
  存在しないスタックを指定すると、によっ `Push-Location` てスタックが作成されます。

- 場所スタックを現在の場所スタックにするには、コマンドレットの **Stackname** パラメーターを使用し `Set-Location` ます。

名前のない既定の場所スタックは、現在の場所スタックになっている場合にだけ、完全にアクセスできます。
名前付きの場所スタックを現在の場所スタックにすると、またはコマンドレットを使用して `Push-Location` `Pop-Location` 既定のスタックから項目を追加または取得したり、このコマンドレットを使用して名前のないスタック内の場所を表示したりすることができなくなります。 名前のないスタックを現在のスタックにするには、コマンドレットの **Stackname** パラメーター `Set-Location` の値を `$null` または空の文字列 () に設定し `""` ます。

## 関連リンク

[Pop-Location](Pop-Location.md)

[Push-Location](Push-Location.md)

[Set-Location](Set-Location.md)

