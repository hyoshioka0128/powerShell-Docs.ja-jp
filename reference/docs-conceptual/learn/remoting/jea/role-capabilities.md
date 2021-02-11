---
ms.date: 07/10/2019
keywords: JEA, PowerShell, セキュリティ
title: JEA ロール機能
description: ロール機能は、拡張子が .psrc の PowerShell データ ファイルであり、接続ユーザーが利用できるすべてのコマンドレット、関数、プロバイダー、外部プログラムが列挙されています。
ms.openlocfilehash: 233d9081f4a8f977f0959addb5573c4566f885d0
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92499996"
---
# <a name="jea-role-capabilities"></a>JEA ロール機能

JEA エンドポイントを作成するときに、JEA セッションでユーザーに許可する操作を説明するロール機能を 1 つ以上定義する必要があります。 ロール機能は、`.psrc` 拡張子を持つ PowerShell データ ファイルです。このファイルには、接続ユーザーが利用できるすべてのコマンドレット、関数、プロバイダー、外部プログラムが列挙されています。

## <a name="determine-which-commands-to-allow"></a>許可するコマンドを決定する

ロール機能ファイルを作成するときの最初の手順は、ユーザーが何にアクセスする必要があるかを検討することです。 要件を集めるこのプロセスには時間がかかることがありますが、重要なプロセスです。 ユーザーに使用を許可するコマンドや関数があまりにも少ないと、仕事ができなくなります。 ユーザーに使用を許可するコマンドや関数があまりにも多すぎると、本来意図した以上の操作が可能になることがあり、セキュリティ姿勢を弱めることになります。

このプロセスをどのように進めるかは、組織と目標によって決まります。 次のヒントが役に立つことがあります。

1. 仕事を行うためにユーザーが使用しているコマンドを **確認** します。 たとえば、IT スタッフに調査を行ったり、自動化スクリプトを調べたり、PowerShell セッションのトランスクリプトとログを分析したりします。
2. 可能な限り、コマンド ライン ツールで行っていたことを PowerShell の同じ操作に **更新** し、最良の監査と JEA カスタマイズを実現します。 外部プログラムは、JEA のネイティブ PowerShell コマンドレットまたは関数ほど細かに制約することはできません。
3. コマンドレットの範囲を **制限** し、特定のパラメーターまたはパラメーター値のみを許可します。 これは特に、ユーザーがシステムの一部のみを管理する場合に重要です。
4. カスタム関数を **作成** し、複雑なコマンドや JEA では制約が難しいコマンドの代わりに使用します。 複雑なコマンドをラップするか、追加の検証ロジックを適用する単純な関数を利用すれば、管理者やエンドユーザーにとって制御機能がさらに使いやすくなります。
5. ユーザーや自動化サービスに許可するコマンドの範囲を **テスト** し、必要に応じて調整します。

### <a name="examples-of-potentially-dangerous-commands"></a>危険性を含むコマンドの例

JEA エンドポイントでユーザーのアクセス許可の昇格が許可されないように、コマンドを慎重に選択することが重要です。

> [!IMPORTANT]
> JEA セッションでユーザーの successCommands に必要な重要な情報は、多くの場合、昇格された特権で実行されます。

次の表に、制約のない状態が許された場合、悪意で使用される可能性があるコマンドの例を示します。 このリストですべてを網羅しているわけではありません。最初に注意するべきものとして提示しているにすぎません。

|                                            リスク                                            |                                例                                |                                                                              関連するコマンド                                                                              |
| ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| JEA を迂回する管理者特権を接続ユーザーに与える                                | `Add-LocalGroupMember -Member 'CONTOSO\jdoe' -Group 'Administrators'` | `Add-ADGroupMember`, `Add-LocalGroupMember`, `net.exe`, `dsadd.exe`                                                                                                        |
| マルウェア、エクスプロイト、防御を迂回するカスタム スクリプトなど、任意のコードを実行する | `Start-Process -FilePath '\\san\share\malware.exe'`                   | `Start-Process`, `New-Service`, `Invoke-Item`, `Invoke-WmiMethod`, `Invoke-CimMethod`, `Invoke-Expression`, `Invoke-Command`, `New-ScheduledTask`, `Register-ScheduledJob` |

## <a name="create-a-role-capability-file"></a>ロール機能ファイルを作成する

