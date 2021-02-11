---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/new-wsmansessionoption?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-WSManSessionOption
ms.openlocfilehash: e7d7f132eb82dc1a709a88cbdef525aa83d867e4
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603032"
---
# New-WSManSessionOption

## 概要
WS-Management コマンドレットの入力パラメーターとして使用するセッションオプションハッシュテーブルを作成します。

## SYNTAX

```
New-WSManSessionOption [-ProxyAccessType <ProxyAccessType>] [-ProxyAuthentication <ProxyAuthentication>]
 [-ProxyCredential <PSCredential>] [-SkipCACheck] [-SkipCNCheck] [-SkipRevocationCheck] [-SPNPort <Int32>]
 [-OperationTimeout <Int32>] [-NoEncryption] [-UseUTF16] [<CommonParameters>]
```

## Description
**新しい-WSManSessionOption** コマンドレットは、wsman セッションオプションハッシュテーブルを作成します。これは wsman コマンドレットに渡すことができます。

- Get-WSManInstance
- Set-WSManInstance
- Invoke-WSManAction
- Connect-WSMan

## 例

### 例 1: 接続オプションを使用する接続を作成する

```
PS C:\> $a = New-WSManSessionOption -OperationTimeout 30000
PS C:\> Connect-WSMan -ComputerName "server01" -SessionOption $a
PS C:\> cd wsman:
PS WSMan:\>
PS WSMan:\> dir
WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan
ComputerName                                  Type
------------                                  ----
localhost                                     Container
server01                                      Container
```

この例では、 **server01 によっ** て定義された接続オプションを使用して、リモートコンピューターへの接続を作成します。

最初のコマンドは、 **New-WSManSessionOption** を使用して、一連の接続設定オプションを $a 変数に格納します。
この場合は、セッション オプションで接続のタイムアウトを 30 秒 (30,000 ミリ秒) に設定します。

2番目のコマンドは、 *Sessionoption* パラメーターを使用して、$a 変数に格納されている資格情報を **接続 WSMan** に渡します。
次に、指定されたセッションオプションを使用して、リモートの server01 コンピューターに **接続** します。

**接続-wsman** は、通常、リモートコンピューター (この場合は server01 コンピューター) に接続するために wsman プロバイダーのコンテキストで使用されます。
ただし、WSMan プロバイダーに変更する前に、このコマンドレットを使用して、リモート コンピューターへの接続を確立することができます。
これらの接続は、 **ComputerName** の一覧に表示されます。

## PARAMETERS

### -NoEncryption
接続が HTTP 経由のリモート操作に暗号化を使用しないことを示します。

既定では、暗号化されていないトラフィックは有効になっていません。
ローカル構成で有効になっている必要があります。

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

### -OperationTimeout
WS-Management 操作のタイムアウト (ミリ秒単位) を指定します。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: OperationTimeoutMSec

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -System.management.automation.remoting.proxyaccesstype
プロキシ サーバーを検索するメカニズムを指定します。
このパラメーターの有効値は、次のとおりです。

- ProxyIEConfig.
現在のユーザーに対して Internet Explorer のプロキシ構成を使用します。
- ProxyWinHttpConfig.
WSMan クライアントは、ProxyCfg.exe ユーティリティを使用して、WinHTTP 用に構成されたプロキシ設定を使用します。
- ProxyAutoDetect.
プロキシサーバーの自動検出を強制します。
- ProxyNoProxyServer.
プロキシサーバーは使用しないでください。
すべてのホスト名をローカルで解決してください。

既定値は ProxyIEConfig です。

```yaml
Type: Microsoft.WSMan.Management.ProxyAccessType
Parameter Sets: (All)
Aliases:
Accepted values: ProxyIEConfig, ProxyWinHttpConfig, ProxyAutoDetect, ProxyNoProxyServer

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyAuthentication
プロキシで使用する認証方法を指定します。
このパラメーターの有効値は、次のとおりです。

- 基本。
Basic は、ユーザー名およびパスワードがクリア テキストでサーバーまたはプロキシに送信されるスキームです。
- ダイジェスト。
Digest は、サーバーで指定されたデータ文字列をチャレンジで使用するチャレンジ/レスポンス スキームです。
- Negotiate
Negotiate は、認証で使用するスキームを特定するために、サーバーまたはプロキシとネゴシエートするチャレンジ/レスポンス スキームです。
たとえば、Kerberos プロトコルや NTLM です。

既定値は Negotiate です。

```yaml
Type: Microsoft.WSMan.Management.ProxyAuthentication
Parameter Sets: (All)
Aliases:
Accepted values: Negotiate, Basic, Digest

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyCredential
中間 Web プロキシ経由でアクセスするためのアクセス許可を持つユーザーアカウントを指定します。

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

### -SkipCACheck
HTTPS 経由で接続する場合、クライアントは、サーバー証明書が信頼された証明機関 (CA) によって署名されているかどうかを検証しません。
このオプションは、リモートコンピューターが別の方法で信頼されている場合にのみ使用します。たとえば、リモートコンピューターが物理的に安全で分離されているネットワークの一部である場合や、リモートコンピューターが WS-Management 構成で信頼されたホストとして一覧表示されている場合などです。

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

### -SkipCNCheck
サーバーの証明書の共通名 (CN) がサーバーのホスト名と一致する必要がないことを指定します。
これは、HTTPS を使用したリモート操作でのみ使用します。
このオプションは、信頼されているコンピューターに対してのみ使用してください。

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

### -SkipRevocationCheck
接続がサーバー証明書の失効状態を検証しないことを示します。

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

### -SPNPort
リモートサーバーの接続サービスプリンシパル名 (SPN) に追加するポート番号を指定します。
SPN は、認証メカニズムが Kerberos またはネゴシエートの場合に使用されます。

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

### -UseUTF16
接続が、要求を UTF8 形式ではなく UTF16 形式でエンコードすることを示します。
既定値は UTF8 エンコードです。

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

## 出力

### SessionOption

## 注

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Disconnect-WSMan](Disconnect-WSMan.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManInstance](New-WSManInstance.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

