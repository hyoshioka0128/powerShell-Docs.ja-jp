---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell プロパティ プロバイダーを作成する
description: Windows PowerShell プロパティ プロバイダーを作成する
ms.openlocfilehash: 5370624afa784598ca784b201f7e7345eb958ff9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "94390917"
---
# <a name="creating-a-windows-powershell-property-provider"></a>Windows PowerShell プロパティ プロバイダーを作成する

このトピックでは、ユーザーがデータストア内の項目のプロパティを操作できるようにするプロバイダーを作成する方法について説明します。 その結果、この種類のプロバイダーは、Windows PowerShell プロパティプロバイダーと呼ばれます。 たとえば、Windows PowerShell によって提供されるレジストリプロバイダーは、レジストリキーの値をレジストリキー項目のプロパティとして処理します。 この種類のプロバイダーは、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider) インターフェイスを .net クラスの実装に追加する必要があります。

> [!NOTE]
> Windows PowerShell には、Windows PowerShell プロバイダーの開発に使用できるテンプレートファイルが用意されています。 TemplateProvider.cs ファイルは、Microsoft Windows Software Development Kit for Windows Vista および .NET Framework 3.0 ランタイムコンポーネントで使用できます。 ダウンロードの手順については、「 [Windows powershell をインストールする方法」および「Windows POWERSHELL SDK をダウンロードする方法](/powershell/scripting/developer/installing-the-windows-powershell-sdk)」を参照してください。
> ダウンロードしたテンプレートは、ディレクトリにあり **\<PowerShell Samples>** ます。 このファイルのコピーを作成し、新しい Windows PowerShell プロバイダーを作成するためにコピーを使用して、不要な機能を削除する必要があります。 その他の Windows PowerShell プロバイダーの実装の詳細については、「 [Windows Powershell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」を参照してください。

> [!CAUTION]
> プロパティプロバイダーのメソッドは、System........................ [Writepropertyobject *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WritePropertyObject) メソッドを使用してオブジェクトを書き込む必要があります。

## <a name="defining-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーの定義

プロパティプロバイダーは、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider) インターフェイスをサポートする .net クラスを作成する必要があります。 Windows PowerShell によって提供される TemplateProvider.cs ファイルの既定のクラス宣言を次に示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyproviderclassdeclaration](Msh_samplestestcmdlets#testcmdletspropertyproviderclassdeclaration)]  -->

## <a name="defining-base-functionality"></a>基本機能の定義

[Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider)インターフェイスは、Drivecmdletprovider クラスを除き、どのプロバイダーの基底クラスにもアタッチできます。このインターフェイスには、 [](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider)クラスが含まれています。 使用する基本クラスに必要な基本機能を追加します。 基本クラスの詳細については、「 [Windows PowerShell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」を参照してください。

## <a name="retrieving-properties"></a>プロパティの取得

プロパティを取得するには、プロバイダーが[Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty)メソッドを実装して、コマンドレットからの呼び出しをサポートする必要があります。 `Get-ItemProperty` このメソッドは、指定されたプロバイダー内部パス (完全修飾) にある項目のプロパティを取得します。

パラメーターは、 `providerSpecificPickList` 取得するプロパティを示します。 このパラメーターが `null` または空の場合、メソッドはすべてのプロパティを取得する必要があります。 さらに、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty) は、取得されたプロパティのプロパティバッグを表す、 [system.servicemodel オブジェクトの](/dotnet/api/System.Management.Automation.PSObject) インスタンスを書き込んでいることを意味しています。 メソッドは何も返しません。

[Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty)の実装では、選択リストの各要素に対して、プロパティ名のワイルドカード展開がサポートされていることをお勧めします。 これを行うには、 [Wildcardpattern](/dotnet/api/System.Management.Automation.WildcardPattern) クラスを使用して、ワイルドカードのパターンマッチングを実行します。

Windows PowerShell によって提供される TemplateProvider.cs ファイルからの [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty) の既定の実装を次に示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyprovidergetproperty](Msh_samplestestcmdlets#testcmdletspropertyprovidergetproperty)]  -->

#### <a name="things-to-remember-about-implementing-getproperty"></a>GetProperty の実装に関する注意事項

[Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell プロパティプロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetProperty) メソッドの実装では、メソッドに渡されるパスが、指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドをオーバーライドしても、ユーザーに表示されないオブジェクトのリーダーを取得することはできません。この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 パスが、ユーザーおよびシステムから非表示になっている項目を表している場合は、エラーを書き込む必要があります。 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force)をに設定します。 `false`

