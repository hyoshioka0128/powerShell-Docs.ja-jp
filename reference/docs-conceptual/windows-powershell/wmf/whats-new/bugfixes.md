---
ms.date: 06/12/2017
title: WMF 5.1 のバグ修正
description: この記事では、WMF 5.1 のリリースで修正されたバグの一覧を示します。
ms.openlocfilehash: 2673860852ecd6e0b6582f6f69076f8c463eeccc
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92660773"
---
# <a name="bug-fixes-in-wmf-51"></a>WMF 5.1 のバグ修正

## <a name="bug-fixes"></a>バグの修正

WMF 5.1 では、次の重要なバグが修正されました。

### <a name="module-auto-discovery-fully-honors-psmodulepath"></a>モジュールの自動検出で PSModulePath が完全に受け入れられる

モジュールの自動検出 (コマンド呼び出し時に Import-Module を明示的に指定しないモジュールの自動的な読み込み) が、WMF 3 で導入されました。 導入時、PowerShell は `$env:PSModulePath` を使用する前に `$PSHome\Modules` のコマンドを確認していました。

WMF 5.1 では、この動作が `$env:PSModulePath` を完全に受け入れるように変更されています。 これにより、PowerShell によって提供されるコマンド (`Get-ChildItem` など) を定義するユーザー作成のモジュールを、自動的に読み込み、組み込みコマンドを正しくオーバーライドできます。

### <a name="file-redirection-no-longer-hard-codes--encoding-unicode"></a>ファイルのリダイレクトで -Encoding Unicode がハードコーディングされなくなった

PowerShell のこれまでのすべてのバージョンでは、ファイル リダイレクト演算子によって使用されるファイル エンコードを制御できませんでした。

WMF 5.1 からは、`$PSDefaultParameterValues` を設定することによってリダイレクトのファイル エンコードを変更できます。

```powershell
$PSDefaultParameterValues["Out-File:Encoding"] = "Ascii"
```

### <a name="fixed-a-regression-in-accessing-members-of-systemreflectiontypeinfo"></a>System.Reflection.TypeInfo のメンバーへのアクセスでのバグ再発の修正

WMF 5.0 で新しく発生したバグにより、`System.Reflection.RuntimeType` のメンバーにアクセスできませんでした (`[int].ImplementedInterfaces` など)。 WMF 5.1 ではこのバグが修正されています。

### <a name="fixed-some-issues-with-com-objects"></a>COM オブジェクトでのいくつかの問題の修正

WMF 5.0 では、COM オブジェクト上のメソッドを呼び出して COM オブジェクトのプロパティにアクセスする新しい COM バインダーが導入されました。 この新しいバインダーによりパフォーマンスが大幅に向上しましたが、バグもいくつか含まれていました。WMF 5.1 ではそれが修正されました。

#### <a name="argument-conversions-were-not-always-performed-correctly"></a>引数の変換が正常に実行されないことがあった

次の例では

```powershell
$obj = New-Object -ComObject WScript.Shell
$obj.SendKeys([char]173)
```

**SendKeys** メソッドは string を受け取りますが、PowerShell では char が string に変換されず、変換は **IDispatch::Invoke** に延期されていました。このメソッドでは、 **VariantChangeType** を使って変換が行われます。 この例では、結果として、期待される **Volume.Mute** キーではなくキー "1"、"7"、"3" が送られます。

#### <a name="enumerable-com-objects-not-always-handled-correctly"></a>列挙可能な COM オブジェクトが正しく処理されないことがある

PowerShell は通常ほとんどの列挙可能なオブジェクトを列挙しますが、WMF 5.0 で発生したバグにより、IEnumerable を実装する COM オブジェクトの列挙が行われませんでした。 次に例を示します。

```powershell
function Get-COMDictionary
{
    $d = New-Object -ComObject Scripting.Dictionary
    $d.Add('a', 2)
    $d.Add('b', 2)
    return $d
}

$x = Get-COMDictionary
```

上の例の WMF 5.0 では、キーと値のペアは列挙されず、誤って **Scripting.Dictionary** がパイプラインに書き込まれます。

### <a name="ordered-was-not-allowed-inside-classes"></a>[ordered] がクラス内で許可されなかった

WMF 5.0 では、クラスで使用されるリテラル形を検証するクラスが導入されました。 `[ordered]` はリテラル形のように見えますが、真の .Net 型ではありません。 WMF 5.0 は、クラスの内の `[ordered]` で誤ってエラーを報告しました。

```powershell
class CThing
{
    [object] foo($i)
    {
        [ordered]@{ Thing = $i }
    }
}
```

### <a name="help-on-about-topics-with-multiple-versions-does-not-work"></a>複数のバージョンがあるトピックについてのヘルプが機能しない

WMF 5.1 より前では、複数のバージョンのモジュールがインストールされていて、そのすべてがヘルプ トピック (about_PSReadline など) を共有している場合、`help about_PSReadline` は複数のトピックを返し、実際のヘルプを表示する明確な方法はありませんでした。

WMF 5.1 では、最新バージョンのトピックのヘルプを返すことでこれが解決されています。

`Get-Help` では、必要なヘルプのバージョンを指定する方法はありません。 これを回避するには、モジュール ディレクトリに移動し、好みのエディターなどのツールで直接ヘルプを表示します。

### <a name="powershellexe-reading-from-stdin-stopped-working"></a>STDIN からの powershell.exe の読み込みが停止する

お客様はネイティブ アプリから `powershell -command -` を使って PowerShell を実行し、STDIN でスクリプトに渡していましたが、コンソール ホストでの他の変更によってこれが壊れていました。

これは、Windows 10 Anniversary Update のバージョン 5.1 で修正されます。

### <a name="powershellexe-creates-spike-in-cpu-usage-on-startup"></a>powershell.exe の起動時に CPU 使用率が急増する

PowerShell は、ログインの遅延を回避するために WMI クエリを使用して、グループ ポリシーから起動したかどうかを確認します。 WMI の **Win32_Process** クラスはローカル タイムゾーンの情報を取得しようとするため、WMI クエリによって、最終的にシステムのすべてのプロセスに tzres.mui.dll が挿入されます。 結果として、 **wmiprvse** (WMI プロバイダーのホスト) で大きな CPU 使用率の急増が発生します。 修正プログラムでは、WMI を使用する代わりに、Win32 API 呼び出しを使用して同じ情報を取得します。
