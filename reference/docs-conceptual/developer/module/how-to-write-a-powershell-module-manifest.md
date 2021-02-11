---
ms.date: 01/04/2021
ms.topic: reference
title: PowerShell モジュール マニフェストを記述する方法
description: PowerShell モジュール マニフェストを記述する方法
ms.openlocfilehash: 8c644391008cb97c1206f985f0f5eca9d7dfcc9e
ms.sourcegitcommit: 04faa7dc1122bce839295d4891bd8b2f0ecb06ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/05/2021
ms.locfileid: "97879373"
---
# <a name="how-to-write-a-powershell-module-manifest"></a>PowerShell モジュールマニフェストを記述する方法

PowerShell モジュールを作成した後、モジュールに関する情報を含むオプションのモジュールマニフェストを追加できます。 たとえば、作成者を記述したり、モジュール内のファイル (入れ子になったモジュールなど) を指定したり、スクリプトを実行してユーザーの環境をカスタマイズしたり、種類とフォーマットファイルを読み込んだり、システム要件を定義したり、モジュールによってエクスポートされるメンバーを制限したりすることができます。

## <a name="creating-a-module-manifest"></a>モジュールマニフェストの作成

**モジュールマニフェスト** は、モジュール `.psd1` の内容を記述し、モジュールの処理方法を決定する PowerShell データファイル () です。 マニフェストファイルは、キーと値のハッシュテーブルを含むテキストファイルです。 マニフェストにモジュールと同じ名前を付け、マニフェストをモジュールのルートディレクトリに格納することで、マニフェストファイルをモジュールにリンクします。

単一のアセンブリまたはバイナリアセンブリのみを含む単純なモジュールの場合 `.psm1` 、モジュールマニフェストは省略可能です。 ただし、可能な限りモジュールマニフェストを使用することをお勧めします。これは、コードを整理したり、バージョン管理情報を管理したりするのに役立ちます。 また、 [グローバルアセンブリキャッシュ](/dotnet/framework/app-domains/gac)にインストールされているアセンブリをエクスポートするには、モジュールマニフェストが必要です。 モジュールマニフェストは、更新可能なヘルプ機能をサポートするモジュールにも必要です。 更新可能なヘルプでは、モジュールマニフェストの **Helpinfouri** キーを使用して、モジュールの更新されたヘルプファイルの場所を含むヘルプ情報 (HelpInfo XML) ファイルを検索します。 更新可能なヘルプの詳細については、「 [更新可能なヘルプのサポート](./supporting-updatable-help.md)」を参照してください。

### <a name="to-create-and-use-a-module-manifest"></a>モジュールマニフェストを作成して使用するには

