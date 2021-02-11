---
ms.date: 07/09/2020
ms.topic: reference
title: 拡張型システム型コンバーター
description: 拡張型システム型コンバーター
ms.openlocfilehash: 0774e9eaae1187162b3d55cc45b902f7411a1f18
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648430"
---
# <a name="ets-type-converters"></a>ETS の型コンバーター

メソッドへの呼び出しが行われるときに、型コンバーターの2つの基本的な型を使用 `LanguagePrimitives.ConvertTo(System.Object, System.Type)` します。 このメソッドが呼び出されると、PowerShell は標準の PowerShell 言語コンバーターまたはカスタムコンバーターを使用して、型変換を実行しようとします。 PowerShell が変換を実行できない場合は、 **PSInvalidCastException** 例外がスローされます。

## <a name="standard-windows-powershell-language-converters"></a>標準の Windows PowerShell 言語コンバーター

これらの標準変換は、カスタム変換の前にチェックされ、オーバーライドすることはできません。 次の表は、メソッドが呼び出されたときに PowerShell によって実行される型変換を示して `ConvertTo(System.Object, System.Type)` います。 **ValueToConvert** パラメーターと **resultType** パラメーターへの参照は、メソッドのパラメーターを参照することに注意してください `ConvertTo(System.Object,System.Type)` 。

| From (valueToConvert) |  To (resultType)  |                                                                               戻り値                                                                               |
| --------------------- | ----------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [Null]                  | String            | ""                                                                                                                                                                  |
| [Null]                  | Char              | '\0'                                                                                                                                                                |
| [Null]                  | 数値           | `0`**resultType** パラメーターで指定された型の。                                                                                                          |
| [Null]                  | ブール型           | メソッドの呼び出しの結果 `IsTrue(System.Object)(Null)` 。                                                                                                        |
| [Null]                  | PSObject          | **PSObject** 型の新しいオブジェクト。                                                                                                                                    |
| [Null]                  | 非値型    | Null。                                                                                                                                                               |
| [Null]                  | Null 値を許容する &lt; T&gt; | Null。                                                                                                                                                               |
| 派生クラス         | 基底クラス        | **valueToConvert**                                                                                                                                                  |
| 何か              | Void              | **Automationnull.value**                                                                                                                                            |
| 何か              | String            | `ToString`機構を呼び出します。                                                                                                                                         |
| 何か              | ブール型           | `IsTrue(System.Object) (valueToConvert)`                                                                                                                            |
| 何か              | PSObject          | メソッドの呼び出しの結果 `AsPSObject(System.Object) (valueToConvert)` 。                                                                                         |
| 何か              | Xml ドキュメント      | **ValueToConvert** を文字列に変換し、 **XMLDocument** コンストラクターを呼び出します。                                                                                      |
| Array                 | Array             | 配列の各要素の変換を試みます。                                                                                                                      |
| シングルトン             | Array             | `Array[0]` は、配列の要素型に変換される **valueToConvert** と等しくなります。                                                                            |
| IDictionary           | ハッシュテーブル        | Hashtable (valueToConvert) の呼び出しの結果。                                                                                                                       |
| String                | Char[]            | `valueToConvert.ToCharArray`                                                                                                                                        |
| String                | RegEx             | の呼び出しの結果 `Regx(valueToConvert)` 。                                                                                                                          |
| String                | Type              | **ValueToConvert** パラメーターを使用して **RunspaceConfiguration** を検索し、適切な型を返します。                                                 |
| String                | 数値           | **ValueToConvert** が "" の場合、 `0` **resultType** のを返します。 それ以外の場合は、カルチャ "culture 不変" を使用して数値が生成されます。                       |
| Integer               | System.Enum       | 整数が列挙体によって定義されている場合は、整数を定数に変換します。 整数が定義されていない場合は、 **PSInvalidCastException** 例外がスローされます。 |

## <a name="custom-conversions"></a>カスタム変換

