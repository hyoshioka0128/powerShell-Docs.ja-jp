---
description: PowerShell でコマンドをパイプラインに結合する
Locale: en-US
ms.date: 09/27/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_pipelines?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Pipelines
ms.openlocfilehash: e4ae85fbbfe5232048a90e1fe4f62db3e95e5f1b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602221"
---
# <a name="about-pipelines"></a>パイプラインについて

## <a name="short-description"></a>簡単な説明

PowerShell でコマンドをパイプラインに結合する

## <a name="long-description"></a>長い説明

パイプラインは、パイプライン演算子 () によって接続された一連のコマンドです ( `|` ASCII 124)。 各パイプライン演算子は、前のコマンドの結果を次のコマンドに送信します。

最初のコマンドの出力は、2番目のコマンドの入力として処理のために送信できます。 また、その出力を他のコマンドに送信することもできます。 結果として、一連の単純なコマンドで構成される複雑なコマンドチェーンまたは _パイプライン_ が生成されます。

たとえば、次のように入力します。

```powershell
Command-1 | Command-2 | Command-3
```

この例では、によって `Command-1` 出力されるオブジェクトがに送信され `Command-2` ます。
`Command-2` オブジェクトを処理し、に送信し `Command-3` ます。 `Command-3` オブジェクトを処理し、パイプラインに送信します。 パイプラインにはこれ以上コマンドがないため、結果はコンソールに表示されます。

パイプラインでは、コマンドは左から右に順番に処理されます。 処理は単一の操作として処理され、生成されると出力が表示されます。

単純な例を次に示します。 次のコマンドは、メモ帳のプロセスを取得し、それを停止します。

たとえば、次のように入力します。

```powershell
Get-Process notepad | Stop-Process
```

最初のコマンドは、 `Get-Process` コマンドレットを使用して、メモ帳プロセスを表すオブジェクトを取得します。 パイプライン演算子 () を使用して、 `|` プロセスオブジェクトをコマンドレットに送信します。これにより `Stop-Process` 、メモ帳のプロセスが停止します。 `Stop-Process`指定されたプロセスはパイプラインを介して送信されるため、このコマンドには、プロセスを指定するための **名前** または **ID** パラメーターがありません。

このパイプラインの例では、現在のディレクトリ内のテキストファイルを取得し、1万バイトを超えるファイルのみを選択し、長さで並べ替え、各ファイルの名前と長さをテーブルに表示します。

```powershell
Get-ChildItem -Path *.txt |
  Where-Object {$_.length -gt 10000} |
    Sort-Object -Property length |
      Format-Table -Property name, length
```

このパイプラインは、指定された順序で4つのコマンドで構成されます。 次の図は、パイプラインの次のコマンドに渡される各コマンドからの出力を示しています。

```
Get-ChildItem -Path *.txt
| (FileInfo objects for *.txt)
V
Where-Object {$_.length -gt 10000}
| (FileInfo objects for *.txt)
| (      Length > 10000      )
V
Sort-Object -Property Length
| (FileInfo objects for *.txt)
| (      Length > 10000      )
| (     Sorted by length     )
V
Format-Table -Property name, length
| (FileInfo objects for *.txt)
| (      Length > 10000      )
| (     Sorted by length     )
| (   Formatted in a table   )
V

Name                       Length
----                       ------
tmp1.txt                    82920
tmp2.txt                   114000
tmp3.txt                   114000
```

## <a name="using-pipelines"></a>パイプラインの使用

ほとんどの PowerShell コマンドレットは、パイプラインをサポートするように設計されています。 ほとんどの場合、 **Get** コマンドレットの結果を同じ名詞の別のコマンドレットに _パイプ_ することができます。
たとえば、コマンドレットの出力をコマンドレット `Get-Service` またはコマンドレットにパイプすることができ `Start-Service` `Stop-Service` ます。

このパイプライン例では、コンピューター上の WMI サービスを開始します。

```powershell
Get-Service wmi | Start-Service
```

