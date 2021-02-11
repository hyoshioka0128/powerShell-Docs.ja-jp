---
external help file: Microsoft.PowerShell.ODataUtils-help.xml
keywords: powershell,コマンドレット
Locale: en-US
Module Name: Microsoft.PowerShell.ODataUtils
ms.date: 06/09/2017
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.odatautils/export-odataendpointproxy?view=powershell-5.1&WT.mc_id=ps-gethelp
schema: 2.0.0
title: Export-ODataEndpointProxy
ms.openlocfilehash: 846aca6974190863a073773fe5148f0c57d77dc3
ms.sourcegitcommit: 2c311274ce721cd1072dcf2dc077226789e21868
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2020
ms.locfileid: "94388197"
---
# Export-ODataEndpointProxy

## 概要
OData エンドポイントを管理するコマンドレットを含むモジュールを生成します。

## SYNTAX

```
Export-ODataEndpointProxy [-Uri] <String> [-OutputModule] <String> [[-MetadataUri] <String>]
 [[-Credential] <PSCredential>] [[-CreateRequestMethod] <String>] [[-UpdateRequestMethod] <String>]
 [[-CmdletAdapter] <String>] [[-ResourceNameMapping] <Hashtable>] [-Force] [[-CustomData] <Hashtable>]
 [-AllowClobber] [-AllowUnsecureConnection] [[-Headers] <Hashtable>] [-WhatIf] [-Confirm] [<CommonParameters>]
```

## Description

`Export-ODataEndpointProxy`コマンドレットは、odata エンドポイントのメタデータを使用して、その odata エンドポイントの管理に使用できるコマンドレットを含むモジュールを生成します。 モジュールは、CDXML に基づいています。 このコマンドレットによってモジュールが生成された後、 **OutputModule** パラメーターで指定したパスとファイル名にモジュールが保存されます。

`Export-ODataEndpointProxy` 作成、読み取り、更新、および削除 (CRUD) 操作、非 CRUD アクション、およびアソシエーション操作のためのコマンドレットを生成します。

`Export-ODataEndpointProxy` エンドポイントリソースごとに1つの CDXML ファイルを生成します。 これらの CDXML ファイルは、モジュールが生成された後で編集できます。 たとえば、Windows PowerShell コマンドレットの名前付けガイドラインに合わせてコマンドレットの名詞または動詞の名前を変更する場合は、ファイルを変更できます。

生成されたモジュールのすべてのコマンドレットには、モジュールが管理するエンドポイントに接続するために **Connectionuri** パラメーターを含める必要があります。

## 例

### 例 1: 製品版 web サービスエンドポイントを管理するモジュールを生成する

```powershell
PS C:\> Export-ODataEndpointProxy -Uri 'http://services.odata.org/v3/(S(snyobsk1hhutkb2yulwldgf1))/odata/odata.svc' -MetadataUri 'http://services.odata.org/v3/(S(snyobsk1hhutkb2yulwldgf1))/odata/odata.svc/$metadata' -AllowUnsecureConnection -OutputModule 'C:\Users\user\GeneratedScript.psm1' -ResourceNameMapping @{Products = 'Merchandise'}
```

このコマンドは、リテールサービスエンドポイントを管理するモジュールを生成します。 このコマンドでは、エンドポイントの URI とエンドポイントメタデータの URI を指定します。 また、このコマンドは、 **OutputModule** パラメーターの値として出力パスとスクリプトモジュール名も提供します。 **ResourceNameMapping** パラメーターの値に対して、コマンドは、リソースコレクション名をコマンドレットセットの目的の名詞にマップするハッシュテーブルを提供します。 この例では、Products はリソースコレクションの名前、 **商品** は名詞です。 HTTPS ではなく、SSL 以外のサイトへの接続を許可するには、 **Allowunsecureconnection** パラメーターを追加します。

## PARAMETERS

### -AllowClobber

このコマンドレットが既存のモジュールを置き換えることを示します。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: 10
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -AllowUnsecureConnection

