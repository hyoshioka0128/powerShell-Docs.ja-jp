---
title: 付録 A - ヘルプの構文
description: この記事では、Get-Help によって表示されるコマンドレットの構文を読んで理解する方法について説明します。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: b8fe218f2a9af1ad1ee6b88740414ecede0194bd
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99605427"
---
# <a name="appendix-a---help-syntax"></a>付録 A - ヘルプの構文

次の例は、`Get-EventLog` コマンドレットのヘルプの **SYNTAX** セクションを表示する方法を示しています。

```powershell
help Get-EventLog
```

```Output
NAME
    Get-EventLog

SYNOPSIS
    Gets the events in an event log, or a list of the event logs, on the local or remote
    computers.


SYNTAX
    Get-EventLog [-LogName] <String> [[-InstanceId] <Int64[]>] [-After <DateTime>]
    [-AsBaseObject] [-Before <DateTime>] [-ComputerName <String[]>] [-EntryType {Error |
    Information | FailureAudit | SuccessAudit | Warning}] [-Index <Int32[]>] [-Message
    <String>] [-Newest <Int32>] [-Source <String[]>] [-UserName <String[]>]
    [<CommonParameters>]

    Get-EventLog [-AsString] [-ComputerName <String[]>] [-List] [<CommonParameters>]
```

この例では、ヘルプの該当する部分のみを示しています。

構文は基本的に複数の左角かっこと右角かっこ (`[]`) で構成されています。 その意味は 2 つあり、使用方法に応じて異なります。 角かっこに含まれるものは、空の角かっこ `[]` を除いて、すべて省略可能です。 空の角かっこは、データ型の後にのみ表示されます (`<string[]>` など)。 これは、特定のパラメーターが、その型の値を複数受け入れることができることを意味します。

`Get-EventLog` の最初のパラメーター セットの最初のパラメーター は **LogName** です。 LogName は角かっこで囲まれており、位置指定パラメーターであることを意味します。 つまり、パラメーターが正しい位置に指定されていれば、パラメーター自体の名前は指定しなくても構いません。 パラメーター名の後の山かっこ (`<>`) の情報は、1 つの **文字列** 値が必要であることを示します。 パラメーター名とデータ型全体が角かっこで囲まれていないので、このパラメーター セットを使用する場合は、**LogName** パラメーターが必要です。

```powershell
Get-EventLog [-LogName] <String>
```

2 番目のパラメーターは **InstanceId** です。 パラメーター名とデータ型の両方が角かっこで囲まれていることに注意してください。 これは、**InstanceId** パラメーターが省略可能で、必須ではないことを意味します。 また、**InstanceId** だけが自身の角かっこで囲まれていることにも注意してください。 **LogName** パラメーターと同様、これは位置指定パラメーターであることを意味します。 データ型の後ろには、1 組の角かっこがあります。 これは、配列またはコンマ区切りのリストの形式で、複数の値を受け入れることができることを意味します。

```
[[-InstanceId] <Int64[]>]
```

2 番目のパラメーター セットには **List** パラメーターがあります。 パラメーター名の後にデータ型がないため、これはスイッチ パラメーターです。 **List** パラメーターが指定されている場合、値は **True** です。 指定されていない場合、値は **False** です。

```
[-List]
```

コマンドの構文情報は、`Get-Command` コマンドで **Syntax** パラメーターを指定して取得することもできます。 これは私がいつも使っている便利なショートカットです。 これによりコマンドの使用方法をすばやく確認できます。複数のページにわたるヘルプ情報を見る必要はありません。 最終的に情報が必要になった場合は、実際のヘルプ コンテンツに戻って情報を確認します。

```powershell
Get-Command -Name Get-EventLog -Syntax
```

```Output
Get-EventLog [-LogName] <string> [[-InstanceId] <long[]>] [-ComputerName <string[]>] [-Newest <int>]
 [-After <datetime>] [-Before <datetime>] [-UserName <string[]>] [-Index <int[]> ]
 [-EntryType <string[]>] [-Source <string[]>] [-Message <string>] [-AsBaseObject]
 [<CommonParameters>]

Get-EventLog [-ComputerName <string[]>] [-List] [-AsString] [<CommonParameters>]
```

PowerShell でヘルプ システムを使用するほど、さまざまなニュアンスが把握しやすくなり、 いつの間にか使うことに慣れるでしょう。
