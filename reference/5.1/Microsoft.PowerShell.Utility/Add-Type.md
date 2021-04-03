---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 04/02/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/add-type?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Add-Type
ms.openlocfilehash: e6fa1d63f2c748c590920db0cbfdf241e0646a73
ms.sourcegitcommit: c91f79576bc54e162bcc7adf78026417b2776687
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/03/2021
ms.locfileid: "106274257"
---
# Add-Type

## 概要
PowerShell セッションに Microsoft .NET クラスを追加します。

## SYNTAX

### FromSource (既定値)

```
Add-Type [-CodeDomProvider <CodeDomProvider>] [-CompilerParameters <CompilerParameters>]
 [-TypeDefinition] <String> [-Language <Language>] [-ReferencedAssemblies <String[]>]
 [-OutputAssembly <String>] [-OutputType <OutputAssemblyType>] [-PassThru] [-IgnoreWarnings]
 [<CommonParameters>]
```

### FromMember

```
Add-Type [-CodeDomProvider <CodeDomProvider>] [-CompilerParameters <CompilerParameters>]
 [-Name] <String> [-MemberDefinition] <String[]> [-Namespace <String>] [-UsingNamespace <String[]>]
 [-Language <Language>] [-ReferencedAssemblies <String[]>] [-OutputAssembly <String>]
 [-OutputType <OutputAssemblyType>] [-PassThru] [-IgnoreWarnings] [<CommonParameters>]
```

### FromPath

```
Add-Type [-CompilerParameters <CompilerParameters>] [-Path] <String[]>
 [-ReferencedAssemblies <String[]>] [-OutputAssembly <String>] [-OutputType <OutputAssemblyType>]
 [-PassThru] [-IgnoreWarnings] [<CommonParameters>]
```

### FromLiteralPath

```
Add-Type [-CompilerParameters <CompilerParameters>] -LiteralPath <String[]>
 [-ReferencedAssemblies <String[]>] [-OutputAssembly <String>] [-OutputType <OutputAssemblyType>]
 [-PassThru] [-IgnoreWarnings] [<CommonParameters>]
```

### FromAssemblyName

```
Add-Type -AssemblyName <String[]> [-PassThru] [-IgnoreWarnings] [<CommonParameters>]
```

## Description

`Add-Type`コマンドレットを使用すると、PowerShell セッションで Microsoft .NET Framework クラスを定義できます。
次に、コマンドレットを使用してオブジェクトをインスタンス化 `New-Object` し、.NET Framework オブジェクトを使用する場合と同じようにオブジェクトを使用できます。 `Add-Type`Powershell プロファイルにコマンドを追加すると、すべての powershell セッションでクラスが使用できるようになります。

既存のアセンブリまたはソース コード ファイルを指定して型を指定することも、インラインで、または変数に保存されたソース コードを指定することもできます。 メソッドだけを指定し、クラスを定義して生成することもでき `Add-Type` ます。 Windows では、この機能を使用して、PowerShell でアンマネージ関数に対するプラットフォーム呼び出し (P/Invoke) 呼び出しを行うことができます。 ソースコードを指定すると、は `Add-Type` 指定されたソースコードをコンパイルし、新しい .NET Framework 型を含むメモリ内アセンブリを生成します。

のパラメーターを使用して、 `Add-Type` 代替言語とコンパイラを指定できます。 C# は、既定、コンパイラオプション、アセンブリの依存関係、クラスの名前空間、型の名前、および生成されたアセンブリです。

## 例

### 例 1: セッションに .NET 型を追加する

この例では、変数に格納されているソースコードを指定することによって、 **Basictest** クラスをセッションに追加します。 **Basictest** クラスは、整数の追加、オブジェクトの作成、および整数の乗算を行うために使用されます。

```powershell
$Source = @"
public class BasicTest
{
  public static int Add(int a, int b)
    {
        return (a + b);
    }
  public int Multiply(int a, int b)
    {
    return (a * b);
    }
}
"@

Add-Type -TypeDefinition $Source
[BasicTest]::Add(4, 3)
$BasicTestObject = New-Object BasicTest
$BasicTestObject.Multiply(5, 2)
```

