---
ms.date: 09/07/2018
ms.topic: reference
title: PowerShell コマンドに承認されている動詞
description: PowerShell コマンドに承認されている動詞
ms.openlocfilehash: fc1ff989ae86862e0f9cc24d8bcba2ff02ef68cc
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355104"
---
# <a name="approved-verbs-for-powershell-commands"></a>PowerShell コマンドに承認されている動詞

PowerShell の場合、コマンドレットの名前を表すため、および派生 .NET クラスを表すために、動詞-名詞のペアを使用します。
名前の動詞の部分は、コマンドレットで実行されるアクションを識別します。 名前の名詞の部分は、アクションが実行される対象であるエンティティを識別します。 たとえば、`Get-Command` は、PowerShell に登録されているすべてのコマンドを取得するコマンドレットです。

> [!NOTE]
> PowerShell の場合、"_動詞_" という用語は、アクションを意味する単語 (その単語が英語の標準的な動詞でない場合でも) を記述するために使用されます。 たとえば、_New_ という用語は英語の動詞ではありませんが、アクションを意味しているため、有効な PowerShell 動詞の名前です。

承認された各動詞には、対応する "_エイリアスのプレフィックス_" が定義されています。 このエイリアスのプレフィックスは、その動詞を使用するコマンドのエイリアスで使用します。 たとえば、`Import` のエイリアスのプレフィックスは `ip` であり、したがって `Import-Module` のエイリアスは `ipmo` です。 これは推奨事項ですが、規則ではありません。特に、他の環境の既知のコマンドを模倣しているコマンド エイリアスの場合は、従う必要はありません。

## <a name="verb-naming-recommendations"></a>動詞の名前付けに関する推奨事項

次の推奨事項は、ご自身が作成するコマンドレットと PowerShell に用意されているコマンドレット、さらに他のユーザーが設計したコマンドレットとの間の一貫性を確保するために、ご自身のコマンドレット用に適切な動詞を選択する場合に役立ちます。

- PowerShell に用意されている定義済み動詞名のいずれかを使用します。
- 動詞を使用してアクションの全般的なスコープを記述し、パラメーターを使用してコマンドレットのアクションをさらに絞り込むことができます。
- 承認された動詞のシノニムは使用しないでください。 たとえば、常に `Remove` を使用し、`Delete` や `Eliminate` は使用しません。
- このトピックに記載されている各動詞の形式のみを使用します。 たとえば、`Get` は使用しますが、`Getting` または `Gets` は使用しません。
- 次の予約された動詞またはエイリアスは使用しないでください。 これらの動詞は、PowerShell 言語またはそのまれないくつかのコマンドレットによって、例外的な状況で使用されます。
  - ForEach (foreach)
  - [Format](/dotnet/api/System.Management.Automation.VerbsCommon.Format) (f): 指定された形式またはレイアウトでオブジェクトを整列します
  - [Group](/dotnet/api/System.Management.Automation.VerbsData.Group) (gp): 1 つ以上のリソースを配置するか関連付けます
  - [Ping](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Ping) (pi)
  - Sort (sr)
  - Tee (te)
  - Where (wh)

`Get-Verb` コマンドレットを使用して、動詞の完全な一覧を取得できます。

## <a name="similar-verbs-for-different-actions"></a>異なるアクションを表す類似した動詞

以下の類似した動詞は、異なるアクションを表します。

### <a name="new-vs-set"></a>New と オン

`New` は、新しいリソースを作成するときに使用します。 `Set` 動詞は、既存のリソースを変更するときに使用します。必要に応じて、`Set-Variable` コマンドレットのように、既存のものがない場合には作成します。

### <a name="find-vs-search"></a>Find と 検索

`Find` 動詞は、オブジェクトを検索するときに使用します。 `Search` 動詞は、コンテナー内のリソースへの参照を作成するときに使用します。

### <a name="get-vs-read"></a>Get と Read

