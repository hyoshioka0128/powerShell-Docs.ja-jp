---
title: Windows PowerShell SDK のインストール
ms.date: 09/13/2016
ms.topic: article
ms.openlocfilehash: da1b3dbb8a599aee2cdbab9115aedcab0b4c78c9
ms.sourcegitcommit: b6871f21bd666f9cd71dd336bb3f844cf472b56c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/03/2019
ms.locfileid: "56853958"
---
# <a name="installing-the-windows-powershell-sdk"></a><span data-ttu-id="9f67d-102">Windows PowerShell SDK のインストール</span><span class="sxs-lookup"><span data-stu-id="9f67d-102">Installing the Windows PowerShell SDK</span></span>

<span data-ttu-id="9f67d-103">適用先:Windows PowerShell 2.0、Windows PowerShell 3.0</span><span class="sxs-lookup"><span data-stu-id="9f67d-103">Applies To: Windows PowerShell 2.0, Windows PowerShell 3.0</span></span>

<span data-ttu-id="9f67d-104">以下のトピックでは、異なるバージョンの Windows に PowerShell SDK をインストールする方法について説明します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-104">The following topic describes how to install the PowerShell SDK on different versions of Windows.</span></span>

## <a name="installing-windows-powershell-30-sdk-for-windows-8-and-windows-server-2012"></a><span data-ttu-id="9f67d-105">Windows 8 と Windows Server 2012 での Windows PowerShell 3.0 SDK のインストール</span><span class="sxs-lookup"><span data-stu-id="9f67d-105">Installing Windows PowerShell 3.0 SDK for Windows 8 and Windows Server 2012</span></span>

<span data-ttu-id="9f67d-106">Windows PowerShell 3.0 は Windows 8 と Windows Server 2012 で自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-106">Windows PowerShell 3.0 is automatically installed with Windows 8 and Windows Server 2012.</span></span> <span data-ttu-id="9f67d-107">さらに、Windows 8 SDK の一部としての Windows PowerShell 3.0 の参照アセンブリもダウンロードおよびインストールできます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-107">In addition, you can download and install the reference assemblies for Windows PowerShell 3.0 as part of the Windows 8 SDK.</span></span> <span data-ttu-id="9f67d-108">これらのアセンブリでは、Windows PowerShell 3.0 のコマンドレット、プロバイダー、およびホスト プログラムを記述できます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-108">These assemblies allow you to write cmdlets, providers, and host programs for Windows PowerShell 3.0.</span></span> <span data-ttu-id="9f67d-109">Windows 8 用 Windows SDK をインストールすると、Windows PowerShell アセンブリが自動的に \Program Files (x86)\Reference Assemblies\Microsoft\WindowsPowerShell\3.0 にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-109">When you install the Windows SDK for Windows 8, the Windows PowerShell assemblies are automatically installed in the reference assembly folder, in \Program Files (x86)\Reference Assemblies\Microsoft\WindowsPowerShell\3.0.</span></span> <span data-ttu-id="9f67d-110">詳細については、サイトのダウンロード Windows 8 SDK を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9f67d-110">For more information, see the Windows 8 SDK download site.</span></span> <span data-ttu-id="9f67d-111">Windows PowerShell のサンプル コードもデベロッパー センターで入手できます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-111">Windows PowerShell code samples are also available on the Development Center.</span></span>
<span data-ttu-id="9f67d-112">詳細については、デベロッパー センターのサイトでデスクトップのコード サンプルのページを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9f67d-112">For more information, see the Desktop code sample page on the Dev center site.</span></span>

