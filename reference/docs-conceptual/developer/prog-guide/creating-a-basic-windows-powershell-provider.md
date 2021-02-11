---
ms.date: 09/13/2016
ms.topic: reference
title: 基本的な Windows PowerShell プロバイダーを作成する
description: 基本的な Windows PowerShell プロバイダーを作成する
ms.openlocfilehash: 03b5784fd063b5457fc64d92a32e286e3bf9cce4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92647505"
---
# <a name="creating-a-basic-windows-powershell-provider"></a>基本的な Windows PowerShell プロバイダーを作成する

このトピックは、Windows PowerShell プロバイダーを作成する方法を学習するための出発点となります。 ここで説明する基本的なプロバイダーは、プロバイダーを開始および停止するためのメソッドを提供します。このプロバイダーは、データストアにアクセスしたり、データストア内のデータを取得または設定したりする手段を提供するものではなく、すべてのプロバイダーで必要な基本的な機能を提供します。

前述のように、ここで説明する基本的なプロバイダーは、プロバイダーを起動および停止するためのメソッドを実装しています。 Windows PowerShell ランタイムは、これらのメソッドを呼び出して、プロバイダーの初期化と初期化解除を行います。

> [!NOTE]
> このプロバイダーのサンプルは、Windows PowerShell によって提供される AccessDBSampleProvider01.cs ファイルにあります。

## <a name="defining-the-windows-powershell-provider-class"></a>Windows PowerShell プロバイダークラスの定義

Windows PowerShell プロバイダーを作成するための最初の手順は、.NET クラスを定義することです。 この基本プロバイダーでは、というクラスを定義しています。このクラスは、という `AccessDBProvider` [プロバイダー](/dotnet/api/System.Management.Automation.Provider.CmdletProvider) の基底クラスから派生します。

プロバイダークラスは、 `Providers` API 名前空間の名前空間 (たとえば、xxx. PowerShell プロバイダー) に配置することをお勧めします。 このプロバイダーは、 `Microsoft.Samples.PowerShell.Provider` すべての Windows PowerShell プロバイダーのサンプルを実行する名前空間を使用します。

> [!NOTE]
> Windows PowerShell プロバイダーのクラスは、明示的にパブリックとしてマークされている必要があります。 パブリックとしてマークされていないクラスは、既定で内部に設定され、Windows PowerShell ランタイムによって検出されません。

この基本プロバイダーのクラス定義を次に示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample01/AccessDBProviderSample01.cs" range="23-24":::

