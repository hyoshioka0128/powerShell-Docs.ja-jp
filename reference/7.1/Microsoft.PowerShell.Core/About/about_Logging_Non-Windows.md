---
description: PowerShell は、エンジン、プロバイダー、およびコマンドレットからの内部操作をログに記録します。
keywords: powershell
Locale: en-US
ms.date: 03/30/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_logging_non-windows?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Logging_Non-Windows
ms.openlocfilehash: f70e2cb2c04287e36ecdf21a97dd099fcfd23d65
ms.sourcegitcommit: 39c2a697228276d5dae39e540995fa479c2b5f39
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93355495"
---
# <a name="about-logging-non-windows"></a>Windows 以外のログ記録について

## <a name="short-description"></a>簡単な説明
PowerShell は、エンジン、プロバイダー、およびコマンドレットからの内部操作をログに記録します。

## <a name="long-description"></a>長い説明

Powershell は、PowerShell 操作の詳細をログに記録します。 たとえば、PowerShell では、エンジンの起動や停止、プロバイダーの開始と停止などの操作をログに記録します。 また、PowerShell コマンドに関する詳細もログに記録されます。

PowerShell ログの場所は、ターゲットプラットフォームに依存します。 **Linux** では、PowerShell ログを **syslog** と **rsyslog** に使用できます。 詳細については、Linux コンピューターのローカルページを参照してください `man` 。 **MacOS** では、 **os_log** ログ記録システムが使用されます。 詳細については、 [os_log 開発者向けドキュメント](https://developer.apple.com/documentation/os/os_log)を参照してください。

## <a name="viewing-powershell-log-output-on-linux"></a>Linux での PowerShell ログ出力の表示

Linux の **syslog** の PowerShell ログと、 **syslog** の内容を表示するためによく使用されるすべてのツールを使用できます。

ログエントリの形式は、次のテンプレートを使用します。

```
TIMESTAMP MACHINENAME powershell[PID]: (COMMITID:TID:CID)
  [EVENTID:TASK.OPCODE.LEVEL] MESSAGE
```

|フィールド        |[説明]                                             |
|-------------|--------------------------------------------------------|
|`TIMESTAMP`  |ログエントリが生成された日付/時刻。            |
|`MACHINENAME`|ログが生成されたシステムの名前。      |
|`PID`        |ログエントリを作成したプロセスのプロセス ID。 |
|`COMMITID`   |`git commit`ビルドの生成に使用される ID またはタグ。   |
|`TID`        |ログエントリを書き込んだスレッドのスレッド ID。   |
|`CID`        |ログエントリの16進チャネル識別子。            |
|             |10 = 運用、11 = 分析                         |
|`EVENTID`    |ログエントリのイベント識別子。                  |
|`TASK`       |イベントエントリのタスク識別子                 |
|`OPCODE`     |イベントエントリのオペコード                          |
|`LEVEL`      |イベントエントリのログレベル                       |
|`MESSAGE`    |イベントエントリに関連付けられているメッセージ             |

> [!NOTE]
> `EVENTID`、 `TASK` 、 `OPCODE` 、および `LEVEL` は、Windows イベントログへのログ記録時に使用される値と同じです。

### <a name="filtering-powershell-log-entries-using-rsyslog"></a>Rsyslog を使用して PowerShell ログエントリをフィルター処理する

通常、PowerShell ログエントリは、syslog の既定値に書き込まれ `location/file` ます。 **syslog** ただし、エントリをカスタムファイルにリダイレクトすることはできます。

1. PowerShell ログ構成の conf を作成し、のように 50 (の場合) 未満の数値を指定し `50-default.conf` `40-powershell.conf` ます。 ファイルは、の下に配置する必要があり `/etc/rsyslog.d` ます。

1. 次のエントリをファイルに追加します。

   ```
   :syslogtag, contains, "powershell[" /var/log/powershell.log
   & stop
   ```

1. に新しいファイルが含まれていることを確認してください `/etc/rsyslog.conf` 。 多くの場合、次のような一般的なインクルードステートメントがあります。

   `$IncludeConfig /etc/rsyslog.d/*.conf`

   そうでない場合は、include ステートメントを手動で追加する必要があります。

1. 属性と権限が適切に設定されていることを確認します。

   ```
   -rw-r--r-- 1 root root   67 Nov 28 12:51 40-powershell.conf
   ```

1. 所有権を **ルート** に設定します。

   ```
   chown root:root /etc/rsyslog.d/40-powershell.conf
   ```

1. アクセス許可の設定: **ルート** に読み取り/書き込みがあり、 **ユーザー** が読み取りました。

   ```
   chmod 644 /etc/rsyslog.d/40-powershell.conf
   ```

## <a name="viewing-powershell-log-output-on-macos"></a>MacOS での PowerShell ログ出力の表示

MacOS で PowerShell ログ出力を表示する最も簡単な方法は、 **コンソール** アプリケーションを使用することです。

1. **コンソール** アプリケーションを検索して起動します。
1. [ **デバイス** ] でコンピューター名を選択します。
1. **検索** フィールドに、 `pwsh` PowerShell のメインバイナリとして「」と入力します。
1. 検索フィルターをからに変更し `Any` `Process` ます。
1. 操作を実行します。
1. 必要に応じて、後で使用するために検索を保存します。

**コンソール** で PowerShell の特定のプロセスインスタンスをフィルター処理するには、変数に `$pid` プロセス ID が含まれています。

1. **検索** フィールドに **PID** (プロセス ID) を入力します。
1. 検索フィルターをに変更し `PID` ます。
1. 操作を実行します。

### <a name="viewing-powershell-log-output-from-a-command-line"></a>コマンドラインからの PowerShell ログ出力の表示

コマンドは、 `log` コマンドラインから PowerShell ログエントリを表示するために使用できます。

```
sudo log stream --predicate 'process == "pwsh"' --info
```

### <a name="persisting-powershell-log-output"></a>PowerShell ログ出力の保持

既定では、PowerShell は macOS の既定のメモリのみのログ記録を使用します。 この動作を変更して、コマンドを使用して永続化を有効にすることができ `log config` ます。

次のスクリプトは、情報レベルのログ記録と永続化を有効にします。

```
log config --subsystem com.microsoft.powershell --mode=persist:info,level:info
```

次のコマンドは、PowerShell のログ記録を既定の状態に戻します。

```
log config --subsystem com.microsoft.powershell --mode=persist:default,level:default
```

永続化を有効にした後、コマンドを使用して `log show` ログ項目をエクスポートできます。 このコマンドは、最後の N 個の項目、特定の時刻からの項目、または特定の期間内の項目をエクスポートするためのオプションを提供します。

たとえば、次のコマンドは、から項目をエクスポートし `9am on April 5 of 2018` ます。

```
log show --info --start "2018-04-05 09:00:00" --predicate "process = 'pwsh'"
```

のヘルプを表示するには、を `log` 実行して `log show --help` 追加の詳細を確認します。

> [!TIP]
> PowerShell プロンプトまたはスクリプトから任意のログコマンドを実行するときは、述語文字列全体を二重引用符で囲み、内の単一引用符を使用します。 これにより、述語文字列内で二重引用符文字をエスケープする必要がなくなります。

また、イベントログを中央のイベントログコレクターや [SIEM][] アグリゲーターなどのより安全な場所に保存することも検討してください。 Azure で SIEM を設定することができます。 詳細については、「 [GENERIC SIEM integration](/cloud-app-security/siem)」を参照してください。

## <a name="configuring-logging-on-a-non-windows-system"></a>Windows 以外のシステムでのログ記録の構成

Windows では、ETW トレースリスナーを作成するか、イベントビューアーを使用して分析ログを有効にすることで、ログが構成されます。 Linux と macOS では、ログはファイルを使用して構成され `powershell.config.json` ます。 このセクションの残りの部分では、Windows 以外のシステムでの PowerShell ログの構成について説明します。

既定では、PowerShell によって、操作チャネルへの情報ログが有効になります。 これは、PowerShell によって生成されるログ出力のうち、操作可能としてマークされているものの、情報がログ記録されるログ (トレース) レベルが高いことを意味します。 診断によっては、詳細ログ出力や分析ログ出力の有効化など、追加のログ出力が必要になる場合があります。

ファイル `powershell.config.json` は、PowerShell ディレクトリに存在する **JSON** 形式のファイルです `$PSHOME` 。 PowerShell の各インストールは、このファイルの独自のコピーを使用します。 通常の操作では、このファイルは変更されません。 これは便利な場合がありますが、ファイル内の一部の設定を変更して診断や同じシステム上の複数の PowerShell バージョンを区別したり、同じバージョンの複数のコピーを区別したりすることもでき `LogIdentity` ます (以下の表を参照)。

構成の例を次のコードに示します。

```json
{
  "Microsoft.PowerShell:ExecutionPolicy": "RemoteSigned",
  "PowerShellPolicies": {
    "ScriptExecution": {
      "ExecutionPolicy": "RemoteSigned",
      "EnableScripts": true
    },
    "ScriptBlockLogging": {
      "EnableScriptBlockInvocationLogging": true,
      "EnableScriptBlockLogging": true
    },
    "ModuleLogging": {
      "EnableModuleLogging": false,
      "ModuleNames": [
        "PSReadline",
        "PowerShellGet"
      ]
    },
    "ProtectedEventLogging": {
      "EnableProtectedEventLogging": false,
      "EncryptionCertificate": [
        "Joe"
      ]
    },
    "Transcription": {
      "EnableTranscripting": true,
      "EnableInvocationHeader": true,
      "OutputDirectory": "F:\\tmp\\new"
    },
    "UpdatableHelp": {
      "DefaultSourcePath": "f:\\temp"
    },
    "ConsoleSessionConfiguration": {
      "EnableConsoleSessionConfiguration": false,
      "ConsoleSessionConfigurationName": "name"
    }
  },
  "LogLevel": "verbose"
}
```

次の表に、PowerShell ログを構成するためのプロパティを示します。 などのアスタリスクでマーク `Operational*` された値は、ファイルに値が指定されていない場合の既定値を示します。

|プロパティ   |値        |[説明]                                  |
|-----------|--------------|---------------------------------------------|
|`LogIdentity`|(文字列名) |ログ記録時に使用する名前。 既定では、  |
|           |powershell   |powershell は id です。 この値は次のようになります。|
|           |              |2つの間の違いを示すために使用されます。      |
|           |              |PowerShell インストールのインスタンス ( |
|           |              |リリースおよびベータ版として。 この値は次のようになります。 |
|           |              |ログ出力をにリダイレクトするためにも使用されます。        |
|           |              |Linux 上の個別のファイル。 「」の説明を参照してください。|
|           |              |上記の **rsyslog** 。                           |
|`LogChannels`|操作  |有効にするチャネル。 値を分離する|
|           |分析      |複数指定する場合は、コンマで区切ります。  |
|`LogLevel`   |Always        |1つの値を指定します。 この値により、  |
|           |Critical      |それより上のすべての値と        |
|           |エラー         |左に一覧表示します。                            |
|           |警告       |                                             |
|           |専用|                                             |
|           |"詳細"       |                                             |
|           |デバッグ         |                                             |
|`LogKeywords`|Runspace      |キーワードを使用すると、ログ記録を制限することができます。|
|           |パイプライン      |PowerShell 内の特定のコンポーネントに対して。 By |
|           |Protocol      |既定では、すべてのキーワードが有効になり、変更されます。 |
|           |トランスポート     |この値は非常に便利です。           |
|           |Host          |特別なトラブルシューティング。                |
|           |コマンドレット       |                                             |
|           |シリアライザー    |                                             |
|           |Session       |                                             |
|           |ManagedPlugin |                                             |

## <a name="see-also"></a>関連項目

Linux の **syslog** と **rsyslog** の情報については、linux コンピューターのローカルページを参照してください `man` 。

MacOS の **os_log** 情報については、 [os_log 開発者向けドキュメント](https://developer.apple.com/documentation/os/os_log)を参照してください。

[about_Logging_Windows](about_Logging_Windows.md)

[汎用 SIEM の統合](/cloud-app-security/siem)

<!-- link references -->
[SIEM]: https://wikipedia.org/wiki/Security_information_and_event_management
