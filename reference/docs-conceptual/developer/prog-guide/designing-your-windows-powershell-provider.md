---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell プロバイダーを設計する
description: Windows PowerShell プロバイダーを設計する
ms.openlocfilehash: 89e1fa9cfc0a2e5928a358aad4244c8e9152fe1a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92654539"
---
# <a name="designing-your-windows-powershell-provider"></a>Windows PowerShell プロバイダーを設計する

製品または構成で、ユーザーが移動または参照するデータベースなど、格納されているデータのセットを公開する場合は、Windows PowerShell プロバイダーを実装する必要があります。 また、製品が複数レベルのコンテナーではなくても、コンテナーを提供している場合は、Windows PowerShell プロバイダーを実装するのが理にかなっています。 たとえば、コマンドレットのコピー、移動、名前の変更、新規作成、または削除を製品または構成データに対する操作として意味がある場合に、Windows PowerShell コンテナープロバイダーを実装することができます。

## <a name="windows-powershell-paths-identify-your-provider"></a>Windows PowerShell のパスでプロバイダーを識別する

Windows powershell ランタイムは、Windows powershell のパスを使用して、適切な Windows PowerShell プロバイダーにアクセスします。 コマンドレットでこれらのパスのいずれかを指定すると、ランタイムは、関連付けられたデータストアへのアクセスに使用するプロバイダーを認識します。 これらのパスには、ドライブ修飾パス、プロバイダー修飾パス、プロバイダー-直接パス、およびプロバイダー内部パスが含まれます。 各 Windows PowerShell プロバイダーは、これらのパスのうち1つ以上をサポートする必要があります。

Windows PowerShell のパスの詳細については、「Windows PowerShell のしくみ」を参照してください。

### <a name="defining-a-drive-qualified-path"></a>Drive-Qualified パスの定義

ユーザーが物理ドライブにあるデータにアクセスできるようにするには、Windows PowerShell プロバイダーがドライブ修飾パスをサポートしている必要があります。 このパスは、ドライブ名の後にコロン (:) など) で始まります。たとえば、mydrive:\ abc\bar. のようになります。

### <a name="defining-a-provider-qualified-path"></a>Provider-Qualified パスの定義

Windows PowerShell ランタイムがプロバイダーを初期化および初期化解除できるようにするには、Windows PowerShell プロバイダーがプロバイダー修飾パスをサポートしている必要があります。 たとえば、FileSystem:: \\ \uncshare\abc\bar は、Windows PowerShell によって提供されるファイルシステムプロバイダーのプロバイダー修飾パスです。

### <a name="defining-a-provider-direct-path"></a>Provider-Direct パスの定義

Windows PowerShell プロバイダーへのリモートアクセスを許可するには、現在の場所に対して Windows PowerShell プロバイダーに直接渡すプロバイダー-直接パスをサポートする必要があります。 たとえば、レジストリの Windows PowerShell プロバイダーでは、 \\ プロバイダーの直接のパスとして \ \ を使用できます。

### <a name="defining-a-provider-internal-path"></a>Provider-Internal パスの定義

プロバイダーコマンドレットが Windows PowerShell 以外のアプリケーションプログラミングインターフェイス (Api) を使用してデータにアクセスできるようにするには、Windows PowerShell プロバイダーがプロバイダー内部パスをサポートしている必要があります。 このパスは、プロバイダーによって修飾されたパスの "::" の後に示されます。 たとえば、プロバイダー-filesystem Windows PowerShell プロバイダーの内部パスは \uncshare\abc\bar. です。 \\

## <a name="changing-stored-data"></a>保存されたデータの変更

基になるデータストアを変更するメソッドをオーバーライドする場合は、常に、そのメソッドによって変更された項目の最新バージョンを使用して、常に system.string を[呼び出します。](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject) プロバイダーインフラストラクチャは、ユーザーが-PassThru パラメーターを指定した場合などに、項目オブジェクトをパイプラインに渡す必要があるかどうかを判断します。 最新の項目を取得する操作がコストのかかる操作である場合 (パフォーマンスを向上させる場合)、その結果の項目を実際に書き込む必要があるかどうかを判断するために、PassThru プロパティをテストできます。

