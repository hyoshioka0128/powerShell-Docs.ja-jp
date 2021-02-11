---
ms.date: 09/13/2016
ms.topic: reference
title: 強くお勧めする開発ガイドライン
description: 強くお勧めする開発ガイドライン
ms.openlocfilehash: e12fa0d1adc0d7a0dad938457bdcd289736df97c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355240"
---
# <a name="strongly-encouraged-development-guidelines"></a>強くお勧めする開発ガイドライン

このセクションでは、コマンドレットを記述するときに従う必要があるガイドラインについて説明します。 コマンドレットを設計するためのガイドラインと、コマンドレットコードを記述するためのガイドラインに分けられています。 これらのガイドラインは、すべてのシナリオに適用できるとは限りません。 ただし、これらのガイドラインに従っていない場合は、コマンドレットを使用すると、ユーザーのエクスペリエンスが低下する可能性があります。

## <a name="design-guidelines"></a>設計ガイドライン

コマンドレットとその他のコマンドレットを使用して一貫したユーザーエクスペリエンスを実現するために、コマンドレットを設計するときは、次のガイドラインに従う必要があります。 状況に当てはまる設計ガイドラインが見つかったら、同様のガイドラインのコードガイドラインを確認してください。

### <a name="use-a-specific-noun-for-a-cmdlet-name-sd01"></a>コマンドレット名に特定の名詞を使用する (SD01)

コマンドレットの名前付けで使用される名詞は、ユーザーがコマンドレットを検出できるように、非常に固有である必要があります。
"Server" などの汎用名詞を製品名の短縮版にプレフィックスとして使用します。 たとえば、名詞が Microsoft SQL Server のインスタンスを実行しているサーバーを参照している場合は、"SQLServer" などの名詞を使用します。 特定の名詞と承認された動詞の短いリストを組み合わせることにより、ユーザーはコマンドレット名間の重複を回避しながら、機能を迅速に検出して予測することができます。

ユーザーエクスペリエンスを向上させるために、コマンドレット名に対して選択する名詞は単数形にする必要があります。 たとえば、Get プロセスではなく名前を使用し `Get-Process` ます。  コマンドレットが複数の項目に対して動作する可能性が高い場合でも、すべてのコマンドレット名に対してこの規則に従うことをお勧めします。

### <a name="use-pascal-case-for-cmdlet-names-sd02"></a>コマンドレット名に Pascal ケースを使用する (SD02)

パラメーター名には Pascal 形式を使用します。 つまり、動詞の最初の文字と名詞で使用されるすべての用語を大文字にします。 (例: "`Clear-ItemProperty`")。

### <a name="parameter-design-guidelines-sd03"></a>パラメーターのデザインガイドライン (SD03)

コマンドレットには、操作が必要なデータを受け取るパラメーターと、操作の特性を決定するために使用される情報を示すパラメーターが必要です。 たとえば、コマンドレットにはパイプラインからデータを受け取るパラメーターがあり、コマンドレットには、 `Name` `Force` コマンドレットが強制的に操作を実行できることを示すパラメーターがある場合があります。 コマンドレットで定義できるパラメーターの数に制限はありません。

#### <a name="use-standard-parameter-names"></a>標準パラメーター名を使用する

コマンドレットでは、ユーザーが特定のパラメーターの意味をすばやく判断できるように、標準のパラメーター名を使用する必要があります。 より具体的な名前が必要な場合は、標準パラメーター名を使用し、別名としてより具体的な名前を指定します。 たとえば、 `Get-Service` コマンドレットには、汎用名 ( `Name` ) とより具体的な別名 () を持つパラメーターがあり `ServiceName` ます。 両方の用語を使用してパラメーターを指定できます。

パラメーター名とそのデータ型の詳細については、「 [コマンドレットパラメーターの名前と機能のガイドライン](./standard-cmdlet-parameter-names-and-types.md)」を参照してください。

#### <a name="use-singular-parameter-names"></a>単数形のパラメーター名を使用する

値が1つの要素であるパラメーターに複数形の名前を使用することは避けてください。 これには、配列またはリストを受け取るパラメーターが含まれます。これは、ユーザーが1つの要素のみを含む配列またはリストを指定する可能性があるためです。

複数形のパラメーター名は、パラメーターの値が常に複数要素の値である場合にのみ使用してください。 このような場合、コマンドレットは複数の要素が指定されていることを確認する必要があり、複数の要素が指定されていない場合は、コマンドレットによってユーザーに警告が表示されます。

