---
ms.date: 09/12/2016
ms.topic: reference
title: ヘルプ ファイルに名前を付ける
description: ヘルプ ファイルに名前を付ける
ms.openlocfilehash: b77af8f9b9510785a4198fed9da1263184a27b99
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667591"
---
# <a name="naming-help-files"></a>ヘルプ ファイルに名前を付ける

このトピックでは、 [get-help](/powershell/module/Microsoft.PowerShell.Core/Get-Help) コマンドレットで検索できるように、XML ベースのヘルプファイルに名前を指定する方法について説明します。 名前の要件は、コマンドの種類によって異なります。

## <a name="cmdlet-help-files"></a>コマンドレットのヘルプファイル

C# コマンドレットのヘルプファイルには、コマンドレットが定義されているアセンブリの名前を付ける必要があります。 次のファイル名形式を使用します。

```
<AssemblyName>.dll-help.xml
```

アセンブリが入れ子になっているモジュールの場合でも、アセンブリ名の形式が必要です。

たとえば、Microsoft.PowerShell.Diagnostics.dll アセンブリでは、 [Get WinEvent](/powershell/module/Microsoft.PowerShell.Diagnostics/Get-WinEvent) コマンドレットが定義されています。 `Get-Help`コマンドレットは、 `Get-WinEvent` モジュールディレクトリ内のファイルでのみ、コマンドレットのヘルプトピックを検索し `Microsoft.PowerShell.Diagnostics.dll-help.xml` ます。

## <a name="provider-help-files"></a>プロバイダーヘルプファイル

PowerShell プロバイダーのヘルプファイルには、プロバイダーが定義されているアセンブリの名前を付ける必要があります。 次のファイル名形式を使用します。

`<AssemblyName>.dll-help.xml`

アセンブリが入れ子になっているモジュールの場合でも、アセンブリ名の形式が必要です。

たとえば、証明書プロバイダーはアセンブリで定義されてい `Microsoft.PowerShell.Security.dll` ます。 この `Get-Help` コマンドレットは、モジュールディレクトリ内のファイルでのみ、証明書プロバイダーのヘルプトピックを検索し `Microsoft.PowerShell.Security.dll-help.xml` ます。

## <a name="function-help-files"></a>関数のヘルプファイル

関数は、 [コメントベースのヘルプ](/powershell/module/microsoft.powershell.core/about/about_comment_based_help) を使用するか、XML ヘルプファイルに記載されているドキュメントに記載されています。 関数が XML ファイルに記述されている場合、関数は `.ExternalHelp` 、xml ファイルに関数を関連付ける comment キーワードを持つ必要があります。 それ以外の場合、 `Get-Help` コマンドレットはヘルプファイルを見つけることができません。

関数のヘルプファイルの名前に関する技術的な要件はありません。 ただし、関数が定義されているスクリプトモジュールのヘルプファイルには、という名前を指定することをお勧めします。 たとえば、次の関数はファイルで定義されてい `MyModule.psm1` ます。

```csharp
#.ExternalHelp MyModule.psm1-help.xml
function Test-Function { ... }
```

## <a name="cim-command-help-files"></a>CIM コマンドのヘルプファイル

Cim コマンドのヘルプファイルには、CIM コマンドが定義されている CDXML ファイルの名前を付ける必要があります。 次のファイル名形式を使用します。

`<FileName>.cdxml-help.xml`

CIM コマンドは、モジュールに入れ子になったモジュールとして含めることができる、CDXML ファイルで定義されています。 CIM コマンドが関数としてセッションにインポートされると、PowerShell によって関数定義に comment キーワードが追加され、この関数 `.ExternalHelp` は、cim コマンドが定義されている CDXML ファイルの名前が付けられた XML ヘルプファイルに関連付けられます。

## <a name="script-workflow-help-files"></a>スクリプトワークフローのヘルプファイル

モジュールに含まれているスクリプトワークフローは、XML ベースのヘルプファイルに記載されています。 ヘルプファイルの名前に関する技術的な要件はありません。 ただし、スクリプトワークフローが定義されているスクリプトモジュールのヘルプファイルには、という名前を指定することをお勧めします。 次に例を示します。

`<ScriptModule>.psm1-help.xml`

スクリプト化された他のコマンドとは異なり、スクリプトワークフローでは、 `.ExternalHelp` コメントキーワードを使用してヘルプファイルに関連付ける必要はありません。 代わりに、PowerShell は、モジュールディレクトリの UI カルチャ固有のサブディレクトリで XML ベースのヘルプファイルを検索し、すべてのファイルでスクリプトワークフローのヘルプを検索します。 `.ExternalHelp` comment キーワードは無視されます。

`.ExternalHelp`Comment キーワードは無視されるため、 `Get-Help` コマンドレットは、モジュールに含まれている場合にのみスクリプトワークフローのヘルプを見つけることができます。