<span data-ttu-id="9f67d-113">さらに、Windows PowerShell 3.0 には Windows PowerShell 2.0 SDK との下位互換性があり、これには多くのサンプルコードが含まれます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-113">In addition, Windows PowerShell 3.0 is backwards-compatible with the Windows PowerShell 2.0 SDK, which includes a number of code samples.</span></span> <span data-ttu-id="9f67d-114">Windows PowerShell 2.0 SDK をダウンロードする方法の詳細については、以下をご覧ください。</span><span class="sxs-lookup"><span data-stu-id="9f67d-114">For more information on how to download the Windows PowerShell 2.0 SDK, see below.</span></span> <span data-ttu-id="9f67d-115">(2.0 のサンプル コードは Windows 8 および Windows PowerShell 3.0 と互換性がありますが、Windows PowerShell 2.0 を Windows 8 プラットフォームにインストールすることはできません)。</span><span class="sxs-lookup"><span data-stu-id="9f67d-115">(Note that while the 2.0 code samples are compatible with Windows 8 and Windows PowerShell 3.0, you cannot install Windows PowerShell 2.0 on a Windows 8 platform.)</span></span>

## <a name="installing-windows-powershell-30-sdk-for-windows-7-and-windows-server-2008-r2"></a><span data-ttu-id="9f67d-116">Windows 7 と Windows Server 2008 R2 での Windows PowerShell 3.0 SDK のインストール</span><span class="sxs-lookup"><span data-stu-id="9f67d-116">Installing Windows PowerShell 3.0 SDK for Windows 7 and Windows Server 2008 R2</span></span>

<span data-ttu-id="9f67d-117">Windows 7 と Windows Server 2008 R2 では、PowerShell 2.0 が自動的にインストールされます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-117">Windows 7 and Windows Server 2008 R2 automatically have PowerShell 2.0 installed.</span></span> <span data-ttu-id="9f67d-118">さらに、これらのシステムに PowerShell 3.0 をインストールできます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-118">In addition, you can install PowerShell 3.0 on these systems.</span></span> <span data-ttu-id="9f67d-119">(詳細については、参照 Windows PowerShell をインストールします。)。</span><span class="sxs-lookup"><span data-stu-id="9f67d-119">(For more information, see Installing Windows PowerShell.).</span></span> <span data-ttu-id="9f67d-120">前述のように、Windows 7 および Windows Server 2008 R2 で Windows 8 SDK をインストールすることもできます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-120">As described above, you can also install the Windows 8 SDK on Windows 7 and Windows Server 2008 R2.</span></span>

## <a name="installing-windows-powershell-20-sdk-for-windows-7-vista-xp-server-2003-and-server-2008"></a><span data-ttu-id="9f67d-121">Windows 7、Vista、XP、Server 2003、Server 2008 での Windows PowerShell 2.0 SDK のインストール</span><span class="sxs-lookup"><span data-stu-id="9f67d-121">Installing Windows PowerShell 2.0 SDK for Windows 7, Vista, XP, Server 2003, and Server 2008</span></span>

<span data-ttu-id="9f67d-122">Windows PowerShell 2.0 SDK は、コマンドレット、プロバイダー、アプリケーションのホストを記述するために必要な参照アセンブリを提供し、コードの記述を開始するときに開始点として使用できる C# サンプル コードを提供します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-122">The Windows PowerShell 2.0 SDK provides the reference assemblies needed to write cmdlets, providers, and hosting applications, and it provides C# sample code that can be used as the starting point when you begin writing code.</span></span>

<span data-ttu-id="9f67d-123">この SDK をインストールするには、Windows PowerShell 2.0 SDK を参照してください。</span><span class="sxs-lookup"><span data-stu-id="9f67d-123">To install this SDK, see Windows PowerShell 2.0 SDK.</span></span>

### <a name="reference-assemblies"></a><span data-ttu-id="9f67d-124">参照アセンブリ</span><span class="sxs-lookup"><span data-stu-id="9f67d-124">Reference assemblies</span></span>

<span data-ttu-id="9f67d-125">参照アセンブリは、既定では、次の場所にインストールされます: c:\Program Files\Reference Assemblies\Microsoft\WindowsPowerShell\V1.0 します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-125">Reference assemblies are installed in the following location by default: c:\Program Files\Reference Assemblies\Microsoft\WindowsPowerShell\V1.0.</span></span>

