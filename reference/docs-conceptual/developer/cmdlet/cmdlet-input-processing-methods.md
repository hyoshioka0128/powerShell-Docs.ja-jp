---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットの入力処理メソッド
description: コマンドレットの入力処理メソッド
ms.openlocfilehash: e1a7b58517d6285250edbf16d14810388c242218
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653394"
---
# <a name="cmdlet-input-processing-methods"></a>コマンドレットの入力処理メソッド

コマンドレットは、このトピックで説明されている1つ以上の入力処理方法をオーバーライドして作業を実行する必要があります。
これらのメソッドを使用すると、コマンドレットは、前処理、入力処理、および後処理の操作を実行できます。
これらのメソッドを使用すると、コマンドレットの処理を停止することもできます。
これらのメソッドの使用方法の詳細な例については、「 [Selectstr チュートリアル](selectstr-tutorial.md)」を参照してください。

## <a name="pre-processing-operations"></a>処理前の操作

コマンドレットは、後でコマンドレットによって処理されるすべてのレコードに対して有効なすべての前処理操作を追加するように、 [このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) オーバーライドする必要があります。
PowerShell がコマンドパイプラインを処理するとき、PowerShell は、パイプラインのコマンドレットの各インスタンスに対してこのメソッドを1回呼び出します。
PowerShell がコマンドパイプラインを呼び出す方法の詳細については、「コマンド [レット処理のライフサイクル](/previous-versions/ms714429(v=vs.85))」を参照してください。

次のコードは、BeginProcessing メソッドの実装を示しています。

```csharp
protected override void BeginProcessing()
{
  // Replace the WriteObject method with the logic required by your cmdlet.
  WriteObject("This is a test of the BeginProcessing template.");
}
```

## <a name="input-processing-operations"></a>入力処理操作

コマンドレットでは [、コマンド](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) レットに送信される入力を処理するために、コマンドレットをオーバーライドできます。
PowerShell はコマンドパイプラインを処理するときに、コマンドレットによって処理される各入力レコードに対してこのメソッドを呼び出します。
PowerShell がコマンドパイプラインを呼び出す方法の詳細については、「コマンド [レット処理のライフサイクル](/previous-versions/ms714429(v=vs.85))」を参照してください。

次のコードは、ProcessRecord メソッドの実装を示しています。

```csharp
protected override void ProcessRecord()
{
  // Replace the WriteObject method with the logic required by your cmdlet.
  WriteObject("This is a test of the ProcessRecord template.");
}
```

## <a name="post-processing-operations"></a>処理後の操作

コマンドレットは、コマンドレットによって処理されたすべてのレコードに対して有効なすべての後処理操作を追加するように、 [このメソッドを](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) オーバーライドする必要があります。
たとえば、処理が完了した後に、コマンドレットでオブジェクト変数のクリーンアップが必要になる場合があります。

PowerShell がコマンドパイプラインを処理するとき、PowerShell は、パイプラインのコマンドレットの各インスタンスに対してこのメソッドを1回呼び出します。
ただし、コマンドレットが入力処理の途中で取り消された場合、またはコマンドレットのいずれかの部分で終了エラーが発生した場合は、PowerShell ランタイムが EndProcessing メソッドを呼び出さないことに注意する必要があります。
このため、オブジェクトのクリーンアップを必要とするコマンドレットは、ファイナライザーを含む完全な [IDisposable](/dotnet/api/System.IDisposable) パターンを実装する必要があります。これにより、処理の終了時にランタイムが EndProcessing メソッドと [system.servicemodel メソッドの両方を呼び](/dotnet/api/System.IDisposable.Dispose) 出すことができるようになります。
PowerShell がコマンドパイプラインを呼び出す方法の詳細については、「コマンド [レット処理のライフサイクル](/previous-versions/ms714429(v=vs.85))」を参照してください。

次のコードは、EndProcessing メソッドの実装を示しています。

```csharp
protected override void EndProcessing()
{
  // Replace the WriteObject method with the logic required by your cmdlet.
  WriteObject("This is a test of the EndProcessing template.");
}
```

## <a name="see-also"></a>参照

[システムの管理... コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing)

[システムの管理....................](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)

[システムの管理... コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing)

[SelectStr チュートリアル](selectstr-tutorial.md)

[System.IDisposable](/dotnet/api/System.IDisposable)

[Windows PowerShell シェル SDK](../windows-powershell-reference.md)
