---
title: 書式設定、エイリアス、プロバイダー、比較
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
description: この章では、出力書式設定、コマンド エイリアス、プロバイダー、比較演算の概念について説明します。
ms.openlocfilehash: 5573ca58601af0c6af15736b772a9792d8d77a79
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99600778"
---
# <a name="chapter-5---formatting-aliases-providers-comparison"></a>第 5 章 - 書式設定、エイリアス、プロバイダー、比較

## <a name="requirements"></a>必要条件

この章で紹介する例の中には、SQL Server PowerShell モジュールを必要とするものがいくつかあります。 このモジュールは、[SQL Server Management Studio (SSMS)][SMSS] の一部としてインストールされます。 このモジュールは、以降の章でも使用されるので、 Windows 10 ラボ環境のコンピューターにダウンロードし、インストールしてください。

## <a name="format-right"></a>右で書式設定

第 4 章では、できるだけ左端でフィルター処理する方法について学習しました。 コマンドの出力を手動で書式設定するためのルールは、このルールに似ています (できるだけ右で実行する必要がある場合を除く)。

最も一般的な書式設定コマンドは `Format-Table` と `Format-List` です。 `Format-Wide` と `Format-Custom` を使用することもできますが、あまり一般的ではありません。

第 3 章で説明したように、カスタム書式設定を使用しない限り、4 つを超えるプロパティを返すコマンドは既定でリストになります。

```powershell
Get-Service -Name w32time | Select-Object -Property Status, DisplayName, Can*
```

```Output
Status              : Running
DisplayName         : Windows Time
CanPauseAndContinue : False
CanShutdown         : True
CanStop             : True
```

`Format-Table` コマンドレットを使用して書式設定を手動でオーバーライドし、リストではなくテーブルに出力を表示します。

```powershell
Get-Service -Name w32time | Select-Object -Property Status, DisplayName, Can* |
Format-Table
```

```Output
 Status DisplayName  CanPauseAndContinue CanShutdown CanStop
 ------ -----------  ------------------- ----------- -------
Running Windows Time               False        True    True
```

`Get-Service` の既定の出力は、テーブル内の 3 つのプロパティです。

```powershell
Get-Service -Name w32time
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  w32time            Windows Time
```

`Format-List` コマンドレットを使用して既定の書式設定をオーバーライドし、結果をリストで返します。

```powershell
Get-Service -Name w32time | Format-List
```

```Output
Name                : w32time
DisplayName         : Windows Time
Status              : Running
DependentServices   : {}
ServicesDependedOn  : {}
CanPauseAndContinue : False
CanShutdown         : True
CanStop             : True
ServiceType         : Win32ShareProcess
```

`Get-Service` を `Format-List` にパイプ処理するだけで、追加のプロパティが返されることに注意してください。 その特定のコマンドについては書式設定がバックグラウンドで行われるため、これはすべてのコマンドで発生するわけではありません。

書式設定コマンドレットを使うときに最も注意しなければならないのは、このコマンドレットによって生成される書式設定オブジェクトは、PowerShell の通常のオブジェクトと異なるという点です。

```powershell
Get-Service -Name w32time | Format-List | Get-Member
```

