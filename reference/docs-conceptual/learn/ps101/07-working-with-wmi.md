---
title: WMI の操作
description: PowerShell には、WMI を操作するためのコマンドレットが最初から備わっています。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: 119bb3381ee55c70340da89d1c0690d84b3e70d2
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99599282"
---
# <a name="chapter-7---working-with-wmi"></a>第 7 章 - WMI の操作

## <a name="wmi-and-cim"></a>WMI と CIM

PowerShell には、Windows Management Instrumentation (WMI) など、その他のテクノロジを操作するためのコマンドレットが既定で付属しています。 PowerShell にはネイティブな WMI コマンドレットがいくつもあり、追加のソフトウェアやモジュールをインストールする必要がありません。

PowerShell には、WMI を操作するためのコマンドレットが最初から備わっています。 PowerShell にどのような WMI コマンドレットがあるのかを特定するには、`Get-Command` を使用できます。 次の結果は、PowerShell バージョン 5.1 を実行している Windows 10 ラボ環境コンピューターからのものです。 実行している PowerShell バージョンによっては、結果が異なる場合があります。

```powershell
Get-Command -Noun WMI*
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Get-WmiObject                                      3.1.0.0    Microsof...
Cmdlet          Invoke-WmiMethod                                   3.1.0.0    Microsof...
Cmdlet          Register-WmiEvent                                  3.1.0.0    Microsof...
Cmdlet          Remove-WmiObject                                   3.1.0.0    Microsof...
Cmdlet          Set-WmiInstance                                    3.1.0.0    Microsof...
```

Common Information Model (CIM) コマンドレットは、PowerShell バージョン 3.0 で導入されました。 CIM コマンドレットは、Windows マシンと Windows 以外のマシンの両方で使用できるように設計されています。 WMI コマンドレットは非推奨化されているため、古い WMI ではなく CIM コマンドレットを使用することをお勧めします。

CIM コマンドレットはすべて、モジュール内に含まれています。 CIM コマンドレットの一覧を取得するには、次の例に示すように、**Module** パラメーターを指定して `Get-Command` を使用します。

```powershell
Get-Command -Module CimCmdlets
```

```Output
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Export-BinaryMiLog                                 1.0.0.0    CimCmdlets
Cmdlet          Get-CimAssociatedInstance                          1.0.0.0    CimCmdlets
Cmdlet          Get-CimClass                                       1.0.0.0    CimCmdlets
Cmdlet          Get-CimInstance                                    1.0.0.0    CimCmdlets
Cmdlet          Get-CimSession                                     1.0.0.0    CimCmdlets
Cmdlet          Import-BinaryMiLog                                 1.0.0.0    CimCmdlets
Cmdlet          Invoke-CimMethod                                   1.0.0.0    CimCmdlets
Cmdlet          New-CimInstance                                    1.0.0.0    CimCmdlets
Cmdlet          New-CimSession                                     1.0.0.0    CimCmdlets
Cmdlet          New-CimSessionOption                               1.0.0.0    CimCmdlets
Cmdlet          Register-CimIndicationEvent                        1.0.0.0    CimCmdlets
Cmdlet          Remove-CimInstance                                 1.0.0.0    CimCmdlets
Cmdlet          Remove-CimSession                                  1.0.0.0    CimCmdlets
Cmdlet          Set-CimInstance                                    1.0.0.0    CimCmdlets
```

CIM コマンドレットでは引き続き WMI を操作できます。そのため、他のユーザーが "PowerShell CIM コマンドレットを使用して WMI のクエリを実行するとき" と言っても、混乱しないようにしてください。

前述したように、WMI は PowerShell とは別個のテクノロジであり、ユーザーは WMI にアクセスするために CIM コマンドレットを使用しているだけです。 次の例のように、WMI Query Language (WQL) を使用して WMI のクエリを実行する古い VB スクリプトが見つかる場合があります。

```vb
strComputer = "."
Set objWMIService = GetObject("winmgmts:" _
    & "{impersonationLevel=impersonate}!\\" & strComputer & "\root\cimv2")

Set colBIOS = objWMIService.ExecQuery _
    ("Select * from Win32_BIOS")

For each objBIOS in colBIOS
    Wscript.Echo "Manufacturer: " & objBIOS.Manufacturer
    Wscript.Echo "Name: " & objBIOS.Name
    Wscript.Echo "Serial Number: " & objBIOS.SerialNumber
    Wscript.Echo "SMBIOS Version: " & objBIOS.SMBIOSBIOSVersion
    Wscript.Echo "Version: " & objBIOS.Version
Next
```

その VB スクリプトから WQL クエリを取得して、変更することなく `Get-CimInstance` コマンドレットで使用できます。

