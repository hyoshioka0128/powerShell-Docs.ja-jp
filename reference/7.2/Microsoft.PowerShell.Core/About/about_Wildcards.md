---
description: PowerShell でワイルドカード文字を使用する方法について説明します。
Locale: en-US
ms.date: 03/28/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_wildcards?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Wildcards
ms.openlocfilehash: b5f13fdbfbc24e19e5ad0b1cd6ecc1b99f68914f
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599718"
---
# <a name="about-wildcards"></a>ワイルドカードについて

## <a name="short-description"></a>概要

PowerShell でワイルドカード文字を使用する方法について説明します。

## <a name="long-description"></a>詳細説明

ワイルドカード文字は、1つまたは複数の文字を表します。 これらのコマンドを使用して、コマンドで単語パターンを作成できます。 たとえば、ディレクトリ内のファイル名拡張子を持つすべてのファイルを取得するには、次のように `C:\Techdocs` `.ppt` 入力します。

```powershell
Get-ChildItem C:\Techdocs\*.ppt
```

この場合、アスタリスク ( `*` ) ワイルドカード文字は、ファイル名拡張子の前に表示される任意の文字を表し `.ppt` ます。

PowerShell では、次のワイルドカード文字がサポートされています。

|ワイルドカード|説明               |例 |一致したもの        |一致なし|
|--------|--------------------------|--------|-------------|--------|
|\*      |0個以上の文字と一致します | ある\*  | aA、ag、Apple | バナナ |
|?       |その位置の1文字と一致します | ? n | 、in、on | 済み |
|\[ \]   |文字の範囲に一致 | \[a-l \] ook | 書籍、クック、ルック | 所要 |
|\[ \]   |特定の文字に一致 | \[bc \] ook | 書籍、クック | 用 |

同じ単語パターンに複数のワイルドカード文字を含めることができます。 たとえば、名前が **a** ~ **l** で始まるテキストファイルを検索するには、次のように入力します。

```powershell
Get-ChildItem C:\Techdocs\[a-l]*.txt
```

多くのコマンドレットでは、パラメーター値にワイルドカード文字を使用できます。 各コマンドレットのヘルプトピックでは、ワイルドカード文字を受け入れるパラメーターについて説明します。 ワイルドカード文字を受け入れるパラメーターの場合、その使用では大文字と小文字が区別されません。

コマンドやスクリプトブロックでワイルドカード文字を使用すると、プロパティ値を表す単語パターンを作成できます。 たとえば、次のコマンドは、 **ServiceType** プロパティ値に **Interactive** が含まれているサービスを取得します。

```powershell
Get-Service | Where-Object {$_.ServiceType -Like "*Interactive*"}
```

次の例では、 `If` ステートメントに、ワイルドカード文字を使用してプロパティ値を検索する条件が含まれています。 復元ポイントの **説明** に **PowerShell** が含まれている場合、コマンドにより、復元ポイントの "バックアップ **後の時間** " プロパティの値がログファイルに追加されます。

```powershell
$p = Get-ComputerRestorePoint
foreach ($point in $p) {
  if ($point.description -like "*PowerShell*") {
    Add-Content -Path C:\TechDocs\RestoreLog.txt "$($point.CreationTime)"
  }
}
```

## <a name="see-also"></a>関連項目

[about_Language_Keywords](about_Language_Keywords.md)

[about_If](about_If.md)

[about_Script_Blocks](about_Script_Blocks.md)