```Output
   TypeName: Microsoft.PowerShell.Commands.Internal.Format.FormatStartData

Name                                    MemberType Definition
----                                    ---------- ----------
Equals                                  Method     bool Equals(System.Object obj)
GetHashCode                             Method     int GetHashCode()
GetType                                 Method     type GetType()
ToString                                Method     string ToString()
autosizeInfo                            Property   Microsoft.PowerShell.Commands.Inter...
ClassId2e4f51ef21dd47e99d3c952918aff9cd Property   string ClassId2e4f51ef21dd47e99d3c9...
groupingEntry                           Property   Microsoft.PowerShell.Commands.Inter...
pageFooterEntry                         Property   Microsoft.PowerShell.Commands.Inter...
pageHeaderEntry                         Property   Microsoft.PowerShell.Commands.Inter...
shapeInfo                               Property   Microsoft.PowerShell.Commands.Inter...

   TypeName: Microsoft.PowerShell.Commands.Internal.Format.GroupStartData

Name                                    MemberType Definition
----                                    ---------- ----------
Equals                                  Method     bool Equals(System.Object obj)
GetHashCode                             Method     int GetHashCode()
GetType                                 Method     type GetType()
ToString                                Method     string ToString()
ClassId2e4f51ef21dd47e99d3c952918aff9cd Property   string ClassId2e4f51ef21dd47e99d3c9...
groupingEntry                           Property   Microsoft.PowerShell.Commands.Inter...
shapeInfo                               Property   Microsoft.PowerShell.Commands.Inter...

   TypeName: Microsoft.PowerShell.Commands.Internal.Format.FormatEntryData

Name                                    MemberType Definition
----                                    ---------- ----------
Equals                                  Method     bool Equals(System.Object obj)
GetHashCode                             Method     int GetHashCode()
GetType                                 Method     type GetType()
ToString                                Method     string ToString()
ClassId2e4f51ef21dd47e99d3c952918aff9cd Property   string ClassId2e4f51ef21dd47e99d3c9...
formatEntryInfo                         Property   Microsoft.PowerShell.Commands.Inter...
outOfBand                               Property   bool outOfBand {get;set;}
writeStream                             Property   Microsoft.PowerShell.Commands.Inter...

   TypeName: Microsoft.PowerShell.Commands.Internal.Format.GroupEndData

Name                                    MemberType Definition
----                                    ---------- ----------
Equals                                  Method     bool Equals(System.Object obj)
GetHashCode                             Method     int GetHashCode()
GetType                                 Method     type GetType()
ToString                                Method     string ToString()
ClassId2e4f51ef21dd47e99d3c952918aff9cd Property   string ClassId2e4f51ef21dd47e99d3c9...
groupingEntry                           Property   Microsoft.PowerShell.Commands.Inter...

   TypeName: Microsoft.PowerShell.Commands.Internal.Format.FormatEndData

Name                                    MemberType Definition
----                                    ---------- ----------
Equals                                  Method     bool Equals(System.Object obj)
GetHashCode                             Method     int GetHashCode()
GetType                                 Method     type GetType()
ToString                                Method     string ToString()
ClassId2e4f51ef21dd47e99d3c952918aff9cd Property   string ClassId2e4f51ef21dd47e99d3c9...
groupingEntry                           Property   Microsoft.PowerShell.Commands.Inter...
```

つまり、書式設定コマンドのパイプ処理の対象となるコマンドは、それほどありません。 一部の `Out-*` コマンドに対してはパイプ処理できますが、それが限度です。 行の最後で書式設定 (右で書式設定) する必要があるのは、そのためです。

## <a name="aliases"></a>エイリアス

PowerShell のエイリアスは、短いコマンド名です。 PowerShell には、一連の組み込みエイリアスが含まれていますが、独自のエイリアスを定義することもできます。

`Get-Alias` コマンドレットは、エイリアスを見つけるときに使用します。 コマンドのエイリアスが既にわかっている場合は、**Name** パラメーターを使用して、エイリアスが関連付けられているコマンドを確認します。

```powershell
Get-Alias -Name gcm
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           gcm -> Get-Command
```

**Name** パラメーターの値には、複数のエイリアスを指定できます。

```powershell
Get-Alias -Name gcm, gm
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           gcm -> Get-Command
Alias           gm -> Get-Member
```

**Name** パラメーターは位置指定パラメーターであるため、省略されていることがよくあります。

```powershell
Get-Alias gm
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           gm -> Get-Member
```

コマンドのエイリアスを検索する場合は、**Definition** パラメーターを使用する必要があります。

```powershell
Get-Alias -Definition Get-Command, Get-Member
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Alias           gcm -> Get-Command
Alias           gm -> Get-Member
```

**Definition** パラメーターは位置指定には使用できないため、必ず指定する必要があります。

エイリアスを使用すると、キーストロークをいくつか省略できるため、コンソールにコマンドを入力するときは便利です。
このエイリアスは、保存するスクリプトや他のユーザーと共有するコードでは使用しないでください。 本書で前述したように、完全なコマンドレットとパラメーター名を使用すると自己文書化されるため、理解しやすくなります。

独自のエイリアスは、お使いのコンピューター上の現在の PowerShell セッションにしか存在しないため、作成するときは注意が必要です。

## <a name="providers"></a>プロバイダー

PowerShell のプロバイダーは、データストアに、ファイル システムのようにアクセスできるようにするインターフェイスです。 PowerShell には、組み込みプロバイダーが複数用意されています。

