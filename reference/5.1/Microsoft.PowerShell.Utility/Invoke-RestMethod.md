---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/20/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-RestMethod
ms.openlocfilehash: 136ae55388256076e1b5843f0569c6ab4b28f0b6
ms.sourcegitcommit: b10731301412afd4111743b85da95e8c25583533
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107756267"
---
# Invoke-RestMethod

## 構文
RESTful Web サービスに HTTP または HTTPS 要求を送信します。

## 構文

```
Invoke-RestMethod [-Method <WebRequestMethod>] [-UseBasicParsing] [-Uri] <Uri>
 [-WebSession <WebRequestSession>] [-SessionVariable <String>] [-Credential <PSCredential>]
 [-UseDefaultCredentials] [-CertificateThumbprint <String>] [-Certificate <X509Certificate>]
 [-UserAgent <String>] [-DisableKeepAlive] [-TimeoutSec <Int32>] [-Headers <IDictionary>]
 [-MaximumRedirection <Int32>] [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-ProxyUseDefaultCredentials]
 [-Body <Object>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>] [-OutFile <String>]
 [-PassThru] [<CommonParameters>]
```

## 説明

コマンドレットは、高度 `Invoke-RestMethod` に構造化されたデータを返す HTTP 要求と HTTPS 要求を、表現された状態転送 (REST) web サービスに送信します。

PowerShell は、データ型に基づいて応答を書式設定します。 RSS フィードまたは ATOM フィードの場合、PowerShell は Item または Entry XML ノードを返します。 JavaScript Object Notation (JSON) または XML の場合、PowerShell はコンテンツをオブジェクトに変換または逆シリアル化し `[PSCustomObject]` ます。

> [!NOTE]
> REST エンドポイントが複数のオブジェクトを返すと、オブジェクトは配列として受信されます。 からの出力を別の `Invoke-RestMethod` コマンドにパイプすると、1つのオブジェクトとして送信され `[Object[]]` ます。 この配列の内容は、パイプラインの次のコマンドに対して列挙されません。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

> [!NOTE]
> 既定では、プロパティを設定するためにページを解析するときに、web ページ内のスクリプトコードが実行される場合があり `ParsedHtml` ます。 これを抑制するには、 **Usebasicparsing** スイッチを使用します。

## 例

### 例 1: PowerShell RSS フィードを取得する

```powershell
Invoke-RestMethod -Uri https://devblogs.microsoft.com/powershell/feed/ |
  Format-Table -Property Title, pubDate
```

```Output
Title                                                                pubDate
-----                                                                -------
Join the PowerShell 10th Anniversary Celebration!                    Tue, 08 Nov 2016 23:00:04 +0000
DSC Resource Kit November 2016 Release                               Thu, 03 Nov 2016 00:19:07 +0000
PSScriptAnalyzer Community Call - Oct 18, 2016                       Thu, 13 Oct 2016 17:52:35 +0000
New Home for In-Box DSC Resources                                    Sat, 08 Oct 2016 07:13:10 +0000
New Social Features on Gallery                                       Fri, 30 Sep 2016 23:04:34 +0000
PowerShellGet and PackageManagement in PowerShell Gallery and GitHub Thu, 29 Sep 2016 22:21:42 +0000
PowerShell Security at DerbyCon                                      Wed, 28 Sep 2016 01:13:19 +0000
DSC Resource Kit September Release                                   Thu, 22 Sep 2016 00:25:37 +0000
PowerShell DSC and implicit remoting broken in KB3176934             Tue, 23 Aug 2016 15:07:50 +0000
PowerShell on Linux and Open Source!                                 Thu, 18 Aug 2016 15:32:02 +0000
```

このコマンドは、 `Invoke-RestMethod` コマンドレットを使用して、PowerShell ブログの RSS フィードから情報を取得します。 このコマンドは、コマンドレットを使用して、 `Format-Table` テーブル内の各ブログの **Title** プロパティと **pubDate** プロパティの値を表示します。

### 例 2

次の例では、ユーザーがを実行して、 `Invoke-RestMethod` ユーザーの組織のイントラネット web サイトで POST 要求を実行します。

