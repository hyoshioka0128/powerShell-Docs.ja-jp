---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレットにパラメーター セットを追加する
description: コマンドレットにパラメーター セットを追加する
ms.openlocfilehash: dd5ee2a880a4d516ea82e5afe0ced12369197243
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92648660"
---
# <a name="adding-parameter-sets-to-a-cmdlet"></a>コマンドレットにパラメーター セットを追加する

## <a name="things-to-know-about-parameter-sets"></a>パラメーターセットについて理解しておくべきこと

Windows PowerShell では、パラメーターセットを、一緒に動作するパラメーターのグループとして定義します。 コマンドレットのパラメーターをグループ化することによって、ユーザーが指定するパラメーターのグループに基づいて機能を変更できる単一のコマンドレットを作成できます。

2つのパラメーターセットを使用して異なる機能を定義するコマンドレットの例とし `Get-EventLog` て、Windows PowerShell によって提供されるコマンドレットがあります。 このコマンドレットは、ユーザーがまたはパラメーターを指定すると、異なる情報を返し `List` `LogName` ます。 パラメーターを指定した場合 `LogName` 、コマンドレットは、特定のイベントログ内のイベントに関する情報を返します。 パラメーターを指定した場合 `List` 、コマンドレットは、ログファイル自体に関する情報 (含まれるイベント情報ではありません) を返します。 この場合、 `List` `LogName` パラメーターとパラメーターは2つの異なるパラメーターセットを識別します。

パラメーターセットについて覚えておく必要がある2つの重要な点は、Windows PowerShell ランタイムでは特定の入力に対して1つのパラメーターセットだけを使用し、各パラメーターセットには、そのパラメーターセットに対して一意のパラメーターを1つ以上含める必要があることです。

最後の点を示すために、この Stop-Proc コマンドレットでは `ProcessName` 、、、およびの3つのパラメーターセットを使用し `ProcessId` `InputObject` ます。 これらの各パラメーターセットには、他のパラメーターセットに含まれていないパラメーターが1つあります。 パラメーターセットは他のパラメーターを共有できますが、コマンドレットは、一意のパラメーター、、およびを使用して、 `ProcessName` `ProcessId` `InputObject` Windows PowerShell ランタイムが使用するパラメーターのセットを識別します。

## <a name="declaring-the-cmdlet-class"></a>コマンドレットクラスの宣言

コマンドレットの作成の最初の手順では、常にコマンドレットに名前を付け、コマンドレットを実装する .NET クラスを宣言します。 このコマンドレットでは、コマンドレットがシステムプロセスを停止するため、ライフサイクル動詞 "Stop" が使用されます。 名詞名 "Proc" が使用されるのは、コマンドレットがプロセスで動作するためです。 次の宣言では、コマンドレット動詞と名詞名がコマンドレットクラスの名前に反映されていることに注意してください。

> [!NOTE]
> 承認されたコマンドレットの動詞名の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

次のコードは、この Stop-Proc コマンドレットのクラス定義です。

```csharp
[Cmdlet(VerbsLifecycle.Stop, "Proc",
        DefaultParameterSetName = "ProcessId",
        SupportsShouldProcess = true)]
public class StopProcCommand : PSCmdlet
```

```vb
<Cmdlet(VerbsLifecycle.Stop, "Proc", DefaultParameterSetName:="ProcessId", _
SupportsShouldProcess:=True)> _
Public Class StopProcCommand
    Inherits PSCmdlet
```

## <a name="declaring-the-parameters-of-the-cmdlet"></a>コマンドレットのパラメーターの宣言

このコマンドレットは、コマンドレットへの入力として必要な3つのパラメーター (これらのパラメーターもパラメーターセットを定義します) と、コマンド `Force` `PassThru` レットがパイプラインを介して出力オブジェクトを送信するかどうかを決定するパラメーターを定義します。 既定では、このコマンドレットはパイプラインを介してオブジェクトを渡しません。 これらの最後の2つのパラメーターの詳細については、「 [システムを変更するコマンドレットを作成](./creating-a-cmdlet-that-modifies-the-system.md)する」を参照してください。

