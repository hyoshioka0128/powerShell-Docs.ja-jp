---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: 複数選択のリスト ボックス
description: この記事では、Windows PowerShell で .NET Framework フォーム作成機能を使用して、複数選択のリスト ボックス コントロールを作成する方法を示します。
ms.openlocfilehash: 2724188695f054d1115b385987cda8a578c102de
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391529"
---
# <a name="multiple-selection-list-boxes"></a>複数選択のリスト ボックス

Windows PowerShell 3.0 と以降のリリースを使用すると、カスタム Windows フォームで複数選択のリスト ボックス コントロールを作成できます。

## <a name="create-list-box-controls-that-allow-multiple-selections"></a>複数選択ができるようにするリスト ボックス コントロールを作成します。

以下を Windows PowerShell ISE にコピーしてから貼り付け、Windows PowerShell スクリプト (.ps1) として保存します。

```powershell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form
$form.Text = 'Data Entry Form'
$form.Size = New-Object System.Drawing.Size(300,200)
$form.StartPosition = 'CenterScreen'

$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Point(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = 'OK'
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)

$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = 'Cancel'
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)

$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please make a selection from the list below:'
$form.Controls.Add($label)

$listBox = New-Object System.Windows.Forms.Listbox
$listBox.Location = New-Object System.Drawing.Point(10,40)
$listBox.Size = New-Object System.Drawing.Size(260,20)

$listBox.SelectionMode = 'MultiExtended'

[void] $listBox.Items.Add('Item 1')
[void] $listBox.Items.Add('Item 2')
[void] $listBox.Items.Add('Item 3')
[void] $listBox.Items.Add('Item 4')
[void] $listBox.Items.Add('Item 5')

$listBox.Height = 70
$form.Controls.Add($listBox)
$form.Topmost = $true

$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItems
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
$OKButton = New-Object System.Windows.Forms.Button
$OKButton.Location = New-Object System.Drawing.Size(75,120)
$OKButton.Size = New-Object System.Drawing.Size(75,23)
$OKButton.Text = 'OK'
$OKButton.DialogResult = [System.Windows.Forms.DialogResult]::OK
$form.AcceptButton = $OKButton
$form.Controls.Add($OKButton)
```

同様に、 **[キャンセル]** ボタンを作成します。 **[キャンセル]** ボタンの位置は、ウィンドウの最上部から 120 ピクセル、左端から 150 ピクセルです。

```powershell
$CancelButton = New-Object System.Windows.Forms.Button
$CancelButton.Location = New-Object System.Drawing.Point(150,120)
$CancelButton.Size = New-Object System.Drawing.Size(75,23)
$CancelButton.Text = 'Cancel'
$CancelButton.DialogResult = [System.Windows.Forms.DialogResult]::Cancel
$form.CancelButton = $CancelButton
$form.Controls.Add($CancelButton)
```

次に、ユーザーに提供する情報を記述するラベルのテキストをウィンドウ上に用意します。

```powershell
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please make a selection from the list below:'
$form.Controls.Add($label)
```

ラベルのテキストに記述されている情報をユーザーが提供できるコントロール (この場合はリスト ボックス) を追加します。 テキスト ボックスに加えて、他に多数の適用可能なコントロールがあります。その他のコントロールについては、「[System.Windows.Forms 名前空間](/dotnet/api/system.windows.forms)」をご覧ください。

```powershell
$listBox = New-Object System.Windows.Forms.Listbox
$listBox.Location = New-Object System.Drawing.Point(10,40)
$listBox.Size = New-Object System.Drawing.Size(260,20)
```

ユーザーが一覧から複数の値を選択できるよう指定する方法を次に示します。

```powershell
$listBox.SelectionMode = 'MultiExtended'
```

次のセクションでは、リスト ボックスでユーザーに対して表示する値を指定します。

```powershell
[void] $listBox.Items.Add('Item 1')
[void] $listBox.Items.Add('Item 2')
[void] $listBox.Items.Add('Item 3')
[void] $listBox.Items.Add('Item 4')
[void] $listBox.Items.Add('Item 5')
```

リスト ボックス コントロールの高さの最大値を指定します。

```powershell
$listBox.Height = 70
```

リスト ボックス コントロールをフォームに追加してから、フォームを開くときに他のウィンドウやダイアログ ボックスより前面に開くよう、Windows に指示します。

```powershell
$form.Controls.Add($listBox)
$form.Topmost = $true
```

次のコード行を追加して、Windows でフォームを表示します。

```powershell
$result = $form.ShowDialog()
```

最後に、**If** ブロック内のコードは、ユーザーがリスト ボックスから 1 つ以上のオプションを選んだ後のフォームの操作を Windows に指示します。その後、 **[OK]** ボタンをクリックするか、**Enter** キーを押します。

```powershell
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItems
    $x
}
```

## <a name="see-also"></a>参照

- [週末スクリプト作成者: PowerShell GUI の修正の例](https://devblogs.microsoft.com/scripting/weekend-scripter-fixing-powershell-gui-examples/)
- [GitHub: Dave Wyatt の WinFormsExampleUpdates](https://github.com/dlwyatt/WinFormsExampleUpdates)
- [Windows PowerShell Tip of the Week:オプションの複数選択リスト - その他](/previous-versions/windows/it-pro/windows-powershell-1.0/ff730950(v=technet.10))
