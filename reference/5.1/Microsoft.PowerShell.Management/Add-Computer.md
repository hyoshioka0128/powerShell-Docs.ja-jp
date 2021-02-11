---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/add-computer?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Add-Computer
ms.openlocfilehash: e3d1c5c071a334bddbfbc547ef2cc07e9e5c90aa
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388350"
---
# Add-Computer

## 概要
ローカル コンピューターをドメインまたはワークグループに追加します。

## SYNTAX

### ドメイン (既定値)

```
Add-Computer [-ComputerName <String[]>] [-LocalCredential <PSCredential>]
 [-UnjoinDomainCredential <PSCredential>] -Credential <PSCredential> [-DomainName] <String> [-OUPath <String>]
 [-Server <String>] [-Unsecure] [-Options <JoinOptions>] [-Restart] [-PassThru] [-NewName <String>] [-Force]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### ワークグループ

```
Add-Computer [-ComputerName <String[]>] [-LocalCredential <PSCredential>] [-Credential <PSCredential>]
 [-WorkgroupName] <String> [-Restart] [-PassThru] [-NewName <String>] [-Force] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

この `Add-Computer` コマンドレットは、ローカルコンピューターまたはリモートコンピューターをドメインまたはワークグループに追加するか、ドメイン間で移動します。 また、アカウントなしでドメインに追加されたコンピューターのドメイン アカウントも作成します。

このコマンドレットのパラメーターを使用して、組織単位 (OU) およびドメイン コントローラーを指定したり、セキュリティで保護されていない参加を実行したりすることができます。

コマンドの結果を取得するには、 **Verbose** パラメーターと **PassThru** パラメーターを使用します。

## 例

### 例 1: ドメインにローカルコンピューターを追加し、コンピューターを再起動する

```powershell
Add-Computer -DomainName Domain01 -Restart
```

このコマンドは、ローカル コンピューターを Domain01 ドメインに追加し、コンピューターを再起動して変更を有効にします。

### 例 2: ワークグループにローカルコンピューターを追加する

```powershell
Add-Computer -WorkgroupName WORKGROUP-A
```

このコマンドは、ローカル コンピューターを Workgroup-A ワークグループに追加します。

### 例 3: ローカルコンピューターをドメインに追加する

```powershell
Add-Computer -DomainName Domain01 -Server Domain01\DC01 -PassThru -Verbose
```

このコマンドは、Domain01\DC01 ドメイン コントローラーを使用して、ローカル コンピューターを Domain01 ドメインに追加します。

コマンドで **PassThru** パラメーターおよび **Verbose** パラメーターを使用して、コマンドの結果に関する詳細な情報を取得します。

### 例 4: OUPath パラメーターを使用してドメインにローカルコンピューターを追加する

```powershell
Add-Computer -DomainName Domain02 -OUPath "OU=testOU,DC=domain,DC=Domain,DC=com"
```

このコマンドは、ローカル コンピューターを Domain02 ドメインに追加します。
OUPath パラメーターを使用して、新しいアカウントの組織単位を指定します。

### 例 5: 資格情報を使用してドメインにローカルコンピューターを追加する

```powershell
Add-Computer -ComputerName Server01 -LocalCredential Server01\Admin01 -DomainName Domain02 -Credential Domain02\Admin02 -Restart -Force
```

このコマンドは、Server01 コンピューターを Domain02 ドメインに追加します。 **LocalCredential** パラメーターを使用して、Server01 コンピューターに接続するアクセス許可を持つユーザー アカウントを指定します。 **Credential** パラメーターを使用して、コンピューターをドメインに追加するアクセス許可を持つユーザー アカウントを指定します。 **Restart** パラメーターを使用して、参加操作の完了後にコンピューターを再起動し、 **Force** パラメーターを使用して、ユーザーへの確認メッセージが表示されないようにします。

### 例 6: コンピューターのグループを新しいドメインに移動する

```powershell
Add-Computer -ComputerName Server01, Server02, localhost -DomainName Domain02 -LocalCredential Domain01\User01 -UnjoinDomainCredential Domain01\Admin01 -Credential Domain02\Admin01 -Restart
```

