---
ms.date: 06/05/2017
keywords: powershell,コマンドレット
title: リスト ボックスから項目を選択する
description: この記事では、Windows PowerShell で .NET Framework のフォーム作成機能を使用して、リスト ボックス コントロールを作成する方法を示します。
ms.openlocfilehash: d6f881f0b92f294da105ae7df5e25e8c20ce5094
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94391240"
---
# <a name="selecting-items-from-a-list-box"></a>リスト ボックスから項目を選択する

Windows PowerShell 3.0 以降のリリースを使用すると、ユーザーがリスト ボックス コントロールから項目を選択できるダイアログ ボックスを作成できます。

## <a name="create-a-list-box-control-and-select-items-from-it"></a>リスト ボックス コントロールを作成してそこから項目を選択する

以下を Windows PowerShell ISE にコピーしてから貼り付け、Windows PowerShell スクリプト (.ps1) として保存します。

```powershell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing

$form = New-Object System.Windows.Forms.Form
$form.Text = 'Select a Computer'
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
$label.Text = 'Please select a computer:'
$form.Controls.Add($label)

$listBox = New-Object System.Windows.Forms.ListBox
$listBox.Location = New-Object System.Drawing.Point(10,40)
$listBox.Size = New-Object System.Drawing.Size(260,20)
$listBox.Height = 80

[void] $listBox.Items.Add('atl-dc-001')
[void] $listBox.Items.Add('atl-dc-002')
[void] $listBox.Items.Add('atl-dc-003')
[void] $listBox.Items.Add('atl-dc-004')
[void] $listBox.Items.Add('atl-dc-005')
[void] $listBox.Items.Add('atl-dc-006')
[void] $listBox.Items.Add('atl-dc-007')

$form.Controls.Add($listBox)

$form.Topmost = $true

$result = $form.ShowDialog()

if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItem
    $x
}
```

このスクリプトは 2 つの .NET Framework クラス、つまり **System.Drawing** と **System.Windows.Forms** を最初に読み込みます。 次に、.NET Framework クラス **System.Windows.Forms.Form** の新しいインスタンスを開始します。これにより、コントロールの追加を開始する空白のフォームまたはウィンドウが作成されます。

```powershell
Add-Type -AssemblyName System.Windows.Forms
Add-Type -AssemblyName System.Drawing
```

フォーム クラスのインスタンスを作成したら、このクラスの次の 3 つのプロパティに値を割り当てます。

- **Text**。 ウィンドウのタイトルになります。

- **Size**。 フォームのサイズをピクセル単位で表します。 前述のスクリプトにより、幅 300 ピクセル、高さ 200 ピクセルのフォームが作成されます。

- **StartingPosition**。 前述のスクリプトでは、この省略可能なプロパティは **CenterScreen** に設定されています。
  このプロパティを追加しない場合は、Windows によりフォームが開かれる場所が選択されます。 **StartingPosition** を **CenterScreen** に設定すると、フォームは読み込まれるたびに画面中央に自動的に表示されます。

```powershell
$form.Text = 'Select a Computer'
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
$form.AcceptButton = $okButton
$form.Controls.Add($okButton)
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

次に、ユーザーに提供する情報を記述するラベルのテキストをウィンドウ上に用意します。 この場合は、ユーザーにコンピューターを選択してもらいます。

```powershell
$label = New-Object System.Windows.Forms.Label
$label.Location = New-Object System.Drawing.Point(10,20)
$label.Size = New-Object System.Drawing.Size(280,20)
$label.Text = 'Please select a computer:'
$form.Controls.Add($label)
```

ラベルのテキストに記述されている情報をユーザーが提供できるコントロール (この場合はリスト ボックス) を追加します。 リスト ボックスに加えて、他に多数の適用可能なコントロールがあります。その他のコントロールについては、「[System.Windows.Forms 名前空間](/dotnet/api/system.windows.forms)」を参照してください。

```powershell
$listBox = New-Object System.Windows.Forms.ListBox
$listBox.Location = New-Object System.Drawing.Point(10,40)
$listBox.Size = New-Object System.Drawing.Size(260,20)
$listBox.Height = 80
```

次のセクションでは、リスト ボックスでユーザーに対して表示する値を指定します。

> [!NOTE]
> このスクリプトによって作成されるリスト ボックスでは、1 つの選択肢のみが許可されています。 複数選択が可能なリスト ボックス コントロールを作成するには、次のように **SelectionMode** プロパティの値を指定します: `$listBox.SelectionMode = 'MultiExtended'`。 詳しくは、「[複数選択のリスト ボックス](Multiple-selection-List-Boxes.md)」を参照してください。

```powershell
[void] $listBox.Items.Add('atl-dc-001')
[void] $listBox.Items.Add('atl-dc-002')
[void] $listBox.Items.Add('atl-dc-003')
[void] $listBox.Items.Add('atl-dc-004')
[void] $listBox.Items.Add('atl-dc-005')
[void] $listBox.Items.Add('atl-dc-006')
[void] $listBox.Items.Add('atl-dc-007')
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

最後に、**If** ブロック内のコードは、ユーザーがリスト ボックスからオプションを選択した後のフォームの操作を Windows に指示します。その後、**[OK]** ボタンをクリックするか、**Enter** キーを押します。

```powershell
if ($result -eq [System.Windows.Forms.DialogResult]::OK)
{
    $x = $listBox.SelectedItem
    $x
}
```

## <a name="see-also"></a>参照

- [GitHub: Dave Wyatt の WinFormsExampleUpdates](https://github.com/dlwyatt/WinFormsExampleUpdates)
- [Windows PowerShell Tip of the Week:リスト ボックスから項目を選択する](/previous-versions/windows/it-pro/windows-powershell-1.0/ff730949(v=technet.10))
