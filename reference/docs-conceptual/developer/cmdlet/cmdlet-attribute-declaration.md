---
ms.date: 09/13/2016
ms.topic: reference
title: コマンドレット属性の宣言
description: コマンドレット属性の宣言
ms.openlocfilehash: 6bdfe39a4ab9ef4d4cc98daa592f69f7fab95e84
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92653542"
---
# <a name="cmdlet-attribute-declaration"></a>コマンドレット属性の宣言

コマンドレット属性は、コマンドレットとして Microsoft .NET Framework クラスを識別し、コマンドレットを呼び出すために使用される動詞と名詞を指定します。

## <a name="syntax"></a>構文

```csharp
[Cmdlet("verbName", "nounName")]
[Cmdlet("verbName", "nounName", Named Parameters...)]
```

#### <a name="parameters"></a>パラメーター

`VerbName` ([System.string](/dotnet/api/System.String)) が必要です。 コマンドレットの動詞を指定します。 この動詞は、コマンドレットによって実行されるアクションを指定します。 承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md) 」および「 [必要な開発ガイドライン](./required-development-guidelines.md)」を参照してください。

`NounName` ([System.string](/dotnet/api/System.String)) が必要です。 コマンドレットの名詞を指定します。 この名詞は、コマンドレットが処理するリソースを指定します。 コマンドレットの名詞の詳細については、「[コマンドレットの宣言](./cmdlet-class-declaration.md)」を[参照してください。](./strongly-encouraged-development-guidelines.md)

`SupportsShouldProcess` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 `True`コマンドレットが、システムを変更するアクションを実行する前にユーザーにプロンプトを表示する方法を提供する、コマンドレットの呼び出しをサポートすることを[示します。](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) `False`既定値は、コマンドレットが、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) の呼び出しをサポートしていないことを示します。 確認要求の詳細については、「 [確認の要求](./requesting-confirmation-from-cmdlets.md)」を参照してください。

`ConfirmImpact` ([システムの管理. Confirmimpact](/dotnet/api/System.Management.Automation.ConfirmImpact)) 省略可能な名前付きパラメーター。 コマンドレットのアクションを、 [システム](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess) の呼び出しによって確認するかどうかを指定します。 コマンドレットの ConfirmImpact 値 (既定では Medium) が変数の値以上である場合にのみ、指定された....[コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)が呼び出されます。 `$ConfirmPreference` このパラメーターは、パラメーターが指定されている場合にのみ指定する必要があり `SupportsShouldProcess` ます。

`DefaultParameterSetName` ([System.string](/dotnet/api/System.String)) 省略可能な名前付きパラメーター。 使用するパラメーターセットを判別できない場合に、Windows PowerShell ランタイムが使用を試みる既定のパラメーターセットを指定します。 各パラメーターの unique パラメーターに必須パラメーターを設定することで、この状況を解消できることに注意してください。

既定のパラメーターセット名が指定されている場合でも、Windows PowerShell で既定のパラメーターセットを使用できないケースが1つあります。 Windows PowerShell ランタイムでは、オブジェクトの種類のみを基にしたパラメーターセットを区別できません。 たとえば、ファイルパスとして文字列を受け取る1つのパラメーターセットと、 **FileInfo** オブジェクトを直接受け取る別のセットがある場合、Windows PowerShell は、コマンドレットに渡された値に基づいて、使用するパラメーターセットを決定できません。また、既定のパラメーターセットを使用しません。 この場合、既定のパラメーターセット名を指定しても、Windows PowerShell はあいまいなパラメーターセットのエラーメッセージをスローします。

`SupportsTransactions` ([System.string) 省略](/dotnet/api/System.Boolean)可能な名前付きパラメーター。 `True` コマンドレットをトランザクション内で使用できることを示します。 `True`を指定すると、Windows PowerShell ランタイムは `UseTransaction` パラメーターをコマンドレットのパラメーターリストに追加します。 `False`既定値は、コマンドレットをトランザクション内で使用できないことを示します。

## <a name="remarks"></a>解説

- 動詞と名詞を一緒に使用して、登録されているコマンドレットを識別し、スクリプト内でコマンドレットを呼び出します。

- コマンドレットが Windows PowerShell コンソールから呼び出されると、コマンドは次のようになります。

**VerbName-NounName**

- Windows PowerShell の外部でリソースを変更するすべてのコマンドレットには、コマンドレット属性が宣言されているときにキーワードを含める必要があります。これにより、コマンドレットは、コマンドレットが `SupportsShouldProcess` アクションを実行する前に、このメソッドを呼び出すことができ[](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)ます。 [コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)の呼び出しでが返された場合 `false` 、アクションを実行する必要はありません。 [コマンドレット](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)の呼び出しによって生成される確認要求の詳細については、「[確認の要求](./requesting-confirmation-from-cmdlets.md)」を参照してください。

`Confirm`および `WhatIf` コマンドレットのパラメーターは、の呼び出しをサポートするコマンドレットでのみ使用でき[ます](/dotnet/api/System.Management.Automation.Cmdlet.ShouldProcess)。

## <a name="example"></a>例

次のクラス定義では、コマンドレット属性を使用して、ローカルコンピューター上で実行されているプロセスに関する情報を取得する **Get Proc** コマンドレットの .NET Framework クラスを識別します。

```csharp
[Cmdlet(VerbsCommon.Get, "Proc")]
public class GetProcCommand : Cmdlet
```

**Get proc** コマンドレットの詳細については、「 [getproc チュートリアル](./getproc-tutorial.md)」を参照してください。

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](./writing-a-windows-powershell-cmdlet.md)
