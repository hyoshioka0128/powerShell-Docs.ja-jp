---
title: セキュリティのパラメーター |Microsoft Docs
ms.custom: ''
ms.date: 09/13/2016
ms.reviewer: ''
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: article
ms.assetid: e199bba3-90d3-41ca-9d78-cb502e58508d
caps.latest.revision: 6
ms.openlocfilehash: 9b4d83aeaf45eab1365dec5fbf48c3c796ed5bde
ms.sourcegitcommit: e7445ba8203da304286c591ff513900ad1c244a4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "62067435"
---
# <a name="security-parameters"></a>セキュリティのパラメーター

次の表は、推奨される名前と証明書のキーと特権情報を指定するパラメーターなどの操作のセキュリティ情報を提供するためのパラメーターの機能を示します。

|パラメーター|機能|
|---|---|
|**ACL**<br>データの種類:String|カタログのまたはする Uniform Resource Identifier (URI) の保護のアクセス制御のレベルを指定するには、このパラメーターを実装します。|
|**CertFile**<br>データの種類:String|ユーザーは、次のいずれかを含むファイルの名前を指定できるように、このパラメーターを実装します。<br>-Base64 または Distinguished Encoding Rules (DER) でエンコードされた x.509 証明書<br>-少なくとも 1 つの証明書とキーを含む公開キー暗号化標準 (PKCS) #12 ファイル|
|**CertIssuerName**<br>データの種類:String|ユーザーが証明書の発行者の名前を指定できるように、または、ユーザーは、部分文字列を指定できるように、このパラメーターを実装します。|
|**CertRequestFile**<br>データの種類:String|Base64 または DER でエンコードされた PKCS #10 証明書の要求を含むファイルの名前を指定するには、このパラメーターを実装します。|
|**CertSerialNumber**<br>データの種類:String|証明機関によって発行されたシリアル番号を指定するには、このパラメーターを実装します。|
|**CertStoreLocation**<br>データの種類:String|ユーザーが証明書ストアの場所を指定できるように、このパラメーターを実装します。 場所は、通常、ファイル パスです。|
|**CertSubjectName**<br>データの種類:String|ユーザーが証明書の発行者を指定できるように、または、ユーザーは、部分文字列を指定できるように、このパラメーターを実装します。|
|**CertUsage**<br>データの種類:String|キー使用法または拡張キー使用法を指定するには、このパラメーターを実装します。 キーは、ビット マスクを少し、オブジェクト識別子 (OID) または文字列を表現できます。|
|**資格情報**<br>データの種類:[System.Management.Automation.PSCredential](/dotnet/api/System.Management.Automation.PSCredential)|このパラメーターを実装するは、コマンドレットによってユーザー名またはパスワードをユーザーが自動的に要求できるようにします。 完全な資格情報が直接指定されていない場合は、両方のプロンプトが表示されます。|
|**CSPName**<br>データの種類:String|ユーザーが証明書サービス プロバイダー (CSP) の名前を指定できるように、このパラメーターを実装します。|
|**CSPType**<br>データの種類:整数|ユーザーは、CSP の種類を指定できるように、このパラメーターを実装します。|
|**グループ**<br>データの種類:String|ユーザーが使用するプリンシパルのアクセスのコレクションを指定できるように、このパラメーターを実装します。 詳細については、の説明を参照して、**プリンシパル**パラメーター。|
|**KeyAlgorithm**<br>データの種類:String|ユーザーがセキュリティに使用するキーの生成アルゴリズムを指定できるように、このパラメーターを実装します。|
|**キーコンテナー名**<br>データの種類:String|ユーザーがキー コンテナーの名前を指定できるように、このパラメーターを実装します。|
|**KeyLength**<br>データの種類:整数|ユーザーは、キーの長さをビット単位で指定できるように、このパラメーターを実装します。|
|**操作**<br>データの種類:String|ユーザーが保護されているオブジェクトで実行できるアクションを指定できるように、このパラメーターを実装します。|
|**プリンシパル**<br>データの種類:String|このパラメーターを実装するは、ユーザーがアクセスを特定できる一意のエンティティを指定できるようにします。|
|**特権**<br>データの種類:String|ユーザーは、右側のコマンドレットは、特定のエンティティの操作を実行する必要がありますを指定できるように、このパラメーターを実装します。|
|**権限**<br>データの種類:権限の配列|ユーザーが特定のエントリには、その操作を実行するコマンドレットが必要な権限を指定できるように、このパラメーターを実装します。|
|**ロール**<br>データの種類:String|ユーザーは、一連のエンティティで実行できる操作を指定できるように、このパラメーターを実装します。|
|**SaveCred**<br>データの種類:SwitchParameter|パラメーターを指定した場合、ユーザーが以前に保存された資格情報が使用できるように、このパラメーターを実装します。|
|**Scope**<br>データの種類:String|ユーザーが保護されているコマンドレットは、オブジェクトのグループを指定できるように、このパラメーターを実装します。|
|**SID**<br>データの種類:String|このパラメーターを実装するは、ユーザーは、プリンシパルを表す一意の識別子を指定できるようにします。|
|**信頼されています。**<br>データの種類:SwitchParameter|このパラメーターを実装するは、パラメーターを指定した場合に、信頼レベルがサポートされているようにします。|
|**trustLevel**<br>データの種類:キーワード|ユーザーがサポートされている信頼レベルを指定できるように、このパラメーターを実装します。 たとえば、使用可能な値には、インターネット、イントラネット、および fulltrust が含まれます。|

## <a name="see-also"></a>参照

[コマンドレットのパラメーター](./cmdlet-parameters.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)

[Windows PowerShell SDK](../windows-powershell-reference.md)
