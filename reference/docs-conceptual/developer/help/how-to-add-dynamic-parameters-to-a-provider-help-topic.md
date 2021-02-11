---
ms.date: 09/13/2016
ms.topic: reference
title: プロバイダーのヘルプ トピックに動的パラメーターを追加する方法
description: プロバイダーのヘルプ トピックに動的パラメーターを追加する方法
ms.openlocfilehash: 9542538cfacf5fb293ca8d1350b80fb250c71ac6
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92649645"
---
# <a name="how-to-add-dynamic-parameters-to-a-provider-help-topic"></a>プロバイダーのヘルプ トピックに動的パラメーターを追加する方法

このセクションでは、プロバイダーヘルプトピックの **動的パラメーター** セクションにデータを設定する方法について説明します。

*動的パラメーター* とは、指定された条件下でのみ使用可能なコマンドレットまたは関数のパラメーターです。

プロバイダーのヘルプトピックに記載されている動的パラメーターは、プロバイダーのドライブでコマンドレットまたは関数が使用されている場合に、プロバイダーがコマンドレットまたは関数に追加する動的パラメーターです。

動的パラメーターは、プロバイダーのカスタムコマンドレットのヘルプにも記載されています。 プロバイダーのヘルプとカスタムコマンドレットのヘルプの両方を作成するときは、両方のドキュメントに動的パラメーターのドキュメントを含めてください。

プロバイダーが動的パラメーターを実装していない場合、プロバイダーヘルプトピックには空の要素が含まれ `DynamicParameters` ます。

### <a name="to-add-dynamic-parameters"></a>動的パラメーターを追加するには

1. ファイルの `<AssemblyName>.dll-help.xml` 要素内に `providerHelp` 要素を追加 `DynamicParameters` します。 要素は、要素の `DynamicParameters` 後、要素の前に記述する必要があり `Tasks` `RelatedLinks` ます。

   次に例を示します。

    ```xml
    <providerHelp>
        <Tasks>
        </Tasks>
        <DynamicParameters>
        </DynamicParameters>
        <RelatedLinks>
        </RelatedLinks>
    </providerHelp>
    ```

   プロバイダーが動的パラメーターを実装していない場合は、 `DynamicParameters` 要素を空にすることができます。

1. 要素内で `DynamicParameters` 、各動的パラメーターに要素を追加し `DynamicParameter` ます。

   次に例を示します。

    ```xml
    <DynamicParameters/>
        <DynamicParameter>
        </DynamicParameter>
    </DynamicParameters>
    ```

1. 各要素に、 `DynamicParameter` `Name` 要素と要素を追加し `CmdletSupported` ます。

   - 名前-パラメーター名を指定します。
   - コマンドレットサポート-パラメーターが有効なコマンドレットを指定します。 コマンドレット名のコンマ区切りリストを入力します。

   たとえば、次の XML ドキュメントでは、 `Encoding` Windows PowerShell FileSystem プロバイダーによって、、コマンドレットに追加される動的パラメーターについて説明して `Add-Content` `Get-Content` `Set-Content` います。

    ```xml
    <DynamicParameters/>
        <DynamicParameter>
            <Name> Encoding </Name>
            <CmdletSupported> Add-Content, Get-Content, Set-Content </CmdletSupported>
    </DynamicParameters>

    ```

1. 各 `DynamicParameter` 要素に要素を追加 `Type` します。 要素は、 `Type` `Name` 動的パラメーターの値の .net 型を含む要素のコンテナーです。

   たとえば、次の XML は、動的パラメーターの .NET 型 `Encoding` が [Filesystemcmdletproviderencoding](/dotnet/api/microsoft.powershell.commands.filesystemcmdletproviderencoding) 列挙体であることを示しています。

    ```xml
    <DynamicParameters/>
        <DynamicParameter>
            <Name> Encoding </Name>
            <CmdletSupported> Add-Content, Get-Content, Set-Content </CmdletSupported>
            <Type>
                <Name> Microsoft.PowerShell.Commands.FileSystemCmdletProviderEncoding </Name>
            <Type>
    ...
    </DynamicParameters>
    ```

1. `Description`要素を追加します。これには、動的パラメーターの簡単な説明が含まれています。 説明を作成する場合は、「 [パラメーター情報を追加する方法](./how-to-add-parameter-information.md)」のすべてのコマンドレットパラメーターに指定されているガイドラインを使用します。

   たとえば、次の XML には、動的パラメーターの説明が含まれてい `Encoding` ます。

    ```xml
    <DynamicParameters/>
        <DynamicParameter>
            <Name> Encoding </Name>
            <CmdletSupported> Add-Content, Get-Content, Set-Content </CmdletSupported>
            <Type>
                <Name> Microsoft.PowerShell.Commands.FileSystemCmdletProviderEncoding </Name>
            <Type>
            <Description> Specifies the encoding of the output file that contains the content. </Description>
    ...
    </DynamicParameters>
    ```

1. `PossibleValues`要素とその子要素を追加します。 これらの要素には、動的パラメーターの値が記述されています。 この要素は、列挙値用に設計されています。 スイッチパラメーターを使用した場合など、動的パラメーターに値がない場合、または値を列挙できない場合は、空の要素を追加し `PossibleValues` ます。

   次の表に、 `PossibleValues` 要素とその子要素の一覧とその説明を示します。

   - 指定された値-この要素はコンテナーです。 その子要素について以下に説明します。 `PossibleValues`各プロバイダーヘルプトピックに1つの要素を追加します。 要素は空にすることができます。
   - 指定された値-この要素はコンテナーです。 その子要素について以下に説明します。 `PossibleValue`動的パラメーターの値ごとに1つの要素を追加します。
   - 値-値の名前を指定します。
   - 説明-この要素には要素が含まれてい `Para` ます。 要素内のテキストは、 `Para` 要素で指定されている値を表し `Value` ます。

   たとえば、次の XML は、 `PossibleValue` 動的パラメーターの1つの要素を示して `Encoding` います。

    ```xml
    <DynamicParameters/>
        <DynamicParameter>
    ...
            <Description> Specifies the encoding of the output file that contains the content. </Description>
            <PossibleValues>
                <PossibleValue>
                    <Value> ASCII </Value>
                    <Description>
                        <para> Uses the encoding for the ASCII (7-bit) character set. </para>
                    </Description>
                </PossibleValue>
    ...
            </PossibleValues>
    </DynamicParameters>
    ```

## <a name="example"></a>例

次の例は、 `DynamicParameters` 動的パラメーターの要素を示して `Encoding` います。

```xml
<DynamicParameters/>
    <DynamicParameter>
        <Name> Encoding </Name>
        <CmdletSupported> Add-Content, Get-Content, Set-Content </CmdletSupported>
        <Type>
            <Name> Microsoft.PowerShell.Commands.FileSystemCmdletProviderEncoding </Name>
        <Type>
        <Description> Specifies the encoding of the output file that contains the content. </Description>
        <PossibleValues>
            <PossibleValue>
                <Value> ASCII </Value>
                <Description>
                    <para> Uses the encoding for the ASCII (7-bit) character set. </para>
                </Description>
            </PossibleValue>
            <PossibleValue>
                <Value> Unicode </Value>
                <Description>
                    <para> Encodes in UTF-16 format using the little-endian byte order. </para>
                </Description>
            </PossibleValue>
        </PossibleValues>
</DynamicParameters>
```
