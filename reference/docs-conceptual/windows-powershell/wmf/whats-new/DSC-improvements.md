---
ms.date: 06/12/2017
title: WMF 5.1 の DSC 機能強化
description: この記事には、WMF 5.1 に含まれる Desired State Configuration (DSC) の機能強化が列挙されています
ms.openlocfilehash: 564cf0e8321b6b2b2e5d856acd4d3644d6566100
ms.sourcegitcommit: 488a940c7c828820b36a6ba56c119f64614afc29
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/27/2020
ms.locfileid: "92667291"
---
# <a name="improvements-in-desired-state-configuration-dsc-in-wmf-51"></a>WMF 5.1 の Desired State Configuration (DSC) の機能強化

## <a name="dsc-class-resource-improvements"></a>DSC クラス リソースの機能強化

WMF 5.1 で、次の既知の問題を修正しました。

- クラスベースの DSC リソースの Get() 関数によって複合/ハッシュ テーブル型が返される場合、Get-DscConfiguration は空の値 (null) やエラーを返すことがあります。
- Get-DscConfiguration は、RunAs 資格情報が DSC 構成で使用される場合、エラーを返します。
- コンポジット構成では、クラス ベースのリソースは使用できません。
- Start-DscConfiguration は、クラス ベースのリソースに独自の型のプロパティがあると、応答を停止します。
- クラス ベースのリソースは排他リソースとして使用できません。

## <a name="dsc-resource-debugging-improvements"></a>DSC リソースのデバッグの機能強化

WMF 5.0 では、PowerShell デバッガーは、クラス ベースのリソース メソッド (Get/Set/Test) で直接停止しませんでした。 WMF 5.1 では、このデバッガーは、MOF ベースのリソース メソッドと同様に、クラス ベースのリソース メソッドで停止します。

## <a name="dsc-pull-client-supports-tls-11-and-tls-12"></a>DSC プル クライアントは TLS 1.1 と TLS 1.2 をサポート

以前、DSC プル クライアントは HTTPS 接続で SSL3.0 と TLS1.0 にのみ対応していました。 より安全なプロトコルを使用するように強制されると、このプル クライアントは動作を停止しました。 WMF 5.1 では、DSC プル クライアントは SSL 3.0 をサポートせず、より安全な TLS 1.1 プロトコルと TLS 1.2 プロトコルをサポートします。

## <a name="improved-pull-server-registration"></a>プル サーバー登録の機能強化

以前のバージョンの WMF では、ESENT データベースを使用しながら同時に DSC プル サーバーに登録/レポートを要求すると、LCM は登録またはレポートに失敗していました。 そのような場合、プル サーバーのイベント ログに "既に使用されているインスタンス名" というエラーが記録されました。 これは、マルチスレッド シナリオで ESENT データベースにアクセスするとき、間違ったパターンが使用されることに起因していました。 WMF 5.1 では、この問題は修正されました。 同時登録または同時レポート (ESENT データベースを含む) が WMF 5.1 では正常に機能します。 この問題は ESENT データベースにのみ関連し、OLEDB データベースには関連しません。

## <a name="enable-circular-log-on-esent-database-instance"></a>ESENT データベース インスタンスでの循環ログの有効化

以前のバージョンの DSC-PullServer では、データベース インスタンスが循環ログなしで作成されていたため、ESENT データベース ログ ファイルがプル サーバーのディスク領域を占有していました。 このリリースには、プルサーバーの web.config を使用してインスタンスの循環ログ動作を制御するオプションがあります。 既定では、CircularLogging が TRUE に設定されます。

```xml
<appSettings>
    <add key="dbprovider" value="ESENT" />
    <add key="dbconnectionstr" value="C:\Program Files\WindowsPowerShell\DscService\Devices.edb" />
    <add key="CheckpointDepthMaxKB" value="512" />
    <add key="UseCircularESENTLogs" value="TRUE" />
  </appSettings>
```

## <a name="wow64-support-for-configuration-keyword"></a>構成キーワードの WOW64 サポート

64 ビット コンピューターの WOW64 で、`Configuration` キーワードがサポートされるようになりました。 これは、64 ビット コンピューターで実行されている Windows PowerShell ISE (x86) などの 32 ビット プロセス内で DSC 構成を定義してコンパイルできることを意味します。

## <a name="pull-partial-configuration-naming-convention"></a>部分構成命名規則のプル

以前のリリースでは、部分構成の命名規則は、プル サーバー/サービスの MOF ファイル名はローカル構成マネージャー設定に指定されている部分構成名に一致する必要があり、ローカル構成マネージャー設定は MOF ファイルに組み込まれている構成名に一致する必要があるというものでした。

