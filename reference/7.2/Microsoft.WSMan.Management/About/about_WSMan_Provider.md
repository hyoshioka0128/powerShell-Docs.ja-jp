---
description: WSMan
Locale: en-US
ms.date: 10/18/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/about/about_wsman_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: WSMan プロバイダー
ms.openlocfilehash: 32baaaec3589fc9d6f4c2319f47d56bca777f738
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600807"
---
# <a name="wsman-provider"></a>WSMan プロバイダー

## <a name="provider-name"></a>プロバイダー名

WSMan

## <a name="drives"></a>ドライブ

`WSMan:`

## <a name="short-description"></a>簡単な説明

Web Services for Management (WS-Management) 構成情報へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

**WSMan** Provider for PowerShell を使用すると、ローカルコンピューターまたはリモートコンピューター上の WS-Management 構成データを追加、変更、消去、削除できます。

**WSMan** プロバイダーは、WS-Management 構成設定の論理グループに対応するディレクトリ構造を持つ PowerShell ドライブを公開します。 これらのグループ化はコンテナーと呼ばれています。

Windows PowerShell 3.0 以降では、 **WSMan** プロバイダーが更新され、 **OutputBufferingMode** などのセッション構成の新しいプロパティをサポートするようになりました。 セッション構成はドライブのプラグインディレクトリに項目として表示され、 `WSMan:` プロパティは各セッション構成の項目として表示されます。

**WSMan** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [Get-ChildItem](xref:Microsoft.PowerShell.Management.Get-ChildItem)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)

> [!NOTE]
> ドライブのコマンドを使用して、 `WSMan:` 新しいプロパティの値を変更できます。 ただし、 `WSMan:` powershell 2.0 のドライブを使用して、Windows powershell 3.0 で導入されたプロパティを変更することはできません。
> エラーは生成されませんが、これらの設定を変更するためのコマンドは有効ではありません。 Windows PowerShell 3.0 で **WSMan** ドライブを使用します。

### <a name="organization-of-the-wsman-drive"></a>WSMan: ドライブの構成

- **クライアント**: WS-Management クライアントのさまざまな面を構成できます。 構成情報はレジストリに保存されます。

- **サービス**: WS-Management サービスのさまざまな側面を構成できます。
  構成情報はレジストリに保存されます。
  > [!NOTE]
  > サービス構成はサーバー構成と呼ばれることもあります。

- **シェル**: リモートシェルアクセスを許可する設定 (**AllowRemoteShellAccess**) や、許可される同時ユーザーの最大数 (**MaxConcurrentUsers**) など、WS-Management シェルのさまざまな側面を構成できます。

- **リスナー**: リスナーを作成して構成できます。 リスナーは WS-Management プロトコルを実装し、メッセージを送受信する管理サービスです。

- **プラグイン**: プラグインが読み込まれ、さまざまな機能を提供するために WS-Management サービスによって使用されます。 既定では、PowerShell には次の3つのプラグインが用意されています。
  - イベント転送プラグイン。
  - Microsoft PowerShell プラグイン。
  - Windows Management Instrumentation (WMI) プロバイダープラグイン。
  この3つのプラグインは、イベントの転送、構成、および WMI アクセスをサポートしています。

- **ClientCertificate**: クライアント証明書を作成して構成することができます。
  証明書認証を使用するように WS-Management クライアントが構成されているとき、クライアント証明書が使用されます。

### <a name="directory-hierarchy-of-the-wsman-provider"></a>WSMan プロバイダーのディレクトリ階層

ローカルコンピューターの WSMan プロバイダーのディレクトリ階層は次のとおりです。

```
WSMan:\localhost
--- Client
--- Service
--- Shell
--- Listener
------ <Specific_Listener>
--- Plugin
------ Event Forwarding Plugin
--------- InitializationParameters
--------- Resources
------------ Security
------ Microsoft.Powershell
--------- InitializationParameters
--------- Resources
------------ Security
------ WMI Provider
--------- InitializationParameters
--------- Resources
------------ Security
--- ClientCertificate
```

リモート コンピューターの WSMan プロバイダーのディレクトリ階層はローカル コンピューターと同じです。 ただし、リモート コンピューターの構成設定にアクセスするには、[Connect-WSMan](xref:Microsoft.WSMan.Management.Connect-WSMan) を利用し、リモート コンピューターに接続する必要があります。 リモート コンピューターに接続すると、リモート コンピューターの名前がプロバイダーに表示されます。

