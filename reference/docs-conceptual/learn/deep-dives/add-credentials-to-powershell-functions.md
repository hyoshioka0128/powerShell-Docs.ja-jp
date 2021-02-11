---
title: PowerShell 関数への資格情報サポートの追加
description: PowerShell のスクリプト、関数、およびコマンドレットに資格情報パラメーターを追加する方法。
ms.date: 10/29/2020
ms.custom: contributor-JoshDuffney
ms.openlocfilehash: 3e4a3f41ccbca1cf97f2e96fd60f22d89be7bc5a
ms.sourcegitcommit: 39c2a697228276d5dae39e540995fa479c2b5f39
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/05/2020
ms.locfileid: "93354628"
---
# <a name="add-credential-support-to-powershell-functions"></a>PowerShell 関数への資格情報サポートの追加

> [!NOTE]
> この記事の[オリジナル バージョン][]は、[@joshduffney][] 氏のブログ記事です。 この記事は、このサイトに追加するために編集されています。 このコンテンツを共有してくださった Josh 氏に、PowerShell チームより感謝を申し上げます。 同氏のブログは [duffney.io][] で確認してください。

この記事では、PowerShell 関数に資格情報パラメーターを追加する方法と、それが必要な理由について説明します。 資格情報パラメーターを使用すると、関数またはコマンドレットを別のユーザーとして実行できます。 最も一般的な用途は、関数またはコマンドレットを管理者特権のユーザー アカウントとして実行することです。

たとえば、コマンドレット `New-ADUser` には **Credential** パラメーターがあり、ドメイン管理者の資格情報を指定してドメイン内にアカウントを作成できます。 PowerShell セッションが実行されている通常のアカウントに、まだそのアクセス権がないことを前提としています。

## <a name="creating-credential-object"></a>資格情報オブジェクトを作成する

[PSCredential][] オブジェクトは、ユーザー名やパスワードなどの一連のセキュリティ資格情報を表します。 オブジェクトは、その資格情報オブジェクト内でユーザー アカウントとして実行される関数にパラメーターとして渡すことができます。 資格情報オブジェクトを作成するには、いくつかの方法があります。 資格情報オブジェクトを作成する 1 つ目の方法は、PowerShell コマンドレット `Get-Credential` を使用する方法です。 パラメーターを指定せずに実行すると、ユーザー名とパスワードの入力が求められます。 または、省略可能なパラメーターをいくつか指定してそのコマンドレットを呼び出すこともできます。

ドメイン名とユーザー名を事前に指定するには、 **Credential** パラメーターまたは **UserName** パラメーターのいずれかを使用できます。 **UserName** パラメーターを使用するときは、 **Message** 値も指定する必要があります。 以下のコードは、そのコマンドレットの使用方法を示しています。 資格情報オブジェクトを変数に格納して、その資格情報が複数回使用されるように設定することもできます。 次の例は、その資格情報オブジェクトが変数 `$Cred` に格納されています。

```powershell
$Cred = Get-Credential
$Cred = Get-Credential -Credential domain\user
$Cred = Get-Credential -UserName domain\user -Message 'Enter Password'
```

場合によっては、前の例で示したように、資格情報オブジェクトを作成する対話型のメソッドを使用できないことがあります。 ほとんどの自動化ツールには、非対話型のメソッドが必要です。 ユーザーの操作なしで資格情報を作成するには、パスワードが含まれるセキュリティで保護された文字列を作成します。 その後、セキュリティで保護された文字列とユーザー名を `System.Management.Automation.PSCredential()` メソッドに渡します。

次のコマンドを使用して、パスワードが含まれるセキュリティで保護された文字列を作成します。

```powershell
ConvertTo-SecureString "MyPlainTextPassword" -AsPlainText -Force
```

**AsPlainText** パラメーターと **Force** パラメーターは両方とも必須です。 これらのパラメーターを指定しないと、セキュリティで保護された文字列にプレーン テキストを渡してはならないことを示す警告メッセージが表示されます。 プレーン テキストのパスワードがさまざまなログに記録されるため、PowerShell によってこの警告が返されます。 セキュリティで保護された文字列を作成したら、それを `PSCredential()` メソッドに渡して資格情報オブジェクトを作成する必要があります。 次の例では、`$password` 変数に資格情報オブジェクトが含まれるセキュリティで保護された文字列 `$Cred` が格納されています。

```powershell
$password = ConvertTo-SecureString "MyPlainTextPassword" -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential ("username", $password)
```

ここまでで、資格情報オブジェクトを作成する方法について説明しました。次は、資格情報パラメーターを PowerShell 関数に追加します。