下のスナップショットを参照してください。

- ローカル構成設定により、あるノードに受信を許可する部分構成が定義されます。

  ![サンプルのメタ構成](media/DSC-improvements/MetaConfigPartialOne.png)

- サンプルの部分構成定義

  ```powershell
  Configuration PartialOne
  {
      Node('localhost')
      {
          File test
          {
              DestinationPath = "$env:TEMP\partialconfigexample.txt"
              Contents = 'Partial Config Example'
          }
      }
  }
  PartialOne
  ```

- 生成された MOF ファイルに組み込まれている 'ConfigurationName'。

  ![生成された MOF ファイルのサンプル](media/DSC-improvements/PartialGeneratedMof.png)

- プル構成リポジトリの FileName

  ![構成リポジトリの FileName](media/DSC-improvements/PartialInConfigRepository.png)

  Azure Automation サービス名により、MOF ファイルが `<ConfigurationName>.<NodeName>.mof` として生成されました。 そのため、下の構成は PartialOne.localhost.mof にコンパイルされます。

  これで、Azure Automation サービスから部分構成をプルできなくなりました。

  ```powershell
  Configuration PartialOne
  {
      Node('localhost')
      {
          File test
          {
              DestinationPath = "$env:TEMP\partialconfigexample.txt"
              Contents = 'Partial Config Example'
          }
      }
  }
  PartialOne
  ```

  WMF 5.1 では、プル サーバー/サービスの部分構成の名前を `<ConfigurationName>.<NodeName>.mof` にできます。 さらに、コンピューターがプル サーバー/サービスから 1 つの構成をプルする場合、プル サーバー構成リポジトリの構成ファイルに任意のファイル名を与えることができます。 このように命名規則が柔軟なことから、ノードを部分的に Azure Automation サービスで管理し (ノードの一部の構成が Azure Automation DSC から誘導されます)、部分構成をローカル管理できます。

  以下のメタ構成では、ローカルと Azure Automation サービスの両方で管理されるようにノードが設定されます。

  ```powershell
  [DscLocalConfigurationManager()]
  Configuration RegistrationMetaConfig
  {
      Settings
      {
          RefreshFrequencyMins = 30
          RefreshMode = "PULL"
      }

      ConfigurationRepositoryWeb web
      {
          ServerURL =  $endPoint
          RegistrationKey = $registrationKey
          ConfigurationNames = $configurationName
      }

      # Partial configuration managed by Azure Automation service.
      PartialConfiguration PartialConfigurationManagedByAzureAutomation
      {
          ConfigurationSource = "[ConfigurationRepositoryWeb]Web"
      }

      # This partial configuration is managed locally.
      PartialConfiguration OnPremisesConfig
      {
          RefreshMode = "PUSH"
          ExclusiveResources = @("Script")
      }

  }

  RegistrationMetaConfig
  Set-DscLocalConfigurationManager -Path .\RegistrationMetaConfig -Verbose
  ```

## <a name="using-psdscrunascredential-with-dsc-composite-resources"></a>PsDscRunAsCredential と DSC 複合リソースを使用する

DSC [複合](/powershell/scripting/dsc/resources/authoringresourcecomposite)リソースで [PsDscRunAsCredential](/powershell/scripting/dsc/configurations/runAsUser) を使用できるようになりました。

構成内で複合リソースを使用するとき、 **PsDscRunAsCredential** の値を指定できるようになりました。 指定されると、すべてのリソースが複合リソース内で RunAs ユーザーとして実行されます。 複合リソースが別の複合リソースを呼び出す場合、すべてのそれらのリソースも RunAs ユーザーとして実行されます。 RunAs 資格情報が複合リソース階層のあらゆるレベルに配信されます。 複合リソース内のいずれかのリソースで **PsDscRunAsCredential** に対して独自の値が指定されている場合、構成コンパイル中に結合エラーが発生します。

この例では、PSDesiredStateConfiguration モジュールに含まれている [WindowsFeatureSet](/powershell/scripting/dsc/reference/resources/windows/windowsfeaturesetresource) 複合リソースの利用が示されています。

