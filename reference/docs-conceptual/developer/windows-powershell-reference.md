---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell リファレンス
description: Windows PowerShell リファレンス
ms.openlocfilehash: 9c1547ac5ec5134c99aa9213e6aaca1af8d5b3e9
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "94390237"
---
# <a name="windows-powershell-reference"></a>Windows PowerShell リファレンス

Windows PowerShell は Microsoft .NET フレームワークに接続された環境であり、管理の自動化のために設計されています。 Windows PowerShell では、コマンドの作成、ソリューションの作成、およびグラフィカルユーザーインターフェイスベースの管理ツールの作成を行うための新しいアプローチが提供されています。

システム管理者は、Windows PowerShell を使用して、直接またはスクリプトを使用してコマンドを実行することで、システムリソースの管理を自動化することができます。

## <a name="developer-audience"></a>対象となる開発者

Windows PowerShell Software Development Kit (SDK) は、Windows PowerShell によって提供される Api に関する参照情報を必要とするコマンド開発者向けに記述されています。 コマンド開発者は、windows powershell を使用して、Windows PowerShell で実行できるタスクを拡張するコマンドとプロバイダーの両方を作成します。

## <a name="windows-powershell-resources"></a>Windows PowerShell のリソース

Windows PowerShell SDK に加えて、次のリソースについて詳しく説明します。

[Windows PowerShell を使用したはじめに](/powershell/scripting/getting-started/getting-started-with-windows-powershell) Windows PowerShell の概要について説明します。言語、コマンドレット、プロバイダー、およびオブジェクトの使用です。

[Windows PowerShell モジュールの作成](./module/writing-a-windows-powershell-module.md) Windows powershell モジュールを使用して Windows PowerShell ソリューションをパッケージ化して配布する必要がある管理者、スクリプト開発者、コマンドレット開発者向けの情報と例を提供します。

[Windows PowerShell コマンドレットの記述](./cmdlet/writing-a-windows-powershell-cmdlet.md) コマンドレットを設計しているプログラムマネージャーと、コマンドレットコードを実装する開発者のための情報とコード例を提供します。

[Windows PowerShell チームブログ](https://devblogs.microsoft.com/powershell/) 他の Windows PowerShell ユーザーから学習し、コラボレーションするための最適なリソースです。 Windows PowerShell チームのブログを読み、Windows PowerShell ユーザー フォーラム (microsoft.public.windows.powershell) に参加してください。
Windows Live Search を使用して、他の Windows PowerShell のブログとリソースを検索してください。 そして、専門知識を身に付けておくと、自由にアイデアを投稿できます。

[PowerShell モジュールブラウザー](/powershell/module/) には、最新バージョンのコマンドラインヘルプトピックが用意されています。

## <a name="class-libraries"></a>クラスライブラリ

[この名前](/dotnet/api/System.Management.Automation) 空間は、Windows PowerShell のルート名前空間です。 これには、カスタムコマンドレットを実装するために必要なクラス、列挙体、およびインターフェイスが含まれています。 具体的には、すべてのコマンドレットクラスを派生させる必要がある基本クラス[です。](/dotnet/api/System.Management.Automation.Cmdlet) コマンドレットの詳細については、「」を参照してください。

System.servicemodel この名前空間には、Windows PowerShell プロバイダーを実装するために必要なクラス、列挙体、およびインターフェイスが含まれてい[ます。](/dotnet/api/System.Management.Automation.Provider) 特に、 [System.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider) .................................................

[この名前](/dotnet/api/Microsoft.PowerShell.Commands) 空間には、Windows powershell によって実装されるコマンドレットとプロバイダーのクラスが含まれています。 同様に、 *YourName* を作成することをお勧めします。コマンドは、実装するコマンドレットの名前空間です。

[この名前](/dotnet/api/System.Management.Automation.Host) 空間には、ユーザーと Windows PowerShell の間の対話を定義するためにコマンドレットが使用するクラス、列挙体、およびインターフェイスが含まれています。

[この名前](/dotnet/api/System.Management.Automation.Internal) 空間には、他の名前空間クラスで使用される基本クラスが含まれています。 たとえば、システムの[管理](/dotnet/api/System.Management.Automation.Internal.CmdletMetadataAttribute)................. [](/dotnet/api/System.Management.Automation.CmdletAttribute) ...................................

[この名前](/dotnet/api/System.Management.Automation.Runspaces) 空間には、Windows PowerShell 実行空間の作成に使用されるクラス、列挙体、およびインターフェイスが含まれています。 このコンテキストでは、Windows PowerShell 実行空間は、1つ以上の Windows PowerShell パイプラインによってコマンドレットが呼び出されるコンテキストです。 つまり、コマンドレットは、Windows PowerShell 実行空間のコンテキスト内で動作します。 Windows PowerShell 実行空間の詳細については、「 [Windows powershell 実行空間](hosting/creating-runspaces.md)」を参照してください。