## <a name="attaching-dynamic-parameters-to-the-get-itemproperty-cmdlet"></a>Get-ItemProperty コマンドレットへの動的パラメーターのアタッチ

`Get-ItemProperty`コマンドレットでは、実行時に動的に指定される追加のパラメーターが必要になる場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell プロパティプロバイダーで [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetPropertyDynamicParameters) メソッドを実装する必要があります。 パラメーターは、 `path` 完全修飾プロバイダーの内部パスを示し `providerSpecificPickList` ます。パラメーターは、コマンドラインで入力されたプロバイダー固有のプロパティを指定します。 `null`プロパティがコマンドレットにパイプされている場合、このパラメーターはまたは空になることがあります。 この場合、このメソッドは、コマンドレットクラスまたは system.servicemodel 型の [Parameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) オブジェクトと同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加します。

ここでは、Windows PowerShell によって提供される TemplateProvider.cs ファイルからの [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetPropertyDynamicParameters) の既定の実装を示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyprovidergetpropertydynamicparameters](Msh_samplestestcmdlets#testcmdletspropertyprovidergetpropertydynamicparameters)]  -->

## <a name="setting-properties"></a>プロパティの設定

プロパティを設定するには、コマンドレットからの呼び出しをサポートするために、Windows PowerShell プロパティプロバイダーで [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty) メソッドを実装する必要があり `Set-ItemProperty` ます。 このメソッドは、指定されたパスにある項目の1つ以上のプロパティを設定し、必要に応じて、指定されたプロパティを上書きします。
また、 [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty)は、更新されたプロパティのプロパティバッグを表す、system.servicemodel[オブジェクトの](/dotnet/api/System.Management.Automation.PSObject)インスタンスも書き込みます。このオブジェクトは、更新されたプロパティのプロパティバッグを表します。

ここでは、Windows PowerShell によって提供される TemplateProvider.cs ファイルからの [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty) の既定の実装を示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyprovidersetproperty](Msh_samplestestcmdlets#testcmdletspropertyprovidersetproperty)]  -->

#### <a name="things-to-remember-about-implementing-set-itemproperty"></a>Set-ItemProperty の実装について覚えておくべきこと

[Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell プロパティプロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty) メソッドを実装することで、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドをオーバーライドしても、ユーザーに表示されないオブジェクトのリーダーを取得することはできません。この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 パスが、ユーザーおよびシステムから非表示になっている項目を表している場合は、エラーを書き込む必要があります。 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force)をに設定します。 `false`

- [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty)メソッドを実装するには、システムを呼び出す必要があります。データストアに変更を加える前に、その戻り値を確認し、その戻り値を確認し[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess) このメソッドは、ファイル名の変更など、システム状態が変更されたときの操作の実行を確認するために使用されます。
  Windows PowerShell ランタイムを使用して、ユーザーに変更されるリソースの名前を送信し、表示される内容を決定するためにコマンドライン設定またはユーザー設定変数を処理して[います。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)

  Ipropertycmdletprovider の呼び出しによってが返された後、危険性の高いシステム変更を行うことができる場合は、システムを呼び出す必要があります。その[ためには](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)、 `true` [](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetProperty)メソッドを呼び出す必要[](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)があります。このメソッドを呼び出す必要があります。 このメソッドは、ユーザーに確認メッセージを送信して、操作を続行する必要があることを示す追加のフィードバックを許可します。

## <a name="attaching-dynamic-parameters-for-the-set-itemproperty-cmdlet"></a>Set-ItemProperty コマンドレットの動的パラメーターのアタッチ

