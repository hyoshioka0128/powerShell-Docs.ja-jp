---
external help file: System.Management.Automation.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 09/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ForEach-Object
ms.openlocfilehash: 8a79dcf9c2af7aed0c52c361467cab23f880a893
ms.sourcegitcommit: fb9bafd041e3615b9dc9fb77c9245581b705cd02
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/22/2020
ms.locfileid: "99603221"
---
# ForEach-Object

## 構文
入力オブジェクトのコレクション内の各項目に対して操作を実行します。

## 構文

### ScriptBlockSet (既定)

```
ForEach-Object [-InputObject <PSObject>] [-Begin <ScriptBlock>] [-Process] <ScriptBlock[]>
 [-End <ScriptBlock>] [-RemainingScripts <ScriptBlock[]>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

### PropertyAndMethodSet

```
ForEach-Object [-InputObject <PSObject>] [-MemberName] <String> [-ArgumentList <Object[]>] [-WhatIf]
 [-Confirm] [<CommonParameters>]
```

### ParallelParameterSet

```
ForEach-Object -Parallel <scriptblock> [-InputObject <PSObject>] [-ThrottleLimit <int>]
[-UseNewRunspace] [-TimeoutSeconds <int>] [-AsJob] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## 説明

`ForEach-Object`コマンドレットは、入力オブジェクトのコレクション内の各項目に対して操作を実行します。 入力オブジェクトは、コマンドレットにパイプすることも、 **InputObject** パラメーターを使用して指定することもできます。

Windows PowerShell 3.0 以降では、コマンドを構築する方法は2つあり `ForEach-Object` ます。

