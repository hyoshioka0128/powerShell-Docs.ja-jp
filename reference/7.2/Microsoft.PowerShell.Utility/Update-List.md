---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 11/11/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/update-list?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Update-List
ms.openlocfilehash: ae473541770948dee1ce927ddee45bd806d8ff29
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602616"
---
# Update-List

## 概要
オブジェクトのコレクションを格納するプロパティ値の項目を追加または削除します。

## SYNTAX

### AddRemoveSet (既定)

```
Update-List [-Add <Object[]>] [-Remove <Object[]>] [-InputObject <PSObject>] [[-Property] <String>]
 [<CommonParameters>]
```

### ReplaceSet

```
Update-List -Replace <Object[]> [-InputObject <PSObject>] [[-Property] <String>] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Update-List` オブジェクトのプロパティ値の項目を追加、削除、または置換して、更新されたオブジェクトを返します。 このコマンドレットは、オブジェクトのコレクションを格納するプロパティ用に設計されています。

**Add** パラメーターと **remove** パラメーターは、個々の項目を追加し、コレクションから削除します。
**Replace** パラメーターは、コレクション全体を置き換えます。

コマンドでプロパティを指定しない場合、はオブジェクトを更新する代わりに、 `Update-List` 更新を説明するオブジェクトを返します。 コマンドレットなどのオブジェクトを変更するコマンドレットに、更新オブジェクトを送信でき `Set` ます。

このコマンドレットは、更新中のプロパティがを使用する **IList** インターフェイスをサポートしている場合にのみ機能 `Update-List` します。 また、 `Set` 更新プログラムを受け入れるすべてのコマンドレットは、 **IList** インターフェイスをサポートしている必要があります。

PowerShell でインストールされるコアコマンドレットは、このインターフェイスをサポートしていません。 コマンドレットがサポートしているかどうかを確認するには `Update-List` 、コマンドレットのヘルプトピックを参照してください。

このコマンドレットは、PowerShell 7 で再導入されました。

## 例

### 例 1: プロパティ値に項目を追加する

この例では、カードを **リスト** コレクションオブジェクトとして格納するカードのデッキを表すクラスを作成します。 **Newdeck ()** メソッドでは、を使用し `Update-List` てカードコレクションにカード値の完全なデッキを追加します。

```powershell
class Cards {

    [System.Collections.Generic.List[string]]$cards
    [string]$name

    Cards([string]$_name) {
        $this.name = $_name
        $this.cards = [System.Collections.Generic.List[string]]::new()
    }

    NewDeck() {
        $_suits = "`u{2663}","`u{2666}","`u{2665}","`u{2660}"
        $_values = 'A',2,3,4,5,6,7,8,9,10,'J','Q','K'
        $_deck = foreach ($s in $_suits){ foreach ($v in $_values){ "$v$s"} }
        $this | Update-List -Property cards -Add $_deck | Out-Null
    }

    Show() {
        Write-Host
        Write-Host $this.name ": " $this.cards[0..12]
        if ($this.cards.count -gt 13) {
            Write-Host (' ' * ($this.name.length+3)) $this.cards[13..25]
        }
        if ($this.cards.count -gt 26) {
            Write-Host (' ' * ($this.name.length+3)) $this.cards[26..38]
        }
        if ($this.cards.count -gt 39) {
            Write-Host (' ' * ($this.name.length+3)) $this.cards[39..51]
        }
    }

    Shuffle() { $this.cards = Get-Random -InputObject $this.cards -Count 52 }

    Sort() { $this.cards.Sort() }
}
```

> [!NOTE]
> `Update-List`コマンドレットは、更新されたオブジェクトをパイプラインに出力します。 出力をにパイプして、 `Out-Null` 不要な表示を抑制します。

### 例 2: コレクションプロパティの項目を追加および削除する

例1のコードを続けて、カードと2人のプレーヤーが保持するカードのデッキを表す **カード** クラスのインスタンスを作成します。 コマンドレットを使用して、 `Update-List` プレーヤーにカードを追加し、デッキからカードを削除します。

```powershell
$player1 = [Cards]::new('Player 1')
$player2 = [Cards]::new('Player 2')

$deck = [Cards]::new('Deck')
$deck.NewDeck()
$deck.Shuffle()
$deck.Show()

# Deal two hands
$player1 | Update-List -Property cards -Add $deck.cards[0,2,4,6,8] | Out-Null
$player2 | Update-List -Property cards -Add $deck.cards[1,3,5,7,9] | Out-Null
$deck | Update-List -Property cards -Remove $player1.cards | Out-Null
$deck | Update-List -Property cards -Remove $player2.cards | Out-Null

$player1.Show()
$player2.Show()
$deck.Show()
```

