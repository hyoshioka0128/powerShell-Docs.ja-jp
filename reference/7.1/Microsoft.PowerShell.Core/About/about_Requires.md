---
description: 必須の要素を使用せずにスクリプトを実行しないようにします。
Locale: en-US
ms.date: 12/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_requires?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Requires
ms.openlocfilehash: af7b557e399385589f3ddbbeb6b1f514c0f550f5
ms.sourcegitcommit: 9a86cac80402d8193147058d4ba50e07b26059dd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/15/2020
ms.locfileid: "97490494"
---
# <a name="about-requires"></a>要求について

## <a name="short-description"></a>簡単な説明
必須の要素を使用せずにスクリプトを実行しないようにします。

## <a name="long-description"></a>長い説明

ステートメントは、 `#Requires` PowerShell のバージョン、モジュール (およびバージョン)、スナップイン (およびバージョン)、およびエディションの前提条件が満たされていない限り、スクリプトを実行しないようにします。 前提条件が満たされていない場合、PowerShell はスクリプトを実行しません。

### <a name="syntax"></a>構文

```
#Requires -Version <N>[.<n>]
#Requires -PSSnapin <PSSnapin-Name> [-Version <N>[.<n>]]
#Requires -Modules { <Module-Name> | <Hashtable> }
#Requires -PSEdition <PSEdition-Name>
#Requires -ShellId <ShellId> -PSSnapin <PSSnapin-Name> [-Version <N>[.<n>]]
#Requires -RunAsAdministrator
```

構文の詳細については、「 [Scriptrequirements 要件](/dotnet/api/system.management.automation.language.scriptrequirements)」を参照してください。

### <a name="rules-for-use"></a>使用するための規則

1つのスクリプトに複数のステートメントを含めることができ `#Requires` ます。 ステートメントは、 `#Requires` スクリプト内の任意の行に記述できます。

ステートメントを `#Requires` 関数内に配置しても、そのスコープは制限されません。 `#Requires`スクリプトを実行する前に、すべてのステートメントがグローバルに適用され、満たされている必要があります。

> [!WARNING]
> ステートメントは、 `#Requires` スクリプト内のどの行にも記述できますが、スクリプト内のその位置はアプリケーションのシーケンスに影響しません。 スクリプトを実行する前に、ステートメントが示すグローバル状態を `#Requires` 満たす必要があります。

例:

```powershell
Get-Module AzureRM.Netcore | Remove-Module
#Requires -Modules AzureRM.Netcore
```

必要なモジュールがステートメントの前に削除されたため、上記のコードが実行されない可能性があり `#Requires` ます。 ただし、 `#Requires` スクリプトが実行される前に、状態が満たされている必要がありました。 次に、スクリプトの最初の行で必要な状態が無効になります。

### <a name="parameters"></a>パラメーター

#### <a name="-assembly-assembly-path--net-assembly-specification"></a>-Assembly \<Assembly path> |\<.NET assembly specification>

> [!IMPORTANT]
> `-Assembly`構文は非推奨とされます。 関数は機能しません。 構文は PowerShell 5.1 で追加されましたが、サポートコードが実装されていませんでした。 構文は、旧バージョンとの互換性のために引き続き受け入れられます。

アセンブリ DLL ファイルまたは .NET アセンブリ名へのパスを指定します。 **Assembly** パラメーターは、PowerShell 5.0 で導入されました。 .NET アセンブリの詳細については、「 [アセンブリ名](/dotnet/standard/assembly/names)」を参照してください。

以下に例を示します。

```
#Requires -Assembly path\to\foo.dll
```

```
#Requires -Assembly "System.Management.Automation, Version=3.0.0.0,
  Culture=neutral, PublicKeyToken=31bf3856ad364e35"
```

#### <a name="-version-nn"></a>-Version \<N\> [. \<n\> ]

スクリプトに必要な PowerShell の最小バージョンを指定します。 メジャーバージョン番号と省略可能なマイナーバージョン番号を入力します。

以下に例を示します。

```powershell
#Requires -Version 6.0
```

#### <a name="-pssnapin-pssnapin-name--version-nn"></a>-Add-pssnapin \<PSSnapin-Name\> [-Version \<N\> [. \<n\> ]]

スクリプトに必要な PowerShell スナップインを指定します。 スナップイン名とオプションのバージョン番号を入力します。

以下に例を示します。

```powershell
#Requires -PSSnapin DiskSnapin -Version 1.2
```

#### <a name="-modules-module-name--hashtable"></a>-モジュール \<Module-Name\> |\<Hashtable\>

スクリプトに必要な PowerShell モジュールを指定します。 モジュール名とオプションのバージョン番号を入力します。