`Get` 動詞は、リソース (ファイルなど) に関する情報を取得するとき、または、リソースへの今後のアクセスに使用できるオブジェクトを取得するときに使用します。 `Read` 動詞は、リソースを開き、その中に含まれる情報を抽出するときに使用します。

### <a name="invoke-vs-start"></a>Invoke と [開始]

`Invoke` 動詞は、同期操作を実行するときに使用します。コマンドを実行し、その終了を待機する場合などです。 `Start` 動詞は、非同期操作を開始するときに使用します。自律的なプロセスを開始する場合などです。

### <a name="ping-vs-test"></a>Ping と テスト

`Test` 動詞を使用してください。

## <a name="common-verbs"></a>Common (共通) 動詞

PowerShell には、ほぼすべてのコマンドレットに適用できる汎用アクションが、[System.Management.Automation.VerbsCommon](/dotnet/api/System.Management.Automation.VerbsCommon) 列挙クラスを使用して定義されています。 次の表は、ほとんどの定義済み動詞を示しています。

|動詞 (エイリアス)|アクション|避けるべきシノニム|
|--------------------|------------|--------------|
|[Add](/dotnet/api/System.Management.Automation.VerbsCommon.Add) (a)|コンテナーにリソースを追加するか、項目を別の項目に付加します。 たとえば、`Add-Content` は、ファイルにコンテンツを追加するコマンドレットです。 この動詞は `Remove` と対になっています。|Append、Attach、Concatenate、Insert|
|[Clear](/dotnet/api/System.Management.Automation.VerbsCommon.Clear) (cl)|コンテナーからオブジェクトをすべて削除しますが、コンテナーは削除しません。 たとえば、`Clear-Content` コマンドレットによって、ファイルの内容は削除されますが、ファイルは削除されません。|Flush、Erase、Release、Unmark、Unset、Nullify|
|[Close](/dotnet/api/System.Management.Automation.VerbsCommon.Close) (cs)|リソースの状態を変更して、アクセス不可または無効にするか、使用できないようにします。 この動詞は `Open.` と対になっています。||
|[Copy](/dotnet/api/System.Management.Automation.VerbsCommon.Copy) (cp)|リソースを別の名前または別のコンテナーにコピーします。 たとえば、`Copy-Item` は、データ ストア内のある場所から別の場所に項目 (ファイルなど) をコピーするコマンドレットです。|Duplicate、Clone、Replicate、Sync|
|[Enter](/dotnet/api/System.Management.Automation.VerbsCommon.Enter) (et)|ユーザーにリソース内への移動を許可するアクションを指定します。 たとえば、`Enter-PSSession` コマンドレットによって、ユーザーは対話型セッションに移行します。 この動詞は `Exit` と対になっています。|Push、Into|
|[Exit](/dotnet/api/System.Management.Automation.VerbsCommon.Exit) (ex)|現在の環境またはコンテキストを、最後に使用したコンテキストに設定します。 たとえば、`Exit-PSSession` コマンドレットによって、ユーザーは対話型セッションを開始するために使用されたセッションに移行します。 この動詞は `Enter` と対になっています。|Pop、Out|
|[Find](/dotnet/api/System.Management.Automation.VerbsCommon.Find) (fd)|不明、暗黙、オプション、または指定のコンテナー内のオブジェクトを検索します。|検索|
|[Get](/dotnet/api/System.Management.Automation.VerbsCommon.Get) (g)|リソースを取得するアクションを指定します。 この動詞は `Set` と対になっています。|Read、Open、Cat、Type、Dir、Obtain、Dump、Acquire、Examine、Find、Search|
|[Hide](/dotnet/api/System.Management.Automation.VerbsCommon.Hide) (h)|リソースを検出できないようにします。 たとえば、名前に Hide 動詞が含まれているコマンドレットを使用すると、ユーザーからサービスを隠すことができます。 この動詞は `Show` と対になっています。|ブロックする|
|[Join](/dotnet/api/System.Management.Automation.VerbsCommon.Join) (j)|リソースを 1 つのリソースに結合します。 たとえば、`Join-Path` は、パスとその子パスの 1 つを組み合わせて 1 つのパスを作成するコマンドレットです。 この動詞は `Split` と対になっています。|Combine、Unite、Connect、Associate|
|[Lock](/dotnet/api/System.Management.Automation.VerbsCommon.Lock) (lk)|リソースをセキュリティで保護します。 この動詞は `Unlock` と対になっています。|Restrict、Secure|
|[Move](/dotnet/api/System.Management.Automation.VerbsCommon.Move) (m)|ある場所から別の場所にリソースを移動します。 たとえば、`Move-Item` は、データ ストア内のある場所から別の場所に項目を移動するコマンドレットです。|Transfer、Name、Migrate|
|[New](/dotnet/api/System.Management.Automation.VerbsCommon.New) (n)|リソースを作成します。 (`Set` 動詞は、`Set-Variable` コマンドレットのように、データを含むリソースを作成するときにも使用できます。)|Create、Generate、Build、Make、Allocate|
|[Open](/dotnet/api/System.Management.Automation.VerbsCommon.Open) (op)|リソースの状態を変更して、アクセス可能または有効にするか、使用できるようにします。 この動詞は `Close` と対になっています。||
|[Optimize](/dotnet/api/System.Management.Automation.VerbsCommon.Optimize) (om)|リソースの有効性を高めます。||
|[Pop](/dotnet/api/System.Management.Automation.VerbsCommon.Pop) (pop)|スタックの一番上から項目を削除します。 たとえば、`Pop-Location` は、現在の場所を、最後にスタックにプッシュされた場所に変更するコマンドレットです。||
|[Push](/dotnet/api/System.Management.Automation.VerbsCommon.Push) (pu)|スタックの一番上に項目を追加します。 たとえば、`Push-Location` は、現在の場所をスタックにプッシュするコマンドレットです。||
|[Redo](/dotnet/api/System.Management.Automation.VerbsCommon.Redo) (re)|元に戻した状態にリソースを再設定します。||
|[Remove](/dotnet/api/System.Management.Automation.VerbsCommon.Remove) (r)|コンテナーからリソースを削除します。 たとえば、`Remove-Variable` は、変数とその値を削除するコマンドレットです。 この動詞は `Add` と対になっています。|Clear、Cut、Dispose、Discard、Erase|
|[Rename](/dotnet/api/System.Management.Automation.VerbsCommon.Rename) (rn)|リソースの名前を変更します。 たとえば、格納されたデータにアクセスするために使用される `Rename-Item` は、データ ストア内の項目の名前を変更するコマンドレットです。|Change|
|[Reset](/dotnet/api/System.Management.Automation.VerbsCommon.Reset) (rs)|リソースを元の状態に設定し直します。||
|[Resize](/dotnet/api/System.Management.Automation.VerbsCommon.Resize)(rz)|リソースのサイズを変更します。||
|[Search](/dotnet/api/System.Management.Automation.VerbsCommon.Search) (sr)|コンテナー内のリソースへの参照を作成します。|Find、Locate|
|[Select](/dotnet/api/System.Management.Automation.VerbsCommon.Select) (sc)|コンテナー内のリソースを検索します。 たとえば、`Select-String` は、文字列およびファイル内のテキストを検索するコマンドレットです。|Find、Locate|
|[Set](/dotnet/api/System.Management.Automation.VerbsCommon.Set) (s)|既存のリソースのデータを置き換えるか、何らかのデータを含むリソースを作成します。 たとえば、`Set-Date` は、ローカル コンピューターのシステム時刻を変更するコマンドレットです (リソースを作成するには、`New` 動詞を使用することもできます)。この動詞は `Get` と対になっています。|Write、Reset、Assign、Configure|
|[Show](/dotnet/api/System.Management.Automation.VerbsCommon.Show) (sh)|リソースをユーザーに表示します。 この動詞は `Hide` と対になっています。|Display、Produce|
|[Skip](/dotnet/api/System.Management.Automation.VerbsCommon.Skip) (sk)|シーケンス内の 1 つ以上のリソースまたはポイントをバイパスします。|Bypass、Jump|
|[Split](/dotnet/api/System.Management.Automation.VerbsCommon.Split) (sl)|リソースの一部を分離します。 たとえば、`Split-Path` は、パスのさまざまな部分を返すコマンドレットです。 この動詞は `Join` と対になっています。|分離|
|[Step](/dotnet/api/System.Management.Automation.VerbsCommon.Step) (st)|シーケンス内の次のポイントまたはリソースに移動します。||
|[Switch](/dotnet/api/System.Management.Automation.VerbsCommon.Switch) (sw)|2 つの場所、責任、または状態の間での変更など、2 つのリソースを交互に切り替えるアクションを指定します。||
|[Undo](/dotnet/api/System.Management.Automation.VerbsCommon.Undo) (un)|リソースを以前の状態に設定します。||
|[Unlock](/dotnet/api/System.Management.Automation.VerbsCommon.Unlock) (uk)|ロックされたリソースを解放します。 この動詞は `Lock` と対になっています。|Release、Unrestrict、Unsecure|
|[Watch](/dotnet/api/System.Management.Automation.VerbsCommon.Watch) (wc)|リソースの変更の有無を継続的に検査または監視します。||

