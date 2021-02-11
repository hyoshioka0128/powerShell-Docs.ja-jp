---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 08/25/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/copy-item?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Copy-Item
ms.openlocfilehash: ee2d8b8a3b736a59b5af4a81710a0d29dd2238d5
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599920"
---
# Copy-Item

## 概要
項目をある場所から別の場所にコピーします。

## SYNTAX

### パス (既定値)

```
Copy-Item [-Path] <String[]> [[-Destination] <String>] [-Container] [-Force] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-Recurse] [-PassThru] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [-FromSession <PSSession>] [-ToSession <PSSession>] [<CommonParameters>]
```

### LiteralPath

```
Copy-Item -LiteralPath <String[]> [[-Destination] <String>] [-Container] [-Force] [-Filter <String>]
 [-Include <String[]>] [-Exclude <String[]>] [-Recurse] [-PassThru] [-Credential <PSCredential>]
 [-WhatIf] [-Confirm] [-FromSession <PSSession>] [-ToSession <PSSession>] [<CommonParameters>]
```

## Description

`Copy-Item`コマンドレットは、同じ名前空間内のある場所から別の場所に項目をコピーします。
たとえば、ファイルをフォルダーにコピーすることはできますが、ファイルを証明書ドライブにコピーすることはできません。

このコマンドレットは、コピーされる項目を切り取りまたは削除しません。 コマンドレットがコピーできる項目は、項目を公開する PowerShell プロバイダーによって異なります。 たとえば、ファイルシステムドライブのファイルとディレクトリ、レジストリキー、レジストリドライブ内のエントリをコピーできます。

このコマンドレットは、同じコマンドで項目のコピーと名前の変更を行うことができます。 項目の名前を変更するには、 **Destination** パラメーターの値に新しい名前を入力します。 項目の名前を変更し、コピーしない場合は、コマンドレットを使用し `Rename-Item` ます。

## 例

### 例 1: 指定されたディレクトリにファイルをコピーする

この例では、 `mar1604.log.txt` ファイルをディレクトリにコピーし `C:\Presentation` ます。 元のファイルは削除されません。

```powershell
Copy-Item "C:\Wabash\Logfiles\mar1604.log.txt" -Destination "C:\Presentation"
```

### 例 2: 既存のディレクトリにディレクトリの内容をコピーする

この例では、ディレクトリの内容を `C:\Logfiles` 既存のディレクトリにコピーし `C:\Drawings` ます。 `Logfiles`ディレクトリはコピーされません。

ディレクトリに `Logfiles` サブディレクトリ内のファイルが含まれている場合、それらのサブディレクトリはそのファイルツリーと共にコピーされます。 既定では、 **Container** パラメーターは **True** に設定されています。これにより、ディレクトリ構造が保持されます。

```powershell
Copy-Item -Path "C:\Logfiles\*" -Destination "C:\Drawings" -Recurse
```

> [!NOTE]
> コピーにディレクトリを含める必要がある場合は、 `Logfiles` `\*` **パス** からを削除します。
> 次に例を示します。
>
> `Copy-Item -Path "C:\Logfiles" -Destination "C:\Drawings" -Recurse`

### 例 3: ディレクトリとコンテンツを新しいディレクトリにコピーする

この例では、ソースディレクトリの内容をコピー `C:\Logfiles` し、新しいコピー先ディレクトリを作成します。 新しいコピー先ディレクトリは、 `\Logs` で作成され `C:\Drawings` ます。

ソースディレクトリの名前を含めるには、 **例 2** に示すように、既存のコピー先ディレクトリにコピーします。 または、新しい宛先ディレクトリに、ソースディレクトリと同じ名前を付けます。

```powershell
Copy-Item -Path "C:\Logfiles" -Destination "C:\Drawings\Logs" -Recurse
```

> [!NOTE]
> **パス** にが含まれている場合 `\*` 、ディレクトリのすべてのファイルの内容 (サブディレクトリツリーを含む) が新しいコピー先ディレクトリにコピーされます。 次に例を示します。
>
> `Copy-Item -Path "C:\Logfiles\*" -Destination "C:\Drawings\Logs" -Recurse`

### 例 4: 指定したディレクトリにファイルをコピーし、ファイルの名前を変更する

この例では、コマンドレットを使用して、 `Copy-Item` `Get-Widget.ps1` ディレクトリからディレクトリにスクリプトをコピーし `\\Server01\Share` `\\Server12\ScriptArchive` ます。 コピー操作の一部として、コマンドは項目名をからに変更し、 `Get-Widget.ps1` `Get-Widget.ps1.txt` 電子メールメッセージに添付できるようにします。

```powershell
Copy-Item "\\Server01\Share\Get-Widget.ps1" -Destination "\\Server12\ScriptArchive\Get-Widget.ps1.txt"
```

### 例 5: リモートコンピューターにファイルをコピーする

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

この `Copy-Item` コマンドレットは、 `test.log` `D:\Folder001` `C:\Folder001_Copy` 変数に格納されているセッション情報を使用して、フォルダーからリモートコンピューター上のフォルダーにコピーし `$Session` ます。 元のファイルは削除されません。