```powershell
Get-CimInstance -Query 'Select * from Win32_BIOS'
```

```Output
SMBIOSBIOSVersion : 090006
Manufacturer      : American Megatrends Inc.
Name              : Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
SerialNumber      : 3810-1995-1654-4615-2295-2755-89
Version           : VRTUAL - 4001628
```

これは、PowerShell で WMI のクエリを実行する通常の方法ではありません。 しかし、これは問題なく使えて、既存の VB スクリプトを PowerShell に簡単に移行できます。 WMI のクエリを実行するワンライナーの記述に取り掛かる際には、次の構文を使用します。

```powershell
Get-CimInstance -ClassName Win32_BIOS
```

```Output
SMBIOSBIOSVersion : 090006
Manufacturer      : American Megatrends Inc.
Name              : Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
SerialNumber      : 3810-1995-1654-4615-2295-2755-89
Version           : VRTUAL - 4001628
```

シリアル番号だけが必要な場合は、出力を `Select-Object` にパイプ処理して、**SerialNumber** プロパティのみを指定できます。

```powershell
Get-CimInstance -ClassName Win32_BIOS | Select-Object -Property SerialNumber
```

```Output
SerialNumber
------------
3810-1995-1654-4615-2295-2755-89
```

既定では、使用されることがないのに背後で取得されるプロパティがいくつかあります。
これは、ローカル コンピューター上で WMI のクエリを実行するときにはそれほど問題にならないでしょう。 しかし、リモート コンピュートに対するクエリを開始すると、その情報を返すのに追加の処理時間がかかるだけでなく、必要ない余計な情報をネットワーク経由でプルしなければならなくなります。 `Get-CimInstance` には、取得される情報を制限する **Property** パラメーターがあります。 これにより、WMI へのクエリの効率が向上します。

```powershell
Get-CimInstance -ClassName Win32_BIOS -Property SerialNumber |
Select-Object -Property SerialNumber
```

```Output
SerialNumber
------------
3810-1995-1654-4615-2295-2755-89
```

前の結果ではオブジェクトが返されました。 単純な文字列を返すには、**ExpandProperty** パラメーターを使用します。

```powershell
Get-CimInstance -ClassName Win32_BIOS -Property SerialNumber |
Select-Object -ExpandProperty SerialNumber
```

```Output
3810-1995-1654-4615-2295-2755-89
```

単純な文字列を返すには、ピリオドを使うスタイルの構文を使用することもできます。 そうすれば `Select-Object` にパイプ処理する必要がありません。

```powershell
(Get-CimInstance -ClassName Win32_BIOS -Property SerialNumber).SerialNumber
```

```Output
3810-1995-1654-4615-2295-2755-89
```

## <a name="query-remote-computers-with-the-cim-cmdlets"></a>CIM コマンドレットによるリモート コンピューターへのクエリ実行

引き続き、ドメイン ユーザーであるローカル管理者として PowerShell を実行しています。 `Get-CimInstance` コマンドレットを使用してリモート コンピューターから情報のクエリを実行しようとすると、アクセス拒否のエラー メッセージが表示されます。

```powershell
Get-CimInstance -ComputerName dc01 -ClassName Win32_BIOS
```

```Output
Get-CimInstance : Access is denied.
At line:1 char:1
+ Get-CimInstance -ComputerName dc01 -ClassName Win32_BIOS
+ ``````````````````````````````````````````````````````~~
    + CategoryInfo          : PermissionDenied: (root\cimv2:Win32_BIOS:String) [Get-CimI
   nstance], CimException
    + FullyQualifiedErrorId : HRESULT 0x80070005,Microsoft.Management.Infrastructure.Cim
   Cmdlets.GetCimInstanceCommand
    + PSComputerName        : dc01
```

多くのユーザーが PowerShell のセキュリティについて懸念を抱いていますが、実際のところ、PowerShell でユーザーが付与されるアクセス許可は GUI とまったく同じです。 それ以上でも以下でもありません。 前の例の問題は、PowerShell を実行しているユーザーに、DC01 サーバーから WMI 情報のクエリを実行する権限がないことです。 `Get-CimInstance` には **Credential** パラメーターがないため、ドメイン管理者として PowerShell を再起動することができます。 ただし、PowerShell から実行する内容がすべてドメイン管理者として実行されるため、この方法はお勧めできません。セキュリティの観点からすると、これは状況によっては危険な場合があります。

