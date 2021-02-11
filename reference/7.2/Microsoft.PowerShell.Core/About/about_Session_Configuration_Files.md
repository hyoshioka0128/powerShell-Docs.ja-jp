---
description: セッション構成 (エンドポイントとも呼ばれます) でセッション構成を使用するセッションの環境を定義するために使用されるセッション構成ファイルについて説明します。
Locale: en-US
ms.date: 01/03/2018
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_session_configuration_files?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Session_Configuration_Files
ms.openlocfilehash: 1ed87e95b2ce823b5a887546545e1158630eabfb
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99599554"
---
# <a name="about-session-configuration-files"></a>セッション構成ファイルについて

## <a name="short-description"></a>概要
セッション構成を使用するセッションの環境を定義するためにセッション構成 ("エンドポイント" とも呼ばれます) で使用されるセッション構成ファイルについて説明します。

## <a name="long-description"></a>詳細説明

"セッション構成ファイル" は、セッション構成のプロパティと値のハッシュテーブルを含む、.pssc ファイル名拡張子を持つテキストファイルです。 セッション構成ファイルを使用すると、セッション構成のプロパティを設定できます。 これにより、そのセッション構成を使用するすべての PowerShell セッションの環境が定義されます。

セッション構成ファイルを使用すると、複雑な C# アセンブリまたはスクリプトを使用しなくても、カスタムセッション構成を簡単に作成できます。

"セッション構成" または "エンドポイント" とは、コンピューター上でセッションを作成できるユーザーを決定する、ローカルコンピューターの設定のコレクションです。これらのセッションでユーザーが実行できるコマンドまた、セッションを特権仮想アカウントとして実行するかどうかを指定します。 セッション構成の詳細については、「 [about_Session_Configurations](about_Session_Configurations.md)」を参照してください。

セッション構成は Windows PowerShell 2.0 で導入され、セッション構成ファイルは Windows PowerShell 3.0 で導入されました。 セッション構成ファイルをセッション構成に含めるには、Windows PowerShell 3.0 を使用する必要があります。 ただし、Windows PowerShell 2.0 (以降) のユーザーは、セッション構成の設定の影響を受けます。

## <a name="creating-custom-sessions"></a>カスタムセッションの作成

セッション構成でセッションプロパティを指定することによって、PowerShell セッションの多くの機能をカスタマイズできます。 セッションをカスタマイズするには、カスタム実行空間を定義する C# プログラムを作成するか、セッション構成ファイルを使用して、セッション構成を使用して作成されたセッションのプロパティを定義します。 一般的な規則として、C# プログラムを記述するよりも、セッション構成ファイルを使用する方が簡単です。

セッション構成ファイルを使用すると、高度に信頼されたユーザーのために、完全に機能するセッションなどの項目を作成できます。最小限のアクセスを許可するロックダウンされたセッション特に設計されたセッション。これらのタスクに必要なモジュールのみが含まれています。特権のないユーザーが特定のコマンドを特権アカウントとしてのみ実行できるようにするセッション。

これに加えて、セッションのユーザーがスクリプトブロックなどの PowerShell 言語要素を使用できるかどうかや、コマンドを実行できるかどうかを管理できます。 セッションで実行できる PowerShell ユーザーのバージョンを管理できます。どのモジュールをセッションにインポートするかを管理します。また、ユーザーが実行できるコマンドレット、関数、およびエイリアスを管理します。 RoleDefinitions フィールドを使用すると、グループのメンバーシップに基づいて、ユーザーにセッションのさまざまな機能を与えることができます。

RoleDefinitions とこの値の定義方法の詳細については、New-PSRoleCapabilityFile コマンドレットのヘルプトピックを参照してください。

## <a name="creating-a-session-configuration-file"></a>セッション構成ファイルの作成

セッション構成ファイルを作成する最も簡単な方法は、New-PSSessionConfigurationFile コマンドレットを使用することです。 このコマンドレットは、正しい構文と形式を使用し、構成ファイルのプロパティ値の多くを自動的に検証するファイルを生成します。

セッション構成ファイルで設定できるプロパティの詳細については、New-PSSessionConfigurationFile コマンドレットのヘルプトピックを参照してください。

次のコマンドは、既定値を使用するセッション構成ファイルを作成します。 パスパラメーター (ファイルパスを指定する) 以外のパラメーターが含まれていないため、結果の構成ファイルでは既定値のみが使用されます。

```powershell
New-PSSessionConfigurationFile -Path .\Defaults.pssc
```

既定のテキストエディターで新しい構成ファイルを表示するには、次のコマンドを使用します。

