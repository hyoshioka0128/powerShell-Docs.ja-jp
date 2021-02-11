---
external help file: Microsoft.PowerShell.Commands.Management.dll-Help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.Management
ms.date: 11/11/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.management/start-process?view=powershell-7.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Start-Process
ms.openlocfilehash: 75f0b0bed808efbe31254fcd04662ddef2f3a22c
ms.sourcegitcommit: aac365f7813756e16b59322832a904e703e0465b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2020
ms.locfileid: "94524741"
---
# Start-Process

## 概要
ローカル コンピューター上の 1 つ以上のプロセスを開始します。

## SYNTAX

### 既定値 (既定値)

```
Start-Process [-FilePath] <String> [[-ArgumentList] <String[]>] [-Credential <PSCredential>]
 [-WorkingDirectory <String>] [-LoadUserProfile] [-NoNewWindow] [-PassThru]
 [-RedirectStandardError <String>] [-RedirectStandardInput <String>]
 [-RedirectStandardOutput <String>] [-WindowStyle <ProcessWindowStyle>] [-Wait] [-UseNewEnvironment]
 [-WhatIf] [-Confirm] [<CommonParameters>]
```

### UseShellExecute

```
Start-Process [-FilePath] <String> [[-ArgumentList] <String[]>] [-WorkingDirectory <String>]
 [-PassThru] [-Verb <String>] [-WindowStyle <ProcessWindowStyle>] [-Wait] [-WhatIf] [-Confirm]
 [<CommonParameters>]
```

## Description

`Start-Process`コマンドレットにより、ローカルコンピューター上の1つ以上のプロセスが開始されます。 既定では、は、 `Start-Process` 現在のプロセスで定義されているすべての環境変数を継承する新しいプロセスを作成します。

プロセスで実行されるプログラムを指定するには、実行可能ファイルやスクリプト ファイル、またはコンピューター上のプログラムで開くことができるファイルの名前を入力します。 実行可能ファイル以外のファイルを指定すると、は、 `Start-Process` コマンドレットと同様に、ファイルに関連付けられているプログラムを起動し `Invoke-Item` ます。

のパラメーターを使用し `Start-Process` て、ユーザープロファイルの読み込み、新しいウィンドウでのプロセスの開始、代替資格情報の使用などのオプションを指定できます。

## 例

### 例 1: 既定値を使用するプロセスを開始する

この例では、現在のフォルダー内のファイルを使用するプロセスを開始 `Sort.exe` します。 このコマンドは、既定のウィンドウスタイル、作業フォルダー、資格情報など、すべての既定値を使用します。

```powershell
Start-Process -FilePath "sort.exe"
```

### 例 2: テキストファイルを印刷する

この例では、ファイルを出力するプロセスを開始し `C:\PS-Test\MyFile.txt` ます。

```powershell
Start-Process -FilePath "myfile.txt" -WorkingDirectory "C:\PS-Test" -Verb Print
```

### 例 3: 新しいファイルに項目を並べ替えるプロセスを開始する

この例では、ファイル内の項目を並べ替え `Testsort.txt` 、ファイル内の並べ替えられた項目を返すプロセスを開始し `Sorted.txt` ます。 エラーはすべてファイルに書き込まれ `SortError.txt` ます。

```powershell
Start-Process -FilePath "Sort.exe" -RedirectStandardInput "Testsort.txt" -RedirectStandardOutput "Sorted.txt" -RedirectStandardError "SortError.txt" -UseNewEnvironment
```

**UseNewEnvironment** パラメーターは、プロセスが独自の環境変数で実行されることを指定します。

### 例 4: 最大化されるウィンドウでプロセスを開始する

この例では、プロセスを開始 `Notepad.exe` します。 ウィンドウが最大化されて、プロセスが完了するまでそのまま維持されます。

```powershell
Start-Process -FilePath "notepad" -Wait -WindowStyle Maximized
```

### 例 5: 管理者として PowerShell を起動する

この例では、[ **管理者として実行** ] オプションを使用して PowerShell を起動します。

```powershell
Start-Process -FilePath "powershell" -Verb RunAs
```

### 例 6: 異なる動詞を使用してプロセスを開始する

この例では、プロセスの開始時に使用できる動詞を検索する方法を示します。 使用可能な動詞は、プロセスで実行されるファイルのファイル名拡張子によって決まります。

```powershell
$startExe = New-Object System.Diagnostics.ProcessStartInfo -Args PowerShell.exe
$startExe.verbs
```

```Output
open
runas
runasuser
```

この例では、を使用して、 `New-Object` PowerShell プロセスで実行されるファイル **PowerShell.exe** の **ProcessStartInfo** オブジェクトを作成します。 **ProcessStartInfo** オブジェクトの **verbs** プロパティは、、、またはファイルを実行するすべてのプロセスで **Open** 動詞と **RunAs** 動詞を使用できることを示して `PowerShell.exe` `.exe` います。

### 例 7: プロセスに引数を指定する

