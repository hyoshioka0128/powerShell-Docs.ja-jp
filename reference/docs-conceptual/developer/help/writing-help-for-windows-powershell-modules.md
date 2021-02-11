---
ms.date: 04/10/2020
ms.topic: reference
title: PowerShell モジュールのヘルプの記述
description: PowerShell モジュールのヘルプの記述
ms.openlocfilehash: 3bef45c0dd8a7e63bc419bb3e5a7a1783810105b
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92654653"
---
# <a name="writing-help-for-powershell-modules"></a>PowerShell モジュールのヘルプの記述

PowerShell モジュールには、モジュールに関するヘルプトピックや、コマンドレット、プロバイダー、関数、スクリプトなどのモジュールメンバーに関するヘルプトピックを含めることができます。 `Get-Help`コマンドレットは、他の PowerShell 項目のヘルプを表示するのと同じ形式のモジュールヘルプトピックを表示し、ユーザーは標準のコマンドを使用して `Get-Help` ヘルプトピックを取得します。

このドキュメントでは、モジュールのヘルプトピックの形式と適切な配置について説明し、モジュールのヘルプコンテンツに関するガイドラインを示します。

## <a name="types-of-module-help"></a>モジュールのヘルプの種類

モジュールには、次の種類のヘルプを含めることができます。

- **コマンドレットのヘルプを参照** してください。 モジュール内のコマンドレットについて説明するヘルプトピックは、コマンドヘルプスキーマを使用する XML ファイルです。

- **プロバイダーのヘルプ**。 モジュール内のプロバイダーについて説明するヘルプトピックは、プロバイダーヘルプスキーマを使用する XML ファイルです。

- **関数のヘルプ**。 モジュール内の関数について説明するヘルプトピックは、関数内のコマンドヘルプスキーマまたはコメントベースのヘルプトピックを使用する XML ファイルにすることも、スクリプトまたはスクリプトモジュールで使用することもできます。

- **スクリプトのヘルプ**。 モジュール内のスクリプトを記述するヘルプトピックは、スクリプトまたはスクリプトモジュールでコマンドヘルプスキーマまたはコメントベースのヘルプトピックを使用する XML ファイルにすることができます。

- **概念 ("About") のヘルプ**。 概念 ("about") ヘルプトピックを使用して、モジュールとそのメンバーを記述したり、メンバーを組み合わせてタスクを実行する方法を説明したりできます。
  概念説明ヘルプトピックは、Unicode (UTF-8) エンコードを使用したテキストファイルです。 ファイル名には、のような形式を使用する必要があり `about_<name>.help.txt` `about_MyModule.help.txt` ます。 既定では、PowerShell には、次の例のように、これらの概念に関するヘルプトピックが100以上含まれています。

  ```Output
  TOPIC
      about_<subject or module name>

  SHORT DESCRIPTION
      A short, one-line description of the topic contents.

  LONG DESCRIPTION
      A detailed, full description of the subject or purpose of the module.

  EXAMPLES
      Examples of how to use the module or how the subject feature works in practice.

  KEYWORDS
      Terms or titles on which you might expect your users to search for the information in this topic.

  SEE ALSO
      Text-only references for further reading. Hyperlinks cannot work in the PowerShell console.

  ```

すべてのスキーマファイルは、フォルダーにあり `$PSHOME\Schemas\PSMaml` ます。

## <a name="placement-of-module-help"></a>モジュールヘルプの配置

この `Get-Help` コマンドレットは、モジュールディレクトリの言語固有のサブディレクトリにあるモジュールヘルプトピックファイルを検索します。

たとえば、次のディレクトリ構造の図は、SampleModule モジュールのヘルプトピックの場所を示しています。

```
<ModulePath>
         \SampleModule
               \<en-US>
                     \about_SampleModule.help.txt
                     \SampleModule.dll-help.xml
                     \SampleNestedModule.dll-help.xml
               \<fr-FR>
                     \about_SampleModule.help.txt
                     \SampleModule.dll-help.xml
                     \SampleNestedModule.dll-help.xml

```

> [!NOTE]
> この例では、 `<ModulePath>` プレースホルダーは `PSModulePath` `$HOME\Documents\Modules` 、、 `$PSHOME\Modules` 、またはユーザーが指定するカスタムパスなど、環境変数内のパスの1つを表します。

## <a name="getting-module-help"></a>モジュールのヘルプを取得する