```powershell
Get-PSProvider
```

```Output
Name                 Capabilities                       Drives
----                 ------------                       ------
Registry             ShouldProcess, Transactions        {HKLM, HKCU}
Alias                ShouldProcess                      {Alias}
Environment          ShouldProcess                      {Env}
FileSystem           Filter, ShouldProcess, Credentials {C, A, D}
Function             ShouldProcess                      {Function}
Variable             ShouldProcess                      {Variable}
Certificate          ShouldProcess                      {Cert}
WSMan                Credentials                        {WSMan}
```

上記の結果が示すように、レジストリ、エイリアス、環境変数、ファイル システム、関数、変数、証明書、および WSMan に対して、組み込みプロバイダーが用意されています。

これらのプロバイダーがデータストアの公開に使用する実際のドライブは、`Get-PSDrive` コマンドレットを使って確認できます。 `Get-PSDrive` コマンドレットによって表示されるのは、プロバイダーが公開しているドライブだけではありません。ネットワーク共有にマップされたドライブなど、Windows 論理ドライブも表示されます。

```powershell
Get-PSDrive
```

```Output
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
A                                      FileSystem    A:\
Alias                                  Alias
C                  14.41        112.10 FileSystem    C:\
Cert                                   Certificate   \
D                                      FileSystem    D:\
Env                                    Environment
Function                               Function
HKCU                                   Registry      HKEY_CURRENT_USER
HKLM                                   Registry      HKEY_LOCAL_MACHINE
Variable                               Variable
WSMan                                  WSMan
```

Active Directory PowerShell モジュール、SQLServer PowerShell モジュールといったサードパーティ モジュールそれぞれによって、独自の PowerShell プロバイダーと PSDrive が追加されます。

Active Directory PowerShell モジュールと SQL Server PowerShell モジュールをインポートします。

```powershell
Import-Module -Name ActiveDirectory, SQLServer
```

追加 PowerShell プロバイダーが追加されたかどうかを確認します。

```powershell
Get-PSProvider
```

```Output
Name                 Capabilities                       Drives
----                 ------------                       ------
Registry             ShouldProcess, Transactions        {HKLM, HKCU}
Alias                ShouldProcess                      {Alias}
Environment          ShouldProcess                      {Env}
FileSystem           Filter, ShouldProcess, Credentials {C, A, D}
Function             ShouldProcess                      {Function}
Variable             ShouldProcess                      {Variable}
ActiveDirectory      Include, Exclude, Filter, Shoul... {AD}
SqlServer            Credentials                        {SQLSERVER}
```

この結果セットでは、2 つの PowerShell プロバイダーが新しく存在していることに注意してください。1 つは Active Directory 用、もう 1 つは SQL Server 用のプロバイダーです。

各モジュールの PSDrive も追加されました。

```powershell
Get-PSDrive
```

```Output
Name           Used (GB)     Free (GB) Provider      Root
----           ---------     --------- --------      ----
A                                      FileSystem    A:\
AD                                     ActiveDire... //RootDSE/
Alias                                  Alias
C                  19.38        107.13 FileSystem    C:\
Cert                                   Certificate   \
D                                      FileSystem    D:\
Env                                    Environment
Function                               Function
HKCU                                   Registry      HKEY_CURRENT_USER
HKLM                                   Registry      HKEY_LOCAL_MACHINE
SQLSERVER                              SqlServer     SQLSERVER:\
Variable                               Variable
WSMan                                  WSMan
```

PSDrives には、従来のファイル システムと同じようにアクセスできます。

```powershell
Get-ChildItem -Path Cert:\LocalMachine\CA
```

```Output
   PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\CA

Thumbprint                                Subject
----------                                -------
FEE449EE0E3965A5246F000E87FDE2A065FD89D4  CN=Root Agency
D559A586669B08F46A30A133F8A9ED3D038E2EA8  OU=www.verisign.com/CPS Incorporated LIABI...
109F1CAED645BB78B3EA2B94C0697C740733031C  CN=Microsoft Windows Hardware Compatibility,...
```

## <a name="comparison-operators"></a>比較演算子

PowerShell には、値を比較したり、特定のパターンに一致する値を検索したりするときに使用する比較演算子が多数含まれています。 表 5-1 は、PowerShell の比較演算子の一覧です。