1. モジュールマニフェストを作成する場合は、 [new-modulemanifest](/powershell/module/Microsoft.PowerShell.Core/New-ModuleManifest) コマンドレットを使用することをお勧めします。 パラメーターを使用して、マニフェストの既定のキーと値の1つ以上を指定できます。 唯一の要件は、ファイルに名前を指定することです。 `New-ModuleManifest` 指定した値を使用してモジュールマニフェストを作成し、残りのキーとその既定値を含めます。 複数のモジュールを作成する必要がある場合は、を使用して `New-ModuleManifest` モジュールマニフェストテンプレートを作成し、さまざまなモジュールに対して変更することができます。 既定のモジュールマニフェストの例については、「 [サンプルモジュールマニフェスト](#sample-module-manifest)」を参照してください。

   `New-ModuleManifest -Path C:\myModuleName.psd1 -ModuleVersion "2.0" -Author "YourNameHere"`

   別の方法としては、必要最小限の情報 ( **ModuleVersion**) を使用してモジュールマニフェストのハッシュテーブルを手動で作成する方法があります。 モジュールと同じ名前でファイルを保存し、ファイル拡張子を使用し `.psd1` ます。 その後、ファイルを編集し、適切なキーと値を追加できます。

1. マニフェストファイルに必要な要素を追加します。

   マニフェストファイルを編集するには、任意のテキストエディターを使用します。 ただし、マニフェストファイルはコードを含むスクリプトファイルであるため、Visual Studio Code など、スクリプトまたは開発環境で編集することができます。 マニフェストファイルのすべての要素は、 **ModuleVersion** 番号を除き、省略可能です。

   モジュールマニフェストに含めることができるキーと値の説明については、「 [module manifest elements](#module-manifest-elements) table」を参照してください。 詳細については、 [new-modulemanifest](/powershell/module/Microsoft.PowerShell.Core/New-ModuleManifest) コマンドレットのパラメーターの説明を参照してください。

1. 基本モジュールマニフェスト要素によってカバーされない可能性のあるシナリオに対処するには、モジュールマニフェストにコードを追加するオプションがあります。

   セキュリティの問題については、PowerShell はモジュールマニフェストファイル内の使用可能な操作の小さなサブセットのみを実行します。 一般に、 `if` ステートメント、算術演算子、比較演算子、および基本的な PowerShell データ型を使用できます。

1. モジュールマニフェストを作成したら、それをテストして、マニフェストに記述されているパスが正しいことを確認します。 モジュールマニフェストをテストするには、 [new-modulemanifest](/powershell/module/Microsoft.PowerShell.Core/Test-ModuleManifest)を使用します。

   `Test-ModuleManifest myModuleName.psd1`

1. モジュールマニフェストが、モジュールが格納されているディレクトリの最上位レベルにあることを確認してください。

   モジュールをシステムにコピーしてインポートすると、PowerShell はモジュールマニフェストを使用してモジュールをインポートします。

1. 必要に応じて、マニフェスト [自体をドットソーシングすること](/powershell/module/Microsoft.PowerShell.Core/Import-Module) で、モジュールマニフェストを直接テストできます。

   `Import-Module .\myModuleName.psd1`

## <a name="module-manifest-elements"></a>モジュールマニフェスト要素

次の表では、モジュールマニフェストに含めることができる要素について説明します。

|要素|Default|説明|
|-------------|-------------|-----------------|
|**RootModule**<br /> 型: `String`|`<empty string>`|このマニフェストに関連付けられているスクリプトモジュールまたはバイナリモジュールファイル。 以前のバージョンの PowerShell では、この要素が **ModuleToProcess** と呼ばれていました。<br /> ルートモジュールに使用できる型は空にすることができます。これにより、 **マニフェスト** モジュール、スクリプトモジュールの名前 ( `.psm1` )、またはバイナリモジュールの名前 ( `.exe` または) が作成さ `.dll` れます。 モジュールマニフェストの名前 ( `.psd1` ) またはスクリプトファイル ( `.ps1` ) をこの要素に配置すると、エラーが発生します。 <br /> 例: `RootModule = 'ScriptModule.psm1'`|
|**ModuleVersion**<br /> 型: `Version`|`'0.0.1'`|このモジュールのバージョン番号。 値が指定されていない場合、は `New-ModuleManifest`   既定のを使用します。 文字列は、などの型に変換できる必要があり `Version` `#.#.#.#.#` ます。 `Import-Module`名前に一致し、少なくとも1つ以上の **ModuleVersion** を持つ、 **$PSModulePath** で見つかった最初のモジュールを **MinimumVersion** パラメーターとして読み込みます。 特定のバージョンをインポートするには、 `Import-Module` コマンドレットの **RequiredVersion** パラメーターを使用します。<br /> 例: `ModuleVersion = '1.0'`|
|**GUID**<br /> 型: `GUID`|`'<GUID>'`|このモジュールを一意に識別するために使用する ID。 値が指定されていない場合、 `New-ModuleManifest` オプティマイザー値を生成します。 現在、 **GUID** によってモジュールをインポートすることはできません。 <br /> 例: `GUID = 'cfc45206-1e49-459d-a8ad-5b571ef94857'`|
|**作成者**<br /> 型: `String`|`'<Current user>'`|このモジュールの作成者。 値が指定されていない場合、は `New-ModuleManifest` 現在のユーザーを使用します。 <br /> 例: `Author = 'AuthorNameHere'`|
|**CompanyName**<br /> 型: `String`|`'Unknown'`|このモジュールの会社またはベンダー。 値が指定されていない場合、は `New-ModuleManifest` 既定のを使用します。<br /> 例: `CompanyName = 'Fabrikam'`|
|**著作権**<br /> 型: `String`|`'(c) <Author>. All rights reserved.'`| このモジュールの著作権に関する声明。 値が指定されていない場合、は `New-ModuleManifest` 現在のユーザーの既定のをとして使用し `<Author>` ます。 作成者を指定するには、 **author** パラメーターを使用します。 <br /> 例: `Copyright = '2019 AuthorName. All rights reserved.'`|
|**説明**<br /> 型: `String`|`<empty string>`|このモジュールによって提供される機能の説明です。<br /> 例: `Description = 'This is the module's description.'`|
|**PowerShellVersion**<br /> 型: `Version`|`<empty string>`|このモジュールに必要な PowerShell エンジンの最小バージョン。 有効な値は、1.0、2.0、3.0、4.0、5.0、5.1、6.0、6.1、6.2、7.0、および7.1 です。<br /> 例: `PowerShellVersion = '5.0'`|
|**PowerShellHostName**<br /> 型: `String`|`<empty string>`|このモジュールが必要とする PowerShell ホストの名前。 この名前は、PowerShell によって提供されます。 ホストプログラムの名前を検索するには、プログラムで「」と入力 `$host.name` します。<br /> 例: `PowerShellHostName = 'ConsoleHost'`|
|**PowerShellHostVersion**<br /> 型: `Version`|`<empty string>`|このモジュールに必要な PowerShell ホストの最小バージョン。<br /> 例: `PowerShellHostVersion = '2.0'`|
|**DotNetFrameworkVersion**<br /> 型: `Version`|`<empty string>`|このモジュールで必要な Microsoft .NET Framework の最小バージョン。 この前提条件は、powershell デスクトップエディションに対してのみ有効です (PowerShell 5.1 など)。<br /> 例: `DotNetFrameworkVersion = '3.5'`|
|**CLRVersion**<br /> 型: `Version`|`<empty string>`|このモジュールで必要な共通言語ランタイム (CLR) の最小バージョン。 この前提条件は、powershell デスクトップエディションに対してのみ有効です (PowerShell 5.1 など)。<br /> 例: `CLRVersion = '3.5'`|
|**ProcessorArchitecture**<br /> 型: `ProcessorArchitecture`|`<empty string>`|このモジュールに必要なプロセッサアーキテクチャ (None、X86、Amd64)。 有効な値は、x86、AMD64、Arm、IA64、MSIL、および None (不明または未指定) です。<br /> 例: `ProcessorArchitecture = 'x86'`|
|**RequiredModules**<br /> 型: `Object[]`|`@()`|このモジュールをインポートする前に、グローバル環境にインポートする必要があるモジュール。 これにより、既に読み込まれている場合を除き、すべてのモジュールが読み込まれます。 たとえば、別のモジュールによって一部のモジュールが既に読み込まれている場合があります。 ではなくを使用して、読み込む特定のバージョンを指定することができ `RequiredVersion` `ModuleVersion` ます。 を使用すると、 `ModuleVersion` 指定された最小バージョンで使用可能な最新バージョンが読み込まれます。 パラメーター値として文字列とハッシュ テーブルを組み合わせることができます。<br /> 例: `RequiredModules = @("MyModule", @{ModuleName="MyDependentModule"; ModuleVersion="2.0"; GUID="cfc45206-1e49-459d-a8ad-5b571ef94857"})`<br /> 例 : `RequiredModules = @("MyModule", @{ModuleName="MyDependentModule"; RequiredVersion="1.5"; GUID="cfc45206-1e49-459d-a8ad-5b571ef94857"})`|
|**RequiredAssemblies**<br /> 型: `String[]`|`@()`|このモジュールをインポートする前に読み込む必要があるアセンブリ。 `.dll`モジュールに必要なアセンブリ () ファイル名を指定します。<br /> PowerShell は、型または形式を更新する前、入れ子になったモジュールをインポートする前、または RootModule キーの値に指定されているモジュールファイルをインポートする前に、指定されたアセンブリを読み込みます。 モジュールに必要なすべてのアセンブリを一覧表示するには、このパラメーターを使用します。<br /> 例: `RequiredAssemblies = @("assembly1.dll", "assembly2.dll", "assembly3.dll")`|
|**ScriptsToProcess**<br /> 型: `String[]`|`@()`|`.ps1`モジュールがインポートされたときに呼び出し元のセッション状態で実行されるスクリプト () ファイル。 グローバルなセッション状態、または入れ子になったモジュールの場合は、別のモジュールのセッション状態になります。 これらのスクリプトを使用すると、ログインスクリプトを使用する場合と同じように、環境を準備できます。<br /> これらのスクリプトは、マニフェストに示されているモジュールのいずれかが読み込まれる前に実行されます。 <br /> 例: `ScriptsToProcess = @("script1.ps1", "script2.ps1", "script3.ps1")`|
|**TypesToProcess**<br /> 型: `String[]`|`@()`|`.ps1xml`このモジュールをインポートするときに読み込む型ファイル ()。 <br /> 例: `TypesToProcess = @("type1.ps1xml", "type2.ps1xml", "type3.ps1xml")`|
|**列挙**<br /> 型: `String[]`|`@()`|`.ps1xml`このモジュールをインポートするときに読み込むフォーマットファイル ()。 <br /> 例: `FormatsToProcess = @("format1.ps1xml", "format2.ps1xml", "format3.ps1xml")`|
|**NestedModules**<br /> 型: `Object[]`|`@()`|**RootModule** で指定されたモジュールの入れ子になったモジュールとしてインポートするモジュール (エイリアス:**ModuleToProcess**)。<br /> モジュール名をこの要素に追加することは `Import-Module` 、スクリプトまたはアセンブリコード内からを呼び出すことに似ています。 マニフェストファイルを使用する場合の主な違いは、読み込み中の内容を簡単に確認できることです。 モジュールの読み込みに失敗した場合、実際のモジュールはまだ読み込まれていません。<br /> 他のモジュールに加えて、スクリプト () ファイルをここに読み込むこともでき `.ps1` ます。 これらのファイルは、ルートモジュールのコンテキストで実行されます。 これは、ルートモジュールでのスクリプトのドットソーシングと同じです。 <br /> 例: `NestedModules = @("script.ps1", @{ModuleName="MyModule"; ModuleVersion="1.0.0.0"; GUID="50cdb55f-5ab7-489f-9e94-4ec21ff51e59"})`|
|**FunctionsToExport**<br /> 型: `String[]`|`@()`|このモジュールからエクスポートする関数を指定します。最適なパフォーマンスを得るには、ワイルドカードを使用せず、エントリを削除しません。エクスポートする関数がない場合は、空の配列を使用します。 既定では、関数はエクスポートされません。 このキーを使用すると、モジュールによってエクスポートされる関数の一覧を表示できます。<br /> モジュールは、関数を呼び出し元のセッション状態にエクスポートします。 呼び出し元のセッション状態は、グローバルなセッション状態にすることも、入れ子になったモジュールの場合は別のモジュールのセッション状態にすることもできます。 入れ子になったモジュールを連結する場合、チェーン内のモジュールが **Functionstoexport** キーを使用して関数を制限しない限り、入れ子になったモジュールによってエクスポートされたすべての関数がグローバルセッション状態にエクスポートされます。<br /> マニフェストによって関数のエイリアスがエクスポートされる場合、このキーを使用する **と、リスト** にエイリアスが含まれている関数を削除できますが、このキーを使用しても、関数エイリアスを一覧に追加することはできません。 <br /> 例: `FunctionsToExport = @("function1", "function2", "function3")`|
|**CmdletsToExport**<br /> 型: `String[]`|`@()`|このモジュールからエクスポートするコマンドレットを指定します。最適なパフォーマンスを得るには、ワイルドカードを使用せず、エントリを削除しません。エクスポートするコマンドレットがない場合は、空の配列を使用します。 既定では、コマンドレットはエクスポートされません。 このキーを使用すると、モジュールによってエクスポートされたコマンドレットを一覧表示できます。<br /> 呼び出し元のセッション状態は、グローバルなセッション状態にすることも、入れ子になったモジュールの場合は別のモジュールのセッション状態にすることもできます。 入れ子になったモジュールをチェーンしている場合、チェーン内のモジュールがコマンドレットをコマンドレットによって **制限しない** 限り、入れ子になったモジュールによってエクスポートされるすべてのコマンドレットがグローバルセッション状態にエクスポートされます。<br /> マニフェストによってコマンドレットのエイリアスがエクスポートされる場合、このキーを使用すると **、リスト** にエイリアスが含まれているコマンドレットを削除できますが、このキーを使用してもコマンドレットのエイリアスを一覧に追加することはできません。 <br /> 例: `CmdletsToExport = @("Get-MyCmdlet", "Set-MyCmdlet", "Test-MyCmdlet")`|
|**変数 Stoexport**<br /> 型: `String[]`|`'*'`|モジュールが呼び出し元のセッション状態にエクスポートする変数を指定します。 ワイルドカード文字を使用できます。 既定では、すべての変数 ( `'*'` ) がエクスポートされます。 このキーを使用すると、モジュールによってエクスポートされる変数を制限できます。<br /> 呼び出し元のセッション状態は、グローバルなセッション状態にすることも、入れ子になったモジュールの場合は別のモジュールのセッション状態にすることもできます。 入れ子になったモジュールをチェーンする場合は、チェーン内のモジュールが variables **Stoexport** キーを使用して変数を制限しない限り、入れ子になったモジュールによってエクスポートされるすべての変数がグローバルセッション状態にエクスポートされます。<br /> マニフェストによって変数のエイリアスもエクスポートされる場合、このキーを使用する **と、リスト** に含まれている別名を持つ変数を削除できますが、このキーでは、変数エイリアスを一覧に追加することはできません。 <br /> 例: `VariablesToExport = @('$MyVariable1', '$MyVariable2', '$MyVariable3')`|
|**AliasesToExport**<br /> 型: `String[]`|`@()`|このモジュールからエクスポートするエイリアスを指定します。最適なパフォーマンスを得るには、ワイルドカードを使用せず、エントリを削除しません。エクスポートするエイリアスがない場合は、空の配列を使用します。 既定では、エイリアスはエクスポートされません。 このキーを使用すると、モジュールによってエクスポートされるエイリアスを一覧表示できます。<br /> モジュールは、エイリアスを呼び出し元のセッション状態にエクスポートします。 呼び出し元のセッション状態は、グローバルなセッション状態にすることも、入れ子になったモジュールの場合は別のモジュールのセッション状態にすることもできます。 入れ子になったモジュールをチェーンしている場合、チェーン内のモジュール **が、このキーを** 使用して別名を制限しない限り、入れ子になったモジュールによってエクスポートされたすべてのエイリアスが最終的にグローバルセッション状態にエクスポートされます。 <br /> 例: `AliasesToExport = @("MyAlias1", "MyAlias2", "MyAlias3")`|
|**DscResourcesToExport**<br /> 型: `String[]`|`@()`|このモジュールからエクスポートする DSC リソースを指定します。 ワイルドカードを使用できます。 <br /> 例: `DscResourcesToExport = @("DscResource1", "DscResource2", "DscResource3")`|
|**ModuleList**<br /> 型: `Object[]`|`@()`|このモジュールでパッケージ化されているすべてのモジュールを指定します。 これらのモジュールは、名前、コンマ区切りの文字列、または **ModuleName** キーと **GUID** キーを持つハッシュテーブルとして入力できます。 ハッシュテーブルには、省略可能な **ModuleVersion** キーを含めることもできます。 **Modulelist** キーは、モジュールインベントリとして機能するように設計されています。 これらのモジュールは自動的に処理されません。 <br /> 例: `ModuleList = @("SampleModule", "MyModule", @{ModuleName="MyModule"; ModuleVersion="1.0.0.0"; GUID="50cdb55f-5ab7-489f-9e94-4ec21ff51e59"})`|
|**FileList**<br /> 型: `String[]`|`@()`|このモジュールでパッケージ化されたすべてのファイルの一覧。 **Modulelist** と同様に、 **FileList** はインベントリリストであり、それ以外の場合は処理されません。 <br /> 例: `FileList = @("File1", "File2", "File3")`|
|**PrivateData**<br /> 型: `Object`|`@{...}`|**RootModule** (Alias: **ModuleToProcess**) キーによって指定されたルートモジュールに渡す必要があるプライベートデータを指定します。 **Privatedata** は、 **Tags**、 **LicenseUri**、 **ProjectURI**、 **IconUri**、 **ReleaseNotes**、 **プレリリース**、 **RequireLicenseAcceptance**、 **externalmoduledependencies** の複数の要素で構成されるハッシュテーブルです。 |
|**タグ** <br /> 型: `String[]` |`@()`| タグは、オンラインギャラリーのモジュール検出に役立ちます。 <br /> 例: `Tags = "PackageManagement", "PowerShell", "Manifest"`|
|**LicenseUri**<br /> 型: `Uri` |`<empty string>`| このモジュールのライセンスの URL。 <br /> 例: `LicenseUri = 'https://www.contoso.com/license'`|
|**ProjectUri**<br /> 型: `Uri` |`<empty string>`| このプロジェクトのメイン web サイトの URL。 <br /> 例: `ProjectUri = 'https://www.contoso.com/project'`|
|**IconUri**<br /> 型: `Uri` |`<empty string>`| このモジュールを表すアイコンの URL。 <br /> 例: `IconUri = 'https://www.contoso.com/icons/icon.png'`|
|**ReleaseNotes**<br /> 型: `String` |`<empty string>`| モジュールのリリースノートを指定します。 <br /> 例: `ReleaseNotes = 'The release notes provide information about the module.`|
|**リリース**<br /> 型: `String` |`<empty string>`| このパラメーターは、PowerShellGet 1.6.6 で追加されました。 オンラインギャラリーのプレリリース版としてモジュールを識別する **プレリリース** 文字列。 <br /> 例: `PreRelease = 'This module is a prerelease version.`|
|**RequireLicenseAcceptance**<br /> 型: `Boolean`|`$true`| このパラメーターは、PowerShellGet 1.5 で追加されました。 モジュールがインストール、更新、または保存のために明示的なユーザー受け入れを必要とするかどうかを示すフラグ。 <br /> 例: `RequireLicenseAcceptance = $false`|
|**ExternalModuleDependencies**<br /> 型: `String[]` |`@()`| このパラメーターは、PowerShellGet v2 で追加されました。 このモジュールが依存している外部モジュールの一覧。 <br /> 例: `ExternalModuleDependencies =  @("ExtModule1", "ExtModule2", "ExtModule3")`|
|**HelpInfoURI**<br /> 型: `String`|`<empty string>`|このモジュールの HelpInfo URI。 <br /> 例: `HelpInfoURI = 'https://www.contoso.com/help'`|
|**DefaultCommandPrefix**<br /> 型: `String`|`<empty string>`|このモジュールからエクスポートされたコマンドの既定のプレフィックス。 を使用して既定のプレフィックスをオーバーライドし `Import-Module -Prefix` ます。 <br /> 例: `DefaultCommandPrefix = 'My'`|

## <a name="sample-module-manifest"></a>サンプルモジュールマニフェスト

次のサンプルモジュールマニフェストは、PowerShell 7 ではを使用して作成され、 `New-ModuleManifest` 既定のキーと値が含まれています。

```powershell
#
# Module manifest for module 'SampleModuleManifest'
#
# Generated by: User01
#
# Generated on: 10/15/2019
#

@{

# Script module or binary module file associated with this manifest.
# RootModule = ''

# Version number of this module.
ModuleVersion = '0.0.1'

# Supported PSEditions
# CompatiblePSEditions = @()

# ID used to uniquely identify this module
GUID = 'b632e90c-df3d-4340-9f6c-3b832646bf87'

# Author of this module
Author = 'User01'

# Company or vendor of this module
CompanyName = 'Unknown'

# Copyright statement for this module
Copyright = '(c) User01. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the PowerShell engine required by this module
# PowerShellVersion = ''

# Name of the PowerShell host required by this module
# PowerShellHostName = ''

# Minimum version of the PowerShell host required by this module
# PowerShellHostVersion = ''

# Minimum version of Microsoft .NET Framework required by this module. This prerequisite is valid for the PowerShell Desktop edition only.
# DotNetFrameworkVersion = ''

# Minimum version of the common language runtime (CLR) required by this module. This prerequisite is valid for the PowerShell Desktop edition only.
# CLRVersion = ''

# Processor architecture (None, X86, Amd64) required by this module
# ProcessorArchitecture = ''

# Modules that must be imported into the global environment prior to importing this module
# RequiredModules = @()

# Assemblies that must be loaded prior to importing this module
# RequiredAssemblies = @()

# Script files (.ps1) that are run in the caller's environment prior to importing this module.
# ScriptsToProcess = @()

# Type files (.ps1xml) to be loaded when importing this module
# TypesToProcess = @()

# Format files (.ps1xml) to be loaded when importing this module
# FormatsToProcess = @()

# Modules to import as nested modules of the module specified in RootModule/ModuleToProcess
# NestedModules = @()

# Functions to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no functions to export.
FunctionsToExport = @()

# Cmdlets to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no cmdlets to export.
CmdletsToExport = @()

# Variables to export from this module
VariablesToExport = '*'

# Aliases to export from this module, for best performance, do not use wildcards and do not delete the entry, use an empty array if there are no aliases to export.
AliasesToExport = @()

# DSC resources to export from this module
# DscResourcesToExport = @()

# List of all modules packaged with this module
# ModuleList = @()

# List of all files packaged with this module
# FileList = @()

# Private data to pass to the module specified in RootModule/ModuleToProcess. This may also contain a PSData hashtable with additional module metadata used by PowerShell.
PrivateData = @{

    PSData = @{

        # Tags applied to this module. These help with module discovery in online galleries.
        # Tags = @()

        # A URL to the license for this module.
        # LicenseUri = ''

        # A URL to the main website for this project.
        # ProjectUri = ''

        # A URL to an icon representing this module.
        # IconUri = ''

        # ReleaseNotes of this module
        # ReleaseNotes = ''

        # Prerelease string of this module
        # Prerelease = ''

        # Flag to indicate whether the module requires explicit user acceptance for install/update/save
        RequireLicenseAcceptance = $true

        # External dependent modules of this module
        # ExternalModuleDependencies = @()

    } # End of PSData hashtable

} # End of PrivateData hashtable

# HelpInfo URI of this module
# HelpInfoURI = ''

# Default prefix for commands exported from this module. Override the default prefix using Import-Module -Prefix.
# DefaultCommandPrefix = ''

}
```

## <a name="see-also"></a>関連項目

[about_Comparison_Operators](/powershell/module/microsoft.powershell.core/about/about_comparison_operators)

[about_If](/powershell/module/microsoft.powershell.core/about/about_if)

[グローバル アセンブリ キャッシュ](/dotnet/framework/app-domains/gac)

[Import-Module](/powershell/module/Microsoft.PowerShell.Core/Import-Module)

[New-ModuleManifest](/powershell/module/microsoft.powershell.core/new-modulemanifest)

[Test-ModuleManifest](/powershell/module/microsoft.powershell.core/test-modulemanifest)

[Update-ModuleManifest](/powershell/module/powershellget/update-modulemanifest)

[Windows PowerShell モジュールを記述する](./writing-a-windows-powershell-module.md)
