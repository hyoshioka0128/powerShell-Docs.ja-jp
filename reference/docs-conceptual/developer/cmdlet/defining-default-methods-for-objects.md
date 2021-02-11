---
ms.date: 09/13/2016
ms.topic: reference
title: オブジェクトの既定のメソッドを定義する
description: オブジェクトの既定のメソッドを定義する
ms.openlocfilehash: c65ca91a7038f32d8c3ef62cfe7881e5ad4dba5a
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355529"
---
# <a name="defining-default-methods-for-objects"></a>オブジェクトの既定のメソッドを定義する

.NET Framework オブジェクトを拡張すると、コードメソッドとスクリプトメソッドをオブジェクトに追加できます。
これらのメソッドの定義に使用される XML については、次のセクションで説明します。

> [!NOTE]
> 次のセクションの例は、 `Types.ps1xml` Windows PowerShell インストールディレクトリ () の種類のファイルに含まれてい `$PSHOME` ます。 詳細については、「 [About Types.ps1xml](/powershell/module/microsoft.powershell.core/about/about_types.ps1xml)」を参照してください。

## <a name="code-methods"></a>コードメソッド

コードメソッドは、.NET Framework オブジェクトの静的メソッドを参照しています。

次の例では、 **ToString** メソッドが [System.Xml.Xmlノード](/dotnet/api/System.Xml.XmlNode) 型に追加されます。 [Pscodemethod](/dotnet/api/system.management.automation.pscodemethod)要素は、拡張メソッドをコードメソッドとして定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name#System_Management_Automation_PSMemberInfo_Name)要素は、拡張メソッドの名前を指定します。 また、 [Codereference 参照](/dotnet/api/system.management.automation.pscodemethod.codereference#System_Management_Automation_PSCodeMethod_CodeReference) 要素は静的メソッドを指定します。 [Pscodemethod](/dotnet/api/system.management.automation.pscodemethod)要素を[psmembers](/dotnet/api/system.management.automation.psmemberset)要素のメンバーに追加することもできます。

```xml
<Type>
  <Name>System.Xml.XmlNode</Name>
  <Members>
    <CodeMethod>
      <Name>ToString</Name>
      <CodeReference>
        <TypeName>Microsoft.PowerShell.ToStringCodeMethods</TypeName>
        <MethodName>XmlNode</MethodName>
      </CodeReference>
    </CodeMethod>
  </Members>
</Type>
```

## <a name="script-methods"></a>スクリプトメソッド

スクリプトメソッドは、値がスクリプトの出力であるメソッドを定義します。 次の例では、 **Converttodatetime** メソッドが [system.management.managementobject](/dotnet/api/System.Management.ManagementObject) 型に追加されています。 [Psscriptmethod](/dotnet/api/system.management.automation.psscriptmethod)要素は、拡張メソッドをスクリプトメソッドとして定義します。 [Name](/dotnet/api/system.management.automation.psmemberinfo.name#System_Management_Automation_PSMemberInfo_Name)要素は、拡張メソッドの名前を指定します。 また、 [script](/dotnet/api/system.management.automation.psscriptmethod.script#System_Management_Automation_PSScriptMethod_Script) 要素は、メソッド値を生成するスクリプトを指定します。 [Psscriptmethod](/dotnet/api/system.management.automation.psscriptmethod)要素は、 [psmembers](/dotnet/api/system.management.automation.psmemberset)要素のメンバーに追加することもできます。

```xml
<Type>
  <Name>System.Management.ManagementObject</Name>
  <Members>
    <ScriptMethod>
      <Name>ConvertToDateTime</Name>
      <Script>
        [System.Management.ManagementDateTimeConverter]::ToDateTime($args[0])
      </Script>
    </ScriptMethod>
  </Members>
</Type>
```

## <a name="see-also"></a>関連項目

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