```
WSMan:\<Remote_Computer_Name>
```

## <a name="navigating-the-wsman-drive"></a>WSMan: ドライブの移動

このコマンドは、コマンドレットを使用して、 `Set-Location` 現在の場所をドライブに変更し `WSMan:` ます。

```powershell
Set-Location WSMan:
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、「」と入力します。

```powershell
Set-Location C:
```

### <a name="navigating-to-a-remote-system-store-location"></a>リモートシステムストアの場所への移動

このコマンドは、コマンドを使用して、 `Set-Location` 現在の場所をリモートシステムストアの場所のルートの場所に変更します。 `\` `/` ドライブのレベルを示すには、円記号またはスラッシュを使用し `WSMan:` ます。

```powershell
Set-Location -Path  WSMan:\SERVER01
```

> [!NOTE]
> 上記のコマンドでは、リモート システムにすでに接続しているものと仮定しています。

## <a name="displaying-the-contents-of-the-wsman-drive"></a>WSMan: ドライブの内容を表示しています

このコマンドは、 `Get-Childitem` コマンドレットを使用して、Localhost ストアの場所に WS-Management ストアを表示します。

```powershell
Get-ChildItem -path WSMan:\Localhost
```

ドライブにいる場合は、 `WSMan:` ドライブ名を省略できます。

このコマンドは、 `Get-Childitem` コマンドレットを使用して、リモートコンピューター "SERVER01" ストアの場所に WS-Management ストアを表示します。

```powershell
Get-ChildItem -path WSMan:\SERVER01
```

> [!NOTE]
> 上記のコマンドでは、リモート システムにすでに接続しているものと仮定しています。

## <a name="setting-the-value-of-items-in-the--wsman-drive"></a>WSMAN: ドライブ内の項目の値を設定する

`Set-Item`コマンドレットを使用して、ドライブの構成設定を変更でき `WSMAN` ます。 次の例では、"contoso.com" というサフィックスが付いたすべてのホストを受け入れるように **TrustedHosts** 値を設定します。

```powershell
# You do not need to specify the -Path parameter name when using Set-Item.
PS WSMAN:\localhost\Client> Set-Item .\TrustedHosts -Value "*.contoso.com"
```

コマンドレットでは、 `Set-Item` `-Concatenate` 値を変更する代わりに値を追加する追加のパラメーターがサポートされています。 次の例では、新しい値 "*. domain2.com" を、に格納されている古い値に追加します。 `TrustedHost:`

```powershell
Set-Item WSMAN:\localhost\Client\TrustedHosts *.domain2.com -Concatenate
```

## <a name="creating-items-in-the-wsman-drive"></a>WSMAN: ドライブに項目を作成する

### <a name="creating-a-new-listener"></a>新しいリスナーの作成

コマンドレットでは、 `New-Item` プロバイダードライブ内に項目を作成します。 各プロバイダーには、さまざまな種類のアイテムを作成できます。 ドライブでは `WSMAN:` 、リモート要求を受信して応答するように構成する *リスナー* を作成できます。 次のコマンドは、コマンドレットを使用して新しい HTTP リスナーを作成し `New-Item` ます。

```powershell
New-Item -Path WSMan:\localhost\Listener -Address * -Transport HTTP -force
```

### <a name="creating-a-new-plug-in"></a>新しいプラグインの作成

このコマンドを実行すると、WS-Management サービスのプラグインが作成 (登録) されます。

```powershell
New-Item -Path WSMan:\localhost\Plugin `
         -Plugin TestPlugin `
         -FileName %systemroot%\system32\WsmWmiPl.dll `
         -Resource http://schemas.dmtf.org/wbem/wscim/2/cim-schema `
         -SDKVersion 1 `
         -Capability "Get","Put","Invoke","Enumerate" `
         -XMLRenderingType text
```

### <a name="creating-a-new-resource-entry"></a>新しいリソースエントリを作成しています

このコマンドは、TestPlugin の Resources ディレクトリにリソースエントリを作成します。 このコマンドは、TestPlugin が個別のコマンドを使用して作成されていることを前提としています。

```powershell
New-Item -Path WSMan:\localhost\Plugin\TestPlugin\Resources `
         -ResourceUri http://schemas.dmtf.org/wbem/wscim/3/cim-schema `
         -Capability "Enumerate"

