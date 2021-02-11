---
ms.date: 09/13/2016
ms.topic: reference
title: 書式のパラメーター
description: 書式のパラメーター
ms.openlocfilehash: 5f970683fedc71b208ff6becad761d94611a91a6
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652821"
---
# <a name="format-parameters"></a>書式のパラメーター

次の表に、データの書式設定または生成に使用されるパラメーターの推奨される名前と機能を示します。

|パラメーター|機能|
|---|---|
|**As**<br>データ型: キーワード|コマンドレットの出力形式を指定するには、このパラメーターを実装します。 たとえば、テキストやスクリプトなどが考えられます。|
|**Binary**<br>データ型: SwitchParameter|コマンドレットがバイナリ値を処理することを示すには、このパラメーターを実装します。|
|**[エンコード]**<br>データ型: キーワード|サポートされているエンコードの種類を指定するには、このパラメーターを実装します。 たとえば、ASCII、UTF8、Unicode、UTF7、BigEndianUnicode、Byte、String などの値を指定できます。|
|**改行**<br>データ型: SwitchParameter|パラメーターを指定したときに改行文字がサポートされるように、このパラメーターを実装します。|
|**ShortName**<br>データ型: SwitchParameter|パラメーターを指定したときに短い名前がサポートされるように、このパラメーターを実装します。|
|**Width**<br>データ型: Int32|ユーザーが出力デバイスの幅を指定できるように、このパラメーターを実装します。|
|**ラップ**<br>データ型: SwitchParameter|パラメーターを指定したときにテキストの折り返しがサポートされるように、このパラメーターを実装します。|
## <a name="see-also"></a>参照

[コマンドレットのパラメーター](./cmdlet-parameters.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
