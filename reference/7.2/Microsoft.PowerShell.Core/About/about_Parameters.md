---
description: PowerShell でコマンドパラメーターを操作する方法について説明します。
Locale: en-US
ms.date: 02/12/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_parameters?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Parameters
ms.openlocfilehash: c9d7ab62c13a7cafcf93103f9a70f6575d5dd7b8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602425"
---
# <a name="about-parameters"></a>パラメーターについて

## <a name="short-description"></a>簡単な説明
PowerShell でコマンドパラメーターを操作する方法について説明します。

## <a name="long-description"></a>長い説明

コマンドレット、関数、スクリプトなど、ほとんどの PowerShell コマンドは、ユーザーがオプションを選択したり入力を指定したりできるように、パラメーターに依存しています。 パラメーターはコマンド名に従い、次の形式になります。

```
-<parameter_name> <parameter_value>
-<parameter_name>:<parameter_value>
```

パラメーターの名前の前にはハイフン (-) が付きます。これは、ハイフンの後に続く単語がパラメーター名であることを PowerShell に通知します。 パラメーターの名前と値は、スペースまたはコロンで区切ることができます。 パラメーターには、パラメーター値を必要としないものと受け入れないものがあります。 その他のパラメーターには値が必要ですが、コマンドでパラメーター名を指定する必要はありません。

パラメーターの種類と要件は異なります。 コマンドのパラメーターに関する情報を検索するには、コマンドレットを使用し `Get-Help` ます。 たとえば、コマンドレットのパラメーターに関する情報を検索するには、次のように `Get-ChildItem` 入力します。

```powershell
Get-Help Get-ChildItem
```

スクリプトのパラメーターに関する情報を検索するには、スクリプトファイルへの完全なパスを使用します。 次に例を示します。

```powershell
Get-Help $home\Documents\Scripts\Get-Function.ps1
```

コマンドレットは、コマンド `Get-Help` の説明、コマンドの構文、パラメーターに関する情報、コマンドでパラメーターを使用する方法を示す例など、コマンドに関するさまざまな詳細情報を返します。

また、コマンドレットの Parameter パラメーターを使用し `Get-Help` て、特定のパラメーターに関する情報を検索することもできます。 または、 **parameter** パラメーターをワイルドカード文字 () 値と共に使用して `*` 、コマンドのすべてのパラメーターに関する情報を検索することもできます。 たとえば、次のコマンドは、コマンドレットのすべてのパラメーターに関する情報を取得し `Get-Member` ます。

```powershell
Get-Help Get-Member -Parameter *
```

### <a name="default-parameter-values"></a>既定のパラメーター値

省略可能なパラメーターには既定値があります。これは、コマンドでパラメーターが指定されていない場合に使用される値です。

たとえば、多くのコマンドレットの **ComputerName** パラメーターの既定値は、ローカルコンピューターの名前です。 このため、 **ComputerName** パラメーターが指定されていない場合、コマンドではローカルコンピューター名が使用されます。

既定のパラメーター値を確認するには、コマンドレットのヘルプトピックを参照してください。 パラメーターの説明には、既定値を含める必要があります。

また、コマンドレットまたは高度な関数の任意のパラメーターに対して、カスタムの既定値を設定することもできます。 カスタムの既定値の設定の詳細については、「 [about_Parameters_Default_Values](about_Parameters_Default_Values.md)」を参照してください。

### <a name="parameter-attribute-table"></a>パラメーター属性テーブル

コマンドレットの **Full**、 **parameter**、または **Online** パラメーターを使用すると `Get-Help` 、パラメーター `Get-Help` の詳細情報を含むパラメーター属性テーブルがによって表示されます。

この情報には、パラメーターを使用するために必要な詳細情報が含まれています。
たとえば、コマンドレットのヘルプトピックには、 `Get-ChildItem` Path パラメーターに関する次の詳細が含まれています。

```
-path <string[]>
    Specifies a path of one or more locations. Wildcard characters are
    permitted. The default location is the current directory (.).

Required?                    false
Position?                    0
Default value                Current directory
Accept pipeline input?       true (ByValue, ByPropertyName)
Accept wildcard characters?  true
```

パラメーター情報には、パラメーターの構文、パラメーターの説明、およびパラメーターの属性が含まれます。 以下のセクションでは、パラメーターの属性について説明します。

#### <a name="parameter-required"></a>パラメーターが必要です

この設定は、パラメーターが必須かどうか、つまり、このコマンドレットを使用するすべてのコマンドにこのパラメーターを含める必要があるかどうかを示します。 この値が **True** で、パラメーターがコマンドにない場合、PowerShell はパラメーターの値を入力するように求めます。

#### <a name="parameter-position"></a>パラメーターの位置

`Position`設定が正の整数に設定されている場合、パラメーター名は必要ありません。 この型のパラメーターは、位置指定パラメーターと呼ばれます。この数値は、他の位置指定パラメーターとの関係でパラメーターを表示する必要がある位置を示します。 名前付きパラメーターは、コマンドレット名の後の任意の位置に表示できます。 位置指定パラメーターのパラメーター名を指定した場合、パラメーターはコマンドレット名の後の任意の位置に一覧表示できます。

