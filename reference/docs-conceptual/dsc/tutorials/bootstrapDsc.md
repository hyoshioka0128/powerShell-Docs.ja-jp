---
ms.date: 11/09/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC を使用した初回起動時の仮想マシンの構成
description: この記事では、初回起動時に DSC を使用して仮想マシンを構成する方法について説明します
ms.openlocfilehash: 09449053ff085209dec6ccbfa800e5d112d1c769
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94389999"
---
# <a name="configure-a-virtual-machines-at-initial-boot-up-by-using-dsc"></a>DSC を使用した初回起動時の仮想マシンの構成

> [!IMPORTANT]
> 適用先:Windows PowerShell 5.0

## <a name="requirements"></a>必要条件

> [!NOTE]
> このトピックで説明されている **DSCAutomationHostEnabled** レジストリ キーは、PowerShell 4.0 では使用できません。 PowerShell 4.0 の初回起動時に、新しい仮想マシンを構成する方法については、「[Want to Automatically Configure Your Machines Using DSC at Initial Boot-up?](https://devblogs.microsoft.com/powershell/want-to-automatically-configure-your-machines-using-dsc-at-initial-boot-up/)」 (初回起動時に DSC を使用して自動的にマシンを構成する方法) をご覧ください。

これらの例を実行するには、以下が必要になります。

- 作業する起動可能な VHD。 [Evaluation Center](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016) で、Windows Server 2016 の評価版と共に ISO をダウンロードできます。
  「[Creating Bootable Virtual Hard Disks](/previous-versions/windows/it-pro/windows-7/gg318049(v=ws.10))」 (起動可能な仮想ハード ディスクの作成) で、ISO イメージから VHD を作成する方法に関する手順を参照してください。
- Hyper-V が有効なホスト コンピューター。 詳細については、「[Hyper-V の概要](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh831531(v=ws.11))」をご覧ください。

  DSC を使用すると、ソフトウェアのインストールと初回起動時のコンピューターの構成を自動化できます。 初回起動時のプロセスで実行されるように、起動可能なメディア (VHD など) に構成 MOF ドキュメントまたはメタ構成のいずれかを挿入して、この操作を行います。 この動作は、`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System` の [DSCAutomationHostEnabled レジストリ キー](DSCAutomationHostEnabled.md) によって指定されます。
  このキーの既定値は 2 です。これは、初回起動時に DSC を実行することを許可します。

  初回起動時に DSC を実行しない場合は、[DSCAutomationHostEnabled レジストリ キー](DSCAutomationHostEnabled.md)を 0 に設定します。

- 構成 MOF ドキュメントを VHD に挿入する
- DSC のメタ構成を VHD に挿入する
- 起動時の DSC を無効にする

> [!NOTE]
> `Pending.mof` と `MetaConfig.mof` を同時にコンピューターに挿入することができます。
> 両方のファイルが存在する場合、`MetaConfig.mof` に指定された設定が優先されます。

## <a name="inject-a-configuration-mof-document-into-a-vhd"></a>構成 MOF ドキュメントを VHD に挿入する

初回起動時に構成を適用するために、コンパイル済みの構成 MOF ドキュメントをその `Pending.mof` ファイルとして VHD に挿入できます。 **DSCAutomationHostEnabled** レジストリ キーを 2 (既定値) に設定した場合、コンピューターが最初に起動されたときに、DSC では `Pending.mof` で定義された構成を適用します。

この例では次の構成を使用します。ここでは、新しいコンピューターに IIS をインストールします。

```powershell
Configuration SampleIISInstall
{
    Import-DscResource -ModuleName 'PSDesiredStateConfiguration'

    node ('localhost')
    {
        WindowsFeature IIS
        {
            Ensure = 'Present'
            Name   = 'Web-Server'
        }
    }
}
```

### <a name="to-inject-the-configuration-mof-document-on-the-vhd"></a>VHD 上で構成 MOF ドキュメントを挿入するには

1. [Mount-VHD](/powershell/module/hyper-v/mount-vhd) コマンドレットを呼び出して、構成を挿入する VHD をマウントします。 次に例を示します。

   ```powershell
   Mount-VHD -Path C:\users\public\documents\vhd\Srv16.vhd
   ```

1. PowerShell 5.0 以降を実行しているコンピューターで、上記の構成 (**SampleIISInstall**) を PowerShell スクリプト (.ps1) ファイルとして保存します。

1. PowerShell コンソールで、.ps1 ファイルを保存したフォルダーに移動します。

1. 次の PowerShell コマンドを実行して、MOF ドキュメントをコンパイルします (DSC 構成のコンパイルについては、「[DSC 構成](../configurations/configurations.md)」をご覧ください):

   ```powershell
   . .\SampleIISInstall.ps1
   SampleIISInstall
   ```

1. この操作を行うと、`SampleIISInstall` という名前の新しいフォルダーに、`localhost.mof` ファイルが作成されます。 [Move-Item](/powershell/module/microsoft.powershell.management/move-item) コマンドレットを使用して、`Pending.mof` に名前を変更し、VHD の適切な場所にそのファイルを移動します。 次に例を示します。

   ```powershell
   Move-Item -Path C:\DSCTest\SampleIISInstall\localhost.mof -Destination E:\Windows\System32\Configuration\Pending.mof
   ```

1. [Dismount-VHD](/powershell/module/hyper-v/dismount-vhd) コマンドレットを呼び出して、VHD のマウントを解除します。
   次に例を示します。

   ```powershell
   Dismount-VHD -Path C:\users\public\documents\vhd\Srv16.vhd
   ```

1. DSC MOF ドキュメントをインストールした VHD を使用して、VM を作成します。

初回起動が行われ、オペレーティング システムがインストールされると、IIS がインストールされます。 [Get-WindowsFeature](/powershell/module/servermanager/get-windowsfeature) コマンドレットを呼び出して、これを確認することができます。

## <a name="inject-a-dsc-metaconfiguration-into-a-vhd"></a>DSC のメタ構成を VHD に挿入する

メタ構成 (「[ローカル構成マネージャーの構成](../managing-nodes/metaConfig.md)」を参照) をその `MetaConfig.mof` ファイルとして VHD に挿入して、コンピューターが初回起動時に構成をプルするように構成することもできます。 **DSCAutomationHostEnabled** レジストリ キーを 2 (既定値) に設定した場合、コンピューターが最初に起動されたときに、`MetaConfig.mof` で定義されたメタ構成が DSC により LCM に適用されます。 メタ構成で LCM がプル サーバーから構成をプルする必要があることを指定した場合、コンピューターは初回起動時にプル サーバーから構成をプルしようとします。 DSC プル サーバーのセットアップについては、「[DSC Web プル サーバーのセットアップ](../pull-server/pullServer.md)」をご覧ください。

この例では、前のセクションで示された構成 (**SampleIISInstall**)、および次のメタ構成の両方を使用します。

```powershell
[DSCLocalConfigurationManager()]
configuration PullClientBootstrap
{
    Node localhost
    {
        Settings
        {
            RefreshMode = 'Pull'
            RefreshFrequencyMins = 30
            RebootNodeIfNeeded = $true
        }
        ConfigurationRepositoryWeb CONTOSO-PullSrv
        {
            ServerURL = 'https://CONTOSO-PullSrv:8080/PSDSCPullServer.svc'
            RegistrationKey = '140a952b-b9d6-406b-b416-e0f759c9c0e4'
            ConfigurationNames = @('SampleIISInstall')
        }
    }
}
```

### <a name="to-inject-the-metaconfiguration-mof-document-on-the-vhd"></a>VHD 上でメタ構成 MOF ドキュメントを挿入するには

1. [Mount-VHD](/powershell/module/hyper-v/mount-vhd) コマンドレットを呼び出して、メタ構成を挿入する VHD をマウントします。 次に例を示します。

   ```powershell
   Mount-VHD -Path C:\users\public\documents\vhd\Srv16.vhd
   ```

1. [DSC Web プル サーバーをセットアップ](../pull-server/pullServer.md)して、**SampleIISInstall** 構成を適切なフォルダーに保存します。

1. PowerShell 5.0 以降を実行しているコンピューターで、上記のメタ構成 (**PullClientBootstrap**) を PowerShell スクリプト (.ps1) ファイルとして保存します。

1. PowerShell コンソールで、.ps1 ファイルを保存したフォルダーに移動します。

1. 次の PowerShell コマンドを実行して、メタ構成 MOF ドキュメントをコンパイルします (DSC 構成のコンパイルについては、「[DSC 構成](../configurations/configurations.md)」をご覧ください)。

   ```powershell
   . .\PullClientBootstrap.ps1
   PullClientBootstrap
   ```

1. この操作を行うと、`PullClientBootstrap` という名前の新しいフォルダーに、`localhost.meta.mof` ファイルが作成されます。 [Move-Item](/powershell/module/microsoft.powershell.management/move-item) コマンドレットを使用して、`MetaConfig.mof` に名前を変更し、VHD の適切な場所にそのファイルを移動します。

   ```powershell
   Move-Item -Path C:\DSCTest\PullClientBootstrap\localhost.meta.mof -Destination E:\Windows\System32\Configuration\MetaConfig.mof
   ```

1. [Dismount-VHD](/powershell/module/hyper-v/dismount-vhd) コマンドレットを呼び出して、VHD のマウントを解除します。
   次に例を示します。

   ```powershell
   Dismount-VHD -Path C:\users\public\documents\vhd\Srv16.vhd
   ```

1. DSC MOF ドキュメントをインストールした VHD を使用して、VM を作成します。

初回起動が行われ、オペレーティング システムがインストールされると、DSC はプル サーバーから構成をプルして、IIS がインストールされます。 [Get-WindowsFeature](/powershell/module/servermanager/get-windowsfeature) コマンドレットを呼び出して、これを確認することができます。

## <a name="disable-dsc-at-boot-time"></a>起動時の DSC を無効にする

`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System\DSCAutomationHostEnabled` キーの値は既定で
2 に設定されます。これにより、コンピューターが保留中または最新の状態である場合に、DSC 構成を実行することができます。 初回起動時に構成を実行しない場合は、このキーの値を 0 に設定する必要があります。

1. [Mount-VHD](/powershell/module/hyper-v/mount-vhd) コマンドレットを呼び出して、VHD をマウントします。 次に例を示します。

   ```powershell
   Mount-VHD -Path C:\users\public\documents\vhd\Srv16.vhd
   ```

1. `reg load` を呼び出して、VHD からレジストリの `HKLM\Software` サブキーを読み込みます。

   ```powershell
   reg load HKLM\Vhd E:\Windows\System32\Config\Software
   ```

1. 読み込まれたハイブで `DSCAutomationHostEnabled` の値を 0 に変更します。

   ```powershell
   reg add "HKLM\Vhd\Software\Microsoft\Windows\CurrentVersion\Policies\System" /v DSCAutomationHostEnabled /t REG_DWORD /d 0 /f
   ```

1. 次のコマンドを実行して、レジストリをアンロードします。

   ```powershell
   reg unload HKLM\Vhd
   ```

## <a name="see-also"></a>参照

[DSC 構成](../configurations/configurations.md)

[DSCAutomationHostEnabled レジストリ キー](DSCAutomationHostEnabled.md)

[ローカル構成マネージャー (LCM) の構成](../managing-nodes/metaConfig.md)

[DSC Web プル サーバーのセットアップ](../pull-server/pullServer.md)