[New-PSRoleCapabilityFile](/powershell/module/microsoft.powershell.core/new-psrolecapabilityfile) コマンドレットで新しい PowerShell ロール機能ファイルを作成できます。

```powershell
New-PSRoleCapabilityFile -Path .\MyFirstJEARole.psrc
```

作成したロール機能ファイルを編集して、ロールに必要なコマンドを許可する必要があります。 PowerShell ヘルプ ドキュメントには、ファイルの構成例が含まれています。

### <a name="allowing-powershell-cmdlets-and-functions"></a>PowerShell のコマンドレットと関数を許可する

PowerShell コマンドレットまたは関数の実行権限をユーザーに与えるには、VisibleCmdlets または VisibleFunctions フィールドにコマンドレットまたは関数の名前を追加します。 コマンドがコマンドレットであるか、関数であるかわからない場合、`Get-Command <name>` を実行し、出力の **CommandType** プロパティを確認してください。

```powershell
VisibleCmdlets = 'Restart-Computer', 'Get-NetIPAddress'
```

特定のコマンドレットまたは関数の範囲が必要以上に広いことがあります。 たとえば、DNS 管理者の場合、DNS サービスを再起動する許可だけが必要になります。 マルチテナント環境では、テナントにセルフサービス管理ツールがあります。 テナントが管理できるリソースは、そのテナントのリソースに限定する必要があります。 そのような場合、コマンドレットまたは関数から公開するパラメーターを制限できます。

```powershell
VisibleCmdlets = @{ Name = 'Restart-Computer'; Parameters = @{ Name = 'Name' }}
```

さらに進んだシナリオでは、ユーザーがこれらのパラメーターに使用できる値も制限する必要があります。 ロール機能では、許可する入力を決定する一連の値または正規表現パターンを定義できます。

```powershell
VisibleCmdlets = @{ Name = 'Restart-Service'; Parameters = @{ Name = 'Name'; ValidateSet = 'Dns', 'Spooler' }},
                 @{ Name = 'Start-Website'; Parameters = @{ Name = 'Name'; ValidatePattern = 'HR_*' }}
```

> [!NOTE]
> 利用できるパラメーターを制限した場合でも、[一般的な PowerShell パラメーター](/powershell/module/microsoft.powershell.core/about/about_commonparameters)は常に許可されます。
> パラメーター フィールドで明示的にリストアップしないでください。

下の表は、表示されるコマンドレットまたは関数をカスタマイズするさまざまな方法をまとめたものです。
**VisibleCmdlets** フィールドで下のあらゆる要素を組み合わせることができます。

|                                           例                                           |                                                             使用事例                                                              |
| ------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `'My-Func'` または `@{ Name = 'My-Func' }`                                                      | パラメーターに制限を適用せずに `My-Func` を実行することをユーザーに許可します。                                                      |
| `'MyModule\My-Func'`                                                                        | パラメーターに制限を適用せずにモジュール `MyModule` から `My-Func` を実行することをユーザーに許可します。                           |
| `'My-*'`                                                                                    | 動詞 `My` でコマンドレットまたは関数を実行することをユーザーに許可します。                                                                 |
| `'*-Func'`                                                                                  | 名詞 `Func` でコマンドレットまたは関数を実行することをユーザーに許可します。                                                               |
| `@{ Name = 'My-Func'; Parameters = @{ Name = 'Param1'}, @{ Name = 'Param2' }}`              | `Param1` パラメーターと `Param2` パラメーターを指定して `My-Func` を実行することをユーザーに許可します。 あらゆる値をパラメーターに指定できます。          |
| `@{ Name = 'My-Func'; Parameters = @{ Name = 'Param1'; ValidateSet = 'Value1', 'Value2' }}` | `Param1` パラメーターを指定して `My-Func` を実行することをユーザーに許可します。 "Value1" と "Value2" のみをパラメーターに指定できます。        |
| `@{ Name = 'My-Func'; Parameters = @{ Name = 'Param1'; ValidatePattern = 'contoso.*' }}`    | `Param1` パラメーターを指定して `My-Func` を実行することをユーザーに許可します。 "contoso" で始まる値をパラメーターに指定できます。 |

> [!WARNING]
> セキュリティのベスト プラクティスとしては、表示されるコマンドレットまたは関数を定義するときは、ワイルドカードの使用は推奨されません。 代わりに、命名規則が同じ他のコマンドが意図せずに許可されるような状況を回避するために、問題のないコマンドを 1 つずつ明示的にリストアップしてください。

