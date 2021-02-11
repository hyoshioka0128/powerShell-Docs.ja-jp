---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell ドライブ プロバイダーを作成する
description: Windows PowerShell ドライブ プロバイダーを作成する
ms.openlocfilehash: 639518fce27d941b7529b091364c5905c91a5c0c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92663042"
---
# <a name="creating-a-windows-powershell-drive-provider"></a>Windows PowerShell ドライブ プロバイダーを作成する

このトピックでは、windows PowerShell ドライブを介してデータストアにアクセスする方法を提供する Windows PowerShell ドライブプロバイダーを作成する方法について説明します。 この種類のプロバイダーは、Windows PowerShell ドライブプロバイダーとも呼ばれます。 プロバイダーによって使用される Windows PowerShell ドライブは、データストアに接続する手段を提供します。

ここに記載されている Windows PowerShell ドライブプロバイダーは、Microsoft Access データベースへのアクセスを提供します。
このプロバイダーの場合、Windows PowerShell ドライブはデータベースを表し (ドライブプロバイダーに任意の数のドライブを追加することができます)、ドライブの最上位のコンテナーはデータベース内のテーブルを表し、コンテナーの項目はテーブル内の行を表します。

## <a name="defining-the-windows-powershell-provider-class"></a>Windows PowerShell プロバイダークラスの定義

Drive プロバイダーは、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) 基底クラスから派生する .net クラスを定義する必要があります。 このドライブプロバイダーのクラス定義を次に示します。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample02/AccessDBProviderSample02.cs" range="29-30":::

この例 [では、](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute) この例では、コマンドの処理中にプロバイダーが windows powershell ランタイムに公開する、プロバイダーのユーザーフレンドリ名と、windows powershell 固有の機能を指定しています。
プロバイダー機能に使用できる値は、system.servicemodel [機能](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) の列挙型によって定義されます。 このドライブプロバイダーは、これらの機能のいずれもサポートしていません。

## <a name="defining-base-functionality"></a>基本機能の定義

「 [Windows PowerShell プロバイダーの設計](./designing-your-windows-powershell-provider.md)」で説明されているように、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) クラスは、プロバイダーの初期化と初期化解除に必要なメソッドを定義する、system.servicemodel [プロバイダー](/dotnet/api/System.Management.Automation.Provider.CmdletProvider) の基底クラスから派生します。 セッション固有の初期化情報を追加し、プロバイダーによって使用されるリソースを解放するための機能を実装するには、「 [基本的な Windows PowerShell プロバイダーの作成](./creating-a-basic-windows-powershell-provider.md)」を参照してください。
ただし、ほとんどのプロバイダー (ここで説明するプロバイダーを含む) は、Windows PowerShell によって提供されるこの機能の既定の実装を使用できます。

## <a name="creating-drive-state-information"></a>ドライブの状態情報の作成

すべての Windows PowerShell プロバイダーはステートレスであると見なされます。つまり、ドライブプロバイダーは、プロバイダーを呼び出すときに、Windows PowerShell ランタイムが必要とする状態情報を作成する必要があります。

このドライブプロバイダーの場合、状態情報には、ドライブ情報の一部として保持されているデータベースへの接続が含まれます。 次に示すコードは、この情報がドライブを説明する [System.Management.Automation.PSDriveinfo](/dotnet/api/System.Management.Automation.PSDriveInfo) オブジェクトにどのように格納されるかを示しています。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample02/AccessDBProviderSample02.cs" range="130-151":::

## <a name="creating-a-drive"></a>ドライブの作成

Windows PowerShell ランタイムがドライブを作成できるようにするには、ドライブプロバイダーが [Drivecmdletprovider *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDrive) メソッドを実装する必要があります。 次のコードは、このドライブプロバイダーの [Drivecmdletprovider *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDrive) メソッドを実装する方法を示しています。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample02/AccessDBProviderSample02.cs" range="42-84":::

このメソッドをオーバーライドするには、次の操作を行う必要があります。

