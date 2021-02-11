---
title: ワンライナーとパイプライン
description: PowerShell ワンライナーは複数のコマンドが含まれた連続する 1 つのパイプラインで、1 つのタスクを実行します。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: 1483ec6b76d17c3dd081356ecff85a929fc43e2c
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99605428"
---
# <a name="chapter-4---one-liners-and-the-pipeline"></a>第 4 章 - ワンライナーとパイプライン

私は PowerShell の学習を初めた当初、PowerShell ワンライナーを使用してタスクを実行できなかった場合、GUI に戻っていました。 そして、スクリプト、関数、モジュールを記述するスキルを徐々に身に付けていきました。 インターネット上で見られる一部の高度な例にひるまないようにしましょう。 最初から PowerShell を使いこなせる人はいません。 だれでもかつては初心者だったのです。

今でも管理に GUI を使用している方に向けて、ちょっとしたアドバイスがあります。管理ワークステーションに管理ツールをインストールし、サーバーをリモートで管理しましょう。 そうすることで、サーバーで実行しているオペレーティング システムのインストールが GUI と Server Core のどちらでも問題になりません。
これは、PowerShell を使用してサーバーをリモートで管理するための準備に役に立ちます。

前の章と同様に、Windows 10 ラボ環境コンピューターで作業するようにしてください。

## <a name="one-liners"></a>ワンライナー

PowerShell の1つのコードは1つの連続したパイプラインであり、必ずしも1つの物理ライン上のコマンドではありません。 1 つの物理的な行になっているすべてのコマンドがワンライナーであるとは限りません。

次のコマンドは、複数の物理的な行になっていても、1 つの連続するパイプラインであるため、PowerShell ワンライナーです。 1 つの物理的な行に記述することもできますが、パイプ記号で改行することを選択しました。 パイプ記号は、PowerShell で自然な改行が許される文字の 1 つです。

```powershell
Get-Service |
  Where-Object CanPauseAndContinue -eq $true |
    Select-Object -Property *
```

```Output
Name                : LanmanWorkstation
RequiredServices    : {NSI, MRxSmb20, Bowser}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Workstation
DependentServices   : {SessionEnv, Netlogon, Browser}
MachineName         : .
ServiceName         : LanmanWorkstation
ServicesDependedOn  : {NSI, MRxSmb20, Bowser}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Automatic
Site                :
Container           :

Name                : Netlogon
RequiredServices    : {LanmanWorkstation}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Netlogon
DependentServices   : {}
MachineName         : .
ServiceName         : Netlogon
ServicesDependedOn  : {LanmanWorkstation}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Automatic
Site                :
Container           :

Name                : vmicheartbeat
RequiredServices    : {}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Hyper-V Heartbeat Service
DependentServices   : {}
MachineName         : .
ServiceName         : vmicheartbeat
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :

Name                : vmickvpexchange
RequiredServices    : {}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Hyper-V Data Exchange Service
DependentServices   : {}
MachineName         : .
ServiceName         : vmickvpexchange
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :

Name                : vmicrdv
RequiredServices    : {}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Hyper-V Remote Desktop Virtualization Service
DependentServices   : {}
MachineName         : .
ServiceName         : vmicrdv
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :

Name                : vmicshutdown
RequiredServices    : {}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Hyper-V Guest Shutdown Service
DependentServices   : {}
MachineName         : .
ServiceName         : vmicshutdown
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :

Name                : vmictimesync
RequiredServices    : {VmGid}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Hyper-V Time Synchronization Service
DependentServices   : {}
MachineName         : .
ServiceName         : vmictimesync
ServicesDependedOn  : {VmGid}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :

Name                : vmicvss
RequiredServices    : {}
CanPauseAndContinue : True
CanShutdown         : False
CanStop             : True
DisplayName         : Hyper-V Volume Shadow Copy Requestor
DependentServices   : {}
MachineName         : .
ServiceName         : vmicvss
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :

Name                : Winmgmt
RequiredServices    : {RPCSS}
CanPauseAndContinue : True
CanShutdown         : True
CanStop             : True
DisplayName         : Windows Management Instrumentation
DependentServices   : {wscsvc, NcaSvc, iphlpsvc}
MachineName         : .
ServiceName         : Winmgmt
ServicesDependedOn  : {RPCSS}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Automatic
Site                :
Container           :
```

