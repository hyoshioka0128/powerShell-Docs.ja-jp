---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットの属性
description: コマンドレットの属性
ms.openlocfilehash: 6a106f33cb34c6c33b88a981815543bc9af4e4ba
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653522"
---
# <a name="cmdlet-attributes"></a>コマンドレットの属性

Windows PowerShell では、コマンドレットに共通機能を追加するために使用できるいくつかの属性が定義されています。独自のコード内でその機能を実装する必要はありません。 これには、コマンドレットクラスとして Microsoft .NET Framework クラスを識別するコマンドレット属性、コマンドレットによって返される .NET Framework 型を指定する OutputType 属性、コマンドレットパラメーターとしてパブリックプロパティを識別するパラメーター属性などが含まれます。

## <a name="in-this-section"></a>このセクションの内容

[コマンドレットコードの属性](./attributes-in-cmdlet-code.md) コマンドレットコードで属性を使用する利点について説明します。

[属性の型](./attribute-types.md) コマンドレットクラスを修飾できるさまざまな属性について説明します。

[エイリアス属性の宣言](./alias-attribute-declaration.md) コマンドレットパラメーター名のエイリアスを定義する方法について説明します。

[コマンドレットの属性宣言](./cmdlet-attribute-declaration.md) .NET Framework クラスをコマンドレットとして定義する方法について説明します。

[Credential 属性の宣言](./credential-attribute-declaration.md)文字列入力を system.string オブジェクトに変換するためのサポートを追加する方法について説明[します。](/dotnet/api/System.Management.Automation.PSCredential)

[OutputType 属性の宣言](./outputtype-attribute-declaration.md) コマンドレットによって返される .NET Framework 型を指定する方法について説明します。

[パラメーター属性の宣言](./parameter-attribute-declaration.md) コマンドレットのパラメーターを定義する方法について説明します。

[Validatecount 属性の宣言](./validatecount-attribute-declaration.md) パラメーターで許容される引数の数を定義する方法について説明します。

[ValidateLength 属性の宣言](./validatelength-attribute-declaration.md) パラメーター引数の長さ (文字数) を定義する方法について説明します。

[Validatepattern 属性の宣言](./validatepattern-attribute-declaration.md) パラメーター引数の有効なパターンを定義する方法について説明します。

[ValidateRange 属性の宣言](./validaterange-attribute-declaration.md) パラメーター引数の有効な範囲を定義する方法について説明します。

[Validateset 属性の宣言](./validateset-attribute-declaration.md) パラメーター引数に使用できる値を定義する方法について説明します。

## <a name="reference"></a>リファレンス

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
