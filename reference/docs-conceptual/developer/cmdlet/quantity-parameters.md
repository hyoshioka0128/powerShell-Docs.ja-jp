---
ms.date: 09/13/2016
ms.topic: reference
title: 数量のパラメーター
description: 数量のパラメーター
ms.openlocfilehash: 3f7c23eec34a709b1f2d59f611c93909b20f4124
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92650299"
---
# <a name="quantity-parameters"></a>数量のパラメーター

次の表に、quantity パラメーターの推奨される名前と機能を示します。

|パラメーター|機能|
|---|---|
|**すべて**<br>データ型: ブール値|このパラメーターは、 `true` リソースの既定のサブセットではなく、すべてのリソースを処理する必要があることを示すために実装します。 リソースのサブセットを示すように、このパラメーターを実装 `false` します。|
|**Allocation**<br>データ型: Int32|割り当てられる項目の数をユーザーが指定できるように、このパラメーターを実装します。|
|**ブロック数**<br>データ型: Int64|ユーザーがブロックカウントを指定できるように、このパラメーターを実装します。|
|**Count**<br>データ型: Int64|ユーザーがカウントを指定できるように、このパラメーターを実装します。|
|**スコープ**<br>データ型: キーワード|操作するスコープをユーザーが指定できるように、このパラメーターを実装します。|

## <a name="see-also"></a>参照

[コマンドレットのパラメーター](./cmdlet-parameters.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