## <a name="adding-a-credential-parameter"></a>資格情報パラメーターを追加する

まずは他のパラメーターと同様に、関数の `param` ブロックに追加します。
パラメーターの名前は、`$Credential` に指定することをお勧めします。これが既存の PowerShell コマンドレットで使用されるためです。 パラメーターの型は `[System.Management.Automation.PSCredential]` にしてください。

次の例は、`Get-Something` と呼ばれる関数のパラメーター ブロックを示しています。 `$Name` と `$Credential` という 2 つのパラメーターがあります。

```powershell
function Get-Something {
    param(
        $Name,
        [System.Management.Automation.PSCredential]$Credential
    )
```

この例のコードは、資格情報パラメーターとして十分に実用的ですが、いくつかの機能を追加して、より堅牢性を高めることができます。

- `[ValidateNotNull()]` 検証属性を追加して、 **Credential** に渡される値を確認する。 パラメーター値が null の場合、この属性により無効な資格情報で関数が実行されなくなります。

- `[System.Management.Automation.Credential()]`を追加します。 これにより、ユーザー名を文字列として渡し、パスワードの入力を求める対話型のプロンプトが表示されます。

- `$Credential` パラメーターの既定値を `[System.Management.Automation.PSCredential]::Empty` に設定する。 この関数では、ユーザーはこの `$Credential` オブジェクトを既存の PowerShell コマンドレットに渡している可能性があります。 関数内で呼び出されたコマンドレットに null 値を指定すると、エラーが発生します。 空の資格情報オブジェクトを指定すると、このエラーを回避できます。

