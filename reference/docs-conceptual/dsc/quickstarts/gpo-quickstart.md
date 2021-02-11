---
ms.date: 07/09/2019
keywords: DSC, GPO, PowerShell, 構成, セットアップ
title: クイック スタート - グループ ポリシーを DSC に変換する
description: このクイック スタートでは、Windows グループ ポリシーを DSC 構成に変換するための必要な手順について説明します。
ms.openlocfilehash: b67f6dd2cf6c91d90fa6ac5b6367f9efc7f40ee0
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92644711"
---
# <a name="quickstart-convert-group-policy-into-dsc"></a>クイック スタート:グループ ポリシーを DSC に変換する

> 適用先:Windows PowerShell 4.0、Windows PowerShell 5.0

グループ ポリシーまたは Azure Security Center ベースラインから DSC 構成を生成できます。 [BaselineManagement](https://www.powershellgallery.com/packages/BaselineManagement) モジュールには、このタスクを達成するために、次のコマンドが含まれます。

- `ConvertFrom-GPO` - グループ ポリシーを変換します。ファイルとして保存されます。 1 つの構成に結合される複数のポリシーを含むディレクトリを指定することもできます。
  - ご利用の環境でグループ ポリシーをエクスポートするには、[Backup-GPO](/powershell/module/grouppolicy/backup-gpo) コマンドレットを使用するか、「[Export a GPO to a File](/microsoft-desktop-optimization-pack/agpm/export-a-gpo-to-a-file)」 (GPO をファイルエクスポートする) の手順に従います。
- `ConvertFrom-SCM` - セキュリティ コンプライアンス マネージャー ベースラインを変換します。`.xml` ファイルとして保存されます。
- `ConvertFrom-ASC` - Azure Security Center ベースラインを変換します。`.json` ファイルとして保存されます。
- `Merge-GPOs` - ターゲット コンピューターに適用されたグループ ポリシーを変換します。

上述のコマンドレットによって、ベースラインが DSC `.mof` ファイルに変換されます。 編集して再コンパイルすることができる、構成スクリプト (`.ps1`) を出力することを選択することもできます。 このコマンドレットでは、不足しているリソースや重複したリソース ブロックに対する複雑なエラーを検出することができます。 複雑なエラーの原因になるリソース ブロックは、コメント アウトされます。

次の例では、[Microsoft セキュリティ ベースライン](https://www.microsoft.com/download/details.aspx?id=55319)が DSC 構成スクリプト (`.ps1`) と `.mof` ファイルに変換されます。

```powershell
Install-Module BaselineManagement
Import-Module BaselineManagement
ConvertFrom-GPO -Path '.\Windows 10 Version 1903 and Windows Server Version 1903 Security Baseline\GPOs\' -OutputConfigurationScript
```

コマンドを実行した後、現在のパスの下に作成された既定の "Output" ディレクトリに 2 つのファイルが表示されます。

```powershell
Get-ChildItem -Path .\Output
```

```Output
    Directory:  C:\Temp

Mode                LastWriteTime     Length Name
----                -------------     ------ ----
-a----         7/9/2019   9:35 AM   227.37KB DSCFromGPO.ps1
-a----         7/9/2019   9:35 AM   410.03KB localhost.mof
```

管理ノードごとに、次の 2 つのモジュールも必要です。

- [SecurityPolicyDSC](https://www.powershellgallery.com/packages/SecurityPolicyDsc)
- [AuditPolicyDSC](https://www.powershellgallery.com/packages/AuditPolicyDsc)

> [!NOTE]
> **BaselineManagement** は、Microsoft からではなく、プロジェクト保守管理者からのコミュニティ ソリューションに対するサポートを、DSC でより検索しやすくするように、コミュニティによって開発されたソリューションです。 [GitHub](https://github.com/microsoft/BaselineManagement) 上で **BaselineManagement** に対して新しい問題を作成することができます。

## <a name="next-steps"></a>次のステップ

- 構成スクリプトを Azure Automation State Configuration にアップロードするには、[作業の開始](/azure/automation/automation-dsc-getting-started#importing-a-configuration-into-azure-automation)に関するページを参照してください。
- **SecurityPolicyDSC** モジュールと **AuditPolicyDSC** モジュールを [Automation Account](/azure/automation/shared-resources/modules) に追加します。
- 「[PowerShell ギャラリー](https://www.powershellgallery.com/)」で DSC の構成とリソースを検索する。