クラス定義の直前に、構文 [[の表示プロバイダー ()] を使用して、"system.servicemodel" 属性を宣言する必要が[あります。](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute)

必要に応じて、クラスをさらに宣言する属性キーワードを設定できます。 ここで宣言されている system.string 属性に [は、2つのパラメーター](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute) が含まれていることに注意してください。 最初の属性パラメーターは、プロバイダーの既定のわかりやすい名前を指定します。ユーザーは後で変更できます。 2番目のパラメーターは、コマンドの処理中にプロバイダーが Windows PowerShell ランタイムに公開する Windows PowerShell 定義の機能を指定します。 プロバイダー機能に使用できる値は、system.servicemodel [機能](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙型によって定義されます。 これは基本プロバイダーであるため、機能をサポートしていません。

> [!NOTE]
> Windows PowerShell プロバイダーの完全修飾名には、アセンブリ名と、プロバイダーの登録時に Windows PowerShell によって決定されるその他の属性が含まれています。

## <a name="defining-provider-specific-state-information"></a>Provider-Specific 状態情報の定義

Windows PowerShell ランタイムでは必要に応じてプロバイダーインスタンスが作成される [ため、この基本クラス](/dotnet/api/System.Management.Automation.Provider.CmdletProvider) とすべての派生クラスはステートレスと見なされます。 したがって、プロバイダー固有のデータに対して完全な制御と状態の保守が必要な場合は、クラスを system.servicemodel クラスから派生させる必要が[あります。](/dotnet/api/System.Management.Automation.ProviderInfo) 派生クラスでは、状態を維持するために必要なメンバーを定義する必要があります。これにより、Windows PowerShell ランタイムが、プロバイダーを初期化するため [に、このメソッドを](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Start) 呼び出すときに、プロバイダー固有のデータにアクセスできるようになります。

Windows PowerShell プロバイダーは、接続ベースの状態を維持することもできます。 接続状態の維持の詳細については、「 [PowerShell ドライブプロバイダーの作成](./creating-a-windows-powershell-drive-provider.md)」を参照してください。

## <a name="initializing-the-provider"></a>プロバイダーを初期化しています

プロバイダーを初期化するために、windows powershell ランタイムは、Windows PowerShell が起動されたときに、 [システムの起動](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Start) を呼び出します。 ほとんどの場合、プロバイダーはこのメソッドの既定の実装を使用できます。これにより、プロバイダーを記述する [system.servicemodel オブジェクトが返されます](/dotnet/api/System.Management.Automation.ProviderInfo) 。 ただし、追加の初期化情報を追加する場合は、独自の [システム](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Start) を実装する必要があります。このメソッドは、プロバイダーに渡される変更されたバージョンの [system.string オブジェクトを](/dotnet/api/System.Management.Automation.ProviderInfo) 返す、変更されたバージョンのを返します。 一般に、このメソッドは渡された [指定さ](/dotnet/api/System.Management.Automation.ProviderInfo) れた system.servicemodel オブジェクト、または他の初期化情報を含む変更された [system.servicemodel オブジェクトを](/dotnet/api/System.Management.Automation.ProviderInfo) 返します。

この基本プロバイダーは、このメソッドをオーバーライドしません。 ただし、次のコードは、このメソッドの既定の実装を示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplesaccessdbprov01#accessdbprov01ProviderStart](Msh_samplesaccessdbprov01#accessdbprov01ProviderStart)]  -->

プロバイダーは、「 [プロバイダー固有のデータの状態の定義](#defining-provider-specific-state-information)」で説明されているように、プロバイダー固有の情報の状態を維持できます。 この場合、の実装では、派生クラスのインスタンスを返すように、 [このメソッドを](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Start) オーバーライドする必要があります。

## <a name="start-dynamic-parameters"></a>動的パラメーターの開始

プロバイダーによる [システム](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Start) の実装では、追加のパラメーターが必要になる場合があります。 この場合、プロバイダー [は、このメソッドを](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.StartDynamicParameters) オーバーライドして、コマンドレットクラスや [system.string オブジェクトと](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) 同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。この場合、プロバイダーはこのメソッドをオーバーライドして、プロパティとフィールドを持つオブジェクトを返します。

この基本プロバイダーは、このメソッドをオーバーライドしません。 ただし、次のコードは、このメソッドの既定の実装を示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplesaccessdbprov01#accessdbprov01ProviderDynamicParameters](Msh_samplesaccessdbprov01#accessdbprov01ProviderDynamicParameters)]  -->

## <a name="uninitializing-the-provider"></a>プロバイダーの初期化解除

Windows PowerShell プロバイダーが使用するリソースを解放するには、プロバイダーが独自の [システムの管理](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Stop) を実装する必要があります。 このメソッドは、セッションの終了時にプロバイダーを初期化解除するために、Windows PowerShell ランタイムによって呼び出されます。

この基本プロバイダーは、このメソッドをオーバーライドしません。 ただし、次のコードは、このメソッドの既定の実装を示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplesaccessdbprov01#accessdbprov01ProviderStop](Msh_samplesaccessdbprov01#accessdbprov01ProviderStop)]  -->

## <a name="code-sample"></a>コード サンプル

完全なサンプルコードについては、「 [AccessDbProviderSample01 のコードサンプル](./accessdbprovidersample01-code-sample.md)」を参照してください。

## <a name="testing-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーのテスト

Windows powershell プロバイダーが Windows PowerShell に登録されたら、サポートされているコマンドレットをコマンドラインで実行することでテストできます。 この基本プロバイダーでは、新しいシェルを実行し、コマンドレットを使用して `Get-PSProvider` プロバイダーの一覧を取得し、AccessDb プロバイダーが存在することを確認します。

```powershell
Get-PSProvider
```

次のような出力が表示されます。

```Output
Name                 Capabilities                  Drives
----                 ------------                  ------
AccessDb             None                          {}
Alias                ShouldProcess                 {Alias}
Environment          ShouldProcess                 {Env}
FileSystem           Filter, ShouldProcess         {C, Z}
Function             ShouldProcess                 {function}
Registry             ShouldProcess                 {HKLM, HKCU}
```

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)

[Windows PowerShell プロバイダーを設計する](./designing-your-windows-powershell-provider.md)