> [!NOTE]
>
> <span data-ttu-id="9f67d-126">Windows PowerShell 2.0 のアセンブリに対してコンパイルされるコードは、Windows PowerShell 1.0 のインストールに読み込むことができません。</span><span class="sxs-lookup"><span data-stu-id="9f67d-126">Code that is compiled against the Windows PowerShell 2.0 assemblies cannot be loaded into Windows PowerShell 1.0 installations.</span></span> <span data-ttu-id="9f67d-127">ただし、Windows PowerShell 1.0 のアセンブリに対してコンパイルされるコードは、Windows PowerShell 2.0 のインストールに読み込むことができます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-127">However, code that is compiled against the Windows PowerShell 1.0 assemblies can be loaded into Windows PowerShell 2.0 installations.</span></span>


### <a name="samples"></a><span data-ttu-id="9f67d-128">サンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-128">Samples</span></span>

<span data-ttu-id="9f67d-129">コード サンプルは、既定では、次の場所にインストールされます。C:\Program Files\Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\.</span><span class="sxs-lookup"><span data-stu-id="9f67d-129">Code samples are installed in the following location by default: C:\Program Files\Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\.</span></span> <span data-ttu-id="9f67d-130">次のセクションでは、各サンプル コードについて簡単に説明します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-130">The following sections provide a brief description of what each sample does.</span></span>

#### <a name="cmdlet-samples"></a><span data-ttu-id="9f67d-131">コマンドレット サンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-131">Cmdlet samples</span></span>

- <span data-ttu-id="9f67d-132">GetProcessSample01 - は、ローカル コンピューターのすべてのプロセスを取得する単純なコマンドレットを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-132">GetProcessSample01 - Shows how to write a simple cmdlet that gets all the processes on the local computer.</span></span>
- <span data-ttu-id="9f67d-133">GetProcessSample02 - は、コマンドレットにパラメーターを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-133">GetProcessSample02 - Shows how to add parameters to the cmdlet.</span></span> <span data-ttu-id="9f67d-134">このコマンドレットは 1 つまたは複数のプロセス名を取得し、それに一致するプロセスを返します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-134">The cmdlet takes one or more process names and returns the matching processes.</span></span>
- <span data-ttu-id="9f67d-135">GetProcessSample03 - は、パイプラインから入力を受け入れるパラメーターを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-135">GetProcessSample03 - Shows how to add parameters that accept input from the pipeline.</span></span>
- <span data-ttu-id="9f67d-136">GetProcessSample04 - は、終了しないエラーを処理する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-136">GetProcessSample04 - Shows how to handle nonterminating errors.</span></span>
- <span data-ttu-id="9f67d-137">GetProcessSample05 - は、指定されたプロセスの一覧を表示する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-137">GetProcessSample05 - Shows how to display a list of specified processes.</span></span>
- <span data-ttu-id="9f67d-138">SelectObject - は、特定のオブジェクトのみを選択するフィルターを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-138">SelectObject - Shows how to write a filter to select only certain objects.</span></span>
- <span data-ttu-id="9f67d-139">SelectString - は、指定されたパターンのファイルを検索する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-139">SelectString - Shows how to search files for specified patterns.</span></span>
- <span data-ttu-id="9f67d-140">StopProcessSample01 -、PassThru パラメーターを実装する方法と、ShouldProcess と ShouldContinue メソッドを呼び出してユーザーからのフィードバックを要求する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-140">StopProcessSample01 - Shows how to implement a PassThru parameter, and how to request user feedback by calls to the ShouldProcess and ShouldContinue methods.</span></span> <span data-ttu-id="9f67d-141">ユーザーが、オブジェクトを取得するコマンドレットを強制するときに、PassThru パラメーターを指定します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-141">Users specify the PassThru parameter when they want to force the cmdlet to return an object,</span></span>
- <span data-ttu-id="9f67d-142">StopProcessSample02 - は、特定のプロセスを停止する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-142">StopProcessSample02 - Shows how to stop a specific process.</span></span>
- <span data-ttu-id="9f67d-143">StopProcessSample03 - は、パラメーターのエイリアスを宣言する方法と、ワイルドカードをサポートする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-143">StopProcessSample03 - Shows how to declare aliases for parameters and how to support wildcards.</span></span>
- <span data-ttu-id="9f67d-144">StopProcessSample04 - パラメーターのセットを宣言する方法を示しています、コマンドレットは、入力、および既定のパラメーターを使用する設定を指定する方法として受け取るオブジェクト。</span><span class="sxs-lookup"><span data-stu-id="9f67d-144">StopProcessSample04 - Shows how to declare parameter sets, the object that the cmdlet takes as input, and how to specify the default parameter set to use.</span></span>

