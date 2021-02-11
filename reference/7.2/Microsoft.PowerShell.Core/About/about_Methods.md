---
description: メソッドを使用して PowerShell のオブジェクトに対してアクションを実行する方法について説明します。
Locale: en-US
ms.date: 04/08/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_methods?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Methods
ms.openlocfilehash: 35eaafcec5d645be6fe88f506bc0971344406273
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602783"
---
# <a name="about-methods"></a>メソッドの概要

## <a name="short-description"></a>簡単な説明
メソッドを使用して PowerShell のオブジェクトに対してアクションを実行する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell はオブジェクトを使用して、データストア内の項目またはコンピューターの状態を表します。 たとえば、FileInfo オブジェクトはファイルシステムドライブ内のファイルを表し、ProcessInfo オブジェクトはコンピューター上のプロセスを表します。

オブジェクトには、オブジェクトに関するデータを格納するプロパティと、オブジェクトを変更するためのメソッドがあります。

"メソッド" は、オブジェクトに対して実行できるアクションを指定する命令のセットです。 たとえば、オブジェクトには、 `FileInfo` オブジェクトが表すファイルをコピーするメソッドが含まれてい `CopyTo` `FileInfo` ます。

任意のオブジェクトのメソッドを取得するには、コマンドレットを使用し `Get-Member` ます。 **MemberType** プロパティは、"Method" という値で使用します。 次のコマンドは、プロセスオブジェクトのメソッドを取得します。

```powershell
Get-Process | Get-Member -MemberType Method
```

```Output
TypeName: System.Diagnostics.Process

Name                      MemberType Definition
----                      ---------- ----------
BeginErrorReadLine        Method     System.Void BeginErrorReadLine()
BeginOutputReadLine       Method     System.Void BeginOutputReadLine()
...
Kill                      Method     System.Void Kill()
Refresh                   Method     System.Void Refresh()
Start                     Method     bool Start()
ToString                  Method     string ToString()
WaitForExit               Method     bool WaitForExit(int milliseconds), ...
WaitForInputIdle          Method     bool WaitForInputIdle(int millisecon...
```

オブジェクトのメソッドを実行または "呼び出す" には、ドット (.)、メソッド名、および一連のかっこ "()" を入力します。 メソッドに引数がある場合は、引数の値をかっこ内に配置します。 引数がない場合でも、すべてのメソッド呼び出しにかっこが必要です。 メソッドが複数の引数を受け取る場合は、コンマで区切る必要があります。

たとえば、次のコマンドは、プロセスの Kill メソッドを呼び出して、コンピューター上のメモ帳プロセスを終了します。

```powershell
$notepad = Get-Process notepad
$notepad.Kill()
```

この例は、上記のステートメントを組み合わせることによって短縮できます。

```powershell
(Get-Process Notepad).Kill()
```

コマンドは、 `Get-Process` Kill メソッドが呼び出される前に実行されるように、かっこで囲まれています。 `Kill`次に、返されたオブジェクトに対してメソッドが呼び出され `Process` ます。

もう1つの便利な方法は、 `Replace` 文字列のメソッドです。 `Replace`メソッドは、文字列内のテキストを置換します。 次の例では、文字列の末尾の引用符の直後にドット (.) を配置できます。

```powershell
'this is rocket science'.Replace('rocket', 'rock')
```

```Output
this is rock science
```

前の例で示したように、コマンド、変数内のオブジェクト、またはオブジェクト (引用符で囲まれた文字列など) を生成するすべてのオブジェクトに対して、メソッドを呼び出すことができます。

PowerShell 4.0 以降では、動的なメソッド名を使用したメソッド呼び出しがサポートされています。

### <a name="learning-about-methods"></a>メソッドについて学習する