```powershell
$Session = New-PSSession -ComputerName "Server01" -Credential "Contoso\User01"
Copy-Item "D:\Folder001\test.log" -Destination "C:\Folder001_Copy\" -ToSession $Session
```

### 例 6: リモートコンピューターにフォルダーをコピーする

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

この `Copy-Item` コマンドレットは、 `D:\Folder002` `C:\Folder002_Copy` 変数に格納されているセッション情報を使用して、リモートコンピューター上のディレクトリにフォルダーをコピーし `$Session` ます。 サブフォルダーまたはファイルは、 **再帰** スイッチを使用せずにコピーされません。
フォルダーがまだ存在しない場合は、操作によって作成され `Folder002_Copy` ます。

```powershell
$Session = New-PSSession -ComputerName "Server02" -Credential "Contoso\User01"
Copy-Item "D:\Folder002\" -Destination "C:\Folder002_Copy\" -ToSession $Session
```

### 例 7: フォルダーの内容全体をリモートコンピューターに再帰的にコピーする

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

この `Copy-Item` コマンドレットは、 `D:\Folder003` `C:\Folder003_Copy` 変数に格納されているセッション情報を使用して、フォルダーの内容全体をリモートコンピューター上のディレクトリにコピーし `$Session` ます。 サブフォルダーは、そのファイルツリーと共にコピーされます。 フォルダーがまだ存在しない場合は、操作によって作成され `Folder003_Copy` ます。

```powershell
$Session = New-PSSession -ComputerName "Server04" -Credential "Contoso\User01"
Copy-Item "D:\Folder003\" -Destination "C:\Folder003_Copy\" -ToSession $Session -Recurse
```

### 例 8: ファイルをリモートコンピューターにコピーし、ファイルの名前を変更する

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

この `Copy-Item` コマンドレットは、 `scriptingexample.ps1` `D:\Folder004` `C:\Folder004_Copy` 変数に格納されているセッション情報を使用して、フォルダーからリモートコンピューター上のフォルダーにコピーし `$Session` ます。 コピー操作の一部として、コマンドは項目名をからに変更し、 `scriptingexample.ps1` `scriptingexample_copy.ps1` 電子メールメッセージに添付できるようにします。 元のファイルは削除されません。

```powershell
$Session = New-PSSession -ComputerName "Server04" -Credential "Contoso\User01"
Copy-Item "D:\Folder004\scriptingexample.ps1" -Destination "C:\Folder004_Copy\scriptingexample_copy.ps1" -ToSession $Session
```

### 例 9: ローカルコンピューターにリモートファイルをコピーする

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

`Copy-Item`コマンドレットは、 `test.log` `C:\MyRemoteData\` `D:\MyLocalData` 変数に格納されているセッション情報を使用して、リモートからローカルフォルダーにコピーし `$Session` ます。 元のファイルは削除されません。

```powershell
$Session = New-PSSession -ComputerName "Server01" -Credential "Contoso\User01"
Copy-Item "C:\MyRemoteData\test.log" -Destination "D:\MyLocalData\" -FromSession $Session
```

### 例 10: リモートフォルダーの内容全体をローカルコンピューターにコピーする

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

`Copy-Item`コマンドレットは、 `C:\MyRemoteData\scripts` `D:\MyLocalData` 変数に格納されているセッション情報を使用して、リモートフォルダーからローカルフォルダーにコンテンツ全体をコピーし `$Session` ます。 Scripts フォルダーにサブフォルダー内のファイルが含まれている場合、それらのサブフォルダーはそのファイルツリーと共にコピーされます。

```powershell
$Session = New-PSSession -ComputerName "Server01" -Credential "Contoso\User01"
Copy-Item "C:\MyRemoteData\scripts" -Destination "D:\MyLocalData\" -FromSession $Session
```

### 例 11: リモートフォルダーの内容全体をローカルコンピューターに再帰的にコピーする

の資格情報を使用して **Server01** という名前のリモートコンピューターにセッションが作成され、と `Contoso\User01` いう名前の変数に結果が格納され `$Session` ます。

`Copy-Item`コマンドレットは、 `C:\MyRemoteData\scripts` `D:\MyLocalData\scripts` 変数に格納されているセッション情報を使用して、リモートフォルダーからローカルフォルダーにコンテンツ全体をコピーし `$Session` ます。 **再帰** パラメーターが使用されるため、スクリプトフォルダーがまだ存在しない場合は、この操作によって作成されます。 Scripts フォルダーにサブフォルダー内のファイルが含まれている場合、それらのサブフォルダーはそのファイルツリーと共にコピーされます。

```powershell
$Session = New-PSSession -ComputerName "Server01" -Credential "Contoso\User01"
Copy-Item "C:\MyRemoteData\scripts" -Destination "D:\MyLocalData\scripts" -FromSession $Session -Recurse
```

### 例 12: フォルダーツリーから現在のフォルダーにファイルを再帰的にコピーする

この例では、複数レベルのフォルダー構造から1つのフラットフォルダーにファイルをコピーする方法を示します。
最初の3つのコマンドは、既存のフォルダー構造と2つのファイル (両方の名前) の内容を表示し `file3.txt` ます。

```powershell
PS C:\temp\test> (Get-ChildItem C:\temp\tree -Recurse).FullName
C:\temp\tree\subfolder
C:\temp\tree\file1.txt
C:\temp\tree\file2.txt
C:\temp\tree\file3.txt
C:\temp\tree\subfolder\file3.txt
C:\temp\tree\subfolder\file4.txt
C:\temp\tree\subfolder\file5.txt