#### <a name="remoting-samples"></a><span data-ttu-id="9f67d-145">リモート処理のサンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-145">Remoting samples</span></span>

- <span data-ttu-id="9f67d-146">RemoteRunspace01 - は、リモート接続を確立するために使用されるリモート実行空間を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-146">RemoteRunspace01 - Shows how to create a remote runspace that is used to establish a remote connection.</span></span>
- <span data-ttu-id="9f67d-147">RemoteRunspacePool01 - は、このプールを使用して、同時に複数のコマンドを実行する方法とリモートの実行空間プールを作成する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-147">RemoteRunspacePool01 - Shows how to construct a remote runspace pool and how to run multiple commands concurrently by using this pool.</span></span>
- <span data-ttu-id="9f67d-148">Serialization01 - は、既存の .NET クラスを見て、このクラスの選択したパブリック プロパティからの情報をシリアル化/逆シリアル化の間で保持するかどうかを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-148">Serialization01 - Shows how to look at an existing .NET class and make sure that information from selected public properties of this class is preserved across serialization/deserialization.</span></span>
- <span data-ttu-id="9f67d-149">Serialization02 - は、既存の .NET クラスを確認し、情報が、クラスのパブリック プロパティで利用できないときにこのクラスのインスタンスからの情報をシリアル化/逆シリアル化の間で保持するかどうかを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-149">Serialization02 - Shows how to look at an existing .NET class and make sure that information from instance of this class is preserved across serialization/deserialization when the information is not available in public properties of the class.</span></span>
- <span data-ttu-id="9f67d-150">Serialization03 - は、既存の .NET クラスを見て、こと、このクラスの派生クラスのインスタンスが (リハイド レート) ライブ .NET オブジェクトに逆シリアル化することを確認する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-150">Serialization03 - Shows how to look at an existing .NET class and make sure that instances of this class and of derived classes are deserialized (rehydrated) into live .NET objects.</span></span>

#### <a name="event-samples"></a><span data-ttu-id="9f67d-151">イベントのサンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-151">Event samples</span></span>

- <span data-ttu-id="9f67d-152">Event01 - は、ObjectEventRegistrationBase から派生することによって、イベント登録のためのコマンドレットを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-152">Event01 - Shows how to create a cmdlet for event registration by deriving from ObjectEventRegistrationBase.</span></span>
- <span data-ttu-id="9f67d-153">Event02 の表示方法をリモート コンピューターで生成される Windows PowerShell イベントの通知を受信する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-153">Event02 - Shows how to shows how to receive notifications of Windows PowerShell events that are generated on remote computers.</span></span> <span data-ttu-id="9f67d-154">実行空間のクラスを通じて公開される PSEventReceived イベントを使用します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-154">It uses the PSEventReceived event exposed through the Runspace class.</span></span>

#### <a name="hosting-application-samples"></a><span data-ttu-id="9f67d-155">アプリケーションのホストのサンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-155">Hosting application samples</span></span>

