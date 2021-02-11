---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 09/19/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/test-json?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Test-Json
ms.openlocfilehash: a29eab449e567f78d1e54a6716ca91d87aa08405
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600106"
---
# Test-Json

## 概要
文字列が有効な JSON ドキュメントであるかどうかをテストします

## SYNTAX

### __AllParameterSets (既定値)
```
Test-Json [-Json] <String> [<CommonParameters>]
```

### SchemaString
```
Test-Json [-Json] <String> [[-Schema] <String>] [<CommonParameters>]
```

### SchemaFile
```
Test-Json [-Json] <String> [-SchemaFile <String>] [<CommonParameters>]
```

## Description

この `Test-Json` コマンドレットは、文字列が有効な json (JavaScript Object Notation) ドキュメントであるかどうかをテストし、必要に応じて、指定されたスキーマに対して json ドキュメントを検証することができます。

検証された文字列をコマンドレットで使用すると、json `ConvertFrom-Json` 形式の文字列を json オブジェクトに変換できます。 json オブジェクトは、PowerShell で簡単に管理することも、json 入力にアクセスする別のプログラムや web サービスに送信することもできます。

多くの Web サイトが、サーバーと Web ベースのアプリ間の通信のために、XML ではなく JSON を使用してデータをシリアル化しています。

このコマンドレットは、PowerShell 6.1 で導入されました。

## 例

### 例 1: オブジェクトが有効な JSON であるかどうかをテストする

この例では、入力文字列が有効な JSON ドキュメントであるかどうかをテストします。

```powershell
"{'name': 'Ashley', 'age': 25}" | Test-Json
```

```Output
True
```

### 例 2: 指定されたスキーマに対してオブジェクトをテストする

この例では、JSON スキーマを含む文字列を取得し、それを入力文字列と比較します。

```powershell
$schema = @'
{
  "definitions": {},
  "$schema": "http://json-schema.org/draft-07/schema#",
  "$id": "http://example.com/root.json",
  "type": "object",
  "title": "The Root Schema",
  "required": [
    "name",
    "age"
  ],
  "properties": {
    "name": {
      "$id": "#/properties/name",
      "type": "string",
      "title": "The Name Schema",
      "default": "",
      "examples": [
        "Ashley"
      ],
      "pattern": "^(.*)$"
    },
    "age": {
      "$id": "#/properties/age",
      "type": "integer",
      "title": "The Age Schema",
      "default": 0,
      "examples": [
        25
      ]
    }
  }
}
'@
"{'name': 'Ashley', 'age': '25'}" | Test-Json -Schema $schema
```

```Output
Test-Json : IntegerExpected: #/age
At line:1 char:37
+ "{'name': 'Ashley', 'age': '25'}" | Test-Json -Schema $schema
+                                     ~~~~~~~~~~~~~~~~~~~~~~~~~
+ CategoryInfo          : InvalidData: (:) [Test-Json], Exception
+ FullyQualifiedErrorId : InvalidJsonAgainstSchema,Microsoft.PowerShell.Commands.TestJsonCommand
False
```

この例では、スキーマには **age** の整数が必要ですが、テストした JSON 入力では文字列値が使用されているため、エラーが発生します。

詳細については、「 [JSON スキーマ](https://json-schema.org/)」を参照してください。

### 例 3: ファイルからスキーマに対してオブジェクトをテストする

JSON スキーマでは、キーワードを使用して定義を参照でき `$ref` ます。 は、 `$ref` 別のファイルを参照する URI に解決できます。 `SchemaFile`パラメーターは json スキーマファイルへのリテラルパスを受け入れ、そのようなスキーマに対して json ファイルを検証できるようにします。

この例では、を `schema.json` 参照するファイルがあり `definitions.json` ます。

```powershell
PS> Get-Content schema.json

{
  "description":"A person",
  "type":"object",
  "properties":{
    "name":{
      "$ref":"definitions.json#/definitions/name"
    },
    "hobbies":{
      "$ref":"definitions.json#/definitions/hobbies"
    }
  }
}

PS> Get-Content definitions.json

{
  "definitions":{
    "name":{
      "type":"string"
    },
    "hobbies":{
      "type":"array",
      "items":{
        "type":"string"
      }
    }
  }
}

'{"name": "James", "hobbies": [".NET", "Blogging"]}' | Test-Json -SchemaFile 'schema.json'
```

```Output
True
```

詳細については、「 [複合スキーマの](https://json-schema.org/understanding-json-schema/structuring.html)構成」を参照してください。

## PARAMETERS

### -Json

有効性をテストする JSON 文字列を指定します。 文字列が格納されている変数を入力するか、文字列を取得するコマンドまたは式を入力します。 パイプを使用して文字列をにパイプすることもでき `Test-Json` ます。

**Json** パラメーターが必要です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -スキーマ

JSON 入力を検証するスキーマを指定します。 渡された場合 `Test-Json` は、Json 入力が **スキーマ** パラメーターで指定された仕様に準拠しているかどうかを検証し、 `$True` 入力が指定されたスキーマに準拠している場合にのみ、を返します。

詳細については、「 [JSON スキーマ](https://json-schema.org/)」を参照してください。

```yaml
Type: System.String
Parameter Sets: SchemaString
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SchemaFile

JSON 入力の検証に使用するスキーマファイルを指定します。 使用する場合、は、 `Test-Json` `$True` JSON 入力が **schemafile** パラメーターで指定されたファイルで定義されているスキーマに準拠している場合にのみを返します。

詳細については、「 [JSON スキーマ](https://json-schema.org/)」を参照してください。

```yaml
Type: System.String
Parameter Sets: SchemaFile
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、JSON 文字列をにパイプすることができ `Test-Json` ます。

## 出力

### Boolean

## 注

`Test-Json`コマンドレットは、 [Njsonschema クラス](https://github.com/RSuter/NJsonSchema)を使用して実装されます。

## 関連リンク

[JavaScript および .NET での JavaScript Object Notation (JSON) の概要](/previous-versions/dotnet/articles/bb299886(v=msdn.10))

[追加の JSON スキーマの詳細](https://json-schema.org/)

[ConvertTo-Json](ConvertTo-Json.md)

[Invoke-WebRequest](Invoke-WebRequest.md)

[Invoke-RestMethod](Invoke-RestMethod.md)
