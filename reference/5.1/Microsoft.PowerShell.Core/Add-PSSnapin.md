---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/add-pssnapin?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Add-PSSnapin
ms.openlocfilehash: a21c2974fd66a9b02929752ae487c8995579b8a7
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388826"
---
# Add-PSSnapin

## 概要
現在のセッションに 1 つまたは複数の Windows PowerShell スナップインを追加します。

## SYNTAX

```
Add-PSSnapin [-Name] <String[]> [-PassThru] [<CommonParameters>]
```

## Description

コマンドレットでは、 `Add-PSSnapin` 登録済みの Windows PowerShell スナップインを現在のセッションに追加します。 スナップインを追加した後、スナップインが現在のセッションでサポートするコマンドレットとプロバイダーを使用できます。

今後のすべての Windows PowerShell セッションにスナップインを追加するには、 `Add-PSSnapin` Windows powershell プロファイルにコマンドを追加します。 詳細については、「 [about_Profiles](about/about_Profiles.md)」を参照してください。

Windows PowerShell 3.0 以降、Windows PowerShell に含まれているコア コマンドはモジュールにパッケージ化されています。 例外は、スナップイン (PSSnapin) の **Microsoft.PowerShell.Core** です。
既定では、 **Microsoft.PowerShell.Core** スナップインのみがセッションに追加されます。 モジュールは初回使用時に自動的にインポートされ、Import-Module コマンドレットを使用してインポートできます。

## 例

### 例 1: スナップインを追加する

```powershell
PS C:\> Add-PSSnapIn -Name Microsoft.Exchange, Microsoft.Windows.AD
```

このコマンドは、現在のセッションに、Microsoft Exchange と Active Directory のスナップインを追加します。

### 例 2: 登録されているすべてのスナップインを追加する

```powershell
PS C:\> Get-PSSnapin -Registered | Add-PSSnapin -Passthru
```

このコマンドは、セッションにすべての登録済み Windows PowerShell スナップインを追加します。 また **、登録済みのパラメーターと** 共に Get-PSSnapin コマンドレットを使用して、登録された各スナップインを表すオブジェクトを取得します。パイプライン演算子 (|) により、結果がに渡され `Add-PSSnapin` 、セッションに追加されます。 **PassThru** パラメーターは、追加された各スナップインを表すオブジェクトを返します。

### 例 3: スナップインを登録して追加する

最初のコマンドは、Windows PowerShell でインストールされたスナップインを含む現在のセッションに追加されているスナップインを取得します。 この例では、ManagementFeatures は返されません。 これは、セッションに追加されていないことを示しています。

2番目のコマンドは、既にセッションに追加されているスナップインを含む、システムに登録されているスナップインを取得します。 Windows PowerShell と共にインストールされるスナップインは含まれません。 この場合、コマンドはスナップインを返しません。これは、ManagementFeatures スナップインがシステムに登録されていないことを示します。

3番目のコマンドは、.NET Framework の Installutil.exe ツールのパスに対して、installutil.exe というエイリアスを作成します。

4番目のコマンドは、Installutil.exe ツールを使用してスナップインを登録します。 このコマンドでは、スナップインの ManagementCmdlets.dll のパス、ファイル名、またはモジュール名を指定します。

5 番目のコマンドは、2 番目のコマンドと同じです。 今度は、それを使用して ManagementCmdlets スナップインが登録されていることを確認します。

6番目のコマンドは、コマンドレットを使用して、 `Add-PSSnapin` ManagementFeatures スナップインをセッションに追加します。 これは、ファイル名ではなく、スナップインの名前、ManagementFeatures を指定します。

スナップインがセッションに追加されたことを確認するために、7番目のコマンドは、Get-Command コマンドレットの **Module** パラメーターを使用します。 スナップインまたはモジュールによってセッションに追加された項目が表示されます。

コマンドレットが返すオブジェクトの **add-pssnapin** プロパティを使用して、コマンドレットが生成された `Get-Command` スナップインまたはモジュールを検索することもできます。 8番目のコマンドは、ドット表記を使用して Set-Alias コマンドレットの Add-pssnapin プロパティの値を検索します。

```powershell
PS C:\> Get-PSSnapin
PS C:\> Get-PSSnapin -Registered
PS C:\> Set-Alias installutil $env:windir\Microsoft.NET\Framework\v2.0.50727\installutil.exe
PS C:\> installutil C:\Dev\Management\ManagementCmdlets.dll
PS C:\> Get-PSSnapin -Registered
PS C:\> add-pssnapin ManagementFeatures
PS C:\> Get-Command -Module ManagementFeatures
PS C:\> (Get-Command Set-Alias).pssnapin
```

この例では、スナップインをシステムに登録し、さらにセッションに追加するプロセスを示します。 ManagementFeatures を使用します。これは、ManagementCmdlets.dll という名前のファイルで実装された架空のスナップインです。

## PARAMETERS

### -Name

スナップインの名前を指定します。 これは、AssemblyName や ModuleName ではなく、名前です。 ワイルドカードを使用できます。

システムに登録されているスナップインの名前を確認するには、「」と入力 `Get-PSSnapin -Registered` します。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: True
```

### -PassThru

このコマンドレットが、追加された各スナップインを表すオブジェクトを返すことを示します。 既定では、このコマンドレットによる出力はありません。

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

### 共通パラメーター

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### なしまたはシステムの管理. PSSnapInInfo

このコマンドレットは、 **PassThru** パラメーターを指定した場合に、スナップインを表す PSSnapInInfo オブジェクトを返します。 それ以外の場合、このコマンドレットによる出力はありません。

## 注

- Windows PowerShell 3.0 以降、Windows PowerShell と共にインストールされるコア コマンドはモジュールにパッケージ化されています。 Windows PowerShell 2.0、およびそれ以降のバージョンの Windows PowerShell で古いスタイルのセッションを作成するホストプログラムでは、コアコマンドはスナップイン (PSSnapins) にパッケージ化されます。 例外は、常にスナップインである、 **PowerShell です** 。 また、New-PSSession コマンドレットによって開始されたリモートセッションは、コアスナップインを含む古い形式のセッションです。

  コアモジュールで新しいスタイルのセッションを作成する **CreateDefault2** メソッドの詳細については、「 [CreateDefault2 メソッド](/dotnet/api/system.management.automation.runspaces.initialsessionstate.createdefault2#System_Management_Automation_Runspaces_InitialSessionState_CreateDefault2)」を参照してください。

- スナップインの詳細については、「 [about_PSSnapins](About/about_PSSnapins.md) 」および「 [Windows PowerShell スナップインを作成する方法](/powershell/scripting/developer/cmdlet/how-to-create-a-windows-powershell-snap-in)」を参照してください。
- `Add-PSSnapin` 現在のセッションにのみ、スナップインを追加します。 スナップインをすべての Windows PowerShell セッションに追加するには、そのスナップインを Windows PowerShell プロファイルに追加します。 詳細については、「about_Profiles」を参照してください。
- Microsoft .NET Framework インストールユーティリティを使用して登録されたスナップインを追加できます。 詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。
- コンピューターに登録されているスナップインの一覧を取得するには、「」と入力 `Get-PSSnapin -Registered` します。
- スナップインを追加する前に、スナップインのバージョンを確認して、 `Add-PSSnapin` 現在のバージョンの Windows PowerShell と互換性があることを確認します。 スナップインがバージョン チェックに失敗した場合、Windows PowerShell はエラーを報告します。

## 関連リンク

[Get-PSSnapin](Get-PSSnapin.md)

[Remove-PSSnapin](Remove-PSSnapin.md)