```powershell
Invoke-Item -Path .\Defaults.pssc
```

ユーザーがコマンドを実行でき、PowerShell 言語の他の要素を使用しないセッションのセッション構成を作成するには、次のように入力します。

```powershell
New-PSSessionConfigurationFile -LanguageMode NoLanguage
-Path .\NoLanguage.pssc
```

前のコマンドで、LanguageMode パラメーターを NoLanguage に設定すると、ユーザーはスクリプトの記述や実行、変数の使用などを実行できなくなります。

ユーザーが Get コマンドレットのみを使用できるセッションのセッション構成を作成するには、次のように入力します。

```powershell
New-PSSessionConfigurationFile -VisibleCmdlets Get-*
-Path .\GetSessions.pssc
```

前の例では、VisibleCmdlets パラメーターを Get-* に設定すると、ユーザーは、文字列値 "Get-" で始まる名前を持つコマンドレットに制限されます。

ユーザーの資格情報ではなく、特権のある仮想アカウントで実行されるセッションのセッション構成を作成するには、次のように入力します。

```powershell
New-PSSessionConfigurationFile -RunAsVirtualAccount
-Path .\VirtualAccount.pssc
```

ユーザーに表示されるコマンドがロール機能ファイルに指定されているセッションのセッション構成を作成するには、次のように入力します。

```powershell
New-PSSessionConfigurationFile -RoleDefinitions
@{ 'CONTOSO\User' = @{ RoleCapabilities = 'Maintenance' }}
-Path .\Maintenance.pssc
```

### <a name="using-a-session-configuration-file"></a>セッション構成ファイルの使用

セッション構成を作成するとき、または追加するときに、セッション構成ファイルを含めることができます。後でセッション構成にファイルを追加できます。

セッション構成を作成するときにセッション構成ファイルを含めるには、Register-PSSessionConfiguration コマンドレットの Path パラメーターを使用します。

たとえば、次のコマンドでは、nolanguage セッション構成を作成するときに、.pssc ファイルを使用します。

```powershell
Register-PSSessionConfiguration -Name NoLanguage
-Path .\NoLanguage.pssc
```

新しい NoLanguage セッションが開始されると、ユーザーは PowerShell コマンドにのみアクセスできるようになります。

セッション構成ファイルを既存のセッション構成に追加するには、Set-PSSessionConfiguration コマンドレットと Path パラメーターを使用します。 これは、指定されたセッション構成で作成された新しいセッションに影響します。 Set-PSSessionConfiguration コマンドレットはセッション自体を変更し、セッション構成ファイルは変更しないことに注意してください。

たとえば、次のコマンドは、LockedDown セッション構成に NoLanguage ファイルを追加します。

```powershell
Set-PSSessionConfiguration -Name LockedDown
-Path .\NoLanguage.pssc
```

ユーザーは、LockedDown セッション構成を使用してセッションを作成するときに、コマンドレットを実行できますが、変数の作成または使用、値の割り当て、または他の PowerShell 言語要素の使用はできません。

次のコマンドは、New-PSSession コマンドレットを使用して、LockedDown セッション構成を使用するコンピューター Srv01 上にセッションを作成し、そのセッションへのオブジェクト参照を $s 変数に保存します。 セッション構成の ACL (アクセス制御リスト) は、セッションの作成に使用できるユーザーを決定します。

```powershell
$s = New-PSSession -ComputerName Srv01
-ConfigurationName LockedDown
```

NoLanguage 制約は LockedDown セッション構成に追加されたため、LockedDown セッションのユーザーは PowerShell コマンドとコマンドレットのみを実行できます。 たとえば、次の2つのコマンドでは、Invoke-Command コマンドレットを使用して、$s 変数で参照されているセッションでコマンドを実行します。 最初のコマンドは、Get-UICulture コマンドレットを実行し、どの変数も使用しません。 $PSUICulture 変数の値を取得する2番目のコマンドは失敗します。

```
Invoke-Command -Session $s {Get-UICulture}
en-US

Invoke-Command -Session $s {$PSUICulture}
The syntax is not supported by this runspace. This might be
because it is in no-language mode.
+ CategoryInfo          : ParserError: ($PSUICulture:String) [],
ParseException
+ FullyQualifiedErrorId : ScriptsNotAllowed
```

## <a name="editing-a-session-configuration-file"></a>セッション構成ファイルを編集する

RunAsVirtualAccount および RunAsVirtualAccountGroups を除くセッション構成のすべての設定は、セッション構成で使用されるセッション構成ファイルを編集することによって変更できます。 これを行うには、まず、セッション構成ファイルのアクティブなコピーを検索します。

