---
description: PowerShell のグループポリシー設定について説明します。
Locale: en-US
ms.date: 03/25/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_group_policy_settings?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Group_Policy_Settings
ms.openlocfilehash: 5ca13d22c69213dd8bae57a0dd08587c9c2b1541
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599905"
---
# <a name="about-group-policy-settings"></a>グループポリシー設定について

## <a name="short-description"></a>簡単な説明
PowerShell のグループポリシー設定について説明します。

## <a name="long-description"></a>長い説明

PowerShell には、エンタープライズ環境で Windows コンピューターの一貫した構成値を定義するのに役立つグループポリシー設定が含まれています。

PowerShell のグループポリシー設定は、次のグループポリシーパスにあります。

```
Computer Configuration\
  Administrative Templates\
    PowerShell Core

User Configuration\
  Administrative Templates\
    PowerShell Core
```

ユーザー構成パスのグループポリシー設定は、コンピューターの構成パスのグループポリシー設定よりも優先されます。

PowerShell 7 では、グループ ポリシーのテンプレートとインストール スクリプトが `$PSHOME` に含まれています。

グループ ポリシー ツールでは、管理用テンプレート ファイル (`.admx`、`.adml`) を使用して、ユーザー インターフェイスにポリシー設定を追加できます。 これにより、管理者はレジストリ ベースのポリシー設定を管理できます。 このスクリプトにより、 `InstallPSCorePolicyDefinitions.ps1` **PowerShell Core 管理用テンプレート** がローカルコンピューターにインストールされます。

```powershell
Get-ChildItem -Path $PSHOME -Filter *Core*Policy*
```

```Output
    Directory: C:\Program Files\PowerShell\7

Mode       LastWriteTime         Length Name
----       -------------         ------ ----
-a---      2/27/2020 12:38 AM     15861 InstallPSCorePolicyDefinitions.ps1
-a---      2/27/2020 12:28 AM      9675 PowerShellCoreExecutionPolicy.adml
-a---      2/27/2020 12:28 AM      6201 PowerShellCoreExecutionPolicy.admx
```

テンプレートをインストールしたら、グループポリシーエディター () でこれらの設定を編集でき `gpedit.msc` ます。

ポリシーは次のとおりです。

- コンソール セッションの構成:PowerShell を実行する構成エンドポイントを設定します。
- モジュールのログ記録を有効にする: モジュールの **Logpipelineexecutiondetails** プロパティを設定します。
- PowerShell スクリプト ブロックのログ記録を有効にする:すべての PowerShell スクリプトの詳細なログ記録を有効にします。
- スクリプトの実行を有効にする:PowerShell 実行ポリシーを設定します。
- PowerShell トランスクリプションを有効にする: PowerShell コマンドの入出力をテキスト ベースのトランスクリプトにキャプチャできるようにします。
- の既定のソースパスを設定する `Update-Help` : 更新可能なヘルプのソースを、インターネットではなくディレクトリに設定します。

各 PowerShell グループポリシー設定には、次のグループポリシーパスにある同様の Windows PowerShell グループポリシー設定の値を使用するオプション ([Windows PowerShell ポリシー設定を使用する] フィールド) があります。

```
Computer Configuration\
  Administrative Templates\
    Windows Components\
      Windows PowerShell

User Configuration\
  Administrative Templates\
    Windows Components\
      Windows PowerShell
```

> [!NOTE]
> これらの **Powershell Core 管理用テンプレート** には、Windows powershell の設定は含まれていません。 他のテンプレートを取得し、グループポリシーを構成する方法の詳細については、「 [Windows でグループポリシー管理用テンプレートの中央ストアを作成および管理する方法][gpstore]」を参照してください。

## <a name="console-session-configuration"></a>コンソールセッションの構成

**コンソールセッション構成** ポリシーの設定では、PowerShell を実行する構成エンドポイントを指定します。 これには、ローカルコンピューターに登録されている任意のエンドポイントを指定できます。これには、既定の PowerShell リモート処理エンドポイントや、特定のユーザーロール機能を持つカスタムエンドポイントが含まれます。