オブジェクトのメソッドの定義を検索するには、オブジェクトの種類のヘルプトピックにアクセスして、そのメソッドのページを探します。 たとえば、次のページでは、process オブジェクトのメソッドについて説明して[います。](/dotnet/api/system.diagnostics.process#methods)

メソッドの引数を確認するには、メソッドの定義を確認します。これは、PowerShell コマンドレットの構文ダイアグラムに似ています。

メソッド定義には、PowerShell コマンドレットのパラメーターセットに似た1つ以上のメソッドシグネチャが含まれている場合があります。 シグネチャは、メソッドを呼び出すためのすべての有効な形式のコマンドを示しています。

たとえば、 `CopyTo` クラスのメソッドには、 `FileInfo` 次の2つのメソッドシグネチャが含まれています。

```
    CopyTo(String destFileName)
    CopyTo(String destFileName, Boolean overwrite)
```

最初のメソッドシグネチャは、コピー先のファイル名 (およびパス) を受け取ります。 次の例では、最初のメソッドを使用して、 `CopyTo` `Final.txt` ファイルをディレクトリにコピーし `C:\Bin` ます。

```powershell
(Get-ChildItem c:\final.txt).CopyTo("c:\bin\final.txt")
```

> [!NOTE]
> PowerShell の _引数_ モードとは異なり、オブジェクトメソッドは、powershell が構築されている .net framework へのパススルーである _式_ モードで実行されます。 _式_ モードでは、 **bareword** の引数 (引用符で囲まれていない文字列) は使用できません。 パスをパラメーターとして使用する場合は、パスを引数として使用する場合に、この違いを確認できます。 解析モードの詳細については、 [about_Parsing](about_Parsing.md)を参照してください。

2番目のメソッドシグネチャは、コピー先ファイル名と、コピー先ファイルが既に存在する場合に上書きするかどうかを決定するブール値を受け取ります。

次の例では、2番目のメソッドを使用して、 `CopyTo` `Final.txt` ファイルをディレクトリにコピーし、既存の `C:\Bin` ファイルを上書きします。

```powershell
(Get-ChildItem c:\final.txt).CopyTo("c:\bin\final.txt", $true)
```

### <a name="methods-of-scalar-objects-and-collections"></a>スカラーオブジェクトとコレクションのメソッド

特定の型の1つの ("スカラー") オブジェクトのメソッドは、多くの場合、同じ型のオブジェクトのコレクションのメソッドとは異なります。

たとえば、すべてのプロセスにメソッドがあり `Kill` ますが、プロセスのコレクションには Kill メソッドがありません。

PowerShell 3.0 以降、PowerShell は、スカラーオブジェクトとコレクションの異なるメソッドに起因するスクリプトエラーを防止しようとします。

コレクションを送信するが、1つの ("スカラー") オブジェクトにのみ存在するメソッドを要求する場合、PowerShell はコレクション内のすべてのオブジェクトに対してメソッドを呼び出します。

メソッドが個別のオブジェクトとコレクションに存在する場合は、コレクションのメソッドだけが呼び出されます。

この機能は、スカラーオブジェクトおよびコレクションのプロパティにも使用できます。 詳細については、「 [about_Properties](about_Properties.md)」を参照してください。

### <a name="examples"></a>例

次の例では、オブジェクトのコレクション内の個々のプロセスオブジェクトの **Kill** メソッドを実行します。

最初のコマンドは、メモ帳プロセスの3つのインスタンスを開始します。 `Get-Process` メモ帳プロセスの3つのインスタンスをすべて取得し、変数に保存し `$p` ます。

```powershell
Notepad; Notepad; Notepad
$p = Get-Process Notepad
$p.Count
```

```Output
3
```

次のコマンドは、変数内の3つのプロセスすべてで **Kill** メソッドを実行し `$p` ます。 このコマンドは、プロセスのコレクションにメソッドがない場合でも機能 `Kill` します。

```powershell
$p.Kill()
Get-Process Notepad
```

コマンドは、 `Get-Process` メソッドが動作したことを確認し `Kill` ます。

```Output
Get-Process : Cannot find a process with the name "notepad". Verify the proc
ess name and call the cmdlet again.
At line:1 char:12
+ Get-Process <<<<  notepad
    + CategoryInfo          : ObjectNotFound: (notepad:String) [Get-Process]
, ProcessCommandException
    + FullyQualifiedErrorId : NoProcessFoundForGivenName,Microsoft.PowerShel
l.Commands.GetProcessCommand
```

この例は機能的には、コマンドレットを使用して、 `Foreach-Object` コレクション内の各オブジェクトに対してメソッドを実行することと同じです。

```powershell
$p | ForEach-Object {$_.Kill()}
```

### <a name="foreach-and-where-methods"></a>ForEach および Where メソッド

PowerShell 4.0 以降では、メソッド構文を使用したコレクションのフィルター処理がサポートされています。 これにより、コレクションとを処理するときに、2つの新しいメソッドを使用できるように `ForEach` `Where` なります。

これらのメソッドの詳細については、「[about_arrays](about_arrays.md)」を参照してください。

### <a name="calling-a-specific-method-when-multiple-overloads-exist"></a>複数のオーバーロードが存在する場合の特定のメソッドの呼び出し

.NET メソッドを呼び出すときは、次のシナリオを考慮してください。 メソッドがオブジェクトを受け取り、より具体的な型を取得するインターフェイスを介してオーバーロードを持つ場合、PowerShell は、明示的にそのインターフェイスにキャストしない限り、オブジェクトを受け入れるメソッドを選択します。

```powershell
Add-Type -TypeDefinition @'

   // Interface
   public interface IFoo {
     string Bar(int p);
   }

   // Type that implements the interface
   public class Foo : IFoo {

   // Direct member method named 'Bar'
   public string Bar(object p) { return $"object: {p}"; }

   // *Explicit* implementation of IFoo's 'Bar' method().
   string IFoo.Bar(int p) {
       return $"int: {p}";
   }

}
'@
```

この例では、 `object` **Bar** メソッドのより限定的なオーバーロードが選択されています。

```powershell
[Foo]::new().Bar(1)
```

```Output
object: 1
```

この例では、メソッドをインターフェイス **IFoo** にキャストして、 **Bar** メソッドのより具体的なオーバーロードを選択します。

```powershell
([IFoo] [Foo]::new()).Bar(1)
```

```Output
int: 1
```

## <a name="see-also"></a>参照

[about_Objects](about_Objects.md)

[about_Properties](about_Properties.md)

[Get-Member](xref:Microsoft.PowerShell.Utility.Get-Member)