## <a name="choose-a-base-class-for-your-provider"></a>プロバイダーの基本クラスを選択します

Windows PowerShell には、独自の Windows PowerShell プロバイダーを実装するために使用できる基本クラスが多数用意されています。 プロバイダーを設計するときは、このセクションで説明する基本クラスを選択します。これは、要件に最も適しています。

各 Windows PowerShell プロバイダーの基本クラスで、一連のコマンドレットを使用できるようになります。 ここではコマンドレットについて説明しますが、パラメーターについては説明しません。

Windows powershell ランタイムでは、セッション状態を使用して、、、 `Get-Location` `Set-Location` `Pop-Location` 、およびコマンドレットなど、特定の windows powershell プロバイダーでいくつかの場所のコマンドレットを使用でき `Push-Location` ます。 コマンドレットを使用し `Get-Help` て、これらの場所のコマンドレットに関する情報を取得できます。

### <a name="cmdletprovider-base-class"></a>"クラス \ プロバイダー" 基本クラス

System.servicemodel [プロバイダー](/dotnet/api/System.Management.Automation.Provider.CmdletProvider) クラスは、基本的な Windows PowerShell プロバイダーを定義します。 このクラスは、プロバイダー宣言をサポートし、すべての Windows PowerShell プロバイダーが使用できるさまざまなプロパティおよびメソッドを提供します。
クラスは、 `Get-PSProvider` セッションで使用可能なすべてのプロバイダーの一覧を取得するために、コマンドレットによって呼び出されます。
このコマンドレットの実装には、セッション状態があります。

> [!NOTE]
> Windows PowerShell プロバイダーは、すべての Windows PowerShell 言語スコープで使用できます。

### <a name="drivecmdletprovider-base-class"></a>DriveCmdletProvider 基本クラス

[Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider)クラスは、新しいドライブを追加したり、既存のドライブを削除したり、既定のドライブを初期化したりする操作をサポートする Windows PowerShell ドライブプロバイダーを定義します。 たとえば、Windows PowerShell によって提供される FileSystem プロバイダーは、ハードドライブや CD/DVD デバイスドライブなど、マウントされているすべてのボリュームのドライブを初期化します。

このクラスは、system.servicemodel [プロバイダー](/dotnet/api/System.Management.Automation.Provider.CmdletProvider) の基底クラスから派生します。 次の表に、このクラスによって公開されるコマンドレットの一覧を示します。 これらのコマンドレットに加えて、 `Get-PSDrive` 使用可能なドライブを取得するために使用されるコマンドレットもあります (セッション状態によって公開されます)。

|      コマンドレット      |                             定義                              |
| ---------------- | ------------------------------------------------------------------- |
| `New-PSDrive`    | セッションの新しいドライブを作成し、ドライブ情報をストリームします。 |
| `Remove-PSDrive` | セッションからドライブを削除します。                                   |

### <a name="itemcmdletprovider-base-class"></a>Itemの提供プロバイダーの基本クラス

System.string [クラスは、データ](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) ストアの個々の項目に対して操作を実行する Windows PowerShell 項目プロバイダーを定義します。また、コンテナーまたはナビゲーション機能を想定していません。 このクラスは、 [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) 基底クラスから派生します。 次の表に、このクラスによって公開されるコマンドレットの一覧を示します。

