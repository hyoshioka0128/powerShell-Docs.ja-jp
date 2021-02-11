---
description: 環境
Locale: en-US
ms.date: 10/18/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_environment_provider?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: 環境プロバイダー
ms.openlocfilehash: f50558ba23d21b5ca145a06086c1c6d9b1fcd3bf
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99604970"
---
# <a name="environment-provider"></a>環境プロバイダー

## <a name="provider-name"></a>プロバイダー名
環境

## <a name="drives"></a>ドライブ

`Env:`

## <a name="capabilities"></a>機能

**ShouldProcess**

## <a name="short-description"></a>簡単な説明

Windows 環境変数へのアクセスを提供します。

## <a name="detailed-description"></a>詳しい説明

PowerShell **環境** プロバイダーを使用すると、powershell で環境変数と値を取得、追加、変更、消去、削除できます。

**環境** 変数は、プログラムが実行される環境を記述する変数として動的に名前が付けられます。 Windows と PowerShell は、環境変数を使用して、システムとプロセスの実行に影響を与える永続的な情報を格納します。 PowerShell 変数とは異なり、環境変数はスコープ制約の対象になりません。

**環境** ドライブは、現在のユーザーのセッションに固有の環境変数を含むフラットな名前空間です。 環境変数に子項目はありません。

**環境** プロバイダーは、この記事で説明されている次のコマンドレットをサポートしています。

- [Get-Location](xref:Microsoft.PowerShell.Management.Get-Location)
- [Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)
- [Get-Item](xref:Microsoft.PowerShell.Management.Get-Item)
- [New-Item](xref:Microsoft.PowerShell.Management.New-Item)
- [Remove-Item](xref:Microsoft.PowerShell.Management.Remove-Item)
- [Clear-Item](xref:Microsoft.PowerShell.Management.Clear-Item)

## <a name="types-exposed-by-this-provider"></a>このプロバイダーによって公開される型

各環境変数は、system.string [エントリ](/dotnet/api/system.collections.dictionaryentry) クラスのインスタンスです。 変数の名前はディクショナリ キーです。 環境変数の値はディクショナリ値です。

## <a name="navigating-the-environment-drive"></a>環境ドライブ内を移動する

**環境** プロバイダーは、ドライブ内のデータストアを公開し `Env:` ます。 環境変数を操作するには、場所を `Env:` ドライブ () に変更するか、別の `Set-Location Env:` PowerShell ドライブから作業します。 別の場所から環境変数を参照するには、 `Env:` パス内のドライブ名を使用します。

```powershell
Set-Location Env:
```

ファイル システム ドライブに戻るには、ドライブ名を入力します。 たとえば、次のように入力します。

```powershell
Set-Location C:
```

他の PowerShell ドライブから **環境** プロバイダーを使用することもできます。 別の場所から環境変数を参照するには、パス内のドライブ名を使用し `Env:` ます。

**環境** プロバイダーは、の変数プレフィックスを使用して環境変数も公開し `$env:` ます。  次のコマンドは、 **ProgramFiles** 環境変数の内容を表示します。 `$env:`変数プレフィックスは、任意の PowerShell ドライブから使用できます。

```
PS C:\> $env:ProgramFiles
C:\Program Files
```

変数プレフィックスを使用して環境変数の値を変更することもでき `$env:` ます。  変更は、アクティブになっている限り、現在の PowerShell セッションにのみ適用されます。

> [!NOTE]
> PowerShell では、エイリアスを使用して、プロバイダーパスを操作するための使い慣れた方法を使用できます。 `dir`やなどのコマンド `ls` は、 [get-childitem](xref:Microsoft.PowerShell.Management.Get-ChildItem)のエイリアスです。これ `cd` は、 [Set Location](xref:Microsoft.PowerShell.Management.Set-Location)のエイリアスです。 と `pwd` は、 [Get Location](xref:Microsoft.PowerShell.Management.Get-Location)のエイリアスです。

## <a name="getting-environment-variables"></a>環境変数の取得