```powershell
$Cred = Get-Credential

# Next, allow the use of self-signed SSL certificates.

[System.Net.ServicePointManager]::ServerCertificateValidationCallback = { $True }

# Create variables to store the values consumed by the Invoke-RestMethod command.
# The search variable contents are later embedded in the body variable.

$Server = 'server.contoso.com'
$Url = "https://${server}:8089/services/search/jobs/export"
$Search = "search index=_internal | reverse | table index,host,source,sourcetype,_raw"

# The cmdlet handles URL encoding. The body variable describes the search criteria, specifies CSV as
# the output mode, and specifies a time period for returned data that starts two days ago and ends
# one day ago. The body variable specifies values for parameters that apply to the particular REST
# API with which Invoke-RestMethod is communicating.

$Body = @{
    search = $Search
    output_mode = "csv"
    earliest_time = "-2d@d"
    latest_time = "-1d@d"
}

# Now, run the Invoke-RestMethod command with all variables in place, specifying a path and file
# name for the resulting CSV output file.

Invoke-RestMethod -Method Post -Uri $url -Credential $Cred -Body $body -OutFile output.csv

{"preview":true,"offset":0,"result":{"sourcetype":"contoso1","count":"9624"}}

{"preview":true,"offset":1,"result":{"sourcetype":"contoso2","count":"152"}}

{"preview":true,"offset":2,"result":{"sourcetype":"contoso3","count":"88494"}}

{"preview":true,"offset":3,"result":{"sourcetype":"contoso4","count":"15277"}}
```

### 例 3: 複数のヘッダーを渡す

この例では、の複数のヘッダーを REST API に渡す方法について説明し `hash-table` ます。

```powershell
$headers = @{
    'userId' = 'UserIDValue'
    'token' = 'TokenValue'
}
Invoke-RestMethod -Uri $uri -Method Post -Headers $headers -Body $body
```

多くの場合、Api は認証や検証などのためにヘッダーを渡す必要があります。

### 例 3: フォームデータを送信する

本文がフォームである場合、または別の呼び出しの出力である場合 `Invoke-WebRequest` 、PowerShell は要求の内容をフォームフィールドに設定します。

次に例を示します。

```powershell
$R = Invoke-WebRequest https://website.com/login.aspx
$R.Forms[0].Name = "MyName"
$R.Forms[0].Password = "MyPassword"
Invoke-RestMethod https://website.com/service.aspx -Body $R.Forms[0]
```

### 例 4: 返された項目をパイプラインで列挙する

GitHub は、複数のオブジェクトを配列として返します。 出力を別のコマンドにパイプすると、1つのオブジェクトとして送信され `[Object[]]` ます。

オブジェクトをパイプラインに列挙するには、をパイプ処理して、 `Write-Output` コマンドレットをかっこで囲みます。 次の例では、GitHub によって返されるオブジェクトの数をカウントします。 は、パイプラインに列挙されたオブジェクトの数をカウントします。

```powershell
$uri = 'https://api.github.com/repos/microsoftdocs/powershell-docs/issues'
$x = 0
Invoke-RestMethod -Uri $uri | ForEach-Object { $x++ }
$x
1

$x = 0
(Invoke-RestMethod -Uri $uri) | ForEach-Object { $x++ }
$x
30

$x = 0
Invoke-RestMethod -Uri $uri | Write-Output | ForEach-Object { $x++ }
$x
30
```

## パラメーター

### -本文

要求の本文を指定します。 本文は、ヘッダーに続く要求の内容です。
パイプを使用して、本文の値をにパイプすることもでき `Invoke-RestMethod` ます。

**Body** パラメーターを使用すると、クエリ パラメーターのリストを指定したり、応答の内容を指定したりできます。

入力が GET 要求で、本文が IDictionary (通常は、ハッシュ テーブル) の場合、本文はクエリ パラメーターとして URI に追加されます。 その他の要求の種類 (POST など) の場合、本文は標準的な "名前=値" の形式で要求本文の値として設定されます。

> [!WARNING]
> `with -1-byte payload`本文のサイズが既知で HTTP ヘッダーで送信されている場合でも、POST 本文の詳細出力はで終了し `Content-Length` ます。

```yaml
Type: System.Object
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -証明書

セキュリティで保護された Web 要求に使用するクライアント証明書を指定します。 証明書が格納されている変数を入力するか、証明書を取得するコマンドまたは式を入力します。

証明書を検索するには、を使用する `Get-PfxCertificate` か、 `Get-ChildItem` 証明書 () ドライブのコマンドレットを使用し `Cert:` ます。 証明書が無効な場合、または証明書に十分な権限がない場合、コマンドは失敗します。

```yaml
Type: System.Security.Cryptography.X509Certificates.X509Certificate
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CertificateThumbprint

