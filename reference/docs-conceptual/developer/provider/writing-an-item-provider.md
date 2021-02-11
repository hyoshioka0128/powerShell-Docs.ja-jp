---
ms.date: 09/13/2016
ms.topic: reference
title: アイテム プロバイダーを記述する
description: アイテム プロバイダーを記述する
ms.openlocfilehash: f70c6ee50277988c4e3b7c255dc4548bc30319dd
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "93355206"
---
# <a name="writing-an-item-provider"></a>アイテム プロバイダーを記述する

このトピックでは、データストア内の項目にアクセスして操作する Windows PowerShell プロバイダーのメソッドを実装する方法について説明します。 項目にアクセスできるようにするには、プロバイダーを、 [システムの管理](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) プロバイダークラスから派生させる必要があります。

このトピックの例のプロバイダーでは、データストアとして Access データベースを使用しています。 データベースとの対話に使用されるヘルパーメソッドとクラスはいくつかあります。 ヘルパーメソッドを含む完全なサンプルについては、「 [AccessDBProviderSample03](./accessdbprovidersample03.md) 」を参照してください。

Windows PowerShell プロバイダーの詳細については、「 [Windows Powershell プロバイダーの概要](./windows-powershell-provider-overview.md)」を参照してください。

## <a name="implementing-item-methods"></a>項目メソッドの実装

