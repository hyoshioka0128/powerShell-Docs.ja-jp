---
ms.date: 09/13/2016
ms.topic: reference
title: 書式設定データを読み込んでエクスポートする
description: 書式設定データを読み込んでエクスポートする
ms.openlocfilehash: 38857526801051bf6d31d300d5be1a3fd2c80391
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92666520"
---
# <a name="loading-and-exporting-formatting-data"></a>書式設定データを読み込んでエクスポートする

フォーマットファイルを作成したら、現在のセッションにファイルを読み込んで、セッションのフォーマットデータを更新する必要があります。 (Windows PowerShell によって提供される書式設定ファイルは、現在のセッションを開いたときに登録されたスナップインによって読み込まれます)。現在のセッションの形式データが更新されると、Windows PowerShell はそのデータを使用して、読み込まれた書式設定ファイルで定義されているビューに関連付けられている .NET オブジェクトを表示します。 現在のセッションに読み込むことができる書式設定ファイルの数に制限はありません。 書式設定データを更新するだけでなく、現在のセッションの形式データを書式設定ファイルにエクスポートすることもできます。

## <a name="loading-format-data"></a>フォーマットデータを読み込んでいます

書式設定ファイルは、次のメソッドを使用して現在のセッションに読み込むことができます。

- コマンドラインから現在のセッションに書式設定ファイルをインポートできます。 次の手順に従って、 [Update-FormatData](/powershell/module/Microsoft.PowerShell.Utility/Update-FormatData) コマンドレットを使用します。

- 書式設定ファイルを参照するモジュールマニフェストを作成できます。 モジュールを使用すると、配布用にフォーマットファイルをパッケージ化することができます。 マニフェストを作成するには [new-modulemanifest](/powershell/module/Microsoft.PowerShell.Core/New-ModuleManifest) コマンドレットを使用し、モジュールを現在のセッションに読み込むには、モジュールの [インポート](/powershell/module/Microsoft.PowerShell.Core/Import-Module) コマンドレットを使用します。 モジュールの詳細については、「 [Windows PowerShell モジュールの記述](../module/writing-a-windows-powershell-module.md)」を参照してください。

- 書式設定ファイルを参照するスナップインを作成できます。 [Add-pssnapin](/dotnet/api/System.Management.Automation.PSSnapIn.Formats)を使用して、書式設定ファイルを参照します。 モジュールをパッケージ化し、配布用に関連付けられているフォーマットファイルと型ファイルをパッケージ化することを強くお勧めします。 モジュールの詳細については、「 [Windows PowerShell モジュールの記述](../module/writing-a-windows-powershell-module.md)」を参照してください。

- プログラムによってコマンドを呼び出す場合は、コマンドが実行される実行空間の初期セッション状態に、書式設定ファイルエントリを追加できます。 書式設定ファイルの追加に使用される .NET 型の詳細については、Sessionstateformatentry を参照してください。 [Displayproperty = Fullname](/dotnet/api/System.Management.Automation.Runspaces.SessionStateFormatEntry) クラス。

フォーマットファイルが読み込まれると、コマンドラインでオブジェクトを表示するときに使用するビューを決定するために Windows PowerShell で使用される内部リストに追加されます。 書式設定ファイルは、リストの先頭に付加することも、リストの末尾に追加することもできます。 Windows PowerShell コアコマンドレットによって返されるオブジェクトの表示方法を変更する場合など、既存のビューが定義されているオブジェクトのビューを定義するフォーマットファイルを読み込む場合は、書式設定ファイルがこのリストに追加されている場所を把握することが重要です。 オブジェクトのビューのみを定義する書式設定ファイルを読み込む場合は、前述の方法のいずれかを使用できます。  オブジェクトの別のビューを定義する書式設定ファイルを読み込む場合は、 [更新プログラムの FormatData](/powershell/module/Microsoft.PowerShell.Utility/Update-FormatData) コマンドレットを使用して、ファイルをリストの先頭に付加する必要があります。

## <a name="storing-your-formatting-file"></a>書式設定ファイルの格納

フォーマットファイルがディスクに格納される場所に関する要件はありません。 ただし、次のフォルダーに格納することを強くお勧めします。 `user\documents\windowspowershell\`

#### <a name="loading-a-format-file-using-import-formatdata"></a>Import-FormatData を使用したフォーマットファイルの読み込み

1. フォーマットファイルをディスクに保存します。

2. 次のいずれかのコマンドを使用して、 [Update-FormatData](/powershell/module/Microsoft.PowerShell.Utility/Update-FormatData) コマンドレットを実行します。

   書式設定ファイルを一覧の先頭に追加するには、次のコマンドを使用します。 オブジェクトの表示方法を変更する場合は、このコマンドを使用します。

   ```powershell
   Update-FormatData -PrependPath PathToFormattingFile
   ```

   書式設定ファイルを一覧の末尾に追加するには、次のコマンドを使用します。

   ```powershell
   Update-FormatData -AppendPath PathToFormattingFile
   ```

   [更新プログラムの FormatData](/powershell/module/Microsoft.PowerShell.Utility/Update-FormatData)コマンドレットを使用してファイルを追加した後は、セッションが開いているときに一覧からファイルを削除することはできません。 一覧から書式設定ファイルを削除するには、セッションを閉じる必要があります。

## <a name="exporting-format-data"></a>フォーマットデータのエクスポート

ここにセクションの本文を挿入します。