|    演算子    |                          定義                          |
| -------------- | ------------------------------------------------------------ |
| `-eq`          | 等しい                                                     |
| `-ne`          | 等しくない                                                 |
| `-gt`          | より大きい                                                 |
| `-ge`          | 以上                                     |
| `-lt`          | より小さい                                                    |
| `-le`          | 以下                                        |
| `-Like`        | `*` ワイルドカード文字を使用した一致                       |
| `-NotLike`     | `*` ワイルドカード文字を使用した不一致              |
| `-Match`       | 指定した正規表現と一致                     |
| `-NotMatch`    | 指定した正規表現と一致しない              |
| `-Contains`    | コレクションに指定した値が含まれているかどうかを確認する        |
| `-NotContains` | コレクションに特定の値が含まれていないことを確認する |
| `-In`          | 指定した値がコレクションに存在するかどうかを確認する           |
| `-NotIn`       | 指定した値がコレクションに含まれていないことを確認する       |
| `-Replace`     | 指定した値を置き換える                                 |

表 5-1 に記載されている演算子はすべて、大文字と小文字が区別されません。 大文字と小文字を区別するには、表 5-1 に示されている演算子の前に `c` を置きます。 たとえば、`-ceq` は、大文字と小文字が区別される `-eq` 比較演算子です。

一部が大文字になっている "PowerShell" は、等号比較演算子を使用した場合、すべてが小文字の "powershell" と等しくなります。

```powershell
'PowerShell' -eq 'powershell'
```

```Output
True
```

大文字と小文字が区別される等号比較演算子を使用した場合、これは等しくなりません。

```powershell
'PowerShell' -ceq 'powershell'
```

```Output
False
```

不等号比較演算子は条件を逆にします。

```powershell
'PowerShell' -ne 'powershell'
```

```Output
False
```

"より大きい"、"以上"、"より小さい"、"以下" のすべてを、文字列または数値で利用できます。

```powershell
5 -gt 5
```

```Output
False
```

この例で "より大きい" ではなく "以上" を使用すると、5 と 5 は等しいため、**ブール値** true が返されます。

```powershell
5 -ge 5
```

```Output
True
```

この 2 つの例の結果から、"より小さい" または "以下" の動作を推測できます。

```powershell
5 -lt 10
```

```Output
True
```

`-Like` 演算子と `-Match` 演算子については、経験豊富な PowerShell ユーザーでも混乱するかもしれません。 `-Like` はワイルドカード文字 `*` および `?` と共に使用され、"like" 照合を実行します。

```powershell
'PowerShell' -like '*shell'
```

```Output
True
```

`-Match` では、正規表現を使用して照合が行われます。

```powershell
'PowerShell' -match '^*.shell$'
```

```Output
True
```

1 から 10 までの数値を変数に格納するには、範囲演算子を使用します。

```powershell
$Numbers = 1..10
```

```Output
```

`$Numbers` 変数に 15 が含まれているかどうかを確認してみましょう。

```powershell
$Numbers -contains 15
```

```Output
False
```

数値 10 が含まれているかどうかを確認します。

```powershell
$Numbers -contains 10
```

```Output
True
```

`-NotContains` はロジックを逆にして、`$Numbers` 変数に値が含まれていないことを確認します。

```powershell
$Numbers -notcontains 15
```

```Output
True
```

この例では、`$Numbers` 変数には確かに 15 が含まれていないため、**ブール値** true が返されます。 しかし、数値 10 は含まれているため、テストを実行すると false になります。

```powershell
$Numbers -notcontains 10
```

```Output
False
```

"in" 比較演算子は、PowerShell バージョン3.0 で初めて導入されました。 これは、値が配列に "ある" かどうかを確認するときに使用します。 `$Numbers` 変数は配列です。この変数には、複数の値が含まれているためです。

```powershell
15 -in $Numbers
```

```Output
False
```

つまり、`-in` は、contains 比較演算子と同じテストを実行します。ただし、逆方向からはテストされません。

```powershell
10 -in $Numbers
```

```Output
True
```

15 は `$Numbers` 配列にないため、次の例では false が返されます。

```powershell
15 -in $Numbers
```

```Output
False
```

