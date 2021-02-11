---
description: コマンドレットパラメーターおよび高度な関数のカスタムの既定値を設定する方法について説明します。
Locale: en-US
ms.date: 05/31/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_parameters_default_values?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Parameters_Default_Values
ms.openlocfilehash: 102f163287717f7c9cd4f430704cc27ddea7ff4c
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600816"
---
# <a name="about-parameters-default-values"></a>パラメーターの既定値について

## <a name="short-description"></a>簡単な説明

コマンドレットパラメーターおよび高度な関数のカスタムの既定値を設定する方法について説明します。

## <a name="long-description"></a>長い説明

`$PSDefaultParameterValues`ユーザー設定変数を使用すると、任意のコマンドレットまたは高度な関数に対してカスタムの既定値を指定できます。 コマンドレットと高度な関数では、コマンドで別の値を指定しない限り、カスタムの既定値が使用されます。

コマンドレットと高度な関数の作成者は、パラメーターに標準の既定値を設定します。 通常、標準の既定値は役に立ちますが、すべての環境に適しているとは限りません。

この機能は、コマンドを使用するたびにほぼ同じ代替パラメーター値を指定する必要がある場合や、電子メールサーバー名やプロジェクト GUID など、特定のパラメーター値が覚えにくい場合に特に便利です。

目的の既定値が予想どおりに変化する場合は、異なる条件下でパラメーターに異なる既定値を提供するスクリプトブロックを指定できます。

`$PSDefaultParameterValues` は、PowerShell 3.0 で導入されました。

## <a name="syntax"></a>構文

`$PSDefaultParameterValues`変数は、キーの形式を、 **System. DefaultParameterDictionary** のオブジェクトの種類として検証するハッシュテーブルです。 ハッシュテーブルには、 **キーと値** のペアが含まれています。 **キー** の形式は `CmdletName:ParameterName` です。 **値** は、キーに割り当てられた **DefaultValue** または **ScriptBlock** です。

ユーザー `$PSDefaultParameterValues` 設定変数の構文は次のとおりです。

```
$PSDefaultParameterValues=@{"CmdletName:ParameterName"="DefaultValue"}

$PSDefaultParameterValues=@{ "CmdletName:ParameterName"={{ScriptBlock}} }

$PSDefaultParameterValues["Disabled"]=$True | $False
```

入力文字列の **名前** と **ParameterName** の値には、ワイルドカード文字を使用できます。

から特定の **キーと値** のペアを設定、変更、追加、または削除するには、 `$PSDefaultParameterValues` メソッドを使用して標準のハッシュテーブルを編集します。 たとえば、 **Add** メソッドと **Remove** メソッドを使用します。 これらのメソッドでは、ハッシュテーブル内の他の値は上書きされません。

既存のハッシュテーブルを上書きしないもう1つの構文があり `$PSDefaultParameterValues` ます。 特定の **キーと値** のペアを追加または変更するには、次の構文を使用します。

```
$PSDefaultParameterValues["CmdletName:ParameterName"]="DefaultValue"
```

コマンドレット **名** は、コマンドレットの名前であるか、またはコマンドレット **バインド** 属性を使用する高度な関数の名前である必要があります。 を使用し `$PSDefaultParameterValues` て、スクリプトまたは単純な関数の既定値を指定することはできません。

**DefaultValue** には、オブジェクトまたはスクリプトブロックを指定できます。 値がスクリプトブロックの場合、PowerShell はスクリプトブロックを評価し、結果をパラメーター値として使用します。 指定されたパラメーターがスクリプトブロックの値を受け入れる場合は、次のように、スクリプトブロックの値を2番目のかっこのセットで囲みます。

```powershell
$PSDefaultParameterValues=@{ "Invoke-Command:ScriptBlock"={{Get-Process}} }
```

詳細については、以下のドキュメントをご覧ください。

- [about_Hash_Tables](about_Hash_Tables.md)
- [about_Script_Blocks](about_Script_Blocks.md)
- [about_Preference_Variables](about_Preference_Variables.md)

