---
description: PowerShell Desired State Configuration (DSC) 機能について簡単に説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 07/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_desiredstateconfiguration?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_DesiredStateConfiguration
ms.openlocfilehash: 5d088934ffc953ad19be401bce72f6287f0fde07
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94387024"
---
# <a name="about_desiredstateconfiguration"></a>about_DesiredStateConfiguration

## <a name="short-description"></a>概要

PowerShell Desired State Configuration (DSC) 機能について簡単に説明します。

## <a name="long-description"></a>詳細説明

DSC は、ソフトウェアサービスの構成データを展開および管理し、これらのサービスが実行される環境を管理できるようにする、PowerShell の管理プラットフォームです。

DSC には、ソフトウェア環境の状態をどのように構成するかを宣言によって指定するために使用できる PowerShell 言語拡張機能、新しいコマンドレット、およびリソースのセットが用意されています。 また、既存の構成を保守し、管理するための手段も提供します。

DSC は、PowerShell 4.0 で導入されました。

DSC の詳細については、「 [PowerShell Desired State Configuration の概要](/powershell/scripting/dsc/overview/overview)」を参照してください。

## <a name="developing-dsc-resources-with-classes"></a>クラスを使用した DSC リソースの開発

PowerShell 5.0 以降では、クラスを使用して DSC リソースを開発できます。
詳細については、「 [about_Classes](about_Classes.md)」と「 [PowerShell クラスを使用したカスタム DSC リソースの作成](/powershell/scripting/dsc/resources/authoringresourceclass)」を参照してください。

## <a name="using-dsc"></a>DSC の使用

DSC を使用して環境を構成するには、まず、Configuration キーワードを使用して PowerShell スクリプトブロックを定義し、次に識別子を指定します。その後に、ブロックを区切る中かっこのペアを続けます。 構成ブロック内では、環境内の各ノード (コンピューター) に必要な構成状態を指定するノードブロックを定義できます。 ノードブロックは、Node キーワードで始まり、その後にターゲットコンピューターの名前が続きます。これには変数を指定できます。 コンピューター名の後に、ノードブロックを区切る中かっこを指定します。 ノードブロック内では、リソースブロックを定義して特定のリソースを構成できます。 リソースブロックは、次の例に示すように、リソースの型名と、そのブロックに指定する識別子、ブロックを区切る中かっこを続けて開始されます。

```powershell
Configuration MyWebConfig {
    # Parameters are optional
    param ($MachineName, $WebsiteFilePath)
    # A Configuration block can have one or more Node blocks
    Node $MachineName
    {
        # Next, specify one or more resource blocks
        # WindowsFeature is one of the resources you can use in a Node block
        # This example ensures the Web Server (IIS) role is installed
        WindowsFeature IIS
        {
            # To ensure that the role is not installed, set Ensure to "Absent"
            Ensure = "Present"
            Name = "Web-Server" # Use the Name property from Get-WindowsFeature
        }

        # You can use the File resource to create files and folders
        # "WebDirectory" is the name you want to use to refer to this instance
        File WebDirectory
        {
            Ensure = "Present"  # You can also set Ensure to "Absent"
            Type = "Directory" # Default is "File"
            Recurse = $true
            SourcePath = $WebsiteFilePath
            DestinationPath = "C:\inetpub\wwwroot"

            # Ensure that the IIS block is successfully run first before
            # configuring this resource
            DependsOn = "[WindowsFeature]IIS"  # Use for dependencies
        }
    }
}
```

構成を作成するには、PowerShell 関数を呼び出すのと同じ方法で構成ブロックを呼び出し、定義した可能性のあるパラメーター (上記の例では2つ) を渡します。 たとえば、この場合は次のようになります。

```powershell
MyWebConfig -MachineName "TestMachine" -WebsiteFilePath `
  "\\filesrv\WebFiles" -OutputPath "C:\Windows\system32\temp"