どちらのコマンドも、Windows コマンドインタープリターを起動し、 `dir` フォルダーに対してコマンドを発行し `Program Files` ます。 この foldername にはスペースが含まれているため、値はエスケープされた引用符で囲まれている必要があります。
最初のコマンドでは、文字列を **ArgumentList** として指定することに注意してください。 2番目のコマンドは文字列配列です。

```powershell
Start-Process -FilePath "$env:comspec" -ArgumentList "/c dir `"%systemdrive%\program files`""
Start-Process -FilePath "$env:comspec" -ArgumentList "/c","dir","`"%systemdrive%\program files`""
```

### 例 8: デタッチされたプロセスを Linux で作成する

Windows では、は、 `Start-Process` 起動中のシェルとは無関係に実行される独立したプロセスを作成します。 Windows 以外のプラットフォームでは、新しく起動されたプロセスは、を起動したシェルにアタッチされます。 起動中のシェルを閉じると、子プロセスは終了します。

Unix のようなプラットフォームで子プロセスを終了しないようにするには、とを組み合わせることができ `Start-Process` `nohup` ます。 次の例では、起動中のセッションを閉じた後でも、Linux で PowerShell のバックグラウンドインスタンスを起動して、そのままにします。 コマンドは、 `nohup` 現在のディレクトリ内のファイルの出力を収集し `nohup.out` ます。

```powershell
# Runs for 2 minutes and appends output to ./nohup.out
Start-Process nohup 'pwsh -noprofile -c "1..120 | % { Write-Host . -NoNewline; sleep 1 }"'
```

