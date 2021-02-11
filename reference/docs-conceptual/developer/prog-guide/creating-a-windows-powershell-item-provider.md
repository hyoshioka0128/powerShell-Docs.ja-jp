---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell アイテム プロバイダーを作成する
description: Windows PowerShell アイテム プロバイダーを作成する
ms.openlocfilehash: f98ea90bf9ce7222076a91fb26dc42977c70bff2
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645182"
---
# <a name="creating-a-windows-powershell-item-provider"></a>Windows PowerShell アイテム プロバイダーを作成する

このトピックでは、データストア内のデータを操作できる Windows PowerShell プロバイダーを作成する方法について説明します。 このトピックでは、ストア内のデータの要素を、データストアの "項目" と呼びます。 その結果、ストア内のデータを操作できるプロバイダーは、Windows PowerShell 項目プロバイダーと呼ばれます。

> [!NOTE]
> このプロバイダーの C# ソースファイル (AccessDBSampleProvider03.cs) は、Windows Vista および .NET Framework 3.0 ランタイムコンポーネントの Microsoft Windows Software Development Kit を使用してダウンロードできます。 ダウンロードの手順については、「 [Windows powershell をインストールする方法」および「Windows POWERSHELL SDK をダウンロードする方法](/powershell/scripting/developer/installing-the-windows-powershell-sdk)」を参照してください。
> ダウンロードしたソースファイルは、ディレクトリにあり **\<PowerShell Samples>** ます。 その他の Windows PowerShell プロバイダーの実装の詳細については、「 [Windows Powershell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」を参照してください。

このトピックで説明する Windows PowerShell 項目プロバイダーは、Access データベースからデータの項目を取得します。 この場合、"item" は Access データベースのテーブルまたはテーブル内の行のいずれかです。

## <a name="defining-the-windows-powershell-item-provider-class"></a>Windows PowerShell 項目プロバイダークラスの定義

Windows PowerShell 項目プロバイダーでは、.NET クラスを定義する必要があります。このクラス [の基底クラス](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) から派生します。 このセクションで説明する項目プロバイダーのクラス定義を次に示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample03/AccessDBProviderSample03.cs" range="34-36":::

このクラス定義では、 [system.string 属性に2つのパラメーター](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute) が含まれていることに注意してください。 最初のパラメーターは、Windows PowerShell によって使用されるプロバイダーのわかりやすい名前を指定します。 2番目のパラメーターは、コマンドの処理中にプロバイダーが Windows PowerShell ランタイムに公開する Windows PowerShell 固有の機能を指定します。 このプロバイダーには、Windows PowerShell 固有の機能は追加されていません。

## <a name="defining-base-functionality"></a>基本機能の定義

「 [Windows PowerShell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」で説明されているように、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) クラスは、さまざまなプロバイダー機能を提供する他のいくつかのクラスから派生します。 そのため、Windows PowerShell 項目プロバイダーは、これらのクラスによって提供されるすべての機能を定義する必要があります。

セッション固有の初期化情報を追加したり、プロバイダーで使用されるリソースを解放したりするための機能を実装する方法の詳細については、「 [基本的な Windows PowerShell プロバイダーの作成](./creating-a-basic-windows-powershell-provider.md)」を参照してください。
ただし、ここで説明するプロバイダーを含むほとんどのプロバイダーは、Windows PowerShell によって提供されるこの機能の既定の実装を使用できます。

Windows PowerShell 項目プロバイダーがストア内の項目を操作できるようにするには、データストアにアクセスするために、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) 基底クラスのメソッドを実装する必要があります。 このクラスの実装の詳細については、「 [Windows PowerShell ドライブプロバイダーの作成](./creating-a-windows-powershell-drive-provider.md)」を参照してください。

## <a name="checking-for-path-validity"></a>パスの有効性を確認しています