|     コマンドレット     |                                                                                                                                                            定義                                                                                                                                                            |
| -------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Clear-Item`   | 指定した場所にある項目の現在の内容を消去し、プロバイダーによって指定された "クリア" 値に置き換えます。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。                                                                                   |
| `Get-Item`     | 指定した位置から項目を取得し、結果のオブジェクトをストリームします。                                                                                                                                                                                                                                                  |
| `Invoke-Item`  | 指定したパスにある項目の既定のアクションを呼び出します。                                                                                                                                                                                                                                                                   |
| `Set-Item`     | 指定した位置に、指定した値を持つ項目を設定します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。                                                                                                                                                   |
| `Resolve-Path` | Windows PowerShell パスのワイルドカードを解決し、パス情報をストリームします。                                                                                                                                                                                                                                              |
| `Test-Path`    | 指定したパスをテストし、 `true` 存在する場合はを返し `false` ます。それ以外の場合はを返します。 このコマンドレットは、システムのパラメーターをサポートするために実装されています。 `IsContainer` [Writeitemobject *](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject) メソッド |

### <a name="containercmdletprovider-base-class"></a>ContainerCmdletProvider 基本クラス

[Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider)クラスは、データストア項目のコンテナーをユーザーに公開する Windows PowerShell コンテナープロバイダーを定義します。このプロバイダーは、コンテナーを公開します。 Windows PowerShell コンテナープロバイダーは、1つのコンテナー (入れ子になったコンテナーを含まない) に項目がある場合にのみ使用できます。 入れ子になったコンテナーがある場合は、Windows PowerShell ナビゲーションプロバイダーを実装する必要があります。

このクラスは、system.object の基底クラスから [派生して](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) います。 次の表は、このクラスによって実装されるコマンドレットを定義しています。

|     コマンドレット      |                                                                        定義                                                                        |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Copy-Item`     | ある場所から別の場所に項目をコピーします。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。 |
| `Get-Childitem` | 指定した位置にある子項目を取得し、それらをオブジェクトとしてストリームします。                                                                        |
| `New-Item`      | 指定した位置に新しい項目を作成し、結果のオブジェクトをストリームします。                                                                           |
| `Remove-Item`   | 指定された場所から項目を削除します。                                                                                                               |
| `Rename-Item`   | 指定した位置にある項目の名前を変更します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。 |

### <a name="navigationcmdletprovider-base-class"></a>Navigationのプロバイダーの基本クラス

System.string [クラスは](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) 、複数のコンテナーを使用する項目に対して操作を実行する Windows PowerShell ナビゲーションプロバイダーを定義します。 このクラスは、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider) 基底クラスから派生します。 次の表に、このクラスによって公開されるコマンドレットの一覧を示します。

|    コマンドレット    |                                                                      定義                                                                      |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| Combine-Path | パス間にプロバイダー固有の区切り記号を使用して、2つのパスを1つのパスに結合します。 このコマンドレットは、文字列をストリームします。                               |
| `Move-Item`  | 項目を指定された場所に移動します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。 |

関連するコマンドレットは、Windows PowerShell によって行う基本的な Parse-Path コマンドレットです。 このコマンドレットを使用すると、パラメーターをサポートするために Windows PowerShell のパスを解析でき `Parent` ます。 親パス文字列をストリームします。

## <a name="select-provider-interfaces-to-support"></a>サポートするプロバイダーインターフェイスの選択

Windows powershell の基本クラスの1つから派生するだけでなく、Windows PowerShell プロバイダーは、次の1つ以上のプロバイダーインターフェイスから派生することによって、他の機能をサポートできます。 このセクションでは、これらのインターフェイスと、それぞれでサポートされているコマンドレットを定義します。 インターフェイスでサポートされているコマンドレットのパラメーターについては説明しません。 コマンドレットのパラメーター情報は、コマンド `Get-Command` レットとコマンドレットを使用してオンラインで入手でき `Get-Help` ます。

### <a name="icontentcmdletprovider"></a>IContentCmdletProvider

[Icontentcmdletprovider](/dotnet/api/System.Management.Automation.Provider.IContentCmdletProvider)インターフェイスは、データ項目のコンテンツに対して操作を実行するコンテンツプロバイダーを定義します。 次の表に、このインターフェイスによって公開されるコマンドレットの一覧を示します。