自然な改行は、コンマ (`,`)、開始角かっこ (`[`)、中かっこ (`{`)、丸かっこ (`(`) など、よく使用される文字で行われる可能性があります。 それほど一般的でないその他のものとしては、セミコロン (`;`)、等号 (`=`)、一重と二重の両方の開始引用符 (`'`、`"`) などがあります。

バックティック (`` ` ``) またはグレーブ アクセント文字を行の連結文字として使用することについては、議論があります。 これは可能な限り回避することをお勧めします。 自然な改行文字の直後にバックティックを使用して PowerShell コマンドが記述されているのをよく見ます。
そのようにしなければならない理由はありません。

```powershell
Get-Service -Name w32time |
>> Select-Object -Property *
```

```Output
Name                : w32time
RequiredServices    : {}
CanPauseAndContinue : False
CanShutdown         : True
CanStop             : True
DisplayName         : Windows Time
DependentServices   : {}
MachineName         : .
ServiceName         : w32time
ServicesDependedOn  : {}
ServiceHandle       : SafeServiceHandle
Status              : Running
ServiceType         : Win32ShareProcess
StartType           : Manual
Site                :
Container           :
```

前の 2 つの例で示したコマンドは、PowerShell コンソールで問題なく動作します。 ただし、PowerShell ISE のコンソール ペインでこれらを実行しようとすると、エラーが発生します。 PowerShell ISE のコンソール ペインでは、PowerShell コンソールのように次の行で残りのコマンドの入力を待機しません。 PowerShell ISE のコンソール ウインドウでこの問題を回避するには、コマンドを次の行で継続する際に、<kbd>Enter</kbd> キーを押すのではなく、<kbd>Shift</kbd>+<kbd>Enter</kbd> キーを使用します。

この次の例は、1 つの連続するパイプラインではないため、PowerShell ワンライナーではありません。 1 つの行でセミコロンによって区切られた、2 つの異なるコマンドです。

```powershell
$Service = 'w32time'; Get-Service -Name $Service
```

```powershell
Status   Name               DisplayName
------   ----               -----------
Running  w32time            Windows Time
```

多くのプログラミング言語とスクリプト言語では、行末にセミコロンが必要です。 PowerShell でもそのような使用法は可能ですが、必要ではないため、推奨されません。

## <a name="filtering-left"></a>左でのフィルター

この章で示されるコマンドの結果は、サブセットへとフィルターされています。 たとえば、返されたサービスのリストを Windows タイム サービスのみにフィルターするために、`Get-Service` は **Name** パラメーターと共に使用されました。

パイプラインでは常に、可能な限り早い段階で結果を目的の内容へとフィルターすることが重要です。 これは、最初 (または左端) のコマンドでパラメーターを使用することで実現されます。
これは "_左でのフィルター_" と呼ばれることがあります。

次の例では、`Get-Service` の **Name** パラメーターを使用して、すぐに結果を Windows タイム サービスのみにフィルターしています。

```powershell
Get-Service -Name w32time
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  w32time            Windows Time
```

フィルターを実行するためにコマンドが `Where-Object` にパイプ処理されている例を見ることが少なくありません。

```powershell
Get-Service | Where-Object Name -eq w32time
```

```Output
Status   Name               DisplayName
------   ----               -----------
Running  W32Time            Windows Time
```

最初の例では、ソースでフィルターが実行され、Windows タイム サービスの結果のみが返されます。
2 つ目の例では、すべてのサービスが返されてから、フィルターを実行するための別のコマンドにパイプ処理されます。 この例では大したことではないように見えますが、Active Directory ユーザーのリストを照会していると想像してみてください。 小さなサブセットへとフィルターする別のコマンドにパイプ処理するためだけに、Active Directory から数千のユーザー アカウントに関する情報を返したいと本当に思うでしょうか。 一見問題がない場合でも、常に左でフィルターすることをお勧めします。 そうすることで、本当に問題がある場合に自然と左でフィルターするようになります。

以前、コマンドを指定する順番は問題にならないと言われたことがあります。 それは事実とはまったく異なります。 実際、コマンドが指定される順番はフィルターを実行する際に問題になります。 たとえば、いくつかのプロパティのみを選択するために `Select-Object`、選択にないプロパティでフィルターするために `Where-Object` を使用しているシナリオを考えてみてください。 このシナリオでは、フィルターは最初に実行される必要があります。そうしないと、フィルターの実行が試みられるときに、プロパティがパイプライン内に存在しません。

```powershell
Get-Service |
Select-Object -Property DisplayName, Running, Status |
Where-Object CanPauseAndContinue
```

前の例のコマンドでは、`Select-Object` の結果が `Where-Object` にパイプ処理されるときに **CanStopAndContinue** プロパティが存在しないため、結果が返されません。 この特定のプロパティは "選択されていませんでした"。 つまり、フィルターで除外されていました。`Select-Object` と `Where-Object` の順番を反対にすることで、目的の結果が得られます。

```powershell
Get-Service |
Where-Object CanPauseAndContinue |
Select-Object -Property DisplayName, Status
```

```Output
DisplayName                                    Status
-----------                                    ------
Workstation                                    Running
Netlogon                                       Running
Hyper-V Heartbeat Service                      Running
Hyper-V Data Exchange Service                  Running
Hyper-V Remote Desktop Virtualization Service  Running
Hyper-V Guest Shutdown Service                 Running
Hyper-V Time Synchronization Service           Running
Hyper-V Volume Shadow Copy Requestor           Running
Windows Management Instrumentation             Running
```

## <a name="the-pipeline"></a>パイプライン

これまで見てきたこのドキュメントの多くの例からわかるように、あるコマンドの出力を別のコマンドの入力として使用できる場合は多々あります。 第 3 章では、コマンドで生成されるオブジェクトの種類を特定するために、`Get-Member` が使用されました。 また第 3 章では、この種類の入力を受け取ったコマンド (ただし、パイプライン入力によっては必ずしも受け取らない) を特定するために、`Get-Command` の **ParameterType** パラメーターも使用して見せました。

コマンドのヘルプの詳細度に応じて、**INPUTS** および **OUTPUTS** セクションが含まれている可能性があります。

```powershell
help Stop-Service -Full
```

```Output
...
INPUTS
    System.ServiceProcess.ServiceController, System.String
        You can pipe a service object or a string that contains the name of a service
        to this cmdlet.

OUTPUTS
    None, System.ServiceProcess.ServiceController
        This cmdlet generates a System.ServiceProcess.ServiceController object that
        represents the service, if you use the PassThru parameter. Otherwise, this
        cmdlet does not generate any output.
...
```

前の結果では、ヘルプの関連するセクションのみが表示されます。 ご覧のとおり、**INPUTS** セクションでは、**ServiceController** または **String** のオブジェクトを `Stop-Service` コマンドレットにパイプ処理できることが述べられています。 ここからは、この種類の入力を受け取るパラメーターがわかりません。 この情報を特定する最も簡単な方法の 1 つは、`Stop-Service` コマンドレットのヘルプの完全なバージョンで各種パラメーターに目を通すことです。

```powershell
help Stop-Service -Full
```

```powershell
...
-DisplayName <String[]>
    Specifies the display names of the services to stop. Wildcard characters are
    permitted.

    Required?                    true
    Position?                    named
    Default value                None
    Accept pipeline input?       False
    Accept wildcard characters?  false

-InputObject <ServiceController[]>
    Specifies ServiceController objects that represent the services to stop. Enter a
    variable that contains the objects, or type a command or expression that gets the
    objects.

    Required?                    true
    Position?                    0
    Default value                None
    Accept pipeline input?       True (ByValue)
    Accept wildcard characters?  false

-Name <String[]>
    Specifies the service names of the services to stop. Wildcard characters are
    permitted.

    Required?                    true
    Position?                    0
    Default value                None
    Accept pipeline input?       True (ByPropertyName, ByValue)
    Accept wildcard characters?  false
...
```

繰り返しますが、前の結果のセットではヘルプの関連部分だけをお見せしました。 **DisplayName** パラメーターではパイプライン入力を受け取らないこと、**InputObject** パラメーターでは **ServiceController** オブジェクトについて **値による** パイプライン入力を受け取ること、**Name** パラメーターでは **String** オブジェクトについて **値による** パイプライン入力を受け取ることに注目してください。 さらに、これは **プロパティ名による** パイプライン入力も受け取ります。

プロパティ名と値の両方によるパイプライン入力をパラメーターが受け取る場合は、常に **値によって** 先に試行されます。 **値によって** 失敗した場合、次に **プロパティ名によって** 試行されます。 "**値による**" という表現には、わずかに語弊があります。 "**種類による**" という表現の方が良いでしょう。 つまり、**ServiceController** というオブジェクトの種類を生成するコマンドの結果を `Stop-Service` にパイプ処理すると、その入力は **InputObject** パラメーターにバインドされます。 しかし、**String** の出力を生成するコマンドの結果を `Stop-Service` にパイプ処理すると、それが **Name** パラメーターにバインドされます。 **ServiceController** または **String** のオブジェクトを生成しないコマンドの結果を `Stop-Service` にパイプ処理しても、**Name** という名前のプロパティが含まれた出力が生成される場合、出力の **Name** プロパティは `Stop-Service` の **Name** パラメーターにバインドされます。

`Get-Service` コマンドで生成される出力の種類を特定します。

```powershell
Get-Service -Name w32time | Get-Member
```

```Output
   TypeName: System.ServiceProcess.ServiceController
```

`Get-Service` では、ServiceController というオブジェクトの種類が生成されます。

先ほどヘルプで見たとおり、`Stop-Service` の **InputObject** パラメーターでは、**値による** (種類による) パイプライン経由で **ServiceController** オブジェクトを受け取ります。 つまり、`Get-Service` コマンドレットの結果が `Stop-Service` にパイプ処理されると、それらは `Stop-Service` の **InputObject** パラメーターにバインドされます。

```powershell
Get-Service -Name w32time | Stop-Service
```

次に文字列入力を試します。 `w32time` を `Get-Member` にパイプ処理して、これが文字列であることだけ確認します。

```powershell
'w32time' | Get-Member
```

```Output
   TypeName: System.String
```

ヘルプで前に示されていたとおり、文字列を `Stop-Service` にパイプ処理すると、それが **値によって** `Stop-Service` の **Name** パラメーターにバインドされます。 それをテストするために、`w32time` を `Stop-Service` にパイプ処理します。

```powershell
'w32time' | Stop-Service
```

前の例では、文字列 `w32time` を囲むのに単一引用符を使用したことに注意してください。 PowerShell では、引用符で囲まれた文字列の内容に、実際の値に拡張する必要がある変数が含まれていない限り、常に二重引用符ではなく一重引用符を使用する必要があります。 一重引用符を使用することで、引用符で囲まれている内容を PowerShell が解析する必要がなくなり、コードの実行速度がわずかに向上します。

カスタム オブジェクトを作成し、`Stop-Service` の **Name** パラメーターについてプロパティ名によるパイプライン入力をテストします。

```powershell
$CustomObject = [pscustomobject]@{
 Name = 'w32time'
 }
```

**CustomObject** 変数の内容は **PSCustomObject** というオブジェクトの種類で、そこには **Name** という名前のプロパティが含まれています。

```powershell
$CustomObject | Get-Member
```

```Output
   TypeName: System.Management.Automation.PSCustomObject

Name        MemberType   Definition
----        ----------   ----------
Equals      Method       bool Equals(System.Object obj)
GetHashCode Method       int GetHashCode()
GetType     Method       type GetType()
ToString    Method       string ToString()
Name        NoteProperty string Name=w32time
```

`$CustomObject` 変数を引用符で囲む場合は、二重引用符を使用します。
そうしないと、リテラル文字列 `$CustomObject` は、変数で囲まれた値ではなく、`Get-Member` にパイプ処理されます。

`$CustomObject` の内容を `Stop-Service` コマンドレットにパイプ処理すると **Name** パラメーターにバインドされますが、ここでは **値によって** ではなく **プロパティ名によって** バインドされます。`$CustomObject` の内容が、**Name** という名前のプロパティを備えたオブジェクトであるためです。

この例では、**Service** などの異なるプロパティ名を使用して、別のカスタム オブジェクトを作成します。

```powershell
$CustomObject = [pscustomobject]@{
  Service = 'w32time'
}
```

`$CustomObject` を `Stop-Service` にパイプ処理しようとするとエラーが発生します。**ServiceController** または **String** のオブジェクトが生成されず、**Name** という名前のプロパティがないためです。

```powershell
$CustomObject | Stop-Service
```

```Output
Stop-Service : Cannot find any service with service name '@{Service=w32time}'.
At line:1 char:17
+ $CustomObject | Stop-Service
+
    + CategoryInfo          : ObjectNotFound: (@{Service=w32time}:String) [Stop-Service]
   , ServiceCommandException
    + FullyQualifiedErrorId : NoServiceFoundForGivenName,Microsoft.PowerShell.Commands.S
   topServiceCommand
```

あるコマンドの出力が別のコマンドのパイプライン入力オプションと揃っていない場合は、プロパティの名前を変更するために `Select-Object` を使用できず、プロパティが正常に揃いません。

```powershell
$CustomObject |
  Select-Object -Property @{name='Name';expression={$_.Service}} |
    Stop-Service
```

この例では、**Service** プロパティを **Name** という名前のプロパティに変更するために `Select-Object` が使用されました。

この例の構文は、最初は少し複雑に思えるかもしれません。 私の経験によると、コードをコピーして貼り付けても構文を学ぶことはできません。 時間を取ってコードを入力してみてください。 何度か入力した後に、自然と身に付きます。 モニターが複数あると、一方の画面にサンプル コードを表示し、もう一方の画面でそれを入力することができるので、非常に便利です。

場合によっては、パイプライン入力を受け取らないパラメーターを使用したいことがあるかもしれません。 次の例では、あるコマンドの出力を別のコマンドの入力として使用する方法を説明します。 最初に、いくつかの Windows サービスの表示名をテキスト ファイルに保存します。

```powershell
'Background Intelligent Transfer Service', 'Windows Time' |
Out-File -FilePath $env:TEMP\services.txt
```

丸かっこ内の必要な出力を、入力を必要とするコマンドのパラメーターの値として指定するコマンドを実行できます。

```powershell
Stop-Service -DisplayName (Get-Content -Path $env:TEMP\services.txt)
```

これは、代数の演算を覚えている人にとっては、その順序と同じです。 丸かっこ内のコマンドは常に、コマンドの外側の部分よりも先に実行されます。

## <a name="powershellget"></a>PowerShellGet

PowerShellGet は、NuGet リポジトリとの間で PowerShell モジュール (およびその他のアーティファクト) の検出、インストール、発行、更新を行うためのコマンドが含まれている PowerShell モジュールです。 PowerShellGet は、PowerShell バージョン5.0 以上に付属しています。 PowerShell バージョン3.0 以上については、個別のダウンロードとして入手できます。

Microsoft は、[PowerShell ギャラリー][]と呼ばれるオンライン NuGet リポジトリをホストしています。 このリポジトリは Microsoft によってホストされていますが、リポジトリ内に含まれているモジュールの大半は、Microsoft によって記述されたものではありません。 PowerShell ギャラリーから入手したコードは、運用環境での使用に適していると見なす前に、分離されたテスト環境で十分に確認する必要があります。

ほとんどの企業では、独自の内部プライベート NuGet リポジトリをホストしようと考えます。そこには、社内専用のモジュールをポストしたり、他のソースからダウンロードしたモジュールを、悪意のないことを確認した後にポストしたりできます。

PowerShellGet モジュールに含まれている `Find-Module` コマンドレットを使用して、PowerShell ギャラリーにある MrToolkit という名前のモジュールを検索します。

```powershell
Find-Module -Name MrToolkit
```

```Output
NuGet provider is required to continue
PowerShellGet requires NuGet provider version '2.8.5.201' or newer to interact with
NuGet-based repositories. The NuGet provider must be available in 'C:\Program
Files\PackageManagement\ProviderAssemblies' or
'C:\Users\MrAdmin\AppData\Local\PackageManagement\ProviderAssemblies'. You can also
install the NuGet provider by running 'Install-PackageProvider -Name NuGet
-MinimumVersion 2.8.5.201 -Force'. Do you want PowerShellGet to install and import the
NuGet provider now?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):