## <a name="examples"></a>例

### <a name="how-to-set-psdefaultparametervalues"></a>PSDefaultParameterValues を設定する方法 \$

`$PSDefaultParameterValues` はユーザー設定変数なので、設定されているセッションにのみ存在します。 既定値はありません。

を設定するには `$PSDefaultParameterValues` 、変数名と、1つまたは複数の **キーと値** のペアを入力します。 別のコマンドを実行すると `$PSDefaultParameterValues` 、既存のハッシュテーブルが上書きされます。

既存のハッシュテーブル値を上書きせずに **キーと値** のペアを変更する方法の例については、「 [ \$ PSDefaultParameterValues に値を追加する方法](#how-to-add-values-to-psdefaultparametervalues) 」または「 [ \$ PSDefaultParameterValues で値を変更する](#how-to-change-values-in-psdefaultparametervalues)方法」を参照してください。

`$PSDefaultParameterValues`今後のセッションのために保存するには、 `$PSDefaultParameterValues` PowerShell プロファイルにコマンドを追加します。 詳細については、「 [about_Profiles](about_Profiles.md)」を参照してください。

#### <a name="set-a-custom-default-value"></a>カスタムの既定値を設定する

**キーと値** のペアは、 `Send-MailMessage:SmtpServer` キーをカスタムの既定値 **server123 to you** に設定します。

```powershell
$PSDefaultParameterValues = @{
  "Send-MailMessage:SmtpServer"="Server123"
}
```

#### <a name="set-default-values-for-multiple-parameters"></a>複数のパラメーターの既定値を設定する

複数のパラメーターの既定値を設定するには、各 **キーと値** のペアをセミコロン () で区切り `;` ます。 `Send-MailMessage:SmtpServer`キーと `Get-WinEvent:LogName` キーは、カスタムの既定値に設定されます。

```powershell
$PSDefaultParameterValues = @{
  "Send-MailMessage:SmtpServer"="Server123";
  "Get-WinEvent:LogName"="Microsoft-Windows-PrintService/Operational"
}
```

#### <a name="use-wildcards-and-switch-parameters"></a>ワイルドカードとスイッチパラメーターを使用する

コマンドレットとパラメーター名には、ワイルドカード文字を含めることができます。 およびを使用し `$True` `$False` て、 **詳細** などのスイッチパラメーターの値を設定します。 共通パラメーターの **Verbose** パラメーターは、 `$True` すべてのコマンドに対してに設定されます。

```powershell
$PSDefaultParameterValues = @{"*:Verbose"=$True}
```

#### <a name="use-an-array-for-the-default-value"></a>既定値に配列を使用する

パラメーターが複数の値 (配列) を受け取ることができる場合は、複数の値を既定値として設定できます。 キーの既定値 `Invoke-Command:ComputerName` は、 **Server01** および **Server02** の配列値に設定されます。

```powershell
$PSDefaultParameterValues = @{
  "Invoke-Command:ComputerName"="Server01","Server02"
}
```

#### <a name="use-a-script-block"></a>スクリプトブロックを使用する

スクリプトブロックを使用して、さまざまな条件下でパラメーターに異なる既定値を指定できます。 PowerShell はスクリプトブロックを評価し、結果を既定のパラメーター値として使用します。

この `Format-Table:AutoSize` キーは、スイッチパラメーターを既定値の **True** に設定します。 ステートメントには `If` 、が `$host.Name` PowerShell コンソール **consolehost** である必要があるという条件が含まれています。

```powershell
$PSDefaultParameterValues=@{
  "Format-Table:AutoSize"={if ($host.Name -eq "ConsoleHost"){$True}}
}
```

パラメーターがスクリプトブロックの値を受け入れる場合は、スクリプトブロックを中かっこのセットで囲みます。 PowerShell が外側のスクリプトブロックを評価すると、結果は内部スクリプトブロックになり、既定のパラメーター値として設定されます。

`Invoke-Command:ScriptBlock`スクリプトブロックが2番目の中かっこで囲まれているため、**システムイベントログ** の既定値に設定されたキー。 スクリプトブロックの結果がコマンドレットに渡され `Invoke-Command` ます。

```powershell
$PSDefaultParameterValues=@{
  "Invoke-Command:ScriptBlock"={{Get-EventLog -Log System}}
}
```

### <a name="how-to-get-psdefaultparametervalues"></a>PSDefaultParameterValues を取得する方法 \$

ハッシュテーブルの値は、PowerShell プロンプトで「」と入力すると表示され `$PSDefaultParameterValues` ます。

`$PSDefaultParameterValues`ハッシュテーブルは、3つの **キーと値** のペアで設定されます。
次の例では、このハッシュテーブルを使用して、の値を追加、変更、および削除する方法について説明し `$PSDefaultParameterValues` ます。

```
PS> $PSDefaultParameterValues = @{
  "Send-MailMessage:SmtpServer"="Server123"
  "Get-WinEvent:LogName"="Microsoft-Windows-PrintService/Operational"
  "Get-*:Verbose"=$True
}

PS> $PSDefaultParameterValues

Name                           Value
----                           -----
Get-WinEvent:LogName           Microsoft-Windows-PrintService/Operational
Get-*:Verbose                  True
Send-MailMessage:SmtpServer    Server123
```

特定のキーの値を取得するには `CmdletName:ParameterName` 、次の構文を使用します。

```
$PSDefaultParameterValues["CmdletName:ParameterName"]
```

たとえば、キーの値を取得し `Send-MailMessage:SmtpServer` ます。

```
PS> $PSDefaultParameterValues["Send-MailMessage:SmtpServer"]
Server123
```

### <a name="how-to-add-values-to-psdefaultparametervalues"></a>PSDefaultParameterValues に値を追加する方法 \$

に値を追加するには、 `$PSDefaultParameterValues` **add** メソッドを使用します。 値を追加しても、ハッシュテーブルの既存の値には影響しません。

`,`**キー** を **値** から区切るには、コンマ () を使用します。 次の構文は、の **Add** メソッドを使用する方法を示して `$PSDefaultParameterValues` います。

```
PS> $PSDefaultParameterValues.Add("CmdletName:ParameterName", "DefaultValue")
```

前の例で作成したハッシュテーブルは、新しい **キーと値** のペアで更新されます。 **Add** メソッドは、 `Get-Process:Name` キーを値 **PowerShell** に設定します。

```powershell
$PSDefaultParameterValues.Add("Get-Process:Name", "PowerShell")
```

次の構文では、同じ結果が得られます。

```powershell
$PSDefaultParameterValues["Get-Process:Name"]="PowerShell"
```

変数は、 `$PSDefaultParameterValues` 更新されたハッシュテーブルを表示します。 `Get-Process:Name`キーが追加されました。

```
PS> $PSDefaultParameterValues

Name                           Value
----                           -----
Get-Process:Name               PowerShell
Get-WinEvent:LogName           Microsoft-Windows-PrintService/Operational
Get-*:Verbose                  True
Send-MailMessage:SmtpServer    Server123
```

### <a name="how-to-remove-values-from-psdefaultparametervalues"></a>PSDefaultParameterValues から値を削除する方法 \$

から値を削除するには `$PSDefaultParameterValues` 、ハッシュテーブルの **remove** メソッドを使用します。 値を削除しても、ハッシュテーブルの既存の値には影響しません。

次の構文は、で **Remove** メソッドを使用する方法を示して `$PSDefaultParameterValues` います。

```
PS> $PSDefaultParameterValues.Remove("CmdletName:ParameterName")
```

この例では、前の例で作成したハッシュテーブルを更新して、 **キーと値** のペアを削除します。 **Remove** メソッドは、キーを削除し `Get-Process:Name` ます。

```powershell
$PSDefaultParameterValues.Remove("Get-Process:Name")
```

変数は、 `$PSDefaultParameterValues` 更新されたハッシュテーブルを表示します。 `Get-Process:Name`キーが削除されました。

```
PS> $PSDefaultParameterValues

Name                           Value
----                           -----
Get-WinEvent:LogName           Microsoft-Windows-PrintService/Operational
Get-*:Verbose                  True
Send-MailMessage:SmtpServer    Server123
```

### <a name="how-to-change-values-in-psdefaultparametervalues"></a>PSDefaultParameterValues の値を変更する方法 \$

特定の値を変更しても、既存のハッシュテーブルの値には影響しません。 で特定の **キーと値** のペアを変更するには `$PSDefaultParameterValues` 、次の構文を使用します。

```
PS> $PSDefaultParameterValues["CmdletName:ParameterName"]="DefaultValue"
```

この例では、前の例で作成したハッシュテーブルを更新して、 **キーと値** のペアを変更します。 次のコマンドを実行 `Send-MailMessage:SmtpServer` すると、キーが **serverxyz** の新しい値に変更されます。

```powershell
$PSDefaultParameterValues["Send-MailMessage:SmtpServer"]="ServerXYZ"
```

変数は、 `$PSDefaultParameterValues` 更新されたハッシュテーブルを表示します。 `Send-MailMessage:SmtpServer`キーが新しい値に変更されました。

```
PS> $PSDefaultParameterValues

Name                           Value
----                           -----
Get-WinEvent:LogName           Microsoft-Windows-PrintService/Operational
Get-*:Verbose                  True
Send-MailMessage:SmtpServer    ServerXYZ
```

### <a name="how-to-disable-and-re-enable-psdefaultparametervalues"></a>PSDefaultParameterValues を無効にしてから再度有効にする方法 \$

を一時的に無効にしてから再び有効にすることができ `$PSDefaultParameterValues` ます。
`$PSDefaultParameterValues`異なる既定のパラメーター値を必要とするスクリプトを実行している場合は、無効にすることをお勧めします。

を無効にするには `$PSDefaultParameterValues` 、値が True のキーを追加し `Disabled` ます。  の値は `$PSDefaultParameterValues` 保持されますが、有効ではありません。

```
PS> $PSDefaultParameterValues.Add("Disabled", $True)
```

次の構文では、同じ結果が得られます。

```
PS> $PSDefaultParameterValues["Disabled"]=$True
```

変数は、 `$PSDefaultParameterValues` 更新されたハッシュテーブルをキーの値と共に表示し `Disabled` ます。

```
PS> $PSDefaultParameterValues

Name                           Value
----                           -----
Disabled                       True
Get-WinEvent:LogName           Microsoft-Windows-PrintService/Operational
Get-*:Verbose                  True
Send-MailMessage:SmtpServer    ServerXYZ
```

再度有効にするに `$PSDefaultParameterValues` は、 **無効になっ** ているキーを削除するか、 **無効になっ** ているキーの値をに変更し `$False` ます。 の前の値 `$PSDefaultParameterValues` は再び有効になります。

```
PS> $PSDefaultParameterValues.Remove("Disabled")
```

次の構文では、同じ結果が得られます。

```
PS> $PSDefaultParameterValues["Disabled"]=$False
```

変数は、 `$PSDefaultParameterValues` 更新されたハッシュテーブルを表示します。 **Remove** メソッドを使用すると、 `Disabled` キーは出力から削除されます。
代替構文を使用して再度有効にした場合、 `$PSDefaultParameterValues` `Disabled` キーは **False** として表示されます。

```
PS> $PSDefaultParameterValues

Name                           Value
----                           -----
Disabled                       False
Get-WinEvent:LogName           Microsoft-Windows-PrintService/Operational
Get-*:Verbose                  True
Send-MailMessage:SmtpServer    ServerXYZ
```

## <a name="see-also"></a>関連項目

[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Functions_CmdletBindingAttribute](about_Functions_CmdletBindingAttribute.md)

[about_Hash_Tables](about_Hash_Tables.md)

[about_Preference_Variables](about_Preference_Variables.md)

[about_Profiles](about_Profiles.md)

[about_Script_Blocks](about_Script_Blocks.md)