別の例として、 `Get-Item` PowerShell レジストリプロバイダー内のまたはの出力をコマンドレットにパイプすることができ `Get-ChildItem` `New-ItemProperty` ます。 この例では、値が **8124** の新しいレジストリエントリ **Noofemployees** を **MyCompany** レジストリキーに追加します。

```powershell
Get-Item -Path HKLM:\Software\MyCompany |
  New-ItemProperty -Name NoOfEmployees -Value 8124
```

、、、、などの多くのユーティリティコマンドレット `Get-Member` は、ほとんどの場合、 `Where-Object` `Sort-Object` `Group-Object` `Measure-Object` パイプラインでのみ使用されます。 パイプを使用して、任意のオブジェクト型をこれらのコマンドレットに送ることができます。 この例では、コンピューター上のすべてのプロセスを、プロセスごとに開いているハンドルの数で並べ替える方法を示します。

```powershell
Get-Process | Sort-Object -Property handles
```

パイプを使用して、オブジェクトを書式設定、エクスポート、および出力コマンドレット (、、、、など) に送ることができ `Format-List` `Format-Table` `Export-Clixml` `Export-CSV` `Out-File` ます。

この例では、コマンドレットを使用して、 `Format-List` プロセスオブジェクトのプロパティの一覧を表示する方法を示します。

```powershell
Get-Process winlogon | Format-List -Property *
```

単純なコマンドをパイプラインにまとめると、時間と入力が節約され、スクリプトの効率が向上することがわかります。

## <a name="how-pipelines-work"></a>パイプラインのしくみ

ここでは、入力オブジェクトをコマンドレットパラメーターにバインドし、パイプラインの実行中に処理する方法について説明します。

### <a name="accepts-pipeline-input"></a>パイプライン入力の受け入れ

パイプライン処理をサポートするには、受信コマンドレットにパイプライン入力を受け入れるパラメーターが必要です。 コマンドを `Get-Help` **Full** または **Parameter** オプションと共に使用して、パイプライン入力を受け入れるコマンドレットのパラメーターを決定します。

たとえば、コマンドレットのどのパラメーターがパイプライン入力を受け入れるかを決定するには、次のように `Start-Service` 入力します。

```powershell
Get-Help Start-Service -Full
```

または

```powershell
Get-Help Start-Service -Parameter *
```

コマンドレットのヘルプでは、 `Start-Service` **InputObject** パラメーターと **Name** パラメーターのみがパイプライン入力を受け入れることが示されています。

```Output
-InputObject <ServiceController[]>
Specifies ServiceController objects representing the services to be started.
Enter a variable that contains the objects, or type a command or expression
that gets the objects.

Required?                    true
Position?                    0
Default value                None
Accept pipeline input?       True (ByValue)
Accept wildcard characters?  false

-Name <String[]>
Specifies the service names for the service to be started.

The parameter name is optional. You can use Name or its alias, ServiceName,
or you can omit the parameter name.

Required?                    true
Position?                    0
Default value                None
Accept pipeline input?       True (ByPropertyName, ByValue)
Accept wildcard characters?  false
```

パイプラインを介してオブジェクトをに送信すると `Start-Service` 、PowerShell は、オブジェクトを **InputObject** パラメーターと **Name** パラメーターに関連付けようとします。

### <a name="methods-of-accepting-pipeline-input"></a>パイプライン入力を受け入れる方法

コマンドレットのパラメーターは、次の2つの方法のいずれかでパイプライン入力を受け入れることができます。

- **Byvalue**: パラメーターは、予期された .net 型に一致する値、またはその型に変換できる値を受け入れます。

  たとえば、の **Name** パラメーターは、 `Start-Service` 値によってパイプライン入力を受け入れます。 文字列に変換できる文字列オブジェクトまたはオブジェクトを受け入れることができます。

- **Bypropertyname**: パラメーターは、入力オブジェクトにパラメーターと同じ名前のプロパティがある場合にのみ、入力を受け入れます。

  たとえば、の Name パラメーターは、 `Start-Service` **name** プロパティを持つオブジェクトを受け入れることができます。 オブジェクトのプロパティを一覧表示するには、をにパイプし `Get-Member` ます。

