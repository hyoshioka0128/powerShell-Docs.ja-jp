---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-restmethod?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-RestMethod
ms.openlocfilehash: 91cd2dda912d6e79177e8a961012a1604d9460ee
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599290"
---
# Invoke-RestMethod

## 概要
RESTful Web サービスに HTTP または HTTPS 要求を送信します。

## SYNTAX

### StandardMethod (既定値)

```
Invoke-RestMethod [-Method <WebRequestMethod>] [-FollowRelLink] [-MaximumFollowRelLink <Int32>]
 [-ResponseHeadersVariable <String>] [-StatusCodeVariable <String>] [-UseBasicParsing] [-Uri] <Uri>
 [-WebSession <WebRequestSession>] [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-ProxyUseDefaultCredentials] [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck] [-PreserveAuthorizationOnRedirect]
 [-SkipHeaderValidation] [<CommonParameters>]
```

### StandardMethodNoProxy

```
Invoke-RestMethod [-Method <WebRequestMethod>] [-FollowRelLink] [-MaximumFollowRelLink <Int32>]
 [-ResponseHeadersVariable <String>] [-StatusCodeVariable <String>] [-UseBasicParsing] [-Uri] <Uri>
 [-WebSession <WebRequestSession>] [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] -NoProxy [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck] [-PreserveAuthorizationOnRedirect]
 [-SkipHeaderValidation] [<CommonParameters>]
```

### CustomMethodNoProxy

```
Invoke-RestMethod -CustomMethod <String> [-FollowRelLink] [-MaximumFollowRelLink <Int32>]
 [-ResponseHeadersVariable <String>] [-StatusCodeVariable <String>] [-UseBasicParsing] [-Uri] <Uri>
 [-WebSession <WebRequestSession>] [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] -NoProxy [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck] [-PreserveAuthorizationOnRedirect]
 [-SkipHeaderValidation] [<CommonParameters>]
```

### CustomMethod

```
Invoke-RestMethod -CustomMethod <String> [-FollowRelLink] [-MaximumFollowRelLink <Int32>]
 [-ResponseHeadersVariable <String>] [-StatusCodeVariable <String>] [-UseBasicParsing] [-Uri] <Uri>
 [-WebSession <WebRequestSession>] [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-ProxyUseDefaultCredentials] [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck] [-PreserveAuthorizationOnRedirect]
 [-SkipHeaderValidation] [<CommonParameters>]
```

## 説明

コマンドレットは、高度 `Invoke-RestMethod` に構造化されたデータを返す HTTP 要求と HTTPS 要求を、表現された状態転送 (REST) web サービスに送信します。

PowerShell は、データ型に基づいて応答を書式設定します。 RSS フィードまたは ATOM フィードの場合、PowerShell は Item または Entry XML ノードを返します。 JavaScript Object Notation (JSON) または XML の場合、PowerShell はコンテンツをオブジェクトに変換または逆シリアル化します。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

