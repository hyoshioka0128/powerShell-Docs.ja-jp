---
description: このトピックでは、すべての Windows PowerShell ワークフローコマンドで有効なパラメーターについて説明します。 Windows PowerShell エンジンによってそれらがワークフローに追加されるため、ワークフローでこれらのパラメーターを使用することができ、作成したワークフローで自動的に有効になります。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/psworkflow/about/about_workflowcommonparameters?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_WorkflowCommonParameters
ms.openlocfilehash: c371666d4f58386848e7ef715b7c804dc1e8f28e
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94387789"
---
# <a name="about-workflowcommonparameters"></a>WorkflowCommonParameters について

## <a name="short-description"></a>概要

このトピックでは、すべての Windows PowerShell ワークフローコマンドで有効なパラメーターについて説明します。 Windows PowerShell エンジンによってそれらがワークフローに追加されるため、ワークフローでこれらのパラメーターを使用することができ、作成したワークフローで自動的に有効になります。

## <a name="long-description"></a>詳細説明

Windows PowerShell ワークフローの共通パラメーターは、すべての Windows PowerShell ワークフローおよびアクティビティで使用できるコマンドレットパラメーターのセットです。 これらは、ワークフローの作成者ではなく、Windows PowerShell ワークフローエンジンによって追加され、ワークフローとアクティビティで自動的に使用できるようになります。 ただし、入れ子になった3つのレベルのワークフローは、ワークフロー共通パラメーターを含む共通パラメーターをサポートしていません。

すべてのワークフローパラメーターは省略可能であり、(位置指定ではありません) という名前です。 パイプラインからの入力を受け取りません。

ほとんどのワークフロー共通パラメーターには、やなどの PS プレフィックスがあり `PSComputerName` `PSCredential` ます。 PS プレフィックス付きパラメーターは、ターゲットコンピューターの接続と実行環境を構成します。 "リモートノード" とも呼ばれます。

やなどのワークフロー共通パラメーターの多くには、 `PSAllowRedirection` `AsJob` Windows PowerShell リモート処理およびバックグラウンドジョブで使用されるパラメーターに似た名前が付いています。 これらのパラメーターは、同様に名前が付けられたリモート処理およびジョブパラメーターと同じように動作するため、リモート処理およびジョブで作成したナレッジを使用してワークフローを管理できます。

ワークフローは、Windows PowerShell 3.0 で導入されました。

### <a name="parameter-descriptions"></a>パラメーターの説明

ここでは、ワークフロー共通パラメーターについて説明します。

#### <a name="-asjob-switchparameter"></a>-AsJob \<SwitchParameter\>

ワークフローをワークフロージョブとして実行します。 Workflow コマンドを実行すると、親ジョブを表すオブジェクトが直ちに返されます。 親ジョブには、各ターゲットコンピューターで実行されている子ジョブが含まれます。 ジョブを管理するには、Job コマンドレットを使用します。 ジョブの結果を取得するには、[Receive-Job](xref:Microsoft.PowerShell.Core.Receive-Job) を使用します。

#### <a name="-jobname-string"></a>-JobName \<String\>

ワークフロージョブのフレンドリ名を指定します。 既定では、ジョブは "Job\<n\>" と名付けられます。ここで、\<n\> は、順序を示す番号です。

ワークフローコマンドで JobName パラメーターを使用すると、ワークフローはジョブとして実行され、コマンドにパラメーターを含めない場合でも、ワークフローコマンドはジョブオブジェクトを返します `AsJob` 。

Windows PowerShell のバックグラウンド ジョブの詳細については、「[about_Jobs](../../Microsoft.PowerShell.Core/About/about_Jobs.md)」を参照してください。

#### <a name="-psallowredirection-switchparameter"></a>-PSAllowRedirection \<SwitchParameter\>

ターゲットコンピューターへの接続のリダイレクトを許可します。

パラメーターを使用すると `PSConnectionURI` 、リモートの送信先は別の URI にリダイレクトする命令を返すことができます。 既定では、Windows PowerShell は接続をリダイレクトしませんが、パラメーターを使用して、 `PSAllowRedirection` ターゲットコンピューターへの接続のリダイレクトを許可することができます。

`MaximumConnectionRedirectionCount`また、ユーザー設定変数のプロパティ、 `$PSSessionOption` または `MaximumConnectionRedirectionCount` の値のプロパティを設定することによって、接続をリダイレクトする回数を制限することもできます `PSSessionOption parameter` 。 既定値は 5 です。 詳細については、 `PSSessionOption` パラメーターと [新しい-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)の説明を参照してください。

