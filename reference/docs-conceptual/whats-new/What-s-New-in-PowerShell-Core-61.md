---
title: PowerShell Core 6.1 の新機能
description: PowerShell Core 6.1 でリリースされた新機能と変更
ms.date: 09/13/2018
ms.openlocfilehash: 4ff70be239197c7a4f64019d2aab42433f82f36c
ms.sourcegitcommit: f9d855dd73b916559a22e337672dea3fbb11c634
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/07/2020
ms.locfileid: "96833822"
---
# <a name="whats-new-in-powershell-core-61"></a>PowerShell Core 6.1 の新機能

以下では、PowerShell Core 6.1 で導入された主要な新機能と変更点からいくつか選んで説明します。

PowerShell をさらに速く安定したものにする **数多くの** "細かな新機能と変更" (それに多数のバグ修正) が他にもあります。 すべての変更点のリストについては、[GitHub の変更ログ](https://github.com/PowerShell/PowerShell/blob/master/CHANGELOG.md)に関するページを確認してください。

以下では何人かの名前を挙げていますが、このリリースを可能にしてくれた[コミュニティのすべての共同作成者](https://github.com/PowerShell/PowerShell/graphs/contributors)に感謝します。

## <a name="net-core-21"></a>.NET Core 2.1

PowerShell Core 6.1 は [5 月のリリース](https://devblogs.microsoft.com/dotnet/announcing-net-core-2-1/)後に .NET Core 2.1 に移行し、次のようないくつかの機能強化が行われました。

- パフォーマンスの向上 ([後述](#performance-improvements)を参照)
- Alpine Linux のサポート (プレビュー)
- [.NET グローバル ツールのサポート](/dotnet/core/tools/global-tools) - PowerShell では近日対応
- [`Span<T>`](/dotnet/api/system.span-1)

## <a name="windows-compatibility-pack-for-net-core"></a>.NET Core 用の Windows 互換機能パック

Windows の .NET チームが [.NET Core 用の Windows 互換機能パック](https://devblogs.microsoft.com/dotnet/announcing-the-windows-compatibility-pack-for-net-core/)を出荷しました。これは、削除された API を Windows の .NET Core に追加して戻すアセンブリのセットです。

この Windows 互換機能パックを PowerShell Core 6.1 リリースに追加し、これらの API を使用するモジュールまたはスクリプトがそれらに依存できるようにしました。

Windows 互換機能パックにより、PowerShell Core では **Windows 10 October 2018 Update および Windows Server 2019 に付属する 1900 を超えるコマンドレット** を使用できます。

## <a name="support-for-application-allow-lists"></a>アプリケーション許可リストのサポート

PowerShell Core 6.1 では、Windows PowerShell 5.1 と同じように、[AppLocker](/windows/security/threat-protection/windows-defender-application-control/applocker/applocker-overview) および [Device Guard](/windows/security/threat-protection/device-guard/introduction-to-device-guard-virtualization-based-security-and-windows-defender-application-control) のアプリケーション許可リストがサポートされています。 アプリケーション許可リストを使用すると、PowerShell の[制約付き言語モード](https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/)で実行できるバイナリを細かく制御できます。

## <a name="performance-improvements"></a>パフォーマンスの向上

PowerShell Core 6.0 では、いくつか大きなパフォーマンス向上が行われます。 PowerShell Core 6.1 では引き続き、特定の操作の速度向上が行われています。

たとえば、`Group-Object` の速度は 66% 向上しました。

```powershell
Measure-Command { 1..100000 | % {Get-Random -Minimum 1 -Maximum 10000} | Group-Object }
```

|    メトリック    | Windows PowerShell 5.1 | PowerShell Core 6.0 | PowerShell Core 6.1 |
| ------------ | ---------------------- | ------------------- | ------------------- |
| 時間 (秒)   | 25.178                 | 19.653              | 6.641               |
| 高速化 (%) | 該当なし                    | 21.9%               | 66.2%               |

同様に、次のような並べ替えのシナリオが 15% 以上向上しています。

```powershell
Measure-Command { 1..100000 | % {Get-Random -Minimum 1 -Maximum 10000} | Sort-Object }
```

|    メトリック    | Windows PowerShell 5.1 | PowerShell Core 6.0 | PowerShell Core 6.1 |
| ------------ | ---------------------- | ------------------- | ------------------- |
| 時間 (秒)   | 12.170                 | 8.493               | 7.08                |
| 高速化 (%) | 該当なし                    | 30.2%               | 16.6%               |

また、`Import-Csv` は Windows PowerShell からの後退の後で大幅に高速化されました。
次の例では、26,616 行 6 列のテスト用 CSV を使用しています。

```powershell
Measure-Command {$a = Import-Csv foo.csv}
```

|    メトリック    | Windows PowerShell 5.1 | PowerShell Core 6.0 |  PowerShell Core 6.1   |
| ------------ | ---------------------- | ------------------- | ---------------------- |
| 時間 (秒)   | 0.441                  | 1.069               | 0.268                  |
| 高速化 (%) | 該当なし                    | -142.4%             | 74.9% (WPS から 39.2%) |

最後に、JSON から `PSObject` への変換は、Windows PowerShell から 50% 以上スピードアップされました。
次の例では、最大 2 MB のテスト用 JSON ファイルを使用します。

```powershell
Measure-Command {Get-Content .\foo.json | ConvertFrom-Json}
```

|    メトリック    | Windows PowerShell 5.1 | PowerShell Core 6.0 |  PowerShell Core 6.1   |
| ------------ | ---------------------- | ------------------- | ---------------------- |
| 時間 (秒)   | 0.259                  | 0.577               | 0.125                  |
| 高速化 (%) | 該当なし                    | -122.8%             | 78.3% (WPS から 51.7%) |

## <a name="check-system32-for-compatible-built-in-modules-on-windows"></a>Windows の互換性のある組み込みモジュールについて、`system32` を確認する

Windows 10 1809 更新プログラムと Windows Server 2019 では、複数の組み込み PowerShell モジュールが更新され、PowerShell Core との互換性があるとしてマークされました。

PowerShell Core 6.1 は起動すると、`PSModulePath` 環境変数の一部として `$windir\System32` を自動的にインクルードします。 ただし、`CompatiblePSEdition` が `Core` と互換性有りとマークされている場合は、`Get-Module` および `Import-Module` に対してのみモジュールを公開します。

```powershell
Get-Module -ListAvailable
```

> [!NOTE]
> インストールされている役割と機能によっては、使用可能として表示されるモジュールが異なる場合があります。

```Output
...
    Directory: C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules

ModuleType Version    Name                                PSEdition ExportedCommands
---------- -------    ----                                --------- ----------------
Manifest   2.0.1.0    Appx                                Core,Desk {Add-AppxPackage, Get-AppxPackage, Get-AppxPacka...
Manifest   1.0.0.0    BitLocker                           Core,Desk {Unlock-BitLocker, Suspend-BitLocker, Resume-Bit...
Manifest   1.0.0.0    DnsClient                           Core,Desk {Resolve-DnsName, Clear-DnsClientCache, Get-DnsC...
Manifest   1.0.0.0    HgsDiagnostics                      Core,Desk {New-HgsTraceTarget, Get-HgsTrace, Get-HgsTraceF...
Binary     2.0.0.0    Hyper-V                             Core,Desk {Add-VMAssignableDevice, Add-VMDvdDrive, Add-VMF...
Binary     1.1        Hyper-V                             Core,Desk {Add-VMDvdDrive, Add-VMFibreChannelHba, Add-VMHa...
Manifest   2.0.0.0    NetAdapter                          Core,Desk {Disable-NetAdapter, Disable-NetAdapterBinding, ...
Manifest   1.0.0.0    NetEventPacketCapture               Core,Desk {New-NetEventSession, Remove-NetEventSession, Ge...
Manifest   2.0.0.0    NetLbfo                             Core,Desk {Add-NetLbfoTeamMember, Add-NetLbfoTeamNic, Get-...
Manifest   1.0.0.0    NetNat                              Core,Desk {Get-NetNat, Get-NetNatExternalAddress, Get-NetN...
Manifest   2.0.0.0    NetQos                              Core,Desk {Get-NetQosPolicy, Set-NetQosPolicy, Remove-NetQ...
Manifest   2.0.0.0    NetSecurity                         Core,Desk {Get-DAPolicyChange, New-NetIPsecAuthProposal, N...
Manifest   1.0.0.0    NetSwitchTeam                       Core,Desk {New-NetSwitchTeam, Remove-NetSwitchTeam, Get-Ne...
Manifest   1.0.0.0    NetWNV                              Core,Desk {Get-NetVirtualizationProviderAddress, Get-NetVi...
Manifest   2.0.0.0    TrustedPlatformModule               Core,Desk {Get-Tpm, Initialize-Tpm, Clear-Tpm, Unblock-Tpm...
...
```

`-SkipEditionCheck` スイッチ パラメーターを使用すると、すべてのモジュールを表示するように、この動作をオーバーライドできます。
また、テーブル出力に対して `PSEdition` プロパティが追加されました。

```powershell
Get-Module Net* -ListAvailable -SkipEditionCheck
```

```Output
    Directory: C:\WINDOWS\system32\WindowsPowerShell\v1.0\Modules

ModuleType Version    Name                        PSEdition ExportedCommands
---------- -------    ----                        --------- ----------------
Manifest   2.0.0.0    NetAdapter                  Core,Desk {Disable-NetAdapter, Disable-NetAdapterBinding, ...
Manifest   1.0.0.0    NetConnection               Core,Desk {Get-NetConnectionProfile, Set-NetConnectionProf...
Manifest   1.0.0.0    NetDiagnostics              Desk      Get-NetView
Manifest   1.0.0.0    NetEventPacketCapture       Core,Desk {New-NetEventSession, Remove-NetEventSession, Ge...
Manifest   2.0.0.0    NetLbfo                     Core,Desk {Add-NetLbfoTeamMember, Add-NetLbfoTeamNic, Get-...
Manifest   1.0.0.0    NetNat                      Core,Desk {Get-NetNat, Get-NetNatExternalAddress, Get-NetN...
Manifest   2.0.0.0    NetQos                      Core,Desk {Get-NetQosPolicy, Set-NetQosPolicy, Remove-NetQ...
Manifest   2.0.0.0    NetSecurity                 Core,Desk {Get-DAPolicyChange, New-NetIPsecAuthProposal, N...
Manifest   1.0.0.0    NetSwitchTeam               Core,Desk {New-NetSwitchTeam, Remove-NetSwitchTeam, Get-Ne...
Manifest   1.0.0.0    NetTCPIP                    Core,Desk {Get-NetIPAddress, Get-NetIPInterface, Get-NetIP...
Manifest   1.0.0.0    NetWNV                      Core,Desk {Get-NetVirtualizationProviderAddress, Get-NetVi...
Manifest   1.0.0.0    NetworkConnectivityStatus   Core,Desk {Get-DAConnectionStatus, Get-NCSIPolicyConfigura...
Manifest   1.0.0.0    NetworkSwitchManager        Core,Desk {Disable-NetworkSwitchEthernetPort, Enable-Netwo...
Manifest   1.0.0.0    NetworkTransition           Core,Desk {Add-NetIPHttpsCertBinding, Disable-NetDnsTransi...
```

この動作について詳しくは、[PowerShell RFC0025](https://github.com/PowerShell/PowerShell-RFC/blob/master/5-Final/RFC0025-PSCore6-and-Windows-Modules.md) をご覧ください。

## <a name="markdown-cmdlets-and-rendering"></a>Markdown のコマンドレットとレンダリング

Markdown は、HTML にレンダリングできる基本的な書式設定で読むことができるプレーンテキスト ドキュメントを作成するための標準です。

6\.1 では、Markdown ドキュメントを変換してコンソールにレンダリングできるコマンドレットがいくつか追加されました。次はその例です。

- `ConvertFrom-Markdown`
- `Get-MarkdownOption`
- `Set-MarkdownOption`
- `Show-Markdown`

たとえば、`Show-Markdown` はマークダウン ファイルをコンソールにレンダリングします。

![Show-Markdown の例](media/What-s-New-in-PowerShell-Core-61/markdown_example.png)

これらのコマンドレットの動作について詳しくは、[こちらの RFC](https://github.com/PowerShell/PowerShell-RFC/blob/master/5-Final/RFC0025-Native-Markdown-Rendering.md) をご覧ください。

## <a name="experimental-feature-flags"></a>試験機能フラグ

[試験的機能][]のサポートが有効になりました。 これにより、PowerShell の開発者は新しい機能を提供し、設計が完了する前にフィードバックを取得できます。 このようにすると、設計の進化に伴って破壊的変更が発生するのを回避できます。

使用できる試験的機能の一覧を取得するには、`Get-ExperimentalFeature` を使います。 これらの機能は、`Enable-ExperimentalFeature` および `Disable-ExperimentalFeature` で有効または無効にできます。

この機能について詳しくは、[PowerShell RFC0029](https://github.com/PowerShell/PowerShell-RFC/blob/master/5-Final/RFC0029-Support-Experimental-Features.md) をご覧ください。

## <a name="web-cmdlet-improvements"></a>Web コマンドレットの機能強化

[@markekraus](https://github.com/markekraus) のおかげで、Web コマンドレット [`Invoke-WebRequest`](/powershell/module/microsoft.powershell.utility/invoke-webrequest)
および [`Invoke-RestMethod`](/powershell/module/microsoft.powershell.utility/invoke-restmethod) に対して多くの機能強化が行われました。

- [PR #6109](https://github.com/PowerShell/PowerShell/pull/6109) - `application-json` 応答に対する既定のエンコードを UTF-8 に設定
- [PR #6018](https://github.com/PowerShell/PowerShell/pull/6018) - 標準に準拠していない `-SkipHeaderValidation` ヘッダーを許可するための `Content-Type` パラメーター
- [PR #5972](https://github.com/PowerShell/PowerShell/pull/5972) - 簡略化された `Form` をサポートするための `multipart/form-data` パラメーター
- [PR #6338](https://github.com/PowerShell/PowerShell/pull/6338) - 準拠、リレーション キーの大文字と小文字を区別しない処理
- [PR #6447](https://github.com/PowerShell/PowerShell/pull/6447) - Web コマンドレット用の `-Resume` パラメーターの追加

## <a name="remoting-improvements"></a>リモート処理の機能強化

### <a name="powershell-direct-for-containers-tries-to-use-powershell-core-first"></a>コンテナー用の PowerShell Direct は最初に PowerShell Core の使用を試みる

PowerShell と Hyper-V の機能である [PowerShell Direct](/virtualization/hyper-v-on-windows/user-guide/powershell-direct) を使用すると、ネットワーク接続または他のリモート管理サービスがなくても、Hyper-V VM またはコンテナーに接続できます。

以前の PowerShell Direct では、コンテナー上の組み込み Windows PowerShell インスタンスを使用して接続していました。 現在の PowerShell Direct は、最初に、`PATH` 環境変数で使用可能な `pwsh.exe` を使用して接続を試みます。 `pwsh.exe` を使用できない場合、PowerShell Direct は `powershell.exe` を使用するようにフォールバックします。

### <a name="enable-psremoting-now-creates-separate-remoting-endpoints-for-preview-versions"></a>`Enable-PSRemoting` はプレビュー バージョン用に別のリモート処理エンドポイントを作成するようになった

`Enable-PSRemoting` では、2 つのリモート処理セッション構成が作成されるようになりました。

- PowerShell のメジャー バージョン用のセッション構成。 たとえば、「 `PowerShell.6` 」のように入力します。 このエンドポイントは、"システム全体" の PowerShell 6 セッション構成として、すべてのマイナー バージョン更新で利用できます。
- 1 つのバージョンに固有のセッション構成。たとえば、`PowerShell.6.1.0` などです。

この動作は、同じコンピューターに PowerShell 6 の複数のバージョンをインストールしてアクセスできるようにしたい場合に便利です。

さらに、`Enable-PSRemoting` コマンドレットを実行した後で、PowerShell のプレビュー バージョンが独自のリモート処理セッション構成を持つようになりました。

```powershell
C:\WINDOWS\system32> Enable-PSRemoting
```

前に WinRM を設定していない場合、出力が異なる場合があります。

```Output
WinRM is already set up to receive requests on this computer.
WinRM is already set up for remote management on this computer.
```

PowerShell 6 のプレビュー ビルドと安定したビルドおよび特定バージョンごとに、異なる PowerShell セッション構成を見ることができます。

```powershell
Get-PSSessionConfiguration
```

```Output
Name          : PowerShell.6.2-preview.1
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : PowerShell.6-preview
PSVersion     : 6.2
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : powershell.6
PSVersion     : 6.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed

Name          : powershell.6.1.0
PSVersion     : 6.1
StartupScript :
RunAsUser     :
Permission    : NT AUTHORITY\INTERACTIVE AccessAllowed, BUILTIN\Administrators AccessAllowed, BUILTIN\Remote Management Users AccessAllowed
```

### <a name="userhostport-syntax-supported-for-ssh"></a>SSH に対してサポートされる `user@host:port` 構文

SSH クライアントは、通常、`user@host:port` の形式で接続文字列をサポートします。 PowerShell リモート処理用のプロトコルとして SSH が追加されたことに伴い、この形式の接続文字列のサポートが追加されました。

`Enter-PSSession -HostName fooUser@ssh.contoso.com:2222`

## <a name="msi-option-to-add-explorer-shell-context-menu-on-windows"></a>Windows でエクスプローラー シェルのコンテキスト メニューを追加するための MSI オプション

[@bergmeister](https://github.com/bergmeister) の尽力により、Windows でコンテキスト メニューを有効にできるようになりました。 エクスプローラーで任意のフォルダーから PowerShell 6.1 のシステム全体のインストールを開くことができます。

![PowerShell 6 のシェル コンテキスト メニュー](media/What-s-New-in-PowerShell-Core-61/shell_context_menu.png)

## <a name="goodies"></a>その他

### <a name="run-as-administrator-in-the-windows-shortcut-jump-list"></a>Windows のショートカット ジャンプ リストでの [管理者として実行]

[@bergmeister](https://github.com/bergmeister) のおかげで、PowerShell Core のショートカットのジャンプ リストに [管理者として実行] が含まれるようになりました。

![PowerShell 6 のジャンプ リストにおける [管理者として実行]](media/What-s-New-in-PowerShell-Core-61/jumplist.png)

### <a name="cd---returns-to-previous-directory"></a>`cd -` は以前のディレクトリを返す

```powershell
C:\Windows\System32> cd C:\
C:\> cd -
C:\Windows\System32>
```

または Linux 上:

```ShellSession
PS /etc> cd /usr/bin
PS /usr/bin> cd -
PS /etc>
```

また、`cd` と `cd --` は `$HOME` に変更します。

### `Test-Connection`

[@iSazonov](https://github.com/iSazonov) のおかげで、[`Test-Connection`](/powershell/module/microsoft.powershell.management/test-connection) コマンドレットが PowerShell Core に移植されました。

### <a name="update-help-as-non-admin"></a>非管理者としての `Update-Help`

要望が多かったので、`Update-Help` は管理者でなくても実行できるようになりました。 `Update-Help` は既定でユーザー スコープのフォルダーにヘルプを保存します。

### <a name="new-methodsproperties-on-pscustomobject"></a>`PSCustomObject` での新しいメソッド/プロパティ

[@iSazonov](https://github.com/iSazonov) のおかげで、`PSCustomObject` に新しいメソッドとプロパティが追加されました。 `PSCustomObject` には、他のオブジェクトと同じように `Count`/`Length` プロパティが含まれるようになっています。

```powershell
$PSCustomObject = [pscustomobject]@{foo = 1}

$PSCustomObject.Length
```

```Output
1
```

```powershell
$PSCustomObject.Count
```

```Output
1
```

この作業には、`PSCustomObject` 項目を操作およびフィルター処理できる `ForEach` と `Where` メソッドも含まれます。

```powershell
$PSCustomObject.ForEach({$_.foo + 1})
```

```Output
2
```

```powershell
$PSCustomObject.Where({$_.foo -gt 0})
```

```Output
foo
---
  1
```

### `Where-Object -Not`

@SimonWahlin のおかげで、`-Not` パラメーターが `Where-Object` に追加されました。 パイプラインのオブジェクトを、存在しないプロパティまたは null/空のプロパティ値でフィルター処理できます。

たとえば、次のコマンドでは、依存サービスが定義されていないすべてのサービスが返されます。

```powershell
Get-Service | Where-Object -Not DependentServices
```

### <a name="new-modulemanifest-creates-a-bom-less-utf-8-document"></a>`New-ModuleManifest` では BOM のない UTF-8 ドキュメントが作成される

PowerShell 6.0 が BOM のない UTF-8 に移行したことに伴い、`New-ModuleManifest` コマンドレットが、UTF-16 のドキュメントではなく BOM なしの UTF-8 ドキュメントを作成するように更新されました。

### <a name="conversions-from-psmethod-to-delegate"></a>PSMethod からデリゲートへの変換

[@powercode](https://github.com/powercode) のおかげで、`PSMethod` のデリゲートへの変換がサポートされるようになりました。 これにより、`PSMethod` `[M]::DoubleStrLen` をデリゲート値として `[M]::AggregateString` に渡すといった操作ができるようになります。

```powershell
class M {
    static [int] DoubleStrLen([string] $value) { return 2 * $value.Length }

    static [long] AggregateString([string[]] $values, [func[string, int]] $selector) {
        [long] $res = 0
        foreach($s in $values){
            $res += $selector.Invoke($s)
        }
        return $res
    }
}

[M]::AggregateString((gci).Name, [M]::DoubleStrLen)
```

この変更について詳しくは、[PR #5287](https://github.com/PowerShell/PowerShell/pull/5287) をご覧ください。

### <a name="standard-deviation-in-measure-object"></a>`Measure-Object` での標準偏差

[@CloudyDino](https://github.com/CloudyDino) のおかげで、`StandardDeviation` プロパティが `Measure-Object` に追加されました。

```powershell
Get-Process | Measure-Object -Property CPU -AllStats
```

```Output
Count             : 308
Average           : 31.3720576298701
Sum               : 9662.59375
Maximum           : 4416.046875
Minimum           :
StandardDeviation : 264.389544720926
Property          : CPU
```

### `GetPfxCertificate -Password`

[@maybe-hello-world](https://github.com/maybe-hello-world) のおかげで、`Get-PfxCertificate` に `Password` パラメーターが追加されました。このパラメーターは `SecureString` を受け取ります。 これにより、非対話的に使用できるようになります。

```powershell
$certFile = '\\server\share\pwd-protected.pfx'
$certPass = Read-Host -AsSecureString -Prompt 'Enter the password for certificate: '

$certThumbPrint = (Get-PfxCertificate -FilePath $certFile -Password $certPass ).ThumbPrint
```

### <a name="removal-of-the-more-function"></a>`more` 関数の削除

以前の PowerShell には、`more.com` をラップする `more` という名前の Windows 上の関数がありました。 この関数は削除されました。

また、`help` 関数が変更され、Windows では `more.com` を、Windows 以外のプラットフォームでは `$env:PAGER` で指定されたシステムの既定のページャーを使用するようになりました。

### <a name="cd-drivename-now-returns-users-to-the-current-working-directory-in-that-drive"></a>`cd DriveName:` では、そのドライブの現在の作業ディレクトリに戻るようになりました

以前は、`Set-Location` または `cd` を使用して PSDrive に戻ると、ユーザーはそのドライブの既定の場所に送られました。

[@mcbobke](https://github.com/mcbobke) のおかげで、ユーザーはそのセッションで最後に認識された現在の作業ディレクトリに送られるようになりました。

### <a name="windows-powershell-type-accelerators"></a>Windows PowerShell の型アクセラレータ

Windows PowerShell では、以下の型アクセラレータが追加されて、それぞれの型を使いやすくなりました。

- `[adsi]`: `System.DirectoryServices.DirectoryEntry`
- `[adsisearcher]`: `System.DirectoryServices.DirectorySearcher`
- `[wmi]`: `System.Management.ManagementObject`
- `[wmiclass]`: `System.Management.ManagementClass`
- `[wmisearcher]`: `System.Management.ManagementObjectSearcher`

これらの型アクセラレータは、PowerShell 6 には含まれませんでしたが、Windows で実行される PowerShell 6.1 には追加されています。

これらの型は、AD および WMI オブジェクトを簡単に構築するのに役立ちます。

たとえば、LDAP を使用してクエリを実行できます。

```powershell
[adsi]'LDAP://CN=FooUse,OU=People,DC=contoso,DC=com'
```

次の例では、Win32_OperatingSystem CIM オブジェクトを作成します。

```powershell
[wmi]"Win32_OperatingSystem=@"
```

```Output
SystemDirectory : C:\WINDOWS\system32
Organization    : Contoso IT
BuildNumber     : 18234
RegisteredUser  : Contoso Corp.
SerialNumber    : 12345-67890-ABCDE-F0123
Version         : 10.0.18234
```

この例では、Win32_OperatingSystem クラスの ManagementClass オブジェクトが返されます。

```powershell
[wmiclass]"Win32_OperatingSystem"
```

```Output
   NameSpace: ROOT\cimv2

Name                                Methods              Properties
----                                -------              ----------
Win32_OperatingSystem               {Reboot, Shutdown... {BootDevice, BuildNumber, BuildType, Caption...}
```

### <a name="-lp-alias-for-all--literalpath-parameters"></a>すべての `-LiteralPath` パラメーターに対する `-lp` エイリアス

[@kvprasoon](https://github.com/kvprasoon) のおかげで、`-lp` パラメーターがあるすべての組み込み PowerShell コマンドレットに対し、パラメーター エイリアス `-LiteralPath` が追加されました。

## <a name="breaking-changes"></a>重大な変更

### <a name="msi-based-installation-paths-on-windows"></a>Windows での MSI ベースのインストール パス

Windows では、MSI パッケージは次のパスにインストールされるようになりました。

- `$env:ProgramFiles\PowerShell\6\`: 6.x の安定したインストールの場合
- `$env:ProgramFiles\PowerShell\6-preview\`: 6.x のプレビュー インストールの場合

この変更により、Microsoft Update で PowerShell Core を更新/保守できるようになります。

詳しくは、[PowerShell RFC0026](https://github.com/PowerShell/PowerShell-RFC/blob/master/5-Final/RFC0026-MSI-Installation-Path.md) をご覧ください。

### <a name="telemetry-can-only-be-disabled-with-an-environment-variable"></a>テレメトリは環境変数でのみ無効にできる

PowerShell Core は、起動されると基本的なテレメトリ データを Microsoft に送信します。 データには、OS 名、OS バージョン、および PowerShell のバージョンが含まれます。 このデータにより、PowerShell が使用されている環境をより深く理解し、新機能と修正の優先順位を付けることができます。

このテレメトリを無効にするには、環境変数 `POWERSHELL_TELEMETRY_OPTOUT` を `true`、`yes`、または `1` に設定します。 ファイル `DELETE_ME_TO_DISABLE_CONSOLEHOST_TELEMETRY` を削除することによるテレメトリの無効化はサポートされなくなっています。

### <a name="disallowed-basic-auth-over-http-in-powershell-remoting-on-unix-platforms"></a>Unix プラットフォーム上の PowerShell リモート処理での HTTP による基本認証が許可されなくなった

暗号化されていないトラフィックの使用を防ぐため、Unix プラットフォーム上の PowerShell リモート処理では、NTLM/ネゴシエートまたは HTTPS を使用することが必要になりました。

これらの変更について詳しくは、[Issue #6779](https://github.com/PowerShell/PowerShell/issues/6779) をご覧ください。

### <a name="removed-visualbasic-as-a-supported-language-in-add-type"></a>Add-Type でサポートされる言語としての `VisualBasic` の削除

これまでは、`Add-Type` コマンドレットを使用して Visual Basic コードをコンパイルできました。 Visual Basic は `Add-Type` ではほとんど使用されませんでした。 この機能を削除して、PowerShell のサイズを小さくしました。

### <a name="cleaned-up-uses-of-commandtypesworkflow-and-workflowinfocleaned"></a>`CommandTypes.Workflow` および `WorkflowInfoCleaned` の使用のクリーンアップ

これらの変更について詳しくは、[PR #6708](https://github.com/PowerShell/PowerShell/pull/6708) をご覧ください。

### <a name="group-object-now-sorts-the-groups"></a>Group-Object でのグループの並べ替え

パフォーマンス向上の一環として、`Group-Object` でグループの並び替えられた一覧が返されるようになりました。
順序に依存すべきではありませんが、最初のグループを必要としていた場合、この変更が破壊的になる可能性があります。 以前の動作に依存することの影響は低いため、このパフォーマンス向上には変更するだけの価値があると判断されました。

この変更の詳細については、[問題 #7409](https://github.com/PowerShell/PowerShell/issues/7409) をご覧ください。

<!-- URL references -->
[試験的機能]: /powershell/module/Microsoft.PowerShell.Core/About/about_Experimental_Features
