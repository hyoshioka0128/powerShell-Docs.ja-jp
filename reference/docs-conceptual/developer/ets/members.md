---
ms.date: 07/09/2020
ms.topic: reference
title: 拡張型システムクラスのメンバー
description: 拡張型システムクラスのメンバー
ms.openlocfilehash: 06488ce423f363a285ab53b21ab45989654346dc
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666843"
---
# <a name="extended-type-system-class-members"></a>拡張型システムクラスのメンバー

は、 **PSMemberTypes** 列挙型によって定義される型を持つさまざまな種類のメンバーを参照します。 これらのメンバー型には、それぞれ独自の CLR 型によって定義されるプロパティ、メソッド、メンバー、およびメンバーセットが含まれます。 たとえば、 **""** というプロパティは、独自の **psnoteproperty** 型によって定義されます。 これらの個々の CLR 型には、独自の一意のプロパティと、 [Psmemberinfo クラス](/dotnet/api/system.management.automation.psmemberinfo)から継承される共通プロパティの両方があります。

## <a name="the-psmemberinfo-class"></a>PSMemberInfo クラス

**Psmemberinfo** クラスは、すべてのメンバー型の基底クラスとして機能します。 このクラスは、すべてのメンバー CLR 型に対して次の基本プロパティを提供します。

- **Name** プロパティ: メンバーの名前。 この名前は、適用されるメンバーまたは拡張メンバーが公開されるときに、ベースオブジェクトまたは PowerShell によって定義できます。
- **Value** プロパティ: 特定のメンバーから返される値。 各メンバーの種類は、メンバーの値をどのように処理するかを定義します。
- **TypeNameOfValue** Property: 値プロパティによって返される値の CLR 型の名前です。

## <a name="accessing-members"></a>メンバーへのアクセス

メンバーのコレクションには、 **PSObject** オブジェクトの **members** プロパティ、**メソッド** プロパティ、および **properties** プロパティを使用してアクセスできます。

## <a name="ets-properties"></a>ETS のプロパティ

プロパティは、プロパティとして扱うことができるメンバーです。 基本的に、式の左側に表示できます。 これには、エイリアスプロパティ、コードプロパティ、PowerShell プロパティ、メモプロパティ、およびスクリプトプロパティが含まれます。 これらの種類のプロパティの詳細については、「 [プロパティのプロパティ](properties.md)」を参照してください。

## <a name="ets-methods"></a>ETS のメソッド

メソッドは、引数を受け取り、結果を返す可能性があり、式の左辺には記述できないメンバーです。 これには、コードメソッド、PowerShell メソッド、スクリプトメソッドが含まれます。
これらの種類のメソッドの詳細については、「 [メソッド](methods.md)の使用」を参照してください。
