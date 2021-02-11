---
description: PowerShell の実行ポリシーについて説明し、その管理方法について説明します。
Locale: en-US
ms.date: 08/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Execution_Policies
ms.openlocfilehash: 1a2b8458b39233f96d47a52e3fea21b31e9b3c42
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599548"
---
# <a name="about-execution-policies"></a>実行ポリシーについて

## <a name="short-description"></a>簡単な説明
PowerShell の実行ポリシーについて説明し、その管理方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell の実行ポリシーは、PowerShell が構成ファイルを読み込み、スクリプトを実行するときの条件を制御する安全性機能です。 この機能は、悪意のあるスクリプトの実行を防止するのに役立ちます。

Windows コンピューターでは、ローカルコンピューター、現在のユーザー、または特定のセッションの実行ポリシーを設定できます。 グループポリシー設定を使用して、コンピューターとユーザーの実行ポリシーを設定することもできます。

ローカルコンピューターと現在のユーザーの実行ポリシーは、レジストリに格納されます。 PowerShell プロファイルで実行ポリシーを設定する必要はありません。
特定のセッションの実行ポリシーはメモリにのみ格納され、セッションが閉じられると失われます。

実行ポリシーは、ユーザーの操作を制限するセキュリティシステムではありません。 たとえば、スクリプトを実行できない場合、ユーザーはコマンドラインでスクリプトの内容を入力することで、ポリシーを簡単にバイパスできます。 代わりに、実行ポリシーを使用して、ユーザーが基本的な規則を設定し、意図せずに違反しないようにすることができます。

Windows 以外のコンピューターでは、既定の実行ポリシーは **無制限** で、変更することはできません。 `Set-ExecutionPolicy`コマンドレットは使用できますが、PowerShell にはサポートされていないコンソールメッセージが表示されます。 `Get-ExecutionPolicy`Windows 以外のプラットフォームでは **無制限** が返されますが、これらのプラットフォームには windows セキュリティゾーンが実装されていないため、この動作は実際には **バイパス** に一致します。

## <a name="powershell-execution-policies"></a>PowerShell 実行ポリシー

これらのポリシーの適用は、Windows プラットフォームでのみ実行されます。 PowerShell の実行ポリシーは次のとおりです。

### <a name="allsigned"></a>AllSigned

- スクリプトを実行できます。
- ローカル コンピューター上に記述するスクリプトを含め、すべてのスクリプトと構成ファイルが信頼された発行元によって署名されている必要があります。
- 信頼済みまたは信頼できないものとして分類されていない発行元からのスクリプトを実行する前に、プロンプトを表示します。
- 署名されたが悪意のあるスクリプトを実行するリスク。

### <a name="bypass"></a>バイパス

- 何もブロックされず、警告やプロンプトは表示されません。
- この実行ポリシーは、PowerShell スクリプトが大規模なアプリケーションに組み込まれる構成や、PowerShell が独自のセキュリティモデルを持つプログラムの基礎となる構成用に設計されています。

### <a name="default"></a>Default

- 既定の実行ポリシーを設定します。
- Windows クライアントでは **制限** されています。
- Windows サーバーの場合は **RemoteSigned** 。

### <a name="remotesigned"></a>RemoteSigned

- Windows server コンピューターの既定の実行ポリシー。
- スクリプトを実行できます。
- では、電子メールやインスタントメッセージングプログラムを含む、インターネットからダウンロードされるスクリプトと構成ファイルに対する信頼された発行元からのデジタル署名が必要です。
- では、ローカルコンピューター上に記述され、インターネットからダウンロードされないスクリプトに対してデジタル署名は必要ありません。
- コマンドレットなどを使用して、スクリプトのブロックが解除された場合に、インターネットからダウンロードされ、署名されていないスクリプトを実行し `Unblock-File` ます。
- インターネット以外のソースからの署名されていないスクリプトや、悪意のあると考えられる署名済みスクリプトを実行するリスク。

### <a name="restricted"></a>制限付き

- Windows クライアントコンピューターの既定の実行ポリシー。
- 個々のコマンドを許可しますが、スクリプトは許可しません。
- 書式設定ファイル、構成ファイル () `.ps1xml` 、モジュールスクリプトファイル ()、PowerShell プロファイル () など、すべてのスクリプトファイルの実行を禁止 `.psm1` `.ps1` します。