要求を送信するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書の拇印を取得するに `Get-Item` は、 `Get-ChildItem` Windows PowerShell () ドライブのまたはコマンドを使用し `Cert:` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ContentType

Web 要求のコンテンツ タイプを指定します。

このパラメーターを省略して、要求メソッドが POST の場合は、 `Invoke-RestMethod` コンテンツの種類を "application/url エンコード" に設定します。 それ以外の場合、呼び出しの中でコンテンツ タイプは指定されません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

要求を送信するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableKeepAlive

HTTP ヘッダーの **KeepAlive** 値を False に設定します。 既定では、**KeepAlive** は True です。
**KeepAlive** は、後続の要求を容易にするために、サーバーへの永続的な接続を確立します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: KeepAlive
Accept pipeline input: False
Accept wildcard characters: False
```

### -ヘッダー

Web 要求のヘッダーを指定します。 ハッシュ テーブルまたは辞書を入力します。

UserAgent ヘッダーを設定するには、**UserAgent** パラメーターを使用します。 このパラメーターを使用して UserAgent または Cookie のヘッダーを指定することはできません。

```yaml
Type: System.Collections.IDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InFile

ファイルから Web 要求の内容を取得します。

パスとファイル名を入力します。 パスを省略した場合、既定値は現在のディレクトリです。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumRedirection

接続を失敗させる前に Windows PowerShell が代替 Uniform Resource Identifier (URI) に接続をリダイレクトする回数を指定します。 既定値は 5 です。 値 0 (ゼロ) を指定した場合、リダイレクトはまったく行われません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 5
Accept pipeline input: False
Accept wildcard characters: False
```

### -メソッド

Web 要求に使用するメソッドを指定します。 このパラメーターの有効値は、次のとおりです。

- `Default`
- `Delete`
- `Get`
- `Head`
- `Merge`
- `Options`
- `Patch`
- `Post`
- `Put`
- `Trace`

```yaml
Type: Microsoft.PowerShell.Commands.WebRequestMethod
Parameter Sets: (All)
Aliases:
Accepted values: Default, Get, Head, Post, Put, Delete, Trace, Options, Merge, Patch

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
Accept wildcard characters: False
```

### -出力

指定した出力ファイルに応答本文を保存します。 パスとファイル名を入力します。 パスを省略した場合、既定値は現在のディレクトリです。

既定では、は `Invoke-RestMethod` パイプラインに結果を返します。 結果をファイルとパイプラインに送信するには、**Passthru** パラメーターを使用します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

結果を返す一方でファイルにも書き込みます。 このパラメーターは、同時に **OutFile** パラメーターがコマンドで使用されている場合にのみ有効です。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: No output
Accept pipeline input: False
Accept wildcard characters: False
```

### -プロキシ

インターネット リソースに直接接続するのではなく、要求のプロキシ サーバーを使用します。 ネットワーク プロキシ サーバーの URI を入力します。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyCredential

**Proxy** パラメーターに指定したプロキシ サーバーを使用するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

「User01」や「Domain01\User01」のようなユーザー名を入力するか、 コマンドレットで生成されるような `Get-Credential` オブジェクトを入力します。

このパラメーターは、 **プロキシ** パラメーターがコマンドでも使用されている場合にのみ有効です。 **ProxyCredential** パラメーターと **ProxyUseDefaultCredentials** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyUseDefaultCredentials

現在のユーザーの資格情報を使用して、**Proxy** パラメーターに指定したプロキシ サーバーにアクセスします。

このパラメーターは、 **プロキシ** パラメーターがコマンドでも使用されている場合にのみ有効です。 **ProxyCredential** パラメーターと **ProxyUseDefaultCredentials** パラメーターを同じコマンドで使用することはできません。

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

### -SessionVariable

Web 要求セッションを作成して、指定した変数の値に保存します。 ドル記号 () を付けずに変数名を入力し `$` ます。

セッション変数を指定すると、は `Invoke-RestMethod` web 要求セッションオブジェクトを作成し、PowerShell セッション内の指定された名前の変数に割り当てます。 コマンドが完了すると、すぐに変数をセッションで使用できます。

リモート セッションとは異なり、Web 要求セッションは永続的な接続ではありません。 Web 要求セッションは、Cookie、資格情報、リダイレクトの最大値、ユーザー エージェント文字列などの、接続と要求に関する情報を含むオブジェクトです。 Web 要求セッションを使用して、Web 要求の間で状態とデータを共有することができます。

