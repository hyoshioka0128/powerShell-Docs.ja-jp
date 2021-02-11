---
description: クラスを使用して独自のカスタム型を作成する方法について説明します。
Locale: en-US
ms.date: 01/19/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_classes?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Classes
ms.openlocfilehash: 7974ec49ebf27338da461cd57fb43cc0229b7323
ms.sourcegitcommit: 94d597c4fb38793bc49ca7610e2c9973b1e577c2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620049"
---
# <a name="about-classes"></a>クラスの概要

## <a name="short-description"></a>簡単な説明
クラスを使用して独自のカスタム型を作成する方法について説明します。

## <a name="long-description"></a>長い説明

PowerShell 5.0 では、クラスやその他のユーザー定義型を定義するための正式な構文が追加されます。 クラスを追加することで、開発者や IT プロフェッショナルは幅広いユースケースに対して PowerShell を採用できるようになります。 PowerShell アーティファクトの開発が簡素化され、管理サーフェイスの範囲が短縮されます。

クラス宣言は、実行時にオブジェクトのインスタンスを作成するために使用されるブループリントです。 クラスを定義すると、クラス名が型の名前になります。 たとえば **、device という名前の** クラスを宣言し、その変数を `$dev` **デバイス** の新しいインスタンスに初期化すると、 `$dev` は **デバイス** または型のインスタンスになります。 **デバイス** の各インスタンスのプロパティには、異なる値を設定できます。

## <a name="supported-scenarios"></a>サポートされるシナリオ

- クラス、プロパティ、メソッド、継承などの使い慣れたオブジェクト指向プログラミングセマンティクスを使用して、PowerShell でカスタム型を定義します。
- PowerShell 言語を使用して型をデバッグします。
- 正式なメカニズムを使用して例外を生成し、処理します。
- PowerShell 言語を使用して、DSC リソースとそれに関連付けられている型を定義します。

## <a name="syntax"></a>構文

クラスは、次の構文を使用して宣言されます。

```syntax
class <class-name> [: [<base-class>][,<interface-list]] {
    [[<attribute>] [hidden] [static] <property-definition> ...]
    [<class-name>([<constructor-argument-list>])
      {<constructor-statement-list>} ...]
    [[<attribute>] [hidden] [static] <method-definition> ...]
}
```

クラスは、次の構文のいずれかを使用してインスタンス化されます。

```syntax
[$<variable-name> =] New-Object -TypeName <class-name> [
  [-ArgumentList] <constructor-argument-list>]
```

```syntax
[$<variable-name> =] [<class-name>]::new([<constructor-argument-list>])
```

> [!NOTE]
> 構文を使用する場合 `[<class-name>]::new()` は、クラス名を囲むかっこが必須です。 角かっこは、PowerShell の型定義を通知します。

### <a name="example-syntax-and-usage"></a>構文と使用例

この例は、使用可能なクラスを作成するために必要な最低限の構文を示しています。

```powershell
class Device {
    [string]$Brand
}

$dev = [Device]::new()
$dev.Brand = "Microsoft"
$dev
```

```Output
Brand
-----
Microsoft
```

## <a name="class-properties"></a>クラスのプロパティ

プロパティは、クラススコープで宣言された変数です。 プロパティには、任意の組み込み型または別のクラスのインスタンスを指定できます。 クラスに含まれるプロパティの数に制限はありません。

### <a name="example-class-with-simple-properties"></a>単純なプロパティを持つクラスの例

```powershell
class Device {
    [string]$Brand
    [string]$Model
    [string]$VendorSku
}

$device = [Device]::new()
$device.Brand = "Microsoft"
$device.Model = "Surface Pro 4"
$device.VendorSku = "5072641000"

$device
```

```Output
Brand     Model         VendorSku
-----     -----         ---------
Microsoft Surface Pro 4 5072641000
```

### <a name="example-complex-types-in-class-properties"></a>クラスプロパティの複合型の例

この例では、**デバイス** クラスを使用して空の **ラック** クラスを定義します。 この後の例では、デバイスをラックに追加する方法と、事前に読み込まれたラックから始める方法について説明します。

```powershell
class Device {
    [string]$Brand
    [string]$Model
    [string]$VendorSku
}

class Rack {
    [string]$Brand
    [string]$Model
    [string]$VendorSku
    [string]$AssetId
    [Device[]]$Devices = [Device[]]::new(8)

}

$rack = [Rack]::new()

$rack
```