変数は、 `$Source` クラスのソースコードを格納します。 この型には、という静的メソッドと、と `Add` いう非静的メソッドがあり `Multiply` ます。

コマンドレットでは、 `Add-Type` クラスをセッションに追加します。 インラインソースコードを使用しているため、このコマンドは **Typedefinition** パラメーターを使用して変数内のコードを指定し `$Source` ます。

`Add` **Basictest** クラスの静的メソッドでは、2つのコロンの文字 () を使用して `::` 、クラスの静的メンバーを指定します。 整数が追加され、合計が表示されます。

`New-Object`コマンドレットは、 **basictest** クラスのインスタンスをインスタンス化します。 このメソッドは、新しいオブジェクトを変数に保存し `$BasicTestObject` ます。

`$BasicTestObject` メソッドを使用 `Multiply` します。 整数が乗算され、製品が表示されます。

### 例 2: 追加された型を確認する

この例では、コマンドレットを使用して、 `Get-Member` `Add-Type` `New-Object` **例 1** で作成したコマンドレットとコマンドレットによって作成されたオブジェクトを確認します。

```powershell
[BasicTest] | Get-Member
```

```Output
   TypeName: System.RuntimeType

Name                 MemberType Definition
----                 ---------- ----------
AsType               Method     type AsType()
Clone                Method     System.Object Clone(), System.Object ICloneable.Clone()
Equals               Method     bool Equals(System.Object obj), bool Equals(type o)
FindInterfaces       Method     type[] FindInterfaces(System.Reflection.TypeFilter filter...
...
```

```powershell
[BasicTest] | Get-Member -Static
```

```Output
  TypeName: BasicTest

Name            MemberType Definition
----            ---------- ----------
Add             Method     static int Add(int a, int b)
Equals          Method     static bool Equals(System.Object objA, System.Object objB)
new             Method     BasicTest new()
ReferenceEquals Method     static bool ReferenceEquals(System.Object objA, System.Object objB)
```

```powershell
$BasicTestObject | Get-Member
```

```Output
   TypeName: BasicTest

Name        MemberType Definition
----        ---------- ----------
Equals      Method     bool Equals(System.Object obj)
GetHashCode Method     int GetHashCode()
GetType     Method     type GetType()
Multiply    Method     int Multiply(int a, int b)
ToString    Method     string ToString()
```

この `Get-Member` コマンドレットは、セッションに追加された **basictest** クラスの型とメンバーを取得し `Add-Type` ます。 `Get-Member`このコマンドは、 **system.runtimetype** オブジェクトであることを明らかにします。これは、 **system.object** クラスから派生したものです。

`Get-Member` **Static** パラメーターは、 **basictest** クラスの静的プロパティと静的メソッドを取得します。 出力は、メソッドが含まれていることを示してい `Add` ます。

この `Get-Member` コマンドレットは、変数に格納されているオブジェクトのメンバーを取得し `$BasicTestObject` ます。
`$BasicTestObject` は、 `New-Object` コマンドレットと **basictest** クラスを使用して作成されました。 出力には、変数の値 `$BasicTestObject` が **basictest** クラスのインスタンスであり、というメンバーが含まれていることがわかり `Multiply` ます。

### 例 3: アセンブリから型を追加する

この例では、アセンブリから現在のセッションにクラスを追加し `Accessibility.dll` ます。

```powershell
$AccType = Add-Type -AssemblyName "accessib*" -PassThru
```

変数は、 `$AccType` コマンドレットを使用して作成されたオブジェクトを格納し `Add-Type` ます。 `Add-Type`**AssemblyName** パラメーターを使用して、アセンブリの名前を指定します。 アスタリスク ( `*` ) のワイルドカード文字を使用すると、名前やスペルがわからない場合でも、正しいアセンブリを取得できます。 **PassThru** パラメーターは、セッションに追加されるクラスを表すオブジェクトを生成します。