最小限の特権の原則に従って、**Credential** パラメーター (コマンドにある場合) を使用し、コマンドごとにドメイン管理アカウントに昇格します。 `Get-CimInstance` には **Credential** パラメーターがないため、このシナリオの解決策は、**CimSession** を先に作成することです。 次に、コンピューター名の代わりに **CimSession** を使用して、リモート コンピューター上の WMI のクエリを実行します。

```powershell
$CimSession = New-CimSession -ComputerName dc01 -Credential (Get-Credential)
```

```Output
cmdlet Get-Credential at command pipeline position 1
Supply values for the following parameters:
Credential
```

CIM セッションは、`$CimSession` という名前の変数に格納されました。 先に実行されるようにかっこ内で `Get-Credential` コマンドレットを指定したことにも注目してください。これで、新しいセッションが作成される前に、代替の資格情報が要求されます。 この章の後半で、代替の資格情報を指定する、より効率的な別の方法を紹介しますが、複雑化する前にこの基本的な概念を理解しておくことが重要です。

前の例で作成した CIM セッションを `Get-CimInstance` コマンドレットと共に使用して、リモート コンピューター上の WMI から BIOS 情報を照会できるようになりました。

```powershell
Get-CimInstance -CimSession $CimSession -ClassName Win32_BIOS
```

```Output
SMBIOSBIOSVersion : 090006
Manufacturer      : American Megatrends Inc.
Name              : Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
SerialNumber      : 0986-6980-3916-0512-6608-8243-13
Version           : VRTUAL - 4001628
PSComputerName    : dc01
```

単にコンピューター名を指定するのではなく、CIM セッションを使用することには、追加のメリットがいくつかあります。 同じコンピューターに対して複数のクエリを実行する場合、クエリごとにコンピューター名を使用するよりも、CIM セッションを使用する方が効率的です。 CIM セッションを作成するだけで、接続が一度に設定されます。
次に、その同じセッションが、複数のクエリで情報を取得するために使用されます。 コンピューター名を使用する場合、コマンドレットで個別のクエリごとに接続を設定して破棄しなければなりません。

`Get-CimInstance` コマンドレットでは既定で WSMan プロトコルを使用します。これは、リモート コンピューターが接続するために PowerShell バージョン 3.0 以上が必要であることを意味します。 実際に問題になるのは、PowerShell バージョンではなくスタック バージョンです。 スタック バージョンは、`Test-WSMan` コマンドレットを使用して特定できます。
バージョン 3.0 が必要です。 これは、PowerShell バージョン 3.0 以上で見つかるバージョンです。

```powershell
Test-WSMan -ComputerName dc01
```

```Output
wsmid           : http://schemas.dmtf.org/wbem/wsman/identity/1/wsmanidentity.xsd
ProtocolVersion : http://schemas.dmtf.org/wbem/wsman/1/wsman.xsd
ProductVendor   : Microsoft Corporation
ProductVersion  : OS: 0.0.0 SP: 0.0 Stack: 3.0
```

古い WMI コマンドレットでは、以前のバージョンの Windows と互換性がある DCOM プロトコルを使用しています。 しかし、DCOM は新しいバージョンの Windows でファイアウォールによってブロックされるのが普通です。 `New-CimSessionOption` コマンドレットでは、`New-CimSession` で使用するための DCOM プロトコル接続を作成できます。 これにより、Windows Server 2000 と同じくらい古いバージョンの Windows と通信するために `Get-CimInstance` コマンドレットを使用できます。 これはまた、DCOM プロトコルを使うように構成された CimSession と共に `Get-CimInstance` コマンドレットを使用する場合、リモート コンピューター上に PowerShell が必要ないことを意味します。

`New-CimSessionOption` コマンドレットを使用して DCOM プロトコル オプションを作成し、それを変数に格納します。

```powershell
$DCOM = New-CimSessionOption -Protocol Dcom
```

効率を上げるために、ドメイン管理者または管理者特権の資格情報を変数に格納できます。そうすることで、それらをコマンドごとに何度も入力する必要がなくなります。

```powershell
$Cred = Get-Credential
```

```Output
cmdlet Get-Credential at command pipeline position 1
Supply values for the following parameters:
Credential
```

Windows Server 2008 (R2 以外) を実行する SQL03 という名前のサーバーがあります。 これは、既定では PowerShell がインストールされていない最新の Windows Server オペレーティング システムです。

DCOM プロトコルを使用して、SQL03 への **CimSession** を作成します。

```powershell
$CimSession = New-CimSession -ComputerName sql03 -SessionOption $DCOM -Credential $Cred
```

前のコマンドでは、今度は再び手動で入力するのではなく、**Credential** パラメーターの値として `$Cred` という名前の変数を指定したことに注目してください。

クエリの出力は、基になるプロトコルとして何が使用されているかに関係なく同じです。