### <a name="undefined"></a>未定義。

- 現在のスコープには実行ポリシーが設定されていません。
- すべてのスコープの実行ポリシーが **定義** されていない場合、有効な実行ポリシーは windows クライアントおよび windows Server 用 **RemoteSigned** に **制限** されます。

### <a name="unrestricted"></a>無制限

- Windows 以外のコンピューターの既定の実行ポリシーを変更することはできません。
- 署名されていないスクリプトを実行できます。 悪意のあるスクリプトを実行するリスクがあります。
- ローカルイントラネットゾーンからのものではないスクリプトと構成ファイルを実行する前に、ユーザーに警告します。

> [!NOTE]
> インターネットパスから汎用名前付け規則 (UNC) パスを区別しないシステムでは、UNC パスによって識別されるスクリプトは、 **RemoteSigned** 実行ポリシーでの実行が許可されていない可能性があります。

## <a name="execution-policy-scope"></a>実行ポリシーのスコープ

特定のスコープでのみ有効な実行ポリシーを設定できます。

**スコープ** の有効な値は **、machinepolicy**、 **userpolicy**、 **Process**、 **CurrentUser**、および **LocalMachine** です。 **LocalMachine** は、実行ポリシーを設定するときの既定値です。

**スコープ** 値は、優先順位順に一覧表示されます。 より制限の厳しいポリシーが優先順位の低いレベルで設定されている場合でも、現在のセッションでは、優先されるポリシーが有効になります。

詳細については、「 [set-executionpolicy](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)」を参照してください。

### <a name="machinepolicy"></a>MachinePolicy

コンピューターのすべてのユーザーに対してグループポリシーによって設定されます。

### <a name="userpolicy"></a>UserPolicy

コンピューターの現在のユーザーのグループポリシーによって設定されます。

### <a name="process"></a>プロセス

**プロセス** のスコープは、現在の PowerShell セッションにのみ影響します。 実行ポリシーは、レジストリではなく、環境変数に保存され `$env:PSExecutionPolicyPreference` ます。 PowerShell セッションが終了すると、変数と値が削除されます。

### <a name="currentuser"></a>CurrentUser

実行ポリシーは、現在のユーザーのみに影響します。 これは **HKEY_CURRENT_USER** レジストリサブキーに格納されています。

### <a name="localmachine"></a>LocalMachine

実行ポリシーは、現在のコンピューター上のすべてのユーザーに影響します。 これは **HKEY_LOCAL_MACHINE** レジストリサブキーに格納されています。

## <a name="managing-the-execution-policy-with-powershell"></a>PowerShell を使用した実行ポリシーの管理

現在の PowerShell セッションに対して有効な実行ポリシーを取得するには、コマンドレットを使用し `Get-ExecutionPolicy` ます。

次のコマンドは、有効な実行ポリシーを取得します。

```powershell
Get-ExecutionPolicy
```

現在のセッションに影響するすべての実行ポリシーを取得し、優先順位で表示するには、次のようにします。

```powershell
Get-ExecutionPolicy -List
```

結果は次のサンプル出力のようになります。

```Output
        Scope ExecutionPolicy
        ----- ---------------
MachinePolicy       Undefined
   UserPolicy       Undefined
      Process       Undefined
  CurrentUser    RemoteSigned
 LocalMachine       AllSigned
```

この場合、有効な実行ポリシーは、現在のユーザーの実行ポリシーがローカルコンピューターに設定されている実行ポリシーよりも優先されるため、 **RemoteSigned** になります。

特定のスコープに対して設定された実行ポリシーを取得するには、の **scope** パラメーターを使用し `Get-ExecutionPolicy` ます。

たとえば、次のコマンドは、 **CurrentUser** スコープの実行ポリシーを取得します。

```powershell
Get-ExecutionPolicy -Scope CurrentUser
```

### <a name="change-the-execution-policy"></a>実行ポリシーを変更する

Windows コンピューターの PowerShell 実行ポリシーを変更するには、コマンドレットを使用し `Set-ExecutionPolicy` ます。 変更はすぐに有効になります。 PowerShell を再起動する必要はありません。

スコープ **LocalMachine** または **CurrentUser** の実行ポリシーを設定した場合、変更はレジストリに保存され、再度変更するまで有効のままになります。

