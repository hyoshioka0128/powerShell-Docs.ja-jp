---
external help file: Microsoft.WSMan.Management.dll-Help.xml
Locale: en-US
Module Name: Microsoft.WSMan.Management
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.wsman.management/disconnect-wsman?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Disconnect-WSMan
ms.openlocfilehash: 948b870948f51bd51424beaeca45ed2b2d195891
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600603"
---
# Disconnect-WSMan

## 概要
リモート コンピューター上の WinRM サービスからクライアントを切断します。

## SYNTAX

```
Disconnect-WSMan [[-ComputerName] <String>] [<CommonParameters>]
```

## Description
**切断-WSMan** コマンドレットは、リモートコンピューター上の WinRM サービスからクライアントを切断します。
WS-Management セッションを変数に保存した場合、セッションオブジェクトは変数に残りますが、WS-Management セッションの状態は閉じられます。
WSMan プロバイダーのコンテキスト内でこのコマンドレットを使用すると、リモート コンピューター上の WinRM サービスからクライアントを切断できます。
ただし、WSMan プロバイダーに変更する前に、このコマンドレットを使用して、リモート コンピューター上の WinRM サービスから切断することもできます。

リモート コンピューター上の WinRM サービスに接続する方法の詳細については、「Connect-WSMan」を参照してください。

## 例

### 例 1: リモートコンピューターへの接続を削除する

```
PS C:\> Disconnect-WSMan -computer server01
PS C:\> cd WSMan:
PS WSMan:\>
PS WSMan:\> dir
WSManConfig: Microsoft.WSMan.Management\WSMan::WSMan
ComputerName                                  Type
------------                                  ----
localhost                                     Container
```

このコマンドは、server01 という名前のリモートコンピューターへの接続を削除します。

このコマンドレットは、通常、リモート コンピューター (この場合は、server01 コンピューター) から切断するために、WSMan プロバイダーのコンテキスト内で使用します。
ただし、WSMan プロバイダーに変更する前に、 **切断-wsman** を使用してリモートコンピューターへの接続を削除することもできます。
これらの接続は、ComputerName の一覧に表示されません。

## PARAMETERS

### -ComputerName
管理操作を実行するコンピューターを指定します。
値には、完全修飾ドメイン名、NetBIOS 名、または IP アドレスを指定できます。
ローカル コンピューターを指定するには、ローカル コンピューター名、localhost、またはドット (.) を使用します。
ローカル コンピューターは、既定値です。
リモート コンピューターがユーザーとは異なるドメインにある場合は、完全修飾ドメイン名を使用する必要があります。
パイプを使用して、このパラメーターの値をコマンドレットに渡すことができます。

ローカルホストから切断することはできません。
つまり、ローカルコンピューターへの既定の接続を切断することはできません。
ただし、たとえば、コンピューター名を使用して、ローカルコンピューターへの別の接続を作成する場合。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 0
Default value: None
Accept pipeline input: False
Accept wildcard characters: False
```

### 共通パラメーター
このコマンドレットは、一般的なパラメーターをサポートしています。-Debug、-ErrorAction、-ErrorVariable、-InformationAction、-InformationVariable、-OutVariable、-OutBuffer、-PipelineVariable、-Verbose、-WarningAction、-WarningVariable です。 詳細については、「[about_CommonParameters](https://go.microsoft.com/fwlink/?LinkID=113216)」を参照してください。

## 入力

### なし
このコマンドレットは入力を受け取りません。

## 出力

### なし
このコマンドレットは出力を生成しません。

## 注

## 関連リンク

[Connect-WSMan](Connect-WSMan.md)

[Disable-WSManCredSSP](Disable-WSManCredSSP.md)

[Enable-WSManCredSSP](Enable-WSManCredSSP.md)

[Get-WSManCredSSP](Get-WSManCredSSP.md)

[Get-WSManInstance](Get-WSManInstance.md)

[Invoke-WSManAction](Invoke-WSManAction.md)

[New-WSManInstance](New-WSManInstance.md)

[New-WSManSessionOption](New-WSManSessionOption.md)

[Remove-WSManInstance](Remove-WSManInstance.md)

[Set-WSManInstance](Set-WSManInstance.md)

[Set-WSManQuickConfig](Set-WSManQuickConfig.md)

[Test-WSMan](Test-WSMan.md)

