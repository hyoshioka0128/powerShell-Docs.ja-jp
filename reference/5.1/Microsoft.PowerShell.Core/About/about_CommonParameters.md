---
description: 任意のコマンドレットで使用できるパラメーターについて説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 01/26/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_commonparameters?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_CommonParameters
ms.openlocfilehash: 898f0897638523291751ce75db1c6a6b4628e778
ms.sourcegitcommit: 11880ca974fe2df308191c9f6dcdfe0b89c2dc67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/27/2021
ms.locfileid: "98860694"
---
# <a name="about-commonparameters"></a>CommonParameters について

## <a name="short-description"></a>概要

任意のコマンドレットで使用できるパラメーターについて説明します。

## <a name="long-description"></a>詳細説明

共通パラメーターは、すべてのコマンドレットで使用できるコマンドレットパラメーターのセットです。 これらは、コマンドレットの開発者ではなく、PowerShell によって実装され、任意のコマンドレットで自動的に使用できるようになります。

共通パラメーターは任意のコマンドレットと共に使用できますが、すべてのコマンドレットに影響を与えない場合があります。 たとえば、コマンドレットで詳細出力が生成されない場合、 **verbose** 共通パラメーターを使用しても効果はありません。

一般的なパラメーターは、表示可能な **バインド** 属性または **Parameter** 属性を使用する高度な関数でも使用できます。

いくつかの一般的なパラメーターでは、システムの既定値や、PowerShell ユーザー設定変数を使用して設定した設定を上書きします。 ユーザー設定変数とは異なり、共通パラメーターは、使用されるコマンドにのみ影響します。

詳細については、「 [about_Preference_Variables](./about_Preference_Variables.md)」を参照してください。

共通パラメーターの一覧を次に示します。 これらのエイリアスは、かっこで囲まれています。

- **Debug** (db)
- **Erroraction** (ea)
- **Errorvariable** (ev)
- **Informationaction** (infa)
- **Informationvariable** (iv)
- **Outvariable** (ov-es)
- **Outbuffer** (ob)
- **PipelineVariable** (pv)
- **Verbose** (vb)
- **警告動作** (wa)
- **警告変数** (wv)

**アクション** パラメーターは **actionpreference** 型の値です。
**Actionpreference** は、次の値を持つ列挙体です。

| 名前             | 値 |
|------------------|-------|
| [中断]          | 5     |
| Ignore           | 4     |
| 照会          | 3     |
| 続行         | 2     |
| Stop             | 1     |
| SilentlyContinue | 0     |

パラメーターと共に名前または値を使用できます。

共通パラメーターに加えて、多くのコマンドレットには、リスク軽減パラメーターが用意されています。 システムまたはユーザーデータに対するリスクが含まれるコマンドレットは、通常、これらのパラメーターを提供します。

リスク軽減のパラメーターは次のとおりです。

- **WhatIf** (wi-fi)
- **確認** (cf)

### <a name="common-parameter-descriptions"></a>共通パラメーターの説明

#### <a name="debug"></a>デバッグ

コマンドによって実行される操作に関するプログラマレベルの詳細を表示します。 このパラメーターは、コマンドによってデバッグメッセージが生成された場合にのみ機能します。 たとえば、コマンドにコマンドレットが含まれている場合、このパラメーターは機能し `Write-Debug` ます。

```yaml
Type: SwitchParameter
Aliases: db

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

既定では、変数の値 `$DebugPreference` が **SilentlyContinue** であるため、デバッグメッセージは表示されません。

対話モードでは、 **Debug** パラメーターによって現在のコマンドの変数の値が上書きされ `$DebugPreference` 、の値が `$DebugPreference` **Inquire** に設定されます。

非対話モードでは、 **Debug** パラメーターによって現在のコマンドの変数の値が上書きされ `$DebugPreference` 、の値 `$DebugPreference` が **Continue** に設定されます。

`-Debug:$true` と同じ効果があり `-Debug` ます。 `-Debug:$false` `$DebugPreference` が **SilentlyContinue** ではない場合 (既定)、を使用して、デバッグメッセージの表示を抑制します。

#### <a name="erroraction"></a>ErrorAction

コマンドレットが、コマンドから終了しないエラーに応答する方法を決定します。
このパラメーターは、コマンドがコマンドレットなどの終了しないエラーを生成した場合にのみ機能し `Write-Error` ます。

```yaml
Type: ActionPreference
Aliases: ea
Accepted values: Suspend, Ignore, Inquire, Continue, Stop, SilentlyContinue

