---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell ナビゲーション プロバイダーを作成する
description: Windows PowerShell ナビゲーション プロバイダーを作成する
ms.openlocfilehash: 73d4971fb91acaef9e1f20226e7b9b883730e394
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92658667"
---
# <a name="creating-a-windows-powershell-navigation-provider"></a>Windows PowerShell ナビゲーション プロバイダーを作成する

このトピックでは、データストア内を移動できる Windows PowerShell ナビゲーションプロバイダーを作成する方法について説明します。 この種類のプロバイダーは、再帰コマンド、入れ子になったコンテナー、および相対パスをサポートしています。

> [!NOTE]
> このプロバイダーの C# ソースファイル (AccessDBSampleProvider05.cs) は、Windows Vista および .NET Framework 3.0 ランタイムコンポーネントの Microsoft Windows Software Development Kit を使用してダウンロードできます。 ダウンロードの手順については、「 [Windows powershell をインストールする方法」および「Windows POWERSHELL SDK をダウンロードする方法](/powershell/scripting/developer/installing-the-windows-powershell-sdk)」を参照してください。
> ダウンロードしたソースファイルは、ディレクトリにあり **\<PowerShell Samples>** ます。 その他の Windows PowerShell プロバイダーの実装の詳細については、「 [Windows Powershell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」を参照してください。

ここで説明するプロバイダーは、ユーザーがデータベース内のデータテーブルに移動できるように、Access データベースをドライブとして処理できるようにします。 独自のナビゲーションプロバイダーを作成するときに、ナビゲーションに必要なドライブ修飾パスを作成したり、相対パスを正規化したり、データストアの項目を移動したり、子名を取得して項目の親パスを取得したり、項目がコンテナーであるかどうかを確認したりするメソッドを実装できます。

> [!CAUTION]
> この設計では、名前が ID のフィールドを持つデータベースと、フィールドの型が LongInteger であることに注意してください。

## <a name="define-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーを定義する

Windows PowerShell ナビゲーションプロバイダー [は、system.servicemodel クラスの基底クラス](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) から派生した .net クラスを作成する必要があります。 ここでは、このセクションで説明するナビゲーションプロバイダーのクラス定義を示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample05/AccessDBProviderSample05.cs" range="31-32":::

このプロバイダーには、2つ [のパラメーター](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute) が含まれていることに注意してください。 最初のパラメーターは、Windows PowerShell によって使用されるプロバイダーのわかりやすい名前を指定します。 2番目のパラメーターは、コマンドの処理中にプロバイダーが Windows PowerShell ランタイムに公開する Windows PowerShell 固有の機能を指定します。 このプロバイダーには、Windows PowerShell 固有の機能は追加されていません。

## <a name="defining-base-functionality"></a>基本機能の定義

「 [PS プロバイダーの設計](./designing-your-windows-powershell-provider.md)」で説明さ [れて](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) いるように、さまざまなプロバイダーの機能を提供する他のいくつかのクラスから派生します。 そのため、Windows PowerShell ナビゲーションプロバイダーは、これらのクラスによって提供されるすべての機能を定義する必要があります。

セッション固有の初期化情報を追加し、プロバイダーによって使用されるリソースを解放するための機能を実装するには、「 [基本的な PS プロバイダーの作成](./creating-a-basic-windows-powershell-provider.md)」を参照してください。 ただし、ほとんどのプロバイダー (ここで説明するプロバイダーを含む) は、Windows PowerShell によって提供されるこの機能の既定の実装を使用できます。

Windows PowerShell ドライブを介してデータストアにアクセスするには、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) 基底クラスのメソッドを実装する必要があります。 これらのメソッドの実装の詳細については、「 [Windows PowerShell ドライブプロバイダーの作成](./creating-a-windows-powershell-drive-provider.md)」を参照してください。

項目の取得、設定、クリアなど、データストアの項目を操作するには、プロバイダー [が、system.object クラスの基本クラス](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) によって提供されるメソッドを実装する必要があります。 これらのメソッドの実装の詳細については、「 [Windows PowerShell 項目プロバイダーの作成](./creating-a-windows-powershell-item-provider.md)」を参照してください。