このコマンドは、Server01、Server02、およびローカル コンピューターを Domain01 から Domain02 に移動します。

**LocalCredential** パラメーターを使用して、関連する 3 つのコンピューターに接続するアクセス許可を持つユーザー アカウントを指定します。 **UnjoinDomainCredential** パラメーターを使用して、Domain01 ドメインからコンピューターを削除するアクセス許可を持つユーザー アカウントを指定し、 **Credential** パラメーターを使用して、Domain02 ドメインにコンピューターを追加するアクセス許可を持つユーザー アカウントを指定します。 **Restart** パラメーターを使用して、移動の完了後、3 つのすべてのコンピューターを再起動します。

### 例 7: コンピューターを新しいドメインに移動し、コンピューターの名前を変更する

```powershell
Add-Computer -ComputerName Server01 -DomainName Domain02 -NewName Server044 -Credential Domain02\Admin01 -Restart
```

このコマンドは、Server01 コンピューターを Domain02 に移動し、コンピューター名を Server044 に変更します。

Server01 コンピューターへの接続には現在のユーザーの資格情報を使用し、このコンピューターを現在のドメインから削除します。 **Credential** パラメーターを使用して、コンピューターを Domain02 ドメインに参加させるアクセス許可を持つユーザーアカウントを指定します。

### 例 8: ファイルに一覧表示されているコンピューターを新しいドメインに追加する

```powershell
Add-Computer -ComputerName (Get-Content Servers.txt) -DomainName Domain02 -Credential Domain02\Admin02 -Options Win9xUpgrade  -Restart
```

このコマンドは、Servers.txt ファイルに記載されているコンピューターを Domain02 ドメインに追加します。 **Options** パラメーターを使用して、 **Win9xUpgrade** オプションを指定します。 **Restart** パラメーターは、参加操作の完了後に、新たに追加されたすべてのコンピューターを再起動します。

### 例 9: 定義済みのコンピューター資格情報を使用してコンピューターをドメインに追加する

この最初のコマンドは、既にドメインに参加しているコンピューターから管理者が実行する必要があり `Domain03` ます。

```powershell
New-ADComputer -Name "Server02" -AccountPassword (ConvertTo-SecureString -String 'TempJoinPA$$' -AsPlainText -Force)

# Then this command is run from `Server02` which is not yet domain-joined:

$joinCred = New-Object pscredential -ArgumentList ([pscustomobject]@{
    UserName = $null
    Password = (ConvertTo-SecureString -String 'TempJoinPA$$' -AsPlainText -Force)[0]
})
Add-Computer -Domain "Domain03" -Options UnsecuredJoin,PasswordPass -Credential $joinCred
```

このコマンドの組み合わせにより、ドメインに参加している既存のコンピューターを使用して、ドメイン内に事前定義された名前と一時的な参加パスワードを持つ新しいコンピューターアカウントを作成します。 次に、定義済みの名前を持つコンピューターは、コンピューター名と一時的な参加パスワードのみを使用してドメインに参加します。
定義済みのパスワードは、結合操作をサポートするためにのみ使用され、コンピューターが結合を完了した後で、通常のコンピューターアカウントの手順の一部として置き換えられます。

## PARAMETERS

### -ComputerName

ドメインまたはワークグループに追加するコンピューターを指定します。
既定値はローカル コンピューターです。

各リモートコンピューターの NetBIOS 名、インターネットプロトコル (IP) アドレス、または完全修飾ドメイン名を入力します。 ローカルコンピューターを指定するには、コンピューター名、ドット ( `.` )、または "localhost" を入力します。

