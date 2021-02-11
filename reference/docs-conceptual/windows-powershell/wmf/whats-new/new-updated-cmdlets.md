---
ms.date: 06/12/2017
title: 新規および更新されたコマンドレット
description: Windows PowerShell 5.1 には、新規または更新されたコマンドレットが多数含まれています。
ms.openlocfilehash: bdfbe5a263864a172aefb0aa5731f59b95ab43ca
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663216"
---
# <a name="new-and-updated-cmdlets"></a>新規および更新されたコマンドレット

Microsoft では、コミュニティからのフィードバックに基づいて、コマンドレットの新たな追加、および既存のコマンドレットの更新を行いました。

## <a name="archive-cmdlets"></a>アーカイブのコマンドレット

2 つの新しいコマンドレット、`Compress-Archive` と `Expand-Archive` では、ZIP ファイルを圧縮および展開できます。

詳細については、[Microsoft.Powershell.Archive](/powershell/module/microsoft.powershell.archive/) モジュールに関するドキュメントを参照してください。

## <a name="catalog-cmdlets"></a>カタログ コマンドレット

Microsoft.PowerShell.Security モジュールに次の新しいコマンドレットが 2 つ追加されました。

- [New-FileCatalog](/powershell/module/microsoft.powershell.security/New-FileCatalog)
- [Test-FileCatalog](/powershell/module/microsoft.powershell.security/Test-FileCatalog)

これらでは、Windows カタログ ファイルが生成および検証されます。

## <a name="clipboard-cmdlets"></a>クリップボードのコマンドレット

`Get-Clipboard` と `Set-Clipboard` を使うと、Windows PowerShell セッションとの間でコンテンツを容易に転送できます。 クリップボードのコマンドレットは、画像、オーディオ ファイル、ファイルのリスト、およびテキストをサポートします。

詳細については、次を参照してください。

- [Get-Clipboard](/powershell/module/Microsoft.PowerShell.Management/Get-Clipboard)
- [Set-Clipboard](/powershell/module/Microsoft.PowerShell.Management/Set-Clipboard)

## <a name="cryptographic-message-syntax-cms-cmdlets"></a>Cryptographic Message Syntax (CMS) コマンドレット

