---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット エラー レポート
description: コマンドレット エラー レポート
ms.openlocfilehash: f06cf98183d56249080623895bd1f5a3e070cefd
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653413"
---
# <a name="cmdlet-error-reporting"></a>コマンドレットのエラー報告

コマンドレットでは、エラーがエラーの終了または終了していないエラーであるかどうかに応じて、異なる方法でエラーを報告します。 終了エラーとは、パイプラインが直ちに終了するエラー、または処理を続行する理由がない場合に発生するエラーです。 終了しないエラーとは、現在のエラー状態を報告するエラーですが、コマンドレットは入力オブジェクトの処理を続行できます。 終了しないエラーが発生した場合、通常、ユーザーには問題が通知されますが、コマンドレットは次の入力オブジェクトの処理を続行します。

## <a name="terminating-and-nonterminating-errors"></a>終了エラーと終了しないエラー

次のガイドラインを使用して、エラー状態が終了エラーまたは終了しないエラーであるかどうかを判断できます。

- コマンドレットがそれ以上の入力オブジェクトを正常に処理できないようにするには、エラー条件を使用します。 その場合、終了エラーになります。

- 特定の入力オブジェクトまたは入力オブジェクトのサブセットに関連するエラー条件ですか。 それ以外の場合は、終了しないエラーになります。

- コマンドレットは、複数の入力オブジェクトを受け入れます。この場合、別の入力オブジェクトで処理が成功する可能性がありますか。 それ以外の場合は、終了しないエラーになります。

- 複数の入力オブジェクトを受け入れることができるコマンドレットでは、特定の状況が1つの入力オブジェクトにのみ適用される場合でも、終了エラーと終了しないエラーのどちらを使用するかを決定する必要があります。

- コマンドレットは、任意の数の入力オブジェクトを受信し、任意の数の成功またはエラーオブジェクトを送信してから、終了例外をスローすることができます。 受信した入力オブジェクトの数と、送信された成功したエラーオブジェクトの数の間にはリレーションシップがありません。

- 0-1 の入力オブジェクトのみを受け入れ、0-1 出力オブジェクトのみを生成できるコマンドレットでは、エラーを終了エラーとして扱い、終了例外を生成する必要があります。

## <a name="reporting-nonterminating-errors"></a>終了しないエラーの報告

終了しないエラーの報告は、常に、コマンドレットによって実行されるようにする必要があり [ます。この](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) メソッドの実装には、または、システムの [管理、.](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) ........................... [...](/dotnet/api/System.Management.Automation.Cmdlet.EndProcessing) ... この種のエラーは、 [WriteError](/dotnet/api/System.Management.Automation.Cmdlet.WriteError) メソッドを呼び出してエラーストリームにエラーレコードを送信することによって報告されます。

## <a name="reporting-terminating-errors"></a>終了エラーの報告

終了エラーは、例外をスローするか、 [ThrowTerminatingError](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError) メソッドを呼び出すことによって報告されます。 コマンドレットでは、 **OutOfMemory** などの例外をキャッチして再スローすることもできますが、PowerShell ランタイムでも例外がキャッチされるため、例外を再スローする必要はありません。

また、状況に固有の問題に対して独自の例外を定義したり、エラーレコードを使用して既存の例外に追加情報を追加したりすることもできます。

## <a name="error-records"></a>エラーレコード

PowerShell では、 [システム管理](/dotnet/api/System.Management.Automation.ErrorRecord) オブジェクトを使用した終了しないエラー状態について説明します。 各オブジェクトは、エラーカテゴリ情報、オプションのターゲットオブジェクト、およびエラー状態に関する詳細を提供します。

### <a name="error-identifiers"></a>エラー識別子

エラー識別子は、コマンドレット内のエラー状態を識別する単純な文字列です。
PowerShell では、この識別子をコマンドレット識別子と組み合わせて、エラーストリームのフィルター処理やエラーのログ記録時、特定のエラーへの応答時、またはユーザー固有のその他の操作を行うときに、後で使用できる完全修飾エラー識別子を作成します。

エラー識別子を指定するときは、次のガイドラインに従う必要があります。

- さまざまな固有のエラー識別子を異なるコードパスに割り当てます。 [WriteError](/dotnet/api/System.Management.Automation.Cmdlet.WriteError)または[ThrowTerminatingError](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError)を呼び出す各コードパスには、独自のエラー識別子を指定することをお勧めします。

- エラー識別子は、終了エラーと終了エラーの両方の共通言語ランタイム (CLR) 例外型に対して一意である必要があります。

- コマンドレットまたは PowerShell プロバイダーのバージョン間でエラー識別子のセマンティクスを変更しないでください。 エラー id のセマンティクスが確立された後は、コマンドレットのライフサイクル全体にわたって一定の状態を維持する必要があります。

- 終了エラーの場合は、特定の CLR 例外の種類に対して一意のエラー識別子を使用します。 例外の種類が変更された場合は、新しいエラー識別子を使用します。

- 終了しないエラーの場合は、特定の入力オブジェクトに対して特定のエラー識別子を使用します。

- Tersely が報告されているエラーに対応する識別子のテキストを選択します。 空白や句読点は使用しないでください。

- 再現できないエラー識別子は生成しません。 たとえば、プロセス識別子を含む識別子は生成しません。 エラー識別子は、同じ問題が発生している他のユーザーによって検出された識別子に対応している場合にのみ役立ちます。

### <a name="error-categories"></a>エラーカテゴリ

エラーカテゴリは、ユーザーのエラーをグループ化するために使用されます。 PowerShell では、これらのカテゴリとコマンドレットを定義します。 PowerShell プロバイダーは、エラーレコードを生成するときにこれらのカテゴリを選択する必要があります。

使用可能なエラーカテゴリの説明については、 [ErrorCategory](/dotnet/api/System.Management.Automation.ErrorCategory) 列挙体を参照してください。 一般に、可能な限り **Noerror**、 **Undefinederror**、 **genericerror** の使用は避けてください。

ユーザーは、カテゴリビューに設定されたときに、カテゴリに基づいてエラーを表示でき `$ErrorView` ます。 

## <a name="see-also"></a>関連項目

[コマンドレットの概要](./cmdlet-overview.md)

[コマンドレット出力の種類](./types-of-cmdlet-output.md)

[Windows PowerShell リファレンス](../windows-powershell-reference.md)
