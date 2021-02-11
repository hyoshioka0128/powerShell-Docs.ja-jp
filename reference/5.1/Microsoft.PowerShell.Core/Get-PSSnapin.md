---
external help file: System.Management.Automation.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Core
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/get-pssnapin?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Get-PSSnapin
ms.openlocfilehash: 156cadecd87910e3c3312e84929b16709770641d
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388673"
---
# Get-PSSnapin

## 概要
コンピューター上の Windows PowerShell スナップインを取得します。

## SYNTAX

```
Get-PSSnapin [[-Name] <String[]>] [-Registered] [<CommonParameters>]
```

## Description

この `Get-PSSnapin` コマンドレットは、現在のセッションに追加されている、またはシステムに登録されている Windows PowerShell スナップインを取得します。 このコマンドレットは、スナップインが検出された順に一覧表示します。

`Get-PSSnapin` 登録されたスナップインのみを取得します。Windows PowerShell スナップインを登録するには、Microsoft .NET Framework 2.0 に含まれている Installutil.exe ツールを使用します。 詳細については、「 [コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions//ms714644(v=vs.85))」を参照してください。

Windows PowerShell 3.0 以降では、Windows PowerShell に含まれるコアコマンドはモジュールにパッケージ化されています。 例外は、スナップイン (PSSnapin) の **Microsoft.PowerShell.Core** です。
既定では、 **Microsoft.PowerShell.Core** スナップインのみがセッションに追加されます。 モジュールは初回使用時に自動的にインポートされ、コマンドレットを使用して `Import-Module` インポートできます。

## 例

### 例 1: 現在読み込まれているスナップインを取得する

```
PS C:\> Get-PSSnapIn
```

このコマンドは、セッションで現在読み込まれている Windows PowerShell スナップインを取得します。 これには、Windows PowerShell でインストールされたスナップインや、セッションに追加されたスナップインが含まれます。

### 例 2: 登録されているスナップインを取得する

```
PS C:\> get-PSSnapIn -Registered
```

このコマンドは、コンピューターに登録された Windows PowerShell スナップイン (既にセッションに追加されているスナップインを含む) を取得します。 Windows PowerShell でインストールされたスナップイン、またはシステムに登録されていない Windows PowerShell スナップインのダイナミック リンク ライブラリ (DLL) は、出力に含まれません。

### 例 3: 文字列に一致する現在のスナップインを取得する

```
PS C:\> Get-PSSnapIn -Name smp*
```

このコマンドは、現在のセッションで、名前が smp で始まる Windows PowerShell スナップインを取得します。

## PARAMETERS

### -Name

スナップイン名の配列を指定します。 このコマンドレットは、指定された Windows PowerShell スナップインのみを取得します。ワイルドカード文字を使用できます。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -登録済み

このコマンドレットが、まだセッションに追加されていない場合でも、システムに登録されている Windows PowerShell スナップインを取得することを示します。

Windows PowerShell でインストールされたスナップインは、この一覧に表示されません。

このパラメーターを指定しない場合、 `Get-PSSnapin` セッションに追加されている Windows PowerShell スナップインを取得します。

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
パイプを使用してこのコマンドレットに入力を渡すことはできません。

## 出力

### システム管理. PSSnapInInfo

`Get-PSSnapin` 取得した各スナップインのオブジェクトを返します。

## 注

Windows powershell 3.0 以降では、Windows PowerShell と共にインストールされるコアコマンドはモジュールにパッケージ化されています。 Windows PowerShell 2.0、およびそれ以降のバージョンの Windows PowerShell で古いスタイルのセッションを作成するホストプログラムでは、コアコマンドはスナップイン ( **add-pssnapin** ) にパッケージ化されます。 例外は、常にスナップインである、 **PowerShell です** 。 また、コマンドレットによって開始されるようなリモートセッション `New-PSSession` は、コアスナップインを含む古いスタイルのセッションです。

 コアモジュールで新しいスタイルのセッションを作成する **CreateDefault2** メソッドの詳細については、「 [CreateDefault2 メソッド](/dotnet/api/system.management.automation.runspaces.initialsessionstate.createdefault2#System_Management_Automation_Runspaces_InitialSessionState_CreateDefault2)」を参照してください。

## 関連リンク

[Add-PSSnapin](Add-PSSnapin.md)

[Remove-PSSnapin](Remove-PSSnapin.md)