`-contains` 演算子と同様に、`not` は `-in` 演算子のロジックを逆にします。

```powershell
10 -notin $Numbers
```

```Output
False
```

この例では、`$Numbers` 配列に 10 が含まれており、この条件では 10 が含まれていないことを確認するテストを行ったため、false が返されます。

15は `$Numbers` 配列に "ない" ため、**ブール値** true が返されます。

```powershell
15 -notin $Numbers
```

```Output
True
```

`-replace` 演算子は、お察しのとおりに動作します。 これは何かを置き換えるときに使用します。 ある値を指定すると、その値がなくなります。 次の例では、"Shell" がなくなっています。

```powershell
'PowerShell' -replace 'Shell'
```

```Output
Power
```

ある値を別の値に置き換える場合は、置換後の新しい値を、置き換えたいパターンの後ろで指定します。 "SQL Saturday in Baton Rouge" は、毎年の講演イベントです。 次の例で、"Saturday" という単語を省略形 "Sat" に置き換えます。

```powershell
'SQL Saturday - Baton Rouge' -Replace 'saturday','Sat'
```

```Output
SQL Sat - Baton Rouge
```

Replace 演算子と同じように動作する **Replace ()** などのメソッドを使って、何かを置き換えることもできます。 ただし、`-Replace` 演算子では、既定では大文字と小文字が区別されませんが、**Replace ()** メソッドでは大文字と小文字が区別されます。

```powershell
'SQL Saturday - Baton Rouge'.Replace('saturday','Sat')
```

```Output
SQL Saturday - Baton Rouge
```

この例では "Saturday" という単語が置き換えられていないことに注意してください。 これは、大文字と小文字の使い方が元の単語と異なるためです。 元の単語と同じように "Saturday" と指定すれば、**Replace ()** メソッドによって、意図したとおりに置き換えられます。

```powershell
'SQL Saturday - Baton Rouge'.Replace('Saturday','Sat')
```

```Output
SQL Sat - Baton Rouge
```

メソッドを使用してデータを変換するときは、"_トルコ テスト_" の失敗のように、予期しない問題が発生する可能性があるため注意が必要です。 例については、「[Pester を使用した他のカルチャでの PowerShell コードのテスト][]」というタイトルのブログ記事を参照してください。 このような問題が発生しないように、できるだけメソッドではなく演算子を使用することをお勧めします。

比較演算子は前の例で紹介したように使用できますが、通常は、`Where-Object` コマンドレットを使用して、何らかのフィルター処理を行っています。

## <a name="summary"></a>まとめ

この章では、右で書式設定、エイリアス、プロバイダー、および比較演算子に関するさまざまなトピックについて学習しました。

## <a name="review"></a>確認

1. できるだけ右で書式設定を行う必要があるのは、なぜですか。
1. `%` エイリアスを対象とした実際のコマンドレットを確認するには、どうすればよいですか。
1. 保存するスクリプトや他のユーザーと共有するコードで使用すべきではないのは、なぜですか。
1. いずれかのレジストリ プロバイダーに関連付けられているドライブで、ディレクトリのリストを実行してください。
1. 置換メソッドではなく置換演算子を使用する主なメリットの 1 つは何ですか。

## <a name="recommended-reading"></a>推奨トピック

- [Format-Table][]
- [Format-List][]
- [Format-Wide][]
- [about_Aliases][]
- [about_Providers][]
- [about_Comparison_Operators][]
- [about_Arrays][]

<!-- link references -->
[SMSS]: /sql/ssms/download-sql-server-management-studio-ssms
[Format-Table]: /powershell/module/microsoft.powershell.utility/format-table
[Format-List]: /powershell/module/microsoft.powershell.utility/format-list
[Format-Wide]: /powershell/module/microsoft.powershell.utility/format-wide
[about_Aliases]: /powershell/module/microsoft.powershell.core/about/about_aliases
[about_Providers]: /powershell/module/microsoft.powershell.core/about/about_providers
[about_Comparison_Operators]: /powershell/module/microsoft.powershell.core/about/about_comparison_operators
[about_Arrays]: /powershell/module/microsoft.powershell.core/about/about_arrays
[Pester を使用した他のカルチャでの PowerShell コードのテスト]: https://mikefrobbins.com/2015/10/22/using-pester-to-test-powershell-code-with-other-cultures/