Cryptographic Message Syntax コマンドレットは、[RFC5652](https://tools.ietf.org/html/rfc5652.html) で説明されているように暗号によってメッセージを保護するための IETF 標準書式を使用して、コンテンツの暗号化と暗号化解除をサポートします。

CMS 暗号化標準では、公開キー暗号化が実装されます。公開キー暗号化では、コンテンツの暗号化に使用されるキー ( *公開キー* ) とコンテンツの暗号化解除に使用されるキー ( *秘密キー* ) は別です。

公開キーは広く共有でき、機密データではありません。 公開キーで暗号化したコンテンツは、秘密キーを使用してのみ暗号化解除できます。 公開キー暗号化の詳細については、「[公開鍵暗号](https://en.wikipedia.org/wiki/Public-key_cryptography)」を参照してください。

詳細情報

- [Get-CmsMessage](/powershell/module/Microsoft.PowerShell.Security/Get-CmsMessage)
- [Protect-CmsMessage](/powershell/module/Microsoft.PowerShell.Security/Protect-CmsMessage)
- [Unprotect-CmsMessage](/powershell/module/Microsoft.PowerShell.Security/unprotect-CmsMessage)

証明書が PowerShell でデータ暗号化証明書として認識されるには、'Code Signing' または 'Encrypted Mail' のような一意のキー使用法識別子 (EKU) が必要です。 証明書プロバイダーでドキュメントの暗号化証明書を表示するには、`Get-ChildItem` の **DocumentEncryptionCert** 動的パラメーターを使用します。

```powershell
Get-ChildItem Cert:\CurrentUser -DocumentEncryptionCert -Recurse
```

## <a name="extract-and-parse-structured-objects-from-string-content"></a>文字列コンテンツから構造化オブジェクトを抽出して分析する

### <a name="convertfrom-string"></a>ConvertFrom-String

新しい `ConvertFrom-String` コマンドレットでは、次の 2 つのモードをサポートしています。

- 基本区切りの解析
- 自動生成されるサンプルによる解析

区切り記号による解析では、既定で、入力を空白の位置で分割し、生成されるグループにプロパティ名を割り当てます。

**UpdateTemplate** パラメーターは、学習アルゴリズムの結果をテンプレート ファイル内のコメントに保存します。 これにより、学習プロセス (最も時間のかかる段階) が 1 回限りのコストになります。 `ConvertFrom-String` をエンコードされた学習アルゴリズムを含むテンプレートを使って実行すると、ほぼ瞬時に完了します。

詳細については、「[ConvertFrom-String](/powershell/module/Microsoft.PowerShell.Utility/ConvertFrom-String)」を参照してください。

### <a name="convert-string"></a>Convert-String

`Convert-String` では、テキストをどのように表示するかのその前後の例を提供します。 テキストは、コマンドレットによって自動的に書式設定されます。

詳細については、「[Convert-String](/powershell/module/Microsoft.PowerShell.Utility/Convert-String)」を参照してください。

## <a name="updates-to-fileinfo-object"></a>FileInfo オブジェクトの更新

ファイルのバージョン情報は、特にファイルにパッチが適用された場合に、間違えやすいです。 WMF 5.0 では、 **FileInfo** オブジェクトに新しいスクリプト プロパティとして **FileVersionRaw** と **ProductVersionRaw** が追加されています。
powershell.exe に対して表示されるプロパティを次に示します ($pid は PowerShell プロセスの ID であるとします)。

```powershell
Get-Process -Id $pid -FileVersionInfo | Format-List *version*
```

```Output
FileVersionRaw    : 10.0.17763.1
ProductVersionRaw : 10.0.17763.1
FileVersion       : 10.0.17763.1 (WinBuild.160101.0800)
ProductVersion    : 10.0.17763.1
```

## <a name="format-hex"></a>Format-Hex

`Format-Hex` を使用すると、テキスト データやバイナリ データを 16 進数形式で表示できます。

詳細については、「[Format-Hex](/powershell/module/microsoft.powershell.utility/format-hex)」を参照してください。

## <a name="get-childitem-has--depth-parameter"></a>Get-ChildItem の -Depth パラメーター

`Get-ChildItem` に、再帰を制限する **Recurse** で使用する **Depth** パラメーターが追加されました。

## <a name="modules-support-for-declaring-version-ranges-1-etc"></a>バージョン範囲 (1. * など) の宣言をサポートするモジュール

**MinimumVersion** と **MaximumVersion** を組み合わせることにより、特定の範囲内でモジュールをインポートできるようになりました。 このパラメーターでは、ワイルドカードもサポートしています。

```powershell
Import-Module psreadline -Verbose -MinimumVersion 1.0 -MaximumVersion 1.2.*
```

```Output
VERBOSE: Loading module from path 'C:\Program Files\WindowsPowerShell\Modules\psreadline\1.1\psreadline.psd1'.
VERBOSE: Importing cmdlet 'Get-PSReadlineKeyHandler'.
VERBOSE: Importing cmdlet 'Get-PSReadlineOption'.
VERBOSE: Importing cmdlet 'Remove-PSReadlineKeyHandler'.
VERBOSE: Importing cmdlet 'Set-PSReadlineKeyHandler'.
VERBOSE: Importing cmdlet 'Set-PSReadlineOption'.
VERBOSE: Importing function 'PSConsoleHostReadline'.
```

## <a name="new-guid"></a>New-Guid

一意の識別子が必要なシナリオは多数あります。 `New-GUID` コマンドレットでは簡単に新しい GUID を作成できます。

```powershell
New-Guid
```

```Output
Guid
----
e19d6ea5-3cc2-4db9-8095-0cdaed5a703d
```

## <a name="nonewline-parameter"></a>NoNewLine パラメーター

`Out-File`、`Add-Content`、および `Set-Content` に、出力後の改行を省略する新しい **NoNewline** スイッチが追加されました。 次に例を示します。

```powershell
"This is " | Out-File -FilePath Example.txt -NoNewline
"a single " | Add-Content -Path Example.txt -NoNewline
"sentence." | Add-Content -Path Example.txt -NoNewline
Get-Content .\Example.txt
```

```Output
This is a single sentence.
```

**NoNewline** を指定しないと、各フラグメントが別個の行に出力されます。

```powershell
"This is " | Out-File -FilePath Example.txt
"a single " | Add-Content -Path Example.txt
"sentence." | Add-Content -Path Example.txt
Get-Content .\Example.txt
```

```Output
This is
a single
sentence.
```

## <a name="interact-with-symbolic-links-using-improved-item-cmdlets"></a>機能を強化された Item コマンドレットを使ってシンボリック リンクを操作する

シンボリック リンクをサポートする、Item コマンドレットや、関連するいくつかのコマンドレットが拡張されました。

### <a name="symbolic-link-files"></a>シンボリック リンク ファイル

この例では、$pshome\profile.ps1 にリンクされる MySymLinkFile.txt という新しいシンボリック リンク ファイルを C:\Temp に作成します。 結果は、3 つの例のいずれでも同じになります。

```powershell
New-Item -ItemType SymbolicLink -Path C:\Temp -Name MySymLinkFile.txt -Value $pshome\profile.ps1
New-Item -ItemType SymbolicLink -Path C:\Temp\MySymLinkFile.txt -Value $pshome\profile.ps1
New-Item -ItemType SymbolicLink -Name C:\Temp\MySymLinkFile.txt -Value $pshome\profile.ps1
```

### <a name="symbolic-link-directories"></a>シンボリック リンク ディレクトリ

この例では、$pshome フォルダーにリンクされる MySymLinkDir という新しいシンボリック リンク ディレクトリを C:\Temp に作成します。 結果は、3 つの例のいずれでも同じになります。

```powershell
New-Item -ItemType SymbolicLink -Path C:\Temp -Name MySymLinkDir -Value $pshome
New-Item -ItemType SymbolicLink -Path C:\Temp\MySymLinkDir -Value $pshome
New-Item -ItemType SymbolicLink -Name C:\Temp\MySymLinkDir -Value $pshome
```

### <a name="hard-links"></a>ハード リンク

**パス** と **名前** の同じ組み合わせが前述のように許可されています。

```powershell
New-Item -ItemType HardLink -Path C:\Temp -Name MyHardLinkFile.txt -Value $pshome\profile.ps1
```

### <a name="directory-junctions"></a>ディレクトリ ジャンクション

**パス** と **名前** の同じ組み合わせが前述のように許可されています。

```powershell
New-Item -ItemType Junction -Path C:\Temp\MyJunctionDir -Value $pshome
```

### <a name="get-childitem"></a>Get-ChildItem

`Get-ChildItem` の **Mode** プロパティに、シンボリック リンク ファイルまたはディレクトリを示す 'l' が表示されるようになりました。

```powershell
Get-ChildItem C:\Temp | sort LastWriteTime -Descending

Directory: C:\Temp

Mode       LastWriteTime Length Name
----       ------------- ------ ----
-a---- 6/13/2014 3:00 PM     16 File.txt
d----- 6/13/2014 3:00 PM        Directory
-a---l 6/13/2014 3:21 PM      0 MySymLinkFile.txt
d----l 6/13/2014 3:22 PM        MySymLinkDir
-a---l 6/13/2014 3:23 PM  23304 MyHardLinkFile.txt
d----l 6/13/2014 3:24 PM        MyJunctionDir
```

### <a name="remove-item"></a>Remove-Item

シンボリック リンクの削除は、その他の任意のアイテムの種類の削除と同様に実行できます。

```powershell
Remove-Item C:\Temp\MySymLinkFile.txt
Remove-Item C:\Temp\MySymLinkDir
```

**Force** パラメーターを使用すると、ターゲット ディレクトリとシンボリック リンクのファイルを削除できます。

```powershell
Remove-Item C:\Temp\MySymLinkDir -Force
```

## <a name="new-temporaryfile"></a>New-TemporaryFile

スクリプトでは、ときおり、一時ファイルを作成する必要があります。 これは、次のように `New-TemporaryFile` コマンドレットで実行できます。

```powershell
$tempFile = New-TemporaryFile
$tempFile.FullName
```

```Output
C:\Users\user1\AppData\Local\Temp\tmp375.tmp
```

## <a name="network-switch-management-with-powershell"></a>PowerShell を使用したネットワーク スイッチの管理

WMF 5.0 で導入されたネットワーク スイッチのコマンドレットを使用すると、Windows Server 2012 R2 ロゴ認定を受けたネットワーク スイッチに、スイッチ、仮想 LAN (VLAN)、および基本的なレイヤー 2 ネットワーク スイッチ ポートの構成を適用することができます。 これらのコマンドレットを使用して、次のことを実行できます。

- 次のようなグローバル スイッチ構成:
  - ホスト名の設定
  - スイッチ バナーの設定
  - 構成の保持
  - 機能の有効化または無効化

- VLAN 構成:
  - VLAN の作成または削除
  - VLAN の有効化または無効化
  - VLAN の列挙
  - VLAN にわかりやすい名前を設定する

- レイヤー 2 ポート構成:
  - ポートの列挙
  - ポートの有効化または無効化
  - ポートのモードおよびプロパティの設定
  - ポートのトランクまたはアクセスに VLAN を追加または関連付ける

詳細については、[NetworkSwitchManager](/powershell/module/networkswitchmanager/) に関するドキュメントを参照してください。

## <a name="generate-powershell-cmdlets-based-on-an-odata-endpoint-with-odatautils"></a>ODataUtils を使用した OData エンドポイントに基づく PowerShell コマンドレットの生成

ODataUtils モジュールでは、OData をサポートする REST エンドポイントから PowerShell コマンドレットを生成できます。 Microsoft.PowerShell.ODataUtils モジュールには、次の機能があります。

- サーバー側エンドポイントからクライアント側への追加情報のチャネル。
- クライアント側ページング サポート
- -Select パラメーターを使用したサーバー側フィルタリング
- Web 要求のヘッダーのサポート

`Export-ODataEndPointProxy` コマンドレットで生成されるプロキシ コマンドレットは、サーバー側の OData エンドポイントからの **情報** ストリームの追加情報を提供します。

```powershell
Import-Module Microsoft.PowerShell.ODataUtils -Force
$generatedProxyModuleDir = Join-Path -Path $env:SystemDrive -ChildPath 'ODataDemoProxy'
$uri = "http://services.odata.org/V3/(S(fhleiief23wrm5a5nhf542q5))/OData/OData.svc/"
Export-ODataEndpointProxy -Uri $uri -OutputModule $generatedProxyModuleDir -Force -AllowUnSecureConnection -Verbose -AllowClobber
```

次の例では、上位の製品を取得してその出力を `$infoStream` 変数にキャプチャしています。

**IncludeTotalResponseCount** パラメーターを指定することにより、サーバー上のすべての **Product** レコードの総数を取得できます。

```powershell
Import-Module $generatedProxyModuleDir -Force
$product = Get-Product -Top 1 -AllowUnsecureConnection -AllowAdditionalData -IncludeTotalResponseCount -InformationVariable infoStream
$additionalInfo = $infoStream.GetEnumerator() | % MessageData
$additionalInfo['odata.count']
```

クライアント側ページング サポートを使用して、サーバーからレコードをバッチで取得できます。 これは、ネットワーク経由でサーバーから大量のデータを取得する必要がある場合に役立ちます。

```powershell
$skipCount = 0
$batchSize = 3
while($skipCount -le $additionalInfo['odata.count'])
{
  Get-Product -AllowUnsecureConnection -AllowAdditionalData -Top $batchSize -Skip $skipCount
  $skipCount += $batchSize
}
```

生成されたプロキシ コマンドレットは、クライアントが必要なレコード プロパティのみを受信するフィルターとして使用できる **Select** パラメーターをサポートします。 フィルター処理はサーバー側で行われるため、ネットワーク経由で転送されるデータの量が削減されます。

```powershell
Get-Product -Top 2 -AllowUnsecureConnection -AllowAdditionalData -Select Name
```

`Export-ODataEndpointProxy` コマンドレットとそれによって生成されるプロキシ コマンドレットで、 **Header** パラメーターがサポートされるようになりました。 このヘッダーを使用すると、OData エンドポイントで必要な追加情報をチャネルできます。

次の例では、サブスクリプション キーを含むハッシュ テーブルが **Headers** パラメーターに提供されます。 これは、認証にサブスクリプション キーを必要とするサービスの典型例です。

```powershell
Export-ODataEndpointProxy -Uri $endPointUri -OutputModule $generatedProxyModuleDir -Force -AllowUnSecureConnection -Verbose -Headers @{'subscription-key'='XXXX'}
```
