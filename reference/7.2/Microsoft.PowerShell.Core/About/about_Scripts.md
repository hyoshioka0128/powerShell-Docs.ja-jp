---
description: PowerShell でスクリプトを実行および記述する方法について説明します。
Locale: en-US
ms.date: 10/06/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_scripts?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Scripts
ms.openlocfilehash: 2fc81d1d43b8cdddaacb917deaa5d58536a541cb
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602212"
---
# <a name="about-scripts"></a>スクリプトについて

## <a name="short-description"></a>簡単な説明
PowerShell でスクリプトを実行および記述する方法について説明します。

## <a name="long-description"></a>長い説明

スクリプトは、1つまたは複数の PowerShell コマンドを含むプレーンテキストファイルです。
PowerShell スクリプトにはファイル拡張子が付いてい `.ps1` ます。

スクリプトの実行は、コマンドレットの実行とよく似ています。 スクリプトのパスとファイル名を入力し、パラメーターを使用してデータを送信し、オプションを設定します。 スクリプトは、コンピューター上または別のコンピューター上のリモートセッションで実行できます。

スクリプトを記述すると、後で使用できるようにコマンドが保存され、他のユーザーと簡単に共有できるようになります。 最も重要なのは、スクリプトパスとファイル名を入力するだけでコマンドを実行できることです。 スクリプトは、ファイル内の1つのコマンドとして簡単に行うことも、複雑なプログラムとして拡張することもできます。

スクリプトには、 `#Requires` 特別なコメント、パラメーターの使用、データセクションのサポート、セキュリティのデジタル署名などの追加機能があります。
スクリプトのヘルプトピックや関数を記述することもできます。

## <a name="how-to-run-a-script"></a>スクリプトを実行する方法

Windows でスクリプトを実行する前に、既定の PowerShell 実行ポリシーを変更する必要があります。 実行ポリシーは、Windows 以外のプラットフォームで実行されている PowerShell には適用されません。

