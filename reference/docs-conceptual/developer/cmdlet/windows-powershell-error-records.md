---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell エラー レコード
description: Windows PowerShell エラー レコード
ms.openlocfilehash: 899acf08508b1469b7ec3985d5665367fc2c1531
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355597"
---
# <a name="windows-powershell-error-records"></a>Windows PowerShell エラー レコード

コマンドレットは、終了エラーと終了エラーのエラー状態を識別する、 [システムの管理. ErrorRecord](/dotnet/api/System.Management.Automation.ErrorRecord) オブジェクトを渡す必要があります。

この [オブジェクトには、](/dotnet/api/System.Management.Automation.ErrorRecord) 次の情報が含まれています。

- エラーを説明する例外。 多くの場合、これは、コマンドレットがキャッチし、エラーレコードに変換した例外です。 すべてのエラーレコードに例外が含まれている必要があります。

コマンドレットが例外をキャッチしなかった場合は、新しい例外を作成し、エラー状態に最も適した例外クラスを選択する必要があります。 ただし、例外をスローする必要はありません。これは、この例外をスローする必要はありません。これは、[システム](/dotnet/api/System.Management.Automation.ErrorRecord)の管理. errorrecord オブジェクトの[system.object プロパティを](/dotnet/api/System.Management.Automation.ErrorRecord.Exception)介してアクセスできるためです。

- 診断の目的で使用できるターゲット指定子と、特定のエラーハンドラーで特定のエラー条件を処理するための Windows PowerShell スクリプトによって提供される、エラー識別子。 すべてのエラーレコードには、エラー識別子を含める必要があります (「エラー識別子」を参照してください)。

- 診断目的で使用できる一般的な指定子を提供するエラーカテゴリ。
  すべてのエラーレコードでエラーカテゴリを指定する必要があります (「エラーカテゴリ」を参照してください)。

- オプションの置換エラーメッセージと推奨される操作 (置換エラーメッセージを参照)。

- エラーをスローしたコマンドレットに関するオプションの呼び出し情報。 この情報は、Windows PowerShell によって指定されます (「呼び出しメッセージ」を参照してください)。

- エラーが発生したときに処理されていたターゲットオブジェクト。 これは入力オブジェクトである場合もあれば、コマンドレットが処理していた別のオブジェクトである場合もあります。 たとえば、コマンドの場合、 `remove-item -recurse c:\somedirectory` エラーは "c:\somedirectory\lockedfile" の FileInfo オブジェクトのインスタンスである可能性があります。 ターゲットオブジェクトの情報は省略可能です。

## <a name="error-identifier"></a>エラー識別子

エラーレコードを作成するときに、コマンドレット内でエラー条件を指定する識別子を指定します。 Windows PowerShell では、対象となる識別子をコマンドレットの名前と組み合わせて、完全修飾エラー識別子を作成します。 完全修飾されたエラー識別子にアクセスするに[は、FullyQualifiedErrorId オブジェクトの](/dotnet/api/System.Management.Automation.ErrorRecord) [](/dotnet/api/System.Management.Automation.ErrorRecord.FullyQualifiedErrorId)プロパティを使用する必要がありますので、 エラー識別子は、単独では使用できません。 完全修飾エラー識別子の一部としてのみ使用できます。

エラーレコードを作成するときにエラー識別子を生成するには、次のガイドラインに従います。

- エラー条件に固有のエラー識別子を作成します。 診断目的のエラー識別子と、特定のエラーハンドラーを使用して特定のエラー条件を処理するスクリプトを対象とします。 ユーザーは、エラー識別子を使用してエラーとそのソースを識別できる必要があります。 また、エラー識別子を使用すると、既存の例外からの特定のエラー条件をレポートして、新しい例外サブクラスが不要になるようにすることもできます。

