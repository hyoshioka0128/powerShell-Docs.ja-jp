---
title: PowerShell Core の WS-Management (WSMan) リモート処理
description: WSMan を使用した PowerShell Core のリモート処理
ms.date: 08/06/2018
ms.openlocfilehash: fdc4159279db28b8ee60bc0853e19512a1f9ec14
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92501305"
---
# <a name="ws-management-wsman-remoting-in-powershell-core"></a>PowerShell Core の WS-Management (WSMan) リモート処理

## <a name="instructions-to-create-a-remoting-endpoint"></a>リモート エンドポイントの作成手順

Windows PowerShell Core パッケージには、WinRM プラグイン (`pwrshplugin.dll`) とインストール スクリプト (`Install-PowerShellRemoting.ps1`) が `$PSHome` に含まれています。 これらのファイルにより、そのエンドポイントが指定されている場合、PowerShell が受信の PowerShell リモート接続を受け入れるようになります。

### <a name="motivation"></a>目的

PowerShell をインストールすると、`New-PSSession` と `Enter-PSSession` を使用したリモート コンピューターへの PowerShell セッションを確立できます。 PowerShell でリモートからの受信接続を受け入れるようにするには、ユーザーが WinRM リモート エンドポイントを作成する必要があります。 これは、ユーザーが Install-PowerShellRemoting.ps1 を実行し、WinRM エンドポイントを作成することを明示的に選択するシナリオです。 このインストール スクリプトは、`Enable-PSRemoting` に同じアクションを実行する機能が追加されるまでの短期的なソリューションです。 詳細については、問題 [#1193](https://github.com/PowerShell/PowerShell/issues/1193) を参照してください。

### <a name="script-actions"></a>[スクリプト操作]

スクリプトは、以下のことを行います

1. `$env:windir\System32\PowerShell` 内にプラグイン用のディレクトリを作成します
1. この場所に pwrshplugin.dll をコピーします
1. 構成ファイルを生成します
1. WinRM にそのプラグインを登録します

### <a name="registration"></a>登録

このスクリプトは、管理者レベルで PowerShell セッション内で実行される必要があり、次の 2 つのモードで実行されます。

#### <a name="executed-by-the-instance-of-powershell-that-it-will-register"></a>登録する PowerShell インスタンスによって実行されます

```powershell
Install-PowerShellRemoting.ps1
```

#### <a name="executed-by-another-instance-of-powershell-on-behalf-of-the-instance-that-it-will-register"></a>登録するインスタンスの代わりの別の PowerShell インスタンスによって実行されます

```powershell
<path to powershell>\Install-PowerShellRemoting.ps1 -PowerShellHome "<absolute path to the instance's $PSHOME>"
```

たとえば次のようになります。

```powershell
Set-Location -Path 'C:\Program Files\PowerShell\6.0.0\'
.\Install-PowerShellRemoting.ps1 -PowerShellHome "C:\Program Files\PowerShell\6.0.0\"
```

> [!NOTE]
> リモート処理登録スクリプトによって、WinRM が再起動されます。 既存のすべての PSRP セッションは、スクリプトが実行された直後に終了します。 リモート セッションの間も実行していると、スクリプトによって接続が終了されます。

## <a name="how-to-connect-to-the-new-endpoint"></a>新しいエンドポイントに接続する方法

`-ConfigurationName "some endpoint name"` を指定して、新しい PowerShell エンドポイントに対して PowerShell セッションを作成します。 上記の例から、PowerShell インスタンスに接続するには、次のいずれかを使用します。

```powershell
New-PSSession ... -ConfigurationName "powershell.6.0.0"
Enter-PSSession ... -ConfigurationName "powershell.6.0.0"
```

なお、`-ConfigurationName` が指定されていない `New-PSSession` と `Enter-PSSession` の呼び出しは、既定の PowerShell のエンドポイントである `microsoft.powershell` をターゲットとします。
