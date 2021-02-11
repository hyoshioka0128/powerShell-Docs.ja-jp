---
ms.date: 09/13/2016
ms.topic: reference
title: Windows PowerShell スナップインを記述する
description: Windows PowerShell スナップインを記述する
ms.openlocfilehash: f658c2fa1211bfb77d2e8edd3999ce7f92df13bb
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92659455"
---
# <a name="writing-a-windows-powershell-snap-in"></a>Windows PowerShell スナップインを記述する

この例では、すべてのコマンドレットと Windows PowerShell プロバイダーをアセンブリに登録するために使用できる Windows PowerShell スナップインを記述する方法を示します。

この種類のスナップインでは、登録するコマンドレットとプロバイダーは選択しません。 登録内容を選択できるスナップインを作成するには、「 [カスタム Windows PowerShell スナップインの作成](./writing-a-custom-windows-powershell-snap-in.md)」を参照してください。

### <a name="writing-a-windows-powershell-snap-in"></a>Windows PowerShell スナップインを記述する

1. Runインストーラ属性属性を追加します。

2. [Add-pssnapin](/dotnet/api/System.Management.Automation.PSSnapIn)クラスから派生するパブリッククラスを作成します。

    この例では、クラス名は "GetProcPSSnapIn01" です。

3. スナップインの名前のパブリックプロパティを追加します (必須)。 スナップインに名前を付ける場合、 `#` 、、 `.` `,` 、、 `(` 、 `)` `{` `}` `[` `]` `&` `-` `/` `\` `$` `;` `:` `"` `'` `<` `>` `|` `?` `@` 、、、 `` ` `` 、、、、、、、、、、、、、、、、、、、、、、、、 `*`

    この例では、スナップインの名前は "GetProcPSSnapIn01" です。

4. スナップインのベンダのパブリックプロパティを追加します (必須)。

    この例では、ベンダーは "Microsoft" です。

5. スナップインのベンダリソースのパブリックプロパティを追加します (省略可能)。

    この例では、vendor リソースは "GetProcPSSnapIn01, Microsoft" です。

6. スナップインの説明のパブリックプロパティを追加します (必須)。

    この例では、説明は "This は Windows PowerShell スナップインです。これは、get proc コマンドレットを登録する" です。

7. スナップインの説明リソースのパブリックプロパティを追加します (省略可能)。

    この例では、ベンダリソースは "GetProcPSSnapIn01, This は Windows PowerShell スナップインであり、これは get proc コマンドレットを登録する" です。

## <a name="example"></a>例

この例では、windows powershell シェルで Get-Proc コマンドレットを登録するために使用できる Windows PowerShell スナップインを記述する方法を示します。 この例では、完全なアセンブリには GetProcPSSnapIn01 スナップインクラスとコマンドレットクラスのみが含まれることに注意してください `Get-Proc` 。

```csharp
[RunInstaller(true)]
public class GetProcPSSnapIn01 : PSSnapIn
{
  /// <summary>
  /// Create an instance of the GetProcPSSnapIn01 class.
  /// </summary>
  public GetProcPSSnapIn01()
         : base()
  {
  }

  /// <summary>
  /// Specify the name of the PowerShell snap-in.
  /// </summary>
  public override string Name
  {
    get
    {
      return "GetProcPSSnapIn01";
    }
  }

  /// <summary>
  /// Specify the vendor for the PowerShell snap-in.
  /// </summary>
  public override string Vendor
  {
    get
    {
      return "Microsoft";
    }
  }

  /// <summary>
  /// Specify the localization resource information for the vendor.
  /// Use the format: resourceBaseName,VendorName.
  /// </summary>
  public override string VendorResource
  {
    get
    {
      return "GetProcPSSnapIn01,Microsoft";
    }
  }

  /// <summary>
  /// Specify a description of the PowerShell snap-in.
  /// </summary>
  public override string Description
  {
    get
    {
      return "This is a PowerShell snap-in that includes the get-proc cmdlet.";
    }
  }

  /// <summary>
  /// Specify the localization resource information for the description.
  /// Use the format: resourceBaseName,Description.
  /// </summary>
  public override string DescriptionResource
  {
    get
    {
      return "GetProcPSSnapIn01,This is a PowerShell snap-in that includes the get-proc cmdlet.";
    }
  }
}
```

## <a name="see-also"></a>参照

[コマンドレット、プロバイダー、およびホストアプリケーションを登録する方法](/previous-versions/ms714644(v=vs.85))

[Windows PowerShell シェル SDK](../windows-powershell-reference.md)