Required: False
Position: Named
Default value: Depends on preference variable
Accept pipeline input: False
Accept wildcard characters: False
```

**Erroraction** パラメーターは、現在のコマンドの変数の値よりも優先され `$ErrorActionPreference` ます。 変数の既定値 `$ErrorActionPreference` は **続行** されるため、 **erroraction** パラメーターを使用しない限り、エラーメッセージが表示され、実行が続行されます。

**Erroraction** パラメーターは、コマンドが正常に完了しないようにする、不足しているデータ、無効なパラメーター、権限の不足などの終了エラーには影響しません。

`-ErrorAction:Continue` エラーメッセージを表示し、コマンドの実行を続行します。 `Continue` は既定値です。

`-ErrorAction:Ignore` エラーメッセージを表示せずに、コマンドの実行を続行します。 **SilentlyContinue** とは異なり、 **Ignore** ではエラーメッセージが自動変数に追加されません `$Error` 。 **Ignore** 値は、PowerShell 3.0 で導入されました。

`-ErrorAction:Inquire` エラーメッセージを表示し、実行を続行する前に確認メッセージを表示します。 この値はほとんど使用されません。

`-ErrorAction:SilentlyContinue` エラーメッセージを表示せずに、コマンドの実行を続行します。

`-ErrorAction:Stop` エラーメッセージを表示し、コマンドの実行を停止します。

`-ErrorAction:Suspend` は、PowerShell 6 以降ではサポートされていないワークフローでのみ使用できます。

> [!NOTE]
> **Erroraction** パラメーターはをオーバーライドしますが、 `$ErrorAction` スクリプトまたは関数を実行するためにコマンドでパラメーターを使用する場合は、ユーザー設定変数の値を置き換えません。

#### <a name="errorvariable"></a>ErrorVariable

**Errorvariable** は、コマンドに関するエラーメッセージを指定された変数と自動変数に格納し `$Error` ます。 詳細については、「」を参照してください [about_Automatic_Variables](about_Automatic_Variables.md)

```yaml
Type: String
Aliases: ev

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

既定では、新しいエラーメッセージは、変数に既に格納されているエラーメッセージを上書きします。 変数の内容にエラーメッセージを追加するには、 `+` 変数名の前にプラス記号 () を入力します。

たとえば、次のコマンドは変数を作成し、 `$a` エラーを格納します。

```powershell
Get-Process -Id 6 -ErrorVariable a
```

次のコマンドを実行すると、エラーメッセージが変数に追加され `$a` ます。

```powershell
Get-Process -Id 2 -ErrorVariable +a
```

次のコマンドを実行すると、の内容が表示され `$a` ます。

```powershell
$a
```

このパラメーターを使用すると、特定のコマンドからのエラーメッセージのみを含む変数を作成でき `$Error` ます。自動変数の動作には影響しません。 自動変数には、 `$Error` セッション内のすべてのコマンドからのエラーメッセージが含まれています。 やなどの配列表記を使用して、 `$a[0]` `$error[1,2]` 変数に格納されている特定のエラーを参照することができます。

> [!NOTE]
> カスタムエラー変数には、入れ子になった関数またはスクリプトの呼び出しからのエラーを含め、コマンドによって生成されたすべてのエラーが含まれます。

#### <a name="informationaction"></a>InformationAction

