---
title: フロー制御
description: PowerShell には、ループを作成し、決定を行い、スクリプト内のコードのフローを論理的に制御するメソッドが用意されています。
ms.date: 06/02/2020
ms.custom: Contributor-mikefrobbins
ms.reviewer: mirobb
ms.openlocfilehash: 74595f8c6a5d4a49b05e72dd6bde1441fd2b464e
ms.sourcegitcommit: df5e6f032ee2d4b556d50406832732d2f7dc2502
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2021
ms.locfileid: "99601787"
---
# <a name="chapter-6---flow-control"></a>第 6 章 - フロー制御

## <a name="scripting"></a>スクリプトの作成

PowerShell のワンライナーの記述からスクリプトの記述に移行するのは、実際よりもはるかに複雑に思えます。 スクリプトは、`.PS1` ファイルとして保存されることを除けば、PowerShell コンソールで対話的に実行するのと同じまたは類似のコマンドにすぎません。 `ForEach-Object` コマンドレットの代わりに `foreach` ループなど、使用できるスクリプト コンストラクトがいくつかあります。 初心者にとってはこの違いがわかりにくく、`foreach` がスクリプト コンストラクトであると同時に `ForEach-Object` コマンドレットのエイリアスでもあることを考えた場合は特にそうです。

## <a name="looping"></a>ループ

PowerShell の優れた点の 1 つとして、1 つの項目に対して何かを実行する方法を理解できれば、何百もの項目に対しても同じタスクを簡単に実行できます。 PowerShell のさまざまな種類のループのいずれかを使用して項目をループ処理するだけです。

### <a name="foreach-object"></a>ForEach-Object

`ForEach-Object` は、PowerShell ワンライナーなどを使用してパイプライン内の項目を反復処理するためのコマンドレットです。 `ForEach-Object` は、パイプラインを介してオブジェクトをストリーム配信します。

`Get-Command` の **Module** パラメーターは、文字列である複数の値を受け取ることができますが、プロパティ名によるパイプライン入力経由またはパラメーター入力経由でのみ、値を受け取ります。 次のシナリオでは、**Module** パラメーターで使用するために 2 つの文字列の値を `Get-Command` にパイプ処理する場合、`ForEach-Object` コマンドレットを使用する必要があります。

```powershell
'ActiveDirectory', 'SQLServer' |
   ForEach-Object {Get-Command -Module $_} |
     Group-Object -Property ModuleName -NoElement |
         Sort-Object -Property Count -Descending
```

```Output
Count Name
----- ----
  147 ActiveDirectory
   82 SqlServer
```

前の例では、`$_` が現在のオブジェクトです。 PowerShell バージョン 3.0 以降では、`$_` の代わりに `$PSItem` を使用できます。 しかし、ほとんどの経験豊富な PowerShell ユーザーの間では、下位互換性があり入力が少ないことから、いまだに `$_` の方が好まれているようです。

`foreach` キーワードを使用する場合は、反復処理するすべての項目を事前にメモリに格納する必要があります。これは、処理する項目の数がわからない場合は扱いが難しくなる可能性があります。

```powershell
$ComputerName = 'DC01', 'WEB01'
foreach ($Computer in $ComputerName) {
  Get-ADComputer -Identity $Computer
}
```

```Output
DistinguishedName : CN=DC01,OU=Domain Controllers,DC=mikefrobbins,DC=com
DNSHostName       : dc01.mikefrobbins.com
Enabled           : True
Name              : DC01
ObjectClass       : computer
ObjectGUID        : c38da20c-a484-469d-ba4c-bab3fb71ae8e
SamAccountName    : DC01$
SID               : S-1-5-21-2989741381-570885089-3319121794-1001
UserPrincipalName :

DistinguishedName : CN=WEB01,CN=Computers,DC=mikefrobbins,DC=com
DNSHostName       : web01.mikefrobbins.com
Enabled           : True
Name              : WEB01
ObjectClass       : computer
ObjectGUID        : 33aa530e-1e31-40d8-8c78-76a18b673c33
SamAccountName    : WEB01$
SID               : S-1-5-21-2989741381-570885089-3319121794-1107
UserPrincipalName :
```

