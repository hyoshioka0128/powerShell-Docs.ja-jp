---
ms.date: 09/13/2016
ms.topic: reference
title: GetProcessSample02 サンプル
description: GetProcessSample02 サンプル
ms.openlocfilehash: a0f43806b707359cb454817341f2c4972033c46a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92660519"
---
# <a name="getprocesssample02-sample"></a>GetProcessSample02 サンプル

このサンプルでは、ローカルコンピューター上のプロセスを取得するコマンドレットを記述する方法を示します。 これには、 `Name` 取得するプロセスを指定するために使用できるパラメーターが用意されています。 このコマンドレットは、 `Get-Process` Windows PowerShell 2.0 によって提供されるコマンドレットの簡略化されたバージョンです。

## <a name="how-to-build-the-sample-using-visual-studio"></a>Visual Studio を使用してサンプルをビルドする方法。

1. Windows PowerShell 2.0 SDK がインストールされている状態で、GetProcessSample02 フォルダーに移動します。 既定の場所は C:\Program Files (x86) \Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\csharp\GetProcessSample02. です。

2. ソリューション (.sln) ファイルのアイコンをダブルクリックします。 これにより、Visual Studio でサンプルプロジェクトが開きます。

3. **[ビルド]** メニューで、 **[ソリューションのビルド]** を選択します。

    サンプルのライブラリは、既定の \bin フォルダーまたは \bin\debug フォルダーに構築されます。

### <a name="how-to-run-the-sample"></a>サンプルを実行する方法

1. 次のモジュールフォルダーを作成します。

    `[user]/documents/windowspowershell/modules/GetProcessSample02`

2. サンプルアセンブリをモジュールフォルダーにコピーします。

3. Windows PowerShell を起動します。

4. 次のコマンドを実行して、Windows PowerShell にアセンブリを読み込みます。

    `import-module getprossessample02`

5. 次のコマンドを実行して、コマンドレットを実行します。

    `get-proc`

## <a name="requirements"></a>要件

このサンプルには、Windows PowerShell 2.0 が必要です。

## <a name="demonstrates"></a>対象

このサンプルでは、次のことを示します。

- コマンドレットの属性を使用して、コマンドレットクラスを宣言します。

- Parameter 属性を使用してコマンドレットパラメーターを宣言しています。

- パラメーターの位置を指定します。

- パラメーター入力の検証属性を宣言しています。

## <a name="example"></a>例

このサンプルでは、パラメーターを含む Get-Proc コマンドレットの実装を示し `Name` ます。

```csharp
namespace Microsoft.Samples.PowerShell.Commands
{
  using System;
  using System.Diagnostics;
  using System.Management.Automation;     // Windows PowerShell namespace

  #region GetProcCommand

  /// <summary>
  /// This class implements the get-proc cmdlet.
  /// </summary>
  [Cmdlet(VerbsCommon.Get, "Proc")]
  public class GetProcCommand : Cmdlet
  {
    #region Parameters

    /// <summary>
    /// The names of the processes retrieved by the cmdlet.
    /// </summary>
    private string[] processNames;

    /// <summary>
    /// Gets or sets the list of process names on which
    /// the Get-Proc cmdlet will work.
    /// </summary>
    [Parameter(Position = 0)]
    [ValidateNotNullOrEmpty]
    public string[] Name
    {
      get { return this.processNames; }
      set { this.processNames = value; }
    }

    #endregion Parameters

    #region Cmdlet Overrides

    /// <summary>
    /// The ProcessRecord method calls the Process.GetProcesses
    /// method to retrieve the processes specified by the Name
    /// parameter. Then, the WriteObject method writes the
    /// associated process objects to the pipeline.
    /// </summary>
    protected override void ProcessRecord()
    {
      // If no process names are passed to the cmdlet, get all
      // processes.
      if (this.processNames == null)
      {
        WriteObject(Process.GetProcesses(), true);
      }
      else
      {
        // If process names are passed to cmdlet, get and write
        // the associated processes.
        foreach (string name in this.processNames)
        {
          WriteObject(Process.GetProcessesByName(name), true);
        }
      } // End if (processNames...).
    } // End ProcessRecord.
    #endregion Cmdlet Overrides
  } // End GetProcCommand class.
  #endregion GetProcCommand
}
```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
