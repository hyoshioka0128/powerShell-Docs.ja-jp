---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/02/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/send-mailmessage?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Send-MailMessage
ms.openlocfilehash: 915f0eb0e27bc753db0bd3a5dd4b77dbc24c3e1f
ms.sourcegitcommit: c91f79576bc54e162bcc7adf78026417b2776687
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106274001"
---
# Send-MailMessage

## 構文
電子メールを送信します。

## 構文

### All

```
Send-MailMessage [-To] <string[]> [-Subject] <string> [[-Body] <string>] [[-SmtpServer] <string>]
 -From <string> [-Attachments <string[]>] [-Bcc <string[]>] [-BodyAsHtml] [-Encoding <Encoding>]
 [-Cc <string[]>] [-DeliveryNotificationOption <DeliveryNotificationOptions>]
 [-Priority <MailPriority>] [-Credential <pscredential>] [-UseSsl] [-Port <int>]
 [<CommonParameters>]
```

## 説明

`Send-MailMessage`コマンドレットは、PowerShell 内から電子メールメッセージを送信します。

簡易メール転送プロトコル (SMTP) サーバーを指定する必要があります。指定しない場合、 `Send-MailMessage` コマンドは失敗します。 **Smtpserver** パラメーターを使用するか、 `$PSEmailServer` 変数を有効な SMTP サーバーに設定します。
に割り当てられた値 `$PSEmailServer` は、PowerShell の既定の SMTP 設定です。 詳細については、「 [about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

> [!WARNING]
> `Send-MailMessage`コマンドレットは互換性のために残されています。 このコマンドレットは、SMTP サーバーへのセキュリティで保護された接続を保証しません。 PowerShell で使用できる即時置換はありませんが、を使用しないことをお勧め `Send-MailMessage` します。 詳細については、「 [Platform Compatibility NOTE DE0005](https://aka.ms/SendMailMessage)」を参照してください。

## 例

### 例 1: 1 人のユーザーから別のユーザーに電子メールを送信する

この例では、あるユーザーから別のユーザーに電子メールメッセージを送信します。

**From**、 **To**、および **Subject** の各パラメーターは、によって要求され `Send-MailMessage` ます。 この例では、 `$PSEmailServer` SMTP サーバーの既定の変数を使用するため、 **smtpserver** パラメーターは必要ありません。

```powershell
Send-MailMessage -From 'User01 <user01@fabrikam.com>' -To 'User02 <user02@fabrikam.com>' -Subject 'Test mail'
```

`Send-MailMessage`コマンドレットは、 **From** パラメーターを使用して、メッセージの送信者を指定します。 **To** パラメーターは、メッセージの受信者を指定します。 **Subject** パラメーターは、省略可能な **Body** パラメーターが含まれていないため、テキスト文字列の **テストメール** をメッセージとして使用します。

### 例 2: 添付ファイルを送信する

この例では、添付ファイルを含む電子メールメッセージを送信します。

```powershell
Send-MailMessage -From 'User01 <user01@fabrikam.com>' -To 'User02 <user02@fabrikam.com>', 'User03 <user03@fabrikam.com>' -Subject 'Sending the Attachment' -Body "Forgot to send the attachment. Sending now." -Attachments .\data.csv -Priority High -DeliveryNotificationOption OnSuccess, OnFailure -SmtpServer 'smtp.fabrikam.com'
```

`Send-MailMessage`コマンドレットは、 **From** パラメーターを使用して、メッセージの送信者を指定します。 **To** パラメーターは、メッセージの受信者を指定します。 **Subject** パラメーターは、メッセージの内容を記述します。 **Body** パラメーターは、メッセージの内容です。

**Attachments** パラメーターは、電子メールメッセージに添付されている現在のディレクトリ内のファイルを指定します。 **Priority** パラメーターを指定すると、メッセージは **高** 優先度に設定されます。 **-Deliverynotificationoption** パラメーターには、 **OnSuccess** と **OnFailure** の2つの値が指定されています。 送信側は、メッセージの配信が成功したか失敗したかを確認する電子メール通知を受信します。
**Smtpserver** パラメーターは、SMTP サーバーを **smtp.fabrikam.com** に設定します。

### 例 3: メーリングリストに電子メールを送信する

この例では、メーリングリストに電子メールメッセージを送信します。

```powershell
Send-MailMessage -From 'User01 <user01@fabrikam.com>' -To 'ITGroup <itdept@fabrikam.com>' -Cc 'User02 <user02@fabrikam.com>' -Bcc 'ITMgr <itmgr@fabrikam.com>' -Subject "Don't forget today's meeting!" -Credential domain01\admin01 -UseSsl
```

`Send-MailMessage`コマンドレットは、 **From** パラメーターを使用して、メッセージの送信者を指定します。 **To** パラメーターは、メッセージの受信者を指定します。 **Cc** パラメーターは、指定された受信者にメッセージのコピーを送信します。 **Bcc** パラメーターを指定すると、メッセージのブラインドコピーが送信されます。 ブラインドコピーは、他の受信者には表示されない電子メールアドレスです。 省略可能な **Body** パラメーターは含まれていないため、 **Subject** パラメーターはメッセージです。

**Credential** パラメーターは、メッセージの送信にドメイン管理者の資格情報を使用することを指定します。 **UseSsl** パラメーターは、Secure Socket LAYER (SSL) がセキュリティで保護された接続を作成することを指定します。

## パラメーター

### -添付ファイル

電子メールメッセージに添付するファイルのパスとファイル名を指定します。 このパラメーターを使用するか、パスとファイル名をにパイプすることができ `Send-MailMessage` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: PsPath

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Bcc

メールのコピーを受信するが、メッセージの受信者としては表示されない電子メールアドレスを指定します。 名前 (省略可能) と電子メールアドレス (など) を入力し `Name <someone@fabrikam.com>` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -本文

電子メールメッセージの内容を指定します。

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

### -BodyAsHtml

**Body** パラメーターの値に HTML が含まれていることを指定します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: BAH

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Cc

電子メールメッセージのカーボンコピー (CC) の送信先となる電子メールアドレスを指定します。 名前 (省略可能) と電子メールアドレス (など) を入力し `Name <someone@fabrikam.com>` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力します。 または、コマンドレットのような **PSCredential** オブジェクトを入力し `Get-Credential` ます。

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

### -DeliveryNotificationOption

電子メールメッセージの配信通知オプションを指定します。 複数の値を指定することができます。
既定値は None です。 このパラメーターのエイリアスは **Dno** です。

配信通知は、 **From** パラメーターのアドレスに送信されます。

このパラメーターに指定できる値は次のとおりです。

- `None`: 通知がありません。
- `OnSuccess`: 配信が成功した場合に通知します。
- `OnFailure`: 配信に失敗した場合に通知します。
- `Delay`: 配信が遅延した場合に通知します。
- `Never`: 通知しないでください。

これらの値はフラグベースの列挙体として定義されます。 このパラメーターを使用すると、複数の値を組み合わせて複数のフラグを設定できます。 値は、値の配列として **Deliverynotification** パラメーターに渡すか、それらの値のコンマ区切りの文字列として渡すことができます。 コマンドレットでは、バイナリまたは演算を使用して値を結合します。 配列として値を渡すのが最も簡単なオプションであり、値に対してタブ補完を使用することもできます。

```yaml
Type: System.Net.Mail.DeliveryNotificationOptions
Parameter Sets: (All)
Aliases: DNO
Accepted values: None, OnSuccess, OnFailure, Delay, Never

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Encoding

ターゲット ファイルのエンコードの種類を指定します。 既定値は `Default` です。

このパラメーターに指定できる値は次のとおりです。

- `ASCII` ASCII (7 ビット) 文字セットを使用します。
- `BigEndianUnicode` は、ビッグエンディアンのバイト順で UTF-16 を使用します。
- `Default` は、システムのアクティブなコードページ (通常は ANSI) に対応するエンコーディングを使用します。
- `OEM` は、システムの現在の OEM コードページに対応するエンコーディングを使用します。
- `Unicode` は、リトルエンディアンのバイト順で UTF-16 を使用します。
- `UTF7` UTF-7 を使用します。
- `UTF8` UTF-8 を使用します。
- `UTF32` は、リトルエンディアンのバイト順で 32 UTF-8 を使用します。

```yaml
Type: System.Text.Encoding
Parameter Sets: (All)
Aliases: BE
Accepted values: ASCII, BigEndianUnicode, Default, OEM, Unicode, UTF7, UTF8, UTF32

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
Accept wildcard characters: False
```

### -From

**From** パラメーターが必要です。 このパラメーターは、送信者の電子メールアドレスを指定します。 名前 (省略可能) と電子メールアドレス (など) を入力し `Name <someone@fabrikam.com>` ます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Port

SMTP サーバーの代替ポートを指定します。 既定値は 25 です。これは、既定の SMTP ポートです。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 25
Accept pipeline input: False
Accept wildcard characters: False
```

### -Priority

電子メールメッセージの優先度を指定します。 既定値は Normal です。 このパラメーターに指定できる値は、Normal、High、および Low です。

```yaml
Type: System.Net.Mail.MailPriority
Parameter Sets: (All)
Aliases:
Accepted values: Normal, High, Low

Required: False
Position: Named
Default value: Normal
Accept pipeline input: False
Accept wildcard characters: False
```

### -SmtpServer

電子メールメッセージを送信する SMTP サーバーの名前を指定します。

既定値は、ユーザー設定変数の値です `$PSEmailServer` 。 ユーザー設定変数が設定されておらず、このパラメーターが使用されていない場合、 `Send-MailMessage` コマンドは失敗します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: ComputerName

Required: False
Position: 3
Default value: $PSEmailServer
Accept pipeline input: False
Accept wildcard characters: False
```

### -Subject

**Subject** パラメーターが必要です。 このパラメーターは、電子メールメッセージの件名を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: sub

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -To

**To** パラメーターが必要です。 このパラメーターは、受信者の電子メールアドレスを指定します。 複数の受信者がいる場合は、それらのアドレスをコンマ () で区切り `,` ます。 名前 (省略可能) と電子メールアドレス (など) を入力し `Name <someone@fabrikam.com>` ます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseSsl

リモートコンピューターへのセキュリティで保護された接続を確立してメールを送信するには、Secure Sockets Layer (SSL) プロトコルを使用します。 既定では、SSL は使用されません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、添付ファイルのパスとファイル名をにパイプすることができ `Send-MailMessage` ます。

## 出力

### なし

このコマンドレットは出力を生成しません。

## Notes

## 関連リンク

[about_Preference_Variables](../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)

[Get-Credential](../Microsoft.PowerShell.Security/Get-Credential.md)
