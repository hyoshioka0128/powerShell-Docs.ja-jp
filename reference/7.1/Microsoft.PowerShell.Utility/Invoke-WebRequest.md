---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 01/26/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-WebRequest
ms.openlocfilehash: 036f5aef42b9413747f4e738bf748fda8bb2d2d2
ms.sourcegitcommit: 11880ca974fe2df308191c9f6dcdfe0b89c2dc67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/27/2021
ms.locfileid: "98860788"
---
# Invoke-WebRequest

## 概要
インターネット上の web ページからコンテンツを取得します。

## SYNTAX

### StandardMethod (既定値)

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] [-Method <WebRequestMethod>]
 [-Proxy <Uri>] [-ProxyCredential <PSCredential>] [-ProxyUseDefaultCredentials] [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck] [-PreserveAuthorizationOnRedirect]
 [-SkipHeaderValidation] [<CommonParameters>]
```

### StandardMethodNoProxy

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] [-Method <WebRequestMethod>] -NoProxy
 [-Body <Object>] [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>]
 [-InFile <String>] [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck]
 [-PreserveAuthorizationOnRedirect] [-SkipHeaderValidation] [<CommonParameters>]
```

### CustomMethod

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] -CustomMethod <String> [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-ProxyUseDefaultCredentials] [-Body <Object>]
 [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>] [-InFile <String>]
 [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck] [-PreserveAuthorizationOnRedirect]
 [-SkipHeaderValidation] [<CommonParameters>]
```

### CustomMethodNoProxy

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>]
 [-SessionVariable <String>] [-AllowUnencryptedAuthentication]
 [-Authentication <WebAuthenticationType>] [-Credential <PSCredential>] [-UseDefaultCredentials]
 [-CertificateThumbprint <String>] [-Certificate <X509Certificate>] [-SkipCertificateCheck]
 [-SslProtocol <WebSslProtocol>] [-Token <SecureString>] [-UserAgent <String>] [-DisableKeepAlive]
 [-TimeoutSec <Int32>] [-Headers <IDictionary>] [-MaximumRedirection <Int32>]
 [-MaximumRetryCount <Int32>] [-RetryIntervalSec <Int32>] -CustomMethod <String> -NoProxy
 [-Body <Object>] [-Form <IDictionary>] [-ContentType <String>] [-TransferEncoding <String>]
 [-InFile <String>] [-OutFile <String>] [-PassThru] [-Resume] [-SkipHttpErrorCheck]
 [-PreserveAuthorizationOnRedirect] [-SkipHeaderValidation] [<CommonParameters>]
```

## Description

`Invoke-WebRequest`コマンドレットは、HTTP 要求と HTTPS 要求を web ページまたは web サービスに送信します。 応答を解析し、リンク、画像、およびその他の重要な HTML 要素のコレクションを返します。

このコマンドレットは、PowerShell 3.0 で導入されました。

