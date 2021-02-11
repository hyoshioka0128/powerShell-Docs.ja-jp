---
ms.date: 09/13/2016
ms.topic: reference
title: コンソール シェルを作成する方法
description: コンソール シェルを作成する方法
ms.openlocfilehash: 9ea67c43b1ee35b1fbfc553b22a1423419317ca2
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92657225"
---
# <a name="how-to-create-a-console-shell"></a>コンソール シェルを作成する方法

Windows PowerShell には、拡張不可能なコンソールシェルを作成するために使用される、"make kit" とも呼ばれる Make-Shell ツールが用意されています。 この新しいツールで作成したシェルは、後で Windows PowerShell スナップインを使用して拡張することはできません。

## <a name="syntax"></a>構文

Make ファイル内から Make-Shell を実行するために使用する構文を次に示します。

```
make-shell
  -out n.exe
  -namespace ns
  [ -lib libdirectory1[,libdirectory2,..] ]
  [ -reference ca1.dll[,ca2.dll,...] ]
  [ -formatdata fd1.format.ps1xml[,fd2.format.ps1xml,...] ]
  [ -typedata td1.type.ps1xml[,td2.type.ps1xml,...] ]
  [ -source c1.cs [,c2.cs,...] ]
  [ -authorizationmanager authorizationManagerType ]
  [ -win32icon i.ico ]
  [ -initscript p.ps1 ]
  [ -builtinscript s1.ps1[,s2.ps1,...] ]
  [ -resource resourcefile.txt ]
  [ -cscflags cscFlags ]
  [ -? | -help ]
```

## <a name="parameters"></a>パラメーター

次に、Make シェルのパラメーターの簡単な説明を示します。

> [!CAUTION]
> アセンブリへの UNC パスは、シェルではサポートされていません。

|パラメーター|説明|
|---------------|-----------------|
|-out n.exe|必須。 生成するシェルの名前。 パスは、このパラメーターの一部として指定されます。<br /><br /> この値が指定されていない場合、シェルによって ".exe" がこの値に追加されます。 **注意:**  参照されている .dll ファイルと同じ名前の出力ファイルを作成しないでください。 これを実行しようとすると、Make-Shell ツールは同じ名前の .cs ファイルを作成します。これにより、コマンドレットのソースコードを含む .cs ファイルが上書きされます。|
|-名前空間 ns|必須。 Runspaceconfiguration クラスに対して使用する名前空間 [。](/dotnet/api/System.Management.Automation.Runspaces.RunspaceConfiguration) これにより、キットが生成およびコンパイルされます。|
|-lib libdirectory1 [, libdirectory2,..]|Windows PowerShell アセンブリ、パラメーターによって指定されたアセンブリ、 `reference` 別のアセンブリによって間接的に参照されるアセンブリ、.net システムアセンブリなど、.net アセンブリを検索するディレクトリ。|
|-参照 ca1.dll [、ca2.dll,...]|シェルに含めるアセンブリのコンマ区切りのリスト。 これらのアセンブリには、すべてのコマンドレットとプロバイダーアセンブリに加えて、読み込む必要のあるリソースアセンブリも含まれています。 このパラメーターが指定されていない場合、Windows PowerShell によって提供されるコアコマンドレットとプロバイダーのみを含むシェルが生成されます。<br /><br /> アセンブリは完全なパスを使用して指定できます。それ以外の場合は、パラメーターで指定されたパスを使用してアセンブリが検索され `lib` ます。|
|-formatdata fd1.format.ps1xml [, fd2.format.ps1xml,...]|シェルに含めるフォーマットデータのコンマ区切りのリスト。 このパラメーターが指定されていない場合、Windows PowerShell によって提供される形式データのみを含むシェルが生成されます。|
|-typedata td1.type.ps1xml [、td2.type.ps1xml,...]|シェルに含める型データのコンマ区切りのリスト。 このパラメーターが指定されていない場合、Windows PowerShell によって提供される型データのみを含むシェルが生成されます。|
|-source c1.cs [, c2.cs,...]|シェル開発者によって提供される、シェルのビルドに必要なすべてのソースコードを含むファイルの名前。<br /><br /> ソースコードファイルには、次のいずれかのソースコードを含めることができます。<br /><br /> -既定の承認マネージャーをオーバーライドする承認マネージャーの実装。 (これは、アセンブリにコンパイルされることもあります)。<br />-アセンブリ情報属性の宣言: Assemblycompanyattribute、Assemblycompanyattribute Attribute、AssemblyFileVersionAttribute、System.reflection.assemblyinformationalversionattribute>、Assemblycompanyattribute、および System.reflection.assemblytrademarkattribute>。|
|-authorizationmanager authorizationManagerType|承認マネージャーの実装を含む型。 これは、ソースコードで定義することも、アセンブリにコンパイルすることもできます (パラメーターで指定 `reference` )。 このパラメーターが指定されていない場合は、既定のセキュリティマネージャーが使用されます。 値は、名前空間を含む完全な型名にする必要があります。|
|-win32icon i .ico|シェルの .exe ファイルのアイコン。 指定しない場合、シェルには、c# コンパイラに含まれるアイコン (存在する場合) が表示されます。|
|-initscript p.ps1|シェルのスタートアッププロファイル。 ファイルは "その他" として含まれています。シェルによる有効性チェックは行われません。|
|-builtinscript s1.ps1 [、s2.ps1,...]|シェルの組み込みスクリプトの一覧。 これらのスクリプトは、パスのスクリプトの前に検出され、シェルが構築されると、その内容を変更することはできません。<br /><br /> ファイルは "その他" として含まれています。シェルによる有効性チェックは行われません。|
|-リソース resourcefile.txt|シェルのヘルプおよびバナーリソースを含む .txt ファイル。 最初のリソースの名前は ShellHelp で、パラメーターを指定してシェルが呼び出された場合に表示されるテキストが含まれてい `help` ます。 2番目のリソースは ShellBanner という名前で、シェルが対話モードで起動したときに表示されるテキストと著作権の情報が含まれています。<br /><br /> このパラメーターが指定されていない場合、またはこれらのリソースが存在しない場合は、一般的なヘルプとバナーが使用されます。|
|-cscflags cscFlags|C# コンパイラに渡す必要があるフラグ (csc.exe)。 これらは変更されずに渡されます。 このパラメーターにスペースが含まれている場合は、二重引用符で囲む必要があります。|
|-?<br /><br /> -help|著作権メッセージと Make-Shell コマンドラインオプションを表示します。|
|-verbose|シェルの作成中に詳細情報を表示します。|

## <a name="see-also"></a>参照

[Windows PowerShell プログラマー ガイド](./windows-powershell-programmer-s-guide.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
