---
ms.date: 06/22/2020
keywords: dsc, powershell, 構成, サービス, セットアップ
title: 構成の作成、コンパイル、適用
description: この演習では、DSC の構成の作成と適用について最初から最後まで説明します。 次の例では、非常に単純な構成を作成して適用する方法について学習します
ms.openlocfilehash: f173fe0dc6cd73e2b49bb8c44a9ee1a53eab475f
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92645025"
---
# <a name="write-compile-and-apply-a-configuration"></a>構成の作成、コンパイル、適用

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

この演習では、Desired State Configuration (DSC) の構成の作成と適用について最初から最後まで説明します。 次の例では、非常に単純な構成を作成して適用する方法について学習します。 この構成により、"HelloWorld.txt" ファイルがローカル コンピューターに存在していることが確保されます。
ファイルを削除する場合は、次に更新されるときに DSC によってファイルが再作成されます。

DSC の概要としくみついては、[開発者向けの Desired State Configuration の概要](../overview/overview.md)に関するページをご覧ください。

## <a name="requirements"></a>必要条件

このサンプルを実行するには、PowerShell 4.0 以降が動作しているコンピューターが必要です。

## <a name="write-the-configuration"></a>構成を記述する

DSC [構成](configurations.md)は、1 つ以上のターゲット コンピューター (ノード) を構成する方法を定義する特別な PowerShell 関数です。

PowerShell ISE、またはその他の PowerShell エディターで、次のように入力します。

```powershell
Configuration HelloWorld {

    # Import the module that contains the File resource.
    Import-DscResource -ModuleName PsDesiredStateConfiguration

    # The Node statement specifies which targets to compile MOF files for, when
    # this configuration is executed.
    Node 'localhost' {

        # The File resource can ensure the state of files, or copy them from a
        # source to a destination with persistent updates.
        File HelloWorld {
            DestinationPath = "C:\Temp\HelloWorld.txt"
            Ensure = "Present"
            Contents   = "Hello World from DSC!"
        }
    }
}
```

> [!IMPORTANT]
> 多くの DSC リソースを同じ構成で操作できるように複数のモジュールをインポートすることが必要な、より高度なシナリオの場合は、`Import-DscResource` を使って各モジュールを別々の行に配置するようにしてください。 こちらの方がソース管理で管理しやすく、また Azure State Configuration の DSC を使用する場合に必要になります。
>
> ```powershell
>  Configuration HelloWorld {
>
>   # Import the module that contains the File resource.
>   Import-DscResource -ModuleName PsDesiredStateConfiguration
>   Import-DscResource -ModuleName xWebAdministration
>
> ```

ファイルを "HelloWorld.ps1" という名前で保存します。

構成を定義するのは、関数の定義と似ています。 `localhost` 場合、 **ノード** ブロックは構成するターゲット ノードを指定します。

構成では、1 つの[リソース](../resources/resources.md) (`File` リソース) を呼び出します。 リソースは、ターゲット ノードが構成によって定義された状態になっているか確認します。

## <a name="compile-the-configuration"></a>構成のコンパイル

DSC 構成をノードに適用するには、最初にコンパイルを行い、MOF ファイルを出力する必要があります。 関数のように構成を実行すると、`Node` ブロックで定義されたノードごとに、`.mof` ファイルが 1 つコンパイルされます。 構成を実行するには、現在の範囲で `HelloWorld.ps1` スクリプトを " _ドット ソース_ " で実行する必要があります。 詳細については、[スクリプト](/powershell/module/microsoft.powershell.core/about/about_scripts#script-scope-and-dot-sourcing)に関するページを参照してください。

<!-- markdownlint-disable MD038 -->
`HelloWorld.ps1` スクリプトを " _ドット ソース_ " で実行するには、`. ` (ドット、スペース) の後で、それを保存した場所のパス内に入力します。 その後、関数のように呼び出すことで、構成を実行することができます。 また、スクリプトの一番下にある構成関数を呼び出すと、ドット ソースで実行する必要がなくなります。
<!-- markdownlint-enable MD038 -->

```powershell
. C:\Scripts\HelloWorld.ps1
HelloWorld
```

これにより、次の出力が生成されます。

```Output
Directory: C:\Scripts\HelloWorld


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        3/13/2017   5:20 PM           2746 localhost.mof
```

## <a name="apply-the-configuration"></a>構成を適用する

コンパイル済みの MOF があるため、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを呼び出すことで、ターゲット ノード (今回の場合はローカル コンピューター) に構成を適用できます。

`Start-DscConfiguration` コマンドレットから[ローカル構成マネージャー (LCM)](../managing-nodes/metaConfig.md) (DSC のエンジン) に対して構成の適用を指示します。 LCM はDSC リソースを呼び出し、構成を適用します。

以下のコードを使用して、`Start-DSCConfiguration` コマンドレットを実行します。 `localhost.mof` が保存されているディレクトリ パスを **Path** パラメーターに指定します。 `Start-DSCConfiguration` コマンドレットは、任意の `<computername>.mof` ファイルに対して指定されたディレクトリが対象になります。 `Start-DSCConfiguration` コマンドレットでは、見つかった各 `.mof` ファイルを、ファイル名 ("localhost"、"server01"、"dc-02" など) で指定された `computername` に適用する試みが行われます。

> [!NOTE]
> `-Wait` パラメーターが指定されていない場合、`Start-DSCConfiguration` では操作を実行するためにバックグラウンド ジョブが作成されます。 `-Verbose` パラメーターを指定すると、操作の **詳細** 出力を確認できます。 `-Wait` および `-Verbose` は、どちらも省略可能なパラメーターです。

```powershell
Start-DscConfiguration -Path C:\Scripts\HelloWorld -Verbose -Wait
```

## <a name="test-the-configuration"></a>構成をテストする

`Start-DSCConfiguration` コマンドレットが完了すると、指定した場所に `HelloWorld.txt` ファイルが表示されます。 [Get-Content](/powershell/module/microsoft.powershell.management/get-content) コマンドレットを使用して、コンテンツを確認できます。

[Test-DSCConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration) を使用して、現在の状態を _テスト_ することもできます。

現在、ノードが適用された構成に準拠している場合、出力は `True` になります。

```powershell
Test-DSCConfiguration
```

```Output
True
```

```powershell
Get-Content -Path C:\Temp\HelloWorld.txt
```

```Output
Hello World from DSC!
```

## <a name="re-applying-the-configuration"></a>構成を再適用する

もう一度構成が適用されることを確認するには、自分の構成で作成されたテキスト ファイルを削除します。 `Start-DSCConfiguration` コマンドレットを `-UseExisting` パラメーターを指定して使用します。 `-UseExisting` パラメーターによって、`Start-DSCConfiguration` に対して、最後に正常に適用された構成を表す、"current.mof" を再適用するように指示されます。

```powershell
Remove-Item -Path C:\Temp\HelloWorld.txt
```

## <a name="next-steps"></a>次のステップ

- 「[DSC 構成](configurations.md)」で DSC 構成の詳細を確認する。
- 「[DSC リソース](../resources/resources.md)」で利用可能な DSC リソースとカスタム DSC リソースの作成方法を確認する。
- 「[PowerShell ギャラリー](https://www.powershellgallery.com/)」で DSC の構成とリソースを検索する。