> [!TIP]
> 資格情報パラメーターを受け取る一部のコマンドレットで、サポートされる必要がある `[System.Management.Automation.PSCredential]::Empty` がサポートされていません。 回避策については、「[レガシ コマンドレットの処理」](#dealing-with-legacy-cmdlets)のセクションをご覧ください。

## <a name="using-credential-parameters"></a>資格情報パラメーターを使用する

次の例は、資格情報パラメーターを使用する方法を示しています。 この例は、[The Pester Book][] から抜粋した `Set-RemoteRegistryValue` と呼ばれる関数を示します。 この関数により、前のセクションで説明した手法を使用して、資格情報パラメーターが定義されます。 その関数により、その関数によって作成された `$Credential` 変数を使用して `Invoke-Command` が呼び出されます。 これにより、`Invoke-Command` を実行しているユーザーを変更できます。 `$Credential` の既定値は空の資格情報であるため、その関数は資格情報の指定なしで実行されます。

```powershell
function Set-RemoteRegistryValue {
    param(
        $ComputerName,
        $Path,
        $Name,
        $Value,
        [ValidateNotNull()]
        [System.Management.Automation.PSCredential]
        [System.Management.Automation.Credential()]
        $Credential = [System.Management.Automation.PSCredential]::Empty
    )
        $null = Invoke-Command -ComputerName $ComputerName -ScriptBlock {
            Set-ItemProperty -Path $using:Path -Name $using:Name -Value $using:Value
        } -Credential $Credential
}
```

次のセクションでは、`Set-RemoteRegistryValue` に資格情報を指定するさまざまな方法を示します。

### <a name="prompting-for-credentials"></a>資格情報の入力を求めるダイアログを表示する

実行時に `Get-Credential` を かっこ (`()`) 内で使用すると、最初に `Get-credential` が実行されます。 ユーザー名とパスワードの入力が求められます。 `Get-credential` の **Credential** パラメーターまたは **UserName** パラメーターを使用して、ユーザー名とドメインを事前に設定できます。 次の例では、スプラッティングという手法を使用して、`Set-RemoteRegistryValue` 関数にパラメーターを渡します。 スプラッティングの詳細については、「[about_Splatting][]」の記事をご覧ください。

```powershell
$remoteKeyParams = @{
    ComputerName = $env:COMPUTERNAME
    Path = 'HKLM:\SOFTWARE\Microsoft\WebManagement\Server'
    Name = 'EnableRemoteManagement'
    Value = '1'
}

Set-RemoteRegistryValue @remoteKeyParams -Credential (Get-Credential)
```

![実行時に資格情報を取得する](./media/add-credentials-to-powershell-functions/GetCredAtRunTime.gif)

`(Get-Credential)` を使用することは、一見煩雑です。 通常、ユーザー名のみが指定された **Credential** パラメーターを使用すると、そのコマンドレットによって自動的にパスワードの入力が求められます。 `[System.Management.Automation.Credential()]` 属性を使用すると、この動作が有効になります。

```powershell
$remoteKeyParams = @{
    ComputerName = $env:COMPUTERNAME
    Path = 'HKLM:\SOFTWARE\Microsoft\WebManagement\Server'
    Name = 'EnableRemoteManagement'
    Value = '1'
}

Set-RemoteRegistryValue @remoteKeyParams -Credential duffney
```

![資格情報の要求](./media/add-credentials-to-powershell-functions/GetCredsPrompt.gif)

> [!NOTE]
> 表示されるレジストリ値を設定するために、これらの例では、Windows の **Web サーバー** 機能がインストールされていることを前提としています。 必要に応じて、`Install-WindowsFeature Web-Server` と `Install-WindowsFeature web-mgmt-tools` を実行します。

### <a name="provide-credentials-in-a-variable"></a>変数に資格情報を指定する

資格情報変数を事前に設定し、`Set-RemoteRegistryValue` 関数の **Credential** パラメーターに渡すこともできます。 この方法は、Jenkins、TeamCity、Octopus Deploy などの継続的インテグレーション/継続的配置 (CI/CD) ツールで使用されます。 Jenkins の使用例については、Hodge のブログ記事「[Windows での Jenkins と PowerShell を使用した自動化 - パート 2][]」をご覧ください。

この例では、.NET メソッドを使用してその資格情報オブジェクトとセキュリティで保護された文字列を作成し、パスワードを渡します。

```powershell
$password = ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential ("duffney", $password)

$remoteKeyParams = @{
    ComputerName = $env:COMPUTERNAME
    Path = 'HKLM:\SOFTWARE\Microsoft\WebManagement\Server'
    Name = 'EnableRemoteManagement'
    Value = '1'
}

Set-RemoteRegistryValue @remoteKeyParams -Credential $Cred
```

この例の場合、クリア テキストのパスワードを使用して、セキュリティで保護された文字列が作成されます。 以前に説明した CI/CD にはすべて、実行時にそのパスワードを指定する、セキュリティで保護されたメソッドが用意されています。 これらのツールを使用するときは、プレーン テキストのパスワードを、使用する CI/CD ツール内で定義された変数に置き換えます。

### <a name="run-without-credentials"></a>資格情報を使用しないで実行する

`$Credential` の既定値は空の資格情報オブジェクトであるため、次の例に示すように、資格情報なしでコマンドを実行できます。

```powershell
$remoteKeyParams = @{
    ComputerName = $env:COMPUTERNAME
    Path = 'HKLM:\SOFTWARE\Microsoft\WebManagement\Server'
    Name = 'EnableRemoteManagement'
    Value = '1'
}

Set-RemoteRegistryValue @remoteKeyParams
```

## <a name="dealing-with-legacy-cmdlets"></a>レガシ コマンドレットに対処する

すべてのコマンドレットで、資格情報オブジェクトがサポートされていたり、空の資格情報が許可されていたりするわけではありません。 代わりに、コマンドレットには文字列としてのユーザー名とパスワードのパラメーターが求められます。 この制限を回避するには、いくつかの方法があります。

### <a name="using-if-else-to-handle-empty-credentials"></a>if-else を使用して空の資格情報を処理する

このシナリオでは、実行するコマンドレットで空の資格情報オブジェクトは受け入れられません。 この例では、 **Credential** パラメーターが空でない場合にのみ、`Invoke-Command` に追加します。 それ以外の場合は、 **Credential** パラメーターなしで `Invoke-Command` が実行されます。

```powershell
function Set-RemoteRegistryValue {
    param(
        $ComputerName,
        $Path,
        $Name,
        $Value,
        [ValidateNotNull()]
        [System.Management.Automation.PSCredential]
        [System.Management.Automation.Credential()]
        $Credential = [System.Management.Automation.PSCredential]::Empty
    )

    if($Credential -ne [System.Management.Automation.PSCredential]::Empty) {
        Invoke-Command -ComputerName:$ComputerName -Credential:$Credential  {
            Set-ItemProperty -Path $using:Path -Name $using:Name -Value $using:Value
        }
    } else {
        Invoke-Command -ComputerName:$ComputerName {
            Set-ItemProperty -Path $using:Path -Name $using:Name -Value $using:Value
        }
    }
}
```

### <a name="using-splatting-to-handle-empty-credentials"></a>スプラッティングを使用して空の資格情報を処理する

この例では、パラメーターのスプラッティングを使用して、レガシ コマンドレットを呼び出します。 `$Credential` オブジェクトは、スプラッティングのためにハッシュ テーブルに条件付きで追加され、`Invoke-Command` スクリプト ブロックを繰り返す必要がなくなります。 関数内のスプラッティングの詳細については、「[高度な関数内でのパラメーターのスプラッティング][]」のブログ記事をご覧ください。

```powershell
function Set-RemoteRegistryValue {
    param(
        $ComputerName,
        $Path,
        $Name,
        $Value,
        [ValidateNotNull()]
        [System.Management.Automation.PSCredential]
        [System.Management.Automation.Credential()]
        $Credential = [System.Management.Automation.PSCredential]::Empty
    )

        $Splat = @{
            ComputerName = $ComputerName
        }

        if ($Credential -ne [System.Management.Automation.PSCredential]::Empty) {
            $Splat['Credential'] = $Credential
        }

        $null = Invoke-Command -ScriptBlock {
            Set-ItemProperty -Path $using:Path -Name $using:Name -Value $using:Value
        } @splat
}
```

### <a name="working-with-string-passwords"></a>文字列のパスワードを使用する

`Invoke-Sqlcmd` コマンドレットは、文字列をパスワードとして受け取るコマンドレットの例です。
`Invoke-Sqlcmd` を使用すると、SQL の単純な insert、update、および delete ステートメントを実行できます。 `Invoke-Sqlcmd` には、よりセキュリティで保護された資格情報オブジェクトではなく、クリアテキストのユーザー名とパスワードが必要です。 この例では、資格情報オブジェクトからユーザー名とパスワードを抽出する方法を示します。

この例の `Get-AllSQLDatabases` 関数では、`Invoke-Sqlcmd` コマンドレットを呼び出して、SQL Server のすべてのデータベースに対してクエリを実行します。 その関数により、前の例で使用したのと同じ属性を持つ **Credential** パラメーターが定義されます。 ユーザー名とパスワードは `$Credential` 変数内に存在するため、それらの値を抽出して `Invoke-Sqlcmd` で使用できます。

ユーザー名は `$Credential` 変数の **UserName** プロパティから使用できます。 パスワードを取得するには、`$Credential` オブジェクトの `GetNetworkCredential()` メソッドを使用する必要があります。 値は、`Invoke-Sqlcmd` へのパラメーターのスプラッティングのために使用されるハッシュ テーブルに追加された変数に抽出されます。

```powershell
function Get-AllSQLDatabases {
    param(
        $SQLServer,
        [ValidateNotNull()]
        [System.Management.Automation.PSCredential]
        [System.Management.Automation.Credential()]
        $Credential = [System.Management.Automation.PSCredential]::Empty
    )

        $UserName = $Credential.UserName
        $Password = $Credential.GetNetworkCredential().Password

        $splat = @{
            UserName = $UserName
            Password = $Password
            ServerInstance = 'SQLServer'
            Query = "Select * from Sys.Databases"
        }

        Invoke-Sqlcmd @splat
}

$credSplat = @{
    TypeName = 'System.Management.Automation.PSCredential'
    ArgumentList = 'duffney',('P@ssw0rd' | ConvertTo-SecureString -AsPlainText -Force)
}
$Credential= New-Object @credSplat
ConvertTo-SecureString -AsPlainText -Force)

Get-AllSQLDatabases -SQLServer SQL01 -Credential $Credential
```

## <a name="continued-learning-credential-management"></a>資格情報の管理を継続的に学習する

資格情報オブジェクトを安全に作成して保存することは困難な場合があります。 次のリソースは、PowerShell 資格情報の管理に役立ちます。

- [BetterCredentials][]
- [Azure Key Vault][]
- [Vault Project][]
- [SecretManagement モジュール][]

<!-- link references -->
[オリジナル バージョン]: https://duffney.io/addcredentialstopowershellfunctions/
[@joshduffney]: https://twitter.com/joshduffney
[duffney.io]: https://duffney.io/posts/
[BetterCredentials]: https://www.powershellgallery.com/packages/BetterCredentials/
[Azure Key Vault]: https://azure.microsoft.com/services/key-vault/
[Vault Project]: https://www.vaultproject.io/
[高度な関数内でのパラメーターのスプラッティング]: https://duffney.io/Splatting-Parameters-Within-AdvancedFunctions
[Windows での Jenkins と PowerShell を使用した自動化 - パート 2]: https://hodgkins.io/automating-with-jenkins-and-powershell-on-windows-part-2
[PSCredential]: /dotnet/api/system.management.automation.pscredential
[The Pester Book]: https://leanpub.com/the-pester-book
[about_Splatting]: /powershell/module/microsoft.powershell.core/about/about_splatting
[SecretManagement モジュール]: https://devblogs.microsoft.com/powershell/secretmanagement-and-secretstore-updates/
