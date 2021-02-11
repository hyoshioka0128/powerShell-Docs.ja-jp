---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell コンテナー プロバイダーを作成する
description: Windows PowerShell コンテナー プロバイダーを作成する
ms.openlocfilehash: 999bd69e3c16bfc0a74519986654ec15bbc0da6d
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645351"
---
# <a name="creating-a-windows-powershell-container-provider"></a>Windows PowerShell コンテナー プロバイダーを作成する

このトピックでは、マルチレイヤーデータストアで動作する Windows PowerShell プロバイダーを作成する方法について説明します。 この種類のデータストアの場合、ストアの最上位レベルにはルート項目が含まれ、それ以降の各レベルは子項目のノードと呼ばれます。 ユーザーがこれらの子ノードを操作できるようにすることで、ユーザーはデータストアを介して階層的に対話できます。

複数レベルのデータストアで使用できるプロバイダーは、Windows PowerShell コンテナープロバイダーと呼ばれます。 ただし、Windows PowerShell コンテナープロバイダーは、1つのコンテナー (入れ子になったコンテナーを含まない) に項目がある場合にのみ使用できます。 入れ子になったコンテナーがある場合は、Windows PowerShell ナビゲーションプロバイダーを実装する必要があります。 Windows PowerShell ナビゲーションプロバイダーの実装の詳細については、「 [Windows Powershell ナビゲーションプロバイダーの作成](./creating-a-windows-powershell-navigation-provider.md)」を参照してください。

> [!NOTE]
> このプロバイダーの C# ソースファイル (AccessDBSampleProvider04.cs) は、Windows Vista および .NET Framework 3.0 ランタイムコンポーネントの Microsoft Windows Software Development Kit を使用してダウンロードできます。 ダウンロードの手順については、「 [Windows powershell をインストールする方法」および「Windows POWERSHELL SDK をダウンロードする方法](/powershell/scripting/developer/installing-the-windows-powershell-sdk)」を参照してください。
> ダウンロードしたソースファイルは、ディレクトリにあり **\<PowerShell Samples>** ます。 その他の Windows PowerShell プロバイダーの実装の詳細については、「 [Windows Powershell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」を参照してください。

ここで説明する Windows PowerShell コンテナープロバイダーは、データベースを1つのコンテナーとして定義します。データベースのテーブルと行は、コンテナーの項目として定義されています。

> [!CAUTION]
> この設計では、名前が ID のフィールドを持つデータベースと、フィールドの型が LongInteger であることに注意してください。

## <a name="defining-a-windows-powershell-container-provider-class"></a>Windows PowerShell コンテナープロバイダークラスの定義

Windows PowerShell コンテナープロバイダーは、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider) 基底クラスから派生する .net クラスを定義する必要があります。 ここでは、このセクションで説明する Windows PowerShell コンテナープロバイダーのクラス定義を示します。

```csharp
[CmdletProvider("AccessDB", ProviderCapabilities.None)]
public class AccessDBProvider : ContainerCmdletProvider
```

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="34-35":::

このクラス定義では、このクラス [の属性](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute) に2つのパラメーターが含まれていることに注意してください。 最初のパラメーターは、Windows PowerShell によって使用されるプロバイダーのわかりやすい名前を指定します。 2番目のパラメーターは、コマンドの処理中にプロバイダーが Windows PowerShell ランタイムに公開する Windows PowerShell 固有の機能を指定します。 このプロバイダーには、Windows PowerShell 固有の機能は追加されていません。

## <a name="defining-base-functionality"></a>基本機能の定義

「 [Windows PowerShell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」で説明したように、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider) クラスは、さまざまなプロバイダー機能を提供する他のいくつかのクラスから派生します。 そのため、Windows PowerShell コンテナープロバイダーは、これらのクラスによって提供されるすべての機能を定義する必要があります。

セッション固有の初期化情報を追加し、プロバイダーによって使用されるリソースを解放するための機能を実装するには、「 [基本的な Windows PowerShell プロバイダーの作成](./creating-a-basic-windows-powershell-provider.md)」を参照してください。
ただし、ほとんどのプロバイダー (ここで説明するプロバイダーを含む) は、Windows PowerShell によって提供されるこの機能の既定の実装を使用できます。

