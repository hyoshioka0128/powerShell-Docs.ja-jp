---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/get-pfxcertificate?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PfxCertificate
ms.openlocfilehash: baf06c34511217f23d876843007525b9ce17f35b
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99605180"
---
# Get-PfxCertificate

## 概要
コンピューター上の PFX 証明書ファイルに関する情報を取得します。

## SYNTAX

### ByPath (既定値)

```
Get-PfxCertificate [-FilePath] <String[]> [-Password <SecureString>] [-NoPromptForPassword]
 [<CommonParameters>]
```

### ByLiteralPath

```
Get-PfxCertificate -LiteralPath <String[]> [-Password <SecureString>] [-NoPromptForPassword]
 [<CommonParameters>]
```

## Description

`Get-PfxCertificate`コマンドレットは、指定された各 PFX 証明書ファイルを表すオブジェクトを取得します。
PFX ファイルには、証明書と秘密キーの両方が含まれています。

## 例

### 例 1: PFX 証明書を取得する

```powershell
Get-PfxCertificate -FilePath "C:\windows\system32\Test.pfx"
```

```output
Password: ******
Signer Certificate:      David Chew (Self Certificate)
Time Certificate:
Time Stamp:
Path:                    C:\windows\system32\zap.pfx
```

このコマンドは、システム上のテスト .pfx 証明書ファイルに関する情報を取得します。

### 例 2: リモートコンピューターから PFX 証明書を取得する

```powershell
Invoke-Command -ComputerName "Server01" -ScriptBlock {Get-PfxCertificate -FilePath "C:\Text\TestNoPassword.pfx"} -Authentication CredSSP
```

このコマンドは、Server01 リモートコンピューターから PFX 証明書ファイルを取得します。 を使用して `Invoke-Command` `Get-PfxCertificate` コマンドをリモートで実行します。

PFX 証明書ファイルがパスワードで保護されていない場合、の **Authentication** パラメーターの値は `Invoke-Command` CredSSP である必要があります。

## PARAMETERS

### -FilePath

セキュリティで保護されたファイルの PFX ファイルへの完全パスを指定します。 このパラメーターの値を指定する場合は、コマンドラインでを入力する必要はありません `-FilePath` 。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -LiteralPath

セキュリティで保護されたファイルの PFX ファイルへの完全パスです。 **FilePath** とは異なり、 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

```yaml
Type: System.String[]
Parameter Sets: ByLiteralPath
Aliases: PSPath

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -NoPromptForPassword

パスワードの入力を求めるメッセージを表示しません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Password

証明書ファイルにアクセスするために必要なパスワードを指定し `.pfx` ます。

このパラメーターは、PowerShell 6.1 で導入されました。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Security.SecureString
Parameter Sets: (All)
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

パイプを使用して、ファイルパスを含む文字列をにパイプすることができ `Get-PfxCertificate` ます。

## 出力

### System.Security.Cryptography.X509Certificates.X509Certificate2

`Get-PfxCertificate` 取得した証明書ごとにオブジェクトを返します。

## 注

コマンドレットを使用して `Invoke-Command` コマンドをリモートで実行 `Get-PfxCertificate` し、PFX 証明書ファイルがパスワードで保護されていない場合、の **Authentication** パラメーターの値は `Invoke-Command` CredSSP である必要があります。

## 関連リンク

[Get-AuthenticodeSignature](Get-AuthenticodeSignature.md)

[Set-AuthenticodeSignature](Set-AuthenticodeSignature.md)

