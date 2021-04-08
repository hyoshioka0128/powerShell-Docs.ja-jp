---
title: PowerShell コマンドとは
description: PowerShell を使用すると、システムで使用できる任意のコマンドを実行できます。また、コマンドレットと呼ばれる PowerShell 固有のコマンドがあります。
ms.date: 03/31/2021
ms.openlocfilehash: b6e54349ec15df3327c1f0525dce1a30ad35a6ac
ms.sourcegitcommit: eeedd4472b6cc6158494296c355579791e688baa
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/31/2021
ms.locfileid: "106104014"
---
# <a name="powershell-commands"></a>PowerShell コマンド

PowerShell を使用すると、cmdlet (コマンドレットと発音します) と呼ばれる PowerShell 固有のコマンドを含め、システムで使用できる任意のコマンドを実行できます。

## <a name="what-is-a-cmdlet"></a>コマンドレットとは

コマンドレットは、PowerShell のパイプライン セマンティクスに参加する 1 つのコマンドであり、通常は .NET オブジェクトが返されます。 コマンドレットはネイティブの PowerShell コマンドであり、スタンドアロンの実行可能ファイルではありません。 コマンドレットは、必要に応じて読み込むことができる PowerShell モジュールにまとめられています。 コマンドレットは、任意のコンパイルされた .NET 言語または PowerShell スクリプト言語自体で記述できます。

## <a name="cmdlet-names"></a>コマンドレット名

PowerShell のコマンドレット名には、"_動詞-名詞_" という名前のペアが使われます。 たとえば、PowerShell に含まれている `Get-Command` コマンドレットは、コマンド シェルに登録されているすべてのコマンドレットを取得するために使用されます。 動詞は、コマンドレットによって実行されるアクションを示し、名詞は、コマンドレットによるアクションの実行対象であるリソースを示します。

## <a name="next-steps"></a>次のステップ

PowerShell の詳細と他のコマンドレットを検索する方法については、PowerShell Bits のチュートリアル「[PowerShell を知る](learn/tutorials/01-discover-powershell.md)」を参照してください。

独自のコマンドレットを作成する方法については、次のリソースを参照してください。

スクリプトベースのコマンドレット

- [about_Functions_Advanced](/powershell/module/microsoft.powershell.core/about/about_functions_advanced)
- [about_Functions_CmdletBindingAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_cmdletbindingattribute)
- [about_Functions_Advanced_Methods](/powershell/module/microsoft.powershell.core/about/about_functions_advanced_methods)

コンパイルされたコマンドレット (PowerShell SDK ドキュメント)

- [コマンドレットの概要](developer/cmdlet/cmdlet-overview.md)
