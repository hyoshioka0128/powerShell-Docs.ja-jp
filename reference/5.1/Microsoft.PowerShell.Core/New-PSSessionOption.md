---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 02/07/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-pssessionoption?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-PSSessionOption
ms.openlocfilehash: 566091a53941cd122a10aa2bcb45cddfb1f40cc4
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388622"
---
# New-PSSessionOption

## 概要
PSSession の詳細設定オプションが格納されたオブジェクトを作成します。

## SYNTAX

```
New-PSSessionOption [-MaximumRedirection <Int32>] [-NoCompression] [-NoMachineProfile] [-Culture <CultureInfo>]
 [-UICulture <CultureInfo>] [-MaximumReceivedDataSizePerCommand <Int32>] [-MaximumReceivedObjectSize <Int32>]
 [-OutputBufferingMode <OutputBufferingMode>] [-MaxConnectionRetryCount <Int32>]
 [-ApplicationArguments <PSPrimitiveDictionary>] [-OpenTimeout <Int32>] [-CancelTimeout <Int32>]
 [-IdleTimeout <Int32>] [-ProxyAccessType <ProxyAccessType>] [-ProxyAuthentication <AuthenticationMechanism>]
 [-ProxyCredential <PSCredential>] [-SkipCACheck] [-SkipCNCheck] [-SkipRevocationCheck]
 [-OperationTimeout <Int32>] [-NoEncryption] [-UseUTF16] [-IncludePortInSPN] [<CommonParameters>]
```

## Description

`New-PSSessionOption`コマンドレットにより、ユーザー管理セッション ( **PSSession** ) の詳細オプションを含むオブジェクトが作成されます。 オブジェクトは、、、などの **PSSession** を作成するコマンドレットの **sessionoption** パラメーターの値として使用でき `New-PSSession` `Enter-PSSession` `Invoke-Command` ます。

パラメーターを指定しない場合は、 `New-PSSessionOption` すべてのオプションの既定値を含むオブジェクトが生成されます。 プロパティはすべて編集できるため、結果のオブジェクトをテンプレートとして使用して、会社用の標準的なオプション オブジェクトを作成できます。

セッションオプションオブジェクトは、ユーザー設定変数に保存することもでき `$PSSessionOption` ます。 この変数の値は、セッション オプションの新しい既定値を確立します。 これらの値は、セッションに対してセッション オプションが設定されていない場合に有効になり、セッション構成に設定されたオプションよりも優先されます。ただし、セッション オプションを指定するか、セッションを作成するコマンドレットでセッション オプション オブジェクトを作成することによって、これらの値をオーバーライドすることができます。 ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

セッションを作成するコマンドレットでセッション オプション オブジェクトを使用すると、セッション オプション値は、$PSSessionOption ユーザー設定変数およびセッション構成に設定されたセッションの既定値よりも優先されます。 ただし、セッション構成で設定された最大値、クォータ、または制限よりも優先されることはありません。 セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

## 例

### 例 1: 既定のセッションオプションを作成する

このコマンドは、既定値をすべて持つセッションオプションオブジェクトを作成します。

```powershell
New-PSSessionOption
```

```Output
MaximumConnectionRedirectionCount : 5
NoCompression                     : False
NoMachineProfile                  : False
ProxyAccessType                   : IEConfig
ProxyAuthentication               : Negotiate
ProxyCredential                   :
SkipCACheck                       : False
SkipCNCheck                       : False
SkipRevocationCheck               : False
OperationTimeout                  : 00:03:00
NoEncryption                      : False
UseUTF16                          : False
Culture                           :
UICulture                         :
MaximumReceivedDataSizePerCommand :
MaximumReceivedObjectSize         :
ApplicationArguments              :
OpenTimeout                       : 00:03:00
CancelTimeout                     : 00:01:00
IdleTimeout                       : 00:04:00
```

### 例 2: セッションオプションオブジェクトを使用してセッションを構成する