```Output
Deck :  4♦ 7♥ J♦ 5♣ A♣ 8♦ J♣ Q♥ 6♦ 3♦ 9♦ 6♣ 2♣
        K♥ 4♠ 10♥ 8♠ 10♦ 9♠ 6♠ K♦ 7♣ 3♣ Q♣ A♥ Q♠
        3♥ 5♥ 2♦ 5♠ J♥ J♠ 10♣ 4♥ Q♦ 10♠ 4♣ 2♠ 2♥
        6♥ 7♦ A♠ 5♦ 8♣ 9♥ K♠ 7♠ 3♠ 9♣ A♦ K♣ 8♥

Player 1 :  4♦ J♦ A♣ J♣ 6♦

Player 2 :  7♥ 5♣ 8♦ Q♥ 3♦

Deck :  9♦ 6♣ 2♣ K♥ 4♠ 10♥ 8♠ 10♦ 9♠ 6♠ K♦ 7♣ 3♣
        Q♣ A♥ Q♠ 3♥ 5♥ 2♦ 5♠ J♥ J♠ 10♣ 4♥ Q♦ 10♠
        4♣ 2♠ 2♥ 6♥ 7♦ A♠ 5♦ 8♣ 9♥ K♠ 7♠ 3♠ 9♣
        A♦ K♣ 8♥
```

出力には、カードがプレーヤーに向けられる前のデッキの状態が表示されます。 各プレーヤーがデッキから5枚のカードを受け取ったことを確認できます。 最終的な出力は、カードをプレーヤーに処理した後のデッキの状態を示しています。 `Update-List` は、デッキからカードを選択してプレイヤーのコレクションに追加するために使用されました。 次に、を使用して、プレイヤーのカードがデッキから削除されました `Update-List` 。

### 例 3: 1 つのコマンドで項目を追加および削除する

`Update-List` では、1つのコマンドで **Add** パラメーターと **Remove** パラメーターを使用できます。 この例では、プレーヤー1がとを `4♦` 破棄 `6♦` し、2つの新しいカードを取得します。

```powershell
# Player 1 wants two new cards - remove 2 cards & add 2 cards
$player1 | Update-List -Property cards -Remove $player1.cards[0,4] -Add $deck.cards[0..1] | Out-Null
$player1.Show()

# remove dealt cards from deck
$deck | Update-List -Property cards -Remove $deck.cards[0..1] | Out-Null
$deck.Show()
```

```Output
Player 1 :  J♦ A♣ J♣ 9♦ 6♣

Deck :  2♣ K♥ 4♠ 10♥ 8♠ 10♦ 9♠ 6♠ K♦ 7♣ 3♣ Q♣ A♥
        Q♠ 3♥ 5♥ 2♦ 5♠ J♥ J♠ 10♣ 4♥ Q♦ 10♠ 4♣ 2♠
        2♥ 6♥ 7♦ A♠ 5♦ 8♣ 9♥ K♠ 7♠ 3♠ 9♣ A♦ K♣
        8♥
```

## PARAMETERS

### -Add

コレクションに追加するプロパティ値を指定します。 コレクションに表示する順序で値を入力します。

```yaml
Type: System.Object[]
Parameter Sets: AddRemoveSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -InputObject

更新するオブジェクトを指定します。 パイプを使用して、更新するオブジェクトをにパイプすることもでき `Update-List` ます。

```yaml
Type: System.Management.Automation.PSObject
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -プロパティ

更新するコレクションを含むプロパティを指定します。 このパラメーターを省略すると、はオブジェクトを変更する代わりに、 `Update-List` 変更を表すオブジェクトを返します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Remove

コレクションから削除するプロパティ値を指定します。

```yaml
Type: System.Object[]
Parameter Sets: AddRemoveSet
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Replace

新しいコレクションを指定します。 このパラメーターは、元のコレクション内のすべての項目を、このパラメーターで指定された項目で置き換えます。

```yaml
Type: System.Object[]
Parameter Sets: ReplaceSet
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### システム管理. PSObject

パイプを使用して、更新するオブジェクトをにパイプすることができ `Update-List` ます。

## 出力

### オブジェクトまたは PSListModifier。

`Update-List` 更新されたオブジェクトを返すか、更新アクションを表すオブジェクトを返します。

## 注

## 関連リンク

[Format-List](Format-List.md)

[Select-Object](Select-Object.md)

