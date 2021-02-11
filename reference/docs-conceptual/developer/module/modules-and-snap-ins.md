---
ms.date: 09/13/2016
ms.topic: reference
title: モジュールとスナップイン
description: モジュールとスナップイン
ms.openlocfilehash: de4ff1de9a29b78d7783fe7ed0265f5516db1fb4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92658682"
---
# <a name="modules-and-snap-ins"></a>モジュールとスナップイン

コマンドレットは、(Windows PowerShell 2.0 によって導入された) モジュールまたはスナップインを使用してセッションに追加できます。コマンドレットをセッションに追加すると、ホストアプリケーションまたはコマンドラインで対話形式で実行できます。

次の理由により、セッションにコマンドレットを追加するための配信方法としてモジュールを使用することをお勧めします。

- モジュールを使用すると、コマンドレットが定義されているアセンブリを読み込むことによってコマンドレットを追加できます。 スナップインクラスを実装する必要はありません。

- モジュールを使用すると、変数、関数、スクリプト、型、書式設定ファイルなどの他のリソースを追加できます。

- スナップインは、セッションにコマンドレットとプロバイダーを追加するためにのみ使用できます。

## <a name="see-also"></a>参照

[Windows PowerShell モジュールを記述する](writing-a-windows-powershell-module.md)

[Windows PowerShell コマンドレットの記述](../cmdlet/cmdlet-overview.md)
