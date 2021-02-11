---
description: PowerShell でオブジェクトを作成する方法について説明します。
Locale: en-US
ms.date: 09/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_object_creation?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Object_Creation
ms.openlocfilehash: 72c0d763fe7f3838c8b2ca68930521e24d0e6139
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605194"
---
# <a name="about-object-creation"></a>オブジェクトの作成について

## <a name="short-description"></a>簡単な説明

PowerShell でオブジェクトを作成する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell でオブジェクトを作成し、コマンドやスクリプトで作成したオブジェクトを使用することができます。

オブジェクトを作成するには多くの方法がありますが、このリストは明確ではありません。

- [New-object](xref:Microsoft.PowerShell.Utility.New-Object): .NET Framework オブジェクトまたは COM オブジェクトのインスタンスを作成します。
- [インポート-Csv](xref:Microsoft.PowerShell.Utility.Import-Csv) /
  [Convertfrom-csv](xref:Microsoft.PowerShell.Utility.ConvertFrom-Csv): コンマ区切り値として定義された項目からカスタムオブジェクト (**pscustomobject**) を作成します。
- [Convertfrom-csv](xref:Microsoft.PowerShell.Utility.ConvertFrom-Json): json (JavaScript Object Notation) で定義されているカスタムオブジェクトを作成します。
- [Convertfrom-csv-convertfrom-stringdata](xref:Microsoft.PowerShell.Utility.ConvertFrom-StringData): キーと値のペアとして定義されたカスタムオブジェクトを作成します。
- [Add-Type](xref:Microsoft.PowerShell.Utility.Add-Type): PowerShell セッションでインスタンス化できるクラスを定義でき `New-Object` ます。
- [New-Module](xref:Microsoft.PowerShell.Core.New-Module): **ascustomobject** いうパラメーターは、スクリプトブロックを使用して定義するカスタムオブジェクトを作成します。
- [メンバーの追加](xref:Microsoft.PowerShell.Utility.Add-Member): 既存のオブジェクトにプロパティを追加します。 を使用すると `Add-Member` 、などの単純な型からカスタムオブジェクトを作成でき `[System.Int32]` ます。
- [[オブジェクトの選択]](xref:Microsoft.PowerShell.Utility.Select-Object): オブジェクトのプロパティを選択します。 を使用する `Select-Object` と、既にインスタンス化されたオブジェクトに対して、カスタムプロパティおよび計算されるプロパティを作成できます。

この記事では、次の追加の方法について説明します。

- 静的メソッドを使用して型のコンストラクターを呼び出す `new()`
- Typecasting ハッシュテーブルのプロパティ名とプロパティ値

## <a name="static-new-method"></a>Static new () メソッド

すべての .NET 型には、 `new()` インスタンスをより簡単に構築できるメソッドが用意されています。 また、指定された型に対して使用可能なすべてのコンストラクターを確認することもできます。

型のコンストラクターを表示するには、 `new` 型名の後にメソッド名を指定し、キーを押し `<ENTER>` ます。

```powershell
[System.Uri]::new
```

```Output
OverloadDefinitions
-------------------
uri new(string uriString)
uri new(string uriString, bool dontEscape)
uri new(uri baseUri, string relativeUri, bool dontEscape)
uri new(string uriString, System.UriKind uriKind)
uri new(uri baseUri, string relativeUri)
uri new(uri baseUri, uri relativeUri)
```

ここで、適切なコンストラクターを指定 **して、system.string を作成** できます。

```powershell
[System.Uri]::new("https://www.bing.com")
```

```Output
AbsolutePath   : /
AbsoluteUri    : https://www.bing.com/
LocalPath      : /
Authority      : www.bing.com
...
```

次のサンプルを使用して、インスタンス化するために現在読み込まれている .NET 型を確認できます。

```powershell
[AppDomain]::CurrentDomain.GetAssemblies() |
  ForEach-Object {
    $_.GetExportedTypes() |
      ForEach-Object { $_.FullName }
  }
```

メソッドを使用して作成されたオブジェクトは、 `new()` PowerShell コマンドレットで作成された同じ型のオブジェクトと同じプロパティを持つことはできません。 PowerShell コマンドレット、プロバイダー、および拡張型システムでは、インスタンスに追加のプロパティを追加できます。

たとえば、PowerShell の FileSystem プロバイダーは、によって返される **DirectoryInfo** オブジェクトに、6つの **メモプロパティ** 値を追加し `Get-Item` ます。

```powershell
$PSDirInfo = Get-Item /
$PSDirInfo | Get-Member | Group-Object MemberType | Select-Object Count, Name
```

```Output
Count Name
----- ----
    4 CodeProperty
   13 Property
    6 NoteProperty
    1 ScriptProperty
   18 Method
```

**DirectoryInfo** オブジェクトを直接作成した場合、そのオブジェクトには、これらの **メモプロパティ** 値は含まれません。

```powershell
$NewDirInfo = [System.IO.DirectoryInfo]::new('/')
$NewDirInfo | Get-Member | Group-Object MemberType | Select-Object Count, Name
```

```Output
Count Name
----- ----
    4 CodeProperty
   13 Property
    1 ScriptProperty
   18 Method
```

拡張型システムの詳細については、「 [about_Types.ps1xml](about_Types.ps1xml.md)」を参照してください。

この機能は PowerShell 5.0 で追加されました

## <a name="create-objects-from-hash-tables"></a>ハッシュテーブルからオブジェクトを作成する