このモジュールが、SSL で保護されていない Uri に接続できることを示します。 このモジュールは、HTTPS サイトに加えて HTTP サイトも管理できます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: 11
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### --のアダプター

コマンドレットアダプターを指定します。 このパラメーターに指定できる値は、ODataAdapter と NetworkControllerAdapter です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: ODataAdapter, NetworkControllerAdapter, ODataV4Adapter

Required: False
Position: 6
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -CreateRequestMethod

要求メソッドを指定します。 このパラメーターに指定できる値は、PUT、POST、PATCH です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Put, Post, Patch

Required: False
Position: 4
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Credential

OData エンドポイントへのアクセス権を持つユーザーアカウントを指定します。 既定値は現在のユーザーです。 リモートコンピューターで windows Vista 以降のバージョンの Windows オペレーティングシステムが実行されている場合、コマンドレットによって資格情報の入力が求められます。

```yaml
Type: System.Management.Automation.PSCredential
Parameter Sets: (All)
Aliases:

Required: False
Position: 3
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -CustomData

カスタムデータのハッシュテーブルを指定します。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: 9
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Force

このコマンドレットが、既存のフォルダー内の同じ名前の既存の生成されたモジュールを上書きすることを示し `Modules` ます。

```yaml
Type: System.Management.Automation.SwitchParameter
Parameter Sets: (All)
Aliases:

Required: False
Position: 8
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ヘッダー

Web 要求のヘッダーを指定します。 ハッシュ テーブルまたは辞書を入力します。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: 12
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -MetadataUri

エンドポイントのメタデータの URI を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: False
Position: 2
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -OutputModule

このコマンドレットによって生成されたプロキシコマンドのモジュールを保存するパスとモジュール名を指定します。

このコマンドレットは、バイナリモジュール、モジュールマニフェスト、およびフォーマットファイル (該当する場合) を指定されたフォルダーにコピーします。 モジュールの名前のみを指定すると、はその `Export-ODataEndpointProxy` モジュールをフォルダーに保存し `$home\Documents\WindowsPowerShell\Modules` ます。 パスを指定すると、コマンドレットによってそのパスにモジュールフォルダーが作成されます。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 1
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -ResourceNameMapping

生成されたコマンドレットをカスタマイズできるようにするマッピングを含むハッシュテーブルを指定します。 このハッシュテーブルでは、リソースコレクション名はキーです。 目的のコマンドレットの名詞は、値です。

たとえば、ハッシュテーブルでは、 `@{Products = 'Merchandise'}` **Products** はキーとして機能するリソースコレクション名です。 **商品** は、結果のコマンドレットの名詞です。 生成されたコマンドレット名は、Windows PowerShell コマンドレットの名前付けガイドラインに一致しない場合があります。 このコマンドレットでモジュールを作成した後で、リソースの CDXML ファイルを変更してコマンドレット名を変更することができます。 詳細については、「 [開発に関する](/powershell/scripting/developer/cmdlet/strongly-encouraged-development-guidelines)推奨事項」を参照してください。

```yaml
Type: System.Collections.Hashtable
Parameter Sets: (All)
Aliases:

Required: False
Position: 7
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -UpdateRequestMethod

更新要求メソッドを指定します。 このパラメーターに指定できる値は、PUT、POST、PATCH です。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:
Accepted values: Put, Post, Patch

Required: False
Position: 5
Default value: None
Accept pipeline input: True (ByPropertyName)
Accept wildcard characters: False
```

### -Uri

エンドポイントの URI を指定します。

```yaml
Type: System.String
Parameter Sets: (All)
Aliases:

Required: True
Position: 0
Default value: None
Accept pipeline input: True (ByPropertyName, ByValue)
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

## 出力

## 注

## 関連リンク

[OData ライブラリ](/previous-versions/dotnet/wcf-data-services/hh525392(v=vs.103))

[OData プロトコルとは](https://www.odata.org/)

[Invoke-RestMethod](../Microsoft.PowerShell.Utility/Invoke-RestMethod.md)
