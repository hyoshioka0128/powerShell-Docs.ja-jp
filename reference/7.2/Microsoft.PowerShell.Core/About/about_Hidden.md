---
description: 既定の Get-Member 結果からクラスメンバーを非表示にする Hidden キーワードについて説明します。
Locale: en-US
ms.date: 01/04/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_hidden?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Hidden
ms.openlocfilehash: 8340a955b7cfa540baccde0da233d6d9cf0c5552
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605436"
---
# <a name="about_hidden"></a>about_Hidden

## <a name="short-description"></a>概要
既定の Get-Member 結果からクラスメンバーを非表示にする Hidden キーワードについて説明します。

## <a name="long-description"></a>詳細説明

スクリプトで Hidden キーワードを使用すると、既定でクラスのメンバーが非表示になります。 Hidden キーワードを使用すると、プロパティ、メソッド (コンストラクター、イベント、エイリアスプロパティ、およびその他のメンバーの種類 (静的メンバーを含む)、Get-Member コマンドレットの既定の結果、IntelliSense とタブ補完の結果から、非表示にすることができます。 Hidden キーワードを使用して非表示にしたメンバーを表示するには、Get-Member コマンドに-Force パラメーターを追加します。

非表示のメンバーは、非表示のメンバーを定義するクラスで完了が発生しない限り、タブ補完や IntelliSense を使用して表示されることはありません。

新しい属性 System.management.automation.hiddenattribute が追加され、C \# コードが PowerShell 内で同じセマンティクスを持つことができるようになりました。

Hidden キーワードは、クラス内のプロパティとメソッドを作成する場合に便利です。このクラスは、クラスの他のユーザーに対しては必ずしも必要ではなく、すぐに編集することもできます。

Hidden キーワードは、クラスのメンバーを表示または変更する方法には影響しません。 PowerShell のすべての言語キーワードと同様に、Hidden では大文字と小文字が区別されず、非表示のメンバーもパブリックになります。

Hidden はカスタムクラスと共に PowerShell 5.0 で導入されました。

## <a name="example"></a>例

次の例では、クラス定義で Hidden キーワードを使用する方法を示します。 Car クラスメソッドドライブには、表示または変更する必要のないプロパティがあります (車のクラスでそのドライブが呼び出された回数、クラスのユーザーにとって重要ではないメトリック)。たとえば、車を購入するときに、車が撮影されたドライブの数について販売者に依頼することはできません。

クラスのユーザーがこのプロパティを変更する必要はほとんどないため、Hidden キーワードを使用して、Get-Member およびオートコンプリートの結果からプロパティを非表示にすることができます。

Hidden キーワードを、プロパティとそのデータ型と同じステートメント行に入力して追加します。 キーワードは、この行の任意の順序で指定できますが、Hidden キーワードを使用してステートメントを開始すると、隠ぺいされたすべてのメンバーを後で簡単に識別できるようになります。

```powershell
class Car
{
   # Properties
   [String] $Color
   [String] $ModelYear
   [int] $Distance

   # Method
   [int] Drive ([int]$miles)
   {
      $this.Distance += $miles
      $this.rides++
      return $this.Distance
   }

   # Hidden property of the Drive method
    hidden [int] $rides = 0
}
```

次に、Car クラスの新しいインスタンスを作成し、変数 testcar に保存し \$ ます。

```powershell
$TestCar = [Car]::new()
```

新しいインスタンスを作成したら、パイプを使用して $TestCar 変数の内容を取得します。 Get-Member コマンドの結果に示されているメンバーの中に、このプロパティが含まれていないことを確認します。

```output
PS C:\Windows\system32> $TestCar | Get-Member

   TypeName: Car

Name        MemberType Definition
----        ---------- ----------
Drive       Method     int Drive(int miles)
Equals      Method     bool Equals(System.Object obj)
GetHashCode Method     int GetHashCode()
GetType     Method     type GetType()
ToString    Method     string ToString()
Color       Property   string Color {get;set;}
Distance    Property   int Distance {get;set;}
ModelYear   Property   string ModelYear {get;set;}

```

ここで Get-Member をもう一度実行してみてください。今回は、-Force パラメーターを追加してください。
結果には、既定で非表示になっている他のメンバーのように、非表示の [実行] プロパティが含まれていることに注意してください。

```output
PS C:\Windows\system32> $TestCar | Get-Member -Force

   TypeName: Car

Name          MemberType   Definition
----          ----------   ----------
pstypenames   CodeProperty System.Collections.ObjectModel.Collection`1
psadapted     MemberSet    psadapted {Color, ModelYear, Distance,
psbase        MemberSet    psbase {Color, ModelYear, Distance,...
psextended    MemberSet    psextended {}
psobject      MemberSet    psobject {BaseObject, Members,...
Drive         Method       int Drive(int miles)
Equals        Method       bool Equals(System.Object obj)
GetHashCode   Method       int GetHashCode()
GetType       Method       type GetType()
get_Color     Method       string get_Color()
get_Distance  Method       int get_Distance()
get_ModelYear Method       string get_ModelYear()
get_rides     Method       int get_rides()
set_Color     Method       void set_Color(string )
set_Distance  Method       void set_Distance(int )
set_ModelYear Method       void set_ModelYear(string )
set_rides     Method       void set_rides(int )
ToString      Method       string ToString()
Color         Property     string Color {get;set;}
Distance      Property     int Distance {get;set;}
ModelYear     Property     string ModelYear {get;set;}
rides         Property     int rides {get;set;}

```

## <a name="see-also"></a>関連項目

[about_Classes](about_Classes.md)

[about_Language_Keywords](about_Language_Keywords.md)

[about_Wildcards](about_Wildcards.md)

[Get-Member](xref:Microsoft.PowerShell.Utility.Get-Member)

