---
description: PowerShell でオブジェクトのプロパティを使用する方法について説明します。
Locale: en-US
ms.date: 12/01/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_properties?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Properties
ms.openlocfilehash: c5cc7abdd580a0fa57134f9c79616d1d8290a7e1
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601390"
---
# <a name="about-properties"></a>プロパティについて

## <a name="short-description"></a>簡単な説明
PowerShell でオブジェクトのプロパティを使用する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell では、オブジェクトと呼ばれる情報の構造化されたコレクションを使用して、データストア内の項目またはコンピューターの状態を表します。 通常は、Microsoft .NET Framework の一部であるオブジェクトを使用しますが、PowerShell でカスタムオブジェクトを作成することもできます。

項目とそのオブジェクトの間の関連付けは非常に近いものです。 オブジェクトを変更する場合は、通常、そのオブジェクトが表すアイテムを変更します。 たとえば、PowerShell でファイルを取得する場合、実際のファイルは取得されません。 代わりに、ファイルを表す FileInfo オブジェクトを取得します。 FileInfo オブジェクトを変更すると、ファイルも変更されます。

ほとんどのオブジェクトにはプロパティがあります。 プロパティは、オブジェクトに関連付けられているデータです。 オブジェクトの種類によって、プロパティが異なります。 たとえば、ファイルを表す FileInfo オブジェクトには、ファイルが読み取り専用の属性を持っている場合は $True を含む **IsReadOnly** プロパティがあり、存在しない場合は $False ます。
ファイルシステムディレクトリを表す DirectoryInfo オブジェクトには、親ディレクトリへのパスを含む親プロパティがあります。

### <a name="object-properties"></a>オブジェクトのプロパティ