PowerShell で標準の PowerShell 言語コンバーターを使用して型を変換できない場合は、カスタムコンバーターがあるかどうかがチェックされます。 PowerShell は、このセクションで説明されている順序でいくつかの種類のカスタムコンバーターを検索します。 **ValueToConvert** パラメーターと **resultType** パラメーターへの参照は、メソッドのパラメーターを参照することに注意してください `ConvertTo(System.Object, System.Type)` 。 カスタムコンバーターが例外をスローした場合、それ以降はオブジェクトの変換は行われず、その例外は **PSInvalidCastException** 例外でラップされ、その例外がスローされます。

## <a name="powershell-type-converter"></a>PowerShell 型コンバーター

PowerShell 型コンバーターは、単一の型または型のファミリ ( **system.enum** クラスから派生したすべての型など) を変換するために使用されます。 PowerShell 型コンバーターを作成するには、PSTypeConverter クラスを実装し、その実装をターゲットクラスに関連付ける必要があります。 PowerShell 型コンバーターとそのターゲットクラスを関連付けるには、2つの方法があります。

- 種類の構成ファイルを使用する
- **TypeConverterAttribute** 属性をターゲットクラスに適用する

[PSTypeConverter](/dotnet/api/system.management.automation.pstypeconverter)抽象クラスから派生した PowerShell 型コンバーターは、オブジェクトを特定の型または特定の型に変換するためのメソッドを提供します。 **ValueToConvert** パラメーターに、powershell 型コンバーターが関連付けられているオブジェクトが含まれている場合、powershell は`PSTypeConverter.ConvertTo(System.Object, System.Type,System.IFormatProvider, System.Boolean)`
オブジェクトを **resultType** パラメーターによって指定された型に変換する、関連付けられたコンバーターのメソッド。 **ResultType** パラメーターが、powershell 型コンバーターが関連付けられている型を参照している場合、powershell はを呼び出します。`PSTypeConverter.ConvertFrom(System.Object,System.Type, System.IFormatProvider, System.Boolean)`
**resultType** パラメーターによって指定された型からオブジェクトを変換する、関連付けられたコンバーターのメソッド。

## <a name="system-type-converter"></a>システム型コンバーター

システム型コンバーターは、特定のターゲットクラスを変換するために使用されます。 この種類のコンバーターを使用して、クラスのファミリを変換することはできません。 システム型コンバーターを作成するには、 [TypeConverter](/dotnet/api/system.management.automation.runspaces.typedata.typeconverter#System_Management_Automation_Runspaces_TypeData_TypeConverter) クラスを実装し、その実装をターゲットクラスに関連付ける必要があります。 システム型コンバーターとそのターゲットクラスを関連付けるには、2つの方法があります。

- 種類の構成ファイルを使用する
- TypeConverterAttribute 属性をターゲットクラスに適用する

## <a name="parse-converter"></a>解析コンバーター

**ValueToConvert** パラメーターが文字列であり、 **resultType** パラメーターのオブジェクト型にメソッドがある場合 `Parse` 、 `Parse` メソッドが呼び出されて文字列が変換されます。

## <a name="constructor-converter"></a>コンストラクターコンバーター

**ResultType** パラメーターのオブジェクトの型に、 **valueToConvert** パラメーターのオブジェクトと同じ型の単一のパラメーターを持つコンストラクターがある場合、このコンストラクターが呼び出されます。

## <a name="implicit-cast-operator-converter"></a>暗黙のキャスト演算子コンバーター

**ValueToConvert** パラメーターに、 **resultType** に変換する暗黙的なキャスト演算子がある場合、その cast 演算子が呼び出されます。 **ResultType** パラメーターに **valueToConvert** から変換する暗黙的なキャスト演算子がある場合、その cast 演算子が呼び出されます。

## <a name="explicit-cast-operator-converter"></a>明示的なキャスト演算子コンバーター

**ValueToConvert** パラメーターに明示的なキャスト演算子があり、 **resultType** に変換する場合、その cast 演算子が呼び出されます。 **ResultType** パラメーターに **valueToConvert** から変換する明示的なキャスト演算子がある場合、その cast 演算子が呼び出されます。
