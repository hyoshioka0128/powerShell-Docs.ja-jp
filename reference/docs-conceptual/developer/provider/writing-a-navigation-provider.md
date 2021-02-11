---
ms.date: 09/13/2016
ms.topic: reference
title: ナビゲーション プロバイダーを記述する
description: ナビゲーション プロバイダーを記述する
ms.openlocfilehash: 3123672d3365c97714557bd0e72a6e444ac228a0
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355223"
---
# <a name="writing-a-navigation-provider"></a>ナビゲーション プロバイダーを記述する

このトピックでは、入れ子になったコンテナー (複数レベルのデータストア)、項目の移動、および相対パスをサポートする Windows PowerShell プロバイダーのメソッドを実装する方法について説明します。 ナビゲーションプロバイダーは、 [system.servicemodel クラスから](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) 派生させる必要があります。

このトピックの例のプロバイダーでは、データストアとして Access データベースを使用しています。 データベースとの対話に使用されるヘルパーメソッドとクラスはいくつかあります。 ヘルパーメソッドを含む完全なサンプルについては、「 [AccessDBProviderSample05](./accessdbprovidersample05.md)」を参照してください。

Windows PowerShell プロバイダーの詳細については、「 [Windows Powershell プロバイダーの概要](./windows-powershell-provider-overview.md)」を参照してください。

## <a name="implementing-navigation-methods"></a>ナビゲーションメソッドの実装

