---
ms.date: 06/12/2017
title: Just Enough Administration (JEA) の強化
description: JEA は、PowerShell のリモート処理によるロールベースの管理を可能にする WMF 5.0 の新機能です。 非管理者が特定のコマンド、スクリプト、および実行可能ファイルを管理者として実行できるようにすることで、既存の制約付きエンドポイント インフラストラクチャを拡張します。
ms.openlocfilehash: f255d0ecbd4bbd9a5ade4b6e19f066cc007481fb
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92654023"
---
# <a name="improvements-to-just-enough-administration-jea"></a>Just Enough Administration (JEA) の強化

Just Enough Administration は、PowerShell のリモート処理によるロールベースの管理を可能にする WMF 5.0 の新機能です。 非管理者が特定のコマンド、スクリプト、および実行可能ファイルを管理者として実行できるようにすることで、既存の制約付きエンドポイント インフラストラクチャを拡張します。 これにより、環境における完全な権限を持つ管理者の数を削減し、セキュリティを向上させることができます。

## <a name="constrained-file-copy-tofrom-jea-endpoints"></a>JEA エンドポイントとの間の制約付きのファイル コピー

JEA エンドポイントとの間でファイルをリモートでコピーできるようになりました。接続ユーザーはシステム上のファイルを " *どれでも* " コピーできるわけではないので安心することができます。 これは、接続ユーザー用のユーザー ドライブをマウントするよう PSSC ファイルを構成することによって可能になります。 ユーザー ドライブは、各接続ユーザーに固有の新しい PSDrive であり、複数のセッションにわたって保持されます。 `Copy-Item` を使用して JEA セッションとの間でファイルのコピーを行うと、該当するユーザー ドライブへのアクセスしか許可しないように制約が課せられます。 その他の PSDrive ファイルへのコピーを試みると失敗します。

JEA セッションの構成ファイルで、ユーザー ドライブを設定するには、次の新しいフィールドを使用します。

```powershell
MountUserDrive = $true
UserDriveMaximumSize = 10485760    # 10 MB
```

ユーザー ドライブをバッキングするフォルダーは `$env:LOCALAPPDATA\Microsoft\Windows\PowerShell\DriveRoots\` に作成されます。 このエンドポイントに接続するユーザーには、それぞれ `$env:USERDOMAIN_$env:USERNAME` という名前のフォルダーが作成されます。

ユーザー ドライブを公開するように構成された JEA エンドポイントとの間で、ユーザー ドライブを使用してファイルのコピーを行うには、`Copy-Item` で `-ToSession` および `-FromSession` パラメーターを使用します。

```powershell
# Connect to the JEA endpoint
$jeasession = New-PSSession -ComputerName 'srv01' -ConfigurationName 'UserDemo'

# Copy a file in the local folder to the remote machine.
# Note: you cannot specify the file name or subfolder on the remote machine.
# You must exactly type "User:"
Copy-Item -Path .\SampleFile.txt -Destination User: -ToSession $jeasession

# Copy the file back from the remote machine to your local machine
Copy-Item -Path User:\SampleFile.txt -Destination . -FromSession $jeasession
```

ユーザー ドライブに格納されたデータを処理して、ロール機能ファイル内のユーザーがそれらを使用できるようにするためのカスタム関数を作成することができます。

## <a name="support-for-group-managed-service-accounts"></a>グループの管理されたサービス アカウントのサポート

場合によって、JEA セッションでユーザーが実行する必要があるタスクは、ローカル コンピューター以外のリソースにアクセスすることが必要な場合があります。 JEA セッションが仮想アカウントを使用するように構成されている場合、そのようなリソースへのアクセスの試みは、仮想アカウントまたは接続ユーザーからではなく、ローカル コンピューターの ID からのアクセスのように見えます。 TP5 では、[[グループの管理されたサービス アカウント]](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj128431\(v=ws.11\)) というコンテキストの下で JEA の実行をサポートするようにしたことで、ドメイン ID を使ってネットワーク リソースにアクセスすることがずっと簡単になりました。

JEA セッションを、gMSA アカウントの下で実行されるように構成するには、PSSC ファイルで次の新しいキーを使用します。

```powershell
# Provide the name of your gMSA account here (don't include a trailing $)
# The local machine must be privileged to use this gMSA in Active Directory
GroupManagedServiceAccount = 'myGMSAforJEA'

# You cannot configure a JEA endpoint to use both a gMSA and virtual account
# You can leave the RunAsVirtualAccount field commented out or explicitly set it to false
RunAsVirtualAccount = $false
```

> [!NOTE]
> グループの管理されたサービス アカウントでは、仮想アカウントの分離または限定的な範囲を許容していません。
> 接続ユーザーはすべて同じ gMSA ID を共有することになります。この ID は企業全体を対象とするアクセス許可を持つ場合もあります。 gMSA の使用を選択する場合は細心の注意を払ってください。可能であればローカル コンピューターに限定された仮想アカウントを常に優先してください。

## <a name="conditional-access-policies"></a>条件付きアクセス ポリシー

あるユーザーがシステムの管理を目的としてシステムに接続したときにその人が実行できる操作を制限する場合に JEA は便利です。しかし、だれかが JEA を使用できる *タイミング* を制限したい場合はどうするのですか? ユーザーが JEA セッションを確立するときに属する必要があるセキュリティ グループを指定できるように、セッション構成ファイル (.pssc) に構成オプションを追加しました。 環境内にジャスト イン タイム (JIT) システムが搭載されていて、高い権限を持つ JEA エンドポイントにアクセスする前にユーザーに自分の権限を昇格させる必要があるとき、このオプションは特に有用です。

PSSC ファイル内の新しい *RequiredGroups* フィールドを使用すると、ユーザーが JEA に接続できるかどうかを判断するロジックを指定することができます。 このロジックでは、ルールを構築するために "And" キーと "Or" キーを使用するハッシュ テーブル (必要に応じて入れ子になっている) を指定します。 このフィールドの使用方法について、次に例を示します。

```powershell
# Example 1: Connecting users must belong to a security group called "elevated-jea"
RequiredGroups = @{ And = 'elevated-jea' }

# Example 2: Connecting users must have signed on with 2 factor authentication or a smart card
# The 2 factor authentication group name is "2FA-logon" and the smart card group name is "smartcard-logon"
RequiredGroups = @{ Or = '2FA-logon', 'smartcard-logon' }

# Example 3: Connecting users must elevate into "elevated-jea" with their JIT system and have logged on with 2FA or a smart card
RequiredGroups = @{ And = 'elevated-jea', @{ Or = '2FA-logon', 'smartcard-logon' }}
```

## <a name="fixed-virtual-accounts-are-now-supported-on-windows-server-2008-r2"></a>固定:Windows Server 2008 R2 で仮想アカウントがサポートされるようになりました。

WMF 5.1 では、Windows Server 2008 R2 で仮想アカウントを使用できるようになりました。これにより、Windows Server 2008 R2 - 2016 にわたり一貫した構成と機能の類似性が提供されます。 Windows 7 で JEA を使用する場合、仮想アカウントはまだサポートされていません。
