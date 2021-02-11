---
description: PowerShell の動作をカスタマイズする変数。
Locale: en-US
ms.date: 04/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_preference_variables?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Preference_Variables
ms.openlocfilehash: d8eadf88d486de4758b56738089f27e8adc3bc91
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600089"
---
# <a name="about-preference-variables"></a>ユーザー設定変数について

## <a name="short-description"></a>簡単な説明

PowerShell の動作をカスタマイズする変数。

## <a name="long-description"></a>長い説明

PowerShell には、その動作をカスタマイズできるようにする一連の変数が含まれています。 これらのユーザー設定変数は、GUI ベースのシステムのオプションと同様に機能します。

ユーザー設定変数は PowerShell オペレーティング環境に影響し、すべてのコマンドは環境内で実行されます。 多くの場合、コマンドレットには、特定のコマンドの基本設定の動作をオーバーライドするために使用できるパラメーターがあります。

次の表に、ユーザー設定変数とその既定値を示します。

|             変数             |       Default value       |
| -------------------------------- | ------------------------- |
| `$ConfirmPreference`             | 高                      |
| `$DebugPreference`               | SilentlyContinue          |
| `$ErrorActionPreference`         | Continue                  |
| `$ErrorView`                     | ConciseView               |
| `$FormatEnumerationLimit`        | 4                         |
| `$InformationPreference`         | SilentlyContinue          |
| `$LogCommandHealthEvent`         | False (ログに記録されません)        |
| `$LogCommandLifecycleEvent`      | False (ログに記録されません)        |
| `$LogEngineHealthEvent`          | True (ログ記録)             |
| `$LogEngineLifecycleEvent`       | True (ログ記録)             |
| `$LogProviderLifecycleEvent`     | True (ログ記録)             |
| `$LogProviderHealthEvent`        | True (ログ記録)             |
| `$MaximumHistoryCount`           | 4096                      |
| `$OFS`                           | (スペース文字 ( `" "` )) |
| `$OutputEncoding`                | UTF8Encoding オブジェクト       |
| `$ProgressPreference`            | 続行                  |
| `$PSDefaultParameterValues`      | (なし-空のハッシュテーブル) |
| `$PSEmailServer`                 | (なし)                    |
| `$PSModuleAutoLoadingPreference` | すべて                       |
| `$PSSessionApplicationName`      | wsman                     |
| `$PSSessionConfigurationName`    | `http://schemas.microsoft.com/powershell/Microsoft.PowerShell` |
| `$PSSessionOption`               | 「」を参照してください [$PSSessionOption](#pssessionoption) |
| `$Transcript`                    | (なし)                    |
| `$VerbosePreference`             | SilentlyContinue          |
| `$WarningPreference`             | Continue                  |
| `$WhatIfPreference`              | False                     |

PowerShell には、ユーザー設定を格納する次の環境変数が含まれています。 これらの環境変数の詳細については、「 [about_Environment_Variables](about_Environment_Variables.md)」を参照してください。

- `env:PSExecutionPolicyPreference`
- `$env:PSModulePath`

> [!NOTE]
> 優先変数に加えた変更は、これらのスクリプトまたは関数が、preference が使用されたスコープと同じスコープで定義されている場合にのみ、スクリプトと関数で有効になります。 詳細については、「 [about_Scopes](about_Scopes.md)」を参照してください。

## <a name="working-with-preference-variables"></a>ユーザー設定変数の使用

このドキュメントでは、各ユーザー設定変数について説明します。

特定のユーザー設定変数の現在の値を表示するには、変数の名前を入力します。 たとえば、次のコマンドは、 `$ConfirmPreference` 変数の値を表示します。

```powershell
 $ConfirmPreference
```

```Output
High
```

変数の値を変更するには、代入ステートメントを使用します。 たとえば、次のステートメントでは、 `$ConfirmPreference` パラメーターの値が **Medium** に変更されます。

```powershell
$ConfirmPreference = "Medium"
```

設定する値は、現在の PowerShell セッションに固有です。 すべての PowerShell セッションで変数を有効にするには、PowerShell プロファイルに変数を追加します。 詳細については、「 [about_Profiles](about_Profiles.md)」を参照してください。

## <a name="working-remotely"></a>リモートでの作業

リモートコンピューターでコマンドを実行する場合、リモートコマンドは、リモートコンピューターの PowerShell クライアントで設定された設定の対象になります。 たとえば、リモートコマンドを実行すると、リモートコンピューターの変数の値によって、 `$DebugPreference` PowerShell がデバッグメッセージに応答する方法が決まります。

リモートコマンドの詳細については、「 [about_Remote](about_Remote.md)」を参照してください。

### <a name="confirmpreference"></a>\$ConfirmPreference

コマンドレットまたは関数を実行する前に、PowerShell によって確認メッセージが自動的に表示されるかどうかを指定します。

`$ConfirmPreference`変数の有効な値は、 **High**、 **Medium**、または **Low** です。 コマンドレットと関数には、 **高**、 **中**、 **低** のリスクが割り当てられています。 変数の値 `$ConfirmPreference` がコマンドレットまたは関数に割り当てられたリスク以下である場合、PowerShell はコマンドレットまたは関数を実行する前に自動的に確認を求めます。

変数の値 `$ConfirmPreference` が **None** の場合は、コマンドレットまたは関数を実行する前に、PowerShell によって自動的にプロンプトが表示されることはありません。

セッション内のすべてのコマンドレットと関数の確認動作を変更するには、 `$ConfirmPreference` 変数の値を変更します。

`$ConfirmPreference`1 つのコマンドのをオーバーライドするには、コマンドレットまたは関数の **Confirm** パラメーターを使用します。 確認を要求するには、を使用 `-Confirm` します。 確認を抑制するには、を使用 `-Confirm:$false` します。

有効な値 `$ConfirmPreference` :

- **なし**: PowerShell は自動的にプロンプトを表示しません。 特定のコマンドの確認を要求するには、コマンドレットまたは関数の **Confirm** パラメーターを使用します。
- [**低**]: PowerShell で、低、中、または高リスクのコマンドレットまたは関数を実行する前に、確認メッセージが表示されます。
- [**中**]: PowerShell でコマンドレットまたは関数を実行する前に、中程度のリスクがあるかどうかを確認するプロンプトが表示されます。
- **High**: コマンドレットまたは関数を高いリスクで実行する前に、PowerShell に確認を求めるメッセージが表示されます。

#### <a name="detailed-explanation"></a>詳細な説明

PowerShell では、アクションを実行する前に自動的に確認メッセージが表示されます。 たとえば、コマンドレットまたは関数がシステムに大きな影響を及ぼしてデータを削除したり、大量のシステムリソースを使用したりした場合などです。

```powershell
Remove-Item -Path C:\file.txt
```

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "Remove File" on Target "C:\file.txt".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [?] Help (default is "Y"):
```

リスクの推定値は、コマンドレットまたは **Confirmimpact** と呼ばれる関数の属性です。 ユーザーはそれを変更することはできません。

システムへのリスクが生じる可能性のあるコマンドレットと関数には、1つのコマンドの確認を要求または抑制するために使用できる **Confirm** パラメーターがあります。

ほとんどのコマンドレットと関数は、既定のリスク値である **Confirmimpact** を使用し、は **Medium** で、既定値 `$ConfirmPreference` は **high** であるため、自動確認はほとんど発生しません。 ただし、の値を [中] または [低] に変更すると、自動確認を有効にすることができ `$ConfirmPreference` ます。  

#### <a name="examples"></a>例

この例は、 `$ConfirmPreference` 変数の既定値 **High** の効果を示しています。 **高い** 値は、危険度の高いコマンドレットと関数を確認するだけです。 ほとんどのコマンドレットと関数は、危険度が中程度であるため、自動的に確認されず `Remove-Item` 、ファイルが削除されます。 `-Confirm`コマンドにを追加すると、ユーザーに確認を求めるメッセージが表示されます。

```powershell
$ConfirmPreference
```

```Output
High
```

```powershell
Remove-Item -Path C:\temp1.txt
```

`-Confirm`確認を要求するために使用します。

```powershell
Remove-Item -Path C:\temp2.txt -Confirm
```

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "Remove File" on Target "C:\temp2.txt".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All
[?] Help (default is "Y"):
```

次の例は、の値を Medium に変更した場合の効果を示して `$ConfirmPreference` います。  ほとんどのコマンドレットと関数は中程度のリスクであるため、自動的に確認されます。 1つのコマンドに対して確認プロンプトを表示しないようにするには、値がで **Confirm** パラメーターを使用し `$false` ます。

```powershell
$ConfirmPreference = "Medium"
Remove-Item -Path C:\temp2.txt
```

```Output
Confirm
Are you sure you want to perform this action?
Performing operation "Remove File" on Target "C:\temp2.txt".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All
[?] Help (default is "Y"):
```

```powershell
Remove-Item -Path C:\temp3.txt -Confirm:$false
```

### <a name="debugpreference"></a>\$DebugPreference

スクリプト、コマンドレット、またはプロバイダーによって生成されたデバッグメッセージ、またはコマンドラインでコマンドを実行して、PowerShell がどのように応答するかを指定し `Write-Debug` ます。

一部のコマンドレットでは、デバッグメッセージが表示されます。これは通常、プログラマやテクニカルサポートプロフェッショナル向けに設計されたテクニカルメッセージです。 既定では、デバッグメッセージは表示されませんが、の値を変更することで、デバッグメッセージを表示でき `$DebugPreference` ます。

コマンドレットの **Debug** 共通パラメーターを使用して、特定のコマンドのデバッグメッセージを表示または非表示にすることができます。 詳細については、「[about_CommonParameters](about_CommonParameters.md)」を参照してください。

有効な値は次のとおりです。

- [**停止**]: デバッグメッセージを表示し、実行を停止します。 コンソールにエラーを書き込みます。
- **Inquire**: デバッグメッセージを表示し、続行するかどうかを確認します。 **デバッグ** 共通パラメーターをコマンドに追加すると、コマンドがデバッグメッセージを生成するように構成されている場合に、 `$DebugPreference` **Inquire** に対して変数の値が変更されます。
- **Continue**: デバッグメッセージを表示し、実行を続行します。
- **SilentlyContinue**: (既定) 効果はありません。 デバッグメッセージは表示されず、実行は中断されることなく続行されます。

#### <a name="examples"></a>例

次の例は、コマンド `$DebugPreference` `Write-Debug` がコマンドラインで入力されたときのの値を変更した場合の影響を示しています。
この変更は、コマンドレットおよびスクリプトによって生成されるメッセージを含む、すべてのデバッグメッセージに影響します。 この例は、1つのコマンドに関連するデバッグメッセージを表示または非表示にする **Debug** パラメーターを示しています。

この例は、 `$DebugPreference` 変数の既定値 **SilentlyContinue** の効果を示しています。 既定では、 `Write-Debug` コマンドレットのデバッグメッセージは表示されず、処理は続行されます。 **Debug** パラメーターを使用すると、1つのコマンドの設定が上書きされます。 デバッグメッセージが表示されます。

```powershell
$DebugPreference
```

```Output
SilentlyContinue
```

```powershell
Write-Debug -Message "Hello, World"
```

```powershell
Write-Debug -Message "Hello, World" -Debug
```

```Output
DEBUG: Hello, World
```

この例では、 `$DebugPreference` **Continue** 値を使用した場合のの効果を示します。 デバッグメッセージが表示され、コマンドが処理を続行します。

```powershell
$DebugPreference = "Continue"
Write-Debug -Message "Hello, World"
```

```Output
DEBUG: Hello, World
```

この例では、 **Debug** パラメーターを値と共に使用して、 `$false` 1 つのコマンドのメッセージを非表示にします。 デバッグメッセージが表示されません。

```powershell
Write-Debug -Message "Hello, World" -Debug:$false
```

この例は、が `$DebugPreference` **停止** 値に設定された場合の効果を示しています。 デバッグメッセージが表示され、コマンドが停止します。

```powershell
$DebugPreference = "Stop"
Write-Debug -Message "Hello, World"
```

```Output
DEBUG: Hello, World
Write-Debug : The running command stopped because the preference variable
 "DebugPreference" or common parameter is set to Stop: Hello, World
At line:1 char:1
+ Write-Debug -Message "Hello, World"
```

この例では、 **Debug** パラメーターを値と共に使用して、 `$false` 1 つのコマンドのメッセージを非表示にします。 デバッグメッセージは表示されず、処理は停止されません。

```powershell
Write-Debug -Message "Hello, World" -Debug:$false
```

この例は、 `$DebugPreference` **Inquire** 値に設定された効果を示しています。 デバッグメッセージが表示され、ユーザーに確認を求めるメッセージが表示されます。

```powershell
$DebugPreference = "Inquire"
Write-Debug -Message "Hello, World"
```

```Output
DEBUG: Hello, World

Confirm
Continue with this operation?
[Y] Yes  [A] Yes to All  [H] Halt Command  [?] Help (default is "Y"):
```

この例では、 **Debug** パラメーターを値と共に使用して、 `$false` 1 つのコマンドのメッセージを非表示にします。 デバッグメッセージが表示されず、処理が続行されます。

```powershell
Write-Debug -Message "Hello, World" -Debug:$false
```

### <a name="erroractionpreference"></a>\$ErrorActionPreference

PowerShell が終了しないエラーに応答する方法を決定します。このエラーは、コマンドレットの処理を停止しません。 たとえば、コマンドライン、スクリプト、コマンドレット、またはプロバイダー (コマンドレットによって生成されたエラーなど) `Write-Error` です。

コマンドレットの **Erroraction** 共通パラメーターを使用して、特定のコマンドの設定をオーバーライドできます。

有効な値は次のとおりです。

- **Break** -エラーが発生したとき、または例外が発生したときにデバッガーを入力します。
- **続行**: (既定) エラーメッセージを表示し、実行を続行します。
- **Ignore**: エラーメッセージを表示せず、コマンドの実行を続行します。 **Ignore** 値は、保存された設定として使用するのではなく、コマンドごとの使用を目的としています。 **Ignore** は、変数の有効な値ではありません `$ErrorActionPreference` 。
- **Inquire**: エラーメッセージを表示し、続行するかどうかを確認します。
- **SilentlyContinue**: 何の効果もありません。 エラーメッセージは表示されず、実行は中断されることなく続行されます。
- [**停止**]: エラーメッセージを表示し、実行を停止します。 **Stop** 値は、生成されたエラーに加えて、エラーストリームに ActionPreferenceStopException オブジェクトを生成します。
  ストリーム (stream)
- **中断**: ワークフロージョブを自動的に中断し、さらに調査できるようにします。 調査後、ワークフローを再開できます。 **Suspend** の値は、保存された設定としてではなく、コマンドごとの使用を目的としています。 **中断** は、変数の有効な値ではありません `$ErrorActionPreference` 。

`$ErrorActionPreference`**Erroraction** パラメーターは、コマンドレットの処理を停止するエラーを終了するために PowerShell がどのように応答するかに影響しません。 **Erroraction** 共通パラメーターの詳細については、「 [about_CommonParameters](about_CommonParameters.md)」を参照してください。

#### <a name="examples"></a>例

これらの例は、変数の異なる値の効果を示して `$ErrorActionPreference` います。 **Erroraction** パラメーターは、値をオーバーライドするために使用され `$ErrorActionPreference` ます。

この例では、 `$ErrorActionPreference` 既定値の [ **続行**] を示します。 終了しないエラーが生成されます。 メッセージが表示され、処理が続行されます。

```powershell
# Change the ErrorActionPreference to 'Continue'
$ErrorActionPreference = 'Continue'
# Generate a non-terminating error and continue processing the script.
Write-Error -Message  'Test Error' ; Write-Host 'Hello World'
```

```Output
Write-Error: Test Error
Hello World
```

この例は、 `$ErrorActionPreference` 既定値の **Inquire** を示しています。 エラーが生成され、アクションのプロンプトが表示されます。

```powershell
# Change the ErrorActionPreference to 'Inquire'
$ErrorActionPreference = 'Inquire'
Write-Error -Message 'Test Error' ; Write-Host 'Hello World'
```

```Output
Confirm
Test Error
[Y] Yes  [A] Yes to All  [H] Halt Command  [S] Suspend  [?] Help (default is "Y"):
```

この例では、 `$ErrorActionPreference` を **SilentlyContinue** に設定しています。
エラーメッセージは表示されません。

```powershell
# Change the ErrorActionPreference to 'SilentlyContinue'
$ErrorActionPreference = 'SilentlyContinue'
# Generate an error message
Write-Error -Message 'Test Error' ; Write-Host 'Hello World'
# Error message is suppressed and script continues processing
```

```Output
Hello World
```

`$ErrorActionPreference`**停止** するように設定する例を次に示します。 また、変数に対して生成された追加のオブジェクトも表示され `$Error` ます。

```powershell
# Change the ErrorActionPreference to 'Stop'
$ErrorActionPreference = 'Stop'
# Error message is is generated and script stops processing
Write-Error -Message 'Test Error' ; Write-Host 'Hello World'

# Show the ActionPreferenceStopException and the error generated
$Error[0]
$Error[1]
```

```Output
Write-Error: Test Error

ErrorRecord                 : Test Error
WasThrownFromThrowStatement : False
TargetSite                  : System.Collections.ObjectModel.Collection`1[System.Management.Automation.PSObject]
                              Invoke(System.Collections.IEnumerable)
StackTrace                  :    at System.Management.Automation.Runspaces.PipelineBase.Invoke(IEnumerable input)
                                 at Microsoft.PowerShell.Executor.ExecuteCommandHelper(Pipeline tempPipeline,
                              Exception& exceptionThrown, ExecutionOptions options)
Message                     : The running command stopped because the preference variable "ErrorActionPreference" or
                              common parameter is set to Stop: Test Error
Data                        : {System.Management.Automation.Interpreter.InterpretedFrameInfo}
InnerException              :
HelpLink                    :
Source                      : System.Management.Automation
HResult                     : -2146233087

Write-Error: Test Error
```

### <a name="errorview"></a>\$ErrorView

PowerShell でのエラーメッセージの表示形式を決定します。

有効な値は次のとおりです。

- **ConciseView**: (既定値) 簡潔なエラーメッセージと、高度なモジュールビルダーのリファクタリングされたビューを提供します。 エラーがコマンドラインからのものである場合は、単一行のエラーメッセージです。 それ以外の場合は、エラーを含む複数行のエラーメッセージと、その行のどこに発生しているかを示すエラーへのポインターを受け取ります。 ターミナルで仮想端末がサポートされている場合は、ANSI カラーコードを使用してカラーアクセントが提供されます。 アクセントの色はで変更でき `$Host.PrivateData.ErrorAccentColor` ます。 `Get-Error`内部例外を含む完全修飾エラーの包括的な詳細ビューを使用するには、コマンドレットを使用します。

  PowerShell 7 で **ConciseView** が追加されました。

- **Normalview**: ほとんどのユーザー向けに設計された詳細なビューです。 エラーの説明とエラーに関係するオブジェクトの名前で構成されます。

- **カテゴリビュー**: 運用環境向けに設計された簡潔で構造化されたビューです。 その形式は次のとおりです。

  {Category}: ({TargetName}: {TargetType}): [{Activity}], {Reason}

**カテゴリビュー** のフィールドの詳細については、「 [errorのカテゴリ情報](/dotnet/api/system.management.automation.errorcategoryinfo)クラス」を参照してください。

#### <a name="examples"></a>例

この例では、の値が既定の ConciseView である場合に、エラーがどのように表示されるかを示し `$ErrorView` ます。  `Get-ChildItem` は、存在しないディレクトリを検索するために使用されます。

```powershell
Get-ChildItem -path 'C:\NoRealDirectory'
```

```Output
Get-ChildItem: Cannot find path 'C:\NoRealDirectory' because it does not exist.
```

この例では、の値が既定の ConciseView である場合に、エラーがどのように表示されるかを示し `$ErrorView` ます。  `Script.ps1` が実行され、ステートメントからエラーがスローされ `Get-Item` ます。

```powershell
./Script.ps1
```

```Output
Get-Item: C:\Script.ps1
Line |
  11 | Get-Item -Path .\stuff
     | ^ Cannot find path 'C:\demo\stuff' because it does not exist.
```

この例では、の値 `$ErrorView` が **normalview** に変更されたときにエラーがどのように表示されるかを示します。 `Get-ChildItem` は、存在しないファイルを検索するために使用されます。

```powershell
Get-ChildItem -Path C:\nofile.txt
```

```Output
Get-ChildItem : Cannot find path 'C:\nofile.txt' because it does not exist.
At line:1 char:1
+ Get-ChildItem -Path C:\nofile.txt
```

この例は、の値 `$ErrorView` が **カテゴリビュー** に変更された場合に、同じエラーがどのように表示されるかを示しています。

```powershell
$ErrorView = "CategoryView"
Get-ChildItem -Path C:\nofile.txt
```

```Output
ObjectNotFound: (C:\nofile.txt:String) [Get-ChildItem], ItemNotFoundException
```

この例では、の値が `$ErrorView` エラー表示にのみ影響することを示しています。 自動変数に格納されているエラーオブジェクトの構造は変更されません `$Error` 。 自動変数の詳細については `$Error` 、「 [about_automatic_variables](about_Automatic_Variables.md)」を参照してください。

次のコマンドは、エラー配列内の最新のエラーに関連付けられた **Errorrecord** オブジェクトを取得し、 **要素 0** を使用して、リスト内のすべてのエラーオブジェクトのプロパティを書式設定します。

```powershell
$Error[0] | Format-List -Property * -Force
```

```Output
PSMessageDetails      :
Exception             : System.Management.Automation.ItemNotFoundException:
                          Cannot find path 'C:\nofile.txt' because it does
                          not exist.
                        at System.Management.Automation.SessionStateInternal.
                          GetChildItems(String path, Boolean recurse, UInt32
                          depth, CmdletProviderContext context)
                        at System.Management.Automation.ChildItemCmdlet
                          ProviderIntrinsics.Get(String path, Boolean
                          recurse, UInt32 depth, CmdletProviderContext context)
                        at Microsoft.PowerShell.Commands.GetChildItemCommand.
                          ProcessRecord()
TargetObject          : C:\nofile.txt
CategoryInfo          : ObjectNotFound: (C:\nofile.txt:String) [Get-ChildItem],
                          ItemNotFoundException
FullyQualifiedErrorId : PathNotFound,
                          Microsoft.PowerShell.Commands.GetChildItemCommand
ErrorDetails          :
InvocationInfo        : System.Management.Automation.InvocationInfo
ScriptStackTrace      : at <ScriptBlock>, <No file>: line 1
PipelineIterationInfo : {0, 1}
```

### <a name="formatenumerationlimit"></a>\$FormatEnumerationLimit

表示に含める列挙された項目の数を決定します。 この変数は、基になるオブジェクトには影響しません。表示のみです。 の値 `$FormatEnumerationLimit` が列挙された項目の数よりも小さい場合は、 `...` 表示されていない項目を示す省略記号 () が PowerShell によって追加されます。

**有効な値**: 整数 ( `Int32` )

**既定値**: 4

#### <a name="examples"></a>例

この例では、変数を使用して、列挙され `$FormatEnumerationLimit` た項目の表示を改善する方法を示します。

この例のコマンドは、コンピューター上で実行されているすべてのサービスを一覧表示するテーブルを生成します。1つはサービスの **実行** 用で、もう1つは **停止** したサービス用です。 コマンドを使用して `Get-Service` すべてのサービスを取得し、パイプラインを介して結果をコマンドレットに送信し `Group-Object` ます。これにより、結果がサービスの状態別にグループ化されます。

その結果、[ **名前** ] 列の状態と [ **グループ]** 列のプロセスが一覧表示されます。 列ラベルを変更するには、「 [about_Hash_Tables](about_Hash_Tables.md)」を参照してください。 詳細については、「 [形式テーブル](xref:Microsoft.PowerShell.Utility.Format-Table)」の例を参照してください。

の現在の値を検索 `$FormatEnumerationLimit` します。

```powershell
$FormatEnumerationLimit
```

```Output
4
```

**状態** 別にグループ化されたすべてのサービスを一覧表示します。 各状態の [**グループ]** 列には、の値が4であるため、最大4つのサービスが表示され `$FormatEnumerationLimit` ます。 

```powershell
Get-Service | Group-Object -Property Status
```

```Output
Count  Name       Group
-----  ----       -----
60     Running    {AdtAgent, ALG, Ati HotKey Poller, AudioSrv...}
41     Stopped    {Alerter, AppMgmt, aspnet_state, ATI Smart...}
```

表示される項目の数を増やすには、の値を `$FormatEnumerationLimit` **1000** に増やします。 `Get-Service` `Group-Object` サービスを表示するには、およびを使用します。

```powershell
$FormatEnumerationLimit = 1000
Get-Service | Group-Object -Property Status
```

```Output
Count  Name       Group
-----  ----       -----
60     Running    {AdtAgent, ALG, Ati HotKey Poller, AudioSrv, BITS, CcmExec...
41     Stopped    {Alerter, AppMgmt, aspnet_state, ATI Smart, Browser, CiSvc...
```

`Format-Table` **Wrap** パラメーターと共にを使用して、サービスの一覧を表示します。

```powershell
Get-Service | Group-Object -Property Status | Format-Table -Wrap
```

```Output
Count  Name       Group
-----  ----       -----
60     Running    {AdtAgent, ALG, Ati HotKey Poller, AudioSrv, BITS, CcmExec,
                  Client for NFS, CryptSvc, DcomLaunch, Dhcp, dmserver,
                  Dnscache, ERSvc, Eventlog, EventSystem, FwcAgent, helpsvc,
                  HidServ, IISADMIN, InoRPC, InoRT, InoTask, lanmanserver,
                  lanmanworkstation, LmHosts, MDM, Netlogon, Netman, Nla,
                  NtLmSsp, PlugPlay, PolicyAgent, ProtectedStorage, RasMan,
                  RemoteRegistry, RpcSs, SamSs, Schedule, seclogon, SENS,
                  SharedAccess, ShellHWDetection, SMT PSVC, Spooler,
                  srservice, SSDPSRV, stisvc, TapiSrv, TermService, Themes,
                  TrkWks, UMWdf, W32Time, W3SVC, WebClient, winmgmt, wscsvc,
                  wuauserv, WZCSVC, zzInterix}

41     Stopped    {Alerter, AppMgmt, aspnet_state, ATI Smart, Browser, CiSvc,
                  ClipSrv, clr_optimization_v2.0.50727_32, COMSysApp,
                  CronService, dmadmin, FastUserSwitchingCompatibility,
                  HTTPFilter, ImapiService, Mapsvc, Messenger, mnmsrvc,
                  MSDTC, MSIServer, msvsmon80, NetDDE, NetDDEdsdm, NtmsSvc,
                  NVSvc, ose, RasAuto, RDSessMgr, RemoteAccess, RpcLocator,
                  SCardSvr, SwPrv, SysmonLog, TlntSvr, upnphost, UPS, VSS,
                  WmdmPmSN, Wmi, WmiApSrv, xmlprov}
```

### <a name="informationpreference"></a>\$Informationpreference は

`$InformationPreference`変数を使用すると、ユーザーに表示する情報ストリームの設定を設定できます。 具体的には、 [書き込み情報](xref:Microsoft.PowerShell.Utility.Write-Information) のコマンドレットを追加して、コマンドまたはスクリプトに追加した情報メッセージです。 **Informationaction** パラメーターが使用されている場合、その値は変数の値よりも優先され `$InformationPreference` ます。 `Write-Information` は、PowerShell 5.0 で導入されました。

有効な値は次のとおりです。

- **Stop**: コマンドの発生時に、コマンドまたはスクリプトを停止し `Write-Information` ます。
- **Inquire**: コマンドで指定した情報メッセージを表示し `Write-Information` 、続行するかどうかを確認します。
- [**続行**]: 情報メッセージを表示し、実行を継続します。
- **Suspend** は、PowerShell 6 以降ではサポートされていないワークフローでのみ使用できます。
- **SilentlyContinue**: (既定) 効果はありません。 情報メッセージは表示されず、スクリプトは中断されることなく続行されます。

### <a name="logevent"></a>\$Log * イベント

**Log * イベント** ユーザー設定変数は、イベントビューアーで PowerShell イベントログに書き込まれるイベントの種類を決定します。 既定では、エンジンイベントとプロバイダーイベントのみがログに記録されます。 ただし、 **log * イベント** ユーザー設定変数を使用して、コマンドに関するイベントのログ記録などのログをカスタマイズできます。

**Log * イベント** ユーザー設定変数は次のとおりです。

- `$LogCommandHealthEvent`: コマンドの初期化および処理中に発生したエラーと例外をログに記録します。 既定値は `$false` (ログに記録されません) です。
- `$LogCommandLifecycleEvent`: コマンド検出のコマンドとコマンドパイプラインおよびセキュリティ例外の開始と停止をログに記録します。 既定値は `$false` (ログに記録されません) です。
- `$LogEngineHealthEvent`: セッションのエラーとエラーをログに記録します。 既定値は `$true` (ログに記録されます) です。
- `$LogEngineLifecycleEvent`: セッションの開始と終了をログに記録します。 既定値は `$true` (ログに記録されます) です。
- `$LogProviderHealthEvent`: 読み取りおよび書き込みエラー、参照エラー、呼び出しエラーなど、プロバイダーのエラーをログに記録します。 既定値は `$true` (ログに記録されます) です。
- `$LogProviderLifecycleEvent`: PowerShell プロバイダーの追加と削除をログに記録します。 既定値は `$true` (ログに記録されます) です。 PowerShell プロバイダーの詳細については、「 [about_Providers](about_Providers.md)」を参照してください。

**Log * イベント** を有効にするには、次の例のように、値を持つ変数を入力し `$true` ます。

```powershell
$LogCommandLifeCycleEvent = $true
```

イベントの種類を無効にするには、という値を持つ変数を入力し `$false` ます。次に例を示します。

```powershell
$LogCommandLifeCycleEvent = $false
```

有効にするイベントは、現在の PowerShell コンソールでのみ有効です。 すべてのコンソールに構成を適用するには、PowerShell プロファイルに変数の設定を保存します。 詳細については、「 [about_Profiles](about_Profiles.md)」を参照してください。

### <a name="maximumhistorycount"></a>\$Maximumhistorycount ユーザー

現在のセッションのコマンド履歴に保存されているコマンドの数を決定します。

**有効な値**: 1-32768 ( `Int32` )

**既定値**: 4096

コマンド履歴に現在保存されているコマンドの数を確認するには、次のように入力します。

```powershell
(Get-History).Count
```

セッションの履歴に保存されているコマンドを確認するには、コマンドレットを使用し `Get-History` ます。 詳細については、「 [about_History](about_History.md)」を参照してください。

### <a name="ofs"></a>\$.OFS

出力フィールド区切り記号 (OFS) は、文字列に変換される配列の要素を区切る文字を指定します。

**有効な値**: 任意の文字列。

**既定値**: Space

既定では、変数は存在せず、 `$OFS` 出力ファイルの区切り記号は空白ですが、この変数を追加して任意の文字列に設定することができます。 セッションのの値を変更する `$OFS` には、「」と入力 `$OFS="<value>"` します。

> [!NOTE]
> `" "`スクリプト、モジュール、または構成の出力で空白 () の既定値が予想される場合は、 `$OFS` 既定値がコード内の他の場所で変更されていないことに注意してください。

#### <a name="examples"></a>例

この例は、配列が文字列に変換されるときに、値を区切るためにスペースを使用することを示しています。 この場合、整数の配列は変数に格納され、変数は文字列としてキャストされます。

```powershell
$array = 1,2,3,4
[string]$array
```

```Output
1 2 3 4
```

区切り記号を変更するには、 `$OFS` 変数に値を代入して変数を追加します。
変数はという名前にする必要があり `$OFS` ます。

```powershell
$OFS = "+"
[string]$array
```

```Output
1+2+3+4
```

既定の動作を復元するには、 `" "` の値に空白 () を割り当てる `$OFS` か、変数を削除します。 次のコマンドは、変数を削除し、区切り記号がスペースであることを確認します。

```powershell
Remove-Variable OFS
[string]$array
```

```Output
1 2 3 4
```

### <a name="outputencoding"></a>\$OutputEncoding

他のアプリケーションにテキストを送信するときに PowerShell が使用する文字エンコード方式を決定します。

たとえば、アプリケーションが Unicode 文字列を PowerShell に返す場合、値を **UnicodeEncoding** に変更して文字を正しく送信することが必要になる場合があります。

有効な値は次のとおりです。 **ASCIIEncoding**、 **SBCSCodePageEncoding**、 **UTF7Encoding**、 **UTF8Encoding**、 **UTF32Encoding**、 **UnicodeEncoding** などのエンコーディングクラスから派生したオブジェクト。

**既定値**: UTF8Encoding Object (UTF8Encoding)

#### <a name="examples"></a>例

この例では、中国語などの Unicode 文字を使用する言語用にローカライズされたコンピューターで Windows **findstr.exe** コマンドを PowerShell で動作させる方法を示します。

最初のコマンドは、の値を検索し `$OutputEncoding` ます。 値はエンコーディングオブジェクトであるため、 **encoding.encodingname** プロパティのみを表示します。

```powershell
$OutputEncoding.EncodingName
```

この例では、 **findstr.exe** コマンドを使用して、ファイル内に存在する2つの中国語文字を検索し `Test.txt` ます。 この **findstr.exe** コマンドを Windows コマンドプロンプト (**cmd.exe**) で実行すると、 **findstr.exe** によってテキストファイル内の文字が検索されます。 ただし、PowerShell で同じ **findstr.exe** コマンドを実行した場合、文字は検出されません。これは、Powershell が Unicode テキストではなく ASCII テキストで **findstr.exe** に送信するためです。

```powershell
findstr <Unicode-characters>
```

PowerShell でコマンドが動作するようにするには、の値 `$OutputEncoding` をコンソールの **outputencoding** プロパティの値に設定します。これは、Windows で選択されているロケールに基づいています。 **Outputencoding** はコンソールの静的プロパティであるため、コマンドで二重コロン () を使用し `::` ます。

```powershell
$OutputEncoding = [console]::OutputEncoding
$OutputEncoding.EncodingName
```

```Output
OEM United States
```

エンコードを変更すると、 **findstr.exe** コマンドによって Unicode 文字が検索されます。

```powershell
findstr <Unicode-characters>
```

```Output
test.txt:         <Unicode-characters>
```

### <a name="progresspreference"></a>\$ProgressPreference

PowerShell が、スクリプト、コマンドレット、またはプロバイダーによって生成された進行状況の更新に応答する方法を決定します。たとえば、 [書き込みの進行状況](xref:Microsoft.PowerShell.Utility.Write-Progress) のコマンドレットによって生成された進行状況バーなどです。
`Write-Progress`コマンドレットは、コマンドの状態を示す進行状況バーを作成します。

有効な値は次のとおりです。

- [**停止**: 進行状況バーを表示しません。 代わりに、エラーメッセージが表示され、実行が停止します。
- **Inquire**: 進行状況バーは表示されません。 続行するためのアクセス許可を要求します。 またはで返信すると `Y` `A` 、進行状況バーが表示されます。
- **続行**: (既定) 進行状況バーを表示し、実行を続行します。
- **SilentlyContinue**: コマンドを実行しますが、進行状況バーは表示されません。

### <a name="psemailserver"></a>\$PSEmailServer

電子メールメッセージの送信に使用する既定の電子メールサーバーを指定します。 このユーザー設定変数は、電子メールを送信するコマンドレット ( [send-mailmessage](xref:Microsoft.PowerShell.Utility.Send-MailMessage) コマンドレットなど) によって使用されます。

### <a name="psdefaultparametervalues"></a>\$PSDefaultParameterValues

コマンドレットおよび高度な関数のパラメーターの既定値を指定します。
の値 `$PSDefaultParameterValues` はハッシュテーブルです。キーはコマンドレット名とパラメーター名で構成され、コロン () で区切られ `:` ます。 値は、ユーザーが指定するカスタムの既定値です。

`$PSDefaultParameterValues` は、PowerShell 3.0 で導入されました。

このユーザー設定変数の詳細については、「 [about_Parameters_Default_Values](about_Parameters_Default_Values.md)」を参照してください。

### <a name="psmoduleautoloadingpreference"></a>\$PSModuleAutoloadingPreference

セッションのモジュールの自動インポートを有効または無効にします。 **All** が既定値です。 変数の値に関係なく、モジュールをインポート [するには、モジュールを](xref:Microsoft.PowerShell.Core.Import-Module) インポートします。

有効な値は次のとおりです。

- **すべて**: モジュールは初回使用時に自動的にインポートされます。 モジュールをインポートするには、モジュール内の任意のコマンドを取得または使用します。 たとえば、 `Get-Command`を使用します。
- **Modulの** 修飾: モジュールは、ユーザーがモジュール内のコマンドのモジュール修飾名を使用した場合にのみ、自動的にインポートされます。 たとえば、ユーザーがと入力すると、 `MyModule\MyCommand` PowerShell は **MyModule** モジュールをインポートします。
- **なし**: モジュールの自動インポートがセッションで無効になっています。 モジュールをインポートするには、 `Import-Module` コマンドレットを使用します。

モジュールの自動インポートの詳細については、「 [about_Modules](about_Modules.md)」を参照してください。

### <a name="pssessionapplicationname"></a>\$Pssessionapplicationname ユーザー

Web Services for Management (WS-MANAGEMENT) テクノロジを使用するリモートコマンドの既定のアプリケーション名を指定します。 詳細については、「 [Windows リモート管理につい](/windows/win32/winrm/about-windows-remote-management)て」を参照してください。

システムの既定のアプリケーション名はです `WSMAN` が、このユーザー設定変数を使用して既定値を変更できます。

アプリケーション名は、接続 URI の最後のノードです。 たとえば、次の URI の例では、アプリケーション名が `WSMAN` です。

`http://Server01:8080/WSMAN`

既定のアプリケーション名は、リモートコマンドで接続 URI またはアプリケーション名が指定されていない場合に使用されます。

**WinRM** サービスは、アプリケーション名を使用して、接続要求を処理するリスナーを選択します。 パラメーターの値は、リモートコンピューター上のリスナーの **Urlprefix** プロパティの値と一致している必要があります。

システムの既定値とこの変数の値を上書きし、特定のセッションに対して別のアプリケーション名を選択するには、[新しい PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)、 [Enter-pssession](xref:Microsoft.PowerShell.Core.Enter-PSSession)、または [Invoke コマンド](xref:Microsoft.PowerShell.Core.Invoke-Command)レットの **connectionuri** または **ApplicationName** パラメーターを使用します。

ユーザー設定 `$PSSessionApplicationName` 変数はローカルコンピューターで設定されますが、リモートコンピューター上のリスナーを指定します。 指定したアプリケーション名がリモートコンピューター上に存在しない場合、セッションを確立するコマンドは失敗します。

### <a name="pssessionconfigurationname"></a>\$Pssessionconfigurationname ユーザー

現在のセッション **で作成された** pssession に使用される既定のセッション構成を指定します。

このユーザー設定変数はローカルコンピューターで設定されますが、リモートコンピューター上にあるセッション構成を指定します。

変数の値 `$PSSessionConfigurationName` は、完全修飾リソース URI です。

既定値は、 `http://schemas.microsoft.com/PowerShell/microsoft.PowerShell` リモートコンピューター上の **Microsoft PowerShell** セッション構成を示します。

構成名だけを指定した場合は、次のスキーマ URI が前に付加されます。

`http://schemas.microsoft.com/PowerShell/`

 `New-PSSession` 、、 `Enter-PSSession` またはコマンドレットの ConfigurationName パラメーターを使用して、既定値を上書きし、特定のセッションに対して別のセッション構成を選択することができ `Invoke-Command` ます。

この変数の値はいつでも変更できます。 この場合は、選択したセッション構成がリモートコンピューターに存在する必要があることに注意してください。 そうでない場合、セッション構成を使用するセッションを作成するコマンドは失敗します。

このユーザー設定変数は、リモートユーザーがこのコンピューターに接続するセッションを作成するときに使用されるローカルセッション構成を決定しません。
ただし、ローカルセッション構成のアクセス許可を使用して、使用できるユーザーを決定することができます。

### <a name="pssessionoption"></a>\$PSSessionOption

リモートセッションでの高度なユーザーオプションの既定値を設定します。
これらのオプションの設定は、セッションオプションのシステムの既定値よりも優先されます。

変数には、 `$PSSessionOption` **Pssessionoption** オブジェクトが含まれています。 詳細については、「system.string [オプション](/dotnet/api/system.management.automation.remoting.pssessionoption)」を参照してください。
オブジェクトの各プロパティは、セッションオプションを表します。 たとえば、 **Nocompression** プロパティは、セッション中のデータ圧縮を有効にします。

既定では、変数には、 `$PSSessionOption` 次に示すように、すべてのオプションの既定値を持つ **Pssessionoption** オブジェクトが含まれています。

```Output
MaximumConnectionRedirectionCount : 5
NoCompression                     : False
NoMachineProfile                  : False
ProxyAccessType                   : None
ProxyAuthentication               : Negotiate
ProxyCredential                   :
SkipCACheck                       : False
SkipCNCheck                       : False
SkipRevocationCheck               : False
OperationTimeout                  : 00:03:00
NoEncryption                      : False
UseUTF16                          : False
IncludePortInSPN                  : False
OutputBufferingMode               : None
Culture                           :
UICulture                         :
MaximumReceivedDataSizePerCommand :
MaximumReceivedObjectSize         : 209715200
ApplicationArguments              :
OpenTimeout                       : 00:03:00
CancelTimeout                     : 00:01:00
IdleTimeout                       : -00:00:00.0010000
```

これらのオプションの説明と詳細については、「 [New-PSSessionOption](xref:Microsoft.PowerShell.Core.New-PSSessionOption)」を参照してください。 リモートコマンドとセッションの詳細については、「 [about_Remote](about_Remote.md) 」および「 [about_PSSessions](about_PSSessions.md)」を参照してください。

ユーザー設定変数の値を変更するには、コマンドレットを使用して、 `$PSSessionOption` `New-PSSessionOption` 必要なオプション値を指定した **pssessionoption** オブジェクトを作成します。 という変数に出力を保存 `$PSSessionOption` します。

```powershell
$PSSessionOption = New-PSSessionOption -NoCompression
```

`$PSSessionOption`すべての powershell セッションでユーザー設定変数を使用するには、 `New-PSSessionOption` powershell プロファイルに変数を作成するコマンドを追加し `$PSSessionOption` ます。 詳細については、「 [about_Profiles](about_Profiles.md)」を参照してください。

特定のリモートセッションに対してカスタムオプションを設定できます。 設定するオプションは、システムの既定値とユーザー設定変数の値よりも優先され `$PSSessionOption` ます。

カスタムセッションオプションを設定するには、コマンドレットを使用して `New-PSSessionOption` **Pssessionoption** オブジェクトを作成します。 次に、、、などのセッションを作成するコマンドレットの **sessionoption** パラメーターの値として **pssessionoption** オブジェクトを使用し `New-PSSession` `Enter-PSSession` `Invoke-Command` ます。

### <a name="transcript"></a>$Transcript

`Start-Transcript`トランスクリプトファイルの名前と場所を指定するために、によって使用されます。 **Path** パラメーターの値を指定しない場合、は `Start-Transcript` グローバル変数の値のパスを使用し `$Transcript` ます。 この変数を作成していない場合は、によっ `Start-Transcript` `$Home\My Documents` てファイルとしてディレクトリにトランスクリプトが保存され `\PowerShell_transcript.<time-stamp>.txt` ます。

### <a name="verbosepreference"></a>\$VerbosePreference

PowerShell が、スクリプト、コマンドレット、またはプロバイダーによって生成される詳細メッセージに応答する方法を決定します。これには、 [書き込み/詳細](xref:Microsoft.PowerShell.Utility.Write-Verbose) コマンドレットによって生成されるメッセージなどがあります。
詳細メッセージには、コマンドを実行するために実行されるアクションが記述されています。

既定では、詳細メッセージは表示されませんが、の値を変更することでこの動作を変更でき `$VerbosePreference` ます。

コマンドレットの **verbose** 共通パラメーターを使用して、特定のコマンドの詳細メッセージを表示または非表示にすることができます。 詳細については、「[about_CommonParameters](about_CommonParameters.md)」を参照してください。

有効な値は次のとおりです。

- [**停止**]: 詳細メッセージとエラーメッセージを表示し、実行を停止します。
- [ **Inquire**]: 詳細メッセージが表示され、続行するかどうかを確認するプロンプトが表示されます。
- **続行**: 詳細メッセージを表示し、実行を続行します。
- **SilentlyContinue**: (既定) では、詳細メッセージは表示されません。 実行を続行します。

#### <a name="examples"></a>例

これらの例では、の異なる値の効果 `$VerbosePreference` と **Verbose** パラメーターを指定して、優先順位の値をオーバーライドしています。

この例は、既定の **SilentlyContinue** 値の効果を示しています。 このコマンドでは、 **message** パラメーターを使用しますが、PowerShell コンソールにはメッセージは書き込まれません。

```powershell
Write-Verbose -Message "Verbose message test."
```

**Verbose** パラメーターを使用すると、メッセージが書き込まれます。

```powershell
Write-Verbose -Message "Verbose message test." -Verbose
```

```Output
VERBOSE: Verbose message test.
```

この例は、 **Continue** 値の効果を示しています。 `$VerbosePreference`変数が **Continue** に設定され、メッセージが表示されます。

```powershell
$VerbosePreference = "Continue"
Write-Verbose -Message "Verbose message test."
```

```Output
VERBOSE: Verbose message test.
```

この例では 、 `$false` **Continue** 値をオーバーライドするの値を持つ Verbose パラメーターを使用します。 メッセージは表示されません。

```powershell
Write-Verbose -Message "Verbose message test." -Verbose:$false
```

この例は、 **Stop** 値の効果を示しています。 `$VerbosePreference`変数が **Stop** に設定され、メッセージが表示されます。 コマンドは停止しています。

```powershell
$VerbosePreference = "Stop"
Write-Verbose -Message "Verbose message test."
```

```Output
VERBOSE: Verbose message test.
Write-Verbose : The running command stopped because the preference variable
  "VerbosePreference" or common parameter is set to Stop: Verbose message test.
At line:1 char:1
+ Write-Verbose -Message "Verbose message test."
```

この例では 、 `$false` **Stop** 値をオーバーライドするの値を持つ Verbose パラメーターを使用します。 メッセージは表示されません。

```powershell
Write-Verbose -Message "Verbose message test." -Verbose:$false
```

この例は、 **Inquire** 値の効果を示しています。 `$VerbosePreference`変数は **Inquire** に設定されています。 メッセージが表示され、ユーザーに確認を求めるメッセージが表示されます。

```powershell
$VerbosePreference = "Inquire"
Write-Verbose -Message "Verbose message test."
```

```Output
VERBOSE: Verbose message test.

Confirm
Continue with this operation?
[Y] Yes  [A] Yes to All  [H] Halt Command  [?] Help (default is "Y"):
```

この例では 、 `$false` **Inquire** 値をオーバーライドするの値を持つ Verbose パラメーターを使用します。 ユーザーにプロンプトが表示されず、メッセージが表示されません。

```powershell
Write-Verbose -Message "Verbose message test." -Verbose:$false
```

### <a name="warningpreference"></a>\$WarningPreference

スクリプト、コマンドレット、またはプロバイダーによって生成される警告メッセージ ( [書き込み警告](xref:Microsoft.PowerShell.Utility.Write-Warning) コマンドレットによって生成されるメッセージなど) に対して PowerShell が応答する方法を指定します。

既定では、警告メッセージが表示され、実行が続行されますが、の値を変更することでこの動作を変更でき `$WarningPreference` ます。

コマンドレットの **Warnings action** 共通パラメーターを使用して、PowerShell が特定のコマンドからの警告に応答する方法を決定できます。 詳細については、「[about_CommonParameters](about_CommonParameters.md)」を参照してください。

有効な値は次のとおりです。

- [**停止**]: 警告メッセージとエラーメッセージを表示し、実行を停止します。
- **Inquire**: 警告メッセージを表示し、続行するためのアクセス許可を要求します。
- **続行**: (既定) 警告メッセージを表示し、実行を続行します。
- **SilentlyContinue**: 警告メッセージが表示されません。 実行を続行します。

#### <a name="examples"></a>例

これらの例は、のさまざまな値の効果を示して `$WarningPreference` います。
**Warnings action** パラメーターは、基本設定値よりも優先されます。

この例は、既定値の効果を示しています。 **続行** します。

```powershell
$m = "This action can delete data."
Write-Warning -Message $m
```

```Output
WARNING: This action can delete data.
```

この例では、 **SilentlyContinue** 値を指定した **warnings action** パラメーターを使用して、警告を非表示にします。 メッセージは表示されません。

```powershell
$m = "This action can delete data."
Write-Warning -Message $m -WarningAction SilentlyContinue
```

この例では、 `$WarningPreference` 変数を **SilentlyContinue** 値に変更します。 メッセージは表示されません。

```powershell
$WarningPreference = "SilentlyContinue"
$m = "This action can delete data."
Write-Warning -Message $m
```

この例では、警告が生成されたときに、 **Warnings action** パラメーターを使用して停止します。

```powershell
$m = "This action can delete data."
Write-Warning -Message $m -WarningAction Stop
```

```Output
WARNING: This action can delete data.
Write-Warning : The running command stopped because the preference variable
  "WarningPreference" or common parameter is set to Stop:
    This action can delete data.
At line:1 char:1
+ Write-Warning -Message $m -WarningAction Stop
```

この例では、 `$WarningPreference` 変数を **Inquire** 値に変更します。 ユーザーに確認を求めるメッセージが表示されます。

```powershell
$WarningPreference = "Inquire"
$m = "This action can delete data."
Write-Warning -Message $m
```

```Output
WARNING: This action can delete data.

Confirm
Continue with this operation?
[Y] Yes  [A] Yes to All  [H] Halt Command  [?] Help (default is "Y"):
```

この例では、 **SilentlyContinue** という値を指定して **warnings action** パラメーターを使用します。 コマンドは引き続き実行され、メッセージは表示されません。

```powershell
$m = "This action can delete data."
Write-Warning -Message $m -WarningAction SilentlyContinue
```

この例では、 `$WarningPreference` 値を **Stop** に変更します。

```powershell
$WarningPreference = "Stop"
$m = "This action can delete data."
Write-Warning -Message $m
```

```Output
WARNING: This action can delete data.
Write-Warning : The running command stopped because the preference variable
  "WarningPreference" or common parameter is set to Stop:
    This action can delete data.
At line:1 char:1
+ Write-Warning -Message $m
```

この例では、 **Inquire** 値を指定して **warnings アクション** を使用します。 警告が発生すると、ユーザーに対してメッセージが表示されます。

```powershell
$m = "This action can delete data."
Write-Warning -Message $m -WarningAction Inquire
```

```Output
WARNING: This action can delete data.

Confirm
Continue with this operation?
[Y] Yes  [A] Yes to All  [H] Halt Command  [?] Help (default is "Y"):
```

### <a name="whatifpreference"></a>\$WhatIfPreference

サポートされているすべてのコマンドに対して **WhatIf** を自動的に有効にするかどうかを決定します。 **WhatIf** が有効になっている場合、コマンドレットはコマンドの予想される効果を報告しますが、コマンドを実行しません。

有効な値は次のとおりです。

- **False** (**0**、有効でない): (既定値) **WhatIf** が自動的に有効になることはありません。 手動で有効にするには、コマンドレットの **WhatIf** パラメーターを使用します。
- **True** (**1**、有効): **WhatIf** は、それをサポートするすべてのコマンドで自動的に有効になります。 ユーザーは、 **WhatIf** パラメーターを値 **False** と共に使用して、などのように手動で無効にすることができ `-WhatIf:$false` ます。

#### <a name="examples"></a>例

これらの例は、のさまざまな値の効果を示して `$WhatIfPreference` います。
この例では、 **WhatIf** パラメーターを使用して、特定のコマンドの設定値をオーバーライドする方法を示します。

この例は、 `$WhatIfPreference` 変数が既定値の **False** に設定された場合の効果を示しています。 `Get-ChildItem`ファイルが存在することを確認するには、を使用します。
`Remove-Item` ファイルを削除します。 ファイルが削除されたら、で削除を確認でき `Get-ChildItem` ます。

```powershell
Get-ChildItem -Path .\test.txt
Remove-Item -Path ./test.txt
```

```Output
    Directory: C:\Test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           9/13/2019    10:53             10 test.txt
```

```powershell
Get-ChildItem -Path .\test.txt
```

```Output
Get-ChildItem : Cannot find path 'C:\Test\test.txt' because it does not exist.
At line:1 char:1
+ Get-ChildItem -File test.txt
```

この例は、の値が False の場合に **WhatIf** パラメーターを使用した場合の効果を示して `$WhatIfPreference` います。 

ファイルが存在することを確認してください。

```powershell
Get-ChildItem -Path .\test2.txt
```

```Output
    Directory: C:\Test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           2/28/2019    17:06             12 test2.txt
```

**WhatIf** パラメーターを使用して、ファイルを削除しようとした結果を確認します。

```powershell
Remove-Item -Path .\test2.txt -WhatIf
```

```Output
What if: Performing the operation "Remove File" on target "C:\Test\test2.txt".
``````

ファイルが削除されていないことを確認します。

```powershell
Get-ChildItem -Path .\test2.txt
```

```Output
    Directory: C:\Test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           2/28/2019    17:06             12 test2.txt
```

この例では、変数の `$WhatIfPreference` 値が **True** に設定された場合の効果を示します。 を使用してファイルを削除すると、ファイル `Remove-Item` のパスが表示されますが、ファイルは削除されません。

ファイルを削除しようとしました。 が実行された場合に何が起こるかについてのメッセージが表示され `Remove-Item` ますが、ファイルは削除されません。

```powershell
$WhatIfPreference = "True"
Remove-Item -Path .\test2.txt
```

```Output
What if: Performing the operation "Remove File" on target "C:\Test\test2.txt".
```

`Get-ChildItem`ファイルが削除されていないことを確認するには、を使用します。

```powershell
Get-ChildItem -Path .\test2.txt
```

```Output
    Directory: C:\Test

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           2/28/2019    17:06             12 test2.txt
```

この例では、の値が True の場合にファイルを削除する方法を示し `$WhatIfPreference` ます。  この例では、 **WhatIf** パラメーターに値を指定 `$false` しています。 `Get-ChildItem`ファイルが削除されたことを確認するには、を使用します。

```powershell
Remove-Item -Path .\test2.txt -WhatIf:$false
Get-ChildItem -Path .\test2.txt
```

```Output
Get-ChildItem : Cannot find path 'C:\Test\test2.txt' because it does not exist.
At line:1 char:1
+ Get-ChildItem -Path .\test2.txt
```

Whatif をサポートしていない whatif をサポートしているコマンドレットの例を次に示します `Get-Process`  `Stop-Process` 。  `$WhatIfPreference`変数の値は **True** です。

`Get-Process`**WhatIf** はサポートされていません。 コマンドが実行されると、 **winword.exe** プロセスが表示されます。

```powershell
Get-Process -Name Winword
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
    130   119.84     173.38       8.39   15024   4 WINWORD
```

`Stop-Process` は **WhatIf** をサポートしています。 **Winword.exe** プロセスは停止されていません。

```powershell
Stop-Process -Name Winword
```

```Output
What if: Performing the operation "Stop-Process" on target "WINWORD (15024)".
```

Whatif 動作をオーバーライドするには、 `Stop-Process`  **whatif** パラメーターに値を指定し `$false` ます。 **Winword.exe** プロセスは停止しています。

```powershell
Stop-Process -Name Winword -WhatIf:$false
```

**Winword.exe** プロセスが停止したことを確認するには、を使用 `Get-Process` します。

```powershell
Get-Process -Name Winword
```

```Output
Get-Process : Cannot find a process with the name "Winword".
  Verify the process name and call the cmdlet again.
At line:1 char:1
+ Get-Process -Name Winword
```

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_CommonParameters](about_CommonParameters.md)

[about_Environment_Variables](about_Environment_Variables.md)

[about_Profiles](about_Profiles.md)

[about_Remote](about_Remote.md)

[about_Scopes](about_Scopes.md)

[about_Variables](about_Variables.md)

