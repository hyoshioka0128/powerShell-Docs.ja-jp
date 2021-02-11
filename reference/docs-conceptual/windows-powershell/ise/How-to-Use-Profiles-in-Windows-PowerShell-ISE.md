---
ms.date: 01/02/2020
title: Windows PowerShell ISE でプロファイルを使用する方法
description: この記事では、Windows PowerShell ISE でプロファイルを使用する方法について説明します。
ms.openlocfilehash: e677a4aaa3b2b8b76f289b0797aaa75c80c2b370
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92663746"
---
# <a name="how-to-use-profiles-in-windows-powershell-ise"></a>Windows PowerShell ISE でプロファイルを使用する方法

この記事では、Windows PowerShell&reg; Integrated Scripting Environment (ISE) でプロファイルを使用する方法について説明します。 このセクションのタスクを実行する前に、[about_Profiles](/powershell/module/microsoft.powershell.core/about/about_profiles) を確認するか、またはコンソール ウィンドウに「`Get-Help about_Profiles`」と入力して <kbd>Enter</kbd> キーを押すことをお勧めします。

プロファイルは、新しいセッションを開始するときに自動的に実行される Windows PowerShell ISE スクリプトです。
Windows PowerShell ISE 用に 1 つ以上の Windows PowerShell プロファイルを作成すると、Windows PowerShell または Windows PowerShell ISE 環境の構成に、自分の用途に合わせて変数、エイリアス、関数、色やフォントの設定などを追加するために利用できます。 プロファイルは、開始するすべての Windows PowerShell ISE セッションに影響を与えます。

> [!NOTE]
> スクリプトの実行やプロファイルの読み込みが可能かどうかは、Windows PowerShell の実行ポリシーによって決まります。
> 既定の実行ポリシーは "Restricted" で、プロファイルを含め、すべてのスクリプトが実行されないようにします。
> "Restricted" ポリシーを使う場合は、プロファイルを読み込めません。 実行ポリシーの詳細については、「[about_Execution_Policies](/powershell/module/microsoft.powershell.core/about/about_execution_policies)」を参照してください。

## <a name="selecting-a-profile-to-use-in-the-windows-powershell-ise"></a>Windows PowerShell ISE で使うプロファイルの選択

Windows PowerShell ISE は、現在のユーザーとすべてのユーザーのプロファイルをサポートしています。 また、すべてのホストに適用される Windows PowerShell プロファイルもサポートしています。

使用するプロファイルは、Windows PowerShell と Windows PowerShell ISE の使用方法によって決まります。

- Windows PowerShell を実行するために Windows PowerShell ISE のみを使う場合は、すべての項目を ISE 固有のプロファイルの 1 つに保存します。たとえば、Windows PowerShell ISE 用の **CurrentUserCurrentHost** プロファイルや、Windows PowerShell ISE 用の **AllUsersCurrentHost** プロファイルです。

- Windows PowerShell を実行するために複数のホスト プログラムを使う場合は、関数、エイリアス、変数、コマンドをすべてのホスト プログラムに影響を与えるプロファイルに保存します。たとえば、CurrentUserAllHosts プロファイルや、 **AllUsersAllHosts** プロファイルです。また、色やフォントのカスタマイズのような ISE 固有の機能は、Windows PowerShell ISE 用の **CurrentUserCurrentHost** プロファイルか、Windows PowerShell ISE 用の **AllUsersCurrentHost** プロファイルに保存します。

Windows PowerShell ISE で作成して利用できるプロファイルは次のとおりです。 各プロファイルは、それぞれ特定のパスに保存されます。

|           プロファイルの種類           |                   プロファイルのパス                   |
| -------------------------------- | ------------------------------------------------ |
| **現在のユーザー、PowerShell ISE** | `$PROFILE.CurrentUserCurrentHost` または `$PROFILE` |
| **すべてのユーザー、PowerShell ISE**    | `$PROFILE.AllUsersCurrentHost`                   |
| **現在のユーザー、すべてのホスト**      | `$PROFILE.CurrentUserAllHosts`                   |
| **すべてのユーザー、すべてのホスト**         | `$PROFILE.AllUsersAllHosts`                      |

## <a name="to-create-a-new-profile"></a>新しいプロファイルを作成するには

"現在のユーザー、Windows PowerShell ISE" の新しいプロファイルを作成するには、次のコマンドを実行します。

```powershell
if (!(Test-Path -Path $PROFILE ))
{ New-Item -Type File -Path $PROFILE -Force }
```

"すべてのユーザー、Windows PowerShell ISE" の新しいプロファイルを作成するには、次のコマンドを実行します。

```powershell
if (!(Test-Path -Path $PROFILE.AllUsersCurrentHost))
{ New-Item -Type File -Path $PROFILE.AllUsersCurrentHost -Force }
```

"現在のユーザー、すべてのホスト" の新しいプロファイルを作成するには、次のコマンドを実行します。

```powershell
if (!(Test-Path -Path $PROFILE.CurrentUserAllHosts))
{ New-Item -Type File -Path $PROFILE.CurrentUserAllHosts -Force }
```

"すべてのユーザー、すべてのホスト" の新しいプロファイルを作成するには、次のように入力します。

```powershell
if (!(Test-Path -Path $PROFILE.AllUsersAllHosts))
{ New-Item -Type File -Path $PROFILE.AllUsersAllHosts -Force }
```

## <a name="to-edit-a-profile"></a>プロファイルを編集するには

1. プロファイルを開くには、編集するプロファイルを設定した変数を指定してコマンド `psEdit` を実行します。 たとえば、"現在のユーザー、Windows PowerShell ISE" のプロファイルを開くには、「`psEdit $PROFILE`」と入力します。

2. プロファイルに、いくつかの項目を追加します。 たとえば、次の例のように入力します。

   - コンソール ウィンドウの既定の背景色を青に変更するには、プロファイル ファイルに「`$psISE.Options.OutputPaneBackground = 'blue'`」と入力します。 `$psISE` 変数について詳しくは、「[Windows PowerShell ISE オブジェクト モデル リファレンス](object-model/The-ISE-Object-Model-Hierarchy.md)」をご覧ください。

   - フォント サイズを 20 に変更するには、プロファイル ファイルに「`$psISE.Options.FontSize =20`」と入力します。

3. プロファイル ファイルを保存するには、 **[ファイル]** メニューの **[保存]** をクリックします。 Windows PowerShell ISE を次に開いた時点で、カスタマイズの内容が適用されます。

## <a name="see-also"></a>参照

- [about_Profiles](/powershell/module/microsoft.powershell.core/about/about_profiles)
- [Windows PowerShell ISE の紹介](Introducing-the-Windows-PowerShell-ISE.md)
