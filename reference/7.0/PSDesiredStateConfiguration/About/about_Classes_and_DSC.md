---
description: クラスを使用して、Desired State Configuration (DSC) を使用して PowerShell で開発する方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 1/11/2019
online version: https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/about/about_classes_and_dsc?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Classes_and_DSC
ms.openlocfilehash: 00a7d18bb1ccf618b22b61d2197053365ea3f21b
ms.sourcegitcommit: cc72c40315fd2981d3009b335accbfa52d57640c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/30/2020
ms.locfileid: "96349791"
---
# <a name="about-classes-and-desired-state-configuration"></a>クラスと Desired State Configuration について

## <a name="short-description"></a>簡単な説明

クラスを使用して、Desired State Configuration (DSC) を使用して PowerShell で開発する方法について説明します。

## <a name="long-description"></a>長い説明

Windows PowerShell 5.0 以降では、他のオブジェクト指向プログラミング言語に似た正式な構文とセマンティクスを使用して、クラスやその他のユーザー定義型を定義するための言語が追加されました。 目標は、開発者や IT プロフェッショナルがさまざまなユースケースで PowerShell を採用できるようにし、DSC リソースなどの PowerShell アーティファクトの開発を簡素化し、管理面の範囲を短縮できるようにすることです。

## <a name="supported-scenarios"></a>サポートされるシナリオ

次のシナリオがサポートされます。

- PowerShell 言語を使用して、DSC リソースとそれに関連付けられている型を定義します。
- クラス、プロパティ、メソッド、継承などの使い慣れたオブジェクト指向プログラミング構造を使用して、PowerShell でカスタム型を定義します。
- PowerShell 言語を使用して型をデバッグします。
- 正式なメカニズムと適切なレベルを使用して、例外を生成して処理します。

## <a name="define-dsc-resources-with-classes"></a>クラスを使用して DSC リソースを定義する

構文の変更とは別に、クラス定義の DSC リソースとコマンドレット DSC リソースプロバイダーの主な違いは次のとおりです。

- 管理オブジェクトフォーマット (MOF) ファイルは必要ありません。
- モジュール フォルダーの DSCResource サブフォルダーは不要です。
- PowerShell モジュールのファイルには、複数の DSC リソース クラスを含めることができます。

## <a name="create-a-class-defined-dsc-resource-provider"></a>クラス定義の DSC リソースプロバイダーを作成する

次の例は、Mydscresource.psd1 というモジュールとして保存されているクラス定義の DSC リソースプロバイダーです。 hbase-runner.psm1 です。 常に、クラス定義の DSC リソースプロバイダーにキープロパティを含める必要があります。