### 例 4: ネイティブ Windows Api を呼び出す

この例では、PowerShell でネイティブ Windows Api を呼び出す方法を示します。 `Add-Type` は、プラットフォーム呼び出し (P/Invoke) メカニズムを使用して、PowerShell からの関数を呼び出し `User32.dll` ます。 この例は、Windows オペレーティングシステムを実行しているコンピューターでのみ機能します。

```powershell
$Signature = @"
[DllImport("user32.dll")]public static extern bool ShowWindowAsync(IntPtr hWnd, int nCmdShow);
"@

$ShowWindowAsync = Add-Type -MemberDefinition $Signature -Name "Win32ShowWindowAsync" -Namespace Win32Functions -PassThru

# Minimize the PowerShell console

$ShowWindowAsync::ShowWindowAsync((Get-Process -Id $pid).MainWindowHandle, 2)

# Restore the PowerShell console

$ShowWindowAsync::ShowWindowAsync((Get-Process -Id $Pid).MainWindowHandle, 4)
```

変数は、 `$Signature` 関数の C# シグネチャを格納し `ShowWindowAsync` ます。 結果として得られるメソッドが PowerShell セッションで確実に表示されるようにするには、 `public` 標準署名にキーワードを追加します。 詳細については、「 [Showwindowasync 関数](/windows/win32/api/winuser/nf-winuser-showwindowasync)」を参照してください。

この `$ShowWindowAsync` 変数は、PassThru パラメーターによって作成されたオブジェクトを格納し `Add-Type` ます。
コマンドレットでは、 `Add-Type` `ShowWindowAsync` 静的メソッドとして PowerShell セッションに関数を追加します。 このコマンドでは、 **memberdefinition** パラメーターを使用して、変数に保存されているメソッド定義を指定して `$Signature` います。 このコマンドは、**Name** および **Namespace**　パラメーターを使用して、クラスの名前と名前空間を指定します。 **PassThru** パラメーターは、型を表すオブジェクトを生成します。

`ShowWindowAsync`PowerShell コンソールを最小化して復元するためのコマンドでは、新しい静的メソッドが使用されます。 このメソッドは、ウィンドウハンドルと、ウィンドウの表示方法を指定する整数の2つのパラメーターを受け取ります。

PowerShell コンソールを最小化するには、 `ShowWindowAsync` `Get-Process` コマンドレットを自動変数と共に使用して、現在の powershell セッションをホストして `$PID` いるプロセスを取得します。 次に、現在のプロセスの **MainWindowHandle** プロパティと、値を表すの値を使用し `2` `SW_MINIMIZE` ます。

ウィンドウを復元するために、は `ShowWindowAsync` 値を表すウィンドウ位置にの値を使用し `4` `SW_RESTORE` ます。

ウィンドウを最大化するには、が表すの値を使用し `3` `SW_MAXIMIZE` ます。

### 例 5: Visual Basic ファイルから型を追加する

この例では、コマンドレットを使用して、 `Add-Type` ファイルで定義されている **VBFromFile** クラスを `Hello.vb` 現在のセッションに追加します。 ファイルのテキスト `Hello.vb` がコマンドの出力に表示されます。

```powershell
Add-Type -Path "C:\PS-Test\Hello.vb"
[VBFromFile]::SayHello(", World")

# From Hello.vb

Public Class VBFromFile
  Public Shared Function SayHello(sourceName As String) As String
    Dim myValue As String = "Hello"
    return myValue + sourceName
  End Function
End Class
```

```Output
Hello, World
```

`Add-Type`**Path** パラメーターを使用してソースファイルを指定し、 `Hello.vb` ファイルで定義されている型を追加します。 `SayHello`関数は、 **VBFromFile** クラスの静的メソッドとして呼び出されます。

### 例 6: JScript.NET を使用してクラスを追加する

この例では、JScript.NET を使用して、PowerShell セッションに新しいクラス **Frectangle** を作成します。

