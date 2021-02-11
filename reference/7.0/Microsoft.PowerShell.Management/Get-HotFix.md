---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 05/20/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/get-hotfix?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-HotFix
ms.openlocfilehash: 277dd2678b54c9e708d09f6ca27d82ab9afd4c1c
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94347620"
---
# Get-HotFix

## 概要
ローカルコンピューターまたはリモートコンピューターにインストールされている修正プログラムを取得します。

## SYNTAX

### 既定値 (既定値)

```
Get-HotFix [[-Id] <String[]>] [-ComputerName <String[]>] [-Credential <PSCredential>]
[<CommonParameters>]
```

### 説明

```
Get-HotFix [-Description <String[]>] [-ComputerName <String[]>] [-Credential <PSCredential>]
[<CommonParameters>]
```

## Description

`Get-Hotfix`コマンドレットは、ローカルコンピューターまたは指定されたリモートコンピューターにインストールされている修正プログラム (更新プログラム) を取得します。 更新プログラムは、Windows Update、Microsoft Update、Windows Server Update Services、または手動でインストールすることによってインストールできます。

## 例

### 例 1: ローカルコンピューター上のすべての修正プログラムを取得する

`Get-Hotfix`コマンドレットは、ローカルコンピューターにインストールされているすべての修正プログラムを取得します。

```powershell
Get-HotFix
```

```output
Source         Description      HotFixID      InstalledBy          InstalledOn
------         -----------      --------      -----------          -----------
Server01       Update           KB4495590     NT AUTHORITY\SYSTEM  5/16/2019 00:00:00
Server01       Security Update  KB4470788     NT AUTHORITY\SYSTEM  1/22/2019 00:00:00
Server01       Update           KB4480056     NT AUTHORITY\SYSTEM  1/24/2019 00:00:00
```

### 例 2: 文字列によってフィルター処理された複数のコンピューターから修正プログラムを取得する

この `Get-Hotfix` コマンドは、パラメーターを使用して、リモートコンピューターにインストールされている修正プログラムを取得します。 結果は、指定された説明文字列によってフィルター処理されます。

```
PS> Get-HotFix -Description Security* -ComputerName Server01, Server02 -Credential Domain01\admin01
```

`Get-Hotfix`**Description** パラメーターと、アスタリスク () ワイルドカードを含む文字列 **セキュリティ** を使用して、出力をフィルター処理し `*` ます。 **ComputerName** パラメーターには、リモートコンピューター名をコンマで区切った文字列が含まれています。 **Credential** パラメーターは、リモートコンピューターにアクセスしてコマンドを実行するアクセス許可を持つユーザーアカウントを指定します。

### 例 3: 更新プログラムがインストールされているかどうかを確認し、コンピューター名をファイルに書き込む

この例のコマンドは、特定の更新プログラムがインストールされているかどうかを確認します。 更新プログラムがインストールされていない場合、コンピューター名はテキストファイルに書き込まれます。

```
PS> $A = Get-Content -Path ./Servers.txt
PS> $A | ForEach-Object { if (!(Get-HotFix -Id KB957095 -ComputerName $_))
         { Add-Content $_ -Path ./Missing-KB957095.txt }}
```

変数には、 `$A` によってテキストファイルから取得されたコンピューター名が含まれてい `Get-Content` ます。 内のオブジェクト `$A` は、パイプラインからに送信され `ForEach-Object` ます。 ステートメントでは、コマンドレットを使用して、 `if` `Get-Hotfix` **id** パラメーターと、コンピューター名ごとに特定の id 番号を指定します。 コンピューターに指定された修正プログラム Id がインストールされていない場合、 `Add-Content` コマンドレットはコンピューター名をファイルに書き込みます。

### 例 4: ローカルコンピューターで最新の修正プログラムを取得する

この例では、コンピューターにインストールされている最新の修正プログラムを取得します。

```powershell
(Get-HotFix | Sort-Object -Property InstalledOn)[-1]
```

`Get-Hotfix` オブジェクトをパイプラインからコマンドレットに送信し `Sort-Object` ます。 `Sort-Object` オブジェクトを昇順で並べ替え、 **プロパティ** パラメーターを使用して、各更新された日付 **を** 評価します。 配列表記は、 `[-1]` 最新のインストールされている修正プログラムを選択します。

## PARAMETERS

### -ComputerName

リモート コンピューターを指定します。 リモートコンピューターの NetBIOS 名、インターネットプロトコル (IP) アドレス、または完全修飾ドメイン名 (FQDN) を入力します。

**ComputerName** パラメーターが指定されていない場合、は `Get-Hotfix` ローカルコンピューター上で実行されます。

**ComputerName** パラメーターは、Windows PowerShell リモート処理に依存しません。 コンピューターがリモートコマンドを実行するように構成されていない場合は、 **ComputerName** パラメーターを使用します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: CN, __Server, IPAddress

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

コンピューターにアクセスしてコマンドを実行するアクセス許可を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -Description

`Get-HotFix`**Description** パラメーターを使用して、修正プログラムの種類を指定します。 ワイルドカードを使用できます。

```yaml
Type: System.String[]
Parameter Sets: Description
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: True
```

### -Id

`Get-HotFix`特定の修正プログラム id の結果をフィルター処理します。 ワイルドカードは受け入れられません。

```yaml
Type: System.String[]
Parameter Sets: Default
Aliases: HFID

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### 文字列型

パイプを使用して1つまたは複数のコンピューター名を修正することができます。

## 出力

### System.management.managementobject # root\CIMV2\ Win32_QuickFixEngineering

`Get-HotFix` コンピューター上の修正プログラムを表すオブジェクトを返します。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

**Win32_QuickFixEngineering** [WMI クラス](/windows/desktop/WmiSdk/retrieving-a-class)は、現在のオペレーティングシステムに適用される、通常はクイック修正エンジニアリング (QFE) の更新と呼ばれる、システム全体の小さな更新プログラムを表します。 このクラスは、コンポーネントベースのサービス (CBS) によって提供される更新プログラムのみを返します。 これらの更新プログラムは、レジストリには記載されていません。 Microsoft Windows インストーラー (MSI) または [Windows Update](https://update.microsoft.com) サイトによって提供される更新プログラムは **Win32_QuickFixEngineering** によって返されません。 詳細については、「 [Win32_QuickFixEngineering クラス](/windows/desktop/CIMWin32Prov/win32-quickfixengineering)」を参照してください。

出力は、 `Get-HotFix` オペレーティングシステムによって異なる場合があります。

## 関連リンク

[about_Arrays](../Microsoft.PowerShell.Core/About/about_Arrays.md)

[Add-Content](Add-Content.md)

[Get-Credential](../Microsoft.PowerShell.Security/Get-Credential.md)

[Win32_QuickFixEngineering クラス](/windows/desktop/CIMWin32Prov/win32-quickfixengineering)
