---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell モジュールを記述する
description: Windows PowerShell モジュールを記述する
ms.openlocfilehash: 307241f0fb4d12c1a5cbd651a0ae4d5303098b27
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659423"
---
# <a name="writing-a-windows-powershell-module"></a>Windows PowerShell モジュールを記述する

このドキュメントは、Windows PowerShell コマンドレットをパッケージ化して配布する必要がある管理者、スクリプト開発者、およびコマンドレットの開発者を対象に書かれています。 Windows PowerShell モジュールを使用すると、コンパイルされた言語を使用せずに、Windows PowerShell ソリューションをパッケージ化して配布することができます。

Windows PowerShell モジュールを使用すると、Windows PowerShell コードをパーティション分割し、自己完結型の再利用可能な単位にまとめることができます。 これらの再利用可能なユニットを使用すると、モジュールを他のユーザーと直接簡単に共有できます。 スクリプト開発者は、サードパーティのモジュールを再パッケージ化して、カスタムスクリプトベースのアプリケーションを作成することもできます。 モジュールは、Perl や Python などの他のスクリプト言語のモジュールに似ており、再頒布可能な再頒布可能コンポーネントを使用する実稼働対応のスクリプトソリューションを実現し、複数のコンポーネントを再パッケージ化および抽象化してカスタムソリューションを作成できるようになりました。

基本的には、Windows PowerShell は、hbase-runner.psm1 ファイルに保存されている有効な Windows PowerShell スクリプトコードをモジュールとして扱います。 また、PowerShell は、バイナリコマンドレットアセンブリをモジュールとして自動的に処理します。 ただし、モジュール (具体的にはモジュールマニフェスト) を使用して、ソリューション全体をまとめてバンドルすることもできます。 次のシナリオでは、Windows PowerShell モジュールの一般的な使用方法について説明します。

### <a name="libraries"></a>ライブラリ

モジュールを使用すると、一般的なタスクを実行する関数のまとまりのあるライブラリをパッケージ化して配布することができます。 通常、これらの関数の名前は、使用される一般的なタスクを反映する1つまたは複数の名詞を共有します。 これらの関数は、パブリックメンバーとプライベートメンバーを持つことができるという点で .NET Framework クラスに似ている場合もあります。 たとえば、ライブラリには、ファイル転送用の一連の関数を含めることができます。 この場合、共通タスクを反映する名詞が "file" である可能性があります。

### <a name="configuration"></a>構成

モジュールを使用して、特定のコマンドレット、プロバイダー、関数、および変数を追加することによって環境をカスタマイズできます。

### <a name="compiled-code-development-and-distribution"></a>コンパイル済みのコードの開発と配布

コマンドレットとプロバイダーの開発者は、モジュールを使用して、スナップインを作成しなくても、コンパイル済みのコードをテストおよび配布できます。これにより、スナップインの作成と登録を必要とせずに、コンパイルされたコードを含むアセンブリをモジュール (バイナリモジュール) としてインポートできます。

## <a name="see-also"></a>参照

[Windows PowerShell モジュールを理解する](./understanding-a-windows-powershell-module.md)

[PowerShell スクリプト モジュールを記述する方法](./how-to-write-a-powershell-script-module.md)

[PowerShell バイナリ モジュールを記述する方法](./how-to-write-a-powershell-binary-module.md)

[PowerShell モジュール マニフェストを記述する方法](how-to-write-a-powershell-module-manifest.md)

[PSModulePath インストール パスを変更する](./modifying-the-psmodulepath-installation-path.md)

[PowerShell モジュールをインポートする](./importing-a-powershell-module.md)

[PowerShell モジュールをインストールする](./installing-a-powershell-module.md)
