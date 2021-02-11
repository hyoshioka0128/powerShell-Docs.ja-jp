---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell プロバイダーの概要
description: Windows PowerShell プロバイダーの概要
ms.openlocfilehash: 2f1c5f5991a64fb2b85ece7feba915164ebd34ee
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355512"
---
# <a name="windows-powershell-provider-overview"></a>Windows PowerShell プロバイダーの概要

Windows PowerShell プロバイダーを使用すると、マウントされたドライブのように、任意のデータストアをファイルシステムとして公開できます。 たとえば、組み込みレジストリプロバイダーを使用すると、コンピューターのドライブに移動する場合と同様に、レジストリ内を移動でき `c` ます。 プロバイダーは、コマンドレット (たとえば、、など) をオーバーライドすることもできます。これにより、 `Item` `Get-Item` `Set-Item` データストア内のデータはファイルシステムの移動時に処理されます。 プロバイダーとドライブ、および Windows PowerShell の組み込みプロバイダーの詳細については、「 [about_Providers](/powershell/module/microsoft.powershell.core/about/about_providers)」を参照してください。

## <a name="providers-and-drives"></a>プロバイダーとドライブ

プロバイダーは、データストアへのアクセス、移動、および編集に使用されるロジックを定義します。ドライブは、プロバイダーによって定義された型のデータストア (またはデータストアの一部) への特定のエントリポイントを指定します。 たとえば、レジストリプロバイダーを使用すると、レジストリのハイブとキーにアクセスできます。 HKLM ドライブと HKCU ドライブは、レジストリ内の対応するハイブを指定します。 HKLM および HKCU ドライブはどちらもレジストリプロバイダーを使用します。

プロバイダーを作成するときに、プロバイダーが利用可能になったときに自動的に作成される既定のドライブドライブを指定できます。 また、そのプロバイダーを使用する新しいドライブを作成するためのメソッドを定義します。

## <a name="type-of-providers"></a>プロバイダーの種類

プロバイダーにはいくつかの種類があり、それぞれに異なるレベルの機能が用意されています。 プロバイダーは、 [SessionStateCategory](/dotnet/api/system.management.automation.sessionstatecategory)のいずれかのプロバイダークラスの子孫から派生するクラスとして実装され 
 ます。 さまざまな種類のプロバイダーの詳細については、「 [プロバイダーの種類](./provider-types.md)」を参照してください。

## <a name="provider-cmdlets"></a>プロバイダー コマンドレット

プロバイダーは、コマンドレットに対応するメソッドを実装し、そのプロバイダーのドライブで使用されている場合に、これらのコマンドレットのカスタム動作を作成できます。 プロバイダーの種類に応じて、異なる一連のコマンドレットを使用できます。 プロバイダーでカスタマイズ可能なコマンドレットの完全な一覧については、「 [プロバイダーコマンドレット](./provider-cmdlets.md)」を参照してください。

## <a name="provider-paths"></a>プロバイダーのパス

ユーザーは、ファイルシステムなどのプロバイダーのドライブを移動します。 このため、ファイルシステムのナビゲーションで使用されるパスに対応するパスの構文が想定されています。 ユーザーは、プロバイダーコマンドレットを実行するときに、アクセスする項目へのパスを指定します。 指定されたパスは、いくつかの方法で解釈できます。 プロバイダーは、次のパスの種類の1つ以上をサポートする必要があります。

### <a name="drive-qualified-paths"></a>ドライブ修飾パス