セッション構成でセッション構成ファイルを使用すると、PowerShell によってセッション構成ファイルのアクティブなコピーが作成され、 \$ \\ ローカルコンピューターの pshome sessionconfig ディレクトリに格納されます。

セッション構成ファイルのアクティブなコピーの場所は、セッション構成オブジェクトの ConfigFilePath プロパティに格納されます。

次のコマンドは、NoLanguage セッション構成のセッション構成ファイルの場所を取得します。

```powershell
(Get-PSSessionConfiguration -Name NoLanguage).ConfigFilePath
```

このコマンドは、次のようなファイルパスを返します。

```
C:\WINDOWS\System32\WindowsPowerShell\v1.0\SessionConfig\
NoLanguage_0c115179-ff2a-4f66-a5eb-e56e5692ba22.pssc
```

.Pssc ファイルは、任意のテキストエディターで編集できます。 ファイルが保存されると、そのセッション構成を使用する新しいセッションによって使用されます。

RunAsVirtualAccount または RunAsVirtualAccountGroups の設定を変更する必要がある場合は、セッション構成を登録解除し、編集した値を含むセッション構成ファイルを再登録する必要があります。

## <a name="testing-a-session-configuration-file"></a>セッション構成ファイルをテストする

Test-PSSessionConfigurationFile コマンドレットを使用して、手動で編集されたセッション構成ファイルをテストします。 これは重要なことです。ファイルの構文と値が有効でない場合、ユーザーはセッション構成を使用してセッションを作成することはできません。

たとえば、次のコマンドは、NoLanguage セッション構成のアクティブなセッション構成ファイルをテストします。

```powershell
Test-PSSessionConfigurationFile -Path C:\WINDOWS\System32\
WindowsPowerShell\v1.0\SessionConfig\
NoLanguage_0c115179-ff2a-4f66-a5eb-e56e5692ba22.pssc
```

構成ファイルの構文と値が有効な場合 Test-PSSessionConfigurationFile は True を返します。 構文と値が有効でない場合、コマンドレットは False を返します。

Test-PSSessionConfigurationFile を使用すると、New-PSSessionConfiguration のコマンドレットによって作成されたファイルを含む、任意のセッション構成ファイルをテストできます。 詳細については、Test-PSSessionConfigurationFile コマンドレットのヘルプトピックを参照してください。

## <a name="removing-a-session-configuration-file"></a>セッション構成ファイルの削除

セッション構成からセッション構成ファイルを削除することはできません。
ただし、ファイルは、既定の設定を使用する新しいファイルに置き換えることができます。 これにより、元の構成ファイルで使用されている設定が実質的にキャンセルされます。

セッション構成ファイルを置き換えるには、既定の設定を使用する新しいセッション構成ファイルを作成し、Set-PSSessionConfiguration コマンドレットを使用してカスタムセッション構成ファイルを新しいファイルに置き換えます。

たとえば、次のコマンドを使用すると、既定のセッション構成ファイルが作成され、NoLanguage セッション構成のアクティブなセッション構成ファイルが置き換えられます。

```powershell
New-PSSessionConfigurationFile -Path .\Default.pssc
Set-PSSessionConfiguration -Name NoLanguage
-Path .\Default.pssc
```

これらのコマンドが完了すると、NoLanguage セッション構成では、そのセッション構成を使用して作成されたすべてのセッションに対して、完全な言語サポート (既定の設定) が提供されます。

セッション構成のプロパティの表示セッション構成ファイルを使用してセッション構成を表すセッション構成オブジェクトには、セッション構成を簡単に検出して分析できるようにする追加のプロパティがあります。 (次に示す型名には、書式設定されたビュー定義が含まれていることに注意してください)。Get-PSSessionConfiguration コマンドレットを実行してプロパティを表示し、返されたデータを Get-Member コマンドレットにパイプすることができます。

```powershell
Get-PSSessionConfiguration NoLanguage | Get-Member
```