```powershell
Add-Type @'
 class FRectangle {
   var Length : double;
   var Height : double;
   function Perimeter() : double {
       return (Length + Height) * 2; }
   function Area() : double {
       return Length * Height;  } }
'@ -Language JScript

$rect = [FRectangle]::new()
$rect
```

```Output
Length Height
------ ------
     0      0
```

### 例 7: F # コンパイラを追加する

この例では、コマンドレットを使用し `Add-Type` て、PowerShell セッションに F # コードコンパイラを追加する方法を示します。 PowerShell でこの例を実行するには、 `FSharp.Compiler.CodeDom.dll` F # 言語でインストールされたが必要です。

```powershell
Add-Type -Path "FSharp.Compiler.CodeDom.dll"
$Provider = New-Object Microsoft.FSharp.Compiler.CodeDom.FSharpCodeProvider
$FSharpCode = @"
let rec loop n =if n <= 0 then () else beginprint_endline (string_of_int n);loop (n-1)end
"@
$FSharpType = Add-Type -TypeDefinition $FSharpCode -CodeDomProvider $Provider -PassThru |
   Where-Object { $_.IsPublic }
$FSharpType::loop(4)
```

```Output
4
3
2
1
```

`Add-Type`**Path** パラメーターを使用してアセンブリを指定し、アセンブリ内の型を取得します。
`New-Object` F # コードプロバイダーのインスタンスを作成し、その結果を変数に保存し `$Provider` ます。 変数は、 `$FSharpCode` **Loop** メソッドを定義する F # コードを保存します。

変数は、 `$FSharpType` `Add-Type` で定義されているパブリック型を保存するコマンドレットの結果を格納し `$FSharpCode` ます。 **TypeDefinition** パラメーターは、型を定義するソース コードを指定します。 **CodeDomProvider** パラメーターは、ソース コード コンパイラを指定します。 **PassThru** パラメーターは `Add-Type` 、型を表す **ランタイム** オブジェクトを返すように指示します。
オブジェクトは、パイプを使用してコマンドレットに送信され `Where-Object` ます。このコマンドレットは、パブリック型のみを返します。 **Where-object** コマンドレットは、結果のパブリック型をサポートするために F # プロバイダーが非パブリック型を生成するために使用されます。

Loop メソッドは、変数に格納されている型の静的メソッドとして呼び出され `$FSharpType` ます。

## PARAMETERS

### -AssemblyName

型を含むアセンブリの名前を指定します。 `Add-Type` 指定したアセンブリから型を取得します。 このパラメーターは、アセンブリ名に基づいて型を作成する場合に必要です。

アセンブリの完全名または簡易名 (部分名とも呼ばれます) を入力します。 アセンブリ名ではワイルドカード文字を使用できます。 単純な名前または部分名を入力すると、は `Add-Type` それを完全な名前に解決し、完全な名前を使用してアセンブリを読み込みます。

このパラメーターは、パスまたはファイル名を受け入れません。 アセンブリダイナミックリンクライブラリ (DLL) ファイルへのパスを入力するには、 **path** パラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: FromAssemblyName
Aliases: AN

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -CodeDomProvider

コード ジェネレーターまたはコンパイラを指定します。 `Add-Type` 指定されたコンパイラを使用して、ソースコードをコンパイルします。 既定値は C# コンパイラです。 **Language** パラメーターを使用して指定できない言語を使用している場合は、このパラメーターを使用します。 指定する **CodeDomProvider** は、ソースコードからアセンブリを生成できる必要があります。

```yaml
Type: System.CodeDom.Compiler.CodeDomProvider
Parameter Sets: FromSource, FromMember
Aliases: Provider

Required: False
Position: Named
Default value: C# compiler
Accept pipeline input: False
Accept wildcard characters: False
```

### -CompilerParameters

ソース コード コンパイラのオプションを指定します。 これらのオプションは、リビジョンなしでコンパイラに送信されます。

