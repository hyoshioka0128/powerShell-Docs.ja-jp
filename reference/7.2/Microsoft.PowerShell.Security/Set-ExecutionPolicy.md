---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 03/22/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-ExecutionPolicy
ms.openlocfilehash: 4c45267113ff7b8a870d5a05bf3e4ab922f7e9c6
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605185"
---
# Set-ExecutionPolicy

## 概要
Windows コンピューターの PowerShell 実行ポリシーを設定します。

## SYNTAX

### すべて

```
Set-ExecutionPolicy [-ExecutionPolicy] <ExecutionPolicy> [[-Scope] <ExecutionPolicyScope>] [-Force]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Set-ExecutionPolicy` Windows コンピューターの PowerShell 実行ポリシーを変更します。 詳細については、「[about_Execution_Policies](../Microsoft.PowerShell.Core/about/about_Execution_Policies.md)」を参照してください。

Windows 以外のコンピューターの PowerShell 6.0 以降では、既定の実行ポリシーは **無制限** であり、変更することはできません。 `Set-ExecutionPolicy`コマンドレットは使用できますが、PowerShell にはサポートされていないコンソールメッセージが表示されます。

実行ポリシーは、PowerShell のセキュリティ戦略の一部です。 実行ポリシーは、PowerShell プロファイルなどの構成ファイルを読み込むか、スクリプトを実行できるかどうかを決定します。 また、スクリプトを実行する前に、そのスクリプトをデジタル署名する必要があるかどうかを示します。

`Set-ExecutionPolicy`コマンドレットの既定のスコープは **LocalMachine** で、コンピューターを使用するすべてのユーザーに影響します。 **LocalMachine** の実行ポリシーを変更するには、[**管理者として実行**] で PowerShell を起動します。

各スコープの実行ポリシーを優先順位に従って表示するには、を使用 `Get-ExecutionPolicy -List` します。 PowerShell セッションの有効な実行ポリシーを確認するには、 `Get-ExecutionPolicy` パラメーターなしで使用します。

## 例

### 例 1: 実行ポリシーを設定する

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
  CurrentUser    RemoteSigned
 LocalMachine    RemoteSigned
```

コマンドレットでは、 `Set-ExecutionPolicy` **set-executionpolicy** パラメーターを使用して、 **RemoteSigned** ポリシーを指定します。 **Scope** パラメーターは、既定のスコープ値である **LocalMachine** を指定します。 実行ポリシー設定を表示するには、 `Get-ExecutionPolicy` コマンドレットを **List** パラメーターと共に使用します。

### 例 2: グループポリシーと競合する実行ポリシーを設定する

このコマンドは、 **LocalMachine** スコープの実行ポリシーを **Restricted** に設定しようとします。
**LocalMachine** はより制限されていますが、グループポリシーと競合するため、有効なポリシーではありません。 **制限** されたポリシーは、レジストリハイブ **HKEY_LOCAL_MACHINE** に書き込まれます。

```
PS> Set-ExecutionPolicy -ExecutionPolicy Restricted -Scope LocalMachine

Set-ExecutionPolicy : PowerShell updated your local preference successfully, but the setting is
overridden by the Group Policy applied to your system. Due to the override, your shell will retain
its current effective execution policy of "AllSigned". Contact your Group Policy administrator for
more information. At line:1 char:20 + Set-ExecutionPolicy <<<< restricted

PS> Get-ChildItem -Path HKLM:\SOFTWARE\Microsoft\PowerShell\1\ShellIds

    Hive: HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShell\1\ShellIds

Name                    Property
----                    --------
Microsoft.PowerShell    Path            : C:\Windows\System32\WindowsPowerShell\v1.0\powershell.exe
                        ExecutionPolicy : Restricted
