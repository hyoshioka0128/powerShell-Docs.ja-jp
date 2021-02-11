---
ms.date: 09/13/2016
ms.topic: reference
title: ErrorRecord オブジェクトを解釈する
description: ErrorRecord オブジェクトを解釈する
ms.openlocfilehash: 5a8556e0ba83e54dd197ed03665a119f0b565f2b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666877"
---
# <a name="interpreting-errorrecord-objects"></a>ErrorRecord オブジェクトを解釈する

ほとんどの場合、 [System. ErrorRecord](/dotnet/api/System.Management.Automation.ErrorRecord) オブジェクトは、コマンドまたはスクリプトによって生成される終了しないエラーを表します。 終了エラーでは、 [Icontainserrorrecord](/dotnet/api/System.Management.Automation.IContainsErrorRecord) インターフェイスを使用して、errorrecord 内の追加情報を指定することもできます。

コマンドまたはスクリプトの実行中に発生した特定のエラーを処理するためのエラーハンドラーをスクリプトまたはホストに記述する場合は、処理するエラーのクラスを表すかどうかを判断するために、 [システムの管理](/dotnet/api/System.Management.Automation.ErrorRecord) オブジェクトを解釈する必要があります。

コマンドレットで終了または終了しないエラーが発生した場合は、エラー状態を説明するエラーレコードを作成する必要があります。 ホストアプリケーションは、これらのエラーレコードを調査し、エラーを軽減する何らかのアクションを実行する必要があります。 また、ホストアプリケーションは、レコードを処理できなかったが続行できた、終了しないエラーのエラーレコードを調査する必要があります。また、パイプラインの停止の原因となったエラーを終了するために、エラーレコードを調査する必要があります。

> [!NOTE]
> 終了エラーの場合、コマンドレットは [Throwterminatingerror *](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError) メソッドを呼び出します。 終了しないエラーの場合、コマンドレットは [WriteError](/dotnet/api/System.Management.Automation.Cmdlet.WriteError) メソッドを呼び出します。

## <a name="error-record-design"></a>エラーレコードのデザイン

エラーレコードは、各エラーレコードの結合された情報が一意であることを確認しながら、例外では利用できない追加のエラー情報を提供するように設計されています。 この一意性により、ホストアプリケーションはエラーレコードのさまざまな部分を検査して、ホストが実行する必要のあるエラー状況とアクションを識別できるようになります。

## <a name="interpreting-error-records"></a>エラーレコードの解釈

エラーレコードのいくつかの部分を確認して、エラーを特定することができます。 これらの部分は次のとおりです。

- エラーカテゴリ

- エラー例外

- 完全修飾エラー識別子 (完全修飾 ID)

- その他の情報

### <a name="the-error-category"></a>エラーカテゴリ

エラーレコードのエラーカテゴリは、 [Errorcategory](/dotnet/api/System.Management.Automation.ErrorCategory) 列挙型によって提供される定義済み定数の1つです。 この情報は、[システム](/dotnet/api/System.Management.Automation.ErrorRecord)の管理. errorrecord オブジェクトの system.servicemodel [info](/dotnet/api/System.Management.Automation.ErrorRecord.CategoryInfo)プロパティを使用して取得できます ()。

コマンドレットでは、CloseError、OpenError、InvalidType、ReadError、および WriteError の各カテゴリとその他のエラーカテゴリを指定できます。 ホストアプリケーションはエラーカテゴリを使用して、エラーのグループをキャプチャできます。

### <a name="the-exception"></a>例外

エラーレコードに含まれる例外はコマンドレットによって提供され、この例外は[、システムの](/dotnet/api/System.Management.Automation.ErrorRecord)管理. errorrecord オブジェクトの system.string [*](/dotnet/api/System.Management.Automation.ErrorRecord.Exception)プロパティを介してアクセスできます。

ホストアプリケーションはキーワードを使用して、 `is` 例外が特定のクラスまたは派生クラスであることを識別できます。 次の例に示すように、例外の種類に分岐することをお勧めします。

`if (MyNonTerminatingError.Exception is AccessDeniedException)`

このようにして、派生クラスをキャッチします。 ただし、例外が逆シリアル化されると、問題が発生します。

### <a name="the-fqid"></a>すべての ID

この ID は、エラーを識別するために使用できる最も具体的な情報です。 これは、コマンドレットで定義された識別子、コマンドレットクラスの名前、およびエラーを報告したソースを含む文字列です。 一般に、エラーレコードは、Windows イベントログのイベントレコードに似ています。 このような ID は、イベントレコードのクラス (*ログ名*、 *ソース*、 *イベント ID*) を識別する次の組に似ています。

使用できる ID は、1つの文字列として検査されるように設計されています。 ただし、エラー識別子がホストアプリケーションによって解析されるように設計されているケースもあります。 次の例は、正しい形式の完全修飾エラー識別子です。

`CommandNotFoundException,Microsoft.PowerShell.Commands.GetCommandCommand.`

前の例では、最初のトークンはエラー識別子であり、その後にコマンドレットクラスの名前が続きます。 エラー識別子には、1つのトークンを指定することも、識別子を検査するときに分岐を許可するドット区切りの識別子を指定することもできます。 エラー識別子に空白または句読点を使用しないでください。 コンマを使用しないことが特に重要です。Windows PowerShell では、識別子とコマンドレットのクラス名を区切るためにコンマが使用されます。

### <a name="other-information"></a>その他の情報

また [、エラー](/dotnet/api/System.Management.Automation.ErrorRecord) が発生した環境を説明する情報を提供することもできます。 この情報には、エラーの詳細、呼び出し情報、エラーが発生したときに処理されていた対象オブジェクトなどが含まれます。 この情報はホストアプリケーションにとって有用な場合がありますが、通常はエラーを識別するために使用されません。 この情報は、次のプロパティを使用して取得できます。

[システム管理. ErrorRecord. Errorrecord](/dotnet/api/System.Management.Automation.ErrorRecord.ErrorDetails)

[InvocationInfo (システムの管理)](/dotnet/api/System.Management.Automation.ErrorRecord.InvocationInfo)

[TargetObject (システムの管理)](/dotnet/api/System.Management.Automation.ErrorRecord.TargetObject)

## <a name="see-also"></a>参照

[システムの管理. ErrorRecord](/dotnet/api/System.Management.Automation.ErrorRecord)

[Errorcategory (システム管理)](/dotnet/api/System.Management.Automation.ErrorCategory)

[システムの管理. Errorカテゴリの情報](/dotnet/api/System.Management.Automation.ErrorCategoryInfo)

[WriteError (システム管理)](/dotnet/api/System.Management.Automation.Cmdlet.WriteError)

[Throwterminatingerror * のようになります。](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError)

[終了しないエラーのレポートをコマンドレットに追加する](./adding-non-terminating-error-reporting-to-your-cmdlet.md)

[Windows PowerShell エラー レポート](./error-reporting-concepts.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
