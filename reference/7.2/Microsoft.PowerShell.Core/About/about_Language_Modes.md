---
description: 言語モードと PowerShell セッションへの影響について説明します。
Locale: en-US
ms.date: 09/09/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_language_modes?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Language_Modes
ms.openlocfilehash: 9e4b1ec2dd16d3442c553460ff217e7e0223f41f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602752"
---
# <a name="about-language-modes"></a>言語モードについて

## <a name="short-description"></a>概要
言語モードと PowerShell セッションへの影響について説明します。

## <a name="long-description"></a>詳細説明

PowerShell セッションの言語モードによって、PowerShell 言語のどの要素をセッションで使用できるかが決まります。

PowerShell は、次の言語モードをサポートしています。

- FullLanguage
- ConstrainedLanguage (PowerShell 3.0 で導入)
- RestrictedLanguage
- NoLanguage

### <a name="what-is-a-language-mode"></a>言語モードとは

言語モードによって、セッションで許可される言語要素が決まります。

言語モードは、実際にはセッションの作成に使用されるセッション構成 (または "エンドポイント") のプロパティです。 特定のセッション構成を使用するすべてのセッションには、セッション構成の言語モードがあります。

すべての PowerShell セッションには、コマンドレットを使用して作成した pssession `New-PSSession` 、ComputerName パラメーターを使用する一時的なセッション、powershell を起動したときに表示される既定のセッションなど、言語モードがあります。

リモートセッションは、リモートコンピューター上のセッション構成を使用して作成されます。 セッション構成で設定されている言語モードによって、セッションの言語モードが決まります。 PSSession のセッション構成を指定するには、セッションを作成するコマンドレットの ConfigurationName パラメーターを使用します。

### <a name="language-modes"></a>言語モード

このセクションでは、PowerShell セッションの言語モードについて説明します。

#### <a name="full-language-fulllanguage"></a>全言語 (FullLanguage)

FullLanguage モードでは、セッションのすべての言語要素が許可されます。
FullLanguage は、windows RT を除くすべてのバージョンの Windows で既定のセッションの既定の言語モードです。

#### <a name="restricted-language-restrictedlanguage"></a>制限付き言語 (RestrictedLanguage)

RestrictedLanguage モードでは、ユーザーはコマンド (コマンドレット、関数、CIM コマンド、ワークフロー) を実行できますが、スクリプトブロックの使用は許可されていません。

既定では、RestrictedLanguage モードでは次の変数のみが許可されます。

- `$PSCulture`
- `$PSUICulture`
- `$True`
- `$False`
- `$Null`

RestrictedLanguage モードを使用するモジュールマニフェストでは、次の追加の変数も許可します。

- `$PSScriptRoot`
- `$PSEdition` (PowerShell Core の場合)
- `$EnabledExperimentalFeatures` (PowerShell Core の場合)

次の比較演算子のみが許可されます。

- `-eq` つの
- `-gt` (より大きい)
- `-lt` (より小さい)

代入ステートメント、プロパティ参照、およびメソッド呼び出しは許可されません。

#### <a name="no-language-nolanguage"></a>言語なし (NoLanguage)

NoLanguage モードは API でのみ使用できます。 NoLanguage モードは、任意の形式のスクリプトテキストが許可されないことを意味します。 これにより、PowerShell スクリプトのフラグメントを送信して解析および実行する **addscript ()** メソッドを使用できなくなります。 **Addcommand ()** と **addcommand ()** のみを使用できます。この場合、パーサーは実行されません。

#### <a name="constrained-language-constrained-language"></a>制約付き言語 (制約付き言語)

ConstrainedLanguage モードでは、すべてのコマンドレットとすべての PowerShell 言語要素が許可されますが、許可される種類は制限されます。

ConstrainedLanguage モードは、Windows RT でのユーザーモードコードの整合性 (UMCI) をサポートするように設計されています。 これは Windows RT で唯一サポートされている言語モードですが、サポートされているすべてのシステムで使用できます。

UMCI は、Microsoft 署名および Microsoft 認定アプリのみを Windows RT ベースのデバイスにインストールできるようにすることで、ARM デバイスを保護します。
ConstrainedLanguage モードでは、ユーザーは PowerShell を使用して UMCI を回避または違反できません。

ConstrainedLanguage モードの機能は次のとおりです。

- Windows モジュールのすべてのコマンドレット、およびその他の UMCI によって承認されたコマンドレットは完全に機能し、メモしている場合を除き、システムリソースに完全にアクセスできます。

- PowerShell スクリプト言語のすべての要素が許可されます。

- Windows に含まれるすべてのモジュールをインポートし、モジュールがエクスポートするすべてのコマンドをセッションで実行できます。

