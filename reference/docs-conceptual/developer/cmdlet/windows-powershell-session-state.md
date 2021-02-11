---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell セッション状態
description: Windows PowerShell セッション状態
ms.openlocfilehash: 51de92f1f392f708cf49c7ccb4a6808fd628076c
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668135"
---
# <a name="windows-powershell-session-state"></a>Windows PowerShell セッション状態

セッション状態とは、Windows PowerShell セッションまたはモジュールの現在の構成を指します。 Windows PowerShell セッションは、コマンドラインユーザーが対話形式で、またはホストアプリケーションによってプログラムによって使用される運用環境です。 セッションのセッション状態は、グローバルセッション状態と呼ばれます。

開発者の観点から見ると、Windows PowerShell セッションとは、ホストアプリケーションが Windows PowerShell の実行空間を開いて実行空間を閉じるまでの時間を指します。 もう1つの方法として、セッションは、実行空間が存在する間に呼び出される Windows PowerShell エンジンのインスタンスの有効期間です。

## <a name="module-session-state"></a>モジュールセッション状態

モジュールセッション状態は、モジュールまたは入れ子になったモジュールのいずれかがセッションにインポートされるたびに作成されます。 モジュールがコマンドレット、関数、またはスクリプトなどの要素をエクスポートすると、その要素への参照がセッションのグローバルセッション状態に追加されます。 ただし、要素が実行されると、モジュールのセッション状態内で実行されます。

## <a name="session-state-data"></a>データの Session-State

セッション状態データは、パブリックまたはプライベートにすることができます。 パブリックデータは、セッション状態の外部からの呼び出しに使用できます。プライベートデータは、セッション状態内からの呼び出しに対してのみ使用できます。 たとえば、モジュールは、モジュールによってのみ呼び出すことができるプライベート関数を持つことができます。または、エクスポートされたパブリック要素によって内部的にのみ呼び出すことができます。 これは、.NET Framework 型のプライベートメンバーとパブリックメンバーに似ています。

セッション状態データは、現在の Windows PowerShell セッションのコンテキスト内で、実行エンジンの現在のインスタンスによって格納されます。 セッション状態データは、次の項目で構成されます。

- パス情報

- ドライブ情報

- Windows PowerShell プロバイダーの情報

- モジュールによってエクスポートされるモジュール要素 (コマンドレット、関数、スクリプトなど) へのインポートされたモジュールおよび参照に関する情報。 この情報とこれらの参照は、グローバルセッション状態のみを対象としています。

- セッション状態変数の情報

## <a name="accessing-session-state-data-within-cmdlets"></a>コマンドレット内の Session-State データへのアクセス

コマンドレットは、コマンドレットクラスの [Sessionstate *](/dotnet/api/System.Management.Automation.PSCmdlet.SessionState) プロパティを介して、または [Sessionstate](/dotnet/api/System.Management.Automation.SessionState) クラスを直接介して、セッション状態データに間接的にアクセスできます。 [Sessionstate](/dotnet/api/System.Management.Automation.SessionState)クラスには、さまざまな種類のセッション状態データを調査するために使用できるプロパティが用意されています。

## <a name="see-also"></a>参照

[PSCmdlet. Sessionstate (システムの管理)](/dotnet/api/System.Management.Automation.PSCmdlet.SessionState)

[Sessionstate ですか?Displayproperty = Fullname](/dotnet/api/System.Management.Automation.SessionState)

[Windows PowerShell コマンドレット](./cmdlet-overview.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[Windows PowerShell シェル SDK](../windows-powershell-reference.md)