#### <a name="use-pascal-case-for-parameter-names"></a>パラメーター名に Pascal ケースを使用する

パラメーター名には Pascal 形式を使用します。 つまり、名前の最初の文字を含め、パラメーター名に含まれる各単語の最初の文字を大文字にします。 たとえば、パラメーター名では、 `ErrorAction` 大文字小文字が正しく使用されます。 次のパラメーター名では大文字と小文字が正しく使用されません。

- `errorAction`
- `erroraction`

#### <a name="parameters-that-take-a-list-of-options"></a>オプションの一覧を受け取るパラメーター

一連のオプションから値を選択できるパラメーターを作成するには、次の2つの方法があります。

- 有効な値を指定する列挙型を定義します (または、既存の列挙型を使用します)。
  次に、列挙型を使用して、その型のパラメーターを作成します。

- パラメーター宣言に **Validateset** 属性を追加します。 この属性の詳細については、「 [Validateset 属性の宣言](./validateset-attribute-declaration.md)」を参照してください。

#### <a name="use-standard-types-for-parameters"></a>パラメーターに標準の型を使用する

他のコマンドレットとの一貫性を確保するには、可能な限りパラメーターに標準の型を使用します。 異なるパラメーターに使用する型の詳細については、「 [標準のコマンドレットパラメーター名と型](./standard-cmdlet-parameter-names-and-types.md)」を参照してください。 このトピックでは、"アクティビティパラメーター" など、標準パラメーターのグループの名前と .NET Framework 型について説明しているいくつかのトピックへのリンクを示します。

#### <a name="use-strongly-typed-net-framework-types"></a>Strongly-Typed .NET Framework の種類を使用する

パラメーターの検証を強化するには、パラメーターを .NET Framework 型として定義する必要があります。 たとえば、値のセットから1つの値に制限されているパラメーターは、列挙型として定義する必要があります。 Uniform Resource Identifier (URI) 値をサポートするには、パラメーターを system.string [型と](/dotnet/api/System.Uri) して定義します。 自由形式のテキストプロパティ以外のすべてに対して、基本的な文字列パラメーターを使用しないようにします。

#### <a name="use-consistent-parameter-types"></a>一貫したパラメーターの型を使用する

複数のコマンドレットで同じパラメーターを使用する場合は、常に同じパラメーターの型を使用します。 たとえば、 `Process` パラメーターが1つのコマンドレットの [Int16](/dotnet/api/System.Int16) 型である場合、 `Process` 別のコマンドレットのパラメーターを [Uint16](/dotnet/api/System.UInt16) 型にしないでください。

#### <a name="parameters-that-take-true-and-false"></a>True および False を受け取るパラメーター

パラメーターがとのみを受け取る場合は `true` `false` 、パラメーターを型 system.string として定義 [します](/dotnet/api/System.Management.Automation.SwitchParameter)。
スイッチパラメーターは、コマンドで指定されている場合と同じように扱われ `true` ます。 パラメーターがコマンドに含まれていない場合、Windows PowerShell はパラメーターの値をと見なし `false` ます。 ブール型パラメーターを定義しないでください。

パラメーターが3つの値 ($true、$false、および "未指定") を区別する必要がある場合は、Null 値を許容する型のパラメーターを定義し \<bool> ます。 3番目の "未指定" 値の必要性は、通常、コマンドレットでオブジェクトのブール型プロパティを変更できる場合に発生します。 この場合、"未指定" は、プロパティの現在の値を変更しないことを意味します。

#### <a name="support-arrays-for-parameters"></a>パラメーターのサポート配列

多くの場合、ユーザーは複数の引数に対して同じ操作を実行する必要があります。 これらのユーザーについては、コマンドレットでパラメーター入力として配列を受け取ることで、ユーザーが Windows PowerShell 変数として引数をパラメーターに渡すことができるようになります。 たとえば、 [Get Process](/powershell/module/Microsoft.PowerShell.Management/Get-Process) コマンドレットは、取得するプロセスの名前を識別する文字列に配列を使用します。

#### <a name="support-the-passthru-parameter"></a>PassThru パラメーターのサポート

