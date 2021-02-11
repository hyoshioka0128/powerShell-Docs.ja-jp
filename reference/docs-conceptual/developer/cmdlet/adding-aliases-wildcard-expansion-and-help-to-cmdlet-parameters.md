---
ms.date: 09/13/2016
ms.topic: reference
title: エイリアス、ワイルドカード展開、ヘルプをコマンドレット パラメーターに追加する
description: エイリアス、ワイルドカード展開、ヘルプをコマンドレット パラメーターに追加する
ms.openlocfilehash: f0f07796370b4613b1ca0ad17b16c6598bfa438d
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660653"
---
# <a name="adding-aliases-wildcard-expansion-and-help-to-cmdlet-parameters"></a>エイリアス、ワイルドカード展開、ヘルプをコマンドレット パラメーターに追加する

このセクションでは、Stop-Proc コマンドレットのパラメーターにエイリアス、ワイルドカード拡張、およびヘルプメッセージを追加する方法について説明します (「 [システムを変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください)。

この Stop-Proc コマンドレットは、Get-Proc コマンドレットを使用して取得されたプロセスを停止しようとします (「 [最初のコマンドレットを作成する](./creating-a-cmdlet-without-parameters.md)」を参照)。

## <a name="defining-the-cmdlet"></a>コマンドレットの定義

コマンドレットの作成の最初の手順では、常にコマンドレットに名前を付け、コマンドレットを実装する .NET クラスを宣言します。 システムを変更するためのコマンドレットを記述しているので、それに応じた名前を付ける必要があります。 このコマンドレットはシステムプロセスを停止するため、プロセスを示すために "Proc" という名詞を使用して、 [Verbslifecycle](/dotnet/api/System.Management.Automation.VerbsLifeCycle) クラスで定義された動詞 "Stop" を使用します。 承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

次のコードは、この Stop-Proc コマンドレットのクラス定義です。

```csharp
[Cmdlet(VerbsLifecycle.Stop, "proc",
        SupportsShouldProcess = true)]
public class StopProcCommand : Cmdlet
```

## <a name="defining-parameters-for-system-modification"></a>システム変更のパラメーターの定義

コマンドレットでは、システムの変更とユーザーフィードバックをサポートするパラメーターを定義する必要があります。 コマンドレットでは、 `Name` パラメーターまたは同等のものを定義して、コマンドレットが何らかの種類の識別子でシステムを変更できるようにする必要があります。 さらに、コマンドレットでは、パラメーターとパラメーターを定義する必要があり `Force` `PassThru` ます。 これらのパラメーターの詳細については、「 [システムを変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください。

## <a name="defining-a-parameter-alias"></a>パラメーターの別名の定義

パラメーターエイリアスは、コマンドレットパラメーターの代替名または適切に定義された1文字または2文字の短い名前にすることができます。 どちらの場合も、エイリアスを使用する目的は、コマンドラインからのユーザーエントリを簡略化することです。 Windows PowerShell では、宣言構文 [Alias ()] を使用する、system.string [属性のパラメーター](/dotnet/api/System.Management.Automation.AliasAttribute) エイリアスがサポートされています。

次のコードは、パラメーターにエイリアスを追加する方法を示して `Name` います。

```csharp
/// <summary>
/// Specify the mandatory Name parameter used to identify the
/// processes to be stopped.
/// </summary>
[Parameter(
           Position = 0,
           Mandatory = true,
           ValueFromPipeline = true,
           ValueFromPipelineByPropertyName = true,
           HelpMessage = "The name of one or more processes to stop. Wildcards are permitted."
)]
[Alias("ProcessName")]
public string[] Name
{
  get { return processNames; }
  set { processNames = value; }
}
private string[] processNames;
```

Windows PowerShell ランタイムでは、 [alias 属性を](/dotnet/api/System.Management.Automation.AliasAttribute) 使用するだけでなく、別名が指定されていない場合でも、部分的な名前の一致が実行されます。 たとえば、コマンドレットにパラメーターがあり、 `FileName` で始まる唯一のパラメーターである場合、 `F` ユーザーは、、、 `Filename` `Filenam` `File` `Fi` 、、またはを入力 `F` しても、パラメーターとしてエントリを認識でき `FileName` ます。

## <a name="creating-help-for-parameters"></a>パラメーターのヘルプの作成

Windows PowerShell では、コマンドレットパラメーターのヘルプを作成できます。 この操作は、システムの変更やユーザーフィードバックに使用される任意のパラメーターに対して行います。 ヘルプをサポートする各パラメーターに対して、属性キーワードを設定することができます。この属性は、属性 `HelpMessage` 宣言で[](/dotnet/api/System.Management.Automation.ParameterAttribute)指定します。 このキーワードは、パラメーターを使用する際の参考としてユーザーに表示するテキストを定義します。 また、キーワードを設定して、 `HelpMessageBaseName` メッセージに使用するリソースの基本名を指定することもできます。 このキーワードを設定する場合は、 `HelpMessageResourceId` リソース識別子を指定するようにキーワードも設定する必要があります。

この Stop-Proc コマンドレットの次のコードは、 `HelpMessage` パラメーターの属性キーワードを定義し `Name` ます。

```csharp
/// <summary>
/// Specify the mandatory Name parameter used to identify the
/// processes to be stopped.
/// </summary>
[Parameter(
           Position = 0,
           Mandatory = true,
           ValueFromPipeline = true,
           ValueFromPipelineByPropertyName = true,
           HelpMessage = "The name of one or more processes to stop. Wildcards are permitted."
)]
```

## <a name="overriding-an-input-processing-method"></a>入力処理メソッドのオーバーライド

コマンドレットでは、入力処理メソッドをオーバーライドする必要があります。これは、ほとんどの場合、このコマンド [レット](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)を使用します。 システムを変更する場合、コマンドレットは、変更を加える前にユーザーがフィードバックを提供できるようにするために、コマンドレットに[よってメソッド](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)を呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) これらの方法の詳細については、「 [システムを変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください。

## <a name="supporting-wildcard-expansion"></a>ワイルドカード拡張のサポート

複数のオブジェクトを選択できるようにするには、コマンドレットで [Wildcardpattern](/dotnet/api/System.Management.Automation.WildcardPattern) クラスと [Wildcardoptions](/dotnet/api/System.Management.Automation.WildcardOptions) クラスを使用して、パラメーター入力のワイルドカード拡張サポートを提供します。 ワイルドカードパターンの例としては、lsa *、 \* .txt、および [a-c] が \* あります。 パターンに文字どおりに使用する必要がある文字が含まれている場合は、二重引用符文字 (') をエスケープ文字として使用します。

ファイル名とパス名のワイルドカードによる展開は、複数のオブジェクトを選択する必要がある場合に、コマンドレットでパス入力のサポートを許可する一般的なシナリオの例です。 通常、ファイルシステムには、現在のフォルダーに存在するすべてのファイルが表示されます。

カスタマイズされたワイルドカードパターン一致の実装が必要になることはほとんどありません。 この場合、コマンドレットは、完全な POSIX 1003.2、3.13 のワイルドカードの展開、または次の簡略化されたサブセットのいずれかをサポートする必要があります。

- **疑問符 (?)。** 指定した位置にある任意の文字と一致します。

- **アスタリスク ( \* )。** 指定した位置から始まる0個以上の文字と一致します。

- **角かっこ ([) を開きます。** 文字または文字の範囲を含めることができるパターン角かっこ式を導入します。 範囲が必要な場合は、範囲を示すためにハイフン (-) が使用されます。

- **閉じかっこ (])。** パターン角かっこ表現を終了します。

- **バッククォートエスケープ文字 (')。** 次の文字を文字どおりに取得する必要があることを示します。 プログラムによって指定するのではなく、コマンドラインからバッククォート文字を指定する場合は、バッククォートのエスケープ文字を2回指定する必要があることに注意してください。

> [!NOTE]
> ワイルドカードパターンの詳細については、「 [コマンドレットパラメーターでのワイルドカードのサポート](./supporting-wildcard-characters-in-cmdlet-parameters.md)」を参照してください。

次のコードは、ワイルドカードオプションを設定し、このコマンドレットのパラメーターの解決に使用するワイルドカードパターンを定義する方法を示して `Name` います。

```csharp
WildcardOptions options = WildcardOptions.IgnoreCase |
                          WildcardOptions.Compiled;
WildcardPattern wildcard = new WildcardPattern(name,options);
```

次のコードは、プロセス名が定義されたワイルドカードパターンに一致するかどうかをテストする方法を示しています。 この場合、プロセス名がパターンと一致しない場合、コマンドレットは次のプロセス名を取得し続けます。

```csharp
if (!wildcard.IsMatch(processName))
{
  continue;
}
```

## <a name="code-sample"></a>コード サンプル

完全な C# サンプルコードについては、「 [StopProcessSample03 sample](./stopprocesssample03-sample.md)」を参照してください。

## <a name="define-object-types-and-formatting"></a>オブジェクトの種類と書式を定義する

Windows PowerShell は、.Net オブジェクトを使用してコマンドレット間で情報を渡します。 そのため、コマンドレットで独自の型を定義する必要がある場合や、コマンドレットで別のコマンドレットによって提供される既存の型を拡張する必要がある場合があります。 新しい型を定義する、または既存の型を拡張する方法の詳細については、「 [オブジェクトの型と書式設定の拡張](/previous-versions//ms714665(v=vs.85))」を参照してください。

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後は、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが Windows PowerShell に登録されている場合は、コマンドラインで実行することでテストできます。 サンプル Stop-Proc コマンドレットをテストしてみましょう。 コマンドラインからコマンドレットを使用する方法の詳細については、「 [Windows PowerShell を使用したはじめに](/powershell/scripting/getting-started/getting-started-with-windows-powershell)」を参照してください。

- Windows PowerShell を起動し、Stop-Proc を使用して、パラメーターの ProcessName エイリアスを使用してプロセスを停止し `Name` ます。

    ```powershell
    PS> stop-proc -ProcessName notepad
    ```

    次のような出力が表示されます。

    ```
    Confirm
    Are you sure you want to perform this action?
    Performing operation "stop-proc" on Target "notepad (3496)".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
    ```

- コマンドラインで次のエントリを作成します。 Name パラメーターは必須であるため、入力を求められます。 「!?」と入力します。 パラメーターに関連付けられたヘルプテキストを表示します。

    ```powershell
    PS> stop-proc
    ```

    次のような出力が表示されます。

    ```
    Cmdlet stop-proc at command pipeline position 1
    Supply values for the following parameters:
    (Type !? for Help.)
    Name[0]: !?
    The name of one or more processes to stop. Wildcards are permitted.
    Name[0]: notepad
    ```

- ここで、ワイルドカードパターン "* note" に一致するすべてのプロセスを停止するには、次のエントリを作成し \* ます。 パターンに一致する各プロセスを停止する前に、メッセージが表示されます。

    ```powershell
    PS> stop-proc -Name *note*
    ```

    次のような出力が表示されます。

    ```
    Confirm
    Are you sure you want to perform this action?
    Performing operation "stop-proc" on Target "notepad (1112)".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
    ```

    次のような出力が表示されます。

    ```
    Confirm
    Are you sure you want to perform this action?
    Performing operation "stop-proc" on Target "ONENOTEM (3712)".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): N
    ```

    次のような出力が表示されます。

    ```
    Confirm
    Are you sure you want to perform this action?
    Performing operation "stop-proc" on Target "ONENOTE (3592)".
    [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): N
    ```

## <a name="see-also"></a>参照

[システムを変更するコマンドレットを作成する](./creating-a-cmdlet-that-modifies-the-system.md)

[Windows PowerShell コマンドレットを作成する方法](/powershell/scripting/developer/cmdlet/writing-a-windows-powershell-cmdlet)

[オブジェクトの種類と書式設定の拡張](/previous-versions//ms714665(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))

[コマンドレットパラメーターでのワイルドカードのサポート](./supporting-wildcard-characters-in-cmdlet-parameters.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