ScriptedDiagnostics     ExecutionPolicy : Unrestricted
```

コマンドレットでは、 `Set-ExecutionPolicy` **set-executionpolicy** パラメーターを使用して **制限** されたポリシーを指定します。 **Scope** パラメーターは、既定のスコープ値である **LocalMachine** を指定します。
コマンドレットでは、 `Get-ChildItem` **Path** パラメーターを **HKLM** プロバイダーと共に使用して、レジストリの場所を指定します。

### 例 3: リモートコンピューターからローカルコンピューターに実行ポリシーを適用する

このコマンドは、リモートコンピューターから実行ポリシーオブジェクトを取得し、ローカルコンピューターのポリシーを設定します。 `Get-ExecutionPolicy`**Microsoft.PowerShell.ExecutionPolicy** オブジェクトをパイプラインの下に送信します。 `Set-ExecutionPolicy` パイプラインの入力を受け入れ、 **set-executionpolicy** パラメーターを必要としません。

```
PS> Invoke-Command -ComputerName Server01 -ScriptBlock { Get-ExecutionPolicy } | Set-ExecutionPolicy
```

`Invoke-Command`コマンドレットは、ローカルコンピューターで実行され、リモートコンピューターに **ScriptBlock** を送信します。 **ComputerName** パラメーターは、リモートコンピューター **Server01** を指定します。 **ScriptBlock** パラメーターは、 `Get-ExecutionPolicy` リモートコンピューター上で実行されます。 `Get-ExecutionPolicy`オブジェクトは、パイプライン内でに送信され `Set-ExecutionPolicy` ます。
`Set-ExecutionPolicy` ローカルコンピューターの既定のスコープである **LocalMachine** に実行ポリシーを適用します。

### 例 4: 実行ポリシーのスコープを設定する

この例では、指定されたスコープ ( **CurrentUser**) の実行ポリシーを設定する方法を示します。 **CurrentUser** スコープは、このスコープを設定したユーザーにのみ影響します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy AllSigned -Scope CurrentUser
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

`Set-ExecutionPolicy`**set-executionpolicy** パラメーターを使用して、 **AllSigned** ポリシーを指定します。
**スコープ** パラメーターで **CurrentUser** を指定します。 実行ポリシー設定を表示するには、 `Get-ExecutionPolicy` コマンドレットを **List** パラメーターと共に使用します。

ユーザーの有効な実行ポリシーが **AllSigned** になります。

### 例 5: 現在のユーザーの実行ポリシーを削除する

この例では、 **未定義** の実行ポリシーを使用して、指定されたスコープの実行ポリシーを削除する方法を示します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy Undefined -Scope CurrentUser
Get-ExecutionPolicy -List
```

```Output
        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser       Undefined
 LocalMachine    RemoteSigned
```

`Set-ExecutionPolicy`**set-executionpolicy** パラメーターを使用して、**未定義** のポリシーを指定します。
**スコープ** パラメーターで **CurrentUser** を指定します。 実行ポリシー設定を表示するには、 `Get-ExecutionPolicy` コマンドレットを **List** パラメーターと共に使用します。

### 例 6: 現在の PowerShell セッションの実行ポリシーを設定する

**プロセス** のスコープは、現在の PowerShell セッションにのみ影響します。 実行ポリシーは環境変数に保存され、 `$env:PSExecutionPolicyPreference` セッションが閉じられると削除されます。

```powershell
Set-ExecutionPolicy -ExecutionPolicy AllSigned -Scope Process
```

```Output
        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       AllSigned
  CurrentUser    RemoteSigned
 LocalMachine    RemoteSigned
```

は、 `Set-ExecutionPolicy` **set-executionpolicy** パラメーターを使用して **AllSigned** ポリシーを指定します。 **スコープ** のパラメーターは、値の **処理** を指定します。 実行ポリシー設定を表示するには、 `Get-ExecutionPolicy` コマンドレットを **List** パラメーターと共に使用します。

### 例 7: 実行ポリシーを変更せずにスクリプトを実行するためのブロックを解除する

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

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ExecutionPolicy

実行ポリシーを指定します。 グループポリシーがなく、各スコープの実行ポリシーが **Undefined** に設定されている場合は、すべてのユーザーに対して有効なポリシー **になります** 。

許容される実行ポリシーの値は次のとおりです。

