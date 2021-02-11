---
ms.date: 09/13/2016
ms.topic: reference
title: パラメーター セットを宣言する方法
description: パラメーター セットを宣言する方法
ms.openlocfilehash: bd4d504a9fe6c7f7626901c49bc08851244f0995
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "92667064"
---
# <a name="how-to-declare-parameter-sets"></a>パラメーター セットを宣言する方法

この例では、コマンドレットのパラメーターを宣言するときに2つのパラメーターセットを定義する方法を示します。 各パラメーターセットには、一意のパラメーターと、両方のパラメーターセットで使用される共有パラメーターの両方があります。 既定のパラメーターセットの指定方法など、パラメーターセットの詳細については、「 [コマンドレットパラメーターセット](./cmdlet-parameter-sets.md)」を参照してください。

> [!IMPORTANT]
> 可能な限り、パラメーターセットの一意のパラメーターを必須パラメーターとして定義します。 ただし、パラメーターを指定せずにコマンドレットを実行する場合は、unique パラメーターを省略可能なパラメーターにすることができます。 たとえば、コマンドレットの一意のパラメーター `Get-Command` は省略可能です。

## <a name="how-to-define-two-parameter-sets"></a>2つのパラメーターセットを定義する方法

1. 最初の `ParameterSet` パラメーターセットの unique パラメーターのパラメーター属性にキーワードを追加します。

   ```csharp
   [Parameter(Position = 0, Mandatory = true,
              ParameterSetName = "Test01")]
   public string UserName
   {
     get { return userName; }
     set { userName = value; }
   }
   private string userName;
   ```

2. `ParameterSet`2 番目のパラメーターセットの unique パラメーターの parameter 属性にキーワードを追加します。

   ```csharp
   [Parameter(Position = 0, Mandatory = true,
              ParameterSetName = "Test02")]
   public string ComputerName
   {
     get { return computerName; }
     set { computerName = value; }
   }
   private string computerName;
   ```

3. 両方のパラメーターセットに属するパラメーターについては、各パラメーターセットにパラメーター属性を追加し、 `ParameterSet` 各セットにキーワードを追加します。 各パラメーター属性では、パラメーターの定義方法を指定できます。 パラメーターは、1つのセットでは省略可能であり、別のセットでは必須です。

   ```csharp
   [Parameter(Mandatory= true, ParameterSetName = "Test01")]
   [Parameter(ParameterSetName = "Test02")]
   public string SharedParam
   {
       get { return sharedParam; }
       set { sharedParam = value; }
   }
   private string sharedParam;
   ```

## <a name="see-also"></a>参照

[コマンドレット パラメーター セット](./cmdlet-parameter-sets.md)

[Windows PowerShell コマンドレットの記述](./writing-a-windows-powershell-cmdlet.md)
