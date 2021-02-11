---
description: コンピューターにリモート接続できるユーザーとそのユーザーが実行できるコマンドを決定するセッション構成について説明します。
Locale: en-US
ms.date: 12/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_session_configurations?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Session_Configurations
ms.openlocfilehash: 4c6d5494f49bc271a7fe128fbce7b27c9d1f675f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599547"
---
# <a name="about-session-configurations"></a>セッション構成について

## <a name="short-description"></a>概要
コンピューターにリモート接続できるユーザーとそのユーザーが実行できるコマンドを決定するセッション構成について説明します。

## <a name="long-description"></a>詳細説明

セッション構成は、"エンドポイント" とも呼ばれ、ローカルコンピューター上の設定のグループで、リモートユーザーまたはローカルユーザーがローカルコンピューター上の PowerShell に接続するときに作成される PowerShell セッションの環境を定義します。

コンピューターの管理者は、セッション構成を使用してコンピューターを保護し、コンピューターに接続するユーザーのカスタム環境を定義できます。

また、管理者は、セッション構成を使用して、リモートでコンピューターに接続するために必要なアクセス許可を決定することもできます。 既定では、管理者グループのメンバーのみが、セッション構成を使用してリモート接続するためのアクセス許可を持っていますが、既定の設定を変更して、すべてのユーザーまたは選択したユーザーがコンピューターにリモート接続できるようにすることができます。

PowerShell 3.0 以降では、セッション構成ファイルを使用して、セッション構成の要素を定義できます。 この機能を使用すると、コードを記述したり、セッション構成のプロパティを検出したりすることなく、簡単にセッションをカスタマイズできます。 セッション構成ファイルを作成するには、New-PSSessionConfiguration コマンドレットを使用します。 セッション構成ファイルの詳細については、「[about_Session_Configuration_Files](about_Session_Configuration_Files.md)」を参照してください。

セッション構成は、Web Services for Management (WS-MANAGEMENT) ベースの PowerShell リモート処理の機能です。 これらは、リモートコンピューターに接続するために、新しい PSSession、Invoke コマンド、または Enter-PSSession コマンドレットを使用する場合にのみ使用されます。

注: セッション構成を管理するには、[管理者として実行] オプションを使用して PowerShell を起動します。

セッション構成について

すべての PowerShell セッションは、セッション構成を使用します。 これには、New-PSSession または Enter-PSSession コマンドレットを使用して作成した永続的なセッションと、WS-MANAGEMENT ベースのリモート処理テクノロジを使用するコマンドレットの ComputerName パラメーターを使用するときに PowerShell で作成される一時的なセッション (Invoke コマンドなど) が含まれます。

管理者は、セッション構成を使用してコンピューターのリソースを保護し、コンピューターに接続するユーザーのカスタム環境を作成することができます。 たとえば、セッション構成を使用して、セッションでコンピューターが受信するオブジェクトのサイズを制限したり、セッションの言語モードを定義したり、セッションで使用できるコマンドレット、プロバイダー、および関数を指定したりすることができます。

セッション構成のセキュリティ記述子を構成することにより、セッション構成を使用してコンピューターに接続できるユーザーを決定します。
セッションで使用するには、ユーザーがセッション構成に対する Execute 権限を持っている必要があります。 コンピューター上の任意のセッション構成を使用するために必要なアクセス許可がユーザーにない場合、ユーザーはリモートでコンピューターに接続することはできません。

既定では、コンピューターの管理者のみが既定のセッション構成を使用するアクセス許可を持っています。 ただし、セキュリティ記述子を変更して、すべてのユーザーが使用できるようにするか、または選択したユーザーのみがコンピューター上でセッション構成を使用できるようにすることができます。

組み込みのセッション構成

PowerShell 3.0 には、Microsoft PowerShell および Microsoft. PowerShell. Workflow という名前の組み込みのセッション構成が含まれています。 Windows の64ビットバージョンを実行しているコンピューターでは、PowerShell は32ビットのセッション構成である Microsoft.powershell32 も提供します。

既定では、セッションのために使用されるのは、セッションを作成するコマンドに、新しい PSSession、Enter PSSession、または Invoke-Command コマンドレットの ConfigurationName パラメーターが含まれていない場合です。

既定のセッション構成のセキュリティ記述子を使用すると、ローカルコンピューター上の Administrators グループのメンバーだけがこれらを使用できます。 そのため、既定の設定を変更しない限り、Administrators グループのメンバーだけがリモートでコンピューターに接続できます。

既定のセッション構成を変更するには、$PSSessionConfigurationName ユーザー設定変数を使用します。 詳細については、「about_Preference_Variables」を参照してください。

ローカルコンピューター上のセッション構成を表示する

ローカルコンピューター上のセッション構成を取得するには、Get-PSSessionConfiguration コマンドレットを使用します。

