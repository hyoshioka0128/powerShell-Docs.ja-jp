---
description: PowerShell で特別な意味を持つため、識別子として使用できない予約語の一覧を示します。
Locale: en-US
ms.date: 07/23/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_reserved_words?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Reserved_Words
ms.openlocfilehash: 95911e328a50c173235f47a7610393124781555d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603039"
---
# <a name="about-reserved-words"></a>予約語について

## <a name="short-description"></a>概要
PowerShell で特別な意味を持つため、識別子として使用できない予約語の一覧を示します。

## <a name="long-description"></a>詳細説明

PowerShell では、特別な意味を持つ特定の単語があります。 これらの単語が引用符で囲まれていない場合、PowerShell は文字列として扱うのではなく、特別な意味を適用しようとします。 これらの単語をコマンドまたはスクリプトで特別な意味を呼び出さずにパラメーター引数として使用するには、予約語を引用符で囲みます。

PowerShell の予約語は次のとおりです。

```
assembly         exit            process
base             filter          public
begin            finally         return
break            for             sequence
catch            foreach         static
class            from (*)        switch
command          function        throw
configuration    hidden          trap
continue         if              try
data             in              type
define (*)       inlinescript    until
do               interface       using
dynamicparam     module          var (*)
else             namespace       while
elseif           parallel        workflow
end              param
enum             private

(*) These keywords are reserved for future use.
```

、、、およびなどのいくつかの言語キーワードに `Foreach` `If` は、独自の `For` `While` ヘルプ記事があります。 これを表示するには、「」 `Get-Help about_` と入力し、キーワードを追加します。 たとえば、ステートメントに関する情報を取得するには、次のように `Foreach` 入力します。

```powershell
Get-Help about_ForEach
```

ステートメントまたはステートメントの構文の詳細について `Filter` は `Return` 、次のように入力してください。

```powershell
Get-Help about_Functions
```

他の予約語の詳細については、次のように入力してください。

```powershell
Get-Help <Reserved_Word>
```

> [!NOTE]
> すべての予約語に独自のヘルプ記事があるわけではありません。 に `Get-Help` よって記事が返されない場合は、次のコマンドを使用して、予約語について説明する記事を検索できます。
>
> ```powershell
> Get-Help <Reserved_Word> -Category:HelpFile
> ```

## <a name="see-also"></a>関連項目

- [about_Command_Syntax](about_Command_Syntax.md)
- [about_Language_Keywords](about_Language_Keywords.md)
- [about_Parsing](about_Parsing.md)
- [about_Quoting_Rules](about_Quoting_Rules.md)
- [about_Script_Blocks](about_Script_Blocks.md)
- [about_Special_Characters](about_Special_Characters.md)