既定では、 [Stop Process](/powershell/module/Microsoft.PowerShell.Management/Stop-Process) コマンドレットなど、システムを変更する多くのコマンドレットは、オブジェクトの "シンク" として機能し、結果を返しません。 これらのコマンドレットは、パラメーターを実装して、 `PassThru` コマンドレットが強制的にオブジェクトを返すようにする必要があります。 パラメーターを指定した場合 `PassThru` 、コマンドレットは [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject) メソッドの呼び出しを使用してオブジェクトを返します。 たとえば、次のコマンドを実行すると、Calc プロセスが停止され、結果のプロセスがパイプラインに渡されます。

```powershell
Stop-Process calc -passthru
```

ほとんどの場合、Add、Set、および New コマンドレットではパラメーターをサポートする必要があり `PassThru` ます。

#### <a name="support-parameter-sets"></a>パラメーターセットのサポート

コマンドレットは、単一の目的を実現することを目的としています。 ただし、多くの場合、操作または操作ターゲットを記述する方法は複数あります。 たとえば、プロセスは名前、識別子、またはプロセスオブジェクトによって識別される場合があります。 コマンドレットは、ターゲットのすべての適切な表現をサポートする必要があります。 通常、コマンドレットは、一緒に動作するパラメーターのセット (パラメーターセットと呼ばれます) を指定することによって、この要件を満たします。 1つのパラメーターは、任意の数のパラメーターセットに属することができます。 パラメーターセットの詳細については、「 [コマンドレットパラメーターセット](./cmdlet-parameter-sets.md)」を参照してください。

パラメーターセットを指定する場合は、[ValueFromPipeline に設定] でパラメーターを1つだけ設定します。 **パラメーター** 属性を宣言する方法の詳細については、「 [parameterattribute 宣言](./parameter-attribute-declaration.md)」を参照してください。

パラメーターセットを使用する場合、既定のパラメーターセットは **コマンドレット** 属性によって定義されます。 既定のパラメーターセットには、対話型の Windows PowerShell セッションで使用される可能性が最も高いパラメーターを含める必要があります。 コマンドレット属性を宣言する方法の詳細については、「**コマンドレット**[属性の宣言](./cmdlet-attribute-declaration.md)」を参照してください。

### <a name="provide-feedback-to-the-user-sd04"></a>ユーザーにフィードバックを提供する (SD04)

このセクションのガイドラインを使用して、ユーザーにフィードバックを提供します。 このフィードバックによって、ユーザーはシステムで発生していることを認識し、管理上の決定をより適切に行うことができます。

ユーザーは、Windows PowerShell ランタイムを使用して、ユーザー設定変数を設定することによって、メソッドの各呼び出しからの出力の処理方法を指定でき `Write` ます。 ユーザーは、システムが情報を表示するかどうかを決定する変数や、追加のアクションを実行する前にシステムがユーザーを照会する必要があるかどうかを判断する変数など、いくつかのユーザー設定変数を設定できます。

#### <a name="support-the-writewarning-writeverbose-and-writedebug-methods"></a>WriteWarning、Writewarning、および Writewarning メソッドのサポート

コマンドレットで、予期しない結果になる可能性のある操作を実行しようとしているときに、コマンドレットでは、 [このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.WriteWarning) 呼び出す必要があります。 たとえば、コマンドレットで読み取り専用ファイルを上書きする場合は、コマンドレットでこのメソッドを呼び出す必要があります。

コマンドレットでは、コマンドレットの実行内容についての詳細が必要な場合 [に、system.servicemodel メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.WriteVerbose) 呼び出す必要があります。 たとえば、コマンドレットの作成者が、コマンドレットの動作に関する詳細情報を必要とするシナリオがあると感じる場合は、コマンドレットでこの情報を呼び出す必要があります。

開発者または製品サポートエンジニアが、コマンドレットの操作を破損していることを理解する必要がある場合は、コマンドレットで [system.servicemodel メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug) 呼び出す必要があります。 このコマンドレットでは、両方の情報セットを指定するため、このメソッドを呼び出すのと同じコードで、 [このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.WriteDebug) 呼び出す必要はありません。 [このメソッドは](/dotnet/api/System.Management.Automation.Cmdlet.WriteVerbose) 、パラメーターによって `Debug` 両方の情報が提示されるためです。

#### <a name="support-writeprogress-for-operations-that-take-a-long-time"></a>長い時間がかかる操作の WriteProgress をサポートする

完了までに時間がかかり、バックグラウンドで実行できないコマンドレット操作は、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.WriteProgress) の定期的な呼び出しによる進行状況の報告をサポートする必要があります。