データストアへのアクセスを取得するには、プロバイダーが [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) 基底クラスのメソッドを実装する必要があります。 これらのメソッドの実装の詳細については、「 [Windows PowerShell ドライブプロバイダーの作成](./creating-a-windows-powershell-drive-provider.md)」を参照してください。

項目の取得、設定、クリアなど、データストアの項目を操作するには、プロバイダー [が、system.object クラスの基本クラス](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) によって提供されるメソッドを実装する必要があります。 これらのメソッドの実装の詳細については、「 [Windows PowerShell 項目プロバイダーの作成](./creating-a-windows-powershell-item-provider.md)」を参照してください。

## <a name="retrieving-child-items"></a>取得 (子項目を)

子項目を取得するには、Windows PowerShell コンテナープロバイダーが[Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)メソッドをオーバーライドして、コマンドレットからの呼び出しをサポートする必要があります。 `Get-ChildItem` このメソッドは、データストアから子項目を取得し、それらをオブジェクトとしてパイプラインに書き込みます。 `recurse`コマンドレットのパラメーターが指定されている場合、メソッドは、どのレベルにあるかに関係なく、すべての子を取得します。 `recurse`パラメーターが指定されていない場合、メソッドは子の1つのレベルのみを取得します。

ここでは、このプロバイダーの [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドの実装を示しています。 このメソッドは、パスが Access データベースを示す場合はすべてのデータベーステーブル内の子項目を取得し、パスがデータテーブルを示す場合はそのテーブルの行から子項目を取得することに注意してください。

```csharp
protected override void GetChildItems(string path, bool recurse)
{
    // If path represented is a drive then the children in the path are
    // tables. Hence all tables in the drive represented will have to be
    // returned
    if (PathIsDrive(path))
    {
        foreach (DatabaseTableInfo table in GetTables())
        {
            WriteItemObject(table, path, true);

            // if the specified item exists and recurse has been set then
            // all child items within it have to be obtained as well
            if (ItemExists(path) && recurse)
            {
                GetChildItems(path + pathSeparator + table.Name, recurse);
            }
        } // foreach (DatabaseTableInfo...
    } // if (PathIsDrive...
    else
    {
        // Get the table name, row number and type of path from the
        // path specified
        string tableName;
        int rowNumber;

        PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

        if (type == PathType.Table)
        {
            // Obtain all the rows within the table
            foreach (DatabaseRowInfo row in GetRows(tableName))
            {
                WriteItemObject(row, path + pathSeparator + row.RowNumber,
                        false);
            } // foreach (DatabaseRowInfo...
        }
        else if (type == PathType.Row)
        {
            // In this case the user has directly specified a row, hence
            // just give that particular row
            DatabaseRowInfo row = GetRow(tableName, rowNumber);
            WriteItemObject(row, path + pathSeparator + row.RowNumber,
                        false);
        }
        else
        {
            // In this case, the path specified is not valid
            ThrowTerminatingInvalidPathException(path);
        }
    } // else
} // GetChildItems
```

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="311-362":::

#### <a name="things-to-remember-about-implementing-getchilditems"></a>GetChildItems の実装に関する注意事項

[Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell コンテナープロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドの実装で、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- このメソッドの実装では、項目がユーザーに表示される可能性のある項目へのアクセス形式を考慮する必要があります。 たとえば、ユーザーがファイルシステムプロバイダー (Windows PowerShell によって提供される) を介してファイルに対する書き込みアクセス権を持っていても、 [System.Management.Automation.Provider.Itemcmdletprovider.Itemexists*](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists)読み取りアクセス権を持っていない場合、ファイルはまだ存在しています`true`。 実装では、子を列挙できるかどうかを確認するために、親項目のチェックが必要になる場合があります。

- 複数の項目を書き込む場合、 [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドには時間がかかることがあります。 プロバイダーを設計して、1回に1つずつ、 [システム](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject) を使用して項目を書き込むようにすることができます。 この手法を使用すると、ストリーム内のユーザーに項目が表示されます。

- [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)の実装は、循環リンクがある場合に無限再帰を防ぐことを担当します。また、同様の処理を行います。 このような状態を反映するには、適切な終了例外をスローする必要があります。

## <a name="attaching-dynamic-parameters-to-the-get-childitem-cmdlet"></a>Get-ChildItem コマンドレットへの動的パラメーターのアタッチ

Containercmdletprovider を呼び出す`Get-ChildItem`コマンドレット[System.Management.Automation.Provider.Containercmdletprovider.Getchilditems*](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)には、実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell コンテナープロバイダーが [Containercmdletprovider. Getchilditemsdynamicparameters *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItemsDynamicParameters) メソッドを実装する必要があります。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスや [system.string オブジェクトと](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) 同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Get-ChildItem` ます。

この Windows PowerShell コンテナープロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidergetchilditemsdynamicparameters](Msh_samplestestcmdlets#testprovidergetchilditemsdynamicparameters)]  -->

## <a name="retrieving-child-item-names"></a>取得 (子項目名を)

子項目の名前を取得するには、Windows PowerShell コンテナープロバイダーが、 [](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames) `Get-ChildItem` `Name` パラメーターが指定されているときにコマンドレットからの呼び出しをサポートするように Containercmdletprovider * メソッドをオーバーライドする必要があります。 このメソッドは、 `returnAllContainers` コマンドレットのパラメーターが指定されている場合、すべてのコンテナーについて、指定されたパスまたは子項目の名前の子項目の名前を取得します。 子の名前は、パスのリーフ部分です。 たとえば、c:\windows\system32\abc.dll パスの子名は "abc.dll" です。 ディレクトリ c:\windows\system32 の子名は "system32" です。

次に示すのは、このプロバイダーの [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames) メソッドの実装について説明します。 パスがテーブルを示す場合、メソッドはテーブル名を取得することに注意してください。指定されたパスが Access データベース (ドライブ) と行番号を示す場合は、

```csharp
protected override void GetChildNames(string path,
                            ReturnContainers returnContainers)
{
    // If the path represented is a drive, then the child items are
    // tables. get the names of all the tables in the drive.
    if (PathIsDrive(path))
    {
        foreach (DatabaseTableInfo table in GetTables())
        {
            WriteItemObject(table.Name, path, true);
        } // foreach (DatabaseTableInfo...
    } // if (PathIsDrive...
    else
    {
        // Get type, table name and row number from path specified
        string tableName;
        int rowNumber;

        PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

        if (type == PathType.Table)
        {
            // Get all the rows in the table and then write out the
            // row numbers.
            foreach (DatabaseRowInfo row in GetRows(tableName))
            {
                WriteItemObject(row.RowNumber, path, false);
            } // foreach (DatabaseRowInfo...
        }
        else if (type == PathType.Row)
        {
            // In this case the user has directly specified a row, hence
            // just give that particular row
            DatabaseRowInfo row = GetRow(tableName, rowNumber);

            WriteItemObject(row.RowNumber, path, false);
        }
        else
        {
            ThrowTerminatingInvalidPathException(path);
        }
    } // else
} // GetChildNames
```

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="369-411":::

#### <a name="things-to-remember-about-implementing-getchildnames"></a>GetChildNames の実装に関する注意事項

[Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell コンテナープロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドの実装で、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

  > [!NOTE]
  > この規則の例外は、 `returnAllContainers` コマンドレットのパラメーターが指定されている場合に発生します。 この場合、メソッドは、コンテナーのすべての子名を取得する必要があります。これは、このメソッドが、 [system](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Filter)の [値 (.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include).................................... [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .....

- 既定では、このメソッドをオーバーライドしても、通常はユーザーに表示されないオブジェクトの名前を取得することはできません。ただし、この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティを指定する必要があります。 指定されたパスがコンテナーを示している場合、 [このプロパティは必須ではあり](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) ません。

- [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNames)を実装すると、循環リンクがあるときに無限再帰を防ぐことができます。また、のようになります。 このような状態を反映するには、適切な終了例外をスローする必要があります。

## <a name="attaching-dynamic-parameters-to-the-get-childitem-cmdlet-name"></a>Get-ChildItem コマンドレットに動的パラメーターをアタッチする (名前)

`Get-ChildItem`コマンドレット (パラメーターを含む) では、 `Name` 実行時に動的に指定される追加のパラメーターが必要になる場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell コンテナープロバイダーが [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildNamesDynamicParameters) を実装する必要があります。このメソッドは、 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Get-ChildItem` ます。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidergetchildnamesdynamicparameters](Msh_samplestestcmdlets#testprovidergetchildnamesdynamicparameters)]  -->

## <a name="renaming-items"></a>項目の名前変更

項目の名前を変更するには、Windows PowerShell コンテナープロバイダーで [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem) メソッドをオーバーライドして、コマンドレットからの呼び出しをサポートする必要があり `Rename-Item` ます。 このメソッドは、指定されたパスにある項目の名前を、指定された新しい名前に変更します。 新しい名前は、常に親項目 (コンテナー) に対する相対パスである必要があります。

このプロバイダーでは、 [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem) メソッドがオーバーライドされていません。 ただし、既定の実装は次のとおりです。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderrenameitem](Msh_samplestestcmdlets#testproviderrenameitem)]  -->

#### <a name="things-to-remember-about-implementing-renameitem"></a>RenameItem の実装に関する注意事項

Containercmdletprovider の実装には、次の条件が当てはまる場合があります [*](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem):

- プロバイダークラスを定義すると、Windows PowerShell コンテナープロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドの実装で、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem)メソッドは、項目の名前の変更のみを目的としています。移動操作では変更できません。
  メソッドの実装では、 `newName` パラメーターにパスの区切り記号が含まれている場合、または項目が親の場所を変更する場合に、エラーを書き込む必要があります。

- 既定では、このメソッドのオーバーライドでは、system.string [*](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティが指定されていない限り、オブジェクトの名前を変更することはできません。 指定されたパスがコンテナーを示している場合、 [このプロパティは必須ではあり](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) ません。

- [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem)メソッドを実装するには、システムを呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)データストアに変更を加える前に、その戻り値を確認してから、その戻り値を確認してください。 このメソッドは、ファイル名の変更など、システム状態が変更されたときの操作の実行を確認するために使用されます。
  このコマンドは、変更されるリソースの名前をユーザー[に送信します。 Windows](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess) PowerShell ランタイムは、表示する内容を決定する際に、コマンドライン設定やユーザー設定変数を考慮します。

  Containercmdletprovider を呼び出すと、[が返されます。この](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)メソッドは、system........................ `true` [](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItem)メソッドを呼び出す必要があります[](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue) 。 このメソッドは、操作を続行する必要があるかどうかを確認するための追加のフィードバックを許可するために、メッセージを確認メッセージとしてユーザーに送信します。 プロバイダーは、system.servicemodel プロバイダーを呼び出す必要があり [ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue) 危険性の高いシステム変更については、追加のチェックとして続行してください。

## <a name="attaching-dynamic-parameters-to-the-rename-item-cmdlet"></a>Rename-Item コマンドレットへの動的パラメーターのアタッチ

コマンドレットには、 `Rename-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell コンテナープロバイダーで [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RenameItemDynamicParameters) メソッドを実装する必要があります。 このメソッドは、指定されたパスにある項目のパラメーターを取得し、コマンドレットクラスや system.servicemodel 型の [Parameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) オブジェクトと同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Rename-Item` ます。

このコンテナープロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderrenameitemdynamicparameters](Msh_samplestestcmdlets#testproviderrenameitemdynamicparameters)]  -->

## <a name="creating-new-items"></a>新しい項目の作成

新しい項目を作成するには、コンテナープロバイダーが[Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)メソッドを実装して、コマンドレットからの呼び出しをサポートする必要があります。 `New-Item` このメソッドは、指定されたパスにあるデータ項目を作成します。 `type`コマンドレットのパラメーターには、新しい項目のプロバイダー定義型が含まれています。 たとえば、FileSystem プロバイダーは、 `type` "file" または "directory" という値を持つパラメーターを使用します。 `newItemValue`コマンドレットのパラメーターは、新しい項目のプロバイダー固有の値を指定します。

ここでは、このプロバイダーの [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem) メソッドの実装を示しています。

```csharp
protected override void NewItem( string path, string type, object newItemValue )
{
    // Create the new item here after
    // performing necessary validations
    //
    // WriteItemObject(newItemValue, path, false);

    // Example
    //
    // if (ShouldProcess(path, "new item"))
    // {
    //      // Create a new item and then call WriteObject
    //      WriteObject(newItemValue, path, false);
    // }

} // NewItem
```

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="939-955":::

#### <a name="things-to-remember-about-implementing-newitem"></a>NewItem の実装に関する注意事項

[Containercmdletprovider. Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)の実装には、次の条件が当てはまる場合があります。

- [Containercmdletprovider. Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)メソッドは、パラメーターで渡された文字列の大文字と小文字を区別しない比較を実行する必要があります。 `type`
  また、あいまい一致が少なくとも許可されている必要があります。 たとえば、"file" と "directory" 型の場合、最初の文字のみを明確にする必要があります。 パラメーターが、 `type` プロバイダーが作成できない型を示す場合、 [Containercmdletprovider Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem) メソッドは、プロバイダーが作成できる型を示すメッセージを含む ArgumentException を書き込む必要があります。

- パラメーターについて `newItemValue` は、 [Containercmdletprovider. Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem) メソッドの実装で、少なくとも文字列を受け入れることをお勧めします。 また、同じパスに対し [て、system.servicemodel](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) ........... によって取得されたオブジェクトの型も受け入れます。 [Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)メソッドは、convertto-html * メソッドを使用して、型を目的の型に変換できます。 [*](/dotnet/api/System.Management.Automation.LanguagePrimitives.ConvertTo)メソッドを使用して Containercmdletprovider します。

- [Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)メソッドの実装では、Containercmdletprovider を呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)データストアに変更を加える前に、戻り値を確認して、その戻り値を確認してください。 Containercmdletprovider の[System.Management.Automation.Provider.Cmdletprovider.ShouldProcess](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)呼び出しによって true が返された後、Newitem * メソッドはを呼び出す[System.Management.Automation.Provider.Containercmdletprovider.Newitem*](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem) 必要があります。この場合、* メソッドは[System.Management.Automation.Provider.Cmdletprovider.ShouldContinue](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)System.object は、危険性の高いシステム変更について追加のチェックとしてメソッドを続行します。

## <a name="attaching-dynamic-parameters-to-the-new-item-cmdlet"></a>New-Item コマンドレットへの動的パラメーターのアタッチ

コマンドレットには、 `New-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、コンテナープロバイダーが [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItemDynamicParameters) メソッドを実装する必要があります。 このメソッドは、指定されたパスにある項目のパラメーターを取得し、コマンドレットクラスや system.servicemodel 型の [Parameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) オブジェクトと同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `New-Item` ます。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、このメソッドの既定の実装です。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidernewitemdynamicparameters](Msh_samplestestcmdlets#testprovidernewitemdynamicparameters)]  -->

## <a name="removing-items"></a>項目の削除

項目を削除するには、Windows PowerShell プロバイダーで [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem) メソッドをオーバーライドして、コマンドレットからの呼び出しをサポートする必要があり `Remove-Item` ます。 このメソッドは、指定されたパスにあるデータストアから項目を削除します。 `recurse` `Remove-Item` コマンドレットのパラメーターがに設定されている場合 `true` 、メソッドは、そのレベルに関係なく、すべての子項目を削除します。 パラメーターがに設定されている場合 `false` 、メソッドは、指定されたパスの1つの項目のみを削除します。

このプロバイダーは項目の削除をサポートしていません。 ただし、次のコードは、 [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)という既定の実装を示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderremoveitem](Msh_samplestestcmdlets#testproviderremoveitem)]  -->

#### <a name="things-to-remember-about-implementing-removeitem"></a>RemoveItem の実装に関する注意事項

[Containercmdletprovider. Newitem *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.NewItem)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell コンテナープロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドの実装で、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドのオーバーライドでは、system.object [*](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティが true に設定されていない限り、オブジェクトを削除しません。 指定されたパスがコンテナーを示している場合、 [このプロパティは必須ではあり](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) ません。

- [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)の実装によって、循環リンクがある場合の無限の再帰を防ぐことができるようになります。 このような状態を反映するには、適切な終了例外をスローする必要があります。

- [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)メソッドを実装するには、システムを呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)その場合は、データストアに変更を加える前に、その戻り値を確認してから、その戻り値を確認してください。 Containercmdletprovider を呼び出すと、[が返され](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)ます。その後、このメソッドは、危険性の高い `true` システム変更を確認するために、追加のチェックとして、このメソッドを呼び出す必要があることを確認してから、 [](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItem)そのようにし[ます](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)。

## <a name="attaching-dynamic-parameters-to-the-remove-item-cmdlet"></a>Remove-Item コマンドレットへの動的パラメーターのアタッチ

コマンドレットには、 `Remove-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、コンテナープロバイダーが [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItemDynamicParameters) メソッドを実装してこれらのパラメーターを処理する必要があります。 このメソッドは、指定されたパスにある項目の動的パラメーターを取得し、コマンドレットクラスまたは system.servicemodel 型の Parameterdictionary と同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 [System.Management.Automation.Runtimedefinedparameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary)オブジェクト。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Remove-Item` ます。

このコンテナープロバイダーは、このメソッドを実装していません。 ただし、次のコードは、 [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.RemoveItemDynamicParameters)の既定の実装であることを示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderremoveitemdynamicparameters](Msh_samplestestcmdlets#testproviderremoveitemdynamicparameters)]  -->

## <a name="querying-for-child-items"></a>子項目のクエリ

指定されたパスに子項目が存在するかどうかを確認するには、Windows PowerShell コンテナープロバイダーが [Haschilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.HasChildItems) メソッドをオーバーライドする必要があります。 `true`項目が子を持っている場合、このメソッドはを返します。それ以外の場合はを返し `false` ます。 Null または空のパスの場合、メソッドはデータストア内のすべての項目を子と見なし、を返し `true` ます。

Containercmdletprovider のオーバーライドを次に示します。 [Haschilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.HasChildItems) メソッドします。 チャンクパスヘルパーメソッドによって作成されたパス部分が3つ以上ある場合、このメソッドはを返し `false` ます。これは、データベースコンテナーとテーブルコンテナーのみが定義されているためです。 このヘルパーメソッドの詳細については、「 [Windows PowerShell 項目プロバイダーの作成](./creating-a-windows-powershell-item-provider.md)」で説明されている「chunkpath メソッド」を参照してください。

```csharp
protected override bool HasChildItems( string path )
{
    return false;
} // HasChildItems
```

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample04/AccessDBProviderSample04.cs" range="1094-1097":::

#### <a name="things-to-remember-about-implementing-haschilditems"></a>HasChildItems の実装に関する注意事項

[Containercmdletprovider. Haschilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.HasChildItems)の実装には、次の条件が当てはまる場合があります。

- コンテナープロバイダーが興味深いマウントポイントを含むルートを公開する場合、パスに null または空の文字列が渡されると、 [Containercmdletprovider. Haschilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.HasChildItems)メソッドの実装はを返す必要があります。 `true`

## <a name="copying-items"></a>項目のコピー

項目をコピーするには、コンテナープロバイダーが [ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem) メソッドを実装して、コマンドレットからの呼び出しをサポートする必要があり `Copy-Item` ます。 このメソッドは、コマンドレットのパラメーターによって示される場所から、パラメーターで示される場所にデータ項目をコピーし `path` `copyPath` ます。 パラメーターを指定した場合、 `recurse` メソッドはすべてのサブコンテナーをコピーします。 パラメーターが指定されていない場合、メソッドは項目の1つのレベルのみをコピーします。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、 [ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)を既定で実装しているものです。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidercopyitem](Msh_samplestestcmdlets#testprovidercopyitem)]  -->

#### <a name="things-to-remember-about-implementing-copyitem"></a>CopyItem の実装に関する注意事項

[ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)の実装には、次の条件が当てはまる場合があります。

- プロバイダークラスを定義すると、Windows PowerShell コンテナープロバイダーは、ExpandWildcards カード、フィルター、包含、または除外のプロバイダー機能を、 [システム](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙体から宣言できます。 このような場合は、 [Containercmdletprovider. Getchilditems *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.GetChildItems) メソッドの実装で、メソッドに渡されるパスが指定された機能の要件を満たしていることを確認する必要があります。 これを行うには、メソッドが適切なプロパティにアクセスする必要があります。たとえば、 ["..](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Exclude) .............................................. [...](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Include) ...

- 既定では、このメソッドのオーバーライドでは、system.object [*](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティがに設定されていない限り、既存のオブジェクトにオブジェクトをコピーしません `true` 。 たとえば、FileSystem プロバイダーは、既存の c:\abc.txt ファイルの c:\temp\abc.txt をコピーしません。ただし、この場合、 [Force *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) プロパティをに設定することはできません `true` 。 パラメーターで指定されたパスが存在し、コンテナーを示している場合は、 `copyPath` " [System.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Force) .................................... この場合、 [ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem) は、パラメーターで示される項目を、 `path` パラメーターによって `copyPath` 子として示されるコンテナーにコピーする必要があります。

- [ContainerCmdletProvider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)の実装は、循環リンクがある場合に無限再帰を防ぐことを担当します。また、同様の処理を行います。 このような状態を反映するには、適切な終了例外をスローする必要があります。

- ContainerCmdletProvider メソッドの実装では、 [](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)を呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)データストアに変更を加える前に、戻り値を確認して、その戻り値を確認してください。 ContainerCmdletProvider の呼び出しによって true[System.Management.Automation.Provider.Cmdletprovider.ShouldProcess](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldProcess)が返された後に、このメソッドはを呼び出す必要[System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItem)があります。この場合、このメソッドは、 [System.Management.Automation.Provider.Cmdletprovider.ShouldContinue](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.ShouldContinue)System.object は、危険性の高いシステム変更について追加のチェックとしてメソッドを続行します。 これらのメソッドの呼び出しの詳細については、「 [項目名の変更](#renaming-items)」を参照してください。

## <a name="attaching-dynamic-parameters-to-the-copy-item-cmdlet"></a>Copy-Item コマンドレットへの動的パラメーターのアタッチ

コマンドレットには、 `Copy-Item` 実行時に動的に指定される追加のパラメーターが必要な場合があります。 これらの動的パラメーターを指定するには、Windows PowerShell コンテナープロバイダーで [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItemDynamicParameters) メソッドを実装してこれらのパラメーターを処理する必要があります。 このメソッドは、指定されたパスにある項目のパラメーターを取得し、コマンドレットクラスや system.servicemodel 型の [Parameterdictionary](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) オブジェクトと同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。 Windows PowerShell ランタイムは、返されたオブジェクトを使用して、コマンドレットにパラメーターを追加し `Copy-Item` ます。

このプロバイダーは、このメソッドを実装していません。 ただし、次のコードは、 [Containercmdletprovider *](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider.CopyItemDynamicParameters)の既定の実装であることを示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidercopyitemdynamicparameters](Msh_samplestestcmdlets#testprovidercopyitemdynamicparameters)]  -->

## <a name="code-sample"></a>コード サンプル

完全なサンプルコードについては、「 [AccessDbProviderSample04 のコードサンプル](./accessdbprovidersample04-code-sample.md)」を参照してください。

## <a name="building-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーの構築

「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法」を](/previous-versions/ms714644(v=vs.85))参照してください。

## <a name="testing-the-windows-powershell-provider"></a>Windows PowerShell プロバイダーのテスト

Windows powershell プロバイダーが Windows PowerShell に登録されている場合は、コマンドラインでサポートされているコマンドレットを実行してテストできます。 次の出力例では、架空の Access データベースが使用されていることに注意してください。

1. コマンドレットを実行して、 `Get-ChildItem` Access データベースの Customers テーブルから子アイテムの一覧を取得します。

   ```powershell
   Get-ChildItem mydb:customers
   ```

   次のような出力が表示されます。

   ```output
   PSPath        : AccessDB::customers
   PSDrive       : mydb
   PSProvider    : System.Management.Automation.ProviderInfo
   PSIsContainer : True
   Data          : System.Data.DataRow
   Name          : Customers
   RowCount      : 91
   Columns       :
   ```

2. `Get-ChildItem`テーブルのデータを取得するには、コマンドレットを再度実行します。

   ```powershell
   (Get-ChildItem mydb:customers).data
   ```

   次のような出力が表示されます。

   ```output
   TABLE_CAT   : c:\PS\northwind
   TABLE_SCHEM :
   TABLE_NAME  : Customers
   TABLE_TYPE  : TABLE
   REMARKS     :
   ```

3. 次に、コマンドレットを使用して、 `Get-Item` データテーブル内の行0の項目を取得します。

   ```powershell
   Get-Item mydb:\customers\0
   ```

   次のような出力が表示されます。

   ```output
   PSPath        : AccessDB::customers\0
   PSDrive       : mydb
   PSProvider    : System.Management.Automation.ProviderInfo
   PSIsContainer : False
   Data          : System.Data.DataRow
   RowNumber     : 0
   ```

4. `Get-Item`行0の項目のデータを取得するために再利用します。

   ```powershell
   (Get-Item mydb:\customers\0).data
   ```

   次のような出力が表示されます。

   ```output
   CustomerID   : 1234
   CompanyName  : Fabrikam
   ContactName  : Eric Gruber
   ContactTitle : President
   Address      : 4567 Main Street
   City         : Buffalo
   Region       : NY
   PostalCode   : 98052
   Country      : USA
   Phone        : (425) 555-0100
   Fax          : (425) 555-0101
   ```

5. 次に、コマンドレットを使用して、 `New-Item` 既存のテーブルに行を追加します。 この `Path` パラメーターは、行への完全パスを指定し、テーブル内の既存の行数よりも大きい行番号を示す必要があります。 パラメーターは、 `Type` 追加する項目の型を指定する "row" を示します。
   最後に、このパラメーターは、 `Value` 行の列値のコンマ区切りリストを指定します。

   ```powershell
   New-Item -Path mydb:\Customers\3 -ItemType "row" -Value "3,CustomerFirstName,CustomerLastName,CustomerEmailAddress,CustomerTitle,CustomerCompany,CustomerPhone, CustomerAddress,CustomerCity,CustomerState,CustomerZip,CustomerCountry"
   ```

6. 次のように、新しい項目の操作が正しいことを確認します。

   ```none
   PS mydb:\> cd Customers
   PS mydb:\Customers> (Get-Item 3).data
   ```

   次のような出力が表示されます。

   ```output
   ID        : 3
   FirstName : Eric
   LastName  : Gruber
   Email     : ericgruber@fabrikam.com
   Title     : President
   Company   : Fabrikam
   WorkPhone : (425) 555-0100
   Address   : 4567 Main Street
   City      : Buffalo
   State     : NY
   Zip       : 98052
   Country   : USA
   ```

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)

[Windows PowerShell プロバイダーを設計する](./designing-your-windows-powershell-provider.md)

[項目を実装する Windows PowerShell プロバイダー](./creating-a-windows-powershell-item-provider.md)

[ナビゲーション Windows PowerShell プロバイダーの実装](./creating-a-windows-powershell-navigation-provider.md)

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions/ms714644(v=vs.85))

[Windows PowerShell SDK](../windows-powershell-reference.md)

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)
