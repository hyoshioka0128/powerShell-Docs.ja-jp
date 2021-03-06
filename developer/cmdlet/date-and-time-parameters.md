---
title: 日付と時刻のパラメーター |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 71da921b-7c32-4155-b2f8-b19f30ec774d
caps.latest.revision: 7
ms.openlocfilehash: 5b1f093de5db364ac806e58c4ed8dbf2948cb6c6
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62068234"
---
# <a name="date-and-time-parameters"></a>日付と時刻のパラメーター

次の表は、推奨される名前と日付と時刻の情報を処理するパラメーターの機能を示します。 通常、日付と時刻のパラメーターは何かが作成またはアクセスしたときの記録に使用されます。

|パラメーター|機能|
|---|---|
|**アクセス**<br>データの種類:SwitchParameter|このパラメーターを実装で指定された日時に基づくコマンドレットは、アクセスされたリソースの動作を指定には、**する前に**と**後**パラメーター。 このパラメーターが指定されている場合、 **Created**と**Modified**パラメーターである必要がありますが指定されていません。|
|**後**<br>データの種類:DateTime|コマンドレットの使用後の日時を指定するには、このパラメーターを実装します。 **後**パラメーターを操作するコマンドレットがあります、**アクセス**、 **Created**、または**Modified**パラメーター。 そのパラメーターを設定する必要があります**true**コマンドレットが呼び出された場合。|
|**以前は**<br>データの種類:DateTime|その前に、コマンドレットが使用された日時を指定するには、このパラメーターを実装します。 **する前に**パラメーターを操作するコマンドレットがあります、**アクセス**、 **Created**、または**Modified**パラメーター。 そのパラメーターを設定する必要があります**true**コマンドレットが呼び出された場合。|
|**作成**<br>データの種類:SwitchParameter|このパラメーターを実装で指定された日時に基づくコマンドレットは、作成されたリソースに対する動作を指定には、**する前に**と**後**パラメーター。 このパラメーターが指定されている場合、**アクセス**と**Modified**パラメーターを指定しない必要があります。|
|**正確です**<br>データの種類:SwitchParameter|このパラメーターを実装するは、リソースの用語が指定される場合に一致するリソース名ようにします。 パラメーターが指定されていない場合、リソースの用語と名前は完全に一致する必要ありません。|
|**変更**<br>データの種類:DateTime|指定された日時に基づく指定される場合、コマンドレットは変更されているリソースに作用するために、このパラメーターを実装、**する前に**と**後**パラメーター。 このパラメーターが指定されている場合、**アクセス**と**Created**パラメーターを指定しない必要があります。|
## <a name="see-also"></a>参照

[コマンドレットのパラメーター](./cmdlet-parameters.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
