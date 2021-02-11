---
description: FileSystem (ファイル システム)
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 11/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_filesystem_provider?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: FileSystem プロバイダー
ms.openlocfilehash: 085d714fce8475dd3eeee656d1cd17db02e772a6
ms.sourcegitcommit: 7f712e12ec5b3f3f3e695da804b050ea0ce58b3a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "94661394"
---
# <a name="filesystem-provider"></a>FileSystem プロバイダー

## <a name="provider-name"></a>プロバイダー名

FileSystem (ファイル システム)

## <a name="drives"></a>ドライブ

`C:`, `D:` ...

## <a name="capabilities"></a>機能

**フィルター****処理**

## <a name="short-description"></a>簡単な説明

ファイルとディレクトリへのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

PowerShell **FileSystem** プロバイダーを使用すると、powershell でファイルとディレクトリを取得、追加、変更、消去、削除できます。

**ファイルシステム** ドライブは、コンピューター上のディレクトリとファイルを含む階層型の名前空間です。 **ファイルシステム** ドライブには、論理ドライブまたは物理ドライブ、ディレクトリ、またはマップされたネットワーク共有を指定できます。

という名前のドライブが `TEMP:` ユーザーの一時ディレクトリパスにマップされます。

>[!NOTE]
> TEMP: ドライブの内容は、PowerShell によって自動的に削除されることはなく、管理するユーザーまたはオペレーティングシステムによって行われます。 この機能は、PowerShell バージョン7.0 の試験的な機能から移行されました。

**FileSystem** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)
- [Invoke-Item](xref:Microsoft.PowerShell.Management.Invoke-Item)
- [Move-Item](xref:Microsoft.PowerShell.Management.Move-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Get-ItemProperty](xref:Microsoft.PowerShell.Management.Get-ItemProperty)
- [Set-ItemProperty](xref:Microsoft.PowerShell.Management.Set-ItemProperty)
- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)
- [Clear-ItemProperty](xref:Microsoft.PowerShell.Management.Clear-ItemProperty)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Remove-ItemProperty](xref:Microsoft.PowerShell.Management.Remove-ItemProperty)
- [Get-Acl](xref:Microsoft.PowerShell.Security.Get-Acl)
- [Set-Acl](xref:Microsoft.PowerShell.Security.Set-Acl)
- [Get-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Get-AuthenticodeSignature)
- [Set-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Set-AuthenticodeSignature)

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

ファイルは、 [system.servicemodel クラスのインスタンスです。](/dotnet/api/system.io.fileinfo) ディレクトリは、 [DirectoryInfo](/dotnet/api/system.io.directoryinfo) クラスのインスタンスです。

## <a name="navigating-the-filesystem-drives"></a>ファイルシステムドライブの移動

**FileSystem** プロバイダーは、コンピューター上のすべての論理ドライブを PowerShell ドライブとしてマップすることによって、そのデータストアを公開します。 **ファイルシステム** ドライブを操作するには、ドライブ名の後にコロン () を使用して、場所をドライブに変更し `:` ます。

```powershell
Set-Location C:
```