**プロセス** スコープの実行ポリシーを設定しても、レジストリには保存されません。 実行ポリシーは、現在のプロセスと子プロセスが閉じられるまで保持されます。

> [!NOTE]
> Windows Vista 以降のバージョンの Windows では、ローカルコンピューターの **LocalMachine** スコープの実行ポリシーを変更するコマンドを実行するには、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

実行ポリシーを変更するには:

```powershell
Set-ExecutionPolicy -ExecutionPolicy <PolicyName>
```

次に例を示します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

特定のスコープで実行ポリシーを設定するには、次のようにします。

```powershell
Set-ExecutionPolicy -ExecutionPolicy <PolicyName> -Scope <scope>
```

次に例を示します。

```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

実行ポリシーを変更するコマンドは成功しても、有効な実行ポリシーは変更されません。

たとえば、ローカルコンピューターの実行ポリシーを設定するコマンドは成功しますが、現在のユーザーの実行ポリシーによって上書きされます。

### <a name="remove-the-execution-policy"></a>実行ポリシーを削除します。

特定のスコープの実行ポリシーを削除するには、実行ポリシーを **Undefined** に設定します。

たとえば、ローカルコンピューターのすべてのユーザーの実行ポリシーを削除するには、次のようにします。

```powershell
Set-ExecutionPolicy -ExecutionPolicy Undefined -Scope LocalMachine
```

**スコープ** の実行ポリシーを削除するには、次のようにします。

```powershell
Set-ExecutionPolicy -ExecutionPolicy Undefined -Scope CurrentUser
```

どのスコープにも実行ポリシーが設定されていない場合は、有効な実行ポリシーが **制限** されます。これは、Windows クライアントの既定の設定です。

### <a name="set-a-different-policy-for-one-session"></a>1つのセッションに対して別のポリシーを設定する

**pwsh.exe** の **set-executionpolicy** パラメーターを使用して、新しい PowerShell セッションの実行ポリシーを設定できます。 ポリシーは、現在のセッションと子セッションにのみ影響します。

新しいセッションの実行ポリシーを設定するには、コマンドライン ( **cmd.exe** や powershell など) で powershell を起動し、 **pwsh.exe** の **set-executionpolicy** パラメーターを使用して実行ポリシーを設定します。

次に例を示します。

```powershell
pwsh.exe -ExecutionPolicy AllSigned
```

設定した実行ポリシーはレジストリに格納されていません。 代わりに、環境変数に格納され `$env:PSExecutionPolicyPreference` ます。 変数は、ポリシーが設定されているセッションを閉じると削除されます。 変数の値を編集することによって、ポリシーを変更することはできません。

セッション中に、セッションに対して設定されている実行ポリシーが、ローカルコンピューターまたは現在のユーザーのレジストリで設定されている実行ポリシーよりも優先されます。 ただし、グループポリシーを使用して設定された実行ポリシーより優先されることはありません。

## <a name="use-group-policy-to-manage-execution-policy"></a>グループポリシーを使用して実行ポリシーを管理する

**[スクリプトのグループポリシー実行を有効** にする] 設定を使用すると、企業内のコンピューターの実行ポリシーを管理できます。 グループポリシー設定は、すべてのスコープの PowerShell で設定されている実行ポリシーよりも優先されます。

**[スクリプト実行の有効化**] ポリシー設定は次のとおりです。

- **[スクリプトの実行を有効** にする] を無効にした場合、スクリプトは実行されません。 これは、 **制限** された実行ポリシーに相当します。
- **[スクリプトの実行** を有効にする] を有効にした場合は、実行ポリシーを選択できます。 グループポリシー設定は、次の実行ポリシー設定と同じです。

  | グループ ポリシー                                  | 実行ポリシー |
  | --------------------------------------------- | ---------------- |
  | すべてのスクリプトを許可する                             | 無制限     |
  | ローカル スクリプトおよびリモートの署名済みスクリプトを許可する | RemoteSigned     |
  | 署名済みスクリプトのみ許可する                     | AllSigned        |

- **[スクリプトの実行を有効にする**] が構成されていない場合、効果はありません。 PowerShell で設定された実行ポリシーが有効になります。

PowerShellExecutionPolicy ファイルと PowerShellExecutionPolicy ファイルは、次のパスのグループポリシーエディターの [コンピューターの構成] ノードと [ユーザーの構成] ノードに **[スクリプト実行を有効に** する] ポリシーを追加します。

Windows XP および Windows Server 2003 の場合:

管理用テンプレート \Windows コンポーネント \Windows PowerShell

Windows Vista 以降のバージョンの場合:

管理 Templates\Classic 管理用テンプレート \
Windows \Windows PowerShell

[コンピュータの構成」ノードで設定されたポリシーは、[ユーザーの構成」ノードで設定されたポリシーよりも優先されます。

詳細については、「[about_Group_Policy_Settings](about_Group_Policy_Settings.md)」をご覧ください。

### <a name="execution-policy-precedence"></a>実行ポリシーの優先順位

PowerShell は、セッションの有効な実行ポリシーを決定するときに、次の優先順位で実行ポリシーを評価します。

- グループポリシー: MachinePolicy
- グループポリシー: UserPolicy
- 実行ポリシー: Process (または `pwsh.exe -ExecutionPolicy` )
- 実行ポリシー: CurrentUser
- 実行ポリシー: LocalMachine

## <a name="manage-signed-and-unsigned-scripts"></a>署名されたスクリプトと署名されていないスクリプトの管理

Windows では、Internet Explorer や Microsoft Edge などのプログラムは、ダウンロードされたファイルに代替データストリームを追加します。 これにより、ファイルが "インターネットからの受信" としてマークされます。 PowerShell の実行ポリシーが **RemoteSigned** の場合、powershell では、電子メールやインスタントメッセージングプログラムを含む、インターネットからダウンロードされた未署名のスクリプトは実行されません。

スクリプトに署名するか、実行ポリシーを変更せずに、署名されていないスクリプトを実行することができます。

PowerShell 3.0 以降では、コマンドレットの **Stream** パラメーターを使用して、 `Get-Item` インターネットからダウンロードされたためにブロックされているファイルを検出することができます。 `Unblock-File`コマンドレットを使用してスクリプトのブロックを解除し、PowerShell で実行できるようにします。

詳細については、「 [about_Signing](about_Signing.md)、 [取得項目](xref:Microsoft.PowerShell.Management.Get-Item)、および [ブロック解除-ファイル](xref:Microsoft.PowerShell.Utility.Unblock-File)」を参照してください。

> [!NOTE]
> 他の方法でファイルをダウンロードすると、ファイルはインターネットゾーンからのものとしてマークされない場合があります。 次に例をいくつか示します。
>
> - `curl.exe`
> - `Invoke-RestMethod`
> - `Invoke-WebRequest`

## <a name="execution-policy-on-windows-server-core-and-window-nano-server"></a>Windows Server Core および Windows Nano Server での実行ポリシー

特定の条件下で Windows Server Core または Windows Nano Server で PowerShell 6 を実行すると、実行ポリシーが次のエラーで失敗することがあります。

```Output
AuthorizationManager check failed.
At line:1 char:1
+ C:\scriptpath\scriptname.ps1
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : SecurityError: (:) [], PSSecurityException
    + FullyQualifiedErrorId : UnauthorizedAccess
```

PowerShell は、Windows デスクトップシェル () の Api を使用し `explorer.exe` て、スクリプトファイルのゾーンを検証します。 Windows Powershell は、windows Server Core および Windows Nano Server では使用できません。

Windows デスクトップシェルが使用できないか、応答しない場合は、Windows システムでこのエラーを受け取ることもできます。 たとえば、サインオン中に、Windows デスクトップの準備が整う前に PowerShell ログオンスクリプトの実行が開始され、エラーが発生します。

**バイパス** または **AllSigned** の実行ポリシーを使用しても、問題を回避するゾーンチェックは必要ありません。

## <a name="see-also"></a>参照

[about_Environment_Variables](about_Environment_Variables.md)

[about_Group_Policy_Settings](about_Group_Policy_Settings.md)

[about_Signing](about_Signing.md)

[Get-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Get-ExecutionPolicy)

[Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)

[Pwsh コンソールのヘルプ](about_pwsh.md)

[Set-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)

[Unblock-File](xref:Microsoft.PowerShell.Utility.Unblock-File)