このパラメーターは、Windows PowerShell リモート処理に依存しません。 **ComputerName** `Add-Computer` コンピューターがリモートコマンドを実行するように構成されていない場合でも、の ComputerName パラメーターを使用できます。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: Local computer
Accept pipeline input: True (ByPropertyName, ByValue)
Accept wildcard characters: False
```

### -Credential

新しいドメインにコンピューターを追加するアクセス許可を持つユーザー アカウントを指定します。
既定値は現在のユーザーです。

「User01」や「Domain01\User01」のようなユーザー名を入力するか、 コマンドレットで生成されるような `Get-Credential` オブジェクトを入力します。 ユーザー名を入力すると、パスワードの入力を促すメッセージが表示されます。

コンピューターを現在のドメインから削除するアクセス許可を持つユーザー アカウントを指定するには、 **UnjoinDomainCredential** パラメーターを使用します。 リモート コンピューターに接続するアクセス許可を持つユーザー アカウントを指定するには、 **LocalCredential** パラメーターを使用します。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: Domain, Workgroup
Aliases: DomainCredential

Required: True (Domain), False (Workgroup)
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -DomainName

コンピューターが追加されるドメインを指定します。 このパラメーターは、コンピューターをドメインに追加する場合に必要です。

```yaml
Type: System.String
Parameter Sets: Domain
Aliases: DN, Domain

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Force

ユーザーへの確認メッセージを表示しないようにします。 このパラメーターを指定しない場合、では、 `Add-Computer` 各コンピューターの追加を確認する必要があります。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

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

### -LocalCredential

**ComputerName** パラメーターで指定されたコンピューターに接続するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

「User01」や「Domain01\User01」のようなユーザー名を入力するか、 コマンドレットで生成されるような `Get-Credential` オブジェクトを入力します。 ユーザー名を入力すると、パスワードの入力を促すメッセージが表示されます。

新しいドメインにコンピューターを追加するアクセス許可を持つユーザー アカウントを指定するには、 **Credential** パラメーターを使用します。 コンピューターを現在のドメインから削除するアクセス許可を持つユーザー アカウントを指定するには、 **UnjoinDomainCredential** パラメーターを使用します。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

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

### -NewName

新しいドメインでのコンピューターの新しい名前を指定します。 このパラメーターは、1 つのコンピューターが追加または移動されている場合にのみ有効です。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -オプション

**コンピューターの追加** 参加操作の詳細オプションを指定します。 1 つ以上の値をコンマ区切りの文字列として入力します。

このパラメーターの有効値は、次のとおりです。

- **Accountcreate** : ドメインアカウントを作成します。 コンピューターの **追加** コマンドレットは、ドメインにコンピューターを追加するときに、ドメインアカウントを自動的に作成します。 このオプションは、完全を期すために用意されています。

- **Win9XUpgrade** : 結合操作が Windows オペレーティングシステムのアップグレードの一部であることを示します。

- **UnsecuredJoin** : セキュリティで保護されていない参加を実行します。 セキュリティで保護されていない参加を要求するには、 *安全* でないパラメーターまたはこのオプションを使用します。 コンピューターのパスワードを渡す場合は、このオプションをオプションと組み合わせて使用する必要があり `PasswordPass` ます。

- **Passwordpass** : セキュリティで保護されていない参加を実行した後、コンピューターのパスワードを *Credential* (DomainCredential) パラメーターの値に設定します。 このオプションは、 *Credential* (DomainCredential) パラメーターの値がコンピューターのパスワードであってユーザーのパスワードではないことも示します。 このオプションは、 `UnsecuredJoin` オプションが指定されている場合にのみ有効です。 このオプションを使用する場合、パラメーターに指定する資格情報には `-Credential` null ユーザー名を指定する *必要があり* ます。

- **Joinwithnewname** : 新しいドメイン内のコンピューター名を、 *NewName* パラメーターで指定した名前に変更します。 *NewName* パラメーターを使用すると、このオプションは自動的に設定されます。 このオプションは、Rename-Computer コマンドレットと共に使用するように設計されています。 コンピューター名の変更 **コマンドレット** を使用してコンピューターの名前を変更しても、コンピューターを再起動せずに変更を有効にした場合は、このパラメーターを使用して、コンピューターを新しい名前でドメインに参加させることができます。

- **Joinreadonly** : 既存のコンピューターアカウントを使用して、コンピューターを読み取り専用ドメインコントローラーに参加させます。 コンピューターアカウントは、パスワードレプリケーションポリシーの許可リストに追加する必要があります。また、アカウントのパスワードは、参加操作の前に読み取り専用ドメインコントローラーにレプリケートする必要があります。