PowerShell 5.0 で導入されました。 使用されているコマンドまたはスクリプト内で、 **Informationaction** 共通パラメーターは、 `$InformationPreference` 既定では **SilentlyContinue** に設定されているユーザー設定変数の値よりも優先されます。 `Write-Information` **Informationaction** を含むスクリプトでを使用すると、 `Write-Information` **informationaction** パラメーターの値に応じて値が表示されます。 の詳細について `$InformationPreference` は、「 [about_Preference_Variables](./about_Preference_Variables.md)」を参照してください。

```yaml
Type: ActionPreference
Aliases: ia
Accepted values: Suspend, Ignore, Inquire, Continue, Stop, SilentlyContinue

Required: False
Position: Named
Default value: Depends on preference variable
Accept pipeline input: False
Accept wildcard characters: False
```

`-InformationAction:Stop` コマンドが発生したときに、コマンドまたはスクリプトを停止し `Write-Information` ます。

`-InformationAction:Ignore` 情報メッセージを表示せずに、コマンドの実行を続行します。 **SilentlyContinue** とは異なり、情報メッセージを完全には **無視** します。情報メッセージは情報ストリームに追加されません。

`-InformationAction:Inquire` コマンドで指定した情報メッセージを表示し、 `Write-Information` 続行するかどうかを確認します。

`-InformationAction:Continue` 情報メッセージを表示し、実行を継続します。

`-InformationAction:Suspend` は、ワークフローでのみ使用できるため、PowerShell Core ではサポートされていません。

`-InformationAction:SilentlyContinue` 情報メッセージが (既定値) 表示されないため、スクリプトは中断されることなく続行されます。

> [!NOTE]
> **Informationaction** パラメーターはをオーバーライドしますが、 `$InformationAction` スクリプトまたは関数を実行するためにコマンドでパラメーターを使用する場合は、ユーザー設定変数の値を置き換えません。

#### <a name="informationvariable"></a>InformationVariable

PowerShell 5.0 で導入されました。 使用されているコマンドまたはスクリプト内で、 **informationvariable** 共通パラメーターは、コマンドを追加して指定した文字列を変数に格納し `Write-Information` ます。 `Write-Information` 値は、 **Informationaction** 共通パラメーターの値に応じて表示されます。 **Informationaction** 共通パラメーターを追加しない場合は、ユーザー `Write-Information` 設定変数の値に応じて文字列が表示され `$InformationPreference` ます。 の詳細について `$InformationPreference` は、「 [about_Preference_Variables](./about_Preference_Variables.md)」を参照してください。

> [!NOTE]
> 情報変数には、入れ子になった関数またはスクリプトの呼び出しからの情報メッセージを含め、コマンドによって生成されたすべての情報メッセージが含まれます。

```yaml
Type: String
Aliases: iv

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

#### <a name="outbuffer"></a>OutBuffer

オブジェクトがパイプラインを介して送信される前に、バッファーに蓄積されるオブジェクトの数を決定します。 このパラメーターを省略すると、オブジェクトは生成時に送信されます。

```yaml
Type: Int32
Aliases: ob

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

このリソース管理パラメーターは、上級ユーザー向けに設計されています。 このパラメーターを使用すると、PowerShell はのバッチで次のコマンドレットにデータを送信 `OutBuffer + 1` します。

次の例では、コマンドレットを使用するプロセスブロックとの間でが交互に表示され `ForEach-Object` `Write-Host` ます。 ディスプレイは、2またはのバッチで交互に表示され `OutBuffer + 1` ます。

```powershell
1..4 | ForEach-Object {
        Write-Host "$($_): First"; $_
      } -OutBuffer 1 | ForEach-Object {
                        Write-Host "$($_): Second" }
```

```Output
1: First
2: First
1: Second
2: Second
3: First
4: First
3: Second
4: Second
```

#### <a name="outvariable"></a>OutVariable

パイプラインに沿って出力を送信するだけでなく、指定した変数のコマンドからの出力オブジェクトを格納します。