PS C:\temp\test> Get-Content C:\temp\tree\file3.txt
This is file3.txt in the root folder

PS C:\temp\test> Get-Content C:\temp\tree\subfolder\file3.txt
This is file3.txt in the subfolder

PS C:\temp\test> Copy-Item -Path C:\temp\tree -Filter *.txt -Recurse -Container:$false
PS C:\temp\test> (Get-ChildItem . -Recurse).FullName
C:\temp\test\subfolder
C:\temp\test\file1.txt
C:\temp\test\file2.txt
C:\temp\test\file3.txt
C:\temp\test\file4.txt
C:\temp\test\file5.txt

PS C:\temp\test> Get-Content .\file3.txt
This is file3.txt in the subfolder
```

`Copy-Item`コマンドレットでは、 **Container** パラメーターがに設定されてい `$false` ます。 これにより、ソースフォルダーの内容がコピーされますが、フォルダー構造は保持されません。 同じ名前のファイルがコピー先フォルダーで上書きされることに注意してください。

## PARAMETERS

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -コンテナー

コピー操作中に、このコマンドレットによってコンテナーオブジェクトが保持されることを示します。 既定では、 **Container** パラメーターは **True** に設定されています。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: True
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

> [!NOTE]
> このパラメーターは、PowerShell でインストールされたプロバイダーではサポートされていません。
> 別のユーザーの権限を借用したり、このコマンドレットの実行時に資格情報を昇格させたりするには、 [Invoke コマンド](../Microsoft.PowerShell.Core/Invoke-Command.md)を使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Destination

新しい場所のパスを指定します。 既定値は、現在のディレクトリです。

コピーする項目の名前を変更するには、 **Destination** パラメーターの値に新しい名前を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: Current directory
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -除外

このコマンドレットによって操作で除外される項目を文字列配列として指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `*.txt` ます。 ワイルドカード文字を使用できます。 **Exclude** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

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

**パス** パラメーターを修飾するフィルターを指定します。 [ファイルシステム](../Microsoft.PowerShell.Core/About/about_FileSystem_Provider.md)プロバイダーは、フィルターの使用をサポートする唯一のインストール済み PowerShell プロバイダーです。 **ファイルシステム** フィルター言語の構文については、 [about_Wildcards](../Microsoft.PowerShell.Core/About/about_Wildcards.md)を参照してください。
フィルターは他のパラメーターよりも効率的です。これは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、コマンドレットがオブジェクトを取得するときに、フィルターが適用されるためです。

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

### -Force

このコマンドレットが、読み取り専用のファイルまたはエイリアスを使用したコピーなど、変更できない項目をコピーすることを示します。

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

### -FromSession

リモートファイルのコピー元の **PSSession** オブジェクトを指定します。 このパラメーターを使用する場合、 **Path** パラメーターと **LiteralPath** パラメーターは、リモートコンピューター上のローカルパスを参照します。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Include

文字列配列として、このコマンドレットによって操作に含まれる項目を指定します。 このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (など) を入力し `"*.txt"` ます。 ワイルドカード文字を使用できます。 **Include** パラメーターは、コマンドに項目の内容 (など) が含まれている場合にのみ有効になり `C:\Windows\*` ます。ワイルドカード文字は、ディレクトリの内容を指定し `C:\Windows` ます。

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

### -LiteralPath

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

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

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットは出力を生成しません。

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

文字列配列として、コピーする項目へのパスを指定します。 ワイルドカード文字を使用できます。

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

### -再帰

このコマンドレットが再帰コピーを行うことを示します。

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

### -ToSession

リモートファイルのコピー先の **PSSession** オブジェクトを指定します。 このパラメーターを使用すると、 **Destination** パラメーターはリモートコンピューター上のローカルパスを参照します。

```yaml
Type: System.Management.Automation.Runspaces.PSSession
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 コマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、このコマンドレットへのパスを含む文字列をパイプすることができます。

## 出力

### None またはコピーされた項目を表すオブジェクト

**PassThru** パラメーターを使用すると、このコマンドレットは、コピーされた項目を表すオブジェクトを返します。 それ以外の場合、このコマンドレットは出力を生成しません。

## 注

このコマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Clear-Item](Clear-Item.md)

[Get-Item](Get-Item.md)

[Get-PSProvider](Get-PSProvider.md)

[Invoke-Item](Invoke-Item.md)

[Move-Item](Move-Item.md)

[New-Item](New-Item.md)

[Remove-Item](Remove-Item.md)

[Rename-Item](Rename-Item.md)

[Set-Item](Set-Item.md)

