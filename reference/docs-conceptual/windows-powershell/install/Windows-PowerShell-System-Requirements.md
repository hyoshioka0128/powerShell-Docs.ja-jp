---
ms.date: 12/06/2019
keywords: powershell,コマンドレット
title: Windows PowerShell のシステム要件
description: この記事では、Windows PowerShell 3.0、Windows PowerShell 4.0、Windows PowerShell 5.0、Windows PowerShell 5.1 のシステム要件を列挙します。
ms.openlocfilehash: a82c0b1d6bf53e4a97db8414050a122ae5b7745d
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663831"
---
# <a name="windows-powershell-system-requirements"></a>Windows PowerShell のシステム要件

この記事では、Windows PowerShell 3.0、Windows PowerShell 4.0、Windows PowerShell 5.0、Windows PowerShell 5.1 のシステム要件を列挙します。 また、Windows PowerShell Integrated Scripting Environment (ISE)、Common Information Model (CIM) コマンド、ワークフローなど、特別な機能を紹介します。

Windows&reg; 8.1 および Windows Server&reg; 2012 R2 には、必要なプログラムがすべて付属しています。 この記事は、以前のリリースの Windows のユーザー向けです。

## <a name="operating-system-requirements"></a>オペレーティング システムの要件

### <a name="windows-powershell-51"></a>Windows PowerShell 5.1

Windows PowerShell 5.1 は、次のバージョンの Windows で実行されます。 Windows PowerShell 5.1 を実行するには、Windows Management Framework 5.1 をインストールします。 詳細については、「[WMF 5.1 のインストールと構成](../wmf/setup/install-configure.md)」を参照してください。

|              Windows のバージョン               |                           システム要件                            |
| ------------------------------------------ | ----------------------------------------------------------------------- |
| Windows Server 2019                        | 既定でインストールされています                                                    |
| Windows Server 2016                        | 既定でインストールされています                                                    |
| Windows Server 2012 R2                     | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows Server 2012                        | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows Server 2008 R2 Service Pack 1 | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows 10 バージョン 1607 以降             | 既定でインストールされています                                                    |
| Windows 10 バージョン 1507、1511              | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows 8.1                                | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows 7 Service Pack 1              | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |

### <a name="windows-powershell-50"></a>Windows PowerShell 5.0

Windows PowerShell 5.0 は、次のバージョンの Windows で実行できます。 Windows PowerShell 5.0 を実行するには、Windows Management Framework 5.1 をインストールします。 詳細については、「[WMF 5.1 のインストールと構成](../wmf/setup/install-configure.md)」を参照してください。 Windows Management Framework 5.1 は Windows Management Framework 5.0 に取って代わります。

