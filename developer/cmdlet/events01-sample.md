---
title: Events01 サンプル |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: 27d0ee5e-2589-4530-92ef-c09996b80994
caps.latest.revision: 10
ms.openlocfilehash: 3edbcabeff0c8d84831823df11749d152b347566
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56863338"
---
# <a name="events01-sample"></a><span data-ttu-id="74708-102">Events01 サンプル</span><span class="sxs-lookup"><span data-stu-id="74708-102">Events01 Sample</span></span>

<span data-ttu-id="74708-103">このサンプルは、によって発生するイベントを登録するユーザーのためのコマンドレットを作成する方法を示しています。 [System.IO.Filesystemwatcher](/dotnet/api/System.IO.FileSystemWatcher)します。</span><span class="sxs-lookup"><span data-stu-id="74708-103">This sample shows how to create a cmdlet that allows the user to register for events that are raised by [System.IO.Filesystemwatcher](/dotnet/api/System.IO.FileSystemWatcher).</span></span> <span data-ttu-id="74708-104">このコマンドレットでは、ユーザーは、特定のディレクトリの下のファイルが作成されるときに実行するアクションを登録できます。</span><span class="sxs-lookup"><span data-stu-id="74708-104">With this cmdlet, users can register an action to execute when a file is created under a specific directory.</span></span> <span data-ttu-id="74708-105">このサンプルはから派生した、 [Microsoft.Powershell.Commands.Objecteventregistrationbase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase)基本クラス。</span><span class="sxs-lookup"><span data-stu-id="74708-105">This sample derives from the [Microsoft.Powershell.Commands.Objecteventregistrationbase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase) base class.</span></span>

## <a name="how-to-build-the-sample-by-using-visual-studio"></a><span data-ttu-id="74708-106">Visual Studio を使用してサンプルをビルドする方法。</span><span class="sxs-lookup"><span data-stu-id="74708-106">How to build the sample by using Visual Studio.</span></span>

1. <span data-ttu-id="74708-107">インストールされている Windows PowerShell 2.0 sdk では、Events01 フォルダーに移動します。</span><span class="sxs-lookup"><span data-stu-id="74708-107">With the Windows PowerShell 2.0 SDK installed, navigate to the Events01 folder.</span></span> <span data-ttu-id="74708-108">既定の場所は、C:\Program Files (x86) \Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\csharp\Events01 します。</span><span class="sxs-lookup"><span data-stu-id="74708-108">The default location is C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\csharp\Events01.</span></span>

2. <span data-ttu-id="74708-109">ソリューション (.sln) ファイルのアイコンをダブルクリックします。</span><span class="sxs-lookup"><span data-stu-id="74708-109">Double-click the icon for the solution (.sln) file.</span></span> <span data-ttu-id="74708-110">これは、Microsoft Visual Studio でサンプル プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="74708-110">This opens the sample project in Microsoft Visual Studio.</span></span>

3. <span data-ttu-id="74708-111">**ビルド**メニューの **ソリューションのビルド**します。</span><span class="sxs-lookup"><span data-stu-id="74708-111">In the **Build** menu, select **Build Solution**.</span></span>

    <span data-ttu-id="74708-112">サンプルのライブラリは、既定の \bin または \bin\debug フォルダーにビルドされます。</span><span class="sxs-lookup"><span data-stu-id="74708-112">The library for the sample will be built in the default \bin or \bin\debug folders.</span></span>

### <a name="how-to-run-the-sample"></a><span data-ttu-id="74708-113">サンプルを実行する方法</span><span class="sxs-lookup"><span data-stu-id="74708-113">How to run the sample</span></span>

1. <span data-ttu-id="74708-114">次のモジュール フォルダーを作成します。</span><span class="sxs-lookup"><span data-stu-id="74708-114">Create the following module folder:</span></span>

    `[user]/documents/windowspowershell/modules/events01`

2. <span data-ttu-id="74708-115">モジュール フォルダーにサンプルのライブラリ ファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="74708-115">Copy the library file for the sample to the module folder.</span></span>

3. <span data-ttu-id="74708-116">Windows PowerShell を起動します。</span><span class="sxs-lookup"><span data-stu-id="74708-116">Start Windows PowerShell.</span></span>

4. <span data-ttu-id="74708-117">Windows PowerShell コマンドレットを読み込むには、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="74708-117">Run the following command to load the cmdlet into Windows PowerShell:</span></span>

    ```powershell
    import-module events01
    ```

5. <span data-ttu-id="74708-118">登録 FileSystemEvent コマンドレットを使用すると、TEMP ディレクトリの下のファイルが作成されるときにメッセージを作成するアクションを登録できます。</span><span class="sxs-lookup"><span data-stu-id="74708-118">Use the Register-FileSystemEvent cmdlet to register an action that will write a message when a file is created under the TEMP directory.</span></span>

    ```powershell
    Register-FileSystemEvent $env:temp Created -filter "*.txt" -action { Write-Host "A file was created in the TEMP directory" }
    ```

6. <span data-ttu-id="74708-119">ディレクトリの TEMP ディレクトリの下にファイルを作成し、アクションが実行されることに注意してください (メッセージが表示されます)。</span><span class="sxs-lookup"><span data-stu-id="74708-119">Create a file under the TEMP directory and note that the action is executed (the message is displayed).</span></span>

<span data-ttu-id="74708-120">これは、次の手順で生成される出力の例です。</span><span class="sxs-lookup"><span data-stu-id="74708-120">This is a sample output that results by following these steps.</span></span>

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