- **InstallInvoke** : **Joindomainorworkgroup** メソッドの **fjoinoptions** パラメーターの作成 (0x2) フラグと削除 (0x4) フラグを設定します。 **Joindomainorworkgroup** メソッドの詳細については、 [Win32_ComputerSystem クラスの Joindomainorworkgroup メソッド](/windows/win32/cimwin32prov/joindomainorworkgroup-method-in-class-win32-computersystem)に関する説明を参照してください。
  これらのオプションの詳細については、「 [Netjoindomain 関数](/windows/win32/api/lmjoin/nf-lmjoin-netjoindomain)」を参照してください。

このパラメーターは Windows PowerShell 3.0 で導入されました。

```yaml
Type: Microsoft.PowerShell.Commands.JoinOptions
Parameter Sets: Domain
Aliases:
Accepted values: AccountCreate, Win9XUpgrade, UnsecuredJoin, PasswordPass, DeferSPNSet, JoinWithNewName, JoinReadOnly, InstallInvoke

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OUPath

ドメイン アカウントの組織単位 (OU) を指定します。 OU の完全識別名を引用符で囲んで入力します。 既定値は、ドメイン内で使用されるコンピューター オブジェクトの既定の OU です。

```yaml
Type: System.String
Parameter Sets: Domain
Aliases: OU

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

作業中の項目を表すオブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

ドメインまたはワークグループに追加されたコンピューターを再起動します。 多くの場合、変更を有効にするには再起動が必要です。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

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

### -Server

ドメインにコンピューターを追加するドメイン コントローラーの名前を指定します。 ドメイン名\コンピューター名の形式で入力します。 既定では、ドメイン コントローラーは指定されません。

```yaml
Type: System.String
Parameter Sets: Domain
Aliases: DC

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UnjoinDomainCredential

コンピューターを現在のドメインから削除するアクセス許可を持つユーザー アカウントを指定します。 既定値は現在のユーザーです。

「User01」や「Domain01\User01」のようなユーザー名を入力するか、 コマンドレットで生成されるような `Get-Credential` オブジェクトを入力します。 ユーザー名を入力すると、パスワードの入力を促すメッセージが表示されます。

このパラメーターは、コンピューターを別のドメインに移動するときに使用します。 新しいドメインに参加するアクセス許可を持つユーザー アカウントを指定するには、 **Credential** パラメーターを使用します。 リモート コンピューターに接続するアクセス許可を持つユーザー アカウントを指定するには、 **LocalCredential** パラメーターを使用します。

このパラメーターは、Windows PowerShell 3.0 で導入されました。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: Domain
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -セキュリティ保護なし

指定されたドメインへのセキュリティで保護されていない参加を行います。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Domain
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ワークスペース

コンピューターを追加するワークグループの名前を指定します。 既定値は "WORKGROUP" です。

```yaml
Type: System.String
Parameter Sets: Workgroup
Aliases: WGN

Required: True
Position: 0
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

### System.String

パイプを使用して、コンピューター名と新しい名前を `Add-Computer` コマンドレットに渡します。

## 出力

### Microsoft. PowerShell. ComputerChangeInfo

**PassThru** パラメーターを使用すると、は `Add-Computer` **computerchangeinfo** オブジェクトを返します。
それ以外の場合、このコマンドレットによる出力はありません。

## 注

- Windows PowerShell 2.0 では、 **Server** `Add-Computer` サーバーが存在する場合でも、の server パラメーターは失敗します。 Windows PowerShell 3.0 では、 **Server** パラメーターが確実に動作するように実装が変更されました。

## 関連リンク

[Checkpoint-Computer](Checkpoint-Computer.md)

[Remove-Computer](Remove-Computer.md)

[Restart-Computer](Restart-Computer.md)

[Rename-Computer](Rename-Computer.md)

[Restore-Computer](Restore-Computer.md)

[Stop-Computer](Stop-Computer.md)

[Test-Connection](Test-Connection.md)