`Set-ItemProperty`コマンドレットでは、実行時に動的に指定される追加のパラメーターが必要になる場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell プロパティプロバイダーで [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.SetPropertyDynamicParameters) メソッドを実装する必要があります。 このメソッドは、コマンドレットクラスや [system.string オブジェクトと](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) 同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 `null`動的パラメーターを追加する必要がない場合は、値を返すことができます。

ここでは、Windows PowerShell によって提供される TemplateProvider.cs ファイルからの [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.GetPropertyDynamicParameters) の既定の実装を示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyprovidersetpropertydynamicparameters](Msh_samplestestcmdlets#testcmdletspropertyprovidersetpropertydynamicparameters)]  -->

## <a name="clearing-properties"></a>プロパティのクリア

プロパティをクリアするには、コマンドレットからの呼び出しをサポートするために、Windows PowerShell プロパティプロバイダーで [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty) メソッドを実装する必要があり `Clear-ItemProperty` ます。 このメソッドは、指定されたパスにある項目の1つ以上のプロパティを設定します。

Windows PowerShell によって提供される TemplateProvider.cs ファイルの [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty) の既定の実装を次に示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyproviderclearproperty](Msh_samplestestcmdlets#testcmdletspropertyproviderclearproperty)]  -->

#### <a name="thing-to-remember-about-implementing-clearproperty"></a>ClearProperty の実装について覚えておくべきこと

[Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell プロパティプロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合、 [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty) メソッドを実装するには、メソッドに渡されるパスが、指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドをオーバーライドしても、ユーザーに表示されないオブジェクトのリーダーを取得することはできません。この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されている必要があり `true` ます。 パスが、ユーザーおよびシステムから非表示になっている項目を表している場合は、エラーを書き込む必要があります。 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force)をに設定します。 `false`

- [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty)メソッドを実装するには、その前に system.object[を呼び出して](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)、データストアに変更を加える前に戻り値を確認する必要があるということを確認してください。 このメソッドは、コンテンツのクリアなど、システム状態が変更される前に操作の実行を確認するために使用されます。
  このコマンドは、変更されるリソースの名前をユーザー[に送信します。 Windows](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess) PowerShell ランタイムは、表示する内容を決定する際に、コマンドライン設定やユーザー設定変数を考慮します。

  Ipropertycmdletprovider の[呼び出しによってが返され](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)た後、危険なシステムの変更が発生する可能性がある場合は、システムを呼び出すメソッドを呼び出す必要があります。このメソッドは、......... `true` [.....。](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearProperty) [](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue) このメソッドは、ユーザーに確認メッセージを送信して、危険な可能性のある操作を続行する必要があることを示す追加のフィードバックを許可します。

## <a name="attaching-dynamic-parameters-to-the-clear-itemproperty-cmdlet"></a>Clear-ItemProperty コマンドレットへの動的パラメーターのアタッチ

`Clear-ItemProperty`コマンドレットでは、実行時に動的に指定される追加のパラメーターが必要になる場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell プロパティプロバイダーで [Ipropertycmdletprovider *](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearPropertyDynamicParameters) メソッドを実装する必要があります。 このメソッドは、コマンドレットクラスや [system.string オブジェクトと](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) 同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 `null`動的パラメーターを追加する必要がない場合は、値を返すことができます。

ここでは、Windows PowerShell によって提供される TemplateProvider.cs ファイルからの [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider.ClearPropertyDynamicParameters) の既定の実装を示します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testcmdletspropertyproviderclearpropertydynamicparameters](Msh_samplestestcmdlets#testcmdletspropertyproviderclearpropertydynamicparameters)]  -->

## <a name="building-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーの構築

「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法」を](/previous-versions//ms714644(v=vs.85))参照してください。

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダー](./designing-your-windows-powershell-provider.md)

[Windows PowerShell プロバイダーを設計する](./designing-your-windows-powershell-provider.md)

[オブジェクトの種類と書式設定の拡張](/previous-versions//ms714665(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))