# OutputPath is optional
```

これにより、指定したパスにあるノードごとに MOF ファイルが生成されます。 これらの MOF ファイルは、各ノードに必要な構成を指定します。 次に、次のコマンドレットを使用して、構成 MOF ファイルを解析し、各ノードに対応する構成を送信し、これらの構成を適用します。 クラスベースの DSC リソース用に個別の MOF ファイルを作成する必要はないことに注意してください。

```powershell
Start-DscConfiguration -Verbose -Wait -Path "C:\Windows\system32\temp"
```

## <a name="using-dsc-to-maintain-configuration-state"></a>DSC を使用して構成の状態を維持する

DSC では、構成はべき等です。 これは、DSC を使用して同じ構成を複数回実行する場合、結果として得られる構成の状態は常に同じであることを意味します。 このため、環境内のいずれかのノードが望ましい状態からドリフトを持つ可能性があると思われる場合は、同じ DSC 構成を再度実行して、目的の状態に戻すことができます。 状態がドリフトであるリソースだけを対象とするように構成スクリプトを変更する必要はありません。

次の例では、特定のノードでの構成の実際の状態が、ノードで有効になっている最後の DSC 構成からドリフトされているかどうかを確認する方法を示します。 この例では、ローカルコンピューターの構成を確認しています。

```powershell
$session = New-CimSession -ComputerName "localhost"
Test-DscConfiguration -CimSession $session
```

## <a name="built-in-dsc-resources"></a>組み込みの DSC リソース

構成スクリプトには、次の組み込みリソースを使用できます。

|名前                  |プロパティ                                         |
|----------------------|---------------------------------------------------|
|ファイル                  |{DestinationPath, 属性, チェックサム, コンテンツ...}|
|アーカイブ               |{Destination、Path、Checksum、Credential...}       |
|環境           |{Name, DependsOn, Path...}                 |
|グループ                 |{GroupName、Credential、DependsOn、Description...} |
|ログ                   |{Message, DependsOn, PsDscRunAsCredential}         |
|パッケージ               |{Name, Path, ProductId, Arguments...}              |
|レジストリ              |{Key, ValueName, DependsOn, 確認...}             |
|スクリプト                |{GetScript、SetScript、TestScript、Credential...}  |
|サービス               |{Name, BuiltInAccount, Credential, Dependencies...}|
|User                  |{UserName, DependsOn, Description, Disabled...}    |
|WaitForAll            |{NodeName, DependsOn, PsDscRunAsC...}|
|WaitForAny            |{NodeName, DependsOn, PsDscRunAsC...}|
|WaitForSome           |{NodeCount, NodeName,, DependsOn...}  |
|WindowsFeature        |{Name, Credential, DependsOn, 確認...}           |
|WindowsOptionalFeature|{Name, DependsOn, LogLevel...}             |
|WindowsProcess        |{Arguments, Path, Credential, DependsOn...}        |

システムで使用可能な DSC リソースの一覧を取得するには、コマンドレットを実行し `Get-DscResource` ます。

> [!NOTE]
> PowerShell バージョン 7.0 より前のバージョンでは、`Get-DscResource` によるクラス ベースの DSC リソースの検出は行われません。

このトピックの例では、File リソースと Add-windowsfeature リソースの使用方法を示します。 リソースで使用できるすべてのプロパティを表示するには、PowerShell ISE の構成スクリプト内のリソースキーワード (ファイルなど) にカーソルを挿入し、 <kbd>CTRL</kbd> + <kbd>space</kbd>キーを押したままにします。

## <a name="find-more-resources"></a>その他のリソースを検索する

PowerShell と DSC ユーザーコミュニティ、および Microsoft によって作成されたその他の利用可能な DSC リソースをダウンロードしてインストールし、学習することができます。 [PowerShell ギャラリー](https://www.powershellgallery.com/)にアクセスして、利用可能な DSC リソースを参照し、詳細を確認してください。

## <a name="see-also"></a>関連項目

[PowerShell Desired State Configuration の概要](/powershell/scripting/dsc/overview/overview)

[PowerShell Desired State Configuration の組み込みリソース](/powershell/scripting/dsc/resources/resources)

[カスタム PowerShell Desired State Configuration リソースのビルド](/powershell/scripting/dsc/resources/authoringResource)
