---
ms.date: 09/12/2016
ms.topic: reference
title: HelpInfo XML スキーマ
description: HelpInfo XML スキーマ
ms.openlocfilehash: 157fd9c0f47c57efbaa9b7888fa174a34ad9567d
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92662017"
---
# <a name="helpinfo-xml-schema"></a>HelpInfo XML スキーマ

このトピックでは、"HelpInfo XML files" と呼ばれる、更新可能なヘルプ情報ファイルの XML スキーマについて説明します。

## <a name="helpinfo-xml-schema"></a>HelpInfo XML スキーマ

HelpInfo XML ファイルは、次の XML スキーマに基づいています。

```xml
<?xml version="1.0" encoding="utf-8"?>
<schema targetNamespace="http://schemas.microsoft.com/powershell/help/2010/05" xmlns="http://www.w3.org/2001/XMLSchema">
  <element name="HelpInfo">
    <complexType>
      <sequence>
        <element name="HelpContentURI" type="anyURI" minOccurs="1" maxOccurs="1" />
        <element name="SupportedUICultures" minOccurs="1" maxOccurs="1">
          <complexType>
            <sequence>
              <element name="UICulture" minOccurs="1" maxOccurs="unbounded">
                <complexType>
                  <sequence>
                    <element name="UICultureName" type="language" minOccurs="1" maxOccurs="1" />
                    <element name="UICultureVersion" type="string" minOccurs="1" maxOccurs="1" />
                  </sequence>
                </complexType>
              </element>
            </sequence>
          </complexType>
        </element>
      </sequence>
    </complexType>
  </element>
</schema>
```

## <a name="helpinfo-xml-elements"></a>HelpInfo XML 要素

HelpInfo XML ファイルには、次の要素が含まれています。

- **Helpcontenturi** -モジュールのヘルプ CAB ファイルの場所の URI が含まれています。 URI は "http" または "https" で始まる必要があります。 URI にはインターネットの場所を指定する必要がありますが、CAB ファイル名を含めることはできません。 **Helpcontenturi** 値は、 **Helpinfouri** 値と同じか、または異なる場合があります。

- **SupportedUICultures** -すべての UI カルチャのモジュールのヘルプファイルを表します。 **UICulture** 要素が含まれます。各要素は、指定された UI カルチャのモジュールのヘルプファイルのセットを表します。

- **UICulture** -指定された UI カルチャに含まれるモジュールのヘルプファイルのセットを表します。 ヘルプファイルが記述されている各 UI カルチャの **UICulture** 要素を追加します。

- **UICultureName** -ヘルプファイルが記述されている UI カルチャの言語コードを格納します。

- **UICultureVersion** -"N1. に4つの部分で構成されるバージョン番号が含まれています。N2.N3.N4 "UI カルチャのヘルプ CAB ファイルのバージョンを表す形式です。 **UICultureName** で指定された UI カルチャで新しいヘルプ CAB ファイルをアップロードするたびに、このバージョン番号をインクリメントします。 この値の詳細については、「 [バージョンクラス](/dotnet/api/system.version)」を参照してください。