```powershell
Configuration InstallWindowsFeature
{
    Import-DscResource -ModuleName PSDesiredStateConfiguration

    Node $AllNodes.NodeName
    {
        WindowsFeatureSet features
        {
            Name = @("Telnet-Client","SNMP-Service")
            Ensure = "Present"
            IncludeAllSubFeature = $true
            PsDscRunAsCredential = Get-Credential
        }
    }
}

$configData = @{
    AllNodes = @(
        @{
            NodeName             = 'localhost'
            PSDscAllowDomainUser = $true
            CertificateFile      = 'C:\publicKeys\targetNode.cer'
            Thumbprint           = '7ee7f09d-4be0-41aa-a47f-96b9e3bdec25'
        }
    )
}

InstallWindowsFeature -ConfigurationData $configData
```

## <a name="allowing-for-identical-duplicate-resources-in-a-configuration"></a>構成における同一重複リソースの許容

DSC は、構成内で競合するリソース定義を許可せず、処理しません。 競合を解決するのではなく、ただの失敗となります。 複合リソースなどの間で構成の再利用が多く行われるようになるほど、競合は頻繁に発生します。 競合するリソース定義が一致する場合、DSC は賢くこれを許可します。 このリリースにより、同一の定義を持つ複数のリソース インスタンスをサポートするようになりました:

```powershell
Configuration IIS_FrontEnd
{
    WindowsFeature FE_IIS         #Identical resource
    {
        Ensure = 'Present'
        Name = 'Web-Server'
    }

    WindowsFeature FTP
    {
        Ensure = 'Present'
        Name = 'Web-FTP-Server'
    }
}

Configuration IIS_Worker
{
    WindowsFeature Worker_IIS      #Identical resource
    {
        Ensure = 'Present'
        Name = 'Web-Server'
    }

    WindowsFeature ASP
    {
        Ensure = 'Present'
        Name = 'Web-ASP-Net45'
    }
}

Configuration WebApplication
{
    IIS_Frontend Web {}

    IIS_Worker ASP {}
}
```

以前のリリースでは、'Web サーバー' ロールが確実にインストールされるようにするために発生する WindowsFeature FE_IIS と WindowsFeature Worker_IIS インスタンス間の競合により、コンパイルの失敗に終わっていました。 構成が実行される *すべて* のプロパティは、これら 2 つの構成間で一致していることに注目してください。 これらの 2 つのプロパティのリソースは *すべて* 一致するため、結果として正常にコンパイルできるようになりました。

2 つのリソース間でいずれかのプロパティが異なる場合、一致していないと見なされ、コンパイルは失敗します。

## <a name="dsc-module-and-configuration-signing-validations"></a>DSC のモジュールと構成の署名検証

DSC では、プル サーバーからマネージド コンピューターに構成とモジュールが配信されます。 プル サーバーが侵害された場合、攻撃者はプル サーバーで構成とモジュールを改ざんし、すべてのマネージド ノードに配信する可能性があります。

WMF 5.1 では、DSC はカタログ ファイルと構成 (.MOF) ファイルのデジタル署名の検証に対応しています。 この機能では、信頼できる署名者が署名していない、あるいは信頼できる署名者が署名した後に改ざんされた構成ファイルまたはモジュール ファイルの実行が防止されます。

### <a name="how-to-sign-configuration-and-module"></a>構成とモジュールに署名する方法

- 構成ファイル (.MOF): 既存の PowerShell コマンドレット [Set-AuthenticodeSignature](/powershell/module/Microsoft.PowerShell.Security/Set-AuthenticodeSignature) が拡張され、MOF ファイルの署名に対応しています。
- モジュール: モジュールの署名は、次の手順を利用し、対応するモジュール カタログに署名することで完了します:
  1. カタログ ファイルの作成: カタログ ファイルには、暗号法のハッシュまたは拇印の集まりが含まれています。 拇印はそれぞれ、モジュールに含まれるファイルに対応します。 新しいコマンドレット [New-FileCatalog](/powershell/module/microsoft.powershell.security/new-filecatalog) が追加され、ユーザーは自分のモジュールのカタログ ファイルを作成できます。
  2. カタログ ファイルの署名: [Set-AuthenticodeSignature](/powershell/module/Microsoft.PowerShell.Security/Set-AuthenticodeSignature) を利用し、カタログ ファイルに署名します。
  3. モジュール フォルダー内にカタログ ファイルを配置します。 慣例では、モジュール カタログ ファイルは、モジュールと同じ名前のモジュール フォルダーの下に配置する必要があります。

### <a name="localconfigurationmanager-settings-to-enable-signing-validations"></a>LocalConfigurationManager 設定で署名検証を有効にする

#### <a name="pull"></a>プル

ノードの LocalConfigurationManager は、その現在の設定に基づき、モジュールと構成の署名を検証します。 既定では、署名検証は無効です。 署名検証は、"SignatureValidation" ブロックを下の図のようにノードのメタ構成定義に追加することで有効にできます。