多くの場合、`foreach` や `ForEach-Object` などのループが必要です。 そうしないと、エラー メッセージが表示されます。

```powershell
Get-ADComputer -Identity 'DC01', 'WEB01'
```

```Output
Get-ADComputer : Cannot convert 'System.Object[]' to the type
'Microsoft.ActiveDirectory.Management.ADComputer' required by parameter 'Identity'.
Specified method is not supported.
At line:1 char:26
+ Get-ADComputer -Identity 'DC01', 'WEB01'
+                          ```````````````
    + CategoryInfo          : InvalidArgument: (:) [Get-ADComputer], ParameterBindingExc
   eption
    + FullyQualifiedErrorId : CannotConvertArgument,Microsoft.ActiveDirectory.Management
   .Commands.GetADComputer
```

また、ループを完全に排除して同じ結果を得ることができる場合もあります。 オプションを理解するには、コマンドレットのヘルプを参照してください。

```powershell
'DC01', 'WEB01' | Get-ADComputer
```

```Output
DistinguishedName : CN=DC01,OU=Domain Controllers,DC=mikefrobbins,DC=com
DNSHostName       : dc01.mikefrobbins.com
Enabled           : True
Name              : DC01
ObjectClass       : computer
ObjectGUID        : c38da20c-a484-469d-ba4c-bab3fb71ae8e
SamAccountName    : DC01$
SID               : S-1-5-21-2989741381-570885089-3319121794-1001
UserPrincipalName :