たとえば、次のように入力します。

```powershell
PS C:> Get-PSSessionConfiguration | Format-List -Property Name, Permission

Name       : microsoft.powershell
Permission : BUILTIN\Administrators AccessAllowed

Name       : microsoft.powershell.workflow
Permission : BUILTIN\Administrators AccessAllowed

Name       : microsoft.powershell32
Permission : BUILTIN\Administrators AccessAllowed
```

セッション構成オブジェクトは、セッション構成ファイルを使用して構成されたセッション構成のプロパティを表示するために、PowerShell 3.0 で展開されます。

たとえば、セッション構成オブジェクトのすべてのプロパティを表示するには、次のように入力します。

```powershell
PS C:> Get-PSSessionConfiguration | Format-List -Property *
```

PowerShell で WSMan プロバイダーを使用して、セッション構成を表示することもできます。 WSMan プロバイダーは、セッションで WSMAN: ドライブを作成します。

WSMAN: ドライブでは、セッション構成はプラグインノードにあります。 (すべてのセッション構成はプラグインノードに含まれていますが、セッション構成ではないプラグインノードに項目があります)。

たとえば、ローカルコンピューター上のセッション構成を表示するには、次のように入力します。

```powershell
PS C:> dir wsman:\localhost\plugin\microsoft*

WSManConfig: Microsoft.WSMan.Management\WSMan::localhost\Plugin

Type       Keys                              Name
----       ----                              ----
Container  {Name=microsoft.powershell}       microsoft.powershell
Container  {Name=microsoft.powershell.wor... microsoft.powershell.workflow
Container  {Name=microsoft.powershell32}     microsoft.powershell32
```

リモートコンピューター上のセッション構成の表示

リモートコンピューター上のセッション構成を表示するには、Connect-WSMan コマンドレットを使用して、リモートコンピューターのメモをローカルコンピューターの WSMAN: ドライブに追加し、WSMAN: ドライブを使用してセッション構成を表示します。

たとえば、次のコマンドは、Server01 リモートコンピューターのノードをローカルコンピューターの WSMAN: ドライブに追加します。

```powershell
PS C:> Connect-WSMan server01.corp.fabrikam.com
```

コマンドが完了したら、Server01 コンピューターのノードに移動して、セッション構成を表示できます。

次に例を示します。

```powershell
PS C:> cd wsman:

PS WSMan:> dir

ComputerName                                  Type
------------                                  ----
localhost                                     Container
server01.corp.fabrikam.com                    Container

PS WSMan:> dir server01\plugin\

WSManConfig: Microsoft.WSMan.Management\WSMan::server01.corp.fabrikam.com\Pl
ugin

Type       Keys                              Name
----       ----                              ----
Container  {Name=microsoft.powershell}       microsoft.powershell
Container  {Name=microsoft.powershell.wor... microsoft.powershell.workflow
Container  {Name=microsoft.powershell32}     microsoft.powershell32
```

セッション構成のセキュリティ記述子の変更

Windows server 2012 および windows Server の新しいリリースでは、組み込みのセッション構成がリモートユーザーに対して既定で有効になっています。 サポートされているその他のバージョンの Windows では、リモートアクセスを許可するようにセッション構成のセキュリティ記述子を変更する必要があります。

コンピューター上のセッション構成へのリモートアクセスを有効にするには、Enable-PSRemoting コマンドレットを使用します。 このコマンドレットは、次の2つのセッション構成を作成します。

- 名前を "PowerShell" に定義します。 + "現在の PowerShell バージョン"
- 名前が "PowerShell. 6" で、特定の PowerShell バージョンには関連付けられていません。

また、既定では、コンピューターの Administrators グループのメンバーだけが既定のセッション構成に対する実行アクセス許可を持っていますが、既定のセッション構成および作成したセッション構成のセキュリティ記述子を変更できます。

コンピューターにリモートで接続するアクセス許可を他のユーザーに付与するには、Set-PSSessionConfiguration コマンドレットを使用して、Microsoft.powershell32 セッション構成のセキュリティ記述子にそれらのユーザーの "実行" アクセス許可を追加します。

たとえば、次のコマンドを実行すると、既定の PowerShell セッション構成のセキュリティ記述子を変更できるプロパティページが開きます。

```powershell
Set-PSSessionConfiguration -name Microsoft.PowerShell `
  -ShowSecurityDescriptorUI