```Output

Brand     :
Model     :
VendorSku :
AssetId   :
Devices   : {$null, $null, $null, $null...}


```

## <a name="class-methods"></a>クラスのメソッド

メソッドは、クラスが実行できるアクションを定義します。 メソッドは、入力データを提供するパラメーターを受け取ることができます。 メソッドは出力を返すことができます。 メソッドによって返されるデータは、任意の定義済みデータ型にすることができます。

クラスのメソッドを定義する場合は、自動変数を使用して現在のクラスオブジェクトを参照し `$this` ます。 これにより、現在のクラスで定義されているプロパティおよびその他のメソッドにアクセスできます。

### <a name="example-simple-class-with-properties-and-methods"></a>プロパティとメソッドを持つ単純なクラスの例

**ラック** クラスを拡張して、デバイスを追加および削除します。

```powershell
class Device {
    [string]$Brand
    [string]$Model
    [string]$VendorSku

    [string]ToString(){
        return ("{0}|{1}|{2}" -f $this.Brand, $this.Model, $this.VendorSku)
    }
}

class Rack {
    [int]$Slots = 8
    [string]$Brand
    [string]$Model
    [string]$VendorSku
    [string]$AssetId
    [Device[]]$Devices = [Device[]]::new($this.Slots)

    [void] AddDevice([Device]$dev, [int]$slot){
        ## Add argument validation logic here
        $this.Devices[$slot] = $dev
    }

    [void]RemoveDevice([int]$slot){
        ## Add argument validation logic here
        $this.Devices[$slot] = $null
    }

    [int[]] GetAvailableSlots(){
        [int]$i = 0
        return @($this.Devices.foreach{ if($_ -eq $null){$i}; $i++})
    }
}

$rack = [Rack]::new()

$surface = [Device]::new()
$surface.Brand = "Microsoft"
$surface.Model = "Surface Pro 4"
$surface.VendorSku = "5072641000"

$rack.AddDevice($surface, 2)

$rack
$rack.GetAvailableSlots()
```

```Output

Slots     : 8
Brand     :
Model     :
VendorSku :
AssetId   :
Devices   : {$null, $null, Microsoft|Surface Pro 4|5072641000, $null...}

0
1
3
4
5
6
7

```

## <a name="output-in-class-methods"></a>クラスのメソッドの出力

メソッドには、戻り値の型が定義されている必要があります。 メソッドが出力を返さない場合、出力の型はになり `[void]` ます。

クラスのメソッドでは、ステートメントに示されているものを除き、パイプラインにオブジェクトが送信されることはありません `return` 。 コードからパイプラインに誤って出力されることはありません。

> [!NOTE]
> これは、すべてがパイプラインに送信される PowerShell 関数が出力を処理する方法とは基本的に異なります。

クラスメソッド内からエラーストリームに書き込まれた終了しないエラーは、渡されません。 終了エラーを表面化させるには、を使用する必要があり `throw` ます。
コマンドレットを使用して `Write-*` も、クラスメソッド内から PowerShell の出力ストリームに書き込むことができます。 ただし、メソッドがステートメントのみを使用してオブジェクトを出力するように避ける必要があり `return` ます。

### <a name="method-output"></a>メソッドの出力

この例では、ステートメントを除き、クラスメソッドからパイプラインに誤って出力されることを示してい `return` ます。

```powershell
class FunWithIntegers
{
    [int[]]$Integers = 0..10

    [int[]]GetOddIntegers(){
        return $this.Integers.Where({ ($_ % 2) })
    }

    [void] GetEvenIntegers(){
        # this following line doesn't go to the pipeline
        $this.Integers.Where({ ($_ % 2) -eq 0})
    }

    [string]SayHello(){
        # this following line doesn't go to the pipeline
        "Good Morning"

        # this line goes to the pipeline
        return "Hello World"
    }
}

$ints = [FunWithIntegers]::new()
$ints.GetOddIntegers()
$ints.GetEvenIntegers()
$ints.SayHello()
```

```Output
1
3
5
7
9
Hello World

```

## <a name="constructor"></a>コンストラクター

コンストラクターを使用すると、クラスのインスタンスを作成する時点で既定値を設定し、オブジェクトロジックを検証することができます。 コンストラクターには、クラスと同じ名前が付けられています。 コンストラクターには、新しいオブジェクトのデータメンバーを初期化するための引数が含まれている場合があります。