Windows powershell ランタイムは、データの項目を検索するときに、windows powershell の [動作につい](/previous-versions/ms714658(v=vs.85))ての「Pspath の概念」セクションで定義されているように、プロバイダーへの windows powershell パスを提供します。
Windows PowerShell 項目プロバイダーは、指定されたパスの構文とセマンティックの有効性を検証する必要があります。そのためには、 [System.](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.IsValidPath) .................. を実装します。 `true`パスが有効な場合、このメソッドはを返し `false` ます。それ以外の場合はを返します。 このメソッドの実装では、パスに項目が存在するかどうかを検証しないことに注意してください。ただし、パスの構文的で意味が正しいことだけを確認してください。

ここでは、このプロバイダーのシステムの管理を実装しています。このプロバイダーの [Isvalidpath *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.IsValidPath) メソッド。 この実装では、NormalizePath ヘルパーメソッドを呼び出して、パス内のすべての区切り記号を一様なものに変換することに注意してください。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample03/AccessDBProviderSample03.cs" range="274-298":::

## <a name="determining-if-an-item-exists"></a>項目が存在するかどうかを判断する

パスを確認した後、Windows PowerShell ランタイムは、データの項目がそのパスに存在するかどうかを判断する必要があります。 この種類のクエリをサポートするために、Windows PowerShell 項目プロバイダーは、 [システムの管理](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists) を実装しています。 このメソッド `true` は、指定されたパスで見つかった項目を返し `false` ます (既定値)。それ以外の場合は。

次に示すのは、このプロバイダーの system.servicemodel.................... [Itemexists *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists) メソッドの実装です。 このメソッドは PathIsDrive、ChunkPath、および GetTable helper メソッドを呼び出し、プロバイダーによって定義されたオブジェクトを使用します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample03/AccessDBProviderSample03.cs" range="229-267":::

#### <a name="things-to-remember-about-implementing-itemexists"></a>ItemExists の実装に関する注意事項

次の条件は、システムの実装に適用される場合があります。 [Itemexists *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists):

- プロバイダークラスを定義すると、Windows PowerShell 項目プロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合、 [システム](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists) の実装では、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- このメソッドの実装では、項目がユーザーに表示される可能性のある項目への任意の形式のアクセスを処理する必要があります。 たとえば、ユーザーがファイルシステムプロバイダー (Windows PowerShell によって提供される) を介してファイルに対する書き込みアクセス権を持っていても、 [System.Management.Automation.Provider.Itemcmdletprovider.Itemexists*](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists)読み取りアクセス権を持っていない場合、ファイルはまだ存在しています`true`。 子項目を列挙できるかどうかを確認するために、の実装で親項目を確認することが必要になる場合があります。

## <a name="attaching-dynamic-parameters-to-the-test-path-cmdlet"></a>Test-Path コマンドレットへの動的パラメーターのアタッチ