- <span data-ttu-id="9f67d-156">Runspace01 - PowerShell クラスを使用して実行する方法を示しています、`Get-Process`コマンドレット同期的にします。</span><span class="sxs-lookup"><span data-stu-id="9f67d-156">Runspace01 - Shows how to use the PowerShell class to run the `Get-Process` cmdlet synchronously.</span></span>
<span data-ttu-id="9f67d-157">`Get-Process`コマンドレットは、ローカル コンピューターで実行されている各プロセスのプロセス オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-157">The `Get-Process` cmdlet returns Process objects for each process running on the local computer.</span></span>
- <span data-ttu-id="9f67d-158">Runspace02 - PowerShell クラスを使用して実行する方法を示しています、`Get-Process`と`Sort-Object`コマンドレット同期的にします。</span><span class="sxs-lookup"><span data-stu-id="9f67d-158">Runspace02 - Shows how to use the PowerShell class to run the `Get-Process` and `Sort-Object` cmdlets synchronously.</span></span> <span data-ttu-id="9f67d-159">`Get-Process`コマンドレットは、ローカル コンピューターで実行されている各プロセスのプロセス オブジェクトを返します、`Sort-Object`それぞれの Id プロパティに基づいてオブジェクトを並べ替えます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-159">The `Get-Process` cmdlet returns Process objects for each process running on the local computer, and the `Sort-Object` sorts the objects based on their Id property.</span></span> <span data-ttu-id="9f67d-160">DataGridView コントロールを使用して、これらのコマンドの結果が表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-160">The results of these commands is displayed by using a DataGridView control.</span></span>
- <span data-ttu-id="9f67d-161">Runspace03 - は、未終了エラーを処理する方法と PowerShell クラスを使用して同期的に、スクリプトを実行する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-161">Runspace03 - Shows how to use the PowerShell class to run a script synchronously, and how to handle non-terminating errors.</span></span> <span data-ttu-id="9f67d-162">このスクリプトはプロセス名の一覧を受信し、これらのプロセスを取得します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-162">The script receives a list of process names and then retrieves those processes.</span></span> <span data-ttu-id="9f67d-163">スクリプトの実行時に生成された終了しないエラーを含む、スクリプトの結果がコンソール ウィンドウに表示されます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-163">The results of the script, including any non-terminating errors that were generated when running the script, are displayed in a console window.</span></span>
- <span data-ttu-id="9f67d-164">Runspace04 - は、PowerShell クラスを使用して、コマンドを実行する方法とをキャッチする方法を示しています。 コマンドの実行時にスローされたエラーを終了しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-164">Runspace04 - Shows how to use the PowerShell class to run commands, and how to catch terminating errors that are thrown when running the commands.</span></span> <span data-ttu-id="9f67d-165">2 つのコマンドが実行され、最後のコマンドには無効なパラメーターの引数が渡されます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-165">Two commands are run, and the last command is passed a parameter argument that is not valid.</span></span> <span data-ttu-id="9f67d-166">結果として、オブジェクトは返されず、終了するエラーがスローされます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-166">As a result, no objects are returned and a terminating error is thrown.</span></span>
- <span data-ttu-id="9f67d-167">Runspace05 - は、実行空間が開いたときに、スナップインのコマンドレットを使用できるように、InitialSessionState オブジェクトにスナップインを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-167">Runspace05 - Shows how to add a snap-in to an InitialSessionState object so that the cmdlet of the snap-in is available when the runspace is opened.</span></span> <span data-ttu-id="9f67d-168">スナップインでは、PowerShell オブジェクトを使用して同期的に実行される Get-proc コマンドレット (GetProcessSample01 サンプルで定義) を提供します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-168">The snap-in provides a Get-Proc cmdlet (defined by the GetProcessSample01 Sample) that is run synchronously by using a PowerShell object.</span></span>
- <span data-ttu-id="9f67d-169">Runspace06 - は、実行空間が開かれたときに、モジュールが読み込まれるように InitialSessionState オブジェクトにモジュールを追加する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-169">Runspace06 - Shows how to add a module to an InitialSessionState object so that the module is loaded when the runspace is opened.</span></span> <span data-ttu-id="9f67d-170">モジュールは、PowerShell オブジェクトを使用して同期的に実行される Get-proc コマンドレット (GetProcessSample02 サンプルで定義) を提供します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-170">The module provides a Get-Proc cmdlet (defined by the GetProcessSample02 Sample) that is run synchronously by using a PowerShell object.</span></span>
- <span data-ttu-id="9f67d-171">Runspace07 - は、実行空間を作成し、その実行空間を使用して、2 つのコマンドレットを PowerShell オブジェクトを使用して同期的に実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-171">Runspace07 - Shows how to create a runspace, and then use that runspace to run two cmdlets synchronously by using a PowerShell object.</span></span>
- <span data-ttu-id="9f67d-172">Runspace08 - は、PowerShell オブジェクトのパイプラインにコマンドと引数を追加する方法と、コマンドを同期的に実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-172">Runspace08 - Shows how to add commands and arguments to the pipeline of a PowerShell object and how to run the commands synchronously.</span></span>
- <span data-ttu-id="9f67d-173">Runspace09 - は、スクリプトを PowerShell オブジェクトのパイプラインに追加する方法と、スクリプトを非同期的に実行する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-173">Runspace09 - Shows how to add a script to the pipeline of a PowerShell object and how to run the script asynchronously.</span></span> <span data-ttu-id="9f67d-174">イベントはスクリプトの出力を処理するために使用されます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-174">Events are used to handle the output of the script.</span></span>
- <span data-ttu-id="9f67d-175">Runspace10 - は、既定の最初のセッション状態を作成する方法、InitialSessionState にコマンドレットを追加する方法、最初のセッションの状態を使用する実行空間を作成する方法、および PowerShell オブジェクトを使用して、コマンドを実行する方法を示しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-175">Runspace10 - Shows how to create a default initial session state, how to add a cmdlet to the InitialSessionState, how to create a runspace that uses the initial session state, and how to run the command by using a PowerShell object.</span></span>
- <span data-ttu-id="9f67d-176">Runspace11 - ProxyCommand クラスを使用して、既存のコマンドレットを呼び出しますが、使用可能なパラメーターのセットを制限するプロキシ コマンドを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-176">Runspace11 - Shows how to use the ProxyCommand class to create a proxy command that calls an existing cmdlet, but restricts the set of available parameters.</span></span> <span data-ttu-id="9f67d-177">このプロキシ コマンドは、制約付き実行空間の作成に使用される最初のセッション状態に追加されます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-177">The proxy command is then added to an initial session state that is used to create a constrained runspace.</span></span> <span data-ttu-id="9f67d-178">つまり、ユーザーはプロキシ コマンドを使用しないとコマンドレットの機能にアクセスできません。</span><span class="sxs-lookup"><span data-stu-id="9f67d-178">This means that the user can access the functionality of the cmdlet only through the proxy command.</span></span>
- <span data-ttu-id="9f67d-179">PowerShell01 - は、InitialSessionState オブジェクトを使用して制約付き実行空間を作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-179">PowerShell01 - Shows how to create a constrained runspace using an InitialSessionState object.</span></span>
- <span data-ttu-id="9f67d-180">PowerShell02 - は、同時に複数のコマンドを実行する実行空間プールを使用する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-180">PowerShell02 - Shows how to use a runspace pool to run multiple commands concurrently.</span></span>

