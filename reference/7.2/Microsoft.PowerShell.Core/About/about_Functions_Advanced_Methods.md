---
description: 属性を指定する関数 `CmdletBinding` が、コンパイルされたコマンドレットで使用できるメソッドとプロパティを使用する方法について説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_functions_advanced_methods?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Functions_Advanced_Methods
ms.openlocfilehash: 13a9d7258f1a52d5fcbb2d8c55b91c8d6454452d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600612"
---
# <a name="about-functions-advanced-methods"></a>Functions の詳細なメソッドについて

## <a name="short-description"></a>簡単な説明

属性を指定する関数 `CmdletBinding` が、コンパイルされたコマンドレットで使用できるメソッドとプロパティを使用する方法について説明します。

## <a name="long-description"></a>長い説明

属性を指定する関数 `CmdletBinding` は、変数を通じて多数のメソッドとプロパティにアクセスでき `$PSCmdlet` ます。 これらのメソッドには、次のメソッドが含まれます。

- コンパイルされたコマンドレットが作業を行うために使用する入力処理メソッド。
- `ShouldProcess` `ShouldContinue` アクションが実行される前にユーザーフィードバックを取得するために使用されるメソッドとメソッド。
- `ThrowTerminatingError`エラーレコードを生成するメソッド。
- `Write`さまざまな種類の出力を返すメソッドがいくつかあります。

**PSCmdlet** クラスのすべてのメソッドとプロパティは、高度な関数で使用できます。 詳細については、「 [PSCmdlet](/dotnet/api/system.management.automation.pscmdlet)」を参照してください。

属性の詳細については `CmdletBinding` 、「 [about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)」を参照してください。
[コマンド](/dotnet/api/system.management.automation.cmdletbindingattribute)レット **bindingattribute** クラスについては、「system.string」を参照してください。

### <a name="input-processing-methods"></a>入力処理メソッド

このセクションで説明するメソッドは、入力処理メソッドと呼ばれます。 関数の場合、これら3つのメソッドは `Begin` 、 `Process` 関数の、、およびの各ブロックによって表され `End` ます。 関数でこれらのブロックを使用する必要はありません。

> [!NOTE]
> これらのブロックは、属性を使用しない関数でも使用でき `CmdletBinding` ます。

#### <a name="begin"></a>開始

このブロックは、関数に対してオプションの1回限りの前処理を行うために使用されます。
PowerShell ランタイムは、パイプライン内の関数の各インスタンスに対して、このブロック内のコードを1回使用します。

#### <a name="process"></a>プロセス

このブロックは、関数のレコードごとの処理を提供するために使用されます。 `Process`ブロックは、他のブロックを定義することなく使用できます。 ブロックの実行回数は、 `Process` 関数の使用方法と、関数が受け取る入力によって異なります。

自動変数 `$_` またはは、 `$PSItem` ブロックで使用するために、パイプライン内の現在のオブジェクトを格納し `Process` ます。 自動変数には、 `$input` 関数とスクリプトブロックでのみ使用できる列挙子が含まれています。
詳細については、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

- パイプラインの先頭または外部で関数を呼び出すと、ブロックが `Process` 1 回実行されます。
- パイプライン内では、 `Process` ブロックは、関数に到達した入力オブジェクトごとに1回実行されます。
- 関数に到達したパイプライン入力が空の場合、 `Process` ブロックは実行 **されません** 。
  - `Begin`ブロックとブロックは依然として `End` 実行されます。

> [!IMPORTANT]
> 関数パラメーターがパイプライン入力を受け入れるように設定されていて、ブロックが定義されていない場合 `Process` 、レコードごとの処理は失敗します。 この場合、関数は、入力に関係なく、1回だけ実行されます。

#### <a name="end"></a>End

このブロックは、関数に対して省略可能な1回限りの後処理を提供するために使用されます。

次の例は、 `Begin` 1 回限りの前処理のブロック、 `Process` 複数のレコード処理のブロック、および1回限りの後処理のブロックを含む関数の概要を示してい `End` ます。

```powershell
Function Test-ScriptCmdlet
{
[CmdletBinding(SupportsShouldProcess=$True)]
    Param ($Parameter1)
    Begin{}
    Process{}
    End{}
}
```

> [!NOTE]
> またはブロックのいずれかを使用する `Begin` には、 `End` 3 つのブロックをすべて定義する必要があります。 3つすべてのブロックを使用する場合は、すべての PowerShell コードがいずれかのブロックに含まれている必要があります。

### <a name="confirmation-methods"></a>確認方法

#### <a name="shouldprocess"></a>ShouldProcess

このメソッドは、システムを変更するアクションを実行する前に、ユーザーからの確認を要求するために呼び出されます。 関数は、メソッドによって返されるブール値に基づいて続行できます。 このメソッドを呼び出すことができるのは `Process{}` 、関数のブロック内からだけです。 また、属性は、 `CmdletBinding` 関数が (前の例で示したように) をサポートすることも宣言する必要があり `ShouldProcess` ます。

このメソッドの詳細については、「 [システムの管理](/dotnet/api/system.management.automation.cmdlet.shouldprocess)」を参照してください。

確認を要求する方法の詳細については、「 [確認](/powershell/scripting/developer/cmdlet/requesting-confirmation)の要求」を参照してください。

