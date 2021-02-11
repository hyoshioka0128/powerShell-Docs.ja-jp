---
ms.date: 09/13/2016
ms.topic: reference
title: カスタム コントロールを作成する
description: カスタム コントロールを作成する
ms.openlocfilehash: 78d8cc2970b2b3e493bef25d78404ba1be195bb1
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92668050"
---
# <a name="creating-custom-controls"></a>カスタム コントロールを作成する

カスタムコントロールは、書式設定ファイルの最も柔軟なコンポーネントです。 データテーブルなどのデータの正式な構造を定義するテーブル、リスト、およびワイドビューとは異なり、カスタムコントロールでは、個々のデータの表示方法を定義できます。 書式設定ファイルのすべてのビューで使用できるカスタムコントロールの共通セットを定義したり、特定のビューで使用できるカスタムコントロールを定義したり、オブジェクトのグループで使用できるコントロールのセットを定義したりできます。

## <a name="custom-control-example"></a>カスタムコントロールの例

次の例は、Certificates.Format.ps1xml ファイルで定義されているカスタムコントロールを示しています。 このカスタムコントロールは、テーブルビューに表示さ [れるオブジェクトを](/dotnet/api/System.Management.Automation.Signature) 区切るために使用されます。

```xml
<Controls>
  <Control>
    <Name>SignatureTypes-GroupingFormat</Name>
    <CustomControl>
      <CustomEntries>
        <CustomEntry>
          <CustomItem>
            <Frame>
              <LeftIndent>4</LeftIndent>
              <CustomItem>
                <Text AssemblyName="System.Management.Automation" BaseName="FileSystemProviderStrings"
                  ResourceId="DirectoryDisplayGrouping"/>
                <ExpressionBinding>
                  <ScriptBlock>split-path $_.Path</ScriptBlock>
                </ExpressionBinding>
                <NewLine/>
              </CustomItem>
            </Frame>
          </CustomItem>
        </CustomEntry>
      </CustomEntries>
    </CustomControl>
  </Control>
</Controls>

```

## <a name="see-also"></a>参照

[PowerShell 書式設定ファイルを記述する](./writing-a-powershell-formatting-file.md)
