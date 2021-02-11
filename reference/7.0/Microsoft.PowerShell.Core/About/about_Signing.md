---
description: PowerShell の実行ポリシーに準拠するようにスクリプトに署名する方法について説明します。
keywords: powershell,コマンドレット
Locale: en-US
ms.date: 07/31/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_signing?view=powershell-7&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Signing
ms.openlocfilehash: 202af7ea491b8fa020ab2ff7ae6b1cc697829b6b
ms.sourcegitcommit: 021ea294327dec542ec040619dac0d2171397a90
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/29/2020
ms.locfileid: "97804131"
---
# <a name="about-signing"></a>署名について

## <a name="short-description"></a>簡単な説明
PowerShell の実行ポリシーに準拠するようにスクリプトに署名する方法について説明します。

## <a name="long-description"></a>長い説明

制限された実行ポリシーでは、スクリプトの実行は許可されていません。 **AllSigned** と **RemoteSigned** の実行ポリシーによって、PowerShell がデジタル署名のないスクリプトを実行できなくなります。

このトピックでは、実行ポリシーが **RemoteSigned** されている場合でも、署名されていない選択したスクリプトを実行する方法と、独自に使用するスクリプトに署名する方法について説明します。

PowerShell 実行ポリシーの詳細については、「 [about_Execution_Policies](about_Execution_Policies.md)」を参照してください。

## <a name="to-permit-signed-scripts-to-run"></a>署名されたスクリプトの実行を許可するには

コンピューターで PowerShell を初めて起動するときは、 **制限** された実行ポリシー (既定) が有効になっている可能性があります。

**制限付き** ポリシーでは、スクリプトの実行は許可されていません。

コンピューターで有効な実行ポリシーを見つけるには、次のように入力します。

```powershell
Get-ExecutionPolicy
```

ローカルコンピューター上で作成され、他のユーザーから署名されたスクリプトを実行するには、[管理者として実行] オプションを使用して PowerShell を起動し、次のコマンドを使用してコンピューターの実行ポリシーを **RemoteSigned** に変更します。

```powershell
Set-ExecutionPolicy RemoteSigned
```

詳細については、コマンドレットのヘルプトピックを参照してください `Set-ExecutionPolicy` 。

## <a name="running-unsigned-scripts-using-the-remotesigned-execution-policy"></a>RemoteSigned 実行ポリシーを使用した未署名のスクリプトの実行

PowerShell の実行ポリシーが **RemoteSigned** の場合、電子メールやインスタントメッセージングプログラムを通じて受信する署名されていないスクリプトを含め、インターネットからダウンロードされた未署名のスクリプトは実行されません。

ダウンロードしたスクリプトを実行しようとすると、PowerShell によって次のエラーメッセージが表示されます。

```Output
The file <file-name> cannot be loaded. The file <file-name> is not digitally
signed. The script will not execute on the system. Please see "Get-Help
about_Signing" for more details.
```

スクリプトを実行する前に、コードを確認して、信頼できることを確認してください。
スクリプトは、実行可能プログラムと同じ効果があります。

署名されていないスクリプトを実行するには、Unblock-File コマンドレットを使用するか、次の手順を使用します。

1. スクリプトファイルをコンピューターに保存します。
2. [スタート] をクリックし、[マイコンピューター] をクリックして、保存されているスクリプトファイルを探します。
3. スクリプトファイルを右クリックし、[プロパティ] をクリックします。
4. [ブロックの解除] をクリックします。

インターネットからダウンロードされたスクリプトがデジタル署名されていても、その発行元を信頼するように選択していない場合は、PowerShell によって次のメッセージが表示されます。

```Output
Do you want to run software from this untrusted publisher?
The file <file-name> is published by CN=<publisher-name>. This
publisher is not trusted on your system. Only run scripts
from trusted publishers.

[V] Never run  [D] Do not run  [R] Run once  [A] Always run
[?] Help (default is "D"):
```

発行元を信頼する場合は、[一度だけ実行する] または [常に実行する] を選択します。 発行元を信頼していない場合は、[実行しない] または [実行しない] のいずれかを選択します。 [実行しない] または [常に実行する] を選択した場合、PowerShell はこのパブリッシャーに対して再度メッセージを表示しません。

## <a name="methods-of-signing-scripts"></a>スクリプトの署名方法

作成したスクリプトと他のソースから取得したスクリプトに署名することができます。 任意のスクリプトに署名する前に、各コマンドを調べて、実行しても安全かどうかを確認します。

コード署名のベストプラクティスについては、「 [コード署名のベストプラクティス](/previous-versions/windows/hardware/design/dn653556(v=vs.85))」を参照してください。