```powershell
enum Ensure
{
    Absent
    Present
}

<#
    This resource manages the file in a specific path.
    [DscResource()] indicates the class is a DSC resource
#>

[DscResource()]
class FileResource
{
    <#
        This property is the fully qualified path to the file that is
        expected to be present or absent.

        The [DscProperty(Key)] attribute indicates the property is a
        key and its value uniquely identifies a resource instance.
        Defining this attribute also means the property is required
        and DSC will ensure a value is set before calling the resource.

        A DSC resource must define at least one key property.
    #>
    [DscProperty(Key)]
    [string]$Path

    <#
        This property indicates if the settings should be present or absent
        on the system. For present, the resource ensures the file pointed
        to by $Path exists. For absent, it ensures the file point to by
        $Path does not exist.

        The [DscProperty(Mandatory)] attribute indicates the property is
        required and DSC will guarantee it is set.

        If Mandatory is not specified or if it is defined as
        Mandatory=$false, the value is not guaranteed to be set when DSC
        calls the resource.  This is appropriate for optional properties.
    #>
    [DscProperty(Mandatory)]
    [Ensure] $Ensure

    <#
        This property defines the fully qualified path to a file that will
        be placed on the system if $Ensure = Present and $Path does not
        exist.

        NOTE: This property is required because [DscProperty(Mandatory)] is
        set.
    #>
    [DscProperty(Mandatory)]
    [string] $SourcePath

    <#
        This property reports the file's create timestamp.

        [DscProperty(NotConfigurable)] attribute indicates the property is
        not configurable in DSC configuration.  Properties marked this way
        are populated by the Get() method to report additional details
        about the resource when it is present.

    #>
    [DscProperty(NotConfigurable)]
    [Nullable[datetime]] $CreationTime

    <#
        This method is equivalent of the Set-TargetResource script function.
        It sets the resource to the desired state.
    #>
    [void] Set()
    {
        $fileExists = $this.TestFilePath($this.Path)
        if($this.ensure -eq [Ensure]::Present)
        {
            if(-not $fileExists)
            {
                $this.CopyFile()
            }
        }
        else
        {
            if($fileExists)
            {
                Write-Verbose -Message "Deleting the file $($this.Path)"
                Remove-Item -LiteralPath $this.Path -Force
            }
        }
    }

    <#

        This method is equivalent of the Test-TargetResource script
        function. It should return True or False, showing whether the
        resource is in a desired state.
    #>
    [bool] Test()
    {
        $present = $this.TestFilePath($this.Path)

        if($this.Ensure -eq [Ensure]::Present)
        {
            return $present
        }
        else
{
            return -not $present
        }
    }

    <#
        This method is equivalent of the Get-TargetResource script function.
        The implementation should use the keys to find appropriate
        resources. This method returns an instance of this class with the
        updated key properties.
    #>
    [FileResource] Get()
    {
        $present = $this.TestFilePath($this.Path)

        if ($present)
        {
            $file = Get-ChildItem -LiteralPath $this.Path
            $this.CreationTime = $file.CreationTime
            $this.Ensure = [Ensure]::Present
        }
        else
        {
            $this.CreationTime = $null
            $this.Ensure = [Ensure]::Absent
        }
        return $this
    }

    <#
        Helper method to check if the file exists and it is correct file
    #>
    [bool] TestFilePath([string] $location)
    {
        $present = $true

        $item = Get-ChildItem -LiteralPath $location -ea Ignore
        if ($null -eq $item)
        {
            $present = $false
        }
        elseif( $item.PSProvider.Name -ne "FileSystem")
        {
            throw "Path $($location) is not a file path."
        }
        elseif($item.PSIsContainer)
        {
            throw "Path $($location) is a directory path."
        }
        return $present
    }

    <#
        Helper method to copy file from source to path
    #>
    [void] CopyFile()
    {
        if(-not $this.TestFilePath($this.SourcePath))
        {
            throw "SourcePath $($this.SourcePath) is not found."
        }

        [System.IO.FileInfo]
        $destFileInfo = new-object System.IO.FileInfo($this.Path)

        if (-not $destFileInfo.Directory.Exists)
        {
            $FullName = $destFileInfo.Directory.FullName
            $Message = "Creating directory $FullName"

            Write-Verbose -Message $Message

            #use CreateDirectory instead of New-Item to avoid code
            # to handle the non-terminating error
            [System.IO.Directory]::CreateDirectory($FullName)
        }

        if(Test-Path -LiteralPath $this.Path -PathType Container)
        {
            throw "Path $($this.Path) is a directory path"
        }

        Write-Verbose -Message "Copying $this.SourcePath to $this.Path"

        #DSC engine catches and reports any error that occurs
        Copy-Item -Path $this.SourcePath -Destination $this.Path -Force
    }
}
```

## <a name="create-a-module-manifest"></a>モジュールマニフェストを作成する

クラス定義の DSC リソース プロバイダーを作成し、モジュールとして保存すると、モジュールのモジュール マニフェストを作成できます。 クラスベースのリソースを DSC エンジンで使用できるようにするには、マニフェスト ファイルに、リソースをエクスポートするようにモジュールに指示する `DscResourcesToExport` ステートメントを含める必要があります。 この例では、次のモジュール マニフェストは MyDscResource.psd1 として保存されます。

```powershell
@{

# Script module or binary module file associated with this manifest.
RootModule = 'MyDscResource.psm1'

DscResourcesToExport = 'FileResource'

# Version number of this module.
ModuleVersion = '1.0'

# ID used to uniquely identify this module
GUID = '81624038-5e71-40f8-8905-b1a87afe22d7'

# Author of this module
Author = 'Microsoft Corporation'

# Company or vendor of this module
CompanyName = 'Microsoft Corporation'

# Copyright statement for this module
Copyright = '(c) 2014 Microsoft. All rights reserved.'

# Description of the functionality provided by this module
# Description = ''

# Minimum version of the PowerShell engine required by this module
PowerShellVersion = '5.0'

# Name of the PowerShell host required by this module
# PowerShellHostName = ''

}
```