System.servicemodel [クラスは、入れ子](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) になったコンテナー、相対パス、および項目の移動をサポートするメソッドを実装します。 これらのメソッドの完全な一覧については、「 [navigation[プロバイダーメソッド](/dotnet/api/system.management.automation.provider.navigationcmdletprovider#methods)]」を参照してください。

> [!NOTE]
> このトピックは、「 [Windows PowerShell プロバイダーのクイックスタート](./windows-powershell-provider-quickstart.md)」の情報に基づいています。 このトピックでは、プロバイダープロジェクトを設定する方法の基本については説明しません。また、ドライブを作成または削除する [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) クラスから継承されたメソッドを実装する方法についても説明しません。 また、このトピックでは、 [Containercmdletprovider](/dotnet/api/System.Management.Automation.Provider.ContainerCmdletProvider)クラスまたはクラスによって公開されるメソッドを実装する方法につい[ては説明](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider)しません。 項目のコマンドレットを実装する方法を示す例については、「 [項目プロバイダーを作成](./writing-an-item-provider.md)する」を参照してください。 コンテナーのコマンドレットを実装する方法を示す例については、「 [コンテナープロバイダーの作成](./writing-a-container-provider.md)」を参照してください。

### <a name="declaring-the-provider-class"></a>プロバイダークラスの宣言

プロバイダーを宣言し [て、system.servicemodel クラスから](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider) 派生させ、それを system.string [属性](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute)で装飾します。このクラスは、このクラスから派生します。

```
[CmdletProvider("AccessDB", ProviderCapabilities.None)]
   public class AccessDBProvider : NavigationCmdletProvider
   {

   }
```

### <a name="implementing-isitemcontainer"></a>IsItemContainer の実装

[Isitemcontainer *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.IsItemContainer)メソッドは、指定されたパスの項目がコンテナーであるかどうかを確認します。

```csharp
protected override bool IsItemContainer(string path)
      {
         if (PathIsDrive(path))
         {
             return true;
         }

         string[] pathChunks = ChunkPath(path);
         string tableName;
         int rowNumber;

         PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

         if (type == PathType.Table)
         {
            foreach (DatabaseTableInfo ti in GetTables())
            {
                if (string.Equals(ti.Name, tableName, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            } // foreach (DatabaseTableInfo...
         } // if (pathChunks...

         return false;
      }
```

### <a name="implementing-getchildname"></a>GetChildName の実装

このメソッドは、指定されたパスにある子項目の name プロパティを取得します。 [Getchildname *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetChildName) メソッド。 指定したパスにある項目がコンテナーの子でない場合、このメソッドはパスを返す必要があります。

```csharp
protected override string GetChildName(string path)
       {
           if (PathIsDrive(path))
           {
               return path;
           }

           string tableName;
           int rowNumber;

           PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

           if (type == PathType.Table)
           {
               return tableName;
           }
           else if (type == PathType.Row)
           {
               return rowNumber.ToString(CultureInfo.CurrentCulture);
           }
           else
           {
               ThrowTerminatingInvalidPathException(path);
           }

           return null;
       }
```

### <a name="implementing-getparentpath"></a>GetParentPath の実装

[Getparentpath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.GetParentPath)メソッドは、指定したパスにある項目の親のパスを取得します。このメソッドは、 指定されたパスにある項目がデータストアのルートである (親がない) 場合、このメソッドはルートパスを返す必要があります。

```csharp
protected override string GetParentPath(string path, string root)
       {
           // If root is specified then the path has to contain
           // the root. If not nothing should be returned
           if (!String.IsNullOrEmpty(root))
           {
               if (!path.Contains(root))
               {
                   return null;
               }
           }

           return path.Substring(0, path.LastIndexOf(pathSeparator, StringComparison.OrdinalIgnoreCase));
       }
```

### <a name="implementing-makepath"></a>MakePath の実装

指定された親パスと指定された子パスを結合してプロバイダー内部パスを作成 [します (](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MakePath) プロバイダーがサポートできるパスの種類の詳細については、「 [Windows PowerShell プロバイダーの概要](./windows-powershell-provider-overview.md)」を参照してください)。 PowerShell エンジンは、ユーザーが次の [コマンドレットを](/dotnet/api/Microsoft.PowerShell.Commands.joinpathcommand) 呼び出すと、このメソッドを呼び出します。

```csharp
protected override string MakePath(string parent, string child)
       {
           string result;

           string normalParent = NormalizePath(parent);
           normalParent = RemoveDriveFromPath(normalParent);
           string normalChild = NormalizePath(child);
           normalChild = RemoveDriveFromPath(normalChild);

           if (String.IsNullOrEmpty(normalParent) && String.IsNullOrEmpty(normalChild))
           {
               result = String.Empty;
           }
           else if (String.IsNullOrEmpty(normalParent) && !String.IsNullOrEmpty(normalChild))
           {
               result = normalChild;
           }
           else if (!String.IsNullOrEmpty(normalParent) && String.IsNullOrEmpty(normalChild))
           {
               if (normalParent.EndsWith(pathSeparator, StringComparison.OrdinalIgnoreCase))
               {
                   result = normalParent;
               }
               else
               {
                   result = normalParent + pathSeparator;
               }
           } // else if (!String...
           else
           {
               if (!normalParent.Equals(String.Empty) &&
                   !normalParent.EndsWith(pathSeparator, StringComparison.OrdinalIgnoreCase))
               {
                   result = normalParent + pathSeparator;
               }
               else
               {
                   result = normalParent;
               }

               if (normalChild.StartsWith(pathSeparator, StringComparison.OrdinalIgnoreCase))
               {
                   result += normalChild.Substring(1);
               }
               else
               {
                   result += normalChild;
               }
           } // else

           return result;
       }
```

### <a name="implementing-normalizerelativepath"></a>NormalizeRelativePath の実装

[Normalizerelativepath *](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.NormalizeRelativePath)メソッドは、パラメーターとパラメーターを受け取り、パラメーター `path` `basepath` と等価で `path` あり、パラメーターに対して相対的な正規化されたパスを返し `basepath` ます。

```csharp
protected override string NormalizeRelativePath(string path,
                                                            string basepath)
       {
           // Normalize the paths first
           string normalPath = NormalizePath(path);
           normalPath = RemoveDriveFromPath(normalPath);
           string normalBasePath = NormalizePath(basepath);
           normalBasePath = RemoveDriveFromPath(normalBasePath);

           if (String.IsNullOrEmpty(normalBasePath))
           {
               return normalPath;
           }
           else
           {
               if (!normalPath.Contains(normalBasePath))
               {
                   return null;
               }

               return normalPath.Substring(normalBasePath.Length + pathSeparator.Length);
           }
       }
```

### <a name="implementing-moveitem"></a>MoveItem の実装

この [メソッドは](/dotnet/api/System.Management.Automation.Provider.NavigationCmdletProvider.MoveItem) 、指定されたパスから指定されたコピー先のパスに項目を移動します。 PowerShell エンジンは、ユーザーが [コマンド](/dotnet/api/Microsoft.PowerShell.Commands.moveitemcommand) レットを呼び出したときに、このメソッドを呼び出します。

```csharp
protected override void MoveItem(string path, string destination)
       {
           // Get type, table name and rowNumber from the path
           string tableName, destTableName;
           int rowNumber, destRowNumber;

           PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

           PathType destType = GetNamesFromPath(destination, out destTableName,
                                    out destRowNumber);

           if (type == PathType.Invalid)
           {
               ThrowTerminatingInvalidPathException(path);
           }

           if (destType == PathType.Invalid)
           {
               ThrowTerminatingInvalidPathException(destination);
           }

           if (type == PathType.Table)
           {
               ArgumentException e = new ArgumentException("Move not supported for tables");

               WriteError(new ErrorRecord(e, "MoveNotSupported",
                   ErrorCategory.InvalidArgument, path));

               throw e;
           }
           else
           {
               OdbcDataAdapter da = GetAdapterForTable(tableName);
               if (da == null)
               {
                   return;
               }

               DataSet ds = GetDataSetForTable(da, tableName);
               DataTable table = GetDataTable(ds, tableName);

               OdbcDataAdapter dda = GetAdapterForTable(destTableName);
               if (dda == null)
               {
                   return;
               }

               DataSet dds = GetDataSetForTable(dda, destTableName);
               DataTable destTable = GetDataTable(dds, destTableName);
               DataRow row = table.Rows[rowNumber];

               if (destType == PathType.Table)
               {
                   DataRow destRow = destTable.NewRow();

                   destRow.ItemArray = row.ItemArray;
               }
               else
               {
                   DataRow destRow = destTable.Rows[destRowNumber];

                   destRow.ItemArray = row.ItemArray;
               }

               // Update the changes
               if (ShouldProcess(path, "MoveItem"))
               {
                   WriteItemObject(row, path, false);
                   dda.Update(dds, destTableName);
               }
           }
       }
```

## <a name="see-also"></a>参照

[コンテナー プロバイダーを記述する](./writing-a-container-provider.md)

[Windows PowerShell プロバイダーの概要](./windows-powershell-provider-overview.md)
