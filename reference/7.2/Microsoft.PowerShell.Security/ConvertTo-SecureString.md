---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 10/22/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: ConvertTo-SecureString
ms.openlocfilehash: 0f95f66bdd13fd57f71823f2d44a28a1323d0d15
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605172"
---
# ConvertTo-SecureString

## 概要
プレーンテキストまたは暗号化された文字列をセキュリティで保護された文字列に変換します。

## SYNTAX

### Secure (既定)

```
ConvertTo-SecureString [-String] <String> [[-SecureKey] <SecureString>] [<CommonParameters>]
```

### PlainText

```
ConvertTo-SecureString [-String] <String> [-AsPlainText] [-Force] [<CommonParameters>]
```

### [ファイル]

```
ConvertTo-SecureString [-String] <String> [-Key <Byte[]>] [<CommonParameters>]
```

## Description

`ConvertTo-SecureString`コマンドレットは、暗号化された標準文字列をセキュリティで保護された文字列に変換します。 プレーンテキストをセキュリティで保護された文字列に変換することもできます。 これは、およびと共に使用され `ConvertFrom-SecureString` `Read-Host` ます。 コマンドレットによって作成されるセキュリティで保護された文字列は、 **SecureString** 型のパラメーターを必要とするコマンドレットまたは関数で使用できます。 セキュリティで保護された文字列は、コマンドレットを使用して、暗号化された標準文字列に変換して戻すことができ `ConvertFrom-SecureString` ます。 この文字列は、後で使用できるようにファイルに格納されます。

変換される標準の文字列が指定したキーを使用して暗号化されている場合は、 `ConvertFrom-SecureString` コマンドレットの **key** または **securekey** パラメーターの値と同じキーを指定する必要があり `ConvertTo-SecureString` ます。