後続の Web 要求で Web 要求セッションを使用するには、**WebSession** パラメーターの値にセッション変数を指定します。 Windows PowerShell は、新しい接続を確立するときに、Web 要求セッション オブジェクトのデータを使用します。 Web 要求セッションの値をオーバーライドするには、**UserAgent**、**Credential** などのコマンドレット パラメーターを使用します。 パラメーターの値は、Web 要求セッションの値よりも優先されます。

**Sessionvariable** と **websession** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: SV

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TimeoutSec

要求がタイムアウトになるまでの待機時間を指定します。値を秒単位で入力します。 既定値は 0 で、無制限のタイムアウトを意味しています。

ドメインネームシステム (DNS) クエリは、返されるまで最大15秒かかることがあります。要求に解決を必要とするホスト名が含まれていて、TimeoutSec を0より大きく15秒未満に設定した場合、WebException がスローされる前に15秒以上かかることがあり、要求はタイムアウトになります。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 0
Accept pipeline input: False
Accept wildcard characters: False
```

### -TransferEncoding

転送エンコード HTTP 応答ヘッダーの値を指定します。 このパラメーターの有効値は、次のとおりです。

- `Chunked`
- `Compress`
- `Deflate`
- `GZip`
- `Identity`

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: chunked, compress, deflate, gzip, identity

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Uri

Web 要求の送信先の Uniform Resource Identifier (URI) を指定します。 このパラメーターは、HTTP、HTTPS、FTP、FILE の値をサポートします。

このパラメーターは必須です。 パラメーター名 (**Uri**) は省略可能です。

```yaml
Type: System.Uri
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseBasicParsing

コマンドレットが基本的な解析を使用することを示します。 コマンドレットは、 **文字列** オブジェクト内の生の HTML を返します。

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

### -UseDefaultCredentials

現在のユーザーの資格情報を使用して Web 要求を送信します。

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

### -UserAgent

Web 要求のユーザー エージェント文字列を指定します。

既定のユーザー エージェントは、"Mozilla/5.0 (Windows NT; Windows NT 6.1; en-US) WindowsPowerShell/3.0" のようになりますが、オペレーティング システムとプラットフォームによって多少異なります。

ほとんどのインターネットブラウザーで使用される標準のユーザーエージェント文字列を使用して web サイトをテストするには、Chrome、FireFox、Internet Explorer、Opera、Safari などの [PSUserAgent](/dotnet/api/microsoft.powershell.commands) クラスのプロパティを使用します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WebSession

Web 要求セッションを指定します。 ドル記号 () を含む変数名を入力し `$` ます。

Web 要求セッションの値をオーバーライドするには、**UserAgent**、**Credential** などのコマンドレット パラメーターを使用します。 パラメーターの値は、Web 要求セッションの値よりも優先されます。

リモート セッションとは異なり、Web 要求セッションは永続的な接続ではありません。 Web 要求セッションは、Cookie、資格情報、リダイレクトの最大値、ユーザー エージェント文字列などの、接続と要求に関する情報を含むオブジェクトです。 Web 要求セッションを使用して、Web 要求の間で状態とデータを共有することができます。

Web 要求セッションを作成するには、コマンドの **Sessionvariable** パラメーターの値に、(ドル記号を付けずに) 変数名を入力し `Invoke-RestMethod` ます。 `Invoke-RestMethod` セッションを作成し、変数に保存します。 後続のコマンドで、この変数を **WebSession** パラメーターの値として使用します。

**Sessionvariable** と **websession** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: Microsoft.PowerShell.Commands.WebRequestSession
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Object

パイプを使用して、web 要求の本文をにパイプすることができ `Invoke-RestMethod` ます。

## 出力

### System.Xml.Xmlドキュメント、Microsoft.PowerShell.Commands.HtmlWebResponseObject、System.string

このコマンドレットの出力は、取得するコンテンツの書式によって異なります。

### PSObject

要求が JSON 文字列を返す場合、は `Invoke-RestMethod` 文字列を表す PSObject を返します。

## Notes

## 関連リンク

[ConvertTo-Json](ConvertTo-Json.md)

[ConvertFrom-Json](ConvertFrom-Json.md)

[Invoke-WebRequest](Invoke-WebRequest.md)