クラスには、0個以上のコンストラクターを定義できます。 コンストラクターが定義されていない場合、クラスには既定のパラメーターなしのコンストラクターが与えられます。 このコンストラクターは、すべてのメンバーを既定値に初期化します。 オブジェクトの型と文字列には null 値が指定されます。 コンストラクターを定義するときに、既定のパラメーターなしのコンストラクターは作成されません。 必要に応じて、パラメーターなしのコンストラクターを作成します。

### <a name="constructor-basic-syntax"></a>コンストラクターの基本構文

この例では、デバイスクラスはプロパティとコンストラクターを使用して定義されています。
このクラスを使用するには、ユーザーがコンストラクターに一覧表示されているパラメーターの値を指定する必要があります。

```powershell
class Device {
    [string]$Brand
    [string]$Model
    [string]$VendorSku

    Device(
        [string]$b,
        [string]$m,
        [string]$vsk
    ){
        $this.Brand = $b
        $this.Model = $m
        $this.VendorSku = $vsk
    }
}

[Device]$surface = [Device]::new("Microsoft", "Surface Pro 4", "5072641000")

$surface
```

```Output
Brand     Model         VendorSku
-----     -----         ---------
Microsoft Surface Pro 4 5072641000
```

### <a name="example-with-multiple-constructors"></a>複数のコンストラクターを使用した例

この例では、 **デバイス** クラスは、プロパティ、既定のコンストラクター、およびインスタンスを初期化するコンストラクターを使用して定義されています。

既定のコンストラクターは、 **ブランド** を **Undefined** に設定し、 **モデル** と **ベンダの sku** を null 値にします。

```powershell
class Device {
    [string]$Brand
    [string]$Model
    [string]$VendorSku

    Device(){
        $this.Brand = 'Undefined'
    }

    Device(
        [string]$b,
        [string]$m,
        [string]$vsk
    ){
        $this.Brand = $b
        $this.Model = $m
        $this.VendorSku = $vsk
    }
}

[Device]$somedevice = [Device]::new()
[Device]$surface = [Device]::new("Microsoft", "Surface Pro 4", "5072641000")

$somedevice
$surface
```

```Output
Brand       Model           VendorSku
-----       -----           ---------
Undefined
Microsoft   Surface Pro 4   5072641000
```

## <a name="hidden-attribute"></a>Hidden 属性

属性は、 `hidden` プロパティまたはメソッドを非表示にします。 プロパティまたはメソッドは、ユーザーが引き続きアクセスでき、オブジェクトが使用可能なすべてのスコープで使用できます。 非表示のメンバーはコマンドレットによって非表示にされ、 `Get-Member` タブ補完または IntelliSense を使用してクラス定義の外部に表示することはできません。

詳細については、「 [About_hidden](About_hidden.md)」を参照してください。

### <a name="example-using-hidden-attributes"></a>隠し属性の使用例

**ラック** オブジェクトが作成されると、デバイスのスロットの数は固定値になり、いつでも変更することはできません。 この値は作成時に認識されます。

開発者は、hidden 属性を使用して、スロットの数を非表示にし、意図しない変更がラックのサイズになるのを防ぐことができます。

```powershell
class Device {
    [string]$Brand
    [string]$Model
}

class Rack {
    [int] hidden $Slots = 8
    [string]$Brand
    [string]$Model
    [Device[]]$Devices = [Device[]]::new($this.Slots)

    Rack ([string]$b, [string]$m, [int]$capacity){
        ## argument validation here

        $this.Brand = $b
        $this.Model = $m
        $this.Slots = $capacity

        ## reset rack size to new capacity
        $this.Devices = [Device[]]::new($this.Slots)
    }
}

[Rack]$r1 = [Rack]::new("Microsoft", "Surface Pro 4", 16)

$r1
$r1.Devices.Length
$r1.Slots
```

```Output
Brand     Model         Devices
-----     -----         -------
Microsoft Surface Pro 4 {$null, $null, $null, $null...}
16
16
```

通知 **スロット** プロパティが出力に表示されません `$r1` 。 ただし、サイズはコンストラクターによって変更されました。

## <a name="static-attribute"></a>静的属性

属性は、 `static` クラスに存在し、インスタンスを必要としないプロパティまたはメソッドを定義します。

静的プロパティは、クラスのインスタンス化とは関係なく、常に使用できます。 静的プロパティは、クラスのすべてのインスタンスで共有されます。 静的メソッドは常に使用できます。 すべての静的プロパティは、セッションスパン全体に対して有効です。

### <a name="example-using-static-attributes-and-methods"></a>静的な属性とメソッドの使用例