- **AllSigned**。 では、すべてのスクリプトと構成ファイルが、信頼された発行元によって署名されている必要があります (ローカルコンピューターで作成されたスクリプトを含む)。
- **バイパス**。 何もブロックされず、警告やプロンプトは表示されません。
- **Default**。 既定の実行ポリシーを設定します。 Windows クライアントの場合は、Windows サーバーの場合は **RemoteSigned** に **制限** されます。
- **RemoteSigned**。 では、インターネットからダウンロードしたすべてのスクリプトと構成ファイルが信頼された発行元によって署名されている必要があります。 Windows server コンピューターの既定の実行ポリシー。
- **制限付き**。 構成ファイルの読み込みやスクリプトの実行を行いません。 既定の実行ポリシー Windows クライアントコンピューター。
- **未定義**。 スコープに対して実行ポリシーが設定されていません。 グループポリシーによって設定されていないスコープから、割り当てられた実行ポリシーを削除します。 すべてのスコープの実行ポリシーが **定義** されていない場合は、有効な実行ポリシーが **制限** されます。
- **無制限**。 PowerShell 6.0 以降では、これは Windows 以外のコンピューターの既定の実行ポリシーであり、変更することはできません。 すべての構成ファイルを読み込んで、すべてのスクリプトを実行します。 インターネットからダウンロードした未署名のスクリプトを実行すると、実行前にアクセス許可を求めるメッセージが表示されます。

```yaml
Type: Microsoft.PowerShell.ExecutionPolicy
Parameter Sets: (All)
Aliases:
Accepted values: AllSigned, Bypass, Default, RemoteSigned, Restricted, Undefined, Unrestricted

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -Force

すべての確認プロンプトを表示しないようにします。 予期しない結果が発生しないようにするには、このパラメーターで注意してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -スコープ

実行ポリシーの影響を受けるスコープを指定します。 既定のスコープは **LocalMachine** です。

有効な実行ポリシーは、次のように優先順位によって決まります。

- **Machinepolicy**。 コンピューターのすべてのユーザーに対してグループポリシーによって設定されます。
- **Userpolicy**。 コンピューターの現在のユーザーのグループポリシーによって設定されます。
- **プロセス**。 現在の PowerShell セッションにのみ影響します。
- **CurrentUser**。 現在のユーザーのみに影響します。
- **LocalMachine**。 コンピューターのすべてのユーザーに影響する既定のスコープ。

**プロセス** のスコープは、現在の PowerShell セッションにのみ影響します。 実行ポリシーは、レジストリではなく、環境変数に保存され `$env:PSExecutionPolicyPreference` ます。 PowerShell セッションが終了すると、変数と値が削除されます。

**CurrentUser** スコープの実行ポリシーは、レジストリハイブ **HKEY_LOCAL_USER** に書き込まれます。

**LocalMachine** スコープの実行ポリシーは、レジストリハイブ **HKEY_LOCAL_MACHINE** に書き込まれます。

```yaml
Type: Microsoft.PowerShell.ExecutionPolicyScope
Parameter Sets: (All)
Aliases:
Accepted values: CurrentUser, LocalMachine, MachinePolicy, Process, UserPolicy

Required: False
Position: 1
Default value: LocalMachine
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### Microsoft.PowerShell.ExecutionPolicy、System.string

パイプを使用して、実行ポリシーオブジェクトまたは実行ポリシーの名前を含む文字列をにパイプすることができ `Set-ExecutionPolicy` ます。

## 出力

### なし

`Set-ExecutionPolicy` は出力を返しません。

## 注

`Set-ExecutionPolicy`**machinepolicy** および **userpolicy** スコープは、グループポリシーによって設定されているため、変更しません。

`Set-ExecutionPolicy` では、ユーザー設定がポリシーよりも制限が厳しい場合でも、グループポリシーはオーバーライドされません。

コンピューターまたはユーザーに対してグループポリシーの **スクリプト実行** を有効にすると、ユーザー設定は保存されますが、有効ではありません。 PowerShell では、競合について説明するメッセージが表示されます。

## 関連リンク

[about_Execution_Policies](../Microsoft.PowerShell.Core/About/about_Execution_Policies.md)

[about_Group_Policy_Settings](../Microsoft.PowerShell.Core/About/about_Group_Policy_Settings.md)

[about_Providers](../Microsoft.PowerShell.Core/About/about_Providers.md)

[Get-AuthenticodeSignature](Get-AuthenticodeSignature.md)

[Get-ChildItem](../Microsoft.PowerShell.Management/Get-ChildItem.md)

[Get-ExecutionPolicy](Get-ExecutionPolicy.md)

[Invoke-Command](../Microsoft.PowerShell.Core/Invoke-Command.md)

[Set-AuthenticodeSignature](Set-AuthenticodeSignature.md)

[Unblock-File](../Microsoft.PowerShell.Utility/Unblock-File.md)

