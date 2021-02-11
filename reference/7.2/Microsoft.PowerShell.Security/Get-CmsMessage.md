---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 02/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-cmsmessage?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-CmsMessage
ms.openlocfilehash: 421b49139f4750787b6c1c04d8a41a624755109a
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600595"
---
# Get-CmsMessage

## 概要
暗号化メッセージ構文形式を使用して暗号化されたコンテンツを取得します。

## SYNTAX

### ByContent

```
Get-CmsMessage [-Content] <String> [<CommonParameters>]
```

### ByPath

```
Get-CmsMessage [-Path] <String> [<CommonParameters>]
```

### ByLiteralPath

```
Get-CmsMessage [-LiteralPath] <String> [<CommonParameters>]
```

## Description

`Get-CmsMessage`コマンドレットでは、暗号化メッセージ構文 (CMS) 形式を使用して暗号化されたコンテンツを取得します。

CMS コマンドレットは、 [RFC5652](https://tools.ietf.org/html/rfc5652)によって説明されているように、メッセージを暗号化して保護するための IETF 形式を使用したコンテンツの暗号化と暗号化解除をサポートします。

CMS 暗号化標準では、公開キー暗号化を使用します。この暗号化では、コンテンツの暗号化に使用されるキー (公開キー) とコンテンツの暗号化を解除するために使用されるキー (秘密キー) は区別されます。 公開キーは広く共有でき、機密性の高いデータではありません。 いずれかのコンテンツがこの公開キーで暗号化された場合、秘密キーのみが暗号化を解除できます。 公開キー暗号化の詳細については、「[公開鍵暗号](https://en.wikipedia.org/wiki/Public-key_cryptography)」を参照してください。

`Get-CmsMessage` CMS 形式で暗号化されたコンテンツを取得します。 コンテンツの暗号化を解除したり保護を解除したりすることはありません。 このコマンドレットを実行すると、暗号化されたコンテンツを取得できます。そのためには、コマンドレットを実行し `Protect-CmsMessage` ます。 暗号化を解除するコンテンツを文字列として指定することも、暗号化されたコンテンツへのパスを指定することもできます。 `Get-CmsMessage` `Unprotect-CmsMessage` コンテンツの暗号化に使用されたドキュメント暗号化証明書に関する情報がある場合は、の結果をにパイプして、コンテンツの暗号化を解除することができます。

PowerShell 7.1 では、Linux と macOS のサポートが追加されました。

## 例

### 例 1: 暗号化されたコンテンツを取得する

```powershell
$Msg = Get-CmsMessage -Path "C:\Users\Test\Documents\PowerShell\Future_Plans.txt"
$Msg.Content
```

```Output
-----BEGIN CMS-----
MIIBqAYJKoZIhvcNAQcDoIIBmTCCAZUCAQAxggFQMIIBTAIBADA0MCAxHjAcBgNVBAMBFWxlZWhv
bG1AbGljcm9zb2Z0LmNvbQIQQYHsbcXnjIJCtH+OhGmc1DANBgkqhkiG9w0BAQcwAASCAQAnkFHM
proJnFy4geFGfyNmxH3yeoPvwEYzdnsoVqqDPAd8D3wao77z7OhJEXwz9GeFLnxD6djKV/tF4PxR
E27aduKSLbnxfpf/sepZ4fUkuGibnwWFrxGE3B1G26MCenHWjYQiqv+Nq32Gc97qEAERrhLv6S4R
G+2dJEnesW8A+z9QPo+DwYP5FzD0Td0ExrkswVckpLNR6j17Yaags3ltNXmbdEXekhi6Psf2MLMP
TSO79lv2L0KeXFGuPOrdzPRwCkV0vNEqTEBeDnZGrjv/5766bM3GW34FXApod9u+VSFpBnqVOCBA
DVDraA6k+xwBt66cV84AHLkh0kT02SIHMDwGCSqGSIb3DQEHATAdBglghkgBZQMEASoEEJbJaiRl
KMnBoD1dkb/FzSWAEBaL8xkFwCu0e1AtDj7nSJc=
-----END CMS-----
```

このコマンドは、C:\Users\Test\Documents\PowerShell\Future_Plans.txt にある暗号化されたコンテンツを取得します。

### 例 2: パイプで暗号化されたコンテンツを Unprotect-CmsMessage に

```powershell
$Msg = Get-CmsMessage -Path "C:\Users\Test\Documents\PowerShell\Future_Plans.txt"
$Msg | Unprotect-CmsMessage -To "cn=youralias@emailaddress.com"
```

```Output
Try the new Break All command
```

このコマンドは、コマンドレットの結果を `Get-CmsMessage` 例1からにパイプし、 `Unprotect-CmsMessage` メッセージの暗号化を解除してプレーンテキストで読み取ります。 この場合、 **To** パラメーターの値は、暗号化された証明書の件名行の値になります。 暗号化が解除されたことを示します。 "新しい Break All コマンドを試してください" というメッセージが表示されます。

## PARAMETERS

### -コンテンツ

暗号化された文字列、または暗号化された文字列を含む変数を指定します。

```yaml
Type: System.String
Parameter Sets: ByContent
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

取得する暗号化されたコンテンツへのパスを指定します。 **Path** とは異なり、**LiteralPath** の値は入力した内容のまま使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、それぞれを単一引用符で囲みます。
単一引用符で囲まれた文字はエスケープ文字として解釈されません。

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

### -Path

暗号化を解除する暗号化されたコンテンツへのパスを指定します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

## 出力

## 注

## 関連リンク

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Protect-CmsMessage](Protect-CmsMessage.md)

[Unprotect-CmsMessage](Unprotect-CmsMessage.md)