ここでインスタンス化されたラックは、データセンターに存在するものとします。 では、コードのラックを追跡する必要があります。

```powershell
class Device {
    [string]$Brand
    [string]$Model
}

class Rack {
    hidden [int] $Slots = 8
    static [Rack[]]$InstalledRacks = @()
    [string]$Brand
    [string]$Model
    [string]$AssetId
    [Device[]]$Devices = [Device[]]::new($this.Slots)

    Rack ([string]$b, [string]$m, [string]$id, [int]$capacity){
        ## argument validation here

        $this.Brand = $b
        $this.Model = $m
        $this.AssetId = $id
        $this.Slots = $capacity

        ## reset rack size to new capacity
        $this.Devices = [Device[]]::new($this.Slots)

        ## add rack to installed racks
        [Rack]::InstalledRacks += $this
    }

    static [void]PowerOffRacks(){
        foreach ($rack in [Rack]::InstalledRacks) {
            Write-Warning ("Turning off rack: " + ($rack.AssetId))
        }
    }
}
```

### <a name="testing-static-property-and-method-exist"></a>静的なプロパティとメソッドをテストしています

```
PS> [Rack]::InstalledRacks.Length
0

PS> [Rack]::PowerOffRacks()

PS> (1..10) | ForEach-Object {
>>   [Rack]::new("Adatum Corporation", "Standard-16",
>>     $_.ToString("Std0000"), 16)
>> } > $null

PS> [Rack]::InstalledRacks.Length
10

PS> [Rack]::InstalledRacks[3]
Brand              Model       AssetId Devices
-----              -----       ------- -------
Adatum Corporation Standard-16 Std0004 {$null, $null, $null, $null...}

PS> [Rack]::PowerOffRacks()
WARNING: Turning off rack: Std0001
WARNING: Turning off rack: Std0002
WARNING: Turning off rack: Std0003
WARNING: Turning off rack: Std0004
WARNING: Turning off rack: Std0005
WARNING: Turning off rack: Std0006
WARNING: Turning off rack: Std0007
WARNING: Turning off rack: Std0008
WARNING: Turning off rack: Std0009
WARNING: Turning off rack: Std0010
```

この例を実行するたびにラック数が増加することに注意してください。

## <a name="property-validation-attributes"></a>プロパティ検証属性

検証属性を使用すると、プロパティに指定された値が定義された要件を満たしているかをテストできます。 値が割り当てられる瞬間に検証がトリガーされます。 「 [About_functions_advanced_parameters](about_functions_advanced_parameters.md)」を参照してください。

### <a name="example-using-validation-attributes"></a>検証属性を使用した例

```powershell
class Device {
    [ValidateNotNullOrEmpty()][string]$Brand
    [ValidateNotNullOrEmpty()][string]$Model
}

[Device]$dev = [Device]::new()

Write-Output "Testing dev"
$dev

$dev.Brand = ""
```

```Output
Testing dev

Brand Model
----- -----

Exception setting "Brand": "The argument is null or empty. Provide an
argument that is not null or empty, and then try the command again."
At C:\tmp\Untitled-5.ps1:11 char:1
+ $dev.Brand = ""
+ ~~~~~~~~~~~~~~~
    + CategoryInfo          : NotSpecified: (:) [], SetValueInvocationException
    + FullyQualifiedErrorId : ExceptionWhenSetting
```

## <a name="inheritance-in-powershell-classes"></a>PowerShell クラスでの継承

クラスを拡張するには、既存のクラスから派生する新しいクラスを作成します。 派生クラスは、基底クラスのプロパティを継承します。 必要に応じて、メソッドとプロパティを追加またはオーバーライドできます。

PowerShell は、多重継承をサポートしていません。 クラスは、複数のクラスから継承することはできません。 ただし、その目的のためにインターフェイスを使用することもできます。

継承の実装は演算子によって定義されます。これは、 `:` このクラスを拡張するか、これらのインターフェイスを実装することを意味します。 派生クラスは、常にクラス宣言の左端にする必要があります。

### <a name="example-using-simple-inheritance-syntax"></a>単純な継承構文を使用した例

この例は、単純な PowerShell クラスの継承構文を示しています。

```powershell
Class Derived : Base {...}
```

この例では、基底クラスの後に来るインターフェイス宣言を使用した継承を示します。

```powershell
Class Derived : Base, Interface {...}
```