## <a name="turn-on-module-logging"></a>モジュールログをオンにする

[ **モジュールログをオンにする** ] ポリシー設定は、選択した PowerShell モジュールのログ記録を有効にします。 この設定は、影響を受けるすべてのコンピューター上のすべてのセッションで有効になります。

このポリシー設定を有効にして1つ以上のモジュールを指定した場合、指定されたモジュールのパイプライン実行イベントは、イベントビューアーの Windows PowerShell ログに記録されます。

このポリシー設定を無効にした場合、すべての PowerShell モジュールに対して実行イベントのログ記録が無効になります。

このポリシー設定が構成されていない場合、各モジュールの **Logpipelineexecutiondetails** プロパティによって、モジュールの実行イベントがログに記録されるかどうかが決まります。 既定では、すべてのモジュールの **Logpipelineexecutiondetails** プロパティは False に設定されています。

モジュールのモジュールログをオンにするには、次のコマンド形式を使用します。 モジュールをセッションにインポートする必要があります。この設定は、現在のセッションでのみ有効です。

```powershell
Import-Module <Module-Name>
(Get-Module <Module-Name>).LogPipelineExecutionDetails = $true
```

特定のコンピューター上のすべてのセッションのモジュールログをオンにするには、前のコマンドを "すべてのユーザー" PowerShell プロファイル () に追加し `$Profile.AllUsersAllHosts` ます。

モジュールログの詳細については、「 [about_Modules](about_Modules.md)」を参照してください。

## <a name="turn-on-powershell-script-block-logging"></a>PowerShell スクリプトブロックのログ記録を有効にする

**[Powershell スクリプトブロックのログ記録を有効に** する] ポリシー設定を使用すると、powershell スクリプトのすべての入力を、Microsoft-Windows Powershell/Operational イベントログに記録できます。 このポリシー設定を有効にした場合、PowerShell Core は、対話形式で起動したか、またはオートメーションを使用して、コマンド、スクリプトブロック、関数、およびスクリプトの処理をログに記録します。

このポリシー設定を無効にすると、PowerShell スクリプトの入力のログ記録は無効になります。 スクリプト ブロックの呼び出しのログ記録を有効にすると、PowerShell はさらに、コマンド、スクリプト ブロック、関数、またはスクリプトの呼び出しの開始時または停止時にイベントを記録します。 呼び出しのログ記録を有効にすると、大量のイベント ログが生成されます。

## <a name="turn-on-script-execution"></a>スクリプトの実行を有効にする

[ **スクリプトの実行を有効に** する] ポリシー設定では、コンピューターとユーザーの実行ポリシーを設定します。これにより、実行が許可されるスクリプトが決まります。

ポリシー設定を有効にした場合は、次のポリシー設定の中から選択できます。

- **署名済みスクリプトのみを許可** するスクリプトは、信頼された発行元によって署名されている場合にのみ実行できます。 このポリシー設定は、AllSigned 実行ポリシーと同じです。

- **ローカルスクリプトおよびリモートの署名済みスクリプトを許可すると、** すべてのローカルスクリプトを実行できるようになります。 インターネットからのスクリプトは、信頼された発行元によって署名されている必要があります。 このポリシー設定は、RemoteSigned 実行ポリシーと同じです。

- **すべてのスクリプトを許可** するすべてのスクリプトの実行を許可します。 このポリシー設定は、無制限の実行ポリシーに相当します。

このポリシー設定を無効にした場合、スクリプトの実行は一切許可されません。 このポリシー設定は、制限された実行ポリシーに相当します。

このポリシー設定を無効にした場合、または構成しなかった場合、コマンドレットによってコンピューターまたはユーザーに設定されている実行ポリシーによって、 `Set-ExecutionPolicy` スクリプトの実行が許可されているかどうかが決まります。 既定値は Restricted です。