## <a name="deploy-a-dsc-resource-provider"></a>DSC リソースプロバイダーをデプロイする

またはで Mydscresource.psd1 フォルダーを作成して、新しい DSC リソースプロバイダーをデプロイし `$pshome\Modules` `$env:SystemDrive\ProgramFiles\WindowsPowerShell\Modules` ます。

DSCResource サブフォルダーを作成する必要はありません。 モジュールとモジュールのマニフェスト ファイル (MyDscResource.psm1 と MyDscResource.psd1) を MyDscResource フォルダーにコピーします。

これ以降、他の DSC リソースの場合と同様に、構成スクリプトを作成して実行します。

## <a name="create-a-dsc-configuration-script"></a>DSC 構成スクリプトを作成する

既に説明したように、クラスとマニフェスト ファイルをフォルダー構造で保存した後で、新しいリソースを使用する構成を作成できます。 次の構成は、Mydscresource.psd1 モジュールを参照します。 構成をスクリプトとして保存し、MyResource.ps1 します。

DSC 構成を実行する方法の詳細については、「 [Windows PowerShell Desired State configuration の概要](/powershell/scripting/dsc/overview/dscforengineers)」を参照してください。

構成を実行する前に、を作成 `C:\test.txt` します。 構成では、ファイルがに存在するかどうかを確認し `c:\test\test.txt` ます。 ファイルが存在しない場合は、構成によってからファイルがコピーされ `C:\test.txt` ます。

```powershell
Configuration Test
{
    Import-DSCResource -module MyDscResource
    FileResource file
    {
        Path = "C:\test\test.txt"
        SourcePath = "C:\test.txt"
        Ensure = "Present"
    }
}
Test
Start-DscConfiguration -Wait -Force Test
```

このスクリプトは、DSC 構成スクリプトの場合と同様に実行します。 構成を開始するには、管理者特権の PowerShell コンソールで、次のように実行します。

`PS C:\test> .\MyResource.ps1`

## <a name="inheritance-in-powershell-classes"></a>PowerShell クラスでの継承

### <a name="declare-base-classes-for-powershell-classes"></a>PowerShell クラスの基底クラスを宣言する

次の例に示すように、PowerShell クラスを別の PowerShell クラスの基本データ型として宣言できます。この例では、 **フルーツ** は **apple** の基本データ型です。

```powershell
class fruit
{
    [int]sold() {return 100500}
}

class apple : fruit {}
    [apple]::new().sold() # return 100500
```

### <a name="declare-implemented-interfaces-for-powershell-classes"></a>PowerShell クラスの実装されたインターフェイスを宣言する

基本型が指定されていない場合は、基本型の後、またはコロン () の直後に、実装されたインターフェイスを宣言でき `:` ます。 コンマを使用してすべての型名を区切ります。 これは、C# の構文に似ています。

```powershell
class MyComparable : system.IComparable
{
    [int] CompareTo([object] $obj)
    {
        return 0;
    }
}

class MyComparableTest : test, system.IComparable
{
    [int] CompareTo([object] $obj)
    {
        return 0;
    }
}
```

### <a name="call-base-class-constructors"></a>基底クラスのコンストラクターの呼び出し

サブクラスから基底クラスのコンストラクターを呼び出すには、 `base` 次の例に示すように、キーワードを追加します。

```powershell
class A {
    [int]$a
    A([int]$a)
    {
        $this.a = $a
    }
}

class B : A
{
    B() : base(103) {}
}

    [B]::new().a # return 103
```

基底クラスに既定のコンストラクター (パラメーターなし) がある場合は、以下に示すように、明示的なコンストラクター呼び出しを省略できます。

```powershell
class C : B
{
    C([int]$c) {}
}
```

### <a name="call-base-class-methods"></a>基底クラスのメソッドの呼び出し

サブクラスで既存のメソッドをオーバーライドすることができます。 オーバーライドを行うには、同じ名前とシグネチャを使用してメソッドを宣言します。