#### <a name="use-the-host-interfaces"></a>ホストインターフェイスを使用する

場合によっては、コマンドレットが、の代わりにユーザーと直接通信する必要があります。そのためには、の代わりに、 [System.](/dotnet/api/System.Management.Automation.Cmdlet) .. cmdlet クラスでサポートされているさまざまな Write メソッドまたは if メソッドを使用します。 この場合、コマンドレットは、 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) クラスから派生し、 [PSCmdlet *](/dotnet/api/System.Management.Automation.PSCmdlet.Host) プロパティを使用する必要があります。この例では、 このプロパティは、PromptForChoice、Prompt、および WriteLine/ReadLine の種類など、さまざまなレベルの通信の種類をサポートしています。 最も具体的なレベルでは、個々のキーの読み取りと書き込みを行ったり、バッファーを処理したりする方法も提供されます。

コマンドレットがグラフィカルユーザーインターフェイス (GUI) を生成するように設計されていない限り、 [PSCmdlet *](/dotnet/api/System.Management.Automation.PSCmdlet.Host) プロパティを使用してホストをバイパスすることはできません。 GUI を生成するように設計されたコマンドレットの例としては、 [Out GridView](/powershell/module/Microsoft.PowerShell.Utility/Out-GridView) コマンドレットがあります。

> [!NOTE]
> コマンドレットで [は、System.string API を使用](/dotnet/api/System.Console) しないでください。

### <a name="create-a-cmdlet-help-file-sd05"></a>コマンドレットのヘルプファイルを作成する (SD05)

各コマンドレットアセンブリについて、コマンドレットに関する情報を含む Help.xml ファイルを作成します。 この情報には、コマンドレットの説明、コマンドレットのパラメーターの説明、コマンドレットの使用例などが含まれます。

## <a name="code-guidelines"></a>コードのガイドライン

コマンドレットとその他のコマンドレットを使用して一貫したユーザーエクスペリエンスを確保するには、次のガイドラインに従う必要があります。 状況に当てはまるコードガイドラインが見つかった場合は、デザインガイドラインで同様のガイドラインを確認してください。

### <a name="coding-parameters-sc01"></a>パラメーターのコーディング (SC01)

**パラメーター属性で** 修飾されたコマンドレットクラスのパブリックプロパティを宣言して、パラメーターを定義します。 パラメーターは、コマンドレットの派生 .NET Framework クラスの静的メンバーである必要はありません。 **パラメーター** 属性を宣言する方法の詳細については、「[パラメーター属性の宣言](./parameter-attribute-declaration.md)」を参照してください。

#### <a name="support-windows-powershell-paths"></a>Windows PowerShell パスのサポート

Windows PowerShell のパスは、名前空間へのアクセスを標準化するためのメカニズムです。 コマンドレットのパラメーターに Windows PowerShell のパスを割り当てると、ユーザーは、特定のパスへのショートカットとして機能するカスタムの "ドライブ" を定義できます。 ユーザーがこのようなドライブを指定すると、レジストリ内のデータなどの格納されたデータを一貫した方法で使用できます。

コマンドレットで、ユーザーがファイルまたはデータソースを指定できるようにするには、 [system.string](/dotnet/api/System.String)型のパラメーターを定義する必要があります。 複数のドライブがサポートされている場合、その型は配列である必要があります。 パラメーターの名前は、のエイリアスを持つである必要があり `Path` `PSPath` ます。
また、 `Path` パラメーターはワイルドカード文字をサポートする必要があります。 ワイルドカード文字のサポートが不要な場合は、 `LiteralPath` パラメーターを定義します。

コマンドレットで読み取りまたは書き込みを行うデータをファイルにする必要がある場合、コマンドレットは Windows PowerShell パス入力を受け入れる必要があります。また、コマンドレットは、 [Sessionstate](/dotnet/api/System.Management.Automation.SessionState.Path) プロパティを使用して、windows powershell パスをファイルシステムが認識するパスに変換します。 具体的なメカニズムには、次のメソッドがあります。

