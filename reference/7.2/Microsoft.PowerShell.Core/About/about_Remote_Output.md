---
description: リモートコマンドの出力を解釈および書式設定する方法について説明します。
Locale: en-US
ms.date: 12/01/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_remote_output?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Remote_Output
ms.openlocfilehash: 476afc62089795d22002ece05c7ce2bf53994237
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601389"
---
# <a name="about-remote-output"></a>リモート出力について

## <a name="short-description"></a>概要
リモートコマンドの出力を解釈および書式設定する方法について説明します。

## <a name="long-description"></a>詳細説明

リモートコンピューター上で実行されたコマンドの出力は、ローカルコンピューターで同じコマンドを実行した場合と同じように見えますが、大きな違いがいくつかあります。

このトピックでは、リモートコンピューターで実行されるコマンドの出力を解釈、書式設定、および表示する方法について説明します。

## <a name="displaying-the-computer-name"></a>コンピューター名を表示しています

Invoke-Command コマンドレットを使用してリモートコンピューターでコマンドを実行すると、そのデータを生成したコンピューターの名前を含むオブジェクトが返されます。 リモートコンピューター名は、PSComputerName プロパティに格納されます。

多くのコマンドでは、PSComputerName が既定で表示されます。 たとえば、次のコマンドは、Server01 と Server02 の2台のリモートコンピューターで Get-Culture コマンドを実行します。 下に表示される出力には、コマンドを実行したリモートコンピューターの名前が含まれています。

```powershell
C:\PS> invoke-command -script {get-culture} -comp Server01, Server02

LCID  Name    DisplayName                PSComputerName
----  ----    -----------                --------------
1033  en-US   English (United States)    Server01
1033  es-AR   Spanish (Argentina)        Server02
```

Invoke-Command の HideComputerName パラメーターを使用して、PSComputerName プロパティを非表示にすることができます。 このパラメーターは、1台のリモートコンピューターからのみデータを収集するコマンド用に設計されています。

次のコマンドは、Server01 リモートコンピューターで Get-Culture コマンドを実行します。 HideComputerName パラメーターを使用して、PSComputerName プロパティと関連するプロパティを非表示にします。

```powershell
C:\PS> invoke-command -scr {get-culture} -comp Server01 -HideComputerName

LCID             Name             DisplayName
----             ----             -----------
1033             en-US            English (United States)
```

PSComputerName プロパティが既定で表示されない場合は、表示することもできます。

たとえば、次のコマンドでは、Format-Table コマンドレットを使用して、リモート Get-Date コマンドの出力に PSComputerName プロパティを追加します。

```powershell
$dates = invoke-command -script {get-date} -computername Server01, Server02
$dates | format-table DateTime, PSComputerName -auto

DateTime                            PSComputerName
--------                            --------------
Monday, July 21, 2008 7:16:58 PM    Server01
Monday, July 21, 2008 7:16:58 PM    Server02
```

## <a name="displaying-the-machinename-property"></a>MACHINENAME プロパティを表示しています

Get Process、Get Service、および EventLog など、いくつかのコマンドレットには、リモートコンピューター上のオブジェクトを取得する ComputerName パラメーターがあります。
これらのコマンドレットは、PowerShell リモート処理を使用しないため、Windows PowerShell のリモート処理用に構成されていないコンピューターでも使用できます。

これらのコマンドレットが返すオブジェクトは、MachineName プロパティにリモートコンピューターの名前を格納します。 (これらのオブジェクトには、PSComputerName プロパティがありません)。

たとえば、次のコマンドは、Server01 および Server02 リモートコンピューター上の PowerShell プロセスを取得します。 既定の表示には、MachineName プロパティは含まれません。

```powershell
C:\PS> get-process PowerShell -computername server01, server02

Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
920      38    97524     114504   575     9.66   2648 PowerShell
194       6    24256      32384   142            3020 PowerShell
352      27    63472      63520   577     3.84   4796 PowerShell
```

Format-Table コマンドレットを使用して、プロセスオブジェクトの MachineName プロパティを表示できます。

たとえば、次のコマンドは、$p 変数にプロセスを保存し、パイプライン演算子 (|) を使用して $p 内のプロセスを Format-Table コマンドに送信します。 このコマンドは、Format-Table の Property パラメーターを使用して、MachineName プロパティをディスプレイに含めます。

```powershell
C:\PS> $p = get-process PowerShell -comp Server01, Server02
C:\PS> $P | format-table -property ID, ProcessName, MachineName -auto

Id ProcessName MachineName
-- ----------- -----------
2648 PowerShell  Server02
3020 PowerShell  Server01
4796 PowerShell  Server02
```