ドライブ修飾パスは、項目名、項目が配置されているコンテナーとサブコンテナー、および項目のアクセスに使用される Windows PowerShell ドライブを組み合わせたものです。 (ドライブは、データストアへのアクセスに使用されるプロバイダーによって定義されます。 このパスの先頭には、ドライブ名とコロン (:) が続きます。 例: `get-childitem C:`

### <a name="provider-qualified-paths"></a>プロバイダー修飾パス

Windows PowerShell エンジンがプロバイダーを初期化および初期化解除できるようにするには、プロバイダーがプロバイダーによって修飾されたパスをサポートする必要があります。 たとえば、ユーザーは、次のプロバイダーによって修飾されたパスを定義するため、FileSystem プロバイダーを初期化および初期化解除 `FileSystem::\\uncshare\abc\bar` できます。

### <a name="provider-direct-paths"></a>プロバイダー-直接パス

Windows PowerShell プロバイダーへのリモートアクセスを許可するには、現在の場所に対して Windows PowerShell プロバイダーに直接渡すプロバイダー-直接パスをサポートする必要があります。 たとえば、Windows PowerShell プロバイダーは、 `\\server\regkeypath` プロバイダーの直接のパスとしてを使用できます。

### <a name="provider-internal-paths"></a>プロバイダー-内部パス

プロバイダーコマンドレットが Windows PowerShell 以外のアプリケーションプログラミングインターフェイス (Api) を使用してデータにアクセスできるようにするには、Windows PowerShell プロバイダーがプロバイダー内部パスをサポートしている必要があります。 このパスは、プロバイダーによって修飾されたパスの "::" の後に示されます。 たとえば、ファイルシステム Windows PowerShell プロバイダーのプロバイダー内部パスは `\\uncshare\abc\bar` です。

## <a name="overriding-cmdlet-parameters"></a>コマンドレットパラメーターのオーバーライド

プロバイダー固有のコマンドレットの動作は、プロバイダーによってオーバーライドされる場合があります。 オーバーライド可能なパラメーターの一覧と、それらをプロバイダークラスでオーバーライドする方法については、「[プロバイダーコマンドレットのパラメーター](./provider-cmdlet-parameters.md) 」を参照してください。

## <a name="dynamic-parameters"></a>動的パラメーター

プロバイダーは、ユーザーがコマンドレットのいずれかの静的パラメーターに特定の値を指定した場合に、プロバイダーコマンドレットに追加される動的パラメーターを定義できます。 プロバイダーは、1つまたは複数の動的パラメーターメソッドを実装することによってこれを行います。 動的パラメーターの追加に使用できるコマンドレットパラメーターの一覧と、それらを実装するために使用されるメソッドについては、「 [プロバイダーコマンドレット動的パラメーター](./provider-cmdlet-dynamic-parameters.md)」を参照してください。

## <a name="provider-capabilities"></a>プロバイダーの機能

プロバイダーがサポートできるさまざまな機能が定義さ [れてい](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) ます ()。 これには、ワイルドカード、フィルター項目、およびサポートトランザクションを使用する機能が含まれます。 プロバイダーの機能を指定するには、 [](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) `OR` プロバイダークラスの system.servicemodel (属性の2番目のパラメーター) プロパティとして、論理操作と組み合わせて、system.string 列挙型の値のリストを追加します。このプロパティ[には、](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute.ProviderCapabilities)プロバイダークラスの[...](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute) ............. プロバイダーの属性属性が含まれます。 たとえば、次の属性では、プロバイダーがシステムの[管理](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities)をサポートしていることを指定しています。プロバイダーは、の 
 **処理** と[システムの管理](/dotnet/api/System.Management.Automation.Provider.ProviderCapabilities) 
 を実行します。

```csharp
[CmdletProvider(RegistryProvider.ProviderName, ProviderCapabilities.ShouldProcess | ProviderCapabilities.Transactions)]

```

## <a name="provider-cmdlet-help"></a>プロバイダーコマンドレットのヘルプ

プロバイダーを作成するときに、サポートするプロバイダーコマンドレットに独自のヘルプを実装できます。
これには、プロバイダーコマンドレットごとに1つのヘルプトピックが含まれています。また、プロバイダーコマンドレットの動作が動的パラメーターの使用方法によって異なる場合は、複数のバージョンのヘルプトピックがあります。 プロバイダーのコマンドレット固有のヘルプをサポートするには、プロバイダーが [Icmdletprovidersupportshelp](/dotnet/api/System.Management.Automation.Provider.ICmdletProviderSupportsHelp) インターフェイスを実装する必要があります。

Windows PowerShell エンジンは、 [Icmdletprovidersupportshelp](/dotnet/api/System.Management.Automation.Provider.ICmdletProviderSupportsHelp.GetHelpMaml) を呼び出して、プロバイダーのコマンドレットのヘルプトピックを表示するためのメソッドを呼び出します。 エンジンは、コマンドレットの実行時にユーザーが指定したコマンドレットの名前 `Get-Help` と、ユーザーの現在のパスを提供します。
プロバイダーが異なるドライブに対して同じプロバイダーコマンドレットの異なるバージョンを実装している場合は、現在のパスが必要です。 メソッドは、コマンドレットヘルプの XML を含む文字列を返す必要があります。

ヘルプファイルの内容は、PSMAML XML を使用して書き込まれます。 これは、スタンドアロンのコマンドレットのヘルプコンテンツを記述するために使用される XML スキーマと同じです。 カスタムコマンドレットのヘルプの内容を、プロバイダーのヘルプファイルの要素の下に追加し `CmdletHelpPaths` ます。 次の例は、 `command` 1 つのプロバイダーコマンドレットの要素を示しています。また、プロバイダーが提供するプロバイダーコマンドレットの名前を指定する方法を示しています。 対応

```xml
<CmdletHelpPaths>
  <command:command>
    <command:details>
      <command:name>ProviderCmdletName</command:name>
      <command:verb>Verb</command:verb>
      <command:noun>Noun</command:noun>
    <command:details>
  </command:command>
<CmdletHelpPath>
```

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーの機能](./provider-types.md)

[プロバイダーコマンドレット](./provider-cmdlets.md)

[Windows PowerShell プロバイダーを記述する](./writing-a-windows-powershell-provider.md)