```output
TypeName: Microsoft.PowerShell.Commands.PSSessionConfigurationCommands
#PSSessionConfiguration

Name                          MemberType     Definition
----                          ----------     ----------
Equals                        Method         bool Equals(System.O...
GetHashCode                   Method         int GetHashCode()
GetType                       Method         type GetType()
ToString                      Method         string ToString()
Architecture                  NoteProperty   System.String Archit...
Author                        NoteProperty   System.String Author...
AutoRestart                   NoteProperty   System.String AutoRe...
Capability                    NoteProperty   System.Object[] Capa...
CompanyName                   NoteProperty   System.String Compan...
configfilepath                NoteProperty   System.String config...
Copyright                     NoteProperty   System.String Copyri...
Enabled                       NoteProperty   System.String Enable...
ExactMatch                    NoteProperty   System.String ExactM...
ExecutionPolicy               NoteProperty   System.String Execut...
Filename                      NoteProperty   System.String Filena...
GUID                          NoteProperty   System.String GUID=0...
ProcessIdleTimeoutSec         NoteProperty   System.String Proces...
IdleTimeoutms                 NoteProperty   System.String IdleTi...
lang                          NoteProperty   System.String lang=e...
LanguageMode                  NoteProperty   System.String Langua...
MaxConcurrentCommandsPerShell NoteProperty   System.String MaxCon...
MaxConcurrentUsers            NoteProperty   System.String MaxCon...
MaxIdleTimeoutms              NoteProperty   System.String MaxIdl...
MaxMemoryPerShellMB           NoteProperty   System.String MaxMem...
MaxProcessesPerShell          NoteProperty   System.String MaxPro...
MaxShells                     NoteProperty   System.String MaxShells
MaxShellsPerUser              NoteProperty   System.String MaxShe...
Name                          NoteProperty   System.String Name=N...
PSVersion                     NoteProperty   System.String PSVersion
ResourceUri                   NoteProperty   System.String Resour...
RunAsPassword                 NoteProperty   System.String RunAsP...
RunAsUser                     NoteProperty   System.String RunAsUser
SchemaVersion                 NoteProperty   System.String Schema...
SDKVersion                    NoteProperty   System.String SDKVer...
OutputBufferingMode           NoteProperty   System.String Output...
SessionType                   NoteProperty   System.String Sessio...
UseSharedProcess              NoteProperty   System.String UseSha...
SupportsOptions               NoteProperty   System.String Suppor...
xmlns                         NoteProperty   System.String xmlns=...
XmlRenderingType              NoteProperty   System.String XmlRen...
Permission                    ScriptProperty System.Object Permis...
```

これらのプロパティを使用すると、特定のセッション構成を簡単に検索できます。
たとえば、Set-executionpolicy プロパティを使用して、RemoteSigned 実行ポリシーを持つセッションをサポートするセッション構成を見つけることができます。
Set-executionpolicy プロパティは、セッション構成ファイルを使用するセッションにのみ存在するため、適合するすべてのセッション構成が返されない場合があることに注意してください。

```powershell
Get-PSSessionConfiguration |
where {$_.ExecutionPolicy -eq "RemoteSigned"}
```

次のコマンドは、RunAsUser が Exchange 管理者であるセッション構成を取得します。

```powershell
 Get-PSSessionConfiguration |
where {$_.RunAsUser -eq "Exchange01\Admin01"}
```

構成に関連付けられているロールの定義に関する情報を表示するには、Get-PSSessionCapability コマンドレットを使用します。 このコマンドレットを使用すると、特定のエンドポイント内の特定のユーザーが使用できるコマンドと環境を確認できます。

## <a name="notes"></a>注

セッション構成では、"空の" セッションと呼ばれる種類のセッションもサポートされます。 空のセッションの種類を使用すると、選択したコマンドでカスタムセッションを作成できます。 モジュール、関数、またはスクリプトを空のセッションに追加しない場合、セッションは式に限定され、実用的に使用されない可能性があります。 SessionType プロパティは、空のセッションを使用しているかどうかを示します。

## <a name="see-also"></a>関連項目

[about_Session_Configurations](about_Session_Configurations.md)

[New-PSSession](xref:Microsoft.PowerShell.Core.New-PSSession)

[Disable-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Disable-PSSessionConfiguration)

[Enable-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Enable-PSSessionConfiguration)

[Get-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Get-PSSessionConfiguration)

[New-PSSessionConfigurationFile](xref:Microsoft.PowerShell.Core.New-PSSessionConfigurationFile)

[Register-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Register-PSSessionConfiguration)

[Set-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Set-PSSessionConfiguration)

[Test-PSSessionConfigurationFile](xref:Microsoft.PowerShell.Core.Test-PSSessionConfigurationFile)

[Unregister-PSSessionConfiguration](xref:Microsoft.PowerShell.Core.Unregister-PSSessionConfiguration)

[Get-PSSessionCapability](xref:Microsoft.PowerShell.Core.Get-PSSessionCapability)

[New-PSRoleCapabilityFile](xref:Microsoft.PowerShell.Core.New-PSRoleCapabilityFile)

