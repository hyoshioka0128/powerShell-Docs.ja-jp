---
external help file: Microsoft.PowerShell.ConsoleHost.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Host
ms.date: 01/26/2021
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.host/start-transcript?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Start-Transcript
ms.openlocfilehash: 32d8893190489be0a102b2db4dee3482133a4243
ms.sourcegitcommit: 11880ca974fe2df308191c9f6dcdfe0b89c2dc67
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/27/2021
ms.locfileid: "98860843"
---
# Start-Transcript

## 構文
テキストファイルへの PowerShell セッションのすべてまたは一部のレコードを作成します。

## 構文

### ByPath (既定値)

```
Start-Transcript [[-Path] <String>] [-Append] [-Force] [-NoClobber] [-IncludeInvocationHeader]
 [-UseMinimalHeader] [-WhatIf] [-Confirm]  [<CommonParameters>]
```

### ByLiteralPath

```
Start-Transcript [[-LiteralPath] <String>] [-Append] [-Force] [-NoClobber]
 [-IncludeInvocationHeader] [-UseMinimalHeader] [-WhatIf] [-Confirm]  [<CommonParameters>]
```

### ByOutputDirectory

```
Start-Transcript [[-OutputDirectory] <String>] [-Append] [-Force] [-NoClobber]
 [-IncludeInvocationHeader] [-UseMinimalHeader] [-WhatIf] [-Confirm]  [<CommonParameters>]
```

## 説明

コマンドレットにより、 `Start-Transcript` PowerShell セッションのすべてまたは一部のレコードがテキストファイルに作成されます。 トランスクリプトには、ユーザーが入力したすべてのコマンドと、コンソールに表示されたすべての出力が含まれます。

Windows PowerShell 5.0 以降、では、 `Start-Transcript` すべてのトランスクリプトの生成されたファイル名にホスト名が含まれています。 これは、企業のログ記録が集中管理されている場合に特に便利です。
コマンドレットによって作成されるファイルには、 `Start-Transcript` 2 つ以上のトランスクリプトが同時に開始された場合に上書きや重複が発生しないように、名前にランダムな文字が含まれます。
これにより、一元化されたファイル共有に保存されているトランスクリプトの不正な検出も防止されます。

**Append** パラメーターを使用する場合、ターゲットファイルにバイトオーダーマーク (BOM) がない場合は、 `Start-Transcript` ターゲットファイルのエンコードが既定で設定され `ASCII` ます。 この動作によって、トランスクリプトで mulitbyte 文字が不適切にエンコードされることがあります。

## 使用例

### 例 1: 既定の設定を使用してトランスクリプトファイルを開始する

```powershell
Start-Transcript
```

このコマンドは、既定のファイルの場所を使用してトランスクリプトを開始します。

### 例 2: 特定の場所でトランスクリプトファイルを開始する

```powershell
Start-Transcript -Path "C:\transcripts\transcript0.txt" -NoClobber
```

このコマンドは、のファイルでトランスクリプトを開始 `Transcript0.txt` `C:\transcripts` します。 **NoClobber** パラメーターが使用されているため、既存のファイルは上書きされません。 ファイルが `Transcript0.txt` 既に存在する場合、コマンドは失敗します。

## パラメーター

### -追加

このコマンドレットが、新しいトランスクリプトを既存のファイルの末尾に追加することを示します。 **Path** パラメーターを使用して、ファイルを指定します。

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

### -Force

コマンドレットがトランスクリプトを既存の読み取り専用ファイルに追加することを許可します。 読み取り専用ファイルで使用した場合、このコマンドレットはファイルのアクセス許可を読み取り/書き込みに変更します。 このパラメーターが使用されている場合、コマンドレットでセキュリティ制限をオーバーライドすることはできません。

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

### -IncludeInvocationHeader

コマンドの実行時に、このコマンドレットによってタイムスタンプがログに記録されることを示します。

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

### -UseMinimalHeader

既定では、詳細ヘッダーではなく、短いヘッダーをトランスクリプトに追加します。 このパラメーターは、PowerShell 6.2 で追加されました。

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

### -LiteralPath

トランスクリプトファイルの場所を指定します。 **Path** パラメーターと異なり、**LiteralPath** パラメーターの値は入力したとおりに使用されます。 ワイルドカードとして解釈される文字はありません。 パスにエスケープ文字が含まれている場合は、単一引用符で囲みます。 単一引用符は、文字をエスケープシーケンスとして解釈しないことを PowerShell に伝えます。

```yaml
Type: System.String
Parameter Sets: ByLiteralPath
Aliases: PSPath, LP

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -NoClobber

このコマンドレットが既存のファイルを上書きしないことを示します。 既定では、指定されたパスにトランスクリプトファイルが存在する場合、は `Start-Transcript` 警告なしにファイルを上書きします。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: NoOverwrite

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -OutputDirectory

トランスクリプトを保存する特定のパスとフォルダーを指定します。 PowerShell では、トランスクリプト名が自動的に割り当てられます。

```yaml
Type: System.String
Parameter Sets: ByOutputDirectory
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Path

トランスクリプトファイルの場所を指定します。 ファイルへのパスを入力 `.txt` します。 ワイルドカードは使用できません。

パスを指定しない場合、は `Start-Transcript` グローバル変数の値のパスを使用し `$Transcript` ます。 この変数を作成していない場合は、によって `Start-Transcript` ファイルにトランスクリプトが格納され `$Home\My Documents directory as \PowerShell_transcript.<time-stamp>.txt` ます。

パス内のいずれかのディレクトリが存在しない場合、コマンドは失敗します。

```yaml
Type: System.String
Parameter Sets: ByPath
Aliases:

Required: False
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

このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし

このコマンドレットにパイプを使用してオブジェクトを渡すことはできません。

## 出力

### System.String

このコマンドレットは、確認メッセージと出力ファイルへのパスを含む文字列を返します。

## 注

トランスクリプトを停止するには、コマンドレットを使用し `Stop-Transcript` ます。

セッション全体を記録するには、 `Start-Transcript` プロファイルにコマンドを追加します。 詳細については、「 [about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md)」を参照してください。

## 関連リンク

[Stop-Transcript](Stop-Transcript.md)
