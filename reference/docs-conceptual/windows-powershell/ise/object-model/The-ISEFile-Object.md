---
ms.date: 12/31/2019
title: ISEFile オブジェクト
description: ISEFile オブジェクトは、Windows PowerShell ISE 内のファイルを表します。
ms.openlocfilehash: b5ea70219787f254fe85d728518cbc4746c00250
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391495"
---
# <a name="the-isefile-object"></a>ISEFile オブジェクト

**ISEFile** オブジェクトは、Windows PowerShell Integrated Scripting Environment (ISE) のファイルを表します。 これは **Microsoft.PowerShell.Host.ISE.ISEFile** クラスのインスタンスです。 このトピックでは、そのメンバー メソッドとメンバー プロパティについて説明します。 `$psISE.CurrentFile` と、PowerShell タブのファイル コレクション内のファイルは、\*\***Microsoft.PowerShell.Host.ISE.ISEFile** クラスのすべてのインスタンスです。

## <a name="methods"></a>メソッド

### <a name="save-saveencoding-"></a>Save\( \[saveEncoding\] \)

Windows PowerShell ISE 2.0 以降でサポートされています。

ファイルをディスクに保存します。

`[saveEncoding]` - 省略可能な [System.Text.Encoding](/dotnet/api/system.text.encoding)。保存したファイルで使用する省略可能な文字エンコード パラメーター。 既定値は **UTF8** です。

### <a name="exceptions"></a>例外

- **System.IO.IOException**: ファイルを保存できませんでした。

```powershell
# Save the file using the default encoding (UTF8)
$psISE.CurrentFile.Save()

# Save the file as ASCII.
$psISE.CurrentFile.Save([System.Text.Encoding]::ASCII)

# Gets the current encoding.
$myfile = $psISE.CurrentFile
$myfile.Encoding
```

### <a name="saveasfilename-saveencoding"></a>SaveAs\(filename, \[saveEncoding\]\)

Windows PowerShell ISE 2.0 以降でサポートされています。

指定したファイル名およびエンコードでファイルを保存します。

**filename** - ファイルを保存するために使用する名前の文字列。

`[saveEncoding]` - 省略可能な [System.Text.Encoding](/dotnet/api/system.text.encoding)。保存したファイルで使用する省略可能な文字エンコード パラメーター。 既定値は **UTF8** です。

### <a name="exceptions"></a>例外

- **System.ArgumentNullException**: **filename** パラメーターが null です。
- **System.ArgumentException**: **filename** パラメータが空です。
- **System.IO.IOException**: ファイルを保存できませんでした。

```powershell
# Save the file with a full path and name.
$fullpath = "c:\temp\newname.txt"
$psISE.CurrentFile.SaveAs($fullPath)
# Save the file with a full path and name and explicitly as UTF8.
$psISE.CurrentFile.SaveAs($fullPath, [System.Text.Encoding]::UTF8)
```

## <a name="properties"></a>Properties

### <a name="displayname"></a>DisplayName

Windows PowerShell ISE 2.0 以降でサポートされています。

このファイルの表示名が含まれている文字列を取得する読み取り専用のプロパティ。 名前は、エディターの上部の **[ファイル]** タブに表示されます。 名前の末尾のアスタリスク (`(*)`) は、保存されていない変更がファイルに含まれていることを示します。

```powershell
# Shows the display name of the file.
$psISE.CurrentFile.DisplayName
```

### <a name="editor"></a>エディター

Windows PowerShell ISE 2.0 以降でサポートされています。

指定したファイルで使用される[エディター オブジェクト](The-ISEEditor-Object.md)を取得する読み取り専用のプロパティ。

```powershell
# Gets the editor and the text.
$psISE.CurrentFile.Editor.Text
```

### <a name="encoding"></a>エンコード

Windows PowerShell ISE 2.0 以降でサポートされています。

元のファイル エンコードを取得する読み取り専用のプロパティ。 これは **System.Text.Encoding** オブジェクトです。

```powershell
# Shows the encoding for the file.
$psISE.CurrentFile.Encoding
```

### <a name="fullpath"></a>FullPath

Windows PowerShell ISE 2.0 以降でサポートされています。

開いているファイルの完全パスを指定する文字列を取得する読み取り専用プロパティ。

```powershell
# Shows the full path for the file.
$psISE.CurrentFile.FullPath
```

### <a name="issaved"></a>IsSaved

Windows PowerShell ISE 2.0 以降でサポートされています。

ファイルが最後に変更された後にファイルが保存されている場合に `$true` を返す読み取り専用のブール型プロパティ。

```powershell
# Determines whether the file has been saved since it was last modified.
$myfile = $psISE.CurrentFile
$myfile.IsSaved
```

### <a name="isuntitled"></a>IsUntitled

Windows PowerShell ISE 2.0 以降でサポートされています。

ファイルにタイトルが指定されたことがない場合に `$true` を返す読み取り専用のプロパティ。

```powershell
# Determines whether the file has never been given a title.
$psISE.CurrentFile.IsUntitled
$psISE.CurrentFile.SaveAs("temp.txt")
$psISE.CurrentFile.IsUntitled
```

## <a name="see-also"></a>参照

- [The ISEFileCollectionObject](The-ISEFileCollection-Object.md)
- [Windows PowerShell ISE スクリプト オブジェクト モデルの目的](Purpose-of-the-Windows-PowerShell-ISE-Scripting-Object-Model.md)
- [ISE オブジェクト モデルの階層](The-ISE-Object-Model-Hierarchy.md)
