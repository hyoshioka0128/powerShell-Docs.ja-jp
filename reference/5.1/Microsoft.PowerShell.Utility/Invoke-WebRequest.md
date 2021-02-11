---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 01/26/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Invoke-WebRequest
ms.openlocfilehash: f3545065d4879830a5051ef687f210c7fbd1251e
ms.sourcegitcommit: 11880ca974fe2df308191c9f6dcdfe0b89c2dc67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/27/2021
ms.locfileid: "98860663"
---
# Invoke-WebRequest

## 概要
インターネット上の Web ページからコンテンツを取得します。

## SYNTAX

```
Invoke-WebRequest [-UseBasicParsing] [-Uri] <Uri> [-WebSession <WebRequestSession>] [-SessionVariable <String>]
 [-Credential <PSCredential>] [-UseDefaultCredentials] [-CertificateThumbprint <String>]
 [-Certificate <X509Certificate>] [-UserAgent <String>] [-DisableKeepAlive] [-TimeoutSec <Int32>]
 [-Headers <IDictionary>] [-MaximumRedirection <Int32>] [-Method <WebRequestMethod>] [-Proxy <Uri>]
 [-ProxyCredential <PSCredential>] [-ProxyUseDefaultCredentials] [-Body <Object>] [-ContentType <String>]
 [-TransferEncoding <String>] [-InFile <String>] [-OutFile <String>] [-PassThru] [<CommonParameters>]
```

## Description

`Invoke-WebRequest`コマンドレットは、HTTP、HTTPS、FTP、およびファイル要求を web ページまたは web サービスに送信します。
さらに、応答を解析し、フォーム、リンク、画像、およびその他の重要な HTML 要素のコレクションを返します。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

> [!NOTE]
> 既定では、プロパティを設定するためにページを解析するときに、web ページ内のスクリプトコードが実行される場合があり `ParsedHtml` ます。
> `-UseBasicParsing`これを抑制するには、スイッチを使用します。

## 例

### 例 1: web 要求を送信する

このコマンドは、コマンドレットを使用して、 `Invoke-WebRequest` web 要求を Bing.com サイトに送信します。

```powershell
$R = Invoke-WebRequest -URI https://www.bing.com?q=how+many+feet+in+a+mile
$R.AllElements | Where-Object {
    $_.name -like "* Value" -and $_.tagName -eq "INPUT"
} | Select-Object Name, Value
```

```Output
name       value
----       -----
From Value 1
To Value   5280
```

最初のコマンドは、要求を発行し、その応答を変数に保存し `$R` ます。

2番目のコマンドは、 **Allelements** プロパティのオブジェクトをフィルター処理します。ここで、 **name** プロパティは "* Value" で、 **tagName** は "INPUT" になります。 フィルター処理された結果は、 `Select-Object` **名前** と **値** のプロパティを選択するためにパイプ処理されます。

### 例 2: ステートフルな web サービスを使用する

この例では、 `Invoke-WebRequest` Facebook などのステートフルな web サービスでコマンドレットを使用する方法を示します。

```powershell
$R = Invoke-WebRequest https://www.facebook.com/login.php -SessionVariable fb
# This command stores the first form in the Forms property of the $R variable in the $Form variable.
$Form = $R.Forms[0]
# This command shows the fields available in the Form.
$Form.fields
```

```Output
Key                     Value
---                     -----
...
email
pass
...
```

```powershell
# These commands populate the username and password of the respective Form fields.
$Form.Fields["email"]="User01@Fabrikam.com"
$Form.Fields["pass"]="P@ssw0rd"
# This command creates the Uri that will be used to log in to facebook.
# The value of the Uri parameter is the value of the Action property of the form.
$Uri = "https://www.facebook.com" + $Form.Action
# Now the Invoke-WebRequest cmdlet is used to sign into the Facebook web service.
# The WebRequestSession object in the $FB variable is passed as the value of the WebSession parameter.
# The value of the Body parameter is the hash table in the Fields property of the form.
# The value of the *Method* parameter is POST. The command saves the output in the $R variable.
$R = Invoke-WebRequest -Uri $Uri -WebSession $FB -Method POST -Body $Form.Fields
$R.StatusDescription
```

