---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: Windows PowerShell 2.0 エンジンの使用
description: Windows PowerShell 2.0 エンジンは、既存のスクリプトまたはホスト プログラムが、Windows PowerShell 2.0 用に記述されており、かつ CLR 2.0 を使用してコンパイルされたホスト プログラムは修正なしには実行できないために、実行できないときに限り使用することを意図しています。
ms.openlocfilehash: 214b87b7314f31974801bb07f98ddea3b68008f0
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92664011"
---
# <a name="using-the-windows-powershell-20-engine"></a>Windows PowerShell 2.0 エンジンの使用

Windows PowerShell は、以前のバージョンとの下位互換性を保つように設計されています。 Windows PowerShell 2.0 用に記述されたコマンドレット、プロバイダー、スナップイン、モジュール、スクリプトは、変更することなく新しいバージョンの Windows PowerShell で実行できます。 ただし、Microsoft .NET Framework 4 では、ランタイムのアクティブ化ポリシーが変更されました。
Windows PowerShell 2.0 用に記述され、共通言語ランタイム (CLR) 2.0 でコンパイルされた Windows PowerShell ホスト プログラムは、変更を加えなければ、CLR 4.0 以降でコンパイルされた新しいバージョンの Windows PowerShell で実行できません。

Windows PowerShell 2.0 エンジンは、既存のスクリプトまたはホスト プログラムが Windows PowerShell 5.1 との互換性がないために実行できない場合に限り使用することが意図されています。 この例には、古いバージョンの Exchange または SQL Server モジュールが含まれます。 そのようなケースはまれと考えられます。

Windows PowerShell 2.0 エンジンを必要とする多くのプログラムは、エンジンを自動的に起動します。 次の手順は、エンジンを手動で起動する必要のあるまれな状況のためのものです。

## <a name="deprecation-and-security-concerns"></a>非推奨とセキュリティに関する考慮事項

Windows PowerShell 2.0 は、2017 年 8 月に非推奨となりました。 詳細については、PowerShell ブログの[お知らせ][]を参照してください。

Windows PowerShell 2.0 には、バージョン 3、4、および 5 で追加された強化機能とセキュリティ機能の多くがありません。 ユーザーはなるべくこれを使用しないことを強くお勧めします。 詳細については、「[シェルとスクリプト言語のセキュリティの比較][]」および「[PowerShell ♥ ブルー チーム][blueteam]」を参照してください。

## <a name="installing-and-enabling-required-programs"></a>必要なプログラムのインストールと有効化

Windows PowerShell 2.0 エンジンを起動する前に、Windows PowerShell 2.0 エンジンと Microsoft .NET Framework 3.5 Service Pack 1 を有効にします。 方法については、「[Windows PowerShell のインストール][]」をご覧ください。

Windows Management Framework 3.0 以降がインストールされているシステムには、すべての必要なコンポーネントが揃っています。 これ以上の構成は必要ありません。 Windows Management Framework のインストールについては、[WMF のインストールと構成][]に関するページを参照してください。

## <a name="how-to-start-the-windows-powershell-20-engine"></a>Windows PowerShell 2.0 エンジンを起動する方法

Windows PowerShell を起動すると、既定で最新バージョンが起動します。 Windows PowerShell 2.0 エンジンで Windows PowerShell を起動するには、`PowerShell.exe` の Version パラメーターを使用します。 コマンドは、Windows PowerShell と Cmd.exe を含む任意のコマンド プロンプトで実行できます。

```
PowerShell.exe -Version 2
```

## <a name="how-to-start-a-remote-session-with-the-windows-powershell-20-engine"></a>Windows PowerShell 2.0 エンジンを使用してリモート セッションを開始する方法

リモート セッションで Windows PowerShell 2.0 エンジンを実行するには、Windows PowerShell 2.0 エンジンを読み込むリモート コンピューターでセッション構成 (" _エンドポイント_ " とも呼ばれる) を作成します。 セッション構成は、リモート コンピューターに保存され、許可されているユーザーは誰でもそれを使って Windows PowerShell 2.0 エンジンを使用するセッションを作成できます。

これは、通常システム管理者によって実行される高度なタスクです。

