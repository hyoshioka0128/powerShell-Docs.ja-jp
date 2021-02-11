---
ms.date: 09/13/2016
ms.topic: reference
title: Events01 サンプル
description: Events01 サンプル
ms.openlocfilehash: ed8b7903537504609602e27693351847d322f904
ms.sourcegitcommit: ba7315a496986451cfc1296b659d73ea2373d3f0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2020
ms.locfileid: "94390407"
---
# <a name="events01-sample"></a>Events01 サンプル

このサンプルでは、ユーザーが、system.object [によって発生した](/dotnet/api/System.IO.FileSystemWatcher)イベントに登録できるようにするコマンドレットを作成する方法を示します。 このコマンドレットを使用すると、ユーザーは、特定のディレクトリにファイルが作成されたときに実行するアクションを登録できます。 このサンプルは、 [ObjectEventRegistrationBase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase) 基底クラスから派生します。

## <a name="how-to-build-the-sample-by-using-visual-studio"></a>Visual Studio を使用してサンプルをビルドする方法。

1. Windows PowerShell 2.0 SDK がインストールされている状態で、Events01 フォルダーに移動します。 既定の場所は `C:\Program Files (x86)\Microsoft SDKs\Windows\v7.0\Samples\sysmgmt\WindowsPowerShell\csharp\Events01` です。

2. ソリューション (.sln) ファイルのアイコンをダブルクリックします。 これにより、Microsoft Visual Studio でサンプルプロジェクトが開きます。

3. **[ビルド]** メニューで、 **[ソリューションのビルド]** を選択します。 サンプルのライブラリは、既定の `\bin` フォルダーまたはフォルダーに組み込まれ `\bin\debug` ます。

### <a name="how-to-run-the-sample"></a>サンプルを実行する方法

1. 次のモジュールフォルダーを作成します。

    `[user]/documents/windowspowershell/modules/events01`

2. サンプルのライブラリファイルをモジュールフォルダーにコピーします。

3. Windows PowerShell を起動します。

4. 次のコマンドを実行して、Windows PowerShell にコマンドレットを読み込みます。

    ```powershell
    import-module events01
    ```

5. Register-FileSystemEvent コマンドレットを使用して、一時ディレクトリにファイルが作成されたときにメッセージを書き込むアクションを登録します。

    ```powershell
    Register-FileSystemEvent $env:temp Created -filter "*.txt" -action { Write-Host "A file was created in the TEMP directory" }
    ```

6. TEMP ディレクトリの下にファイルを作成し、アクションが実行されることを確認します (メッセージが表示されます)。

これは、次の手順に従って生成される出力サンプルです。

```output
Id              Name            State      HasMoreData     Location             Command
--              ----            -----      -----------     --------             -------
1               26932870-d3b... NotStarted False                                 Write-Host "A f...

```

```powershell
Set-Content $env:temp\test.txt "This is a test file"
```

```output
A file was created in the TEMP directory
```

## <a name="requirements"></a>要件

このサンプルには、Windows PowerShell 2.0 が必要です。

## <a name="demonstrates"></a>対象

このサンプルでは、次のことを示します。

### <a name="how-to-write-a-cmdlet-for-event-registration"></a>イベント登録用のコマンドレットを記述する方法

コマンドレットは、コマンドレットに共通のパラメーターをサポートする[ObjectEventRegistrationBase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase)クラスから派生します。 `Register-*Event` [ObjectEventRegistrationBase](/dotnet/api/Microsoft.PowerShell.Commands.ObjectEventRegistrationBase)から派生したコマンドレットでは、特定のパラメーターを定義し、 `GetSourceObject` および抽象メソッドをオーバーライドするだけで済みます。 `GetSourceObjectEventName`

## <a name="example"></a>例

このサンプルでは、system.object [によって](/dotnet/api/System.IO.FileSystemWatcher)生成されるイベントに登録する方法を示します。

```csharp
namespace Sample
{
    using System;
    using System.IO;
    using System.Management.Automation;
    using System.Management.Automation.Runspaces;
    using Microsoft.PowerShell.Commands;

    [Cmdlet(VerbsLifecycle.Register, "FileSystemEvent")]
    public class RegisterObjectEventCommand : ObjectEventRegistrationBase
    {
        /// <summary>The FileSystemWatcher that exposes the events.</summary>
        private FileSystemWatcher fileSystemWatcher = new FileSystemWatcher();

        /// <summary>Name of the event to which the cmdlet registers.</summary>
        private string eventName = null;

        /// <summary>
        /// Gets or sets the path that will be monitored by the FileSystemWatcher.
        /// </summary>
        [Parameter(Mandatory = true, Position = 0)]
        public string Path
        {
            get
            {
                return this.fileSystemWatcher.Path;
            }

            set
            {
                this.fileSystemWatcher.Path = value;
            }
        }

        /// <summary>
        /// Gets or sets the name of the event to which the cmdlet registers.
        /// <para>
        /// Currently System.IO.FileSystemWatcher exposes 6 events: Changed, Created,
        /// Deleted, Disposed, Error, and Renamed. Check the documentation of
        /// FileSystemWatcher for details on each event.
        /// </para>
        /// </summary>
        [Parameter(Mandatory = true, Position = 1)]
        public string EventName
        {
            get
            {
                return this.eventName;
            }

            set
            {
                this.eventName = value;
            }
        }

        /// <summary>
        /// Gets or sets the filter that will be user by the FileSystemWatcher.
        /// </summary>
        [Parameter(Mandatory = false)]
        public string Filter
        {
            get
            {
                return this.fileSystemWatcher.Filter;
            }

            set
            {
                this.fileSystemWatcher.Filter = value;
            }
        }

        /// <summary>
        /// Derived classes must implement this method to return the object that generates
        /// the events to be monitored.
        /// </summary>
        /// <returns> This sample returns an instance of System.IO.FileSystemWatcher</returns>
        protected override object GetSourceObject()
        {
            return this.fileSystemWatcher;
        }

        /// <summary>
        /// Derived classes must implement this method to return the name of the event to
        /// be monitored. This event must be exposed by the input object.
        /// </summary>
        /// <returns> This sample returns the event specified by the user with the -EventName parameter.</returns>
        protected override string GetSourceObjectEventName()
        {
            return this.eventName;
        }
    }
}
```

## <a name="see-also"></a>参照

[Writing a Windows PowerShell Cmdlet (Windows PowerShell コマンドレットの記述)](writing-a-windows-powershell-cmdlet.md)
