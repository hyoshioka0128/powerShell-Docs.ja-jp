---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットのヘルプ トピックにコマンドレットの名前と概要を追加する方法
description: コマンドレットのヘルプ トピックにコマンドレットの名前と概要を追加する方法
ms.openlocfilehash: aeeb0cdd1d6d17b88067928ff952dc57a9441917
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659057"
---
# <a name="how-to-add-the-cmdlet-name-and-synopsis-to-a-cmdlet-help-topic"></a>コマンドレットのヘルプ トピックにコマンドレットの名前と概要を追加する方法

このセクションでは、コマンドレットのヘルプの [ **名前** ] セクションと [ **概要** ] セクションに表示されるコンテンツを追加する方法について説明します。 ヘルプファイルでは、各コマンドレットのコマンドノードにこのコンテンツが追加されます。

> [!NOTE]
> ヘルプファイルの完全なビューを表示するには、 `dll-Help.xml` PowerShell インストールディレクトリにあるいずれかのファイルを開きます。 たとえば、ファイルには、 `Microsoft.PowerShell.Commands.Management.dll-Help.xml` いくつかの PowerShell コマンドレットのコンテンツが含まれています。

## <a name="to-add-the-cmdlet-name-and-a-synopsis"></a>コマンドレット名と概要を追加するには

- コマンドレットのヘルプには、コマンドレットの2つの説明が表示されます。 最初の説明は、簡単な説明です。 2番目の説明では、詳細な説明を [コマンドレットヘルプトピックに追加](./how-to-add-a-cmdlet-description.md)する方法について詳しく説明します。
  これらの説明は、両方とも1つの段落として記述する必要があります。

- ここでは、コマンドレット名を繰り返しません。 コマンドレットがサーバーを取得することをユーザーに通知するの `Get-Server` は簡単ですが、情報はありません。 代わりに、シノニムを使用し、詳細を説明に追加します。

  例: "ローカルコンピューターまたはリモートコンピューターを表すオブジェクトを取得します。"

- "Get"、"create"、"change" のような単純な動詞を使用します。 "Set" はあいまいであるため、"modify" などの凝った単語は使用しないでください。

  例: "ファイル内の Authenticode 署名に関する情報を取得します。"

- アクティブな音声で書き込みます。 たとえば、"TimeSpan オブジェクトの使用...""TimeSpan オブジェクトを使用して..." を

- オブジェクトを取得するコマンドレットを記述するときは、動詞 "display" を避けてください。 Windows PowerShell にはコマンドレットのデータが表示されますが、このコマンドレットによって返されるデータが表示されない .NET Framework オブジェクトが返されるという概念をユーザーに紹介することが重要です。 表示を強調すると、コマンドレットで表示されない他の便利なプロパティやメソッドが多数返された可能性があることをユーザーが認識できない可能性があります。

## <a name="see-also"></a>参照

[Windows PowerShell SDK](../windows-powershell-reference.md)