他の PowerShell ドライブから **FileSystem** プロバイダーを使用することもできます。 別の場所からファイルまたはディレクトリを参照するには、パスのドライブ名 ( `C:` 、 `D:` 、...) を使用します。

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスです。これ `cd` は、 [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスです。 と `pwd` は、 [Get Location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

## <a name="getting-files-and-directories"></a>ファイルとディレクトリの取得

この `Get-ChildItem` コマンドレットは、現在の場所にあるすべてのファイルとディレクトリを返します。 別のパスを指定して、組み込みパラメーターを検索して使用し、再帰の深さをフィルター処理したり制御したりすることができます。

```powershell
Get-ChildItem
```

コマンドレットの使用方法の詳細については、「 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)」を参照してください。

## <a name="copying-files-and-directories"></a>ファイルとディレクトリのコピー

`Copy-Item`コマンドレットは、指定した場所にファイルとディレクトリをコピーします。
パラメーターは、のようにフィルターと再帰に使用でき `Get-ChildItem` ます。

次のコマンドは、パス "C:\temp" の下にあるすべてのファイルとディレクトリ \" をフォルダー "C:\Windows\Temp" にコピーします。

```powershell
Copy-Item -Path C:\temp\* -Destination C:\Windows\Temp -Recurse -File
```

`Copy-Item` 確認メッセージを表示せずに、宛先ディレクトリ内のファイルを上書きします。

このコマンドは、 `a.txt` ディレクトリからディレクトリにファイルをコピーし `C:\a` `C:\a\bb` ます。

```powershell
Copy-Item -Path C:\a\a.txt -Destination C:\a\bb\a.txt
```

ディレクトリ内のすべてのディレクトリとファイル `C:\a` をディレクトリにコピーし `C:\c` ます。 コピーするディレクトリがコピー先ディレクトリにすでに存在する場合、Force パラメーターを指定しないとコマンドは失敗します。

```powershell
Copy-Item -Path C:\a\* -Destination C:\c -Recurse
```

詳細については、「 [コピー項目](xref:Microsoft.PowerShell.Management.Copy-Item)」を参照してください。

## <a name="moving-files-and-directories"></a>ファイルとディレクトリの移動

このコマンドは、 `c.txt` ディレクトリ内のファイル `C:\a` をディレクトリに移動し `C:\a\aa` ます。

```powershell
Move-Item -Path C:\a\c.txt -Destination C:\a\aa
```

このコマンドを実行して同じ名前を持つ既存ファイルが自動的に上書きされることはありません。 既存ファイルを上書きするようにコマンドレットに強制するには、Force パラメーターを指定します。

ディレクトリが現在の場所のとき、そのディレクトリを移動することはできません。 を使用し `Move-Item` てディレクトリを現在の場所に移動すると、このエラーが表示されます。

```
C:\temp> Move-Item -Path C:\temp\ -Destination C:\Windows\Temp

Move-Item : Cannot move item because the item at 'C:\temp\' is in use.
At line:1 char:1
+ Move-Item C:\temp\ C:\temp2\
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : InvalidOperation: (:) [Move-Item], PSInvalidOperationException
    + FullyQualifiedErrorId : InvalidOperation,Microsoft.PowerShell.Commands.MoveItemCommand
```

## <a name="managing-file-content"></a>ファイルコンテンツの管理

### <a name="get-the-content-of-a-file"></a>ファイルの内容を取得します。

このコマンドは、"Test.txt" ファイルの内容を取得し、コンソールに表示します。

```powershell
Get-Content -Path Test.txt
```

ファイルのコンテンツを別のコマンドレットにパイプで送ることができます。 たとえば、次のコマンドは、ファイルの内容を読み取っ `Test.txt` て、 [convertto-html](xref:Microsoft.PowerShell.Utility.ConvertTo-Html) コマンドレットへの入力として提供します。

```powershell
Get-Content -Path Test.txt | ConvertTo-Html
```

プロバイダーのパスにドル記号 () を付けることで、ファイルの内容を取得することもでき `$` ます。 パスは、変数の名前付けの制限により、中かっこで囲む必要があります。 詳細については、「 [about_Variables](about_Variables.md)」を参照してください。

```powershell
${C:\Windows\System32\Drivers\etc\hosts}
```

### <a name="add-content-to-a-file"></a>ファイルにコンテンツを追加する

このコマンドは、ファイルに "test content" という文字列を追加し `Test.txt` ます。

```powershell
Add-Content -Path test.txt -Value "test content"
```

ファイル内の既存のコンテンツ `Test.txt` は削除されません。

### <a name="replace-the-content-of-a-file"></a>ファイルの内容を置き換える

このコマンドは、ファイルの内容を `Test.txt` "test content" 文字列に置き換えます。

```powershell
Set-Content -Path test.txt -Value "test content"
```

の内容を上書き `Test.txt` します。 [新しい項目](xref:Microsoft.PowerShell.Management.New-Item)のコマンドレットの **Value** パラメーターを使用して、作成時にファイルにコンテンツを追加できます。

### <a name="loop-through-the-contents-of-a-file"></a>ファイルの内容をループする

既定では、 `Get-Content` コマンドレットは行末文字を区切り記号として使用するため、ファイルを文字列のコレクションとして取得し、各行をファイル内の1つの文字列として取得します。

パラメーターを使用して、 `-Delimiter` 別の区切り記号を指定できます。 区切り文字をセクションの終わりか次のセクションの始まりを示す文字に設定した場合、ファイルを論理部分に分割できます。

最初のコマンドは、 `Employees.txt` ファイルを取得し、セクションに分割します。各セクションは "end Of Employee Record" という語で終わり、変数に保存さ `$e` れます。

2番目のコマンドは、配列表記を使用して、のコレクション内の最初の項目を取得し `$e` ます。 PowerShell 配列は0から始まるため、0のインデックスが使用されます。

コマンドレットの詳細については、「」を `Get-Content` 参照 [し](xref:Microsoft.PowerShell.Management.Get-Content)てください。

配列の詳細については、「 [about_Arrays](../About/about_Arrays.md)」を参照してください。

```powershell
$e = Get-Content c:\test\employees.txt -Delimited "End Of Employee Record"
$e[0]
```

## <a name="managing-security-descriptors"></a>セキュリティ記述子の管理

### <a name="view-the-acl-for-a-file"></a>ファイルの ACL を表示する

このコマンドは、 [accesscontrol-namespace. system.security.accesscontrol.filesecurity](/dotnet/api/system.security.accesscontrol.filesecurity) オブジェクトを返します。

```powershell
Get-Acl -Path test.txt | Format-List -Property *
```

このオブジェクトの詳細については、コマンドを [Get Member](xref:Microsoft.PowerShell.Utility.Get-Member) コマンドレットにパイプ処理してください。 または、「 [System.security.accesscontrol.filesecurity](/dotnet/api/system.security.accesscontrol.filesecurity) クラス」を参照してください。

### <a name="modify-the-acl-for-a-file"></a>ファイルの ACL を変更する

### <a name="create-and-set-an-acl-for-a-file"></a>ファイルの ACL を作成および設定する

## <a name="creating-files-and-directories"></a>ファイルとディレクトリの作成

### <a name="create-a-directory"></a>ディレクトリを作成する

このコマンドにより、ドライブにディレクトリが作成され `logfiles` `C` ます。

```powershell
New-Item -Path c:\ -Name logfiles -Type directory
```

PowerShell には、 `mkdir` `md` [new Item](xref:Microsoft.PowerShell.Management.New-Item) コマンドレットを使用して新しいディレクトリを作成する関数 (別名) も含まれています。

### <a name="create-a-file"></a>ファイルを作成する

このコマンドは、 `log2.txt` ディレクトリにファイルを作成し、 `C:\logfiles` ファイルに "test log" という文字列を追加します。

```powershell
New-Item -Path c:\logfiles -Name log2.txt -Type file
```

### <a name="create-a-file-with-content"></a>コンテンツを持つファイルを作成する

ディレクトリにという名前のファイルを作成 `log2.txt` `C:\logfiles` し、ファイルに "test log" という文字列を追加します。

```powershell
New-Item -Path c:\logfiles -Name log2.txt -Type file -Value "test log"
```

## <a name="renaming-files-and-directories"></a>ファイルとディレクトリの名前の変更

### <a name="rename-a-file"></a>ファイル名を変更する

このコマンドにより `a.txt` 、ディレクトリ内のファイルの名前が次のように変更され `C:\a` `b.txt` ます。

```powershell
Rename-Item -Path c:\a\a.txt -NewName b.txt
```

### <a name="rename-a-directory"></a>ディレクトリ名の変更

このコマンドにより、ディレクトリの名前が次のように変更され `C:\a\cc` `C:\a\dd` ます。

```powershell
Rename-Item -Path c:\a\cc -NewName dd
```

## <a name="deleting-files-and-directories"></a>削除 (ファイルとディレクトリを)

### <a name="delete-a-file"></a>ファイルを削除する

このコマンドを実行すると、現在のディレクトリ内のファイルが削除され `Test.txt` ます。

```powershell
Remove-Item -Path test.txt
```

### <a name="delete-files-using-wildcards"></a>ワイルドカードを使用してファイルを削除する

次のコマンドは、ファイル名拡張子を持つ現在のディレクトリ内のすべてのファイルを削除し `.xml` ます。

```powershell
Remove-Item -Path *.xml
```

## <a name="starting-a-program-by-invoking-an-associated-file"></a>関連付けられたファイルを呼び出してプログラムを起動する

### <a name="invoke-a-file"></a>ファイルを呼び出す

最初のコマンドは、 [Get Service](xref:Microsoft.PowerShell.Management.Get-Service) コマンドレットを使用して、ローカルサービスに関する情報を取得します。

情報を [エクスポート Csv](xref:Microsoft.PowerShell.Utility.Export-Csv) コマンドレットにパイプし、その情報をファイルに格納し `Services.csv` ます。

2番目のコマンドは、 [呼び出し項目](xref:Microsoft.PowerShell.Management.Invoke-Item) を使用して、 `services.csv` 拡張機能に関連付けられているプログラムでファイルを開き `.csv` ます。

```powershell
Get-Service | Export-Csv -Path services.csv
Invoke-Item -Path services.csv
```

## <a name="getting-files-and-folders-with-specified-attributes"></a>指定された属性を持つファイルとフォルダーを取得する

### <a name="get-system-files"></a>システムファイルを取得する

このコマンドを実行すると、現在のディレクトリとそのサブディレクトリにあるシステム ファイルが取得されます。

パラメーターを使用して、 `-File` (ディレクトリではなく) ファイルのみを取得し、パラメーターを使用して `-System` "system" 属性を持つ項目のみを取得します。

パラメーターを使用して、 `-Recurse` 現在のディレクトリとすべてのサブディレクトリ内の項目を取得します。

```powershell
Get-ChildItem -File -System -Recurse
```

### <a name="get-hidden-files"></a>隠しファイルの取得

このコマンドは、非表示ファイルを含む、現在のディレクトリにあるすべてのファイルを取得します。

**Attributes** パラメーターには、 `!Directory+Hidden` 非表示ファイルを取得すると、 `!Directory` 他のすべてのファイルを取得するという2つの値があります。

```powershell
Get-ChildItem -Attributes !Directory,!Directory+Hidden
```

`dir -att !d,!d+h` は、このコマンドに相当します。

### <a name="get-compressed-and-encrypted-files"></a>圧縮されたファイルと暗号化されたファイルを取得する

このコマンドを実行すると、現在のディレクトリにあり、圧縮または暗号化されているファイルが取得されます。

`-Attributes`2 つの値 (と) を持つパラメーターを使用し `Compressed` `Encrypted` ます。 値は、 `,` "or" 演算子を表すコンマで区切られます。

```powershell
Get-ChildItem -Attributes Compressed,Encrypted
```

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターは、PowerShell プロバイダーによって追加されるコマンドレットパラメーターであり、プロバイダーに対応したドライブでコマンドレットが使用されている場合にのみ使用できます。

### <a name="encoding-microsoftpowershellcommandsfilesystemcmdletproviderencoding"></a>Encoding \<Microsoft.PowerShell.Commands.FileSystemCmdletProviderEncoding\>

ファイル エンコードを指定します。 既定値は ASCII です。

- **Ascii**: ascii (7 ビット) 文字セットのエンコーディングを使用します。
- **BigEndianUnicode**: ビッグエンディアンのバイト順を使用して utf-16 形式でエンコードします。
- **String**: 文字列のエンコーディングの種類を使用します。
- **Unicode**: リトルエンディアンのバイト順を使用して utf-16 形式でエンコードします。
- **UTF7**: utf-7 形式でエンコードします。
- **UTF8**: utf-8 形式でエンコードします。
- **UTF8BOM**: バイト順マーク (BOM) を使用して utf-8 形式でエンコードします。
- **UF8NOBOM**: バイト順マーク (BOM) を使用せずに utf-8 形式でエンコードします。
- **UTF32**:32 utf-8 形式でエンコードします。
- **既定** 値は、インストールされている既定のコードページでエンコードされます。
- **OEM**: MS-DOS およびコンソールプログラムの既定のエンコードを使用します。
- **不明**: エンコードの種類が不明または無効です。 データはバイナリとして処理できます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Add-Content](xref:Microsoft.PowerShell.Management.Add-Content)
- [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)
- [Set-Content](xref:Microsoft.PowerShell.Management.Set-Content)