## <a name="communications-verbs"></a>Communications (通信) 動詞

PowerShell には、通信に適用されるアクションが、[System.Management.Automation.VerbsCommunications](/dotnet/api/System.Management.Automation.VerbsCommunications) クラスを使用して定義されています。 次の表は、ほとんどの定義済み動詞を示しています。

|動詞 (エイリアス)|アクション|避けるべきシノニム|
|--------------------|------------|--------------|
|[Connect](/dotnet/api/System.Management.Automation.VerbsCommunications.Connect) (cc)|ソースと宛先の間にリンクを作成します。 この動詞は `Disconnect` と対になっています。|Join、Telnet|
|[Disconnect](/dotnet/api/System.Management.Automation.VerbsCommunications.Disconnect) (dc)|ソースと宛先の間のリンクを解除します。 この動詞は `Connect` と対になっています。|Break、Logoff|
|[Read](/dotnet/api/System.Management.Automation.VerbsCommunications.Read) (rd)|ソースから情報を取得します。 この動詞は `Write` と対になっています。|Acquire、Prompt、Get|
|[Receive](/dotnet/api/System.Management.Automation.VerbsCommunications.Receive) (rc)|ソースから送信された情報を受け入れます。 この動詞は `Send` と対になっています。|Read、Accept、Peek|
|[Send](/dotnet/api/System.Management.Automation.VerbsCommunications.Send) (sd)|宛先に情報を配信します。 この動詞は `Receive` と対になっています。|Put、Broadcast、Mail、Fax|
|[Write](/dotnet/api/System.Management.Automation.VerbsCommunications.Write) (wr)|ターゲットに情報を追加します。 この動詞は `Read` と対になっています。|Put、Print|

