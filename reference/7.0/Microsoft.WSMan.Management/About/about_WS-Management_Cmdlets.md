---
description: Windows PowerShell の WS-Management コマンドレットを使用するための背景としての Web Services for Management (WS-MANAGEMENT) の概要について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 01/04/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/about/about_ws-management_cmdlets?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_WS Management_Cmdlets
ms.openlocfilehash: d11b8ca72951409a1b9a508623b3f39cae46e318
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390475"
---
# <a name="about-ws-management-cmdlets"></a>WS-Management のコマンドレットについて

## <a name="short-description"></a>概要

Windows PowerShell の WS-Management コマンドレットを使用するための背景としての Web Services for Management (WS-MANAGEMENT) の概要について説明します。

## <a name="long-description"></a>詳細説明

このトピックでは、Windows PowerShell の WS-Management コマンドレットを使用するための背景としての Web Services for Management (WS-MANAGEMENT) の概要について説明します。 このトピックでは、WS-MANAGEMENT に関する詳細情報へのリンクも示します。 Microsoft による WS-Management の実装は、Windows リモート管理 (WinRM) とも呼ばれています。

## <a name="about-ws-management"></a>WS-Management について

Windows リモート管理は、WS-Management プロトコルの Microsoft による実装であり、さまざまなベンダーのハードウェアおよびオペレーティングシステムが相互運用できる標準の SOAP ベースのファイアウォール対応プロトコルです。 WS-Management プロトコル仕様は、情報技術 (IT) インフラストラクチャを介してシステムが管理情報にアクセスして交換するための一般的な方法を提供します。 WS-Management とインテリジェントなプラットフォーム管理インターフェイス (IPMI) は、イベントコレクターと共に、Windows ハードウェア管理機能のコンポーネントです。

WS-Management プロトコルは、次の標準 Web サービス仕様に基づいています: HTTPS、SOAP over HTTP (WS-I profile)、SOAP 1.2、WS-ADDRESSING、WS-FEDERATION、WS 列挙型、および WS-I イベント。

## <a name="ws-management-and-wmi"></a>WS-Management と WMI

WS-Management は、Windows Management Instrumentation (WMI) によって公開されるデータを取得するために使用できます。 WMI データは、WS-Management スクリプト API または WinRM コマンドラインツールを使用するスクリプトまたはアプリケーションを使用して取得できます。 WS-Management は、埋め込みオブジェクトなど、使い慣れた WMI のクラスと操作の大部分をサポートしています。 WS-Management は、WMI を利用して、リソースに関するデータを収集したり、Windows ベースのコンピューター上のリソースを管理したりできます。 つまり、既存の WMI クラスのセットを使用して、企業内のディスク、ネットワークアダプター、サービス、プロセスなどのオブジェクトに関するデータを取得できます。 標準の WMI IPMI プロバイダーから利用可能なハードウェアデータにアクセスすることもできます。

## <a name="ws-management-windows-powershell-provider-wsman"></a>Windows PowerShell プロバイダー (WSMan) の WS-Management

WSMan プロバイダーは、使用可能な WS-Management 構成設定の階層ビューを提供します。 プロバイダーでは、さまざまな WS-Management 構成オプションを調べて設定することができます。

## <a name="ws-management-configuration"></a>WS-Management 構成

WS-Management がインストールおよび構成されていない場合、Windows PowerShell リモート処理は使用できません。 WS-Management のコマンドレットは実行されず、WS-Management スクリプトは実行されず、WSMan プロバイダーはデータ操作を実行できません。 WS-Management コマンドラインツール、WinRM、およびイベント転送は、WS-Management の構成にも依存します。

## <a name="ws-management-cmdlets"></a>WS-Management コマンドレット

WS-Management 機能は、一連のコマンドレットと WSMan プロバイダーを含むモジュールを使用して、Windows PowerShell に実装されます。 これらのコマンドレットを使用して、ローカルコンピューターとリモートコンピューターの WS-Management 設定を管理するために必要なエンドツーエンドのタスクを実行できます。

次の WS-Management コマンドレットを使用できます。

## <a name="connection-cmdlets"></a>接続コマンドレット

- 接続-WSMan: ローカルコンピューターをリモートコンピューター上の WS-Management (WinRM) サービスに接続します。

- 切断-WSMan: ローカルコンピューターをリモートコンピューター上の WS-Management (WinRM) サービスから切断します。

## <a name="management-data-cmdlets"></a>Management-Data コマンドレット

- Get-WSManInstance: リソース URI によって指定されたリソースインスタンスの管理情報を表示します。