## <a name="requirements"></a><span data-ttu-id="74708-121">要件</span><span class="sxs-lookup"><span data-stu-id="74708-121">Requirements</span></span>

<span data-ttu-id="74708-122">このサンプルでは、Windows PowerShell 2.0 が必要です。</span><span class="sxs-lookup"><span data-stu-id="74708-122">This sample requires Windows PowerShell 2.0.</span></span>

## <a name="demonstrates"></a><span data-ttu-id="74708-123">使用例</span><span class="sxs-lookup"><span data-stu-id="74708-123">Demonstrates</span></span>

<span data-ttu-id="74708-124">このサンプルは、次を示します。</span><span class="sxs-lookup"><span data-stu-id="74708-124">This sample demonstrates the following.</span></span>

- <span data-ttu-id="74708-125">イベントの登録のためのコマンドレットを記述する方法。</span><span class="sxs-lookup"><span data-stu-id="74708-125">How to write a cmdlet for event registration.</span></span> <span data-ttu-id="74708-126">派生したコマンドレット、 [Microsoft.Powershell.Commands.Objecteventregistrationbase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase)クラスを登録-一般的なパラメーターのサポートを提供します。 \* イベント コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="74708-126">The cmdlet derives from the [Microsoft.Powershell.Commands.Objecteventregistrationbase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase) class, which provides support for parameters common to the Register-\*Event cmdlets.</span></span> <span data-ttu-id="74708-127">派生したコマンドレット[Microsoft.Powershell.Commands.Objecteventregistrationbase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase) 、特定のパラメーターを定義し、オーバーライドするだけ、`GetSourceObject`と`GetSourceObjectEventName`抽象メソッド。</span><span class="sxs-lookup"><span data-stu-id="74708-127">Cmdlets that are derived from [Microsoft.Powershell.Commands.Objecteventregistrationbase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase) need only to define their particular parameters and override the `GetSourceObject` and `GetSourceObjectEventName` abstract methods.</span></span>

## <a name="example"></a><span data-ttu-id="74708-128">例</span><span class="sxs-lookup"><span data-stu-id="74708-128">Example</span></span>

<span data-ttu-id="74708-129">このサンプルは、によって生成されるイベントに登録する方法を示しています。 [System.IO.FileSystemWatcher](https://msdn.microsoft.com/en-us/library/system.io.filesystemwatcher\(v=vs.110\).aspx)します。</span><span class="sxs-lookup"><span data-stu-id="74708-129">This sample shows how to register for events raised by [System.IO.FileSystemWatcher](https://msdn.microsoft.com/en-us/library/system.io.filesystemwatcher\(v=vs.110\).aspx).</span></span>

```csharp
namespace Sample
{
    using System;
    using System.IO;
    using System.Management.Automation;
    using System.Management.Automation.Runspaces;
    using Microsoft.PowerShell.Commands;

    [Cmdlet(VerbsLifecycle.Register, "FileSystemEvent")]
    public class RegisterObjectEventCommand : ObjectEventRegistrationBase
    {
        /// <summary>The FileSystemWatcher that exposes the events.</summary>
        private FileSystemWatcher fileSystemWatcher = new FileSystemWatcher();

        /// <summary>Name of the event to which the cmdlet registers.</summary>
        private string eventName = null;

        /// <summary>
        /// Gets or sets the path that will be monitored by the FileSystemWatcher.
        /// </summary>
        [Parameter(Mandatory = true, Position = 0)]
        public string Path
        {
            get
            {
                return this.fileSystemWatcher.Path;
            }

            set
            {
                this.fileSystemWatcher.Path = value;
            }
        }

        /// <summary>
        /// Gets or sets the name of the event to which the cmdlet registers.
        /// <para>
        /// Currently System.IO.FileSystemWatcher exposes 6 events: Changed, Created,
        /// Deleted, Disposed, Error, and Renamed. Check the MSDN documentation of
        /// FileSystemWatcher for details on each event.
        /// </para>
        /// </summary>
        [Parameter(Mandatory = true, Position = 1)]
        public string EventName
        {
            get
            {
                return this.eventName;
            }

            set
            {
                this.eventName = value;
            }
        }

        /// <summary>
        /// Gets or sets the filter that will be user by the FileSystemWatcher.
        /// </summary>
        [Parameter(Mandatory = false)]
        public string Filter
        {
            get
            {
                return this.fileSystemWatcher.Filter;
            }

            set
            {
                this.fileSystemWatcher.Filter = value;
            }
        }

        /// <summary>
        /// Derived classes must implement this method to return the object that generates
        /// the events to be monitored.
        /// </summary>
        /// <returns> This sample returns an instance of System.IO.FileSystemWatcher</returns>
        protected override object GetSourceObject()
        {
            return this.fileSystemWatcher;
        }

        /// <summary>
        /// Derived classes must implement this method to return the name of the event to
        /// be monitored. This event must be exposed by the input object.
        /// </summary>
        /// <returns> This sample returns the event specified by the user with the -EventName parameter.</returns>
        protected override string GetSourceObjectEventName()
        {
            return this.eventName;
        }
    }
}
```

## <a name="see-also"></a><span data-ttu-id="74708-130">参照</span><span class="sxs-lookup"><span data-stu-id="74708-130">See Also</span></span>

[<span data-ttu-id="74708-131">Windows PowerShell コマンドレットの記述</span><span class="sxs-lookup"><span data-stu-id="74708-131">Writing a Windows PowerShell Cmdlet</span></span>](./writing-a-windows-powershell-cmdlet.md)