このパラメーターを使用すると、コンパイラに対して、実行可能ファイルの生成、リソースの埋め込み、オプションなどのコマンドラインオプションの設定を行うように指示でき `/unsafe` ます。 このパラメーターは、 **CompilerParameters** クラス **CompilerParameters** を実装します。

**CompilerParameters** パラメーターと **referencedassemblies** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.CodeDom.Compiler.CompilerParameters
Parameter Sets: FromSource, FromMember, FromPath, FromLiteralPath
Aliases: CP

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -IgnoreWarnings

コンパイラの警告は無視します。 `Add-Type`コンパイラの警告がエラーとして処理されないようにするには、このパラメーターを使用します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -言語

ソース コードで使用される言語を指定します。 コマンドレットでは、 `Add-Type` このパラメーターの値を使用して、適切な **CodeDomProvider** を選択します。 CSharp が既定値です。 このパラメーターに指定できる値は次のとおりです。

- CSharp
- CSharpVersion2
- CSharpVersion3
- JScript
- VisualBasic

```yaml
Type: Microsoft.PowerShell.Commands.Language
Parameter Sets: FromSource, FromMember
Aliases:
Accepted values: CSharp, CSharpVersion2, CSharpVersion3, JScript, VisualBasic

Required: False
Position: Named
Default value: CSharp
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath

型を含むソース コード ファイルまたはアセンブリ DLL ファイルへのパスを指定します。 **Path** と異なり、 **LiteralPath** パラメーターの値は、型指定されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: FromLiteralPath
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -MemberDefinition

クラスの新しいプロパティまたはメソッドを指定します。 `Add-Type` プロパティまたはメソッドをサポートするために必要なテンプレートコードを生成します。

Windows では、この機能を使用して、PowerShell でアンマネージ関数に対するプラットフォーム呼び出し (P/Invoke) 呼び出しを行うことができます。

```yaml
Type: System.String[]
Parameter Sets: FromMember
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Name

作成するクラスの名前を指定します。 メンバー定義から型を生成する場合は、このパラメーターは必須です。

型名と名前空間はセッション内で一意である必要があります。 型をアンロードまたは変更することはできません。
型のコードを変更するには、名前を変更するか、新しい PowerShell セッションを開始する必要があります。
それ以外の場合、コマンドは失敗します。

```yaml
Type: System.String
Parameter Sets: FromMember
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Namespace

型の名前空間を指定します。

このパラメーターがコマンドに含まれていない場合、型は、名前空間 " **Microsoft. PowerShell** ..." に作成されます。 パラメーターが、空の文字列値または値を持つコマンドに含まれている場合 `$Null` 、型はグローバル名前空間で生成されます。

```yaml
Type: System.String
Parameter Sets: FromMember
Aliases: NS

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutputAssembly

その場所に指定した名前のアセンブリ DLL ファイルを生成します。 省略可能なパスとファイル名を入力します。 ワイルドカード文字を使用できます。 既定では、は `Add-Type` メモリ内にのみアセンブリを生成します。

```yaml
Type: System.String
Parameter Sets: FromSource, FromMember, FromPath, FromLiteralPath
Aliases: OA

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -OutputType

出力アセンブリの出力の種類を指定します。 既定では、出力の種類は指定されていません。 このパラメーターは、コマンドで出力アセンブリが指定されている場合にのみ有効です。 値の詳細については、「 [OutputAssemblyType Enumeration](/dotnet/api/microsoft.powershell.commands.outputassemblytype)」を参照してください。

このパラメーターに指定できる値は次のとおりです。

- ConsoleApplication
- ライブラリ
- 構文は

```yaml
Type: Microsoft.PowerShell.Commands.OutputAssemblyType
Parameter Sets: FromSource, FromMember, FromPath, FromLiteralPath
Aliases: OT
Accepted values: ConsoleApplication, Library, WindowsApplication

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

追加された型を表す **System.Runtime**　オブジェクトを返します。 既定では、このコマンドレットは出力を生成しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

型を含むソース コード ファイルまたはアセンブリ DLL ファイルへのパスを指定します。