```powershell
class baseClass
{
    [int]days() {return 100500}
}
class childClass1 : baseClass
{
    [int]days () {return 200600}
}

    [childClass1]::new().days() # return 200600
```

オーバーライドされた実装から基本クラスメソッドを呼び出すには、呼び出し時に基本クラスにキャストし `([baseclass]$this)` ます。

```powershell
class childClass2 : baseClass
{
    [int]days()
    {
        return 3 * ([baseClass]$this).days()
    }
}

    [childClass2]::new().days() # return 301500
```

すべての PowerShell メソッドは仮想です。 オーバーライドの場合と同じ構文を使用して、サブクラスで非仮想 .NET メソッドを非表示にすることができます。同じ名前およびシグネチャを持つメソッドを宣言します。

```powershell
class MyIntList : system.collections.generic.list[int]
{
    # Add is final in system.collections.generic.list
    [void] Add([int]$arg)
    {
        ([system.collections.generic.list[int]]$this).Add($arg * 2)
    }
}

$list = [MyIntList]::new()
$list.Add(100)
$list[0] # return 200
```

### <a name="current-limitations-with-class-inheritance"></a>クラス継承に関する現在の制限事項

クラスの継承に関する制限は、PowerShell でインターフェイスを宣言する構文がないことです。

## <a name="defining-custom-types-in-powershell"></a>PowerShell でのカスタム型の定義

Windows PowerShell 5.0 では、いくつかの言語要素が導入されました。

### <a name="class-keyword"></a>class キーワード

新しいクラスを定義します。
`class`キーワードは、true .NET Framework 型です。
クラスメンバーはパブリックです。

```powershell
class MyClass
{
}
```

### <a name="enum-keyword-and-enumerations"></a>enum キーワードおよび列挙

キーワードのサポートが `enum` 追加されましたが、互換性に影響する変更点です。 `enum`現在、区切り記号は改行です。 既にを使用しているユーザーのための回避策 `enum` は、 `&` 単語の前にアンパサンド () を挿入することです。 現在の制限事項: 列挙子自体を使用して列挙子を定義することはできませんが、 `enum` 次の例に示すように、別の条件で初期化することができ `enum` ます。

現在、基本データ型を指定することはできません。 基本型は常に [int] です。

```powershell
enum Color2
{
    Yellow = [Color]::Blue
}
```

列挙子の値は、解析時定数である必要があります。 列挙子の値を、呼び出されたコマンドの結果に設定することはできません。

```powershell
enum MyEnum
{
    Enum1
    Enum2
    Enum3 = 42
    Enum4 = [int]::MaxValue
}
```

`Enum` では、次の例に示すように算術演算がサポートされています。

```powershell
enum SomeEnum { Max = 42 }
enum OtherEnum { Max = [SomeEnum]::Max + 1 }
```

### <a name="hidden-keyword"></a>Hidden キーワード

`hidden`Windows PowerShell 5.0 で導入されたキーワードは、既定の結果からクラスメンバーを非表示にし `Get-Member` ます。 次の行に示すように、非表示のプロパティを指定します。

```powershell
hidden [type] $classmember = <value>
```

非表示のメンバーは、非表示のメンバーを定義するクラスで完了が発生しない限り、タブ補完や IntelliSense を使用して表示されることはありません。

新しい属性 **system.management.automation.hiddenattribute** が追加され、C# コードが PowerShell 内で同じセマンティクスを持つことができるようになりました。

詳細については、「 [about_Hidden](../../Microsoft.PowerShell.Core/About/about_hidden.md)」を参照してください。

### <a name="import-dscresource"></a>Import-DscResource

`Import-DscResource` は真の動的キーワードです。 PowerShell では、DscResource 属性を含むクラスを探して、指定されたモジュールのルート モジュールを解析します。

### <a name="properties"></a>Properties

新しいフィールドが `ImplementingAssembly` に追加されました `ModuleInfo` 。 スクリプトでクラスが定義されている場合、またはバイナリモジュール用に読み込まれたアセンブリ `ImplementingAssembly` が、スクリプトモジュール用に作成された動的アセンブリに設定されている場合。 ModuleType = Manifest の場合は設定されません。

フィールドのリフレクション `ImplementingAssembly` では、モジュール内のリソースが検出されます。 これは、PowerShell または他の管理言語で記述されたリソースを検出できることを意味します。