```yaml
Type: String
Aliases: ov

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

出力を変数に追加するには、既に格納されている可能性のある出力を置き換えるのではなく、 `+` 変数名の前にプラス記号 () を入力します。

たとえば、次のコマンドは、変数を作成 `$out` し、その中に process オブジェクトを格納します。

```powershell
Get-Process PowerShell -OutVariable out
```

次のコマンドを実行すると、process オブジェクトが変数に追加され `$out` ます。

```powershell
Get-Process iexplore -OutVariable +out
```

次のコマンドを実行すると、変数の内容が表示され `$out` ます。

```powershell
$out
```

> [!NOTE]
> **Outvariable** パラメーターによって作成された変数は `[System.Collections.ArrayList]` です。

#### <a name="pipelinevariable"></a>PipelineVariable

**PipelineVariable** は、現在のパイプライン要素の値を変数として格納します。これは、パイプラインを介してフローする任意の名前付きコマンドに使用します。

>[!NOTE]
> 高度な関数には、、、およびの3つまでのスクリプトブロックを含めることができ `begin` `process` `end` ます。 高度な関数で **PipelineVariable** パラメーターを使用する場合、関数の実行時に、最初に定義されたスクリプトブロックの値のみが変数に割り当てられます。 詳細については、「 [高度な関数](./about_functions_advanced.md)」を参照してください。
> PowerShell 7.2 では、この動作を修正します。

```yaml
Type: String
Aliases: pv

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

有効な値は文字列で、任意の変数名と同じです。

**PipelineVariable** の動作の例を次に示します。 この例では、 コマンド `Foreach-Object` の結果を変数に格納するために、PipelineVariable パラメーターがコマンドに追加されています。 1 ~ 10 の範囲の数値は最初のコマンドにパイプされ `Foreach-Object` 、その結果は **Left** という名前の変数に格納されます。

最初のコマンドの結果は、 `Foreach-Object` 2 番目のコマンドにパイプされ `Foreach-Object` ます。このコマンドは、最初のコマンドによって返されたオブジェクトをフィルター処理し `Foreach-Object` ます。 2番目のコマンドの結果は、 **Right** という名前の変数に格納されます。

3番目のコマンドでは、 `Foreach-Object` 変数によって表される最初の2つのパイプされたコマンドの結果 `Foreach-Object` が、乗算演算子を使用して処理されます。   このコマンドは、 **左** と **右** の変数に格納されているオブジェクトに乗算するように指示し、結果を "左辺の範囲メンバー * 右範囲メンバー = 製品" として表示するように指定します。

```powershell
1..10 | Foreach-Object -PipelineVariable Left -Process { $_ } |
  Foreach-Object -PV Right -Process { 1..10 } |
  Foreach-Object -Process { "$Left * $Right = " + ($Left*$Right) }
```

```output
1 * 1 = 1
1 * 2 = 2
1 * 3 = 3
1 * 4 = 4
1 * 5 = 5
...
```

#### <a name="verbose"></a>"詳細"

コマンドによって実行された操作に関する詳細情報を表示します。 この情報は、トレースまたはトランザクションログの情報に似ています。 このパラメーターは、コマンドによって詳細メッセージが生成された場合にのみ機能します。 たとえば、コマンドにコマンドレットが含まれている場合、このパラメーターは機能し `Write-Verbose` ます。

```yaml
Type: SwitchParameter
Aliases: vb

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

**Verbose** パラメーターは、現在のコマンドの変数の値よりも優先され `$VerbosePreference` ます。 変数の既定値 `$VerbosePreference` は **SilentlyContinue** であるため、詳細メッセージは既定では表示されません。

`-Verbose:$true` と同じ効果があります。 `-Verbose`

`-Verbose:$false` 詳細メッセージの表示を抑制します。 の値が SilentlyContinue (既定値) でない場合に、このパラメーターを使用し `$VerbosePreference` ます。 

#### <a name="warningaction"></a>WarningAction

コマンドレットがコマンドからの警告に応答する方法を決定します。 **Continue** が既定値です。 このパラメーターは、コマンドで警告メッセージが生成された場合にのみ機能します。 たとえば、コマンドにコマンドレットが含まれている場合、このパラメーターは機能し `Write-Warning` ます。

```yaml
Type: ActionPreference
Aliases: wa
Accepted values: Suspend, Ignore, Inquire, Continue, Stop, SilentlyContinue