詳細については、「[about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

## <a name="turn-on-powershell-transcription"></a>Powershell の議事録を有効にする

**[Powershell のトランスクリプトを有効に** する] ポリシー設定を使用すると、powershell Core コマンドの入力と出力をテキストベースのトランスクリプトにキャプチャできます。 このポリシー設定を有効にした場合、powershell core と powershell core エンジンを利用するその他のアプリケーションの議事録ログが有効になります。 既定では、PowerShell Core は各ユーザーの My Documents ディレクトリにトランスクリプト出力を記録します。ファイル名には ' PowerShell_transcript ' が含まれ、コンピューター名と時刻が開始されます。
このポリシーを有効にすることは、各 PowerShell コアセッションで Start-Transcript コマンドレットを呼び出すことと同じです。

このポリシー設定を無効にした場合、PowerShell ベースのアプリケーションの議事録のログ記録は既定で無効になりますが、Start-Transcript コマンドレットを使用してトランスクリプトを有効にすることはできます。

OutputDirectory 設定を使用して、共有の場所への議事録ログ記録を有効にする場合は、そのディレクトリへのアクセスを制限して、他のユーザーやコンピューターのトランスクリプトをユーザーが表示できないようにしてください。

## <a name="set-the-default-source-path-for-update-help"></a>Update-Help の既定のソース パスを設定する

Update-help ポリシー設定 **の既定のソースパス** を設定すると、コマンドレットの **SourcePath** パラメーターの既定値が設定され `Update-Help` ます。
この設定は、ユーザーがコマンドレットを使用して `Update-Help` インターネットからヘルプファイルをダウンロードできないようにします。

> [!NOTE]
> このグループポリシー設定は、[ **コンピューターの構成** ] と [ユーザーの **構成**] の下に表示されます。 ただし、[ **コンピューターの構成** ] の下のグループポリシー設定のみが有効です。 [ **ユーザーの構成** ] の下のグループポリシー設定は無視されます。

`Update-Help`コマンドレットは、PowerShell モジュールの最新のヘルプファイルをダウンロードしてインストールし、コンピューターにインストールします。 既定では、は、 `Update-Help` モジュールによって指定されたインターネット上の場所から新しいヘルプファイルをダウンロードします。

ただし、コマンドレットを使用し `Save-Help` て、最新のヘルプファイルをネットワーク共有などのファイルシステムの場所にダウンロードしてから、コマンドレットを使用して `Update-Help` ファイルシステムの場所からヘルプファイルを取得し、コンピューターにインストールすることができます。 コマンドレットの **SourcePath** パラメーターは、 `Update-Help` ファイルシステムの場所を指定します。

**Sourcepath** パラメーターの既定値を指定すると、このグループポリシー設定によって、 **sourcepath** パラメーターがすべてのコマンドに暗黙的に追加され `Update-Help` ます。 ユーザーは、別のファイルシステムの場所を入力することによって、既定値として指定されている特定のファイルシステムの場所を上書きできます。
ただし、コマンドから **SourcePath** パラメーターを削除することはできません `Update-Help` 。

このポリシー設定を有効にした場合、 **SourcePath** パラメーターの既定値を指定できます。 ファイルシステムの場所を入力してください。

このポリシー設定を無効にした場合、または構成しなかった場合、コマンドレットの **SourcePath** パラメーターの既定値はありません `Update-Help` 。 ユーザーは、インターネットまたは任意のファイルシステムの場所からヘルプをダウンロードできます。

詳しくは、「[about_Updatable_Help](about_Updatable_Help.md)」をご覧ください。

## <a name="keywords"></a>Keywords

about_Group_Policies about_GroupPolicy

## <a name="see-also"></a>関連項目

[PowerShell Core ポリシー RFC](https://github.com/PowerShell/PowerShell-RFC/blob/master/4-Experimental-Accepted/RFC0041-Policy.md)

[about_Execution_Policies](about_Execution_Policies.md)

[about_Modules](about_Modules.md)

[about_Updatable_Help](about_Updatable_Help.md)

[Get-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Get-ExecutionPolicy)

[Set-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)

[Get-Module](xref:Microsoft.PowerShell.Core.Get-Module)

[Update-Help](xref:Microsoft.PowerShell.Core.Update-Help)

[Save-Help](xref:Microsoft.PowerShell.Core.Save-Help)

<!-- link references -->
[gpstore]: https://support.microsoft.com/help/3087759