#### <a name="host-samples"></a><span data-ttu-id="9f67d-181">ホストのサンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-181">Host samples</span></span>

- <span data-ttu-id="9f67d-182">Host01 というは、カスタム ホストを使用するホスト アプリケーションを実装する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-182">Host01 - Shows how to implement a host application that uses a custom host.</span></span> <span data-ttu-id="9f67d-183">カスタムのホストを使用する実行空間を作成するこのサンプルで、"exit"を呼び出すスクリプトを実行する PowerShell API を使用し、</span><span class="sxs-lookup"><span data-stu-id="9f67d-183">In this sample a runspace is created that uses the custom host, and then the PowerShell API is used to run a script that calls “exit.”</span></span> <span data-ttu-id="9f67d-184">ホスト アプリケーションはスクリプトの出力を確認し、結果を印刷します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-184">The host application then looks at the output of the script and prints out the results.</span></span>
- <span data-ttu-id="9f67d-185">Host02 - は、カスタム ホストの実装と共に Windows PowerShell ランタイムを使用するホスト アプリケーションを記述する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-185">Host02 - Shows how to write a host application that uses the Windows PowerShell runtime along with a custom host implementation.</span></span> <span data-ttu-id="9f67d-186">ホスト アプリケーションは、ドイツ語、実行するホストのカルチャを設定、`Get-Process`コマンドレットと同じ結果が表示されますが見る pwrsh.exe、および現在のデータと時刻を出力をドイツ語を使用しています。</span><span class="sxs-lookup"><span data-stu-id="9f67d-186">The host application sets the host culture to German, runs the `Get-Process` cmdlet and displays the results as you would see them by using pwrsh.exe, and then prints out the current data and time in German.</span></span>
- <span data-ttu-id="9f67d-187">Host03 - は、コマンドラインからコマンドを読み取ってや、コマンドを実行し、結果をコンソールに表示する対話型コンソール ベースのホスト アプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-187">Host03 - Shows how to build an interactive console-based host application that reads commands from the command line, executes the commands, and then displays the results to the console.</span></span>
- <span data-ttu-id="9f67d-188">Host04 - は、コマンドラインからコマンドを読み取ってや、コマンドを実行し、結果をコンソールに表示する対話型コンソール ベースのホスト アプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-188">Host04 - Shows how to build an interactive console-based host application that reads commands from the command line, executes the commands, and then displays the results to the console.</span></span> <span data-ttu-id="9f67d-189">このホスト アプリケーションでは、複数選択を指定するための確認メッセージを表示することもできます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-189">This host application also supports displaying prompts that allow the user to specify multiple choices.</span></span>
- <span data-ttu-id="9f67d-190">Host05 - は、コマンドラインからコマンドを読み取ってや、コマンドを実行し、結果をコンソールに表示する対話型コンソール ベースのホスト アプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-190">Host05 - Shows how to build an interactive console-based host application that reads commands from the command line, executes the commands, and then displays the results to the console.</span></span> <span data-ttu-id="9f67d-191">このホスト アプリケーションを使用してリモート コンピューターへの呼び出しをサポートすることも、`Enter-PsSession`と`Exit-PsSession`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="9f67d-191">This host application also supports calls to remote computers by using the `Enter-PsSession` and `Exit-PsSession` cmdlets.</span></span>
- <span data-ttu-id="9f67d-192">Host06 - は、コマンドラインからコマンドを読み取ってや、コマンドを実行し、結果をコンソールに表示する対話型コンソール ベースのホスト アプリケーションを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-192">Host06 - Shows how to build an interactive console-based host application that reads commands from the command line, executes the commands, and then displays the results to the console.</span></span> <span data-ttu-id="9f67d-193">さらに、このサンプルでは、トークナイザー API を使用して、ユーザーが入力したテキストの色を指定します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-193">In addition, this sample uses the Tokenizer APIs to specify the color of the text that is entered by the user.</span></span>