```powershell
Get-CimInstance -CimSession $CimSession -ClassName Win32_BIOS
```

```Output
SMBIOSBIOSVersion : 090006
Manufacturer      : American Megatrends Inc.
Name              : Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
SerialNumber      : 7237-7483-8873-8926-7271-5004-86
Version           : VRTUAL - 4001628
PSComputerName    : sql03
```

`Get-CimSession` コマンドレットは、現在接続されている **CimSession** の内容と、それらで使用されているプロトコルを確認するために使用されます。

```powershell
Get-CimSession
```

```Output
Id           : 1
Name         : CimSession1
InstanceId   : 80742787-e38e-41b1-a7d7-fa1369cf1402
ComputerName : dc01
Protocol     : WSMAN

Id           : 2
Name         : CimSession2
InstanceId   : 8fcabd81-43cf-4682-bd53-ccce1e24aecb
ComputerName : sql03
Protocol     : DCOM
```

前に作成した **CimSession** を両方取得して、`$CimSession` という名前の変数に格納します。

```powershell
$CimSession = Get-CimSession
```

1 つのコマンドを使用して、両方のコンピューターに対してクエリを実行します。1 つには WSMan プロトコルを、もう 1 つには DCOM を使用します。

```powershell
Get-CimInstance -CimSession $CimSession -ClassName Win32_BIOS
```

```Output
SMBIOSBIOSVersion : 090006
Manufacturer      : American Megatrends Inc.
Name              : Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
SerialNumber      : 0986-6980-3916-0512-6608-8243-13
Version           : VRTUAL - 4001628
PSComputerName    : dc01

SMBIOSBIOSVersion : 090006
Manufacturer      : American Megatrends Inc.
Name              : Intel(R) Xeon(R) CPU E3-1505M v5 @ 2.80GHz
SerialNumber      : 7237-7483-8873-8926-7271-5004-86
Version           : VRTUAL - 4001628
PSComputerName    : sql03
```

私は WMI と CIM のコマンドレットについて多数のブログ記事を執筆しています。 最も役に立つものの 1 つでは、WSMan と DCOM のどちらを使用すべきかを自動的に特定し、どれを手動で設定するかを把握する必要なしに CIM セッションを自動的に設定するために作成した関数について説明しています。 そのブログ記事のタイトルは、「[DCOM へのフォールバックを使用してリモート コンピューターへの CimSession を作成する PowerShell 関数][]」です。

CIM セッションが完了したら、`Remove-CimSession` コマンドレットを使用してそれらを削除する必要があります。 すべての CIM セッションを削除するには、`Get-CimSession` を `Remove-CimSession` にパイプ処理するだけでかまいません。

```powershell
Get-CimSession | Remove-CimSession
```

## <a name="summary"></a>まとめ

この章では、PowerShell を使用してローカルとリモート両方のコンピューター上の WMI を操作する方法について学習しました。 さらに、CIM コマンドレットを使用して、WSMan と DCOM の両方のプロトコルでリモート コンピューターを操作する方法を学習しました。

## <a name="review"></a>確認

1. WMI コマンドレットと CIM コマンドレットの違いは何か。
1. 既定では、`Get-CimInstance` コマンドレットで使用されるプロトコルは何か。
1. `Get-CimInstance` を使用してコンピューター名を指定する代わりに、CIM セッションを使用するメリットは何か。
1. `Get-CimInstance` で使用する既定のプロトコルとは異なる、別のプロトコルを指定する方法は何か。
1. CIM セッションはどのように終了または削除するか。

## <a name="recommended-reading"></a>推奨トピック

- [about_WMI][]
- [about_WMI_Cmdlets][]
- [about_WQL][]
- [CimCmdlets モジュール][]
- [ビデオ: CIM コマンドレットと CIM セッションの使用][]

<!-- link references -->
[about_WMI]: /powershell/module/microsoft.powershell.core/about/about_wmi
[about_WMI_Cmdlets]: /powershell/module/microsoft.powershell.core/about/about_wmi_cmdlets
[about_WQL]: /powershell/module/microsoft.powershell.core/about/about_wql
[CimCmdlets モジュール]: /powershell/module/cimcmdlets/
[ビデオ: CIM コマンドレットと CIM セッションの使用]: https://mikefrobbins.com/2013/09/12/phillyposh-user-group-meeting-presentation-follow-up-powershell-second-hop-problem-with-cimsessions/
[DCOM へのフォールバックを使用してリモート コンピューターへの CimSession を作成する PowerShell 関数]: https://mikefrobbins.com/2014/08/28/powershell-function-to-create-cimsessions-to-remote-computers-with-fallback-to-dcom/