- 一般に、異なるコードパスに別のエラー識別子を割り当てます。 エンドユーザーは、特定の識別子を活用できます。 多くの場合、 [WriteError](/dotnet/api/System.Management.Automation.Cmdlet.WriteError) または [Throwterminatingerror *](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError) を呼び出す各コードパスには、独自の識別子があります。このような場合は、 規則として、エラーメッセージの新しいテンプレート文字列を定義するときに新しい識別子を定義します。その逆も同様です。 エラーメッセージを識別子として使用しないでください。

- 特定のエラー識別子を使用してコードを発行すると、完全な製品サポートライフサイクルのために、その識別子でのエラーのセマンティクスが確立されます。 元のコンテキストとは意味が異なるコンテキストでは再利用しないでください。 このエラーのセマンティクスが変更された場合は、新しい識別子を作成して使用します。

- 通常は、特定の CLR 型の例外に対してのみ、特定のエラー識別子を使用する必要があります。 例外の型またはターゲットオブジェクトの型が変更された場合は、新しい識別子を作成して使用します。

- 報告するエラーに簡潔に対応する、エラー id のテキストを選択します。 標準の .NET Framework 名前付け規則と大文字小文字の表記規則を使用します。 空白や句読点は使用しないでください。 エラー識別子はローカライズしないでください。

- エラー識別子は、再現不可能な方法で動的に生成しないようにしてください。 たとえば、プロセス ID などのエラー情報を組み込むことはできません。 エラー識別子は、同じエラー条件が発生している他のユーザーによって検出されたエラー識別子に対応している場合にのみ役立ちます。

## <a name="error-category"></a>エラー カテゴリ

エラーレコードを作成するときに、 [ErrorCategory](/dotnet/api/System.Management.Automation.ErrorCategory) 列挙体で定義されている定数の1つを使用して、エラーのカテゴリを指定します。 Windows PowerShell は、ユーザーが変数をに設定したときに、エラーカテゴリを使用してエラー情報を表示し `$ErrorView` `"CategoryView"` ます。

[ErrorCategory](/dotnet/api/System.Management.Automation.ErrorCategory) 
 **notspecified** 定数は使用しないでください。 エラーまたはエラーの原因となった操作に関する情報がある場合は、カテゴリが完全に一致していない場合でも、エラーまたは操作について最もよく説明されているカテゴリを選択します。

Windows PowerShell によって表示される情報は、カテゴリビュー文字列として参照され、 [システム](/dotnet/api/System.Management.Automation.ErrorCategoryInfo) のプロパティから構築されます。 (このクラスには、エラーの "システムの管理... [ErrorRecord. カテゴリ情報](/dotnet/api/System.Management.Automation.ErrorRecord.CategoryInfo) " プロパティを使用してアクセスします)。

```
{Category}: ({TargetName}:{TargetType}):[{Activity}], {Reason}
```

次の一覧に、表示される情報を示します。

- Category: Windows PowerShell で定義された [ErrorCategory](/dotnet/api/System.Management.Automation.ErrorCategory) 定数。

- TargetName: 既定では、エラーが発生したときにコマンドレットが処理していたオブジェクトの名前。
  または、別のコマンドレットで定義された文字列。

- TargetType: 既定では、ターゲットオブジェクトの型です。 または、別のコマンドレットで定義された文字列。

- アクティビティ: 既定では、エラーレコードを作成したコマンドレットの名前。 または、他のコマンドレットで定義された文字列。

- 理由: 既定では、例外の種類です。 または、別のコマンドレットで定義された文字列。

## <a name="replacement-error-message"></a>置換エラーメッセージ

コマンドレットのエラーレコードを作成する場合、エラーの既定のエラーメッセージは、 [system.exception](/dotnet/api/System.Exception.Message) プロパティの既定のメッセージテキストから取得されます。 これは、(.NET Framework のガイドラインに従って) デバッグの目的でのみ使用されるメッセージテキストを含む読み取り専用のプロパティです。 既定のメッセージテキストを置換または補強するエラーメッセージを作成することをお勧めします。 メッセージをよりわかりやすく、コマンドレットにより具体的なものにします。

