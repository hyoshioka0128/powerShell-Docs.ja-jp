---
ms.date: 07/10/2019
keywords: JEA, PowerShell, セキュリティ
title: JEA の監査とレポート
description: 監査は、JEA エンドポイントに適切なユーザーがアクセスしていること、ロールの割り当てが適切であることを評価するのに役立ちます。
ms.openlocfilehash: 2140d6b756ae38d82e4943c373e8a75beea30e28
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92500013"
---
# <a name="auditing-and-reporting-on-jea"></a>JEA の監査とレポート

JEA を展開したら、JEA 構成を定期的に監査する必要があります。 監査は、JEA エンドポイントに適切なユーザーがアクセスしていること、ロールの割り当てが適切であることを評価するのに役立ちます。

## <a name="find-registered-jea-sessions-on-a-machine"></a>コンピューターに登録されている JEA セッションを見つける

コンピューターに登録されている JEA セッションを確認するには、[Get-PSSessionConfiguration](/powershell/module/microsoft.powershell.core/get-pssessionconfiguration) コマンドレットを使用します。

```powershell
# Filter for sessions that are configured as 'RestrictedRemoteServer' to find JEA-like session configurations
Get-PSSessionConfiguration | Where-Object { $_.SessionType -eq 'RestrictedRemoteServer' }
```

```Output
Name          : JEAMaintenance
PSVersion     : 5.1
StartupScript :
RunAsUser     :
Permission    : CONTOSO\JEA_DNS_ADMINS AccessAllowed, CONTOSO\JEA_DNS_OPERATORS AccessAllowed,
                CONTOSO\JEA_DNS_AUDITORS AccessAllowed
```

エンドポイントに対して有効な権限は **Permission** プロパティで確認できます。 これらのユーザーには、JEA エンドポイントに接続する権限が与えられています。 ただし、アクセスできるロールとコマンドは、エンドポイントの登録に使用された [セッション構成ファイル](session-configurations.md)の **RoleDefinitions** プロパティによって決まります。 **RoleDefinitions** プロパティを展開して、登録されている JEA エンドポイントのロール マッピングを評価します。

```powershell
# Get the desired session configuration
$jea = Get-PSSessionConfiguration -Name 'JEAMaintenance'

# Enumerate users/groups and which roles they have access to
$jea.RoleDefinitions.GetEnumerator() | Select-Object Name, @{
  Name = 'Role Capabilities'
  Expression = { $_.Value.RoleCapabilities }
}
```

## <a name="find-available-role-capabilities-on-the-machine"></a>コンピューターで利用できるロール機能を見つける

JEA では、PowerShell モジュール内の **RoleCapabilities** フォルダーに格納されている `.psrc` ファイルからロール機能を取得します。 次の関数は、コンピューターで使用可能なすべてのロール機能を検索します。

```powershell
function Find-LocalRoleCapability {
    $results = @()

    # Find modules with a "RoleCapabilities" subfolder and add any PSRC files to the result set
    Get-Module -ListAvailable | ForEach-Object {
        $psrcpath = Join-Path -Path $_.ModuleBase -ChildPath 'RoleCapabilities'
        if (Test-Path $psrcpath) {
            $results += Get-ChildItem -Path $psrcpath -Filter *.psrc
        }
    }

    # Format the results nicely to make it easier to read
    $results | Select-Object @{ Name = 'Name'; Expression = { $_.Name.TrimEnd('.psrc') }}, @{
        Name = 'Path'; Expression = { $_.FullName }
    } | Sort-Object Name
}
```

> [!NOTE]
> この関数の結果の順序は、必ずしも、複数のロール機能で同じ名前が共有されているときのロール機能の選択順に一致しません。

## <a name="check-effective-rights-for-a-specific-user"></a>特定のユーザーに対して有効な権限を確認する

[Get-PSSessionCapability](/powershell/module/microsoft.powershell.core/Get-PSSessionCapability) コマンドレットは、ユーザーのグループ メンバーシップに基づいて、JEA エンドポイントで使用可能なすべてのコマンドを列挙します。
`Get-PSSessionCapability` の出力は、JEA セッションで `Get-Command -CommandType All` を実行している指定ユーザーのそれと同じになります。

```powershell
Get-PSSessionCapability -ConfigurationName 'JEAMaintenance' -Username 'CONTOSO\Alice'
```

ユーザーが、追加 JEA 権限が与えられるグループの永続的なメンバーではないとき、このコマンドレットでこれらの追加のアクセス許可が反映されない場合があります。 これは Just-In-Time の特権アクセス (必要なときに必要な許可を与える) 管理システムを利用し、セキュリティ グループに一時的に属することをユーザーに許可する場合に発生します。 ユーザーがジョブを正常に実行するために必要なアクセスのレベルだけを得られるように、ユーザーのロールと機能へのマッピングを注意深く評価します。