## <a name="data-verbs"></a>Data (データ) 動詞

PowerShell には、データ処理に適用されるアクションが、[System.Management.Automation.VerbsData](/dotnet/api/System.Management.Automation.VerbsData) クラスを使用して定義されています。 次の表は、ほとんどの定義済み動詞を示しています。

|動詞名 (エイリアス)|アクション|避けるべきシノニム|
|-------------------------|------------|--------------|
|[Backup](/dotnet/api/System.Management.Automation.VerbsData.Backup) (ba)|データをレプリケートして格納します。|Save、Burn、Replicate、Sync|
|[Checkpoint](/dotnet/api/System.Management.Automation.VerbsData.Checkpoint) (ch)|データの現在の状態またはその構成のスナップショットを作成します。|[Diff]|
|[Compare](/dotnet/api/System.Management.Automation.VerbsData.Compare) (cr)|あるリソースのデータを、別のリソースのデータと比較して評価します。|[Diff]|
|[Compress](/dotnet/api/System.Management.Automation.VerbsData.Compress) (cm)|リソースのデータを圧縮します。 `Expand` と対になっています。|コンパクト|
|[Convert](/dotnet/api/System.Management.Automation.VerbsData.Convert) (cv)|コマンドレットで双方向変換がサポートされている場合、またはコマンドレットで複数のデータ型間の変換がサポートされている場合、データの表現をあるものから別のものに変更します。|Change、Resize、Resample|
|[ConvertFrom](/dotnet/api/System.Management.Automation.VerbsData.ConvertFrom) (cf)|1 種類のプライマリ入力 (コマンドレットの名詞が入力を示します) を、1 つ以上のサポートされている出力の種類に変換します。|Export、Output、Out|
|[ConvertTo](/dotnet/api/System.Management.Automation.VerbsData.ConvertTo) (ct)|1 つ以上の入力の種類を 1 つのプライマリ出力の種類に変換します (コマンドレットの名詞が出力の種類を示します)。|Import、Input、In|
|[Dismount](/dotnet/api/System.Management.Automation.VerbsData.Dismount) (dm)|名前付きエンティティを場所からデタッチします。 この動詞は `Mount` と対になっています。|Unmount、Unlink|
|[Edit](/dotnet/api/System.Management.Automation.VerbsData.Edit) (ed)|コンテンツを追加または削除することによって既存のデータを変更します。|Change、Update、Modify|
|[Expand](/dotnet/api/System.Management.Automation.VerbsData.Expand) (en)|圧縮されたリソースのデータを元の状態に復元します。 この動詞は `Compress` と対になっています。|Explode、Uncompress|
|[Export](/dotnet/api/System.Management.Automation.VerbsData.Export) (ep)|プライマリ入力をファイルなどの永続的なデータ ストアまたはインターチェンジ形式にカプセル化します。 この動詞は `Import` と対になっています。|Extract、Backup|
|[Import](/dotnet/api/System.Management.Automation.VerbsData.Import) (ip)|永続的なデータ ストア (ファイルなど) に、またはインターチェンジ形式で格納されているデータから、リソースを作成します。 たとえば、`Import-CSV` は、コンマ区切り値 (CSV) ファイルから、他のコマンドレットで使用できるオブジェクトにデータをインポートするコマンドレットです。 この動詞は `Export` と対になっています。|BulkLoad、Load|
|[Initialize](/dotnet/api/System.Management.Automation.VerbsData.Initialize) (in)|使用するリソースを準備し、それを既定の状態に設定します。|Erase、Init、Renew、Rebuild、Reinitialize、Setup|
|[Limit](/dotnet/api/System.Management.Automation.VerbsData.Limit) (l)|リソースに制約を適用します。|Quota|
|[Merge](/dotnet/api/System.Management.Automation.VerbsData.Merge) (mg)|複数のリソースから 1 つのリソースを作成します。|Combine、Join|
|[Mount](/dotnet/api/System.Management.Automation.VerbsData.Mount) (mt)|名前付きエンティティを場所にアタッチします。 この動詞は `Dismount` と対になっています。|接続する|
|[Out](/dotnet/api/System.Management.Automation.VerbsData.Out) (o)|環境からデータを送信します。 たとえば、`Out-Printer` は、プリンターにデータを送信するコマンドレットです。||
|[Publish](/dotnet/api/System.Management.Automation.VerbsData.Publish) (pb)|リソースを他のユーザーが使用できるようにします。 この動詞は `Unpublish` と対になっています。|Deploy、Release、Install|
|[Restore](/dotnet/api/System.Management.Automation.VerbsData.Restore) (rr)|リソースを定義済みの状態 (`Checkpoint` によって設定された状態など) に設定します。 たとえば、`Restore-Computer` は、ローカル コンピューター上でシステムの復元を開始するコマンドレットです。|Repair、Return、Undo、Fix|
|[Save](/dotnet/api/System.Management.Automation.VerbsData.Save) (sv)|損失を避けるためにデータを保持します。||
|[Sync](/dotnet/api/System.Management.Automation.VerbsData.Sync) (sy)|2 つ以上のリソースが同じ状態にあることを保証します。|Replicate、Coerce、Match|
|[Unpublish](/dotnet/api/System.Management.Automation.VerbsData.Unpublish) (ub)|リソースを他のユーザーが使用できないようにします。 この動詞は `Publish` と対になっています。|Uninstall、Revert、Hide|
|[Update](/dotnet/api/System.Management.Automation.VerbsData.Update) (ud)|状態、精度、準拠、またはコンプライアンスを維持するために、リソースを最新の状態にします。 たとえば、`Update-FormatData` は、フォーマット ファイルを更新して現在の PowerShell コンソールに追加するコマンドレットです。|Refresh、Renew、Recalculate、Re-index|

