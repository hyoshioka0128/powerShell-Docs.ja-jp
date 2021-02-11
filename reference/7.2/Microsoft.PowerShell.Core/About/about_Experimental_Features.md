---
description: PowerShell の試験的機能のサポートは、試験的機能を PowerShell または PowerShell モジュールの既存の安定した機能と共存させるためのメカニズムを提供します。
Locale: en-US
ms.date: 03/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_experimental_features?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: 試験的な機能について
ms.openlocfilehash: 82f5ef9838fcbb98e71e362ad00b1ec68f9a9f67
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600809"
---
# <a name="experimental-features"></a>試験的な機能

PowerShell の試験的機能のサポートは、試験的機能を PowerShell または PowerShell モジュールの既存の安定した機能と共存させるためのメカニズムを提供します。

試験的機能は、設計が未完成です。 この機能は、ユーザーがテストしてフィードバックを提供するために使用できます。 試験的機能が完成すると、設計の変更が破壊的変更になります。 試験的機能は、変更が破壊的になる可能性があるため、運用環境で使用することは想定されていません。

試験的な機能は既定で無効になっており、システムのユーザーまたは管理者が明示的に有効にする必要があります。

有効な試験的な機能は、 `powershell.config.json` `$PSHOME` 特定のユーザーのすべてのユーザーまたはユーザー固有の構成ファイルのファイルに一覧表示されます。

> [!NOTE]
> ユーザー構成ファイルで有効になっている試験的な機能は、システム構成ファイルに記載されている試験的な機能よりも優先されます。

## <a name="the-experimental-attribute"></a>実験的属性

属性を使用し `Experimental` て、実験用として一部のコードを宣言します。

次の構文を使用して、 `Experimental` 試験的な機能の名前を提供する属性と、実験的な機能が有効になっている場合に実行するアクションを宣言します。

```csharp
[Experimental(NameOfExperimentalFeature, ExperimentAction)]
```

モジュールの場合、は `NameOfExperimentalFeature` の形式に従っている必要があり `<modulename>.<experimentname>` ます。 `ExperimentAction`パラメーターを指定する必要があります。有効な値は次のとおりです。

- `Show` 機能が有効になっている場合に、この試験的な機能を表示することを意味します
- `Hide` 機能が有効になっている場合に、この実験的な機能を非表示にすることを意味します。

### <a name="declaring-experimental-features-in-modules-written-in-c"></a>C で記述されたモジュールでの試験的な特徴の宣言\#

試験的な機能フラグを使用するモジュールの作成者は、属性を使用して、コマンドレットを試験段階として宣言でき `Experimental` ます。

```csharp
[Experimental("MyWebCmdlets.PSWebCmdletV2", ExperimentAction.Show)]
[Cmdlet(Verbs.Invoke, "WebRequest")]
public class InvokeWebRequestCommandV2 : WebCmdletBaseV2 { ... }
```

### <a name="declaring-experimental-features-in-modules-written-in-powershell"></a>PowerShell で記述されたモジュールでの試験的な機能の宣言

PowerShell で記述されたモジュールは、属性を使用して試験的なコマンドレットを宣言することもでき `Experimental` ます。

```powershell
function Enable-SSHRemoting {
    [Experimental("MyRemoting.PSSSHRemoting", "Show")]
    [CmdletBinding()]
    param()
    ...
}
```

### <a name="mutually-exclusive-experimental-features"></a>相互排他的な実験用の機能

試験的な機能を既存の機能または別の試験的な機能と並行して共存させることができない場合があります。

たとえば、既存のコマンドレットをオーバーライドする実験的なコマンドレットを使用できます。 2つのバージョンをサイドバイサイドで共存させることはできません。 この設定では、 `ExperimentAction.Hide` 2 つのコマンドレットのうち1つだけを同時に有効にすることができます。

この例では、新しい実験的なコマンドレットを作成し `Invoke-WebRequest` ます。
`InvokeWebRequestCommand` 試験的ではない実装が含まれています。
`InvokeWebRequestCommandV2` コマンドレットの実験用バージョンを含みます。

を使用すると、 `ExperimentAction.Hide` 2 つの機能のうち1つのみを同時に有効にすることができます。

```csharp
[Experimental("MyWebCmdlets.PSWebCmdletV2", ExperimentAction.Show)]
[Cmdlet(Verbs.Invoke, "WebRequest")]
public class InvokeWebRequestCommandV2 : WebCmdletBaseV2 { ... }

[Experimental("MyWebCmdlets.PSWebCmdletV2", ExperimentAction.Hide)]
[Cmdlet(Verbs.Invoke, "WebRequest")]
public class InvokeWebRequestCommand : WebCmdletBase { ... }
```

試験的 `MyWebCmdlets.PSWebCmdletV2` な機能が有効になっている場合、既存の `InvokeWebRequestCommand` 実装は非表示になり、は `InvokeWebRequestCommandV2` の実装を提供し `Invoke-WebRequest` ます。

これにより、ユーザーは新しいコマンドレットを試して、フィードバックを提供し、必要に応じて試験的ではないバージョンに戻すことができます。

### <a name="experimental-parameters-in-cmdlets"></a>コマンドレットの試験的なパラメーター

属性は、 `Experimental` 個々のパラメーターにも適用できます。 これにより、完全に新しいコマンドレットを使用するのではなく、既存のコマンドレットの試験的なパラメーターセットを作成できます。

C# の例を次に示します。

```csharp
[Experimental("MyModule.PSNewAddTypeCompilation", ExperimentAction.Show)]
[Parameter(ParameterSet = "NewCompilation")]
public CompilationParameters CompileParameters { ... }

[Experimental("MyModule.PSNewAddTypeCompilation", ExperimentAction.Hide)]
[Parameter()]
public CodeDom CodeDom { ... }
```

PowerShell スクリプトの別の例を次に示します。

```powershell
param(
    [Experimental("MyModule.PSNewFeature", "Show")]
    [string] $NewName,

    [Experimental("MyModule.PSNewFeature", "Hide")]
    [string] $OldName
)
```

## <a name="checking-if-an-experimental-feature-is-enabled"></a>試験的な機能が有効かどうかを確認しています

コードでは、適切な処置を行う前に試験的な機能が有効になっているかどうかを確認する必要があります。 クラスの静的メソッドを使用して試験的な機能が有効になっているかどうかを確認でき `IsEnabled()` `System.Management.Automation.ExperimentalFeature` ます。

C# の例を次に示します。

```csharp
if (ExperimentalFeature.IsEnabled("MyModule.MyExperimentalFeature"))
{
   // code specific to the experimental feature
}
```

PowerShell スクリプトの例を次に示します。

```powershell
if ([ExperimentalFeature]::IsEnabled("MyModule.MyExperimentalFeature"))
{
  # code specific to the experimental feature
}
```

## <a name="see-also"></a>関連項目

[Enable-ExperimentalFeature](xref:Microsoft.PowerShell.Core.Enable-ExperimentalFeature)

[Disable-ExperimentalFeature](xref:Microsoft.PowerShell.Core.Disable-ExperimentalFeature)

[Get-ExperimentalFeature](xref:Microsoft.PowerShell.Core.Get-ExperimentalFeature)