```

### <a name="creating-a-new-security-entry-for-a-resource"></a>リソースの新しいセキュリティエントリを作成する

このコマンドを実行すると、Resource_5967683 (特定のリソース) の Security ディレクトリでセキュリティ エンティティが作成されます。 このコマンドは、リソースエントリが個別のコマンドを使用して作成されていることを前提としています。

```powershell
$path = "WSMan:\localhost\Plugin\TestPlugin\Resources\Resource_5967683"
New-Item -Path $path\Security `
         -Sddl "O:NSG:BAD:P(A;;GA;;;BA)S:P(AU;FA;GA;;;WD)(AU;SA;GWGX;;;WD)"
```

### <a name="creating-a-new-client-certificate"></a>新しいクライアント証明書の作成

このコマンドは、WS-Management クライアントが使用できる **ClientCertificate** エントリを作成します。 新しい **ClientCertificate** は、 **ClientCertificate** ディレクトリの下に "ClientCertificate_1234567890" として表示されます。 すべてのパラメーターが必須です。 **発行者** は、発行者証明書の拇印である必要があります。

```powershell
$cred = Get-Credential
New-Item -Path WSMan:\localhost\ClientCertificate `
         -Issuer 1b3fd224d66c6413fe20d21e38b304226d192dfe `
         -URI wmicimv2/* `
         -Credential $cred;
```

### <a name="creating-a-new-initialization-parameter"></a>新しい初期化パラメーターの作成

このコマンドは、"InitializationParameters" ディレクトリに "testparametername" という名前の初期化パラメーターを作成します。 このコマンドは、"TestPlugin" が別のコマンドを使用して作成されていることを前提としています。

```powershell
New-Item -Path WSMan:\localhost\Plugin\TestPlugin\InitializationParameters `
         -ParamName testparametername `
         -ParamValue testparametervalue