### <a name="declaring-the-name-parameter"></a>Name パラメーターの宣言

この入力パラメーターを使用すると、停止するプロセスの名前をユーザーが指定できます。 `ParameterSetName`このパラメーターに設定されているパラメーターを指定することに注意して[ください。](/dotnet/api/System.Management.Automation.ParameterAttribute) `ProcessName`

:::code language="csharp" source="~/../powershell-sdk-samples/SDK-2.0/csharp/StopProcessSample04/StopProcessSample04.cs" range="44-58":::

```vb
<Parameter(Position:=0, ParameterSetName:="ProcessName", _
Mandatory:=True, _
ValueFromPipeline:=True, ValueFromPipelineByPropertyName:=True, _
HelpMessage:="The name of one or more processes to stop. " & _
    "Wildcards are permitted."), [Alias]("ProcessName")> _
Public Property Name() As String()
    Get
        Return processNames
    End Get
    Set(ByVal value As String())
        processNames = value
    End Set
End Property

Private processNames() As String
```

また、エイリアス "ProcessName" がこのパラメーターに指定されていることにも注意してください。

### <a name="declaring-the-id-parameter"></a>Id パラメーターの宣言

この入力パラメーターを使用すると、停止するプロセスの識別子をユーザーが指定できます。 System.object 属性 `ParameterSetName` の attribute キーワードによっ[](/dotnet/api/System.Management.Automation.ParameterAttribute)て、パラメーターセットが指定されていることに注意して `ProcessId` ください。

```csharp
[Parameter(
           ParameterSetName = "ProcessId",
           Mandatory = true,
           ValueFromPipelineByPropertyName = true,
           ValueFromPipeline = true
)]
[Alias("ProcessId")]
public int[] Id
{
  get { return processIds; }
  set { processIds = value; }
}
private int[] processIds;
```

```vb
<Parameter(ParameterSetName:="ProcessId", _
Mandatory:=True, _
ValueFromPipelineByPropertyName:=True, _
ValueFromPipeline:=True), [Alias]("ProcessId")> _
Public Property Id() As Integer()
    Get
        Return processIds
    End Get
    Set(ByVal value As Integer())
        processIds = value
    End Set
End Property
Private processIds() As Integer
```

また、エイリアス "ProcessId" がこのパラメーターに指定されていることにも注意してください。

### <a name="declaring-the-inputobject-parameter"></a>InputObject パラメーターの宣言

この入力パラメーターを使用すると、停止するプロセスに関する情報を含む入力オブジェクトをユーザーが指定できます。 `ParameterSetName`このパラメーターに設定されているパラメーターを指定することに注意して[ください。](/dotnet/api/System.Management.Automation.ParameterAttribute) `InputObject`

```csharp
[Parameter(
           ParameterSetName = "InputObject",
           Mandatory = true,
           ValueFromPipeline = true)]
public Process[] InputObject
{
  get { return inputObject; }
  set { inputObject = value; }
}
private Process[] inputObject;
```

```vb
<Parameter(ParameterSetName:="InputObject", _
Mandatory:=True, ValueFromPipeline:=True)> _
Public Property InputObject() As Process()
    Get
        Return myInputObject
    End Get
    Set(ByVal value As Process())
        myInputObject = value
    End Set
End Property
Private myInputObject() As Process
```

また、このパラメーターにエイリアスが含まれていないことにも注意してください。

### <a name="declaring-parameters-in-multiple-parameter-sets"></a>複数のパラメーターセットでのパラメーターの宣言

パラメーターセットごとに一意のパラメーターが必要ですが、パラメーターは複数のパラメーターセットに属することができます。 このような場合は、共有パラメーターに、パラメーターが属している各セットの system.object 属性 [宣言を指定](/dotnet/api/System.Management.Automation.ParameterAttribute) します。 パラメーターがすべてのパラメーターセットに含まれている場合は、パラメーター属性を1回だけ宣言するだけで、パラメーターセット名を指定する必要はありません。

