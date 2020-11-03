---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/start-job?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Start-Job
ms.openlocfilehash: 116e007cc28a91e3c97fd980cc27461932390b7c
ms.sourcegitcommit: 9b28fb9a3d72655bb63f62af18b3a5af6a05cd3f
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/07/2020
ms.locfileid: "93212736"
---
# Start-Job

## 概要
PowerShell バックグラウンドジョブを開始します。

## SYNTAX

### ComputerName (既定値)

```
Start-Job [-Name <String>] [-ScriptBlock] <ScriptBlock> [-Credential <PSCredential>]
 [-Authentication <AuthenticationMechanism>] [[-InitializationScript] <ScriptBlock>] [-RunAs32]
 [-PSVersion <Version>] [-InputObject <PSObject>] [-ArgumentList <Object[]>] [<CommonParameters>]
```

### DefinitionName

```
Start-Job [-DefinitionName] <String> [[-DefinitionPath] <String>] [[-Type] <String>]
 [<CommonParameters>]
```

### FilePathComputerName

```
Start-Job [-Name <String>] [-Credential <PSCredential>] [-FilePath] <String>
 [-Authentication <AuthenticationMechanism>] [[-InitializationScript] <ScriptBlock>] [-RunAs32]
 [-PSVersion <Version>] [-InputObject <PSObject>] [-ArgumentList <Object[]>] [<CommonParameters>]
```

### LiteralFilePathComputerName

```
Start-Job [-Name <String>] [-Credential <PSCredential>] -LiteralPath <String>
 [-Authentication <AuthenticationMechanism>] [[-InitializationScript] <ScriptBlock>] [-RunAs32]
 [-PSVersion <Version>] [-InputObject <PSObject>] [-ArgumentList <Object[]>] [<CommonParameters>]
```

## Description

コマンドレットにより、 `Start-Job` ローカルコンピューターで PowerShell バックグラウンドジョブが開始されます。

PowerShell バックグラウンドジョブは、現在のセッションと対話せずにコマンドを実行します。 バックグラウンドジョブを開始すると、ジョブの完了に時間がかかる場合でも、ジョブオブジェクトは直ちに返されます。 ジョブの実行中は、中断されることなく引き続きセッションで作業できます。

ジョブオブジェクトには、ジョブに関する有用な情報が含まれていますが、ジョブの結果は含まれていません。
ジョブが完了したら、コマンドレットを使用して `Receive-Job` ジョブの結果を取得します。 バックグラウンド ジョブの詳細については、「[about_Jobs](./About/about_Jobs.md)」を参照してください。

リモートコンピューターでバックグラウンドジョブを実行するには、多くのコマンドレットで使用できる **AsJob** パラメーターを使用するか、コマンドレットを使用して `Invoke-Command` `Start-Job` リモートコンピューターでコマンドを実行します。 詳細については、「 [about_Remote_Jobs](./About/about_Remote_Jobs.md)」を参照してください。

PowerShell 3.0 以降では、 `Start-Job` スケジュールされたジョブなど、カスタムのジョブの種類のインスタンスを開始できます。 を使用してカスタム型のジョブを開始する方法の詳細については、 `Start-Job` ジョブの種類の機能に関するヘルプドキュメントを参照してください。

ジョブの既定の作業ディレクトリはハードコーディングされています。 Windows の既定値はで、 `$HOME\Documents` Linux または macOS では既定値は `$HOME` です。 バックグラウンドジョブで実行されているスクリプトコードでは、必要に応じて作業ディレクトリを管理する必要があります。

## 例

### 例 1: バックグラウンドジョブを開始する

この例では、ローカルコンピューターで実行されるバックグラウンドジョブを開始します。

```powershell
Start-Job -ScriptBlock { Get-Process -Name powershell }
```

```Output
Id  Name   PSJobTypeName   State     HasMoreData   Location    Command
--  ----   -------------   -----     -----------   --------    -------
1   Job1   BackgroundJob   Running   True          localhost   Get-Process -Name powershell
```

`Start-Job`**ScriptBlock** パラメーターを使用し `Get-Process` て、バックグラウンドジョブとして実行します。 **Name** パラメーターは、PowerShell プロセスを検索するように指定し `powershell` ます。 ジョブ情報が表示され、ジョブがバックグラウンドで実行されている間、PowerShell がプロンプトに戻ります。

ジョブの出力を表示するには、 `Receive-Job` コマンドレットを使用します。 たとえば、「 `Receive-Job -Id 1` 」のように入力します。

### 例 2: Invoke-Command を使用してジョブを開始する

この例では、複数のコンピューターでジョブを実行します。 ジョブは変数に格納され、PowerShell コマンドラインで変数名を使用して実行されます。

