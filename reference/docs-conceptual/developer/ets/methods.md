---
ms.date: 07/09/2020
ms.topic: reference
title: 拡張型システムクラスのメソッド
description: 拡張型システムクラスのメソッド
ms.openlocfilehash: b53604a36e0a0c3587f345262e8f274e3a2c4859
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660380"
---
# <a name="ets-class-methods"></a>クラスのメソッド

メソッドは、引数を受け取り、結果を返す可能性があり、式の左辺には記述できないメンバーです。 内で使用できるメソッドには、コード、Windows PowerShell、スクリプトメソッドなどがあります。

> [!NOTE]
> スクリプトからは、メソッド名の末尾にかっこを追加することで、他のメンバーと同じ構文を使用してメソッドにアクセスします。

## <a name="code-methods"></a>コードメソッド

コードメソッドは、CLR 言語で定義されている拡張メンバーです。 これは、基本オブジェクトで定義されているメソッドと同様の機能を提供します。ただし、コードメソッドは、 **PSObject** オブジェクトに動的に追加される場合があります。 コードメソッドを使用できるようにするには、開発者がプロパティを CLR 言語で記述し、コンパイルして、結果のアセンブリを配布する必要があります。 このアセンブリは、コードメソッドが必要な実行空間で使用できる必要があります。 コードメソッドの実装はスレッドセーフである必要があることに注意してください。 これらのメソッドへのアクセスは、次のパブリックメソッドとプロパティを提供する [Pscodemethod](/dotnet/api/system.management.automation.pscodemethod) オブジェクトを使用して行います。

- `PSCodeMethod.Copy` method: **Pscodemethod** オブジェクトの正確なコピーを作成します。
- `PSCodeMethod.Invoke(System.Object[])` method: 基になるコードメソッドを呼び出します。
- `PSCodeMethod.ToString` method: **Pscodemethod** オブジェクトを文字列に変換します。
- `PSCodeMethod.CodeReference` property: コードメソッドの基になるメソッドを取得します。
- **Psmemberinfo. IsInstance** プロパティ: メンバーのソースを示す **ブール** 値を取得します。
- **Pscodemethod** プロパティ: このメソッドをコードメソッドとして識別する **PSMemberTypes メソッド** 列挙定数を取得します。
- **PSMemberInfo.Name** property: 基になるコードメソッドの名前を取得します。
- **Pscodemethod. OverloadDefinitions** プロパティ: 基になるコードメソッドのすべてのオーバーロードの定義を取得します。
- **Pscodemethod** プロパティ: コードメソッドの完全な名前を取得します。
- **Psmemberinfo. 値** プロパティ: **pscodemethod** オブジェクトを取得します。

## <a name="windows-powershell-methods"></a>Windows PowerShell のメソッド

PowerShell メソッドは、基本オブジェクトで定義された CLR メソッドであるか、アダプターを介してアクセスできるようにします。 これらのメソッドへのアクセスは、次のパブリックメソッドとプロパティを提供する **Psmethod** オブジェクトを介して行われます。

- `PSMethod.Copy` method: **psmethod** オブジェクトの正確なコピーを作成します。
- `PSMethod.Invoke(System.Object[])` method: 基になるメソッドを呼び出します。
- `PSMethod.ToString` method: **Psmethod** オブジェクトを文字列に変換します。
- **Psmemberinfo. IsInstance** プロパティ: メンバーのソースを示す **ブール** 値を取得します。
- **Psmethod** プロパティ: このメソッドを PowerShell メソッドとして識別する **PSMemberTypes** 列挙定数を取得します。
- **PSMemberInfo.Name** property: 基になるメソッドの名前を取得します。
- **Psmethod. OverloadDefinitions** プロパティ: 基になるメソッドのすべてのオーバーロードの定義を取得します。
- **Psmethod** プロパティ: このメソッドの型を取得します。
- **Psmemberinfo. 値** プロパティ: **Psmemberinfo** オブジェクトを取得します。

## <a name="script-methods"></a>スクリプトメソッド

スクリプトメソッドは、PowerShell 言語で定義されている拡張メンバーです。 これは、基本オブジェクトで定義されているメソッドと同様の機能を提供します。ただし、スクリプトメソッドは、 **PSObject** オブジェクトに動的に追加される場合があります。 これらのメソッドへのアクセスは、次のパブリックメソッドとプロパティを提供する [Psscriptmethod](/dotnet/api/system.management.automation.psscriptmethod) オブジェクトを使用して行います。

- `PSScriptMethod.Copy` method: **Psscriptmethod** オブジェクトの正確なコピーを作成します。
- `PSScriptMethod.Invoke(System.Object[])` method: 基になるスクリプトメソッドを呼び出します。
- `PSScriptMethod.ToString` method: **Psscriptmethod** オブジェクトを文字列に変換します。
- **Psmemberinfo. IsInstance** プロパティ: メンバーのソースを示す **ブール** 値を取得します。
- **Psscriptmethod** プロパティ: このメソッドをスクリプトメソッドとして識別する **PSMemberTypes メソッド** 列挙定数を取得します。
- **PSMemberInfo.Name** property: 基になるコードメソッドの名前を取得します。
- **Psscriptmethod. OverloadDefinitions** プロパティ: 基になるスクリプトメソッドのすべてのオーバーロードの定義を取得します。
- **Psscriptmethod** プロパティ: このメソッドの型を取得します。
- **Psscriptmethod. スクリプト** プロパティ: メソッドを呼び出すために使用するスクリプトを取得します。
- **Psmemberinfo. 値** プロパティ: **psscriptmethod** オブジェクトを取得します。