場合によっては、コマンドレットには、 `Test-Path` 実行時に動的に指定される追加のパラメーターが必要です[。](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists) これらの動的パラメーターを指定するには、Windows PowerShell 項目プロバイダーで、system.servicemodel.............................- [Dynamicsdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExistsDynamicParameters) メソッドを実装します。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Test-Path` ます。

この Windows PowerShell 項目プロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovideritemexistdynamicparameters](Msh_samplestestcmdlets#testprovideritemexistdynamicparameters)]  -->

## <a name="retrieving-an-item"></a>項目を取得する

項目を取得するには、Windows PowerShell 項目プロバイダーが、コマンドレットからの呼び出しをサポートするように、system.string [* Getitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) メソッドをオーバーライドする必要があり `Get-Item` ます。 このメソッドは、 [システム](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject) の.............................. を使用して項目を書き込みます。

次に示すのは、このプロバイダーに対する system.servicemodel......................... [...](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) を実装したものです。 このメソッドは、GetTable と GetRow のヘルパーメソッドを使用して、Access データベース内のテーブルまたはデータテーブル内の行のいずれかの項目を取得します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample03/AccessDBProviderSample03.cs" range="132-163":::

#### <a name="things-to-remember-about-implementing-getitem"></a>GetItem の実装に関する注意事項

次の条件は、 [システム](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem)の実装に適用される可能性があります。...

- プロバイダークラスを定義すると、Windows PowerShell 項目プロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、メソッドに渡されるパスが要件を満たしているかどうかを確認する必要[があります。](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドをオーバーライドしても、通常はユーザーに表示されないオブジェクトを取得しないでください。この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 たとえば、FileSystem プロバイダーの場合は、system [.](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) ................................................... [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) ... [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject) .......。

## <a name="attaching-dynamic-parameters-to-the-get-item-cmdlet"></a>Get-Item コマンドレットへの動的パラメーターのアタッチ

コマンドレットには、 `Get-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell 項目プロバイダーで [Getitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItemDynamicParameters) メソッドを実装する必要があります。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Get-Item` ます。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidergetitemdynamicparameters](Msh_samplestestcmdlets#testprovidergetitemdynamicparameters)]  -->

## <a name="setting-an-item"></a>項目を設定する

項目を設定するには、Windows PowerShell 項目プロバイダーで[Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem)メソッドをオーバーライドして、コマンドレットからの呼び出しをサポートする必要があります。 `Set-Item` このメソッドは、指定されたパスにある項目の値を設定します。

このプロバイダーでは、  [Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) メソッドのオーバーライドは提供されていませんが、 ただし、このメソッドの既定の実装は次のとおりです。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidersetitem](Msh_samplestestcmdlets#testprovidersetitem)]  -->

#### <a name="things-to-remember-about-implementing-setitem"></a>SetItem の実装に関する注意事項

[Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem)の実装には、次の条件が当てはまる場合があります:。

- プロバイダークラスを定義すると、Windows PowerShell 項目プロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) の実装によって、メソッドに渡されるパスが要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドのオーバーライドでは、ユーザーに表示されないオブジェクトを設定または作成することはできません。この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 パスが非表示の項目と [WriteError](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteError) * を表している場合は、このメソッドにエラーを送信する必要があります。 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) は、に設定されてい `false` ます。

- [Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem)メソッドを実装するには、システムを呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)その場合は、データストアに変更を加える前に、その戻り値を確認してから、その戻り値を確認してください。 このメソッドは、ファイルの削除など、データストアに変更が加えられたときの操作の実行を確認するために使用されます。 " [System.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess) .......................................................................

  Setitem を呼び出すと、[が返され](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)ます。このメソッドは、system........................ を呼び出す `true` 必要があり[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) [](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)ます。 このメソッドは、ユーザーにメッセージを送信して、操作を続行する必要があるかどうかを確認するフィードバックを許可します。 System.object を呼び出すと、危険性の高いシステム変更を追加で確認できるように[なります。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)

## <a name="retrieving-dynamic-parameters-for-setitem"></a>SetItem の動的パラメーターの取得

コマンドレットには、 `Set-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell 項目プロバイダーで [Setitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItemDynamicParameters) メソッドを実装する必要があります。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Set-Item` ます。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidersetitemdynamicparameters](Msh_samplestestcmdlets#testprovidersetitemdynamicparameters)]  -->

## <a name="clearing-an-item"></a>項目のクリア

項目をクリアするには、Windows PowerShell 項目プロバイダーが、コマンドレットからの呼び出しをサポートするために、system.object を実装します。 [Clearitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItem)メソッドを実装します。 `Clear-Item` このメソッドは、指定されたパスのデータ項目を消去します。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderclearitem](Msh_samplestestcmdlets#testproviderclearitem)]  -->

#### <a name="things-to-remember-about-implementing-clearitem"></a>ClearItem の実装に関する注意事項

次の条件は、システムの実装に適用される場合があります。 [clearitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItem):

- プロバイダークラスを定義すると、Windows PowerShell 項目プロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合、 [システム](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItem) の実装では、メソッドに渡されるパスが要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドのオーバーライドでは、ユーザーに表示されないオブジェクトを設定または作成することはできません。この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 パスが、ユーザーおよび [WriteError](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteError) * によって非表示になっている項目を表している場合、このメソッドにはエラーを送信する必要があります。 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) はに設定され `false` ます。

- [Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem)メソッドを実装するには、システムを呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)その場合は、データストアに変更を加える前に、その戻り値を確認してから、その戻り値を確認してください。 このメソッドは、ファイルの削除など、データストアに変更が加えられたときの操作の実行を確認するために使用されます。 [System.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess) ........................................................

  Setitem を呼び出すと、[が返され](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)ます。このメソッドは、system........................ を呼び出す `true` 必要があり[](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) [](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)ます。 このメソッドは、ユーザーにメッセージを送信して、操作を続行する必要があるかどうかを確認するフィードバックを許可します。 System.object を呼び出すと、危険性の高いシステム変更を追加で確認できるように[なります。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)

## <a name="retrieve-dynamic-parameters-for-clearitem"></a>ClearItem の動的パラメーターの取得

コマンドレットには、 `Clear-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell 項目プロバイダーで、システムを実装する必要があります。 [Clearitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ClearItemDynamicParameters) メソッド。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Clear-Item` ます。

この項目プロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderclearitemdynamicparameters](Msh_samplestestcmdlets#testproviderclearitemdynamicparameters)]  -->

## <a name="performing-a-default-action-for-an-item"></a>項目の既定のアクションを実行する

Windows PowerShell 項目プロバイダーは、 [Invokedefaultaction *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction) メソッドを実装して、コマンドレットからの呼び出しをサポートできます。これにより、 `Invoke-Item` プロバイダーは、指定されたパスにある項目に対して既定のアクションを実行できます。 たとえば、FileSystem プロバイダーは、このメソッドを使用して、特定の項目に対して ShellExecute を呼び出す場合があります。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderinvokedefaultaction](Msh_samplestestcmdlets#testproviderinvokedefaultaction)]  -->

#### <a name="things-to-remember-about-implementing-invokedefaultaction"></a>InvokeDefaultAction の実装に関する注意事項

[Invokedefaultaction *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction)の実装には、次の条件が適用される場合があります。

- プロバイダークラスを定義すると、Windows PowerShell 項目プロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Invokedefaultaction *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultAction) の実装によって、メソッドに渡されるパスが要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドのオーバーライドでは、ユーザーに対して非表示のオブジェクトを設定または作成することはできません。ただし、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 パスが、ユーザーおよび [WriteError](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteError) * によって非表示になっている項目を表している場合、このメソッドにはエラーを送信する必要があります。 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) はに設定され `false` ます。

## <a name="retrieve-dynamic-parameters-for-invokedefaultaction"></a>InvokeDefaultAction の動的パラメーターの取得

コマンドレットには、 `Invoke-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell 項目プロバイダーでは、system.string [*](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.InvokeDefaultActionDynamicParameters) メソッドを実装する必要があります。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットに動的パラメーターを追加し `Invoke-Item` ます。