たとえば、 `Get-ChildItem` コマンドレットには Path パラメーターと Exclude パラメーターがあります。 `Position`**パス** の設定は **0** であり、これは位置指定パラメーターであることを意味します。 `Position` **Exclude** の設定には **という名前が付け** られます。

これは、 **Path** ではパラメーター名を必要としないことを意味しますが、パラメーター値はコマンドの最初のパラメーター値または無名のパラメーター値である必要があります。
ただし、Exclude パラメーターは名前付きパラメーターであるため、コマンド内の任意の位置に配置できます。

`Position`これら2つのパラメーターの設定の結果として、次のコマンドのいずれかを使用できます。

```powershell
Get-ChildItem -Path c:\techdocs -Exclude *.ppt
Get-ChildItem c:\techdocs -Exclude *.ppt
Get-ChildItem -Exclude *.ppt -Path c:\techdocs
Get-ChildItem -Exclude *.ppt c:\techdocs
```

パラメーター名を含めずに別の位置指定パラメーターを追加する場合、そのパラメーターは、設定で指定された順序で配置される必要があり `Position` ます。

#### <a name="parameter-type"></a>パラメーターの型

この設定では、パラメーター値の Microsoft .NET Framework 型を指定します。 たとえば、型が **Int32** の場合、パラメーター値は整数である必要があります。 型が string の場合、パラメーター値は文字列である必要があります。 文字列に空白が含まれている場合は、値を引用符で囲む必要があります。または、スペースの前にエスケープ文字 (') を付ける必要があります。

#### <a name="default-value"></a>既定値

この設定では、他の値が指定されていない場合に、パラメーターが想定する値を指定します。 たとえば、Path パラメーターの既定値は、多くの場合、現在のディレクトリです。 必須のパラメーターに既定値を指定することはできません。
省略可能なパラメーターの多くでは、パラメーターが使用されていない場合は効果がないため、既定値はありません。

#### <a name="accepts-multiple-values"></a>複数の値を受け取る

この設定は、パラメーターが複数のパラメーター値を受け入れるかどうかを示します。
パラメーターが複数の値を受け取る場合は、コマンドのパラメーターの値としてコンマ区切りのリストを入力するか、コンマ区切りのリスト (配列) を変数に保存してから、その変数をパラメーター値として指定します。

たとえば、コマンドレットの ServiceName パラメーターには、複数の値を指定 `Get-Service` できます。 次のコマンドは両方とも有効です。

```powershell
Get-Service -servicename winrm, netlogon
```

```powershell
$s = "winrm", "netlogon"
Get-Service -servicename $s
```

#### <a name="accepts-pipeline-input"></a>パイプライン入力の受け入れ

この設定は、パイプライン演算子 () を使用して、パラメーターに値を送信できるかどうかを示し `|` ます。

```
Value                    Description
-----                    -----------
False                    Indicates that you cannot pipe a value to the
                         parameter.

True (by Value)          Indicates that you can pipe any value to the
                         parameter, just so the value has the .NET
                         Framework type specified for the parameter or the
                         value can be converted to the specified .NET
                         Framework type.
```

パラメーターが "True (値渡し)" の場合、PowerShell は他のメソッドがコマンドを解釈する前に、パイプされた値をそのパラメーターに関連付けようとします。

```
True (by Property Name)  Indicates that you can pipe a value to the
                         parameter, but the .NET Framework type of the
                         parameter must include a property with the same
                         name as the parameter.
```

たとえば、値 **に name という名前** のプロパティがある場合にのみ、値を **名前** パラメーターにパイプすることができます。

> [!NOTE]
> パイプラインの入力 () または () を受け入れる型指定されたパラメーター `by Value` `by PropertyName` では、パラメーターで **遅延バインディング** スクリプトブロックを使用できます。
>
> **遅延バインド** スクリプトブロックは、 **parameterbinding** 中に自動的に実行されます。 結果は、パラメーターにバインドされます。 遅延バインディングは、型または型として定義されたパラメーターに対しては機能し **ません** `ScriptBlock` `System.Object` 。スクリプトブロックは呼び出さ **ず** に渡されます。
>
> **遅延バインドの** スクリプトブロックについては、「about_Script_Blocks」を参照 [してください。](about_Script_Blocks.md)

#### <a name="accepts-wildcard-characters"></a>ワイルドカード文字を許可する

この設定では、パラメーターの値にワイルドカード文字を含めることができるかどうかを示します。これにより、ターゲットコンテナー内の複数の既存の項目とパラメーター値を照合できるようになります。

#### <a name="common-parameters"></a>共通パラメーター

共通パラメーターは、任意のコマンドレットで使用できるパラメーターです。 共通パラメーターの詳細については、「 [about_CommonParameters](about_CommonParameters.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Command_syntax](about_Command_syntax.md)

[about_Comment_Based_Help](about_Comment_Based_Help.md)

[about_Functions_Advanced](about_Functions_Advanced.md)

[about_Parameters_Default_Values](about_Parameters_Default_Values.md)

[about_Pipelines](about_Pipelines.md)

[about_Wildcards](about_Wildcards.md)