System.string [クラスは](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) 、データストア内の項目にアクセスして操作するために使用できるいくつかのメソッドを公開します。
これらのメソッドの完全な一覧については、「 [itemの表示プロバイダーのメソッド](/dotnet/api/system.management.automation.provider.itemcmdletprovider#methods)」を参照してください。
この例では、これらの4つのメソッドを実装します。
System.string。指定されたパスの項目[を取得し](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem)ます。...
[Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem) は、指定された項目の値を設定しています。
System.object は、指定されたパスに項目が存在するかどうか[を確認します。](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists)
System.string は、データストア内の場所にマップされているかどうかを確認するためのパス[を確認します。](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.IsValidPath)

> [!NOTE]
> このトピックは、「 [Windows PowerShell プロバイダーのクイックスタート](./windows-powershell-provider-quickstart.md)」の情報に基づいています。 このトピックでは、プロバイダープロジェクトを設定する方法の基本については説明しません。また、ドライブを作成または削除する [Drivecmdletprovider](/dotnet/api/System.Management.Automation.Provider.DriveCmdletProvider) クラスから継承されたメソッドを実装する方法についても説明しません。

### <a name="declaring-the-provider-class"></a>プロバイダークラスの宣言

プロバイダーを宣言して、system.servicemodel [プロバイダー](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider) クラスから派生させ、そのプロバイダーを system.string [属性](/dotnet/api/System.Management.Automation.Provider.CmdletProviderAttribute)で装飾します。このクラスを宣言してください。

```csharp
[CmdletProvider("AccessDB", ProviderCapabilities.None)]

   public class AccessDBProvider : ItemCmdletProvider
   {

  }

```

### <a name="implementing-getitem"></a>GetItem の実装

ユーザーがプロバイダーで[GetItemCommand](/dotnet/api/Microsoft.PowerShell.Commands.getitemcommand)コマンドレットを呼び出したときに、PowerShell エンジンによって呼び出された場合は、" [System.](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.GetItem) ......................................。 メソッドは、指定されたパスにある項目を返します。 Access データベースの例では、メソッドは、アイテムがドライブ自体であるか、データベース内のテーブルであるか、またはデータベース内の行であるかを確認します。 メソッドは、 [System.](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject) ...........................................。

```csharp
protected override void GetItem(string path)
      {
          // check if the path represented is a drive
          if (PathIsDrive(path))
          {
              WriteItemObject(this.PSDriveInfo, path, true);
              return;
          }// if (PathIsDrive...

           // Get table name and row information from the path and do
           // necessary actions
           string tableName;
           int rowNumber;

           PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

           if (type == PathType.Table)
           {
               DatabaseTableInfo table = GetTable(tableName);
               WriteItemObject(table, path, true);
           }
           else if (type == PathType.Row)
           {
               DatabaseRowInfo row = GetRow(tableName, rowNumber);
               WriteItemObject(row, path, false);
           }
           else
           {
               ThrowTerminatingInvalidPathException(path);
           }

       }
```

### <a name="implementing-setitem"></a>SetItem の実装

[Setitem *](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.SetItem)メソッドは、ユーザーが SetItemCommand コマンドレットを呼び出したときに、powershell エンジンによって呼び出されます。このメソッドは、ユーザーが[](/dotnet/api/Microsoft.PowerShell.Commands.setitemcommand)コマンドレットを呼び出したときに呼び出されます。 このメソッドは、指定されたパスにある項目の値を設定します。

Access データベースの例では、項目が行である場合にのみ項目の値を設定することが理にかなっています。そのため、項目が行でない場合、メソッドは [NotSupportedException](/dotnet/api/system.notsupportedexception) をスローします。

```csharp
protected override void SetItem(string path, object values)
       {
           // Get type, table name and row number from the path specified
           string tableName;
           int rowNumber;

           PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

           if (type != PathType.Row)
           {
               WriteError(new ErrorRecord(new NotSupportedException(
                     "SetNotSupported"), "",
                  ErrorCategory.InvalidOperation, path));

               return;
           }

           // Get in-memory representation of table
           OdbcDataAdapter da = GetAdapterForTable(tableName);

           if (da == null)
           {
               return;
           }
           DataSet ds = GetDataSetForTable(da, tableName);
           DataTable table = GetDataTable(ds, tableName);

           if (rowNumber >= table.Rows.Count)
           {
               // The specified row number has to be available. If not
               // NewItem has to be used to add a new row
               throw new ArgumentException("Row specified is not available");
           } // if (rowNum...

           string[] colValues = (values as string).Split(',');

           // set the specified row
           DataRow row = table.Rows[rowNumber];

           for (int i = 0; i < colValues.Length; i++)
           {
               row[i] = colValues[i];
           }

           // Update the table
           if (ShouldProcess(path, "SetItem"))
           {
               da.Update(ds, tableName);
           }

       }
```

### <a name="implementing-itemexists"></a>ItemExists の実装

ユーザーが[Microsoft. PowerShell. TestPathCommand](/dotnet/api/Microsoft.PowerShell.Commands.Testpathcommand)コマンドレットを呼び出したときに、powershell エンジンによって呼び出された場合は、[このメソッドが呼び出されます。](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.ItemExists) メソッドは、指定されたパスに項目があるかどうかを判断します。 項目が存在する場合、メソッドは、その項目を PowerShell エンジンに渡します [。これ](/dotnet/api/System.Management.Automation.Provider.CmdletProvider.WriteItemObject)を行うには、このメソッドを呼び出します。

```csharp
protected override bool ItemExists(string path)
       {
           // check if the path represented is a drive
           if (PathIsDrive(path))
           {
               return true;
           }

           // Obtain type, table name and row number from path
           string tableName;
           int rowNumber;

           PathType type = GetNamesFromPath(path, out tableName, out rowNumber);

           DatabaseTableInfo table = GetTable(tableName);

           if (type == PathType.Table)
           {
               // if specified path represents a table then DatabaseTableInfo
               // object for the same should exist
               if (table != null)
               {
                   return true;
               }
           }
           else if (type == PathType.Row)
           {
               // if specified path represents a row then DatabaseTableInfo should
               // exist for the table and then specified row number must be within
               // the maximum row count in the table
               if (table != null && rowNumber < table.RowCount)
               {
                   return true;
               }
           }

           return false;

       }
```

### <a name="implementing-isvalidpath"></a>IsValidPath の実装

この [メソッドは](/dotnet/api/System.Management.Automation.Provider.ItemCmdletProvider.IsValidPath) 、指定されたパスが現在のプロバイダーに対して構文的に有効かどうかを確認します。 項目がパスに存在するかどうかは確認されません。

```csharp
protected override bool IsValidPath(string path)
       {
           bool result = true;

           // check if the path is null or empty
           if (String.IsNullOrEmpty(path))
           {
               result = false;
           }

           // convert all separators in the path to a uniform one
           path = NormalizePath(path);

           // split the path into individual chunks
           string[] pathChunks = path.Split(pathSeparator.ToCharArray());

           foreach (string pathChunk in pathChunks)
           {
               if (pathChunk.Length == 0)
               {
                   result = false;
               }
           }
           return result;
       }
```

## <a name="next-steps"></a>次のステップ

典型的な現実世界のプロバイダーは、他の項目を含む項目をサポートしたり、ドライブ内のあるパスから別のパスに項目を移動したりすることができます。 コンテナーをサポートするプロバイダーの例については、「 [コンテナープロバイダーの作成](./writing-a-container-provider.md)」を参照してください。 項目の移動をサポートするプロバイダーの例については、「 [ナビゲーションプロバイダーの作成](./writing-a-navigation-provider.md)」を参照してください。

## <a name="see-also"></a>参照

[コンテナー プロバイダーを記述する](./writing-a-container-provider.md)

[ナビゲーション プロバイダーを記述する](./writing-a-navigation-provider.md)

[Windows PowerShell プロバイダーの概要](./windows-powershell-provider-overview.md)