## <a name="diagnostic-verbs"></a>Diagnostic (診断) 動詞

PowerShell には、診断に適用されるアクションが、[System.Management.Automation.VerbsDiagnostic](/dotnet/api/System.Management.Automation.VerbsDiagnostic) クラスを使用して定義されています。 次の表は、ほとんどの定義済み動詞を示しています。

|動詞 (エイリアス)|アクション|避けるべきシノニム|
|--------------------|------------|--------------|
|[Debug](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Debug) (db)|リソースを調べて、運用上の問題を診断します。|診断|
|[Measure](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Measure) (ms)|指定された操作によって使用されたリソースを識別するか、リソースに関する統計情報を取得します。|Calculate、Determine、Analyze|
|[Repair](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Repair) (rp)|リソースを使用できる状態に復元します。|Fix、Restore|
|[Resolve](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Resolve) (rv)|リソースの簡略表現をより完全な表現にマップします。|Expand、Determine|
|[Test](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Test) (t)|リソースの操作または整合性を検証します。|Diagnose、Analyze、Salvage、Verify|
|[Trace](/dotnet/api/System.Management.Automation.VerbsDiagnostic.Trace) (tr)|リソースのアクティビティを追跡します。|Track、Follow、Inspect、Dig|

## <a name="lifecycle-verbs"></a>Lifecycle (ライフサイクル) 動詞