パラメーターによっては、値またはプロパティ名の両方でオブジェクトを受け取ることができるため、パイプラインからの入力が簡単になります。

### <a name="parameter-binding"></a>パラメーターのバインド

パイプを使用してオブジェクトを1つのコマンドから別のコマンドにパイプすると、PowerShell は、パイプされたオブジェクトを受信側のコマンドレットのパラメーターに関連付けます。

PowerShell のパラメーターバインドコンポーネントは、次の条件に従って、入力オブジェクトをコマンドレットパラメーターに関連付けます。

- パラメーターは、パイプラインからの入力を受け入れる必要があります。
- パラメーターは、送信されるオブジェクトの型または必要な型に変換できる型を受け入れる必要があります。
- パラメーターがコマンドで使用されませんでした。

たとえば、 `Start-Service` コマンドレットには多くのパラメーターがありますが、そのうちの 2 つだけがパイプライン入力を受け入れます。 **Name** パラメーターは文字列を受け取り、 **InputObject** パラメーターはサービスオブジェクトを受け取ります。 したがって、文字列、サービスオブジェクト、およびオブジェクトを、文字列またはサービスオブジェクトに変換できるプロパティと共にパイプすることができます。

PowerShell は、可能な限り効率的にパラメーターバインディングを管理します。 特定のパラメーターにバインドする PowerShell を提案したり、強制したりすることはできません。 PowerShell がパイプされたオブジェクトをバインドできない場合、コマンドは失敗します。

