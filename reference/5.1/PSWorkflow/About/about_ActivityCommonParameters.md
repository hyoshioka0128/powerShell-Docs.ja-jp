---
description: Windows PowerShell ワークフローによってアクティビティに追加されるパラメーターについて説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/psworkflow/about/about_activitycommonparameters?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_ActivityCommonParameters
ms.openlocfilehash: 93fdcdb9c5afe0b73e843baf2474ec7d3f96a6cf
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94387806"
---
# <a name="about-activitycommonparameters"></a>ActivityCommonParameters について

## <a name="short-description"></a>概要

Windows PowerShell ワークフローによってアクティビティに追加されるパラメーターについて説明します。

## <a name="long-description"></a>詳細説明

Windows PowerShell ワークフローは、PSActivity 基本クラスから派生したアクティビティにアクティビティ共通パラメーターを追加します。 このカテゴリには、アクティビティとして実装されている InlineScript アクティビティおよび Windows PowerShell コマンドレットが含まれます。たとえば、Get-Process や Get-WinEvent などです。

アクティビティ共通パラメーターは Suspend-Workflow および Checkpoint-Workflow アクティビティでは無効であり、InlineScript スクリプトブロックまたは同様のアクティビティで Windows PowerShell ワークフローが自動的に実行するコマンドレットや式には追加されません。 アクティビティ共通パラメーターは、InlineScript アクティビティでは使用できますが、InlineScript スクリプト ブロックのコマンドには使用できません。

アクティビティ共通パラメーターのいくつかは、ワークフロー共通パラメーターまたは Windows PowerShell 共通パラメーターでもあります。 その他のアクティビティ共通パラメーターは、アクティビティに固有です。

ワークフロー共通パラメーターについては、「about_WorkflowCommonParameters」を参照してください。 Windows PowerShell の共通パラメーターの詳細については、「about_CommonParameters」を参照してください。

### <a name="list-of-activity-common-parameters"></a>アクティビティ共通パラメーターの一覧

```
AppendOutput                      PSDebug
Debug                             PSDisableSerialization
DisplayName                       PSDisableSerializationPreference
ErrorAction                       PSError
Input                             PSPersist
MergeErrorToOutput                PSPort
PSActionRetryCount                PSProgress
PSActionRetryIntervalSec          PSProgressMessage
PSActionRunningTimeoutSec         PSRemotingBehavior
PSApplicationName                 PSRequiredModules
PSAuthentication                  PSSessionOption
PSCertificateThumbprint           PSUseSSL
PSComputerName                    PSVerbose
PSConfigurationName               PSWarning
PSConnectionRetryCount            Result
PSConnectionRetryIntervalSec      UseDefaultInput
PSConnectionURI                   Verbose
PSCredential                      WarningAction
```

### <a name="parameter-descriptions"></a>パラメーターの説明

ここでは、アクティビティ共通パラメーターについて説明します。

#### <a name="appendoutput-boolean"></a>AppendOutput \<Boolean\>

値をにすると、 `$True` アクティビティの出力が変数の値に追加されます。
値をに `$False` しても効果はありません。 既定では、変数に値を代入することで変数の値が置き換わります。

たとえば、次のコマンドは、変数内のサービスオブジェクトにプロセスオブジェクトを追加し `$x` ます。

```powershell
Workflow Test-Workflow
{
    $x = Get-Service
    $x = Get-Process -AppendOutput $true
}
```

このパラメーターは、XAML ベースのワークフロー用に設計されています。 スクリプトワークフローでは、次の例に示すように、+ = 代入演算子を使用して、出力を変数の値に追加することもできます。

```powershell
Workflow Test-Workflow
{
    $x = Get-Service
    $x += Get-Process
}
```

#### <a name="debug-switchparameter"></a>デバック \<SwitchParameter\>

