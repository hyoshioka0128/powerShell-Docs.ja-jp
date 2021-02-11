---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: DSCAutomationHostEnabled レジストリ キー
description: この記事では、DSCAutomationHostEnabled レジストリ キーで設定できる値を定義します
ms.openlocfilehash: 50f752dd882e9b0787ed4a4cbc22731fc1d608f5
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92656181"
---
# <a name="dscautomationhostenabled-registry-key"></a>DSCAutomationHostEnabled レジストリ キー

> 適用対象:Windows PowerShell 5.0

DSC は、 **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System** の下の **DSCAutomationHostEnabled** というレジストリ キーを使用してコンピューターの起動時の構成を有効にします。 **DSCAutomationHostEnabled** は 3 つのモードをサポートしています。

| DSCAutomationHostEnabled の値 |                                              説明                                              |
| ------------------------------ | ----------------------------------------------------------------------------------------------------- |
| 0                              | 起動時にコンピューターの構成を行いません。                                                           |
| 1                              | 起動時にコンピューターの構成を行います。                                                            |
| 2                              | コンピューターの構成は、DSC の状態が保留中または最新の場合にのみ行います。 これが既定値です。 |

## <a name="see-also"></a>参照

この機能を使用して初回起動時に構成を実行する方法の例については、「[DSC を使用した初回起動時の仮想マシンの構成](bootstrapDsc.md)」をご覧ください。