- Invoke-WSManAction: リソース URI およびセレクターによって指定されたターゲットオブジェクトに対してアクションを呼び出します。

- 新規-WSManInstance: 新しい管理リソースインスタンスを作成します。

- -WSManInstance の削除: 管理リソースインスタンスを削除します。

- 設定-WSManInstance: リソースに関連する管理情報を変更します。

## <a name="setup-and-configuration-cmdlets"></a>セットアップと構成のコマンドレット

- Set-wsmanquickconfig: リモート管理用にローカルコンピューターを構成します。
  Set-WSManQuickConfig コマンドレットを使用して WS-Management を構成し、WS-Management (WinRM) サービスへのリモート接続を許可することができます。 Set-WSManQuickConfig コマンドレットは、次の操作を実行します。
  - WS-Management (WinRM) サービスが実行されているかどうかを判断します。 WinRM サービスが実行されていない場合は、Set-WSManQuickConfig コマンドレットによってサービスが開始されます。
  - WS-Management (WinRM) サービスのスタートアップの種類が [自動] に設定されます。
  - 任意の IP アドレスからの要求を受け入れるリスナーを作成します。 既定のトランスポートプロトコルは HTTP です。
  - WS-Management トラフィックに対してファイアウォールの例外を有効にします。

  注: windows Vista、Windows Server 2008、およびそれ以降のバージョンの Windows でこのコマンドレットを実行するには、[管理者として実行] オプションを使用して Windows PowerShell を起動する必要があります。

- テスト WSMan: WS-Management がインストールおよび構成されていることを確認します。 Test-WSMan コマンドレットは、WS-Management (WinRM) サービスが実行されていて、ローカルまたはリモートのコンピューターで構成されているかどうかをテストします。

- Enable-wsmancredssp: クライアントコンピューターで CredSSP 認証を無効にします。

- Enable-wsmancredssp: クライアントコンピューターで CredSSP 認証を有効にします。

- Enable-wsmancredssp: クライアントコンピューターの CredSSP 関連の構成を取得します。

## <a name="ws-management-specific-cmdlets"></a>WS-MANAGEMENT 固有のコマンドレット

- New-WSManSessionOption: WS-Management コマンドレットの1つ以上のパラメーターへの入力として使用する WSManSessionOption オブジェクトを作成します。

## <a name="additional-ws-management-information"></a>その他の WS-Management 情報

WS-MANAGEMENT の詳細については、Windows のドキュメントの次のトピックを参照してください。

[Windows リモート管理](/windows/win32/winrm/portal)

[Windows リモート管理について](/windows/win32/winrm/about-windows-remote-management)

[Windows リモート管理のインストールと構成](/windows/win32/winrm/installation-and-configuration-for-windows-remote-management)

[Windows リモート管理アーキテクチャ](/windows/win32/winrm/windows-remote-management-architecture)

[WS-Management Protocol (WS-Management プロトコル)](/windows/win32/winrm/ws-management-protocol)

[Windows リモート管理と WMI](/windows/win32/winrm/windows-remote-management-and-wmi)

[リソース URI](/windows/win32/winrm/resource-uris)

[リモートハードウェア管理](/windows/win32/winrm/remote-hardware-management)

[イベント](/windows/win32/winrm/events)

## <a name="see-also"></a>関連項目

[Connect-WSMan](xref:Microsoft.WSMan.Management.Connect-WSMan)

[Disable-WSManCredSSP](xref:Microsoft.WSMan.Management.Disable-WSManCredSSP)

[Disconnect-WSMan](xref:Microsoft.WSMan.Management.Disconnect-WSMan)

[Enable-WSManCredSSP](xref:Microsoft.WSMan.Management.Enable-WSManCredSSP)

[Get-WSManCredSSP](xref:Microsoft.WSMan.Management.Get-WSManCredSSP)

[Get-WSManInstance](xref:Microsoft.WSMan.Management.Get-WSManInstance)

[Invoke-WSManAction](xref:Microsoft.WSMan.Management.Invoke-WSManAction)

[New-WSManInstance](xref:Microsoft.WSMan.Management.New-WSManInstance)

[Remove-WSManInstance](xref:Microsoft.WSMan.Management.Remove-WSManInstance)

[Set-WSManInstance](xref:Microsoft.WSMan.Management.Set-WSManInstance)

[Set-WSManQuickConfig](xref:Microsoft.WSMan.Management.Set-WSManQuickConfig)

[Test-WSMan](xref:Microsoft.WSMan.Management.Test-WSMan)