コマンドによって実行される操作に関するプログラマレベルの詳細を表示します。
Debug パラメーターは、現在のコマンドの $DebugPreference 変数の値よりも優先されます。 このパラメーターは、コマンドによってデバッグメッセージが生成される場合にのみ機能します。 このパラメーターは、Windows PowerShell 共通パラメーターでもあります。

#### <a name="displayname-string"></a>名 \<String\>

アクティビティのフレンドリ名を指定します。 DisplayName の値は、ワークフローの実行中に進行状況バーに表示され、ワークフロージョブの Progress プロパティの値に表示されます。 Psprogress Message パラメーターもコマンドに含まれている場合、進行状況バーの内容は次の形式で表示され \<DisplayName\> \<PSProgressMessage\> ます。

#### <a name="erroraction-actionpreference"></a>ErrorAction \<ActionPreference\>

コマンドからの終了しないエラーに対してアクティビティがどのように応答するかを決定します。 終了エラーには影響しません。 このパラメーターは、コマンドが Write-Error コマンドレットなどの終了しないエラーを生成した場合にのみ機能します。 ErrorAction パラメーターは、現在のコマンドの $ErrorActionPreference 変数の値よりも優先されます。 このパラメーターは、Windows PowerShell 共通パラメーターでもあります。

有効な値:

- まま. エラーメッセージを表示し、コマンドの実行を続行します。 "Continue" が既定値です。

- 無視。 エラーメッセージを表示せずに、コマンドの実行を続行します。
  SilentlyContinue とは異なり、Ignore では、エラーメッセージが $Error 自動変数に追加されません。 Ignore 値は、Windows PowerShell 3.0 で導入されました。

- Gina. エラーメッセージを表示し、実行を続行する前に確認メッセージを表示します。 この値はほとんど使用されません。

- 中断 :  では、ワークフロージョブが自動的に中断され、さらに調査できるようになります。 調査後、ワークフローを再開できます。

- SilentlyContinue. エラーメッセージを表示せずに、コマンドの実行を続行します。

- 停止。 エラーメッセージを表示し、コマンドの実行を停止します。

#### <a name="input-object"></a>代入 \<Object[]\>

オブジェクトのコレクションをアクティビティに送信します。 これは、オブジェクトを 1 つずつアクティビティにパイプ処理することに代わるものです。

#### <a name="mergeerrortooutput-boolean"></a>MergeErrorToOutput \<Boolean\>

値をにすると、 `$True` エラーが出力ストリームに追加されます。 の値は無効 `$False` です。 このパラメーターを Parallel およびキーワードと共に使用して、 `ForEach -Parallel` 1 つのコレクション内の複数の並列コマンドからのエラーと出力を収集します。

#### <a name="psactionretrycount-int32"></a>PSActionRetryCount \<Int32\>

最初の試行が失敗した場合に、そのアクティビティの実行を繰り返し試みます。 既定値は 0 で、再試行は行いません。

#### <a name="psactionretryintervalsec-int32"></a>PSActionRetryIntervalSec \<Int32\>

操作の再試行の間隔を秒単位で指定します。 既定値は 0 で、操作をすぐに再試行します。 このパラメーターは、PSActionRetryCount パラメーターがコマンドでも使用されている場合にのみ有効です。

#### <a name="psactionrunningtimeoutsec-int32"></a>PSActionRunningTimeoutSec \<Int32\>

アクティビティをそれぞれのターゲット コンピューターでどれくらいの時間にわたって実行できるかを指定します。 タイムアウトの期限が切れる前にアクティビティが完了しない場合、Windows PowerShell ワークフローは終了エラーを生成し、影響を受けるターゲットコンピューターでのワークフローの処理を停止します。

#### <a name="psallowredirection-boolean"></a>PSAllowRedirection \<Boolean\>

値 $True を指定すると、ターゲットコンピューターへの接続をリダイレクトできます。
$False の値は無効です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

PSConnectionURI パラメーターを使用すると、リモートの送信先は別の URI にリダイレクトする命令を返すことができます。 既定では、Windows PowerShell は接続をリダイレクトしませんが、PSAllowRedirection パラメーターを $True の値と共に使用して、ターゲットコンピューターへの接続のリダイレクトを許可することができます。