#### <a name="-psapplicationname-string"></a>-PSApplicationName \<String\>

ターゲットコンピューターへの接続に使用される接続 URI のアプリケーション名セグメントを指定します。 コマンドでパラメーターを使用しない場合は、このパラメーターを使用してアプリケーション名を指定し `ConnectionURI` ます。

既定値は、 `$PSSessionApplicationName` ローカルコンピューター上のユーザー設定変数の値です。 このユーザー設定変数が定義されていない場合、既定値は WSMAN です。 この値はほとんどの用途に適しています。 詳細については、「 [about_Preference_Variables](../../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

WinRM サービスは、アプリケーション名を使用して、接続要求を処理するリスナーを選択します。 このパラメーターの値は、 `URLPrefix` リモートコンピューター上のリスナーのプロパティの値と一致している必要があります。

#### <a name="-psauthentication-authenticationmechanism"></a>-PSAuthentication \<AuthenticationMechanism\>

対象のコンピュータに接続するときに、ユーザーの資格情報を認証するために使用されるメカニズムを指定します。

有効な値は次のとおりです。

- **[Default]**
- **Basic**
- **Credssp**
- **ダイジェスト**
- **Kerberos**
- **ネゴシエーション**
- **NegotiateWithImplicitCredential**

既定値は **Default** です。

このパラメーターの値の詳細については、PowerShell SDK の列挙体の説明を参照してください `System.Management.Automation.Runspaces.AuthenticationMechanism` 。

> [!WARNING]
> ユーザーの資格情報が認証対象のリモート コンピューターに渡される Credential Security Service Provider (CredSSP) 認証は、リモート ネットワーク共有にアクセスする場合など、複数のリソースの認証を必要とするコマンドを対象としています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

#### <a name="-psauthenticationlevel-authenticationlevel"></a>-PSAuthenticationLevel \<AuthenticationLevel\>

対象のコンピュータへの接続の認証レベルを指定します。
既定値は **Default** です。

有効な値は次のとおりです。

|名前 |[説明] |
|---------|---------|
|**Unchanged** | 認証レベルは前のコマンドと同じです。 |
|**[Default]** | [Windows 認証]。 |
|**なし** | COM 認証なし。   |
|**のインスタンスに接続するときには、** | 接続レベルの COM 認証。|
|**Call** | 呼び出しレベルの COM 認証。   |
|**パック** | パケット レベルの COM 認証。|
|**PacketIntegrity** | パケット整合性レベルの COM 認証。  |
|**PacketPrivacy** | パケット プライバシ レベルの COM 認証。 |

#### <a name="-pscertificatethumbprint-string"></a>-PSCertificateThumbprint \<String\>

この処理を実行するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書の拇印を入力します。

証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書は、ローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。

証明書を取得するには、 [Get Item](xref:Microsoft.PowerShell.Management.Get-Item) または [get-childitem] (../../Microsoft.PowerShell.Management/Get-Childitem.md) Windows PowerShell Cert: ドライブのコマンドレット。

#### <a name="-pscomputername-string"></a>-PSComputerName \<String[]\>

ワークフローのターゲットノードであるコンピューターの一覧を指定します。
ワークフロー内のコマンドまたはアクティビティは、このパラメーターを使用して指定されたコンピューター上で実行されます。 既定値はローカル コンピューターです。

1 台または複数のコンピューターの NETBIOS 名、IP アドレス、または完全修飾ドメイン名をコンマ区切りリストで入力します。 ローカルのコンピューターを指定するには、コンピューター名、「localhost」、またはドット (.) を入力します。

パラメーターの値にローカルコンピューターを含めるには `PSComputerName` 、[管理者として実行] オプションを使用して Windows PowerShell を開きます。

コマンドでこのパラメーターを省略した場合、またはその値が `$null` または空の文字列の場合、ワークフローターゲットはローカルコンピューターであり、Windows PowerShell リモート処理はコマンドの実行には使用されません。

パラメーターの値に IP アドレスを使用するには、 `ComputerName` コマンドにパラメーターを含める必要があり `PSCredential` ます。 また、HTTPS トランスポート用にコンピューターを構成するか、リモート コンピューターの IP アドレスをローカル コンピューター上の WinRM TrustedHosts 一覧に含める必要があります。 TrustedHosts の一覧にコンピューター名を追加する手順については、 [about_Remote_Troubleshooting](../../Microsoft.PowerShell.Core/About/about_Remote_Troubleshooting.md)の「 *信頼されたホストの一覧にコンピューターを追加する方法」* を参照してください。

#### <a name="-psconfigurationname-string"></a>-PSConfigurationName \<String\>

対象のコンピューターでセッションを構成するために使用されるセッション構成を指定します。 (ワークフローサーバーコンピューターではなく) ターゲットコンピューターにセッション構成を入力します。 既定値は、`Microsoft.PowerShell.Workflow` です。

#### <a name="-psconnectionretrycount-uint"></a>-PSConnectionRetryCount \<UInt\>

最初の接続試行が失敗した場合に、各ターゲットコンピュータへの接続試行回数の最大値を指定します。 1から 4294967295 (UInt) までの数値を入力します。 既定値のゼロ (0) は、再試行の回数を表します。

#### <a name="-psconnectionretryintervalsec-uint"></a>-PSConnectionRetryIntervalSec \<UInt\>

接続再試行の間隔を秒単位で指定します。 既定値は 0 です。 このパラメーターは、PSConnectionRetryCount の値が1以上の場合にのみ有効です。

#### <a name="-psconnectionuri-systemuri"></a>-PSConnectionURI \<System.Uri\>

ターゲットコンピューター上のワークフローの接続エンドポイントを定義する Uniform Resource Identifier (URI) を指定します。 URI は完全修飾名にする必要があります。

この文字列の形式は次のとおりです。

`<Transport>://<ComputerName>:<Port>/<ApplicationName>`

既定値は `http://localhost:5985/WSMAN` です。

を指定しない場合は、、、、およびの各パラメーターを使用して `PSConnectionURI` 値を `PSUseSSL` 指定でき `PSComputerName` `PSPort` `PSApplicationName` `PSConnectionURI` ます。

URI のトランスポートセグメントの有効な値は、 **HTTP** と **HTTPS** です。
トランスポートセグメントを使用して接続 URI を指定し、ポートを指定しない場合、セッションは標準ポート80で HTTP 用および 443 (HTTPS 用) で作成されます。 Windows PowerShell リモート処理用の既定のポートを使用するには、HTTP の場合は 5985、HTTPS の場合は 5986 を指定します。

#### <a name="-pscredential-pscredential"></a>-PSCredential \<PSCredential\>

対象のコンピューターでワークフローを実行する権限を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。 このパラメーターは、PSComputerName パラメーターがコマンドに含まれている場合にのみ有効です。

"User01" や "Domain01\user01」" などのユーザー名を入力するか、 `PSCredential` コマンドレットから返されるオブジェクトなどのオブジェクトを含む変数を入力し `Get-Credential` ます。 ユーザー名のみを入力すると、パスワードの入力を促すメッセージが表示されます。

#### <a name="-pselapsedtimeoutsec-uint32"></a>-PSElapsedTimeoutSec \<UInt32\>

ワークフローと関連するすべてのリソースがシステムで保持される期間を指定します。 タイムアウトが経過すると、ワークフローはまだ処理中であっても、削除されます。 10 ~ 4294967295 の値を入力してください。 既定値の 0 (ゼロ) は、経過タイムアウトがないことを意味します。

#### <a name="-psparametercollection-hashtable"></a>-PSParameterCollection \<Hashtable[]\>

異なるターゲットコンピューターに対して、異なるワークフロー共通パラメーター値を指定します。

ターゲットコンピューターごとに1つのハッシュテーブルを含むハッシュテーブルのコンマ区切りの一覧を入力します。 各ハッシュテーブルでは、最初のキーはで、 `PSComputerName` その値はターゲットコンピューターの名前です。 コンピューター名にはワイルドカード文字を使用できます。 ハッシュテーブル内の残りのキーについては、キーがパラメーター名で、値がパラメーター値になります。

次に例を示します。

```powershell
-PSParameterCollection @{PSComputerName="*"; PSElapsedTimeoutSec=20},
@{PSComputerName="Server02"},
@{PSComputerName="Server03"},
@{PSComputerName="Server01"; PSElapsedTimeoutSec=10}
```

上の例では、すべての接続に既定の PSElapsedTimeoutSec 20 秒が設定されています。ただし、Server01 では、独自のタイムアウトを10秒に指定することで既定値を上書きします。

#### <a name="-pspersist-boolean"></a>-PSPersist \<Boolean\>

ワークフローで指定されているチェックポイントに加えて、ワークフローにチェックポイントを追加します。

このパラメーターでは、 `PSPersist` アクティビティ共通パラメーター、 `Checkpoint-Workflow` アクティビティ、または変数を使用して指定されたチェックポイントなど、ワークフローのチェックポイントを抑制することはできません `$PSPersistPreference` 。

"チェックポイント" または "永続化ポイント" は、ワークフローの実行中にキャプチャされ、ディスクまたは SQL データベースの永続ストアに保存される、ワークフローの状態とデータのスナップショットです。 Windows PowerShell ワークフローでは、保存されたデータを使用して、ワークフローを再開するのではなく、最後の永続化ポイントから中断または中断されたワークフローを再開します。

有効な値:

- ( *既定値* )このパラメーターを省略すると、ワークフローで指定されているチェックポイントだけでなく、ワークフローの先頭と末尾にチェックポイントが追加されます。

- `$True`. ワークフローで指定されているチェックポイントだけでなく、ワークフローの開始時と終了時、および各アクティビティの後のチェックポイントにチェックポイントを追加します。

- `$False`. チェックポイントは追加されません。 チェックポイントは、ワークフローで指定されている場合にのみ取得されます。

#### <a name="-psport-int32"></a>-PSPort \<Int32\>

ターゲットコンピューターのネットワークポートを指定します。 既定のポート番号は 5985 (HTTP 用の WinRM ポート) と 5986 (HTTPS 用の WinRM ポート) です。

必要な場合を除き、PSPort パラメーターは使用しないでください。 コマンドで設定されたポートは、コマンドを実行するすべてのコンピューターまたはセッションに適用されます。 代替ポートの設定によっては、コマンドがすべてのコンピューターで実行されない場合があります。 代替ポートを使用する前に、そのポートでリッスンするようにリモート コンピューター上の WinRM リスナーを構成する必要があります。

#### <a name="-psprivatemetadata-hashtable"></a>-PSPrivateMetadata \<Hashtable\>

ワークフロージョブにカスタマイズされた情報を提供します。 ハッシュテーブルを入力します。 これらのキーと値は、ワークフローごとにカスタマイズされます。 ワークフローのプライベートメタデータの詳細については、ワークフローのヘルプトピックを参照してください。

このパラメーターは、Windows PowerShell ワークフローエンジンによって処理されません。
代わりに、エンジンがハッシュテーブルをワークフローに直接渡します。

#### <a name="-psrunningtimeoutsec-uint32"></a>-PSRunningTimeoutSec \<UInt32\>

ワークフローが中断された時間を除く、ワークフローの実行時間を秒単位で指定します。 時間が経過してもワークフローの実行が完了しない場合、Windows PowerShell ワークフローエンジンはワークフローの実行を強制的に停止します。

#### <a name="-pssessionoption-pssessionoption"></a>-PSSessionOption \<PSSessionOption\>

セッションの詳細オプションを対象のコンピューターに設定します。 コマンドレットを使用して作成したオブジェクトなどのオブジェクトを入力し `PSSessionOption` `New-PSSessionOption` ます。

セッションオプションの既定値は、設定されている場合は、ユーザー設定変数の値によって決まり `$PSSessionOption` ます。 それ以外の場合は、セッション構成で指定された値が使用されます。

既定値を含むセッションオプションの説明については、コマンドレットのヘルプトピック `New-PSSessionOption` (../../Microsoft.PowerShell.Core/New-PSSessionOption.md). ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](../../Microsoft.PowerShell.Core/About/about_Preference_Variables.md)」を参照してください。

#### <a name="-psusessl-switchparameter"></a>-PSUseSSL \<SwitchParameter\>

は、Secure Sockets Layer (SSL) プロトコルを使用して、対象のコンピューターへの接続を確立します。 既定では、SSL は使用されません。

WS-Management は、ネットワークを介して転送されるすべての Windows PowerShell コンテンツを暗号化します。 UseSSL は、HTTP 経由ではなく HTTPS 経由でデータを送信するために追加された保護機能です。 コマンドに使用するポートで SSL が使用できない場合に、このパラメーターを指定すると、コマンドは失敗します。

## <a name="see-also"></a>関連項目

- [about_ActivityCommonParameters](about_ActivityCommonParameters.md)
- [about_Workflows](about_Workflows.md)
- [Invoke-AsWorkflow](xref:PSWorkflowUtility.Invoke-AsWorkflow)
- [New-PSWorkflowExecutionOption](xref:PSWorkflow.New-PSWorkflowExecutionOption)
- [New-PSWorkflowSession](xref:PSWorkflow.New-PSWorkflowSession)