### <a name="example-of-simple-inheritance-in-powershell-classes"></a>PowerShell クラスでの単純な継承の例

この例では、前の例で使用した **ラック** および **デバイス** クラスが、プロパティの繰り返しを回避し、共通のプロパティをより適切に配置し、一般的なビジネスロジックを再利用するために、より適切に定義されています。

データセンター内のほとんどのオブジェクトは会社の資産であり、資産としての追跡を開始するのに適しています。 デバイスの種類は列挙型によって定義され `DeviceType` ます。列挙の詳細については、 [about_Enum](about_Enum.md) を参照してください。

この例では、とを定義しているだけで、 `Rack` `ComputeServer` 両方の拡張機能がクラスに対して定義されてい `Device` ます。

```powershell
enum DeviceType {
    Undefined = 0
    Compute = 1
    Storage = 2
    Networking = 4
    Communications = 8
    Power = 16
    Rack = 32
}

class Asset {
    [string]$Brand
    [string]$Model
}

class Device : Asset {
    hidden [DeviceType]$devtype = [DeviceType]::Undefined
    [string]$Status

    [DeviceType] GetDeviceType(){
        return $this.devtype
    }
}

class ComputeServer : Device {
    hidden [DeviceType]$devtype = [DeviceType]::Compute
    [string]$ProcessorIdentifier
    [string]$Hostname
}

class Rack : Device {
    hidden [DeviceType]$devtype = [DeviceType]::Rack
    hidden [int]$Slots = 8

    [string]$Datacenter
    [string]$Location
    [Device[]]$Devices = [Device[]]::new($this.Slots)

    Rack (){
        ## Just create the default rack with 8 slots
    }

    Rack ([int]$s){
        ## Add argument validation logic here
        $this.Devices = [Device[]]::new($s)
    }

    [void] AddDevice([Device]$dev, [int]$slot){
        ## Add argument validation logic here
        $this.Devices[$slot] = $dev
    }

    [void] RemoveDevice([int]$slot){
        ## Add argument validation logic here
        $this.Devices[$slot] = $null
    }
}

$FirstRack = [Rack]::new(16)
$FirstRack.Status = "Operational"
$FirstRack.Datacenter = "PNW"
$FirstRack.Location = "F03R02.J10"

(0..15).ForEach({
    $ComputeServer = [ComputeServer]::new()
    $ComputeServer.Brand = "Fabrikam, Inc."       ## Inherited from Asset
    $ComputeServer.Model = "Fbk5040"              ## Inherited from Asset
    $ComputeServer.Status = "Installed"           ## Inherited from Device
    $ComputeServer.ProcessorIdentifier = "x64"    ## ComputeServer
    $ComputeServer.Hostname = ("r1s" + $_.ToString("000")) ## ComputeServer
    $FirstRack.AddDevice($ComputeServer, $_)
  })

$FirstRack
$FirstRack.Devices
```

```Output
Datacenter : PNW
Location   : F03R02.J10
Devices    : {r1s000, r1s001, r1s002, r1s003...}
Status     : Operational
Brand      :
Model      :

ProcessorIdentifier : x64
Hostname            : r1s000
Status              : Installed
Brand               : Fabrikam, Inc.
Model               : Fbk5040

ProcessorIdentifier : x64
Hostname            : r1s001
Status              : Installed
Brand               : Fabrikam, Inc.
Model               : Fbk5040

<... content truncated here for brevity ...>

ProcessorIdentifier : x64
Hostname            : r1s015
Status              : Installed
Brand               : Fabrikam, Inc.
Model               : Fbk5040
```

### <a name="calling-base-class-constructors"></a>基底クラスのコンストラクターの呼び出し

サブクラスから基底クラスのコンストラクターを呼び出すには、キーワードを追加し `base` ます。

```powershell
class Person {
    [int]$Age

    Person([int]$a)
    {
        $this.Age = $a
    }
}

class Child : Person
{
    [string]$School

    Child([int]$a, [string]$s ) : base($a) {
        $this.School = $s
    }
}

[Child]$littleone = [Child]::new(10, "Silver Fir Elementary School")

$littleone.Age
```

```Output

10
```

### <a name="invoke-base-class-methods"></a>基底クラスのメソッドの呼び出し

サブクラスの既存のメソッドをオーバーライドするには、同じ名前とシグネチャを使用してメソッドを宣言します。

```powershell
class BaseClass
{
    [int]days() {return 1}
}
class ChildClass1 : BaseClass
{
    [int]days () {return 2}
}

[ChildClass1]::new().days()
```

