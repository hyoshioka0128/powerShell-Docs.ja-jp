---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/disconnect-pssession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disconnect-PSSession
ms.openlocfilehash: b87db1af49ae293225f417fd4bec85e346c79f28
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599516"
---
# Disconnect-PSSession

## 概要
セッションから切断します。

## SYNTAX

### セッション (既定)

```
Disconnect-PSSession [-Session] <PSSession[]> [-IdleTimeoutSec <Int32>]
 [-OutputBufferingMode <OutputBufferingMode>] [-ThrottleLimit <Int32>] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

### 名前

```
Disconnect-PSSession [-IdleTimeoutSec <Int32>] [-OutputBufferingMode <OutputBufferingMode>]
 [-ThrottleLimit <Int32>] -Name <String[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### InstanceId

```
Disconnect-PSSession [-IdleTimeoutSec <Int32>] [-OutputBufferingMode <OutputBufferingMode>]
 [-ThrottleLimit <Int32>] -InstanceId <Guid[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

### Id

```
Disconnect-PSSession [-IdleTimeoutSec <Int32>] [-OutputBufferingMode <OutputBufferingMode>]
 [-ThrottleLimit <Int32>] [-Id] <Int32[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットは、 `Disconnect-PSSession` コマンドレットを使用して開始された PowerShell セッション ("PSSession") を `New-PSSession` 現在のセッションから切断します。 その結果、PSSession は切断状態になります。 切断された PSSession には、現在のセッションから、またはローカル コンピューターや別のコンピューターの別のセッションから接続できます。

`Disconnect-PSSession`コマンドレットは、現在のセッションに接続されている開いている pssession のみを切断します。 `Disconnect-PSSession` 壊れた、または閉じた PSSessions、またはコマンドレットを使用して開始された対話型 PSSessions を切断することはできません。また、 `Enter-PSSession` 他のセッションに接続している pssession を切断することもできません。

切断された PSSession に再接続するに `Connect-PSSession` は、またはコマンドレットを使用し `Receive-PSSession` ます。

PSSession がタイムアウトになるか、出力バッファーがいっぱいになって PSSession 内のコマンドがブロックされた場合を除き、PSSession が切断された場合でも、PSSession 内のコマンドは完了するまで継続して実行されます。 アイドル タイムアウトを変更するには、**IdleTimeoutSec** パラメーターを使用します。 出力バッファーモードを変更するには、 **OutputBufferingMode** パラメーターを使用して、コマンドレットの **indisconnectedsession** パラメーターを使用して、 `Invoke-Command` 切断されたセッションでコマンドを実行することもできます。

切断されたセッションの機能の詳細については、「[about_Remote_Disconnected_Sessions](./About/about_Remote_Disconnected_Sessions.md)」を参照してください。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1-名前を指定してセッションを切断する

このコマンドは、Server01 コンピューター上の UpdateSession PSSession を現在のセッションから切断します。 このコマンドは、 **Name** パラメーターを使用して PSSession を識別します。

```
PS> Disconnect-PSSession -Name UpdateSession
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
1  UpdateSession   Server01        Disconnected  Microsoft.PowerShell          None
```

出力には、切断の試行が成功したことが示されます。 セッションの状態は Disconnected であり、Availability は None です。つまり、セッションがビジー状態ではなく、再接続できることを示しています。

### 例 2-特定のコンピューターからセッションを切断する

このコマンドは、Server12 コンピューター上の ITTask PSSession を現在のセッションから切断します。
ITTask セッションは、現在のセッションで作成され、Server12 コンピューターに接続されています。 このコマンドは、コマンドレットを使用してセッションを取得し、コマンドレットを使用し `Get-PSSession` てセッションを `Disconnect-PSSession` 切断します。

```
PS> Get-PSSession -ComputerName Server12 -Name ITTask |
  Disconnect-PSSession -OutputBufferingMode Drop -IdleTimeoutSec 86400
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
1  ITTask          Server12        Disconnected  ITTasks               None
```

この `Disconnect-PSSession` コマンドは、 **OutputBufferingMode** パラメーターを使用して出力モードを **Drop** に設定します。 この設定により、セッション出力バッファーがいっぱいになった場合でも、セッションで実行されているスクリプトの実行が続行されます。 スクリプトの出力はファイル共有のレポートに書き込まれるため、他の出力は失われる可能性があります。

また、このコマンドでは、**IdleTimeoutSec** パラメーターを使用して、セッションのアイドル タイムアウトを 24 時間に延長します。 この設定により、この管理者または他の管理者はセッションに再接続し、実行されたスクリプトを検証して、必要に応じてトラブルシューティングを行うための時間を得ることができます。

### 例 3-複数のコンピューターでの複数の pssession の使用

この一連のコマンドは、 `Disconnect-PSSession` エンタープライズシナリオでコマンドレットがどのように使用されるかを示しています。 このケースでは、新しい技術者がリモート コンピューター上のセッションのスクリプトを開始したときに問題が発生したため、 経験豊富なマネージャーによるセッションへの接続および問題解決を依頼できるように、セッションを切断しました。

```
PS> $s = New-PSSession -ComputerName Srv1, Srv2, Srv30 -Name ITTask
PS> Invoke-Command $s -FilePath \\Server01\Scripts\Get-PatchStatus.ps1
PS> Get-PSSession -Name ITTask -ComputerName Srv1 | Disconnect-PSSession
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
1 ITTask           Srv1            Disconnected  Microsoft.PowerShell          None

PS> Get-PSSession -ComputerName Srv1, Srv2, Srv30 -Name ITTask
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
 1 ITTask          Srv1            Disconnected  Microsoft.PowerShell          None
 2 ITTask          Srv2            Opened        Microsoft.PowerShell     Available
 3 ITTask          Srv30           Opened        Microsoft.PowerShell     Available

PS> Get-PSSession -ComputerName Srv1 -Name ITTask -Credential Domain01\User01
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
 1 ITTask          Srv1            Disconnected  Microsoft.PowerShell          None

PS> $s = Connect-PSSession -ComputerName Srv1 -Name ITTask -Credential Domain01\User01
PS> Invoke-Command -Session $s {dir $home\Scripts\PatchStatusOutput.ps1}
PS> Invoke-Command -Session $s {mkdir $home\Scripts\PatchStatusOutput}
PS> Invoke-Command -Session $s -FilePath \\Server01\Scripts\Get-PatchStatus.ps1
PS> Disconnect-PSSession -Session $s
```

この技術者は、最初に複数のリモート コンピューターでセッションを作成し、各セッションでスクリプトを実行しました。 最初のコマンドは、 `New-PSSession` コマンドレットを使用して、3台のリモートコンピューター上に ITTask セッションを作成します。 このコマンドは、セッションを $s 変数に格納します。 2番目のコマンドは、コマンドレットの **FilePath** パラメーターを使用して、 `Invoke-Command` $s 変数内のセッションでスクリプトを実行します。

Srv1 コンピューターで実行中のスクリプトで、予期しないエラーが生成されました。 技術者はマネージャーに連絡し、サポートを依頼しました。 マネージャーは、調査できるように、セッションから切断するように技術者に指示します。2番目のコマンドは、コマンドレットを使用して、 `Get-PSSession` Srv1 コンピューター上の ITTask セッションを取得し、コマンドレットを使用し `Disconnect-PSSession` て切断します。 このコマンドは、他のコンピューター上の ITTask セッションには影響しません。

3番目のコマンドは、 `Get-PSSession` コマンドレットを使用して ITTask セッションを取得します。 出力には、切断のコマンドが Srv2 および Srv30 コンピューターの ITTask セッションに影響を与えなかったことが示されます。

マネージャーはホームコンピューターにログオンし、会社のネットワークに接続して PowerShell を起動し、コマンドレットを使用して `Get-PSSession` Srv1 コンピューター上の ITTask セッションを取得します。 マネージャーは、技術者の資格情報を使用してセッションにアクセスします。

次に、コマンドレットを使用して、 `Connect-PSSession` Srv1 コンピューター上の ITTask セッションに接続します。 このコマンドは、セッションを $s 変数に保存します。

マネージャーは、コマンドレットを使用して、 `Invoke-Command` 変数内のセッションでいくつかの診断コマンドを実行し `$s` ます。 これにより、必要なディレクトリが見つからなかったためにスクリプトが失敗したことが判明しました。
マネージャーは、関数を使用して `MkDir` ディレクトリを作成し、スクリプトを再起動して `Get-PatchStatus.ps1` セッションから切断します。マネージャーは、その結果を技術者に報告し、タスクを完了するためにセッションに再接続し、必要なディレクトリが存在しない場合に作成するコマンドをスクリプトに追加するように指示し `Get-PatchStatus.ps1` ます。

### 例 4-PSSession のタイムアウト値を変更する

この例では、セッションを切断できるように **IdleTimeout** プロパティの値を修正する方法を示しています。

切断されたセッションが削除されるまでの期間は、セッションのアイドル タイムアウト プロパティで決定されるため、このプロパティは切断するセッションにとって重要です。 アイドル タイムアウト オプションは、セッションの作成時およびセッションの切断時に設定できます。 セッションのアイドルタイムアウトの既定値は、ローカルコンピューターのユーザー設定 `$PSSessionOption` 変数と、リモートコンピューター上のセッション構成で設定されます。 セッションに設定された値は、セッション構成に設定された値よりも優先されますが、セッション値はセッション構成で設定されたクォータ (**MaxIdleTimeoutMs** 値など) を超えることはできません。

```
PS> $Timeout = New-PSSessionOption -IdleTimeout 172800000
PS> $s = New-PSSession -Computer Server01 -Name ITTask -SessionOption $Timeout
PS> Disconnect-PSSession -Session $s
Disconnect-PSSession : The session ITTask cannot be disconnected because the specified
idle timeout value 172800(seconds) is either greater than the server maximum allowed
43200 (seconds) or less that the minimum allowed60(seconds).  Choose an idle time out
value that is within the allowed range and try again.

PS> Invoke-Command -ComputerName Server01 {Get-PSSessionConfiguration Microsoft.PowerShell} |
 Format-List -Property *

Architecture                  : 64
Filename                      : %windir%\system32\pwrshplugin.dll
ResourceUri                   : http://schemas.microsoft.com/powershell/microsoft.powershell
MaxConcurrentCommandsPerShell : 1000
UseSharedProcess              : false
ProcessIdleTimeoutSec         : 0
xmlns                         : http://schemas.microsoft.com/wbem/wsman/1/config/PluginConfiguration
MaxConcurrentUsers            : 5
lang                          : en-US
SupportsOptions               : true
ExactMatch                    : true
RunAsUser                     :
IdleTimeoutms                 : 7200000
PSVersion                     : 3.0
OutputBufferingMode           : Block
AutoRestart                   : false
SecurityDescriptorSddl        : O:NSG:BAD:P(A;;GA;;;BA)S:P(AU;FA;GA;;;WD)(AU;SA;GXGW;;;WD)
MaxMemoryPerShellMB           : 1024
MaxIdleTimeoutms              : 2147483647
Uri                           : http://schemas.microsoft.com/powershell/microsoft.powershell
SDKVersion                    : 2
Name                          : microsoft.powershell
XmlRenderingType              : text
Capability                    : {Shell}
RunAsPassword                 :
MaxProcessesPerShell          : 15
ParentResourceUri             : http://schemas.microsoft.com/powershell/microsoft.powershell
Enabled                       : true
MaxShells                     : 25
MaxShellsPerUser              : 25
Permission                    : BUILTIN\Administrators AccessAllowed
PSComputerName                : localhost
RunspaceId                    : aea84310-6dbf-4c21-90ac-13980039925a
PSShowComputerName            : True


PS> $s.Runspace.ConnectionInfo
ConnectionUri                     : http://Server01/wsman
ComputerName                      : Server01
Scheme                            : http
Port                              : 80
AppName                           : /wsman
Credential                        :
ShellUri                          : http://schemas.microsoft.com/powershell/Microsoft.PowerShell
AuthenticationMechanism           : Default
CertificateThumbprint             :
MaximumConnectionRedirectionCount : 5
MaximumReceivedDataSizePerCommand :
MaximumReceivedObjectSize         : 209715200
UseCompression                    : True
NoMachineProfile                  : False
ProxyAccessType                   : None
ProxyAuthentication               : Negotiate
ProxyCredential                   :
SkipCACheck                       : False
SkipCNCheck                       : False
SkipRevocationCheck               : False
NoEncryption                      : False
UseUTF16                          : False
OutputBufferingMode               : Drop
IncludePortInSPN                  : False
Culture                           : en-US
UICulture                         : en-US
OpenTimeout                       : 180000
CancelTimeout                     : 60000
OperationTimeout                  : 180000
IdleTimeout                       : 172800000

PS> Disconnect-PSSession $s -IdleTimeoutSec 43200
Id Name            ComputerName    State         ConfigurationName     Availability
-- ----            ------------    -----         -----------------     ------------
 4 ITTask          Server01        Disconnected  Microsoft.PowerShell          None

PS> $s.Runspace.ConnectionInfo.IdleTimeout
43200000
```

最初のコマンドは、コマンドレットを使用して `New-PSSessionOption` セッションオプションオブジェクトを作成します。 **IdleTimeout** パラメーターを使用して、アイドル タイムアウトを 48 時間 (172,800,000 ミリ秒) に設定します。 セッション オプション オブジェクトは $Timeout 変数に保存されます。

2番目のコマンドは、コマンドレットを使用して、 `New-PSSession` Server01 コンピューター上の ITTask セッションを作成します。 セッションは $Timeout 変数に保存されます。 **SessionOption** パラメーターの値は、$Timeout 変数内の 48 時間のアイドル タイムアウトです。

3 番目のコマンドは、$s 変数内の ITTask セッションを切断します。 セッションのアイドル タイムアウト値がセッション構成の **MaxIdleTimeoutMs** クォータを超えているため、このコマンドは失敗します。 アイドル タイムアウトはセッションが切断されるまで使用されないため、セッションの使用中にはこの違反が検出されない場合があります。

4番目のコマンドは、コマンドレットを使用して、 `Invoke-Command` `Get-PSSessionConfiguration` Server01 コンピューター上の Microsoft PowerShell セッション構成のコマンドを実行します。 このコマンドは、コマンドレットを使用して、 `Format-List` セッション構成のすべてのプロパティを一覧に表示します。出力には、セッション構成を使用するセッションで許可される **IdleTimeout** の最大値を確立する **MaxIdleTimeoutMS** プロパティが4320万ミリ秒 (12 時間) であることが示されています。

5 番目のコマンドは、$s 変数内のセッションのセッション オプション値を取得します。 多くのセッションオプションの値は、セッションの実行 **空間** プロパティの **ConnectionInfo** プロパティのプロパティです。出力は、セッションの **IdleTimeout** プロパティの値が1億7280万ミリ秒 (48 時間) であることを示しています。これは、セッション構成の12時間の **MaxIdleTimeoutMs** クォータに違反します。この競合を解決するには、 **ConfigurationName** パラメーターを使用して別のセッション構成を選択するか、 **IdleTimeout** パラメーターを使用してセッションのアイドルタイムアウトを減らすことができます。

6 番目のコマンドは、セッションを切断します。 これは **IdleTimeoutSec** パラメーターを使用して、アイドル タイムアウトを 12 時間に設定します。

7 番目のコマンドは、切断されたセッションのミリ秒で測定された **IdleTimeout** プロパティの値を取得します。 コマンドが成功したことを出力で確認します。

## PARAMETERS

### -Id

指定されたセッション ID を持つセッションから切断します。 1 つ以上の ID をコンマで区切って入力するか、範囲演算子 (..) を使用して ID の範囲を指定します。

セッションの ID を取得するには、コマンドレットを使用し `Get-PSSession` ます。 インスタンス ID は、セッションの **ID** プロパティに格納されています。

```yaml
Type: System.Int32[]
Parameter Sets: Id
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -IdleTimeoutSec

切断された PSSession のアイドル タイムアウトを変更します。 値を秒単位で入力します。 最小値は 60 (1 分) です。

アイドル タイムアウトにより、切断された PSSession がリモート コンピューターで維持される期間が決まります。 タイムアウトになると、PSSession が削除されます。

PSSession が切断されると、その時点からアイドル状態であると判断されます。これは、そのセッションでコマンドを実行している場合であっても同じです。

セッションのアイドル タイムアウトの既定値は、セッション構成の **IdleTimeoutMs** プロパティにより設定されます。 既定値は 7,200,000 ミリ秒 (2 時間) です。

このパラメーターの値は、$PSSessionOption 設定変数の **IdleTimeout** プロパティの値およびセッション構成の既定のアイドル タイムアウト値よりも優先されます。 ただし、この値はセッション構成の **MaxIdleTimeoutMs** プロパティの値を超えることはできません。 **MaxIdleTimeoutMs** の既定値は 12 時間 (43,200,000 ミリ秒) です。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 60
Accept pipeline input: False
Accept wildcard characters: False
```

### -InstanceId

指定されたインスタンス ID を持つセッションから切断します。

インスタンス ID は、ローカル コンピューターまたはリモート コンピューターのセッションを一意に識別する GUID です。 複数のコンピューター上の複数のセッションであっても、インスタンス ID は一意です。

セッションのインスタンス ID を取得するには、コマンドレットを使用し `Get-PSSession` ます。 インスタンス ID は、セッションの **InstanceID** プロパティに格納されます。

```yaml
Type: System.Guid[]
Parameter Sets: InstanceId
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Name

指定されたフレンドリ名を持つセッションから切断します。 ワイルドカードを使用できます。

セッションのフレンドリ名を取得するには、コマンドレットを使用し `Get-PSSession` ます。 フレンドリ名は、セッションの **Name** プロパティに格納されています。

```yaml
Type: System.String[]
Parameter Sets: Name
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -セッション

指定された PSSession から切断します。 コマンドレットから返されるような PSSession オブジェクトを入力し `New-PSSession` ます。 パイプを使用して PSSession オブジェクトをにパイプすることもでき `Disconnect-PSSession` ます。

`Get-PSSession`コマンドレットは、リモートコンピューターで終了するすべての pssession を取得できます。これには、切断された pssession と、他のコンピューター上の他のセッションに接続されている pssession も含まれます。 `Disconnect-PSSession` 現在のセッションに接続されている PSSession のみを切断します。 他の PSSessions をにパイプすると `Disconnect-PSSession` 、 `Disconnect-PSSession` コマンドは失敗します。

```yaml
Type: System.Management.Automation.Runspaces.PSSession[]
Parameter Sets: Session
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -ThrottleLimit

コマンドのスロットル制限を設定し `Disconnect-PSSession` ます。

スロットルの制限は、このコマンドを実行するために確立できるコンカレント接続の最大値です。 このパラメーターを省略した場合、または値 0 を入力した場合は、既定値の 32 が使用されます。

スロットル制限は現在のコマンドのみに適用され、セッションまたはコンピューターには適用されません。

```yaml
Type: System.Int32
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: 32
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutputBufferingMode

出力バッファーがいっぱいになったときに切断されたセッションでコマンド出力を管理する方法を決定します。 既定値は **Block** です。

切断されたセッションのコマンドで出力が返されたが、出力バッファーがいっぱいになっている場合に、コマンドをセッションの切断中に継続して実行するかどうかは、実質的にこのパラメーターの値によって決定されます。 値が **Block** の場合は、セッションが再接続されるまでコマンドが中断されます。 値が **Drop** の場合は、データが失われる可能性はありますが、コマンドを完了できます。 **Drop** 値を使用すると、コマンド出力がディスク上のファイルにリダイレクトされます。

有効な値は次のとおりです。

- **Block**: 出力バッファーがいっぱいになると、バッファーがクリアされるまで実行が中断されます。
- **Drop**: 出力バッファーがいっぱいになると、実行が続行されます。 新しい出力が保存されると、最も古い出力が破棄されます。
- **None**: 出力バッファリングモードが指定されていません。 セッション構成の **OutputBufferingMode** プロパティの値は、切断されたセッションに使用されます。

```yaml
Type: System.Management.Automation.Runspaces.OutputBufferingMode
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Block
Accept pipeline input: False
Accept wildcard characters: False
```

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

### -WhatIf

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](./About/about_CommonParameters.md)」を参照してください。

## 入力

### システム管理. 実行空間

パイプを使用してセッションをにパイプすることができ `Disconnect-PSSession` ます。

## 出力

### システム管理. 実行空間

`Disconnect-PSSession` 切断されたセッションを表すオブジェクトを返します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

- `Disconnect-PSSession`コマンドレットは、ローカルコンピューターとリモートコンピューターで PowerShell 3.0 以降が実行されている場合にのみ機能します。
- 切断された `Disconnect-PSSession` セッションでコマンドレットを使用した場合、このコマンドはセッションに影響を与えず、エラーを生成しません。
- 対話型セキュリティ トークン (**EnableNetworkAccess** パラメーターにより作成される) を使用しているループバック セッションが切断された場合には、そのセッションを作成したコンピューターからのみ再接続できます。 この制約は、悪意のあるアクセスからコンピューターを保護するためのものです。
- PSSession を切断した場合には、セッションの状態が **Disconnected**、可用性が **None** に、それぞれ変更されます。

  **State** プロパティの値は、現在のセッションに関連付けられています。 そのため、**Disconnected** という値は、PSSession が現在のセッションに接続されていないことを意味します。 ただし、PSSession がすべてのセッションから切断されていることを意味するわけではありません。 別のセッションに接続されている可能性があるためです。 セッションに接続または再接続できるかどうかを確認するには、**Availability** プロパティを使用します。

  **Availability** の値が **None** の場合は、セッションに接続できることを示します。 **Busy** は、PSSession が別のセッションに接続されているため、PSSession に接続できないことを示します。

  セッションの **State** プロパティの値の詳細については、「 [RunspaceState Enumeration](/dotnet/api/system.management.automation.runspaces.runspacestate)」を参照してください。

  セッションの **Availability** プロパティの値の詳細については、「 [RunspaceAvailability Enumeration](/dotnet/api/system.management.automation.runspaces.runspaceavailability)」を参照してください。

## 関連リンク

[Connect-PSSession](Connect-PSSession.md)

[Enter-PSSession](Enter-PSSession.md)

[Exit-PSSession](Exit-PSSession.md)

[Get-PSSession](Get-PSSession.md)

[Get-PSSessionConfiguration](Get-PSSessionConfiguration.md)

[New-PSSession](New-PSSession.md)

[New-PSSessionOption](New-PSSessionOption.md)

[New-PSSessionOption](New-PSTransportOption.md)

[Receive-PSSession](Receive-PSSession.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Remove-PSSession](Remove-PSSession.md)

[about_PSSessions](About/about_PSSessions.md)

[about_Remote](About/about_Remote.md)

[about_Remote_Disconnected_Sessions](About/about_Remote_Disconnected_Sessions.md)