Required: False
Position: Named
Default value: Depends on preference variable
Accept pipeline input: False
Accept wildcard characters: False
```

**Warnings action** パラメーターは、現在のコマンドの変数の値よりも優先され `$WarningPreference` ます。 変数の既定値 `$WarningPreference` は **Continue** であるため、 **warnings action** パラメーターを使用しない限り、警告が表示され、実行が続行されます。

`-WarningAction:Continue` 警告メッセージを表示し、コマンドの実行を続行します。 `Continue` は既定値です。

`-WarningAction:Inquire` 警告メッセージを表示し、実行を続行する前に確認メッセージを表示します。 この値はほとんど使用されません。

`-WarningAction:SilentlyContinue` 警告メッセージを表示せずに、コマンドの実行を続行します。

`-WarningAction:Stop` 警告メッセージを表示し、コマンドの実行を停止します。

> [!NOTE]
> **警告動作** パラメーターはをオーバーライドしますが、 `$WarningAction` スクリプトまたは関数を実行するためにコマンドでパラメーターを使用する場合は、ユーザー設定変数の値を置き換えません。

#### <a name="warningvariable"></a>WarningVariable

指定された変数にコマンドに関する警告を格納します。

```yaml
Type: String
Aliases: wv

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

警告がユーザーに表示されない場合でも、生成されたすべての警告は変数に保存されます。

警告を変数の内容に追加するには、既に格納されている可能性のある警告を置き換えるのではなく、 `+` 変数名の前にプラス記号 () を入力します。

たとえば、次のコマンドは変数を作成し、 `$a` 警告を格納します。

```powershell
Get-Process -Id 6 -WarningVariable a
```

次のコマンドは、変数に警告を追加し `$a` ます。

```powershell
Get-Process -Id 2 -WarningVariable +a
```

次のコマンドを実行すると、の内容が表示され `$a` ます。

```powershell
$a
```

このパラメーターを使用すると、特定のコマンドからの警告のみを含む変数を作成できます。 やなどの配列表記を使用して、 `$a[0]` `$warning[1,2]` 変数に格納されている特定の警告を参照することができます。

> [!NOTE]
> 警告変数には、入れ子になった関数またはスクリプトの呼び出しからの警告など、コマンドによって生成されるすべての警告が含まれます。
### <a name="risk-management-parameter-descriptions"></a>リスク管理のパラメーターの説明

#### <a name="whatif"></a>WhatIf

コマンドを実行する代わりに、コマンドの効果を説明するメッセージを表示します。

```yaml
Type: SwitchParameter
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

**WhatIf** パラメーターは、現在のコマンドの変数の値よりも優先され `$WhatIfPreference` ます。 変数の既定値 `$WhatIfPreference` は 0 (無効) であるため、whatif パラメーターを指定しないと **whatif** 動作は実行されません。 詳細については、「」を参照してください [about_Preference_Variables](about_Preference_Variables.md)

`-WhatIf:$true` と同じ効果があり `-WhatIf` ます。

`-WhatIf:$false` 変数の値が1の場合に生成される自動 WhatIf 動作を抑制し `$WhatIfPreference` ます。

たとえば、次のコマンドでは、 `-WhatIf` コマンドでパラメーターを使用し `Remove-Item` ます。

```powershell
Remove-Item Date.csv -WhatIf
```

PowerShell では、項目を削除する代わりに、実行する操作と影響を受ける項目が一覧表示されます。 このコマンドでは次の出力が生成されます。

```output
What if: Performing operation "Remove File" on
Target "C:\ps-test\date.csv".
```

#### <a name="confirm"></a>Confirm

コマンドを実行する前に確認メッセージを表示します。

```yaml
Type: SwitchParameter
Aliases: cf

