---
ms.date: 12/31/2019
title: ISE オブジェクト モデルの階層
description: この記事では、Windows PowerShell ISE の一部であるオブジェクトの階層について説明します。
ms.openlocfilehash: 00980cda72f05bc6ccb398079b129de13a98f783
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92658311"
---
# <a name="the-ise-object-model-hierarchy"></a>ISE オブジェクト モデルの階層

この記事では、Windows PowerShell Integrated Scripting Environment (ISE) の一部であるオブジェクトの階層について説明します。 Windows PowerShell ISE は、Windows PowerShell 3.0、4.0、5.1 に付属しています。 オブジェクトをクリックすると、そのオブジェクトを定義するクラスのリファレンス ドキュメントに移動します。

## <a name="psise-object"></a>$psISE オブジェクト

`$psISE` オブジェクトは、Windows PowerShell ISE オブジェクト階層の[ルート オブジェクト](The-ObjectModelRoot-Object.md)です。 最上位のこのオブジェクトでは、スクリプト作成に次のオブジェクトを使用できます。

## <a name="psisecurrentfile"></a>[$psISE.CurrentFile](The-ISEFile-Object.md)

`$psISE.CurrentFile` オブジェクトは、[ISEFile](The-ISEFile-Object.md) クラスのインスタンスです。

## <a name="psisecurrentpowershelltab"></a>[$psISE.CurrentPowerShellTab](The-PowerShellTab-Object.md)

`$psISE.CurrentPowerShellTab` オブジェクトは、[PowerShellTab](The-PowerShellTab-Object.md) クラスのインスタンスです。

## <a name="psisecurrentvisiblehorizontaltool"></a>$psISE.CurrentVisibleHorizontalTool

`$psISE.CurrentVisibleHorizontalTool` オブジェクトは、[ISEAddOnTool](The-ISEAddOnTool-Object.md) クラスのインスタンスです。 現在 [Windows PowerShell ISE] ウィンドウの上端にドッキングされているインストール済みのアドオンを表しています。

## <a name="psisecurrentvisibleverticaltool"></a>$psISE.CurrentVisibleVerticalTool

`$psISE.CurrentVisibleHorizontalTool` オブジェクトは、[ISEAddOnTool](The-ISEAddOnTool-Object.md) クラスのインスタンスです。 現在 [Windows PowerShell ISE] ウィンドウの右端にドッキングされているインストール済みのアドオンを表しています。

## <a name="psiseoptions"></a>[$psISE.Options](The-ISEOptions-Object.md)

`$psISE.Options` オブジェクトは、[ISEOptions](The-ISEOptions-Object.md) クラスのインスタンスです。 ISEOptions オブジェクトは、Windows PowerShell ISE のさまざまな設定を表します。 これは Microsoft.PowerShell.Host.ISE.ISEOptions クラスのインスタンスです。

## <a name="psisepowershelltabs"></a>[$psISE.PowerShellTabs](The-PowerShellTabCollection-Object.md)

`$psISE.PowerShellTabs` オブジェクトは、[PowerShellTabCollection](The-PowerShellTabCollection-Object.md) クラスのインスタンスです。 これは現在開いているすべての PowerShell タブのコレクションで、ローカル コンピューターまたは接続されているリモート コンピューターで利用可能な Windows PowerShell の実行環境を表しています。 コレクション内の個々のメンバーは [PowerShellTab](The-PowerShellTab-Object.md) クラスのインスタンスです。

## <a name="see-also"></a>参照

- [Windows PowerShell ISE スクリプト オブジェクト モデルの目的](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [ISE オブジェクト モデルの階層](The-ISE-Object-Model-Hierarchy.md)