ユーザーがモジュールをセッションにインポートすると、そのモジュールのヘルプトピックがモジュールと共にセッションにインポートされます。 モジュールマニフェストの FileList キーの値にヘルプトピックファイルを一覧表示することはできますが、ヘルプトピックはコマンドレットの影響を受けません `Export-ModuleMember` 。

さまざまな言語でモジュールのヘルプトピックを提供できます。 この `Get-Help` コマンドレットは、コントロールパネルの [地域と言語のオプション] 項目で、現在のユーザーに対して指定されている言語のモジュールヘルプトピックを自動的に表示します。 Windows Vista 以降のバージョンの Windows では、は、Windows 用に設定された `Get-Help` 言語フォールバック標準に従って、モジュールディレクトリの言語固有のサブディレクトリにあるヘルプトピックを検索します。

PowerShell 3.0 以降で `Get-Help` は、コマンドレットまたは関数に対してコマンドを実行すると、モジュールの自動インポートがトリガーされます。 この `Get-Help` コマンドレットは、モジュール内のヘルプトピックの内容をすぐに表示します。

モジュールにヘルプトピックが含まれておらず、ユーザーのコンピューターのモジュール内のコマンドに関するヘルプトピックがない場合は、 `Get-Help` 自動生成されたヘルプを表示します。 自動生成されたヘルプには、コマンドの構文、パラメーター、および入力と出力の型が含まれていますが、説明は含まれていません。 自動生成されたヘルプには、コマンドレットを使用して、 `Update-Help` インターネットまたはファイル共有からコマンドのヘルプをダウンロードするように指示するテキストが含まれています。 また、コマンドレットの **online** パラメーターを使用して、 `Get-Help` ヘルプトピックのオンラインバージョンを取得することをお勧めします。

## <a name="supporting-updatable-help"></a>更新可能なヘルプのサポート

Powershell 3.0 およびそれ以降のバージョンの PowerShell を使用するユーザーは、モジュールの更新されたヘルプファイルをインターネットまたはローカルファイル共有からダウンロードしてインストールできます。 `Update-Help` `Save-Help` コマンドレットおよびコマンドレットは、ユーザーからの管理の詳細を非表示にします。 ユーザーはコマンドレットを実行し、コマンドレットを使用して、 `Update-Help` `Get-Help` PowerShell コマンドプロンプトでモジュールの最新のヘルプファイルを読み取ります。
ユーザーは、Windows または PowerShell を再起動する必要はありません。

ファイアウォールの背後にあるユーザーやインターネットにアクセスできないユーザーも、更新可能なヘルプを使用できます。
インターネットアクセスが可能な管理者は、コマンドレットを使用して、 `Save-Help` 最新のヘルプファイルをダウンロードしてファイル共有にインストールします。 次に、コマンドレットの **Path** パラメーターを使用して、 `Update-Help` ファイル共有から最新のヘルプファイルを取得します。

モジュールの作成者は、モジュールにヘルプファイルを含めたり、更新可能なヘルプを使用してヘルプファイルを更新したり、モジュールからヘルプファイルを除外したり、更新可能なヘルプを使用してインストールしたり更新したりできます。

更新可能なヘルプの詳細については、「 [更新可能なヘルプのサポート](./supporting-updatable-help.md)」を参照してください。

## <a name="supporting-online-help"></a>オンライン ヘルプのサポート

更新されたヘルプファイルをコンピューターにインストールできない、またはインストールしないユーザーは、多くの場合、オンラインバージョンのモジュールヘルプトピックに依存しています。 コマンドレットの **online** パラメーターは、 `Get-Help` 既定のインターネットブラウザーでユーザーのコマンドレットまたは高度な関数のヘルプトピックを開きます。

コマンドレットでは、 `Get-Help` コマンドレットまたは関数の "/" プロパティの値を使用して、ヘルプトピックのオンラインバージョンを検索します。

PowerShell 3.0 以降では、コマンドレットと関数のヘルプトピックのオンラインバージョンを検索するために、コマンドレットクラスまたはコマンドレット **バインド** 属性の **"/" プロパティ** を定義することができます。 属性の値は、コマンドレットまたは関数のすべて **のプロパティの** 値です。

詳細については、「 [オンラインヘルプのサポート](./supporting-online-help.md)」を参照してください。

## <a name="see-also"></a>参照

[PowerShell モジュールの記述](../module/writing-a-windows-powershell-module.md)

[更新可能なヘルプのサポート](./supporting-updatable-help.md)

[オンライン ヘルプのサポート](./supporting-online-help.md)
