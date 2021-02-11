---
ms.date: 07/09/2020
ms.topic: reference
title: 拡張型システムプロパティ
description: 拡張型システムプロパティ
ms.openlocfilehash: cccd3c400a8822ee25c44e8e1625e35571420259
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92646128"
---
# <a name="ets-properties"></a>ETS のプロパティ

プロパティは、プロパティとして扱うことができるメンバーです。 基本的に、式の左側に表示できます。 使用できるプロパティには、alias、code、note、および script の各プロパティがあります。

## <a name="alias-property"></a>Alias プロパティ

エイリアスプロパティは、 **PSObject** オブジェクトに含まれている別のプロパティを参照するプロパティです。 これは主に、参照されるプロパティの名前を変更するために使用されます。 ただし、参照されるプロパティの値を別の型に変換する場合にも使用できます。 この型のプロパティは常に、 [PSAliasProperty](/dotnet/api/system.management.automation.psaliasproperty) クラスによって定義されている拡張メンバーであるため、 クラスには、次のプロパティが含まれています。

- **ConversionType** property: 参照されるメンバーの値を変換するために使用される CLR 型。
- **IsGettable** property: 参照されたプロパティの値を取得できるかどうかを示します。
  このプロパティは、参照されるプロパティの **IsGettable** プロパティを調べることによって動的に決定されます。
- **Issettable** 可能プロパティ: 参照されたプロパティの値を設定できるかどうかを示します。 このプロパティは、参照されるプロパティの **issettable** 可能なプロパティを調べることによって動的に決定されます。
- **MemberType** property: エイリアスプロパティとしてこのプロパティを定義する別名 **プロパティ** 列挙定数です。
- **Referencedmembername** プロパティ: このエイリアスが参照する参照されるプロパティの名前。
- **TypeNameOfValue** property: 参照されるプロパティの値の CLR 型の完全な名前。
- **Value** プロパティ: 参照されるプロパティの値。

## <a name="code-property"></a>Code プロパティ

コードプロパティは、CLR 言語で定義されている getter および setter であるプロパティです。 開発者は、コードプロパティを使用できるようにするために、プロパティを CLR 言語で記述し、コンパイルして、結果のアセンブリを配布する必要があります。 このアセンブリは、コードプロパティが必要な実行空間で使用できる必要があります。 この型のプロパティは常に拡張メンバーであり、 [Pscodeproperty](/dotnet/api/system.management.automation.pscodeproperty) クラスによって定義されます。 クラスには、次のプロパティが含まれています。

- **Gettercodereference 参照** プロパティ: コードプロパティの値を取得するために使用されるメソッド。
- **IsGettable** property: コードプロパティの値を取得できるかどうかを示します。 **Settercodereference 参照** プロパティは、コードプロパティの値を設定するために使用されるメソッドです。
- **Issettable** 可能プロパティ: **Settercodereference 参照** プロパティが null でないことを示す、コードプロパティの値を設定できるかどうかを示します。
- **MemberType** property: このプロパティをコードプロパティとして定義する **codeproperty** 列挙定数です。
- **Settercodereference 参照** プロパティ: コードプロパティの値を取得するために使用されるメソッド。
- **TypeNameOfValue** property: properties get 操作によって返されるコードプロパティ値の CLR 型。
- **Value** プロパティ: code プロパティの値。 このプロパティを取得すると、GetterCodeReference 参照プロパティの getter コードが呼び出され、現在の **PSObject** オブジェクトを渡して、呼び出しによって返された値を返します。 このプロパティが設定されている場合、 **Settercodereference 参照** プロパティのセッターコードが呼び出され、最初の引数として現在の **PSObject** オブジェクトが渡され、2番目の引数として値を設定するために使用されるオブジェクトが渡されます。

## <a name="note-property"></a>Note プロパティ

Note プロパティは、名前と値の組み合わせを持つプロパティです。 この型のプロパティは常に拡張メンバーであり、 [psnoteproperty](/dotnet/api/system.management.automation.psnoteproperty) クラスによって定義されます。 クラスには、次のプロパティが含まれています。

- **IsGettable** property: note プロパティの値を取得できるかどうかを示します。
- **Issettable** 可能なプロパティ: note プロパティの値を設定できるかどうかを示します。
- **MemberType** property: このプロパティを note プロパティとして定義する、注意の **プロパティ** 列挙定数です。
- **TypeNameOfValue** property: note プロパティの get 操作によって返されるオブジェクトの完全修飾型名です。
- **Value**: note プロパティの値。

## <a name="powershell-property"></a>PowerShell プロパティ

PowerShell プロパティは、ベースオブジェクトで定義されるプロパティか、アダプターを介して使用できるようにするプロパティです。 Clr フィールドと CLR プロパティの両方を参照できます。 この型のプロパティは、値の指定に基づいて、基本メンバーまたはアダプターメンバーにすることができ、 [Psproperty](/dotnet/api/system.management.automation.psproperty) クラスによって定義されます。 クラスには、次のプロパティが含まれています。

- **IsGettable** property: ベースまたは適用可能なプロパティの値を取得できるかどうかを示します。
- **Issettable** 可能プロパティ: ベースまたは適用可能なプロパティの値を設定できるかどうかを示します。
- **MemberType** property: このプロパティを PowerShell プロパティとして定義するプロパティ列挙定数です。
- **TypeNameOfValue** property: プロパティ値型の完全修飾名。 たとえば、値が文字列であるプロパティの場合、プロパティ値の型は **system.string** です。
- **Value** プロパティ: プロパティの値。 この操作をサポートしていないプロパティで get または set 操作が呼び出されると、 **getvalueexception** または **setvalueexception** 例外がスローされます。

## <a name="powershell-script-property"></a>PowerShell スクリプトのプロパティ

スクリプトプロパティは、getter スクリプトとセッタースクリプトを持つプロパティです。 この型のプロパティは常に拡張メンバーであり、 [Psscriptproperty](/dotnet/api/system.management.automation.psscriptproperty) クラスによって定義されます。 クラスには、次のプロパティが含まれています。

- **Getterscript** プロパティ: スクリプトのプロパティ値を取得するために使用されるスクリプトです。
- **IsGettable** Property: **getterscript** プロパティがスクリプトブロックを公開するかどうかを示します。
- **Issettable** 可能プロパティ: **setterscript** プロパティがスクリプトブロックを公開するかどうかを示します。
- **MemberType** property: このプロパティをスクリプトプロパティとして識別する scriptproperty 列挙定数です。
- **Setterscript** プロパティ: スクリプトのプロパティ値を設定するために使用されるスクリプトです。
- **TypeNameOfValue** property: getter スクリプトによって返されるオブジェクトの完全修飾型名。 この場合、 **system.object** は常に返されます。
- **Value** プロパティ: スクリプトプロパティの値。 Get は getter スクリプトを呼び出し、指定された値を返します。 Set はセッタースクリプトを呼び出します。
