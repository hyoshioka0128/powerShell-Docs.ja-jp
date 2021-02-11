---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 07/27/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/convertfrom-securestring?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertFrom-SecureString
ms.openlocfilehash: 9dcc79755854cc7b9f1928cfbc5d602632eca3cc
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99602610"
---
# ConvertFrom-SecureString

## 概要
セキュリティで保護された文字列を暗号化された標準文字列に変換します。

## SYNTAX

### Secure (既定)

```
ConvertFrom-SecureString [-SecureString] <SecureString> [[-SecureKey] <SecureString>] [<CommonParameters>]
```

### AsPlainText

```
ConvertFrom-SecureString [-SecureString] <SecureString> [-AsPlainText] [<CommonParameters>]
```

### [ファイル]

```
ConvertFrom-SecureString [-SecureString] <SecureString> [-Key <Byte[]>] [<CommonParameters>]
```

## Description

**SecureString** コマンドレットは、セキュリティで保護された文字列 (**SecureString**) を暗号化された標準文字列 (**system.string**) に変換します。 セキュリティで保護された文字列とは異なり、暗号化された標準文字列は、ファイルに保存して後で使用することができます。 暗号化された標準文字列は、コマンドレットを使用して、セキュリティで保護された文字列形式に変換でき `ConvertTo-SecureString` ます。

**Key** パラメーターまたは **SecureKey** パラメーターで暗号化キーを指定すると、高度暗号化標準 (AES) という暗号化アルゴリズムが使用されます。 指定するキーの長さは、AES 暗号化アルゴリズムによってサポートされている 128、192、または 256 ビットにする必要があります。 キーを指定しない場合には、Windows データ保護 API (DPAPI) が使用されて標準文字列の表現が暗号化されます。

> [!NOTE]
> [DotNet](/dotnet/api/system.security.securestring?view=netcore-2.1#remarks)ごとに、SecureString の内容は Windows 以外のシステムでは暗号化されないことに注意してください。

## 例

### 例 1: セキュリティで保護された文字列を作成する

```powershell
$SecureString = Read-Host -AsSecureString
```

このコマンドは、コマンド プロンプトで入力した文字からセキュリティで保護された文字列を作成します。 コマンドを入力した後で、セキュリティで保護された文字列として格納する文字列を入力します。 `*`入力した各文字を表すアスタリスク () が表示されます。

### 例 2: セキュリティで保護された文字列を暗号化された標準文字列に変換する

```powershell
$StandardString = ConvertFrom-SecureString $SecureString
```

このコマンドは、変数内のセキュリティで保護された文字列 `$SecureString` を暗号化された標準文字列に変換します。 結果として生成される暗号化された標準文字列は、変数に格納され `$StandardString` ます。

### 例 3: セキュリティで保護された文字列を、192ビットキーを使用した暗号化された標準文字列に変換する

```powershell
$Key = (3,4,2,3,56,34,254,222,1,1,2,23,42,54,33,233,1,34,2,7,6,5,35,43)
$StandardString = ConvertFrom-SecureString $SecureString -Key $Key
```

これらのコマンドは、Advanced Encryption Standard (AES) アルゴリズムを使用して、変数に格納されているセキュリティで保護された文字列 `$SecureString` を、暗号化された標準文字列に192ビットキーで変換します。 結果として生成される暗号化された標準文字列は、変数に格納され `$StandardString` ます。

最初のコマンドは、キーを変数に格納し `$Key` ます。 キーは24個の10進数字の配列であり、それぞれが1つの符号なしバイトに収まるように256未満である必要があります。

10進数は1バイト (8 ビット) を表しているため、キーの合計は192ビット (8 x 24) の24桁になります。 これは、AES アルゴリズムの有効なキー長さです。

2番目のコマンドは、変数内のキーを使用して、 `$Key` セキュリティで保護された文字列を暗号化された標準文字列に変換します。

### 例 4: セキュリティで保護された文字列をプレーンテキスト文字列に直接変換する

```powershell
$secureString = ConvertTo-SecureString -String 'Example' -AsPlainText
$secureString # 'System.Security.SecureString'
ConvertFrom-SecureString -SecureString $secureString -AsPlainText # 'Example'
```

## PARAMETERS

### -AsPlainText

設定すると、 `ConvertFrom-SecureString` セキュリティで保護された文字列を出力として復号化されたプレーンテキスト文字列に変換します。

このパラメーターは、PowerShell 7.0 で追加されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: AsPlainText
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Key

暗号化キーを、バイト配列として指定します。

```yaml
Type: System.Byte[]
Parameter Sets: Open
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecureKey

暗号化キーを、セキュリティで保護された文字列として指定します。 セキュリティで保護された文字列値は、キーとして使用される前に、バイト配列に変換されます。

```yaml
Type: System.Security.SecureString
Parameter Sets: Secure
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -SecureString

暗号化された標準文字列に変換するセキュリティで保護された文字列を指定します。

```yaml
Type: System.Security.SecureString
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、、、、、、、、、、、およびの共通パラメーターをサポートしてい `-Debug` `-ErrorAction` `-ErrorVariable` `-InformationAction` `-InformationVariable` `-OutVariable` `-OutBuffer` `-PipelineVariable` `-Verbose` `-WarningAction` `-WarningVariable` ます。
詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.Security.SecureString

パイプを使用して、 **SecureString** オブジェクトを Convertfrom-csv に SecureString に送ることができます。

## 出力

### System.String

ConvertFrom-SecureString は、標準文字列オブジェクトを返します。

## 注

- コマンドプロンプトで入力した文字からセキュリティで保護された文字列を作成するには、コマンドレットの **AsSecureString** パラメーターを使用し `Read-Host` ます。
- **キーまたは** **securekey** パラメーターを使用してキーを指定する場合、キーの長さは正しいものである必要があります。 たとえば、128ビットのキーは、16進数のバイト配列として指定できます。
  同様に、192ビットと256ビットのキーは、それぞれ24および32の10進数のバイト配列に対応します。
- 絵文字などの一部の文字は、それらを含む文字列内の複数のコードポイントに対応します。 これらの文字は、パスワードで使用するときに問題や誤解を招く可能性があるため、使用しないでください。

## 関連リンク

[ConvertTo-SecureString](ConvertTo-SecureString.md)

[Read-Host](../Microsoft.PowerShell.Utility/Read-Host.md)