既定の実行ポリシーでは、 `Restricted` ローカルコンピューター上に作成したスクリプトを含め、すべてのスクリプトの実行を禁止します。 詳細については、「[about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

実行ポリシーはレジストリに保存されるため、各コンピューターで1回だけ変更する必要があります。

実行ポリシーを変更するには、次の手順に従います。

コマンド プロンプトに、次のコマンドを入力します。

```powershell
Set-ExecutionPolicy AllSigned
```

または

```powershell
Set-ExecutionPolicy RemoteSigned
```

変更はすぐに有効になります。

スクリプトを実行するには、スクリプトファイルの完全な名前と完全なパスを入力します。

たとえば、Get-ServiceLog.ps1 スクリプトを C:\Scripts ディレクトリで実行するには、次のように入力します。

```powershell
C:\Scripts\Get-ServiceLog.ps1
```

現在のディレクトリでスクリプトを実行するには、現在のディレクトリへのパスを入力するか、ドットを使用して現在のディレクトリを表し、その後にパスバックスラッシュ () を入力し `.\` ます。

たとえば、ローカルディレクトリで ServicesLog.ps1 スクリプトを実行するには、次のように入力します。

```powershell
.\Get-ServiceLog.ps1
```

スクリプトにパラメーターがある場合は、スクリプトファイル名の後にパラメーターとパラメーター値を入力します。

たとえば、次のコマンドは、Get-ServiceLog スクリプトの ServiceName パラメーターを使用して、WinRM サービスアクティビティのログを要求します。

```powershell
.\Get-ServiceLog.ps1 -ServiceName WinRM
```

セキュリティ機能として、エクスプローラーでスクリプトアイコンをダブルクリックした場合や、スクリプトが現在のディレクトリにある場合でもスクリプト名を完全なパスなしで入力した場合、PowerShell ではスクリプトが実行されません。 PowerShell でコマンドとスクリプトを実行する方法の詳細については、「 [about_Command_Precedence](about_Command_Precedence.md)」を参照してください。

### <a name="run-with-powershell"></a>PowerShell を使用しての実行

PowerShell 3.0 以降では、エクスプローラーからスクリプトを実行できます。

"PowerShell で実行" 機能を使用するには、次のようにします。

ファイルエクスプローラーを実行し、スクリプトファイル名を右クリックして、[PowerShell で実行] を選択します。

"PowerShell を使用して実行" 機能は、必要なパラメーターを持たず、コマンドプロンプトに出力を返さないスクリプトを実行するように設計されています。

詳細については、「[about_Run_With_PowerShell](about_Run_With_PowerShell.md)」を参照してください。

### <a name="running-scripts-on-other-computers"></a>他のコンピューターでのスクリプトの実行

1つまたは複数のリモートコンピューターでスクリプトを実行するには、コマンドレットの **FilePath** パラメーターを使用し `Invoke-Command` ます。

**FilePath** パラメーターの値として、スクリプトのパスとファイル名を入力します。 スクリプトは、ローカル コンピューター上か、またはローカル コンピューターがアクセスできるディレクトリ内に存在する必要があります。

次のコマンドは、 `Get-ServiceLog.ps1` Server01 と Server02 という名前のリモートコンピューター上でスクリプトを実行します。

```powershell
Invoke-Command -ComputerName Server01,Server02 -FilePath `
  C:\Scripts\Get-ServiceLog.ps1
```

## <a name="get-help-for-scripts"></a>スクリプトのヘルプを取得する

Get-Help コマンドレットは、スクリプトのヘルプトピックや、コマンドレットやその他の種類のコマンドを取得します。 スクリプトのヘルプトピックを取得するには、 `Get-Help` スクリプトのパスとファイル名を続けて入力します。 スクリプトのパスが環境変数にある場合は、 `Path` パスを省略できます。

たとえば、ServicesLog.ps1 スクリプトのヘルプを表示するには、次のように入力します。

```powershell
get-help C:\admin\scripts\ServicesLog.ps1
```

## <a name="how-to-write-a-script"></a>スクリプトを記述する方法

スクリプトには、任意の有効な PowerShell コマンドを含めることができます。これには、1つのコマンド、パイプライン、関数、および If ステートメントや For ループなどの制御構造を使用するコマンドが含まれます。

スクリプトを作成するには、テキストエディターで新しいファイルを開き、コマンドを入力して、ファイル拡張子がの有効なファイル名を持つファイルに保存し `.ps1` ます。

次の例は、現在のシステムで実行されているサービスを取得し、ログファイルに保存する単純なスクリプトです。 ログファイル名は、現在の日付から作成されます。

```powershell
$date = (get-date).dayofyear
get-service | out-file "$date.log"
```

このスクリプトを作成するには、テキストエディターまたはスクリプトエディターを開き、これらのコマンドを入力して、という名前のファイルに保存し `ServiceLog.ps1` ます。

### <a name="parameters-in-scripts"></a>スクリプトのパラメーター

スクリプトでパラメーターを定義するには、Param ステートメントを使用します。 ステートメントは、 `Param` スクリプト内の最初のステートメントである必要があります。ただし、コメントとステートメントは除き `#Require` ます。

スクリプトパラメーターは、関数パラメーターと同様に機能します。 パラメーター値は、スクリプト内のすべてのコマンドで使用できます。 パラメーター属性とその名前付き引数を含む、関数パラメーターのすべての機能は、スクリプトでも有効です。

スクリプトを実行すると、スクリプトユーザーは、スクリプト名の後にパラメーターを入力します。

次の例は、 `Test-Remote.ps1` **ComputerName** パラメーターを持つスクリプトを示しています。 両方のスクリプト関数が **ComputerName** パラメーター値にアクセスできます。

```powershell
param ($ComputerName = $(throw "ComputerName parameter is required."))

function CanPing {
   $error.clear()
   $tmp = test-connection $computername -erroraction SilentlyContinue

   if (!$?)
       {write-host "Ping failed: $ComputerName."; return $false}
   else
       {write-host "Ping succeeded: $ComputerName"; return $true}
}

function CanRemote {
    $s = new-pssession $computername -erroraction SilentlyContinue

    if ($s -is [System.Management.Automation.Runspaces.PSSession])
        {write-host "Remote test succeeded: $ComputerName."}
    else
        {write-host "Remote test failed: $ComputerName."}
}

if (CanPing $computername) {CanRemote $computername}
```

このスクリプトを実行するには、スクリプト名の後にパラメーター名を入力します。 次に例を示します。

```powershell
C:\PS> .\test-remote.ps1 -computername Server01

Ping succeeded: Server01
Remote test failed: Server01
```

Param ステートメントと関数のパラメーターの詳細については、「 [about_Functions](about_Functions.md) 」および「 [about_Functions_Advanced_Parameters](about_Functions_Advanced_Parameters.md)」を参照してください。

### <a name="writing-help-for-scripts"></a>スクリプトのヘルプの作成

スクリプトのヘルプトピックを作成するには、次の2つの方法のいずれかを使用します。

- スクリプトの Comment-Based ヘルプ

  コメントに特殊なキーワードを使用して、ヘルプトピックを作成します。 スクリプトのコメントベースのヘルプを作成するには、スクリプトファイルの先頭または末尾にコメントを配置する必要があります。 コメントベースのヘルプの詳細については、「 [about_Comment_Based_Help](about_Comment_Based_Help.md)」を参照してください。

- スクリプトの XML-Based ヘルプ

  コマンドレットに対して通常作成される型など、XML ベースのヘルプトピックを作成します。 ヘルプトピックを複数の言語に翻訳する場合は、XML ベースのヘルプが必要です。

スクリプトを XML ベースのヘルプトピックに関連付けるには、を使用します。ExternalHelp ヘルプコメントキーワード。 ExternalHelp キーワードの詳細については、「 [about_Comment_Based_Help](about_Comment_Based_Help.md)」を参照してください。 XML ベースのヘルプの詳細については、「 [How To Write Cmdlet help](/powershell/scripting/developer/help/writing-help-for-windows-powershell-cmdlets)」を参照してください。

### <a name="returning-an-exit-value"></a>終了値を返す

既定では、スクリプトはスクリプトの終了時に終了状態を返しません。 `exit`スクリプトから終了コードを返すには、ステートメントを使用する必要があります。 既定では、 `exit` ステートメントはを返し `0` ます。 数値を指定すると、別の終了ステータスを返すことができます。 ゼロ以外の終了コードは、通常、エラーを通知します。

Windows では、との間に任意の数の数値を `[int]::MinValue` `[int]::MaxValue` 指定できます。

Unix で `[byte]::MinValue` は、(0) から (255) までの正の数値のみ `[byte]::MaxValue` が許可されます。 からまでの範囲の負の数値は、を `-1` `-255` 追加することにより、自動的に正の数値に変換されます。
256. たとえば、 `-2` はに変換され `254` ます。

PowerShell では、ステートメントによって `exit` 変数の値が設定され `$LASTEXITCODE` ます。 Windows コマンドシェル (cmd.exe) では、exit ステートメントによって環境変数の値が設定され `%ERRORLEVEL%` ます。

数値以外、またはプラットフォーム固有の範囲外の引数は、の値に変換され `0` ます。

## <a name="script-scope-and-dot-sourcing"></a>スクリプトスコープとドットソーシング

各スクリプトは、独自のスコープで実行されます。 スクリプトで作成された関数、変数、エイリアス、およびドライブは、スクリプトスコープにのみ存在します。 スクリプトが実行されているスコープ内のこれらのアイテムや値にはアクセスできません。

別のスコープでスクリプトを実行するには、グローバルまたはローカルなどのスコープを指定するか、スクリプトをドットソースにすることができます。

ドットソーシング機能を使用すると、スクリプトのスコープ内ではなく、現在のスコープでスクリプトを実行できます。 ドットソースのスクリプトを実行すると、スクリプト内のコマンドは、コマンドプロンプトで入力した場合と同じように実行されます。 スクリプトによって作成される関数、変数、エイリアス、およびドライブは、作業中のスコープ内に作成されます。 スクリプトの実行後、作成した項目を使用して、セッションの値にアクセスできます。

ドットソーススクリプトを作成するには、スクリプトパスの前にドット (.) とスペースを入力します。

次に例を示します。

```powershell
. C:\scripts\UtilityFunctions.ps1
```

or

```powershell
. .\UtilityFunctions.ps1
```

スクリプトを実行すると、スクリプトによって `UtilityFunctions.ps1` 作成される関数と変数が現在のスコープに追加されます。

たとえば、スクリプトによって `UtilityFunctions.ps1` 関数と変数が作成され `New-Profile` `$ProfileName` ます。

```powershell
#In UtilityFunctions.ps1

function New-Profile
{
  Write-Host "Running New-Profile function"
  $profileName = split-path $profile -leaf

  if (test-path $profile)
    {write-error "Profile $profileName already exists on this computer."}
  else
    {new-item -type file -path $profile -force }
}
```

スクリプトを独自の `UtilityFunctions.ps1` スクリプトスコープで実行した場合、 `New-Profile` 関数と変数は、 `$ProfileName` スクリプトの実行中にのみ存在します。 スクリプトが終了すると、次の例に示すように、関数と変数が削除されます。

```powershell
C:\PS> .\UtilityFunctions.ps1

C:\PS> New-Profile
The term 'new-profile' is not recognized as a cmdlet, function, operable
program, or script file. Verify the term and try again.
At line:1 char:12
+ new-profile <<<<
   + CategoryInfo          : ObjectNotFound: (new-profile:String) [],
   + FullyQualifiedErrorId : CommandNotFoundException

C:\PS> $profileName
C:\PS>
```

スクリプトをドットで変換して実行すると、スクリプトによって、 `New-Profile` `$ProfileName` スコープ内のセッションに関数と変数が作成されます。 スクリプトの実行後、 `New-Profile` 次の例に示すように、関数をセッションで使用できます。

```powershell
C:\PS> . .\UtilityFunctions.ps1

C:\PS> New-Profile

    Directory: C:\Users\juneb\Documents\WindowsPowerShell

    Mode    LastWriteTime     Length Name
    ----    -------------     ------ ----
    -a---   1/14/2009 3:08 PM      0 Microsoft.PowerShellISE_profile.ps1

C:\PS> $profileName
Microsoft.PowerShellISE_profile.ps1
```

スコープの詳細については、「about_Scopes」を参照してください。

## <a name="scripts-in-modules"></a>モジュール内のスクリプト

モジュールは、一連の関連する PowerShell リソースであり、1つの単位として配布できます。 モジュールを使用して、スクリプト、関数、およびその他のリソースを整理することができます。 モジュールを使用して、コードを他のユーザーに配布したり、信頼されたソースからコードを取得したりすることもできます。

モジュールにスクリプトを含めることも、スクリプトモジュールを作成することもできます。スクリプトモジュールは、スクリプトとサポートリソースの全体または主に構成されているモジュールです。 スクリプトモジュールは、hbase-runner.psm1 ファイル拡張子を持つスクリプトにすぎません。

モジュールの詳細については、「 [about_Modules](about_Modules.md)」を参照してください。

## <a name="other-script-features"></a>その他のスクリプト機能

PowerShell には、スクリプトで使用できる便利な機能が多数用意されています。

- `#Requires` -ステートメントを使用し `#Requires` て、指定されたモジュールまたはスナップインと PowerShell のバージョンが指定されていない場合にスクリプトが実行されないようにすることができます。 詳細については、「about_Requires」を参照してください。

- `$PSCommandPath` -実行されているスクリプトの完全なパスと名前が含まれています。 このパラメーターは、すべてのスクリプトで有効です。 この自動変数は、PowerShell 3.0 で導入されました。

- `$PSScriptRoot` -スクリプトの実行元のディレクトリが含まれています。 PowerShell 2.0 では、この変数はスクリプトモジュール () でのみ有効です `.psm1` 。
  PowerShell 3.0 以降では、すべてのスクリプトで有効になっています。

- `$MyInvocation` -自動変数には、 `$MyInvocation` 現在のスクリプトに関する情報が含まれています。これには、その開始方法や "呼び出し" に関する情報も含まれます。 この変数とそのプロパティを使用して、実行中のスクリプトに関する情報を取得できます。 たとえば、のように `$MyInvocation` なります。MyCommand. Path 変数には、スクリプトのパスとファイル名が含まれています。 `$MyInvocation`.行には、すべてのパラメーターと値を含む、スクリプトを開始したコマンドが含まれています。

  PowerShell 3.0 以降、には、 `$MyInvocation` 現在のスクリプトを呼び出したスクリプトまたは呼び出されたスクリプトに関する情報を提供する2つの新しいプロパティがあります。 これらのプロパティの値は、呼び出し元または呼び出し元がスクリプトの場合にのみ設定されます。

  - **Pscommandpath** には、現在のスクリプトを呼び出した、または呼び出されたスクリプトの完全なパスと名前が含まれています。

  - **Psscriptroot** には、現在のスクリプトを呼び出した、または呼び出されたスクリプトのディレクトリが含まれています。

  現在の `$PSCommandPath` `$PSScriptRoot` スクリプトに関する情報を含むおよび自動変数とは異なり、変数の **Pscommandpath** および **pscommandpath** プロパティには、 `$MyInvocation` 現在のスクリプトを呼び出したスクリプトに関する情報が含まれています。

- データセクション-キーワードを使用して、 `Data` スクリプト内のロジックとデータを分けることができます。 データセクションでも、ローカライズが容易になります。 詳細については、「 [about_Data_Sections](about_Data_Sections.md) 」および「 [about_Script_Internationalization](about_Script_Internationalization.md)」を参照してください。

- スクリプトの署名-デジタル署名をスクリプトに追加できます。 実行ポリシーによっては、デジタル署名を使用して、安全でないコマンドを含む可能性のあるスクリプトの実行を制限することができます。 詳細については、「 [about_Execution_Policies](about_Execution_Policies.md) 」および「 [about_Signing](about_Signing.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Command_Precedence](about_Command_Precedence.md)

[about_Comment_Based_Help](about_Comment_Based_Help.md)

[about_Execution_Policies](about_Execution_Policies.md)

[about_Functions](about_Functions.md)

[about_Modules](about_Modules.md)

[about_Profiles](about_Profiles.md)

[about_Requires](about_Requires.md)

[about_Run_With_PowerShell](about_Run_With_PowerShell.md)

[about_Scopes](about_Scopes.md)

[about_Script_Blocks](about_Script_Blocks.md)

[about_Signing](about_Signing.md)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)
