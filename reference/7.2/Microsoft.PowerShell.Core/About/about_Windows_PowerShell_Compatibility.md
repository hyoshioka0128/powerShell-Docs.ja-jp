---
description: PowerShell 7 の Windows PowerShell 互換性機能について説明します。
Locale: en-US
ms.date: 04/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_windows_powershell_compatibility?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Windows_PowerShell_Compatibility
ms.openlocfilehash: 3a7605765da8c17bad2d98a1d3fc3f7cc96f57b8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605442"
---
# <a name="about-windows-powershell-compatibility"></a>Windows PowerShell の互換性について

## <a name="short-description"></a>概要

PowerShell 7 の Windows PowerShell 互換性機能について説明します。

## <a name="long-description"></a>詳細説明

モジュールマニフェストによってモジュールが PowerShell Core と互換性があることが示されていない限り、フォルダー内のモジュール `%windir%\system32\WindowsPowerShell\v1.0\Modules` は windows powershell の互換性機能によってバックグラウンドの Windows powershell 5.1 プロセスで読み込まれます。

### <a name="using-the-compatibility-feature"></a>互換性機能の使用

Windows PowerShell の互換性機能を使用して最初のモジュールをインポートすると、PowerShell によって、 `WinPSCompatSession` バックグラウンドの Windows powershell 5.1 プロセスで実行されているという名前のリモートセッションが作成されます。 このプロセスは、互換性機能が最初のモジュールをインポートするときに作成されます。 このプロセスは、最後のモジュールが削除されたとき (を使用 `Remove-Module` )、または PowerShell プロセスが終了したときに閉じられます。

セッションに読み込まれたモジュール `WinPSCompatSession` は、暗黙的なリモート処理によって使用され、現在の PowerShell セッションに反映されます。 これは、PowerShell ジョブで使用されるトランスポート方法と同じです。

モジュールをセッションにインポートすると `WinPSCompatSession` 、暗黙的なリモート処理によってユーザーのディレクトリにプロキシモジュールが生成され、 `$env:Temp` このプロキシモジュールが現在の PowerShell セッションにインポートされます。 これにより、PowerShell は、Windows PowerShell の互換性機能を使用してモジュールが読み込まれたことを検出できます。

セッションが作成されると、逆シリアル化されたオブジェクトで正しく動作しない操作に対して使用できます。 パイプライン全体が Windows PowerShell で実行され、最終的な結果だけが返されます。 次に例を示します。

```powershell
$s = Get-PSSession -Name WinPSCompatSession
Invoke-Command -Session $s -ScriptBlock {
  "Running in Windows PowerShell version $($PSVersionTable.PSVersion)"
}
```

互換性機能は、次の2つの方法で呼び出すことができます。

- **Usewindowspowershell** パラメーターを使用してモジュールを明示的にインポートする

   ```powershell
   Import-Module -Name ScheduledTasks -UseWindowsPowerShell
   ```

- Windows PowerShell モジュールをモジュール名、パス、またはコマンド検出による自動読み込みによって暗黙的にインポートします。

   ```powershell
   Import-Module -Name ServerManager
   Get-AppLockerPolicy -Local
   ```

   まだ読み込まれていない場合は、を実行すると、AppLocker モジュールが自動読み込みされ  `Get-AppLockerPolicy` ます。

Windows PowerShell 互換性ブロックは、PowerShell 構成ファイルの設定に一覧表示されているモジュールの読み込みをブロック `WindowsPowerShellCompatibilityModuleDenyList` します。

この設定の既定値は次のとおりです。

```json
"WindowsPowerShellCompatibilityModuleDenyList":  [
   "PSScheduledJob","BestPractices","UpdateServices"
]
```

### <a name="managing-implicit-module-loading"></a>暗黙的なモジュール読み込みの管理

Windows PowerShell の互換性機能の暗黙的なインポート動作を無効にするには、 `DisableImplicitWinCompat` powershell 構成ファイルの設定を使用します。 この設定をファイルに追加でき `powershell.config.json` ます。 詳細については、「 [about_powershell_config](about_powershell_config.md)」を参照してください。

この例では、Windows PowerShell の互換性に関するモジュール読み込みの暗黙的な機能を無効にする構成ファイルを作成する方法を示します。

```powershell
$ConfigPath = "$PSHOME\DisableWinCompat.powershell.config.json"
$ConfigJSON = ConvertTo-Json -InputObject @{
  "DisableImplicitWinCompat" = $true
  "Microsoft.PowerShell:ExecutionPolicy" = "RemoteSigned"
}
$ConfigJSON | Out-File -Force $ConfigPath
pwsh -settingsFile $ConfigPath
```

モジュールの互換性に関する最新情報については、 [PowerShell 7 モジュールの互換性](https://aka.ms/PSModuleCompat) リストを参照してください。

### <a name="managing-cmdlet-clobbering"></a>コマンドレット clobbering の管理

Windows PowerShell の互換性機能は、暗黙的なリモート処理を使用してモジュールを互換モードで読み込みます。 その結果、モジュールによってエクスポートされたコマンドは、現在の PowerShell 7 セッションで同じ名前のコマンドよりも優先されるようになります。 PowerShell 7.0.0 リリースでは、PowerShell に付属するコアモジュールがこれに含まれていました。

PowerShell 7.1 では、次のコア PowerShell モジュールが上書きされないように動作が変更されました。

- Microsoft. PowerShell ホスト
- Microsoft.PowerShell.Diagnostics
- Microsoft.PowerShell.Host
- Microsoft.PowerShell.Management
- Microsoft.PowerShell.Security
- Microsoft.PowerShell.Utility
- Microsoft.WSMan.Management

また、PowerShell 7.1 では、互換性モードで上書きできない追加のモジュールを一覧表示する機能も追加されました。

`WindowsPowerShellCompatibilityNoClobberModuleList`PowerShell 構成ファイルに設定を追加できます。 この設定の値は、モジュール名のコンマ区切りのリストです。 この設定の既定値は次のとおりです。

```json
"WindowsPowerShellCompatibilityNoClobberModuleList": [ ]
```

## <a name="limitations"></a>制限事項

Windows PowerShell の互換性機能:

1. Windows コンピューターでのみローカルに機能する
1. Windows PowerShell 5.1 が必要です。
1. ライブオブジェクトではなく、シリアル化されたコマンドレットパラメーターと戻り値を操作します。
1. Windows PowerShell リモート処理セッションにインポートされたすべてのモジュールは、同じ実行空間を共有します。

## <a name="keywords"></a>Keywords

about_Windows_PowerShell_Compatibility

## <a name="see-also"></a>関連項目

[about_Modules](about_Modules.md)

[Import-Module](xref:Microsoft.PowerShell.Core.Import-Module)