置換メッセージ [は、system.servicemodel オブジェクトに](/dotnet/api/System.Management.Automation.ErrorDetails) よって提供されます。 Windows PowerShell で使用できる追加のローカライズ情報を提供するため、このオブジェクトの次のいずれかのコンストラクターを使用します。

- [Errordetails (コマンドレット、文字列、文字列、オブジェクト [])](/dotnet/api/system.management.automation.errordetails.-ctor#System_Management_Automation_ErrorDetails__ctor_System_Management_Automation_Cmdlet_System_String_System_String_System_Object___): テンプレート文字列が、コマンドレットが実装されている同じアセンブリ内のリソース文字列であるか、またはテンプレート文字列をオーバーライドすることによってテンプレート文字列を読み込む場合 [は、この](/dotnet/api/System.Management.Automation.Cmdlet.GetResourceString) コンストラクターを使用します。

- [Errordetails (Assembly、string、string、Object [])](/dotnet/api/system.management.automation.errordetails.-ctor#System_Management_Automation_ErrorDetails__ctor_System_Reflection_Assembly_System_String_System_String_System_Object___): このコンストラクターは、テンプレート文字列が別のアセンブリ内にあり、それをオーバーライドして読み込むことができない場合に使用し[ます。](/dotnet/api/System.Management.Automation.Cmdlet.GetResourceString)

この置換メッセージは、例外メッセージを作成するための .NET Framework 設計ガイドラインに従っている必要がありますが、わずかな違いがあります。 例外メッセージが開発者向けに記述されていることを示すガイドラインの状態。 これらの置換メッセージは、コマンドレットユーザー用に記述する必要があります。

置換エラーメッセージは、 [WriteError](/dotnet/api/System.Management.Automation.Cmdlet.WriteError) または [Throwterminatingerror *](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError) メソッドが呼び出される前に追加されている必要があります。このメッセージは、 置換メッセージを追加するには、エラーレコード [の "system.string](/dotnet/api/System.Management.Automation.ErrorRecord.ErrorDetails) " プロパティを設定します。 このプロパティが設定されている場合、Windows PowerShell は、既定のメッセージテキストではなく、system.string [*](/dotnet/api/System.Management.Automation.ErrorDetails.Message) プロパティを表示します。

## <a name="recommended-action-information"></a>推奨される操作情報

また [、エラー](/dotnet/api/System.Management.Automation.ErrorDetails) が発生したときに推奨される操作に関する情報を提供することもできます。

## <a name="invocation-information"></a>呼び出し情報

コマンドレットが [WriteError](/dotnet/api/System.Management.Automation.Cmdlet.WriteError) または [Throwterminatingerror *](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError) を使用してエラーレコードを報告すると、Windows PowerShell によって、エラーが発生したときに呼び出されたコマンドを説明する情報が自動的に追加されます。 この情報は、 [Invocationinfo](/dotnet/api/System.Management.Automation.InvocationInfo) オブジェクトによって提供されます。このオブジェクトには、コマンドによって呼び出されたコマンドレットの名前、コマンド自体、およびパイプラインまたはスクリプトに関する情報が含まれています。 このプロパティは読み取り専用です。

## <a name="see-also"></a>参照

[WriteError (システム管理)](/dotnet/api/System.Management.Automation.Cmdlet.WriteError)

[Throwterminatingerror * のようになります。](/dotnet/api/System.Management.Automation.Cmdlet.ThrowTerminatingError)

[ErrorCategory (システム管理)](/dotnet/api/System.Management.Automation.ErrorCategory)

[システムの管理. Errorカテゴリの情報](/dotnet/api/System.Management.Automation.ErrorCategoryInfo)

[システムの管理. ErrorRecord](/dotnet/api/System.Management.Automation.ErrorRecord)

[システムの管理. ErrorDetails](/dotnet/api/System.Management.Automation.ErrorDetails)

[Invocationinfo (システム管理)](/dotnet/api/System.Management.Automation.InvocationInfo)

[Windows PowerShell エラー レポート](./error-reporting-concepts.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
