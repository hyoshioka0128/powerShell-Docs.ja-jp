---
ms.date: 09/13/2016
ms.topic: reference
title: データ ストアにアクセスするためのコマンドレットを作成する
description: データ ストアにアクセスするためのコマンドレットを作成する
ms.openlocfilehash: d6ae4779a96b0789f11952a1d66bb96a394c3211
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668186"
---
# <a name="creating-a-cmdlet-to-access-a-data-store"></a>データ ストアにアクセスするためのコマンドレットを作成する

このセクションでは、Windows PowerShell プロバイダーを介して格納されたデータにアクセスするコマンドレットを作成する方法について説明します。 この種類のコマンドレットは、Windows PowerShell ランタイムの Windows PowerShell プロバイダーインフラストラクチャを使用するため、コマンドレットクラスは [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) 基底クラスから派生する必要があります。

ここで説明する Select-Str コマンドレットは、ファイルまたはオブジェクト内の文字列を検索して選択できます。 文字列を識別するために使用されるパターンは、コマンドレットのパラメーターを使用して明示的に指定することも、パラメーターを使用して暗黙的に指定することもでき `Path` `Script` ます。

コマンドレットは、 [Icontentcmdletprovider](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider)から派生した任意の Windows PowerShell プロバイダーを使用するように設計されています。 たとえば、コマンドレットでは、Windows PowerShell によって提供されるファイルシステムプロバイダーまたは変数プロバイダーを指定できます。 Windows PowerShell プロバイダーの詳細については、「 [Windows powershell プロバイダーの設計](../prog-guide/designing-your-windows-powershell-provider.md)」を参照してください。

## <a name="defining-the-cmdlet-class"></a>コマンドレットクラスの定義

コマンドレットの作成の最初の手順では、常にコマンドレットに名前を付け、コマンドレットを実装する .NET クラスを宣言します。 このコマンドレットは特定の文字列を検出するため、ここで選択した動詞名は "Select" で、 [Verbscommon](/dotnet/api/System.Management.Automation.VerbsCommon) クラスで定義されています。 名詞名 "Str" は、コマンドレットが文字列に対して動作するために使用されます。 次の宣言では、コマンドレット動詞と名詞名がコマンドレットクラスの名前に反映されていることに注意してください。 承認されたコマンドレット動詞の詳細については、「 [コマンドレットの動詞名](./approved-verbs-for-windows-powershell-commands.md)」を参照してください。

このコマンドレットの .NET クラスは、windows powershell プロバイダーのインフラストラクチャを公開するために Windows PowerShell ランタイムが必要とするサポートを提供するため、 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) 基底クラスから派生する必要があります。 このコマンドレットは、 [system.text.regularexpressions.regexoptions](/dotnet/api/System.Text.RegularExpressions.Regex)などの .NET Framework 正規表現クラスを使用することにも注意してください。

次のコードは、この Select-Str コマンドレットのクラス定義です。

```csharp
[Cmdlet(VerbsCommon.Select, "Str", DefaultParameterSetName="PatternParameterSet")]
public class SelectStringCommand : PSCmdlet
```

このコマンドレットは、 `DefaultParameterSetName` class 宣言に attribute キーワードを追加することによって、既定のパラメーターセットを定義します。 パラメーターが指定されていない場合は、既定のパラメーターセット `PatternParameterSet` が使用され `Script` ます。 このパラメーターセットの詳細については、 `Pattern` 次のセクションのおよびパラメーターの説明を参照してください `Script` 。

## <a name="defining-parameters-for-data-access"></a>データアクセスのためのパラメーターの定義

このコマンドレットは、ユーザーが格納されたデータにアクセスして確認できるようにするいくつかのパラメーターを定義します。 これらのパラメーターには、 `Path` データストアの場所を示すパラメーター、 `Pattern` 検索で使用するパターンを指定するパラメーター、および検索の実行方法をサポートするその他のいくつかのパラメーターが含まれます。

> [!NOTE]
> パラメーターの定義の基本の詳細については、「 [コマンドライン入力を処理するパラメーターの追加](./adding-parameters-that-process-command-line-input.md)」を参照してください。

### <a name="declaring-the-path-parameter"></a>Path パラメーターの宣言

データストアを検索するには、このコマンドレットで Windows PowerShell のパスを使用して、データストアにアクセスするように設計されている Windows PowerShell プロバイダーを識別する必要があります。 したがって、 `Path` プロバイダーの場所を示す文字列配列型のパラメーターを定義します。

```csharp
[Parameter(
           Position = 0,
           ParameterSetName = "ScriptParameterSet",
           Mandatory = true)]
[Parameter(
           Position = 0,
           ParameterSetName = "PatternParameterSet",
           ValueFromPipeline = true,
           Mandatory = true)]
           [Alias("PSPath")]
public string[] Path
{
  get { return paths; }
  set { paths = value; }
}
private string[] paths;
```

このパラメーターは、2つの異なるパラメーターセットに属しており、別名を持っていることに注意してください。

