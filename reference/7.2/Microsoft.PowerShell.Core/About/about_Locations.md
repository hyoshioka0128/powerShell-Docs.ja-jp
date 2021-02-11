---
description: PowerShell で作業場所から項目にアクセスする方法について説明します。
Locale: en-US
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_locations?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Locations
ms.openlocfilehash: 4876abe3c651ad2279b85355f2e5e029203b6d4e
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600973"
---
# <a name="about_locations"></a>about_Locations

## <a name="short-description"></a>概要
PowerShell で作業場所から項目にアクセスする方法について説明します。

## <a name="long-description"></a>詳細説明

現在の作業場所は、コマンドが指す既定の場所です。
つまり、これは、コマンドの影響を受ける項目または場所への明示的なパスを指定していない場合に PowerShell が使用する場所です。 ほとんどの場合、現在の作業場所は、PowerShell FileSystem プロバイダーを介してアクセスされるドライブで、場合によってはそのドライブ上のディレクトリです。
たとえば、現在の作業場所を次の場所に設定できます。

```powershell
C:\Program Files\Windows PowerShell
```

その結果、別のパスが明示的に指定されていない限り、すべてのコマンドがこの場所から処理されます。

PowerShell は、ドライブが現在のドライブではない場合でも、各ドライブの現在の作業場所を保持します。 これにより、別の場所のドライブのみを参照して、現在の作業場所から項目にアクセスできるようになります。
たとえば、現在の作業場所が C: Windows であるとし \\ ます。 ここで、次のコマンドを使用して、現在の作業場所を HKLM: ドライブに変更したとします。

```powershell
Set-Location HKLM:
```

現在の場所はレジストリドライブになっていますが、 \\ 次の例に示すように、c: ドライブを使用するだけで c: Windows ディレクトリ内の項目にアクセスできます。

```powershell
Get-ChildItem C:
```

PowerShell は、そのドライブの現在の作業場所が Windows ディレクトリであることを記憶しているので、そのディレクトリから項目を取得します。 次のコマンドを実行した場合、結果は同じになります。

```powershell
Get-ChildItem C:\Windows
```

PowerShell では、Get-Location コマンドを使用して現在の作業場所を特定できます。また、Set-Location コマンドを使用して、現在の作業場所を設定できます。 たとえば、次のコマンドは、現在の作業場所を C: ドライブの Windows ディレクトリに設定します。

```powershell
Set-Location c:\windows
```

現在の作業場所を設定した後も、次の例に示すように、コマンドにドライブ名 (コロンの後にコロンを付ける) を含めるだけで、他のドライブから項目にアクセスできます。

```powershell
Get-ChildItem HKLM:\software
```

この例のコマンドは、レジストリの HKEY ローカルコンピューターハイブのソフトウェアコンテナー内の項目の一覧を取得します。

PowerShell では、特殊文字を使用して、現在の作業場所とその親の場所を表すこともできます。 現在の作業場所を表すには、1つのピリオドを使用します。 現在の作業場所の親を表すには、2つの期間を使用します。 たとえば、次の例では、現在の作業場所のシステムサブディレクトリを指定しています。

```powershell
Get-ChildItem .\system
```

現在の作業場所が C: windows の場合 \\ 、このコマンドは c: windows システム内のすべての項目の一覧を返し \\ \\ ます。 ただし、2つの期間を使用する場合は、次の例に示すように、現在の作業ディレクトリの親ディレクトリが使用されます。

```powershell
Get-ChildItem ..\"program files"
```

この場合、PowerShell は2つの期間を C: ドライブとして扱います。そのため、コマンドは C: Program Files ディレクトリ内のすべての項目を取得し \\ ます。

スラッシュで始まるパスは、現在のドライブのルートからのパスを識別します。 たとえば、現在の作業場所が C: \\ Program Files PowerShell の場合、 \\ ドライブのルートは c になります。したがって、次のコマンドでは、C: Windows ディレクトリ内のすべての項目が一覧表示され \\ ます。

```powershell
Get-ChildItem \windows
```

コンテナーまたは項目の名前を指定するときに、ドライブ名、スラッシュ、ピリオドで始まるパスを指定しなかった場合、コンテナーまたは項目は現在の作業場所に配置されていると見なされます。 たとえば、現在の作業場所が C: windows の場合、 \\ 次のコマンドは c: windows システムディレクトリ内のすべての項目を返し \\ \\ ます。

```powershell
Get-ChildItem system
```

ディレクトリ名ではなくファイル名を指定すると、PowerShell によってそのファイルの詳細が返されます (ファイルが現在の作業場所にあることを前提としています)。

## <a name="see-also"></a>関連項目

[Set-Location](xref:Microsoft.PowerShell.Management.Set-Location)

[about_Providers](about_Providers.md)

[about_Path_Syntax](about_Path_Syntax.md)

