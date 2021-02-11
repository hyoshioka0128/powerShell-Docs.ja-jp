---
external help file: Microsoft.PowerShell.Commands.Utility.dll-Help.xml
Locale: en-US
Module Name: Microsoft.PowerShell.Utility
ms.date: 10/14/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/write-error?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Write-Error
ms.openlocfilehash: d0ee66e1002e4f1d58f63e198bcb7f03b1c8d3a8
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99601402"
---
# Write-Error

## 概要
エラー ストリームにオブジェクトを書き込みます。

## SYNTAX

### NoException (既定値)

```
Write-Error [-Message] <String> [-Category <ErrorCategory>] [-ErrorId <String>] [-TargetObject <Object>]
 [-RecommendedAction <String>] [-CategoryActivity <String>] [-CategoryReason <String>]
 [-CategoryTargetName <String>] [-CategoryTargetType <String>] [<CommonParameters>]
```

### WithException

```
Write-Error -Exception <Exception> [[-Message] <String>] [-Category <ErrorCategory>] [-ErrorId <String>]
 [-TargetObject <Object>] [-RecommendedAction <String>] [-CategoryActivity <String>] [-CategoryReason <String>]
 [-CategoryTargetName <String>] [-CategoryTargetType <String>] [<CommonParameters>]
```

### ErrorRecord

```
Write-Error -ErrorRecord <ErrorRecord> [-RecommendedAction <String>] [-CategoryActivity <String>]
 [-CategoryReason <String>] [-CategoryTargetName <String>] [-CategoryTargetType <String>] [<CommonParameters>]
```

## Description

`Write-Error`コマンドレットは、終了しないエラーを宣言します。 既定では、エラー ストリームで出力と共に表示されるホスト プログラムにエラーが送信されます。

未終了エラーを書き込むには、エラー メッセージ文字列の **ErrorRecord** オブジェクトまたは **Exception** オブジェクトを入力します。 の他のパラメーターを使用し `Write-Error` て、エラーレコードを設定します。

未終了エラーにより、エラー ストリームにエラーが書き込まれますが、コマンド処理は停止されません。
未終了エラーが、入力項目のコレクション内の 1 つの項目で宣言された場合、コマンドはコレクション内の他の項目の処理を続行します。

終了エラーを宣言するには、キーワードを使用し `Throw` ます。
詳細については、「 [about_Throw](../Microsoft.PowerShell.Core/About/about_Throw.md)」を参照してください。

## 例

### 例 1: RegistryKey オブジェクトのエラーを書き込む

```powershell
Get-ChildItem | ForEach-Object {
    if ($_.GetType().ToString() -eq "Microsoft.Win32.RegistryKey")
    {
        Write-Error "Invalid object" -ErrorId B1 -TargetObject $_
    }
    else
    {
        $_
    }
}
```

このコマンドレットによっ `Get-ChildItem` `Microsoft.Win32.RegistryKey` て、 `HKLM:` PowerShell レジストリプロバイダーのまたはドライブ内のオブジェクトなどのオブジェクトが返された場合に、終了しないエラーが宣言され `HKCU:` ます。

### 例 2: コンソールにエラーメッセージを書き込む

```powershell
Write-Error "Access denied."
```

このコマンドは、未終了エラーを宣言し、"Access denied" エラーを書き込みます。 このコマンドでは、**Message** パラメーターを使用してメッセージを指定しますが、オプションの **Message** パラメーター名は省略します。

### 例 3: コンソールにエラーを書き込み、カテゴリを指定する

```powershell
Write-Error -Message "Error: Too many input values." -Category InvalidArgument
```

このコマンドは、未終了エラーを宣言し、エラー カテゴリを書き込みます。

### 例 4: 例外オブジェクトを使用してエラーを記述する

```powershell
$E = [System.Exception]@{Source="Get-ParameterNames.ps1";HelpLink="https://go.microsoft.com/fwlink/?LinkID=113425"}
Write-Error -Exception $E -Message "Files not found. The $Files location does not contain any XML files."
```

このコマンドは、**Exception** オブジェクトを使用して未終了エラーを宣言します。

最初のコマンドは、ハッシュ テーブルを使用して、**System.Exception** オブジェクトを作成します。 例外オブジェクトは変数に保存され `$E` ます。 ハッシュ テーブルを使用して、null コンストラクターを持つ種類のオブジェクトを作成します。

2番目のコマンドは、コマンドレットを使用して、 `Write-Error` 終了しないエラーを宣言します。 **Exception** パラメーターの値は、変数内の **exception** オブジェクトです `$E` 。

## PARAMETERS

### -カテゴリ

エラーのカテゴリを指定します。 既定値は **Notspecified** です。 このパラメーターの有効値は、次のとおりです。

