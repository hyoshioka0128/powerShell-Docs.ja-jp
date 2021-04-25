---
title: PowerShell コマンドとは
description: PowerShell のコマンドは、cmdlets と呼ばれます (「コマンドレット」と発音されます)
ms.date: 03/31/2021
ms.openlocfilehash: 3980ed53f0e0c6f86a5ebab7b6ca64aeee6b173e
ms.sourcegitcommit: d63769de0e59d117d90171799bda6544bf2f2f0b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2021
ms.locfileid: "107855568"
---
# <a name="what-is-a-powershell-command-cmdlet"></a>PowerShell コマンド (コマンドレット) とは

PowerShell のコマンドは、cmdlets と呼ばれます (「コマンドレット」と発音されます)。 PowerShell では、コマンドレットに加えて、ご利用のシステム上で使用可能な任意のコマンドも実行できます。

## <a name="what-is-a-cmdlet"></a>コマンドレットとは

コマンドレットはネイティブの PowerShell コマンドであり、スタンドアロンの実行可能ファイルではありません。 コマンドレットは、必要に応じて読み込むことができる PowerShell モジュールにまとめられています。 コマンドレットは、任意のコンパイルされた .NET 言語または PowerShell スクリプト言語自体で記述できます。

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