この例では、セッション オプション オブジェクトを使用してセッションを構成する方法を示します。

```powershell
$pso = New-PSSessionOption -Culture "fr-fr" -MaximumReceivedObjectSize 10MB
New-PSSession -ComputerName Server01 -SessionOption $pso
```

最初のコマンドは、新しいセッションオプションオブジェクトを作成し、変数の値に保存し `$pso` ます。 2番目のコマンドは、コマンドレットを使用して、 `New-PSSession` Server01 リモートコンピューター上にセッションを作成します。 このコマンドは、変数の値に含まれる session option オブジェクトを、 `$pso` コマンドの **sessionoption** パラメーターの値として使用します。

### 例 3: 対話型セッションを開始する

このコマンドは、 `Enter-PSSession` コマンドレットを使用して、Server01 コンピューターとの対話型セッションを開始します。

```powershell
Enter-PSSession -ComputerName Server01 -SessionOption (New-PSSessionOption -NoEncryption -NoCompression)
```

**Sessionoption** パラメーターの値は、 `New-PSSessionOption` **Noencryption** および **noencryption** パラメーターを持つコマンドです。

コマンドは、 `New-PSSessionOption` コマンドの前に実行されるように、かっこで囲まれてい `Enter-PSSession` ます。

### 例 4: セッションオプションオブジェクトを変更する

この例は、セッションオプションオブジェクトを変更できることを示しています。 すべてのプロパティは、読み取り/書き込み値を持ちます。

```powershell
$a = New-PSSessionOption
$a.OpenTimeout
```

```Output
Days              : 0
Hours             : 0
Minutes           : 3
Seconds           : 0
Milliseconds      : 0
Ticks             : 1800000000
TotalDays         : 0.00208333333333333
TotalHours        : 0.05
TotalMinutes      : 3
TotalSeconds      : 180
TotalMilliseconds : 180000
```

```powershell
$a.UICulture = (Get-UICulture)
$a.OpenTimeout = (New-Timespan -Minutes 4)
$a.MaximumConnectionRedirectionCount = 1
$a
```

```Output
MaximumConnectionRedirectionCount : 1
NoCompression                     : False
NoMachineProfile                  : False
ProxyAccessType                   : IEConfig
ProxyAuthentication               : Negotiate
ProxyCredential                   :
SkipCACheck                       : False
SkipCNCheck                       : False
SkipRevocationCheck               : False
OperationTimeout                  : 00:03:00
NoEncryption                      : False
UseUTF16                          : False
Culture                           :
UICulture                         : en-US
MaximumReceivedDataSizePerCommand :
MaximumReceivedObjectSize         :
ApplicationArguments              :
OpenTimeout                       : 00:04:00
CancelTimeout                     : 00:01:00
IdleTimeout                       : 00:04:00
```

この方法を使用して、会社の標準のセッション オブジェクトを作成し、特定の用途向けにカスタマイズされたバージョンを作成します。

### 例 5: ユーザー設定変数を作成する

このコマンドは、ユーザー設定変数を作成し `$PSSessionOption` ます。

```powershell
$PSSessionOption = New-PSSessionOption -OpenTimeOut 120000
```

`$PSSessionOption`セッションでユーザー設定変数が発生すると、 `New-PSSession` 、 `Enter-PSSession` 、およびコマンドレットを使用して作成されたセッションのオプションの既定値が確立され `Invoke-Command` ます。

`$PSSessionOption`変数をすべてのセッションで使用できるようにするには、powershell セッションと powershell プロファイルに追加します。

ユーザー設定変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。
プロファイルの詳細については、「[about_Profiles](About/about_Profiles.md)」を参照してください。

### 例 6: リモートセッション構成の要件を満たす

この例では、 **SessionOption** オブジェクトを使用してリモート セッション構成の要件を満たす方法を示します。

```powershell
$skipCN = New-PSSessionOption -SkipCNCheck
New-PSSession -ComputerName 171.09.21.207 -UseSSL -Credential Domain01\User01 -SessionOption $SkipCN
```