### <a name="delimiter-systemstring"></a>Delimiter \<System.String\>

[Get-Content](xref:Microsoft.PowerShell.Management.Get-Content) で使用され、読み込み中にファイルをオブジェクトに分割する区切り文字を指定します。

既定値は `\n` 、行末文字です。

テキストファイルを読み取るときに、 [Get Content](xref:Microsoft.PowerShell.Management.Get-Content) は文字列オブジェクトのコレクションを返します。各オブジェクトは、区切り文字で終わります。

ファイルに存在しない区切り文字を入力すると、 [Get Content](xref:Microsoft.PowerShell.Management.Get-Content) は、ファイル全体を単一の区切られていないオブジェクトとして返します。

このパラメーターを使用し、大きなファイルを小さなファイルに分割します。「End of Example」のようなファイル区切り記号を区切り文字として指定します。 区切り文字は予約されています (破棄されません)。各ファイル セクションの最後の項目になります。

> [!NOTE]
> 現在、パラメーターの値が空の文字列の場合、 `-Delimiter` [Get](xref:Microsoft.PowerShell.Management.Get-Content) は何も返しません。
> これは既知の問題です。 ファイル全体を 1 つの区切りのない文字列として返すように [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content) に強制するには、ファイルに存在しない値を入力します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)

### <a name="wait-systemmanagementautomationswitchparameter"></a>Wait \<System.Management.Automation.SwitchParameter\>