初期化子を持つフィールド。

`[int] $i = 5`

Static はサポートされており、型の制約と同様に属性のように動作するため、任意の順序で指定できます。

`static [int] $count = 0`

型は省略できます。

`$s = "hello"`

すべてのメンバーはパブリックです。 プロパティには、改行文字かセミコロンが必要です。 オブジェクトの種類が指定されていない場合、プロパティの型は **object** です。

### <a name="constructors-and-instantiation"></a>コンストラクターとインスタンス化

PowerShell クラスは、クラスと同じ名前を持つコンストラクターを持つことができます。 コンストラクターはオーバーロードできます。 静的コンストラクターがサポートされています。
初期化式を持つプロパティは、コンストラクター内のコードを実行する前に初期化されます。 静的プロパティは、静的コンストラクターの本体の前に初期化され、インスタンスのプロパティは、非静的コンストラクターの本体の前に初期化されます。 現在、C# 構文など、別のコンストラクターからコンストラクターを呼び出すための構文はありませ `": this()")` ん。 対応策は、一般的な Init メソッドを定義することです。

クラスをインスタンス化する方法を次に示します。

- 既定のコンストラクターを使用したインスタンス化。 は、 `New-Object` このリリースではサポートされていないことに注意してください。

  `$a = [MyClass]::new()`

- パラメーターを使用してコンストラクターを呼び出します。

  `$b = [MyClass]::new(42)`

- 複数のパラメーターを持つコンストラクターに配列を渡す

  `$c = [MyClass]::new(@(42,43,44), "Hello")`

このリリースでは、型名は構文的にのみ表示されます。つまり、クラスを定義するモジュールまたはスクリプトの外部では参照できません。 関数は、PowerShell で定義されているクラスのインスタンスを返すことができ、インスタンスはモジュールまたはスクリプトの外部で正常に動作します。

`Get-Member`**静的** パラメーターはコンストラクターを一覧表示しているため、他のメソッドと同様にオーバーロードを表示できます。 また、この構文のパフォーマンスは、`New-Object` より大幅に高速です。

次の例に示すように、**new** という名前の擬似静的メソッドは .NET 型で機能します。 `[hashtable]::new()`

`Get-Member` を使って、またはこの例で示すようにして、コンストラクターのオーバーロードを確認できるようになりました。

```powershell
[hashtable]::new
OverloadDefinitions
-------------------
hashtable new()
hashtable new(int capacity)
hashtable new(int capacity, float loadFactor)
```

### <a name="methods"></a>メソッド

PowerShell のクラス メソッドは、end ブロックのみを持つ **ScriptBlock** として実装されます。 すべてのメソッドはパブリックです。 **DoSomething** という名前のメソッドを定義する例を次に示します。

```powershell
class MyClass
{
    DoSomething($x)
    {
        $this._doSomething($x)       # method syntax
    }
    private _doSomething($a) {}
}
```

### <a name="method-invocation"></a>メソッドの呼び出し

オーバーロードされたメソッドがサポートされています。 オーバーロードされたメソッドには、既存のメソッドと同じ名前が付けられますが、指定された値によって区別されます。

```powershell
$b = [MyClass]::new()
$b.DoSomething(42)
```

### <a name="invocation"></a>呼び出し