```

コンピューター上のすべてのセッション構成に対するすべてのアクセス許可を拒否するには、Disable-PSSessionConfiguration コマンドレットを使用します。 たとえば、次のコマンドは、コンピューター上の既定のセッション構成を無効にします。

```powershell
PS C:> Disable-PSSessionConfiguration -Name Microsoft.PowerShell
```

リモートユーザーがコンピューターに接続できないようにし、ローカルユーザーが接続できるようにするには、Disable-PSRemoting コマンドレットを使用します。 Disable-PSRemoting は、コンピューター上のすべてのセッション構成に "Network_Deny_All" エントリを追加します。

```powershell
PS C:> Disable-PSRemoting
```

リモートユーザーがコンピューター上のすべてのセッション構成を使用できるようにするには、Enable-PSRemoting または Enable-PSSessionConfiguration コマンドレットを使用します。 たとえば、次のコマンドは、組み込みのセッション構成へのリモートアクセスを可能にします。

```powershell
PS C:> Enable-PSSessionConfiguration -name Microsoft.Power*
```

セッション構成のセキュリティ記述子にその他の変更を加えるには、Set-PSSessionConfiguration コマンドレットを使用します。 SDDL 文字列値を送信するには、Security記述子 Sddl パラメーターを使用します。 Showsecurity記述子 Ui パラメーターを使用して、新しい SDDL の作成に役立つユーザーインターフェイスプロパティシートを表示します。

次に例を示します。

```powershell
Set-PSSessionConfiguration -Name Microsoft.PowerShell `
  -ShowSecurityDescriptorUI
```

新しいセッション構成の作成

ローカルコンピューターで新しいセッション構成を作成するには、Register-PSSessionConfiguration コマンドレットを使用します。 新しいセッション構成を定義するには、C# アセンブリ、PowerShell スクリプト、および Register-PSSessionConfiguration コマンドレットのパラメーターを使用します。

たとえば、次のコマンドは、リモートコマンドから受信したデータを20メガバイト (MB) に制限する点を除いて、Microsoft PowerShell セッション構成と同じセッション構成を作成します。 (既定値は 50 MB)。

```powershell
Register-PSSessionConfiguration -Name NewConfig `
  -MaximumReceivedDataSizePerCommandMB 20
```

セッション構成を作成する場合は、他のセッション構成コマンドレットを使用して管理でき、WSMAN: ドライブに表示されます。

詳細については、「Register-pssessionconfiguration」を参照してください。

セッション構成の削除

セッション構成をローカルコンピューターから削除するには、Unregister-PSSessionConfiguration コマンドレットを使用します。 たとえば、次のコマンドは、NewConfig セッション構成をコンピューターから削除します。

```powershell
PS C:> Unregister-PSSessionConfiguration -Name NewConfig
```

詳細については、「Register-pssessionconfiguration」を参照してください。

セッション構成の復元

誤って削除 (登録解除) された既定のセッション構成を復元するには、Enable-PSRemoting コマンドレットを使用します。

Enable-PSRemoting コマンドレットは、コンピューター上に存在しないすべての既定のセッション構成を再作成します。 既存のセッション構成のプロパティ値を上書きしたり変更したりすることはありません。

既定のセッション構成の元のプロパティ値を復元するには、Unregister-PSSessionConfiguration を使用してセッション構成を削除し、Enable-PSRemoting コマンドレットを使用してその構成を再作成します。

セッション構成の選択

セッションの特定のセッション構成を選択するには、新しい PSSession、Enter-PSSession、または Invoke コマンドの ConfigurationName パラメーターを使用します。

たとえば、次のコマンドは、New-PSSession コマンドレットを使用して、Server01 コンピューター上の PSSession を開始します。 このコマンドは、ConfigurationName パラメーターを使用して、Server01 コンピューターで WithProfile 構成を選択します。

```powershell
PS C:> New-PSSession -ComputerName Server01 -ConfigurationName WithProfile
```

このコマンドは、現在のユーザーに WithProfile セッション構成を使用するアクセス許可がある場合、または必要なアクセス許可を持つユーザーの資格情報を指定できる場合にのみ成功します。

また、$PSSessionConfigurationName ユーザー設定変数を使用して、コンピューター上の既定のセッション構成を変更することもできます。 $PSSessionConfigurationName ユーザー設定変数の詳細については、「about_Preference_Variables」を参照してください。

## <a name="keywords"></a>KEYWORDS

about_Endpoints about_SessionConfigurations

## <a name="see-also"></a>関連項目

[about_Preference_Variables](about_Preference_Variables.md)

[about_PSSessions](about_PSSessions.md)

[about_Remote](about_Remote.md)

[about_Session_Configuration_Files](about_Session_Configuration_Files.md)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

[Disable-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Disable-PSSessionConfiguration)

[Enable-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Enable-PSSessionConfiguration)

[Get-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Get-PSSessionConfiguration)

[New-PSSessionConfigurationFile](xref:Microsoft.PowerShell.Core.New-PSSessionConfigurationFile)

[Register-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Register-PSSessionConfiguration)

[Set-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Set-PSSessionConfiguration)

[Test-PSSessionConfigurationFile](xref:Microsoft.PowerShell.Core.Test-PSSessionConfigurationFile)

[Unregister-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Unregister-PSSessionConfiguration)

