---
ms.date: 08/11/2020
keywords: DSC, PowerShell, 構成, セットアップ
title: DSC リソース メソッドの直接呼び出し
description: Invoke-DscResource コマンドレットは、DSC リソースの関数またはメソッドを呼び出すために使用できます。 これは、DSC リソースを使用するサード パーティが使用したり、リソースの開発中に役立つツールとして利用したりできます。
ms.openlocfilehash: 5ccf0f589b60cef4ec197d1e0a583af9ed60d5e7
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92650990"
---
# <a name="calling-dsc-resource-methods-directly"></a>DSC リソース メソッドの直接呼び出し

> 適用先: Windows PowerShell 5.0

[Invoke DscResource](/powershell/module/PSDesiredStateConfiguration/Invoke-DscResource) コマンドレットを使用すると、DSC リソースの関数またはメソッド (MOF ベースのリソースの `Get-TargetResource` 関数、`Set-TargetResource` 関数、`Test-TargetResource` 関数、またはクラスベースのリソースの **Get** メソッド、 **Set** メソッド、 **Test** メソッド) を直接呼び出すことができます。 これは、DSC リソースを使用するサード パーティが使用したり、リソースの開発中に役立つツールとして利用したりできます。

> [!NOTE]
> PowerShell 7.0 以降では、`Invoke-DscResource` は WMI DSC リソースの呼び出しをサポートしなくなりました。 これには、 **PSDesiredStateConfiguration** の **ファイル** リソースと **ログ** リソースが含まれます。

通常このコマンドレットは、メタ構成プロパティ `refreshMode = 'Disabled'` と組み合わせて使用されますが、どの **refreshMode** が設定されているかに関係なく使用できます。

`Invoke-DscResource` コマンドレットを呼び出すときに、 **Method** パラメーターを使用して、呼び出すメソッドまたは関数を指定します。 リソースのプロパティを指定するには、ハッシュ テーブルを、 **Property** パラメーターの値として渡します。

リソースのメソッドを直接呼び出す例を次に示します。

## <a name="ensure-a-file-is-present"></a>ファイルが存在することを確認します

```powershell
$result = Invoke-DscResource -Name File -Method Set -Property @{
              DestinationPath = "$env:SystemDrive\\DirectAccess.txt";
              Contents = 'This file is create by Invoke-DscResource'} -Verbose
$result | fl
```

## <a name="test-that-a-file-is-present"></a>ファイルが存在することをテストします

```powershell
$result = Invoke-DscResource -Name File -Method Test -Property @{
              DestinationPath="$env:SystemDrive\\DirectAccess.txt";
              Contents='This file is create by Invoke-DscResource'} -Verbose
$result | fl
```

## <a name="get-the-contents-of-file"></a>ファイルの内容を取得します

```powershell
$result = Invoke-DscResource -Name File -Method Get -Property @{
              DestinationPath="$env:SystemDrive\\DirectAccess.txt";
              Contents='This file is create by Invoke-DscResource'} -Verbose
$result.ItemValue | fl
```

> [!NOTE]
> 複合リソースのメソッドを直接呼び出すことはできません。 代わりに、複合リソースの基になるリソースのメソッドを呼び出してください。

## <a name="see-also"></a>参照

- [MOF を使用したカスタム DSC リソースの記述](../resources/authoringResourceMOF.md)
- [PowerShell クラスを使用したカスタム DSC リソースの記述](../resources/authoringResourceClass.md)
- [DSC リソースのデバッグ](../troubleshooting/debugResource.md)