- [PSCmdlet. GetResolvedProviderPathFromPSPath (システムの管理)](/dotnet/api/System.Management.Automation.PSCmdlet.GetResolvedProviderPathFromPSPath)
- [PSCmdlet. GetUnresolvedProviderPathFromPSPath (システムの管理)](/dotnet/api/System.Management.Automation.PSCmdlet.GetUnresolvedProviderPathFromPSPath)
- [GetResolvedProviderPathFromPSPath (システムの管理)](/dotnet/api/System.Management.Automation.PathIntrinsics.GetResolvedProviderPathFromPSPath)
- [GetUnresolvedProviderPathFromPSPath (システムの管理)](/dotnet/api/System.Management.Automation.PathIntrinsics.GetUnresolvedProviderPathFromPSPath)

コマンドレットによって読み取りまたは書き込みが行うデータがファイルではなく文字列のセットである場合、コマンドレットではプロバイダーのコンテンツ情報 ( `Content` メンバー) を使用して読み取りと書き込みを行う必要があります。 この情報は、システムの [管理](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.InvokeProvider) ............. プロパティから取得されます。 これらのメカニズムにより、データの読み取りと書き込みに他のデータストアを参加させることができます。

#### <a name="support-wildcard-characters"></a>ワイルドカード文字のサポート

可能であれば、コマンドレットでワイルドカード文字をサポートする必要があります。 ワイルドカード文字のサポートは、コマンドレットの多くの場所で発生します (特に、パラメーターがオブジェクトのセットから1つのオブジェクトを識別するために文字列を受け取る場合)。 たとえば、 [Stopproc チュートリアル](./stopproc-tutorial.md)のサンプルの **Stop proc** コマンドレットは、 `Name` プロセス名を表す文字列を処理するパラメーターを定義します。 このパラメーターは、ユーザーが停止するプロセスを簡単に指定できるように、ワイルドカード文字をサポートしています。

ワイルドカード文字のサポートが使用可能な場合、コマンドレットの操作では通常、配列が生成されます。
場合によっては、ユーザーが一度に1つの項目のみを使用する可能性があるため、配列をサポートすることは意味がありません。 たとえば、 [Set location](/powershell/module/Microsoft.PowerShell.Management/Set-Location) コマンドレットは、ユーザーが1つの場所のみを設定するため、配列をサポートする必要はありません。 このインスタンスでは、コマンドレットはワイルドカード文字を引き続きサポートしますが、1つの場所を強制的に解決します。

ワイルドカード文字パターンの詳細については、「 [コマンドレットパラメーターでのワイルドカード文字のサポート](./supporting-wildcard-characters-in-cmdlet-parameters.md)」を参照してください。

#### <a name="defining-objects"></a>オブジェクトの定義

ここでは、コマンドレットのオブジェクトを定義し、既存のオブジェクトを拡張するためのガイドラインについて説明します。

##### <a name="define-standard-members"></a>標準メンバーの定義

カスタム Types.ps1xml ファイル内のオブジェクトの種類を拡張する標準メンバーを定義します (Windows PowerShell Types.ps1xml ファイルをテンプレートとして使用します)。 標準メンバーは、PSStandardMembers という名前のノードによって定義されます。 これらの定義により、他のコマンドレットと Windows PowerShell ランタイムは、一貫した方法でオブジェクトを操作できます。

##### <a name="define-objectmembers-to-be-used-as-parameters"></a>パラメーターとして使用する ObjectMembers を定義する

コマンドレットのオブジェクトをデザインする場合は、そのオブジェクトが使用されるコマンドレットのパラメーターに、そのメンバーが直接マップされていることを確認してください。 このマッピングにより、オブジェクトをパイプラインに簡単に送信し、1つのコマンドレットから別のコマンドレットに渡すことができます。

コマンドレットによって返される既存の .NET Framework オブジェクトには、多くの場合、スクリプト開発者やユーザーが必要とする重要なメンバーや便利なメンバーが不足しています。 これらの不足しているメンバーは、表示する場合や、正しいメンバー名を作成する場合に特に重要であり、オブジェクトをパイプラインに正しく渡すことができます。 これらの必要なメンバーを文書化するためのカスタム Types.ps1xml ファイルを作成します。 このファイルを作成するときは、次の名前付け規則に従うことをお勧めします。 *<Your_Product_Name>*.Types.ps1xml です。

たとえば、スクリプトプロパティを追加して、 `Mode` ファイルの[](/dotnet/api/System.IO.FileInfo)属性をより明確に表示することができます。 また、alias プロパティを system.string 型に追加して、 `Count` (ではなく) そのプロパティ名を一貫して使用できるようにすることもでき[ます。](/dotnet/api/System.Array) `Length`