最初のコマンドは、 `New-PSSessionOption` コマンドレットを使用して、 **Skipcncheck** プロパティを持つセッションオプションオブジェクトを作成します。 このコマンドは、結果として得られるセッションオブジェクトを変数に保存し `$skipCN` ます。

2番目のコマンドは、コマンドレットを使用して、 `New-PSSession` リモートコンピューター上に新しいセッションを作成します。 `$skipCN`Check 変数は、 **sessionoption** パラメーターの値で使用されます。

コンピューターは IP アドレスによって識別されるので、 **ComputerName** パラメーターの値は、SECURE SOCKETS LAYER (SSL) に使用されている証明書の共通名のいずれとも一致しません。 その結果、 **SkipCNCheck** オプションが必要になります。

### 例 7: リモートセッションで引数を使用できるようにする

この例では、コマンドレットの **Applicationarguments** パラメーターを使用して `New-PSSessionOption` 、リモートセッションで追加のデータを使用できるようにする方法を示します。

```powershell
$team = @{Team="IT"; Use="Testing"}
$TeamOption = New-PSSessionOption -ApplicationArguments $team
$s = New-PSSession -ComputerName Server01 -SessionOption $TeamOption
Invoke-Command -Session $s {$PSSenderInfo.ApplicationArguments}
```

```Output
Name                 Value
----                 -----
Team                 IT
Use                  Testing
PSVersionTable       {CLRVersion, BuildVersion, PSVersion, WSManStackVersion...}
```

```powershell
Invoke-Command -Session $s {
  if ($PSSenderInfo.ApplicationArguments.Use -ne "Testing") {
    .\logFiles.ps1
  }
  else {
    "Just testing."
  }
}
```

```Output
Just testing.
```

最初のコマンドは、 **チーム** と **使用** の2つのキーを持つハッシュテーブルを作成します。 このコマンドは、ハッシュテーブルを変数に保存し `$team` ます。 ハッシュ テーブルの詳細については、「[about_Hash_Tables (ハッシュ テーブルについて)](about/about_Hash_Tables.md)」をご覧ください。

次に、 `New-PSSessionOption` **applicationarguments** パラメーターを使用してコマンドレットを作成し、変数に保存されたセッションオプションオブジェクトを作成し `$team` ます。 `New-PSSessionOption`は、セッションオプションオブジェクトを作成するときに、 **applicationarguments** パラメーターの値のハッシュテーブルをプリミティブディクショナリに自動的に変換します。これにより、データが確実にリモートセッションに送信されるようになります。

`New-PSSession`コマンドレットは、Server01 コンピューターでセッションを開始します。 この例では、 **Sessionoption** パラメーターを使用して、変数にオプションを含めて `$teamOption` います。

`Invoke-Command`コマンドレットでは、変数内のデータを `$team` リモートセッションのコマンドで使用できることを示しています。 データは、自動変数の **Applicationarguments** プロパティに表示され `$PSSenderInfo` ます。

最後に、 `Invoke-Command` データがどのように使用されるかを示します。

## PARAMETERS

### -ApplicationArguments

リモート セッションに送信されるプリミティブな辞書を指定します。 セッション構成のスタートアップスクリプトを含む、リモートセッションのコマンドとスクリプトは、自動変数の **Applicationarguments** プロパティでこのディクショナリを見つけることができ `$PSSenderInfo` ます。 このパラメーターを使用すると、リモート セッションにデータを送信することができます。

詳細については、「 [about_Hash_Tables](about/about_Hash_Tables.md)、 [about_Session_Configurations](About/about_Session_Configurations.md)、および [about_Automatic_Variables](about/about_Automatic_Variables.md)」を参照してください。