## <a name="overriding-an-input-processing-method"></a>入力処理メソッドのオーバーライド

すべてのコマンドレットは、入力処理メソッドをオーバーライドする必要があります。これは、 [ほとんどの場合、この](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) メソッドになります。 このコマンドレットでは、コマンドレットが任意の数のプロセスを処理できるよう [に、system.string メソッドが](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) オーバーライドされます。 これには、ユーザーが指定したパラメーターセットに基づいて別の方法を呼び出す Select ステートメントが含まれています。

```csharp
protected override void ProcessRecord()
{
  switch (ParameterSetName)
  {
    case "ProcessName":
         ProcessByName();
         break;

    case "ProcessId":
         ProcessById();
         break;

    case "InputObject":
         foreach (Process process in inputObject)
         {
           SafeStopProcess(process);
         }
         break;

    default:
         throw new ArgumentException("Bad ParameterSet Name");
  } // switch (ParameterSetName...
} // ProcessRecord
```

```vb
Protected Overrides Sub ProcessRecord()
    Select Case ParameterSetName
        Case "ProcessName"
            ProcessByName()

        Case "ProcessId"
            ProcessById()

        Case "InputObject"
            Dim process As Process
            For Each process In myInputObject
                SafeStopProcess(process)
            Next process

        Case Else
            Throw New ArgumentException("Bad ParameterSet Name")
    End Select

End Sub 'ProcessRecord ' ProcessRecord
```

ここでは、Select ステートメントによって呼び出されるヘルパーメソッドについては説明しませんが、次のセクションの完全なコードサンプルで実装を確認できます。

## <a name="code-sample"></a>コード サンプル

完全な C# サンプルコードについては、「 [StopProcessSample04 sample](./stopprocesssample04-sample.md)」を参照してください。

## <a name="defining-object-types-and-formatting"></a>オブジェクトの種類と書式設定の定義

Windows PowerShell は、.NET オブジェクトを使用してコマンドレット間で情報を渡します。 そのため、コマンドレットで独自の型を定義する必要がある場合や、コマンドレットで別のコマンドレットによって提供される既存の型を拡張する必要がある場合があります。 新しい型を定義する、または既存の型を拡張する方法の詳細については、「 [オブジェクトの型と書式設定の拡張](/previous-versions//ms714665(v=vs.85))」を参照してください。

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが Windows PowerShell に登録されている場合は、コマンドラインで実行してテストします。 ここでは、 `ProcessId` パラメーターとパラメーターを使用して、 `InputObject` プロセスを停止するためにパラメーターセットをテストする方法を示すテストをいくつか示します。

- Windows PowerShell を起動した状態で、パラメーターを設定して Stop-Proc コマンドレットを実行し、 `ProcessId` その識別子に基づいてプロセスを停止します。 この場合、コマンドレットはパラメーターセットを使用して `ProcessId` プロセスを停止します。

  ```
  PS> stop-proc -Id 444
  Confirm
  Are you sure you want to perform this action?
  Performing operation "stop-proc" on Target "notepad (444)".
  [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): Y
  ```

- Windows PowerShell を起動した状態で、パラメーターを設定して Stop-Proc コマンドレットを実行し、 `InputObject` コマンドによって取得されるメモ帳オブジェクトのプロセスを停止し `Get-Process` ます。

  ```
  PS> get-process notepad | stop-proc
  Confirm
  Are you sure you want to perform this action?
  Performing operation "stop-proc" on Target "notepad (444)".
  [Y] Yes  [A] Yes to All  [N] No  [L] No to All  [S] Suspend  [?] Help (default is "Y"): N
  ```

## <a name="see-also"></a>参照

[システムを変更するコマンドレットを作成する](./creating-a-cmdlet-that-modifies-the-system.md)

[Windows PowerShell コマンドレットを作成する方法](/powershell/scripting/developer/cmdlet/writing-a-windows-powershell-cmdlet)

[オブジェクトの種類と書式設定の拡張](/previous-versions//ms714665(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))

[Windows PowerShell SDK](../windows-powershell-reference.md)
