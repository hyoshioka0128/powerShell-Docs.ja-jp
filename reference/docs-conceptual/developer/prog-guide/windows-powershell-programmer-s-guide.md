---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell プログラマーズ&#39;s ガイド
description: Windows PowerShell プログラマーズ&#39;s ガイド
ms.openlocfilehash: 446be2241bb7b05b4bd7ffe8b7797214a7564944
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355291"
---
# <a name="windows-powershell-programmer39s-guide"></a>Windows PowerShell プログラマーズ&#39;s ガイド

このプログラマーズガイドは、システム管理者向けのコマンドライン管理環境を提供することに関心がある開発者を対象としています。 Windows PowerShell では、.NET オブジェクトを公開する管理コマンドを簡単に構築できます。一方、Windows PowerShell では、ほとんどの作業を行うことができます。

従来のコマンド開発では、パラメーターパーサー、パラメーターバインダー、フィルター、および各コマンドによって公開されるその他のすべての機能を記述する必要があります。 Windows PowerShell には、コマンドを簡単に記述できるように、次の機能が用意されています。

- 独自のパーサーとコマンドパラメーターを自動的にバインドするためのメカニズムを備えた強力な Windows PowerShell ランタイム (実行エンジン)。

- コマンドラインインタープリター (CLI) を使用してコマンドの結果を書式設定および表示するためのユーティリティ。

- 格納されたデータに簡単にアクセスできるようにする (Windows PowerShell プロバイダーを使用した) 高レベルの機能がサポートされています。

  わずかなコストで、管理者に完全なコマンドラインエクスペリエンスを提供する豊富なコマンドまたは一連のコマンドを使用して .NET オブジェクトを表現できます。

  次のセクションでは、Windows PowerShell の基本的な概念と用語について説明します。 開発を開始する前に、これらの概念と用語について理解を深めてください。

## <a name="about-windows-powershell"></a>Windows PowerShell について

Windows PowerShell では、開発で使用できるコマンドの種類がいくつか定義されています。 これらのコマンドには、関数、フィルター、スクリプト、エイリアス、および実行可能ファイル (アプリケーション) があります。 このガイドで説明されている主なコマンドの種類は、"コマンドレット" と呼ばれる単純な小さなコマンドです。 Windows PowerShell は、一連のコマンドレットを提供し、環境に合わせてコマンドレットのカスタマイズを完全にサポートしています。 Windows PowerShell ランタイムは、コマンドレットと同じように、パイプラインを使用して、すべてのコマンドの種類を処理します。