```yaml
Type: System.Management.Automation.PSPrimitiveDictionary
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -CancelTimeout

PowerShell がキャンセル操作 (CTRL + C) が終了するまで待機する時間を指定します。
値をミリ秒単位で入力します。

既定値は 60,000 (1 分) です。 値 0 (ゼロ) は、タイムアウトがないことを表します。つまり、コマンドは無期限に続行されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: CancelTimeoutMSec

Required: False
Position: Named
Default value: 60000
Accept pipeline input: False
Accept wildcard characters: False
```

### -Culture

セッションに使用するカルチャを指定します。 カルチャ名を `<languagecode2>-<country/regioncode2>` 形式 (like など `ja-JP` )、 **cultureinfo** オブジェクトを含む変数、または **cultureinfo** オブジェクトを取得するコマンドを入力します。

既定値はであり、 `$Null` オペレーティングシステムで設定されているカルチャがセッションで使用されます。

```yaml
Type: System.Globalization.CultureInfo
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IdleTimeout

リモートコンピューターがローカルコンピューターから通信を受信していない場合に、セッションが開いたままになる期間を指定します。 これにはハートビート信号も含まれます。 この期間が経過すると、セッションは閉じられます。

セッションを切断して再接続する場合、アイドルタイムアウト値は非常に重要です。 セッションがタイムアウトしていない場合にのみ再接続することができます。

値をミリ秒単位で入力します。 最小値は 6万 (1 分) です。 最大値は、セッション構成の **MaxIdleTimeoutms** プロパティの値です。 既定値の-1 では、アイドルタイムアウトは設定されません。

セッションでは、セッションオプションで設定されているアイドルタイムアウトを使用します (存在する場合)。 何も設定されていない場合 (-1)、セッション構成の **IdleTimeoutMs** プロパティの値、または WSMan シェルのタイムアウト値 ( `WSMan:\<ComputerName>\Shell\IdleTimeout` ) のいずれか短い方が使用されます。

セッション オプションに設定されたアイドル タイムアウトがセッション構成の **MaxIdleTimeoutMs** プロパティの値を超える場合、セッション作成するコマンドは失敗します。

**IdleTimeoutMs** **の既定の** セッション構成の値は720万ミリ秒 (2 時間) です。 **MaxIdleTimeoutMs** 値は2147483647ミリ秒 ( \> 24 日) です。 WSMan シェルのアイドルタイムアウト () の既定値 `WSMan:\<ComputerName>\Shell\IdleTimeout` は720万ミリ秒 (2 時間) です。

セッションのアイドルタイムアウト値は、セッションから切断するときや、セッションに再接続するときに変更することもできます。 詳細については、次のトピックを参照してください。 `Disconnect-PSSession` および `Connect-PSSession`

Windows PowerShell 2.0 では、 **IdleTimeout** パラメーターの既定値は 240,000 (4 分) です。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: IdleTimeoutMSec

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IncludePortInSPN

Kerberos 認証に使用されるサービスプリンシパル名 (SPN) のポート番号が含まれます。たとえば、のように `HTTP://<ComputerName>:5985` なります。 このオプションを使用すると、クライアントは、既定以外の SPN を使用して、Kerberos 認証を使用するリモート コンピューターに対して認証できます。

このオプションは、Kerberos 認証をサポートする複数のサービスが異なるユーザー アカウントで実行されている企業向けに設計されています。 たとえば、Kerberos 認証を許可する IIS アプリケーションでは、既定の SPN をコンピューターアカウントとは異なるユーザーアカウントに登録する必要がある場合があります。 このような場合、PowerShell リモート処理では、コンピューターアカウントに登録されている SPN が必要であるため、認証に Kerberos を使用することはできません。 この問題を解決するには、管理者は、さまざまなユーザーアカウントに登録されていて、SPN にポート番号を含めることによって異なる Spn を作成できます ( **Setspn.exe** を使用するなど)。

詳細については、「 [Setspn の概要](/previous-versions/windows/it-pro/windows-server-2003/cc773257(v=ws.10))」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

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

### -MaxConnectionRetryCount

