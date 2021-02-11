---
description: Windows PowerShell スナップインについて説明し、それらを使用および管理する方法を示します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pssnapins?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_PSSnapins
ms.openlocfilehash: 494b3275e4fe8a3aacdc358317950542962957cf
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388894"
---
# <a name="about-pssnapins"></a>PSSnapins について

## <a name="short-description"></a>概要

Windows PowerShell スナップインについて説明し、それらを使用および管理する方法を示します。

## <a name="long-description"></a>詳細説明

Windows PowerShell スナップインは、Windows PowerShell プロバイダーと、 \/ またはコマンドレットを含む Microsoft .NET Framework アセンブリです。 Windows PowerShell には一連の基本的なスナップインが含まれていますが、作成したり他のユーザーから取得したプロバイダーやコマンドレットを含むスナップインを追加することで、Windows PowerShell のパワーと値を拡張できます。

スナップインを追加すると、そのスナップインに含まれているコマンドレットとプロバイダーは、現在のセッションですぐに使用できるようになりますが、変更は現在のセッションにのみ影響します。

今後のすべてのセッションにスナップインを追加するには、そのスナップインを Windows PowerShell プロファイルに保存します。 また、Export-Console コマンドレットを使用して、スナップイン名をコンソールファイルに保存し、今後のセッションで使用することもできます。 複数のコンソールファイルを、それぞれ異なるスナップインのセットで保存することもできます。

注: windows powershell スナップイン (PSSnapins) は、windows PowerShell 3.0 および Windows PowerShell 2.0 で使用できます。 これらは、今後のバージョンで変更されたり使用できなくなったりする可能性があります。 Windows PowerShell のコマンドレットとプロバイダーをパッケージするには、モジュールを使用します。 モジュールの作成と、モジュールへのスナップインの変換の詳細については、「 [Windows PowerShell モジュールの記述](/powershell/scripting/developer/module/writing-a-windows-powershell-module)」を参照してください。

### <a name="finding-snap-ins"></a>スナップインの検索

コンピューター上の Windows PowerShell スナップインの一覧を取得するには、次のように入力します。

```powershell
Get-PSSnapin
```

各 Windows PowerShell プロバイダーのスナップインを取得するには、次のように入力します。

```powershell
Get-PSProvider | Format-List name, pssnapin
```

Windows PowerShell スナップインでコマンドレットの一覧を取得するには、次のように入力します。

```powershell
Get-Command -Module <snap-in_name>
```

### <a name="installing-a-snap-in"></a>スナップインのインストール

組み込みのスナップインはシステムに登録され、Windows PowerShell の起動時に既定のセッションに追加されます。 ただし、他のユーザーから作成または取得したスナップインを登録してから、セッションにスナップインを追加する必要があります。

### <a name="registering-a-snap-in"></a>スナップインの登録

Windows PowerShell スナップインは、.dll ファイルにコンパイルされる .NET Framework の言語で記述されたプログラムです。 スナップインでプロバイダーとコマンドレットを使用するには、最初にスナップインを登録 (レジストリに追加) する必要があります。

ほとんどのスナップインには、.dll ファイルを登録するインストールプログラム (.exe または .msi ファイル) が含まれています。 ただし、.dll ファイルとしてスナップインを受け取った場合は、システムに登録することができます。 詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

システムに登録されているすべてのスナップインを取得したり、スナップインが登録されていることを確認したりするには、次のように入力します。

```powershell
Get-PSSnapin -registered
```

### <a name="adding-the-snap-in-to-the-current-session"></a>現在のセッションにスナップインを追加しています

登録されているスナップインを現在のセッションに追加するには、Add-PsSnapin コマンドレットを使用します。 たとえば、Microsoft SQL Server スナップインをセッションに追加するには、次のように入力します。

```powershell
Add-PSSnapin sql
```

コマンドが完了すると、スナップインのプロバイダーとコマンドレットがセッションで使用できるようになります。 ただし、保存しない限り、現在のセッションでのみ使用できます。

### <a name="saving-the-snap-ins"></a>スナップインを保存しています

今後の Windows PowerShell セッションでスナップインを使用するには、Windows PowerShell プロファイルに Add-PsSnapin コマンドを追加します。 または、スナップインの名前をコンソールファイルにエクスポートします。

プロファイルに Add-PSSnapin コマンドを追加すると、それ以降のすべての Windows PowerShell セッションで使用できるようになります。 セッションでスナップインの名前をエクスポートする場合、スナップインが必要な場合にのみ、エクスポートファイルを使用できます。

Windows PowerShell プロファイルに Add-PsSnapin コマンドを追加するには、プロファイルを開き、コマンドを貼り付けるか入力して、プロファイルを保存します。 詳細については、「about_Profiles」を参照してください。

