---
ms.date: 09/12/2016
ms.topic: reference
title: スクリプトにコメント ベースのヘルプを配置する
description: スクリプトにコメント ベースのヘルプを配置する
ms.openlocfilehash: b0d32b7ab063269085899a643b0c3a17da2073fc
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92645444"
---
# <a name="placing-comment-based-help-in-scripts"></a>スクリプトにコメント ベースのヘルプを配置する

このトピックでは、スクリプトにコメントベースのヘルプトピックを配置する方法について説明し `Get-Help` ます。このコマンドレットは、スクリプトに含まれる関数ではなく、スクリプトにコメントベースのヘルプトピックを関連付けます。

## <a name="where-to-place-comment-based-help-for-a-script"></a>スクリプトの Comment-Based ヘルプの配置場所

- スクリプトファイルの先頭にあります。

  スクリプトのヘルプは、コメントと空白行によってのみスクリプトの先頭に配置できます。

- スクリプトファイルの末尾にあります。

  スクリプト本体の最初の項目 (ヘルプの後) が関数宣言の場合、スクリプトヘルプの末尾と関数宣言の間には、少なくとも2つの空白行が必要です。 それ以外の場合、ヘルプは、スクリプトのヘルプではなく、関数のヘルプとして解釈されます。

## <a name="examples-of-help-placement-in-a-script"></a>スクリプト内のヘルプの配置例

次の例は、スクリプトのコメントベースのヘルプの各配置オプションを示しています。

### <a name="help-at-the-beginning-of-a-script"></a>スクリプトの先頭のヘルプ

次の例では、スクリプトの先頭にあるコメントに基づいています。

```powershell
<#
.Description
This script performs a series of network connection tests.
#>

param [string]$ComputerName
...
```

### <a name="help-at-the-end-of-a-script"></a>スクリプトの最後のヘルプ

 次の例では、スクリプトの最後にコメントを使用してコメントを示します。

```powershell
...
function Ping { Test-Connection -ComputerName $ComputerName }

<#
.Description
This script performs a series of network connection tests.
#>
```