```powershell
$jobWRM = Invoke-Command -ComputerName (Get-Content -Path C:\Servers.txt) -ScriptBlock {
   Get-Service -Name WinRM } -JobName WinRM -ThrottleLimit 16 -AsJob
```

を使用するジョブ `Invoke-Command` が作成され、変数に格納され `$jobWRM` ます。 `Invoke-Command`**ComputerName** パラメーターを使用して、ジョブを実行するコンピューターを指定します。 `Get-Content` ファイルからサーバー名を取得し `C:\Servers.txt` ます。

**ScriptBlock** パラメーターは、WinRM サービスを取得するコマンドを指定し `Get-Service` ます。 **WinRM** **JobName** パラメーターは、ジョブのフレンドリ名を指定します ( **WinRM** )。 **ThrottleLimit** パラメーターは、同時実行コマンドの数を16に制限します。 **AsJob** パラメーターは、サーバーでコマンドを実行するバックグラウンドジョブを開始します。

### 例 3: ジョブ情報を取得する

この例では、ジョブに関する情報を取得し、ローカルコンピューターで実行された完了したジョブの結果を表示します。

```powershell
$j = Start-Job -ScriptBlock { Get-WinEvent -Log System } -Credential Domain01\User01
$j | Select-Object -Property *
```

```Output
State         : Completed
HasMoreData   : True
StatusMessage :
Location      : localhost
Command       : Get-WinEvent -Log System
JobStateInfo  : Completed
Finished      : System.Threading.ManualResetEvent
InstanceId    : 27ce3fd9-40ed-488a-99e5-679cd91b9dd3
Id            : 18
Name          : Job18
ChildJobs     : {Job19}
PSBeginTime   : 8/8/2019 14:41:57
PSEndTime     : 8/8/2019 14:42:07
PSJobTypeName : BackgroundJob
Output        : {}
Error         : {}
Progress      : {}
Verbose       : {}
Debug         : {}
Warning       : {}
Information   : {}
```

`Start-Job`**ScriptBlock** パラメーターを使用して、 `Get-WinEvent` **システム** ログを取得するように指定したコマンドを実行します。 **Credential** パラメーターには、コンピューター上でジョブを実行するアクセス許可を持つドメインユーザーアカウントを指定します。 ジョブオブジェクトは変数に格納され `$j` ます。

変数内のオブジェクト `$j` は、パイプラインの下にに送信され `Select-Object` ます。 **Property** パラメーターは、 `*` すべてのジョブオブジェクトのプロパティを表示するアスタリスク () を指定します。

### 例 4: バックグラウンドジョブとしてスクリプトを実行する

この例では、ローカルコンピューター上のスクリプトがバックグラウンドジョブとして実行されます。

```powershell
Start-Job -FilePath C:\Scripts\Sample.ps1
```

`Start-Job`**FilePath** パラメーターを使用して、ローカルコンピューターに格納されているスクリプトファイルを指定します。

### 例 5: バックグラウンドジョブを使用してプロセスを取得する

この例では、バックグラウンドジョブを使用して、指定されたプロセスを名前で取得します。

```powershell
Start-Job -Name PShellJob -ScriptBlock { Get-Process -Name PowerShell }
```

`Start-Job`**Name** パラメーターを使用して、わかりやすいジョブ名 **pshelljob** を指定します。 **ScriptBlock** パラメーターは、 `Get-Process` **PowerShell** という名前のプロセスを取得するように指定します。

### 例 6: バックグラウンドジョブを使用してデータを収集して保存する

この例では、大量のマップデータを収集してファイルに保存するジョブを開始し `.tif` ます。

```powershell
Start-Job -Name GetMappingFiles -InitializationScript {Import-Module MapFunctions} -ScriptBlock {
   Get-Map -Name * | Set-Content -Path D:\Maps.tif } -RunAs32
```

`Start-Job`**Name** パラメーターを使用して、わかりやすいジョブ名である **getmappingfiles** を指定します。 **InitializationScript** パラメーターは、 **mapfunctions** モジュールをインポートするスクリプトブロックを実行します。 **ScriptBlock** パラメーターは、 `Get-Map` `Set-Content` **Path** パラメーターによって指定された場所にデータを実行して保存します。 **RunAs32** パラメーターは、64ビットオペレーティングシステムであっても、32ビットとしてプロセスを実行します。

### 例 7: バックグラウンドジョブに入力を渡す

この例では、 `$input` 自動変数を使用して入力オブジェクトを処理します。 `Receive-Job`ジョブの出力を表示するには、を使用します。

```powershell
Start-Job -ScriptBlock { Get-Content $input } -InputObject "C:\Servers.txt"
Receive-Job -Name Job45 -Keep
```

