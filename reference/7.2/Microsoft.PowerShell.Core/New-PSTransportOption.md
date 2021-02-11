---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/new-pstransportoption?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: New-PSSessionOption
ms.openlocfilehash: fa19ee7d1856eee91a6b1d6a8352017457031202
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600585"
---
# New-PSSessionOption

## 概要
セッション構成の詳細設定オプションが格納されたオブジェクトを作成します。

## SYNTAX

```
New-PSTransportOption [-MaxIdleTimeoutSec <Int32>] [-ProcessIdleTimeoutSec <Int32>] [-MaxSessions <Int32>]
 [-MaxConcurrentCommandsPerSession <Int32>] [-MaxSessionsPerUser <Int32>] [-MaxMemoryPerSessionMB <Int32>]
 [-MaxProcessesPerSession <Int32>] [-MaxConcurrentUsers <Int32>] [-IdleTimeoutSec <Int32>]
 [-OutputBufferingMode <OutputBufferingMode>] [<CommonParameters>]
```

## Description

**New-PSTransportOption** コマンドレットは、セッション構成のトランスポート オプションを含むオブジェクトを作成します。
オブジェクトは、Register-PSSessionConfiguration や Set-PSSessionConfiguration のコマンドレットなど、セッション構成を作成または変更するコマンドレットの *TransportOption* パラメーターの値として使用できます。

また、WSMan: ドライブのセッション構成のプロパティの値を編集することにより、トランスポート オプション設定を変更することもできます。
詳細については、「WSMan Provider」を参照してください。

セッション構成オプションは、サーバー側またはリモート接続の受信側で設定されたセッション値を表します。
クライアント側または接続の送信側は、セッションが作成されたとき、またはクライアントがセッションから切断または再接続したときに、セッションオプションの値を設定できます。
特に明記しない限り、設定値が競合した場合、クライアント側の値が優先されます。
ただし、クライアント側の値は、セッション構成に設定された最大値とクォータを超えることはできません。

パラメーターを指定しない場合、 **new-pstransportoption** は、すべてのオプションに対して null 値を持つトランスポートオプションオブジェクトを生成します。
パラメーターを省略した場合、オブジェクトではパラメーターが表すプロパティに対して null 値が保持されます。
Null 値は、セッション構成には影響しません。

セッションオプションの詳細については、「New-PSSessionOption」を参照してください。
セッション構成の詳細については、「 [about_Session_Configurations](About/about_Session_Configurations.md)」を参照してください。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: 既定のトランスポートオプションを生成する

```powershell
New-PSTransportOption
```

```Output
ProcessIdleTimeoutSec           :
MaxIdleTimeoutSec               :
MaxSessions                     :
MaxConcurrentCommandsPerSession :
MaxSessionsPerUser              :
MaxMemoryPerSessionMB           :
MaxProcessesPerSession          :
MaxConcurrentUsers              :
IdleTimeoutSec                  :
OutputBufferingMode             :
```

このコマンドは、パラメーターを指定せずに **new-pstransportoption** を実行します。
出力には、コマンドレットがすべてのプロパティに対して null 値を持つトランスポートオプションオブジェクトを生成することが示されています。

### 例 2: セッション構成オプションを取得する

この例では、トランスポートオプションオブジェクトを使用してセッション構成オプションを設定する方法を示します。

```powershell
$t = New-PSTransportOption -MaxSessions 40
Register-PSSessionConfiguration -Name ITTasks -TransportOption $t
Get-PSSessionConfiguration -Name ITTasks | Format-List -Property *
```

```Output
Architecture                  : 64
Filename                      : %windir%\system32\pwrshplugin.dll
ResourceUri                   : http://schemas.microsoft.com/powershell/ITTasks
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
MaxShells                     : 40
MaxMemoryPerShellMB           : 1024
MaxIdleTimeoutms              : 43200000
SDKVersion                    : 2
Name                          : ITTasks
XmlRenderingType              : text
Capability                    : {Shell}
RunAsPassword                 :
MaxProcessesPerShell          : 15
Enabled                       : True
MaxShellsPerUser              : 25
Permission                    :
```

