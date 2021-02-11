---
title: PowerShell モジュール アセンブリの依存関係の競合の解決
description: C# でバイナリ PowerShell モジュールを記述する場合、機能を提供するために他のパッケージやライブラリに依存するのは自然なことです。
ms.date: 06/25/2020
ms.custom: rjmholt
ms.openlocfilehash: 93bb39bdd440c7f97c27aa81e68f68331569b69e
ms.sourcegitcommit: 2fc6ee49a70bda4c59135136bd5cc7782836a124
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/18/2020
ms.locfileid: "94810404"
---
# <a name="resolving-powershell-module-assembly-dependency-conflicts"></a>PowerShell モジュール アセンブリの依存関係の競合の解決

C# でバイナリ PowerShell モジュールを記述する場合、機能を提供するために他のパッケージやライブラリに依存するのは自然なことです。 コードの再利用のためには、他のライブラリに依存するのは望ましいことです。 PowerShell では、アセンブリは常に同じコンテキストに読み込まれます。 そのため、モジュールの依存関係が既に読み込まれている DLL と競合し、そうでなければ同じ PowerShell セッション内で関係のないはずの 2 つのモジュールを、使用できなくなる場合があるという問題が発生します。

この問題が発生した場合は、次のようなエラー メッセージが表示されます。

![アセンブリ読み込み競合のエラー メッセージ](./media/resolving-dependency-conflicts/moduleconflict.png)

この記事では、どのような場合に PowerShell で依存関係の競合が発生するか、そして依存関係の競合の問題を軽減する方法について説明します。 モジュールの作成者でない場合でも、ここで説明するいくつかのテクニックは、モジュールの使用時に発生する依存関係の競合に役立ちます。

## <a name="why-do-dependency-conflicts-occur"></a>依存関係の競合が発生する理由

