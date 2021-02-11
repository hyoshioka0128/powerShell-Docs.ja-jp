---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 5/1/2019
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/rename-computer?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Rename-Computer
ms.openlocfilehash: 2fc21594a4765a0901f61dba7b7f1a79f3259886
ms.sourcegitcommit: 177ae45034b58ead716853096b2e72e4864e6df6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2020
ms.locfileid: "94346141"
---
# Rename-Computer

## 概要
コンピューターの名前を変更します。

## SYNTAX

```
Rename-Computer [-ComputerName <String>] [-PassThru] [-DomainCredential <PSCredential>]
 [-LocalCredential <PSCredential>] [-NewName] <String> [-Force] [-Restart] [-WsmanAuthentication <String>]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Rename-Computer`コマンドレットは、ローカルコンピューターまたはリモートコンピューターの名前を変更します。
各コマンドで 1 つのコンピューターの名前を変更します。

このコマンドレットは、Windows PowerShell 3.0 で導入されました。

## 例

### 例 1: ローカルコンピューターの名前を変更する

このコマンドは、ローカルコンピューターの名前をに変更し、 `Server044` 再起動して変更を有効にします。

```powershell
Rename-Computer -NewName "Server044" -DomainCredential Domain01\Admin01 -Restart
```

### 例 2: リモートコンピューターの名前を変更する

このコマンドにより、コンピューターの名前 `Srv01` がに変更さ `Server001` れます。 コンピューターは再起動されません。

**DomainCredential** パラメーターは、ドメイン内のコンピューターの名前を変更するアクセス許可を持つユーザーの資格情報を指定します。

**Force** パラメーターを指定すると、確認プロンプトは表示されません。

```powershell
Rename-Computer -ComputerName "Srv01" -NewName "Server001" -DomainCredential Domain01\Admin01 -Force
```

## PARAMETERS

### -ComputerName

指定したリモート コンピューターの名前を変更します。
既定値はローカル コンピューターです。

リモート コンピューターの NetBIOS 名、IP アドレス、または完全修飾ドメイン名を入力します。
ローカルコンピューターを指定するには、コンピューター名、ドット ( `.` )、またはを入力し `localhost` ます。

このパラメーターは、PowerShell リモート処理に依存しません。
**ComputerName** `Rename-Computer` コンピューターがリモートコマンドを実行するように構成されていない場合でも、の ComputerName パラメーターを使用できます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Local Computer
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -DomainCredential

ドメインに接続するアクセス許可を持つユーザー アカウントを指定します。
ドメインに参加しているコンピューターの名前を変更するには、明示的な資格情報が必要です。

またはなどのユーザー名を入力する `User01` `Domain01\User01` か、コマンドレットによって生成されるような **PSCredential** オブジェクトを入力し `Get-Credential` ます。

ユーザー名を入力すると、このコマンドレットによってパスワードの入力が求められます。

**ComputerName** パラメーターで指定されたコンピューターに接続するアクセス許可を持つユーザー アカウントを指定するには、 **LocalCredential** パラメーターを使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーに確認せずに、直ちにコマンドを実行します。

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

### -LocalCredential

**ComputerName** パラメーターで指定されたコンピューターに接続するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

またはなどのユーザー名を入力する `User01` `Domain01\User01` か、コマンドレットによって生成されるような **PSCredential** オブジェクトを入力し `Get-Credential` ます。

ユーザー名を入力すると、このコマンドレットによってパスワードの入力が求められます。

ドメインに接続するアクセス許可を持つユーザー アカウントを指定するには、 **DomainCredential** パラメーターを使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Current User
Accept pipeline input: False
Accept wildcard characters: False
```

### -NewName

コンピューターの新しい名前を指定します。
このパラメーターは必須です。

標準名には、文字 ( `a-z` )、( `A-Z` )、数字 ( `0-9` )、ハイフン () を含めることができ `-` ますが、スペースやピリオド () は使用できません `.` 。 名前には数字だけを使用することはできません。また、63文字以内にする必要があります。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -PassThru

コマンドの結果を返します。
それ以外の場合、このコマンドレットによる出力はありません。

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

### -Restart

このコマンドレットは、名前が変更されたコンピューターを再起動することを示します。
多くの場合、変更を有効にするには再起動が必要です。

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

### -WsmanAuthentication

このコマンドレットで WSMan プロトコルを使用するときに、ユーザー資格情報の認証に使用されるメカニズムを指定します。 このパラメーターの有効値は、次のとおりです。

- **基本**
- **CredSSP**
- **[Default]**
- **ダイジェスト**
- **Kerberos**
- **ネゴシエーション**

既定値は **Default** です。

このパラメーターの値の詳細については、「 [Authenticationmechanism 列挙型](/dotnet/api/system.management.automation.runspaces.authenticationmechanism)」を参照してください。

> [!WARNING]
> ユーザー資格情報が認証対象のリモートコンピューターに渡される Credential Security Service Provider (CredSSP) 認証は、リモートネットワーク共有へのアクセスなど、複数のリソースでの認証を必要とするコマンド向けに設計されています。
> このメカニズムを使用すると、リモート操作のセキュリティ リスクが高まります。
> リモートコンピューターが侵害された場合、そのコンピューターに渡された資格情報を使用して、ネットワークセッション > 制御できます。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Default, Basic, Negotiate, CredSSP, Digest, Kerberos

Required: False
Position: Named
Default value: None
Accept pipeline input: False
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

コマンドレットの実行時に発生する内容を示します。
このコマンドレットは実行されません。

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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](../Microsoft.PowerShell.Core/About/about_CommonParameters.md)」を参照してください。

## 入力

### なし

このコマンドレットには、値による入力を受け取るパラメーターはありません。 ただし、パイプを使用してオブジェクトの **ComputerName** および **NewName** プロパティの値をこのコマンドレットに渡すことはできます。

## 出力

### Microsoft. PowerShell. ComputerChangeInfo

**PassThru** パラメーターを指定した場合、このコマンドレットは **computerchangeinfo** オブジェクトを返します。
それ以外の場合、出力は返しません。

## 注

このコマンドレットは、Windows プラットフォームでのみ使用できます。

## 関連リンク

[Restart-Computer](Restart-Computer.md)

[Stop-Computer](Stop-Computer.md)