コンテンツがファイルに追加されるのを待ちます。 コンテンツが追加される場合、追加されたコンテンツを返します。 コンテンツが変更された場合、ファイル全体が返されます。

待機中に、[Get-Content](xref:Microsoft.PowerShell.Management.Get-Content) は 1 秒おきにファイルをチェックします。これは CTRL+C を押すなどして中断するまで続きます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)

### <a name="attributes-flagsexpression"></a>Attributes \<FlagsExpression\>

指定した属性のファイルとフォルダーを取得します。 このパラメーターはすべての属性をサポートします。複雑な属性の組み合わせを指定できます。

この `-Attributes` パラメーターは、Windows PowerShell 3.0 で導入されました。

パラメーターでは `-Attributes` 、次の属性がサポートされています。

- **Archive**
- **Compressed**
- **[デバイス]**
- **ディレクトリ**
- **Encrypted**
- **[非表示]**
- **標準**
- **NotContentIndexed**
- **オフライン**
- **ReadOnly**
- **ReparsePoint**
- **Sparc ファイル**
- **システム**
- **一時**

これらの属性の詳細については、「 [Fileattributes](/dotnet/api/system.io.fileattributes) 列挙型」を参照してください。

次の演算子を利用して属性を結合します。

- `!` -NOT
- `+` -および
- `,` または