```Output
Server01
Server02
Server03
Server04
```

`Start-Job`**ScriptBlock** パラメーターを使用し `Get-Content` て、自動変数を指定して実行し `$input` ます。 `$input`変数は **InputObject** パラメーターからオブジェクトを取得します。 `Receive-Job`**Name** パラメーターを使用してジョブを指定し、結果を出力します。 **Keep** パラメーターを指定すると、ジョブの出力が保存され、PowerShell セッション中に再度表示できるようになります。

### 例 8: ArgumentList パラメーターを使用して配列を指定する

この例では、 **ArgumentList** パラメーターを使用して、引数の配列を指定します。 配列は、プロセス名のコンマ区切りのリストです。

```powershell
Start-Job -ScriptBlock { Get-Process -Name $args } -ArgumentList powershell, pwsh, notepad
```

```Output
Id     Name      PSJobTypeName   State       HasMoreData     Location     Command
--     ----      -------------   -----       -----------     --------     -------
1      Job1      BackgroundJob   Running     True            localhost    Get-Process -Name $args
```

コマンドレットでは、 `Start-Job` **ScriptBlock** パラメーターを使用してコマンドを実行します。 `Get-Process`**Name** パラメーターを使用して、自動変数を指定し `$args` ます。 **ArgumentList** パラメーターは、プロセス名の配列をに渡し `$args` ます。 このプロセス名は、ローカルコンピューター上で実行されているプロセスです。

ジョブの出力を表示するには、 `Receive-Job` コマンドレットを使用します。 たとえば、「 `Receive-Job -Id 1` 」のように入力します。

## PARAMETERS

### -ArgumentList

**FilePath** パラメーターで指定されたスクリプト、または **ScriptBlock** パラメーターで指定されたコマンドで指定されたスクリプトの引数またはパラメーター値の配列を指定します。