DistinguishedName : CN=WEB01,CN=Computers,DC=mikefrobbins,DC=com
DNSHostName       : web01.mikefrobbins.com
Enabled           : True
Name              : WEB01
ObjectClass       : computer
ObjectGUID        : 33aa530e-1e31-40d8-8c78-76a18b673c33
SamAccountName    : WEB01$
SID               : S-1-5-21-2989741381-570885089-3319121794-1107
UserPrincipalName :
```

前の例でわかるように、`Get-ADComputer` の **Identity** パラメーターは、入力がパラメーター入力を介して提供される場合は単一の値のみを受け取りますが、パイプライン入力を介して提供される場合は複数の項目を受け取ります。

### <a name="for"></a>For

`for` ループでは、指定された条件が true である間、処理が繰り返されます。 私は `for` ループを頻繁には使用しませんが、これには確かな用途があります。

```powershell
for ($i = 1; $i -lt 5; $i++) {
  Write-Output "Sleeping for $i seconds"
  Start-Sleep -Seconds $i
}
```

```Output
Sleeping for 1 seconds
Sleeping for 2 seconds
Sleeping for 3 seconds
Sleeping for 4 seconds
```

前の例では、ループは 4 回 (つまり、番号 1 で始まり、カウンター変数 `$i` が 5 未満の間) 繰り返されます。 合計で 10 秒間スリープ状態になります。

### <a name="do"></a>次のことを行います

PowerShell には、2 つの異なる `do` ループがあります。 `Do Until` は、指定された条件が false の間実行されます。

```powershell
$number = Get-Random -Minimum 1 -Maximum 10
do {
  $guess = Read-Host -Prompt "What's your guess?"
  if ($guess -lt $number) {
    Write-Output 'Too low!'
  }
  elseif ($guess -gt $number) {
    Write-Output 'Too high!'
  }
}
until ($guess -eq $number)
```

```Output
What's your guess?: 1
Too low!
What's your guess?: 2
Too low!
What's your guess?: 3
```

前の例は、推測した数字が `Get-Random` コマンドレットによって生成された数字に一致するまで続く数字ゲームです。

`Do While` は、その逆に作用します。 指定された条件が true と評価される限り、実行されます。

```powershell
$number = Get-Random -Minimum 1 -Maximum 10
do {
  $guess = Read-Host -Prompt "What's your guess?"
  if ($guess -lt $number) {
    Write-Output 'Too low!'
  } elseif ($guess -gt $number) {
    Write-Output 'Too high!'
  }
}
while ($guess -ne $number)
```

```Output
What's your guess?: 1
Too low!
What's your guess?: 2
Too low!
What's your guess?: 3
Too low!
What's your guess?: 4
```

テスト条件を逆の "等しくない" にすることにより、`Do While` ループでも同じ結果が得られます。

条件はループの最後で評価されるため、`Do` ループは少なくとも 1 回は必ず実行されます。

### <a name="while"></a>While

`Do While` ループと同様に、`While` ループは、指定された条件が true である限り実行されます。 ただし、`While` ループではコードが実行される前にループの先頭で条件が評価される点が異なります。 したがって、条件が false と評価された場合は実行されません。

```powershell
$date = Get-Date -Date 'November 22'
while ($date.DayOfWeek -ne 'Thursday') {
  $date = $date.AddDays(1)
}
Write-Output $date
```

```Output
Thursday, November 23, 2017 12:00:00 AM
```

前の例では、米国の感謝祭の日を計算しています。 この日は常に 11 月の第 4 木曜日に該当します。 したがって、このループでは、11 月 22 日を開始日として、曜日が木曜日と等しくなければ 1 日ずつ追加されていきます。 22 日が木曜日の場合、ループはまったく実行されません。

## <a name="break-continue-and-return"></a>Break、Continue、および Return

`Break` は、ループから抜け出すように設計されています。 また、`switch` ステートメントでも一般的に使用されます。

```powershell
for ($i = 1; $i -lt 5; $i++) {
  Write-Output "Sleeping for $i seconds"
  Start-Sleep -Seconds $i
  break
}
```

```Output
Sleeping for 1 seconds
```

前の例に示されている `break` ステートメントにより、ループは最初の反復で終了します。

Continue は、ループの次の反復にスキップするように設計されています。

```powershell
while ($i -lt 5) {
  $i += 1
  if ($i -eq 3) {
    continue
  }
  Write-Output $i
}
```

```Output
1
2
4
5
```

前の例では、番号 1、2、4、および 5 が出力されます。 番号 3 がスキップされ、ループの次の反復に続きます。 `break` と同様に、`continue` は現在の反復のみを除いてループを中断します。 実行は、ループを中断して停止するのではなく、次の反復に続きます。

Return は、既存のスコープを終了するように設計されています。

```powershell
$number = 1..10
foreach ($n in $number) {
  if ($n -ge 4) {
    Return $n
  }
}
```

```Output
4
```

前の例においいて、return は最初の結果を出力した後でループを終了することに注意してください。 私のブログ記事の 1 つで結果のステートメントを詳しく説明しています: ["PowerShell の return キーワード"][]。

## <a name="summary"></a>まとめ

この章では、PowerShell にあるさまざまな種類のループについて学習しました。

## <a name="review"></a>確認

1. `ForEach-Object` コマンドレットと foreach スクリプト コンストラクトの違いは何ですか?
1. Do While または Do Until ループの代わりに While ループを使用する主な利点は何ですか。
1. break ステートメントと continue ステートメントはどのように異なりますか?

## <a name="recommended-reading"></a>推奨トピック

- [ForEach-Object][]
- [about_ForEach][]
- [about_For][]
- [about_Do][]
- [about_While][]
- [about_Break][]
- [about_Continue][]
- [about_Return][]

<!-- link references -->
[ForEach-Object]: /powershell/module/microsoft.powershell.core/foreach-object
[about_ForEach]: /powershell/module/microsoft.powershell.core/about/about_foreach
[about_For]: /powershell/module/microsoft.powershell.core/about/about_for
[about_Do]: /powershell/module/microsoft.powershell.core/about/about_do
[about_While]: /powershell/module/microsoft.powershell.core/about/about_while
[about_Break]: /powershell/module/microsoft.powershell.core/about/about_break
[about_Continue]: /powershell/module/microsoft.powershell.core/about/about_continue
[about_Return]: /powershell/module/microsoft.powershell.core/about/about_return
["PowerShell の return キーワード"]: https://mikefrobbins.com/2015/07/23/the-powershell-return-keyword/
