---
ms.date: 07/08/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: カスタム Windows PowerShell Desired State Configuration のビルド
description: この記事では、開発リソースの概要および特定の情報と例を含む記事へのリンクを示します。
ms.openlocfilehash: ff9a0c8ae10583155217fbeccc62e6e1c94f9a11
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92656407"
---
# <a name="build-custom-windows-powershell-desired-state-configuration-resources"></a>カスタム Windows PowerShell Desired State Configuration のビルド

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

Windows PowerShell Desired State Configuration (DSC) には、環境の構成に使用できる組み込みのリソースがあります この記事では、開発リソースの概要および特定の情報と例を含む記事へのリンクを示します。

## <a name="dsc-resource-components"></a>DSC リソース コンポーネント

DSC リソースは、Windows PowerShell モジュールです。 モジュールには、リソースのスキーマ (構成可能なプロパティの定義) と実装 (構成で指定された実際の作業を実行するコード) の両方が含まれています。 DSC リソースのスキーマは MOF ファイルで定義でき、実装はスクリプト モジュールによって実行されます。 バージョン 5 で PowerShell クラスがサポートされるようになってから、スキーマと実装は両方ともクラスで定義できます。 次の記事では、DSC リソースを作成する方法をさらに詳しく説明します。

- [MOF を使用したカスタム DSC リソースの記述](authoringResourceMOF.md)
- [Implementing a DSC resource in C# (C# での DSC リソースの実装)](authoringResourceMofCS.md)
- [PowerShell クラスを使用したカスタム DSC リソースの記述](authoringResourceClass.md)
- [複合リソース: リソースとしての DSC 構成の使用](authoringResourceComposite.md)
- [リソース デザイナー ツールの使用](authoringResourceMofDesigner.md)