引数は、1次元の配列引数として **ArgumentList** に渡す必要があります。 たとえば、コンマ区切りのリストです。 **ArgumentList** の動作の詳細については、「 [about_Splatting](about/about_Splatting.md#splatting-with-arrays)」を参照してください。

```yaml
Type: System.Object[]
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -認証

ユーザー資格情報の認証に使用するメカニズムを指定します。

このパラメーターに指定できる値は次のとおりです。

- Default
- Basic
- Credssp
- ダイジェスト
- Kerberos
- ネゴシエート
- NegotiateWithImplicitCredential

既定値は Default です。

CredSSP 認証は、windows Vista、Windows Server 2008、およびそれ以降のバージョンの Windows オペレーティングシステムでのみ使用できます。

このパラメーターの値の詳細については、「 [Authenticationmechanism](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!CAUTION]
> ユーザーの資格情報が認証対象のリモート コンピューターに渡される Credential Security Support Provider (CredSSP) 認証は、リモート ネットワーク共有にアクセスする場合など、複数のリソースの認証を必要とするコマンドを対象としています。 このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。 リモート コンピューターのセキュリティが低下している場合は、そのリモート コンピューターに渡される資格情報を使用してネットワーク セッションが制御される場合があります。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:
Accepted values: Default, Basic, Negotiate, NegotiateWithImplicitCredential, Credssp, Digest, Kerberos

Required: False
Position: Named
Default value: Default
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 **Credential** パラメーターが指定されていない場合、コマンドは現在のユーザーの資格情報を使用します。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -DefinitionName

このコマンドレットが開始するジョブの定義名を指定します。 このパラメーターは、スケジュールされたジョブのように定義名を持つカスタムのジョブを開始するために使用します。

を使用し `Start-Job` てスケジュールされたジョブのインスタンスを開始すると、ジョブトリガーやジョブオプションに関係なく、ジョブは直ちに開始されます。 結果のジョブインスタンスはスケジュールされたジョブですが、スケジュールされたジョブのトリガーなどのディスクには保存されません。 の **ArgumentList** パラメーターを使用して、 `Start-Job` スケジュールされたジョブで実行されるスクリプトのパラメーターの値を指定することはできません。 詳細については、「 [about_Scheduled_Jobs](../PSScheduledJob/About/about_Scheduled_Jobs.md)」を参照してください。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DefinitionName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DefinitionPath

このコマンドレットが開始するジョブの定義のパスを指定します。 定義のパスを入力します。 **Definitionpath** パラメーターと **definitionpath** パラメーターの値を連結したものは、ジョブ定義の完全修飾パスです。 このパラメーターは、スケジュールされたジョブのように定義パスを持つカスタムのジョブを開始するために使用します。

スケジュールされたジョブの場合、 **Definitionpath** パラメーターの値は `$home\AppData\Local\Windows\PowerShell\ScheduledJob` です。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DefinitionName
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

`Start-Job`バックグラウンドジョブとして実行するローカルスクリプトを指定します。 スクリプトのパスとファイル名を入力するか、パイプラインを使用してスクリプトパスを送信 `Start-Job` します。 スクリプトは、ローカルコンピューター上、またはローカルコンピューターがアクセスできるフォルダー内にある必要があります。

このパラメーターを使用すると、PowerShell によって、指定したスクリプトファイルの内容がスクリプトブロックに変換され、バックグラウンドジョブとしてスクリプトブロックが実行されます。

```yaml
Type: System.String
Parameter Sets: FilePathComputerName
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InitializationScript

ジョブの開始前に実行するコマンドを指定します。 スクリプトブロックを作成するには、コマンドを中かっこ ( `{}` ) で囲みます。

このパラメーターは、ジョブを実行するセッションを準備するために使用します。 たとえば、このパラメーターを使用して、関数、スナップイン、モジュールをセッションに追加できます。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

コマンドへの入力を指定します。 オブジェクトが格納されている変数を入力するか、オブジェクトを生成するコマンドまたは式を入力します。

**ScriptBlock** パラメーターの値で、自動変数を使用して `$input` 入力オブジェクトを表します。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -LiteralPath

このコマンドレットをバックグラウンドジョブとして実行するローカルスクリプトを指定します。 ローカルコンピューター上のスクリプトのパスを入力します。

`Start-Job`**LiteralPath** パラメーターの値を、型指定されたとおりに使用します。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String
Parameter Sets: LiteralFilePathComputerName
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

新しいジョブのフレンドリ名を指定します。 この名前を使用して、コマンドレットなど、他のジョブコマンドレットに対してジョブを識別でき `Stop-Job` ます。

既定の表示名はです `Job#` 。ここで、 `#` は、ジョブごとに増分される序数です。

```yaml
Type: System.String
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PSVersion

バージョンを指定します。 `Start-Job` PowerShell のバージョンでジョブを実行します。 このパラメーターに指定できる値は、 `2.0` と `3.0` です。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.Version
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RunAs32

が `Start-Job` 32 ビットプロセスでジョブを実行することを示します。 **RunAs32** は、64ビットオペレーティングシステム上でも、32ビットプロセスでジョブを強制的に実行します。

Windows 7 と Windows Server 2008 R2 の64ビットバージョンでは、コマンドに `Start-Job` **RunAs32** パラメーターが含まれている場合、 **Credential** パラメーターを使用して別のユーザーの資格情報を指定することはできません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: ComputerName, LiteralFilePathComputerName, FilePathComputerName
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -ScriptBlock

バックグラウンド ジョブで実行するコマンドを指定します。 スクリプトブロックを作成するには、コマンドを中かっこ ( `{}` ) で囲みます。 `$input` **InputObject** パラメーターの値にアクセスするには、自動変数を使用します。 このパラメーターは必須です。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ComputerName
Aliases: Command

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Type

によって開始されるジョブのカスタムの種類を指定し `Start-Job` ます。 カスタムのジョブの種類の名前を入力します (スケジュールされたジョブの場合は PSScheduledJob、ワークフロー ジョブの場合は PSWorkflowJob)。 このパラメーターは、標準のバックグラウンドジョブでは無効です。

このパラメーターは、PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: DefinitionName
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプラインを使用し **て、name プロパティを** 持つオブジェクトを **name** パラメーターに送信できます。 たとえば、 **FileInfo** オブジェクトをからにパイプラインでき `Get-ChildItem` `Start-Job` ます。

## 出力

### システムの管理. PSRemotingJob

`Start-Job` 開始したジョブを表す **Psremotingjob** オブジェクトを返します。

## 注

バックグラウンドでを実行するために、は、 `Start-Job` 現在のセッションで独自のセッションで実行されます。 コマンドレットを使用して `Invoke-Command` `Start-Job` リモートコンピューター上のセッションでコマンドを実行すると、は `Start-Job` リモートセッションのセッションで実行されます。

## 関連リンク

[about_Arrays](./about/about_arrays.md)

[about_Automatic_Variables](./about/about_automatic_variables.md)

[about_Jobs](./About/about_Jobs.md)

[about_Job_Details](./About/about_Job_Details.md)

[about_Remote_Jobs](./About/about_Remote_Jobs.md)

[Get-Job](Get-Job.md)

[Invoke-Command](Invoke-Command.md)

[Receive-Job](Receive-Job.md)

[Remove-Job](Remove-Job.md)

[Resume-Job](Resume-Job.md)

[Stop-Job](Stop-Job.md)

[Suspend-Job](Suspend-Job.md)

[Wait-Job](Wait-Job.md)