```

## <a name="dynamic-parameters"></a>動的パラメーター

動的パラメーターは、PowerShell プロバイダーによって追加されるコマンドレットパラメーターであり、プロバイダーに対応したドライブでコマンドレットが使用されている場合にのみ使用できます。

### <a name="address-string"></a>アドレス \<String\>

このリスナーが作成されたアドレスを指定します。 値は次のいずれかになります。

- リテラル文字列 "*"。 (ワイルドカード文字 () を使用すると、すべての IP アドレスがすべての `*` ネットワークアダプターにバインドされます)。
- リテラル文字列 "IP:" の後に、IPv4 のドット区切り10進数形式または IPv6 で複製された16進数形式の有効な IP アドレスが続きます。
- リテラル文字列 "MAC:" の後にアダプターの MAC アドレスが続きます。
  例: MAC:32-a3-58-90-cc。

> [!NOTE]
> Address 値はリスナーの作成時に設定されます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="capability-enumeration"></a>Capability \<Enumeration\>

*プラグイン* を使用する場合、このパラメーターは、この UNIFORM RESOURCE IDENTIFIER (URI) でサポートされる操作を指定します。 URI でサポートされる操作の種類ごとに 1 つのエントリを作成する必要があります。 操作でサポートされている場合は、特定の操作に対して有効な属性を指定できます。

これらの属性には、 **Supportsfiltering** や **supportsfiltering** などがあります。

- **Create**: 作成操作は URI でサポートされています。
  - Create 操作で概念がサポートされている場合は、 **Supportfragment**  属性が使用されます。
  - **Supportfiltering** 属性は作成操作に対して無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **Delete**: delete 操作は URI でサポートされています。
  - 削除操作で概念がサポートされている場合は、 **Supportfragment** 属性が使用されます。
  - **Supportfiltering** 属性は削除操作に対して無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **列挙**: 列挙操作は URI でサポートされています。
  - **Supportfragment** 属性は、列挙操作ではサポートされていません。 False に設定する必要があります。
  - **Supportfiltering** 属性は有効で、プラグインがフィルター処理をサポートしている場合は、この属性を "True" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **Get**: URI で get 操作がサポートされています。
  - Get 操作で概念がサポートされている場合は、 **Supportfragment** 属性が使用されます。
  - **Supportfiltering** 属性は Get 操作に対して無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **Invoke**: 呼び出し操作は URI でサポートされています。
  - **Supportfragment** 属性は、呼び出し操作ではサポートされていません。 False に設定する必要があります。
  - **Supportfiltering** 属性は無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **Put**: put 操作は URI でサポートされています。
  - Put 操作で概念がサポートされている場合は、 **Supportfragment** 属性が使用されます。
  - **Supportfiltering** 属性は、Put 操作に対して無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **Subscribe**: サブスクライブ操作は URI でサポートされています。
  - **Supportfragment** 属性は、サブスクライブ操作ではサポートされていません。 False に設定する必要があります。
  - **Supportfiltering** 属性はサブスクライブ操作に対して無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > Shell 操作もサポートされている場合、URI に対してこの操作は有効ではありません。
- **Shell**: シェル操作は URI でサポートされています。
  - **Supportfragment** 属性はシェル操作ではサポートされていないため、"False" に設定する必要があります。
  - **Supportfiltering** 属性はシェル操作に対して無効であるため、"False" に設定する必要があります。
  > [!NOTE]
  > 他の操作もサポートされている場合、この操作は URI に対して無効です。
  > [!NOTE]
  > Shell 操作が URI に構成されている場合、Get、Put、Create、Delete、Invoke、Enumerate 操作は WS-Management (WinRM) サービスの内部で処理され、シェルを管理します。 結果として、プラグインは操作を処理できません。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="certificatethumbprint-string"></a>CertificateThumbprint \<String\>

サービス証明書の拇印を指定します。

この値は証明書の拇印フィールドの 2 桁の 16 進数値の文字列を表します。 この処理を実行するアクセス許可を持つユーザー アカウントのデジタル公開キー証明書 (X509) を指定します。 証明書は、クライアント証明書ベースの認証で使用されます。 これらの証明書はローカル ユーザー アカウントにしかマップできません。ドメイン アカウントでは機能しません。 証明書の拇印を取得するに `Get-Item` は、PowerShell ドライブのまたはコマンドレットを使用し `Get-ChildItem` `Cert:` ます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="enabled-boolean"></a>Enabled \<Boolean\>

リスナーが有効であるか、無効であるかを指定します。 既定値は True です。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="filename-plugin-string"></a>FileName (Plugin) \<String\>

操作プラグインのファイル名を指定します。 このエントリに格納されている環境変数は、要求が受信されると、ユーザーのコンテキストで展開されます。 各ユーザーは同じ環境変数の異なるバージョンを持つことができるため、各ユーザーが異なるプラグインを持つ可能性があります。 このエントリを空白にすることはできず、有効なプラグインをポイントする必要があります。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="hostname-string"></a>HostName \<String\>

WS-Management (WinRM) サービスが実行されているコンピューターのホスト名を指定します。

値は完全修飾ドメイン名、IPv4 または IPv6 のリテラル文字列、またはワイルドカード文字にする必要があります。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="issuer-string"></a>Issuer \<String\>

証明書を発行した証明機関の名前を指定します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="plugin--ws-management-plug-ins-are-native-dynamic-link-libraries-dlls"></a>プラグイン \<\> WS-Management プラグインはネイティブダイナミックリンクライブラリ (dll) です。

これにより、WS-Management の機能に接続して拡張します。 WSW-Management プラグイン API は、サポートされているリソース Uri と操作に対して特定の Api を実装することによって、ユーザーがプラグインを記述できるようにする機能を提供します。 WS-Management (WinRM) サービスと Internet Information Services (IIS) のいずれかにプラグインを構成すると、WS-Management ホストと IIS ホストにそれぞれプラグインが読み込まれます。 リモート要求はこれらのプラグイン エントリ ポイントに送信され、操作が実行されます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="port-unsigned-short-integer"></a>Port \<Unsigned Short Integer\>

このリスナーが作成された TCP ポートを指定します。 1 から 65535 までの値を指定できます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="resource-string"></a>Resource \<String\>

はっきりと識別できる種類の管理操作または値を表すエンドポイントを指定します。 サービスは 1 つまたは複数のリソースを公開します。一部のリソースには複数のインスタンスが含まれることがあります。 管理リソースは WMI クラスまたはデータベース テーブルに似ています。インスタンスはクラスのインスタンスまたはテーブルの行に似ています。 たとえば、 **Win32_LogicalDisk** クラスはリソースを表します。 `Win32_LogicalDisk="C:\\"` は、リソースの特定のインスタンスです。

Uniform Resource Identifier (URI) にはリソースの接頭辞とパスが含まれます。
次に例を示します。

`http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`

`http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/CIM_NumericSensor`

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="resource-string"></a>Resource \<String\>

ディスクまたはプロセスなど、コンピューター上の特定のリソースを識別する Uniform Resource Identifier (URI) を指定します。

URI は、プレフィックスとリソースのパスで構成されます。 次に例を示します。

`http://schemas.microsoft.com/wbem/wsman/1/wmi/root/cimv2/Win32_LogicalDisk`