PowerShell 7.0 以降では、は `Invoke-WebRequest` 環境変数によって定義されたプロキシ構成をサポートしています。 この記事の「 [メモ](#notes) 」セクションを参照してください。

## 例

### 例 1: web 要求を送信する

この例では、コマンドレットを使用して、 `Invoke-WebRequest` Bing.com サイトに web 要求を送信します。

```powershell
$Response = Invoke-WebRequest -URI https://www.bing.com/search?q=how+many+feet+in+a+mile
$Response.InputFields | Where-Object {
    $_.name -like "* Value*"
} | Select-Object Name, Value
```

```Output
name       value
----       -----
From Value 1
To Value   5280
```

最初のコマンドは、要求を発行し、その応答を変数に保存し `$Response` ます。

2番目のコマンドは、 **Name** プロパティがのような **inputfield** を取得し `"* Value"` ます。 フィルター処理された結果は、 `Select-Object` **名前** と **値** のプロパティを選択するためにパイプ処理されます。

### 例 2: ステートフルな web サービスを使用する

この例は、 `Invoke-WebRequest` コマンドレットをステートフル web サービスと共に使用する方法を示しています。

```powershell
$Body = @{
    User = 'jdoe'
    password = 'P@S$w0rd!'
}
$LoginResponse = Invoke-WebRequest 'https://www.contoso.com/login/' -SessionVariable 'Session' -Body $Body -Method 'POST'

$Session

$ProfileResponse = Invoke-WebRequest 'https://www.contoso.com/profile/' -WebSession $Session

$ProfileResponse
```

への最初の呼び出しでは、 `Invoke-WebRequest` サインイン要求が送信されます。 このコマンドは、 **-sessionvariable** パラメーターの値に "Session" という値を指定し、結果を変数に保存し `$LoginResponse` ます。 コマンドが完了すると、 `$LoginResponse` 変数にはが含まれ、 `BasicHtmlWebResponseObject` 変数には `$Session` オブジェクトが含まれ `WebRequestSession` ます。 これにより、ユーザーがサイトに記録されます。

を呼び出す `$Session` ことによって、変数内のオブジェクトが表示され `WebRequestSession` ます。

への2回目の呼び出しでは、ユーザーが `Invoke-WebRequest` サイトにログインする必要があるユーザーのプロファイルがフェッチされます。 変数に格納されているセッションデータ `$Session` は、ログイン中に作成されたサイトにセッション cookie を提供するために使用されます。 結果は変数に保存され `$ProfileResponse` ます。

によるの呼び出しは、 `$ProfileResponse` 変数内のを表示し `BasicHtmlWebResponseObject` ます。

### 例 3: web ページからリンクを取得する

この例では、web ページ内のリンクを取得します。 コマンドレットを使用して、 `Invoke-WebRequest` web ページのコンテンツを取得します。 次に、を返すの **Links** プロパティ `BasicHtmlWebResponseObject` `Invoke-WebRequest` と、各リンクの **Href** プロパティを使用します。

```powershell
(Invoke-WebRequest -Uri "https://aka.ms/pscore6-docs").Links.Href
```

### 例 4: 要求されたページで定義されているエンコーディングを使用して、応答の内容をファイルに書き込みます。

この例では、コマンドレットを使用して、 `Invoke-WebRequest` PowerShell ドキュメントページの web ページの内容を取得します。

```powershell
$Response = Invoke-WebRequest -Uri "https://aka.ms/pscore6-docs"
$Stream = [System.IO.StreamWriter]::new('.\docspage.html', $false, $Response.Encoding)
try {
    $Stream.Write($Response.Content)
}
finally {
    $Stream.Dispose()
}
```

最初のコマンドは、ページを取得し、その応答オブジェクトを変数に保存し `$Response` ます。

2番目のコマンドは、 `StreamWriter` 応答の内容をファイルに書き込むために使用するを作成します。 応答オブジェクトの **encoding** プロパティは、ファイルのエンコーディングを設定するために使用されます。

最後のいくつかのコマンドは、 **コンテンツ** プロパティをファイルに書き込んだ後、を破棄し `StreamWriter` ます。

Web 要求がテキストコンテンツを返さない場合、 **Encoding** プロパティは null になることに注意してください。

### 例 5: マルチパート/フォームデータファイルを送信する

この例では、コマンドレットを使用し `Invoke-WebRequest` て、送信としてファイルをアップロードし `multipart/form-data` ます。 ファイル `c:\document.txt` は、のを含むフォームフィールドとして送信され `document` `Content-Type` `text/plain` ます。

```powershell
$FilePath = 'c:\document.txt'
$FieldName = 'document'
$ContentType = 'text/plain'

$FileStream = [System.IO.FileStream]::new($filePath, [System.IO.FileMode]::Open)
$FileHeader = [System.Net.Http.Headers.ContentDispositionHeaderValue]::new('form-data')
$FileHeader.Name = $FieldName
$FileHeader.FileName = Split-Path -leaf $FilePath
$FileContent = [System.Net.Http.StreamContent]::new($FileStream)
$FileContent.Headers.ContentDisposition = $FileHeader
$FileContent.Headers.ContentType = [System.Net.Http.Headers.MediaTypeHeaderValue]::Parse($ContentType)

$MultipartContent = [System.Net.Http.MultipartFormDataContent]::new()
$MultipartContent.Add($FileContent)

$Response = Invoke-WebRequest -Body $MultipartContent -Method 'POST' -Uri 'https://api.contoso.com/upload'
```

### 例 6: 単純化されたマルチパート/フォーム-データ送信

一部の Api `multipart/form-data` では、ファイルのアップロードと混合コンテンツの送信が必要です。 この例では、ユーザープロファイルの更新を示します。

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
$Result = Invoke-WebRequest -Uri $Uri -Method Post -Form $Form
```

プロファイルフォームには、、、 `firstName` 、、、 `lastName` `email` `avatar` `birthday` および `hobbies` の各フィールドが必要です。 API では、ユーザープロファイル pic をフィールドに指定するイメージが必要です `avatar` 。 この API は、 `hobbies` 同じ形式で複数のエントリを送信することもできます。

`$Form`ハッシュテーブルを作成する場合、キー名はフォームフィールド名として使用されます。 既定では、ハッシュテーブルの値は文字列に変換されます。 System.servicemodel の **値が** 存在する場合は、ファイルの内容が送信されます。 配列やリストなどのコレクションが存在する場合、フォームフィールドは複数回送信されます。

キーでを使用する `Get-Item` `avatar` と、 `FileInfo` オブジェクトが値として設定されます。 その結果、のイメージデータが送信され `jdoe.png` ます。

キーにリストを指定することによって、 `hobbies` `hobbies` フィールドはリスト項目ごとに1回送信されます。

### 例 7: Invoke-WebRequest からの失敗したメッセージをキャッチする

が `Invoke-WebRequest` 成功以外の HTTP メッセージ (404、500など) を検出すると、出力は返されず、終了エラーをスローします。 エラーをキャッチして **StatusCode** を表示するには、ブロックで実行を囲むことができ `try/catch` ます。

```powershell
try
{
    $Response = Invoke-WebRequest -Uri "www.microsoft.com/unkownhost"
    # This will only execute if the Invoke-WebRequest is successful.
    $StatusCode = $Response.StatusCode
}
catch
{
    $StatusCode = $_.Exception.Response.StatusCode.value__
}
$StatusCode
```

```Output
404
```

終了エラーは、 `catch` **例外** オブジェクトから **StatusCode** を取得するブロックによってキャッチされます。

## PARAMETERS

### -AllowUnencryptedAuthentication

暗号化されていない接続を介した資格情報とシークレットの送信を許可します。 既定では、で開始されていない **Uri** を持つ **資格情報** または任意の **認証** オプションを指定 `https://` すると、エラーが発生し、要求は中止されます。これにより、暗号化されていない接続を介してプレーンテキストでシークレットが誤って通信されるのを防ぐ 独自のリスクでこの動作をオーバーライドするには、 **Allowunencryptedauthentication** パラメーターを指定します。

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

- **None**: **認証** が指定されていない場合の既定のオプションです。明示的な認証は使用されません。
- **基本**: **資格情報** が必要です。 資格情報は、RFC 7617 の基本認証ヘッダーでという形式で送信され `base64(user:password)` ます。
- **ベアラー**: **トークン** が必要です。 指定された `Authorization: Bearer` トークンを使用して RFC 6750 ヘッダーを送信します。 これは **OAuth** のエイリアスです
- **OAuth**: **トークン** が必要です。 指定された `Authorization: Bearer` トークンを使用して RFC 6750 ヘッダーを送信します。 これは **ベアラー** のエイリアスです

**認証** `Authorization` を指定すると、**ヘッダー** に指定されたヘッダーまたは **web セッション** に含まれるヘッダーが上書きされます。

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
パイプを使用して、本文の値をにパイプすることもでき `Invoke-WebRequest` ます。

**Body** パラメーターを使用すると、クエリ パラメーターのリストを指定したり、応答の内容を指定したりできます。

入力が GET 要求で、本文が `IDictionary` (通常はハッシュテーブル) の場合、本文はクエリパラメーターとして URI に追加されます。 他の要求の種類 (POST など) の場合、本文は標準形式の要求本文の値として設定され `name=value` ます。

**Body** パラメーターは、オブジェクトを受け取ることもでき `System.Net.Http.MultipartFormDataContent` ます。 これにより、要求が容易に `multipart/form-data` なります。 **MultipartFormDataContent** オブジェクトが **Body** に指定されている場合、 **ContentType**、 **headers**、または **websession** パラメーターに指定されたコンテンツに関連するヘッダーは、 **MultipartFormDataContent** オブジェクトのコンテンツヘッダーによってオーバーライドされます。 この機能は、PowerShell 6.0.0 で追加されました。

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

セキュリティで保護された web 要求に使用されるクライアント証明書を指定します。 証明書が格納されている変数を入力するか、証明書を取得するコマンドまたは式を入力します。

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

証明書は、クライアント証明書ベースの認証で使用されます。 ローカルユーザーアカウントにのみマップできます。ドメインアカウントでは機能しません。

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

このパラメーターを省略して、要求メソッドが POST の場合は、 `Invoke-WebRequest` コンテンツの種類をに設定し `application/x-www-form-urlencoded` ます。 それ以外の場合は、の呼び出しでコンテンツの種類が指定されていません。

**MultipartFormDataContent** オブジェクトが **本文** に指定されている場合、 **ContentType** はオーバーライドされます。

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

Web 要求に使用するカスタムメソッドを指定します。 これは、エンドポイントに必要な要求メソッドが、 **メソッド** で使用可能なオプションではない場合に使用できます。 **メソッド** と **custommethod** を一緒に使用することはできません。

この例では `TEST` 、API に対して HTTP 要求を行います。

`Invoke-WebRequest -uri 'https://api.contoso.com/widget/' -CustomMethod 'TEST'`

この機能は、PowerShell 6.0.0 で追加されました。

```yaml
Type: System.String
Parameter Sets: CustomMethod, CustomMethodNoProxy
Aliases: CM

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DisableKeepAlive

コマンドレットが HTTP ヘッダーの **KeepAlive** 値を **False** に設定することを示します。 既定では、 **KeepAlive** は **True** です。 **KeepAlive** は、後続の要求を容易にするために、サーバーへの永続的な接続を確立します。

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

### -フォーム

辞書を送信に変換 `multipart/form-data` します。 **フォーム** を **本文** と共に使用することはできません。
**ContentType** が使用されている場合は無視されます。

ディクショナリのキーは、フォームフィールド名として使用されます。 既定では、フォーム値は文字列値に変換されます。

値が **system.servicemodel オブジェクトの** 場合は、バイナリファイルの内容が送信されます。 ファイル名は **filename** プロパティとして送信されます。 MIME の種類はとして設定され `application/octet-stream` ます。 `Get-Item` を使用 **すると、system.object オブジェクト** を簡単に指定できます。

```powershell
$Form = @{
    resume = Get-Item 'c:\Users\jdoe\Documents\John Doe.pdf'
}
```

値がコレクション型 (配列やリストなど) の場合、for フィールドは複数回送信されます。
既定では、リストの値は文字列として扱われます。 値が **system.servicemodel オブジェクトの** 場合は、バイナリファイルの内容が送信されます。 入れ子になったコレクションはサポートされていません。

```powershell
$Form = @{
    tags     = 'Vacation', 'Italy', '2017'
    pictures = Get-ChildItem 'c:\Users\jdoe\Pictures\2017-Italy\'
}
```

上の例では `tags` 、フィールドは、、、およびの各につき1回、フォームに3回指定されてい `Vacation` `Italy` `2017` ます。 また、フィールドは、 `pictures` フォルダー内のファイルごとに1回送信され `2017-Italy` ます。 そのフォルダー内のファイルのバイナリコンテンツは、値として送信されます。

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

UserAgent ヘッダーを設定するには、**UserAgent** パラメーターを使用します。 このパラメーターを使用して **、ユーザーエージェント** または cookie のヘッダーを指定することはできません。

などのコンテンツ関連ヘッダー `Content-Type` は、 **MultipartFormDataContent** オブジェクトが **本文** に指定されたときにオーバーライドされます。

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

ファイルから Web 要求の内容を取得します。 パスとファイル名を入力します。 パスを省略した場合、既定値は現在のディレクトリです。

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

接続に失敗する前に PowerShell が代替 Uniform Resource Identifier (URI) に接続をリダイレクトする回数を指定します。 既定値は 5 です。 値 0 (ゼロ) を指定した場合、リダイレクトはまったく行われません。

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
- Merge
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

コマンドレットが送信先に接続するためにプロキシを使用しないことを示します。 環境で構成されているプロキシをバイパスする必要がある場合は、このスイッチを使用します。 この機能は、PowerShell 6.0.0 で追加されました。

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

このコマンドレットが応答本文を保存する出力ファイルを指定します。 パスとファイル名を入力します。
パスを省略した場合、既定値は現在のディレクトリです。 名前はリテラルパスとして扱われます。
角かっこ () を含む名前 `[]` は、単一引用符 () で囲む必要があり `'` ます。

既定では、は `Invoke-WebRequest` パイプラインに結果を返します。 結果をファイルとパイプラインに送信するには、**Passthru** パラメーターを使用します。

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

コマンドレットがファイルに書き込むだけでなく、結果を返すことを示します。 このパラメーターは、同時に **OutFile** パラメーターがコマンドで使用されている場合にのみ有効です。

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

インターネットリソースに直接接続するのではなく、要求のプロキシサーバーを指定します。
ネットワーク プロキシ サーバーの URI を入力します。

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
Default value: Current user
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

### -再開

部分ファイルのダウンロードを再開するのに最適な試行を実行します。 **再開** に **は、出力** 出力が必要です。

**Resume** はローカルファイルとリモートファイルのサイズでのみ動作し、ローカルファイルとリモートファイルが同じであることを検証しません。

ローカルファイルのサイズがリモートファイルのサイズよりも小さい場合、コマンドレットはファイルのダウンロードを再開し、残りのバイトをファイルの末尾に追加しようとします。

ローカルファイルのサイズがリモートファイルのサイズと同じ場合、アクションは実行されず、コマンドレットはダウンロードが既に完了していると想定します。

ローカルファイルのサイズがリモートファイルのサイズより大きい場合は、ローカルファイルが上書きされ、リモートファイル全体が再ダウンロードされます。 この動作は、 **Resume****を使用しない場合** と同じです。

リモートサーバーがダウンロードの再開をサポートしていない場合は、ローカルファイルが上書きされ、リモートファイル全体が再ダウンロードされます。 この動作は、 **Resume****を使用しない場合** と同じです。

ローカルファイルが存在しない場合は、ローカルファイルが作成され、リモートファイル全体がダウンロードされます。 この動作は、 **Resume****を使用しない場合** と同じです。

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

セッション変数を指定すると、は `Invoke-WebRequest` web 要求セッションオブジェクトを作成し、PowerShell セッション内の指定された名前の変数に割り当てます。 コマンドが完了すると、すぐに変数をセッションで使用できます。

リモート セッションとは異なり、Web 要求セッションは永続的な接続ではありません。 これは、cookie、資格情報、リダイレクトの最大値、ユーザーエージェント文字列など、接続と要求に関する情報を含むオブジェクトです。 Web 要求セッションを使用して、Web 要求の間で状態とデータを共有することができます。

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

証明書の検証チェックをスキップします。 これには、有効期限、失効、信頼されたルート証明機関などのすべての検証が含まれます。

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

このスイッチは、 **ContentType**、 **Headers** 、および **UserAgent** パラメーターに渡される値の検証を無効にします。

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

**Sslprotocol** では **websslprotocol** フラグ列挙型が使用されます。 フラグ表記を使用して複数のプロトコルを指定したり、複数の **Websslprotocol** オプションを **bor** と組み合わせたりすることはできますが、複数のプロトコルを指定することは、すべてのプラットフォームでサポートされているわけではありません。

> [!NOTE]
> Windows 以外のプラットフォームで `Tls` は、 `Tls12` オプションとしてまたはを指定することはできません。 のサポート `Tls13` は、すべてのオペレーティングシステムで使用できるわけではなく、オペレーティングシステムごとに検証する必要があります。

この機能は PowerShell 6.0.0 で追加され、 `Tls13` powershell 7.1 でがサポートされるようになりました。

```yaml
Type: Microsoft.PowerShell.Commands.WebSslProtocol
Parameter Sets: (All)
Aliases:
Accepted values: Default, Tls, Tls11, Tls12

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
Default value: 0
Accept pipeline input: False
Accept wildcard characters: False
```

### -トークン

要求に含める OAuth またはベアラートークン。 **トークン** は、特定の **認証** オプションに必要です。 個別に使用することはできません。

**トークン** は、 `SecureString` トークンを含むを受け取ります。 トークンを手動で指定するには、次のようにします。

`Invoke-WebRequest -Uri $uri -Authentication OAuth -Token (Read-Host -AsSecureString)`

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

Web 要求の送信先となるインターネットリソースの Uniform Resource Identifier (URI) を指定します。 URI を入力します。 このパラメーターは、HTTP または HTTPS のみをサポートします。

このパラメーターは必須です。 パラメーター名 **Uri** は省略可能です。

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

たとえば、次のコマンドは、Internet Explorer のユーザーエージェント文字列を使用します。

```powershell
Invoke-WebRequest -Uri https://website.com/ -UserAgent ([Microsoft.PowerShell.Commands.PSUserAgent]::InternetExplorer)
```

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

Web 要求セッションを作成するには、コマンドの **sessionvariable** パラメーターの値にドル記号を付けずに変数名を入力し `Invoke-WebRequest` ます。 `Invoke-WebRequest` セッションを作成し、変数に保存します。 後続のコマンドで、この変数を **WebSession** パラメーターの値として使用します。

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

パイプを使用して、web 要求の本文をにパイプすることができ `Invoke-WebRequest` ます。

## 出力

### Microsoft. PowerShell. BasicHtmlWebResponseObject

## 注

PowerShell 6.0.0 以降で `Invoke-WebRequest` は、基本的な解析のみがサポートされます。

詳細については、「 [Basichtmlwebresponseobject](/dotnet/api/microsoft.powershell.commands.basichtmlwebresponseobject)」を参照してください。

.NET Core 3.1 の変更のため、PowerShell 7.0 以降では、 [Httpclient. defaultproxy](/dotnet/api/system.net.http.httpclient.defaultproxy?view=netcore-3.1) プロパティを使用してプロキシの構成を決定します。

このプロパティの値は、プラットフォームによって決まります。

- **Windows の場合**: 環境変数からプロキシ構成を読み取ります。 これらの変数が定義されていない場合、プロパティはユーザーのプロキシ設定から派生します。
- **MacOS の場合**: 環境変数からプロキシ構成を読み取ります。 これらの変数が定義されていない場合、プロパティはシステムのプロキシ設定から派生します。
- **Linux の場合**: 環境変数からプロキシ構成を読み取ります。 これらの変数が定義されていない場合、プロパティは、すべてのアドレスをバイパスするように構成されていないインスタンスを初期化します。

Windows および Unix ベースのプラットフォームでの初期化に使用される環境変数 `DefaultProxy` は次のとおりです。

- ` HTTP_PROXY`: HTTP 要求で使用されるプロキシサーバーのホスト名または IP アドレス。
- `HTTPS_PROXY`: HTTPS 要求で使用されるプロキシサーバーのホスト名または IP アドレス。
- `ALL_PROXY`: HTTP および HTTPS 要求で使用されるプロキシサーバーのホスト名または IP アドレス `HTTP_PROXY` (またはが定義されて `HTTPS_PROXY` いない場合)。
- `NO_PROXY`: プロキシから除外する必要があるホスト名のコンマ区切りのリスト。

## 関連リンク

[Invoke-RestMethod](Invoke-RestMethod.md)

[ConvertFrom-Json](ConvertFrom-Json.md)

[ConvertTo-Json](ConvertTo-Json.md)
