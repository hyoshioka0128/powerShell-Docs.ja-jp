---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 03/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-ChildItem
ms.openlocfilehash: 0bcd46e49559ad625621a7ff81162af695f6f93c
ms.sourcegitcommit: 7f712e12ec5b3f3f3e695da804b050ea0ce58b3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "94661326"
---
# Get-ChildItem

## 概要

1 つ以上の指定された場所から項目および子項目を取得します。

## SYNTAX

### 項目 (既定)

```
Get-ChildItem [[-Path] <string[]>] [[-Filter] <string>] [-Include <string[]>] [-Exclude <string[]>]
 [-Recurse] [-Depth <uint32>] [-Force] [-Name] [-Attributes <FlagsExpression[FileAttributes]>]
 [-FollowSymlink] [-Directory] [-File] [-Hidden] [-ReadOnly] [-System] [<CommonParameters>]
```

### LiteralItems

```
Get-ChildItem [[-Filter] <string>] -LiteralPath <string[]> [-Include <string[]>]
 [-Exclude <string[]>] [-Recurse] [-Depth <uint32>] [-Force] [-Name]
 [-Attributes <FlagsExpression[FileAttributes]>] [-FollowSymlink] [-Directory] [-File] [-Hidden]
 [-ReadOnly] [-System] [<CommonParameters>]
```

## Description

`Get-ChildItem`コマンドレットは、指定された1つまたは複数の場所にある項目を取得します。 項目がコンテナーの場合は、コンテナーの中にある項目 (子項目) を取得します。 **再帰** パラメーターを使用すると、すべての子コンテナー内の項目を取得し、 **Depth** パラメーターを使用して再帰的なレベル数を制限できます。

`Get-ChildItem` 空のディレクトリは表示されません。 コマンドに `Get-ChildItem` **深度** パラメーターまたは **再帰** パラメーターが含まれている場合、空のディレクトリは出力に含まれません。

場所は、PowerShell プロバイダーによってに公開され `Get-ChildItem` ます。 場所には、ファイルシステムディレクトリ、レジストリハイブ、または証明書ストアを指定できます。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 例

### 例 1: ファイルシステムディレクトリから子項目を取得する

この例では、ファイルシステムディレクトリから子項目を取得します。 ファイル名とサブディレクトリ名が表示されます。 空の場所の場合、コマンドは出力を返さず、PowerShell プロンプトに戻ります。

コマンドレットでは、 `Get-ChildItem` **Path** パラメーターを使用してディレクトリを指定し `C:\Test` ます。
`Get-ChildItem` ファイルとディレクトリを PowerShell コンソールに表示します。

```powershell
Get-ChildItem -Path C:\Test
```

```Output
   Directory: C:\Test

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/15/2019     08:29                Logs
-a----        2/13/2019     08:55             26 anotherfile.txt
-a----        2/12/2019     15:40         118014 Command.txt
-a----         2/1/2019     08:43            183 CreateTestFile.ps1
-ar---        2/12/2019     14:31             27 ReadOnlyFile.txt
```

既定で `Get-ChildItem` は、モード (**属性**)、 **lastwritetime**、ファイルサイズ (**長さ**)、およびアイテムの **名前** が一覧表示されます。 **Mode** プロパティの文字は、次のように解釈できます。

- `l` リンク
- `d` 名簿
- `a` アイテム
- `r` (読み取り専用)
- `h` 表示
- `s` (システム)。