- PowerShell ワークフローでは、スクリプトワークフロー (PowerShell 言語で記述されたワークフロー) を記述して実行できます。 XAML ベースのワークフローはサポートされていないため、を使用するなどして、XAML をスクリプトワークフローで実行することはできません `Invoke-Expression -Language XAML` 。 また、入れ子になったワークフローを使用することもできますが、ワークフローで他のワークフローを呼び出すことはできません。

- コマンドレットは署名された `Add-Type` アセンブリを読み込むことができますが、任意の C# コードまたは Win32 api を読み込むことはできません。

- コマンドレットは、許可されて `New-Object` いる型 (以下の一覧を参照) でのみ使用できます。

- PowerShell で使用できるのは、許可されている型のみです (以下を参照)。 その他の型は使用できません。

- 型変換は許可されますが、結果が許可される型である場合に限ります。

- 文字列入力を型に変換するコマンドレットパラメーターは、結果の型が許可されている型である場合にのみ機能します。

- **ToString ()** メソッドと、許可されている型 (以下の一覧を参照) の .net メソッドを呼び出すことができます。 他のメソッドを呼び出すことはできません。

- ユーザーは、許可された型のすべてのプロパティを取得できます。 ユーザーは、コア型に対してのみプロパティの値を設定できます。 次の COM オブジェクトのみが許可されます。

  - **スクリプト. Dictionary**
  - **Scripting.FileSystemObject**
  - **VBScript. RegExp**

ConstrainedLanguage モードでは、次の型を使用できます。 ユーザーは、プロパティの取得、メソッドの呼び出し、およびオブジェクトの型への変換を行うことができます。

許可される型:

- エイリアス属性
- AllowEmptyCollectionAttribute
- AllowEmptyStringAttribute
- AllowNullAttribute
- Array
- Bool
- byte
- char
- 属性の Bindingbindingattribute
- DateTime
- decimal
- DirectoryEntry
- DirectorySearcher
- double
- float
- Guid
- Hashtable
- INT
- Int16
- long
- ManagementClass
- System.management.managementobject
- ManagementObjectSearcher
- NullString
- OutputTypeAttribute
- ParameterAttribute
- PSCredential
- PSDefaultValueAttribute
- PSListModifier
- PSObject
- PSPrimitiveDictionary
- PSReference
- PSTypeNameAttribute
- Regex
- SByte
- string
- SupportsWildcardsAttribute
- SwitchParameter
- システムのグローバリゼーション
- System .Net. IPAddress
- システム .Net. Mail. MailAddress
- BigInteger
- System.Security.SecureString
- TimeSpan
- UInt16
- UInt32
- UInt64

### <a name="finding-the-language-mode-of-a-session-configuration"></a>セッション構成の言語モードの検索

セッション構成ファイルを使用してセッション構成を作成する場合、セッション構成には LanguageMode プロパティがあります。 言語モードを確認するには、LanguageMode プロパティの値を取得します。

```powershell
(Get-PSSessionConfiguration -Name Test).LanguageMode
FullLanguage
```

他のセッション構成では、セッション構成を使用して作成されたセッションの言語モードを見つけることで、言語モードを間接的に見つけることができます。

### <a name="finding-the-language-mode-of-a-session"></a>セッションの言語モードの検索

セッション状態の LanguageMode プロパティの値を取得することで、FullLanguage または ConstrainedLanguage セッションの言語モードを確認できます。

次に例を示します。

```powershell
$ExecutionContext.SessionState.LanguageMode
ConstrainedLanguage
```

ただし、RestrictedLanguage モードと NoLanguage モードのセッションでは、ドットメソッドを使用してプロパティ値を取得することはできません。 代わりに、エラーメッセージに言語モードが表示されます。

RestrictedLanguage セッションでコマンドを実行すると、 `$ExecutionContext.SessionState.LanguageMode` PowerShell は PropertyReferenceNotSupportedInDataSection および VariableReferenceNotSupportedInDataSection エラーメッセージを返します。

- PropertyReferenceNotSupportedInDataSection: プロパティ参照は、制限付き言語モードまたはデータセクションでは許可されていません。
- VariableReferenceNotSupportedInDataSection は、制限された言語モードまたはデータセクションで参照できない変数を参照しています。

`$ExecutionContext.SessionState.LanguageMode`NoLanguage セッションでコマンドを実行すると、PowerShell によって、スクリプトで許可されているエラーメッセージが返されます。

- スクリプト Notallowed: 構文は、この実行空間ではサポートされていません。 これは、言語なしモードであることが原因である可能性があります。

## <a name="see-also"></a>関連項目

- [about_Session_Configuration_Files](about_Session_Configuration_Files.md)
- [about_Session_Configurations](about_Session_Configurations.md)