> [!NOTE]
> [DotNet](/dotnet/api/system.security.securestring#remarks)ごとに、SecureString の内容は Windows 以外のシステムでは暗号化されないことに注意してください。

## 例

### 例 1: セキュリティで保護された文字列を暗号化された文字列に変換する

この例では、ユーザー入力からセキュリティで保護された文字列を作成し、セキュリティで保護された文字列を暗号化された標準文字列に変換してから、暗号化された標準文字列をセキュリティで保護された文字列に戻す方法を示します。

```powershell
PS C:\> $Secure = Read-Host -AsSecureString
PS C:\> $Secure
System.Security.SecureString
PS C:\> $Encrypted = ConvertFrom-SecureString -SecureString $Secure
PS C:\> $Encrypted
01000000d08c9ddf0115d1118c7a00c04fc297eb010000001a114d45b8dd3f4aa11ad7c0abdae98000000000
02000000000003660000a8000000100000005df63cea84bfb7d70bd6842e7efa79820000000004800000a000
000010000000f10cd0f4a99a8d5814d94e0687d7430b100000008bf11f1960158405b2779613e9352c6d1400
0000e6b7bf46a9d485ff211b9b2a2df3bd6eb67aae41
PS C:\> $Secure2 = ConvertTo-SecureString -String $Encrypted
PS C:\> $Secure2
System.Security.SecureString
```

最初のコマンドは、コマンドレットの **AsSecureString** パラメーターを使用して、 `Read-Host` セキュリティで保護された文字列を作成します。 コマンドを入力すると、入力した文字がセキュリティで保護された文字列に変換され、変数に保存され `$Secure` ます。

2番目のコマンドは、変数の内容を表示し `$Secure` ます。 変数には `$Secure` セキュリティで保護された文字列が含まれているため、PowerShell では **SecureString** 型のみが表示されます。

3番目のコマンドは、コマンドレットを使用して、 `ConvertFrom-SecureString` 変数内のセキュリティで保護された文字列を `$Secure` 暗号化された標準文字列に変換します。 結果は変数に保存され `$Encrypted` ます。

4番目のコマンドは、暗号化された文字列を変数の値で表示し `$Encrypted` ます。

5番目のコマンドは、コマンドレットを使用して、 `ConvertTo-SecureString` 変数内の暗号化された標準文字列を `$Encrypted` セキュリティで保護された文字列に変換します。 結果は変数に保存され `$Secure2` ます。
6番目のコマンドは、変数の値を表示し `$Secure2` ます。 SecureString 型は、コマンドが成功したことを示します。

### 例 2: ファイル内の暗号化された文字列からセキュリティで保護された文字列を作成する

この例では、ファイルに保存されている暗号化された標準文字列からセキュリティで保護された文字列を作成する方法を示します。

```powershell
$Secure = Read-Host -AsSecureString
$Encrypted = ConvertFrom-SecureString -SecureString $Secure -Key (1..16)
$Encrypted | Set-Content Encrypted.txt
$Secure2 = Get-Content Encrypted.txt | ConvertTo-SecureString -Key (1..16)
```

最初のコマンドは、コマンドレットの **AsSecureString** パラメーターを使用して、 `Read-Host` セキュリティで保護された文字列を作成します。 コマンドを入力すると、入力した文字がセキュリティで保護された文字列に変換され、変数に保存され `$Secure` ます。

2番目のコマンドは、コマンドレットを使用して、 `ConvertFrom-SecureString` 指定されたキーを使用して、変数内のセキュリティで保護された文字列を `$Secure` 暗号化された標準文字列に変換します。 内容は変数に保存され `$Encrypted` ます。

3番目のコマンドは、パイプライン演算子 () を使用して、 `|` 変数の値を `$Encrypted` コマンドレットに送信し `Set-Content` ます。これにより、Encrypted.txt ファイルに値が保存されます。

4番目のコマンドは、コマンドレットを使用して、 `Get-Content` 暗号化された標準文字列を Encrypted.txt ファイルで取得します。 このコマンドは、パイプライン演算子を使用して暗号化された文字列をコマンドレットに送信し `ConvertTo-SecureString` 、指定されたキーを使用してセキュリティで保護された文字列に変換します。
結果は変数に保存され `$Secure2` ます。

### 例 3: プレーンテキスト文字列をセキュリティで保護された文字列に変換する

このコマンドは、プレーンテキスト文字列を `P@ssW0rD!` セキュリティで保護された文字列に変換し、その結果を変数に格納し `$Secure_String_Pwd` ます。 **Asplaintext** パラメーターを使用するには、 **Force** パラメーターもコマンドに含める必要があります。

```powershell
$Secure_String_Pwd = ConvertTo-SecureString "P@ssW0rD!" -AsPlainText -Force
```

> [!CAUTION]
> スクリプトまたはコマンドラインからプレーンテキスト文字列を使用することは避けてください。 プレーンテキストは、イベントログとコマンド履歴ログに表示されます。

## PARAMETERS

### -AsPlainText

セキュリティで保護された文字列に変換するプレーンテキスト文字列を指定します。 セキュリティで保護された文字列コマンドレットは、秘密情報テキストを保護できます。 テキストはプライバシー保護のために暗号化され、使用後にコンピューターのメモリから削除されます。 このパラメーターを使用してプレーンテキストを入力として提供すると、システムはこの方法でその入力を保護することはできません。 このパラメーターを使用するには、 **Force** パラメーターも指定する必要があります。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PlainText
Aliases:

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

**Asplaintext** パラメーターを使用することによる影響を理解し、それを使用することを確認します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: PlainText
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Key

元のセキュリティ文字列を暗号化された標準文字列に変換するために使用する暗号化キーを指定します。 有効なキーの長さは、16、24、および32バイトです。

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

元のセキュリティ文字列を暗号化された標準文字列に変換するために使用する暗号化キーを指定します。 セキュリティで保護された文字列の形式で、キーを指定する必要があります。 セキュリティで保護された文字列は、キーとして使用されるバイト配列に変換されます。 セキュリティで保護された有効なキーの長さは、8、12、16のコードポイントです。

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

### -文字列

セキュリティで保護された文字列に変換する文字列を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、標準の暗号化された文字列をにパイプすることができ `ConvertTo-SecureString` ます。

## 出力

### System.Security.SecureString

`ConvertTo-SecureString`**SecureString** オブジェクトを返します。

## 注

絵文字などの一部の文字は、それらを含む文字列内の複数のコードポイントに対応します。 これらの文字は、パスワードで使用するときに問題や誤解を招く可能性があるため、使用しないでください。

## 関連リンク

[ConvertFrom-SecureString](ConvertFrom-SecureString.md)

[Read-Host](../Microsoft.PowerShell.Utility/Read-Host.md)