現在の試行がネットワークの問題のために失敗した場合に、PowerShell が対象コンピューターへの接続を試行する回数を指定します。 既定値は 5 です。

このパラメーターは、PowerShell バージョン5.0 用に追加されました。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumReceivedDataSizePerCommand

ローカル コンピューターがリモート コンピューターから 1 つのコマンドで受け取ることができる最大バイト数を指定します。 値をバイト単位で入力します。 既定では、データのサイズに関する制限はありません。

このオプションは、クライアント コンピューター上のリソースを保護するために用意されています。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumReceivedObjectSize

ローカル コンピューターがリモート コンピューターから受信できるオブジェクトの最大サイズを指定します。 このオプションは、クライアント コンピューター上のリソースを保護するために用意されています。 値をバイト単位で入力します。

Windows PowerShell 2.0 では、このパラメーターを省略した場合、オブジェクトのサイズに関する制限はありません。 Windows PowerShell 3.0 以降では、このパラメーターを省略した場合、既定値は 200 MB です。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 200 MB
Accept pipeline input: False
Accept wildcard characters: False
```

### -MaximumRedirection

接続に失敗する前に PowerShell が代替 Uniform Resource Identifier (URI) に接続をリダイレクトする回数を指定します。 既定値は 5 です。 値 0 (ゼロ) を指定した場合、リダイレクトはまったく行われません。

このオプションは、セッションを作成するコマンドで **Allowredirection** パラメーターが使用されている場合にのみ、セッションで使用されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 5
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoCompression

セッションでのパケットの圧縮を無効にします。 圧縮処理には多くのプロセッサ サイクルが使用されますが、転送が高速化されます。

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

### -NoEncryption

データの暗号化を無効にします。

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

### -NoMachineProfile

ユーザーの Windows ユーザー プロファイルを読み込まないようにします。 その場合、セッションをより高速に作成できる可能性がありますが、ユーザー固有のレジストリ設定、環境変数などの項目、および証明書をセッションで使用できなくなります。

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

### -OpenTimeout

セッションの接続が確立されるのをクライアント コンピューターが待機する期間を指定します。 この期間が経過すると、接続を確立するコマンドは失敗します。 値をミリ秒単位で入力します。

既定値は 180,000 (3 分) です。 値 0 (ゼロ) は、タイムアウトがないことを表します。つまり、コマンドは無期限に続行されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: OpenTimeoutMSec

Required: False
Position: Named
Default value: 180000 (3 minutes)
Accept pipeline input: False
Accept wildcard characters: False
```

### -OperationTimeout

セッションの任意の操作を実行できる最大時間を決定します。 この期間が経過すると、操作は失敗します。 値をミリ秒単位で入力します。

既定値は 180,000 (3 分) です。 値 0 (ゼロ) は、タイムアウトがないことを表します。つまり、操作は無期限に続行されます。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases: OperationTimeoutMSec

Required: False
Position: Named
Default value: 180000 (3 minutes)
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutputBufferingMode

出力バッファーがいっぱいになったときに切断されたセッションでコマンドの出力を管理する方法を指定します。

出力バッファー モードがセッションまたはセッション構成で設定されていない場合、既定値は **Block** です。 ユーザーは、セッションの切断時に出力バッファー モードを変更することもできます。

このパラメーターを省略した場合、セッションオプションオブジェクトの **OutputBufferingMode** の値は None になります。 値 **Block** または **Drop** は、セッション構成に設定された出力バッファー モード トランスポート オプションをオーバーライドします。 このパラメーターの有効値は、次のとおりです。

- ブロック。 出力バッファーがいっぱいのとき、バッファーが空くまで実行が中断されます。
- Drop :  出力バッファーがいっぱいのとき、実行は続行されます。 新しい出力が保存されると、最も古い出力が破棄されます。
- なし。 出力バッファー モードが指定されません。

