---
ms.date: 07/09/2020
ms.topic: reference
title: 拡張型システムのエラーと例外
description: 拡張型システムのエラーと例外
ms.openlocfilehash: 295c16ad9abb67b0c4967bf32125bfc7ee0a35da
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652470"
---
# <a name="errors-and-exceptions-in-the-extended-type-system"></a>拡張型システムのエラーと例外

型データの初期化中、 **PSObject** オブジェクトのメンバーにアクセスするとき、または **言語プリミティブ** などのユーティリティクラスの1つを使用するときに、エラーが発生することがあります。

## <a name="runtime-errors"></a>実行時エラー

1つの例外として、キャスト時に、実行時に発生した例外はすべて、 **ExtendedTypeSystemException** 例外または **ExtendedTypeSystemException** クラスから派生した例外のいずれかになります。 これにより、スクリプト開発者は、スクリプト内のステートメントを使用して、これらの例外をトラップでき `Trap` ます。

## <a name="errors-getting-member-values"></a>メンバー値を取得するときのエラー

メンバー (プロパティ、メソッド、またはパラメーター化されたプロパティ) の値を取得するときに発生するすべてのエラーにより、 **Getvalueexception** または **GetValueInvocationException** exception がスローされます。
場合、エラーが発生したことを認識する **Getvalueexception** 例外がスローされます。 参照されるメンバーの基になる getter がエラーが発生したことを認識すると、 **GetValueInvocationException** 例外がスローされ、get 呼び出しエラーの原因となった内部例外が含まれる場合と、含まれない場合があります。

## <a name="errors-setting-member-values"></a>メンバー値の設定エラー

プロパティの値を設定するときに発生するすべてのエラーにより、 **Setvalueexception** または **SetValueInvocationException** 例外がスローされます。 は、エラーが発生したことを認識すると、 **Setvalueexception** 例外がスローされます。 参照されるプロパティの基になる setter がエラーが発生したことを認識すると、 **SetValueInvocationException** 例外がスローされます。この例外は、set 呼び出しエラーの原因となった内部例外を含んでいる場合と、含まれない場合があります。

## <a name="errors-invoking-a-method"></a>メソッドの呼び出しエラー

メソッドの呼び出し時に発生したすべてのエラーによって、 **MethodException** または **MethodInvocationException** 例外がスローされます。 **MethodException** は、エラーが発生したことを認識すると、例外がスローされます。 参照されたメソッドがエラーが発生したことを認識すると、 **MethodInvocationException** 例外がスローされます。これには、呼び出しエラーの原因となった内部例外が含まれる場合と、含まれない場合があります。

## <a name="casting-errors"></a>キャストエラー

無効なキャストが試行されると、 **PSInvalidCastException** がスローされます。 この例外は **InvalidCastException** から派生するため、スクリプトから直接トラップすることはできません。 キャストを試行するエンティティは、スクリプトによってトラップできるように、 **Psruntimeexception** で **PSInvalidCastException** をラップする必要があることに注意してください。 **PSPropertySet**、 **psmemberset** セット、 **Psmemberset**、または **ReadOnlyPSMemberInfoCollection ' 1** のメンバーの値を設定しようとすると、 **NotSupportedException** がスローされます。

## <a name="common-runtime-errors"></a>一般的なランタイムエラー

その他の一般的なランタイムエラーは、 **ExtendedTypeSystemException** exception 型であり、追加の特定の例外の種類はありません。

## <a name="initialization-errors"></a>初期化エラー

の初期化中にエラーが発生する可能性があり `types.ps1xml` ます。 通常、これらのエラーは、PowerShell ランタイムの起動時に表示されます。 ただし、モジュールが読み込まれたときに表示することもできます。