この項目プロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderinvokedefaultactiondynamicparameters](Msh_samplestestcmdlets#testproviderinvokedefaultactiondynamicparameters)]  -->

## <a name="implementing-helper-methods-and-classes"></a>ヘルパーメソッドとクラスの実装

この項目プロバイダーは、Windows PowerShell で定義されているパブリックオーバーライドメソッドによって使用されるいくつかのヘルパーメソッドとクラスを実装します。 これらのヘルパーメソッドとクラスのコードについては、「 [コードサンプル](#code-sample) 」セクションを参照してください。

### <a name="normalizepath-method"></a>NormalizePath メソッド

この項目プロバイダーは、パスが一貫した形式であることを保証するために、NormalizePath ヘルパーメソッドを実装します。 指定された形式では、区切り記号として円記号 () が使用され \\ ます。

### <a name="pathisdrive-method"></a>PathIsDrive メソッド

この項目プロバイダーは、PathIsDrive ヘルパーメソッドを実装して、指定されたパスが実際にドライブ名であるかどうかを判断します。

### <a name="chunkpath-method"></a>ChunkPath メソッド

この項目プロバイダーは、プロバイダーが個々の要素を識別できるように、指定されたパスを分割する ChunkPath ヘルパーメソッドを実装します。 パス要素で構成される配列を返します。

### <a name="gettable-method"></a>GetTable メソッド

この項目プロバイダーは、呼び出しで指定されたテーブルに関する情報を表すデータオブジェクトを返す、GetTables ヘルパーメソッドを実装します。

### <a name="getrow-method"></a>GetRow メソッド

この項目プロバイダーの system.servicemodel............... この [メソッドは、](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) GetRows ヘルパーメソッドを呼び出します。 このヘルパーメソッドは、テーブル内の指定された行に関する情報を表す DatabaseRowInfo オブジェクトを取得します。

### <a name="databasetableinfo-class"></a>"クラス"

この項目プロバイダーは、データベース内のデータテーブル内の情報のコレクションを表すデータセットを定義します。 このクラスは、 [Directoryinfo](/dotnet/api/System.IO.DirectoryInfo) クラスに似ています。

サンプル項目プロバイダーは、データベース内のテーブルを定義するテーブル情報オブジェクトのコレクションを返す、データセットメソッドを定義します。 このメソッドには try/catch ブロックが含まれており、すべてのデータベースエラーがエントリが0の行として表示されるようになっています。

### <a name="databaserowinfo-class"></a>DatabaseRowInfo クラス

このアイテムプロバイダーは、データベースのテーブルの行を表す DatabaseRowInfo helper クラスを定義します。 このクラス [は、system.servicemodel](/dotnet/api/System.IO.FileInfo) クラスに似ています。

サンプルプロバイダーは、指定されたテーブルの行情報オブジェクトのコレクションを返す DatabaseRowInfo メソッドを定義します。 このメソッドには、例外をトラップするための try/catch ブロックが含まれています。 エラーが発生すると、行情報は生成されません。

## <a name="code-sample"></a>コード サンプル

完全なサンプルコードについては、「 [AccessDbProviderSample03 のコードサンプル](./accessdbprovidersample03-code-sample.md)」を参照してください。

## <a name="defining-object-types-and-formatting"></a>オブジェクトの種類と書式設定の定義

プロバイダーを作成する場合は、既存のオブジェクトにメンバーを追加したり、新しいオブジェクトを定義したりすることが必要になる場合があります。 完了したら、Windows PowerShell がオブジェクトのメンバーを識別するために使用できる型ファイルと、オブジェクトの表示方法を定義するフォーマットファイルを作成します。 の詳細については、「 [オブジェクトの種類と書式設定の拡張](/previous-versions/ms714665(v=vs.85))」を参照してください。

## <a name="building-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーの構築

「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法」を](/previous-versions/ms714644(v=vs.85))参照してください。

## <a name="testing-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーのテスト

この Windows PowerShell 項目プロバイダーが Windows PowerShell に登録されている場合、プロバイダーの基本およびドライブの機能のみをテストできます。 項目の操作をテストするには、「 [コンテナーの Windows PowerShell プロバイダーの実装](./creating-a-windows-powershell-container-provider.md)」で説明されているコンテナー機能も実装する必要があります。

## <a name="see-also"></a>参照

[Windows PowerShell SDK](../windows-powershell-reference.md)

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)

[Windows PowerShell プロバイダーの設計](./designing-your-windows-powershell-provider.md)

[オブジェクトの種類と書式設定の拡張](/previous-versions/ms714665(v=vs.85))

[Windows PowerShell の動作](/previous-versions/ms714658(v=vs.85))

[コンテナーの Windows PowerShell プロバイダーの作成](./creating-a-windows-powershell-container-provider.md)

[ドライブ Windows PowerShell プロバイダーの作成](./creating-a-windows-powershell-drive-provider.md)

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions/ms714644(v=vs.85))
