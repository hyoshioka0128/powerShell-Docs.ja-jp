---
ms.date: 09/13/2016
ms.topic: reference
title: PowerShell バイナリ モジュールを記述する方法
description: PowerShell バイナリ モジュールを記述する方法
ms.openlocfilehash: 1d5cea474ac418f78cc782360b7c23b7614d6669
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92663076"
---
# <a name="how-to-write-a-powershell-binary-module"></a>PowerShell バイナリ モジュールを記述する方法

バイナリモジュールには、コマンドレットクラスを含む任意のアセンブリ (.dll) を指定できます。 既定では、バイナリモジュールをインポートするときに、アセンブリ内のすべてのコマンドレットがインポートされます。 ただし、ルートモジュールがアセンブリであるモジュールマニフェストを作成することによって、インポートされるコマンドレットを制限できます。 (たとえば、マニフェストのコマンドレット Stoexport キーを使用すると、必要なコマンドレットだけをエクスポートできます)。さらに、バイナリモジュールには、追加のファイル、ディレクトリ構造、および1つのコマンドレットでは使用できない有用な管理情報を含めることができます。

次の手順では、PowerShell バイナリモジュールを作成してインストールする方法について説明します。

#### <a name="how-to-create-and-install-a-powershell-binary-module"></a>PowerShell バイナリモジュールを作成してインストールする方法

1. 必要な機能を備えたバイナリ PowerShell ソリューション (C# で記述されたコマンドレットなど) を作成し、正常に動作することを確認します。

   コードの観点から見ると、バイナリモジュールの中核となるのは、単なるコマンドレットアセンブリです。 実際、PowerShell は、1つのコマンドレットアセンブリを読み込みとアンロードという点でモジュールとして処理し、開発者の作業を追加する必要はありません。 コマンドレットの記述の詳細については、「 [Windows PowerShell コマンドレットの記述](../cmdlet/writing-a-windows-powershell-cmdlet.md)」を参照してください。

2. 必要に応じて、ソリューションの残りの部分 (追加のコマンドレット、XML ファイルなど) を作成し、モジュールマニフェストで記述します。

   モジュールマニフェストでは、ソリューション内のコマンドレットアセンブリを記述するだけでなく、モジュールをエクスポートおよびインポートする方法、どのコマンドレットを公開するか、およびモジュールに追加されるその他のファイルについても記述できます。
   前に説明したように、PowerShell では、追加の作業を必要とせずに、モジュールと同様にバイナリコマンドレットを扱うことができます。
   そのため、モジュールマニフェストは、主に、複数のファイルを1つのパッケージに結合したり、特定のアセンブリに対してパブリケーションを明示的に制御したりする場合に役立ちます。
   詳細については、「 [PowerShell モジュールマニフェストを記述する方法](how-to-write-a-powershell-module-manifest.md)」を参照してください。

   次のコードは、モジュールとして使用できる同じファイル内に3つのコマンドレットを含む、非常に単純な C# コードブロックです。

   ```csharp
   using System.Management.Automation;           // Windows PowerShell namespace.

   namespace ModuleCmdlets
   {
     [Cmdlet(VerbsDiagnostic.Test,"BinaryModuleCmdlet1")]
     public class TestBinaryModuleCmdlet1Command : Cmdlet
     {
       protected override void BeginProcessing()
       {
         WriteObject("BinaryModuleCmdlet1 exported by the ModuleCmdlets module.");
       }
     }

     [Cmdlet(VerbsDiagnostic.Test, "BinaryModuleCmdlet2")]
     public class TestBinaryModuleCmdlet2Command : Cmdlet
     {
         protected override void BeginProcessing()
         {
             WriteObject("BinaryModuleCmdlet2 exported by the ModuleCmdlets module.");
         }
     }

     [Cmdlet(VerbsDiagnostic.Test, "BinaryModuleCmdlet3")]
     public class TestBinaryModuleCmdlet3Command : Cmdlet
     {
         protected override void BeginProcessing()
         {
             WriteObject("BinaryModuleCmdlet3 exported by the ModuleCmdlets module.");
         }
     }

   }
   ```

3. ソリューションをパッケージ化し、PowerShell モジュールパス内の任意の場所にパッケージを保存します。

   `PSModulePath`グローバル環境変数は、PowerShell がモジュールを検索するために使用する既定のパスを記述します。 たとえば、システムにモジュールを保存するための一般的なパスは、のようになり `%SystemRoot%\users\<user>\Documents\WindowsPowerShell\Modules\<moduleName>` ます。 既定のパスを使用しない場合は、インストール中にモジュールの場所を明示的に指定する必要があります。 ソリューションの複数のアセンブリとファイルを格納するフォルダーが必要になる場合があるので、モジュールを保存するためのフォルダーを必ず作成してください。

   技術的には、でモジュールをインストールする必要がないことに注意してください `PSModulePath` 。これらは、PowerShell がモジュールを検索する既定の場所です。 ただし、モジュールを他の場所に格納することが適切な理由がない限り、ベストプラクティスと考えてください。 詳細については、「 [Powershell モジュールのインストール](./installing-a-powershell-module.md) 」および「 [Powershell モジュールのインストールパスの変更](./modifying-the-psmodulepath-installation-path.md)」を参照してください。

4. モジュールを PowerShell にインポートし[、モジュールを呼び出します。](/powershell/module/Microsoft.PowerShell.Core/Import-Module)

   モジュールを [インポート](/powershell/module/Microsoft.PowerShell.Core/Import-Module) するためにを呼び出すと、モジュールがアクティブメモリに読み込まれます。 PowerShell 3.0 以降を使用している場合は、コードでモジュールの名前を呼び出すだけでもインポートされます。詳細については、「 [PowerShell モジュールのインポート](./importing-a-powershell-module.md)」を参照してください。

## <a name="importing-snap-in-assemblies-as-modules"></a>モジュールとしてのスナップインアセンブリのインポート

スナップインアセンブリに存在するコマンドレットとプロバイダーは、バイナリモジュールとして読み込むことができます。 スナップインアセンブリがバイナリモジュールとして読み込まれると、スナップインのコマンドレットとプロバイダーはユーザーが使用できますが、アセンブリのスナップインクラスは無視され、スナップインは登録されません。 その結果、コマンドレットとプロバイダーをセッションで使用できる場合でも、Windows PowerShell によって提供されるスナップインコマンドレットでスナップインを検出することはできません。

また、スナップインによって参照されるすべての書式設定ファイルまたは型ファイルは、バイナリモジュールの一部としてインポートできません。
書式設定ファイルと型ファイルをインポートするには、モジュールマニフェストを作成する必要があります。
「 [PowerShell モジュールマニフェストを記述する方法](how-to-write-a-powershell-module-manifest.md)」を参照してください。

## <a name="see-also"></a>参照

[Windows PowerShell モジュールを記述する](./writing-a-windows-powershell-module.md)
