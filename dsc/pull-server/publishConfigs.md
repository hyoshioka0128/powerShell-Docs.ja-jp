---
ms.date: 12/12/2018
keywords: DSC, PowerShell, 構成, セットアップ
title: 構成 ID (v4/v5) を使用してプル サーバーに発行する
ms.openlocfilehash: 0144fec43d7a8d65b79891567cc0dc3952175343
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62079508"
---
# <a name="publish-to-a-pull-server-using-configuration-ids-v4v5"></a>構成 ID (v4/v5) を使用してプル サーバーに発行する

以下のセクションでは、プル サーバーを既にセットアップしてあるものとします。 プル サーバーをセットアップしていない場合は、次のガイドを使用できます。

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)

各ターゲット ノードは、構成やリソースをダウンロードし、さらにその状態を報告するように構成できます。 この記事では、ダウンロードできるようにリソースをアップロードする方法、およびリソースを自動的にダウンロードするようにクライアントを構成する方法を示します。 ノードは、割り当てられた構成を**プル**または**プッシュ** (v5) によって受け取ると、構成で必要なすべてのリソースを LCM で指定された場所から自動的にダウンロードします。

## <a name="compile-configurations"></a>構成をコンパイルする

[構成](../configurations/configurations.md)をプル サーバーに格納するための最初のステップは、構成を ".mof" ファイルにコンパイルすることです。 構成を汎用にし、より多くのクライアントに適用できるようにするため、Node ブロックでは `localhost` を使用します。 次の例では、特定のクライアント名ではなく `localhost` を使用する構成シェルを示します。

```powershell
Configuration GenericConfig
{
    Node localhost
    {

    }
}
GenericConfig
```

汎用の構成をコンパイルした後は、"localhost.mof" ファイルが必要です。

## <a name="renaming-the-mof-file"></a>MOF ファイルの名前を変更する

構成の ".mof" ファイルは、**ConfigurationName** または **ConfigurationID** でプル サーバーに格納できます。 プル クライアントのセットアップ方法に応じて、以下のセクションを選択し、コンパイル済みの ".mof" ファイルの名前を適切に変更できます。

### <a name="configuration-ids-guid"></a>構成 ID (GUID)

"localhost.mof" ファイルの名前を、"<GUID>.mof" ファイルに変更する必要があります。 以下の例を使用するか、または [New-Guid](/powershell/module/microsoft.powershell.utility/new-guid) コマンドレットを使用して、ランダムな **Guid** を作成できます。

```powershell
[System.Guid]::NewGuid()
```

サンプル出力

```output
Guid
----
64856475-939e-41fb-aba5-4469f4006059
```

その後、許される任意の方法を使って、".mof" ファイルの名前を変更することができます。 次の例では、[Rename-Item](/powershell/module/microsoft.powershell.management/rename-item) コマンドレットを使っています。

```powershell
Rename-Item -Path .\localhost.mof -NewName '64856475-939e-41fb-aba5-4469f4006059.mof'
```

環境で **Guid** を使用する詳細については、[Guid の計画](/powershell/dsc/secureserver#guids)に関する項を参照してください。

### <a name="configuration-names"></a>構成名

"localhost.mof" ファイルの名前を、"<Configuration Name>.mof" ファイルに変更する必要があります。 次の例では、前のセクションの構成名を使っています。 その後、許される任意の方法を使って、".mof" ファイルの名前を変更することができます。 次の例では、[Rename-Item](/powershell/module/microsoft.powershell.management/rename-item) コマンドレットを使っています。

```powershell
Rename-Item -Path .\localhost.mof -NewName 'GenericConfig.mof'
```

## <a name="create-the-checksum"></a>チェックサムを作成する

プル サーバーまたは SMB 共有に格納される各 ".mof" ファイルには、".checksum" ファイルを関連付ける必要があります。 このファイルにより、クライアントでは、関連付けられている ".mof" ファイルが変更されて再度ダウンロードする必要があることを認識できます。

**チェックサム**は [New-DSCCheckSum](/powershell/module/psdesiredstateconfiguration/new-dscchecksum) コマンドレットで作成できます。 また、`-Path` パラメーターを使用すると、ファイルのディレクトリに対して `New-DSCCheckSum` を実行することもできます。 チェックサムが既に存在する場合は、`-Force` パラメーターを使用して強制的に再作成できます。 次の例では、前のセクションの ".mof" ファイルが含まれるディレクトリを指定し、`-Force` パラメーターを使用しています。

```powershell
New-DscChecksum -Path '.\' -Force
```

出力は示されませんが、"<GUID or Configuration Name>.mof.checksum" ファイルが表示されるようになるはずです。

## <a name="where-to-store-mof-files-and-checksums"></a>MOF ファイルとチェックサムを格納する場所

### <a name="on-a-dsc-http-pull-server"></a>DSC HTTP プル サーバー上

HTTP プル サーバーをセットアップするときは、「[DSC HTTP プル サーバーを設定する](pullServer.md)」で説明されているように、**ModulePath** キーと **ConfigurationPath** キーに対するディレクトリを指定します。 **ConfigurationPath** キーは、".mof" ファイルを格納する必要がある場所を示します。 **ConfigurationPath** は、".mof" ファイルと ".checksum" ファイルを格納する必要がある場所を示します。

```powershell
    xDscWebService PSDSCPullServer
    {
    ...
        ModulePath              = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Modules"
        ConfigurationPath       = "$env:PROGRAMFILES\WindowsPowerShell\DscService\Configuration"
    ...
    }

```

### <a name="on-an-smb-share"></a>SMB 共有上

SMB 共有を使うようプル クライアントを設定するときは、**ConfigurationRepositoryShare** を指定します。 すべての ".mof" ファイルと ".checksum" ファイルを、**ConfigurationRepositoryShare** ブロックの **SourcePath** ディレクトリに格納する必要があります。

```powershell
ConfigurationRepositoryShare SMBPullServer
{
    SourcePath = '\\SMBPullServer\Pull'
}
```

## <a name="next-steps"></a>次の手順

次に、指定した構成をプルするようにプル クライアントを構成します。 詳しくは、次のいずれかのガイドをご覧ください。

- [構成 ID を使用してプル クライアントを設定する (v4)](pullClientConfigId4.md)
- [構成 ID を使用してプル クライアントを設定する (v5)](pullClientConfigId.md)
- [構成名を使用してプル クライアントを設定する (v5)](pullClientConfigNames.md)

## <a name="see-also"></a>関連項目

- [DSC SMB プル サーバーを設定する](pullServerSmb.md)
- [DSC HTTP プル サーバーを設定する](pullServer.md)
- [リソースをパッケージ化してプル サーバーにアップロードする](package-upload-resources.md)