```powershell
[DSCLocalConfigurationManager()]
Configuration EnableSignatureValidation
{
    Settings
    {
        RefreshMode = 'PULL'
    }

    ConfigurationRepositoryWeb pullserver{
      ConfigurationNames = 'sql'
      ServerURL = 'http://localhost:8080/PSDSCPullServer/PSDSCPullServer.svc'
      AllowUnsecureConnection = $true
      RegistrationKey = 'd6750ff1-d8dd-49f7-8caf-7471ea9793fc' # Replace this with correct registration key.
    }
    SignatureValidation validations{
        # If the TrustedStorePath property is provided then LCM will use the custom path. Otherwise, the LCM will use default trusted store path (Cert:\LocalMachine\DSCStore) to find the signing certificate.
        TrustedStorePath = 'Cert:\LocalMachine\DSCStore'
        SignedItemType = 'Configuration','Module'         # This is a list of DSC artifacts, for which LCM need to verify their digital signature before executing them on the node.
    }

}
EnableSignatureValidation
Set-DscLocalConfigurationManager -Path .\EnableSignatureValidation -Verbose
```

ノードに上記のメタ構成を設定すると、ダウンロードした構成とモジュールで署名を検証できます。 ローカル構成マネージャーは次の手順でデジタル署名を検証します。

1. `Get-AuthenticodeSignature` コマンドレットを使って、構成ファイル (.MOF) での署名が有効であることを検証します。
2. 署名者が信頼できることを認定した証明機関を検証します。
3. 構成のモジュール/リソース依存性を一時的な場所にダウンロードします。
4. モジュール内に含まれるカタログの署名を検証します。
   - `<moduleName>.cat` ファイルを見つけ、`Get-AuthenticodeSignature` を使ってその署名を検証します。
   - 署名者が信頼できることを認定した証明機関を検証します。
   - 新しいコマンドレット `Test-FileCatalog` を利用し、モジュールのコンテンツが改ざんされていないことを検証します。
5. `$env:ProgramFiles\WindowsPowerShell\Modules\` に対して `Install-Module` を行います
6. プロセス構成

> [!NOTE]
> モジュール カタログと構成の署名検証は、構成がシステムに最初に適用されるときか、モジュールがダウンロードされ、インストールされるときにのみ実行されます。
> 整合性実行では、Current.mof やそのモジュール依存性の署名は検証されません。 何らかの段階で検証に失敗した場合、たとえば、プル サーバーからプルされた構成に署名がされていない場合、構成の処理が中止となり、下にエラーが表示されます。一時ファイルはすべて削除されます。

![構成のエラー出力のサンプル](media/DSC-improvements/PullUnsignedConfigFail.png)

同様に、カタログに署名のないモジュールがプルされると次のエラーが発生します。

![モジュールのエラー出力のサンプル](media/DSC-improvements/PullUnisgnedCatalog.png)

#### <a name="push"></a>プッシュ

プッシュで配信された構成は、ノードに届く前にその出所で改ざんされていた可能性があります。 ローカル構成マネージャーは、プッシュまたは公開された構成に対して同様の署名検証手順を実行します。 以下は、プッシュの署名検証の完全例です。

- ノードで署名検証を有効にします。

  ```powershell
  [DSCLocalConfigurationManager()]
  Configuration EnableSignatureValidation
  {
      Settings
      {
          RefreshMode = 'PUSH'
      }
      SignatureValidation validations{
          TrustedStorePath = 'Cert:\LocalMachine\DSCStore'
          SignedItemType =  'Configuration','Module'
      }

  }
  EnableSignatureValidation
  Set-DscLocalConfigurationManager -Path .\EnableSignatureValidation -Verbose
  ```

- サンプル構成ファイルを作成します。

  ```powershell
  # Sample configuration
  Configuration Test
  {

      File foo
      {
          DestinationPath =  "$env:TEMP\signingTest.txt"
          Contents = "ABC"
      }
  }
  Test
  ```

- 署名のない構成ファイルをノードにプッシュしてみます。

  ```powershell
  Start-DscConfiguration -Path .\Test -Wait -Verbose -Force
  ```

  ![エラー - 署名のない MOF ファイルがプッシュされました](media/DSC-improvements/PushUnsignedMof.png)

- コード署名証明書を利用し、構成ファイルに署名します。

  ![MOF ファイルに署名する](media/DSC-improvements/SignMofFile.png)

- 署名された MOF ファイルをプッシュしてみます。

  ![署名された MOF ファイルをプッシュする](media/DSC-improvements/PushSignedMof.png)
