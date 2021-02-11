---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: クイック スタート - DSC を使用して Web サイトを作成する
description: このクイック スタートでは、DSC 構成を使用して新しい Web サイトを作成する方法を示します。
ms.openlocfilehash: ece1ae964bce00a4102de4b13d99d6ee1259117a
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92650899"
---
# <a name="quickstart---create-a-website-with-desired-state-configuration-dsc"></a>クイックスタート - Desired State Configuration (DSC) を使用して Web サイトを作成する

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

この演習では、Desired State Configuration (DSC) の構成の作成と適用について最初から最後まで説明します。 これから使用するサンプルでは、サーバーの `Web-Server` (IIS) 機能が有効になっており、そのサーバのディレクトリ `inetpub\wwwroot` に「Hello World」サンプルのコンテンツが存在することを確認します。

DSC の概要としくみついては、「[意思決定者向け Desired State Configuration の概要](../overview/decisionMaker.md)」をご覧ください。

## <a name="requirements"></a>必要条件

このサンプルを実行するには、Windows Server 2012 以降と PowerShell 4.0 以降が動作しているコンピューターが必要です。

## <a name="write-and-place-the-indexhtm-file"></a>index.htm ファイルを記述し、配置する

最初に、Web サイトのコンテンツとして使用する HTML ファイルを作成します。

ルート フォルダーに `test` という名前のフォルダを作成します。

テキスト エディターで、次のテキストを入力します。

```html
<head></head>
<body>
<p>Hello World!</p>
</body>
```

これを先ほど作成した `test` フォルダに `index.htm` というファイル名でこのファイルを保存します。

## <a name="write-the-configuration"></a>構成を記述する

[DSC 構成](../configurations/configurations.md)は 1 つまたは複数のターゲット コンピューター (ノード) を構成する方法を定義する特別な PowerShell 関数です。

PowerShell ISE で、次のように入力します。

```powershell
Configuration WebsiteTest {

    # Import the module that contains the resources we're using.
    Import-DscResource -ModuleName PsDesiredStateConfiguration

    # The Node statement specifies which targets this configuration will be applied to.
    Node 'localhost' {

        # The first resource block ensures that the Web-Server (IIS) feature is enabled.
        WindowsFeature WebServer {
            Ensure = "Present"
            Name   = "Web-Server"
        }

        # The second resource block ensures that the website content copied to the website root folder.
        File WebsiteContent {
            Ensure = 'Present'
            SourcePath = 'c:\test\index.htm'
            DestinationPath = 'c:\inetpub\wwwroot'
        }
    }
}
```

このファイルを `WebsiteTest.ps1` として保存します。

関数名の前に **Configuration** というキーワードを追加することで、PowerShell の関数のように表示されます。

**Node** ブロックでは、構成するターゲット ノードを指定します。 例では、 `localhost`が使用されます。

構成は 2 つの [リソース](../resources/resources.md) ( **WindowsFeature** と **ファイル** ) を呼び出します。
リソースは、ターゲット ノードが構成によって定義された状態になっているか確認します。

## <a name="compile-the-configuration"></a>構成のコンパイル

DSC 構成をノードに適用するには、最初にコンパイルを行い、MOF ファイルを出力する必要があります。 これを行うには、構成を関数のように実行します。 PowerShell コンソールで構成が保存されているフォルダーに移動し、次のコマンドを実行して構成のコンパイルを行い、MOF ファイルを出力します。

```powershell
. .\WebsiteTest.ps1
WebsiteTest
```

これにより、次の出力が生成されます。

```
Directory: C:\ConfigurationTest\WebsiteTest


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        3/13/2017   5:20 PM           2746 localhost.mof
```

最初の行では、コンソールで利用可能な構成関数を作成します。 2 行目では構成を実行します。 その結果、現在のフォルダのサブ フォルダとして `WebsiteTest` という名前の新しいフォルダが作成されます。 `WebsiteTest` フォルダーには、`localhost.mof` という名前のファイルが含まれています。 これは、ターゲット ノードに適用できるファイルです。

## <a name="apply-the-configuration"></a>構成を適用する

コンパイル済みの MOF があるため、[Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration) コマンドレットを呼び出すことで、ターゲット ノード (今回の場合はローカル コンピューター) に構成を適用できます。

`Start-DscConfiguration` コマンドレットから DSC のエンジンである [Local Configuration Manager (LCM)](../managing-nodes/metaConfig.md) に対して構成の適用を指示します。 LCM はDSC リソースを呼び出し、構成を適用します。

> [!NOTE]
> DSC が実行できるようにするには、`localhost` の構成を実行している場合でも PowerShell のリモート コマンドを受信するように、Windows を構成する必要があります。 環境を簡単に正しく構成するには、管理者特権の PowerShell ターミナルで `Set-WsManQuickConfig -Force` を実行するだけです。

PowerShell コンソールで構成が保存されているフォルダーに移動し、次のコマンドを実行します。

```powershell
Start-DscConfiguration .\WebsiteTest
```

## <a name="test-the-configuration"></a>構成をテストする

[Get-DscConfigurationStatus](/powershell/module/psdesiredstateconfiguration/get-dscconfigurationstatus) コマンドレットを呼び出すことで、構成の適用が正しく行われたか確認できます。

今回の場合は、Web ブラウザーで `http://localhost/` を参照することで、結果を直接テストすることもできます。 この例の最初の手順として、作成した「Hello World」 HTML ページが表示されます。

## <a name="next-steps"></a>次のステップ

- 「[DSC 構成](../configurations/configurations.md)」で DSC 構成の詳細を確認する。
- 「[DSC リソース](../resources/resources.md)」で利用可能な DSC リソースとカスタム DSC リソースの作成方法を確認する。
- 「[PowerShell ギャラリー](https://www.powershellgallery.com/)」で DSC の構成とリソースを検索する。
