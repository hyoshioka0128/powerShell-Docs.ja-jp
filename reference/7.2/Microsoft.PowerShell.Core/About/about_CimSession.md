---
description: '**CimSession** オブジェクトと、CIM セッションと PowerShell セッションの違いについて説明します。'
Locale: en-US
ms.date: 05/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_cimsession?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_CimSession
ms.openlocfilehash: 556c3db21ea5e410b28f5546cdd8a433e0bc3e50
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599316"
---
# <a name="about-cimsession"></a>CimSession について

## <a name="short-description"></a>簡単な説明
**CimSession** オブジェクトと、CIM セッションと PowerShell セッションの違いについて説明します。

## <a name="long-description"></a>長い説明

Common Information Model (CIM) セッションは、ローカルコンピューターまたはリモートコンピューターへの接続を表すクライアント側オブジェクトです。 CIM セッションは、PowerShell セッション (PSSessions) の代わりに使用できます。 どちらの方法にも利点があります。

コマンドレットを使用して、 `New-CimSession` 接続に関する情報 (コンピューター名、接続に使用されるプロトコル、セッション id、インスタンス id など) を含む CIM セッションを作成できます。

接続を確立するために必要な情報を指定する **CimSession** オブジェクトを作成した後、PowerShell は接続をすぐには確立しません。 コマンドレットで CIM セッションを使用すると、PowerShell は指定されたコンピューターに接続し、コマンドレットが完了すると接続を終了します。

CIM セッションを使用する代わりに **PSSession** を作成した場合は、PowerShell によって接続設定が検証され、接続が確立され、維持されます。 CIM セッションを使用する場合、PowerShell は必要になるまでネットワーク接続を開いていません。 PowerShell セッションの詳細については、「 [about_PSSessions](about_PSSessions.md)」を参照してください。

## <a name="when-to-use-a-cim-session"></a>CIM セッションを使用する場合

CIM セッションを受け入れる WS-Man、Windows Management Instrumentation (WMI) プロバイダーまたは CIM を使用して操作するコマンドレットだけが使用できます。 他のコマンドレットの場合は、 **Pssessions** を使用します。

CIM セッションを使用すると、PowerShell はローカルクライアントでコマンドレットを実行します。 CIM セッションを使用して WMI プロバイダーに接続します。 ターゲットコンピューターには、PowerShell、または Windows オペレーティングシステムの任意のバージョンが必要ではありません。

これに対して、コマンドレットは、 **PSSession** を使用してターゲットコンピューター上で実行されます。
ターゲットシステムに PowerShell が必要です。 さらに、コマンドレットは、ローカルコンピューターにデータを送り返します。 PowerShell は、接続を介して送信されるデータを管理し、Windows リモート管理 (WinRM) によって設定された制限内でサイズを保持します。 CIM セッションでは、WinRM の制限は課せられません。

## <a name="using-cdxml-cmdlets"></a>CDXML コマンドレットの使用

CIM ベースのコマンドレット定義 XML (CDXML) コマンドレットは、任意の WMI プロバイダーを使用するように記述できます。 すべての WMI プロバイダーは、 **CimSession** オブジェクトを使用します。 CDXML の詳細については、「 [cdxml の定義と用語](/previous-versions/windows/desktop/wmi_v2/cdxml-overview)」を参照してください。

CDXML コマンドレットには、 **CimSession** オブジェクトの配列を受け取ることができる自動 **CimSession** パラメーターがあります。 既定では、PowerShell は、同時に実行される CIM 接続の数を15に制限します。 この制限は、 **ThrottleLimit** を実装する CDXML コマンドレットでオーバーライドできます。 **ThrottleLimit** を理解するには、個々のコマンドレットのドキュメントを参照してください。

## <a name="see-also"></a>関連項目

[New-CimSession](xref:CimCmdlets.New-CimSession)

[about_PSSessions](about_PSSessions.md)