次のより複雑なコマンドは、MachineName プロパティを既定のプロセスの表示に追加します。 ハッシュテーブルを使用して、計算されるプロパティを指定します。 幸い、それを使用するために理解する必要はありません。

(バックティック ['] が継続文字であることに注意してください)。

```powershell
C:\PS> $p = get-process PowerShell -comp Server01, Server02

C:\PS> $p | format-table -property Handles, `
@{Label="NPM(K)";Expression={int}}, `
@{Label="PM(K)";Expression={int}}, `
@{Label="WS(K)";Expression={int}}, `
@{Label="VM(M)";Expression={int}}, `
@{Label="CPU(s)";Expression={if ($.CPU -ne $()){ $.CPU.ToString("N")}}}, `
Id, ProcessName, MachineName -auto

Handles NPM(K) PM(K)  WS(K) VM(M) CPU(s)   Id ProcessName MachineName
------- ------ -----  ----- ----- ------   -- ----------- -----------
920     38 97560 114532   576        2648 PowerShell  Server02
192      6 24132  32028   140        3020 PowerShell  Server01
438     26 48436  59132   565        4796 PowerShell  Server02

```

## <a name="deserialized-objects"></a>逆シリアル化されたオブジェクト

出力を生成するリモートコマンドを実行すると、コマンドの出力がネットワーク経由でローカルコンピューターに転送されます。

ほとんどのライブ Microsoft .NET フレームワークオブジェクト (PowerShell コマンドレットが返すオブジェクトなど) はネットワーク経由で転送できないため、ライブオブジェクトは "シリアル化" されます。 つまり、ライブオブジェクトは、オブジェクトとそのプロパティの XML 表現に変換されます。 次に、XML ベースのシリアル化されたオブジェクトがネットワーク経由で転送されます。

ローカルコンピューターでは、PowerShell は xml ベースのシリアル化されたオブジェクトを受け取り、XML ベースのオブジェクトを標準の .NET Framework オブジェクトに変換することによって "逆シリアル化" します。

ただし、逆シリアル化されたオブジェクトはライブオブジェクトではありません。 これは、シリアル化された時点のオブジェクトのスナップショットであり、プロパティは含まれていますが、メソッドは含まれていません。 これらのオブジェクトは、パイプラインで渡す、選択したプロパティを表示する、書式設定するなど、PowerShell で使用および管理できます。

逆シリアル化されたオブジェクトのほとんどは、Types.ps1xml ファイルまたは Format.ps1xml ファイル内のエントリによって表示されるように自動的に書式設定されます。 ただし、リモートコンピューター上で生成されたすべての逆シリアル化されたオブジェクトのフォーマットファイルがローカルコンピューターにない可能性があります。 オブジェクトが書式設定されていない場合、各オブジェクトのプロパティはすべて、ストリーミングリストのコンソールに表示されます。

オブジェクトが自動的に書式設定されない場合は、Format-Table や形式一覧などの書式設定コマンドレットを使用して、選択したプロパティを書式設定して表示できます。 または、Out-GridView コマンドレットを使用して、テーブル内のオブジェクトを表示することもできます。

また、ローカルコンピューター上に存在しないコマンドレットを使用しているリモートコンピューターでコマンドを実行した場合、そのコマンドが返すオブジェクトは、コンピューター上のオブジェクトの書式設定ファイルを持っていないため、正しくフォーマットされていない可能性があります。 別のコンピューターから書式設定データを取得するには、Get-FormatData と Export-FormatData のコマンドレットを使用します。

DirectoryInfo オブジェクトや Guid など、オブジェクトの種類によっては、受信時にライブオブジェクトに変換されます。 これらのオブジェクトは、特別な処理や書式設定を必要としません。

## <a name="ordering-the-results"></a>結果の順序付け

コマンドレットの ComputerName パラメーターに指定されたコンピューター名の順序によって、PowerShell がリモートコンピューターに接続する順序が決まります。 ただし、結果は、ローカルコンピューターが受信した順序で表示されます。順序は異なる場合があります。

結果の順序を変更するには、Sort-Object コマンドレットを使用します。 PSComputerName または MachineName プロパティで並べ替えることができます。 また、別のコンピューターの結果が散在するように、オブジェクトの別のプロパティを並べ替えることもできます。

## <a name="see-also"></a>関連項目

[about_Remote](about_Remote.md)

[about_Remote_Variables](about_Remote_Variables.md)

[Format-Table](xref:Microsoft.PowerShell.Utility.Format-Table)

[Get-Process](xref:Microsoft.PowerShell.Management.Get-Process)

[Get-Service](xref:Microsoft.PowerShell.Management.Get-Service)

[Invoke-Command](xref:Microsoft.PowerShell.Core.Invoke-Command)

[Select-Object](xref:Microsoft.PowerShell.Utility.Select-Object)