最初のコマンドは、 `Invoke-WebRequest` コマンドレットを使用してサインイン要求を送信します。 このコマンドは、 **Sessionvariable** パラメーターの値に "FB" という値を指定し、結果を変数に保存し `$R` ます。 コマンドが完了すると、 `$R` 変数に **Htmlwebresponseobject** が含まれ、変数には `$FB` **WebRequestSession** オブジェクトが含まれます。

`Invoke-WebRequest`コマンドレットが facebook にサインインすると、変数内の web 応答オブジェクトの **statusdescription** プロパティは、 `$R` ユーザーが正常にサインインしたことを示します。

### 例 3: web ページからリンクを取得する

このコマンドは、Web ページ内のリンクを取得します。

```powershell
(Invoke-WebRequest -Uri "https://devblogs.microsoft.com/powershell/").Links.Href
```

コマンドレットでは、 `Invoke-WebRequest` web ページの内容を取得します。 次に、返された **Htmlwebresponseobject** の **Links** プロパティを使用して、各リンクの **Href** プロパティを表示します。

### 例 4: Invoke-WebRequest からの失敗したメッセージをキャッチする

が `Invoke-WebRequest` 成功以外の HTTP メッセージ (404、500など) を検出すると、出力は返されず、終了エラーをスローします。 エラーをキャッチして **StatusCode** を表示するには、ブロックで実行を囲むことができ `try/catch` ます。 次の例は、これを実現する方法を示しています。

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

### -本文

要求の本文を指定します。
本文は、ヘッダーに続く要求の内容です。
パイプを使用して、本文の値をにパイプすることもでき `Invoke-WebRequest` ます。

**Body** パラメーターを使用すると、クエリ パラメーターのリストを指定したり、応答の内容を指定したりできます。

入力が GET 要求で、本文が **IDictionary** (通常はハッシュテーブル) の場合、本文はクエリパラメーターとして URI に追加されます。 他の GET 要求の場合、本文は標準形式の要求本文の値として設定され `name=value` ます。

本文がフォームである場合、または呼び出しの出力である場合 `Invoke-WebRequest` 、PowerShell は要求の内容をフォームフィールドに設定します。
以下に例を示します。

`$r = Invoke-WebRequest https://website.com/login.aspx`
`$r.Forms\[0\].Name = "MyName"`
`$r.Forms\[0\].Password = "MyPassword"`
`Invoke-RestMethod https://website.com/service.aspx -Body $r`

- または

`Invoke-RestMethod https://website.com/service.aspx -Body $r.Forms\[0\]`

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

証明書を検索するには、を使用する `Get-PfxCertificate` か、 `Get-ChildItem` **証明書** () ドライブのコマンドレットを使用し `Cert:` ます。 証明書が無効な場合、または証明書に十分な権限がない場合、コマンドは失敗します。

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

要求を送信するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。 証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書の拇印を取得するに `Get-Item` は、PowerShell ドライブでまたはコマンドを使用し `Get-ChildItem` `Cert:` ます。

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

このパラメーターを省略して、要求メソッドが POST の場合は、 `Invoke-WebRequest` コンテンツの種類を application/url エンコードに設定します。 それ以外の場合、呼び出しの中でコンテンツ タイプは指定されません。

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

### -ヘッダー

Web 要求のヘッダーを指定します。 ハッシュ テーブルまたは辞書を入力します。

**UserAgent** ヘッダーを設定するには、 **UserAgent** パラメーターを使用します。 このパラメーターを使用して、 **UserAgent** または cookie のヘッダーを指定することはできません。

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