Windows PowerShell では、コマンドに加えて、特定のコマンドレットのセットを提供するさまざまなカスタマイズ可能な Windows PowerShell プロバイダーがサポートされています。 シェルは、Windows PowerShell によって提供されるホストアプリケーション (Windows PowerShell.exe) 内で動作しますが、特定の要件を満たすために開発できるカスタムホストアプリケーションからも同様にアクセスできます。 詳細については、「 [Windows PowerShell のしくみ](/previous-versions//ms714658(v=vs.85))」を参照してください。

### <a name="windows-powershell-cmdlets"></a>Windows PowerShell コマンドレット

コマンドレットは、Windows PowerShell 環境で使用される簡易コマンドです。 Windows PowerShell ランタイムは、コマンドラインで指定されたオートメーションスクリプトのコンテキスト内でこれらのコマンドレットを呼び出します。また、windows powershell ランタイムは、Windows PowerShell Api を使用してプログラムによってそれらのコマンドレットを呼び出します。

コマンドレットの詳細については、「 [Windows PowerShell コマンドレットの記述](../cmdlet/writing-a-windows-powershell-cmdlet.md)」を参照してください。

### <a name="windows-powershell-providers"></a>Windows PowerShell プロバイダー

ユーザーは、管理タスクを実行するときに、データストアに格納されているデータ (ファイルシステム、Windows レジストリ、証明書ストアなど) の確認が必要になる場合があります。 これらの操作を簡単にするために、windows PowerShell では、windows レジストリなどの特定のデータストアにアクセスするために使用できる Windows PowerShell プロバイダーと呼ばれるモジュールを定義しています。 各プロバイダーは、一連の関連コマンドレットをサポートして、ストア内のデータの対称ビューをユーザーに提供します。

Windows PowerShell には、いくつかの既定の Windows PowerShell プロバイダーが用意されています。 たとえば、レジストリプロバイダーは、Windows レジストリのナビゲーションと操作をサポートしています。 レジストリキーは項目として表され、レジストリ値はプロパティとして扱われます。

ユーザーがアクセスする必要があるデータストアを公開する場合は、「 [Windows Powershell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)」で説明されているように、独自の windows powershell プロバイダーを記述することが必要になる場合があります。 Windows PowerShell プロバイダーの詳細については、「 [Windows powershell のしくみ](/previous-versions//ms714658(v=vs.85))」を参照してください。

### <a name="host-application"></a>[ホスト アプリケーション]

Windows PowerShell には、既定のホストアプリケーション powershell.exe が含まれています。これは、ユーザーと対話し、コンソールウィンドウを使用して Windows PowerShell ランタイムをホストするコンソールアプリケーションです。

カスタマイズはサポートされていますが、Windows PowerShell 用に独自のホストアプリケーションを作成する必要が生じることはほとんどありません。 独自のアプリケーションが必要になるケースの1つとして、既定のホストアプリケーションによって提供されるインターフェイスよりも高度な GUI インターフェイスの要件がある場合が挙げられます。 コマンドラインで GUI を使用する場合は、カスタムアプリケーションが必要になることもあります。 詳細については、「 [Windows PowerShell ホストアプリケーションを作成する方法](/powershell/scripting/developer/hosting/writing-a-windows-powershell-host-application)」を参照してください。

### <a name="windows-powershell-runtime"></a>Windows PowerShell ランタイム

Windows PowerShell ランタイムは、コマンド処理を実装する実行エンジンです。 これには、ホストアプリケーションと Windows PowerShell のコマンドとプロバイダーの間のインターフェイスを提供するクラスが含まれています。 Windows PowerShell ランタイムは、現在の Windows PowerShell セッションの実行空間オブジェクトとして実装されます。これは、シェルとコマンドが実行される運用環境です。 操作の詳細については、「 [Windows PowerShell のしくみ](/previous-versions//ms714658(v=vs.85))」を参照してください。

### <a name="windows-powershell-language"></a>Windows PowerShell 言語

Windows PowerShell 言語には、コマンドを呼び出すスクリプト関数とメカニズムが用意されています。 詳細なスクリプト情報については、Windows powershell に付属している Windows PowerShell の言語リファレンスを参照してください。

### <a name="extended-type-system-ets"></a>拡張型システム (ETS)

Windows PowerShell では、.NET や XML オブジェクトなど、さまざまなオブジェクトにアクセスできます。
結果として、すべてのオブジェクトの種類に共通の抽象化を提示するために、シェルは拡張型システム (この) を使用します。 ほとんどの機能はユーザーに対して透過的ですが、スクリプトまたは .NET 開発者は次の目的でこの機能を使用します。

- 特定のオブジェクトのメンバーのサブセットを表示する。 Windows PowerShell には、いくつかの特定のオブジェクトの種類の "調整された" ビューが用意されています。

- 既存のオブジェクトにメンバーを追加しています。

- シリアル化されたオブジェクトへのアクセス。

- カスタマイズされたオブジェクトを書き込んでいます。

  この方法を使用すると、Windows PowerShell 言語と互換性のある柔軟な新しい "型" を作成できます。 .NET 開発者は、Windows PowerShell 言語がスクリプトに適用されるのと同じセマンティクスを使用してオブジェクトを操作できます。たとえば、オブジェクトがに評価されるかどうかを判断し `true` ます。

  Windows PowerShell でのオブジェクトの使用方法と方法の詳細については、「 [Windows Powershell オブジェクトの概念](/powershell/scripting/learn/understanding-important-powershell-concepts)」を参照してください。

## <a name="programming-for-windows-powershell"></a>Windows PowerShell のプログラミング

Windows PowerShell では、.NET Framework を使用して、コマンド、プロバイダー、およびその他のプログラムモジュールのコードを定義します。 Windows PowerShell 用にカスタマイズされたモジュールを作成するための Microsoft Visual Studio の使用に限定されていませんが、このガイドで提供されているサンプルはこのツールで実行することがわかっています。 クラスの継承と属性の使用をサポートする任意の .NET 言語を使用できます。 場合によっては、Windows PowerShell Api では、プログラミング言語がジェネリック型にアクセスできる必要があります。

## <a name="programmers-reference"></a>プログラマーズリファレンス

Windows PowerShell 用に開発する場合の参考資料については、 [Windows POWERSHELL SDK](../windows-powershell-reference.md)を参照してください。

## <a name="getting-started-using-windows-powershell"></a>Windows PowerShell を使用したはじめに

Windows PowerShell シェルの使用を開始する方法の詳細については、「windows powershell に付属している [Windows powershell でのはじめに](/powershell/scripting/getting-started/getting-started-with-windows-powershell) 」を参照してください。 コマンドレットの使用に関する入門資料としても、クイックリファレンスの3折ドキュメントが提供されています。

## <a name="contents-of-this-guide"></a>このガイドの内容

|トピック|定義|
|-----------|----------------|
|[Windows PowerShell プロバイダーを作成する方法](./how-to-create-a-windows-powershell-provider.md)|このセクションでは、windows PowerShell 用の Windows PowerShell プロバイダーを構築する方法について説明します。|
|[Windows PowerShell ホストアプリケーションを作成する方法](/powershell/scripting/developer/hosting/writing-a-windows-powershell-host-application)|このセクションでは、実行空間を操作するホストアプリケーションを記述する方法と、独自のカスタムホストを実装するホストアプリケーションを作成する方法について説明します。|
|[Windows PowerShell スナップインを作成する方法](../cmdlet/how-to-create-a-windows-powershell-snap-in.md)|このセクションでは、アセンブリ内のすべてのコマンドレットとプロバイダーを登録するために使用されるスナップインを作成する方法と、カスタムスナップインを作成する方法について説明します。|
|[コンソール シェルを作成する方法](./how-to-create-a-console-shell.md)|このセクションでは、拡張できないコンソールシェルを作成する方法について説明します。|
|[Windows PowerShell の概念](./windows-powershell-concepts.md)|このセクションには、開発者の視点から Windows PowerShell を理解するのに役立つ概念情報が含まれています。|

## <a name="see-also"></a>参照

[Windows PowerShell SDK](../windows-powershell-reference.md)
