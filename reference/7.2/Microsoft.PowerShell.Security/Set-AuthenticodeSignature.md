---
external help file: Microsoft.PowerShell.Security.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Security
ms.date: 04/10/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.security/set-authenticodesignature?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Set-AuthenticodeSignature
ms.openlocfilehash: 831f40e9bd24cee20eeae54a41e0b022dc6300da
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601812"
---
# Set-AuthenticodeSignature

## 概要
[Authenticode](/windows-hardware/drivers/install/authenticode)署名を PowerShell スクリプトまたはその他のファイルに追加します。

## SYNTAX

### ByPath (既定値)

```
Set-AuthenticodeSignature [-Certificate] <X509Certificate2> [-IncludeChain <String>]
 [-TimestampServer <String>] [-HashAlgorithm <String>] [-Force] [-FilePath] <String[]>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByLiteralPath

```
Set-AuthenticodeSignature [-Certificate] <X509Certificate2> [-IncludeChain <String>]
 [-TimestampServer <String>] [-HashAlgorithm <String>] [-Force] -LiteralPath <String[]>
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ByContent

```
Set-AuthenticodeSignature [-Certificate] <X509Certificate2> [-IncludeChain <String>]
 [-TimestampServer <String>] [-HashAlgorithm <String>] [-Force] -SourcePathOrExtension <String[]>
 -Content <Byte[]> [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Set-AuthenticodeSignature` Subject Interface Package (SIP) をサポートする任意のファイルに Authenticode 署名を追加します。

PowerShell スクリプトファイルでは、署名は、スクリプトで実行される命令の最後を示すテキストのブロックの形式になります。 このコマンドレットが実行されるときにファイル内に署名がある場合、その署名は削除されます。

## 例

### 例 1-ローカル証明書ストアの証明書を使用してスクリプトに署名する

これらのコマンドは、PowerShell 証明書プロバイダーからコード署名証明書を取得し、それを使用して PowerShell スクリプトに署名します。

```powershell
$cert=Get-ChildItem -Path Cert:\CurrentUser\My -CodeSigningCert
Set-AuthenticodeSignature -FilePath PsTestInternet2.ps1 -Certificate $cert
```

最初のコマンドは、 `Get-ChildItem` コマンドレットと PowerShell 証明書プロバイダーを使用して、 `Cert:\CurrentUser\My` 証明書ストアのサブディレクトリにある証明書を取得します。 ドライブは、 `Cert:` 証明書プロバイダーによって公開されているドライブです。 **Codesigningcert** パラメーターは、証明書プロバイダーによってのみサポートされます。これにより、取得される証明書は、コード署名権限を持つ証明書に限定されます。 このコマンドは、変数に結果を格納し `$cert` ます。

2番目のコマンドは、 `Set-AuthenticodeSignature` コマンドレットを使用してスクリプトに署名し `PSTestInternet2.ps1` ます。 **FilePath** パラメーターを使用してスクリプトの名前を指定し、 **certificate** パラメーターを使用して、証明書が変数に格納されることを指定し `$cert` ます。

> [!NOTE]
> **Codesigningcert** パラメーターをと共に使用する `Get-ChildItem` と、コード署名権限を持ち、秘密キーを含む証明書のみが返されます。 秘密キーがない場合、証明書を署名に使用することはできません。

### 例 2-PFX ファイルの証明書を使用してスクリプトに署名する

これらのコマンドは、 `Get-PfxCertificate` コマンドレットを使用してコード署名証明書を読み込みます。 次に、それを使用して PowerShell スクリプトに署名します。

```powershell
$cert = Get-PfxCertificate -FilePath C:\Test\Mysign.pfx
Set-AuthenticodeSignature -FilePath ServerProps.ps1 -Certificate $cert
```

最初のコマンドは、 `Get-PfxCertificate` コマンドレットを使用して、C:\Test\MySign.pfx 証明書を変数に読み込み `$cert` ます。

2番目のコマンドは、を使用して `Set-AuthenticodeSignature` スクリプトに署名します。 の **FilePath** パラメーターでは、 `Set-AuthenticodeSignature` 署名されているスクリプトファイルへのパスを指定します。 **Cert** パラメーターは、 `$cert` 証明書を含む変数をに渡し `Set-AuthenticodeSignature` ます。

証明書ファイルがパスワードで保護されている場合、PowerShell はパスワードの入力を求めます。

### 例 3-ルート証明機関を含む署名を追加する

このコマンドは、信頼チェーンのルート証明機関を含むデジタル署名を追加します。これは、サードパーティのタイムスタンプ サーバーにより署名されます。

```powershell
Set-AuthenticodeSignature -FilePath c:\scripts\Remodel.ps1 -Certificate $cert -IncludeChain All -TimestampServer "http://timestamp.fabrikam.com/scripts/timstamper.dll"
```

