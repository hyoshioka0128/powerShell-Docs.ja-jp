---
description: PowerShell は、エンジン、プロバイダー、およびコマンドレットからの内部操作を Windows イベントログに記録します。
Locale: en-US
ms.date: 03/30/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_logging_windows?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Logging-Windows
ms.openlocfilehash: d6ae50b50911417da8dd306cad69e3fc7129fedc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602597"
---
# <a name="about-logging-windows"></a>ログ記録ウィンドウについて

## <a name="short-description"></a>簡単な説明
PowerShell は、エンジン、プロバイダー、およびコマンドレットからの内部操作を Windows イベントログに記録します。

## <a name="long-description"></a>長い説明

Powershell では、エンジンとプロバイダーの起動と停止、PowerShell コマンドの実行など、PowerShell 操作に関する詳細がログに記録されます。

> [!NOTE]
> Windows PowerShell バージョン3.0、4.0、5.0、および5.1 には、Windows イベントログ用の **EventLog** コマンドレットが含まれています。 これらのバージョンで、 **EventLog** コマンドレットの一覧を表示するには、「」と入力 `Get-Command -Noun EventLog` します。 詳細については、コマンドレットのドキュメントと、使用している Windows PowerShell のバージョンの about_EventLogs を参照してください。

## <a name="viewing-the-powershell-event-log-entries-on-windows"></a>Windows での PowerShell イベントログエントリの表示

PowerShell ログは、Windows イベントビューアーを使用して表示できます。 イベントログは、[アプリケーションとサービスログ] グループにあり、という名前が付けられてい `PowerShellCore` ます。 関連付けられている ETW プロバイダー `GUID` は `{f90714a8-5509-434a-bf6d-b1624c8a19a2}` です。

スクリプトブロックのログ記録を有効にすると、PowerShell によって次のイベントがログに記録され `PowerShellCore/Operational` ます。

|  フィールド  |       値       |
| ------- | ----------------- |
| EventId | `4104` / `0x1008` |
| チャネル | `Operational`     |
| Level   | `Verbose`         |
| オペコード  | `Create`          |
| タスク    | `CommandStart`    |
| キーワード | `Runspace`        |

### <a name="registering-the-powershell-event-provider-on-windows"></a>Windows での PowerShell イベントプロバイダーの登録

Linux または macOS とは異なり、イベントをイベントログに書き込む前に、Windows でイベントプロバイダーを登録する必要があります。 PowerShell イベントプロバイダーを有効にするには、管理者特権の PowerShell プロンプトから次のコマンドを実行します。

```powershell
$PSHOME\RegisterManifest.ps1
```

### <a name="unregistering-the-powershell-event-provider-on-windows"></a>Windows での PowerShell イベントプロバイダーの登録解除

イベントプロバイダーを登録すると、イベントのデコードに使用されるバイナリライブラリにロックが配置されます。 このライブラリを更新するには、このロックを解放するためにプロバイダーの登録を解除する必要があります。

PowerShell プロバイダーの登録を解除するには、管理者特権の PowerShell プロンプトから次のコマンドを実行します。

```powershell
$PSHOME\RegisterManifest.ps1 -Unregister
```

PowerShell を更新した後、を実行し `$PSHOME\RegisterManifest.ps1` て更新されたイベントプロバイダーを登録します。

## <a name="enabling-script-block-logging"></a>スクリプトブロックのログ記録を有効にする

スクリプトブロックのログ記録を有効にすると、PowerShell によって処理されるすべてのスクリプトブロックの内容が記録されます。 有効にすると、新しい PowerShell セッションによってこの情報がログに記録されます。

> [!NOTE]
> 診断以外の目的でスクリプトブロックログを使用する場合は、次に示すように、保護されたイベントログを有効にすることをお勧めします。

スクリプトブロックのログ記録は、グループポリシーまたはレジストリ設定を使用して有効にすることができます。

### <a name="using-group-policy"></a>グループ ポリシーの使用

自動議事録を有効にするには、グループポリシーでこの機能を有効にし `Turn on PowerShell Script Block
Logging` `Administrative Templates -> Windows
Components -> Windows PowerShell` ます。

### <a name="using-the-registry"></a>レジストリの使用

次の関数を実行します。