このコマンドは、現在のセッションのすべての環境変数を一覧表示します。

```powershell
Get-Item -Path Env:
```

このコマンドは、任意の PowerShell ドライブから使用できます。

環境プロバイダーにはコンテナーがないため、上のコマンドはと共に使用した場合と同じ効果があり `Get-ChildItem` ます。

```powershell
Get-ChildItem -Path Env:
```

### <a name="get-a-selected-environment-variable"></a>選択した環境変数を取得します

このコマンドは、 `WINDIR` 環境変数を取得します。

```powershell
Get-ChildItem -Path Env:windir
```

また、変数プレフィックス形式も使用できます。

```powershell
$env:windir
```

## <a name="create-an-environment-variable"></a>環境変数を作成する

このコマンドは、 `USERMODE` "非管理者" という値を持つ環境変数を作成します。 パラメーター値により、 `-Path` ドライブに新しい項目が作成され `Env:` ます。 新しい環境変数は、アクティブである限り、現在の PowerShell セッションでのみ使用できます。

```powershell
PS C:\> New-Item -Path Env: -Name USERMODE -Value Non-Admin
```

## <a name="changing-an-environment-variable"></a>環境変数の変更

### <a name="rename-an-environment-variable"></a>環境変数の名前を変更する

このコマンドは、コマンドレットを使用して、作成した `Rename-Item` 環境変数の名前をに変更し `USERMODE` `USERROLE` ます。 システムが使用する環境変数の名前は変更しないでください。 これらの変更は現在のセッションにのみ影響を与えますが、システムまたはプログラムの誤作動の原因になりかねません。

```powershell
Rename-Item -Path Env:USERMODE -NewName USERROLE
```

### <a name="change-an-environment-variable"></a>環境変数の変更

このコマンドは、コマンドレットを使用して `Set-Item` 環境変数の値を `USERROLE` "Administrator" に変更します。

```powershell
Set-Item -Path Env:USERROLE -Value Administrator
```

## <a name="copy-an-environment-variable"></a>環境変数をコピーする

このコマンドは、環境変数の値 `USERROLE` を `USERROLE2` 環境変数にコピーします。

```powershell
Copy-Item -Path Env:USERROLE -Destination Env:USERROLE2
```

## <a name="remove-an-environment-variable"></a>環境変数を削除する

このコマンドは、 `USERROLE2` 現在のセッションから環境変数を削除します。

```powershell
Remove-Item -Path Env:USERROLE2
```

## <a name="remove-an-environment-variable-with-clear-item"></a>Clear-Item を使用して環境変数を削除する

このコマンドは、 `USERROLE` 値をクリアして環境変数を削除します。

```powershell
Clear-Item -Path Env:USERROLE
```

## <a name="using-the-pipeline"></a>パイプラインの使用

プロバイダーのコマンドレットは、パイプラインの入力を受け入れます。 パイプラインを使用すると、1つのコマンドレットから別のプロバイダーのコマンドレットにプロバイダーデータを送信することにより、タスクを簡単に行うことができます。
プロバイダーコマンドレットでパイプラインを使用する方法の詳細については、この記事全体で説明されているコマンドレットリファレンスを参照してください。

## <a name="getting-help"></a>ヘルプの表示

Windows PowerShell 3.0 より、プロバイダー コマンドレットのためにカスタマイズされたヘルプ トピックを取得できます。これはファイル システム ドライブでのプロバイダー コマンドレットの動作を説明します。

ファイルシステムドライブ用にカスタマイズされたヘルプトピックを取得するには、ファイルシステムドライブで [get-help](xref:Microsoft.PowerShell.Core.Get-Help) コマンドを実行するか、 `-Path` [get-help](xref:Microsoft.PowerShell.Core.Get-Help) のパラメーターを使用してファイルシステムドライブを指定します。

```powershell
Get-Help Get-ChildItem
```

```powershell
Get-Help Get-ChildItem -Path env:
```

## <a name="see-also"></a>関連項目

[about_Providers](../About/about_Providers.md)