このコマンドは、 **FilePath** パラメーターを使用して署名されるスクリプトを指定し、 **certificate** パラメーターを使用して、変数に保存されている証明書を指定し `$cert` ます。 **IncludeChain** パラメーターを使用して、ルート証明機関を含む、信頼チェーン内のすべての署名を含めます。 また、timestamp **サーバー** パラメーターを使用して、署名にタイムスタンプを追加します。
これにより、証明の有効期限が切れた場合にもスクリプトは失敗しません。

## PARAMETERS

### -証明書

スクリプトまたはファイルへの署名に使用する証明書を指定します。 証明書を表すオブジェクトを格納する変数、または証明書を取得する式を入力します。

証明書を検索するには、を使用する `Get-PfxCertificate` か、 `Get-ChildItem` 証明書ドライブのコマンドレットを使用し `Cert:` ます。 証明書が有効でない場合、または権限を持っていない場合 `code-signing` 、コマンドは失敗します。

```yaml
Type: System.Security.Cryptography.X509Certificates.X509Certificate2
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

署名するファイルへのパスを指定します。

```yaml
Type: System.String[]
Parameter Sets: ByPath
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Force

コマンドレットで読み取り専用ファイルに署名を追加できるようにします。 **Force** パラメーターを使用しても、コマンドレットはセキュリティ制限を上書きできません。

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

### -HashAlgorithm

Windows でファイルのデジタル署名の計算に使用されるハッシュ アルゴリズムを指定します。

PowerShell 3.0 の既定値は SHA256 で、これは Windows の既定のハッシュアルゴリズムです。 PowerShell 2.0 の場合、既定値は SHA1 です。 異なるハッシュ アルゴリズムで署名されたファイルは、他のシステムで認識されない可能性があります。 サポートされるアルゴリズムは、オペレーティングシステムのバージョンによって異なります。

使用可能な値の一覧については、「 [2Csystem.security.cryptography.hashalgorithmname Struct](/dotnet/api/system.security.cryptography.hashalgorithmname?view=netframework-4.7.2#properties)」を参照してください。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: SHA256
Accept pipeline input: False
Accept wildcard characters: False
```

### -IncludeChain

証明書の信頼チェーン内のどの証明書がデジタル署名に含まれているかを判断します。
**Notroot** が既定値です。

有効な値は次のとおりです。

- 署名者: 署名者の証明書のみが含まれます。
- NotRoot: ルート証明機関を除く、証明書チェーン内のすべての証明書が含まれます。
- All: 証明書チェーン内のすべての証明書が含まれます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: NotRoot
Accept pipeline input: False
Accept wildcard characters: False
```

### -タイムスタンプサーバー

指定されたタイム スタンプ サーバーを使用して、署名にタイム スタンプを追加します。 タイム スタンプ サーバーの URL を文字列として入力します。

タイム スタンプは、証明書がファイルに追加された正確な時間を表します。 タイム スタンプを使用すると、署名時に証明書が有効だったことをユーザーとプログラムで検証できるため、証明書の期限が過ぎている場合であってもスクリプトは失敗しません。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -LiteralPath

署名するファイルへのパスを指定します。 **FilePath** とは異なり、 **LiteralPath** パラメーターの値は、入力されたとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符で囲まれた文字はエスケープシーケンスとして解釈されません。

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

### -SourcePathOrExtension

デジタル署名が追加されるコンテンツのファイルまたはファイルの種類へのパス。 このパラメーターは、ファイルの内容がバイト配列として渡される **コンテンツ** で使用されます。

```yaml
Type: System.String[]
Parameter Sets: ByContent
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -コンテンツ

デジタル署名が追加されるバイト配列としてのファイルの内容。 このパラメーターは、 **Sourcepathorextension** パラメーターと共に使用する必要があります。 ファイルの内容は Unicode (16LE) 形式である必要があります。

```yaml
Type: System.Byte[]
Parameter Sets: ByContent
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

Required: False
Position: Named
Default value: False
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### System.String

パイプを使用して、ファイルパスを含む文字列をにパイプすることができ `Set-AuthenticodeSignature` ます。

## 出力

### システム... Automation. 署名

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク

[Get-AuthenticodeSignature](Get-AuthenticodeSignature.md)

[Get-ExecutionPolicy](Get-ExecutionPolicy.md)

[Get-PfxCertificate](Get-PfxCertificate.md)

[Set-ExecutionPolicy](Set-ExecutionPolicy.md)

[about_Execution_Policies](../Microsoft.PowerShell.Core/About/about_Execution_Policies.md)

[about_Signing](../Microsoft.PowerShell.Core/About/about_Signing.md)
