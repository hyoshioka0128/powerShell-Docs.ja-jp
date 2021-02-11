---
description: .NET framework クラスで使用できる型アクセラレータについて説明します。
Locale: en-US
ms.date: 05/01/2020
online version: https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_type_accelerators?view=powershell-7.2&WT.mc_id=ps-gethelp
schema: 2.0.0
title: about_Type_Accelerators
ms.openlocfilehash: 67264326cc17b733e267225d521cfda0fa36d458
ms.sourcegitcommit: 95d41698c7a2450eeb70ef2fb6507fe7e6eff3b6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/17/2020
ms.locfileid: "99603013"
---
# <a name="about-type-accelerators"></a>型アクセラレータについて

## <a name="short-desription"></a>短いの
.NET framework クラスで使用できる型アクセラレータについて説明します。

## <a name="long-description"></a>詳細説明

型アクセラレータは、.NET framework クラスのエイリアスです。 クラス名全体を明示的に入力しなくても、特定の .NET framework クラスにアクセスできます。 たとえば、 **エイリアス属性** クラスをからに短縮でき `[System.Management.Automation.AliasAttribute]` `[Alias]` ます。

> [!NOTE]
> すべての型アクセラレータは、引き続き角かっこ () で囲む必要があり `[]` ます。

## <a name="available-type-accelerators"></a>使用可能な型アクセラレータ