|     コマンドレット      |                                                                                        定義                                                                                        |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Add-Content`   | 指定された値の長さを、指定した項目の内容に追加します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。 |
| `Clear-Content` | 指定した項目の内容を "クリア" 値に設定します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。               |
| `Get-Content`   | 指定した項目の内容を取得し、結果のオブジェクトをストリームします。                                                                                                         |
| `Set-Content`   | 指定した項目の既存の内容を置き換えます。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。                     |

### <a name="ipropertycmdletprovider"></a>IPropertyCmdletProvider

[Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider)インターフェイスは、データストア内の項目のプロパティに対して操作を実行する Windows PowerShell プロバイダーのプロパティを定義します。 次の表に、このインターフェイスによって公開されるコマンドレットの一覧を示します。

> [!NOTE]
> `Path`これらのコマンドレットのパラメーターは、プロパティを識別するのではなく、項目へのパスを示します。

|        コマンドレット        |                                                                                   定義                                                                                    |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Clear-ItemProperty` | 指定された項目のプロパティを "clear" 値に設定します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。      |
| `Get-ItemProperty`   | 指定した項目からプロパティを取得し、結果のオブジェクトをストリームします。                                                                                                |
| `Set-ItemProperty`   | 指定した値を使用して、指定した項目のプロパティを設定します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。 |

### <a name="idynamicpropertycmdletprovider"></a>IDynamicPropertyCmdletProvider

[Idynamicpropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IDynamicPropertyCmdletProvider)インターフェイスは、 [Ipropertycmdletprovider](/dotnet/api/System.Management.Automation.Provider.IPropertyCmdletProvider)から派生した、サポートされているコマンドレットの動的パラメーターを指定するプロバイダーを定義しています。 この型のプロバイダーは、実行時にプロパティを定義できる操作 (たとえば、新しいプロパティ操作) を処理します。 静的に定義されたプロパティを持つアイテムでは、このような操作は実行できません。
次の表に、このインターフェイスによって公開されるコマンドレットの一覧を示します。

|        コマンドレット         |                                                                                定義                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `Copy-ItemProperty`   | 指定した項目から別の項目にプロパティをコピーします。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。 |
| `Move-ItemProperty`   | 指定した項目から別の項目にプロパティを移動します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。  |
| `New-ItemProperty`    | 指定した項目に対してプロパティを作成し、結果のオブジェクトをストリームします。                                                                                             |
| `Remove-ItemProperty` | 指定した項目のプロパティを削除します。                                                                                                                              |
| `Rename-ItemProperty` | 指定した項目のプロパティの名前を変更します。 このコマンドレットは、 `PassThru` パラメーターが指定されていない限り、パイプラインを介して出力オブジェクトを渡しません。                 |

### <a name="isecuritydescriptorcmdletprovider"></a>Isecurity記述子の提供プロバイダー

System.servicemodel [プロバイダー](/dotnet/api/System.Management.Automation.Provider.ISecurityDescriptorCmdletProvider) インターフェイスは、セキュリティ記述子の機能をプロバイダーに追加しています。 このインターフェイスを使用すると、ユーザーはデータストア内の項目のセキュリティ記述子情報を取得して設定できます。 次の表に、このインターフェイスによって公開されるコマンドレットの一覧を示します。

|  コマンドレット   |                                                                                                                                                                                                          定義                                                                                                                                                                                                          |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Get-Acl` | ファイルやオブジェクトなど、オペレーティングシステムのリソースを保護するために使用されるセキュリティ記述子の一部であるアクセス制御リスト (ACL) に含まれる情報を取得します。                                                                                                                                                                                                                                      |
| `Set-Acl` | ACL の情報を設定します。 これは、指定されたパスに対して指定された項目の [accesscontrol-namespace](/dotnet/api/System.Security.AccessControl.ObjectSecurity) のインスタンスの形式です。 このコマンドレットでは、Windows PowerShell プロバイダーがセキュリティ情報の設定をサポートしている場合に、レジストリ内のファイル、キー、サブキーに関する情報、またはその他のプロバイダー項目に関する情報を設定できます。 |

## <a name="see-also"></a>参照

[Windows PowerShell プロバイダーの作成](./how-to-create-a-windows-powershell-provider.md)

[Windows PowerShell の動作](/previous-versions/ms714658(v=vs.85))

[Windows PowerShell SDK](../windows-powershell-reference.md)