2[つの system.object 属性は](/dotnet/api/System.Management.Automation.ParameterAttribute)、パラメーターがとに属していることを宣言します。 `Path` `ScriptParameterSet` `PatternParameterSet` パラメーターセットの詳細については、「 [コマンドレットへのパラメーターセットの追加](./adding-parameter-sets-to-a-cmdlet.md)」を参照してください。

System.string[属性は、](/dotnet/api/System.Management.Automation.AliasAttribute) `PSPath` パラメーターの別名を宣言します。 `Path` Windows PowerShell プロバイダーにアクセスする他のコマンドレットとの一貫性を確保するために、このエイリアスを宣言することを強くお勧めします。 Windows PowerShell パスの詳細については、「 [Windows powershell の動作](/previous-versions//ms714658(v=vs.85))のしくみ」の「PowerShell パスの概念」を参照してください。

### <a name="declaring-the-pattern-parameter"></a>Pattern パラメーターの宣言

検索するパターンを指定するために、このコマンドレット `Pattern` は文字列の配列であるパラメーターを宣言します。 データストアにパターンが見つかった場合は、正の結果が返されます。 これらのパターンは、コンパイル済みの正規表現の配列、またはリテラル検索に使用されるワイルドカードパターンの配列にコンパイルできます。

```csharp
[Parameter(
           Position = 1,
           ParameterSetName = "PatternParameterSet",
           Mandatory = true)]
public string[] Pattern
{
  get { return patterns; }
  set { patterns = value; }
}
private string[] patterns;
private Regex[] regexPattern;
private WildcardPattern[] wildcardPattern;
```

このパラメーターを指定すると、コマンドレットは既定のパラメーターセットを使用し `PatternParameterSet` ます。 この場合、コマンドレットは、ここで指定されたパターンを使用して文字列を選択します。 これに対し `Script` て、パラメーターを使用して、パターンを含むスクリプトを指定することもできます。 `Script`パラメーターと `Pattern` パラメーターは2つの異なるパラメーターセットを定義するため、相互に排他的です。

### <a name="declaring-search-support-parameters"></a>検索サポートパラメーターの宣言

このコマンドレットは、コマンドレットの検索機能を変更するために使用できる次のサポートパラメーターを定義します。

パラメーターは、 `Script` コマンドレットの代替検索メカニズムを提供するために使用できるスクリプトブロックを指定します。 このスクリプトには、照合に使用されるパターンが含まれている必要があります。 [また、このオブジェクトを](/dotnet/api/System.Management.Automation.PSObject) 返します。 このパラメーターは、パラメーターセットを識別する一意のパラメーターでもあることに注意して `ScriptParameterSet` ください。 Windows PowerShell ランタイムは、このパラメーターを認識すると、パラメーターセットに属するパラメーターのみを使用し `ScriptParameterSet` ます。

```csharp
[Parameter(
           Position = 1,
           ParameterSetName = "ScriptParameterSet",
           Mandatory = true)]
public ScriptBlock Script
{
  set { script = value; }
  get { return script; }
}
ScriptBlock script;
```

パラメーターは、 `SimpleMatch` 指定されたパターンをコマンドレットが明示的に一致させるかどうかを示すスイッチパラメーターです。 ユーザーがコマンドライン () でパラメーターを指定すると、コマンド `true` レットは、指定されたパターンを使用します。 パラメーターが指定されていない場合 ( `false` )、コマンドレットは正規表現を使用します。 このパラメーターの既定値は `false` です。

```csharp
[Parameter]
public SwitchParameter SimpleMatch
{
  get { return simpleMatch; }
  set { simpleMatch = value; }
}
private bool simpleMatch;
```

パラメーターは、 `CaseSensitive` 大文字と小文字を区別する検索を実行するかどうかを示すスイッチパラメーターです。 ユーザーがコマンドライン () でパラメーターを指定すると、パターンを比較するときに、コマンドレットによって `true` 大文字と小文字がチェックされます。 パラメーターが指定されていない場合 ( `false` )、このコマンドレットでは大文字と小文字が区別されません。 たとえば、"MyFile" と "myfile" は、両方とも正のヒットとして返されます。 このパラメーターの既定値は `false` です。

```csharp
[Parameter]
public SwitchParameter CaseSensitive
{
  get { return caseSensitive; }
  set { caseSensitive = value; }
}
private bool caseSensitive;
```

`Exclude`およびパラメーターは、 `Include` 検索に明示的に除外されている項目または検索に含まれる項目を識別します。 既定では、このコマンドレットはデータストア内のすべての項目を検索します。 ただし、コマンドレットによって実行される検索を制限するために、これらのパラメーターを使用して、検索に含める項目を明示的に指定することも、省略することもできます。

```csharp
[Parameter]
public SwitchParameter CaseSensitive
{
  get { return caseSensitive; }
  set { caseSensitive = value; }
}
private bool caseSensitive;
```

```csharp
[Parameter]
[ValidateNotNullOrEmpty]
public string[] Include
{
  get
  {
    return includeStrings;
  }
  set
  {
    includeStrings = value;

    this.include = new WildcardPattern[includeStrings.Length];
    for (int i = 0; i < includeStrings.Length; i++)
    {
      this.include[i] = new WildcardPattern(includeStrings[i], WildcardOptions.IgnoreCase);
    }
  }
}

internal string[] includeStrings = null;
internal WildcardPattern[] include = null;
```

### <a name="declaring-parameter-sets"></a>パラメーターセットの宣言

このコマンドレットは `ScriptParameterSet` `PatternParameterSet` 、データアクセスで使用される2つのパラメーターセットの名前として、2つのパラメーターセット (既定) を使用します。 `PatternParameterSet` は既定のパラメーターセットであり、パラメーターが指定されている場合に使用され `Pattern` ます。 `ScriptParameterSet` は、ユーザーがパラメーターを使用して代替検索機構を指定するときに使用され `Script` ます。 パラメーターセットの詳細については、「 [コマンドレットへのパラメーターセットの追加](./adding-parameter-sets-to-a-cmdlet.md)」を参照してください。

## <a name="overriding-input-processing-methods"></a>オーバーライド (入力処理メソッドを)

コマンドレットは、 [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet) クラスの1つ以上の入力処理メソッドをオーバーライドする必要があります。 入力処理方法の詳細については、「 [最初のコマンドレットの作成](./creating-a-cmdlet-without-parameters.md)」を参照してください。

このコマンドレットは、起動時にコンパイルされた正規表現の配列を構築するために、 [システム管理](/dotnet/api/System.Management.Automation.Cmdlet.BeginProcessing) メソッドをオーバーライドします。 これにより、単純一致を使用しない検索時のパフォーマンスが向上します。

```csharp
protected override void BeginProcessing()
{
  WriteDebug("Validating patterns.");
  if (patterns != null)
  {
    foreach(string pattern in patterns)
    {
      if (pattern == null)
      ThrowTerminatingError(new ErrorRecord(
                            new ArgumentNullException(
                            "Search pattern cannot be null."),
                            "NullSearchPattern",
                            ErrorCategory.InvalidArgument,
                            pattern)
                            );
    }

    WriteVerbose("Search pattern(s) are valid.");

    // If a simple match is not specified, then
    // compile the regular expressions once.
    if (!simpleMatch)
    {
      WriteDebug("Compiling search regular expressions.");

      RegexOptions regexOptions = RegexOptions.Compiled;
      if (!caseSensitive)
         regexOptions |= RegexOptions.Compiled;
      regexPattern = new Regex[patterns.Length];

      for (int i = 0; i < patterns.Length; i++)
      {
        try
        {
          regexPattern[i] = new Regex(patterns[i], regexOptions);
        }
        catch (ArgumentException ex)
        {
          ThrowTerminatingError(new ErrorRecord(
                        ex,
                        "InvalidRegularExpression",
                        ErrorCategory.InvalidArgument,
                        patterns[i]
                     ));
        }
      } //Loop through patterns to create RegEx objects.

      WriteVerbose("Pattern(s) compiled into regular expressions.");
    }// If not a simple match.

    // If a simple match is specified, then compile the
    // wildcard patterns once.
    else
    {
      WriteDebug("Compiling search wildcards.");

      WildcardOptions wildcardOptions = WildcardOptions.Compiled;

      if (!caseSensitive)
      {
        wildcardOptions |= WildcardOptions.IgnoreCase;
      }

      wildcardPattern = new WildcardPattern[patterns.Length];
      for (int i = 0; i < patterns.Length; i++)
      {
        wildcardPattern[i] =
                     new WildcardPattern(patterns[i], wildcardOptions);
      }

      WriteVerbose("Pattern(s) compiled into wildcard expressions.");
    }// If match is a simple match.
  }// If valid patterns are available.
}// End of function BeginProcessing().
```

また、このコマンドレットは、コマンドラインでユーザーが選択した文字列を処理するため [に、system.string メソッドも](/dotnet/api/System.Management.Automation.Cmdlet.ProcessRecord) オーバーライドします。 プライベート **matchstring** メソッドを呼び出すことにより、カスタムオブジェクトの形式で文字列選択の結果を書き込みます。

```csharp
protected override void ProcessRecord()
{
  UInt64 lineNumber = 0;
  MatchInfo result;
  ArrayList nonMatches = new ArrayList();

  // Walk the list of paths and search the contents for
  // any of the specified patterns.
  foreach (string psPath in paths)
  {
    // Once the filepaths are expanded, we may have more than one
    // path, so process all referenced paths.
    foreach(PathInfo path in
            SessionState.Path.GetResolvedPSPathFromPSPath(psPath)
           )
    {
      WriteVerbose("Processing path " + path.Path);

      // Check if the path represents one of the items to be
      // excluded. If so, continue to next path.
      if (!MeetsIncludeExcludeCriteria(path.ProviderPath))
         continue;

      // Get the content reader for the item(s) at the
      // specified path.
      Collection<IContentReader> readerCollection = null;
      try
      {
        readerCollection =
                    this.InvokeProvider.Content.GetReader(path.Path);
      }
      catch (PSNotSupportedException ex)
      {
        WriteError(new ErrorRecord(ex,
                   "ContentAccessNotSupported",
                    ErrorCategory.NotImplemented,
                    path.Path)
                   );
        return;
      }

      foreach(IContentReader reader in readerCollection)
      {
        // Reset the line number for this path.
        lineNumber = 0;

        // Read in a single block (line in case of a file)
        // from the object.
        IList items = reader.Read(1);

        // Read and process one block(line) at a time until
        // no more blocks(lines) exist.
        while (items != null && items.Count == 1)
        {
          // Increment the line number each time a line is
          // processed.
          lineNumber++;

          String message = String.Format("Testing line {0} : {1}",
                                        lineNumber, items[0]);

          WriteDebug(message);

          result = SelectString(items[0]);

          if (result != null)
          {
            result.Path = path.Path;
            result.LineNumber = lineNumber;

            WriteObject(result);
          }
          else
          {
            // Add the block(line) that did not match to the
            // collection of non matches , which will be stored
            // in the SessionState variable $NonMatches
            nonMatches.Add(items[0]);
          }

          // Get the next line from the object.
          items = reader.Read(1);

        }// While loop for reading one line at a time.
      }// Foreach loop for reader collection.
    }// Foreach loop for processing referenced paths.
  }// Foreach loop for walking of path list.

  // Store the list of non-matches in the
  // session state variable $NonMatches.
  try
  {
    this.SessionState.PSVariable.Set("NonMatches", nonMatches);
  }
  catch (SessionStateUnauthorizedAccessException ex)
  {
    WriteError(new ErrorRecord(ex,
               "CannotWriteVariableNonMatches",
               ErrorCategory.InvalidOperation,
               nonMatches)
              );
  }

}// End of protected override void ProcessRecord().
```

## <a name="accessing-content"></a>コンテンツへのアクセス

コマンドレットは、Windows PowerShell パスによって示されるプロバイダーを開いて、データにアクセスできるようにする必要があります。 実行空間の [Sessionstate](/dotnet/api/System.Management.Automation.SessionState) オブジェクトは、プロバイダーへのアクセスに使用されます。また、コマンドレットの [PSCmdlet](/dotnet/api/System.Management.Automation.PSCmdlet.InvokeProvider) プロパティは、プロバイダーを開くために使用されていますが、 コンテンツへのアクセスは、開いているプロバイダーの [システムの管理](/dotnet/api/System.Management.Automation.ProviderIntrinsics) オブジェクトを取得することによって提供されます。

このサンプル Select-Str コマンドレットでは、スキャンするコンテンツを公開するために、system.string [*](/dotnet/api/System.Management.Automation.ProviderIntrinsics.Content) プロパティを使用します。 次に、必要な Windows PowerShell のパスを渡して、 [System.](/dotnet/api/System.Management.Automation.ContentCmdletProviderIntrinsics.GetReader) ...... というメソッドを呼び出すことができます。

## <a name="code-sample"></a>コード サンプル

次のコードは、この Select-Str コマンドレットのこのバージョンの実装を示しています。 このコードには、コマンドレットによって使用されるプライベートメソッドと、コマンドレットの登録に使用される Windows PowerShell スナップインコードが含まれていることに注意してください。 コマンドレットの登録の詳細については、「 [コマンドレットのビルド](#defining-the-cmdlet-class)」を参照してください。

```csharp
//
// Copyright (c) 2006 Microsoft Corporation. All rights reserved.
//
// THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF
// ANY KIND, EITHER EXPRESSED OR IMPLIED, INCLUDING BUT NOT LIMITED TO
// THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR FITNESS FOR A
// PARTICULAR PURPOSE.
//
using System;
using System.Text.RegularExpressions;
using System.Collections;
using System.Collections.ObjectModel;
using System.Management.Automation;
using System.Management.Automation.Provider;
using System.ComponentModel;

namespace Microsoft.Samples.PowerShell.Commands
{
  #region SelectStringCommand
  /// <summary>
  /// This cmdlet searches through PSObjects for particular patterns.
  /// </summary>
  /// <remarks>
  /// This cmdlet can be used to search any object, such as a file or a
  /// variable, whose provider exposes methods for reading and writing
  /// content.
  /// </remarks>
  [Cmdlet(VerbsCommon.Select, "Str", DefaultParameterSetName="PatternParameterSet")]
  public class SelectStringCommand : PSCmdlet
  {
    #region Parameters
    /// <summary>
    /// Declare a Path parameter that specifies where the data is stored.
    /// This parameter must specify a PowerShell that indicates the
    /// PowerShell provider that is used to access the objects to be
    /// searched for matching patterns. This parameter should also have
    /// a PSPath alias to provide consistency with other cmdlets that use
    /// PowerShell providers.
    /// </summary>
    /// <value>Path of the object(s) to search.</value>
    [Parameter(
               Position = 0,
               ParameterSetName = "ScriptParameterSet",
               Mandatory = true)]
    [Parameter(
               Position = 0,
               ParameterSetName = "PatternParameterSet",
               ValueFromPipeline = true,
               Mandatory = true)]
               [Alias("PSPath")]
    public string[] Path
    {
      get { return paths; }
      set { paths = value; }
    }
    private string[] paths;

    /// <summary>
    /// Declare a Pattern parameter that specifies the pattern(s)
    /// used to find matching patterns in the string representation
    /// of the objects. A positive result will be returned
    /// if any of the patterns are found in the objects.
    /// </summary>
    /// <remarks>
    /// The patterns will be compiled into an array of wildcard
    /// patterns for a simple match (literal string matching),
    /// or the patterns will be converted into an array of compiled
    /// regular expressions.
    /// </remarks>
    /// <value>Array of patterns to search.</value>
    [Parameter(
               Position = 1,
               ParameterSetName = "PatternParameterSet",
               Mandatory = true)]
    public string[] Pattern
    {
      get { return patterns; }
      set { patterns = value; }
    }
    private string[] patterns;
    private Regex[] regexPattern;
    private WildcardPattern[] wildcardPattern;

    /// <summary>
    /// Declare a Script parameter that specifies a script block
    /// that is called to perform the matching operations
    /// instead of the matching performed by the cmdlet.
    /// </summary>
    /// <value>Script block that will be called for matching</value>
    [Parameter(
               Position = 1,
               ParameterSetName = "ScriptParameterSet",
               Mandatory = true)]
    public ScriptBlock Script
    {
      set { script = value; }
      get { return script; }
    }
    ScriptBlock script;

    /// <summary>
    /// Declare a switch parameter that specifies if the pattern(s) are used
    /// literally. If not (default), searching is
    /// done using regular expressions.
    /// </summary>
    /// <value>If True, a literal pattern is used.</value>
    [Parameter]
    public SwitchParameter SimpleMatch
    {
      get { return simpleMatch; }
      set { simpleMatch = value; }
    }
    private bool simpleMatch;

    /// <summary>
    /// Declare a switch parameter that specifies if a case-sensitive
    /// search is performed.  If not (default), a case-insensitive search
    /// is performed.
    /// </summary>
    /// <value>If True, a case-sensitive search is made.</value>
    [Parameter]
    public SwitchParameter CaseSensitive
    {
      get { return caseSensitive; }
      set { caseSensitive = value; }
    }
    private bool caseSensitive;

    /// <summary>
    /// Declare an Include parameter that species which
    /// specific items are searched.  When this parameter
    /// is used, items that are not listed here are omitted
    /// from the search.
    /// </summary>
    [Parameter]
    [ValidateNotNullOrEmpty]
    public string[] Include
    {
      get
      {
        return includeStrings;
      }
      set
      {
        includeStrings = value;

        this.include = new WildcardPattern[includeStrings.Length];
        for (int i = 0; i < includeStrings.Length; i++)
        {
          this.include[i] = new WildcardPattern(includeStrings[i], WildcardOptions.IgnoreCase);
        }
      }
    }

    internal string[] includeStrings = null;
    internal WildcardPattern[] include = null;

    /// <summary>
    /// Declare an Exclude parameter that species which
    /// specific items are omitted from the search.
    /// </summary>
    ///
    [Parameter]
    [ValidateNotNullOrEmpty]
    public string[] Exclude
    {
      get
      {
        return excludeStrings;
      }
      set
      {
        excludeStrings = value;

        this.exclude = new WildcardPattern[excludeStrings.Length];
        for (int i = 0; i < excludeStrings.Length; i++)
        {
          this.exclude[i] = new WildcardPattern(excludeStrings[i], WildcardOptions.IgnoreCase);
        }
      }
    }
    internal string[] excludeStrings;
    internal WildcardPattern[] exclude;

    #endregion Parameters

    #region Overrides
    /// <summary>
    /// If regular expressions are used for pattern matching,
    /// then build an array of compiled regular expressions
    /// at startup. This increases performance during scanning
    /// operations when simple matching is not used.
    /// </summary>
    protected override void BeginProcessing()
    {
      WriteDebug("Validating patterns.");
      if (patterns != null)
      {
        foreach(string pattern in patterns)
        {
          if (pattern == null)
          ThrowTerminatingError(new ErrorRecord(
                                new ArgumentNullException(
                                "Search pattern cannot be null."),
                                "NullSearchPattern",
                                ErrorCategory.InvalidArgument,
                                pattern)
                                );
        }

        WriteVerbose("Search pattern(s) are valid.");

        // If a simple match is not specified, then
        // compile the regular expressions once.
        if (!simpleMatch)
        {
          WriteDebug("Compiling search regular expressions.");

          RegexOptions regexOptions = RegexOptions.Compiled;
          if (!caseSensitive)
             regexOptions |= RegexOptions.Compiled;
          regexPattern = new Regex[patterns.Length];

          for (int i = 0; i < patterns.Length; i++)
          {
            try
            {
              regexPattern[i] = new Regex(patterns[i], regexOptions);
            }
            catch (ArgumentException ex)
            {
              ThrowTerminatingError(new ErrorRecord(
                            ex,
                            "InvalidRegularExpression",
                            ErrorCategory.InvalidArgument,
                            patterns[i]
                         ));
            }
          } //Loop through patterns to create RegEx objects.

          WriteVerbose("Pattern(s) compiled into regular expressions.");
        }// If not a simple match.

        // If a simple match is specified, then compile the
        // wildcard patterns once.
        else
        {
          WriteDebug("Compiling search wildcards.");

          WildcardOptions wildcardOptions = WildcardOptions.Compiled;

          if (!caseSensitive)
          {
            wildcardOptions |= WildcardOptions.IgnoreCase;
          }

          wildcardPattern = new WildcardPattern[patterns.Length];
          for (int i = 0; i < patterns.Length; i++)
          {
            wildcardPattern[i] =
                         new WildcardPattern(patterns[i], wildcardOptions);
          }

          WriteVerbose("Pattern(s) compiled into wildcard expressions.");
        }// If match is a simple match.
      }// If valid patterns are available.
    }// End of function BeginProcessing().

    /// <summary>
    /// Process the input and search for the specified patterns.
    /// </summary>
    protected override void ProcessRecord()
    {
      UInt64 lineNumber = 0;
      MatchInfo result;
      ArrayList nonMatches = new ArrayList();

      // Walk the list of paths and search the contents for
      // any of the specified patterns.
      foreach (string psPath in paths)
      {
        // Once the filepaths are expanded, we may have more than one
        // path, so process all referenced paths.
        foreach(PathInfo path in
                SessionState.Path.GetResolvedPSPathFromPSPath(psPath)
               )
        {
          WriteVerbose("Processing path " + path.Path);

          // Check if the path represents one of the items to be
          // excluded. If so, continue to next path.
          if (!MeetsIncludeExcludeCriteria(path.ProviderPath))
             continue;

          // Get the content reader for the item(s) at the
          // specified path.
          Collection<IContentReader> readerCollection = null;
          try
          {
            readerCollection =
                        this.InvokeProvider.Content.GetReader(path.Path);
          }
          catch (PSNotSupportedException ex)
          {
            WriteError(new ErrorRecord(ex,
                       "ContentAccessNotSupported",
                        ErrorCategory.NotImplemented,
                        path.Path)
                       );
            return;
          }

          foreach(IContentReader reader in readerCollection)
          {
            // Reset the line number for this path.
            lineNumber = 0;

            // Read in a single block (line in case of a file)
            // from the object.
            IList items = reader.Read(1);

            // Read and process one block(line) at a time until
            // no more blocks(lines) exist.
            while (items != null && items.Count == 1)
            {
              // Increment the line number each time a line is
              // processed.
              lineNumber++;

              String message = String.Format("Testing line {0} : {1}",
                                            lineNumber, items[0]);

              WriteDebug(message);

              result = SelectString(items[0]);

              if (result != null)
              {
                result.Path = path.Path;
                result.LineNumber = lineNumber;

                WriteObject(result);
              }
              else
              {
                // Add the block(line) that did not match to the
                // collection of non matches , which will be stored
                // in the SessionState variable $NonMatches
                nonMatches.Add(items[0]);
              }

              // Get the next line from the object.
              items = reader.Read(1);

            }// While loop for reading one line at a time.
          }// Foreach loop for reader collection.
        }// Foreach loop for processing referenced paths.
      }// Foreach loop for walking of path list.

      // Store the list of non-matches in the
      // session state variable $NonMatches.
      try
      {
        this.SessionState.PSVariable.Set("NonMatches", nonMatches);
      }
      catch (SessionStateUnauthorizedAccessException ex)
      {
        WriteError(new ErrorRecord(ex,
                   "CannotWriteVariableNonMatches",
                   ErrorCategory.InvalidOperation,
                   nonMatches)
                  );
      }

    }// End of protected override void ProcessRecord().
    #endregion Overrides

    #region PrivateMethods
    /// <summary>
    /// Check for a match using the input string and the pattern(s)
    /// specified.
    /// </summary>
    /// <param name="input">The string to test.</param>
    /// <returns>MatchInfo object containing information about
    /// result of a match</returns>
    private MatchInfo SelectString(object input)
    {
      string line = null;

      try
      {
        // Convert the object to a string type
        // safely using language support methods
        line = (string)LanguagePrimitives.ConvertTo(
                                                    input,
                                                    typeof(string)
                                                    );
        line = line.Trim(' ','\t');
      }
      catch (PSInvalidCastException ex)
      {
        WriteError(new ErrorRecord(
                   ex,
                   "CannotCastObjectToString",
                   ErrorCategory.InvalidOperation,
                   input)
                   );

        return null;
      }

      MatchInfo result = null;

      // If a scriptblock has been specified, call it
      // with the path for processing.  It will return
      // one object.
      if (script != null)
      {
        WriteDebug("Executing script block.");

        Collection<PSObject> psObjects =
                             script.Invoke(
                                           line,
                                           simpleMatch,
                                           caseSensitive
                                          );

        foreach (PSObject psObject in psObjects)
        {
          if (LanguagePrimitives.IsTrue(psObject))
          {
            result = new MatchInfo();
            result.Line = line;
            result.IgnoreCase = !caseSensitive;

            break;
          } //End of If.
        } //End ForEach loop.
      } // End of If if script exists.

      // If script block exists, see if this line matches any
      // of the match patterns.
      else
      {
        int patternIndex = 0;

        while (patternIndex < patterns.Length)
        {
          if ((simpleMatch &&
              wildcardPattern[patternIndex].IsMatch(line))
              || (regexPattern != null
              && regexPattern[patternIndex].IsMatch(line))
             )
          {
            result = new MatchInfo();
            result.IgnoreCase = !caseSensitive;
            result.Line = line;
            result.Pattern = patterns[patternIndex];

            break;
          }

          patternIndex++;

        }// While loop through patterns.
      }// Else for no script block specified.

      return result;

    }// End of SelectString

    /// <summary>
    /// Check whether the supplied name meets the include/exclude criteria.
    /// That is - it's on the include list if the include list was
    /// specified, and not on the exclude list if the exclude list was specified.
    /// </summary>
    /// <param name="path">path to validate</param>
    /// <returns>True if the path is acceptable.</returns>
    private bool MeetsIncludeExcludeCriteria(string path)
    {
      bool ok = false;

      // See if the file is on the include list.
      if (this.include != null)
      {
        foreach (WildcardPattern patternItem in this.include)
        {
          if (patternItem.IsMatch(path))
          {
            ok = true;
            break;
          }
        }
      }
      else
      {
        ok = true;
      }

      if (!ok)
         return false;

      // See if the file is on the exclude list.
      if (this.exclude != null)
      {
        foreach (WildcardPattern patternItem in this.exclude)
        {
          if (patternItem.IsMatch(path))
          {
            ok = false;
            break;
          }
        }
      }

      return ok;
    } //MeetsIncludeExcludeCriteria
    #endregion Private Methods

  }// class SelectStringCommand

  #endregion SelectStringCommand

  #region MatchInfo

  /// <summary>
  /// Class representing the result of a pattern/literal match
  /// that is passed through the pipeline by the Select-Str cmdlet.
  /// </summary>
  public class MatchInfo
  {
    /// <summary>
    /// Indicates if the match was done ignoring case.
    /// </summary>
    /// <value>True if case was ignored.</value>
    public bool IgnoreCase
    {
      get { return ignoreCase; }
      set { ignoreCase = value; }
    }
    private bool ignoreCase;

    /// <summary>
    /// Specifies the number of the matching line.
    /// </summary>
    /// <value>The number of the matching line.</value>
    public UInt64 LineNumber
    {
      get { return lineNumber; }
      set { lineNumber = value; }
    }
    private UInt64 lineNumber;

    /// <summary>
    /// Specifies the text of the matching line.
    /// </summary>
    /// <value>The text of the matching line.</value>
    public string Line
    {
      get { return line; }
      set { line = value; }
    }
    private string line;

    /// <summary>
    /// Specifies the full path of the object(file) containing the
    /// matching line.
    /// </summary>
    /// <remarks>
    /// It will be "inputStream" if the object came from the input
    /// stream.
    /// </remarks>
    /// <value>The path name</value>
    public string Path
    {
      get { return path; }
      set
      {
        pathSet = true;
        path = value;
      }
    }
    private string path;
    private bool pathSet;

    /// <summary>
    /// Specifies the pattern that was used in the match.
    /// </summary>
    /// <value>The pattern string</value>
    public string Pattern
    {
      get { return pattern; }
      set { pattern = value; }
    }
    private string pattern;

    private const string MatchFormat = "{0}:{1}:{2}";

    /// <summary>
    /// Returns the string representation of this object. The format
    /// depends on whether a path has been set for this object or
    /// not.
    /// </summary>
    /// <remarks>
    /// If the path component is set, as would be the case when
    /// matching in a file, ToString() returns the path, line
    /// number and line text.  If path is not set, then just the
    /// line text is presented.
    /// </remarks>
    /// <returns>The string representation of the match object.</returns>
    public override string ToString()
    {
      if (pathSet)
         return String.Format(
         System.Threading.Thread.CurrentThread.CurrentCulture,
         MatchFormat,
         this.path,
         this.lineNumber,
         this.line
         );
      else
         return this.line;
    }
  }// End class MatchInfo

  #endregion

  #region PowerShell snap-in

  /// <summary>
  /// Create a PowerShell snap-in for the Select-Str cmdlet.
  /// </summary>
  [RunInstaller(true)]
  public class SelectStringPSSnapIn : PSSnapIn
  {
    /// <summary>
    /// Create an instance of the SelectStrPSSnapin class.
    /// </summary>
    public SelectStringPSSnapIn()
           : base()
    {
    }

    /// <summary>
    /// Specify the name of the PowerShell snap-in.
    /// </summary>
    public override string Name
    {
      get
      {
        return "SelectStrPSSnapIn";
      }
    }

    /// <summary>
    /// Specify the vendor of the PowerShell snap-in.
    /// </summary>
    public override string Vendor
    {
      get
      {
        return "Microsoft";
      }
    }

    /// <summary>
    /// Specify the localization resource information for the vendor.
    /// Use the format: SnapinName,VendorName.
    /// </summary>
    public override string VendorResource
    {
      get
      {
        return "SelectStrSnapIn,Microsoft";
      }
    }

    /// <summary>
    /// Specify the description of the PowerShell snap-in.
    /// </summary>
    public override string Description
    {
      get
        {
          return "This is a PowerShell snap-in for the Select-Str cmdlet.";
        }
    }

    /// <summary>
    /// Specify the localization resource information for the description.
    /// Use the format: SnapinName,Description.

    /// </summary>
    public override string DescriptionResource
    {
      get
      {
          return "SelectStrSnapIn,This is a PowerShell snap-in for the Select-Str cmdlet.";
      }
    }
  }
  #endregion PowerShell snap-in

} //namespace Microsoft.Samples.PowerShell.Commands;
```

## <a name="building-the-cmdlet"></a>コマンドレットのビルド

コマンドレットを実装した後、Windows powershell スナップインを使用して Windows PowerShell に登録する必要があります。 コマンドレットの登録の詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

## <a name="testing-the-cmdlet"></a>コマンドレットのテスト

コマンドレットが Windows PowerShell に登録されている場合は、コマンドラインで実行することでテストできます。 次の手順を使用して、サンプル Select-Str コマンドレットをテストできます。

1. Windows PowerShell を起動し、".NET" という式が含まれている行を検索します。 パスが複数の単語で構成されている場合にのみ、パス名を引用符で囲む必要があることに注意してください。

    ```powershell
    select-str -Path "notes" -Pattern ".NET" -SimpleMatch=$false
    ```

    次のような出力が表示されます。

    ```output
    IgnoreCase   : True
    LineNumber   : 8
    Line         : Because Windows PowerShell works directly with .NET objects, there is often a .NET object
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : .NET
    IgnoreCase   : True
    LineNumber   : 21
    Line         : You should normally define the class for a cmdlet in a .NET namespace
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : .NET
    ```

2. メモファイルで、"over" という単語が続き、その後に他のテキストが含まれている行を検索します。 パラメーターは、 `SimpleMatch` の既定値を使用し `false` ます。 `CaseSensitive`パラメーターがに設定されているため、検索では大文字と小文字が区別され `false` ません。

    ```powershell
    select-str -Path notes -Pattern "over*" -SimpleMatch -CaseSensitive:$false
    ```

    次のような出力が表示されます。

    ```output
    IgnoreCase   : True
    LineNumber   : 45
    Line         : Override StopProcessing
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : over*
    IgnoreCase   : True
    LineNumber   : 49
    Line         : overriding the StopProcessing method
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : over*
    ```

3. パターンとして正規表現を使用して、メモファイルを検索します。 コマンドレットでは、かっこで囲まれた英文字と空白文字を検索します。

    ```powershell
    select-str -Path notes -Pattern "\([A-Za-z:blank:]" -SimpleMatch:$false
    ```

    次のような出力が表示されます。

    ```output
    IgnoreCase   : True
    LineNumber   : 1
    Line         : Advisory Guidelines (Consider Following)
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : \([A-Za-z:blank:]
    IgnoreCase   : True
    LineNumber   : 53
    Line         : If your cmdlet has objects that are not disposed of (written to the pipeline)
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : \([A-Za-z:blank:]
    ```

4. "Parameter" という単語が出現する場合に、大文字と小文字を区別してメモファイルを検索します。

    ```powershell
    select-str -Path notes -Pattern Parameter -CaseSensitive
    ```

    次のような出力が表示されます。

    ```output
    IgnoreCase   : False
    LineNumber   : 6
    Line         : Support an InputObject Parameter
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : Parameter
    IgnoreCase   : False
    LineNumber   : 30
    Line         : Support Force Parameter
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\notes
    Pattern      : Parameter
    ```

5. Windows PowerShell に付属している変数プロバイダーで、0 ~ 9 の数値を持つ変数を検索します。

    ```powershell
    select-str -Path * -Pattern "[0-9]"
    ```

    次のような出力が表示されます。

    ```output
    IgnoreCase   : True
    LineNumber   : 1
    Line         : 64
    Path         : Variable:\MaximumHistoryCount
    Pattern      : [0-9]
    ```

6. スクリプトブロックを使用して、ファイル SelectStrCommandSample.cs で文字列 "Pos" を検索します。 スクリプトの **cmatch** 関数は、大文字と小文字を区別しないパターン一致を実行します。

    ```powershell
    select-str -Path "SelectStrCommandSample.cs" -Script { if ($args[0] -cmatch "Pos"){ return $true } return $false }
    ```

    次のような出力が表示されます。

    ```output
    IgnoreCase   : True
    LineNumber   : 37
    Line         :    Position = 0.
    Path         : C:\PowerShell-Progs\workspace\Samples\SelectStr\SelectStrCommandSample.cs
    Pattern      :
    ```

## <a name="see-also"></a>参照

[Windows PowerShell コマンドレットを作成する方法](/powershell/scripting/developer/cmdlet/writing-a-windows-powershell-cmdlet)

[最初のコマンドレットの作成](./creating-a-cmdlet-without-parameters.md)

[システムを変更するコマンドレットを作成する](./creating-a-cmdlet-that-modifies-the-system.md)

[Windows PowerShell プロバイダーを設計する](../prog-guide/designing-your-windows-powershell-provider.md)

[Windows PowerShell の動作](/previous-versions//ms714658(v=vs.85))

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))

[Windows PowerShell SDK](../windows-powershell-reference.md)