#### <a name="shouldcontinue"></a>ShouldContinue

このメソッドは、2番目の確認メッセージを要求するために呼び出されます。 メソッドから制御が戻ったときに呼び出す必要があり `ShouldProcess` `$true` ます。 このメソッドの詳細については、「 [システムの管理](/dotnet/api/system.management.automation.cmdlet.shouldcontinue)」を参照してください。

### <a name="error-methods"></a>エラーメソッド

関数は、エラーが発生したときに2つの異なるメソッドを呼び出すことができます。 終了しないエラーが発生した場合、関数はメソッドを呼び出す必要があり `WriteError` ます。これについては、 `Write` 「メソッド」セクションを参照してください。 終了エラーが発生し、関数を続行できない場合は、メソッドを呼び出す必要があり `ThrowTerminatingError` ます。 また、ステートメントを使用して `Throw` エラーを終了し、 [書き込みエラー](xref:Microsoft.PowerShell.Utility.Write-Error) コマンドレットを使用して、終了しないエラーを確認することもできます。

詳細については、「 [ThrowTerminatingError](/dotnet/api/system.management.automation.cmdlet.throwterminatingerror)」を参照してください。

### <a name="write-methods"></a>メソッドの書き込み

関数は、次のメソッドを呼び出して、異なる種類の出力を返すことができます。
すべての出力がパイプラインの次のコマンドに送られているわけではないことに注意してください。 などのさまざまなコマンドレットを使用することもでき `Write` `Write-Error` ます。

#### <a name="writecommanddetail"></a>WriteCommandDetail

メソッドの詳細については `WriteCommandDetails` 、「 [WriteCommandDetail](/dotnet/api/system.management.automation.cmdlet.writecommanddetail)」を参照してください。

#### <a name="writedebug"></a>WriteDebug

関数のトラブルシューティングに使用できる情報を提供するには、関数がメソッドを呼び出すようにし `WriteDebug` ます。 メソッドは、 `WriteDebug` ユーザーにデバッグメッセージを表示します。 詳細については [、「system.string](/dotnet/api/system.management.automation.cmdlet.writedebug)」を参照してください。

#### <a name="writeerror"></a>WriteError

関数は、終了しないエラーが発生し、関数がレコードの処理を続行するように設計されている場合に、このメソッドを呼び出す必要があります。 詳細については、「 [WriteError](/dotnet/api/system.management.automation.cmdlet.writeerror)」を参照してください。

> [!NOTE]
> 終了エラーが発生した場合、関数は [ThrowTerminatingError](/dotnet/api/system.management.automation.cmdlet.throwterminatingerror) メソッドを呼び出す必要があります。

#### <a name="writeobject"></a>WriteObject

`WriteObject`メソッドを使用すると、関数は、パイプラインの次のコマンドにオブジェクトを送信できます。 ほとんどの場合、 `WriteObject` は、関数がデータを返すときに使用するメソッドです。 詳細については、「 [PSCmdlet. WriteObject](/dotnet/api/system.management.automation.cmdlet.writeobject)」を参照してください。

#### <a name="writeprogress"></a>WriteProgress

完了に時間がかかるアクションを含む関数の場合、このメソッドは、 `WriteProgress` 進行状況の情報が表示されるように、関数がメソッドを呼び出すことができるようにします。 たとえば、完了した割合を表示できます。
詳細については、「 [PSCmdlet](/dotnet/api/system.management.automation.cmdlet.writeprogress)」を参照してください。

#### <a name="writeverbose"></a>WriteVerbose

関数の動作に関する詳細情報を提供するには、メソッドを呼び出して、 `WriteVerbose` ユーザーに詳細メッセージを表示するようにします。 既定では、詳細メッセージは表示されません。 詳細については、「 [PSCmdlet](/dotnet/api/system.management.automation.cmdlet.writeverbose)」を参照してください。

#### <a name="writewarning"></a>WriteWarning

予期しない結果になる可能性のある条件に関する情報を提供するには、関数が WriteWarning メソッドを呼び出して、警告メッセージをユーザーに表示します。 既定では、警告メッセージが表示されます。 詳細については、「 [PSCmdlet](/dotnet/api/system.management.automation.cmdlet.writewarning)」を参照してください。

> [!NOTE]
> また、変数を構成する `$WarningPreference` か、 `Verbose` およびコマンドラインオプションを使用して、警告メッセージを表示することもでき `Debug` ます。 変数の詳細については `$WarningPreference` 、「 [about_Preference_Variables](about_Preference_Variables.md)」を参照してください。

### <a name="other-methods-and-properties"></a>その他のメソッドとプロパティ

変数を使用してアクセスできるその他のメソッドとプロパティの詳細については `$PSCmdlet` 、「 [PSCmdlet](/dotnet/api/system.management.automation.pscmdlet)」を参照してください。

たとえば、 [ParameterSetName](/dotnet/api/system.management.automation.pscmdlet.parametersetname) プロパティを使用すると、使用されているパラメーターセットを確認できます。 パラメーターセットを使用すると、関数の実行時に指定されたパラメーターに基づいてさまざまなタスクを実行する関数を作成できます。

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Functions](about_Functions.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

[about_Functions_OutputTypeAttribute](about_Functions_OutputTypeAttribute.md)

[about_Preference_Variables](about_Preference_Variables.md)

