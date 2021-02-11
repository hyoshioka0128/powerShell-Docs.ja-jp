---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: ユーザー設定の入力ボックスを作成する
description: この記事では、Windows PowerShell で .NET Framework のフォーム作成機能を使用して、カスタム入力ボックスを作成する方法を示します。
ms.openlocfilehash: b7786706d2461c329da429c1bd4971d7dc874d6d
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94390084"
---
# <a name="creating-a-custom-input-box"></a>ユーザー設定の入力ボックスを作成する

Windows PowerShell 3.0 以降のリリースで、Microsoft .NET Framework フォーム作成機能を使用してカスタムのグラフィカル入力ボックスをスクリプト化します。

## <a name="create-a-custom-graphical-input-box"></a>カスタムのグラフィカル入力ボックスの作成

以下を Windows PowerShell ISE にコピーしてから貼り付け、Windows PowerShell スクリプト (.ps1) として保存します。

```powershell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form
$form.Text = 'Data Entry Form'
$form.Size = New-Object System.Drawing.Size(300,200)
$form.StartPosition = 'CenterScreen'

$okButton = New-Object System.Windows.Forms.Button
$okButton.Location = New-Object System.Drawing.Point(75,120)
$okButton.Size = New-Object System.Drawing.Size(75,23)
$okButton.Text = 'OK'
$okButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $okButton
$form.Controls.Add($okButton)

$cancelButton = New-Object System.Windows.Forms.Button
$cancelButton.Location = New-Object System.Drawing.Point(150,120)
$cancelButton.Size = New-Object System.Drawing.Size(75,23)
$cancelButton.Text = 'Cancel'
$cancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $cancelButton
$form.Controls.Add($cancelButton)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please enter the information in the space below:'
$form.Controls.Add($label)

$textBox = New-Object System.Windows.Forms.TextBox
$textBox.Location = New-Object System.Drawing.Point(10,40)
$textBox.Size = New-Object System.Drawing.Size(260,20)
$form.Controls.Add($textBox)

$form.Topmost = $true

$form.Add_Shown({$textBox.Select()})
$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

このスクリプトは 2 つの .NET Framework クラス、つまり **System.Drawing** と **System.Windows.Forms** を最初に読み込みます。 次に、.NET Framework クラス **System.Windows.Forms.Form** の新しいインスタンスを開始します。これにより、コントロールの追加を開始する空白のフォームまたはウィンドウが作成されます。

```powershell
$form = New-Object System.Windows.Forms.Form
```

フォーム クラスのインスタンスを作成したら、このクラスの次の 3 つのプロパティに値を割り当てます。

- **Text**。 ウィンドウのタイトルになります。

- **Size**。 フォームのサイズをピクセル単位で表します。 前述のスクリプトにより、幅 300 ピクセル、高さ 200 ピクセルのフォームが作成されます。

- **StartingPosition**。 前述のスクリプトでは、この省略可能なプロパティは **CenterScreen** に設定されています。
  このプロパティを追加しない場合は、Windows によりフォームが開かれる場所が選択されます。 **StartingPosition** を **CenterScreen** に設定すると、フォームは読み込まれるたびに画面中央に自動的に表示されます。

```powershell
$form.Text = 'Data Entry Form'
$form.Size = New-Object System.Drawing.Size(300,200)
$form.StartPosition = 'CenterScreen'
```

次に、フォームに **[OK]** ボタンを作成します。 **[OK]** ボタンのサイズと動作を指定します。 この例では、ボタンの位置は、フォームの上端から 120 ピクセル、左端から 75 ピクセルです。 ボタンの高さは 23 ピクセル、ボタンの幅は 75 ピクセルです。 スクリプトは、ボタンの動作を決定するために定義済みの Windows フォームの型を使用します。

```powershell
$okButton = New-Object System.Windows.Forms.Button
$okButton.Location = New-Object System.Drawing.Point(75,120)
$okButton.Size = New-Object System.Drawing.Size(75,23)
$okButton.Text = 'OK'
$okButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

同様に、 **[キャンセル]** ボタンを作成します。 **[キャンセル]** ボタンの位置は、ウィンドウの最上部から 120 ピクセル、左端から 150 ピクセルです。

```powershell
$cancelButton = New-Object System.Windows.Forms.Button
$cancelButton.Location = New-Object System.Drawing.Point(150,120)
$cancelButton.Size = New-Object System.Drawing.Size(75,23)
$cancelButton.Text = 'Cancel'
$cancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $cancelButton
$form.Controls.Add($cancelButton)
```

次に、ユーザーに提供する情報を記述するラベルのテキストをウィンドウ上に用意します。

```powershell
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please enter the information in the space below:'
$form.Controls.Add($label)
```

ラベルのテキストに記述されている情報をユーザーが提供できるコントロール (この場合はテキスト ボックス) を追加します。 テキスト ボックスに加えて、他に多数の適用可能なコントロールがあります。その他のコントロールについては、「[System.Windows.Forms 名前空間](/dotnet/api/system.windows.forms)」をご覧ください。

```powershell
$textBox = New-Object System.Windows.Forms.TextBox
$textBox.Location = New-Object System.Drawing.Point(10,40)
$textBox.Size = New-Object System.Drawing.Size(260,20)
$form.Controls.Add($textBox)
```

**Topmost** プロパティを **$true** に設定すると、開いているその他のウィンドウとダイアログ ボックスの最上部にウィンドウが強制的に開きます。

```powershell
$form.Topmost = $true
```

続いて、次のコード行を追加してフォームをアクティブにするとともに、作成したテキスト ボックスにフォーカスを設定します。

```powershell
$form.Add_Shown({$textBox.Select()})
```

次のコード行を追加して、Windows でフォームを表示します。

```powershell
$result = $form.ShowDialog()
```

最後に、**If** ブロック内のコードは、ユーザーがテキスト ボックスにテキストを入力した後の Windows の動作を指示します。続いて **[OK]** ボタンをクリックするか、**Enter** キーを押します。

```powershell
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $textBox.Text
    $x
}
```

## <a name="see-also"></a>参照

- [GitHub: Dave Wyatt の WinFormsExampleUpdates](/previous-versions/windows/it-pro/windows-powershell-1.0/ff730941(v=technet.10))
- [Windows PowerShell Tip of the Week: ユーザー設定の入力ボックスを作成する](https://technet.microsoft.com/library/ff730941.aspx)