- NotSpecified
- OpenError
- CloseError
- DeviceError
- DeadlockDetected
- InvalidArgument
- InvalidData
- InvalidOperation
- InvalidResult
- InvalidType
- MetadataError
- NotImplemented
- NotInstalled
- ObjectNotFound
- OperationStopped
- OperationTimeout
- SyntaxError
- ParserError
- PermissionDenied
- ResourceBusy
- ResourceExists
- リソースを使用できません
- ReadError
- WriteError
- FromStdErr
- SecurityError
- ProtocolError
- ConnectionError
- AuthenticationError
- LimitsExceeded
- QuotaExceeded
- NotEnabled

エラーカテゴリの詳細については、「 [ErrorCategory Enumeration](https://go.microsoft.com/fwlink/?LinkId=143600)」を参照してください。

```yaml
Type: System.Management.Automation.ErrorCategory
Parameter Sets: NoException, WithException
Aliases:
Accepted values: NotSpecified, OpenError, CloseError, DeviceError, DeadlockDetected, InvalidArgument, InvalidData, InvalidOperation, InvalidResult, InvalidType, MetadataError, NotImplemented, NotInstalled, ObjectNotFound, OperationStopped, OperationTimeout, SyntaxError, ParserError, PermissionDenied, ResourceBusy, ResourceExists, ResourceUnavailable, ReadError, WriteError, FromStdErr, SecurityError, ProtocolError, ConnectionError, AuthenticationError, LimitsExceeded, QuotaExceeded, NotEnabled

Required: False
Position: Named
Default value: NotSpecified
Accept pipeline input: False
Accept wildcard characters: False
```

### -カテゴリアクティビティ

エラーの原因となったアクションを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: Activity

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -カテゴリの理由

アクティビティでエラーが発生した方法または理由を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: Reason

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -カテゴリのターゲット \ ターゲット

エラーが発生したときに処理されていたオブジェクトの名前を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: TargetName

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -カテゴリ Targettype

エラーが発生したときに処理されていたオブジェクトの種類を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: TargetType

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ErrorId

エラーを識別する ID 文字列を指定します。 この文字列、はエラーに対して一意である必要があります。

```yaml
Type: System.String
Parameter Sets: NoException, WithException
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -ErrorRecord

エラーを表す エラー レコード オブジェクトを指定します。 エラーを説明するオブジェクトのプロパティを使用します。

エラーレコードオブジェクトを作成するには、 `New-Object` コマンドレットを使用するか、自動変数の配列からエラーレコードオブジェクトを取得し `$Error` ます。

```yaml
Type: System.Management.Automation.ErrorRecord
Parameter Sets: ErrorRecord
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -例外

エラーを表す例外オブジェクトを指定します。 エラーを説明するオブジェクトのプロパティを使用します。

例外オブジェクトを作成するには、ハッシュテーブルを使用するか、コマンドレットを使用し `New-Object` ます。

```yaml
Type: System.Exception
Parameter Sets: WithException
Aliases:

Required: True
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -メッセージ

エラーのメッセージ テキストを指定します。 テキストにスペースまたは特殊文字が含まれる場合は、二重引用符で囲みます。 また、パイプを使用してメッセージ文字列をにパイプすることもでき `Write-Error` ます。

```yaml
Type: System.String
Parameter Sets: NoException, WithException
Aliases: Msg

Required: True (NoException), False (WithException)
Position: 0
Default value: None
Accept pipeline input: True (ByValue)
Accept wildcard characters: False
```

### -RecommendedAction

エラーを解決または回避するためにユーザーが実行する必要のあるアクションを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -TargetObject

エラーが発生したときに処理されていたオブジェクトを指定します。 オブジェクト、オブジェクトを含む変数、またはオブジェクトを取得するコマンドを入力します。

```yaml
Type: System.Object
Parameter Sets: NoException, WithException
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

### System.String

パイプを使用して、エラーメッセージを含む文字列をにパイプすることができ `Write-Error` ます。

## 出力

### エラー オブジェクト

`Write-Error` エラーストリームにのみ書き込みます。 このコマンドはオブジェクトを返しません。

## 注

`Write-Error` は自動変数の値を変更しない `$?` ため、終了エラー状態を通知しません。 終了エラーを通知するには、 [WriteError ()](/dotnet/api/system.management.automation.cmdlet.writeerror) メソッドを $PSCmdlet 使用します。

## 関連リンク

[about_Automatic_Variables](../microsoft.powershell.core/about/about_automatic_variables.md)

[about_Output_Streams](../Microsoft.PowerShell.Core/About/about_Output_Streams.md)

[about_Redirection](../Microsoft.PowerShell.Core/About/about_Redirection.md)

[Write-Debug](Write-Debug.md)

[Write-Host](Write-Host.md)

[Write-Output](Write-Output.md)

[Write-Progress](Write-Progress.md)

[Write-Verbose](Write-Verbose.md)

[Write-Warning](Write-Warning.md)
