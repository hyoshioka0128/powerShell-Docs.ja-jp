---
ms.date: 09/13/2016
ms.topic: reference
title: GetProcessSample01 サンプル
description: GetProcessSample01 サンプル
ms.openlocfilehash: 159c277d17a8551d2b5c52377a230babacafc9ab
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92652770"
---
# <a name="getprocesssample01-sample"></a>GetProcessSample01 サンプル

このサンプルでは、ローカルコンピューター上のプロセスを取得するコマンドレットを実装する方法を示します。 このコマンドレットは、 `Get-Process` Windows PowerShell 2.0 によって提供されるコマンドレットの簡略化されたバージョンです。

## <a name="how-to-build-the-sample-by-using-visual-studio"></a>Visual Studio を使用してサンプルをビルドする方法。

1. Windows PowerShell 2.0 SDK がインストールされている状態で、GetProcessSample01 フォルダーに移動します。 既定の場所は C:\Program Files (x86) \Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\csharp\GetProcessSample01. です。

2. ソリューション (.sln) ファイルのアイコンをダブルクリックします。 これにより、Microsoft Visual Studio でサンプルプロジェクトが開きます。

3. **[ビルド]** メニューで、 **[ソリューションのビルド]** を選択します。

  サンプルのライブラリは、既定の \bin フォルダーまたは \bin\debug フォルダーに構築されます。

### <a name="how-to-run-the-sample"></a>サンプルを実行する方法

1. コマンド プロンプト ウィンドウを開きます。

2. サンプル .dll ファイルが格納されているディレクトリに移動します。

3. Installutil.exe "GetProcessSample01.dll" を実行します。

4. Windows PowerShell を起動します。

5. 次のコマンドを実行して、スナップインをシェルに追加します。

   `Add-PSSnapin GetProcPSSnapIn01`

6. 次のコマンドを入力して、コマンドレットを実行します。 `get-proc`

   `get-proc`

   これは、次の手順を実行した結果として得られるサンプル出力です。

   ```output
   Id              Name            State      HasMoreData     Location             Command
   --              ----            -----      -----------     --------             -------
   1               26932870-d3b... NotStarted False                                 Write-Host "A f...

   ```

   ```powershell
   Set-Content $env:temp\test.txt "This is a test file"
   ```

   ```output
   A file was created in the TEMP directory
   ```

## <a name="requirements"></a>要件

このサンプルには、Windows PowerShell 1.0 以降が必要です。

## <a name="demonstrates"></a>対象

このサンプルでは、次のことを示します。

- 基本的なサンプルコマンドレットを作成しています。

- コマンドレットの属性を使用して、コマンドレットクラスを定義します。

- Windows PowerShell 1.0 と Windows PowerShell 2.0 の両方で動作するスナップインを作成する。 後続のサンプルでは、Windows PowerShell 2.0 を必要とするために、スナップインではなくモジュールを使用します。

## <a name="example"></a>例

このサンプルでは、簡単なコマンドレットとそのスナップインを作成する方法を示します。

```csharp
using System;
using System.Diagnostics;
using System.Management.Automation;             //Windows PowerShell namespace
using System.ComponentModel;

namespace Microsoft.Samples.PowerShell.Commands
{

   #region GetProcCommand

   /// <summary>
   /// This class implements the Get-Proc cmdlet.
   /// </summary>
   [Cmdlet(VerbsCommon.Get, "Proc")]
   public class GetProcCommand : Cmdlet
   {
      #region Cmdlet Overrides

      /// <summary>
      /// The ProcessRecord method calls the Process.GetProcesses
      /// method to retrieve the processes of the local computer.
      /// Then, the WriteObject method writes the associated processes
      /// to the pipeline.
      /// </summary>
      protected override void ProcessRecord()
      {
         // Retrieve the current processes.
         Process[] processes = Process.GetProcesses();

         // Write the processes to the pipeline to make them available
         // to the next cmdlet. The second argument (true) tells Windows
         // PowerShell to enumerate the array and to send one process
         // object at a time to the pipeline.
         WriteObject(processes, true);
      }

      #endregion Overrides

   } //GetProcCommand

   #endregion GetProcCommand

   #region PowerShell snap-in

   /// <summary>
   /// Create this sample as an PowerShell snap-in
   /// </summary>
   [RunInstaller(true)]
   public class GetProcPSSnapIn01 : PSSnapIn
   {
       /// <summary>
       /// Create an instance of the GetProcPSSnapIn01
       /// </summary>
       public GetProcPSSnapIn01()
           : base()
       {
       }

       /// <summary>
       /// Get a name for this PowerShell snap-in. This name will be used in registering
       /// this PowerShell snap-in.
       /// </summary>
       public override string Name
       {
           get
           {
               return "GetProcPSSnapIn01";
           }
       }

       /// <summary>
       /// Vendor information for this PowerShell snap-in.
       /// </summary>
       public override string Vendor
       {
           get
           {
               return "Microsoft";
           }
       }

       /// <summary>
       /// Gets resource information for vendor. This is a string of format:
       /// resourceBaseName,resourceName.
       /// </summary>
       public override string VendorResource
       {
           get
           {
               return "GetProcPSSnapIn01,Microsoft";
           }
       }

       /// <summary>
       /// Description of this PowerShell snap-in.
       /// </summary>
       public override string Description
       {
           get
           {
               return "This is a PowerShell snap-in that includes the get-proc cmdlet.";
           }
       }
   }

   #endregion PowerShell snap-in
}
```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