PowerShell には、リソースのライフサイクルに適用されるアクションが、[System.Management.Automation.VerbsLifeCycle](/dotnet/api/System.Management.Automation.VerbsLifeCycle) クラスを使用して定義されています。 次の表は、ほとんどの定義済み動詞を示しています。

|動詞 (エイリアス)|アクション|避けるべきシノニム|
|--------------------|------------|--------------|
|[Approve](/dotnet/api/System.Management.Automation.VerbsLifecycle.Approve) (ap)|リソースまたはプロセスの状態を確認するか、それに同意します。||
|[Assert](/dotnet/api/System.Management.Automation.VerbsLifecycle.Assert) (as)|リソースの状態を表明します。|Certify|
|[Build](/dotnet/api/System.Management.Automation.VerbsLifecycle.Build) (bd)|何らかの入力ファイル セット (通常はソース コードまたは宣言ドキュメント) から成果物 (通常はバイナリまたはドキュメント) を作成します。この動詞は、PowerShell 6 で追加されました。||
|[Complete](/dotnet/api/system.management.automation.host.buffercelltype) (cp)|操作を終了します。||
|[Confirm](/dotnet/api/System.Management.Automation.VerbsLifecycle.Confirm) (cn)|リソースまたはプロセスの状態を承認、検証、または確認します。|Acknowledge、Agree、Certify、Validate、Verify|
|[Deny](/dotnet/api/System.Management.Automation.VerbsLifecycle.Deny) (dn)|リソースまたはプロセスの状態を拒否、反対、ブロック、または禁止します。|Block、Object、Refuse、Reject|
|[Deploy](/dotnet/api/System.Management.Automation.VerbsLifecycle.Deploy) (dp)|アプリケーション、Web サイト、またはソリューションをリモート ターゲットに送信して、そのソリューションのコンシューマーが展開の完了後にアクセスできるようにします。 この動詞は、PowerShell 6 で追加されました。||
|[Disable](/dotnet/api/System.Management.Automation.VerbsLifecycle.Disable) (d)|リソースを使用不可または非アクティブの状態に構成します。 たとえば、`Disable-PSBreakpoint` コマンドレットを使用すると、ブレークポイントが非アクティブになります。 この動詞は `Enable` と対になっています。|Halt、Hide|
|[Enable](/dotnet/api/System.Management.Automation.VerbsLifecycle.Enable) (e)|リソースを使用可能またはアクティブな状態に構成します。 たとえば、`Enable-PSBreakpoint` コマンドレットを使用すると、ブレークポイントがアクティブになります。 この動詞は `Disable` と対になっています。|Start、Begin|
|[Install](/dotnet/api/System.Management.Automation.VerbsLifecycle.Install) (is)|リソースを場所に配置し、必要に応じて初期設定します。 この動詞は `Uninstall` と対になっています。|セットアップ|
|[Invoke](/dotnet/api/System.Management.Automation.VerbsLifecycle.Invoke) (i)|コマンドやメソッドの実行などのアクションを実行します。|Run、Start|
|[Register](/dotnet/api/System.Management.Automation.VerbsLifecycle.Register) (rg)|データベースなどのリポジトリにリソースのエントリを作成します。 この動詞は `Unregister` と対になっています。||
|[Request](/dotnet/api/System.Management.Automation.VerbsLifecycle.Request) (rq)|リソースを要求するか、アクセス許可を要求します。||
|[Restart](/dotnet/api/System.Management.Automation.VerbsLifecycle.Restart) (rt)|操作を停止し、再び開始します。 たとえば、`Restart-Service` は、サービスを停止してから開始するコマンドレットです。|リサイクル|
|[Resume](/dotnet/api/System.Management.Automation.VerbsLifecycle.Resume) (ru)|中断されていた操作を開始します。 たとえば、`Resume-Service` は、中断されていたサービスを開始するコマンドレットです。 この動詞は `Suspend` と対になっています。||
|[Start](/dotnet/api/System.Management.Automation.VerbsLifecycle.Start) (sa)|操作を開始します。 たとえば、`Start-Service` はサービスを開始するコマンドレットです。 この動詞は `Stop` と対になっています。|Launch、Initiate、Boot|
|[Stop](/dotnet/api/System.Management.Automation.VerbsLifecycle.Stop) (sp)|アクティビティを中止します。 この動詞は `Start` と対になっています。|End、Kill、Terminate、Cancel|
|[Submit](/dotnet/api/System.Management.Automation.VerbsLifecycle.Submit) (sb)|承認を受けるためにリソースを提示します。|投稿|
|[Suspend](/dotnet/api/System.Management.Automation.VerbsLifecycle.Suspend) (ss)|アクティビティを一時停止します。 たとえば、`Suspend-Service` はサービスを一時停止するコマンドレットです。 この動詞は `Resume` と対になっています。|一時停止|
|[Uninstall](/dotnet/api/System.Management.Automation.VerbsLifecycle.Uninstall) (us)|指示した場所からリソースを削除します。 この動詞は `Install` と対になっています。||
|[Unregister](/dotnet/api/System.Management.Automation.VerbsLifecycle.Unregister) (ur)|リポジトリからリソースのエントリを削除します。 この動詞は `Register` と対になっています。|[削除]|
|[Wait](/dotnet/api/System.Management.Automation.VerbsLifecycle.Wait) (w)|指定されたイベントが発生するまで、操作を一時停止します。 たとえば、`Wait-Job` は、1 つ以上のバックグラウンド ジョブが完了するまで操作を一時停止するコマンドレットです。|Sleep、Pause|