Required: False
Position: Named
Default value: Depends on preference variable
Accept pipeline input: False
Accept wildcard characters: False
```

**Confirm** パラメーターは、現在のコマンドの変数の値よりも優先され `$ConfirmPreference` ます。 既定値は、true です。 詳細については、「」を参照してください [about_Preference_Variables](about_Preference_Variables.md)

`-Confirm:$true` と同じ効果があり `-Confirm` ます。

`-Confirm:$false` 自動確認を抑制します。これは、の値 `$ConfirmPreference` がコマンドレットの推定リスク以下である場合に発生します。

たとえば、次のコマンドでは、 **Confirm** パラメーターをコマンドと共に使用し `Remove-Item` ます。 項目を削除する前に、PowerShell によって実行される操作と影響を受ける項目が一覧表示され、承認が求められます。

```
PS C:\ps-test> Remove-Item tmp*.txt -Confirm

Confirm
Are you sure you want to perform this action?
Performing operation "Remove File" on Target " C:\ps-test\tmp1.txt
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend
[?] Help (default is "Y"):
```

Confirm response オプションは次のとおりです。

|Response       |結果                                                     |
|---------------|-----------------------------------------------------------|
|はい (Y)        |アクションを実行します。                                        |
|すべてはい (A) |すべてのアクションを実行し、後続のクエリの確認を抑制する|
|               |次のコマンドを実行します。                                          |
|いいえ (N):        |操作は実行しないでください。                                 |
|すべていいえ (L): |アクションを実行せず、その後の確認を抑制する |
|               |このコマンドを照会します。                                  |
|中断 (S):   |コマンドを一時停止し、一時的なセッションを作成します。          |
|ヘルプ (?)       |これらのオプションのヘルプを表示します。                            |

**Suspend** オプションを指定すると、コマンドが保留状態になり、一時的な入れ子になったセッションが作成されます。このセッションでは、[**確認**] オプションを選択する準備ができています。 入れ子になったセッションのコマンドプロンプトには、元の親コマンドの子操作であることを示すために、2つの追加のキャレット (>>) があります。 入れ子になったセッションでは、コマンドとスクリプトを実行できます。 入れ子になったセッションを終了し、元のコマンドの Confirm オプションに戻るには、「exit」と入力します。

次の例では、ユーザーがコマンドパラメーターのヘルプを確認している間に、 **Suspend** オプションを使用してコマンドを一時的に停止しています。 ユーザーは必要な情報を取得した後、"exit" を入力して入れ子になったプロンプトを終了し、確認クエリに対して [はい] (y) の応答を選択します。

```
PS C:\ps-test> New-Item -ItemType File -Name Test.txt -Confirm

Confirm
Are you sure you want to perform this action?

Performing operation "Create File" on Target "Destination:
C:\ps-test\test.txt".
[Y] Yes [A] Yes to All [N] No [L] No to All [S] Suspend [?] Help (default
is "Y"): s

PS C:\ps-test> Get-Help New-Item -Parameter ItemType

-ItemType <string>
Specifies the provider-specified type of the new item.

Required?                    false
Position?                    named
Default value
Accept pipeline input?       true (ByPropertyName)
Accept wildcard characters?  false

PS C:\ps-test> exit

Confirm
Are you sure you want to perform this action?
Performing operation "Create File" on Target "Destination: C:\ps-test\test
.txt".
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (defau
lt is "Y"): y

Directory: C:\ps-test

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a---         8/27/2010   2:41 PM          0 test.txt
```

## <a name="keywords"></a>KEYWORDS

about_Common_Parameters

## <a name="see-also"></a>関連項目

[about_Preference_Variables](about_Preference_Variables.md)

[Write-Debug](xref:Microsoft.PowerShell.Utility.Write-Debug)

[Write-Warning](xref:Microsoft.PowerShell.Utility.Write-Warning)

[Write-Error](xref:Microsoft.PowerShell.Utility.Write-Error)

[Write-Verbose](xref:Microsoft.PowerShell.Utility.Write-Verbose)