出力バッファーモードトランスポートオプションの詳細については、「」を参照してください `New-PSTransportOption` 。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.Runspaces.OutputBufferingMode
Parameter Sets: (All)
Aliases:
Accepted values: None, Drop, Block

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -System.management.automation.remoting.proxyaccesstype

ホスト名の解決に使用するメカニズムを指定します。 このパラメーターの有効値は、次のとおりです。

- IEConfig
- WinHttpConfig
- AutoDetect
- NoProxyServer
- なし

既定値は None です。

このパラメーターの値の詳細については、「 [System.management.automation.remoting.proxyaccesstype Enumeration](/dotnet/api/system.management.automation.remoting.proxyaccesstype)」を参照してください。

```yaml
Type: System.Management.Automation.Remoting.ProxyAccessType
Parameter Sets: (All)
Aliases:
Accepted values: None, IEConfig, WinHttpConfig, AutoDetect, NoProxyServer

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyAuthentication

プロキシの解決に使用する認証方法を指定します。 このパラメーターに指定できる値は、 **Basic** 、 **Digest** 、および **Negotiate** です。 既定値は **Negotiate** です。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

```yaml
Type: System.Management.Automation.Runspaces.AuthenticationMechanism
Parameter Sets: (All)
Aliases:
Accepted values: Default, Basic, Negotiate, NegotiateWithImplicitCredential, Credssp, Digest, Kerberos

Required: False
Position: Named
Default value: Negotiate
Accept pipeline input: False
Accept wildcard characters: False
```

### -ProxyCredential

プロキシ認証に使用する資格情報を指定します。 **Pscredential** オブジェクトを含む変数、またはコマンドなどの **pscredential** オブジェクトを取得するコマンドを入力し `Get-Credential` ます。 このオプションを設定しない場合、資格情報は指定されません。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SkipCACheck

HTTPS 経由で接続する場合、クライアントは、サーバー証明書が信頼された証明機関 (CA) によって署名されているかどうかを検証しません。

このオプションは、リモート コンピューターが別のメカニズムを使用して信頼されている場合にのみ使用します。たとえば、リモート コンピューターが物理的に安全であり切り離されているネットワークの一部である場合や、WinRM 構成において信頼されるホストとして記載されている場合が該当します。

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

### -SkipCNCheck

サーバーの証明書の共通名 (CN) がサーバーのホスト名と一致する必要がないことを指定します。 このオプションは、HTTPS プロトコルを使用するリモート操作でのみ使用されます。

このオプションは、信頼されるコンピューターに対してのみ使用します。

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

### -SkipRevocationCheck

サーバー証明書の失効状態を検証しません。

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

### -UICulture

セッションに使用する UI カルチャを指定します。

有効な値は、次のとおりです。

- 形式のカルチャ名 `<languagecode2>-<country/regioncode2>` (など) `ja-JP`
- **CultureInfo** オブジェクトを含む変数
- **CultureInfo** オブジェクトを取得するコマンド (`Get-Culture`

既定値は `$null` で、セッションの作成時にオペレーティングシステムで設定された UI カルチャがセッションで使用されます。

```yaml
Type: System.Globalization.CultureInfo
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseUTF16

このコマンドレットが、要求を UTF8 形式ではなく UTF16 形式でエンコードすることを示します。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システムの管理.... PSSessionOption

## 注

**PSSession** を作成するためにコマンドで **sessionoption** パラメーターを使用しない場合、セッションオプションは、設定されている場合は、ユーザー設定変数のプロパティ値によって決まり `$PSSessionOption` ます。 変数の詳細については `$PSSessionOption` 、「 [about_Preference_Variables](About/about_Preference_Variables.md)」を参照してください。

セッション構成オブジェクトのプロパティは、セッション構成に設定されているオプションとその値によって異なります。 また、セッション構成ファイルを使用するセッション構成には追加のプロパティがあります。

## 関連リンク

[Enter-PSSession](Enter-PSSession.md)

[Invoke-Command](Invoke-Command.md)

[New-PSSession](New-PSSession.md)