この例で `Start-Process` は、は、デタッチされ `nohup` `pwsh` たプロセスとして起動する Linux コマンドを実行しています。 詳細については、 [nohup](https://linux.die.net/man/1/nohup)の man ページを参照してください。

## PARAMETERS

### -ArgumentList

このコマンドレットでプロセスを開始するときに使用するパラメーターまたはパラメーター値を指定します。 引数は、スペースで区切られた1つの文字列として、またはコンマで区切られた文字列の配列として受け取ることができます。

パラメーターまたはパラメーターの値にスペースが含まれている場合は、エスケープされた二重引用符で囲む必要があります。 詳細については、「 [about_Quoting_Rules](../Microsoft.PowerShell.Core/About/about_Quoting_Rules.md)」を参照してください。

```yaml
Type: System.String[]
Parameter Sets: (All)
Aliases: Args

Required: False
Position: 1
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Credential

この処理を実行するアクセス許可を持つユーザー アカウントを指定します。 既定では、コマンドレットは現在のユーザーの資格情報を使用します。

**User01** や **domain01\user01」** などのユーザー名を入力するか、コマンドレットによって生成される **PSCredential** オブジェクトを入力し `Get-Credential` ます。 ユーザー名を入力すると、パスワードを入力するように求められます。

資格情報は [PSCredential](/dotnet/api/system.management.automation.pscredential) オブジェクトに格納され、パスワードは [SecureString](/dotnet/api/system.security.securestring)として格納されます。

> [!NOTE]
> **SecureString** data protection の詳細については、「 [How To secure is SecureString?](/dotnet/api/system.security.securestring#how-secure-is-securestring)」を参照してください。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: Default
Aliases: RunAs

Required: False
Position: Named
Default value: Current user
Accept pipeline input: False
Accept wildcard characters: False
```

### -FilePath

プロセスで実行されるプログラムのパスとファイル名 (省略可能) を指定します。 `.txt` `.doc` コンピューター上のプログラムに関連付けられている実行可能ファイルまたはドキュメント (やファイルなど) の名前を入力します。 このパラメーターは必須です。

ファイル名のみを指定する場合は、 **WorkingDirectory** パラメーターを使用してパスを指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases: PSPath, Path

Required: True
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Processmodel.loaduserprofile

このコマンドレットにより、 `HKEY_USERS` 現在のユーザーのレジストリキーに格納されている Windows ユーザープロファイルが読み込まれることを示します。 このパラメーターは、Windows 以外のシステムには適用されません。

このパラメーターは、PowerShell プロファイルには影響しません。 詳細については、「 [about_Profiles](../Microsoft.PowerShell.Core/About/about_Profiles.md)」を参照してください。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Default
Aliases: Lup

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -None Wwindow

現在のコンソール ウィンドウで新しいプロセスを開始します。 Windows の既定では、PowerShell によって新しいウィンドウが開きます。 Windows 以外のシステムでは、新しいターミナルウィンドウは表示されません。

**NoNewWindow** と **WindowStyle** パラメーターを同じコマンドで使用することはできません。

このパラメーターは、Windows 以外のシステムには適用されません。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Default
Aliases: nnw

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -PassThru

コマンドレットが開始した各プロセスのプロセス オブジェクトを返します。 既定では、このコマンドレットによる出力はありません。

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

### -RedirectStandardError

ファイルを指定します。 このコマンドレットは、プロセスによって生成されたすべてのエラーを、指定したファイルに送信します。
パスとファイル名を入力します。 既定では、エラーはコンソールに表示されます。

```yaml
Type: System.String
Parameter Sets: Default
Aliases: RSE

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RedirectStandardInput

ファイルを指定します。 このコマンドレットは、指定されたファイルから入力を読み取ります。 入力ファイルのパスとファイル名を入力します。 既定では、入力はキーボードから取得されます。

```yaml
Type: System.String
Parameter Sets: Default
Aliases: RSI

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -RedirectStandardOutput

ファイルを指定します。 このコマンドレットは、プロセスによって生成された出力を、指定したファイルに送信します。
パスとファイル名を入力します。 既定では、出力はコンソールに表示されます。

```yaml
Type: System.String
Parameter Sets: Default
Aliases: RSO

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -UseNewEnvironment

このコマンドレットが、プロセスに対して指定された新しい環境変数を使用することを示します。 既定では、開始されたプロセスは、親プロセスから継承された環境変数を使用して実行されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: Default
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -動詞

このコマンドレットがプロセスを開始するときに使用する動詞を指定します。 使用可能な動詞は、プロセスで実行されるファイルのファイル名拡張子によって決まります。

次の表に、いくつかの一般的なプロセス ファイルの種類の動詞を示します。

| ファイルの種類 |                動詞                |
| --------- | ----------------------------------- |
| .cmd      | Edit、Open、Print、RunAs、RunAsUser |
| .exe      | Open、RunAs、RunAsUser              |
| .txt      | Open、Print、PrintTo                |
| .wav      | 開く、再生                          |

プロセスで実行されるファイルと共に使用できる動詞を検索するには、コマンドレットを使用して、 `New-Object` ファイルの **ProcessStartInfo** オブジェクトを作成します。 使用可能な動詞は、 **ProcessStartInfo** オブジェクトの **verb** プロパティにあります。 詳細については、例を参照してください。

このパラメーターは、Windows 以外のシステムには適用されません。

```yaml
Type: System.String
Parameter Sets: UseShellExecute
Aliases:

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -Wait

このコマンドレットが、指定されたプロセスとその子孫が完了するまで待機してから、より多くの入力を受け入れることを示します。 このパラメーターを指定すると、コマンドプロンプトが表示されなくなるか、またはプロセスが終了するまでウィンドウが保持されます。

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

### -WindowStyle

新しいプロセスに使用するウィンドウの状態を指定します。 このパラメーターに指定できる値は、 **Normal** 、 **Hidden** 、 **最小化** 、および **最大化** です。 既定値は **Normal** です。

**WindowStyle** と **NoNewWindow** パラメーターを同じコマンドで使用することはできません。

このパラメーターは、Windows 以外のシステムには適用されません。

```yaml
Type: System.Diagnostics.ProcessWindowStyle
Parameter Sets: (All)
Aliases:
Accepted values: Normal, Hidden, Minimized, Maximized

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WorkingDirectory

新しいプロセスを開始する場所を指定します。 既定値は、開始する実行可能ファイルまたはドキュメントの場所です。 ワイルドカードはサポートされていません。 パス名には、ワイルドカードとして解釈される文字を含めることはできません。

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

### -Confirm

コマンドレットの実行前に確認を求めるメッセージが表示されます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: cf

Required: False
Position: Named
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### -WhatIf

コマンドレットの実行時に発生する内容を示します。 このコマンドレットは実行されません。

このパラメーターは、PowerShell 6.0 で導入されました。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases: wi

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

### なし、システム。診断プロセス

**PassThru** パラメーターを指定した場合、このコマンドレット **では、system.string オブジェクトが** 生成されます。 それ以外の場合、このコマンドレットによる戻り値はありません。

## 注

- このコマンドレットは、 **system.servicemodel クラスの** **Start** メソッドを使用して実装されます。 このメソッドの詳細については、「 [Process. Start メソッド](/dotnet/api/system.diagnostics.process.start#overloads)」を参照してください。

- Windows では、 **UseNewEnvironment** を使用すると、新しいプロセスは、 **マシン** スコープに対して定義されている既定の環境変数のみを含むように開始されます。 これにより、 `$env:USERNAME` が **システム** に設定されているという副作用があります。 **ユーザー** スコープの変数は含まれません。

- Windows では、の最も一般的なユースケース `Start-Process` は、新しいプロセスが終了するまで、 **Wait** パラメーターを使用して進行状況をブロックすることです。 Windows 以外のシステムでは、コマンドラインアプリケーションの既定の動作はと同じであるため、これはほとんど必要あり `Start-Process -Wait` ません。

- Windows 以外のシステムでを使用する場合 `Start-Process` 、新しいターミナルウィンドウは表示されません。

## 関連リンク

[about_Quoting_Rules](../Microsoft.PowerShell.Core/About/about_Quoting_Rules.md)

[Debug-Process](Debug-Process.md)

[Get-Process](Get-Process.md)

[Start-Service](Start-Service.md)

[Stop-Process](Stop-Process.md)

[Wait-Process](Wait-Process.md)
