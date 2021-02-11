---
description: PowerShell での出力ストリームの可用性と用途について説明します。
Locale: en-US
ms.date: 10/13/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_output_streams?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Output_Streams
ms.openlocfilehash: 1dd6424ea14aa241b084a0a2c97a7e9bf6927518
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99600119"
---
# <a name="about-output-streams"></a>出力ストリームについて

## <a name="short-description"></a>簡単な説明
PowerShell での出力ストリームの可用性と用途について説明します。

## <a name="long-description"></a>長い説明

PowerShell には複数の出力ストリームが用意されています。 ストリームは、さまざまな種類のメッセージのチャネルを提供します。 これらのストリームには、関連付けられているコマンドレットまたはリダイレクトを使用して書き込むことができます。 詳細については、「 [about_Redirection](about_Redirection.md)」を参照してください。

PowerShell では、次の出力ストリームがサポートされています。

| 一連# |      説明       | で導入  |    Write コマンドレット     |
| -------- | ---------------------- | -------------- | ------------------- |
| 1        | **成功** のストリーム     | PowerShell 2.0 | `Write-Output`      |
| 2        | **エラー** ストリーム       | PowerShell 2.0 | `Write-Error`       |
| 3        | **警告** ストリーム     | PowerShell 3.0 | `Write-Warning`     |
| 4        | **詳細** ストリーム     | PowerShell 3.0 | `Write-Verbose`     |
| 5        | **デバッグ** ストリーム       | PowerShell 3.0 | `Write-Debug`       |
| 6        | **情報** ストリーム | PowerShell 5.0 | `Write-Information` |
| 該当なし      | **進行状況** ストリーム    | PowerShell 3.0 | `Write-Progress`    |

> [!NOTE]
> **進行状況** ストリームはリダイレクトをサポートしていません。

## <a name="success-stream"></a>成功のストリーム

**成功** のストリームは、通常の正常な結果の既定のストリームです。
`Write-Output`このストリームにオブジェクトを明示的に書き込むには、コマンドレットを使用します。 このストリームは、PowerShell パイプラインを介してオブジェクトを渡すために使用されます。 **正常に完了** したストリームは、ネイティブアプリケーションの **stdout** ストリームに接続されます。

## <a name="error-stream"></a>エラーストリーム

エラー **ストリームは** 、エラー結果の既定のストリームです。 `Write-Error`このストリームに明示的に書き込むには、コマンドレットを使用します。 **エラー** ストリームは、ネイティブアプリケーションの **stderr** ストリームに接続されています。 ほとんどの状況では、これらのエラーによって実行パイプラインが終了します。 このストリームに書き込まれたエラーは、自動変数にも追加され `$Error` ます。 詳細については、「 [about_Automatic_Variables](about_Automatic_Variables.md)」を参照してください。

## <a name="warning-stream"></a>警告ストリーム

**警告** ストリーム **は、エラーストリームに** 書き込まれたエラーよりも重大度が低いエラー状態を対象としています。 通常の状況では、これらの警告は実行を終了しません。 警告は自動変数に書き込まれません `$Error` 。 `Write-Warning`このストリームに明示的に書き込むには、コマンドレットを使用します。

## <a name="verbose-stream"></a>詳細ストリーム

**詳細** ストリームは、対話形式で実行されるか、スクリプトから実行される場合でも、ユーザーのトラブルシューティングに役立つメッセージを対象としています。 コマンドレットを使用して、 `Write-Verbose` このストリームにメッセージを明示的に書き込みます。 多くのコマンドレットは、コマンドレットの内部動作を理解するのに役立つ詳細出力を提供します。 詳細メッセージは、共通パラメーターを使用した場合にのみ出力され `-Verbose` ます。 詳細については、「[about_CommonParameters](about_CommonParameters.md)」を参照してください。

## <a name="debug-stream"></a>デバッグ ストリーム

**デバッグ** ストリームは、コードが失敗した理由をスクリプトによって理解するのに役立つメッセージに使用されます。 `Write-Debug`このストリームに明示的に書き込むには、コマンドレットを使用します。 デバッグメッセージは、共通パラメーターを使用した場合にのみ出力され `-Debug` ます。 詳細については、「[about_CommonParameters](about_CommonParameters.md)」を参照してください。

デバッグメッセージは、エンドユーザーよりも多くのスクリプトとコマンドレット開発者を対象としています。 これらのデバッグメッセージには、詳細なトラブルシューティングに必要な内部の詳細が含まれている場合があります。

## <a name="information-stream"></a>情報ストリーム

この **情報** ストリームは、スクリプトが何を実行しているかをユーザーが理解するのに役立つメッセージを提供することを目的としています。 また、PowerShell を介して情報を渡すために使用される追加のストリームとして開発者が使用することもできます。 開発者はストリームデータにタグを付け、そのストリームに対して特定の処理を行うことができます。 `Write-Information`このストリームに明示的に書き込むには、コマンドレットを使用します。

## <a name="progress-stream"></a>進行状況ストリーム

**進行状況** ストリームは、実行時間が長いコマンドやスクリプトの進行状況を通知するメッセージに使用されます。 コマンドレットを使用して、 `Write-Progress` このストリームにメッセージを明示的に書き込みます。 **進行状況** ストリームはリダイレクトをサポートしていません。

## <a name="see-also"></a>関連項目

- [Write-Debug](xref:Microsoft.PowerShell.Utility.Write-Debug)
- [Write-Error](xref:Microsoft.PowerShell.Utility.Write-Error)
- [Write-Host](xref:Microsoft.PowerShell.Utility.Write-Host)
- [Write-Information](xref:Microsoft.PowerShell.Utility.Write-Information)
- [Write-Output](xref:Microsoft.PowerShell.Utility.Write-Output)
- [Write-Progress](xref:Microsoft.PowerShell.Utility.Write-Progress)
- [Write-Verbose](xref:Microsoft.PowerShell.Utility.Write-Verbose)
- [Write-Warning](xref:Microsoft.PowerShell.Utility.Write-Warning)
- [about_CommonParameters](about_CommonParameters.md)
- [about_Redirection](about_Redirection.md)