項目を作成、コピー、名前変更、および削除するメソッドだけでなく、データストアの子項目、またはその名前を取得するには、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider) 基底クラスによって提供されるメソッドを実装する必要があります。 これらのメソッドの実装の詳細については、「 [Windows PowerShell コンテナープロバイダーの作成](./creating-a-windows-powershell-container-provider.md)」を参照してください。

## <a name="creating-a-windows-powershell-path"></a>Windows PowerShell のパスを作成する

Windows PowerShell ナビゲーションプロバイダーは、プロバイダー内部の Windows PowerShell パスを使用して、データストアの項目を移動します。 プロバイダーの内部パスを作成するには、プロバイダーが、Combine-Path コマンドレットからの呼び出しをサポートするように、system.servicemodel [path *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath) メソッドを実装する必要があります。 このメソッドは、親パスと子パスの間にプロバイダー固有のパス区切り記号を使用して、親と子のパスをプロバイダーの内部パスに結合します。

既定の実装では、パスの区切り文字として "/" または "" を含むパスを使用し \\ 、パスの区切り記号を "" に正規化します。次に、親と子のパスの部分を結合して、パスを \\ 結合した文字列を返します。

このナビゲーションプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、 [システム](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath) の既定の実装であり、このメソッドを実装しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidermakepath](Msh_samplestestcmdlets#testprovidermakepath)]  -->

#### <a name="things-to-remember-about-implementing-makepath"></a>MakePath の実装に関する注意事項

次の条件は、使用している [システム](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath)の実装に適用される可能性があります。

- [システム](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath)の実装では、このパスをプロバイダーの名前空間内の有効な完全修飾パスとして検証しないようにしてください。 各パラメーターはパスの一部のみを表すことができ、結合された部分は完全修飾パスを生成しない場合があることに注意してください。 たとえば、filesystem プロバイダーの windows\system32 [*](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath)メソッドは、パラメーターに "" を受け取り、 `parent` パラメーターに "abc.dll" を受け取ることがあります。これは、ファイルシステムプロバイダーの場合です。 `child` メソッドは、これらの値を " \\ " 区切り記号と結合し、完全修飾ファイルシステムパスではない "windows\system32\abc.dll" を返します。

  > [!IMPORTANT]
  > [システム](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath)の呼び出しで指定されたパス部分には、プロバイダーの名前空間で使用できない文字が含まれている可能性があります。 これらの文字は、ワイルドカードの展開に使用されることが多いため、このメソッドの実装では削除しないでください。

## <a name="retrieving-the-parent-path"></a>親パスの取得

Windows PowerShell ナビゲーションプロバイダーは、 [Getparentpath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetParentPath) メソッドを実装して、指定された完全または部分プロバイダー固有のパスの親部分を取得します。 メソッドは、パスの子部分を削除し、親パス部分を返します。 パラメーターは、 `root` ドライブのルートへの完全修飾パスを指定します。 マウントされたドライブが取得操作に使用されていない場合、このパラメーターは null または空にすることができます。 ルートが指定されている場合、メソッドはルートと同じツリー内のコンテナーへのパスを返す必要があります。

サンプルナビゲーションプロバイダーは、このメソッドをオーバーライドしませんが、既定の実装を使用します。
パス区切り記号として "/" と "" の両方を使用するパスを受け入れ \\ ます。 まず、"" 区切り記号のみを含むようにパスを正規化 \\ し、次に親パスを最後の "" に分割 \\ し、親パスを返します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidergetparentpath](Msh_samplestestcmdlets#testprovidergetparentpath)]  -->

#### <a name="to-remember-about-implementing-getparentpath"></a>GetParentPath の実装について覚えておくには

[Getparentpath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetParentPath)メソッドの実装では、プロバイダーの名前空間のパス区切りでパスを構文的に分割する必要があります。 たとえば、filesystem プロバイダーは、このメソッドを使用して最後の " \\ " を検索し、区切り記号の左側にあるすべての要素を返します。

## <a name="retrieve-the-child-path-name"></a>子パス名を取得する

ナビゲーションプロバイダーは、指定された完全または部分的なプロバイダー固有のパスにある項目の子の名前 (リーフ要素) を取得するために、system.string. [Getchildname *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetChildName) メソッドを実装します。

サンプルナビゲーションプロバイダーでは、このメソッドはオーバーライドされません。 既定の実装は次のとおりです。 パス区切り記号として "/" と "" の両方を使用するパスを受け入れ \\ ます。 まず、"" 区切り記号のみを含むようにパスを正規化 \\ し、次に親パスを最後の "" に分割し、 \\ 子パス部分の名前を返します。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidergetchildname](Msh_samplestestcmdlets#testprovidergetchildname)]  -->

#### <a name="things-to-remember-about-implementing-getchildname"></a>GetChildName の実装に関する注意事項

の実装では、パスの区切り記号でパスを構文的に分割する必要があります。 [Getchildname *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetChildName) メソッドを実装します。 指定されたパスにパスの区切り文字が含まれていない場合、メソッドはパスを変更せずに返します。

> [!IMPORTANT]
> このメソッドの呼び出しで指定されたパスに、プロバイダーの名前空間では無効な文字が含まれている可能性があります。 これらの文字は、ワイルドカードの展開や正規表現の照合に使用されることが多いため、このメソッドの実装では削除しないでください。

## <a name="determining-if-an-item-is-a-container"></a>項目がコンテナーであるかどうかを判断する

ナビゲーションプロバイダーは、 [Isitemcontainer *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.IsItemContainer) メソッドを実装して、指定したパスがコンテナーを示すかどうかを判断できます。 パスがコンテナーを表している場合は true、それ以外の場合は false を返します。 ユーザーは、指定され `Test-Path` たパスに対してコマンドレットを使用できるようにするために、このメソッドを必要とします。

次のコードは、サンプルナビゲーションプロバイダーでの [Isitemcontainer *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.IsItemContainer) の実装を示していますが、 メソッドは、指定されたパスが正しいことと、テーブルが存在するかどうかを検証し、パスがコンテナーを示す場合は true を返します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample05/AccessDBProviderSample05.cs" range="847-872":::

#### <a name="things-to-remember-about-implementing-isitemcontainer"></a>IsItemContainer の実装に関する注意事項

ナビゲーションプロバイダー .NET クラスは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言する場合があります。 この場合、 [Isitemcontainer *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.IsItemContainer) の実装では、渡されたパスが要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、例 [では、](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .

## <a name="moving-an-item"></a>項目の移動

コマンドレットのサポートでは `Move-Item` 、ナビゲーションプロバイダーによって、 [システムの管理](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) を実装しています。 このメソッドは、パラメーターによって指定された項目を、パラメーターに指定された `path` パスのコンテナーに移動し `destination` ます。

サンプルのナビゲーションプロバイダーでは、system.string [*](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) メソッドはオーバーライドされていませんが、 既定の実装は次のとおりです。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidermoveitem](Msh_samplestestcmdlets#testprovidermoveitem)]  -->

#### <a name="things-to-remember-about-implementing-moveitem"></a>MoveItem の実装に関する注意事項

ナビゲーションプロバイダー .NET クラスは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言する場合があります。 この場合、渡されたパスが要件を満たしていることを確認するために、 [システムの管理](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) を実装する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、プロパティを指定し **ます。**

既定では、このメソッドのオーバーライドでは、system.object [*](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されていない限り、既存のオブジェクトにオブジェクトを移動することはできません `true` 。 たとえば、filesystem プロバイダーは、既存の c:\bar.txt ファイルの c:\temp\abc.txt をコピーしません。ただし、この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティをに設定することはできません `true` 。 パラメーターで指定されたパスが存在し、コンテナーである場合は、 `destination` " [System.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) ...................................... この場合は、パラメーターで示されている項目 [を、](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) パラメーターによって `path` `destination` 子として指定されたコンテナーに移動する必要があります。

[システム](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem)の実装では、このメソッドの実装によって、 [system](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess) ...................... を呼び出し、戻り値を確認してから、データストアに変更を加える必要があります。 このメソッドは、ファイルを削除するなど、システム状態が変更されたときの操作の実行を確認するために使用されます。
このコマンドは、変更するリソースの名前をユーザーに送信[し](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)ます。 Windows PowerShell ランタイムは、ユーザーに表示される内容を決定する際に、コマンドライン設定またはユーザー設定変数を考慮します。

System.object を呼び出す[と、が返され](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)た後 `true` 、システムの[管理](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem)......................................................... [..。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue) このメソッドは、ユーザーにメッセージを送信して、操作を続行する必要があるかどうかをフィードバックできるようにします。 プロバイダーは、system.servicemodel プロバイダーを呼び出す必要があり [ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue) 危険性の高いシステム変更については、追加のチェックとして続行してください。

## <a name="attaching-dynamic-parameters-to-the-move-item-cmdlet"></a>Move-Item コマンドレットへの動的パラメーターのアタッチ

`Move-Item`コマンドレットには、実行時に動的に提供される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、ナビゲーションプロバイダーが、指定されたパスにある項目から必要なパラメーター値を取得し、コマンドレットクラス[や system.object に](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)類似した解析属性を持つプロパティとフィールドを持つオブジェクトを返すために、システムの[管理](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItemDynamicParameters)を実装する必要があります。

このナビゲーションプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、既定の実装である、system.object の既定の実装です。 [Moveitemdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItemDynamicParameters)です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidermoveitemdynamicparameters](Msh_samplestestcmdlets#testprovidermoveitemdynamicparameters)]  -->

## <a name="normalizing-a-relative-path"></a>相対パスの正規化

ナビゲーションプロバイダーは、 [Normalizerelativepath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.NormalizeRelativePath)メソッドを実装して、パラメーターで指定された完全修飾パスを、 `path` パラメーターで指定されたパスに対して相対的なものとして正規化します。 `basePath` メソッドは、正規化されたパスの文字列形式を返します。 パラメーターに存在しないパスが指定されている場合、エラーが書き込ま `path` れます。

サンプルナビゲーションプロバイダーでは、このメソッドはオーバーライドされません。 既定の実装は次のとおりです。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidernormalizepath](Msh_samplestestcmdlets#testprovidernormalizepath)]  -->

#### <a name="things-to-remember-about-implementing-normalizerelativepath"></a>NormalizeRelativePath の実装に関する注意事項

[Normalizerelativepath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.NormalizeRelativePath)の実装では、パラメーターを解析する必要があります `path` が、純粋な構文解析を使用する必要はありません。 このメソッドは、パスを使用してデータストア内のパス情報を参照し、大文字と小文字の区別と標準化されたパス構文に一致するパスを作成するように設計することをお勧めします。

## <a name="code-sample"></a>コード サンプル

完全なサンプルコードについては、「 [AccessDbProviderSample05 のコードサンプル](./accessdbprovidersample05-code-sample.md)」を参照してください。

## <a name="defining-object-types-and-formatting"></a>オブジェクトの種類と書式設定の定義

プロバイダーは、既存のオブジェクトにメンバーを追加したり、新しいオブジェクトを定義したりすることができます。 詳細については、「[オブジェクトの種類と書式設定の拡張](/previous-versions/ms714665(v=vs.85))」を参照してください。

## <a name="building-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーの構築

詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions/ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーのテスト

Windows powershell プロバイダーが Windows PowerShell に登録されている場合は、コマンドラインでサポートされているコマンドレットを実行してテストできます。これには、派生によって使用可能なコマンドレットも含まれます。 この例では、サンプルナビゲーションプロバイダーをテストします。

1. 新しいシェルを実行し、コマンドレットを使用して、 `Set-Location` アクセスデータベースを示すパスを設定します。

   ```powershell
   Set-Location mydb:
   ```

2. 次に、コマンドレットを実行して、 `Get-Childitem` 使用可能なデータベーステーブルであるデータベースアイテムの一覧を取得します。 このコマンドレットは、テーブルごとにテーブル行の数も取得します。

   ```powershell
   Get-ChildItem | Format-Table rowcount,name -AutoSize
   ```

   ```Output
   RowCount   Name
   --------   ----
        180   MSysAccessObjects
          0   MSysACEs
          1   MSysCmdbars
          0   MSysIMEXColumns
          0   MSysIMEXSpecs
          0   MSysObjects
          0   MSysQueries
          7   MSysRelationships
          8   Categories
         91   Customers
          9   Employees
       2155   Order Details
        830   Orders
         77   Products
          3   Shippers
         29   Suppliers
   ```

3. もう一度コマンドレットを使用して、 `Set-Location` Employees データテーブルの場所を設定します。

   ```powershell
   Set-Location Employees
   ```

4. ここで、コマンドレットを使用して、 `Get-Location` Employees テーブルへのパスを取得します。

   ```powershell
   Get-Location
   ```

   ```Output
   Path
   ----
   mydb:\Employees
   ```

5. ここで、コマンドレット `Get-Childitem` にパイプを使用して `Format-Table` コマンドレットを実行します。 この一連のコマンドレットは、テーブル行である Employees データテーブルの項目を取得します。 これらは、コマンドレットによって指定された形式になってい `Format-Table` ます。

   ```powershell
   Get-ChildItem | Format-Table rownumber,psiscontainer,data -AutoSize
   ```

   ```Output
   RowNumber   PSIsContainer   Data
   ---------   --------------   ----
   0           False            System.Data.DataRow
   1           False            System.Data.DataRow
   2           False            System.Data.DataRow
   3           False            System.Data.DataRow
   4           False            System.Data.DataRow
   5           False            System.Data.DataRow
   6           False            System.Data.DataRow
   7           False            System.Data.DataRow
   8           False            System.Data.DataRow
   ```

6. これで、コマンドレットを実行して、 `Get-Item` Employees データテーブルの行0の項目を取得できるようになりました。

   ```powershell
   Get-Item 0
   ```

   ```Output
   PSPath        : AccessDB::C:\PS\Northwind.mdb\Employees\0
   PSParentPath  : AccessDB::C:\PS\Northwind.mdb\Employees
   PSChildName   : 0
   PSDrive       : mydb
   PSProvider    : System.Management.Automation.ProviderInfo
   PSIsContainer : False
   Data           : System.Data.DataRow
   RowNumber      : 0
   ```

7. コマンドレットをもう一度使用して、 `Get-Item` 行0の項目の従業員データを取得します。

   ```powershell
   (Get-Item 0).data
   ```

   ```Output
   EmployeeID      : 1
   LastName        : Davis
   FirstName       : Sara
   Title           : Sales Representative
   TitleOfCourtesy : Ms.
   BirthDate       : 12/8/1968 12:00:00 AM
   HireDate        : 5/1/1992 12:00:00 AM
   Address         : 4567 Main Street
                     Apt. 2A
   City            : Buffalo
   Region          : NY
   PostalCode      : 98052
   Country         : USA
   HomePhone       : (206) 555-9857
   Extension       : 5467
   Photo           : EmpID1.bmp
   Notes           : Education includes a BA in psychology from
                     Colorado State University. She also completed "The
                     Art of the Cold Call."  Nancy is a member of
                     Toastmasters International.
   ReportsTo       : 2
   ```

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)

[Windows PowerShell プロバイダーを設計する](./designing-your-windows-powershell-provider.md)

[オブジェクトの種類と書式設定の拡張](/previous-versions/ms714665(v=vs.85))

[コンテナーの Windows PowerShell プロバイダーを実装する](./creating-a-windows-powershell-container-provider.md)

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions/ms714644(v=vs.85))

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