最初のコマンドは、**New-PSTransportOption** コマンドレットを使用して、$t 変数に保存されるトランスポート オプション オブジェクトを作成します。 このコマンドは、*MaxSessions* パラメーターを使用して、セッションの最大数を 40 に増やします。

2 番目のコマンドは、**Register-PSSessionConfiguration** コマンドレットを使用して、ITTasks セッション構成を作成します。 このコマンドは、*TransportOption* パラメーターを使用して、$t 変数内のトランスポート オプション オブジェクトを指定します。

3番目のコマンドは、Get-PSSessionConfiguration コマンドレットを使用して ITTasks セッション構成を取得し、Format-List コマンドレットを使用して、セッション構成オブジェクトのすべてのプロパティを一覧に表示します。 出力には、セッション構成の **MaxShells** プロパティが 40 であることが示されます。

### 例 3: トランスポートオプションを設定する

このコマンドは、セッション構成内のトランスポート オプションを設定することで、セッション構成を使用するセッションに与える影響を示します。

```powershell
$t = New-PSTransportOption -IdleTimeoutSec 3600
Set-PSSessionConfiguration -Name ITTasks -TransportOption $t
$s = New-PSSession -Name MyITTasks -ConfigurationName ITTasks
$s | Format-List -Property *
```

```Output
State                  : Opened
IdleTimeout            : 3600000
OutputBufferingMode    : Block
ComputerName           : localhost
ConfigurationName      : ITTasks
InstanceId             : 4110c3f5-68ea-40fa-9bbf-04a433dbb02d
Id                     : 1
Name                   : MyITTasks
Availability           : Available
ApplicationPrivateData : {PSVersionTable}
Runspace               : System.Management.Automation.RemoteRunspace
```

最初のコマンドは、**New-PSTransportOption** コマンドレットを使用して、トランスポート オプション オブジェクトを作成します。 このコマンドは、*IdleTimeoutSec* パラメーターを使用して、オブジェクトの **IdleTimeoutSec** プロパティ値を 1 時間 (3,600 秒) に設定します。 このコマンドは、トランスポート オブジェクトを $t 変数に保存します。

2番目のコマンドは、Set-PSSessionConfiguration コマンドレットを使用して、ITTasks セッション構成のトランスポートオプションを変更します。 このコマンドは、*TransportOption* パラメーターを使用して、$t 変数内のトランスポート オプション オブジェクトを指定します。

3番目のコマンドは、New-PSSession コマンドレットを使用して、ローカルコンピューター上に MyITTasks セッションを作成します。 このコマンドは、**ConfigurationName** プロパティを使用して、ITTasks セッション構成を指定します。 このコマンドは、セッションを $s 変数に保存します。このコマンドでは、**新しい-PSSession** の *sessionoption* パラメーターを使用して、セッションのカスタムアイドルタイムアウトを設定していないことに注意してください。 失敗した場合、セッションオプションで設定されているアイドルタイムアウト値は、セッション構成で設定されているアイドルタイムアウトよりも優先されます。

4番目のコマンドは、Format-List コマンドレットを使用して、セッションのすべてのプロパティを $s 変数に一覧表示します。 出力には、セッションのアイドルタイムアウトが1時間 (36万ミリ秒) であることが示されています。

## PARAMETERS

### -IdleTimeoutSec

リモートコンピューターがローカルコンピューターからの通信を受信していない場合に、各セッションが開いたままになる期間を指定します。
これにはハートビート信号も含まれます。
この期間が経過すると、セッションは閉じられます。

アイドルタイムアウト値は、ユーザーがセッションを切断して再接続する場合に非常に重要です。
ユーザーは、セッションがまだタイムアウトしていない場合のみ再接続できます。

*IdleTimeoutSec* パラメーターは、セッション構成の **IdleTimeoutMs** プロパティに対応します。

値を秒単位で入力します。
既定値は 7,200 (2 時間) です。
最小値は 60 (1 分) です。
最大値は、WSMan 構成 () 内のシェルオブジェクトの **IdleTimeout** プロパティの値です `WSMan:\\\<ComputerName\>\Shell\IdleTimeout` 。
既定値は 7,200,000 ミリ秒 (2 時間) です。