オブジェクトのプロパティを取得するには、コマンドレットを使用し `Get-Member` ます。 たとえば、 **fileinfo** オブジェクトのプロパティを取得するには、コマンドレットを使用して、 `Get-ChildItem` ファイルを表す fileinfo オブジェクトを取得します。 次に、パイプライン演算子 (&#124;) を使用して、 **FileInfo** オブジェクトをに送信し `Get-Member` ます。 次のコマンドは、PowerShell.exe ファイルを取得してに送信し `Get-Member` ます。
\$Pshome 自動変数には、PowerShell インストールディレクトリのパスが含まれています。

```powershell
Get-ChildItem $pshome\PowerShell.exe | Get-Member
```

コマンドの出力には、 **FileInfo** オブジェクトのメンバーが一覧表示されます。
メンバーには、プロパティとメソッドの両方が含まれます。 PowerShell で作業する場合は、オブジェクトのすべてのメンバーにアクセスできます。

メソッドではなく、オブジェクトのプロパティのみを取得するには、 `Get-Member` 次の例に示すように、コマンドレットの MemberType パラメーターに値 "property" を指定します。

```powershell
Get-ChildItem $pshome\PowerShell.exe | Get-Member -MemberType property
```

```Output
TypeName: System.IO.FileInfo

Name              MemberType Definition
----              ---------- ----------
Attributes        Property   System.IO.FileAttributes Attributes {get;set;}
CreationTime      Property   System.DateTime CreationTime {get;set;}
CreationTimeUtc   Property   System.DateTime CreationTimeUtc {get;set;}
Directory         Property   System.IO.DirectoryInfo Directory {get;}
DirectoryName     Property   System.String DirectoryName {get;}
Exists            Property   System.Boolean Exists {get;}
Extension         Property   System.String Extension {get;}
FullName          Property   System.String FullName {get;}
IsReadOnly        Property   System.Boolean IsReadOnly {get;set;}
LastAccessTime    Property   System.DateTime LastAccessTime {get;set;}
LastAccessTimeUtc Property   System.DateTime LastAccessTimeUtc {get;set;}
LastWriteTime     Property   System.DateTime LastWriteTime {get;set;}
LastWriteTimeUtc  Property   System.DateTime LastWriteTimeUtc {get;set;}
Length            Property   System.Int64 Length {get;}
Name              Property   System.String Name {get;}
```

プロパティが見つかったら、それらを PowerShell コマンドで使用できます。

## <a name="property-values"></a>プロパティ値

特定の型のすべてのオブジェクトに同じプロパティがありますが、これらのプロパティの値によって特定のオブジェクトが記述されます。 たとえば、すべての FileInfo オブジェクトには、"ファイルの時刻" プロパティがありますが、そのプロパティの値はファイルごとに異なります。

オブジェクトのプロパティの値を取得する最も一般的な方法は、ドットメソッドを使用することです。 オブジェクトを格納する変数などのオブジェクトへの参照、またはオブジェクトを取得するコマンドを入力します。 次に、ドット (.) の後にプロパティ名を入力します。

たとえば、次のコマンドは、PowerShell.exe ファイルの "値の指定" プロパティの値を表示します。 この `Get-ChildItem` コマンドは、PowerShell.exe ファイルを表す FileInfo オブジェクトを返します。 コマンドは、プロパティがアクセスされる前に実行されるように、かっこで囲まれています。 `Get-ChildItem`次に示すように、コマンドの後には、ドットと、"値の指定" プロパティの名前が続きます。

```powershell
(Get-ChildItem $pshome\PowerShell.exe).creationtime
```

```output
Tuesday, March 18, 2008 12:07:52 AM
```

また、次の例に示すように、オブジェクトを変数に保存し、ドットメソッドを使用してそのプロパティを取得することもできます。

```powershell
$a = Get-ChildItem $pshome\PowerShell.exe
$a.CreationTime
```

```output
Tuesday, March 18, 2008 12:07:52 AM
```

また、 `Select-Object` `Format-List` コマンドレットとコマンドレットを使用して、オブジェクトのプロパティ値を表示することもできます。 `Select-Object``Format-List`それぞれに **プロパティ** パラメーターがあります。 **Property** パラメーターを使用して、1つまたは複数のプロパティとその値を指定できます。 または、ワイルドカード文字 ( \* ) を使用してすべてのプロパティを表すことができます。

たとえば、次のコマンドは、PowerShell.exe ファイルのすべてのプロパティの値を表示します。

```powershell
Get-ChildItem $pshome\PowerShell.exe | Format-List -Property *
```

```output
PSPath            : Microsoft.PowerShell.Core\FileSystem::C:\Windows\System3
                    2\WindowsPowerShell\v1.0\PowerShell.exe
PSParentPath      : Microsoft.PowerShell.Core\FileSystem::C:\Windows\System3
                    2\WindowsPowerShell\v1.0
PSChildName       : PowerShell.exe
PSDrive           : C
PSProvider        : Microsoft.PowerShell.Core\FileSystem
PSIsContainer     : False
Mode              : -a----
VersionInfo       : File:             C:\Windows\System32\WindowsPowerShell\
                    v1.0\PowerShell.exe
                    InternalName:     POWERSHELL
                    OriginalFilename: PowerShell.EXE.MUI
                    FileVersion:      10.0.16299.15 (WinBuild.160101.0800)
                    FileDescription:  Windows PowerShell
                    Product:          Microsoft Windows Operating System
                    ProductVersion:   10.0.16299.15
                    Debug:            False
                    Patched:          False
                    PreRelease:       False
                    PrivateBuild:     False
                    SpecialBuild:     False
                    Language:         English (United States)

BaseName          : PowerShell
Target            : {C:\Windows\WinSxS\amd64_microsoft-windows-powershell-ex
                    e_31bf3856ad364e35_10.0.16299.15_none_8c022aa6735716ae\p
                    owershell.exe}
LinkType          : HardLink
Name              : PowerShell.exe
Length            : 449024
DirectoryName     : C:\Windows\System32\WindowsPowerShell\v1.0
Directory         : C:\Windows\System32\WindowsPowerShell\v1.0
IsReadOnly        : False
Exists            : True
FullName          : C:\Windows\System32\WindowsPowerShell\v1.0\PowerShell.ex
Extension         : .exe
CreationTime      : 9/29/2017 6:43:19 AM
CreationTimeUtc   : 9/29/2017 1:43:19 PM
LastAccessTime    : 9/29/2017 6:43:19 AM
LastAccessTimeUtc : 9/29/2017 1:43:19 PM
LastWriteTime     : 9/29/2017 6:43:19 AM
LastWriteTimeUtc  : 9/29/2017 1:43:19 PM
Attributes        : Archive
```

### <a name="static-properties"></a>静的プロパティ

PowerShell では、.NET クラスの静的プロパティを使用できます。 静的プロパティは、オブジェクトのプロパティである標準プロパティとは異なり、クラスのプロパティです。

クラスの静的プロパティを取得するには、Get-Member コマンドレットの Static パラメーターを使用します。

たとえば、次のコマンドは、クラスの静的プロパティを取得し `System.DateTime` ます。

```powershell
Get-Date | Get-Member -MemberType Property -Static
```

```Output
TypeName: System.DateTime

Name     MemberType Definition
----     ---------- ----------
MaxValue Property   static datetime MaxValue {get;}
MinValue Property   static datetime MinValue {get;}
Now      Property   datetime Now {get;}
Today    Property   datetime Today {get;}
UtcNow   Property   datetime UtcNow {get;}
```

静的プロパティの値を取得するには、次の構文を使用します。

```
[<ClassName>]::<Property>
```

たとえば、次のコマンドは、クラスの UtcNow 静的プロパティの値を取得し `System.DateTime` ます。

```powershell
[System.DateTime]::UtcNow
```

### <a name="properties-of-scalar-objects-and-collections"></a>スカラーオブジェクトとコレクションのプロパティ

特定の型の1つの ("スカラー") オブジェクトのプロパティは、多くの場合、同じ型のオブジェクトのコレクションのプロパティとは異なります。
たとえば、すべてのサービスには **displayname** プロパティがありますが、サービスのコレクションには **displayname** プロパティがありません。

次のコマンドは、' Audiosrv ' サービスの **DisplayName** プロパティの値を取得します。

```powershell
(Get-Service Audiosrv).DisplayName
```

```output
Windows Audio
```

PowerShell 3.0 以降、PowerShell は、スカラーオブジェクトとコレクションの異なるプロパティに起因するスクリプトエラーを防止しようとします。 同じコマンドを実行すると、を返すすべてのサービスの **DisplayName** プロパティの値が返され `Get-Service` ます。

```powershell
(Get-Service).DisplayName
```

```output
Application Experience
Application Layer Gateway Service
Windows All-User Install Agent
Application Identity
Application Information
...
```

コレクションを送信するときに、単一の ("スカラー") オブジェクトにのみ存在するプロパティを要求した場合、PowerShell はコレクション内のすべてのオブジェクトについて、そのプロパティの値を返します。

すべてのコレクションには、コレクションに含まれるオブジェクトの数を返す **Count** プロパティがあります。

```powershell
(Get-Service).Count
```

```output
176
```

PowerShell 3.0 以降、0個のオブジェクトまたは1つのオブジェクトの Count または Length プロパティを要求した場合、PowerShell は正しい値を返します。

```powershell
(Get-Service Audiosrv).Count
```

```output
1
```

プロパティが個々のオブジェクトとコレクションに存在する場合は、コレクションのプロパティだけが返されます。

 ```powershell
 $collection = @(
 [pscustomobject]@{length = "foo"}
 [pscustomobject]@{length = "bar"}
 )
 # PowerShell returns the collection's Length.
 $collection.length
 ```

 ```output
 2
 ```

この機能は、スカラーオブジェクトおよびコレクションのメソッドに対しても機能します。 詳細については、「 [about_Methods](about_methods.md)」を参照してください。

## <a name="see-also"></a>関連項目

[about_Methods](about_Methods.md)

[about_Objects](about_Objects.md)

[Get-Member](xref:Microsoft.PowerShell.Utility.Get-Member)

[Select-Object](xref:Microsoft.PowerShell.Utility.Select-Object)

[Format-List](xref:Microsoft.PowerShell.Utility.Format-List)

