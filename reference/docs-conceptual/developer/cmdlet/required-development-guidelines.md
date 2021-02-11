---
ms.date: 09/13/2016
ms.topic: reference
title: 必要な開発ガイドライン
description: 必要な開発ガイドライン
ms.openlocfilehash: 98db075b314eb7f54f2deb56022799d9f830f9ef
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92655760"
---
# <a name="required-development-guidelines"></a>必要な開発ガイドライン

コマンドレットを記述するときは、次のガイドラインに従う必要があります。 コマンドレットを設計するためのガイドラインと、コマンドレットコードを記述するためのガイドラインに分けられています。 これらのガイドラインに従っていない場合、コマンドレットは失敗し、コマンドレットを使用するとユーザーのエクスペリエンスが低下する可能性があります。

## <a name="in-this-topic"></a>このトピックの内容

### <a name="design-guidelines"></a>設計ガイドライン

- [承認された動詞のみを使用する (RD01)](./required-development-guidelines.md#use-only-approved-verbs-rd01)

- [コマンドレット名: 使用できない文字 (RD02)](./required-development-guidelines.md#cmdlet-names-characters-that-cannot-be-used-rd02)

- [使用できないパラメーター名 (RD03)](./required-development-guidelines.md#parameters-names-that-cannot-be-used-rd03)

- [確認要求のサポート (RD04)](./required-development-guidelines.md#support-confirmation-requests-rd04)

- [対話型セッションの Force パラメーターのサポート (RD05)](./required-development-guidelines.md#support-force-parameter-for-interactive-sessions-rd05)

- [ドキュメント出力オブジェクト (RD06)](./required-development-guidelines.md#document-output-objects-rd06)

### <a name="code-guidelines"></a>コードのガイドライン

- [コマンドレットまたは PSCmdlet クラスからの派生 (RC01)](./required-development-guidelines.md#derive-from-the-cmdlet-or-pscmdlet-classes-rc01)

- [コマンドレットの属性 (RC02) を指定します。](./required-development-guidelines.md#specify-the-cmdlet-attribute-rc02)

- [入力処理メソッドのオーバーライド (RC03)](./required-development-guidelines.md#override-an-input-processing-method-rc03)

- [OutputType 属性の指定 (RC04)](./required-development-guidelines.md#specify-the-outputtype-attribute-rc04)

- [出力オブジェクトのハンドルを保持しない (RC05)](./required-development-guidelines.md#do-not-retain-handles-to-output-objects-rc05)

- [エラーを確実に処理する (RC06)](./required-development-guidelines.md#handle-errors-robustly-rc06)

- [Windows PowerShell モジュールを使用してコマンドレットを展開する (RC07)](./required-development-guidelines.md#use-a-windows-powershell-module-to-deploy-your-cmdlets-rc07)

## <a name="design-guidelines"></a>設計ガイドライン

コマンドレットとその他のコマンドレットを使用して一貫したユーザーエクスペリエンスを実現するために、コマンドレットを設計する際には、次のガイドラインに従う必要があります。 状況に当てはまる設計ガイドラインが見つかったら、同様のガイドラインのコードガイドラインを確認してください。

### <a name="use-only-approved-verbs-rd01"></a>承認された動詞のみを使用する (RD01)

コマンドレット属性で指定する動詞は、Windows PowerShell によって提供される、認識された動詞のセットから取得する必要があります。 禁止されたシノニムの1つである必要はありません。 次の列挙体で定義されている定数文字列を使用して、コマンドレット動詞を指定します。

- [System.Management.Automation.VerbsCommon](/dotnet/api/System.Management.Automation.VerbsCommon)

- [System.Management.Automation.VerbsCommunications](/dotnet/api/System.Management.Automation.VerbsCommunications)

- [System.Management.Automation.VerbsData](/dotnet/api/System.Management.Automation.VerbsData)

- [System.Management.Automation.VerbsDiagnostic](/dotnet/api/System.Management.Automation.VerbsDiagnostic)

- [System.Management.Automation.VerbsLifeCycle](/dotnet/api/System.Management.Automation.VerbsLifeCycle)

- [System.Management.Automation.VerbsSecurity](/dotnet/api/System.Management.Automation.VerbsSecurity)

- [System.Management.Automation.VerbsOther](/dotnet/api/System.Management.Automation.VerbsOther)

承認された動詞名の詳細については、「 [コマンドレットの動詞](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

ユーザーには、検出可能な一連のコマンドレット名が必要です。 適切な動詞を使用して、ユーザーがコマンドレットの動作を迅速に評価し、システムの機能を簡単に検出できるようにします。 たとえば、次のコマンドラインコマンドは、名前が "start" で始まるシステム上のすべてのコマンドの一覧を取得し `get-command start-*` ます。 コマンドレットの名詞を使用して、他のコマンドレットとコマンドレットを区別します。 名詞は、操作が実行されるリソースを示します。 操作自体は動詞によって表されます。

### <a name="cmdlet-names-characters-that-cannot-be-used-rd02"></a>コマンドレット名: 使用できない文字 (RD02)

コマンドレットに名前を指定するときは、次の特殊文字は使用しないでください。

|文字|名前|
|---------------|----------|
|#|数値の符号|
|,|コンマ|
|()|かっこ|
|{}|かっこ|
|[]|カッコ|
|&|アンパサンド|
|-|ハイフン **メモ:**  ハイフンを使用して、名詞から動詞を区切ることができますが、名詞内または動詞内で使用することはできません。|
|/|スラッシュ記号|
|\\| 円記号|
|$|ドル記号|
|^|caret|
|;|セミコロン|
|:|:)|
|"|二重引用符|
|'|単一引用符|
|<>|山かっこ|
|&#124;|縦棒|
|?|疑問符|
|@|アットマーク記号|
|`|バックチック (アクサングラーブ)|
|*|アスタリスク|
|%|パーセント記号|
|+|プラス記号|
|=|等号 (=)|
|~|否定|

### <a name="parameters-names-that-cannot-be-used-rd03"></a>使用できないパラメーター名 (RD03)

Windows PowerShell は、すべてのコマンドレットにパラメーターを設定し、特定の状況で追加されるパラメーターを追加します。 独自のコマンドレットを設計する場合、Confirm、Debug、ErrorAction、Erroraction、OutBuffer、Outbuffer、Warnings Action、Warnings Variable、WhatIf、UseTransaction、Verbose の各名前は使用できません。 これらのパラメーターの詳細については、「 [共通パラメーター名](./common-parameter-names.md)」を参照してください。

### <a name="support-confirmation-requests-rd04"></a>確認要求のサポート (RD04)

システムを変更する操作を実行するコマンドレットについては、確認を要求するために、system.string [*](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) メソッドを呼び出す必要があります。また、特殊なケース [では、](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) このメソッドを呼び出します。 (" [System](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) ......................................................... [...](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)

これらの呼び出しを行うには、コマンドレット属性のキーワードを設定して、コマンドレットで確認要求をサポートするように指定する必要があり `SupportsShouldProcess` ます。 この属性の設定の詳細については、「 [コマンドレット属性の宣言](./cmdlet-attribute-declaration.md)」を参照してください。

> [!NOTE]
> コマンドレットクラスの Cmdlet 属性によって、コマンドレットが [システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) の呼び出しをサポートしていることが示されている場合、コマンドレットは、system..... [. コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) の呼び出しを実行できません。また、コマンドレットは、システムを予期せず変更する可能性があります。

システムを変更する場合は、System... [コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) を使用します。 ユーザー設定とパラメーターによって `WhatIf` 、system.string [*](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) メソッドが制御されます。 これに [対して、"..](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) ....................................... このメソッドは、ユーザー設定またはパラメーターによって制御されません `WhatIf` 。 コマンドレットで、system.string [*](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) メソッドを呼び出す場合は、 `Force` この2つのメソッドの呼び出しをバイパスして操作を続行するパラメーターが必要です。 これは、非対話型のスクリプトやホストでコマンドレットを使用できるようにするために重要です。

コマンドレットがこれらの呼び出しをサポートしている場合、ユーザーはアクションを実際に実行するかどうかを判断できます。 たとえば、 [Stop Process](/powershell/module/microsoft.powershell.management/stop-process) コマンドレットは、システム、Winlogon、spoolsv.exe の各プロセスを含む一連の重要なプロセスを停止する前に、system.string [*](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) メソッドを呼び出します。

これらのメソッドのサポートの詳細については、「 [確認の要求](./requesting-confirmation-from-cmdlets.md)」を参照してください。

### <a name="support-force-parameter-for-interactive-sessions-rd05"></a>対話型セッションの Force パラメーターのサポート (RD05)

コマンドレットを対話的に使用する場合は、プロンプトや入力行の読み取りなど、対話型アクションをオーバーライドする Force パラメーターを常に指定します。 これは、非対話型のスクリプトやホストでコマンドレットを使用できるようにするために重要です。 対話型ホストでは、次のメソッドを実装できます。

- [PSHostUserInterface * というメッセージが表示されます。](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface.Prompt)

- [Pshostuserinterface のように選択します。](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface.PromptForChoice)

- [Ihostuisupportsmultiplechoiceselection のように選択します。](/dotnet/api/System.Management.Automation.Host.IHostUISupportsMultipleChoiceSelection.PromptForChoice)

- [Pshostuserinterface * のようにしてください。](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface.PromptForCredential)

- [Pshostuserinterface * のようになります。](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface.ReadLine)

- [Pshostuserinterface. ReadLineAsSecureString * のようになります。](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface.ReadLineAsSecureString)

### <a name="document-output-objects-rd06"></a>ドキュメント出力オブジェクト (RD06)

Windows PowerShell は、パイプラインに書き込まれたオブジェクトを使用します。 各コマンドレットによって返されるオブジェクトをユーザーが利用できるようにするには、返されたオブジェクトを文書化し、返されたオブジェクトのメンバーがどのように使用されているかを文書化する必要があります。

## <a name="code-guidelines"></a>コードのガイドライン

コマンドレットコードを記述するときは、次のガイドラインに従う必要があります。 状況に当てはまるコードガイドラインが見つかった場合は、デザインガイドラインで同様のガイドラインを確認してください。

### <a name="derive-from-the-cmdlet-or-pscmdlet-classes-rc01"></a>コマンドレットまたは PSCmdlet クラスからの派生 (RC01)

コマンドレットは、 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet)基底クラス[またはのいずれ](/dotnet/api/System.Management.Automation.Cmdlet)かから派生する必要があります。 System.servicemodel [クラスから派生した](/dotnet/api/System.Management.Automation.Cmdlet) コマンドレットは、Windows PowerShell ランタイムに依存しません。 これらは、任意の Microsoft .NET Framework 言語から直接呼び出すことができます。 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet)クラスから派生したコマンドレットは、Windows PowerShell ランタイムに依存します。 そのため、実行空間内で実行されます。

実装するすべてのコマンドレットクラスは、パブリッククラスである必要があります。 これらのコマンドレットクラスの詳細については、「 [コマンドレットの概要](./cmdlet-overview.md)」を参照してください。

### <a name="specify-the-cmdlet-attribute-rc02"></a>コマンドレットの属性 (RC02) を指定します。

Windows PowerShell によってコマンドレットが認識されるようにするには、その .NET Framework クラスをコマンドレット属性で修飾する必要があります。 この属性は、コマンドレットの次の機能を指定します。

- コマンドレットを識別する動詞と名詞のペア。

- 複数のパラメーターセットが指定されている場合に使用される既定のパラメーターセット。 既定のパラメーターセットは、Windows PowerShell に、使用するパラメーターセットを決定するのに十分な情報がない場合に使用されます。

- コマンドレットで、システムの呼び出しをサポートするかどうかを示し [ます](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) 。 このメソッドは、コマンドレットによってシステムが変更される前に、ユーザーに確認メッセージを表示します。 確認要求の実行方法の詳細については、「 [確認](./requesting-confirmation-from-cmdlets.md)の要求」を参照してください。

- 確認メッセージに関連付けられているアクションの影響レベル (または重大度) を示します。 ほとんどの場合、既定値の Medium が使用されます。 影響レベルがユーザーに表示される確認要求に与える影響の詳細については、「 [確認の要求](./requesting-confirmation-from-cmdlets.md)」を参照してください。

コマンドレット属性を宣言する方法の詳細については、「コマンドレット [属性の宣言](./cmdlet-attribute-declaration.md)」を参照してください。

### <a name="override-an-input-processing-method-rc03"></a>入力処理メソッドのオーバーライド (RC03)

コマンドレットを Windows PowerShell 環境に参加させるには、次の *入力処理メソッド* の少なくとも1つをオーバーライドする必要があります。

[このメソッド](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) は、1回だけ呼び出され、事前処理機能を提供するために使用されます。

[このメソッド](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) は、複数回呼び出されます。また、レコードごとの機能を提供するために使用されます。

[このメソッド](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) は、1回だけ呼び出され、後処理機能を提供するために使用されます。

### <a name="specify-the-outputtype-attribute-rc04"></a>OutputType 属性の指定 (RC04)

OutputType 属性 (Windows PowerShell 2.0 で導入) では、コマンドレットがパイプラインに返す .NET Framework の種類を指定します。 コマンドレットの出力の種類を指定することで、コマンドレットによって返されるオブジェクトを他のコマンドレットで検出できるようにします。 この属性を使用してコマンドレットクラスを装飾する方法の詳細については、「 [OutputType 属性の宣言](./outputtype-attribute-declaration.md)」を参照してください。

### <a name="do-not-retain-handles-to-output-objects-rc05"></a>出力オブジェクトのハンドルを保持しない (RC05)

コマンドレットは、 [WriteObject *](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject) メソッドに渡されるオブジェクトへのハンドルを保持しないようにする必要があります。 これらのオブジェクトは、パイプラインの次のコマンドレットに渡されるか、スクリプトによって使用されます。 オブジェクトへのハンドルを保持すると、2つのエンティティが各オブジェクトを所有し、エラーが発生します。

### <a name="handle-errors-robustly-rc06"></a>エラーを確実に処理する (RC06)

管理環境は、本質的に、管理しているシステムに対して重要な変更を検出し、それを行います。 このため、コマンドレットでエラーを正しく処理することが重要です。 エラーレコードの詳細については、「 [Windows PowerShell エラー報告](./error-reporting-concepts.md)」を参照してください。

- エラーが原因で、コマンドレットがそれ以上のレコードの処理を続行できない場合は、終了エラーになります。 コマンドレットは、 [ThrowTerminatingError *](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError) メソッドを呼び出す必要があります。このメソッドは、 [このオブジェクトを参照します](/dotnet/api/System.Management.Automation.ErrorRecord) 。 コマンドレットによって例外がキャッチされない場合、Windows PowerShell ランタイム自体は、より少ない情報を含む終了エラーをスローします。

- パイプラインからの次のレコード (たとえば、別のプロセスによって生成されるレコード) での操作を停止しない未終了エラーについては、コマンドレットで[WriteError *](/dotnet/api/System.Management.Automation.Cmdlet.WriteError)メソッドを呼び出す必要があります。このメソッドは、[このオブジェクトを参照します。](/dotnet/api/System.Management.Automation.ErrorRecord) 終了しないエラーの例としては、特定のプロセスの停止に失敗した場合に発生するエラーがあります。 [WriteError *](/dotnet/api/System.Management.Automation.Cmdlet.WriteError)メソッドを呼び出すと、ユーザーは、要求された操作を一貫して実行し、失敗した特定の操作に関する情報を保持することができます。 コマンドレットでは、各レコードを可能な限り個別に処理する必要があります。

- [ThrowTerminatingError *](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError)メソッドと WriteError * メソッドによって参照されている、 [*](/dotnet/api/System.Management.Automation.Cmdlet.WriteError)メソッドのコアでは、例外が必要になります。[このオブジェクトを](/dotnet/api/System.Management.Automation.ErrorRecord)参照してください。 使用する例外を決定するときは、.NET Framework 設計ガイドラインに従ってください。 エラーが意味的に既存の例外と同じ場合は、その例外を使用するか、その例外から派生させます。 それ以外の場合は、新しい例外または例外の階層を [system.exception](/dotnet/api/System.Exception) 型から直接派生させます。

また、 [システムの管理. ErrorRecord](/dotnet/api/System.Management.Automation.ErrorRecord) オブジェクトには、ユーザーのエラーをグループ化するエラーカテゴリも必要です。 ユーザーは、シェル変数の値を category ビューに設定することにより、カテゴリに基づいてエラーを表示でき `$ErrorView` ます。 使用可能なカテゴリは、 [ErrorCategory](/dotnet/api/System.Management.Automation.ErrorCategory) 列挙型によって定義されます。

- コマンドレットによって新しいスレッドが作成され、そのスレッドで実行されているコードがハンドルされない例外をスローした場合、Windows PowerShell はエラーをキャッチせず、プロセスを終了します。

- ハンドルされない例外を発生させるコードがデストラクター内にある場合、Windows PowerShell はエラーをキャッチせず、プロセスを終了します。 これは、オブジェクトがハンドルされない例外の原因となった Dispose メソッドを呼び出した場合にも発生します。

### <a name="use-a-windows-powershell-module-to-deploy-your-cmdlets-rc07"></a>Windows PowerShell モジュールを使用してコマンドレットを展開する (RC07)

コマンドレットをパッケージ化してデプロイするための Windows PowerShell モジュールを作成します。 モジュールのサポートは、Windows PowerShell 2.0 で導入されました。 コマンドレットクラスを含むアセンブリは、バイナリモジュールファイルとして直接使用できます (コマンドレットをテストする場合に非常に便利です)。または、コマンドレットアセンブリを参照するモジュールマニフェストを作成することもできます。 (モジュールを使用する場合は、既存のスナップインアセンブリを追加することもできます)。モジュールの詳細については、「 [Windows PowerShell モジュールの記述](../module/writing-a-windows-powershell-module.md)」を参照してください。

## <a name="see-also"></a>参照

[強くお勧めする開発ガイドライン](./strongly-encouraged-development-guidelines.md)

[お勧めする開発ガイドライン](./advisory-development-guidelines.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