また、ユーザー設定変数の MaximumConnectionRedirectionCount プロパティを設定することによって、 `$PSSessionOption` またはセッションを作成するコマンドレットの SSessionOption パラメーターの値の MaximumConnectionRedirectionCount プロパティを設定することによって、接続をリダイレクトする回数を制限することもできます。 既定値は 5 です。

#### <a name="psapplicationname-string"></a>PSApplicationName \<String\>

ターゲットコンピューターへの接続に使用される接続 URI のアプリケーション名セグメントを指定します。 コマンドで ConnectionURI パラメーターを使用しない場合は、このパラメーターを使用してアプリケーション名を指定します。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

既定値は、対象のコンピューターのユーザー設定変数の値です `$PSSessionApplicationName` 。 このユーザー設定変数が定義されていない場合、既定値は WSMAN です。 この値はほとんどの用途に適しています。 詳細については、「 [about_Preference_Variables](../../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

WinRM サービスは、アプリケーション名を使用して、接続要求を処理するリスナーを選択します。 このパラメーターの値は、リモート コンピューター上のリスナーの URLPrefix プロパティの値に一致している必要があります。

#### <a name="psauthentication-authenticationmechanism"></a>PSAuthentication \<AuthenticationMechanism\>

対象のコンピュータに接続するときに、ユーザーの資格情報を認証するために使用されるメカニズムを指定します。 有効な値は、Default、Basic、Credssp、Digest、Kerberos、Negotiate、および NegotiateWithImplicitCredential です。 既定値は Default です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

このパラメーターの値の詳細については、PowerShell SDK の「 **システムの管理** 」列挙型の説明を参照してください。

> [!WARNING]
> ユーザーの資格情報が認証対象のリモート コンピューターに渡される Credential Security Service Provider (CredSSP) 認証は、リモート ネットワーク共有にアクセスする場合など、複数のリソースの認証を必要とするコマンドを対象としています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

#### <a name="pscertificatethumbprint-string"></a>PSCertificateThumbprint \<String\>

この処理を実行するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書を取得するには、Windows PowerShell Cert: ドライブで [Get Item](xref:Microsoft.PowerShell.Management.Get-Item) または [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem) コマンドレットを使用します。

#### <a name="pscomputername-string"></a>PSComputerName \<String[]\>

アクティビティを実行する対象のコンピューターを指定します。 既定値はローカル コンピューターです。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

1 台または複数のコンピューターの NETBIOS 名、IP アドレス、または完全修飾ドメイン名をコンマ区切りリストで入力します。 ローカルのコンピューターを指定するには、コンピューター名、「localhost」、またはドット (.) を入力します。

PSComputerName パラメーターの値にローカルコンピューターを含めるには、[管理者として実行] オプションを使用して Windows PowerShell を開きます。

コマンドでこのパラメーターを省略した場合、または値が $null または空の文字列の場合、ワークフローターゲットはローカルコンピューターであり、Windows PowerShell リモート処理はコマンドの実行には使用されません。

ComputerName パラメーターの値に IP アドレスを使用するには、コマンドに PSCredential パラメーターを含める必要があります。 また、HTTPS トランスポート用にコンピューターを構成するか、リモート コンピューターの IP アドレスをローカル コンピューター上の WinRM TrustedHosts 一覧に含める必要があります。 TrustedHosts の一覧にコンピューター名を追加する手順については、 [about_Remote_Troubleshooting](../../Microsoft.PowerShell.Core/About/about_Remote_Troubleshooting.md)の「信頼されたホストの一覧にコンピューターを追加する方法」を参照してください。

#### <a name="psconfigurationname-string"></a>PSConfigurationName \<String\>

対象のコンピューターでセッションを作成するために使用されるセッション構成を指定します。 ワークフローを実行しているコンピューターではなく、対象のコンピューターにセッション構成の名前を入力します。 既定値は、Microsoft. PowerShell です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

#### <a name="psconnectionretrycount-uint"></a>PSConnectionRetryCount \<UInt\>

最初の接続試行が失敗した場合に、各ターゲットコンピュータへの接続試行回数の最大値を指定します。 1から 4294967295 (UInt) までの数値を入力します。 既定値のゼロ (0) は、再試行の回数を表します。
このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

#### <a name="psconnectionretryintervalsec-uint"></a>PSConnectionRetryIntervalSec \<UInt\>

接続再試行の間隔を秒単位で指定します。 既定値は 0 です。 このパラメーターは、PSConnectionRetryCount の値が1以上の場合にのみ有効です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

#### <a name="psconnectionuri-systemuri"></a>PSConnectionURI \<System.Uri\>

ターゲットコンピューター上のアクティビティの接続エンドポイントを定義する Uniform Resource Identifier (URI) を指定します。 URI は完全修飾名にする必要があります。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

この文字列の形式は次のとおりです。

```
<Transport>://<ComputerName>:<Port>/<ApplicationName>
```

既定値は `http://localhost:5985/WSMAN` です。

PSConnectionURI を指定しない場合は、PSUseSSL、PSComputerName、PSPort、および PSApplicationName パラメーターを使用して PSConnectionURI 値を指定できます。

URI のトランスポート セグメントの有効な値は、HTTP および HTTPS です。 トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合、セッションは標準ポート80で HTTP 用および 443 (HTTPS 用) で作成されます。 Windows PowerShell リモート処理用の既定のポートを使用するには、HTTP の場合は 5985、HTTPS の場合は 5986 を指定します。

#### <a name="pscredential-pscredential"></a>PSCredential \<PSCredential\>

対象のコンピューターでアクティビティを実行するアクセス許可を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。 このパラメーターは、PSComputerName パラメーターがコマンドに含まれている場合にのみ有効です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

"User01" や "Domain01\user01」" などのユーザー名を入力するか、Get-Credential コマンドレットから返されるような PSCredential オブジェクトを含む変数を入力します。 ユーザー名のみを入力すると、パスワードの入力を促すメッセージが表示されます。

#### <a name="psdebug-psdatacollectiondebugrecord"></a>Set-psdebug \<PSDataCollection[DebugRecord]\>

デバッグメッセージをコンソールに書き込むのではなく、指定されたデバッグレコードコレクションに追加します。また、ワークフロージョブの Debug プロパティの値にも追加します。 複数のアクティビティからのデバッグ メッセージを、同じデバッグ レコード コレクション オブジェクトに追加できます。

このアクティビティ共通パラメーターを使用するには、New-Object コマンドレットを使用して、DebugRecord 型の PSDataCollection オブジェクトを作成し、そのオブジェクトを変数に保存します。 その後、次の例に示すように、1つ以上のアクティビティの PSDebug パラメーターの値として変数を使用します。

```powershell
Workflow Test-Workflow
{
    $debugCollection = New-Object -Type `
    System.Management.Automation.PSDataCollection[System.Management.Automation.DebugRecord]
    InlineScript {\Server01\Share01\Get-AssetData.ps1} -PSDebug $debugCollection -Debug $True
    InlineScript {\Server01\Share01\Set-AssetData.ps1} -PSDebug $debugCollection -Debug $True
    if ($debugCollection -like "Missing") { ...}
}
```

#### <a name="psdisableserialization-boolean"></a>PSDisableSerialization 場合 \<Boolean\>

"ライブ" (シリアル化されていない) オブジェクトをワークフローに返すことをアクティビティに指示します。
結果として得ることができるオブジェクトには、メソッドと共にプロパティがありますが、チェックポイントの取得時に保存することはできません。

#### <a name="psdisableserializationpreference-boolean"></a>PSDisableSerializationPreference \<Boolean\>

PSDisableSerialization パラメーターに相当するものを、アクティビティだけでなく、ワークフロー全体に適用します。 オブジェクトをシリアル化しないワークフローは再開または永続化できないため、通常、このパラメーターを追加することは推奨されません。

有効な値:

- 標準省略した場合、PSDisableSerialization パラメーターをアクティビティに追加していないと、オブジェクトがシリアル化されます。

- `$True`. ワークフロー内のすべてのアクティビティに、"ライブ" (シリアル化されていない) オブジェクトを返すよう指示します。 結果として得ることができるオブジェクトには、メソッドと共にプロパティがありますが、チェックポイントの取得時に保存することはできません。
- `$False`. ワークフロー オブジェクトがシリアル化されます。

#### <a name="pserror-psdatacollectionerrorrecord"></a>PSError \<PSDataCollection[ErrorRecord]\>

エラーメッセージをコンソールやワークフロージョブの Error プロパティの値に書き込む代わりに、アクティビティからのエラーメッセージを、指定したエラーレコードコレクションに追加します。 複数のアクティビティからのエラー メッセージを、同じエラー レコード コレクション オブジェクトに追加できます。

このアクティビティ共通パラメーターを使用するには、コマンドレットを使用して、 `New-Object` ErrorRecord 型の PSDataCollection オブジェクトを作成し、そのオブジェクトを変数に保存します。 次に、次の例に示すように、1つ以上のアクティビティの PSError パラメーターの値として変数を使用します。

```powershell
Workflow Test-Workflow
{
   $typeName = "System.Management.Automation.PSDataCollection"
   $typeName += '[System.Management.Automation.ErrorRecord]'
   $ec = New-Object $typeName
   InlineScript {\Server01\Share01\Get-AssetData.ps1} -PSError $ec
   InlineScript {\Server01\Share01\Set-AssetData.ps1} -PSError $ec
   if ($ec.Count -gt 2)
   {
      # Do Some Work.
   }
}
```

#### <a name="pspersist-boolean"></a>PSPersist \<Boolean\>

は、アクティビティの後にチェックポイントを取得します。 このチェックポイントは、ワークフローで指定されているすべてのチェックポイントに追加されます。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

"チェックポイント" または "永続化ポイント" とは、ワークフローの実行中にキャプチャされ、ディスク上の永続化ストアに保存される、ワークフローの状態とデータのスナップショットです。 Windows PowerShell ワークフローでは、保存されたデータを使用して、ワークフローを再開するのではなく、最後の永続化ポイントから中断または中断されたワークフローを再開します。

有効な値:

- 標準このパラメーターを省略すると、チェックポイントは追加されません。 チェックポイントは、ワークフローの設定に基づいて作成されます。

- `$True`. アクティビティの完了後に、チェックポイントを取得します。
  このチェックポイントは、ワークフローで指定されているすべてのチェックポイントに追加されます。

- `$False`. チェックポイントは追加されません。 チェックポイントは、ワークフローで指定されている場合にのみ取得されます。

#### <a name="psport-int32"></a>PSPort \<Int32\>

ターゲットコンピューターのネットワークポートを指定します。 既定のポート番号は 5985 (HTTP 用の WinRM ポート) と 5986 (HTTPS 用の WinRM ポート) です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

必要な場合を除き、PSPort パラメーターは使用しないでください。 コマンドで設定されたポートは、コマンドを実行するすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。 代替ポートを使用する前に、そのポートでリッスンするようにリモート コンピューター上の WinRM リスナーを構成する必要があります。

#### <a name="psprogress-psdatacollectionprogressrecord"></a>PSProgress \<PSDataCollection[ProgressRecord]\>

進行状況メッセージをコンソールに書き込むのではなく、アクティビティからの進行状況メッセージを、ワークフロージョブの progress プロパティの値に追加します。 複数のアクティビティからの進行状況メッセージを、同じ進行状況レコード コレクション オブジェクトに追加できます。

#### <a name="psprogressmessage-string"></a>Ps進捗メッセージ \<String\>

アクティビティに関するわかりやすい説明を指定します。 Psprogress メッセージ値は、ワークフローの実行中に進行状況バーに表示されます。 DisplayName がコマンドにも含まれている場合、進行状況バーの内容は次の形式で表示され \<DisplayName\> \<PSProgressMessage\> ます。

このパラメーターは、ForEach-Parallel スクリプトブロック内のアクティビティを識別する場合に特に便利です。 このメッセージがない場合、すべての並列分岐内のアクティビティは、同じ名前で識別されます。

#### <a name="psremotingbehavior-remotingbehavior"></a>PSRemotingBehavior \<RemotingBehavior\>

ターゲット コンピューターでのアクティビティの実行時にリモート処理を管理する方法を指定します。
PowerShell が既定値です。

有効な値は次のとおりです。

- [なし]: このアクティビティは、リモートコンピューターでは実行されません。

- PowerShell: Windows PowerShell リモート処理は、対象のコンピューターでアクティビティを実行するために使用されます。

- カスタム: アクティビティは、独自の種類のリモート処理をサポートします。 この値は、アクティビティとして実装されているコマンドレットが RemotingCapability ビリティ属性の値を SupportedByCommand に設定し、コマンドに ComputerName パラメーターが含まれている場合に有効です。

#### <a name="psrequiredmodules-string"></a>PSRequiredModules \<String[]\>

コマンドの実行前に、指定されたモジュールをインポートします。 モジュール名を入力します。 モジュールはターゲットコンピューターにインストールする必要があります。

PSModulePath 環境変数で指定されたパスにインストールされているモジュールは、モジュール内のコマンドを初めて使用するときに自動的にインポートされます。
PSModulePath の場所にないモジュールをインポートするには、このパラメーターを使用します。

ワークフロー内の各アクティビティは独自のセッションで実行されるため、Import-Module コマンドは、アクティビティが実行されるセッションにのみモジュールをインポートします。 その他のアクティビティが実行されるセッションにはモジュールはインポートされません。

#### <a name="pssessionoption-pssessionoption"></a>PSSessionOption \<PSSessionOption\>

セッションの詳細オプションを対象のコンピューターに設定します。 New-PSSessionOption コマンドレットを使用して作成したものなど、PSSessionOption オブジェクトを入力します。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

セッションオプションの既定値は、設定されている場合は、ユーザー設定変数の値によって決まり `$PSSessionOption` ます。 それ以外の場合は、セッション構成で指定された値が使用されます。

既定値を含むセッションオプションの説明については、New-PSSessionOption コマンドレットの [New-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)のヘルプトピックを参照してください。

$PSSessionOption ユーザー設定変数の詳細については、「 [about_Preference_Variables](../../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

#### <a name="psusessl-boolean"></a>PSUseSSL \<Boolean\>

$True の値は、Secure Sockets Layer (SSL) プロトコルを使用して、対象のコンピューターへの接続を確立します。 既定では、SSL は使用されません。 $False の値は無効です。 このアクティビティ共通パラメーターは、ワークフロー共通パラメーターでもあります。

WS-Management は、ネットワークを介して転送されるすべての Windows PowerShell コンテンツを暗号化します。 UseSSL は、HTTP 経由ではなく HTTPS 経由でデータを送信するために追加された保護機能です。 コマンドに使用するポートで SSL が使用できない場合に、このパラメーターを指定すると、コマンドは失敗します。

#### <a name="psverbose-psdatacollectionverboserecord"></a>PSVerbose \<PSDataCollection[VerboseRecord]\>

詳細メッセージをコンソールに書き込むのではなく、アクティビティからの詳細メッセージを指定した詳細レコードコレクションに追加します。または、ワークフロージョブの Verbose プロパティの値にも追加します。 複数のアクティビティからの詳細メッセージを、同じ詳細レコード コレクション オブジェクトに追加できます。

#### <a name="pswarning-psdatacollectionwarningrecord"></a>PSWarning \<PSDataCollection[WarningRecord]\>

警告メッセージをコンソールに書き込むのではなく、指定した警告レコードコレクションに警告メッセージを追加します。また、ワークフロージョブの Warning プロパティの値にも追加します。 複数のアクティビティからの警告メッセージを、同じ警告レコード コレクション オブジェクトに追加できます。

#### <a name="result"></a>結果

このパラメーターは、XAML ワークフローでのみ有効です。

#### <a name="usedefaultinput-boolean"></a>UseDefaultInput \<Boolean\>

すべてのワークフロー入力を、アクティビティへの値ごとの入力として受け取ります。

たとえば、次のサンプル ワークフローの Get-Process アクティビティでは、UseDefaultInput アクティビティ共通パラメーターを使用して、ワークフローに渡される値を取得します。 入力を伴うワークフローを実行する場合、その入力がアクティビティによって使用されます。

```powershell
workflow Test-Workflow
{
    Get-Service -UseDefaultInput $True
}

