---
ms.date: 06/11/2020
ms.topic: reference
title: コマンドレットの概要
description: コマンドレットの概要
ms.openlocfilehash: ed3082e1a821bb9643ea2eef13b7348eb48488e4
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653230"
---
# <a name="cmdlet-overview"></a>コマンドレットの概要

コマンドレットは、PowerShell 環境で使用される簡易コマンドです。 PowerShell ランタイムは、コマンドラインで指定されたオートメーションスクリプトのコンテキスト内でこれらのコマンドレットを呼び出します。 Powershell ランタイムでは、PowerShell Api を使用してプログラムで呼び出すこともできます。

## <a name="cmdlets"></a>コマンドレット

コマンドレットは、アクションを実行し、通常、パイプラインの次のコマンドに Microsoft .NET オブジェクトを返します。 コマンドレットは、PowerShell のパイプラインセマンティクスに関与する単一のコマンドです。
これには、バイナリ (C#) コマンドレット、高度なスクリプト関数、CDXML、ワークフローが含まれます。

この SDK ドキュメントでは、C# で記述されたバイナリコマンドレットを作成する方法について説明します。 スクリプトベースのコマンドレットの詳細については、以下を参照してください。

- [about_Functions_Advanced](/powershell/module/microsoft.powershell.core/about/about_functions_advanced)
- [about_Functions_CmdletBindingAttribute](/powershell/module/microsoft.powershell.core/about/about_functions_cmdletbindingattribute)
- [about_Functions_Advanced_Methods](/powershell/module/microsoft.powershell.core/about/about_functions_advanced_methods)

バイナリコマンドレットを作成するには、2つの特殊なコマンドレットの基底クラスから派生したコマンドレットクラスを実装する必要があります。 派生クラスには次のものが必要です。

- 派生クラスをコマンドレットとして識別する属性を宣言します。
- コマンドレットのパラメーターとしてパブリックプロパティを識別する属性で装飾されたパブリックプロパティを定義します。
- 1つ以上の入力処理メソッドをオーバーライドしてレコードを処理します。

クラスを含むアセンブリ [は、tialsessionstate コマンドレット](/powershell/module/microsoft.powershell.core/import-module) を使用して直接読み込むことも、または、 [System.Management.Automation.Runspaces.Ini](/dotnet/api/System.Management.Automation.Runspaces.InitialSessionState) API を使用してアセンブリを読み込むホストアプリケーションを作成することもできます。 どちらの方法でも、プログラムおよびコマンドラインからコマンドレットの機能にアクセスできます。

## <a name="cmdlet-terms"></a>コマンドレットの使用条件

PowerShell コマンドレットのドキュメントでは、次の用語が頻繁に使用されます。

### <a name="cmdlet-attribute"></a>コマンドレット属性

コマンドレットクラスをコマンドレットとして宣言するために使用される .NET 属性。 PowerShell では、省略可能な他のいくつかの属性が使用されますが、コマンドレットの属性は必須です。 この属性の詳細については、「 [コマンドレット属性の宣言](cmdlet-attribute-declaration.md)」を参照してください。

### <a name="cmdlet-parameter"></a>コマンドレット パラメーター

ユーザーが使用できるパラメーター、またはコマンドレットを実行しているアプリケーションに対して使用可能なパラメーターを定義するパブリックプロパティ。 コマンドレットには、必須の、名前付き、位置指定、および *スイッチ* のパラメーターを指定できます。 スイッチパラメーターを使用すると、呼び出しでパラメーターが指定されている場合にのみ評価されるパラメーターを定義できます。 さまざまな種類のパラメーターの詳細については、「 [コマンドレットパラメーター](cmdlet-parameters.md)」を参照してください。

### <a name="parameter-set"></a>パラメーター セット

特定のアクションを実行するために同じコマンドで使用できるパラメーターのグループ。 コマンドレットには複数のパラメーターセットを含めることができますが、各パラメーターセットには一意のパラメーターが少なくとも1つ必要です。 適切なコマンドレットの設計では、unique パラメーターも必須パラメーターであることを強く示唆しています。
パラメーターセットの詳細については、「 [コマンドレットパラメーターセット](cmdlet-parameter-sets.md)」を参照してください。

### <a name="dynamic-parameter"></a>[動的パラメーター]

実行時にコマンドレットに追加されるパラメーター。 通常、動的パラメーターは、別のパラメーターが特定の値に設定されている場合に、コマンドレットに追加されます。 動的パラメーターの詳細については、「 [コマンドレット動的パラメーター](cmdlet-dynamic-parameters.md)」を参照してください。

### <a name="input-processing-methods"></a>入力処理メソッド

System.string [クラスは](/dotnet/api/System.Management.Automation.Cmdlet) 、レコードを処理するために使用される次の仮想メソッドを提供します。 派生したすべてのコマンドレットクラスは、最初の3つのメソッドをオーバーライドする必要があります。

- [System.servicemodel []: コマンド](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing)レットに対して、オプションの1回限りの前処理機能を提供するために使用されます。
- System.string: コマンドレットのレコードごとの処理機能を提供するために使用され[ます。](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) コマンドレットの入力によっては、すべての回数 (またはまったくではない) の呼び出しが必要になる場合があり[ます。](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord)
- System.servicemodel: コマンドレットに対して、省略可能な1回限りの後処理機能を提供するために使用され[ます。](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing)
- [システムの管理]-[ [StopProcessing](/dotnet/api/System.Management.Automation.Cmdlet.StopProcessing)]: ユーザーがコマンドレットを非同期に停止したときに処理を停止するために使用されます (たとえば、 <kbd>CTRL</kbd>C キーを押すなど + <kbd></kbd>)。

これらのメソッドの詳細については、「 [コマンドレットの入力処理方法](./cmdlet-input-processing-methods.md)」を参照してください。

コマンドレットを実装する場合は、これらの入力処理メソッドの少なくとも1つをオーバーライドする必要があります。
通常、 **Processrecord ()** は、コマンドレットが処理するすべてのレコードに対して呼び出されるため、オーバーライドするメソッドです。 これに対して、 **beginprocessing ()** メソッドと **EndProcessing ()** メソッドは、レコードの前処理または後処理を実行するために1回呼び出されます。 これらのメソッドの詳細については、「 [入力処理方法](cmdlet-input-processing-methods.md)」を参照してください。

### <a name="shouldprocess-feature"></a>機能を処理する

PowerShell では、コマンドレットがシステムに変更を加える前に、ユーザーにフィードバックを求めるコマンドレットを作成できます。 この機能を使用するには、コマンドレットの属性を宣言するときに、この機能がサポートされていることをコマンドレットが宣言する必要があります。また、コマンドレットは、 `ShouldProcess` 入力処理メソッド内からメソッド[を](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)呼び出す必要があり[ます。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldContinue) 機能をサポートする方法の詳細については `ShouldProcess` 、「 [確認の要求](requesting-confirmation-from-cmdlets.md)」を参照してください。

### <a name="transaction"></a>トランザクション

1つのタスクとして扱われるコマンドの論理グループ。 グループ内のいずれかのコマンドが失敗し、ユーザーがトランザクション内で実行されたアクションを受け入れるか拒否するかを選択できる場合、タスクは自動的に失敗します。 コマンドレットは、トランザクションに参加するには、コマンドレットの属性が宣言されているときにトランザクションをサポートするように宣言する必要があります。 トランザクションのサポートは、Windows PowerShell 2.0 で導入されました。 トランザクションの詳細については、「 [トランザクションをサポートする方法](how-to-support-transactions.md)」を参照してください。

## <a name="how-cmdlets-differ-from-commands"></a>コマンドレットの違い

コマンドレットは、次の方法で他のコマンドシェル環境のコマンドとは異なります。

- コマンドレットは .NET クラスのインスタンスです。これらはスタンドアロンの実行可能ファイルではありません。
- コマンドレットは、数十のコード行から作成できます。
- コマンドレットは、通常、独自の解析、エラー表示、または出力書式設定を行いません。 解析、エラー表示、および出力の書式設定は、PowerShell ランタイムによって処理されます。
- コマンドレットは、テキストのストリームからではなく、パイプラインからの入力オブジェクトを処理します。また、コマンドレットは通常、オブジェクトをパイプラインに出力として配信します。
- コマンドレットは、一度に1つのオブジェクトを処理するため、レコード指向です。

## <a name="cmdlet-base-classes"></a>コマンドレットの基本クラス

Windows PowerShell は、次の2つの基本クラスから派生したコマンドレットをサポートしています。

- ほとんどのコマンドレットは、 [system.servicemodel 基底クラス](/dotnet/api/System.Management.Automation.Cmdlet) から派生した .net クラスに基づいています。
  このクラスから派生させることにより、コマンドレットは、Windows PowerShell ランタイムの最小限の依存関係を使用できます。 これには 2 つ利点があります。 最初の利点は、コマンドレットオブジェクトが小さく、PowerShell ランタイムの変更によって影響を受ける可能性が低いことです。 2つ目の利点は、必要に応じて、コマンドレットオブジェクトのインスタンスを直接作成し、PowerShell ランタイムを使用して呼び出すのではなく、直接呼び出すことができることです。

- より複雑なコマンドレットは、 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) 基底クラスから派生した .net クラスに基づいています。 このクラスから派生させることにより、PowerShell ランタイムにはるかに多くのアクセス権を与えることができます。 このアクセスにより、コマンドレットは、スクリプトを呼び出したり、プロバイダーにアクセスしたり、現在のセッション状態にアクセスしたりすることができます。
  (現在のセッション状態にアクセスするには、セッション変数とユーザー設定を取得して設定します)。ただし、このクラスから派生すると、コマンドレットオブジェクトのサイズが大きくなり、コマンドレットが現在のバージョンの PowerShell ランタイムに密に結合されることになります。

