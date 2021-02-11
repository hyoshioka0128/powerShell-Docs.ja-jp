---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 02/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/protect-cmsmessage?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Protect-CmsMessage
ms.openlocfilehash: 57213b72bee5733f6e8089ff7dcbb9be82104d8a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601021"
---
# Protect-CmsMessage

## 概要
暗号化メッセージ構文形式を使用してコンテンツを暗号化します。

## SYNTAX

### ByContent (既定)

```
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-Content] <PSObject> [[-OutFile] <String>]
 [<CommonParameters>]
```

### ByPath

```
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-Path] <String> [[-OutFile] <String>] [<CommonParameters>]
```

### ByLiteralPath

```
Protect-CmsMessage [-To] <CmsMessageRecipient[]> [-LiteralPath] <String> [[-OutFile] <String>]
 [<CommonParameters>]
```

## Description

コマンドレットでは、 `Protect-CmsMessage` 暗号化メッセージ構文 (CMS) 形式を使用してコンテンツを暗号化します。

CMS コマンドレットは、 [RFC5652](https://tools.ietf.org/html/rfc5652.html)によって説明されているように、IETF 形式を使用したコンテンツの暗号化と暗号化解除をサポートしています。

CMS 暗号化標準では、公開キー暗号化を使用します。この暗号化では、コンテンツの暗号化に使用されるキー (公開キー) とコンテンツの暗号化を解除するために使用されるキー (秘密キー) は区別されます。 公開キーは広く共有でき、機密性の高いデータではありません。 いずれかのコンテンツがこの公開キーで暗号化された場合、秘密キーのみが暗号化を解除できます。 公開キー暗号化の詳細については、「[公開鍵暗号](https://en.wikipedia.org/wiki/Public-key_cryptography)」を参照してください。

コマンドレットを実行するには、事前に `Protect-CmsMessage` 暗号化証明書を設定しておく必要があります。
PowerShell で認識されるようにするには、暗号化証明書をデータ暗号化証明書として識別するために一意の拡張キー使用法 ([EKU](/windows/desktop/SecCrypto/eku)) ID が必要です (コード署名および暗号化されたメールの id など)。 ドキュメントの暗号化に使用する証明書の例については、このトピックの例1を参照してください。

PowerShell 7.1 では、Linux と macOS のサポートが追加されました。

## 例

### 例 1: コンテンツを暗号化するための証明書を作成する

コマンドレットを実行する前に `Protect-CmsMessage` 、暗号化証明書を作成する必要があります。 次のテキストを使用して、件名行の名前を名前、電子メール、またはその他の識別子に変更し、証明書をファイルに保存し `DocumentEncryption.inf` ます (この例で示すように)。

```powershell
# Create .INF file for certreq
{[Version]
Signature = "$Windows NT$"

[Strings]
szOID_ENHANCED_KEY_USAGE = "2.5.29.37"
szOID_DOCUMENT_ENCRYPTION = "1.3.6.1.4.1.311.80.1"

[NewRequest]
Subject = "cn=youralias@emailaddress.com"
MachineKeySet = false
KeyLength = 2048
KeySpec = AT_KEYEXCHANGE
HashAlgorithm = Sha1
Exportable = true
RequestType = Cert
KeyUsage = "CERT_KEY_ENCIPHERMENT_KEY_USAGE | CERT_DATA_ENCIPHERMENT_KEY_USAGE"
ValidityPeriod = "Years"
ValidityPeriodUnits = "1000"

[Extensions]
%szOID_ENHANCED_KEY_USAGE% = "{text}%szOID_DOCUMENT_ENCRYPTION%"
} | Out-File -FilePath DocumentEncryption.inf

# After you have created your certificate file, run the following command to add
# the certificate file to the certificate store. Now you are ready to encrypt and
# decrypt content with the next two examples.
certreq.exe -new DocumentEncryption.inf DocumentEncryption.cer
```

### 例 2: 電子メールで送信されたメッセージを暗号化する

```powershell
$Protected = "Hello World" | Protect-CmsMessage -To "*youralias@emailaddress.com*"
```

次の例では、メッセージ "Hello World" をコマンドレットにパイプして暗号化し、 `Protect-CmsMessage` 暗号化されたメッセージを変数に保存します。 **To** パラメーターは、証明書の Subject 行の値を使用します。

### 例 3: ドキュメントの暗号化証明書を表示する

```
PS C:\> cd Cert:\CurrentUser\My
PS Cert:\CurrentUser\My> Get-ChildItem -DocumentEncryptionCert
```

証明書プロバイダーでドキュメントの暗号化証明書を表示するには、 [get-childitem](../Microsoft.PowerShell.Management/Get-ChildItem.md)の **documentencryptioncert** 動的パラメーターを追加します。このパラメーターは、証明書プロバイダーが読み込まれたときにのみ使用できます。

## PARAMETERS

### -コンテンツ

暗号化するコンテンツを含む **PSObject** を指定します。 たとえば、イベントメッセージの内容を暗号化し、そのメッセージを含む変数 ( `$Event` この例では) を **content** パラメーターの値として使用できます `$event = Get-WinEvent -ProviderName "PowerShell" -MaxEvents 1` 。 また、コマンドレットを使用し `Get-Content` て、Microsoft Word 文書などのファイルの内容を取得し **、content** パラメーターの値として使用する変数にコンテンツを保存することもできます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: ByContent
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

暗号化するコンテンツへのパスを指定します。 **Path** とは異なり、**LiteralPath** の値は入力した内容のまま使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -出力

暗号化されたコンテンツを送信するファイルのパスとファイル名を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

暗号化するコンテンツへのパスを指定します。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -To

次のいずれかの形式で識別される、1つまたは複数の CMS メッセージ受信者を指定します。

- (証明書プロバイダーから取得された) 実際の証明書。
- 証明書を含むファイルへのパス。
- 証明書が格納されているディレクトリへのパス。
- 証明書の拇印 (証明書ストアを検索するために使用されます)。
- 証明書のサブジェクト名 (証明書ストアを検索するために使用されます)。

```yaml
Type: System.Management.Automation.CmsMessageRecipient[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

## 出力

## 注

## 関連リンク

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Get-CmsMessage](Get-CmsMessage.md)

[Unprotect-CmsMessage](Unprotect-CmsMessage.md)