#### <a name="provider-samples"></a><span data-ttu-id="9f67d-194">プロバイダーのサンプル</span><span class="sxs-lookup"><span data-stu-id="9f67d-194">Provider samples</span></span>

- <span data-ttu-id="9f67d-195">-AccessDBProviderSample01 CmdletProvider クラスから直接派生するプロバイダー クラスを宣言する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-195">AccessDBProviderSample01 - Shows how to declare a provider class that derives directly from the CmdletProvider class.</span></span> <span data-ttu-id="9f67d-196">すべてを網羅しておく目的でここに含めておきます。</span><span class="sxs-lookup"><span data-stu-id="9f67d-196">It is included here only for completeness.</span></span>

- <span data-ttu-id="9f67d-197">AccessDBProviderSample02 の呼び出しをサポートする NewDrive と RemoveDrive メソッドを上書きする方法を示しています、`New-PSDrive`と`Remove-PSDrive`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="9f67d-197">AccessDBProviderSample02 - Shows how to overwrite the NewDrive and RemoveDrive methods to support calls to the `New-PSDrive` and `Remove-PSDrive` cmdlets.</span></span> <span data-ttu-id="9f67d-198">このサンプルのプロバイダー クラスは、DriveCmdletProvider クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-198">The provider class in this sample derives from the DriveCmdletProvider class.</span></span>