PowerShell 7.0 以降では、は `Invoke-RestMethod` 環境変数によって定義されたプロキシ構成をサポートしています。 この記事の「 [メモ](#notes) 」セクションを参照してください。

## 例

### 例 1: PowerShell RSS フィードを取得する

この例では、 `Invoke-RestMethod` コマンドレットを使用して、PowerShell ブログの RSS フィードから情報を取得します。 このコマンドは、コマンドレットを使用して、 `Format-Table` テーブル内の各ブログの **Title** プロパティと **pubDate** プロパティの値を表示します。

```powershell
Invoke-RestMethod -Uri https://blogs.msdn.microsoft.com/powershell/feed/ |
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

### 例 2: POST 要求を実行する

この例では、ユーザーがを実行して、 `Invoke-RestMethod` ユーザーの組織のイントラネット web サイトで POST 要求を実行します。

```powershell
$Cred = Get-Credential
$Url = "https://server.contoso.com:8089/services/search/jobs/export"
$Body = @{
    search = "search index=_internal | reverse | table index,host,source,sourcetype,_raw"
    output_mode = "csv"
    earliest_time = "-2d@d"
    latest_time = "-1d@d"
}
Invoke-RestMethod -Method 'Post' -Uri $url -Credential $Cred -Body $body -OutFile output.csv
```

資格情報の入力が求められ、に格納され `$Cred` ます。また、には、アクセスする URL が定義されてい `$Url` ます。

変数は、 `$Body` 検索条件を記述し、CSV を出力モードとして指定します。また、返されるデータの期間を指定します。この時間を経過すると、2日前の日前に終了します。 Body 変数は、が通信する特定の REST API に適用されるパラメーターの値を指定し `Invoke-RestMethod` ます。

`Invoke-RestMethod`コマンドは、すべての変数を使用して実行され、結果として得られる CSV 出力ファイルのパスとファイル名を指定します。

### 例 3: リレーションリンクに従う

一部の REST Api では、 [RFC5988](https://tools.ietf.org/html/rfc5988#page-6)ごとの関係リンクを使用した改ページがサポートしています。 ヘッダーを解析して次のページの URL を取得する代わりに、コマンドレットでこの操作を行うことができます。 この例では、PowerShell GitHub リポジトリから、問題の最初の2ページが返されます。

```powershell
$url = 'https://api.github.com/repos/powershell/powershell/issues'
Invoke-RestMethod $url -FollowRelLink -MaximumFollowRelLink 2
```

### 例 4: 単純化されたマルチパート/フォーム-データ送信

一部の Api `multipart/form-data` では、ファイルのアップロードと混合コンテンツの送信が必要です。 この例では、ユーザーのプロファイルを更新する方法を示します。

```powershell
$Uri = 'https://api.contoso.com/v2/profile'
$Form = @{
    firstName  = 'John'
    lastName   = 'Doe'
    email      = 'john.doe@contoso.com'
    avatar     = Get-Item -Path 'c:\Pictures\jdoe.png'
    birthday   = '1980-10-15'
    hobbies    = 'Hiking','Fishing','Jogging'
}
$Result = Invoke-RestMethod -Uri $Uri -Method Post -Form $Form
```

プロファイルフォームには、、、 `firstName` 、、、 `lastName` `email` `avatar` `birthday` および `hobbies` の各フィールドが必要です。 API では、ユーザープロファイル pic をフィールドに指定するイメージが必要です `avatar` 。 API は、 `hobbies` 同じ形式で複数のエントリを送信することも許可します。

`$Form`ハッシュテーブルを作成する場合、キー名はフォームフィールド名として使用されます。 既定では、ハッシュテーブルの値は文字列に変換されます。 値が存在する場合は、 `System.IO.FileInfo` ファイルの内容が送信されます。 配列やリストなどのコレクションが存在する場合、フォームフィールドは複数回送信されます。

キーでを使用する `Get-Item` `avatar` と、 `FileInfo` オブジェクトが値として設定されます。 結果として、のイメージデータが `jdoe.png` 送信されます。

キーにリストを指定すると、 `hobbies` フィールドは、 `hobbies` リスト項目ごとに1回送信されます。

### 例 5: 複数のヘッダーを渡す

多くの場合、Api は認証または検証のために渡されたヘッダーを必要とします。 この例では、から複数のヘッダーを REST API に渡す方法について説明し `hash-table` ます。

```powershell
$headers = @{
    'userId' = 'UserIDValue'
    'token' = 'TokenValue'
}
Invoke-RestMethod -Uri $uri -Method Post -Headers $headers -Body $body
```

## パラメーター

### -AllowUnencryptedAuthentication

暗号化されていない接続を介した資格情報とシークレットの送信を許可します。 既定では、で始まらない **Uri** を指定した **資格情報** または任意の **認証** オプションを指定すると、エラーが発生します。この要求は、暗号化されていない接続を `https://` 介してプレーンテキストで誤ってシークレットを通信するのを防ぐために中止されます。 独自のリスクでこの動作をオーバーライドするには、 **Allowunencryptedauthentication** パラメーターを指定します。

> [!WARNING]
> このパラメーターの使用はセキュリティで保護されていないため、推奨されません。 これは、暗号化された接続を提供できないレガシシステムとの互換性のためだけに用意されています。 ご自身の責任でをご利用ください。

この機能は、PowerShell 6.0.0 で追加されました。

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

### -認証

要求に使用する明示的な認証の種類を指定します。 既定値は **[None]\(なし\)** です。
**UseDefaultCredentials** で **認証** を使用することはできません。

使用可能な認証オプション:

- **None**: **認証** が指定されていない場合の既定のオプションです。 明示的な認証は使用されません。
- **基本**: **資格情報** が必要です。 資格情報は、RFC 7617 の基本認証ヘッダーを形式で送信するために使用され `Authorization: Basic` `base64(user:password)` ます。
- **ベアラー**: **トークン** が必要です。 は、指定されたトークンを使用して、および RFC 6750 ヘッダーを送信し `Authorization: Bearer` ます。 これは **OAuth** のエイリアスです
- **OAuth**: **トークン** が必要です。 は、指定された `Authorization: Bearer` トークンを使用して RFC 6750 ヘッダーを送信します。 これは **ベアラー** のエイリアスです

**認証** を指定すると、 `Authorization` **ヘッダー** に指定されたヘッダーまたは **web セッション** に含まれるヘッダーが上書きされます。

この機能は、PowerShell 6.0.0 で追加されました。

```yaml
Type: Microsoft.PowerShell.Commands.WebAuthenticationType
Parameter Sets: (All)
Aliases:
Accepted values: None, Basic, Bearer, OAuth

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -本文

要求の本文を指定します。 本文は、ヘッダーに続く要求の内容です。
パイプを使用して、本文の値をにパイプすることもでき `Invoke-RestMethod` ます。

**Body** パラメーターを使用すると、クエリ パラメーターのリストを指定したり、応答の内容を指定したりできます。

入力が GET 要求で、本文が `IDictionary` (通常はハッシュテーブル) の場合、本文はクエリパラメーターとして Uniform Resource Identifier (URI) に追加されます。 その他の要求の種類 (POST など) の場合、本文は標準的な "名前=値" の形式で要求本文の値として設定されます。

本文がフォームである場合、または別の呼び出しの出力である場合 `Invoke-WebRequest` 、PowerShell は要求の内容をフォームフィールドに設定します。

**Body** パラメーターは、 **MultipartFormDataContent** オブジェクトを受け取ることもできます。 これにより、要求が容易になり `multipart/form-data` ます。 **MultipartFormDataContent** オブジェクトが **本文** に指定されている場合、 **ContentType**、 **headers**、または **websession** パラメーターに指定されたコンテンツに関連するヘッダーは、オブジェクトのコンテンツヘッダーによってオーバーライドされ `MultipartFormDataContent` ます。 この機能は、PowerShell 6.0.0 で追加されました。

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

証明書を検索するには、を使用する `Get-PfxCertificate` か、 `Get-ChildItem` 証明書 () ドライブのコマンドレットを使用し `Cert:` ます。 証明書が有効でないか、十分な権限がない場合、コマンドは失敗します。

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

証明書の拇印を取得するに `Get-Item` は、PowerShell ドライブでまたはコマンドを使用し `Get-ChildItem` `Cert:` ます。

> [!NOTE]
> この機能は現在、Windows OS プラットフォームでのみサポートされています。

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

このパラメーターを省略して、要求メソッドが POST の場合は、 `Invoke-RestMethod` コンテンツの種類をに設定し `application/x-www-form-urlencoded` ます。 それ以外の場合は、の呼び出しでコンテンツの種類が指定されていません。

 `MultipartFormDataContent` オブジェクトが **本文** に対して指定されると、ContentType はオーバーライドされます。

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

**資格情報** は、単独で使用することも、特定の **認証** パラメーターオプションと組み合わせて使用することもできます。 リモートサーバーが認証チャレンジ要求を送信した場合にのみ、リモートサーバーに資格情報を提供します。 **認証** オプションと共に使用すると、資格情報が明示的に送信されます。

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

### -CustomMethod

Web 要求に使用するカスタムメソッドを指定します。 これは、エンドポイントで必要とされる要求メソッドで使用できますが、 **メソッド** では使用できません。 **メソッド** と **custommethod** を一緒に使用することはできません。

例:

`Invoke-RestMethod -uri 'https://api.contoso.com/widget/' -CustomMethod 'TEST'`

これにより `TEST` 、API に HTTP 要求が作成されます。

この機能は、PowerShell 6.0.0 で追加されました。

```yaml
Type: System.String
Parameter Sets: CustomMethodNoProxy, CustomMethod
Aliases: CM

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableKeepAlive

コマンドレットが HTTP ヘッダーの **KeepAlive** 値を False に設定することを示します。 既定では、**KeepAlive** は True です。 **KeepAlive** は、後続の要求を容易にするために、サーバーへの永続的な接続を確立します。

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

### -リンク

コマンドレットが関係リンクに従う必要があることを示します。

一部の REST Api では、 [RFC5988](https://tools.ietf.org/html/rfc5988#page-6)ごとの関係リンクを使用した改ページがサポートしています。 ヘッダーを解析して次のページの URL を取得する代わりに、コマンドレットでこの操作を行うことができます。 リレーションリンクに従う回数を設定するには、 **Maximumの Rellink** パラメーターを使用します。

このスイッチを使用すると、コマンドレットは結果のページのコレクションを返します。 結果の各ページには、複数の結果項目が含まれる場合があります。

この機能は、PowerShell 6.0.0 で追加されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: FL

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -フォーム

辞書を送信に変換 `multipart/form-data` します。 **フォーム** を **本文** と共に使用することはできません。
**ContentType** が無視される場合は。

ディクショナリのキーは、フォームフィールド名として使用されます。 既定では、フォーム値は文字列値に変換されます。

値が **system.servicemodel オブジェクトの** 場合は、バイナリファイルの内容が送信されます。
ファイルの名前は、として送信され `filename` ます。 MIME はとして設定され `application/octet-stream` ます。 `Get-Item` を使用 **すると、system.object オブジェクト** を簡単に指定できます。

```powershell
$Form = @{
    resume = Get-Item 'c:\Users\jdoe\Documents\John Doe.pdf'
}
```

値が配列やリストなどのコレクション型である場合、for フィールドは複数回送信されます。 既定では、リストの値は文字列として扱われます。 値が **system.servicemodel オブジェクトの** 場合は、バイナリファイルの内容が送信されます。 入れ子になったコレクションはサポートされていません。

```powershell
$Form = @{
    tags     = 'Vacation', 'Italy', '2017'
    pictures = Get-ChildItem 'c:\Users\jdoe\Pictures\2017-Italy\'
}
```

上の例では、 `tags` フィールドは、、、およびの各につき1回、フォームで3回指定され `Vacation` `Italy` `2017` ます。 この `pictures` フィールドは、フォルダー内のファイルごとに1回送信され `2017-Italy` ます。 そのフォルダー内のファイルのバイナリコンテンツは、値として送信されます。

この機能は、PowerShell 6.1.0 で追加されました。

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

### -ヘッダー

Web 要求のヘッダーを指定します。 ハッシュ テーブルまたは辞書を入力します。

UserAgent ヘッダーを設定するには、**UserAgent** パラメーターを使用します。 このパラメーターを使用して `User-Agent` 、またはクッキーヘッダーを指定することはできません。

などのコンテンツ関連ヘッダーは、 `Content-Type` `MultipartFormDataContent` オブジェクトが **本文** に指定されたときにオーバーライドされます。

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

### -MaximumFollowRelLink

**リンク** が使用されている場合に、関係リンクに従う回数を指定します。 要求が多すぎることが原因で REST api が調整された場合は、小さい値が必要になることがあります。 既定値は `[Int32]::MaxValue` です。 値を 0 (ゼロ) にすると、次の関係リンクが禁止されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: ML

Required: False
Position: Named
Default value: Int32.MaxValue
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumRedirection

接続に失敗する前に PowerShell が代替 Uniform Resource Identifier (URI) に接続をリダイレクトする回数を指定します。 既定値は 5 です。 値 0 (ゼロ) を指定した場合、リダイレクトはまったく行われません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumRetryCount

400と599の間のエラーコード (包括的または 304) を受信したときに PowerShell が接続を再試行する回数を指定します。 再試行回数を指定する場合は、 **Retryintervalsec** パラメーターも参照してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -メソッド

Web 要求に使用するメソッドを指定します。 このパラメーターの有効値は、次のとおりです。

- Default
- 削除
- 取得
- Head
- マージする
- オプション
- 修正プログラム
- 投稿
- Put
- Trace

**Custommethod** パラメーターは、上に一覧表示されていない要求メソッドに使用できます。

```yaml
Type: Microsoft.PowerShell.Commands.WebRequestMethod
Parameter Sets: StandardMethod, StandardMethodNoProxy
Aliases:
Accepted values: Default, Get, Head, Post, Put, Delete, Trace, Options, Merge, Patch

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoProxy

コマンドレットが宛先に接続するためにプロキシを使用しないことを示します。

Internet Explorer で構成されているプロキシ、または環境に指定されているプロキシをバイパスする必要がある場合は、このスイッチを使用します。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: StandardMethodNoProxy, CustomMethodNoProxy
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -出力

指定した出力ファイルに応答本文を保存します。 パスとファイル名を入力します。 パスを省略した場合、既定値は現在のディレクトリです。 名前はリテラルパスとして扱われます。 角かっこ () を含む名前 `[]` は、単一引用符 () で囲む必要があり `'` ます。

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

### -PreserveAuthorizationOnRedirect

コマンドレットで、リダイレクト全体でヘッダーを保持する必要があることを示し `Authorization` ます。

既定では、コマンドレットは、 `Authorization` リダイレクトの前にヘッダーを除去します。 このパラメーターを指定すると、ヘッダーがリダイレクト場所に送信される必要がある場合に、このロジックが無効になります。

この機能は、PowerShell 6.0.0 で追加されました。

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

### -プロキシ

は、インターネットリソースに直接接続するのではなく、要求にプロキシサーバーを使用します。 ネットワークプロキシサーバーの Uniform Resource Identifier (URI) を入力します。

この機能は、PowerShell 6.0.0 で追加されました。

```yaml
Type: System.Uri
Parameter Sets: StandardMethod, CustomMethod
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyCredential

**Proxy** パラメーターに指定したプロキシ サーバーを使用するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力する **User@Domain.Com** か、 `PSCredential` コマンドレットによって生成されたオブジェクトなどのオブジェクトを入力し `Get-Credential` ます。

このパラメーターは、 **プロキシ** パラメーターがコマンドでも使用されている場合にのみ有効です。 **Proxycredential** パラメーターと **ProxyUseDefaultCredentials** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: StandardMethod, CustomMethod
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyUseDefaultCredentials

コマンドレットが、 **プロキシ** パラメーターで指定されたプロキシサーバーにアクセスするために、現在のユーザーの資格情報を使用することを示します。

このパラメーターは、 **プロキシ** パラメーターがコマンドでも使用されている場合にのみ有効です。 **Proxycredential** パラメーターと **ProxyUseDefaultCredentials** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: StandardMethod, CustomMethod
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ResponseHeadersVariable

応答ヘッダーディクショナリを作成し、指定した変数の値に保存します。 ディクショナリのキーには、web サーバーから返された応答ヘッダーのフィールド名が含まれ、値はそれぞれのフィールド値になります。

この機能は、PowerShell 6.0.0 で追加されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: RHV

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -再開

部分ファイルのダウンロードを再開するのに最適な試行を実行します。 **Resume** パラメーター **には、出力パラメーターが** 必要です。

**Resume** はローカルファイルとリモートファイルのサイズでのみ動作し、ローカルファイルとリモートファイルが同じであることを検証しません。

ローカルファイルのサイズがリモートファイルのサイズよりも小さい場合、コマンドレットはファイルのダウンロードを再開し、残りのバイトをファイルの末尾に追加しようとします。

ローカルファイルのサイズがリモートファイルのサイズと同じ場合、アクションは実行されず、コマンドレットはダウンロードが既に完了しているものと想定します。

ローカルファイルのサイズがリモートファイルのサイズよりも大きい場合、ローカルファイルは上書きされ、リモートファイル全体が完全に再ダウンロードされます。 この動作は、 **Resume****を使用しない場合** と同じです。

リモートサーバーがダウンロードの再開をサポートしていない場合、ローカルファイルは上書きされ、リモートファイル全体が完全に再ダウンロードされます。 この動作は、 **Resume****を使用しない場合** と同じです。

ローカルファイルが存在しない場合は、ローカルファイルが作成され、リモートファイル全体が完全にダウンロードされます。 この動作は、 **Resume****を使用しない場合** と同じです。

この機能は、PowerShell 6.1.0 で追加されました。

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

### -RetryIntervalSec

400と 304 599 の間のエラーコードを受信したときの、接続の再試行の間隔を指定します。 再試行回数を指定するには、 **Maximumretrycount** パラメーターも参照してください。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SessionVariable

このコマンドレットによって web 要求セッションが作成され、値に保存される変数を指定します。
ドル記号 () を付けずに変数名を入力し `$` ます。

セッション変数を指定すると、は `Invoke-RestMethod` web 要求セッションオブジェクトを作成し、PowerShell セッション内の指定された名前の変数に割り当てます。 コマンドが完了すると、すぐに変数をセッションで使用できます。

リモートセッションとは異なり、web 要求セッションは永続的な接続ではありません。 これは、cookie、資格情報、リダイレクトの最大値、ユーザーエージェント文字列など、接続と要求に関する情報を含むオブジェクトです。 Web 要求セッションを使用して、Web 要求の間で状態とデータを共有することができます。

後続の Web 要求で Web 要求セッションを使用するには、**WebSession** パラメーターの値にセッション変数を指定します。 PowerShell は、新しい接続を確立するときに、web 要求セッションオブジェクトのデータを使用します。 Web 要求セッションの値をオーバーライドするには、**UserAgent**、**Credential** などのコマンドレット パラメーターを使用します。 パラメーターの値は、Web 要求セッションの値よりも優先されます。

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

### -SkipCertificateCheck

有効期限、失効、信頼されたルート証明機関などのすべての検証を含む証明書検証チェックをスキップします。

> [!WARNING]
> このパラメーターの使用はセキュリティで保護されていないため、推奨されません。 このスイッチは、テスト目的で自己署名証明書を使用する既知のホストに対してのみ使用することを目的としています。 ご自身の責任でをご利用ください。

この機能は、PowerShell 6.0.0 で追加されました。

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

### -SkipHeaderValidation

コマンドレットが検証なしで要求にヘッダーを追加する必要があることを示します。

標準に準拠していないヘッダー値が必要なサイトには、このスイッチを使用する必要があります。
このスイッチを指定すると、値をオフにするための検証が無効になります。 指定した場合、すべてのヘッダーが検証なしで追加されます。

これにより、 **ContentType**、 **Headers**、および **UserAgent** パラメーターに渡された値の検証が無効になります。

この機能は、PowerShell 6.0.0 で追加されました。

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

### -SkipHttpErrorCheck

このパラメーターを指定すると、コマンドレットは HTTP エラー状態を無視し、応答の処理を続行します。
エラー応答は、成功したかのようにパイプラインに書き込まれます。

このパラメーターは、PowerShell 7 で導入されました。

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

### -SslProtocol

Web 要求に対して許可されている SSL/TLS プロトコルを設定します。 既定では、システムでサポートされているすべての SSL/TLS プロトコルが許可されます。 **Sslprotocol** では、コンプライアンスのために特定のプロトコルを制限できます。

**Sslprotocol** では、 `WebSslProtocol` フラグ列挙型が使用されます。 フラグ表記を使用して複数のプロトコルを指定したり、複数のオプションをと組み合わせたりすることはでき `WebSslProtocol` `-bor` ますが、複数のプロトコルを指定することは、すべてのプラットフォームでサポートされているわけではありません。

> [!NOTE]
> Windows 以外のプラットフォームで `Tls` は、 `Tls12` オプションとしてまたはを指定することはできません。 のサポート `Tls13` は、すべてのオペレーティングシステムで使用できるわけではなく、オペレーティングシステムごとに検証する必要があります。

この機能は PowerShell 6.0.0 で追加され、 `Tls13` powershell 7.1 でがサポートされるようになりました。

```yaml
Type: Microsoft.PowerShell.Commands.WebSslProtocol
Parameter Sets: (All)
Aliases:
Accepted values: Default, Tls, Tls11, Tls12, Tls13

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -StatusCodeVariable

このパラメーターは、ステータスコードの整数値が割り当てられている変数を指定します。 パラメーターでは、 **SkipHttpErrorCheck** パラメーターと共に使用すると、成功メッセージまたはエラーメッセージを識別できます。

パラメーターの変数名をなどの文字列として入力し `-StatusCodeVariable "scv"` ます。

このパラメーターは、PowerShell 7 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TimeoutSec

要求がタイムアウトになるまでの待機時間を指定します。値を秒単位で入力します。 既定値は 0 で、無制限のタイムアウトを意味しています。

ドメインネームシステム (DNS) クエリは、返されるまで最大15秒かかることがあります。要求に解決を必要とするホスト名が含まれていて、 **Timeoutsec** を0より大きく15秒未満に設定した場合、WebException がスローされる前に15秒以上かかることがあり、要求はタイムアウトになります。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -トークン

要求に含める OAuth またはベアラートークン。 **トークン** は、特定の **認証** オプションに必要です。 個別に使用することはできません。

**トークンは** 、 `SecureString` トークンを含むを受け取ります。 トークンを指定するには、次のように手動で使用します。

`Invoke-RestMethod -Uri $uri -Authentication OAuth -Token (Read-Host -AsSecureString)`

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Security.SecureString
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TransferEncoding

転送エンコード HTTP 応答ヘッダーの値を指定します。 このパラメーターの有効値は、次のとおりです。

- まとめ
- 圧縮
- Deflate
- GZip
- ID

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

Web 要求の送信先となるインターネットリソースの Uniform Resource Identifier (URI) を指定します。 このパラメーターは、HTTP、HTTPS、FTP、FILE の値をサポートします。

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

このパラメーターは非推奨とされました。 PowerShell 6.0.0 以降では、すべての Web 要求で基本的な解析のみが使用されます。 このパラメーターは下位互換性のためだけに含まれています。このパラメーターを使用しても、コマンドレットの操作には影響しません。

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

コマンドレットが、web 要求を送信するために現在のユーザーの資格情報を使用することを示します。 **認証** または **資格情報** と共に使用することはできません。また、すべてのプラットフォームでサポートされていない可能性があります。

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

既定のユーザーエージェントは、 `Mozilla/5.0 (Windows NT 10.0; Microsoft Windows 10.0.15063; en-US) PowerShell/6.0.0` オペレーティングシステムとプラットフォームによって微妙な違いがあるに似ています。

ほとんどのインターネットブラウザーで使用される標準のユーザーエージェント文字列を使用して web サイトをテストするには、Chrome、FireFox、InternetExplorer、Opera、Safari などの [PSUserAgent](/dotnet/api/microsoft.powershell.commands.psuseragent) クラスのプロパティを使用します。

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

Web 要求セッションの値をオーバーライドするには、**UserAgent**、**Credential** などのコマンドレット パラメーターを使用します。 パラメーターの値は、Web 要求セッションの値よりも優先されます。 `Content-Type`**本文** に **MultipartFormDataContent** オブジェクトが指定されている場合、などのコンテンツ関連ヘッダーもオーバーライドされます。

リモートセッションとは異なり、web 要求セッションは永続的な接続ではありません。 これは、cookie、資格情報、リダイレクトの最大値、ユーザーエージェント文字列など、接続と要求に関する情報を含むオブジェクトです。 Web 要求セッションを使用して、Web 要求の間で状態とデータを共有することができます。

Web 要求セッションを作成するには、コマンドの **sessionvariable** パラメーターの値にドル記号を付けずに変数名を入力し `Invoke-RestMethod` ます。 `Invoke-RestMethod` セッションを作成し、変数に保存します。 後続のコマンドで、この変数を **WebSession** パラメーターの値として使用します。

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

### Int64、System.string、System.Xml.Xmlドキュメント

このコマンドレットの出力は、取得するコンテンツの書式によって異なります。

### PSObject

要求が JSON 文字列を返す場合、は `Invoke-RestMethod` 文字列を表す **PSObject** を返します。

## 注

一部の機能は、一部のプラットフォームでは使用できない場合があります。

.NET Core 3.1 の変更のため、PowerShell 7.0 以降では、 [Httpclient. defaultproxy](/dotnet/api/system.net.http.httpclient.defaultproxy?view=netcore-3.1) プロパティを使用してプロキシの構成を決定します。

このプロパティの値は、プラットフォームによって異なります。

- **Windows の場合**: 環境変数から、または定義されていない場合は、ユーザーのプロキシ設定からプロキシ構成を読み取ります。
- **MacOS の** 場合: 環境変数から、またはシステムのプロキシ設定からプロキシ構成が定義されていない場合は、その構成を読み取ります。
- **Linux の場合**: 環境変数からプロキシ構成を読み取ります。これらが定義されていない場合、このプロパティは、すべてのアドレスをバイパスする構成されていないインスタンスを初期化します。

Windows および Unix ベースのプラットフォームでの初期化に使用される環境変数 `DefaultProxy` は次のとおりです。

- ` HTTP_PROXY`: HTTP 要求で使用されるプロキシサーバーのホスト名または IP アドレス。
- `HTTPS_PROXY`: HTTPS 要求で使用されるプロキシサーバーのホスト名または IP アドレス。
- `ALL_PROXY`: HTTP および HTTPS 要求で使用されるプロキシサーバーのホスト名または IP アドレス `HTTP_PROXY` (またはが定義されて `HTTPS_PROXY` いない場合)。
- `NO_PROXY`: プロキシから除外する必要があるホスト名のコンマ区切りのリスト。

## 関連リンク

[ConvertTo-Json](ConvertTo-Json.md)

[ConvertFrom-Json](ConvertFrom-Json.md)

[Invoke-WebRequest](Invoke-WebRequest.md)