必要なモジュールが現在のセッションにない場合は、PowerShell によってインポートされます。
モジュールをインポートできない場合、PowerShell は終了エラーをスローします。

モジュールごとに、モジュール名 ( \<String\> ) またはハッシュテーブルを入力します。 値には、文字列とハッシュテーブルの組み合わせを指定できます。 ハッシュテーブルには、次のキーがあります。

- `ModuleName` - **必須** モジュール名を指定します。
- `GUID` - **省略可能** モジュールの GUID を指定します。
- 以下の3つのキーのいずれかを指定する **必要** もあります。 これらのキーを一緒に使用することはできません。
  - `ModuleVersion` -モジュールの許容される最小バージョンを指定します。
  - `RequiredVersion` -モジュールの正確な必須バージョンを指定します。
  - `MaximumVersion` -モジュールの許容される最大バージョンを指定します。

> [!NOTE]
> `RequiredVersion` は Windows PowerShell 5.0 で追加されました。
> `MaximumVersion` は Windows PowerShell 5.1 で追加されました。

以下に例を示します。

`AzureRM.Netcore`(バージョン `0.12.0` 以上) がインストールされている必要があります。

```powershell
#Requires -Modules @{ ModuleName="AzureRM.Netcore"; ModuleVersion="0.12.0" }
```

`AzureRM.Netcore`(バージョン **のみ** `0.12.0` ) がインストールされている必要があります。

```powershell
#Requires -Modules @{ ModuleName="AzureRM.Netcore"; RequiredVersion="0.12.0" }
```

`AzureRM.Netcore`(バージョン `0.12.0` 以降) がインストールされている必要があります。

```powershell
#Requires -Modules @{ ModuleName="AzureRM.Netcore"; MaximumVersion="0.12.0" }
```

およびのすべてのバージョンがインストールされている必要があり `AzureRM.Netcore` `PowerShellGet` ます。

```powershell
#Requires -Modules AzureRM.Netcore, PowerShellGet
```

キーを使用する場合は `RequiredVersion` 、バージョン文字列が必要なバージョン文字列と完全に一致していることを確認します。

```powershell
Get-Module AzureRM.Netcore -ListAvailable
```

```Output
    Directory: /home/azureuser/.local/share/powershell/Modules

ModuleType Version Name            PSEdition ExportedCommands
---------- ------- ----            --------- ----------------
Script     0.12.0  AzureRM.Netcore Core
```

次の例は、 **0.12** が厳密には **0.12.0** と一致しないため、失敗します。

```powershell
#Requires -Modules @{ ModuleName="AzureRM.Netcore"; RequiredVersion="0.12" }
```

#### <a name="-psedition-psedition-name"></a>-PSEdition \<PSEdition-Name\>

スクリプトに必要な PowerShell エディションを指定します。 有効な値は、PowerShell Core の **コア** と Windows powershell 用の **デスクトップ** です。

以下に例を示します。

```powershell
#Requires -PSEdition Core
```

#### <a name="-shellid"></a>-ShellId

スクリプトに必要なシェルを指定します。 シェル ID を入力します。 **ShellId** パラメーターを使用する場合は、 **add-pssnapin** パラメーターも含める必要があります。
自動変数に対してクエリを実行することで、現在の **ShellId** を見つけることができ `$ShellId` ます。

以下に例を示します。

```powershell
#Requires -ShellId MyLocalShell -PSSnapin Microsoft.PowerShell.Core
```

> [!NOTE]
> このパラメーターは、非推奨とされたミニシェルで使用することを目的としています。

#### <a name="-runasadministrator"></a>-RunAsAdministrator

このスイッチパラメーターをステートメントに追加すると、スクリプトを実行している `#Requires` PowerShell セッションを管理者特権で起動する必要があることが指定されます。 Windows 以外のオペレーティングシステムでは、 **RunAsAdministrator** パラメーターは無視されます。 **RunAsAdministrator** パラメーターは、PowerShell 4.0 で導入されました。

次に例を示します。

```powershell
#Requires -RunAsAdministrator
```

### <a name="examples"></a>例

次のスクリプトには2つのステートメントがあり `#Requires` ます。 両方のステートメントで指定された要件を満たしていない場合、スクリプトは実行されません。 各 `#Requires` ステートメントは、行の最初の項目である必要があります。

```powershell
#Requires -Modules AzureRM.Netcore
#Requires -Version 6.0
Param
(
    [parameter(Mandatory=$true)]
    [String[]]
    $Path
)
...
```

## <a name="see-also"></a>関連項目

[about_Automatic_Variables](about_Automatic_Variables.md)

[about_Language_Keywords](about_Language_Keywords.md)