```Output

2
```

オーバーライドされた実装から基本クラスメソッドを呼び出すには、呼び出し時に基本クラス ([baseclass] $this) にキャストします。

```powershell
class BaseClass
{
    [int]days() {return 1}
}
class ChildClass1 : BaseClass
{
    [int]days () {return 2}
    [int]basedays() {return ([BaseClass]$this).days()}
}

[ChildClass1]::new().days()
[ChildClass1]::new().basedays()
```

```Output

2
1
```

### <a name="inheriting-from-interfaces"></a>継承 (インターフェイスから)

PowerShell クラスは、基底クラスの拡張に使用したのと同じ継承構文を使用してインターフェイスを実装できます。 インターフェイスでは複数の継承が許可されるため、インターフェイスを実装する PowerShell クラスは、型名をコロン () の後に `:` コンマ () で区切って、複数の型から継承でき `,` ます。 インターフェイスを実装する PowerShell クラスは、そのインターフェイスのすべてのメンバーを実装する必要があります。 実装インターフェイスのメンバーを省略すると、スクリプトで解析時エラーが発生します。

> [!NOTE]
> Powershell では、PowerShell スクリプトでの新しいインターフェイスの宣言は現在サポートされていません。

```powershell
class MyComparable : System.IComparable
{
    [int] CompareTo([object] $obj)
    {
        return 0;
    }
}

class MyComparableBar : bar, System.IComparable
{
    [int] CompareTo([object] $obj)
    {
        return 0;
    }
}
```

## <a name="importing-classes-from-a-powershell-module"></a>PowerShell モジュールからのクラスのインポート

`Import-Module` また、ステートメントでは、モジュール `#requires` で定義されているモジュール関数、エイリアス、および変数のみをインポートします。 クラスはインポートされません。 ステートメントは、 `using module` モジュールで定義されているクラスをインポートします。 現在のセッションでモジュールが読み込まれていない場合、 `using` ステートメントは失敗します。 ステートメントの詳細については `using` 、「 [about_Using](about_Using.md)」を参照してください。

ステートメントは、 `using module` `ModuleToProcess` スクリプトモジュールまたはバイナリモジュールのルートモジュール () からクラスをインポートします。 入れ子になったモジュールで定義されているクラスや、ドットソースのスクリプトで定義されているクラスをモジュールに常にインポートすることはできません。 モジュールの外部のユーザーが使用できるようにするクラスは、ルートモジュールで定義する必要があります。

## <a name="loading-newly-changed-code-during-development"></a>開発中に新しく変更されたコードの読み込み

スクリプトモジュールの開発時には、コードに変更を加えた後、Force パラメーターを指定してを使用して新しいバージョンのモジュールを読み込むのが一般的です `Import-Module` 。  これは、ルートモジュールの関数の変更に対してのみ機能します。 `Import-Module` では、入れ子になったモジュールは再読み込みされません。 また、更新されたクラスを読み込む方法はありません。

最新バージョンを実行していることを確認するには、コマンドレットを使用してモジュールをアンロードする必要があり `Remove-Module` ます。 `Remove-Module` ルートモジュール、すべての入れ子になったモジュール、およびモジュールで定義されているすべてのクラスを削除します。 次に、およびステートメントを使用して、モジュールとクラスを再度読み込みます `Import-Module` `using module` 。

もう1つの一般的な開発手法は、コードを別のファイルに分割することです。 別のモジュールで定義されているクラスを使用する関数が1つのファイルにある場合は、ステートメントを使用して、 `using module` 必要なクラス定義が関数に確実に含まれるようにする必要があります。

## <a name="the-psreference-type-is-not-supported-with-class-members"></a>PSReference 型はクラスメンバーではサポートされていません

`[ref]`クラスメンバーによる型キャストの使用は、暗黙的に失敗します。 パラメーターを使用する Api `[ref]` は、クラスメンバーでは使用できません。 **Psreference** クラスは、COM オブジェクトをサポートするように設計されています。 COM オブジェクトには、参照によっての値を渡す必要があるケースがあります。

型の詳細については `[ref]` 、「 [Psreference クラス](/dotnet/api/system.management.automation.psreference)」を参照してください。

## <a name="see-also"></a>関連項目

- [About_hidden](About_hidden.md)
- [about_Enum](about_Enum.md)
- [about_Language_Keywords](about_language_keywords.md)
- [about_Methods](about_methods.md)
- [about_Using](about_using.md)