モードフラグの詳細については、「 [about_Filesystem_Provider](../microsoft.powershell.core/about/about_filesystem_provider.md#attributes-flagsexpression)」を参照してください。

### 例 2: ディレクトリ内の子項目の名前を取得する

この例では、ディレクトリ内の項目の名前のみを一覧表示します。

コマンドレットでは、 `Get-ChildItem` **Path** パラメーターを使用してディレクトリを指定し `C:\Test` ます。 **Name** パラメーターは、指定されたパスからファイル名またはディレクトリ名のみを返します。

```powershell
Get-ChildItem -Path C:\Test -Name
```

```Output
Logs
anotherfile.txt
Command.txt
CreateTestFile.ps1
ReadOnlyFile.txt
```

### 例 3: 現在のディレクトリとサブディレクトリ内の子項目を取得する

この例では、現在のディレクトリとそのサブディレクトリにある .txt ファイルを表示し **ます。**

```powershell
Get-ChildItem -Path C:\Test\*.txt -Recurse -Force
```

```Output
    Directory: C:\Test\Logs\Adirectory

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/12/2019     16:16             20 Afile4.txt
-a-h--        2/12/2019     15:52             22 hiddenfile.txt
-a----        2/13/2019     13:26             20 LogFile4.txt

    Directory: C:\Test\Logs\Backup

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/12/2019     16:16             20 ATextFile.txt
-a----        2/12/2019     15:50             20 LogFile3.txt

    Directory: C:\Test\Logs

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/12/2019     16:16             20 Afile.txt
-a-h--        2/12/2019     15:52             22 hiddenfile.txt
-a----        2/13/2019     13:26             20 LogFile1.txt

    Directory: C:\Test

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/13/2019     08:55             26 anotherfile.txt
-a----        2/12/2019     15:40         118014 Command.txt
-a-h--        2/12/2019     15:52             22 hiddenfile.txt
-ar---        2/12/2019     14:31             27 ReadOnlyFile.txt
```

この `Get-ChildItem` コマンドレットは、 **Path** パラメーターを使用してを指定し `C:\Test\*.txt` ます。 **Path** はアスタリスク ( `*` ) ワイルドカードを使用して、ファイル名拡張子を持つすべてのファイルを指定し `.txt` ます。 **再帰** パラメーターは、**ディレクトリ:** 見出しに示されているように、**パス** ディレクトリ内のサブディレクトリを検索します。 **Force** パラメーターは、 `hiddenfile.txt` モードが **h** のなどの非表示ファイルを表示します。

### 例 4: Include パラメーターを使用して子項目を取得する

この例では、 `Get-ChildItem` **Include** パラメーターを使用して、 **Path** パラメーターで指定されたディレクトリから特定の項目を検索します。

```powershell
# When using the -Include parameter, if you don't include an asterisk in the path
# the command returns no output.
Get-ChildItem -Path C:\Test\ -Include *.txt
```

```Output

```

```powershell
Get-ChildItem -Path C:\Test\* -Include *.txt
```

```Output
    Directory: C:\Test

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        2/13/2019     08:55             26 anotherfile.txt
-a----        2/12/2019     15:40         118014 Command.txt
-ar---        2/12/2019     14:31             27 ReadOnlyFile.txt
```

この `Get-ChildItem` コマンドレットでは、 **Path** パラメーターを使用して、ディレクトリ **C:\Test** を指定します。 **Path** パラメーターには、ディレクトリの内容を指定するための末尾のアスタリスク () ワイルドカードが含まれてい `*` ます。
**Include** パラメーターでは、アスタリスク ( `*` ) ワイルドカードを使用して、ファイル名拡張子 **.txt** を持つすべてのファイルを指定します。

**Include** パラメーターを使用する場合、 **Path** パラメーターには、 `*` ディレクトリの内容を指定するための末尾のアスタリスク () ワイルドカードが必要です。 たとえば、「 `-Path C:\Test\*` 」のように入力します。

- **再帰** パラメーターがコマンドに追加された場合、Path パラメーターの末尾のアスタリスク ( `*` ) は省略可能です。  **再帰** パラメーターは、**パス** ディレクトリとそのサブディレクトリから項目を取得します。 たとえば、`-Path C:\Test\ -Recurse -Include *.txt` のように指定します。
- パスパラメーターに末尾のアスタリスク () が含まれていない場合 `*` 、コマンドは出力を返さず、PowerShell プロンプトに戻ります。  たとえば、「 `-Path C:\Test\` 」のように入力します。

### 例 5: Exclude パラメーターを使用して子項目を取得する

この例の出力は、ディレクトリ **C:\Test\Logs** の内容を示しています。 出力は、 **Exclude** および **再帰** パラメーターを使用する他のコマンドの参照です。

```powershell
Get-ChildItem -Path C:\Test\Logs
```

```Output
    Directory: C:\Test\Logs

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/15/2019     13:21                Adirectory
d-----        2/15/2019     08:28                AnEmptyDirectory
d-----        2/15/2019     13:21                Backup
-a----        2/12/2019     16:16             20 Afile.txt
-a----        2/13/2019     13:26             20 LogFile1.txt
-a----        2/12/2019     16:24             23 systemlog1.log
```

```powershell
Get-ChildItem -Path C:\Test\Logs\* -Exclude A*
```

```Output
    Directory: C:\Test\Logs

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/15/2019     13:21                Backup
-a----        2/13/2019     13:26             20 LogFile1.txt
-a----        2/12/2019     16:24             23 systemlog1.log
```

コマンドレットでは、 `Get-ChildItem` **Path** パラメーターを使用してディレクトリを指定し `C:\Test\Logs` ます。
**Exclude** パラメーターでは、アスタリスク () ワイルドカードを使用して、 `*` またはで始まる任意のファイルまたはディレクトリを出力から除外します。 

**Exclude** パラメーターを使用する場合、Path パラメーターの末尾のアスタリスク ( `*` ) は省略可能です。  たとえば、`-Path C:\Test\Logs` または `-Path C:\Test\Logs\*` です。

- Path パラメーターに末尾のアスタリスク ( `*` ) が含まれていない場合は、 **path** パラメーターの内容が表示されます。 例外は、 **Exclude** パラメーターの値と一致するファイル名またはサブディレクトリ名です。
- Path パラメーターに末尾のアスタリスク ( `*` ) が含まれている場合、コマンドは **path** パラメーターのサブディレクトリに繰り返しします。 例外は、 **Exclude** パラメーターの値と一致するファイル名またはサブディレクトリ名です。
- **再帰パラメーターが** コマンドに追加された場合、 **Path** パラメーターに末尾のアスタリスク () が含まれているかどうかにかかわらず、再帰の出力は同じに `*` なります。

### 例 6: レジストリハイブからレジストリキーを取得する

この例では、からすべてのレジストリキーを取得し `HKEY_LOCAL_MACHINE\HARDWARE` ます。

`Get-ChildItem`**Path** パラメーターを使用して、レジストリキーを指定し `HKLM:\HARDWARE` ます。 PowerShell コンソールには、ハイブのパスとレジストリキーの最上位レベルが表示されます。

詳細については、「 [about_Registry_Provider](../Microsoft.PowerShell.Core/About/about_Registry_Provider.md)」を参照してください。

```powershell
Get-ChildItem -Path HKLM:\HARDWARE
```

```Output
    Hive: HKEY_LOCAL_MACHINE\HARDWARE

Name             Property
----             --------
ACPI
DESCRIPTION
DEVICEMAP
RESOURCEMAP
UEFI
```

```powershell
Get-ChildItem -Path HKLM:\HARDWARE -Exclude D*
```

```Output
   Hive: HKEY_LOCAL_MACHINE\HARDWARE

Name                           Property
----                           --------
ACPI
RESOURCEMAP
```

最初のコマンドは、レジストリキーの内容を表示し `HKLM:\HARDWARE` ます。 **Exclude** パラメーターは、 `Get-ChildItem` で始まるサブキーを返さないように指示 `D*` します。 現在、 **Exclude** パラメーターは、項目のプロパティではなく、サブキーに対してのみ機能します。

### 例 7: コード署名権限を持つすべての証明書を取得する

この例では、コード署名権限を持つ、PowerShell **Cert:** ドライブ内の各証明書を取得します。

コマンドレットでは、 `Get-ChildItem` **Path** パラメーターを使用して **Cert:** provider を指定します。 **再帰** パラメーターは、 **Path** とそのサブディレクトリで指定されたディレクトリを検索します。 **Codesigningcert** パラメーターは、コード署名権限を持つ証明書のみを取得します。

```powershell
Get-ChildItem -Path Cert:\* -Recurse -CodeSigningCert
```

証明書プロバイダーと Cert: ドライブの詳細については、「 [about_Certificate_Provider](../Microsoft.PowerShell.Security/About/about_Certificate_Provider.md)」を参照してください。

### 例 8: Depth パラメーターを使用して項目を取得する

この例では、ディレクトリとそのサブディレクトリ内の項目を表示します。 **Depth** パラメーターは、再帰に含めるサブディレクトリレベルの数を決定します。 空のディレクトリは、出力から除外されます。

```powershell
Get-ChildItem -Path C:\Parent -Depth 2
```

```Output
    Directory: C:\Parent

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/14/2019     10:24                SubDir_Level1
-a----        2/13/2019     08:55             26 file.txt

    Directory: C:\Parent\SubDir_Level1

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/14/2019     10:24                SubDir_Level2
-a----        2/13/2019     08:55             26 file.txt

    Directory: C:\Parent\SubDir_Level1\SubDir_Level2

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        2/14/2019     10:22                SubDir_Level3
-a----        2/13/2019     08:55             26 file.txt
```

この `Get-ChildItem` コマンドレットは、 **path** パラメーターを使用して、 **c:\ 親** を指定します。 **Depth** パラメーターでは、2つのレベルの再帰が指定されています。 `Get-ChildItem`**Path** パラメーターで指定したディレクトリの内容と、サブディレクトリの2つのレベルを表示します。

### 例 9: ハードリンク情報を取得する

PowerShell 6.2 では、ハードリンク情報を取得するために代替ビューが追加されました。

```powershell
Get-ChildItem -Path C:\PathContainingHardLink | Format-Table -View childrenWithHardLink
```

### 例 9: Windows 以外のオペレーティングシステムの出力

Unix システムの PowerShell 7.1 では、は `Get-ChildItem` unix に似た出力を提供します。

```powershell
PS> Get-ChildItem /etc/r*
```

```Output
    Directory: /etc

UnixMode   User Group    LastWriteTime Size Name
--------   ---- -----    ------------- ---- ----
drwxr-xr-x root wheel  9/30/2019 19:19  128 racoon
-rw-r--r-- root wheel  9/26/2019 18:20 1560 rc.common
-rw-r--r-- root wheel  7/31/2017 17:30 1560 rc.common~previous
-rw-r--r-- root wheel  9/27/2019 20:34 5264 rc.netboot
lrwxr-xr-x root wheel  11/8/2019 15:35   22 resolv.conf -> /private/var/run/resolv.conf
-rw-r--r-- root wheel 10/23/2019 17:41    0 rmtab
-rw-r--r-- root wheel 10/23/2019 17:41 1735 rpc
-rw-r--r-- root wheel  7/25/2017 18:37 1735 rpc~previous
-rw-r--r-- root wheel 10/23/2019 18:42  891 rtadvd.conf
-rw-r--r-- root wheel  8/24/2017 21:54  891 rtadvd.conf~previous
```

出力の一部である新しいプロパティは次のとおりです。

- **UnixMode** は、Unix システムで表されるファイルのアクセス許可です。
- **ユーザー** はファイルの所有者です
- **グループは** グループの所有者です
- **サイズ** は、Unix システムで表されるファイルまたはディレクトリのサイズです。

> [!NOTE]
> この機能は、PowerShell 7.1 の試験段階からメインストリームに移行されました。

## パラメーター

### -属性

指定した属性のファイルとフォルダーを取得します。 このパラメーターはすべての属性をサポートします。複雑な属性の組み合わせを指定できます。

たとえば、暗号化または圧縮されている非システム ファイル (非ディレクトリ) を取得するには、次のように入力します。

`Get-ChildItem -Attributes !Directory+!System+Encrypted, !Directory+!System+Compressed`

よく使用される属性を持つファイルとフォルダーを検索するには、 **attributes** パラメーターを使用します。 または、パラメーター **Directory**、 **File**、 **Hidden**、 **ReadOnly**、および **System** です。

**Attributes** パラメーターでは、次のプロパティがサポートされています。

- **Archive**
- **Compressed**
- **[デバイス]**
- **ディレクトリ**
- **Encrypted**
- **[非表示]**
- **IntegrityStream**
- **標準**
- **NoScrubData**
- **NotContentIndexed**
- **オフライン**
- **ReadOnly**
- **ReparsePoint**
- **Sparc ファイル**
- **システム**
- **一時**

これらの属性の詳細については、「 [Fileattributes 列挙型](/dotnet/api/system.io.fileattributes)」を参照してください。

属性を結合するには、次の演算子を使用します。

- `!` 非
- `+` と
- `,` もしくは

演算子とその属性の間にはスペースを使用しないでください。 コンマの後にスペースを入れることができます。

共通属性の場合は、次の省略形を使用します。

- `D` 名簿
- `H` 表示
- `R` (読み取り専用)
- `S` (システム)

```yaml
Type: System.Management.Automation.FlagsExpression`1[System.IO.FileAttributes]
Parameter Sets: (All)
Aliases:
Accepted values: Archive, Compressed, Device, Directory, Encrypted, Hidden, IntegrityStream, Normal, NoScrubData, NotContentIndexed, Offline, ReadOnly, ReparsePoint, SparseFile, System, Temporary

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -深さ

このパラメーターは、PowerShell 5.0 で追加され、再帰の深さを制御できるようになりました。 既定では、 `Get-ChildItem` 親ディレクトリの内容が表示されます。 **Depth** パラメーターは、再帰に含まれるサブディレクトリレベルの数を決定し、その内容を表示します。

たとえば、には、 `Depth 2` **パス** パラメーターのディレクトリ、サブディレクトリの最初のレベル、およびサブディレクトリの第2レベルが含まれます。 既定では、ディレクトリ名とファイル名が出力に含まれます。

> [!NOTE]
> PowerShell または **cmd.exe** の Windows コンピューターでは、 **tree.com** コマンドを使用してディレクトリ構造のグラフィカルビューを表示できます。

```yaml
Type: System.UInt32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Directory

ディレクトリの一覧を取得するには、 **directory パラメーターまた** は **Attributes** パラメーターを **directory** プロパティと共に使用します。 **再帰** パラメーターは **ディレクトリ** と共に使用できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: ad, d

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -除外

このコマンドレットによって操作から除外されるプロパティまたはプロパティを、文字列配列として指定します。
このパラメーターの値は、**Path** パラメーターを修飾します。 パス要素またはパターン (やなど) を入力し `*.txt` `A*` ます。 ワイルドカード文字を使用できます。

Path パラメーターの末尾のアスタリスク ( `*` ) は省略可能です。  たとえば、`-Path C:\Test\Logs` または `-Path C:\Test\Logs\*` です。 末尾のアスタリスク ( `*` ) が含まれている場合、コマンドは **Path** パラメーターのサブディレクトリに繰り返しします。 アスタリスク () がない `*` 場合は、 **Path** パラメーターの内容が表示されます。 詳細については、例5とメモのセクションを参照してください。

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

### -File

ファイルの一覧を取得するには、 **File** パラメーターを使用します。 **ファイル** では、**再帰** パラメーターを使用できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: af

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Filter

**パス** パラメーターを修飾するフィルターを指定します。 [ファイルシステム](../Microsoft.PowerShell.Core/About/about_FileSystem_Provider.md)プロバイダーは、フィルターをサポートする唯一のインストール済み PowerShell プロバイダーです。 フィルターは他のパラメーターよりも効率的です。 プロバイダーは、取得後にオブジェクトを PowerShell でフィルター処理するのではなく、コマンドレットがオブジェクトを取得したときにフィルターを適用します。 フィルター文字列は、ファイルを列挙するために .NET API に渡されます。 API では、とのワイルドカードのみがサポートさ `*` `?` れています。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -のシンボリックリンク

既定では、この `Get-ChildItem` コマンドレットは再帰中に検出されたディレクトリへのシンボリックリンクを表示しますが、それらのリンクには再帰しません。 これらのシンボリックリンクを対象としているディレクトリを検索するに **は、のようにします** 。 この **引数は動的** パラメーターであり、 **FileSystem** プロバイダーでのみサポートされています。

このパラメーターは、PowerShell 6.0 で導入されました。

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

### -Force

隠しファイルやシステムファイルなど、ユーザーがアクセスできない項目をコマンドレットで取得できるようにします。 **Force** パラメーターは、セキュリティ制限をオーバーライドしません。 実装はプロバイダーごとに異なります。 詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

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

### -Hidden

非表示の項目のみを取得するには、hidden パラメーターまたは **Attributes** パラメーターを **hidden** プロパティと共に **使用します**。 既定では、は非表示の `Get-ChildItem` 項目を表示しません。 **Force** パラメーターを使用して、非表示の項目を取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: ah, h

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

1 つ以上の場所へのパスを指定します。 **LiteralPath** の値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符は、どの文字もエスケープシーケンスとして解釈されないように PowerShell に指示します。

詳細については、「 [about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: LiteralItems
Aliases: PSPath, LP

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

場所内の項目の名前のみを取得します。 出力は、他のコマンドにパイプラインから送信できる文字列オブジェクトです。 ワイルドカードを使用できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Path

1 つ以上の場所へのパスを指定します。 ワイルドカードを使用できます。 既定の場所は、現在のディレクトリ ( `.` ) です。

```yaml
Type: System.String[]
Parameter Sets: Items
Aliases:

Required: False
Position: 0
Default value: Current directory
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: True
```

### -ReadOnly

読み取り専用の項目のみを取得するには、 **readonly** パラメーターまたは **Attributes** パラメーター **readonly** プロパティを使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: ar

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -再帰

指定された場所にある項目と、その場所のすべての子項目にある項目を取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: s

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -System

システムファイルとディレクトリのみを取得します。 システムファイルとフォルダーのみを取得するには **、システムパラメーターまた** は **Attributes** パラメーター **システム** プロパティを使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: as

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、パスを含む文字列をにパイプすることができ `Get-ChildItem` ます。

## 出力

### System.Object

が返すオブジェクトの型 `Get-ChildItem` は、プロバイダーのドライブパス内のオブジェクトによって決まります。

### System.String

**Name** パラメーターを使用すると、では、 `Get-ChildItem` オブジェクト名が文字列として返されます。

## 注

- `Get-ChildItem` は、組み込みのエイリアスである、、およびを使用して実行でき `ls` `dir` `gci` ます。 詳細については、「 [about_Aliases](../Microsoft.PowerShell.Core/About/about_Aliases.md)」を参照してください。
- `Get-ChildItem` 既定では、非表示の項目は取得されません。 隠し項目を取得するには、**Force** パラメーターを使用します。
- `Get-ChildItem`コマンドレットは、プロバイダーによって公開されるデータを使用するように設計されています。 セッションで使用可能なプロバイダーの一覧を表示するには、「」と入力 `Get-PSProvider` します。
  詳細については、「[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)」を参照してください。

## 関連リンク

[about_Certificate_Provider](../Microsoft.PowerShell.Security/About/about_Certificate_Provider.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[about_Quoting_Rules](../Microsoft.Powershell.Core/About/about_Quoting_Rules.md)

[about_Registry_Provider](../Microsoft.PowerShell.Core/About/about_Registry_Provider.md)

[ForEach-Object](../Microsoft.PowerShell.Core/ForEach-Object.md)

[Get-Alias](../Microsoft.PowerShell.Utility/Get-Alias.md)

[Get-Item](Get-Item.md)

[Get-Location](Get-Location.md)

[Get-Process](Get-Process.md)

[Get-PSProvider](Get-PSProvider.md)

[Split-Path](Split-Path.md)