```yaml
Type: Microsoft.PowerShell.Commands.WebRequestMethod
Parameter Sets: (All)
Aliases:
Accepted values: Default, Get, Head, Post, Put, Delete, Trace, Options, Merge, Patch

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -出力

このコマンドレットが応答本文を保存する出力ファイルを指定します。 パスとファイル名を入力します。
パスを省略した場合、既定値は現在のディレクトリです。

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

### -プロキシ

インターネットリソースに直接接続するのではなく、要求のプロキシサーバーを指定します。
ネットワーク プロキシ サーバーの URI を入力します。

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

またはなどのユーザー名を入力する `User01` `Domain01\User01` か、コマンドレットによって生成されるような **PSCredential** オブジェクトを入力し `Get-Credential` ます。

このパラメーターは、 **プロキシ** パラメーターがコマンドでも使用されている場合にのみ有効です。 **ProxyCredential** パラメーターと **ProxyUseDefaultCredentials** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyUseDefaultCredentials

コマンドレットが、 **プロキシ** パラメーターで指定されたプロキシサーバーにアクセスするために、現在のユーザーの資格情報を使用することを示します。

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

このコマンドレットによって web 要求セッションが作成され、値に保存される変数を指定します。
ドル記号 () を付けずに変数名を入力し `$` ます。

セッション変数を指定すると、は `Invoke-WebRequest` web 要求セッションオブジェクトを作成し、PowerShell セッション内の指定された名前の変数に割り当てます。 コマンドが完了すると、すぐに変数をセッションで使用できます。

リモート セッションとは異なり、Web 要求セッションは永続的な接続ではありません。 Web 要求セッションは、Cookie、資格情報、リダイレクトの最大値、ユーザー エージェント文字列などの、接続と要求に関する情報を含むオブジェクトです。 Web 要求セッションを使用して、Web 要求の間で状態とデータを共有することができます。

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

### -TimeoutSec

要求がタイムアウトになるまでの待機時間を指定します。値を秒単位で入力します。 既定値は 0 で、無制限のタイムアウトを意味しています。

ドメインネームシステム (DNS) クエリは、返されるまで最大15秒かかることがあります。要求に解決を必要とするホスト名が含まれていて、 **Timeoutsec** を0より大きく15秒未満に設定した場合、 **WebException** がスローされる前に15秒以上かかることがあり、要求はタイムアウトになります。

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

Web 要求の送信先の Uniform Resource Identifier (URI) を指定します。 URI を入力します。 このパラメーターは、HTTP、HTTPS、FTP、FILE の値をサポートします。

このパラメーターは必須です。

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

コマンドレットがドキュメントオブジェクトモデル (DOM) 解析を行わずに、HTML コンテンツの応答オブジェクトを使用することを示します。 このパラメーターは、Windows Server オペレーティング システムの Server Core インストールなど、コンピューターに Internet Explorer がインストールされていない場合に必要です。

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

Cmdet が、web 要求を送信するために現在のユーザーの資格情報を使用することを示します。

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

Web 要求のユーザー エージェント文字列を指定します。 既定のユーザーエージェントは、 `Mozilla/5.0 (Windows NT; Windows NT 6.1; en-US) WindowsPowerShell/3.0` オペレーティングシステムとプラットフォームによって微妙な違いがあるに似ています。

ほとんどのインターネットブラウザーで使用される標準のユーザーエージェント文字列を使用して web サイトをテストするには、Chrome、FireFox、InternetExplorer、Opera、Safari などの [PSUserAgent](/dotnet/api/microsoft.powershell.commands.psuseragent) クラスのプロパティを使用します。 たとえば、次のコマンドは、Internet Explorer のユーザーエージェント文字列を使用します。

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

Web 要求セッションを作成するには、コマンドの **Sessionvariable** パラメーターの値に、(ドル記号を付けずに) 変数名を入力し `Invoke-WebRequest` ます。 `Invoke-WebRequest` セッションを作成し、変数に保存します。 後続のコマンドで、この変数を **WebSession** パラメーターの値として使用します。

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

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Object

パイプを使用して、web 要求の本文をにパイプすることができ `Invoke-WebRequest` ます。

## 出力

### Microsoft.PowerShell.Commands.HtmlWebResponseObject

## 注

## 関連リンク

[Invoke-RestMethod](Invoke-RestMethod.md)

[ConvertFrom-Json](ConvertFrom-Json.md)

[ConvertTo-Json](ConvertTo-Json.md)
