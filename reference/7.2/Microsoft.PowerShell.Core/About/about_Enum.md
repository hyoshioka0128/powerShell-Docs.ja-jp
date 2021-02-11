---
description: '`enum`ステートメントは、列挙型を宣言するために使用されます。 列挙体は、列挙子リストと呼ばれる一連の名前付きラベルで構成される別個の型です。'
Locale: en-US
ms.date: 11/27/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_enum?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Enum
ms.openlocfilehash: 5572a717bbf9b546a30b227b3baf215196c4145d
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601009"
---
# <a name="about-enum"></a>列挙型について

## <a name="short-description"></a>概要
`enum`ステートメントは、列挙型を宣言するために使用されます。 列挙体は、列挙子リストと呼ばれる一連の名前付きラベルで構成される別個の型です。

## <a name="long-description"></a>詳細説明

`enum`ステートメントを使用すると、厳密に型指定されたラベルのセットを作成できます。 この列挙体は、解析やスペルミスの確認を必要とせずに、コード内で使用できます。

列挙体は、内部的には0の開始値を持つ整数として表現されます。 リストの最初のラベルには、0という値が割り当てられます。 残りのラベルは、連続した数値で割り当てられます。

定義では、ラベルに任意の整数値を指定できます。 値が割り当てられていないラベルは、次の整数値を取得します。

## <a name="syntax-basic"></a>構文 (基本)

```syntax
enum <enum-name> {
    <label> [= <int-value>]
    ...
}
```

## <a name="usage-example"></a>使用例

次の例は、メディアファイルとして表示できるオブジェクトの列挙を示しています。 定義により、、、の基になる値に明示的な値が割り当てられ `music` `picture` `video` ます。 明示的な代入の直後にあるラベルは、次の整数値を取得します。 シノニムは、別のラベルに同じ値を割り当てることによって作成できます。、、 `ogg` `oga` `mogg` 、または `jpg` 、、 `jpeg` または `mpg` `mpeg` の構築された値を確認します。

```powershell
enum MediaTypes {
    unknown
    music = 10
    mp3
    aac
    ogg = 15
    oga = 15
    mogg = 15
    picture = 20
    jpg
    jpeg = 21
    png
    video = 40
    mpg
    mpeg = 41
    avi
    m4v
}
```

メソッドは、 `GetEnumNames()` 列挙体のラベルの一覧を返します。

```powershell
[MediaTypes].GetEnumNames()
unknown
music
mp3
aac
ogg
oga
mogg
picture
jpg
jpeg
png
video
mpg
mpeg
avi
m4v
```

メソッドは、 `GetEnumValues()` 列挙体の値のリストを返します。

```powershell
[MediaTypes].GetEnumValues()
unknown
music
mp3
aac
oga
oga
oga
picture
jpeg
jpeg
png
video
mpeg
mpeg
avi
m4v
```

**注**: getenumnames () と Getenumnames () は、同じ結果を返すように見えます。
ただし、内部的には、PowerShell は値をラベルに変更します。 リストを注意深く読んで `oga` `mogg` ください。とは ' get Names ' の結果の下に記載されてい `jpg` ますが、、、およびに対する "値の取得" のような出力の下にはありません `jpeg` `mpg` `mpeg` 。

```powershell
[MediaTypes].GetEnumName(15)
oga

[MediaTypes].GetEnumNames() | ForEach-Object {
  "{0,-10} {1}" -f $_,[int]([MediaTypes]::$_)
}
unknown    0
music      10
mp3        11
aac        12
ogg        15
oga        15
mogg       15
picture    20
jpg        21
jpeg       21
png        22
video      40
mpg        41
mpeg       41
avi        42
m4v        43
```

## <a name="enumerations-as-flags"></a>フラグとしての列挙型

列挙は、ビットフラグのコレクションとして定義できます。
ここで、列挙は、有効になっている1つ以上のフラグを表します。

フラグが適切に機能するように列挙するには、各ラベルに2つの値の累乗が必要です。

## <a name="syntax-flags"></a>構文 (flags)

```syntax
[Flags()] enum <enum-name> {
    <label 0> [= 1]
    <label 1> [= 2]
    <label 2> [= 4]
    <label 3> [= 8]
    ...
}
```

## <a name="flags-usage-example"></a>Flags の使用例

次の例では、 *Fileattributes* 列挙が作成されます。

```powershell
[Flags()] enum FileAttributes {
    Archive = 1
    Compressed = 2
    Device = 4
    Directory = 8
    Encrypted = 16
    Hidden = 32
}

[FileAttributes]$file1 = [FileAttributes]::Archive
[FileAttributes]$file1 +=[FileAttributes]::Compressed
[FileAttributes]$file1 +=  [FileAttributes]::Device
"file1 attributes are: $file1"

[FileAttributes]$file2 = [FileAttributes]28 ## => 16 + 8 + 4
"file2 attributes are: $file2"
```

```output
file1 attributes are: Archive, Compressed, Device
file2 attributes are: Device, Directory, Encrypted
```

特定のが設定されていることをテストするには、バイナリ比較演算子を使用し `-band` ます。 この例では、の値で **デバイス** と **アーカイブ** 属性をテストし `$file2` ます。

```
PS > ($file2 -band [FileAttributes]::Device) -eq [FileAttributes]::Device
True

PS > ($file2 -band [FileAttributes]::Archive) -eq [FileAttributes]::Archive
False
```