## <a name="powershell-event-logs"></a>PowerShell イベント ログ

システムでモジュールやスクリプト ブロックのログ記録を有効にしている場合、ユーザーが JEA セッションで実行したコマンドごとに、Windows イベント ログでイベントを確認できます。 これらのイベントを検索するには、**Microsoft-Windows-PowerShell/Operational** イベント ログを開き、イベント ID が **4104** のイベントを探します。

各イベント ログ エントリに、コマンドを実行したセッションに関する情報が含まれます。 JEA セッションの場合、イベントには **ConnectedUser** と **RunAsUser** に関する情報が含まれます。 **ConnectedUser** は JEA セッションを作成した実際のユーザーです。 **RunAsUser** は JEA でコマンドを実行するために使用されたアカウントです。

アプリケーション イベント ログには、**RunAsUser** により行われた変更が表示されます。 そのため、特定のコマンドの呼び出しを **ConnectedUser** にさかのぼって追跡するには、モジュールおよびスクリプトのログ記録を有効にしておく必要があります。

## <a name="application-event-logs"></a>アプリケーション イベント ログ

外部のアプリケーションやサービスと情報を交換する JEA セッションでコマンドを実行すると、独自のイベント ログにイベントが記録されることがあります。 PowerShell ログおよびトランスクリプトと異なり、他のログ記録メカニズムでは JEA セッションの接続されているユーザーは取得されません。 代わりに、これらのアプリケーションでは、仮想実行ユーザーのみが記録されます。
コマンドの実行ユーザーを判断するには、[セッション トランスクリプト](#session-transcripts)を確認するか、PowerShell イベント ログと、アプリケーション イベント ログに表示されている時刻とユーザーを関連付ける必要があります。

WinRM ログは、アプリケーション イベント ログの実行ユーザーと接続ユーザーを関連付けることにも役立ちます。 **Microsoft-Windows-Windows Remote Management/Operational** ログのイベント ID **193** では、すべての新しい JEA セッションの接続ユーザーと実行ユーザーの両方に対して、セキュリティ識別子 (SID) とアカウント名が記録されます。

## <a name="session-transcripts"></a>セッションのトランスクリプト

ユーザー セッションごとにトランスクリプトを作成するように JEA を構成した場合、すべてのユーザーのアクションのテキスト コピーが指定のフォルダーに保存されます。

次のコマンドでは (管理者として実行した場合)、すべてのトランスクリプト ディレクトリを検索します。

```powershell
Get-PSSessionConfiguration |
  Where-Object { $_.TranscriptDirectory -ne $null } |
    Format-Table Name, TranscriptDirectory
```

各トランスクリプトは、セッションの開始時刻、セッションに接続したユーザー、そのユーザーに割り当てられた JEA ID に関する情報で始まります。

```
**********************
Windows PowerShell transcript start
Start time: 20160710144736
Username: CONTOSO\Alice
RunAs User: WinRM Virtual Users\WinRM VA_1_CONTOSO_Alice
Machine: SERVER01 (Microsoft Windows NT 10.0.14393.0)
[...]
```

トランスクリプトの本文には、ユーザーが呼び出した各コマンドに関する情報が含まれています。 使用されたコマンドの正確な構文は、PowerShell リモート処理のためにコマンドが変換される方法のため、JEA セッションでは使用できません。 ただし、実行された有効なコマンドを判断することはできます。 次は、JEA セッションでユーザーが `Get-Service Dns` を実行した場合のトランスクリプト例からの抜粋です。

```
PS>CommandInvocation(Get-Service): "Get-Service"
>> ParameterBinding(Get-Service): name="Name"; value="Dns"
>> CommandInvocation(Out-Default): "Out-Default"
>> ParameterBinding(Out-Default): name="InputObject"; value="Dns"

Running  Dns                DNS Server
```

ユーザーが実行するコマンドごとに、**CommandInvocation** 行が書き込まれます。 **ParameterBindings** はコマンドと共に指定された各パラメーターとその値を記録します。 前の例では、`Get-Service` コマンドレットでパラメーター **Name** に値 **Dns** が指定されていることを確認できます。

各コマンドの出力も **CommandInvocation** をトリガーします (通常は `Out-Default` に)。 `Out-Default` の **InputObject** は、コマンドから返される PowerShell オブジェクトです。 そのオブジェクトの詳細が数行下に出力されています。ユーザーに表示される内容と同様のものが表示されます。

## <a name="see-also"></a>関連項目

[*PowerShell ♥ the Blue Team* のセキュリティに関するブログ投稿](https://devblogs.microsoft.com/powershell/powershell-the-blue-team/)