`http://schemas.dmtf.org/wbem/wscim/1/cim-schema/2/CIM_NumericSensor`

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="sdkversion-string"></a>SDKVersion \<String\>

WS-Management プラグイン SDK のバージョンを指定します。 唯一の有効な値  です。
1.

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="subject-string"></a>Subject \<String\>

証明書で識別されるエンティティを指定します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="transport-string"></a>Transport \<String\>

WS-Management プロトコルの要求と応答を送受信するためのトランスポートを指定します。 値は HTTP と HTTPS のいずれかになります。

注: トランスポート値はリスナーの作成時に設定されます。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="uri-string"></a>URI \<String\>

アクセスが Sddl パラメーターの値に基づいて認証される URI を識別します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="urlprefix-string"></a>URLPrefix \<String\>

HTTP 要求または HTTPS 要求を受け取る URL 接頭辞。 これは、文字、、 `[a-z]` `[A-Z]` `[9-0]` 、アンダースコア ( `_` )、および円記号 ( `/` ) のみを含む文字列です。 文字列の先頭または末尾を円記号 () にすることはできません `/` 。 たとえば、コンピューター名が "SampleComputer" の場合、WS-Management クライアントは `http://SampleMachine/URLPrefix` 宛先アドレスでを指定します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="value-string"></a>Value \<String\>

構成オプションを指定するプラグイン固有値である初期化パラメーターの値を指定します。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

### <a name="xmlrenderingtype-string"></a>XMLRenderingType \<String\>

**WSMAN_DATA** オブジェクトを使用して、XML がプラグインに渡される形式を指定します。 有効な値は次のようになります。

- Text: 受信 XML データは **WSMAN_DATA_TYPE_TEXT** 構造体に含まれており、これは Xml を **PCWSTR** メモリバッファーとして表します。
- XMLReader: 受信 XML データは **WSMAN_DATA_TYPE_WS_XML_READER** 構造体に含まれています。これは、Xml を **XmlReader** オブジェクトとして表します。これは、"resource.h" ヘッダーファイルで定義されています。

#### <a name="cmdlets-supported"></a>サポートされているコマンドレット

- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)

## <a name="using-the-pipeline"></a>パイプラインの使用

プロバイダーのコマンドレットは、パイプラインの入力を受け入れます。 パイプラインを使用すると、1つのコマンドレットから別のプロバイダーのコマンドレットにプロバイダーデータを送信することにより、タスクを簡単に行うことができます。
プロバイダーコマンドレットでパイプラインを使用する方法の詳細については、この記事全体で説明されているコマンドレットリファレンスを参照してください。

## <a name="getting-help"></a>ヘルプの表示

Windows PowerShell 3.0 より、プロバイダー コマンドレットのためにカスタマイズされたヘルプ トピックを取得できます。これはファイル システム ドライブでのプロバイダー コマンドレットの動作を説明します。

ファイルシステムドライブ用にカスタマイズされたヘルプトピックを取得するには、ファイルシステムドライブで [get-help](xref:Microsoft.PowerShell.Core.Get-Help) コマンドを実行するか、 `-Path` [get-help](xref:Microsoft.PowerShell.Core.Get-Help) のパラメーターを使用してファイルシステムドライブを指定します。

```powershell
Get-Help Get-ChildItem
```

```powershell
Get-Help Get-ChildItem -Path wsman:
```

## <a name="see-also"></a>関連項目

[about_Providers](../../Microsoft.PowerShell.Core/About/about_Providers.md)