バインディングエラーのトラブルシューティングの詳細については、この記事で後述する「 [パイプラインエラーの調査](#investigating-pipeline-errors) 」を参照してください。

### <a name="one-at-a-time-processing"></a>1回限りの処理

オブジェクトをコマンドにパイプするのは、コマンドのパラメーターを使用してオブジェクトを送信するのとよく似ています。 パイプラインの例を見てみましょう。 この例では、パイプラインを使用して、サービスオブジェクトのテーブルを表示します。

```powershell
Get-Service | Format-Table -Property Name, DependentServices
```

機能的には、これはの **InputObject** パラメーターを使用してオブジェクトコレクションを送信するのと似てい `Format-Table` ます。

たとえば、 **InputObject** パラメーターを使用して渡された変数にサービスのコレクションを保存できます。

```powershell
$services = Get-Service
Format-Table -InputObject $services -Property Name, DependentServices
```

または、 **InputObject** パラメーターにコマンドを埋め込むこともできます。

```powershell
Format-Table -InputObject (Get-Service) -Property Name, DependentServices
```

ただし、重要な違いがあります。 パイプを使用して複数のオブジェクトをコマンドに渡した場合、PowerShell は一度に1つずつコマンドにオブジェクトを送信します。 コマンドパラメーターを使用すると、オブジェクトは1つの配列オブジェクトとして送信されます。 この軽微な違いには大きな影響があります。

パイプラインを実行すると、PowerShell は、インターフェイスを実装する任意の型を自動的に列挙 `IEnumerable` し、パイプラインを介して一度に1つずつメンバーを送信します。 例外は `[hashtable]` で、メソッドを呼び出す必要があり `GetEnumerator()` ます。

次の例では、配列とハッシュテーブルをコマンドレットにパイプして、 `Measure-Object` パイプラインから受信したオブジェクトの数をカウントしています。 配列に複数のメンバーがあり、ハッシュテーブルに複数のキーと値のペアが含まれています。 配列は、一度に1つずつ列挙されます。

```powershell
@(1,2,3) | Measure-Object
```

```Output
Count    : 3
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

```powershell
@{"One"=1;"Two"=2} | Measure-Object
```

```Output
Count    : 1
Average  :
Sum      :
Maximum  :
Minimum  :
Property :
```

同様に、コマンドレットからコマンドレットに複数のプロセスオブジェクトをパイプすると、 `Get-Process` `Get-Member` PowerShell は各プロセスオブジェクトを一度に1つずつに送信 `Get-Member` します。 `Get-Member` プロセスオブジェクトの .NET クラス (型)、およびそのプロパティとメソッドを表示します。

```powershell
Get-Process | Get-Member
```

```Output
TypeName: System.Diagnostics.Process

Name      MemberType     Definition
----      ----------     ----------
Handles   AliasProperty  Handles = Handlecount
Name      AliasProperty  Name = ProcessName
NPM       AliasProperty  NPM = NonpagedSystemMemorySize
...
```

> [!NOTE]
> `Get-Member` 重複を排除します。したがって、オブジェクトの種類がすべて同じである場合は、オブジェクトの種類が1つだけ表示されます。

ただし、の **InputObject** パラメーターを使用すると、は `Get-Member` `Get-Member` 1 つの単位として system.string オブジェクトの配列を受け取ります。 オブジェクトの配列のプロパティを表示します。 ( `[]` **System.object** 型名の後にある配列シンボル () に注意してください)。

たとえば、次のように入力します。

```powershell
Get-Member -InputObject (Get-Process)
```

```Output
TypeName: System.Object[]

Name               MemberType    Definition
----               ----------    ----------
Count              AliasProperty Count = Length
Address            Method        System.Object& Address(Int32 )
Clone              Method        System.Object Clone()
...
```

この結果は意図したものではない可能性があります。 しかし、理解したら、それを使用することができます。 たとえば、すべての配列オブジェクトには **Count** プロパティがあります。 これを使用すると、コンピューター上で実行されているプロセスの数をカウントできます。

たとえば、次のように入力します。

```powershell
(Get-Process).count
```

パイプラインを介して送信されたオブジェクトは一度に1つずつ配信されることに注意してください。

## <a name="investigating-pipeline-errors"></a>パイプラインエラーの調査

PowerShell がパイプ処理されたオブジェクトを受信側のコマンドレットのパラメーターに関連付けられない場合、コマンドは失敗します。

次の例では、レジストリエントリをあるレジストリキーから別のレジストリキーに移動しようとしています。 `Get-Item`コマンドレットは、ターゲットパスを取得します。これは、コマンドレットにパイプされ `Move-ItemProperty` ます。 この `Move-ItemProperty` コマンドは、移動するレジストリエントリの現在のパスと名前を指定します。

```powershell
Get-Item -Path HKLM:\Software\MyCompany\sales |
Move-ItemProperty -Path HKLM:\Software\MyCompany\design -Name product
```

コマンドが失敗し、PowerShell によって次のエラーメッセージが表示されます。

```Output
Move-ItemProperty : The input object can't be bound to any parameters for
the command either because the command doesn't take pipeline input or the
input and its properties do not match any of the parameters that take
pipeline input.
At line:1 char:23
+ $a | Move-ItemProperty <<<<  -Path HKLM:\Software\MyCompany\design -Name p
```

調査するには、コマンドレットを使用して、 `Trace-Command` PowerShell のパラメーターバインドコンポーネントをトレースします。 次の例では、パイプラインの実行中にパラメーターバインドをトレースします。 **PSHost** パラメーターは、コンソールにトレース結果を表示し、 **FilePath** パラメーターは、 `debug.txt` 後で参照するためにトレース結果をファイルに送信します。

```powershell
Trace-Command -Name ParameterBinding -PSHost -FilePath debug.txt -Expression {
  Get-Item -Path HKLM:\Software\MyCompany\sales |
    Move-ItemProperty -Path HKLM:\Software\MyCompany\design -Name product
}
```

トレースの結果は時間がかかりますが、コマンドレットにバインドされている値 `Get-Item` と、コマンドレットにバインドされている名前付きの値を示してい `Move-ItemProperty` ます。

```Output
...
BIND NAMED cmd line args [`Move-ItemProperty`]
BIND arg [HKLM:\Software\MyCompany\design] to parameter [Path]
...
BIND arg [product] to parameter [Name]
...
BIND POSITIONAL cmd line args [`Move-ItemProperty`]
...
```

最後に、パスを **Destination** パラメーターにバインドしようとして失敗したことを示して `Move-ItemProperty` います。

```Output
...
BIND PIPELINE object to parameters: [`Move-ItemProperty`]
PIPELINE object TYPE = [Microsoft.Win32.RegistryKey]
RESTORING pipeline parameter's original values
Parameter [Destination] PIPELINE INPUT ValueFromPipelineByPropertyName NO
COERCION
Parameter [Credential] PIPELINE INPUT ValueFromPipelineByPropertyName NO
COERCION
...
```

コマンドレットを使用して、 `Get-Help` **Destination** パラメーターの属性を表示します。

```Output
Get-Help Move-ItemProperty -Parameter Destination

-Destination <String>
    Specifies the path to the destination location.

    Required?                    true
    Position?                    1
    Default value                None
    Accept pipeline input?       True (ByPropertyName)
    Accept wildcard characters?  false
```

結果には、 **変換先** がプロパティ名によってのみパイプライン入力を受け取ることが示されています。 したがって、パイプされたオブジェクトには **Destination** という名前のプロパティが必要です。

`Get-Member`から取得するオブジェクトのプロパティを表示するには、を使用し `Get-Item` ます。

```powershell
Get-Item -Path HKLM:\Software\MyCompany\sales | Get-Member
```

出力には、項目が、**変換先** のプロパティを持たない、 **Microsoft の Win32 の RegistryKey** オブジェクトであることが示されています。 コマンドが失敗した理由について説明します。

**Path** パラメーターは、名前または値によってパイプラインの入力を受け入れます。

```Output
Get-Help Move-ItemProperty -Parameter Path

-Path <String[]>
    Specifies the path to the current location of the property. Wildcard
    characters are permitted.

    Required?                    true
    Position?                    0
    Default value                None
    Accept pipeline input?       True (ByPropertyName, ByValue)
    Accept wildcard characters?  true
```

コマンドを修正するには、コマンドレットで出力先を指定し、を使用して `Move-ItemProperty` `Get-Item` 移動する項目の **パス** を取得する必要があります。

たとえば、次のように入力します。

```powershell
Get-Item -Path HKLM:\Software\MyCompany\design |
Move-ItemProperty -Destination HKLM:\Software\MyCompany\sales -Name product
```

## <a name="intrinsic-line-continuation"></a>組み込みの行連結

既に説明したように、パイプラインはパイプライン演算子 () によって接続された一連のコマンドであり `|` 、通常は単一行に書き込まれます。 ただし、読みやすくするために、PowerShell ではパイプラインを複数の行に分割することができます。
パイプ演算子が行の最後のトークンである場合、PowerShell パーサーは次の行を現在のコマンドに結合して、パイプラインの構築を続行します。

たとえば、次の単一行パイプラインを使用します。

```powershell
Command-1 | Command-2 | Command-3
```

次のように記述できます。

```powershell
Command-1 |
  Command-2 |
    Command-3
```

後続の行の先頭のスペースは重要ではありません。 インデントは読みやすくなります。

PowerShell 7 では、行の先頭にパイプライン文字を使用したパイプラインの継続のサポートが追加されます。 次の例は、この新機能を使用する方法を示しています。

```powershell
# Wrapping with a pipe at the beginning of a line (no backtick required)
Get-Process | Where-Object CPU | Where-Object Path
    | Get-Item | Where-Object FullName -match "AppData"
    | Sort-Object FullName -Unique

# Wrapping with a pipe on a line by itself
Get-Process | Where-Object CPU | Where-Object Path
    |
    Get-Item | Where-Object FullName -match "AppData"
    |
    Sort-Object FullName -Unique
```

> [!IMPORTANT]
> シェルで対話的に作業する場合は、 <kbd>Ctrl</kbd>V を使用して貼り付けるときにのみ、パイプラインを使用してコードを行の先頭に貼り付け + <kbd></kbd>ます。
> [貼り付け操作] を右クリックすると、行が1つずつ挿入されます。 行の末尾がパイプライン文字ではないため、PowerShell は入力が完了したと見なし、入力された行を実行します。

## <a name="see-also"></a>参照

[about_PSReadLine](../../PSReadLine/About/about_PSReadLine.md)

[about_Objects](about_objects.md)

[about_Parameters](about_parameters.md)

[about_Command_Syntax](about_command_syntax.md)

[about_ForEach](about_foreach.md)