Version    Name                                Repository           Description
-------    ----                                ----------           -----------
1.1        MrToolkit                           PSGallery            Misc PowerShell Tools
```

PowerShellGet モジュールのいずれかのコマンドを初めて使用する際には、NuGet プロバイダーをインストールすることが求められます。

MrToolkit モジュールをインストールするには、前のコマンドを `Install-Module` にパイプ処理します。

```powershell
Find-Module -Name MrToolkit | Install-Module
```

```Output
Untrusted repository
You are installing the modules from an untrusted repository. If you trust this
repository, change its InstallationPolicy value by running the Set-PSRepository cmdlet.
Are you sure you want to install the modules from
'https://www.powershellgallery.com/api/v2/'?
[Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "N"): y
```

PowerShell ギャラリーは信頼されていないリポジトリのため、モジュールのインストールを承認するよう求められます。

## <a name="finding-pipeline-input-the-easy-way"></a>パイプラインの入力を確認する簡単な方法

MrToolkit モジュールには、`Get-MrPipelineInput` という名前の関数が含まれています。 このコマンドレットを使用すると、コマンドのどのパラメーターがパイプライン入力を受け取るのか、それらが受け取るオブジェクトの種類は何か、**値** と **プロパティ名** のどちらによるパイプライン入力が受け取られるのかが、簡単に特定できます。

```powershell
Get-MrPipelineInput -Name Stop-Service
```

```Output
ParameterName ParameterType                             ValueFromPipeline ValueFromPipelineByPropertyName
------------- -------------                             ----------------- ---------------
InputObject   System.ServiceProcess.ServiceController[]              True           False
Name          System.String[]                                        True            True
```

ご覧のとおり、この関数を使用すれば、先ほどヘルプを精査することで特定した情報を簡単に特定できます。

## <a name="summary"></a>まとめ

この章では、PowerShell ワンライナーについて学習しました。 コマンドの物理的な行の数は、それが PowerShell ワンライナーであるかどうかに関係ないことを学習しました。 また、左でのフィルター、パイプライン、PowerShellGet についても学習しました。

## <a name="review"></a>確認

1. PowerShell ワンライナーとは何か。
1. PowerShell で自然な改行が行われる可能性がある文字は何か。
1. 左でフィルターする必要があるのはなぜか。
1. PowerShell コマンドでパイプライン入力を受け取る 2 つの方法は何か。
1. PowerShell ギャラリーにあるコマンドを信頼すべきでないのはなぜか。

## <a name="recommended-reading"></a>推奨トピック

- [about_Pipelines][]
- [about_Command_Syntax][]
- [about_Parameters][]
- [PowerShellGet: PowerShell モジュールを検出、インストール、更新する非常に簡単な方法][psget]

<!-- link references-->
[about_Pipelines]: /powershell/module/microsoft.powershell.core/about/about_pipelines
[about_Command_Syntax]: /powershell/module/microsoft.powershell.core/about/about_command_syntax
[about_Parameters]: /powershell/module/microsoft.powershell.core/about/about_parameters
[psget]: https://mikefrobbins.com/2015/04/23/powershellget-the-big-easy-way-to-discover-install-and-update-powershell-modules/
[PowerShell ギャラリー]: https://www.powershellgallery.com/