次の手順では、 [Register-PSSessionConfiguration][] コマンドレットの **PSVersion** パラメーターを使用して、Windows PowerShell 2.0 エンジンを使用するセッション構成を作成します。 [New-PSSessionConfigurationFile][] コマンドレットの **PowerShellVersion** パラメーターを使用して、Windows PowerShell 2.0 エンジンを読み込むセッションのセッション構成ファイルを作成することも、 [Set-PSSessionConfiguration][] パラメーターの **PSVersion** パラメーターを使用して、Windows PowerShell 2.0 エンジンを使用するようにセッション構成を変更することもできます。

セッション構成ファイルの詳細については、「[about_Session_Configuration_Files][]」を参照してください。
セットアップとセキュリティを含むセッション構成の詳細については、「[about_Session_Configurations][]」を参照してください。

### <a name="to-start-a-remote-windows-powershell-20-session"></a>リモートの Windows PowerShell 2.0 セッションを開始するには

1. Windows PowerShell 2.0 エンジンを必要とするセッション構成を作成するには、 **PSVersion** パラメーターに値 `2.0` を指定して `Register-PSSessionConfiguration` コマンドレットを使用します。
   接続の "サーバー側" (受信側) にあるコンピューターでこのコマンドを実行します。

   次のサンプル コマンドは、Server01 コンピューター上に PS2 セッション構成を作成します。 このコマンドを実行するには、 **[管理者として実行]** オプションを使用して Windows PowerShell を起動します。

   ```powershell
   Register-PSSessionConfiguration -Name PS2 -PSVersion 2.0
   ```

1. PS2 セッション構成を使用する Server01 コンピューターでセッションを作成するには、リモート セッションを作成するコマンドレット (`New-PSSession` コマンドレットなど) の **ConfigurationName** パラメーターを使用します。

   セッション構成を使用するセッションを開始すると、Windows PowerShell 2.0 エンジンが自動的にセッションに読み込まれます。

   次のコマンドは、PS2 セッション構成を使用する Server01 コンピューターでセッションを開始します。 このコマンドでは、セッションが `$s` 変数に保存されます。

   ```powershell
   $s = New-PSSession -ComputerName Server01 -ConfigurationName PS2
   ```

## <a name="how-to-start-a-background-job-with-the-windows-powershell-20-engine"></a>Windows PowerShell 2.0 エンジンを使用してバックグラウンド ジョブを開始する方法

Windows PowerShell 2.0 エンジンを使用してバックグラウンド ジョブを開始するには、 [Start-Job][] コマンドレットの **PSVersion** パラメーターを使用します。

次のコマンドは、Windows PowerShell 2.0 エンジンを使用してバックグラウンド ジョブを開始します。

```powershell
Start-Job {Get-Process} -PSVersion 2.0
```

バックグラウンド ジョブの詳細については、「[about_Jobs][]」を参照してください。

<!-- link references -->
[お知らせ]: https://devblogs.microsoft.com/powershell/windows-powershell-2-0-deprecation/
[シェルとスクリプト言語のセキュリティの比較]: https://devblogs.microsoft.com/powershell/a-comparison-of-shell-and-scripting-language-security/
[blueteam]: https://devblogs.microsoft.com/powershell/powershell-the-blue-team/
[Windows PowerShell のインストール]: install/Installing-Windows-PowerShell.md
[WMF のインストールと構成]: wmf/setup/install-configure.md
[Register-PSSessionConfiguration]: /powershell/module/Microsoft.PowerShell.Core/Register-PSSessionConfiguration
[New-PSSessionConfigurationFile]: /powershell/module/Microsoft.PowerShell.Core/New-PSSessionConfigurationFile
[Set-PSSessionConfiguration]: /powershell/module/Microsoft.PowerShell.Core/Set-PSSessionConfiguration
[about_Session_Configuration_Files]: /powershell/module/Microsoft.PowerShell.Core/about/about_Session_Configuration_Files
[about_Session_Configurations]: /powershell/module/Microsoft.PowerShell.Core/about/about_Session_Configurations
[Start-Job]: /powershell/module/microsoft.powershell.core/start-job
[about_Jobs]: /powershell/module/microsoft.powershell.core/about/about_jobs