コンソールファイル (. .psc1) でセッションからスナップインを保存するには、Export-Console コマンドレットを使用します。 たとえば、現在のセッション構成のスナップインを現在のディレクトリの .psc1 ファイルに保存するには、次のように入力します。

```powershell
Export-Console NewConsole
```

詳細については、「Export-Console」を参照してください。

### <a name="opening-windows-powershell-with-a-console-file"></a>コンソールファイルを使用して WINDOWS POWERSHELL を開く

スナップインを含むコンソールファイルを使用するには、Cmd.exe または別の Windows PowerShell セッションでコマンドプロンプトから Windows PowerShell (PowerShell.exe) を起動します。 PsConsoleFile パラメーターを使用して、スナップインを含むコンソールファイルを指定します。 たとえば、次のコマンドは、.psc1 コンソールファイルを使用して Windows PowerShell を起動します。

```powershell
PowerShell.exe -psconsolefile NewConsole.psc1
```

スナップインのプロバイダーとコマンドレットが、セッションで使用できるようになりました。

### <a name="removing-a-snap-in"></a>スナップインの削除

現在のセッションから Windows PowerShell スナップインを削除するには、Remove-PsSnapin コマンドレットを使用します。 たとえば、現在のセッションから SQL Server スナップインを削除するには、次のように入力します。

```powershell
Remove-PSSnapin sql
```

このコマンドレットは、セッションからスナップインを削除します。 スナップインはまだ読み込まれていますが、サポートされているプロバイダーとコマンドレットは使用できなくなりました。

### <a name="built-in-commands"></a>組み込みコマンド

Windows powershell 2.0 および windows PowerShell 3.0 以降の古いスタイルのホストプログラムでは、Windows PowerShell と共にインストールされる組み込みのコマンドは、すべての Windows PowerShell セッションに自動的に追加されるスナップインにパッケージ化されます。

Windows PowerShell 3.0 以降では、新しいスタイルのホストプログラム (InitialSessionState. CreateDefault2 メソッドを使用してセッションを開始するもの) が含まれています。組み込みのコマンドはモジュールにパッケージ化されています。 例外は、常にスナップインとして表示される、PowerShell です。 コアスナップインは、既定ではすべてのセッションに含まれています。 組み込みモジュールは、初回使用時に自動的に読み込まれます。

注: New-PSSession コマンドレットを使用して開始されたセッションを含めたリモートセッションは、組み込みコマンドがスナップインにパッケージ化されている古い形式のセッションです。

次のスナップイン (モジュール) は、Windows PowerShell と共にインストールされます。

- Microsoft. PowerShell-Windows PowerShell の基本機能の管理に使用するプロバイダーとコマンドレットが含まれています。 これには、FileSystem、Registry、Alias、Environment、Function、および Variable プロバイダーと、Get-help、Get-Command、および Get-help などの基本的なコマンドレットが含まれます。

- Microsoft. PowerShell-Windows PowerShell ホストによって使用されるコマンドレットが含まれています。たとえば、Start-Transcript や、トランスクリプトを停止します。

- Microsoft. PowerShell. 管理-Windows ベースの機能の管理に使用される Get-Service や Get-ChildItem などのコマンドレットが含まれています。

- Get-authenticodesignature-Windows PowerShell のセキュリティを管理するために使用される証明書プロバイダーとコマンドレットが含まれています。たとえば、Get Acl、Convertto-html、SecureString などです。

- Microsoft. PowerShell: オブジェクトとデータを操作するために使用するコマンドレットが含まれています (Get Member、Write Host、Format List など)。

- Enable-wsmancredssp-Connect-WSMan やなど、Windows リモート管理サービスを管理する WSMan プロバイダーとコマンドレットが含まれています。

## <a name="logging-snap-in-events"></a>スナップインイベントのログ記録

Windows PowerShell 3.0 以降では、モジュールの LogPipelineExecutionDetails プロパティを TRUE に設定することにより、Windows PowerShell モジュールおよびスナップインでコマンドレットの実行イベントを記録できます。 詳細については、「 [about_EventLogs](about_EventLogs.md)」を参照してください。

## <a name="see-also"></a>関連項目

[Add-pssnapin](xref:Microsoft.PowerShell.Core.Add-PSSnapin)

[Add-pssnapin](xref:Microsoft.PowerShell.Core.Get-PSSnapin)

[Add-pssnapin](xref:Microsoft.PowerShell.Core.Remove-PSSnapin)

[Export-Console](xref:Microsoft.PowerShell.Core.Export-Console)

[Get-Command](xref:Microsoft.PowerShell.Core.Get-Command)

[about_Profiles](about_Profiles.md)

[about_Modules](about_Modules.md)

## <a name="keywords"></a>KEYWORDS

about_Snapins、about_Snap_ins、about_Snap
