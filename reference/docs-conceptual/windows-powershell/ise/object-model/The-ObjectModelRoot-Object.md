---
ms.date: 08/25/2017
title: ObjectModelRoot オブジェクト
description: PowerShell ISE のプリンシパル ルート オブジェクトである $psISE オブジェクトは、Microsoft.PowerShell.Host.ISE.ObjectModelRoot クラスのインスタンスです。 このトピックでは、ObjectModelRoot オブジェクトのプロパティについて説明します。
ms.openlocfilehash: c8ae703c2663256ca037090fd3b1eb239cfc431a
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92660949"
---
# <a name="the-objectmodelroot-object"></a>ObjectModelRoot オブジェクト

Windows PowerShell&reg; Integrated Scripting Environment (ISE) のプリンシパル ルート オブジェクトである `$psISE` オブジェクトは、Microsoft.PowerShell.Host.ISE.ObjectModelRoot クラスのインスタンスです。 このトピックでは、 **ObjectModelRoot** オブジェクトのプロパティについて説明します。

## <a name="properties"></a>Properties

### <a name="currentfile"></a>CurrentFile

> Windows PowerShell ISE 2.0 以降でサポートされています。

現在フォーカスしているこのホスト オブジェクトに関連付けられているファイルを取得する読み取り専用のプロパティ。

### <a name="currentpowershelltab"></a>CurrentPowerShellTab

> Windows PowerShell ISE 2.0 以降でサポートされています。

フォーカスしている PowerShell タブを取得する読み取り専用のプロパティ。

### <a name="currentvisiblehorizontaltool"></a>CurrentVisibleHorizontalTool

> Windows PowerShell ISE 2.0 以降でサポートされています。

エディターの下部にある水平方向のツール ウィンドウに現在表示されている Windows PowerShell ISE アドオン ツールを取得する読み取り専用のプロパティ。

### <a name="currentvisibleverticaltool"></a>CurrentVisibleVerticalTool

> Windows PowerShell ISE 2.0 以降でサポートされています。

エディターの右側にある垂直方向のツール ウィンドウに現在表示されている Windows PowerShell ISE アドオン ツールを取得する読み取り専用のプロパティ。

### <a name="options"></a>オプション

> Windows PowerShell ISE 2.0 以降でサポートされています。

Windows PowerShell ISE の設定を変更することができるさまざまなオプションを取得する読み取り専用のプロパティ。

### <a name="powershelltabs"></a>PowerShellTabs

> Windows PowerShell ISE 2.0 以降でサポートされています。

Windows PowerShell ISE で開かれている PowerShell タブのコレクションを取得する読み取り専用のプロパティ。 既定では、このオブジェクトには 1 つの PowerShell タブが含まれています。ただし、スクリプトまたは Windows PowerShell ISE のメニューを使用して、このオブジェクトに他の PowerShell タブを追加することができます。

## <a name="see-also"></a>参照

- [Windows PowerShell ISE スクリプト オブジェクト モデルの目的](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [ISE オブジェクト モデルの階層](The-ISE-Object-Model-Hierarchy.md)
