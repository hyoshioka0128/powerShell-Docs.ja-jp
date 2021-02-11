---
description: オブジェクトのコレクションに対するスクリプトフィルターの、より簡単で自然言語の方法について説明します。
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_simplified_syntax?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Simplified_Syntax
ms.openlocfilehash: dbd30d566414f784e7d5eca04af716c2bf1642b9
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602219"
---
# <a name="about_simplified_syntax"></a>about_Simplified_Syntax

## <a name="short-description"></a>概要
オブジェクトのコレクションに対するスクリプトフィルターの、より簡単で自然言語の方法について説明します。

## <a name="long-description"></a>詳細説明

Windows PowerShell 3.0 で導入された簡略化された構文を使用すると、スクリプトブロックを使用せずにいくつかのフィルターコマンドを作成できます。 簡略化された構文は自然言語に似ています。これは主に、コマンドと、 `Where-Object` `ForEach-Object` それに対応するエイリアスおよびにパイプされるオブジェクトのコレクションに役立ち `where` `foreach` ます。

スクリプトブロック内の自動変数を参照せずに、コレクションのメンバー (通常は配列) に対してメソッドを使用でき `$_` ます。

次の2つの呼び出しについて考えてみます。

### <a name="standard-syntax"></a>標準構文

```powershell
dir Cert:\LocalMachine\Root | where { $_.FriendlyName -eq 'Verisign' }
dir Cert:\ -Recurse | foreach { $_.GetKeyAlgorithm() }
```

### <a name="simplified-syntax"></a>簡略化された構文

簡略化された構文では、コレクション内のオブジェクトのメンバーに対して機能する比較演算子は、パラメーターとして扱われます。 スクリプトブロック内の自動変数を参照せずに、コレクション内のオブジェクトに対してメソッドを呼び出すことができ `$_` ます。
次の2つの呼び出しを、前の例の呼び出しと比較します。

```powershell
dir Cert:\LocalMachine\Root | where FriendlyName -eq 'Verisign'
dir Cert:\ -Recurse | foreach GetKeyAlgorithm
```

どちらの構文も機能しますが、簡略化された構文は、スクリプトブロック内の自動変数を参照せずに結果を返し `$_` ます。
メソッド名 `GetKeyAlgorithm` は、のパラメーターとして扱われ `ForEach-Object` ます。
2番目のコマンドは、指定された引数が適用されなかった項目の結果を返さないようにするため、エラーが発生することなく、同じ結果を返します。

この例では、 `Process` プロパティ `Description` がメンバー名パラメーターとしてコマンドに渡され `ForEach-Object` ます。 結果として、アクティブなプロセスの説明が表示されます。

```powershell
Get-Process | foreach Description
```

この例では、 `DirectoryInfo` メソッド `GetFiles` はコマンドのメンバー名パラメーターとして渡され `ForEach-Object` ます。  
メソッドは、検索パターンパラメーターを使用して呼び出され `.*` ます。  
結果は、 `FileInfo` ユーザーのホームディレクトリにあるすべての Unix スタイルの隠しファイルのレコードになります。 

```powershell
Get-ChildItem /home -Directory | foreach GetFiles .*
```

## <a name="see-also"></a>関連項目

- [about_Comparison_Operators](about_Comparison_Operators.md)
- [about_Foreach](about_Foreach.md)
- [Where-Object](xref:Microsoft.PowerShell.Core.Where-Object)
- [Foreach-オブジェクト](xref:Microsoft.PowerShell.Core.ForEach-Object)