スクリプトファイルに署名する方法の詳細については、「 [get-authenticodesignature](xref:Microsoft.PowerShell.Security.Set-AuthenticodeSignature)」を参照してください。

`New-SelfSignedCertificate`PowerShell 3.0 の PKI モジュールで導入されたコマンドレットは、テストに適した自己署名証明書を作成します。 詳細については、New-SelfSignedCertificate コマンドレットのヘルプトピックを参照してください。

デジタル署名をスクリプトに追加するには、コード署名証明書を使用して署名する必要があります。 スクリプトファイルの署名には、次の2種類の証明書が適しています。

- 証明機関によって作成された証明書: 有料では、パブリック証明機関が id を検証し、コード署名証明書を提供します。 信頼できる証明機関から証明書を購入すると、他のコンピューターが証明機関を信頼しているため、Windows を実行している他のコンピューターのユーザーとスクリプトを共有できます。

- 証明書を作成する: 自分のコンピューターが証明書を作成する機関である自己署名証明書を作成できます。 この証明書は無料で使用でき、コンピューター上でスクリプトの記述、署名、および実行を行うことができます。 ただし、自己署名証明書によって署名されたスクリプトは、他のコンピューターでは実行されません。

通常は、自己署名証明書を使用して、自分で使用するために作成したスクリプトに署名したり、安全であることを確認した他のソースから取得したスクリプトに署名したりすることができます。 企業内でも共有されるスクリプトには適していません。

自己署名証明書を作成する場合は、証明書に対して強力な秘密キーの保護を有効にする必要があります。 これにより、悪意のあるプログラムがユーザーに代わってスクリプトに署名するのを防ぐことができます。 手順については、このトピックの最後に記載されています。

## <a name="create-a-self-signed-certificate"></a>自己署名証明書を作成する

自己署名証明書を作成するには、 `New-SelfSignedCertificate` PKI モジュールのコマンドレットを使用します。 このモジュールは PowerShell 3.0 で導入され、Windows 8 および Windows Server 2012 に含まれています。 詳細については、コマンドレットのヘルプトピックを参照してください `New-SelfSignedCertificate` 。

以前のバージョンの Windows で自己署名証明書を作成するには、証明書作成ツールを使用し `MakeCert.exe` ます。 このツールは、Microsoft .NET SDK (バージョン1.1 以降) と Microsoft Windows SDK に含まれています。

MakeCert.exe ツールの構文とパラメーターの説明の詳細については、「 [証明書作成ツール (MakeCert.exe)](/previous-versions/dotnet/netframework-2.0/bfsktky3(v=vs.80))」を参照してください。

MakeCert.exe ツールを使用して証明書を作成するには、SDK コマンドプロンプトウィンドウで次のコマンドを実行します。

注: 最初のコマンドは、コンピューターのローカル証明機関を作成します。 2番目のコマンドは、証明機関から個人証明書を生成します。

注: コマンドは、表示されたとおりにコピーまたは入力できます。 証明書の名前は変更できますが、置換は不要です。

```powershell
makecert -n "CN=PowerShell Local Certificate Root" -a sha1 `
-eku 1.3.6.1.5.5.7.3.3 -r -sv root.pvk root.cer `
-ss Root -sr localMachine

makecert -pe -n "CN=PowerShell User" -ss MY -a sha1 `
-eku 1.3.6.1.5.5.7.3.3 -iv root.pvk -ic root.cer
```

MakeCert.exe ツールでは、秘密キーのパスワードの入力を求められます。 パスワードは、同意せずに証明書を使用またはアクセスできないようにします。
覚えやすいパスワードを作成して入力します。 このパスワードは後で証明書を取得するために使用します。

証明書が正しく生成されたことを確認するには、次のコマンドを使用して、コンピューター上の証明書ストアにある証明書を取得します。 証明書ファイルがファイルシステムディレクトリに見つかりません。

PowerShell プロンプトで次のように入力します。

```powershell
Get-ChildItem cert:\CurrentUser\my -codesigning
```

このコマンドは、PowerShell 証明書プロバイダーを使用して、証明書に関する情報を表示します。

証明書が作成された場合、出力には、次のような画面で証明書を識別する **サムプリント** が表示されます。

```Output
Directory: Microsoft.PowerShell.Security\Certificate::CurrentUser\My