演算子とその属性の間にはスペースを挿入できません。 ただし、コンマの前にはスペースを挿入できます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="directory-systemmanagementautomationswitchparameter"></a>Directory \<System.Management.Automation.SwitchParameter\>

ディレクトリ (フォルダー) を取得します。

この `-Directory` パラメーターは、Windows PowerShell 3.0 で導入されました。

ディレクトリだけを取得するには、パラメーターを使用 `-Directory` してパラメーターを省略し `-File` ます。 ディレクトリを除外するには、パラメーターを使用 `-File` してパラメーターを省略する `-Directory` か、パラメーターを使用し `-Attributes` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="file-systemmanagementautomationswitchparameter"></a>File \<System.Management.Automation.SwitchParameter\>

ファイルを取得します。

この `-File` パラメーターは、Windows PowerShell 3.0 で導入されました。

ファイルのみを取得するには、パラメーターを使用 `-File` してパラメーターを省略し `-Directory` ます。 ファイルを除外するには、パラメーターを使用 `-Directory` してパラメーターを省略する `-File` か、パラメーターを使用し `-Attributes` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="hidden-systemmanagementautomationswitchparameter"></a>Hidden \<System.Management.Automation.SwitchParameter\>

非表示のファイルとディレクトリ (フォルダー) のみを取得します。 既定では、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem) は非表示項目のみを取得します。

この `-Hidden` パラメーターは、Windows PowerShell 3.0 で導入されました。

非表示の項目のみを取得するには、パラメーター、 `-Hidden` その `h` または `ah` 別名、またはパラメーターの **非表示** の値を使用し `-Attributes` ます。 非表示の項目を除外するには、パラメーターを省略する `-Hidden` か、パラメーターを使用 `-Attributes` します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="readonly-systemmanagementautomationswitchparameter"></a>ReadOnly \<System.Management.Automation.SwitchParameter\>

読み取り専用のファイルとディレクトリ (フォルダー) を取得します。

この `-ReadOnly` パラメーターは、Windows PowerShell 3.0 で導入されました。

読み取り専用の項目だけを取得するには、パラメーター、 `-ReadOnly` その `ar` エイリアス、またはパラメーターの **ReadOnly** 値を使用し `-Attributes` ます。 読み取り専用の項目を除外するには、パラメーターを使用し `-Attributes` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="system-systemmanagementautomationswitchparameter"></a>System \<System.Management.Automation.SwitchParameter\>