##### <a name="implement-the-icomparable-interface"></a>IComparable インターフェイスを実装する

すべての出力オブジェクトに対して [system.icomparable](/dotnet/api/System.IComparable) インターフェイスを実装します。
これにより、さまざまな並べ替えおよび分析のコマンドレットに出力オブジェクトを簡単にパイプできます。

##### <a name="update-display-information"></a>表示情報の更新

オブジェクトの表示で予期した結果が得られない場合は、 *\<YourProductName>* そのオブジェクトのカスタム.Format.ps1xml ファイルを作成します。

### <a name="support-well-defined-pipeline-input-sc02"></a>適切に定義されたパイプライン入力のサポート (SC02)

#### <a name="implement-for-the-middle-of-a-pipeline"></a>パイプラインの中間に実装する

パイプラインの途中から呼び出されることを前提としてコマンドレットを実装します (つまり、他のコマンドレットが入力を生成したり、出力を使用したりします)。 たとえば、 `Get-Process` コマンドレットがデータを生成するため、パイプラインの最初のコマンドレットとしてのみ使用されるとします。
ただし、このコマンドレットはパイプラインの途中で設計されているため、このコマンドレットを使用すると、パイプライン内の前のコマンドレットまたはデータで、取得するプロセスを指定できます。

#### <a name="support-input-from-the-pipeline"></a>パイプラインからの入力のサポート

コマンドレットの各パラメーターセットに、パイプラインからの入力をサポートするパラメーターを少なくとも1つ含めます。 パイプライン入力のサポートにより、ユーザーはデータまたはオブジェクトを取得し、正しいパラメーターセットに送信し、結果をコマンドレットに直接渡すことができます。

**パラメーター属性に** `ValueFromPipeline` キーワード、 `ValueFromPipelineByPropertyName` キーワード属性、またはその両方のキーワードが宣言内に含まれている場合、パラメーターはパイプラインからの入力を受け取ります。 パラメーターセット内のパラメーターでキーワードまたはキーワードがサポートされていない場合は、 `ValueFromPipeline` `ValueFromPipelineByPropertyName` コマンドレットを別のコマンドレットの後に配置することはできません。これは、パイプラインの入力を無視するためです。

#### <a name="support-the-processrecord-method"></a>ProcessRecord メソッドのサポート

パイプラインで前のコマンドレットのすべてのレコードを受け入れるには、コマンドレットで [、system.object メソッドを](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) 実装する必要があります。 Windows PowerShell は、コマンドレットに送信されるすべてのレコードに対して、このメソッドを複数回呼び出します。

### <a name="write-single-records-to-the-pipeline-sc03"></a>1つのレコードをパイプラインに書き込む (SC03)

コマンドレットによってオブジェクトが返された場合、コマンドレットでは、オブジェクトが生成されるとすぐに書き込みます。 コマンドレットでは、これらを組み合わせた配列にバッファーするためにそれらを保持することはできません。 オブジェクトを入力として受け取るコマンドレットは、遅延なしで出力オブジェクトを処理、表示、または処理し、表示できるようになります。 出力オブジェクトを一度に1つずつ生成するコマンドレットは、 [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject) メソッドを呼び出す必要があります。 出力オブジェクトをバッチで生成するコマンドレット (たとえば、基になる API が出力オブジェクトの配列を返すため) は、2番目のパラメーターをに設定して [WriteObject](/dotnet/api/System.Management.Automation.Cmdlet.WriteObject) メソッドを呼び出す必要があり `true` ます。

### <a name="make-cmdlets-case-insensitive-and-case-preserving-sc04"></a>コマンドレットの Case-Insensitive と Case-Preserving (SC04) を作成する

既定では、Windows PowerShell 自体は大文字と小文字を区別しません。 ただし、既存の多くのシステムを処理するため、Windows PowerShell では、操作と互換性のために大文字と小文字が維持されます。
つまり、文字が大文字で指定されている場合、Windows PowerShell はそれを大文字で保持します。 システムが正常に機能するためには、コマンドレットでこの規則に従う必要があります。 可能であれば、大文字と小文字を区別しない方法で動作します。 ただし、後でコマンドまたはパイプラインで実行されるコマンドレットの場合は、元のケースを保持する必要があります。

## <a name="see-also"></a>参照

[必要な開発ガイドライン](./required-development-guidelines.md)

[お勧めする開発ガイドライン](./advisory-development-guidelines.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
