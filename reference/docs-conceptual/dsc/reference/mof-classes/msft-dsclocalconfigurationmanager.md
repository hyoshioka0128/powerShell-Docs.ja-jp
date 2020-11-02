---
ms.date: 07/14/2020
ms.topic: reference
title: MSFT_DSCLocalConfigurationManager クラス
description: MSFT_DSCLocalConfigurationManager クラス
ms.openlocfilehash: 31112c7d15884699171ec732ac20b6960b0858a9
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92644813"
---
# <a name="msft_dsclocalconfigurationmanager-class"></a>MSFT_DSCLocalConfigurationManager クラス

構成ファイルの状態を制御し、構成エージェントを使用してその構成を適用するローカル構成マネージャー (LCM)。

次の構文はマネージド オブジェクト フォーマット (MOF) のコードを単純化したもので、すべての継承されたプロパティを含みます。

## <a name="syntax"></a>構文

```
[ClassVersion("1.0.0"), dynamic, provider("dsccore"), AMENDMENT]
class MSFT_DSCLocalConfigurationManager
{
};
```

## <a name="members"></a>メンバー

**MSFT_DSCLocalConfigurationManager** クラスには次のメンバーがあります。

- [メソッド][]

### <a name="methods"></a>メソッド

**MSFT_DSCLocalConfigurationManager** クラスでは、次のメソッドを使用できます。

|メソッド |説明 |
|:--- |:---|
| [ApplyConfiguration (ブール値)](msft-dsclocalconfigurationmanager-applyconfiguration.md)| 構成エージェントを使用して、保留中の構成を適用します。|
| [DisableDebugConfiguration()](msft-dsclocalconfigurationmanager-disabledebugconfiguration.md)| DSC リソースのデバッグを無効にします。|
| [EnableDebugConfiguration (ブール値)](msft-dsclocalconfigurationmanager-enabledebugconfiguration.md)| DSC リソースのデバッグを有効にします。|
| [GetConfiguration()](msft-dsclocalconfigurationmanager-getconfiguration.md)| 構成ドキュメントを管理ノードに送信し、構成エージェントの **Get** メソッドを使用して構成を適用します。|
| [GetConfigurationResultOutput](msft-dsclocalconfigurationmanager-getconfigurationresultoutput.md)| 特定のジョブに関連する構成エージェントの出力を取得します。|
| [GetConfigurationStatus](msft-dsclocalconfigurationmanager-getconfigurationstatus.md)| 構成状態の履歴を取得します。|
| [GetMetaConfiguration](msft-dsclocalconfigurationmanager-getmetaconfiguration.md)| 構成エージェントを制御するために使用する LCM 設定を取得します。|
| [PerformRequiredConfigurationChecks](msft-dsclocalconfigurationmanager-performrequiredconfigurationchecks.md)| 整合性チェックを開始します。|
| [RemoveConfiguration](msft-dsclocalconfigurationmanager-removeconfiguration.md)| 構成ファイルを削除します。|
| [ResourceGet](msft-dsclocalconfigurationmanager-resourceget.md)| DSC リソースの **Get** メソッドを直接呼び出します。|
| [ResourceSet](msft-dsclocalconfigurationmanager-resourceset.md)| DSC リソースの **Set** メソッドを直接呼び出します。|
| [ResourceTest](msft-dsclocalconfigurationmanager-resourcetest.md)| DSC リソースの **Test** メソッドを直接呼び出します。|
| [RollBack](msft-dsclocalconfigurationmanager-rollback.md)| 以前の構成にロールバックします。|
| [SendConfiguration](msft-dsclocalconfigurationmanager-sendconfiguration.md)| 構成ドキュメントを管理ノードに送信し、保留中の変更として保存します。|
| [SendConfigurationApply](msft-dsclocalconfigurationmanager-sendconfigurationapply.md)| 構成ドキュメントを管理ノードに送信し、構成エージェントを使用して構成を適用します。|
| [SendConfigurationApplyAsync](msft-dsclocalconfigurationmanager-sendconfigurationapplyasync.md)| 構成ドキュメントを管理ノードに送信し、構成エージェントの使用を開始して構成を適用します。 GetConfigurationResultOutput を使用して、結果の出力を取得します。|
| [SendMetaConfigurationApply](msft-dsclocalconfigurationmanager-sendmetaconfigurationapply.md)| 構成エージェントを制御するために使用する LCM の設定を設定します。|
| [StopConfiguration](msft-dsclocalconfigurationmanager-stopconfiguration.md)| 進行中の構成を停止します。|
| [TestConfiguration](msft-dsclocalconfigurationmanager-testconfiguration.md)| 構成ドキュメントを管理ノードに送信し、そのドキュメントに対して現在の構成を検証します。|

## <a name="requirements"></a>必要条件

**MOF:** DscCore.mof

**名前空間** : Root\Microsoft\Windows\DesiredStateConfiguration
