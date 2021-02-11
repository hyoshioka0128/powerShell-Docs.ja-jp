---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 3/22/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-executionpolicy?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-ExecutionPolicy
ms.openlocfilehash: 96d840d68240d069511afa44753789f472618e19
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347246"
---
# Get-ExecutionPolicy

## 概要
現在のセッションの実行ポリシーを取得します。

## SYNTAX

### すべて

```
Get-ExecutionPolicy [[-Scope] <ExecutionPolicyScope>] [-List] [<CommonParameters>]
```

## Description

各スコープの実行ポリシーを優先順位に従って表示するには、を使用 `Get-ExecutionPolicy -List` します。 PowerShell セッションの有効な実行ポリシーを確認するには、 `Get-ExecutionPolicy` パラメーターなしで使用します。

有効な実行ポリシーは、およびグループポリシー設定によって設定される実行ポリシーによって決定され `Set-ExecutionPolicy` ます。

詳細については、「[about_Execution_Policies](../Microsoft.PowerShell.Core/about/about_Execution_Policies.md)」を参照してください。

## 例

### 例 1: すべての実行ポリシーを取得する

このコマンドは、各スコープの実行ポリシーを優先順位順に表示します。

```powershell
Get-ExecutionPolicy -List
```

```Output
Scope          ExecutionPolicy
-----          ---------------
MachinePolicy  Undefined
UserPolicy     Undefined
Process        Undefined
CurrentUser    AllSigned
LocalMachine   Undefined
```

この `Get-ExecutionPolicy` コマンドレットは、 **List** パラメーターを使用して、各スコープの実行ポリシーを表示します。

### 例 2: 実行ポリシーを設定する

この例では、ローカルコンピューターの実行ポリシーを設定する方法を示します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine
Get-ExecutionPolicy -List
```

```Output
        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       AllSigned
 LocalMachine    RemoteSigned
```

コマンドレットでは、 `Set-ExecutionPolicy` **set-executionpolicy** パラメーターを使用して、 **RemoteSigned** ポリシーを指定します。 **Scope** パラメーターは、既定のスコープ値である **LocalMachine** を指定します。 実行ポリシー設定を表示するには、 `Get-ExecutionPolicy` コマンドレットを **List** パラメーターと共に使用します。

### 例 3: 有効な実行ポリシーを取得する

この例では、PowerShell セッションの有効な実行ポリシーを表示する方法を示します。

```
PS> Get-ExecutionPolicy -List

        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       AllSigned
 LocalMachine    RemoteSigned

PS> Get-ExecutionPolicy

AllSigned
```

この `Get-ExecutionPolicy` コマンドレットは、 **List** パラメーターを使用して、各スコープの実行ポリシーを表示します。 `Get-ExecutionPolicy`コマンドレットは、有効な実行ポリシー **AllSigned** を表示するパラメーターなしで実行されます。

### 例 4: 実行ポリシーを変更せずにスクリプトを実行するためのブロックを解除する

この例では、 **RemoteSigned** 実行ポリシーを使用して、署名されていないスクリプトを実行できないようにする方法を示します。

コマンドレットを使用 **する前** に、スクリプトのコードを読み取って安全であることを確認することをお勧めし `Unblock-File` ます。 コマンドレットを実行すると、スクリプトのブロックが解除され `Unblock-File` ますが、実行ポリシーは変更されません。

```
PS> Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope LocalMachine

PS> Get-ExecutionPolicy

RemoteSigned

PS> .\Start-ActivityTracker.ps1

.\Start-ActivityTracker.ps1 : File .\Start-ActivityTracker.ps1 cannot be loaded.
The file .\Start-ActivityTracker.ps1 is not digitally signed.
The script will not execute on the system.
For more information, see about_Execution_Policies at https://go.microsoft.com/fwlink/?LinkID=135170.
At line:1 char:1
+ .\Start-ActivityTracker.ps1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : NotSpecified: (:) [], PSSecurityException
+ FullyQualifiedErrorId : UnauthorizedAccess

PS> Unblock-File -Path .\Start-ActivityTracker.ps1

PS> Get-ExecutionPolicy

RemoteSigned

PS> .\Start-ActivityTracker.ps1

Task 1:
```

は、 `Set-ExecutionPolicy` **set-executionpolicy** パラメーターを使用して **RemoteSigned** ポリシーを指定します。 ポリシーは、既定のスコープである **LocalMachine** に設定されています。

この `Get-ExecutionPolicy` コマンドレットは、 **RemoteSigned** が現在の PowerShell セッションの有効な実行ポリシーであることを示しています。

**Start-ActivityTracker.ps1** スクリプトが現在のディレクトリから実行されます。 スクリプトがデジタル署名されていないため、スクリプトは **RemoteSigned** によってブロックされます。

この例では、スクリプトのコードが確認され、実行の安全性が確認されています。 コマンドレットでは、 `Unblock-File` **Path** パラメーターを使用してスクリプトのブロックを解除します。

が実行ポリシーを変更していないことを確認するには `Unblock-File` 、 `Get-ExecutionPolicy` 有効な実行ポリシー **RemoteSigned** を表示します。

スクリプト **Start-ActivityTracker.ps1** は、現在のディレクトリから実行されます。 スクリプトは、コマンドレットによってブロック解除されたため、実行を開始し `Unblock-File` ます。

## PARAMETERS

### -List

セッションのすべての実行ポリシー値を取得し、優先順位順に一覧表示します。 既定では、は `Get-ExecutionPolicy` 有効な実行ポリシーのみを取得します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -スコープ

実行ポリシーの影響を受けるスコープを指定します。

有効な実行ポリシーは、次のように優先順位によって決まります。

- **Machinepolicy** 。 コンピューターのすべてのユーザーに対してグループポリシーによって設定されます。
- **Userpolicy** 。 コンピューターの現在のユーザーのグループポリシーによって設定されます。
- **プロセス** 。 現在の PowerShell セッションにのみ影響します。
- **CurrentUser** 。 現在のユーザーのみに影響します。
- **LocalMachine** 。 コンピューターのすべてのユーザーに影響する既定のスコープ。

```yaml
Type: Microsoft.PowerShell.ExecutionPolicyScope
Parameter Sets: (All)
Aliases:
Accepted values: CurrentUser, LocalMachine, MachinePolicy, Process, UserPolicy

Required: False
Position: 0
Default value: Effective execution policy
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

`Get-ExecutionPolicy` は、パイプラインからの入力を受け入れません。

## 出力

### Microsoft.PowerShell.ExecutionPolicy

コマンドレットは、Linux および macOS プラットフォームでは常に **無制限** を返します。

## 注

実行ポリシーは、PowerShell のセキュリティ戦略の一部です。 実行ポリシーは、PowerShell プロファイルなどの構成ファイルを読み込むか、スクリプトを実行できるかどうかを決定します。 また、スクリプトを実行する前に、そのスクリプトをデジタル署名する必要があるかどうかを示します。

## 関連リンク

[about_Execution_Policies](../Microsoft.PowerShell.Core/about/about_Execution_Policies.md)

[about_Group_Policy_Settings](../Microsoft.PowerShell.Core/About/about_Group_Policy_Settings.md)

[Get-AuthenticodeSignature](Get-AuthenticodeSignature.md)

[Set-AuthenticodeSignature](Set-AuthenticodeSignature.md)

[Set-ExecutionPolicy](Set-ExecutionPolicy.md)