プロパティとプロパティ値のハッシュテーブルからオブジェクトを作成できます。

構文は次のとおりです。

```
[<class-name>]@{
  <property-name>=<property-value>
  <property-name>=<property-value>
}
```

このメソッドは、パラメーターなしのコンストラクターを持つクラスに対してのみ機能します。 オブジェクトのプロパティは、パブリックで設定可能である必要があります。

この機能は PowerShell バージョン3.0 で追加されました

## <a name="create-custom-objects-from-hash-tables"></a>ハッシュテーブルからカスタムオブジェクトを作成する

カスタムオブジェクトは非常に便利で、ハッシュテーブルメソッドを使用して簡単に作成できます。 **Pscustomobject** 各クラスは、特にこの目的のために設計されています。

カスタムオブジェクトは、関数またはスクリプトからカスタマイズされた出力を返すための優れた方法です。 これは、他のコマンドに再フォーマットしたりパイプを使用したりできない書式設定された出力を返すよりも便利です。

のコマンドは、 `Test-Object function` 変数の値を設定し、それらの値を使用してカスタムオブジェクトを作成します。 このオブジェクトは、コマンドレットのヘルプトピックの「例」のセクションで使用して `Update-Help` います。

```powershell
function Test-Object {
  $ModuleName = "PSScheduledJob"
  $HelpCulture = "en-us"
  $HelpVersion = "3.1.0.0"
  [PSCustomObject]@{
    "ModuleName"=$ModuleName
    "UICulture"=$HelpCulture
    "Version"=$HelpVersion
  }
  $ModuleName = "PSWorkflow"
  $HelpCulture = "en-us"
  $HelpVersion = "3.0.0.0"
  [PSCustomObject]@{
    "ModuleName"=$ModuleName
    "UICulture"=$HelpCulture
    "Version"=$HelpVersion
  }
}
Test-Object
```

この関数の出力は、既定ではテーブルとして書式設定されたカスタムオブジェクトのコレクションです。

```Output
ModuleName        UICulture      Version
---------         ---------      -------
PSScheduledJob    en-us          3.1.0.0
PSWorkflow        en-us          3.0.0.0
```

ユーザーは、標準のオブジェクトの場合と同じように、カスタムオブジェクトのプロパティを管理できます。

```powershell
(Test-Object).ModuleName
```

```Output
 PSScheduledJob
 PSWorkflow
```

## <a name="create-non-custom-objects-from-hash-tables"></a>ハッシュテーブルからの非カスタムオブジェクトの作成

また、ハッシュテーブルを使用して、非カスタムクラスのオブジェクトを作成することもできます。 カスタムではないクラスのオブジェクトを作成する場合は、名前空間で修飾された型名が必要です。ただし、最初の **システム** 名前空間コンポーネントは省略できます。

たとえば、次のコマンドでは、セッションオプションオブジェクトを作成します。

```powershell
[System.Management.Automation.Remoting.PSSessionOption]@{
  IdleTimeout=43200000
  SkipCnCheck=$True
}
```

ハッシュテーブル機能の要件 (特にパラメーターなしのコンストラクターの要件) によって、既存のクラスが多数削除されます。 ただし、ほとんどの PowerShell _オプション_ クラスは、この機能を使用するように設計されています。また、 **ProcessStartInfo** クラスなどの非常に便利なクラスでも使用できます。

```powershell
[System.Diagnostics.ProcessStartInfo]@{
  CreateNoWindow="$true"
  Verb="run as"
}
```

```Output
Arguments               :
ArgumentList            : {}
CreateNoWindow          : True
EnvironmentVariables    : {OneDriveConsumer, PROCESSOR_ARCHITECTURE,
                           CommonProgramFiles(x86), APPDATA...}
Environment             : {[OneDriveConsumer, C:\Users\user1\OneDrive],
                           [PROCESSOR_ARCHITECTURE, AMD64],
                           [CommonProgramFiles(x86),
                           C:\Program Files (x86)\Common Files],
                           [APPDATA, C:\Users\user1\AppData\Roaming]...}
RedirectStandardInput   : False
RedirectStandardOutput  : False
RedirectStandardError   : False
...
```

また、パラメーター値を設定するときに、ハッシュテーブル機能を使用することもできます。 たとえば、の **Sessionoption** パラメーターの値 `New-PSSession` 。
コマンドレットはハッシュテーブルにすることができます。

```powershell
New-PSSession -ComputerName Server01 -SessionOption @{
  IdleTimeout=43200000
  SkipCnCheck=$True
}
Register-ScheduledJob Name Test -FilePath .\Get-Inventory.ps1 -Trigger @{
  Frequency="Daily"
  At="15:00"
}
```

## <a name="generic-objects"></a>汎用オブジェクト

PowerShell で汎用オブジェクトを作成することもできます。 ジェネリックは、格納または使用される 1 つ以上の型のプレースホルダー (型パラメーター) を持つクラス、構造体、インターフェイス、およびメソッドです。

次の例では、 **ディクショナリ** オブジェクトを作成します。

```powershell
$dict = New-Object 'System.Collections.Generic.Dictionary[String,Int]'
$dict.Add("One", 1)
$dict
```

```Output
Key Value
--- -----
One     1
```

ジェネリックの詳細については、「 [.net のジェネリック](/dotnet/standard/generics)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Objects](about_Objects.md)

[about_Methods](about_Methods.md)

[about_Properties](about_Properties.md)

[about_Pipelines](about_Pipelines.md)

[about_Types.ps1xml](about_Types.ps1xml.md)