Thumbprint                                Subject
----------                                -------
4D4917CB140714BA5B81B96E0B18AAF2C4564FDF  CN=PowerShell User ]
```

## <a name="sign-a-script"></a>スクリプトに署名する

自己署名証明書を作成したら、スクリプトに署名することができます。 **AllSigned** 実行ポリシーを使用すると、スクリプトに署名することで、コンピューターでスクリプトを実行することが許可されます。

次のサンプルスクリプトでは、 `Add-Signature.ps1` スクリプトに署名します。 ただし、 **AllSigned** 実行ポリシーを使用している場合は、スクリプトを実行する前に署名する必要があり `Add-Signature.ps1` ます。

> [!IMPORTANT]
> スクリプトは、ASCII または UTF8NoBOM エンコードを使用して保存する必要があります。別のエンコードを使用するスクリプトファイルに署名することができます。 ただし、スクリプトを実行できないか、スクリプトを含むモジュールをインポートできません。

このスクリプトを使用するには、テキストファイルに次のテキストをコピーして、という名前を指定し `Add-Signature.ps1` ます。

```powershell
## Signs a file
param([string] $file=$(throw "Please specify a filename."))
$cert = @(Get-ChildItem cert:\CurrentUser\My -codesigning)[0]
Set-AuthenticodeSignature $file $cert
```

スクリプトファイルに署名するには `Add-Signature.ps1` 、PowerShell コマンドプロンプトで次のコマンドを入力します。

```powershell
$cert = @(Get-ChildItem cert:\CurrentUser\My -codesigning)[0]
Set-AuthenticodeSignature add-signature.ps1 $cert
```

スクリプトが署名されたら、ローカルコンピューターで実行できます。 ただし、このスクリプトは、PowerShell の実行ポリシーが信頼できる機関からのデジタル署名を必要とするコンピューターでは実行されません。 試行すると、PowerShell によって次のエラーメッセージが表示されます。

```Output
The file C:\remote_file.ps1 cannot be loaded. The signature of the
certificate cannot be verified.
At line:1 char:15
+ .\ remote_file.ps1 <<<<
```

書き込みが行われていないスクリプトを実行したときに PowerShell でこのメッセージが表示される場合は、署名されていないスクリプトを扱う場合と同様に、ファイルを扱います。 コードを確認して、スクリプトを信頼できるかどうかを判断します。

## <a name="enable-strong-private-key-protection-for-your-certificate"></a>証明書の秘密キーの強力な保護を有効にする

コンピューターにプライベート証明書がある場合は、悪意のあるプログラムによって、ユーザーに代わってスクリプトに署名できる可能性があります。これにより、PowerShell の実行が承認されます。

自動的に署名を行わないようにするには、証明書マネージャーを使用し `Certmgr.exe` て署名証明書をファイルにエクスポートし `.pfx` ます。 証明書マネージャーは、Microsoft .NET SDK、Microsoft Windows SDK、および internet Explorer に含まれています。

証明書をエクスポートするには:

1. 証明書マネージャーを起動します。
2. PowerShell ローカル証明書のルートによって発行された証明書を選択します。
3. [エクスポート] をクリックして、証明書のエクスポートウィザードを開始します。
4. [はい、秘密キーをエクスポートします] を選択し、[次へ] をクリックします。
5. [強力な保護を有効にする] を選択します。
6. パスワードを入力し、確認のためにもう一度入力します。
7. ファイル名拡張子が .pfx のファイル名を入力します。
8. [完了] をクリックします。

証明書を再インポートするには:

1. 証明書マネージャーを起動します。
2. [インポート] をクリックして、証明書のインポートウィザードを開始します。
3. エクスポートプロセスで作成した .pfx ファイルの場所を開きます。
4. [パスワード] ページで、[秘密キーの保護を強力にする] を選択し、エクスポート処理中に割り当てたパスワードを入力します。
5. [ 個人 ] 証明書ストアを選択します。
6. [完了] をクリックします。

## <a name="prevent-the-signature-from-expiring"></a>署名の有効期限が切れないようにする

スクリプトのデジタル署名は、署名証明書の有効期限が切れている間に、署名証明書の有効期限が切れるか、タイムスタンプサーバーでスクリプトが署名されたことを確認できる限り有効です。

ほとんどの署名証明書は1年間有効であるため、タイムスタンプサーバーを使用すると、ユーザーがスクリプトを何年も使用できるようになります。

## <a name="see-also"></a>関連項目

[about_Execution_Policies](about_Execution_Policies.md)

[about_Profiles](about_Profiles.md)

[Get-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Get-ExecutionPolicy)

[Set-ExecutionPolicy](xref:Microsoft.PowerShell.Security.Set-ExecutionPolicy)

[Set-AuthenticodeSignature](xref:Microsoft.PowerShell.Security.Set-AuthenticodeSignature)

[コード署名の概要](/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537361(v=vs.85))