PS C:> Test-Workflow -InputObject WinRm
```

```output
Status   Name        DisplayName                            PSComputerName
------   ----        -----------                            --------------
Running  winrm       Windows Remote Management (WS-Manag... localhost
```

#### <a name="verbose-switchparameter"></a>な \<SwitchParameter\>

コマンドによって実行される操作に関する詳細情報を表示します。
この情報は、トレースまたはトランザクションログの情報に似ています。
Verbose パラメーターは、現在のコマンドの $VerbosePreference 変数の値よりも優先されます。 このパラメーターは、コマンドによって詳細メッセージが生成された場合にのみ機能します。 このパラメーターは、Windows PowerShell 共通パラメーターでもあります。

#### <a name="warningaction-actionpreference"></a>WarningAction \<ActionPreference\>

アクティビティが警告に応答する方法を決定します。 "Continue" が既定値です。 Warnings Action パラメーターは、現在のコマンドの $WarningPreference 変数の値よりも優先されます。 このパラメーターは、コマンドで警告メッセージが生成された場合にのみ機能します。 このパラメーターは、Windows PowerShell 共通パラメーターでもあります。

有効な値 :

- SilentlyContinue. 警告メッセージを表示せずに、コマンドの実行を続行します。

- まま. 警告メッセージを表示し、コマンドの実行を続行します。
  "Continue" が既定値です。

- Gina. 警告メッセージを表示し、実行を続行する前に確認メッセージを表示します。 この値はほとんど使用されません。

- 停止。 警告メッセージを表示し、コマンドの実行を停止します。

> [!NOTE]
> パラメーターをコマンドで使用してスクリプトまたは関数を実行する場合、Warnings Action パラメーターは $WarningAction preference 変数の値をオーバーライドしません。

### <a name="examples"></a>例

アクティビティ共通パラメーターはきわめて便利です。 たとえば、PSComputerName パラメーターを使用して、対象のコンピューターのサブセットのみで特定のアクティビティを実行できます。

または、PSConnectionRetryCount パラメーターと PSConnectionRetryIntervalSec パラメーターを使用して、特定のアクティビティの再試行値を調整することもできます。

次の例では、PSComputerName activity 共通パラメーターを使用して、特定のドメインにあるコンピューターでのみ Get-EventLog アクティビティを実行する方法を示します。

```powershell
Workflow Test-Workflow
{
    $UserDomain = Get-Content -Path '.\UserComputers.txt'
    $Log = (Get-EventLog -LogName "Windows PowerShell" `
      -PSComputerName $UserDomain)

    if ($Log)
    {
        # Do Work Here.
    }
}
```

<!--
# KEYWORDS
[about_Activity_Common_Parameters](about_Activity_Common_Parameters.md)
[about_Activity_Parameters](about_Activity_Parameters.md)
[about_ActivityParameters]()about_ActivityParameters.md) -->

## <a name="see-also"></a>関連項目

[about_Workflows](about_workflows.md) 
[about_WorkflowCommonParameters](about_WorkflowCommonParameters.md)
