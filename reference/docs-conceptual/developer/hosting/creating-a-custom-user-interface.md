---
ms.date: 09/13/2016
ms.topic: reference
title: カスタム ユーザー インターフェイスを作成する
description: カスタム ユーザー インターフェイスを作成する
ms.openlocfilehash: 411165f868cd524c0cef0ba9cce3188c60a7dbdf
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645405"
---
# <a name="creating-a-custom-user-interface"></a>カスタム ユーザー インターフェイスを作成する

Windows PowerShell には、Windows PowerShell エンジンをホストするカスタム対話型 UI を作成できる抽象クラスとインターフェイスが用意されています。 カスタム UI を作成するには、 [PSHost](/dotnet/api/System.Management.Automation.Host.PSHost) クラスを実装する必要があります。 必要に応じて、 [Pshostrawuserinterface](/dotnet/api/System.Management.Automation.Host.PSHostRawUserInterface)クラスと Pshostuserinterface クラス、および Ihostsupportsinteractivesession および[](/dotnet/api/System.Management.Automation.Host.PSHostUserInterface)インターフェイスを実装することもできます。と Ihostuisupportsmultiplechoiceselection の各インターフェイスを実装することもできます。 [](/dotnet/api/System.Management.Automation.Host.IHostSupportsInteractiveSession)と[](/dotnet/api/System.Management.Automation.Host.IHostUISupportsMultipleChoiceSelection)のインターフェイスも同様です。