|              Windows のバージョン               |                           システム要件                            |
| ------------------------------------------ | ----------------------------------------------------------------------- |
| Windows Server 2019                        | 上位バージョンが既定でインストール済み                                     |
| Windows Server 2016                        | 上位バージョンが既定でインストール済み                                     |
| Windows Server 2012 R2                     | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows Server 2012                        | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows Server 2008 R2 Service Pack 1 | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows 10 バージョン 1607 以降             | 上位バージョンが既定でインストール済み                                     |
| Windows 10 バージョン 1507、1511              | 既定でインストールされています                                                    |
| Windows 8.1                                | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |
| Windows 7 Service Pack 1              | [Windows Management Framework 5.1](https://aka.ms/wmf5download) をインストールする |

### <a name="windows-powershell-40"></a>Windows PowerShell 4.0

Windows PowerShell 4.0 は、次のバージョンの Windows で実行できます。 Windows PowerShell 4.0 を実行するには、お使いのオペレーティング システム用のバージョンの Windows Management Framework をインストールします。

|               Windows のバージョン               |                                             システム要件                                             |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Windows 8.1                                 | 既定でインストールされています                                                                                       |
| Windows Server 2012 R2                      | 既定でインストールされています                                                                                       |
| Windows&reg; 7 Service Pack 1              | [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) をインストールする |
| Windows Server&reg; 2008 R2 Service Pack 1 | [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855) をインストールする |

### <a name="windows-powershell-30"></a>Windows PowerShell 3.0

Windows PowerShell 3.0 は、次のバージョンの Windows で実行できます。 Windows PowerShell 3.0 を実行するには、お使いのオペレーティング システム用のバージョンの Windows Management Framework をインストールします。

|               Windows のバージョン               |                                             システム要件                                             |
| ------------------------------------------- | ---------------------------------------------------------------------------------------------------------- |
| Windows 8                                   | 既定でインストールされています                                                                                       |
| Windows Server 2012                         | 既定でインストールされています                                                                                       |
| Windows&reg; 7 Service Pack 1              | [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) をインストールする |
| Windows Server&reg; 2008 R2 Service Pack 1 | [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) をインストールする |
| Windows Server 2008 Service Pack 2     | [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) をインストールする |

## <a name="microsoft-net-framework-requirements"></a>Microsoft .NET Framework の要件

次の表は、Windows PowerShell の .NET Framework 要件をまとめたものです。

|        Version         |                                                                                 .NET 要件                                                                                  |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Windows PowerShell 5.1 | Microsoft .NET Framework 4.5 のフル インストールが必要です。 Windows 8.1 と Windows Server 2012 R2 には、既定で Microsoft .NET Framework 4.5 が付属しています。                           |
| Windows PowerShell 5.0 | Microsoft .NET Framework 4.5 のフル インストールが必要です。 Windows 8.1 と Windows Server 2012 R2 には、既定で Microsoft .NET Framework 4.5 が付属しています。                           |
| Windows PowerShell 4.0 | Microsoft .NET Framework 4.5 のフル インストールが必要です。 Windows 8.1 と Windows Server 2012 R2 には、既定で Microsoft .NET Framework 4.5 が付属しています。                           |
| Windows PowerShell 3.0 | Microsoft .NET Framework 4 のフル インストールが必要です。 Windows 8 と Windows Server 2012 には、既定で Microsoft .NET Framework 4.5 が含まれるため、この要件は満たされています。 |

次のリンクを使用し、Microsoft .NET Framework バージョンを Microsoft ダウンロード センターからダウンロードします。

|                     Version                      |                                                     Link                                                     |
| ------------------------------------------------ | ------------------------------------------------------------------------------------------------------------ |
| .NET Framework 4.5 (`dotNetFx45_Full_setup.exe`) | [Microsoft .NET Framework 4.5](https://go.microsoft.com/fwlink/?LinkID=242919)                               |
| .NET Framework 4 (`dotNetFx40_Full_setup.exe`)   | [Microsoft .NET Framework 4 (Web インストーラー)](https://www.microsoft.com/download/details.aspx?id=17851) |

## <a name="windows-management-framework-40"></a>Windows Management Framework 4.0

Windows PowerShell 5.0 には、Windows Server 2008 R2 SP1 と Windows 7 SP1 に Windows Management Framework 4.0 をプレインストールする必要があります。

## <a name="ws-management-30"></a>WS-Management 3.0

Windows PowerShell 3.0 と Windows PowerShell 4.0 は、WinRM サービスおよび WSMan プロトコルをサポートする WS-Management 3.0 を必要とします。 このプログラムは、Windows 8.1、Windows Server 2012 R2、Windows 8、Windows Server 2012、Windows Management Framework 4.0、Windows Management Framework 3.0 にも組み込まれています。

## <a name="windows-management-instrumentation-30"></a>Windows Management Instrumentation 3.0

Windows PowerShell 3.0 と Windows PowerShell 4.0 には、Windows Management Instrumentation 3.0 (WMI) が必要です。 このプログラムは、Windows 8.1、Windows Server 2012 R2、Windows 8、Windows Server 2012、Windows Management Framework 4.0、Windows Management Framework 3.0 にも組み込まれています。 このプログラムがコンピューターにインストールされていない場合、CIM コマンドなどの、WMI を必要とする機能は実行されません。

## <a name="common-language-runtime-40"></a>共通言語ランタイム 4.0

Windows PowerShell 3.0、Windows PowerShell 4.0、および Windows PowerShell 5.0 は、共通言語ランタイム (CLR) 4.0 に対してコンパイルされます。

## <a name="graphical-user-interface-requirements"></a>グラフィカル ユーザー インターフェイスの要件

Windows PowerShell はグラフィカル ユーザー インターフェイスを必要としないコンソール ベースのアプリケーションです。
画面 (モニター) もユーザー インターフェイスも持たない、Windows Server 2012 R2 または Windows Server 2012 の Server Core インストール オプションなどのコンピューターに適しています。

一部の項目では、グラフィカル ユーザー インターフェイスが必要です。 詳細については、各項目のヘルプ記事をご覧ください。

- Windows PowerShell Integrated Scripting Environment (ISE)。 詳細については、「[Windows PowerShell ISE の紹介](/powershell/scripting/components/ise/introducing-the-windows-powershell-ise)」を参照してください。
- コマンドレット
  - [Out-GridView](/powershell/module/microsoft.powershell.utility/out-gridview)
  - [Show-Command](/powershell/module/Microsoft.PowerShell.Utility/Show-Command)
  - [Show-ControlPanelItem](/powershell/module/Microsoft.PowerShell.Management/Show-ControlPanelItem)
  - [Show-EventLog](/powershell/module/Microsoft.PowerShell.Management/Show-EventLog)
- パラメーター
  - [Get-Help](/powershell/module/Microsoft.PowerShell.Core/Get-Help) コマンドレットの **ShowWindow** パラメーター。
  - [Register-PSSessionConfiguration](/powershell/module/Microsoft.PowerShell.Core/Register-PSSessionConfiguration) と [Set-PSSessionConfiguration](/powershell/module/Microsoft.PowerShell.Core/Set-PSSessionConfiguration) コマンドレットの **ShowSecurityDescriptorUI** パラメーター。

## <a name="windows-powershell-engine-requirements"></a>Windows PowerShell エンジンの要件

Windows PowerShell 4.0 は、Windows PowerShell 3.0 および Windows PowerShell 2.0 との下位互換性を保つように設計されています。 Windows PowerShell 2.0 と Windows PowerShell 3.0 用に記述されたコマンドレット、プロバイダー、スナップイン、モジュール、スクリプトは、未変更のまま Windows PowerShell 4.0 で実行できます。

ただし、Microsoft .NET Framework 4 でランタイムのアクティブ化ポリシーが変更されたため、Windows PowerShell 2.0 用に記述され、共通言語ランタイム (CLR) 2.0 でコンパイルされた Windows PowerShell ホスト プログラムは、変更を加えなければ Windows PowerShell 3.0 (CLR 4.0 でコンパイルされたもの) で実行できません。

Windows PowerShell 2.0 エンジンの最小要件は Microsoft .NET Framework 2.0.50727 です。 この要件は Microsoft .NET Framework 3.5 Service Pack 1 で満たされています。 Microsoft .NET Framework 4 以降のリリースの Microsoft .NET Framework では、この要件が満たされません。

Windows PowerShell 2.0 エンジンの追加とインストールや、必要なバージョンの Microsoft .NET Framework の追加とインストールについては、「[Windows PowerShell 2.0 エンジンのインストール](Installing-the-Windows-PowerShell-2.0-Engine.md)」をご覧ください。 Windows PowerShell 2.0 エンジンの開始に関する情報については、「[Windows PowerShell 2.0 エンジンの開始](../Starting-the-Windows-PowerShell-2.0-Engine.md)」を参照してください。

## <a name="windows-preinstallation-environment"></a>Windows プレインストール環境

Windows PowerShell 2.0、Windows PowerShell 3.0、および Windows PowerShell 4.0 は、Windows プレインストール環境 (Windows PE) で実行できます。 ただし、次のコマンドレットはサポートされていません。

- バックグラウンド インテリジェント転送サービス (BITS) のコマンドレット 詳細については、「[BitsTransfer](/powershell/module/bitstransfer/)」を参照してください。
- [Get-EventLog](/powershell/module/Microsoft.PowerShell.Management/Get-EventLog)
- [Get-WinEvent](/powershell/module/Microsoft.PowerShell.Diagnostics/Get-WinEvent)
- [Save-Help](/powershell/module/Microsoft.PowerShell.Core/Save-Help)
- [Update-Help](/powershell/module/Microsoft.PowerShell.Core/Update-Help)

**WinRM** サービスは Windows PE に存在しません。

## <a name="see-also"></a>関連項目

[Windows PowerShell のインストール](Installing-Windows-PowerShell.md)

[Windows PowerShell の開始](../Starting-Windows-PowerShell.md)

[Windows Management Framework](../wmf/overview.md)