- System.Management.Automation.PSDriveinfo であることを確認し [ ます。ルート *](/dotnet/api/System.Management.Automation.PSDriveInfo.Root) メンバーが存在し、データストアへの接続を確立できます。
- コマンドレットのサポートで、ドライブを作成し、接続メンバーを設定し `New-PSDrive` ます。
- 提案されたドライブの [System.Management.Automation.PSDriveinfo](/dotnet/api/System.Management.Automation.PSDriveInfo) オブジェクトを検証します。
- ドライブを記述する [System.Management.Automation.PSDriveinfo](/dotnet/api/System.Management.Automation.PSDriveInfo) オブジェクトを、必要なパフォーマンス情報または信頼性情報を使用して変更するか、ドライブを使用して呼び出し元に追加のデータを提供します。
- [WriteError](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteError)メソッドを使用してエラーを処理してから、を返し `null` ます。

  このメソッドは、メソッドに渡されたドライブ情報またはプロバイダー固有のバージョンのいずれかを返します。

## <a name="attaching-dynamic-parameters-to-newdrive"></a>動的パラメーターを NewDrive にアタッチしています

`New-PSDrive`ドライブプロバイダーでサポートされているコマンドレットでは、追加のパラメーターが必要になる場合があります。 これらの動的パラメーターをコマンドレットにアタッチするために、プロバイダーは [Drivecmdletprovider. Newdrivedynamicparameters *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.NewDriveDynamicParameters) メソッドを実装します。 このメソッドは、コマンドレットクラスや [system.string オブジェクトと](/dotnet/api/System.Management.Automation.RuntimeDefinedParameterDictionary) 同様に解析属性を持つプロパティとフィールドを持つオブジェクトを返します。