## <a name="security-verbs"></a>Security (セキュリティ) 動詞

PowerShell には、セキュリティに適用されるアクションが、[System.Management.Automation.VerbsSecurity](/dotnet/api/System.Management.Automation.VerbsSecurity) クラスを使用して定義されています。 次の表は、ほとんどの定義済み動詞を示しています。

|動詞 (エイリアス)|アクション|避けるべきシノニム|
|--------------------|------------|--------------|
|[Block](/dotnet/api/System.Management.Automation.VerbsSecurity.Block) (bl)|リソースへのアクセスを制限します。 この動詞は `Unblock` と対になっています。|Prevent、Limit、Deny|
|[Grant](/dotnet/api/System.Management.Automation.VerbsSecurity.Grant) (gr)|リソースへのアクセスを許可します。 この動詞は `Revoke` と対になっています。|Allow、Enable|
|[Protect](/dotnet/api/System.Management.Automation.VerbsSecurity.Protect) (pt)|リソースを攻撃や損失から保護します。 この動詞は `Unprotect` と対になっています。|Encrypt、Safeguard、Seal|
|[Revoke](/dotnet/api/System.Management.Automation.VerbsSecurity.Revoke) (rk)|リソースへのアクセスを許可しないアクションを指定します。 この動詞は `Grant` と対になっています。|Remove、Disable|
|[Unblock](/dotnet/api/System.Management.Automation.VerbsSecurity.Unblock) (ul)|リソースに対する制限を削除します。 この動詞は `Block` と対になっています。|Clear、Allow|
|[Unprotect](/dotnet/api/System.Management.Automation.VerbsSecurity.Unprotect) (up)|攻撃や損失を防ぐために追加された保護をリソースから削除します。 この動詞は `Protect` と対になっています。|Decrypt、Unseal|

