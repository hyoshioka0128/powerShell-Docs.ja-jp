---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットのクラス宣言
description: コマンドレットのクラス宣言
ms.openlocfilehash: 854b0a4ca9f6c87c4fad3b71ee726beade585e02
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653504"
---
# <a name="cmdlet-class-declaration"></a>コマンドレットのクラス宣言

Microsoft .NET Framework クラスは、クラスのメタデータとして **コマンドレット** 属性を指定することで、コマンドレットとして宣言されます。 ( **コマンドレット** 属性は、すべてのコマンドレットに必要な唯一の属性です)。
**コマンドレット** の属性を指定する場合は、コマンドレットを識別する動詞と名詞のペアをユーザーに指定する必要があります。 また、コマンドレットがサポートする Windows PowerShell の機能について説明する必要があります。 **コマンドレット** 属性の指定に使用される宣言構文の詳細については、「[コマンドレット属性の宣言](./cmdlet-attribute-declaration.md)」を参照してください。

> [!NOTE]
> **コマンドレット** 属性は、system.servicemodel [属性](/dotnet/api/System.Management.Automation.CmdletAttribute)クラスによって定義されます。 このクラスのプロパティは、属性を宣言するときに使用される宣言パラメーターに対応します。

## <a name="nouns"></a>名詞

コマンドレットの名詞は、コマンドレットが処理するリソースを指定します。 名詞は、コマンドレットを他のコマンドレットと区別します。

コマンドレット名の名詞は固有である必要があります。また、 *サーバー* などの汎用名詞の場合は、リソースを他の類似リソースと区別する短いプレフィックスを追加することをお勧めします。 たとえば、というプレフィックスを持つ名詞を含むコマンドレット名は、 `Get-SQLServer` です。 特定の名詞とより一般的な動詞を組み合わせることで、ユーザーはコマンドレットをアクションですばやく検索し、不要なコマンドレット名の重複を回避して、コマンドレットをリソースで識別できます。

コマンドレット名に使用できない特殊文字の一覧については、「 [必要な開発ガイドライン](./required-development-guidelines.md)」を参照してください。

## <a name="verbs"></a>動詞

動詞を指定する場合、開発ガイドラインでは、Windows PowerShell によって提供される定義済み動詞のいずれかを使用する必要があります。 これらの定義済み動詞のいずれかを使用すると、作成したコマンドレットと、Microsoft によって作成されたコマンドレットと他のコマンドレットとの間の一貫性が確保されます。 たとえば、データを取得するコマンドレットには、"Get" 動詞を使用します。

動詞のガイドラインの詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。 コマンドレット名に使用できない特殊文字の一覧については、「 [必要な開発ガイドライン](./required-development-guidelines.md)」を参照してください。

## <a name="supporting-windows-powershell-functionality"></a>Windows PowerShell の機能のサポート

**コマンドレット** 属性では、コマンドレットが Windows PowerShell によって提供されるいくつかの一般的な機能をサポートするように指定することもできます。 これには、ユーザーフィードバックの確認 ("ユーザーの操作" 機能のサポートと呼ばれます) などの一般的な機能のサポートと、トランザクションのサポートが含まれます。 (トランザクションのサポートは、Windows PowerShell 2.0 で導入されました)。

**コマンドレット** 属性の指定に使用される宣言構文の詳細については、「[コマンドレット属性の宣言](./cmdlet-attribute-declaration.md)」を参照してください。

## <a name="cmdlet-class-definition"></a>コマンドレットのクラス定義

次のコードは、GetProc cmdlet クラスの定義です。 Pascal の大文字と小文字の区別が使用され、クラスの名前にはコマンドレットの動詞と名詞が含まれていることに注意してください。

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/GetProcessSample01/GetProcessSample01.cs" range="33-34":::

## <a name="pascal-casing"></a>Pascal 形式の文字種

コマンドレットに名前を指定する場合は、Pascal 形式を使用します。 たとえば、コマンドレット `Get-Item` と `Get-ItemProperty` コマンドレットは、コマンドレットに名前を付けるときに大文字小文字を使用する正しい方法を示しています。

## <a name="see-also"></a>参照

[....... の属性](/dotnet/api/System.Management.Automation.CmdletAttribute)

[属性の宣言](./cmdlet-attribute-declaration.md)

[コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