.NET では、同じアセンブリの 2 つのバージョンが同じ "_アセンブリ読み込みコンテキスト_" に読み込まれると、依存関係の競合が発生します。 この用語の意味は、.NET プラットフォームによって若干異なります。これについては、[後で](#powershell-and-net)説明します。 この競合は一般的な問題であり、バージョン管理された依存関係を使用するすべてのソフトウェアで発生します。 簡単な解決策はなく、多くの依存関係解決フレームワークでは異なる方法で問題の解決が試みられるため、このような状況は "依存関係の地獄" と呼ばれることがよくあります。

競合の問題は、プロジェクトで同じ依存関係の 2 つのバージョンに、意図的に、または直接依存することはほとんどないという事実によって、さらに複雑になります。 そうではなく、プロジェクトの複数の依存関係のそれぞれで、同じ依存関係の異なるバージョンが必要になります。

たとえば、次のような .NET アプリケーション `DuckBuilder` があり、その機能の一部を実行するために 2 つの依存関係が読み込まれているとします。

![DuckBuilder の 2 つの依存関係は、Newtonsoft.Json の異なるバージョンに依存している](./media/resolving-dependency-conflicts/dep-conflict.jpg)

`Contoso.ZipTools` と `Fabrikam.FileHelpers` はどちらも異なるバージョンの `Newtonsoft.Json` に依存しているため、各依存関係の読み込み方法によっては、依存関係の競合が発生する可能性があります。

### <a name="conflicting-with-powershells-dependencies"></a>PowerShell の依存関係での競合

Powershell では、PowerShell モジュールと PowerShell 独自の依存関係が同じ共有コンテキストに読み込まれるため、依存関係の競合の問題がいっそう大きくなります。 これは、PowerShell エンジンと、読み込まれるすべての PowerShell モジュールに、競合する依存関係があってはならないことを意味します。 これの典型的な例が `Newtonsoft.Json` です。

![FictionalTools モジュールは PowerShell より新しいバージョンの Newtonsoft.Json に依存している](./media/resolving-dependency-conflicts/engine-conflict.jpg)

この例では、モジュール `FictionalTools` は `Newtonsoft.Json` のバージョン `12.0.3` に依存していますが、これは例の PowerShell に付属する `11.0.2` より新しいバージョンの `Newtonsoft.Json` です。

> [!NOTE]
> これは例であり、現在の PowerShell 7 には `Newtonsoft.Json 12.0.3` が付属しています。

モジュールは新しいバージョンのアセンブリに依存しているため、PowerShell によって既に読み込まれているバージョンを受け付けません。 しかし、PowerShell によってアセンブリのあるバージョンが既に読み込まれているため、モジュールでは従来の読み込みメカニズムを使用して独自のバージョンを読み込むことはできません。

### <a name="conflicting-with-another-modules-dependencies"></a>別のモジュールの依存関係との競合

PowerShell でのもう 1 つの一般的なシナリオは、あるバージョンのアセンブリに依存するモジュールが読み込まれた後、そのアセンブリの異なるバージョンに依存する別のモジュールが読み込まれる場合です。

多くの場合、これは次のようになります。

![2 つの PowerShell モジュールでは、異なるバージョンの Microsoft.Extensions.Logging 依存関係が必要である](./media/resolving-dependency-conflicts/mod-conflict.jpg)

この場合、`FictionalTools` モジュールでは `FilesystemManager` モジュールより新しいバージョンの `Microsoft.Extensions.Logging` が必要です。

これらのモジュールで、ルート モジュール アセンブリと同じディレクトリに依存関係アセンブリを配置することにより、依存関係を読み込むものとします。 これにより、.NET では名前を使用して暗黙的にそれらを読み込むことができます。 (.NET Core 3.1 上の) PowerShell 7 を実行している場合は、`FictionalTools` を読み込んで実行した後、問題なく `FilesystemManager` を読み込んで実行できます。 しかし、新しいセッションで `FilesystemManager` を読み込んで実行した後、`FictionalTools` を読み込むと、読み込まれているものより新しいバージョンの `Microsoft.Extensions.Logging` が必要になるため、`FictionalTools` コマンドで `FileLoadException` が発生します。
同じ名前のアセンブリが既に読み込まれているため、`FictionalTools` では必要なバージョンを読み込むことができません。

## <a name="powershell-and-net"></a>PowerShell と .NET

PowerShell は .NET プラットフォーム上で実行されます。 アセンブリの依存関係の解決と読み込みは最終的に .NET によって行われるため、依存関係の競合について理解するには、.NET の動作方法を理解する必要があります。

また、異なるバージョンの PowerShell は異なる .NET の実装で実行されるという事実についても考える必要があります。 一般に、PowerShell 5.1 以前は .NET Framework で実行され、PowerShell 6 以降は .NET Core で実行されます。 .NET のこれら 2 つの実装では、アセンブリの読み込みと処理が異なります。
つまり、依存関係の競合の解決は、基になる .NET プラットフォームによって異なる場合があります。

### <a name="assembly-load-contexts"></a>アセンブリ読み込みコンテキスト

.NET では、アセンブリが読み込まれるランタイム名前空間である "_アセンブリ読み込みコンテキスト_" (ALC)。 アセンブリの名前は一意である必要があります。 この概念により、各 ALC 内の名前によってアセンブリを一意に解決できます。

### <a name="assembly-reference-loading-in-net"></a>.NET でのアセンブリ参照読み込み

アセンブリ読み込みのセマンティクスは、.NET の実装 (.NET Core または .NET Framework) と、特定のアセンブリの読み込みに使用される .NET API の両方に依存します。 ここでは詳細については説明しません。.NET の各実装で .NET アセンブリの読み込みがどのように機能するかについて詳しくは、「[関連項目](#further-reading)」セクションのリンクをご覧ください。

この記事では、次のメカニズムについて説明します。

- .NET により .NET コード内の静的アセンブリ参照からの名前でのアセンブリの読み込みが暗黙的に試みられるときの、暗黙でのアセンブリの読み込み (実質的には `Assembly.Load(AssemblyName)`)。
- `Assembly.LoadFrom()`。読み込まれた DLL の依存関係を解決するためのハンドラーを追加する、プラグイン指向の読み込み API。 このメソッドでは、必要な方法で依存関係が解決されない場合があります。
- `Assembly.LoadFile()`。要求されたアセンブリの読み込みだけが行われ、依存関係の処理は行われない、基本的な読み込み API。

### <a name="differences-in-net-framework-vs-net-core"></a>.NET Framework と .NET Core の相違点

.NET Core と .NET Framework では、これらの API の動作が微妙に変更されているため、[リンク](#further-reading)の参照先をよく読んでください。 重要なのは、アセンブリ読み込みコンテキストと他のアセンブリ解決メカニズムが、.NET Framework と .NET Core の間で変更されていることです。

具体的には、.NET Framework には次の機能があります。

- グローバル アセンブリ キャッシュは、コンピューター全体のアセンブリ解決用です
- アプリケーション ドメインは、アセンブリの分離に関してはプロセス内サンドボックスと同様に機能しますが、競合するシリアル化レイヤーも提供されます
- 制限付きアセンブリ読み込みコンテキスト モデル (詳細については[こちら](/dotnet/framework/deployment/best-practices-for-assembly-loading)を参照) には、それぞれが独自の動作を備えたアセンブリ読み込みコンテキストの固定セットがあります
  - 既定の読み込みコンテキストでは、アセンブリが既定で読み込まれます
  - 読み込み元コンテキストは、実行時に手動でアセンブリを読み込むためのものです
  - リフレクションのみのコンテキストは、アセンブリを実行せずにそのメタデータを読み取る、安全にアセンブリを読み込むためのものです
  - ミステリアス void には、`Assembly.LoadFile(string path)` と `Assembly.Load(byte[] asmBytes)` で読み込まれるアセンブリが存在します

.NET Core (および .NET 5 以降) では、この複雑さが単純なモデルに置き換えられています。

- グローバル アセンブリ キャッシュはありません。 アプリケーション自体のすべての依存関係がアプリケーションに読み込まれます。 これにより、アプリケーションで依存関係を解決するための外部要因がなくなり、依存関係の解決がより再現しやすくなります。
  プラグイン ホストとしての PowerShell により、モジュールではこれが少し複雑になります。 `$PSHOME` 内の依存関係は、すべてのモジュールで共有されます。
- アプリケーション ドメインは 1 つだけで、新しいものを作成することはできません。 .NET Core では、アプリケーション ドメインの概念は .NET プロセスのグローバルな状態としてのみ維持されています。
- 新しい拡張可能なアセンブリ読み込みコンテキスト モデル。 アセンブリの解決は、新しい ALC に配置することによって、名前空間化できます。 .NET Core のプロセスは、すべてのアセンブリが読み込まれる既定の 1 つの ALC で始まります。 (`Assembly.LoadFile(string)` および `Assembly.Load(byte[])` で読み込まれるものは除きます)。ただし、プロセスでは、独自の読み込みロジックを使用して独自のカスタム ALC を作成および定義することができます。 アセンブリが読み込まれるとき、最初の読み込み先 ALC によって依存関係の解決が行われます。 これにより、強力な .NET プラグイン読み込みメカニズムを実装することができます。

どちらの実装でも、アセンブリは遅延読み込みされます。 これは、その型を必要とするメソッドが初めて実行されるときに読み込まれることを意味します。

たとえば、同じコードの 2 つのバージョンがあり、依存関係を読み込むタイミングが異なるとします。

1 番目では、依存関係の参照がメソッド内に構文的に存在するため、依存関係は `Program.GetRange()` が呼び出されるときに常に読み込まれます。

```csharp
using Dependency.Library;

public static class Program
{
    public static List<int> GetRange(int limit)
    {
        var list = new List<int>();
        for (int i = 0; i < limit; i++)
        {
            if (i >= 20)
            {
                // Dependency.Library will be loaded when GetRange is run
                // because the dependency call occurs directly within the method
                DependencyApi.Use();
            }

            list.Add(i);
        }
        return list;
    }
}
```

2 番目では、メソッドによる内部間接参照であるため、依存関係は `limit` パラメーターが 20 以上の場合にのみ読み込まれます。

```csharp
using Dependency.Library;

public static class Program
{
    public static List<int> GetNumbers(int limit)
    {
        var list = new List<int>();
        for (int i = 0; i < limit; i++)
        {
            if (i >= 20)
            {
                // Dependency.Library is only referenced within
                // the UseDependencyApi() method,
                // so will only be loaded when limit >= 20
                UseDependencyApi();
            }

            list.Add(i);
        }
        return list;
    }

    private static void UseDependencyApi()
    {
        // Once UseDependencyApi() is called, Dependency.Library is loaded
        DependencyApi.Use();
    }
}
```

メモリとファイル システムの I/O が最小限になり、リソースの使用効率が向上するため、これは良い方法です。 残念ながら、これには、アセンブリの読み込みを試みるコード パスに達するまで、アセンブリの読み込みが失敗することがわからないという副作用があります。

また、アセンブリ読み込みの競合に対して、タイミング条件が作成される可能性もあります。 同じプログラムの 2 つの部分で、同じアセンブリの異なるバージョンの読み込みが試みられた場合、読み込まれるバージョンは、最初に実行されるコード パスによって決まります。

PowerShell の場合、これは、次の要因がアセンブリ読み込みの競合に影響する可能性があることを意味します。

- どのモジュールが最初に読み込まれたか
- 依存関係ライブラリを使用するコード パスは実行されたか
- PowerShell では、競合する依存関係が、起動時に読み込まれるか、それとも特定のコード パスでのみ読み込まれるか

## <a name="quick-fixes-and-their-limitations"></a>簡単な修正とその制限事項

場合によっては、モジュールを微調整し、最小限の労力で修正することができます。 ただし、これらの解決策には注意が伴います。 モジュールに適用できますが、モジュールによっては機能しないことがあります。

### <a name="change-your-dependency-version"></a>依存関係のバージョンを変更する

依存関係の競合を回避する最も簡単な方法は、依存関係に同意することです。 これは、次の場合にできる可能性があります。

- 競合がモジュールの直接的な依存関係に関するものであり、自分でバージョンを管理している。
- 競合は間接的な依存関係に関するものだが、動作可能な間接的依存関係バージョンを使用するように、直接的な依存関係を構成できる。
- 競合しているバージョンがわかっており、それが変更されていないことを信頼できる。

`Newtonsoft.Json` パッケージは、この最後のシナリオの好例です。 これは、PowerShell 6 以降の依存関係であり、Windows PowerShell では使用されません。 つまり、バージョン管理の競合を解決する簡単な方法は、対象とする PowerShell のすべてのバージョンで最も低いバージョンの `Newtonsoft.Json` をターゲットにすることです。

たとえば、PowerShell 6.2.6 と PowerShell 7.0.2 のどちらでも、現在は `Newtonsoft.Json` バージョン 12.0.3 が使用されています。 そのため、Windows PowerShell、PowerShell 6、PowerShell 7 をターゲットとするモジュールを作成するには、依存関係として `Newtonsoft.Json 12.0.3` をターゲットにし、ビルドされるモジュールにそれを組み込みます。 モジュールが PowerShell 6 または 7 に読み込まれるときには、PowerShell 独自の `Newtonsoft.Json` アセンブリが既に読み込まれています。 また、それはモジュールに必要な最低のバージョンであるため、解決は成功します。 Windows PowerShell では、アセンブリは PowerShell 内にまだ存在しません。 そのため、代わりにモジュール フォルダーから読み込まれます。

一般に、`Microsoft.PowerShell.Sdk` や `System.Management.Automation` などの具体的な PowerShell パッケージをターゲットとする場合、NuGet で必要とされる適切な依存関係のバージョンを解決できるはずです。 Windows PowerShell と PowerShell 6 以降の両方をターゲットにすると、複数のフレームワークまたは `PowerShellStandard.Library` のどちらを対象にするかを選択する必要があるため、より困難になります。

次のような状況では、共通の依存関係バージョンへの固定は動作しません。

- 競合が間接的な依存関係に関するものであり、どの依存関係も共通バージョンを使用するように構成できない。
- 他の依存関係のバージョンが頻繁に変更される可能性があるため、共通バージョンへの固定は短期的な解決にすぎない。

### <a name="add-an-assemblyresolve-event-handler-to-create-a-dynamic-binding-redirect"></a>AssemblyResolve イベント ハンドラーを追加して動的バインド リダイレクトを作成する

独自の依存関係のバージョンを変更することが不可能な場合や、難しすぎる場合があります。 別の方法として、`AssemblyResolve` イベントに対してイベント ハンドラーを登録することで、動的バインド リダイレクトを設定することもできます。

静的バインド リダイレクトと同様に、重要なのは、依存関係のすべてのコンシューマーで、実際の同じアセンブリを使用させることです。 依存関係を読み込む呼び出しをインターセプトし、それらを目的のバージョンに常にリダイレクトします。

これは次のようになります。

```csharp
// Register the event handler as early as you can in your code.
// A good option is to use the IModuleAssemblyInitializer interface
// that PowerShell provides to run code early on when your module is loaded.

// This class will be instantiated on module import and the OnImport() method run.
// Make sure that:
//  - the class is public
//  - the class has a public, parameterless constructor
//  - the class implements IModuleAssemblyInitializer
public class MyModuleInitializer : IModuleAssemblyInitializer
{
    public void OnImport()
    {
        AppDomain.CurrentDomain.AssemblyResolve += DependencyResolution.ResolveNewtonsoftJson;
    }
}

// Clean up the event handler when the the module is removed
// to prevent memory leaks.
//
// Like IModuleAssemblyInitializer, IModuleAssemblyCleanup allows
// you to register code to run when a module is removed (with Remove-Module).
// Make sure it is also public with a public parameterless contructor
// and implements IModuleAssemblyCleanup.
public class MyModuleCleanup : IModuleAssemblyCleanup
{
    public void OnRemove()
    {
        AppDomain.CurrentDomain.AssemblyResolve -= DependencyResolution.ResolveNewtonsoftJson;
    }
}

internal static class DependencyResolution
{
    private static readonly string s_modulePath = Path.GetDirectoryName(
        Assembly.GetExecutingAssembly().Location);

    public static Assembly ResolveNewtonsoftJson(object sender, ResolveEventArgs args)
    {
        // Parse the assembly name
        var assemblyName = new AssemblyName(args.Name);

        // We only want to handle the dependency we care about.
        // In this example it's Newtonsoft.Json.
        if (!assemblyName.Name.Equals("Newtonsoft.Json"))
        {
            return null;
        }

        // Generally the version of the dependency you want to load is the higher one,
        // since it's the most likely to be compatible with all dependent assemblies.
        // The logic here assumes our module always has the version we want to load.
        // Also note the use of Assembly.LoadFrom() here rather than Assembly.LoadFile().
        return Assembly.LoadFrom(Path.Combine(s_modulePath, "Newtonsoft.Json.dll"));
    }
}
```

技術的には PowerShell 内の ScriptBlock を登録して `AssemblyResolve` イベントを処理できますが、次のようになります。

- `AssemblyResolve` イベントは任意のスレッドでトリガーされる可能性があり、PowerShell で処理できないものである場合があります。
- イベントが適切なスレッドで処理されている場合でも、PowerShell のスコープ設定の概念では、外部で定義されている変数に依存しないように、イベント ハンドラーの ScriptBlock を慎重に記述する必要があります。

共通バージョンへのリダイレクトが機能しない場合があります。

- 依存関係のバージョン間で破壊的変更が行われた場合、またはそれ以外で、リダイレクト先のバージョンでは利用できない機能に依存コードが依存している場合。
- 競合する依存関係より前に、モジュールが読み込まれない場合。 依存関係が読み込まれた後で `AssemblyResolve` イベント ハンドラーを登録すると、そのイベント ハンドラーは実行されません。 別のモジュールとの依存関係の競合を回避しようとしている場合、最初に他のモジュールが読み込まれる可能性があるため、これが問題になることがあります。

### <a name="use-the-dependency-out-of-process"></a>プロセス外の依存関係を使用する

この解決策は、モジュール作成者よりモジュール ユーザー向けです。 この解決策は、既存の依存関係の競合が原因でモジュールが動作しない場合に使用します。

依存関係の競合は、同じアセンブリの 2 つのバージョンが同じ .NET プロセスに読み込まれることが原因で発生します。 簡単な解決策は、異なる両方のプロセスからでも機能を使用できる場合は、それらを異なるプロセスに読み込むことです。

PowerShell には、これを実現するための方法がいくつかあります。

- サブプロセスとして PowerShell を呼び出す

  現在のプロセスの外部で PowerShell コマンドを実行する簡単な方法は、コマンドの呼び出しで新しい PowerShell プロセスを直接開始することです。

  ```powershell
  pwsh -c 'Invoke-ConflictingCommand'
  ```

  この方法の主な制限事項は、他のオプションより、結果の再構築が困難であったり、エラーが発生しやすくなったりすることです。

- PowerShell ジョブ システム

  PowerShell ジョブ システムでも、コマンドを新しい PowerShell プロセスに送信し、結果を返すことにより、プロセスの外部でコマンドを実行できます。

  ```powershell
  $result = Start-Job { Invoke-ConflictingCommand } | Receive-Job -Wait
  ```

  この場合は、変数と状態が正しく渡されることを確認するだけで済みます。

  また、ジョブ システムは、小さいコマンドを実行するときは多少煩雑になることがあります。

- PowerShell リモート処理

  PowerShell リモート処理を使用できる場合は、プロセスの外部でコマンドを実行する便利な手段になります。 リモート処理では、新しいプロセス内に新しい **PSSession** を作成し、PowerShell リモート処理を使用してそのコマンドを呼び出した後、競合する依存関係が含まれる他のモジュールでローカルに結果を使用できます。

  次に例を示します。

  ```powershell
  # Create a local PowerShell session
  # where the module with conflicting assemblies will be loaded
  $s = New-PSSession

  # Import the module with the conflicting dependency via remoting,
  # exposing the commands locally
  Import-Module -PSSession $s -Name ConflictingModule

  # Run a command from the module with the conflicting dependencies
  Invoke-ConflictingCommand
  ```

- Windows PowerShell に対する暗黙的なリモート処理

  PowerShell 7 でのもう 1 つのオプションは、`Import-Module` で `-UseWindowsPowerShell` フラグを使用することです。 これにより、ローカル リモート処理セッションを通じて Windows PowerShell にモジュールがインポートされます。

  ```powershell
  Import-Module -Name ConflictingModule -UseWindowsPowerShell
  ```

  Windows PowerShell では、モジュールが動作しない場合、または動作が異なる場合があることに注意してください。

#### <a name="when-out-of-process-invocation-should-not-be-used"></a>プロセス外の呼び出しを使用してはいけない場合

モジュールの作成者に関しては、プロセス外のコマンド呼び出しをモジュールに組み込むのは難しく、問題の原因となるエッジ ケースが存在する場合があります。 具体的には、モジュールが動作する必要がある環境によっては、リモート処理とジョブを利用できない場合があります。 ただし、実装をプロセス外に移動し、PowerShell モジュールをシン クライアントとして使用できるようにするための一般的な原則が、引き続き適用される可能性があります。

モジュールのユーザーに関しては、プロセス外の呼び出しが機能しない場合があります。

- 機能を使用する権限がないか、機能が有効になっていないため、PowerShell リモート処理を使用できない場合。
- メソッドまたは別のコマンドへの入力として、特定の .NET 型が出力で必要な場合。
  PowerShell リモート処理経由で実行されるコマンドからは、厳密に型指定された .NET オブジェクトではなく、逆シリアル化されたオブジェクトが出力されます。 これは、リモート処理経由でインポートされたコマンドの出力では、メソッドの呼び出しや厳密に型指定された API が動作しないことを意味します。

## <a name="more-robust-solutions"></a>より堅牢な解決策

これまでの解決策にはすべて、動作しないシナリオとモジュールがありました。 ただし、これらには、正しく実装するのが比較的簡単であるという長所もあります。 以下のソリューションはより堅牢ですが、適切に実装するにはより多くの労力が必要になり、慎重に記述されていない場合は微妙なバグが発生する可能性があります。

### <a name="loading-through-net-core-assembly-load-contexts"></a>.NET Core アセンブリ読み込みコンテキスト経由での読み込み

.NET Core 1.0 では、同じアセンブリの複数のバージョンを同じランタイムに読み込む必要がある場合に特に対処するため、実装可能な API としての[アセンブリ読み込みコンテキスト](/dotnet/api/system.runtime.loader.assemblyloadcontext) (ALC) が導入されました。

.NET Core および .NET 5 では、それらによって、アセンブリの競合するバージョンの読み込みの問題に対する最も堅牢な解決策が提供されます。 ただし、カスタム ALC は .NET Framework では使用できません。 これは、この解決策が PowerShell 6 以降でのみ機能することを意味します。

現在、PowerShell での依存関係の分離に対して ALC を使用する場合の最適な例は、Visual Studio Code 用の PowerShell 拡張機能に対する言語サーバーである PowerShell エディター サービスでのものです。 PowerShell モジュール内で PowerShell エディター サービスの独自の依存関係がクラッシュしないように、[ALC が使用されます](https://github.com/PowerShell/PowerShellEditorServices/blob/master/src/PowerShellEditorServices.Hosting/Internal/PsesLoadContext.cs)。

ALC を使用してモジュールの依存関係の分離を実装することは、概念的には難しいことですが、ここでは最小限の例について説明します。 PowerShell 7 だけで動作することが意図された簡単なモジュールについて考えます。
ソース コードは次のように構成されています。

```
+ AlcModule.psd1
+ src/
    + TestAlcModuleCommand.cs
    + AlcModule.csproj
```

コマンドレットの実装は次のようになります。

```csharp
using Shared.Dependency;

namespace AlcModule
{
    [Cmdlet(VerbsDiagnostic.Test, "AlcModule")]
    public class TestAlcModuleCommand : Cmdlet
    {
        protected override void EndProcessing()
        {
            // Here's where our dependency gets used
            Dependency.Use();
            // Something trivial to make our cmdlet do *something*
            WriteObject("done!");
        }
    }
}
```

(大幅に単純化された) マニフェストは次のようになります。

```powershell
@{
    Author = 'Me'
    ModuleVersion = '0.0.1'
    RootModule = 'AlcModule.dll'
    CmdletsToExport = @('Test-AlcModule')
    PowerShellVersion = '7.0'
}
```

そして、`csproj` は次のようになります。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Shared.Dependency" Version="1.0.0" />
    <PackageReference Include="Microsoft.PowerShell.Sdk" Version="7.0.1" PrivateAssets="all" />
  </ItemGroup>
</Project>
```

このモジュールをビルドすると、生成される出力のレイアウトは次のようになります。

```
AlcModule/
  + AlcModule.psd1
  + AlcModule.dll
  + Shared.Dependency.dll
```

この例の `Shared.Dependency.dll` アセンブリには、競合する依存関係の問題があるものと仮定します。 これは、モジュール固有のバージョンを使用できるように、ALC の背後に配置する必要がある依存関係です。

次のようにモジュールを再設計する必要があります。

- 競合が発生しないように、モジュールの依存関係は、PowerShell の ALC ではなく、カスタム ALC にのみ読み込みます。 さらに、プロジェクトに依存関係を追加するとき、読み込みが動作し続けるよう、コードを継続的に追加するようなことはしたくありません。 代わりに、再利用可能で汎用的な依存関係解決ロジックが必要です。
- PowerShell でのモジュールの読み込みは通常どおりに動作します。 PowerShell モジュール システムで必要なコマンドレットと他の型は、PowerShell 独自の ALC 内で定義されています。

これらの 2 つの要件を仲介するには、モジュールを 2 つのアセンブリに分割する必要があります。

- PowerShell のモジュール システムでモジュールを正しく読み込むために必要なすべての型の定義が含まれるコマンドレット アセンブリ `AlcModule.Cmdlets.dll`。 つまり、`Cmdlet` 基底クラスと、`AssemblyLoadContext.Default.Resolving` でカスタム ALC を通して `AlcModule.Engine.dll` が適切に読み込まれるようにイベント ハンドラーを設定する `IModuleAssemblyInitializer` が実装されているクラスの、すべての実装。 PowerShell 7 では、他の ALC で読み込まれるアセンブリで定義されている型は意図的に非表示にされているため、PowerShell にパブリックに公開される型は、ここでも定義する必要があります。 最後に、カスタム ALC の定義がこのアセンブリで定義されている必要があります。 このアセンブリ内のそれ以外のコードは、できるだけ少なくする必要があります。
- モジュールの実際の実装を処理するエンジン アセンブリ `AlcModule.Engine.dll`。
  PowerShell ALC でこれに含まれる型を使用できますが、最初はカスタム ALC を通じて読み込まれます。 その依存関係は、カスタム ALC にのみ読み込まれます。 実質的に、これは 2 つの ALC 間の "_ブリッジ_" になります。

このブリッジの概念を使用すると、新しいアセンブリの状況は次のようになります。

![2 つの ALC をブリッジする AlcModule.Engine.dll を表す図](./media/resolving-dependency-conflicts/alc-diagram.jpg)

既定の ALC の依存関係プローブ ロジックによって、カスタム ALC に読み込まれる依存関係が解決されないようにするには、モジュールのこれら 2 つの部分を、異なるディレクトリに分ける必要があります。 新しいモジュール レイアウトは次のような構造になります。

```
AlcModule/
  AlcModule.Cmdlets.dll
  AlcModule.psd1
  Dependencies/
  | + AlcModule.Engine.dll
  | + Shared.Dependency.dll
```

実装がどのように変化するかを確認するため、最初に `AlcModule.Engine.dll` の実装について説明します。

```csharp
using Shared.Dependency;

namespace AlcModule.Engine
{
    public class AlcEngine
    {
        public static void Use()
        {
            Dependency.Use();
        }
    }
}
```

これは依存関係 `Shared.Dependency.dll` 用の簡単なコンテナーですが、他のアセンブリ内のコマンドレットによって PowerShell 用にラップされる機能に対する .NET API と考える必要があります。

`AlcModule.Cmdlets.dll` のコマンドレットは次のようになります。

```csharp
// Reference our module's Engine implementation here
using AlcModule.Engine;

namespace AlcModule.Cmdlets
{
    [Cmdlet(VerbsDiagnostic.Test, "AlcModule")]
    public class TestAlcModuleCommand : Cmdlet
    {
        protected override void EndProcessing()
        {
            AlcEngine.Use();
            WriteObject("done!");
        }
    }
}
```

この時点で、**AlcModule** を読み込んで `Test-AlcModule` を実行すると、既定の ALC によって `Alc.Engine.dll` が読み込まれて `EndProcessing()` の実行が試みられるときに、`FileNotFoundException` が発生します。 これは、非表示にする依存関係が既定の ALC によって検出されないことを意味するので、適切なことです。

次に、`AlcModule.Engine.dll` の解決方法を認識できるように、`AlcModule.Cmdlets.dll` にコードを追加する必要があります。 最初に、モジュールの `Dependencies` ディレクトリからアセンブリを解決するカスタム ALC を定義する必要があります。

```csharp
namespace AlcModule.Cmdlets
{
    internal class AlcModuleAssemblyLoadContext : AssemblyLoadContext
    {
        private readonly string _dependencyDirPath;

        public AlcModuleAssemblyLoadContext(string dependencyDirPath)
        {
            _dependencyDirPath = dependencyDirPath;
        }

        protected override Assembly Load(AssemblyName assemblyName)
        {
            // We do the simple logic here of
            // looking for an assembly of the given name
            // in the configured dependency directory
            string assemblyPath = Path.Combine(
                _dependencyDirPath,
                $"{assemblyName.Name}.dll");

            // The ALC must use inherited methods to load assemblies
            // Assembly.Load*() won't work here
            return LoadFromAssemblyPath(assemblyPath);
        }
    }
}
```

次に、カスタム ALC を既定の ALC の `Resolving` イベントにフックする必要があります。これは、アプリケーション ドメインでの `AssemblyResolve` イベントの ALC バージョンです。 `EndProcessing()` が呼び出されると、`AlcModule.Engine.dll` を見つけるためにこのイベントが発生します。

```csharp
namespace AlcModule.Cmdlets
{
    public class AlcModuleResolveEventHandler : IModuleAssemblyInitializer, IModuleAssemblyCleanup
    {
        // Get the path of the dependency directory.
        // In this case we find it relative to the AlcModule.Cmdlets.dll location
        private static readonly string s_dependencyDirPath = Path.GetFullPath(
            Path.Combine(
                Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location),
                "Dependencies"));

        private static readonly AlcModuleAssemblyLoadContext s_dependencyAlc = new AlcModuleAssemblyLoadContext(s_dependencyDirPath);

        public void OnImport()
        {
            // Add the Resolving event handler here
            AssemblyLoadContext.Default.Resolving += ResolveAlcEngine;
        }

        public void OnRemove()
        {
          // Remove the Resolving event handler here
          AssemblyLoadContext.Default.Resolving -= ResolveAlcEngine;
        }

        private static Assembly ResolveAlcEngine(
            AssemblyLoadContext defaultAlc,
            AssemblyName assemblyToResolve)
        {
            // We only want to resolve the Alc.Engine.dll assembly here.
            // Because this will be loaded into the custom ALC,
            // all of *its* dependencies will be resolved
            // by the logic we defined for that ALC's implementation.
            //
            // Note that we are safe in our assumption that the name is enough
            // to distinguish our assembly here,
            // since it's unique to our module.
            // There should be no other AlcModule.Engine.dll on the system.
            if (!assemblyToResolve.Name.Equals("AlcModule.Engine"))
            {
                return null;
            }

            // Allow our ALC to handle the directory discovery concept
            //
            // This is where Alc.Engine.dll is loaded into our custom ALC
            // and then passed through into PowerShell's ALC,
            // becoming the bridge between both
            return s_dependencyAlc.LoadFromAssemblyName(assemblyToResolve);
        }
    }
}
```

新しい実装で、モジュールが読み込まれて `Test-AlcModule` が実行されるときに発生する呼び出しのシーケンスを確認します。

![カスタム ALC を使用して依存関係を読み込む呼び出しのシーケンスの図](./media/resolving-dependency-conflicts/alc-sequence.png)

興味深い点は次のとおりです。

- モジュールで `Resolving` イベントが読み込まれて設定されると、`IModuleAssemblyInitializer` が最初に実行されます。
- `Test-AlcModule` が実行され、その `EndProcessing()` メソッドが呼び出されるまで、依存関係は読み込まれません。
- `EndProcessing()` が呼び出されると、既定の ALC では `AlcModule.Engine.dll` を検出できず、`Resolving` イベントが発生します。
- イベント ハンドラーによってカスタム ALC が既定の ALC にフックされ、`AlcModule.Engine.dll` のみが読み込まれます。
- `AlcModule.Engine.dll` 内で `AlcEngine.Use()` が呼び出されると、カスタム ALC が再び開始して `Shared.Dependency.dll` が解決されます。 具体的には、既定の ALC 内の何とも競合せず、`Dependencies` ディレクトリだけが検索されるので、"_この_" `Shared.Dependency.dll` が常に読み込まれます。

実装をアセンブルすると、新しいソース コード レイアウトは次のようになります。

```
+ AlcModule.psd1
+ src/
  + AlcModule.Cmdlets/
  | + AlcModule.Cmdlets.csproj
  | + TestAlcModuleCommand.cs
  | + AlcModuleAssemblyLoadContext.cs
  | + AlcModuleInitializer.cs
  |
  + AlcModule.Engine/
  | + AlcModule.Engine.csproj
  | + AlcEngine.cs
```

AlcModule.Cmdlets.csproj は次のようになります。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <ProjectReference Include="..\AlcModule.Engine\AlcModule.Engine.csproj" />
    <PackageReference Include="Microsoft.PowerShell.Sdk" Version="7.0.1" PrivateAssets="all" />
  </ItemGroup>
</Project>
```

AlcModule.Engine.csproj は次のようになります。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.1</TargetFramework>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="Shared.Dependency" Version="1.0.0" />
  </ItemGroup>
</Project>
```

したがって、モジュールをビルドするときの戦略は次のようになります。

- `AlcModule.Engine` をビルドします
- `AlcModule.Cmdlets` をビルドします
- `AlcModule.Engine` から `Dependencies` ディレクトリにすべてのものをコピーし、何をコピーしたかを憶えておきます
- `AlcModule.Engine` に存在しなかったすべてのものを、`AlcModule.Cmdlets` からベース モジュール ディレクトリにコピーします

ここでのモジュール レイアウトは依存関係の分離にとって非常に重要であるため、ソース ルートから使用するビルド スクリプトは次のようになります。

```powershell
param(
    # The .NET build configuration
    [ValidateSet('Debug', 'Release')]
    [string]
    $Configuration = 'Debug'
)

# Convenient reusable constants
$mod = "AlcModule"
$netcore = "netcoreapp3.1"
$copyExtensions = @('.dll', '.pdb')

# Source code locations
$src = "$PSScriptRoot/src"
$engineSrc = "$src/$mod.Engine"
$cmdletsSrc = "$src/$mod.Cmdlets"

# Generated output locations
$outDir = "$PSScriptRoot/out/$mod"
$outDeps = "$outDir/Dependencies"

# Build AlcModule.Engine
Push-Location $engineSrc
dotnet publish -c $Configuration
Pop-Location

# Build AlcModule.Cmdlets
Push-Location $cmdletsSrc
dotnet publish -c $Configuration
Pop-Location

# Ensure out directory exists and is clean
Remove-Item -Path $outDir -Recurse -ErrorAction Ignore
New-Item -Path $outDir -ItemType Directory
New-Item -Path $outDeps -ItemType Directory

# Copy manifest
Copy-Item -Path "$PSScriptRoot/$mod.psd1"

# Copy each Engine asset and remember it
$deps = [System.Collections.Generic.Hashtable[string]]::new()
Get-ChildItem -Path "$engineSrc/bin/$Configuration/$netcore/publish/" |
    Where-Object { $_.Extension -in $copyExtensions } |
    ForEach-Object { [void]$deps.Add($_.Name); Copy-Item -Path $_.FullName -Destination $outDeps }

# Now copy each Cmdlets asset, not taking any found in Engine
Get-ChildItem -Path "$cmdletsSrc/bin/$Configuration/$netcore/publish/" |
    Where-Object { -not $deps.Contains($_.Name) -and $_.Extension -in $copyExtensions } |
    ForEach-Object { Copy-Item -Path $_.FullName -Destination $outDir }
```

最後に、時間の経過と共に依存関係が追加されても堅牢な状態が維持されるアセンブリ読み込みコンテキストを使用して、モジュールの依存関係を分離する、一般的な方法があります。

詳細な例については、こちらの [GitHub リポジトリ](https://github.com/rjmholt/ModuleDependencyIsolationExample)を参照してください。 この例では、.NET Framework 内でモジュールを動作させたまま、ALC を使用するようにそのモジュールを移行する方法が示されています。 また、.NET Standard と PowerShell Standard を使用して、中核となる実装を簡略化する方法についても説明されています。

### <a name="assembly-resolve-handler-for-side-by-side-loading-with-assemblyloadfile"></a>`Assembly.LoadFile()` での side-by-side 読み込みのためのアセンブリ解決ハンドラー

side-by-side アセンブリ読み込みを実現するための、もう 1 つの、おそらくはさらに簡単な方法は、`AssemblyResolve` イベントを `Assembly.LoadFile()` にフックすることです。 `Assembly.LoadFile()` を使用すると、最も簡単に解決策を実装できるメリットがあり、.NET Core と .NET Framework の両方で動作します。

これを示すため、動的バインド リダイレクトの例と、上記のアセンブリ読み込みコンテキストの例のアイデアを組み合わせる実装の簡単な例を見てみましょう。

このモジュールは **LoadFileModule** という名前で、簡単なコマンド `Test-LoadFile [-Path] <path>` が含まれます。 このモジュールでは、`CsvHelper` アセンブリ (バージョン 15.0.5) の依存関係を取得します。

ALC モジュールと同様に、最初にモジュールを 2 つの部分に分割する必要があります。

1 つ目の部分 `LoadFileModule.Engine` では、実際の実装を行います。

```csharp
using CsvHelper;

namespace LoadFileModule.Engine
{
    public class Runner
    {
        public void Use(string path)
        {
            // Here's where we use the CsvHelper dependency
            using (var reader = new StreamReader(path))
            using (var csvReader = new CsvReader(reader, CultureInfo.InvariantCulture))
            {
                // Imagine we do something useful here...
            }
        }
    }
}
```

2 つ目の部分 `LoadFileModule.Cmdlets` は、PowerShell によって直接読み込まれるものです。 ALC モジュールと同様の戦略を使用して、依存関係を別のディレクトリに分離します。 ただし今回は、`LoadFileModule.Engine.dll` だけでなく、すべてのアセンブリを解決イベントと共に読み込みます。

```csharp
using LoadFileModule.Engine;

namespace LoadFileModule.Cmdlets
{
    // Actual cmdlet definition
    [Cmdlet(VerbsDiagnostic.Test, "LoadFile")]
    public class TestLoadFileCommand : Cmdlet
    {
        [Parameter(Mandatory = true)]
        public string Path { get; set; }

        protected override void EndProcessing()
        {
            // Here's where we use LoadFileModule.Engine
            new Runner().Use(Path);
        }
    }

    // This class controls our dependency resolution
    public class ModuleContextHandler : IModuleAssemblyInitializer, IModuleAssemblyCleanup
    {
        // We catalog our dependencies here to ensure we don't load anything else
        private static IReadOnlyDictionary<string, int> s_dependencies = new Dictionary<string, int>
        {
            { "CsvHelper", 15 },
        };

        // Set up the path to our dependency directory within the module
        private static string s_dependenciesDirPath = Path.GetFullPath(
            Path.Combine(
                Path.GetDirectoryName(Assembly.GetExecutingAssembly().Location),
                "Dependencies"));

        // This makes sure we only try to resolve dependencies when the module is loaded
        private static bool s_engineLoaded = false;

        public void OnImport()
          {
            // Set up our event when the module is loaded
            AppDomain.CurrentDomain.AssemblyResolve += HandleResolveEvent;
        }

        public void OnRemove(PSModuleInfo psModuleInfo)
        {
            // Unset the event when the module is unloaded
            AppDomain.CurrentDomain.AssemblyResolve -= HandleResolveEvent;
        }

        private static Assembly HandleResolveEvent(object sender, ResolveEventArgs args)
        {
            var asmName = new AssemblyName(args.Name);

            // First we want to know if this is the top-level assembly
            if (asmName.Name.Equals("LoadFileModule.Engine"))
            {
                s_engineLoaded = true;
                return Assembly.LoadFile(Path.Combine(s_dependenciesDirPath, "Unrelated.Engine.dll"));
            }

            // Otherwise, if that assembly has been loaded, we must try to resolve its dependencies too
            if (s_engineLoaded
                && s_dependencies.TryGetValue(asmName.Name, out int requiredMajorVersion)
                && asmName.Version.Major == requiredMajorVersion)
            {
                string asmPath = Path.Combine(s_dependenciesDirPath, $"{asmName.Name}.dll");
                return Assembly.LoadFile(asmPath);
            }

            return null;
        }
    }
}
```

最後に、モジュールのレイアウトも ALC モジュールと似ています。

```
LoadFileModule/
  + LoadFileModule.Cmdlets.dll
  + LoadFileModule.psd1
  + Dependencies/
  |  + LoadFileModule.Engine.dll
  |  + CsvHelper.dll
```

この構造を使用すると、**LoadFileModule** を、**CsvHelper** に依存関係が含まれる他のモジュールと共に読み込むことができるようになります。

このハンドラーは、アプリケーション ドメイン全体の **すべての** `AssemblyResolve` イベントに適用されるため、ここでは設計に関していくつか特定の選択を行う必要があります。

- このイベントによって他の読み込みが妨げられる可能性のある時間を短くするため、`LoadFileModule.Engine.dll` が読み込まれた後でのみ、一般的な依存関係の読み込みの処理を開始します。
- PowerShell ではなく `LoadFile()` の呼び出しによって読み込まれるように、`LoadFileModule.Engine.dll` を Dependencies ディレクトリに移動します。 これは、PowerShell で (たとえば) 別の `CsvHelper.dll` が読み込まれている場合でも、独自の依存関係の読み込みによって常に `AssemblyResolve` イベントが発生することを意味します。
  これにより、適切な依存関係を見つける機会ができます。
- モジュール用に特定の依存関係のみを解決する必要があるため、依存関係の名前とバージョンの辞書をハンドラーにコーディングする必要があります。 このような場合は、`ResolveEventArgs.RequestingAssembly` プロパティを使用して、`CsvHelper` がモジュールによって要求されているかどうかを調べることができます。 ただし、たとえば `CsvHelper` 自体によって `AssemblyResolve` イベントが発生した場合など、依存関係の依存関係に対してはこれは機能しません。 これを行うには、要求されたアセンブリと `Dependencies` ディレクトリ内のアセンブリのメタデータの比較など、さらに困難な他の方法があります。 ただし、この例では、問題を強調し、例を簡単にするため、このチェックをさらに明示的に行っています。

これは、`LoadFile()` 戦略と ALC 戦略の重要な相違点です。`AssemblyResolve` イベントではアプリケーション ドメイン内のすべての読み込みを処理する必要があるため、この依存関係の解決が他のモジュールによって混乱しないようにすることが非常に困難になります。 ただし、.NET Framework には ALC がないため、このオプションは理解しておく価値があります。

### <a name="custom-application-domains"></a>カスタム アプリケーション ドメイン

アセンブリを分離するための最後の最も極端なオプションは、カスタム アプリケーション ドメインを使用することです。
アプリケーション ドメインは、.NET Framework でのみ使用できます。 これらは、.NET アプリケーションのパーツをプロセス内で分離するために使用されます。 用途の 1 つは、同じプロセス内でアセンブリの読み込みを相互に分離することです。

ただし、アプリケーション ドメインはシリアル化の境界です。 あるアプリケーション ドメイン内のオブジェクトを、別のアプリケーション ドメイン内のオブジェクトから直接参照および使用することはできません。 この問題を回避するには、`MarshalByRefObject` を実装します。 ただし、依存関係でよくあるように、自分で型を制御できない場合は、ここで実装を強制することはできません。 唯一の解決策は、アーキテクチャを大幅に変更することです。 シリアル化の境界を使用すると、パフォーマンスにも大きな影響があります。

アプリケーション ドメインにはこのような重大な制限があり、実装が複雑であり、.NET Framework でしか機能しないため、ここでは使用方法の例を示しません。 可能性として触れておく価値はありますが、推奨されません。

カスタム アプリケーション ドメインを使用することに関心がある場合は、次のリンクが役に立つ可能性があります。

- [アプリケーション ドメインの概念に関するドキュメント](/dotnet/framework/app-domains/application-domains)
- [アプリケーション ドメインの使用例](/dotnet/framework/app-domains/use)

## <a name="solutions-for-dependency-conflicts-that-dont-work-for-powershell"></a>PowerShell では機能しない依存関係の競合の解決策

最後に、.NET で .NET 依存関係の競合を調べるときに有望そうに見えるいくつかの可能性について説明します。ただし、通常は PowerShell では機能しません。

これらの解決策には、アプリケーションとおそらくはコンピューター全体を制御できる環境での配置構成の変更という共通のテーマがあります。 これらの解決策は、サーバー環境に配置された Web サーバーや他のアプリケーションを対象とし、環境はアプリケーションをホストするためのもので、配置を行うユーザーが自由に構成できるというシナリオのためのものです。 また、これらはかなり .NET Framework 向けである傾向があり、PowerShell 6 以降では機能しません。

完全に制御できる Windows PowerShell 5.1 環境だけでモジュールを使用することがわかっている場合は、これらの一部を使用できる可能性があります。 ただし、一般に、**モジュールではこのようなグローバルなコンピューターの状態を変更しないでください**。 構成が壊れ、`powershell.exe`、他のモジュール、または他の依存アプリケーションで問題が発生し、モジュールが予期しない方法で失敗する可能性があります。

### <a name="static-binding-redirect-with-appconfig-to-force-using-the-same-dependency-version"></a>静的バインド リダイレクトと app.config を使用して、同じバージョンの依存関係の使用を強制する

.NET Framework アプリケーションでは、`app.config` ファイルを利用して、アプリケーションの動作の一部を宣言によって構成できます。 アセンブリの読み込みを特定のバージョンにリダイレクトするようにアセンブリのバインドを構成する `app.config` エントリを作成することができます。

PowerShell では、次の 2 つの問題があります。

- .NET Core では `app.config` がサポートされていないため、この解決策は `powershell.exe` にのみ適用されます。
- `powershell.exe` は、`System32` ディレクトリに存在する共有アプリケーションです。 多くのシステムでは、モジュールでその内容を変更できない可能性があります。 可能な場合でも、`app.config` を変更すると、既存の構成が壊れたり、他のモジュールの読み込みに影響したりする可能性があります。

### <a name="setting-codebase-with-appconfig"></a>app.config での `codebase` の設定

同じ理由から、`app.config` で `codebase` の設定を構成しようとしても、PowerShell モジュールでは機能しません。

### <a name="installing-dependencies-to-the-global-assembly-cache-gac"></a>グローバル アセンブリ キャッシュ (GAC) への依存関係のインストール

.NET Framework での依存関係のバージョンの競合を解決するもう 1 つの方法は、GAC に依存関係をインストールし、GAC から異なるバージョンを side-by-side で読み込めるようにすることです。

やはり、PowerShell モジュールの場合、ここでの主な問題は次のとおりです。

- GAC は .NET Framework にのみ適用されるため、PowerShell 6 以降では役に立ちません。
- GAC へのアセンブリのインストールは、コンピューターのグローバルな状態の変更であり、他のアプリケーションや他のモジュールで副作用が発生する可能性があります。 また、モジュールに必要なアクセス特権がある場合でも、正しく行うことは難しい可能性があります。 誤って行うと、他の .NET アプリケーションでコンピューター全体の深刻な問題が発生する可能性があります。

## <a name="further-reading"></a>関連項目

.NET アセンブリのバージョンの依存関係の競合については、多くの資料があります。 手始めに読むのに適したものをいくつか紹介します。

- [.NET: .NET のアセンブリ](/dotnet/standard/assembly/)
- [.NET Core: マネージド アセンブリの読み込みアルゴリズム](/dotnet/core/dependency-loading/loading-managed)
- [.NET Core: System.Runtime.Loader.AssemblyLoadContext について](/dotnet/core/dependency-loading/understanding-assemblyloadcontext)
- [.NET Core: side-by-side アセンブリ読み込みソリューションについての説明](https://github.com/dotnet/runtime/issues/13471)
- [.NET Framework: アセンブリ バージョンのリダイレクト](/dotnet/framework/configure-apps/redirect-assembly-versions)
- [.NET Framework: アセンブリの読み込みのベスト プラクティス](/dotnet/framework/deployment/best-practices-for-assembly-loading)
- [.NET Framework: ランタイムがアセンブリを検索する方法](/dotnet/framework/deployment/how-the-runtime-locates-assemblies)
- [.NET Framework: アセンブリ読み込みを解決する](/dotnet/standard/assembly/resolve-loads)
- [StackOverflow:アセンブリ バインドのリダイレクト、方法、理由](https://stackoverflow.com/questions/43365736/assembly-binding-redirect-how-and-why)
- [PowerShell: AssemblyLoadContexts の実装に関する説明](https://github.com/PowerShell/PowerShell/issues/11571)
- [PowerShell: `Assembly.LoadFile()` で既定の AssemblyLoadContext に読み込まれない](https://github.com/PowerShell/PowerShell/issues/12052)
- [Rick Strahl: .NET アセンブリの依存関係はいつ読み込まれるか](https://weblog.west-wind.com/posts/2012/Nov/03/Back-to-Basics-When-does-a-NET-Assembly-Dependency-get-loaded)
- [Jon Skeet: .NET でのバージョン管理の概要](https://codeblog.jonskeet.uk/2019/06/30/versioning-limitations-in-net/)
- [Nate McMaster: .NET Core プリミティブの詳細](https://natemcmaster.com/blog/2017/12/21/netcore-primitives/)