一般に、PowerShell ランタイムへの拡張アクセスが必要な場合を除き、 [このクラスから](/dotnet/api/System.Management.Automation.Cmdlet) 派生させる必要があります。
ただし、PowerShell ランタイムには、コマンドレットを実行するための広範なログ記録機能があります。 監査モデルがこのログに依存している場合は、 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) クラスから派生することによって、別のコマンドレット内からコマンドレットを実行できないようにすることができます。

## <a name="cmdlet-attributes"></a>コマンドレットの属性

PowerShell では、コマンドレットの管理に使用されるいくつかの .NET 属性と、PowerShell によって提供され、コマンドレットで必要となる可能性がある共通の機能を指定します。 たとえば、属性を使用して、クラスをコマンドレットとして指定したり、コマンドレットのパラメーターを指定したり、コマンドレットの開発者がコマンドレットコードにその機能を実装する必要がないように入力の検証を要求したりします。 属性の詳細については、「 [PowerShell attributes](./cmdlet-attributes.md)」を参照してください。

## <a name="cmdlet-names"></a>コマンドレット名

PowerShell では、動詞と名詞の名前の組み合わせを使用して、コマンドレットに名前を指定します。 たとえば、PowerShell に含まれているコマンドレットを使用して、 `Get-Command` コマンドシェルに登録されているすべてのコマンドレットを取得します。 動詞はコマンドレットが実行するアクションを識別し、名詞は、コマンドレットがアクションを実行するリソースを識別します。

これらの名前は、.NET クラスがコマンドレットとして宣言されている場合に指定します。 コマンドレットとして .NET クラスを宣言する方法の詳細については、「 [コマンドレット属性の宣言](./cmdlet-class-declaration.md)」を参照してください。

## <a name="writing-cmdlet-code"></a>コマンドレットコードの記述

このドキュメントでは、コマンドレットコードの記述方法を確認する2つの方法について説明します。 コードをよく説明する必要がない場合は、 [コマンドレットコードの例](./examples-of-cmdlet-code.md)を参照してください。 コードの詳細については、「 [Getproc チュートリアル](./getproc-tutorial.md)」、「 [stopproc チュートリアル](./stopproc-tutorial.md)」、または「 [selectstr チュートリアル](./selectstr-tutorial.md) 」のトピックを参照してください。

コマンドレットの記述に関するガイドラインの詳細については、「 [コマンドレットの開発ガイドライン](./cmdlet-development-guidelines.md)」を参照してください。

## <a name="see-also"></a>参照

[PowerShell コマンドレットの概念](./windows-powershell-cmdlet-concepts.md)

[PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[PowerShell SDK](../windows-powershell-reference.md)
