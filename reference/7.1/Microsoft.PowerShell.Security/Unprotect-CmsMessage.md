---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 02/03/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/unprotect-cmsmessage?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Unprotect-CmsMessage
ms.openlocfilehash: 5897c792b16ce0b3a98e9e3cd0f1dea52cac2afd
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346379"
---
# Unprotect-CmsMessage

## 概要
暗号化メッセージの構文形式を使用して暗号化されたコンテンツを復号化します。

## SYNTAX

### ByWinEvent (既定値)

```
Unprotect-CmsMessage [-EventLogRecord] <PSObject> [-IncludeContext] [[-To] <CmsMessageRecipient[]>]
 [<CommonParameters>]
```

### ByContent

```
Unprotect-CmsMessage [-Content] <String> [-IncludeContext] [[-To] <CmsMessageRecipient[]>] [<CommonParameters>]
```

### ByPath

```
Unprotect-CmsMessage [-Path] <String> [-IncludeContext] [[-To] <CmsMessageRecipient[]>] [<CommonParameters>]
```

### ByLiteralPath

```
Unprotect-CmsMessage [-LiteralPath] <String> [-IncludeContext] [[-To] <CmsMessageRecipient[]>]
 [<CommonParameters>]
```

## Description

この `Unprotect-CmsMessage` コマンドレットは、暗号化メッセージ構文 (CMS) 形式を使用して暗号化されたコンテンツを復号化します。

CMS コマンドレットは、 [RFC5652](https://tools.ietf.org/html/rfc5652)によって説明されているように、メッセージを暗号化して保護するための IETF 標準形式を使用したコンテンツの暗号化と暗号化解除をサポートします。

CMS 暗号化標準では、公開キー暗号化を使用します。この暗号化では、コンテンツの暗号化に使用されるキー (公開キー) とコンテンツの暗号化を解除するために使用されるキー (秘密キー) は区別されます。 公開キーは広く共有でき、機密性の高いデータではありません。 いずれかのコンテンツがこの公開キーで暗号化された場合、秘密キーのみが暗号化を解除できます。 公開キー暗号化の詳細については、「[公開鍵暗号](https://en.wikipedia.org/wiki/Public-key_cryptography)」を参照してください。

`Unprotect-CmsMessage` CMS 形式で暗号化されたコンテンツを復号化します。 このコマンドレットを実行すると、コマンドレットを実行して暗号化したコンテンツの暗号化を解除でき `Protect-CmsMessage` ます。 暗号化を解除するコンテンツを文字列として指定したり、暗号化イベントログの ID 番号を使用したり、暗号化されたコンテンツへのパスを指定したりすることができます。 コマンドレットにより、復号化され `Unprotect-CmsMessage` たコンテンツが返されます。

PowerShell 7.1 では、Linux と macOS のサポートが追加されました。

## 例

### 例 1: メッセージの暗号化を解除する

次の例では、リテラルパスにあるコンテンツの暗号化を解除し `C:\Users\Test\Documents\PowerShell` ます。 [ **必須]** パラメーターの値の場合、この例では、暗号化の実行に使用された証明書のサムプリントを使用します。 暗号化が解除されたことを示します。 "新しい Break All コマンドを試してください" というメッセージが表示されます。

```powershell
$parameters = @{
  LiteralPath = "C:\Users\Test\Documents\PowerShell\Future_Plans.txt"
  To = '0f 8j b1 ab e0 ce 35 1d 67 d2 f2 6f a2 d2 00 cl 22 z9 m9 85'
}
Unprotect-CmsMessage -LiteralPath @parameters
```

```Output
Try the new Break All command
```

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
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -EventLogRecord

CMS 暗号化操作を表すイベントログレコード ID を指定します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: ByWinEvent
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -IncludeContext

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

### -LiteralPath

暗号化を解除する暗号化されたコンテンツへのパスを指定します。 **Path** とは異なり、 **LiteralPath** の値は入力した内容のまま使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases:

Required: True
Position: 0
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
Position: 0
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

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### EventLogRecord または System.string を指定します。

パイプを使用して、暗号化されたコンテンツを含むオブジェクトをにパイプすることができ `Unprotect-CmsMessage` ます。

## 出力

### System.String

暗号化されていないメッセージ。

## 注

## 関連リンク

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Get-CmsMessage](Get-CmsMessage.md)

[Protect-CmsMessage](Protect-CmsMessage.md)