- **スクリプトブロック**。 スクリプト ブロックを使用して、操作を指定することができます。 スクリプトブロック内で、変数を使用して `$_` 現在のオブジェクトを表します。 スクリプト ブロックは、**Process** パラメーターの値です。 スクリプトブロックには、任意の PowerShell スクリプトを含めることができます。

  たとえば、次のコマンドでは、コンピューター上の各プロセスの **ProcessName** プロパティの値を取得します

  `Get-Process | ForEach-Object {$_.ProcessName}`

  `ForEach-Object` 「 `begin` `process` `end` [about_functions](about/about_functions.md#piping-objects-to-functions)」で説明されているように、、、およびの各ブロックをサポートします。

  > [!NOTE]
  > スクリプトブロックは、呼び出し元のスコープで実行されます。 したがって、ブロックはそのスコープ内の変数にアクセスでき、コマンドレットの完了後にそのスコープに保持される新しい変数を作成できます。

- **Operation ステートメント**。 また、操作ステートメントを記述することもできます。これは自然言語に似ています。 操作のステートメントを使用してプロパティの値を指定することも、メソッドを呼び出すこともできます。 操作のステートメントは、Windows PowerShell 3.0 で導入されました。

  たとえば、次のコマンドでは、コンピューター上の各プロセスの **ProcessName** プロパティの値も取得します。

  `Get-Process | ForEach-Object ProcessName`

- **並列実行スクリプトブロック** です。 PowerShell 7.0 以降では、3番目のパラメーターセットを使用して、各スクリプトブロックを並列に実行できます。 **ThrottleLimit** パラメーターは、一度に実行される並列スクリプトの数を制限します。 前と同様に、変数を使用して、 `$_` スクリプトブロック内の現在の入力オブジェクトを表します。 `$using:`実行中のスクリプトに変数参照を渡すには、キーワードを使用します。

  PowerShell 7 では、ループの反復ごとに新しい実行空間が作成され、最大の分離が実現されます。
  これは、新しい実行空間の作成と比較した場合や、大量の作業を実行するイテレーションが多数ある場合に、パフォーマンスとリソースに大きな影響を与える可能性があります。 PowerShell 7.1 では、実行空間プールからの実行空間が既定で再利用されます。 実行空間プールのサイズは、 **ThrottleLimit** パラメーターによって指定されます。 既定の実行空間プールサイズは5です。 **UseNewRunspace** スイッチを使用して、反復ごとに新しい実行空間を作成することもできます。

  既定では、parallel scriptblocks は、並列タスクを開始した呼び出し元の現在の作業ディレクトリを使用します。

  終了しないエラーは、並列実行される scriptblocks で発生するようにコマンドレットエラーストリームに書き込まれます。 並列 scriptblock の実行順序を決定できないため、エラーストリームにエラーが表示される順序はランダムになります。 同様に、警告、詳細、情報など、他のデータストリームに書き込まれたメッセージは、不定の順序でこれらのデータストリームに書き込まれます。

  例外などの終了エラーは、発生した scriptblocks の個々の並列インスタンスを終了します。 1つの scriptblocks で終了エラーが発生しても、コマンドレットが終了しないことがあり `Foreach-Object` ます。 並列で実行されている他の scriptblocks は、終了エラーも発生しない限り実行を継続します。 終了エラーは、の **FullyQualifiedErrorId** を持つ **errorrecord** としてエラーデータストリームに書き込まれ `PSTaskException` ます。
  終了エラーは、PowerShell の try/catch ブロックまたはトラップブロックを使用して、終了しないエラーに変換できます。

## 例

### 例 1: 配列の整数を除算する

この例では、3つの整数の配列を受け取り、それぞれを1024で除算します。

```powershell
30000, 56798, 12432 | ForEach-Object -Process {$_/1024}
```

```Output
29.296875
55.466796875
12.140625
```

### 例 2: ディレクトリ内のすべてのファイルの長さを取得する

この例では、PowerShell インストールディレクトリ内のファイルとディレクトリを処理し `$PSHOME` ます。

```powershell
Get-ChildItem $PSHOME |
  ForEach-Object -Process {if (!$_.PSIsContainer) {$_.Name; $_.Length / 1024; " " }}
```

オブジェクトがディレクトリではない場合、スクリプトブロックはファイルの名前を取得し、その **長さ** のプロパティの値を1024で割り、次のエントリと区別するためにスペース ("") を追加します。 コマンドレットでは、 **PSISContainer** プロパティを使用して、オブジェクトがディレクトリであるかどうかを確認します。

### 例 3: 最新のシステムイベントを操作する

この例では、システムイベントログから1000の最新のイベントをテキストファイルに書き込みます。 イベントの処理の前後に、現在の時刻が表示されます。

```powershell
$Events = Get-EventLog -LogName System -Newest 1000
$events | ForEach-Object -Begin {Get-Date} -Process {Out-File -FilePath Events.txt -Append -InputObject $_.Message} -End {Get-Date}
```

`Get-EventLog` システムイベントログから1000の最新のイベントを取得し、変数に格納し `$Events` ます。 `$Events` 次に、をコマンドレットにパイプ処理し `ForEach-Object` ます。 **Begin** パラメーターは、現在の日付と時刻を示します。 次に、 **Process** パラメーターは、コマンドレットを使用して `Out-File` events.txt という名前のテキストファイルを作成し、そのファイル内の各イベントの message プロパティを格納します。 最後に、**End** パラメーターは、すべての処理が完了した後の日付と時刻を示すために使用されます。

### 例 4: レジストリキーの値を変更する

この例では、キーの下にあるすべてのサブキーの **RemotePath** レジストリエントリの値 `HKCU:\Network` を大文字に変更します。

```powershell
Get-ItemProperty -Path HKCU:\Network\* |
  ForEach-Object {Set-ItemProperty -Path $_.PSPath -Name RemotePath -Value $_.RemotePath.ToUpper();}
```

このフォーマットを使用して、レジストリ エントリ値の形式や内容を変更することができます。

**ネットワーク** キーの各サブキーは、サインオン時に再接続する、マップされたネットワークドライブを表します。 **RemotePath** エントリには、接続されているドライブの UNC パスが含まれます。 たとえば、E: ドライブをにマップすると、 `\\Server\Share` で RemotePath レジストリ値がに設定された **e** サブキーが作成され `HKCU:\Network`  `\\Server\Share` ます。

このコマンドは、コマンドレットを使用して `Get-ItemProperty` **ネットワーク** キーのすべてのサブキーを取得し、コマンドレットを使用して `Set-ItemProperty` 各キーの **RemotePath** レジストリエントリの値を変更します。
コマンドの `Set-ItemProperty` パスは、レジストリキーの **pspath** プロパティの値です。 これは、レジストリエントリではなく、レジストリキーを表す Microsoft .NET Framework オブジェクトのプロパティです。 このコマンドでは、 **RemotePath** 値の **ToUpper ()** メソッドを使用します。これは文字列 (REG_SZ) です。

`Set-ItemProperty`は各キーのプロパティを変更するため、 `ForEach-Object` プロパティにアクセスするにはコマンドレットが必要です。

### 例 5: $Null 自動変数を使用する

この例は、 `$Null` 自動変数をコマンドレットにパイプした場合の効果を示して `ForEach-Object` います。

```powershell
1, 2, $null, 4 | ForEach-Object {"Hello"}
```

```Output
Hello
Hello
Hello
Hello
```

PowerShell では null が明示的なプレースホルダーとして扱われるため、 `ForEach-Object` このコマンドレットは `$Null` 、パイプを使用する他のオブジェクトと同様に、の値を生成します。

### 例 6: プロパティ値を取得する

この例では、コマンドレットの **MemberName** パラメーターを使用して、インストールされているすべての PowerShell モジュールの **Path** プロパティの値を取得し `ForEach-Object` ます。

```powershell
Get-Module -ListAvailable | ForEach-Object -MemberName Path
Get-Module -ListAvailable | Foreach Path
```

2 番目のコマンドは、1 番目のコマンドと同等です。 `Foreach`コマンドレットのエイリアスを使用 `ForEach-Object` し、 **MemberName** パラメーターの名前を省略します。これは省略可能です。

コマンドレットは、プロパティ値の `ForEach-Object` 型を変更する **Format** コマンドレットやコマンドレットとは異なり、型を変更せずに値を取得するため、プロパティ値を取得する場合に便利です `Select-Object` 。

### 例 7: モジュール名をコンポーネント名に分割する

この例では、2つのドットで区切られたモジュール名をコンポーネント名に分割する3つの方法を示します。

```powershell
"Microsoft.PowerShell.Core", "Microsoft.PowerShell.Host" | ForEach-Object {$_.Split(".")}
"Microsoft.PowerShell.Core", "Microsoft.PowerShell.Host" | ForEach-Object -MemberName Split -ArgumentList "."
"Microsoft.PowerShell.Core", "Microsoft.PowerShell.Host" | Foreach Split "."
```

```Output
Microsoft
PowerShell
Core
Microsoft
PowerShell
Host
```

このコマンドは、文字列の **Split** メソッドを呼び出します。 3 つのコマンドは、別の構文を使用しますが、すべて同等であり、交換可能です。

最初のコマンドは、スクリプトブロックと現在のオブジェクト演算子を含む従来の構文を使用し `$_` ます。 メソッドの指定にドット構文を使用し、区切り記号の引数を囲むためにかっこを使用します。

2 番目のコマンドは、**MemberName** パラメーターを使用して **Split** メソッドを指定し、**ArgumentName** パラメーターを使用してドット (".") を分割の区切り記号として識別します。

3番目のコマンドは、コマンドレットの **Foreach** エイリアスを使用 `ForEach-Object` して、 **MemberName** および **ArgumentList** パラメーターの名前を省略します (省略可能)。

### 例 8: 2 つのスクリプトブロックと共に ForEach-Object を使用する

この例では、2つのスクリプトブロック位置を渡します。 すべてのスクリプトブロックが **Process** パラメーターにバインドされます。 ただし、これらは **Begin** および **Process** パラメーターに渡されたものとして扱われます。

```powershell
1..2 | ForEach-Object { 'begin' } { 'process' }
```

```Output
begin
process
process
```

### 例 9: 3 つ以上のスクリプトブロックと共に ForEach-Object を使用する

この例では、2つのスクリプトブロック位置を渡します。 すべてのスクリプトブロックが **Process** パラメーターにバインドされます。 ただし、これらは **Begin**、 **Process**、および **End** パラメーターに渡されたものとして扱われます。

```powershell
1..2 | ForEach-Object { 'begin' } { 'process A' }  { 'process B' }  { 'end' }
```

```Output
begin
process A
process B
process A
process B
end
```

> [!NOTE]
> 最初のスクリプトブロックは常にブロックにマップされ、最後のブロックはブロックにマップされ、 `begin` `end` between 内のブロックはすべてブロックにマップされ `process` ます。

### 例 10: パイプライン項目ごとに複数のスクリプトブロックを実行する

前の例で示したように、 **Process** パラメーターを使用して渡された複数のスクリプトブロックは、 **Begin** および **End** パラメーターにマップされます。 このマッピングを回避するには、 **Begin** および **End** パラメーターに明示的な値を指定する必要があります。

```powershell
1..2 | ForEach-Object -Begin $null -Process { 'one' }, { 'two' }, { 'three' } -End $null
```

```Output
one
two
three
one
two
three
```

### 例 11: 並列バッチで低速スクリプトを実行する

この例では、文字列を評価し、1秒間スリープする単純なスクリプトブロックを実行します。

```powershell
$Message = "Output:"

1..8 | ForEach-Object -Parallel {
    "$using:Message $_"
    Start-Sleep 1
} -ThrottleLimit 4
```

```Output
Output: 1
Output: 2
Output: 3
Output: 4
Output: 5
Output: 6
Output: 7
Output: 8
```

入力が4つのバッチで処理されるように、 **ThrottleLimit** パラメーターの値は4に設定されています。
キーワードは、 `$using:` `$Message` 各並列スクリプトブロックに変数を渡すために使用されます。

### 例 12: ログエントリを並列で取得する

この例では、ローカル Windows コンピューター上の5つのシステムログから5万のログエントリを取得します。

```powershell
$logNames = 'Security','Application','System','Windows PowerShell','Microsoft-Windows-Store/Operational'

$logEntries = $logNames | ForEach-Object -Parallel {
    Get-WinEvent -LogName $_ -MaxEvents 10000
} -ThrottleLimit 5

$logEntries.Count
```

```Output
50000
```

**Parallel** パラメーターは、入力ログ名ごとに並列実行されるスクリプト ブロックを指定します。 **ThrottleLimit** パラメーターを指定すると、5つのすべてのスクリプトブロックが同時に実行されるようになります。

### 例 13: ジョブとして並列に実行する

この例では、単純なスクリプトブロックを並列に実行し、同時に2つのバックグラウンドジョブを作成します。

```powershell
$job = 1..10 | ForEach-Object -Parallel {
    "Output: $_"
    Start-Sleep 1
} -ThrottleLimit 2 -AsJob

$job | Receive-Job -Wait
```

```Output
Output: 1
Output: 2
Output: 3
Output: 4
Output: 5
Output: 6
Output: 7
Output: 8
Output: 9
Output: 10
```

変数は、 `$job` 出力データを収集し、実行状態を監視するジョブオブジェクトを受け取ります。
ジョブオブジェクトは、待機スイッチパラメーターを使用してにパイプ処理され `Receive-Job` ます。  このストリームは、が AsJob なしで実行された場合と同様に、コンソールに出力 `ForEach-Object -Parallel` されます。 

### 例 14: スレッドセーフな変数参照を使用する

この例では、スクリプトブロックを並行して呼び出して、一意の名前を付けたプロセスオブジェクトを収集します。

```powershell
$threadSafeDictionary = [System.Collections.Concurrent.ConcurrentDictionary[string,object]]::new()
Get-Process | ForEach-Object -Parallel {
    $dict = $using:threadSafeDictionary
    $dict.TryAdd($_.ProcessName, $_)
}

$threadSafeDictionary["pwsh"]
```

```Output
 NPM(K)    PM(M)      WS(M)     CPU(s)      Id  SI ProcessName
 ------    -----      -----     ------      --  -- -----------
     82    82.87     130.85      15.55    2808   2 pwsh
```

オブジェクトを収集するために、 **ConcurrentDictionary** オブジェクトの1つのインスタンスが各スクリプトブロックに渡されます。 **ConcurrentDictionary** はスレッドセーフであるため、各並列スクリプトによって安全に変更できます。 スレッドセーフでないオブジェクト ( **system.string など)** は、ここでは安全に使用できません。

> [!NOTE]
> この例は、 **並列** パラメーターの非常に非効率的な使用方法です。 このスクリプトは、単純に入力オブジェクトを同時辞書オブジェクトに追加します。 これは簡単で、各スクリプトを個別のスレッドで呼び出すオーバーヘッドにはなりません。 `ForEach-Object`**並列** スイッチを使用せずに通常どおりに実行すると、より効率的で高速になります。 この例は、スレッドセーフな変数の使用方法を示すことのみを目的としています。

### 例 15: 並列実行によるエラーの書き込み

この例では、エラーストリームへの書き込みを並行して行います。書き込まれたエラーの順序はランダムになります。

```powershell
1..3 | ForEach-Object -Parallel {
    Write-Error "Error: $_"
}
```

```Output
Write-Error: Error: 1
Write-Error: Error: 3
Write-Error: Error: 2
```

### 例 16: 並列実行のエラーを終了する

この例では、1つの並列実行 scriptblock での終了エラーを示します。

```powershell
1..5 | ForEach-Object -Parallel {
    if ($_ -eq 3)
    {
        throw "Terminating Error: $_"
    }

    Write-Output "Output: $_"
}
```

```Output
Exception: Terminating Error: 3
Output: 1
Output: 4
Output: 2
Output: 5
```

`Output: 3` は、その反復処理の並列 scriptblock が終了したため、書き込まれませんでした。

## パラメーター

### -ArgumentList

メソッド呼び出しの引数の配列を指定します。 **ArgumentList** の動作の詳細については、「 [about_Splatting](about/about_Splatting.md#splatting-with-arrays)」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Object[]
Parameter Sets: PropertyAndMethodSet
Aliases: Args

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -開始

このコマンドレットが入力オブジェクトを処理する前に実行されるスクリプトブロックを指定します。 このスクリプトブロックは、パイプライン全体に対して1回だけ実行されます。 ブロックの詳細については `begin` 、「 [about_Functions](about/about_functions.md#piping-objects-to-functions)」を参照してください。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ScriptBlockSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -End

このコマンドレットによってすべての入力オブジェクトが処理された後に実行されるスクリプトブロックを指定します。 このスクリプトブロックは、パイプライン全体に対して1回だけ実行されます。 ブロックの詳細については `end` 、「 [about_Functions](about/about_functions.md#piping-objects-to-functions)」を参照してください。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ScriptBlockSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

入力オブジェクトを指定します。 `ForEach-Object` 各入力オブジェクトに対して、スクリプトブロックまたは操作ステートメントを実行します。 オブジェクトが格納されている変数を入力するか、オブジェクトを取得するコマンドまたは式を入力します。

で **inputobject** パラメーターを使用すると、 `ForEach-Object` コマンドの結果をにパイプするのではなく、 `ForEach-Object` **inputobject** 値が1つのオブジェクトとして扱われます。 これは、値が、などのコマンドの結果であるコレクションである場合にも当てはまり `-InputObject (Get-Process)` ます。
**InputObject** は配列またはオブジェクトのコレクションから個々のプロパティを返すことができないため、を使用し `ForEach-Object` て、定義されたプロパティに特定の値を持つオブジェクトのオブジェクトのコレクションに対して操作を実行する場合は、 `ForEach-Object` このトピックの例に示すように、パイプラインでを使用することをお勧めします。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -MemberName

取得するプロパティまたは呼び出すメソッドを指定します。

ワイルドカード文字は使用できますが、結果の文字列が一意の値に解決される場合にのみ機能します。
たとえば、を実行すると、 `Get-Process | ForEach -MemberName *Name` ワイルドカードパターンは、コマンドが失敗する原因となる複数のメンバーと一致します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: PropertyAndMethodSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Process

各入力オブジェクトに対して実行する操作を指定します。 このスクリプトブロックは、パイプライン内のすべてのオブジェクトに対して実行されます。 ブロックの詳細については `process` 、「 [about_Functions](about/about_functions.md#piping-objects-to-functions)」を参照してください。

**Process** パラメーターに複数のスクリプトブロックを指定した場合、最初のスクリプトブロックは常にブロックにマップされ `begin` ます。 スクリプトブロックが2つしかない場合は、2番目のブロックがブロックにマップされ `process` ます。 3つ以上のスクリプトブロックがある場合、最初のスクリプトブロックは常にブロックにマップされ、最後のブロックはブロックにマップされ、 `begin` `end` between 内のブロックはすべてブロックにマップされ `process` ます。

```yaml
Type: System.Management.Automation.ScriptBlock[]
Parameter Sets: ScriptBlockSet
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RemainingScripts

**Process** パラメーターによって取得されないすべてのスクリプトブロックを指定します。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.ScriptBlock[]
Parameter Sets: ScriptBlockSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Parallel

入力オブジェクトの並列処理に使用するスクリプトブロックを指定します。 操作を記述するスクリプト ブロックを入力します。

このパラメーターは、PowerShell 7.0 で導入されました。

```yaml
Type: System.Management.Automation.ScriptBlock
Parameter Sets: ParallelParameterSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ThrottleLimit

並列でスクリプトブロックの数を指定します。 入力オブジェクトは、実行中のスクリプトブロックカウントが **ThrottleLimit** を下回るまでブロックされます。 既定値は `5` です。

このパラメーターは、PowerShell 7.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: ParallelParameterSet
Aliases:

Required: False
Position: Named
Default value: 5
Accept pipeline input: False
Accept wildcard characters: False
```

### -TimeoutSeconds

すべての入力が並行して処理されるまで待機する秒数を指定します。 指定されたタイムアウト時間が経過すると、実行中のすべてのスクリプトが停止します。 また、処理するその他の入力オブジェクトは無視されます。 の既定値 `0` はタイムアウトを無効にし、 `ForEach-Object -Parallel` 無期限に実行できます。 <kbd></kbd> + コマンドラインで Ctrl<kbd>C</kbd>キーを押すと、実行中のコマンドが停止 `ForEach-Object -Parallel` します。 このパラメーターは、 **AsJob** パラメーターと共に使用することはできません。

このパラメーターは、PowerShell 7.0 で導入されました。

```yaml
Type: System.Int32
Parameter Sets: ParallelParameterSet
Aliases:

Required: False
Position: Named
Default value: 0
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseNewRunspace

実行空間プールから実行空間を再利用するのではなく、並列呼び出しによって、ループの反復ごとに新しい実行空間が作成されます。

このパラメーターは、PowerShell 7.1 で導入されました。

```yml
Type: SwitchParameter
Parameter Sets: ParallelParameterSet
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -AsJob

並列呼び出しを PowerShell ジョブとして実行します。 実行中のスクリプトブロックからの出力ではなく、1つのジョブオブジェクトが返されます。 ジョブオブジェクトには、実行される各並列スクリプトブロックの子ジョブが含まれています。 ジョブオブジェクトは、実行状態を監視してデータを取得するために、すべての PowerShell ジョブコマンドレットで使用できます。

このパラメーターは、PowerShell 7.0 で導入されました。

```yaml
Type: SwitchParameter
Parameter Sets: ParallelParameterSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

```yaml
Type: SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して任意のオブジェクトをこのコマンドレットに渡します。

## 出力

### システム管理. PSObject

このコマンドレットは、入力によって決定されたオブジェクトを返します。

## メモ

- `ForEach-Object`コマンドレットは **foreach** ステートメントとよく似ていますが、入力を **foreach** ステートメントにパイプすることはできません。 **Foreach** ステートメントの詳細については、「 [about_Foreach](./About/about_Foreach.md)」を参照してください。

- PowerShell 4.0 以降では `Where` 、 `ForEach` コレクションで使用するためのメソッドとメソッドが追加されました。 これらの新しいメソッドの詳細については、こちらを参照してください [about_arrays](./About/about_Arrays.md)

- `ForEach-Object -Parallel`パラメーターセットは、PowerShell の内部 API を使用して各スクリプトブロックを実行します。 これは `ForEach-Object` 、通常のシーケンシャル処理で実行する場合よりも、オーバーヘッドが大幅に増加します。 **並列実行** のオーバーヘッドは、スクリプトブロックで実行される作業と比較して小さくすることが重要です。 次に例を示します。

  - マルチコアマシンでの多くのコンピューティング処理を要するスクリプト
  - 結果の待機時間またはファイル操作の実行に時間を費やすスクリプト

  **Parallel** パラメーターを使用すると、スクリプトの実行速度が通常より遅くなる可能性があります。 並列スクリプトが単純な場合は特にそうです。 **並列** で実験して、利点がある場所を見つけます。

  > [!IMPORTANT]
  > `ForEach-Object -Parallel`パラメーターセットは、個別のプロセススレッドでスクリプトブロックを並列に実行します。 `$using:`キーワードを使用すると、実行中の各スクリプトブロックスレッドに、コマンドレット呼び出しスレッドから変数参照を渡すことができます。 スクリプトブロックは異なるスレッドで実行されるため、参照渡しで渡されるオブジェクト変数は安全に使用する必要があります。 一般に、変更されていない参照先オブジェクトからは安全に読み取ることができます。 ただし、オブジェクトの状態が変更されている場合は、.Net system.string などのスレッドセーフなオブジェクトを使用する必要が **あります (** 例11を参照)。

## 関連リンク

[Compare-Object](../Microsoft.PowerShell.Utility/Compare-Object.md)

[Where-Object](Where-Object.md)

[Group-Object](../Microsoft.PowerShell.Utility/Group-Object.md)

[Measure-Object](../Microsoft.PowerShell.Utility/Measure-Object.md)

[New-Object](../Microsoft.PowerShell.Utility/New-Object.md)

[Select-Object](../Microsoft.PowerShell.Utility/Select-Object.md)

[Sort-Object](../Microsoft.PowerShell.Utility/Sort-Object.md)

[Tee-Object](../Microsoft.PowerShell.Utility/Tee-Object.md)
