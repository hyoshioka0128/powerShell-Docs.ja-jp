---
ms.date: 12/11/2018
title: 互換性のある PowerShell エディションまたはオペレーティング システムが含まれるパッケージ
description: この記事では、特定のプラットフォームやエディションとの互換性を条件にして PowerShell ギャラリーを検索する方法について説明します。
ms.openlocfilehash: 9806c09c85febfd74bb69adf3d294fb4f559ff23
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92661247"
---
# <a name="packages-with-compatible-powershell-editions-or-operating-systems"></a>互換性のある PowerShell エディションまたはオペレーティング システムが含まれるパッケージ

バージョン 5.1 以降、機能セットとプラットフォーム互換性が異なる別エディションの PowerShell が用意されています。

## <a name="searching-by-powershell-edition"></a>PowerShell エディションで検索する

PowerShell の 2 つのエディション:

- **Desktop Edition:** .NET Framework 上に構築され、Windows の完全フットプリント エディション (Server Core、Windows Desktop など) で実行される PowerShell のバージョンをターゲットとするスクリプトおよびモジュールと互換性があります。
- **Core Edition:** .NET Core 上に構築されており、Nano Server や Windows IoT などの Windows の縮小エディションで実行する PowerShell のバージョンを対象とするスクリプトおよびモジュールとの互換性を提供します。

### <a name="powershell-gallery-allows-you-to-filter-packages-compatible-for-specific-powershell-editions"></a>PowerShell ギャラリーでは、特定の PowerShell エディションに対して互換性のあるパッケージをフィルタリングできます

パッケージが指定された PSEditions と互換性がある場合、'PowerShell Editions' の一部としてパッケージの表示ページおよびパッケージ結果に表示されます。
PowerShell を使用して互換性のあるパッケージを検索することもできます。

![PSEditions での項目表示ページ](media/searching-by-compatibility/packagedisplaypagewithpseditions.PNG)

### <a name="search-for-packages-in-the-gallery-ui-that-work-on-powershell-core"></a>PowerShell Core で機能するギャラリー UI のパッケージを検索する

Tags:"PSEdition_Desktop" と Tags:"PSEdition_Core" を使用して PowerShell ギャラリー上のパッケージをフィルター処理します。

### <a name="use-tagspsedition_core-to-search-items-compatible-with-powershell-core-edition"></a>Tags:"PSEdition_Core" を使用して PowerShell Core エディションと互換性のある項目を検索します

![Core PSEdition と互換性のある項目の検索](media/searching-by-compatibility/searchresultswithpseditions.PNG)

### <a name="use-tagspsedition_desktop-to-search-items-compatible-with-powershell-desktop-edition"></a>Tags:"PSEdition_Desktop" を使用して PowerShell Desktop エディションと互換性のある項目を検索します

![Desktop PSEdition と互換性のある項目の検索](media/searching-by-compatibility/searchresultswithpseditionsdesktop.PNG)

### <a name="search-for-packages-to-find-compatible-editions-using-powershell"></a>PowerShell を使用し、パッケージの中から互換性のあるエディションを見つける

PowerShell エディションと OS のタグを指定してフィルタリングできます。 `Find-Package` コマンドレットを指定します。`-Tag` パラメーターを指定し、対象のエディション (と OS) を指定します。 例:

```powershell
# Find modules compatible with PowerShell Core:
Find-Module -Tag PSEdition_Core

# Find modules compatible with PowerShell Core on Linux:
Find-Module -Tag PSEdition_Core, Linux
```

## <a name="searching-by-operating-system"></a>オペレーティング システムで検索する

PowerShell Core は Windows、Linux、MacOS で利用できるため、ギャラリーのパッケージは、それらの OS の組み合わせに合わせて設計されている場合があります。 ギャラリー UI では、次の検索タグを使用し、オペレーティング システムのタグが付けられたパッケージを見つけます。

- タグ:"Windows"
- タグ:"Linux"
- タグ:"MacOS"

`Find-Module` (と PowerShellGet モジュールのその他のコマンドレット) でこれらのタグを指定できます。例:

```powershell
# Find Modules compatible with Windows
Find-Module -Tag Linux
```

## <a name="searching-for-multiple-compatibilities"></a>複数の互換性の検索

次の構文を使用し、互換性が複数あるパッケージを検索できます。

タグ:"Compatibility1" "Compatibility2"

たとえば、Windows コンピューターと Linux コンピューターの両方で実行される PowerShell Core Compatibility が与えられたパッケージを検索する場合、次の検索タグを使用します。

タグ:"PSEdition_Core" "Windows" "Linux"

PowerShell を使用して検索するには、`Find-Module` (と PowerShellGet モジュールのその他のコマンドレット) を使用できます。例:

```powershell
# Find scripts compatible with PowerShell Core, Windows, and Linux
Find-Script -Tag PSEdition_Core,Linux,Windows

# Find modules compatible with PowerSHellCore and MacOS
Find-Module -Tag PSEdition_Core,MacOS
```

## <a name="more-details-on-authoring-and-finding-the-packages-with-compatible-powershell-editions"></a>互換性のある PowerShell エディションが含まれるパッケージの作成と検索に関する詳細

- [PSEditions が含まれるモジュール](../../concepts/module-psedition-support.md)
- [PSEditions のスクリプト](../../concepts/script-psedition-support.md)
