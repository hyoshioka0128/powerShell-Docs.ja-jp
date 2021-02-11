---
description: セッションで使用される名前空間を指定できます。
Locale: en-US
ms.date: 01/19/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_using?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Using
ms.openlocfilehash: 2ada269fd0ce6b34a5f7faccfddf47a799301eb9
ms.sourcegitcommit: 94d597c4fb38793bc49ca7610e2c9973b1e577c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98619943"
---
# <a name="about-using"></a>使用について

## <a name="short-description"></a>概要
セッションで使用される名前空間を指定できます。

## <a name="long-description"></a>詳細説明

`using`ステートメントでは、セッションで使用する名前空間を指定できます。 名前空間を追加すると、.NET のクラスおよびメンバーの使用が簡略化され、スクリプトモジュールとアセンブリからクラスをインポートできるようになります。

ステートメントは、 `using` スクリプトまたはモジュール内の他のステートメントの前に記述する必要があります。 パラメーターを含め、コメント解除されたステートメントの前には使用できません。

ステートメントには、 `using` 変数を含めることはできません。

ステートメントは、 `using` `using:` 変数のスコープ修飾子と混同しないようにしてください。 詳細については、「 [about_Remote_Variables](about_Remote_Variables.md)」を参照してください。

## <a name="namespace-syntax"></a>名前空間の構文

型の解決に使用する .NET 名前空間を指定するには、次のようにします。

```
using namespace <.NET-namespace>
```

名前空間を指定すると、短い名前を使用して型を簡単に参照できます。

## <a name="module-syntax"></a>モジュールの構文

PowerShell モジュールからクラスを読み込むには、次のようにします。

```
using module <module-name>
```

の値には、 `<module-name>` モジュール名、モジュールの完全な指定、またはモジュールファイルへのパスを指定できます。

`<module-name>`がパスの場合は、完全修飾パスまたは相対パスを指定できます。 相対パスは、using ステートメントを含むスクリプトに対して相対的に解決されます。

> [!NOTE]
> 相対パスにスラッシュ () が含まれている場合 `/` 、PowerShell では、スクリプトの場所を基準とした相対パスではなく、現在の場所を基準とした相対パスが処理されます。 このバグは、PowerShell 7.1 で修正されています。

`<module-name>`が名前またはモジュールの指定である場合、PowerShell は指定されたモジュールの **PSModulePath** を検索します。

モジュール仕様は、次のキーを持つハッシュテーブルです。

- `ModuleName` - **必須** モジュール名を指定します。
- `GUID` - **省略可能** モジュールの GUID を指定します。
- 以下の3つのキーのいずれかを指定する **必要** もあります。 これらのキーを一緒に使用することはできません。
  - `ModuleVersion` -モジュールの許容される最小バージョンを指定します。
  - `RequiredVersion` -モジュールの正確な必須バージョンを指定します。
  - `MaximumVersion` -モジュールの許容される最大バージョンを指定します。

ステートメントは、 `using module` `ModuleToProcess` スクリプトモジュールまたはバイナリモジュールのルートモジュール () からクラスをインポートします。 入れ子になったモジュールで定義されているクラスや、ドットソースのスクリプトで定義されているクラスをモジュールに常にインポートすることはできません。 モジュールの外部のユーザーが使用できるようにするクラスは、ルートモジュールで定義する必要があります。

スクリプトモジュールの開発時には、コードに変更を加えた後、Force パラメーターを指定してを使用して新しいバージョンのモジュールを読み込むのが一般的です `Import-Module` 。  これは、ルートモジュールの関数の変更に対してのみ機能します。 `Import-Module` では、入れ子になったモジュールは再読み込みされません。 また、更新されたクラスを読み込む方法はありません。

最新バージョンを実行していることを確認するには、コマンドレットを使用してモジュールをアンロードする必要があり `Remove-Module` ます。 `Remove-Module` ルートモジュール、すべての入れ子になったモジュール、およびモジュールで定義されているすべてのクラスを削除します。 次に、およびステートメントを使用して、モジュールとクラスを再度読み込みます `Import-Module` `using module` 。

## <a name="assembly-syntax"></a>アセンブリ構文

.NET アセンブリから型をプリロードするには、次のようにします。

```
using assembly <.NET-assembly-path>
```

アセンブリを読み込むと、解析時にそのアセンブリから .NET 型がスクリプトにプリロードされます。 これにより、プリロードされたアセンブリの型を使用する新しい PowerShell クラスを作成できます。

新しい PowerShell クラスを作成しない場合は、 `Add-Type` 代わりにコマンドレットを使用します。 詳細については、「 [Add-Type](xref:Microsoft.PowerShell.Utility.Add-Type)」を参照してください。

## <a name="examples"></a>使用例

### <a name="example-1---add-namespaces-for-typename-resolution"></a>例 1-typename の解決のための名前空間を追加する

次のスクリプトは、"Hello World" 文字列の暗号化ハッシュを取得します。

とでは、との間の参照が簡略化されていることに注意して `using namespace System.Text` `using namespace System.IO` `[UnicodeEncoding]` `System.Text` `[Stream]` `[MemoryStream]` `System.IO` ください。

```powershell
using namespace System.Text
using namespace System.IO

[string]$string = "Hello World"
## Valid values are "SHA1", "SHA256", "SHA384", "SHA512", "MD5"
[string]$algorithm = "SHA256"

[byte[]]$stringbytes = [UnicodeEncoding]::Unicode.GetBytes($string)

[Stream]$memorystream = [MemoryStream]::new($stringbytes)
$hashfromstream = Get-FileHash -InputStream $memorystream `
  -Algorithm $algorithm
$hashfromstream.Hash.ToString()
```

### <a name="example-2---load-classes-from-a-script-module"></a>例 2-スクリプトモジュールからクラスを読み込む

この例では、次のクラスを定義する **Cardgames** という名前の PowerShell スクリプトモジュールを用意しています。

- **CardGames。デッキ**
- **CardGames. カード**

`Import-Module` また、ステートメントでは、モジュール `#requires` で定義されているモジュール関数、エイリアス、および変数のみをインポートします。 クラスはインポートされません。 この `using module` コマンドは、モジュールをインポートし、クラス定義も読み込みます。

```powershell
using module CardGames
using namespace CardGames

[Deck]$deck = [Deck]::new()
$deck.Shuffle()
[Card[]]$hand1 = $deck.Deal(5)
[Card[]]$hand2 = $deck.Deal(5)
[Card[]]$hand3 = $deck.Deal(5)
```

### <a name="example-3---load-classes-from-an-assembly"></a>例 3-アセンブリからクラスを読み込む

この例では、クラスを使用して新しい PowerShell クラスを作成できるように、アセンブリを読み込みます。 次のスクリプトは、 **Directorycontext** クラスから派生した新しい PowerShell クラスを作成します。

```powershell
using assembly 'C:\Program Files\PowerShell\7\System.DirectoryServices.dll'
using namespace System.DirectoryServices.ActiveDirectory

class myDirectoryClass : System.DirectoryServices.ActiveDirectory.DirectoryContext
{

  [DirectoryContext]$domain

  myDirectoryClass([DirectoryContextType]$ctx) : base($ctx)
  {
    $this.domain = [DirectoryContext]::new([DirectoryContextType]$ctx)
  }

}

$myDomain = [myDirectoryClass]::new([DirectoryContextType]::Domain)
$myDomain
```

```Output
domain                                                    Name UserName ContextType
------                                                    ---- -------- -----------
System.DirectoryServices.ActiveDirectory.DirectoryContext                    Domain
```