システムのファイルとディレクトリ (フォルダー) を取得します。

この `-System` パラメーターは、Windows PowerShell 3.0 で導入されました。

システムファイルとフォルダーのみを取得するには、パラメーター、 `-System` その `as` エイリアス、またはパラメーターの **システム** 値を使用し `-Attributes` ます。 システムファイルとフォルダーを除外するには、パラメーターを使用し `-Attributes` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)

### <a name="newerthan-systemdatetime"></a>NewerThan \<System.DateTime\>

`$True` `LastWriteTime` ファイルの値が指定した日付よりも大きい場合に、を返します。 それ以外の場合は `$False`を返します。

[Datetime](/dotnet/api/system.datetime)オブジェクトを入力します。たとえば、 [Get Date](xref:Microsoft.PowerShell.Utility.Get-Date)コマンドレットが返す値や、 [datetime](/dotnet/api/system.datetime)オブジェクトに変換できる文字列 (など) を入力し `"August 10, 2011 2:00 PM"` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Test-Path](xref:Microsoft.PowerShell.Management.Test-Path)

### <a name="olderthan-systemdatetime"></a>OlderThan \<System.DateTime\>

`$True` `LastWriteTime` ファイルの値が指定した日付よりも小さい場合に、を返します。 それ以外の場合は `$False`を返します。

[Datetime](/dotnet/api/system.datetime)オブジェクトを入力します。たとえば、 [Get Date](xref:Microsoft.PowerShell.Utility.Get-Date)コマンドレットが返す値や、 [datetime](/dotnet/api/system.datetime)オブジェクトに変換できる文字列 (など) を入力し `"August 10, 2011 2:00 PM"` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Test-Path](xref:Microsoft.PowerShell.Management.Test-Path)

### <a name="stream-systemstring"></a>Stream \<System.String\>

代替データ ストリームを管理します。 ストリーム名を入力します。 ワイルドカードは、ファイルシステムドライブ内の項目の [取得](xref:Microsoft.PowerShell.Management.Get-Item) および [削除](xref:Microsoft.PowerShell.Management.Remove-Item) コマンドでのみ使用できます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Add-Content](xref:Microsoft.PowerShell.Management.Add-Content)
- [Clear-Content](xref:Microsoft.PowerShell.Management.Clear-Content)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Set-Content](xref:Microsoft.PowerShell.Management.Set-Content)

### <a name="raw-switchparameter"></a>Raw \<SwitchParameter\>

改行文字が無視されます。 コンテンツを 1 つの項目として返します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [Get-Content](xref:Microsoft.PowerShell.Management.Get-Content)

### <a name="itemtype-string"></a>ItemType \<String\>

このパラメーターを使用すると、作成する項目の tye を指定できます。 `New-Item`

このパラメーターで使用できる値は、使用している現在のプロバイダーによって異なります。

ドライブでは、 `FileSystem` 次の値が許可されます。

- ファイル
- ディレクトリ
- SymbolicLink
- ジャンクション
- Hardlink create

### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

## <a name="using-the-pipeline"></a>パイプラインの使用

プロバイダーのコマンドレットは、パイプラインの入力を受け入れます。 パイプラインを使用すると、1つのコマンドレットから別のプロバイダーのコマンドレットにプロバイダーデータを送信することにより、タスクを簡単に行うことができます。
プロバイダーコマンドレットでパイプラインを使用する方法の詳細については、この記事全体で説明されているコマンドレットリファレンスを参照してください。

## <a name="getting-help"></a>ヘルプの表示

Windows PowerShell 3.0 より、プロバイダー コマンドレットのためにカスタマイズされたヘルプ トピックを取得できます。これはファイル システム ドライブでのプロバイダー コマンドレットの動作を説明します。

ファイルシステムドライブ用にカスタマイズされたヘルプトピックを取得するには、ファイルシステムドライブで [get-help](xref:Microsoft.PowerShell.Core.Get-Help) コマンドを実行するか、 `-Path` [get-help](xref:Microsoft.PowerShell.Core.Get-Help) のパラメーターを使用してファイルシステムドライブを指定します。

```powershell
Get-Help Get-ChildItem
```

```powershell
Get-Help Get-ChildItem -Path c:
```

## <a name="see-also"></a>関連項目

[about_Providers](../About/about_Providers.md)