ソースコードファイルを送信すると、は `Add-Type` ファイル内のコードをコンパイルし、その型のメモリ内アセンブリを作成します。 **Path** の値に指定されたファイル名拡張子によって、が使用するコンパイラが決まり `Add-Type` ます。

アセンブリファイルを送信すると、は `Add-Type` アセンブリからの型を受け取ります。 メモリ内アセンブリまたはグローバル アセンブリ キャッシュを指定するには、**AssemblyName** パラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: FromPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ReferencedAssemblies

型が依存しているアセンブリを指定します。 既定では、は `Add-Type` およびを参照し `System.dll` `System.Management.Automation.dll` ます。 既定のアセンブリに加え、このパラメーターを使用して指定されたアセンブリも参照されます。

**CompilerParameters** パラメーターと **referencedassemblies** パラメーターを同じコマンドで使用することはできません。

```yaml
Type: System.String[]
Parameter Sets: FromSource, FromMember, FromPath, FromLiteralPath
Aliases: RA

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TypeDefinition

型定義を含むソース コードを指定します。 文字列またはヒア文字列のソース コードを入力するか、ソース コードを含む変数を入力します。 ここで説明する文字列の詳細については、「 [about_Quoting_Rules](../Microsoft.PowerShell.Core/about/about_Quoting_Rules.md)」を参照してください。

型定義に、名前空間宣言を含めます。 名前空間の宣言を省略すると、型が別の型または別の型のショートカットと同名となり、意図しない上書きを引き起こす場合があります。 たとえば、 **exception** という型を定義した場合、 **system.exception のショート** カットとして **例外** を使用するスクリプトは失敗します。

```yaml
Type: System.String
Parameter Sets: FromSource
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -名前空間を介して

クラスに必要な他の名前空間を指定します。 これは、C# のキーワードとよく似て `Using` います。

既定では、は `Add-Type` **System** 名前空間を参照します。 **Memberdefinition** パラメーターが使用されている場合、は `Add-Type` 既定で **InteropServices** 名前空間も参照します。 既定の名前空間に加え、**UsingNamespace** パラメーターを使用して追加された名前空間も参照されます。

```yaml
Type: System.String[]
Parameter Sets: FromMember
Aliases: Using

Required: False
Position: Named
Default value: System namespace
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

オブジェクトをパイプラインからに送信することはできません `Add-Type` 。

## 出力

### None または system.string

**PassThru** パラメーターを使用すると、 `Add-Type` 新しい型を表す system.string オブジェクトが返され **ます。** それ以外の場合、このコマンドレットは出力を生成しません。

## 注

追加した型は、現在のセッションのみに存在します。 すべてのセッションで型を使用するには、それらを PowerShell プロファイルに追加します。 プロファイルの詳細については、「 [about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md)」を参照してください。

型名と名前空間は、セッション内で一意である必要があります。 型をアンロードまたは変更することはできません。 型のコードを変更する必要がある場合は、名前を変更するか、新しい PowerShell セッションを開始する必要があります。
それ以外の場合、コマンドは失敗します。

IronPython や J# などの一部の言語の **CodeDomProvider** クラスでは、出力は生成されません。 そのため、これらの言語で記述された型をで使用することはできません `Add-Type` 。

このコマンドレットは、Microsoft .NET Framework [CodeDomProvider クラス](/dotnet/api/system.codedom.compiler.codedomprovider)に基づいています。

## 関連リンク

[about_Profiles](../Microsoft.PowerShell.Core/About/about_profiles.md)

[about_Quoting_Rules](../Microsoft.PowerShell.Core/About/about_Quoting_Rules.md)

[Add-Member](Add-Member.md)

[New-Object](New-Object.md)

[OutputAssemblyType](/dotnet/api/microsoft.powershell.commands.outputassemblytype)

[プラットフォーム呼び出し (P/Invoke)](/dotnet/standard/native-interop/pinvoke)

[Where-Object](../Microsoft.PowerShell.Core/Where-Object.md)
