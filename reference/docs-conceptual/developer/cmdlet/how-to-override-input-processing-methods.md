---
ms.date: 09/13/2016
ms.topic: reference
title: 入力処理メソッドをオーバーライドする方法
description: 入力処理メソッドをオーバーライドする方法
ms.openlocfilehash: 4e8d71a34a1480ce63435ac6cc5dce60d4219c03
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667013"
---
# <a name="how-to-override-input-processing-methods"></a>入力処理メソッドをオーバーライドする方法

これらの例は、コマンドレット内の入力処理メソッドを上書きする方法を示しています。 これらのメソッドは、次の操作を実行するために使用されます。

- コマンドレットによって処理されるすべてのオブジェクトに対して有効な1回限りのスタートアップ操作を実行するには、 [このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) 使用します。 Windows PowerShell ランタイムは、このメソッドを1回だけ呼び出します。

- コマンドレットに渡されたオブジェクトを処理するには、.................. [ProcessRecord](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) メソッドを使用します。 Windows PowerShell ランタイムは、コマンドレットに渡された各オブジェクトに対してこのメソッドを呼び出します。

- 1回限りの処理操作を実行するには、system.servicemodel メソッドを [使用します](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) 。 Windows PowerShell ランタイムは、このメソッドを1回だけ呼び出します。

## <a name="to-override-the-beginprocessing-method"></a>BeginProcessing メソッドをオーバーライドするには

- [システム](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing)の保護されたオーバーライドを宣言してください。

次のクラスは、サンプルメッセージを出力します。 このクラスを使用するには、コマンドレット属性の動詞と名詞を変更し、新しい動詞と名詞を反映するようにクラスの名前を変更します。次に、必要な機能を、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) のオーバーライドメソッドのオーバーライドに追加します。

```csharp
[Cmdlet(VerbsDiagnostic.Test, "BeginProcessingClass")]
public class TestBeginProcessingClassTemplate : Cmdlet
{
  // Override the BeginProcessing method to add preprocessing
  //operations to the cmdlet.
  protected override void BeginProcessing()
  {
    // Replace the WriteObject method with the logic required
    // by your cmdlet. It is used here to generate the following
    // output:
    // "This is a test of the BeginProcessing template."
    WriteObject("This is a test of the BeginProcessing template.");
  }
}
```

## <a name="to-override-the-processrecord-method"></a>ProcessRecord メソッドをオーバーライドするには

- 保護されたオーバーライドを宣言して、このメソッドを [宣言します](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) 。

次のクラスは、サンプルメッセージを出力します。 このクラスを使用するには、コマンドレット属性の動詞と名詞を変更し、新しい動詞と名詞を反映するようにクラスの名前を変更します。次に、必要な機能を、 [このメソッドの](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) オーバーライドに追加します。

```csharp
[Cmdlet(VerbsDiagnostic.Test, "ProcessRecordClass")]
public class TestProcessRecordClassTemplate : Cmdlet
{
    // Override the ProcessRecord method to add processing
    //operations to the cmdlet.
    protected override void ProcessRecord()
    {
        // Replace the WriteObject method with the logic required
        // by your cmdlet. It is used here to generate the following
        // output:
        // "This is a test of the ProcessRecord template."
        WriteObject("This is a test of the ProcessRecord template.");
    }
}

```

## <a name="to-override-the-endprocessing-method"></a>EndProcessing メソッドをオーバーライドするには

- System.object メソッドの保護されたオーバーライドを宣言し[ます。](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing)

次のクラスは、サンプルを出力します。 このクラスを使用するには、コマンドレット属性の動詞と名詞を変更し、新しい動詞と名詞を反映するようにクラスの名前を変更します。次に、必要な機能を、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) のオーバーライドメソッドのオーバーライドに追加します。

```csharp
[Cmdlet(VerbsDiagnostic.Test, "EndProcessingClass")]
public class TestEndProcessingClassTemplate : Cmdlet
{
  // Override the EndProcessing method to add postprocessing
  //operations to the cmdlet.
  protected override void EndProcessing()
  {
    // Replace the WriteObject method with the logic required
    // by your cmdlet. It is used here to generate the following
    // output:
    // "This is a test of the BeginProcessing template."
    WriteObject("This is a test of the EndProcessing template.");
  }
}
```

## <a name="see-also"></a>参照

[システムの管理... コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing)

[システムの管理... コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing)

[システムの管理....................](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