```powershell
function Enable-PSScriptBlockLogging
{
    $basePath = 'HKLM:\Software\Policies\Microsoft\Windows' +
      '\PowerShell\ScriptBlockLogging'

    if(-not (Test-Path $basePath))
    {
        $null = New-Item $basePath -Force
    }

    Set-ItemProperty $basePath -Name EnableScriptBlockLogging -Value "1"
}
```

## <a name="protected-event-logging"></a>保護されたイベントのログ記録

システムのログ記録レベルを上げると、ログに記録されたコンテンツに機密データが含まれる可能性が高くなります。 たとえば、スクリプトのログ記録を有効にすると、スクリプトによって使用される資格情報やその他の機密データをイベントログに書き込むことができます。 機密データがログに記録されているコンピューターが侵害されると、ログによって、攻撃者は、それらの情報を拡張するために必要な情報を得ることができます。

この情報を保護するために、Windows 10 では保護されたイベントログが導入されています。
保護されたイベントログを使用すると、参加アプリケーションは、イベントログに書き込まれた機密データを暗号化できます。 後で、より安全で集中管理されたログコレクター上で、これらのログの暗号化を解除して処理することができます。

イベントログの内容は、IETF Cryptographic Message 構文 (CMS) 標準を使用して保護されています。 CMS は公開キー暗号化を使用します。 コンテンツの暗号化とコンテンツの暗号化解除に使用されるキーは、個別に保持されます。

公開キーは広く共有でき、機密性の高いデータではありません。 この公開キーで暗号化されたコンテンツは、秘密キーによってのみ復号化できます。 公開キー暗号化の詳細については、「 [Wikipedia-公開キー暗号化](https://en.wikipedia.org/wiki/Public-key_cryptography)」を参照してください。

保護されたイベントログポリシーを有効にするには、保護するイベントログデータがあるすべてのコンピューターに公開キーを展開します。 対応する秘密キーは、中央のイベントログコレクターや [SIEM][] アグリゲーターなどのより安全な場所でイベントログを後処理するために使用されます。 Azure で SIEM を設定することができます。 詳細については、「 [GENERIC SIEM integration](/cloud-app-security/siem)」を参照してください。

### <a name="enabling-protected-event-logging-via-group-policy"></a>グループポリシーを使用した保護されたイベントのログ記録の有効化

保護されたイベントのログ記録を有効にするには、 `Enable Protected Event Logging` グループポリシーで機能を有効に `Administrative Templates -> Windows Components
-> Event Logging` します。 この設定には暗号化証明書が必要です。暗号化証明書は、次のいずれかの形式で指定できます。

- Base-64 でエンコードされた x.509 証明書のコンテンツ ( `Export` 証明書マネージャーのオプションによって提供されるなど)。
- ローカルコンピューターの証明書ストアにある証明書の拇印 (PKI インフラストラクチャで展開できます)。
- 証明書の完全なパス (ローカルまたはリモート共有を指定できます)。
- 証明書または証明書を含むディレクトリへのパス (ローカルまたはリモート共有を指定できます)。
- ローカルコンピューターの証明書ストアで検索できる証明書のサブジェクト名 (PKI インフラストラクチャによって展開できます)。

結果として得られる証明書は、 `Document Encryption` 拡張キー使用法 () として、 `1.3.6.1.4.1.311.80.1` `Data Encipherment` またはまたは `Key
Encipherment` キー使用法が有効になっている必要があります。

> [!WARNING]
> 秘密キーは、イベントのログを記録するマシンにデプロイしないでください。 メッセージの暗号化を解除する安全な場所に保管しておく必要があります。

### <a name="decrypting-protected-event-logging-messages"></a>保護されたイベントログメッセージの復号化

次のスクリプトは、秘密キーがあることを前提として、取得と復号化を行います。

```powershell
Get-WinEvent Microsoft-Windows-PowerShell/Operational |
  Where-Object Id -eq 4104 | Unprotect-CmsMessage
```

## <a name="see-also"></a>関連項目

[about_Logging_Non-Windows](about_Logging_Non-Windows.md)

[Blue Team の PowerShell](https://devblogs.microsoft.com/powershell/powershell-the-blue-team/)

[汎用 SIEM の統合](/cloud-app-security/siem)

<!-- link references -->
[SIEM]: https://wikipedia.org/wiki/Security_information_and_event_management