## <a name="other-verbs"></a>Other (その他) 動詞

PowerShell には、共通、通信、データ、ライフサイクル、またはセキュリティ動詞名の動詞など、特定の動詞名カテゴリに当てはまらない正規動詞名が、[System.Management.Automation.VerbsOther](/dotnet/api/System.Management.Automation.VerbsOther) クラスを使用して定義されています。

|動詞 (エイリアス)|アクション|避けるべきシノニム|
|--------------------|------------|--------------|
|[Use](/dotnet/api/System.Management.Automation.VerbsOther.Use) (u)|リソースを使用または組み込んで何らかの処理を行います。||

## <a name="see-also"></a>参照

- [System.Management.Automation.VerbsCommon](/dotnet/api/System.Management.Automation.VerbsCommon)
- [System.Management.Automation.VerbsCommunications](/dotnet/api/System.Management.Automation.VerbsCommunications)
- [System.Management.Automation.VerbsData](/dotnet/api/System.Management.Automation.VerbsData)
- [System.Management.Automation.VerbsDiagnostic](/dotnet/api/System.Management.Automation.VerbsDiagnostic)
- [System.Management.Automation.VerbsLifeCycle](/dotnet/api/System.Management.Automation.VerbsLifeCycle)
- [System.Management.Automation.VerbsSecurity](/dotnet/api/System.Management.Automation.VerbsSecurity)
- [System.Management.Automation.VerbsOther](/dotnet/api/System.Management.Automation.VerbsOther)
- [コマンドレットの宣言](./cmdlet-class-declaration.md)
- [Windows PowerShell プログラマー ガイド](../prog-guide/windows-powershell-programmer-s-guide.md)
- [Windows PowerShell シェル SDK](../windows-powershell-reference.md)
