---
ms.date: 09/12/2016
ms.topic: reference
title: コマンドレットのヘルプ トピックに入力の種類を追加する方法
description: コマンドレットのヘルプ トピックに入力の種類を追加する方法
ms.openlocfilehash: f2ad87c54230bcdd7e0ea708e9a1869daef7495f
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "94391104"
---
# <a name="how-to-add-input-types-to-a-cmdlet-help-topic"></a>コマンドレットのヘルプ トピックに入力の種類を追加する方法

ここでは、PowerShell コマンドレットのヘルプトピックに **入力** セクションを追加する方法について説明します。 [ **入力** ] セクションには、コマンドレットがパイプラインからの入力として受け取るオブジェクトの .net クラスが、値またはプロパティ名のいずれかで一覧表示されます。

**入力** セクションに追加できるクラスの数に制限はありません。 入力型はノードで囲まれ `<command:inputTypes>` 、各クラスは要素で囲まれて `<command:inputType>` います。

スキーマには、各要素に2つの要素が含まれてい `<maml:description>` `<command:inputType>` ます。
ただし、この `Get-Help` コマンドレットは要素の内容のみを表示し `<command:inputType>/<maml:description>` ます。

PowerShell 3.0 以降では、 `Get-Help` コマンドレットによって要素の内容が表示され `<maml:uri>` ます。
この要素を使用すると、.NET クラスを説明するトピックにユーザーを誘導できます。

次の XML は、ノードを示して `<maml:inputTypes>` います。

```xml
<command:inputTypes>
  <command:inputType>
    <dev:type>
      <maml:name> Class name </maml:name>
      <maml:uri>  URI of a topic that describes the class </maml:uri>
      <maml:description/>
    </dev:type>
    <maml:description>
      <maml:para> Brief description </maml:para>
    </maml:description>
  </command:inputType>
</command:inputTypes>
```

次の XML は、ノードを使用して入力の種類をドキュメント化する例を示して `<maml:inputTypes>` います。

```xml
<command:inputTypes>
  <command:inputType>
    <dev:type>
      <maml:name>System.DateTime</maml:name>
      <maml:uri>https://docs.microsoft.com/dotnet/api/system.datetime</maml:uri>
      <maml:description/>
    </dev:type>
    <maml:description>
      <maml:para> You can pipe a date to the Set-Date cmdlet. <maml:para>
    <maml:description>
  </command:inputType>
</command:inputTypes>
```