**ValidatePattern** と **ValidateSet** の両方を同じコマンドレットまたは関数に適用することはできません。

適用すると、**ValidatePattern** によって **ValidateSet** がオーバーライドされます。

**ValidatePattern** の詳細については、[この「*Hey, Scripting Guy!* 」投稿](https://devblogs.microsoft.com/scripting/validate-powershell-parameters-before-running-the-script/)と [PowerShell 正規表現](/powershell/module/microsoft.powershell.core/about/about_regular_expressions)参照コンテンツをご覧ください。

### <a name="allowing-external-commands-and-powershell-scripts"></a>外部コマンドと PowerShell スクリプトを許可する

JEA セッションで実行可能ファイルと PowerShell スクリプト (.ps1) を実行することをユーザーに許可するには、**VisibleExternalCommands** フィールドに各プログラムの完全パスを追加する必要があります。

```powershell
VisibleExternalCommands = 'C:\Windows\System32\whoami.exe', 'C:\Program Files\Contoso\Scripts\UpdateITSoftware.ps1'
```

可能であれば、PowerShell コマンドレットおよび関数で許可されたパラメーターを制御できるため、権限を与える外部実行可能ファイルに対して相当する PowerShell コマンドレットまたは関数を使用します。

実行可能ファイルの多くでは、さまざまなパラメーターを指定することで、現在の状態を読み取ってからそれを変更することができます。

たとえば、システムでホストされているネットワーク共有を管理するファイル サーバー管理者ロールを考えてみてください。 共有の管理方法の 1 つは、`net share` を使用することです。 しかし、**net.exe** を利用すると、`net group Administrators unprivilegedjeauser /add` の管理者特権が簡単に得られる可能性があるため、このコマンドを許可するのは危険です。 より安全な方法は、[Get-SmbShare](/powershell/module/smbshare/get-smbshare) を許可することです。これで同じ結果が得られますが、範囲がはるかに限られます。

JEA セッションで外部コマンドの利用をユーザーに許可するときは、実行可能ファイルの完全パスを常に指定してください。 これによりシステム上のどこかに置かれている、同じような名前の悪意のあるプログラムが実行されるのを防止します。

### <a name="allowing-access-to-powershell-providers"></a>PowerShell プロバイダーへのアクセスを許可する

既定で、JEA セッションでは PowerShell プロバイダーを利用できません。 これにより機密情報や構成設定が接続ユーザーに公開されるリスクが軽減されます。

必要なときに、`VisibleProviders` コマンドを利用して PowerShell プロバイダーへのアクセスを許可できます。 プロバイダーの完全一覧が必要な場合、`Get-PSProvider` を実行してください。

```powershell
VisibleProviders = 'Registry'
```

ファイル システム、レジストリ、証明書ストア、その他の機密プロバイダーにアクセスする必要がある単純な作業に関しては、ユーザーの代わりにプロバイダーとやりとりするカスタム関数の記述を検討してください。 JEA セッションで利用できる関数、コマンドレット、外部プログラムは、JEA と同じ制約の対象になりません。 既定では、これらはあらゆるプロバイダーにアクセスできます。 JEA エンドポイントとの間でファイルのコピーが必要な場合、[ユーザー ドライブ](session-configurations.md#user-drive)の利用も検討してください。

### <a name="creating-custom-functions"></a>カスタム関数を作成する

ロール機能ファイルでカスタム関数を作成し、エンド ユーザーのために複雑なタスクを簡単にできます。 カスタム関数は、コマンドレット パラメーター値に高度な検証ロジックが必要なときにも便利です。 **FunctionDefinitions** で次のように単純な関数を記述できます。

```powershell
VisibleFunctions = 'Get-TopProcess'

FunctionDefinitions = @{
    Name = 'Get-TopProcess'

    ScriptBlock = {
        param($Count = 10)

        Get-Process | Sort-Object -Property CPU -Descending |
            Microsoft.PowerShell.Utility\Select-Object -First $Count
    }
}
```

> [!IMPORTANT]
> JEA ユーザーが実行できるように、**VisibleFunctions** フィールドにカスタム関数の名前を必ず追加してください。

カスタム関数の本文 (スクリプト ブロック) はシステムの既定の言語モードで実行され、JEA の言語制約の対象になりません。 つまり、関数はファイル システムとレジストリにアクセスし、ロール機能ファイルに表示されないコマンドを実行できます。 パラメーターを使用するときは、任意のコードを実行しないように注意してください。 `Invoke-Expression` のようなコマンドレットにユーザー入力がパイプ処理されないようにします。

上記の例では、簡潔な `Select-Object` ではなく、完全修飾モジュール名 (FQMN) `Microsoft.PowerShell.Utility\Select-Object` が使用されていることに注目してください。
ロール機能ファイルに定義されている関数は、JEA セッションの範囲に入ります。この範囲には、JEA で作成されるプロキシ関数が含まれ、既存のコマンドが制約されます。

既定では、`Select-Object` は、オブジェクト上で任意プロパティを選択することができない、すべての JEA セッションの制約付きコマンドレットです。 関数で制約なしの `Select-Object` を使用するには、FQMN を使用して、完全な実装を明示的に要求する必要があります。 JEA セッションの制約付きコマンドレットはすべて、関数から呼び出されるときに同じ制約があります。 詳細については、「[about_Command_Precedence](/powershell/module/microsoft.powershell.core/about/about_command_precedence)」(コマンドの優先順位について) を参照してください。

複数のカスタム関数を記述する場合、それらを PowerShell スクリプト モジュールに入れると利便性が高まります。 組み込みやサードパーティのモジュールの場合のように、**VisibleFunctions** フィールドを利用し、JEA セッションでこれらの関数を表示させることができます。

JEA セッションでタブ補完が正しく機能するためには、**VisibleFunctions** リストに組み込み関数 `tabexpansion2` を含める必要があります。

## <a name="make-the-role-capabilities-available-to-a-configuration"></a>構成でロール機能を使用できるようにする

PowerShell 6 より前の場合、PowerShell でロール機能ファイルを検出できるようにするには、ファイルを PowerShell モジュールの **RoleCapabilities** フォルダーに保存する必要があります。 モジュールは `$env:PSModulePath` 環境変数に含まれるあらゆるフォルダーに保存できますが、`$env:SystemRoot\System32` や、信頼されていないユーザーがファイルを変更できる可能性があるフォルダーには配置しないでください。

次の例では、ロール機能ファイルをホストするための **ContosoJEA** という名前の PowerShell スクリプト モジュールを `$env:ProgramFiles` パスに作成します。

```powershell
# Create a folder for the module
$modulePath = Join-Path $env:ProgramFiles "WindowsPowerShell\Modules\ContosoJEA"
New-Item -ItemType Directory -Path $modulePath

# Create an empty script module and module manifest.
# At least one file in the module folder must have the same name as the folder itself.
New-Item -ItemType File -Path (Join-Path $modulePath "ContosoJEAFunctions.psm1")
New-ModuleManifest -Path (Join-Path $modulePath "ContosoJEA.psd1") -RootModule "ContosoJEAFunctions.psm1"

# Create the RoleCapabilities folder and copy in the PSRC file
$rcFolder = Join-Path $modulePath "RoleCapabilities"
New-Item -ItemType Directory $rcFolder
Copy-Item -Path .\MyFirstJEARole.psrc -Destination $rcFolder
```

PowerShell モジュールの詳細については、[PowerShell モジュールの概要](/powershell/scripting/developer/windows-powershell)に関するページを参照してください。

PowerShell 6 以降では、**RoleDefinitions** プロパティがセッション構成ファイルに追加されました。 このプロパティを使用すると、使用するロールの定義のロール構成ファイルの場所を指定できます。 「[New-PSSessionConfigurationFile](/powershell/module/microsoft.powershell.core/new-pssessionconfigurationfile)」に記載されている例を参照してください。

## <a name="updating-role-capabilities"></a>ロール機能を更新する

ロール機能ファイルを編集していつでも設定を更新できます。 ロール機能の更新後に新しく JEA セッションを起動すると、変更後の機能が反映されます。

そのため、ロール機能フォルダーのアクセスを制御することはとても重要となります。 ロール機能ファイルの変更は、非常に信頼できる管理者にのみ許可する必要があります。 信頼されていないユーザーにロール機能ファイルの変更を許可すると、特権を昇格させるコマンドレットの利用を簡単に許可してしまいます。

ロール機能へのアクセスを管理者がロックダウンする場合、ローカル システムにロール機能ファイルとそれが含まれるモジュールの読み取りアクセスが与えられるようにします。

## <a name="how-role-capabilities-are-merged"></a>ロール機能を結合する方法

ユーザーは、JEA セッションに入るときに、[セッション構成ファイル](session-configurations.md)の一致するすべてのロール機能へのアクセスが与えられます。 JEA では、ロールで許可される、最も制限の少ないコマンド セットをユーザーに与えようとします。

### <a name="visiblecmdlets-and-visiblefunctions"></a>VisibleCmdlets と VisibleFunctions

最も複雑な結合ロジックがコマンドレットと関数に適用され、JEA でそのパラメーターとパラメーター値が制限されます。

ルールは次のようになります。

1. あるコマンドレットが 1 つのロールでのみ表示される場合、該当するあらゆるパラメーター制約の下でユーザーに表示されます。
2. あるコマンドレットが複数のロールで表示されるとき、そのコマンドレットで各ロールに同じ制約が与えられる場合、その制約の下でユーザーに表示されます。
3. あるコマンドレットが複数のロールで表示されるとき、ロールごとにパラメーター セットが異なる場合、そのコマンドレットとすべてのロールで定義されているすべてのパラメーターがユーザーに表示されます。
   1 つのロールにパラメーターの制約がない場合、すべてのパラメーターが許可されます。
4. あるロールであるコマンドレット パラメーターの検証セットまたは検証パターンが定義されているとき、別のロールでそのパラメーターが許可されるが、パラメーター値が制約されない場合、その検証セットまたはパターンは無視されます。
5. 複数のロールの同じコマンドレット パラメーターに 1 つの検証セットが定義されている場合、すべての検証セットのすべての値が許可されます。
6. 複数のロールの同じコマンドレット パラメーターに 1 つの検証パターンが定義されている場合、いずれかのパターンに一致するすべての値が許可されます。
7. 複数のロールに 1 つの検証セットが定義されているとき、別のロールで、同じコマンドレット パラメーターに 1 つの検証パターンが定義されている場合、その検証セットは無視され、残りの検証パターンにルール (6) が適用されます。

以下には、これらの規則に従ってロールをマージする方法の例を示します。

```powershell
# Role A Visible Cmdlets
$roleA = @{
    VisibleCmdlets = 'Get-Service',
                     @{ Name = 'Restart-Service';
                        Parameters = @{ Name = 'DisplayName'; ValidateSet = 'DNS Client' } }
}

# Role B Visible Cmdlets
$roleB = @{
    VisibleCmdlets = @{ Name = 'Get-Service';
                        Parameters = @{ Name = 'DisplayName'; ValidatePattern = 'DNS.*' } },
                     @{ Name = 'Restart-Service';
                        Parameters = @{ Name = 'DisplayName'; ValidateSet = 'DNS Server' } }
}

# Resulting permissions for a user who belongs to both role A and B
# - The constraint in role B for the DisplayName parameter on Get-Service
#   is ignored because of rule #4
# - The ValidateSets for Restart-Service are merged because both roles use
#   ValidateSet on the same parameter per rule #5
$mergedAandB = @{
    VisibleCmdlets = 'Get-Service',
                     @{ Name = 'Restart-Service';
                        Parameters = @{ Name = 'DisplayName'; ValidateSet = 'DNS Client', 'DNS Server' } }
}
```

### <a name="visibleexternalcommands-visiblealiases-visibleproviders-scriptstoprocess"></a>VisibleExternalCommands、VisibleAliases、VisibleProviders、ScriptsToProcess

ロール機能ファイルの他のすべてのフィールドは、許可される外部コマンド、エイリアス、プロバイダー、スタートアップ スクリプトの累積セットに追加されます。 JEA ユーザーは、1 つのロール機能で使用できるあらゆるコマンド、エイリアス、プロバイダー、スクリプトを利用できます。

あるロール機能からのプロバイダーと別のロール機能からのコマンドレット/関数/コマンドが組み合わされることで、本来の意図とは異なるシステム リソース アクセスがユーザーに与えられるような事態が発生しないように注意してください。
たとえば、あるロールで `Remove-Item` コマンドレットが許可され、別のロールで `FileSystem` プロバイダーが許可される場合、JEA ユーザーがコンピューター上のファイルを削除する可能性があります。 効果的なユーザー アクセス許可を確認する方法については、[JEA の監査](audit-and-report.md)に関する記事に追加情報があります。

## <a name="next-steps"></a>次のステップ

[セッション構成ファイルを作成する](session-configurations.md)