|        アクセラレータ          |                           完全クラス名                           |
|---------------------------- | ------------------------------------------------------------------- |
|adsi                         | System.directoryservices                             |
|adsisearcher                 | System.directoryservices. DirectorySearcher                          |
|エイリアス                        | System. Automation. エイリアス属性                         |
|AllowEmptyCollection         | システムの管理. AllowEmptyCollectionAttribute          |
|AllowEmptyString             | システムの管理. AllowEmptyStringAttribute              |
|AllowNull                    | システムの管理. AllowNullAttribute                     |
|ArgumentCompleter            | システムの管理... 引数の属性             |
|引数の入力候補          | ArgumentCompletionsAttribute (システム管理)           |
|array                        | System.Array                                                        |
|bigint                       | BigInteger                                          |
|[bool]                         | System.Boolean                                                      |
|byte                         | System.Byte                                                         |
|char                         | System.Char                                                         |
|cimclass                     | CimClass (Microsoft 管理)                        |
|cimconverter                 | CimConverter (Microsoft 管理)                    |
|ciminstance                  | Microsoft.Management.Infrastructure.CimInstance                     |
|CimSession                   | Microsoft.Management.Infrastructure.CimSession                      |
|cimtype                      | CimType (Microsoft 管理)                         |
|CmdletBinding                | System.....................                 |
|cultureinfo                  | システムのグローバリゼーション                                    |
|datetime                     | System.DateTime                                                     |
|decimal                      | System.Decimal                                                      |
|double                       | System.Double                                                       |
|Get-dsclocalconfigurationmanager | システムの管理. DscLocalConfigurationManagerAttribute  |
|DscProperty                  | DscPropertyAttribute (システム管理)                   |
|DscResource                  | システムの管理. DscResourceAttribute                   |
|ExperimentAction             | ExperimentAction (システム管理)                       |
|実験用                 | ExperimentalAttribute (システム管理)                  |
|ExperimentalFeature          | ExperimentalFeature (システム管理)                    |
|float                        | System.Single                                                       |
|guid                         | System.Guid                                                         |
|テーブル                    | System.Collections.Hashtable                                        |
|initialsessionstate          | System.Management.Automation.Runspaces.InitialSessionState          |
|INT                          | System.Int32                                                        |
|int16                        | System.Int16                                                        |
|int32                        | System.Int32                                                        |
|int64                        | System.Int64                                                        |
|ipaddress                    | System .Net. IPAddress                                                |
|IPEndpoint                   | IPEndPoint                                               |
|long                         | System.Int64                                                        |
|mailaddress                  | システム .Net. Mail. MailAddress                                         |
|NullString                   | System.string (...)                    |
|ObjectSecurity               | Accesscontrol-namespace. ObjectSecurity                        |
|OutputType                   | OutputTypeAttribute (システム管理)                    |
|パラメーター                    | System.string. ParameterAttribute                     |
|PhysicalAddress              | システム .Net. NetworkInformation. PhysicalAddress                       |
|powershell                   | System. Automation. PowerShell                             |
|psaliasproperty              | PSAliasProperty (システム管理)                        |
|pscredential                 | システム.... PSCredential                           |
|pscustomobject               | システム管理. PSObject                               |
|PSDefaultValue               | System.Management.Automation.PSDefaultValueAttribute                |
|pslistmodifier               | PSListModifier (システム管理)                         |
|psmoduleinfo                 | PSModuleInfo (システム管理)                           |
|ps注プロパティ               | System. Management. Ps注プロパティ                         |
|psobject                     | システム管理. PSObject                               |
|psprimitivedictionary        | PSPrimitiveDictionary (システム管理)                  |
|pspropertyexpression         | Microsoft. PowerShell. PSPropertyExpression                  |
|psscriptmethod               | システムの管理. PSScriptMethod                         |
|psscriptproperty             | システムの管理. PSScriptProperty                       |
|PSTypeNameAttribute          | PSTypeNameAttribute (システム管理)                    |
|system.management.automation.psvariable                   | システム管理. PSVariable                             |
|ps変数プロパティ           | システムの管理. Ps変数プロパティ                     |
|ref                          | システムの管理. PSReference                            |
|regex                        | System.Text.RegularExpressions.Regex                                |
|実行空間                     | システム管理. 実行空間                     |
|runspacefactory              | RunspaceFactory (システム管理)              |
|sbyte                        | System.SByte                                                        |
|scriptblock                  | システムの管理. ScriptBlock                            |
|securestring                 | System.Security.SecureString                                        |
|semver                       | SemanticVersion (システム管理)                        |
|short                        | System.Int16                                                        |
|single                       | System.Single                                                       |
|string                       | System.String                                                       |
|SupportsWildcards            | SupportsWildcardsAttribute (システム管理)             |
|switch                       | System.Management.Automation.SwitchParameter                        |
|TimeSpan                     | System.TimeSpan                                                     |
|type                         | System.Type                                                         |
|uint                         | System.UInt32                                                       |
|uint16                       | System.UInt16                                                       |
|uint32                       | System.UInt32                                                       |
|uint64                       | System.UInt64                                                       |
|ulong                        | System.UInt64                                                       |
|uri                          | System.Uri                                                          |
|ushort                       | System.UInt16                                                       |
|ValidateCount                | System. Automation. ValidateCountAttribute                 |
|ValidateDrive                | System.servicemodel. Validate! 属性                 |
|ValidateLength               | ValidateLengthAttribute (システム管理)                |
|ValidateNotNull              | System.string. ValidateNotNullAttribute               |
|ValidateNotNullOrEmpty       | システムの管理. ValidateNotNullOrEmptyAttribute        |
|ValidatePattern              | システムの管理. Validatepattern 属性               |
|ValidateRange                | System. Automation. ValidateRangeAttribute                 |
|ValidateScript               | ValidateScriptAttribute (システム管理)                |
|ValidateSet                  | ValidateSetAttribute (システム管理)                   |
|ValidateTrustedData          | システムの管理. ValidateTrustedDataAttribute           |
|ValidateUserDrive            | System.servicemodel. Validateuseros 属性             |
|version                      | System.Version                                                      |
|void                         | System.Void                                                         |
|WildcardPattern              | WildcardPattern (システム管理)                        |
|wmi                          | System.management.managementobject                                  |
|wmiclass                     | ManagementClass                                   |
|wmisearcher                  | ManagementObjectSearcher                          |
|System.security.cryptography.x509certificates.x500distinguishedname        | System.Security.Cryptography.X509Certificates.X500DistinguishedName |
|X509Certificate              | System.Security.Cryptography.X509Certificates.X509Certificate       |
|xml                          | System.Xml.XmlDocument                                              |