- <span data-ttu-id="9f67d-199">AccessDBProviderSample03 の呼び出しをサポートする SetItem、GetItem、およびメソッドを上書きする方法を示しています、`Get-Item`と`Set-Item`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="9f67d-199">AccessDBProviderSample03 - Shows how to overwrite the GetItem and SetItem methods to support calls to the `Get-Item` and `Set-Item` cmdlets.</span></span> <span data-ttu-id="9f67d-200">このサンプルのプロバイダー クラスは、ItemCmdletProvider クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-200">The provider class in this sample derives from the ItemCmdletProvider class.</span></span>

- <span data-ttu-id="9f67d-201">AccessDBProviderSample04 の呼び出しをサポートするコンテナーのメソッドを上書きする方法を示しています、 `Copy-Item`、 `Get-ChildItem`、 `New-Item`、および`Remove-Item`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="9f67d-201">AccessDBProviderSample04 - Shows how to overwrite container methods to support calls to the `Copy-Item`, `Get-ChildItem`, `New-Item`, and `Remove-Item` cmdlets.</span></span> <span data-ttu-id="9f67d-202">これらのメソッドは、データ ストアにコンテナーであるアイテムが含まれる場合に実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f67d-202">These methods should be implemented when the data store contains items that are containers.</span></span> <span data-ttu-id="9f67d-203">コンテナーは、共通の親項目の子項目のグループです。</span><span class="sxs-lookup"><span data-stu-id="9f67d-203">A container is a group of child items under a common parent item.</span></span> <span data-ttu-id="9f67d-204">このサンプルのプロバイダー クラスは、ItemCmdletProvider クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-204">The provider class in this sample derives from the ItemCmdletProvider class.</span></span>

- <span data-ttu-id="9f67d-205">AccessDBProviderSample05 の呼び出しをサポートするコンテナーのメソッドを上書きする方法を示しています、`Move-Item`と`Join-Path`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="9f67d-205">AccessDBProviderSample05 - Shows how to overwrite container methods to support calls to the `Move-Item` and `Join-Path` cmdlets.</span></span> <span data-ttu-id="9f67d-206">これらのメソッドは、ユーザーがコンテナー内で項目を移動する必要があり、データ ストアに入れ子状態のコンテナーが含まれる場合に実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f67d-206">These methods should be implemented when the user needs to move items within a container and if the data store contains nested containers.</span></span> <span data-ttu-id="9f67d-207">このサンプルのプロバイダー クラスは、NavigationCmdletProvider クラスから派生します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-207">The provider class in this sample derives from the NavigationCmdletProvider class.</span></span>

- <span data-ttu-id="9f67d-208">AccessDBProviderSample06 の呼び出しをサポートするコンテンツのメソッドを上書きする方法を示しています、 `Clear-Content`、 `Get-Content`、および`Set-Content`コマンドレット。</span><span class="sxs-lookup"><span data-stu-id="9f67d-208">AccessDBProviderSample06 - Shows how to overwrite content methods to support calls to the `Clear-Content`, `Get-Content`, and `Set-Content` cmdlets.</span></span> <span data-ttu-id="9f67d-209">これらのメソッドは、ユーザーがデータ ストア内の項目のコンテンツを管理する必要がある場合に実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="9f67d-209">These methods should be implemented when the user needs to manage the content of the items in the data store.</span></span> <span data-ttu-id="9f67d-210">このサンプルのプロバイダー クラスは、NavigationCmdletProvider クラスから派生し、IContentCmdletProvider インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="9f67d-210">The provider class in this sample derives from the NavigationCmdletProvider class, and it implements the IContentCmdletProvider interface.</span></span>