アイドルタイムアウト値がセッションオプションとセッション構成で設定されている場合、セッションオプションで設定された値が優先されますが、セッション構成の **MaxIdleTimeoutMs** プロパティの値を超えることはできません。
**MaxIdleTimeoutMs** プロパティの値を設定するには、*MaxIdleTimeoutSec* パラメーターを使用します。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxConcurrentCommandsPerSession

各セッションで同時に実行できるコマンドの数を指定された値に制限します。
既定値は 1000 です。

*MaxConcurrentCommandsPerSession* パラメーターは、セッション構成の **MaxConcurrentCommandsPerShell** プロパティに対応しています。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxConcurrentUsers

各セッションで同時にコマンドを実行できるユーザーの数を指定された値に制限します。
既定値は 5 です。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxIdleTimeoutSec

各セッションのアイドルタイムアウトセットを、指定された値に制限します。
既定値は \[ Int \] :: MaxValue (~ 25 日) です。

アイドルタイムアウト値は、ユーザーがセッションを切断して再接続する場合に非常に重要です。
ユーザーは、セッションがまだタイムアウトしていない場合のみ再接続できます。

*MaxIdleTimeoutSec* パラメーターは、セッション構成の **MaxIdleTimeoutMs** プロパティに対応しています。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxMemoryPerSessionMB

各セッションで使用されるメモリを、指定された値に制限します。
値はメガバイトで入力します。
既定値は 1,024 メガバイト (1 GB) です。

*Maxmemorypersessionmb* パラメーターは、セッション構成の **Maxmemorypersessionmb** プロパティに対応しています。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxProcessesPerSession

各セッションで実行するプロセスの数を、指定された値に制限します。
既定値は 15 です。

*MaxProcessesPerSession* パラメーターは、セッション構成の **Maxproces pershell** プロパティに対応しています。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxSessions

セッション構成を使用するセッションの数を制限します。
既定値は 25 です。

*Maxsessions* パラメーターは、セッション構成の **maxsessions** プロパティに対応しています。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MaxSessionsPerUser

セッション構成を使用して特定のユーザーの資格情報で実行するセッションの数を、指定された値に制限します。
既定値は 25 です。

この値を指定する場合は、多くのユーザーが実行ユーザーの資格情報を使用している可能性があります。

*Maxsessionsperuser* パラメーターは、セッション構成の **MaxShellsPerUser** プロパティに対応しています。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -OutputBufferingMode

出力バッファーがいっぱいになったときに切断されたセッションでコマンドの出力を管理する方法を指定します。
このパラメーターの有効値は、次のとおりです。

- ブロック。
出力バッファーがいっぱいのとき、バッファーが空くまで実行が中断されます。
- Drop : 
出力バッファーがいっぱいのとき、実行は続行されます。
新しい出力が保存されると、最も古い出力が破棄されます。
- [なし] :
出力バッファー モードが指定されません。

セッションの **OutputBufferingMode** プロパティの既定値は Block です。

```yaml
Type: System.Nullable`1[System.Management.Automation.Runspaces.OutputBufferingMode]
Parameter Sets: (All)
Aliases:
Accepted values: None, Drop, Block

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ProcessIdleTimeoutSec

各ホストプロセスのタイムアウトを、指定された値に制限します。
既定値の0は、プロセスのタイムアウト値がないことを意味します。

他のセッション構成には、プロセスごとのタイムアウト値があります。
たとえば、28800秒 (8 時間) のプロセスごとのタイムアウト値が設定さ **れている** とします。

```yaml
Type: System.Nullable`1[System.Int32]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### Microsoft. PowerShell. コマンド。 WSManConfigurationOption

## 注

- セッション構成オブジェクトのプロパティは、セッション構成に設定されているオプションとその値によって異なります。 また、セッション構成ファイルを使用するセッション構成には追加のプロパティがあります。

## 関連リンク

[New-PSSession](New-PSSession.md)

[New-PSSessionOption](New-PSSessionOption.md)

[Register-PSSessionConfiguration](Register-PSSessionConfiguration.md)

[Set-PSSessionConfiguration](Set-PSSessionConfiguration.md)