このドライブプロバイダーは、このメソッドをオーバーライドしません。 ただし、次のコードは、このメソッドの既定の実装を示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testprovidernewdrivedynamicparameters](Msh_samplestestcmdlets#testprovidernewdrivedynamicparameters)]  -->

## <a name="removing-a-drive"></a>ドライブの取り外し

データベース接続を閉じるには、ドライブプロバイダーが [Drivecmdletprovider *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.RemoveDrive) メソッドを実装している必要があります。 このメソッドは、プロバイダー固有の情報をクリーンアップした後に、ドライブへの接続を閉じます。

次のコードは、このドライブプロバイダーの [Drivecmdletprovider *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.RemoveDrive) メソッドを実装する方法を示しています。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/AccessDBProviderSample02/AccessDBProviderSample02.cs" range="91-116":::

ドライブを削除できる場合、メソッドはパラメーターを使用してメソッドに渡された情報を返す必要があり `drive` ます。 ドライブを削除できない場合、メソッドは例外を書き込み、を返す必要があり `null` ます。 プロバイダーがこのメソッドをオーバーライドしない場合、このメソッドの既定の実装は、入力として渡されたドライブ情報だけを返します。

## <a name="initializing-default-drives"></a>既定のドライブの初期化

ドライブプロバイダーは、ドライブをマウントするために [System.Management.Automation.Provider.Drivecmdletprovider.Initializedefaultdrives *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.InitializeDefaultDrives) メソッドを実装します。 たとえば、コンピューターがドメインに参加している場合、Active Directory プロバイダーが既定の名前付けコンテキストのドライブをマウントすることがあります。

このメソッドは、初期化されたドライブまたは空のコレクションに関するドライブ情報のコレクションを返します。 このメソッドの呼び出しは、Windows PowerShell ランタイムによっ [て、プロバイダー](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.Start) を初期化するために、を呼び出した後に実行されます。

このドライブプロバイダーは、 [System.Management.Automation.Provider.Drivecmdletprovider.Initializedefaultdrives *](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider.InitializeDefaultDrives) メソッドをオーバーライドしません。 ただし、次のコードは、空のドライブコレクションを返す既定の実装を示しています。

<!-- TODO!!!: review snippet reference  [!CODE [Msh_samplestestcmdlets#testproviderinitializedefaultdrives](Msh_samplestestcmdlets#testproviderinitializedefaultdrives)]  -->

#### <a name="things-to-remember-about-implementing-initializedefaultdrives"></a>InitializeDefaultDrives の実装に関する注意事項

すべてのドライブプロバイダーは、ユーザーが見つけやすいように、ルートドライブをマウントする必要があります。 ルートドライブには、他のマウントされたドライブのルートとして機能する場所が一覧表示される場合があります。 たとえば、Active Directory プロバイダーは、 `namingContext` ルート分散システム環境 (DSE) の属性で見つかった名前付けコンテキストを一覧表示するドライブを作成する場合があります。 これは、ユーザーが他のドライブのマウントポイントを検出するのに役立ちます。

## <a name="code-sample"></a>コード サンプル

完全なサンプルコードについては、「 [AccessDbProviderSample02 のコードサンプル](./accessdbprovidersample02-code-sample.md)」を参照してください。

## <a name="testing-the-windows-powershell-drive-provider"></a>Windows PowerShell ドライブプロバイダーのテスト

Windows PowerShell プロバイダーが Windows PowerShell に登録されている場合は、コマンドラインでサポートされているコマンドレットを実行してテストできます。これには、派生によって使用可能なコマンドレットも含まれます。 サンプルドライブプロバイダーをテストしてみましょう。

1. コマンドレットを実行して `Get-PSProvider` プロバイダーの一覧を取得し、AccessDB ドライブプロバイダーが存在することを確認します。

   **PS> `Get-PSProvider`**

   次のような出力が表示されます。

   ```Output
   Name                 Capabilities                  Drives
   ----                 ------------                  ------
   AccessDB             None                          {}
   Alias                ShouldProcess                 {Alias}
   Environment          ShouldProcess                 {Env}
   FileSystem           Filter, ShouldProcess         {C, Z}
   Function             ShouldProcess                 {function}
   Registry             ShouldProcess                 {HKLM, HKCU}
   ```

2. オペレーティングシステムの **管理ツール** の [**データソース**] 部分にアクセスして、データベースのデータベースサーバー名 (DSN) が存在することを確認します。 **ユーザー DSN** テーブルで、[ **MS Access データベース**] をダブルクリックし、ドライブパスを追加し `C:\ps\northwind.mdb` ます。

3. サンプルドライブプロバイダーを使用して新しいドライブを作成します。

   ```powershell
   new-psdrive -name mydb -root c:\ps\northwind.mdb -psprovider AccessDb`
   ```

   次のような出力が表示されます。

   ```Output
   Name     Provider     Root                   CurrentLocation
   ----     --------     ----                   ---------------
   mydb     AccessDB     c:\ps\northwind.mdb
   ```

4. 接続を検証します。 接続はドライブのメンバーとして定義されているため、Get-PDDrive コマンドレットを使用して確認できます。

   > [!NOTE]
   > ユーザーは、その対話にコンテナー機能を必要とするため、プロバイダーをドライブとして操作することはできません。 詳細については、「 [Windows PowerShell コンテナープロバイダーの作成](./creating-a-windows-powershell-container-provider.md)」を参照してください。

   **PS> (psdrive mydb). 接続**

   次のような出力が表示されます。

   ```Output
   ConnectionString  : Driver={Microsoft Access Driver (*.mdb)};DBQ=c:\ps\northwind.mdb
   ConnectionTimeout : 15
   Database          : c:\ps\northwind
   DataSource        : ACCESS
   ServerVersion     : 04.00.0000
   Driver            : odbcjt32.dll
   State             : Open
   Site              :
   Container         :
   ```

5. ドライブを取り外し、シェルを終了します。

   ```powershell
   PS> remove-psdrive mydb
   PS> exit
   ```

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)

[Windows PowerShell プロバイダーを設計する](./designing-your-windows-powershell-provider.md)

[基本的な Windows PowerShell プロバイダーを作成する](./creating-a-basic-windows-powershell-provider.md)
