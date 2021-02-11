---
ms.date: 09/13/2016
ms.topic: reference
title: 更新可能なヘルプの作成-ステップバイステップ
description: 更新可能なヘルプの作成-ステップバイステップ
ms.openlocfilehash: c4aecdb801cac16c9cb07853317835fd87e6a0a8
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92658819"
---
# <a name="updatable-help-authoring-step-by-step"></a>更新可能なヘルプの作成: ステップバイステップ

このドキュメントでは、更新可能なヘルプを作成するプロセスの手順を示します。

## <a name="authoring-updatable-help-step-by-step"></a>更新可能なヘルプの作成: ステップバイステップ

更新可能なヘルプはエンドユーザー向けに設計されていますが、モジュールの作成者やヘルプライターにも大きな利点があります。これには、コンテンツの追加、エラーの修正、複数の UI カルチャでの配信、およびモジュールが出荷された後のユーザーのコメントと要求への応答が含まれます。 このトピックでは、ヘルプファイルをパッケージ化してアップロード[し、ユーザーが update-help コマンド](/powershell/module/Microsoft.PowerShell.Core/Update-Help)[レットを](/powershell/module/Microsoft.PowerShell.Core/Save-Help)使用してダウンロードおよびインストールできるようにする方法について説明します。

次の手順では、更新可能なヘルプをサポートするプロセスの概要を説明します。

### <a name="step-1-find-an-internet-site-for-your-help-files"></a>手順 1: ヘルプファイルのインターネットサイトを検索する

更新可能なヘルプを作成するための最初の手順は、モジュールのヘルプファイルのインターネット上の場所を見つけることです。 実際には、2つの異なる場所を使用できます。 モジュールのヘルプ情報ファイル (後述の HelpInfo XML) を1つのインターネット上の場所に保存し、ヘルプコンテンツの CAB ファイルを別のインターネット上の場所に保存することができます。 モジュールのすべてのヘルプコンテンツ CAB ファイルは、同じ場所にある必要があります。 異なるモジュールのヘルプコンテンツ CAB ファイルを同じ場所に配置することができます。

### <a name="step-2-add-a-helpinfouri-key-to-your-module-manifest"></a>手順 2: HelpInfoURI キーをモジュールマニフェストに追加する

**Helpinfouri** キーをモジュールマニフェストに追加します。 キーの値は、モジュールの HelpInfo XML 情報ファイルの場所の Uniform Resource Identifier (URI) です。 セキュリティのため、アドレスは "http" または "https" で始まる必要があります。 URI はインターネットの場所を指定する必要がありますが、HelpInfo XML ファイル名を含めることはできません。

次に例を示します。

```powershell

@{
RootModule = TestModule.psm1
ModuleVersion = '2.0'
HelpInfoURI = 'https://go.microsoft.com/fwlink/?LinkID=0123'
}
```

### <a name="step-3-create-a-helpinfo-xml-file"></a>手順 3: HelpInfo XML ファイルを作成する

HelpInfo XML 情報ファイルには、ヘルプファイルのインターネット上の場所の URI と、サポートされている各 UI カルチャのモジュールの最新のヘルプファイルのバージョン番号が含まれています。 すべての Windows PowerShell モジュールには、HelpInfo XML ファイルが1つあります。 ヘルプファイルを更新するときに、HelpInfo XML ファイルを編集または置換します。別のユーザーを追加することはできません。 詳細については、「 [How To Create a HELPINFO XML File](./how-to-create-a-helpinfo-xml-file.md)」を参照してください。

### <a name="step-4-create-cab-files"></a>手順 4: CAB ファイルを作成する

キャビネット () ファイルを作成するツール (など) を使用して `.cab` `MakeCab.exe` 、モジュールのヘルプファイルを含む CAB ファイルを作成します。 サポートされている各 UI カルチャで、ヘルプファイル用の個別の CAB ファイルを作成します。 詳細については、「 [更新可能なヘルプ CAB ファイルを準備する方法](./how-to-prepare-updatable-help-cab-files.md)」を参照してください。

### <a name="step-5-upload-your-files"></a>手順 5: ファイルをアップロードする

新しいまたは更新されたヘルプファイルを発行するには、HelpInfo XML ファイルの **Helpcontenturi** 要素によって指定されたインターネット上の場所に CAB ファイルをアップロードします。 次に、モジュールマニフェストの **Helpinfouri** キーの値によって指定されたインターネット上の場所に、HelpInfo XML ファイルをアップロードします。
