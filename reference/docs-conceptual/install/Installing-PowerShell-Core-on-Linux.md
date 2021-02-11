---
title: Linux への PowerShell のインストール
description: さまざまな Linux ディストリビューションへの PowerShell のインストールに関する情報
ms.date: 02/02/2021
ms.openlocfilehash: d02fa6e3828d539cf70d7bbcdb8ee7bd8ab35c1d
ms.sourcegitcommit: 40b6d8e9b6d791ac69e2ff85224e900b21552bc1
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/04/2021
ms.locfileid: "99536509"
---
# <a name="installing-powershell-on-linux"></a>Linux への PowerShell のインストール

すべてのパッケージは GitHub [リリース][] ページにあります。 パッケージがインストールされたら、ターミナルから `pwsh` を実行します。 [プレビュー リリース](#installing-preview-releases)をインストールした場合は、`pwsh-preview` を実行します。

> [!NOTE]
> PowerShell 7 はインプレース アップグレードで、PowerShell Core 6.x は削除されます。
>
> `/usr/local/microsoft/powershell/6` フォルダーは `/usr/local/microsoft/powershell/7` に置き換えられました。
>
> PowerShell 6 を PowerShell 7 と side-by-side 実行する必要がある場合は、[バイナリ アーカイブ](#binary-archives)方法を使用して PowerShell 6 を再インストールします。

公式にサポートされていない Linux ディストリビューションの場合は、[PowerShell の Snap パッケージ][snap]を使った PowerShell のインストールを試すことができます。 また、Linux [`tar.gz` アーカイブ][tar]を使用して、直接 PowerShell バイナリを展開できますが、場合によっては、OS に基づいて依存関係を別の手順で設定する必要があります。

[snap]: #snap-package
[tar]: #binary-archives

<!-- TODO: Update for supported releases v7.0 & v7.1 -->

PowerShell 7.1 で公式にサポートされているプラットフォーム リリース

- Ubuntu 16.04/18.04/20.04 (ARM64 を含む)
- Ubuntu 19.10 (スナップ パッケージを使用)
- Debian 9/10
- CentOS と RHEL 7/8
- Fedora 30
- Alpine 3.11 以降 (ARM64 を含む)

PowerShell 7.0 で公式にサポートされているプラットフォーム リリース

- Ubuntu 16.04
- Ubuntu 18.04 および 20.04
- Debian 8
- Debian 9
- Debian 10
- Alpine 3.9 および 3.10
- CentOS 7
- Red Hat Enterprise Linux (RHEL) 7
- Fedora 28
- Fedora 29
- Fedora 30
- openSUSE 42.3
- openSUSE Leap 15

コミュニティでサポートされているリリース

- Ubuntu 18.10
- Ubuntu 19.10 および 20.10
- Arch Linux
- Kali
- Raspbian (試験段階)

代替のインストール方法

- Snap パッケージ
- バイナリ アーカイブ
- .NET グローバル ツール

## <a name="ubuntu-1604"></a>Ubuntu 16.04

### <a name="installation-via-package-repository---ubuntu-1604"></a>パッケージ リポジトリによるインストール - Ubuntu 16.04

Linux 向けの PowerShell は、インストールと更新を容易にするためにパッケージ リポジトリに公開されています。

推奨される方法は次のとおりです。

```sh
# Update the list of packages
sudo apt-get update
# Install pre-requisite packages.
sudo apt-get install -y wget apt-transport-https software-properties-common
# Download the Microsoft repository GPG keys
wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb
# Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
# Update the list of packages after we added packages.microsoft.com
sudo apt-get update
# Install PowerShell
sudo apt-get install -y powershell
# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo apt-get install powershell` を使って PowerShell を更新できます。

### <a name="installation-via-direct-download---ubuntu-1604"></a>直接ダウンロードによるインストール - Ubuntu 16.04

[リリース][] ページから Ubuntu コンピューターに Debian パッケージ `powershell_7.1.1-1.ubuntu.16.04_amd64.deb` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo dpkg -i powershell_7.1.1-1.ubuntu.16.04_amd64.deb
sudo apt-get install -f
```

> [!NOTE]
> `dpkg -i` コマンドは依存関係が満たされずに失敗します。 次のコマンド `apt-get install -f` でこれらの問題は解決され、PowerShell パッケージの構成が完了します。

### <a name="uninstallation---ubuntu-1604"></a>アンインストール - Ubuntu 16.04

```sh
sudo apt-get remove powershell
```

## <a name="ubuntu-1804"></a>Ubuntu 18.04

### <a name="installation-via-package-repository---ubuntu-1804"></a>パッケージ リポジトリによるインストール - Ubuntu 18.04

Linux 向けの PowerShell は、インストールと更新を容易にするためにパッケージ リポジトリに公開されています。

推奨される方法は次のとおりです。

```sh
# Update the list of packages
sudo apt-get update
# Install pre-requisite packages.
sudo apt-get install -y wget apt-transport-https software-properties-common
# Download the Microsoft repository GPG keys
wget -q https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb
# Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
# Update the list of products
sudo apt-get update
# Enable the "universe" repositories
sudo add-apt-repository universe
# Install PowerShell
sudo apt-get install -y powershell
# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo apt-get install powershell` を使って PowerShell を更新できます。

### <a name="installation-via-direct-download---ubuntu-1804"></a>直接ダウンロードによるインストール - Ubuntu 18.04

[リリース][] ページから Ubuntu コンピューターに Debian パッケージ `powershell_7.1.1-1.ubuntu.18.04_amd64.deb` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo dpkg -i powershell_7.1.1-1.ubuntu.18.04_amd64.deb
sudo apt-get install -f
```

> [!NOTE]
> `dpkg -i` コマンドは依存関係が満たされずに失敗します。 次のコマンド `apt-get install -f` でこれらの問題は解決され、PowerShell パッケージの構成が完了します。

### <a name="uninstallation---ubuntu-1804"></a>アンインストール - Ubuntu 18.04

```sh
sudo apt-get remove powershell
```

## <a name="ubuntu-2004"></a>Ubuntu 20.04

### <a name="installation-via-package-repository---ubuntu-2004"></a>パッケージ リポジトリによるインストール - Ubuntu 20.04

Linux 向けの PowerShell は、インストールと更新を容易にするためにパッケージ リポジトリに公開されています。

推奨される方法は次のとおりです。

```sh
# Update the list of packages
sudo apt-get update
# Install pre-requisite packages.
sudo apt-get install -y wget apt-transport-https software-properties-common
# Download the Microsoft repository GPG keys
wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb
# Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb
# Update the list of products
sudo apt-get update
# Enable the "universe" repositories
sudo add-apt-repository universe
# Install PowerShell
sudo apt-get install -y powershell
# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo apt-get install powershell` を使って PowerShell を更新できます。

### <a name="installation-via-direct-download---ubuntu-2004"></a>直接ダウンロードによるインストール - Ubuntu 20.04

[リリース][] ページから Ubuntu コンピューターに Debian パッケージ `powershell_7.1.1-1.ubuntu.20.04_amd64.deb` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo dpkg -i powershell_7.1.1-1.ubuntu.20.04_amd64.deb
sudo apt-get install -f
```

> [!NOTE]
> `dpkg -i` コマンドは依存関係が満たされずに失敗します。 次のコマンド `apt-get install -f` でこれらの問題は解決され、PowerShell パッケージの構成が完了します。

### <a name="uninstallation---ubuntu-2004"></a>アンインストール - Ubuntu 20.04

```sh
sudo apt-get remove powershell
```

## <a name="ubuntu-1810"></a>Ubuntu 18.10

`snapd` によるインストールがサポートされています。 手順については、「[Snap パッケージ][snap]」をご覧ください。

> [!NOTE]
> Ubuntu 18.10 は、[コミュニティでサポートされている](../powershell-support-lifecycle.md)[中間リリース](https://www.ubuntu.com/about/release-cycle)です。

## <a name="ubuntu-1910-and-2010"></a>Ubuntu 19.10 および 20.10

`snapd` によるインストールがサポートされています。 手順については、「[Snap パッケージ][snap]」をご覧ください。

> [!NOTE]
> Ubuntu 19.10 は、[コミュニティでサポートされている](../powershell-support-lifecycle.md)[中間リリース](https://www.ubuntu.com/about/release-cycle)です。

## <a name="debian-8"></a>Debian 8

### <a name="installation-via-package-repository---debian-8"></a>パッケージ リポジトリによるインストール - Debian 8

Linux 向けの PowerShell は、インストールと更新を容易にするためにパッケージ リポジトリに公開されています。

推奨される方法は次のとおりです。

```sh
# Install system components
sudo apt-get update
sudo apt-get install -y curl apt-transport-https

# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Product feed
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-jessie-prod jessie main" > /etc/apt/sources.list.d/microsoft.list'

# Update the list of products
sudo apt-get update

# Install PowerShell
sudo apt-get install -y powershell

# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo apt-get install powershell` を使って PowerShell を更新できます。

## <a name="debian-9"></a>Debian 9

### <a name="installation-via-package-repository---debian-9"></a>パッケージ リポジトリによるインストール - Debian 9

Linux 向けの PowerShell は、インストールと更新を容易にするためにパッケージ リポジトリに公開されています。

推奨される方法は次のとおりです。

```sh
# Install system components
sudo apt-get update
sudo apt-get install -y curl gnupg apt-transport-https

# Import the public repository GPG keys
curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -

# Register the Microsoft Product feed
sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-stretch-prod stretch main" > /etc/apt/sources.list.d/microsoft.list'

# Update the list of products
sudo apt-get update

# Install PowerShell
sudo apt-get install -y powershell

# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo apt-get install powershell` を使って PowerShell を更新できます。

### <a name="installation-via-direct-download---debian-9"></a>直接ダウンロードによるインストール - Debian 9

[リリース][] ページから Debian コンピューターに Debian パッケージ `powershell_7.1.1-1.debian.9_amd64.deb` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo dpkg -i powershell_7.1.1-1.debian.9_amd64.deb
sudo apt-get install -f
```

### <a name="uninstallation---debian-9"></a>アンインストール - Debian 9

```sh
sudo apt-get remove powershell
```

## <a name="debian-10"></a>Debian 10

> [!NOTE]
> Debian 10 は、PowerShell 7.0 以降でのみサポートされます。

### <a name="installation-via-package-repository---debian-10"></a>パッケージ リポジトリによるインストール - Debian 10

Linux 向けの PowerShell は、インストールと更新を容易にするためにパッケージ リポジトリに公開されています。

推奨される方法は次のとおりです。

```sh
# Download the Microsoft repository GPG keys
wget https://packages.microsoft.com/config/debian/10/packages-microsoft-prod.deb

# Register the Microsoft repository GPG keys
sudo dpkg -i packages-microsoft-prod.deb

# Update the list of products
sudo apt-get update

# Install PowerShell
sudo apt-get install -y powershell

# Start PowerShell
pwsh
```

### <a name="installation-via-direct-download---debian-10"></a>直接ダウンロードによるインストール - Debian 10

[リリース][] ページから Debian コンピューターに tar.gz パッケージ `powershell-7.1.1-linux-x64.tar.gz` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo apt-get update
# install the requirements
sudo apt-get install -y \
        less \
        locales \
        ca-certificates \
        libicu63 \
        libssl1.1 \
        libc6 \
        libgcc1 \
        libgssapi-krb5-2 \
        liblttng-ust0 \
        libstdc++6 \
        zlib1g \
        curl

# Download the powershell '.tar.gz' archive
curl -L  https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-linux-x64.tar.gz -o /tmp/powershell.tar.gz

# Create the target folder where powershell will be placed
sudo mkdir -p /opt/microsoft/powershell/7

# Expand powershell to the target folder
sudo tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7

# Set execute permissions
sudo chmod +x /opt/microsoft/powershell/7/pwsh

# Create the symbolic link that points to pwsh
sudo ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh

# Start PowerShell
pwsh
```

## <a name="alpine-39-and-310"></a>Alpine 3.9 および 3.10

> [!NOTE]
> Alpine 3.9 および 3.10 は、PowerShell 7.0 以降でのみサポートされます。

### <a name="installation-via-direct-download---alpine-39-and-310"></a>直接ダウンロードによるインストール - Alpine 3.9 および 3.10

[リリース][] ページから Alpine コンピューターに tar.gz パッケージ `powershell-7.1.1-linux-alpine-x64.tar.gz` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
# install the requirements
sudo apk add --no-cache \
    ca-certificates \
    less \
    ncurses-terminfo-base \
    krb5-libs \
    libgcc \
    libintl \
    libssl1.1 \
    libstdc++ \
    tzdata \
    userspace-rcu \
    zlib \
    icu-libs \
    curl

sudo apk -X https://dl-cdn.alpinelinux.org/alpine/edge/main add --no-cache \
    lttng-ust

# Download the powershell '.tar.gz' archive
curl -L https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-linux-alpine-x64.tar.gz -o /tmp/powershell.tar.gz

# Create the target folder where powershell will be placed
sudo mkdir -p /opt/microsoft/powershell/7

# Expand powershell to the target folder
sudo tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7

# Set execute permissions
sudo chmod +x /opt/microsoft/powershell/7/pwsh

# Create the symbolic link that points to pwsh
sudo ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh

# Start PowerShell
pwsh
```

## <a name="centos-7"></a>CentOS 7

> [!NOTE]
> このパッケージは Oracle Linux 7 で動作します。

### <a name="installation-via-package-repository-preferred---centos-7"></a>パッケージ リポジトリによるインストール (推奨) - CentOS 7

Linux 向け PowerShell は、インストールと更新を容易にするために、公式の Microsoft リポジトリに公開されています。

```sh
# Register the Microsoft RedHat repository
curl https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft.repo

# Install PowerShell
sudo yum install -y powershell

# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo yum update powershell` を使って PowerShell を更新できます。

### <a name="installation-via-direct-download---centos-7"></a>直接ダウンロードによるインストール - CentOS 7

[CentOS 7][] を利用し、[リリース][] ページから CentOS コンピューターに RPM パッケージ `powershell-7.1.1-1.rhel.7.x86_64.rpm` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo yum install powershell-7.1.1-1.rhel.7.x86_64.rpm
```

ダウンロードという中間の手順なしで RPM をインストールすることができます。

```sh
sudo yum install https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-1.rhel.7.x86_64.rpm
```

### <a name="uninstallation---centos-7"></a>アンインストール - CentOS 7

```sh
sudo yum remove powershell
```

[CentOS 7]: https://www.centos.org/download/

## <a name="red-hat-enterprise-linux-rhel-7"></a>Red Hat Enterprise Linux (RHEL) 7

### <a name="installation-via-package-repository-preferred---red-hat-enterprise-linux-rhel-7"></a>パッケージ リポジトリによるインストール (推奨) - Red Hat Enterprise Linux (RHEL) 7

Linux 向け PowerShell は、インストールと更新を容易にするために、公式の Microsoft リポジトリに公開されています。

```sh
# Register the Microsoft RedHat repository
curl https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft.repo

# Install PowerShell
sudo yum install -y powershell

# Start PowerShell
pwsh
```

スーパーユーザーとして、Microsoft リポジトリを 1 回登録します。 登録後は、`sudo yum update powershell` を使って PowerShell を更新できます。

### <a name="installation-via-direct-download---red-hat-enterprise-linux-rhel-7"></a>直接ダウンロードによるインストール - Red Hat Enterprise Linux (RHEL) 7

[リリース][] ページから Red Hat Enterprise Linux コンピューターに RPM パッケージ `powershell-7.1.1-1.rhel.7.x86_64.rpm` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo yum install powershell-7.1.1-1.rhel.7.x86_64.rpm
```

ダウンロードという中間の手順なしで RPM をインストールすることができます。

```sh
sudo yum install https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-1.rhel.7.x86_64.rpm
```

### <a name="uninstallation---red-hat-enterprise-linux-rhel-7"></a>アンインストール - Red Hat Enterprise Linux (RHEL) 7

```sh
sudo yum remove powershell
```

## <a name="opensuse"></a>openSUSE

### <a name="installation---opensuse-423"></a>インストール - openSUSE 42.3

```sh
# Install dependencies
zypper update && zypper --non-interactive install curl tar libicu52_1

# Download the powershell '.tar.gz' archive
curl -L https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-linux-x64.tar.gz -o /tmp/powershell.tar.gz

# Create the target folder where powershell will be placed
mkdir -p /opt/microsoft/powershell/7

# Expand powershell to the target folder
tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7

# Set execute permissions
chmod +x /opt/microsoft/powershell/7/pwsh

# Create the symbolic link that points to pwsh
ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh

# Start PowerShell
pwsh
```

### <a name="installation---opensuse-leap-15"></a>インストール - openSUSE Leap 15

```sh
# Install dependencies
zypper update && zypper --non-interactive install curl tar gzip libopenssl1_0_0 libicu60_2

# Download the powershell '.tar.gz' archive
curl -L https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-linux-x64.tar.gz -o /tmp/powershell.tar.gz

# Create the target folder where powershell will be placed
mkdir -p /opt/microsoft/powershell/7

# Expand powershell to the target folder
tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7

# Set execute permissions
chmod +x /opt/microsoft/powershell/7/pwsh

# Create the symbolic link that points to pwsh
ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh

# Start PowerShell
pwsh
```

### <a name="uninstallation---opensuse-423-opensuse-leap-15"></a>アンインストール - openSUSE 42.3、openSUSE Leap 15

```sh
rm -rf /usr/bin/pwsh /opt/microsoft/powershell
```

## <a name="fedora"></a>Fedora

> [!NOTE]
> Fedora 28 は、PowerShell 6.1 以降でのみサポートされます。

> [!NOTE]
> Fedora 29 および 30 は、PowerShell 7.0 以降でのみサポートされます。

### <a name="installation-via-package-repository-preferred---fedora-28-29-and-30"></a>パッケージ リポジトリによるインストール (推奨) - Fedora 28、29、および 30

Linux 向け PowerShell は、インストールと更新を容易にするために、公式の Microsoft リポジトリに公開されています。

```sh
# Register the Microsoft signature key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc

# Register the Microsoft RedHat repository
curl https://packages.microsoft.com/config/rhel/7/prod.repo | sudo tee /etc/yum.repos.d/microsoft.repo

# Update the list of products
sudo dnf check-update

# Install a system component
sudo dnf install compat-openssl10

# Install PowerShell
sudo dnf install -y powershell

# Start PowerShell
pwsh
```

### <a name="installation-via-direct-download---fedora-28-29-and-30"></a>直接ダウンロードによるインストール - Fedora 28、29、および 30

[リリース][] ページから Fedora コンピューターに RPM パッケージ `powershell-7.1.1-1.rhel.7.x86_64.rpm` をダウンロードします。

次に、ターミナルで、以下のコマンドを実行します。

```sh
sudo dnf install compat-openssl10
sudo dnf install powershell-7.1.1-1.rhel.7.x86_64.rpm
```

ダウンロードという中間の手順なしで RPM をインストールすることができます。

```sh
sudo dnf install compat-openssl10
sudo dnf install https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-1.rhel.7.x86_64.rpm
```

### <a name="uninstallation---fedora-28-29-and-30"></a>アンインストール - Fedora 28、29、および 30

```sh
sudo dnf remove powershell
```

## <a name="arch-linux"></a>Arch Linux

> [!NOTE]
> Arch のサポートは、Microsoft では正式にサポートされておらず、コミュニティによって維持されています。

PowerShell は [Arch Linux][] User Repository (AUR) から入手できます。

- [タグが付けられた最新のリリース][arch-release]でコンパイルできます
- [最新のコミットからマスターに][arch-git]コンパイルできます
- [最新のリリース バイナリ][arch-bin]でインストールできます

AUR のパッケージはコミュニティによって管理されています。公式のサポートは存在しません。

AUR からパッケージをインストールする方法については、[Arch Linux の wiki](https://wiki.archlinux.org/index.php/Arch_User_Repository#Installing_packages) か「[Docker での PowerShell の使用](powershell-in-docker.md)」を参照してください。

[Arch Linux]: https://www.archlinux.org/download/
[arch-release]: https://aur.archlinux.org/packages/powershell/
[arch-git]: https://aur.archlinux.org/packages/powershell-git/
[arch-bin]: https://aur.archlinux.org/packages/powershell-bin/

## <a name="snap-package"></a>Snap パッケージ

### <a name="getting-snapd"></a>Snapd の取得

Snap を実行するには、`snapd` が必要です。 `snapd` がインストールされているかどうかを確認するには、[こちらの手順](https://docs.snapcraft.io/core/install)を使用してください。

### <a name="installation-via-snap"></a>Snap を使用したインストール

Linux 向けの PowerShell は、インストールと更新を容易にするために [Snap ストア](https://snapcraft.io/store)に公開されています。

推奨される方法は次のとおりです。

```sh
# Install PowerShell
sudo snap install powershell --classic

# Start PowerShell
pwsh
```

プレビュー バージョンをインストールするには、次の方法を使用します。

```sh
# Install PowerShell
sudo snap install powershell-preview --classic

# Start PowerShell
pwsh-preview
```

インストールしたら、Snap は自動的にアップグレードされます。 `sudo snap refresh powershell` または `sudo snap refresh powershell-preview` を使って、アップグレードをトリガーすることができます。

### <a name="uninstallation"></a>アンインストール

```sh
sudo snap remove powershell
```

or

```sh
sudo snap remove powershell-preview
```

## <a name="kali"></a>Kali

> [!NOTE]
> Kali サポートは、Microsoft では正式にサポートされておらず、コミュニティによって維持されています。

### <a name="installation---kali"></a>インストール - Kali

```sh
# Install PowerShell package
apt update && apt -y install powershell

# Start PowerShell
pwsh
```

### <a name="uninstallation---kali"></a>アンインストール - Kali

```sh
# Uninstall PowerShell package
apt -y remove powershell
```

## <a name="raspbian"></a>Raspbian

> [!NOTE]
> Raspbian のサポートは試験段階です。

現在のところ、PowerShell は Raspbian Stretch でのみサポートされています。

CoreCLR と PowerShell は、Pi 2 および Pi 3 デバイス上でのみ動作します。[Pi Zero](https://github.com/dotnet/coreclr/issues/10605) のような他のデバイスには、サポート外のプロセッサが搭載されているためです。

[Raspbian Stretch](https://www.raspberrypi.org/downloads/raspbian/) をダウンロードし、[インストール手順](https://www.raspberrypi.org/documentation/installation/installing-images/README.md)に従って Pi にインストールしてください。

### <a name="installation---raspbian"></a>インストール - Raspbian

```sh
###################################
# Prerequisites

# Update package lists
sudo apt-get update

# Install libunwind8 and libssl1.0
# Regex is used to ensure that we do not install libssl1.0-dev, as it is a variant that is not required
sudo apt-get install '^libssl1.0.[0-9]$' libunwind8 -y

###################################
# Download and extract PowerShell

# Grab the latest tar.gz
wget https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-linux-arm32.tar.gz

# Make folder to put powershell
mkdir ~/powershell

# Unpack the tar.gz file
tar -xvf ./powershell-7.1.1-linux-arm32.tar.gz -C ~/powershell

# Start PowerShell
~/powershell/pwsh
```

必要に応じて、`pwsh` バイナリへのパスを指定せずに、シンボリック リンクを作成して PowerShell を起動することができます

```sh
# Start PowerShell from bash with sudo to create a symbolic link
sudo ~/powershell/pwsh -c New-Item -ItemType SymbolicLink -Path "/usr/bin/pwsh" -Target "$PSHOME/pwsh" -Force

# alternatively you can run following to create a symbolic link
# sudo ln -s ~/powershell/pwsh /usr/bin/pwsh

# Now to start PowerShell you can just run "pwsh"
```

### <a name="uninstallation---raspbian"></a>アンインストール - Raspbian

```sh
rm -rf ~/powershell
```

## <a name="installing-preview-releases"></a>プレビュー リリースのインストール

パッケージ リポジトリを介して、Linux 用の PowerShell Preview リリースをインストールすると、パッケージ名が `powershell` から `powershell-preview` に変わります。

直接ダウンロードによるインストールでは、ファイル名以外は変わりません。

さまざまなパッケージ マネージャーを使用して安定版およびプレビュー版パッケージをインストールするためのコマンドを、以下の表に示します。

| ディストリビューション |            安定したコマンド            |               プレビュー コマンド                |
| --------------- | ------------------------------------ | -------------------------------------------- |
| Ubuntu、Debian  | `sudo apt-get install -y powershell` | `sudo apt-get install -y powershell-preview` |
| CentOS、RedHat  | `sudo yum install -y powershell`     | `sudo yum install -y powershell-preview`     |
| Fedora          | `sudo dnf install -y powershell`     | `sudo dnf install -y powershell-preview`     |

## <a name="install-as-a-net-global-tool"></a>.NET グローバル ツールとしてインストールする

[.NET Core SDK](/dotnet/core/sdk) が既にインストールされている場合は、PowerShell を [.NET グローバル ツール](/dotnet/core/tools/global-tools)として簡単にインストールできます。

```
dotnet tool install --global PowerShell
```

dotnet tool install によって、`PATH` 環境変数に `~/.dotnet/tools` が追加されます。 ただし、現在実行中のシェルには更新された `PATH` が設定されていません。 新しいシェルからは、「`pwsh`」と入力すると PowerShell を起動できるはずです。

## <a name="binary-archives"></a>バイナリ アーカイブ

Linux プラットフォームで高度な展開シナリオを実行するために、PowerShell バイナリ `tar.gz` アーカイブが用意されています。

### <a name="dependencies"></a>依存関係

PowerShell はすべての Linux ディストリビューションに移植可能なバイナリをビルドします。 ただし、.NET Core ランタイムにはディストリビューションごとに異なる依存関係が必要であり、PowerShell でも同じです。

次の表は、公式にサポートされている Linux ディストリビューションと .NET Core 2.0 の依存関係をまとめたものです。

| OS                 | 依存関係 |
| ------------------ | ------------ |
| Ubuntu 16.04       | libc6、libgcc1、libgssapi-krb5-2、liblttng-ust0、libstdc++6、 <br> libcurl3、libunwind8、libuuid1、zlib1g、libssl1.0.0、libicu55 |
| Ubuntu 17.10       | libc6、libgcc1、libgssapi-krb5-2、liblttng-ust0、libstdc++6、 <br> libcurl3、libunwind8、libuuid1、zlib1g、libssl1.0.0、libicu57 |
| Ubuntu 18.04       | libc6、libgcc1、libgssapi-krb5-2、liblttng-ust0、libstdc++6、 <br> libcurl3、libunwind8、libuuid1、zlib1g、libssl1.0.0、libicu60 |
| Debian 8 (Jessie)  | libc6、libgcc1、libgssapi-krb5-2、liblttng-ust0、libstdc++6、 <br> libcurl3、libunwind8、libuuid1、zlib1g、libssl1.0.0、libicu52 |
| Debian 9 (Stretch) | libc6、libgcc1、libgssapi-krb5-2、liblttng-ust0、libstdc++6、 <br> libcurl3、libunwind8、libuuid1、zlib1g、libssl1.0.2、libicu57 |
| CentOS 7 <br> Oracle Linux 7 <br> RHEL 7 | libunwind、libcurl、openssl-libs、libicu |
| openSUSE 42.3 | libcurl4、libopenssl1_0_0、libicu52_1 |
| openSUSE Leap 15 | libcurl4、libopenssl1_0_0、libicu60_2 |
| Fedora 27 <br> Fedora 28 | libunwind、libcurl、openssl-libs、libicu、compat-openssl10 |

公式にサポートされていない Linux ディストリビューションに PowerShell バイナリを展開するには、別の手順によってターゲット OS に必要な依存関係をインストールする必要があります。 たとえば、[Amazon Linux Dockerfile][amazon-dockerfile] では依存関係を先にインストールし、それから Linux `tar.gz` アーカイブを抽出します。

[amazon-dockerfile]: https://github.com/PowerShell/PowerShell-Docker/blob/master/release/community-stable/amazonlinux/docker/Dockerfile

### <a name="installation---binary-archives"></a>インストール - バイナリ アーカイブ

#### <a name="linux"></a>Linux

```sh
# Download the powershell '.tar.gz' archive
curl -L -o /tmp/powershell.tar.gz https://github.com/PowerShell/PowerShell/releases/download/v7.1.1/powershell-7.1.1-linux-x64.tar.gz

# Create the target folder where powershell will be placed
sudo mkdir -p /opt/microsoft/powershell/7

# Expand powershell to the target folder
sudo tar zxf /tmp/powershell.tar.gz -C /opt/microsoft/powershell/7

# Set execute permissions
sudo chmod +x /opt/microsoft/powershell/7/pwsh

# Create the symbolic link that points to pwsh
sudo ln -s /opt/microsoft/powershell/7/pwsh /usr/bin/pwsh
```

### <a name="uninstalling-binary-archives"></a>バイナリ アーカイブのアンインストール

```sh
sudo rm -rf /usr/bin/pwsh /opt/microsoft/powershell
```

## <a name="paths"></a>パス

- `$PSHOME` は `/opt/microsoft/powershell/7/` です
- ユーザー プロファイルは `~/.config/powershell/profile.ps1` から読み込まれます
- 既定のプロファイルは `$PSHOME/profile.ps1` から読み込まれます
- ユーザー モジュールは `~/.local/share/powershell/Modules` から読み込まれます
- 共有モジュールは `/usr/local/share/powershell/Modules` から読み込まれます
- 既定のモジュールは `$PSHOME/Modules` から読み込まれます
- PSReadLine 履歴は、`~/.local/share/powershell/PSReadLine/ConsoleHost_history.txt` に記録されます

プロファイルは PowerShell のホスト別構成を順守します。そのため、既定のホスト固有プロファイルは同じ場所の `Microsoft.PowerShell_profile.ps1` にあります。

PowerShell では、Linux の [XDG ベース ディレクトリ仕様][xdg-bds]を尊重しています。

## <a name="installation-support"></a>インストールのサポート

Microsoft は、このドキュメントでインストール方法をサポートしています。 他のソースには、利用可能な別のインストール方法が存在する可能性があります。 そのようなツールや方法が役に立つものであっても、Microsoft は、そのような方法をサポートすることはできません。

<!-- link references -->
[リリース]: https://aka.ms/PowerShell-Release?tag=stable
[xdg-bds]: https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html
[distros]: https://github.com/dotnet/core/blob/master/release-notes/3.0/3.0-supported-os.md#linux
[Distribution Support Request]: https://github.com/PowerShell/PowerShell/issues/new?assignees=&labels=Distribution-Request&template=Distribution_Request.md&title=Distribution+Support+Request
