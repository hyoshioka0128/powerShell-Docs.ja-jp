---
ms.date: 06/12/2017
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC on Nano Server の使用
description: DSC は、Windows Nano Server 用の VHD を作成するときにインストールできる、オプションのパッケージです。
ms.openlocfilehash: 18585323359abd85515d4db194dae4adbad7c3d8
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92647079"
---
# <a name="using-dsc-on-nano-server"></a>DSC on Nano Server の使用

> 適用先:Windows PowerShell 5.0

**DSC on Nano Server** は、Windows Server 2016 メディアの `NanoServer\Packages` フォルダーに含まれるオプションのパッケージです。 このパッケージをインストールするには、Nano Server の VHD を作成するときに、 **New-NanoServerImage** 関数の **Packages** パラメーターの値として **Microsoft-NanoServer-DSC-Package** を指定します。 たとえば、仮想マシンの VHD を作成する場合、コマンドは次のようになります。

```powershell
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1\Nano.vhd -ComputerName Nano1 -Packages Microsoft-NanoServer-DSC-Package
```

Nano Server のインストールと使用、および PowerShell リモート処理による Nano Server の管理方法については、「[Getting Started with Nano Server (Nano Server の概要)](/windows-server/get-started/getting-started-with-nano-server)」を参照してください。

## <a name="dsc-features-available-on-nano-server"></a>Nano Server で使用できる DSC 機能

Nano Server でサポートされる API のセットは、通常版の Windows Server と比べると限定的であるため、当面の間、DSC on Nano Server では、すべての SKU で DSC が動作する、完全に機能するパリティを利用できません。 DSC on Nano Server は現在開発中であり、まだ完全な機能ではありません。

現在、Nano Server で使用できる DSC 機能は次のとおりです。

プッシュ モードとプル モード

- 以下を含む、通常版の Windows Server に存在するすべての DSC コマンドレット
- [Get-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Get-DscLocalConfigurationManager)
- [Set-DscLocalConfigurationManager](/powershell/module/PSDesiredStateConfiguration/Set-DscLocalConfigurationManager)
- [Enable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Enable-DscDebug)
- [Disable-DscDebug](/powershell/module/PSDesiredStateConfiguration/Disable-DscDebug)
- [Start-DscConfiguration](/powershell/module/psdesiredstateconfiguration/start-dscconfiguration)
- [Stop-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Stop-DscConfiguration)
- [Get-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Get-DscConfiguration)
- [Test-DscConfiguration](/powershell/module/psdesiredstateconfiguration/Test-DSCConfiguration)
- [Publish-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Publish-DscConfiguration)
- [Update-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Update-DscConfiguration)
- [Restore-DscConfiguration](/powershell/module/PSDesiredStateConfiguration/Restore-DscConfiguration)
- [Remove-DscConfigurationDocument](/powershell/module/PSDesiredStateConfiguration/Remove-DscConfigurationDocument)
- [Get-DscConfigurationStatus](/powershell/module/PSDesiredStateConfiguration/Get-DscConfigurationStatus)
- [Invoke-DscResource](/powershell/module/PSDesiredStateConfiguration/Invoke-DscResource)
- [Find-DscResource](/powershell/module/powershellget/find-dscresource)
- [Get-DscResource](/powershell/module/PSDesiredStateConfiguration/Get-DscResource)
- [New-DscChecksum](/powershell/module/PSDesiredStateConfiguration/New-DSCCheckSum)

- 構成のコンパイル (「[DSC 構成](../configurations/configurations.md)」を参照)

  **問題:** 構成のコンパイル中にパスワードの暗号化 (「 [MOF ファイルのセキュリティ保護](../pull-server/secureMOF.md)」を参照) が機能しません。

- メタ構成のコンパイル (「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」を参照)

- ユーザー コンテキストでのリソースの実行 ([ユーザーの資格情報を指定した DSC の実行 (RunAs)](../configurations/runAsUser.md) に関するページを参照)

- クラスベースのリソース (「[PowerShell クラスを使用したカスタム DSC リソースの記述](/previous-versions//dn948461(v=technet.10))」を参照)

- DSC リソースのデバッグ (「[DSC リソースのデバッグ](../troubleshooting/debugResource.md)」を参照)

  **問題:** リソースで PsDscRunAsCredential が使用されている場合に機能しません (「 [ユーザーの資格情報を指定して DSC を実行する](../configurations/runAsUser.md)」を参照)

- [ノードの相互依存関係の指定](../configurations/crossNodeDependencies.md)

- [リソースのバージョン管理](../configurations/sxsResource.md)

- プル クライアント (構成とリソース) (「[構成名を使用したプル クライアントのセットアップ](../pull-server/pullClientConfigNames.md)」を参照)

- [部分構成 (プルとプッシュ)](../pull-server/partialConfigs.md)

- [プル サーバーへの報告](../pull-server/reportServer.md)

- MOF 暗号化

- イベント ログ

- Azure Automation DSC レポート

- 完全に機能するリソース

  - **Archive**
  - **Environment**
  - **[最近使ったファイル]**
  - **Log**
  - **ProcessSet**
  - **レジストリ**
  - **スクリプト**
  - **WindowsPackageCab**
  - **WindowsProcess**
  - **WaitForAll** (「[ノードの相互依存関係の指定](../configurations/crossNodeDependencies.md)」を参照)
  - **WaitForAny** (「[ノードの相互依存関係の指定](../configurations/crossNodeDependencies.md)」を参照)
  - **WaitForSome** (「[ノードの相互依存関係の指定](../configurations/crossNodeDependencies.md)」を参照)

- 部分的に機能するリソース

  - **グループ**
  - **GroupSet**

    **問題:** 特定のインスタンスを 2 回呼び出す (同じ構成を 2 回実行する) と上記のリソースでエラーが発生します

  - **サービス**
  - **ServiceSet**

    **問題:** サービス (状態) の開始/停止でのみ正常に動作します。 StartupType、資格情報、説明などのほかのサービス属性を変更しようとするとエラーが発生します。 次のようなエラーがスローされます。

    ```
    Cannot find type [management.managementobject]: verify that the assembly containing this type is loaded.
    ```

- 機能しないリソース

  - **User**

## <a name="dsc-features-not-available-on-nano-server"></a>Nano Server で使用できない DSC 機能

現在、Nano Server で使用できない DSC 機能は次のとおりです。

- 暗号化パスワードを使用した MOF ドキュメントの暗号化解除
- プル サーバー - 現在、Nano Server でプル サーバーをセットアップすることはできません
- 動作する機能の一覧に含まれていないもの

## <a name="using-custom-dsc-resources-on-nano-server"></a>Nano Server でのカスタム DSC リソースの使用

Nano Server で使用できる Windows API と CLR ライブラリは限定されているため、完全な CLR バージョンの Windows で動作する DSC は、必ずしも Nano Server で動作するとは限りません。
DSC カスタム リソースを運用環境に展開する前に、エンド ツー エンドのテストを完了してください。

## <a name="see-also"></a>参照

- [Nano Server の概要](/windows-server/get-started/getting-started-with-nano-server)