「 [メソッドの呼び出し](#method-invocation)」を参照してください。

### <a name="attributes"></a>属性

`DscResource`、 `DscResourceKey` 、およびという3つの新しい属性が追加されました `DscResourceMandatory` 。

### <a name="return-types"></a>戻り値の型

戻り値の型はコントラクトです。 戻り値は、予期された型に変換されます。
戻り値の型が指定されていない場合、戻り値の型は void です。 オブジェクトのストリーミングはありません。オブジェクトは意図的にも誤ってもパイプラインに書き込むことができません。

### <a name="lexical-scoping-of-variables"></a>変数の語彙的スコープ

今回のリリースで語彙的スコープが機能する方法の例を次に示します。

```powershell
$d = 42  # Script scope

function bar
{
    $d = 0  # Function scope
    [MyClass]::DoSomething()
}

class MyClass
{
    static [object] DoSomething()
    {
        return $d  # error, not found dynamically
        return $script:d # no error

        $d = $script:d
        return $d # no error, found lexically
    }
}

$v = bar
$v -eq $d # true
```

## <a name="example-create-custom-classes"></a>例: カスタムクラスの作成

次の例では、HTML 動的スタイルシート言語 (DSL) を実装するために、いくつかの新しいカスタムクラスを作成します。 この例では、要素クラスの一部として特定の要素型を作成するヘルパー関数を追加します。これは、モジュールのスコープ外では型を使用できないためです。

```powershell
# Classes that define the structure of the document
#
class Html
{
    [string] $docType
    [HtmlHead] $Head
    [Element[]] $Body

    [string] Render()
    {
        $text = "<html>`n<head>`n"
        $text += $Head
        $text += "`n</head>`n<body>`n"
        $text += $Body -join "`n" # Render all of the body elements
        $text += "</body>`n</html>"
        return $text
    }
    [string] ToString() { return $this.Render() }
}

class HtmlHead
{
    $Title
    $Base
    $Link
    $Style
    $Meta
    $Script
    [string] Render() { return "<title>$Title</title>" }
    [string] ToString() { return $this.Render() }
}

class Element
{
    [string] $Tag
    [string] $Text
    [hashtable] $Attributes
    [string] Render() {
        $attributesText= ""
        if ($Attributes)
        {
            foreach ($attr in $Attributes.Keys)
            {
                $attributesText = " $attr=`"$($Attributes[$attr])`""
            }
        }

        return "<${tag}${attributesText}>$text</$tag>`n"
    }
    [string] ToString() { return $this.Render() }
}

#
# Helper functions for creating specific element types on top of the classes.
# These are required because types aren't visible outside of the module.
#
function H1 {[Element] @{Tag = "H1"; Text = $args.foreach{$_} -join " "}}
function H2 {[Element] @{Tag = "H2"; Text = $args.foreach{$_} -join " "}}
function H3 {[Element] @{Tag = "H3"; Text = $args.foreach{$_} -join " "}}
function P  {[Element] @{Tag = "P" ; Text = $args.foreach{$_} -join " "}}
function B  {[Element] @{Tag = "B" ; Text = $args.foreach{$_} -join " "}}
function I  {[Element] @{Tag = "I" ; Text = $args.foreach{$_} -join " "}}
function HREF
{
    param (
        $Name,
        $Link
    )

    return [Element] @{
        Tag = "A"
        Attributes = @{ HREF = $link }
        Text = $name
    }
}
function Table
{
    param (
        [Parameter(Mandatory)]
        [object[]]
            $Data,
        [Parameter()]
        [string[]]
            $Properties = "*",
        [Parameter()]
        [hashtable]
            $Attributes = @{ border=2; cellpadding=2; cellspacing=2 }
    )

    $bodyText = ""
    # Add the header tags
    $bodyText +=  $Properties.foreach{TH $_}
    # Add the rows
    $bodyText += foreach ($row in $Data)
                {
                            TR (-join $Properties.Foreach{ TD ($row.$_) } )
                }

    $table = [Element] @{
                Tag = "Table"
                Attributes = $Attributes
                Text = $bodyText
            }
    $table
}
function TH  {([Element] @{Tag="TH"; Text=$args.foreach{$_} -join " "})}
function TR  {([Element] @{Tag="TR"; Text=$args.foreach{$_} -join " "})}
function TD  {([Element] @{Tag="TD"; Text=$args.foreach{$_} -join " "})}

function Style
{
    return  [Element]  @{
        Tag = "style"
        Text = "$args"
    }
}

# Takes a hash table, casts it to and HTML document
# and then returns the resulting type.
#
function Html ([HTML] $doc) { return $doc }
```

## <a name="see-also"></a>関連項目

[about_Enum](../../Microsoft.PowerShell.Core/About/about_Enum.md)

[about_Hidden](../../Microsoft.PowerShell.Core/About/about_hidden.md)

[about_Language_Keywords](../../Microsoft.PowerShell.Core/About/about_Language_Keywords.md)

[about_Methods](../../Microsoft.PowerShell.Core/About/about_Methods.md)

[カスタム PowerShell Desired State Configuration リソースのビルド](/powershell/scripting/dsc/resources/authoringResource)
