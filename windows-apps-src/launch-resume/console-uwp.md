---
title: 创建通用 Windows 平台控制台应用
description: 本主题介绍如何编写在控制台窗口中运行的 UWP 应用。
keywords: 控制台 UWP
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: cab52a84e631404fc4cbd682d6cedef7e799d387
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162661"
---
# <a name="create-a-universal-windows-platform-console-app"></a>创建通用 Windows 平台控制台应用

本主题介绍如何创建 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 或 c + +/cx 通用 WINDOWS 平台 (UWP) 控制台应用。

从 Windows 10 版本1803开始，可以编写在控制台窗口中运行的 c + +/WinRT 或 c + +/CX UWP 控制台应用，如 DOS 或 PowerShell 控制台窗口。 控制台应用使用控制台窗口进行输入和输出，并可使用诸如**printf**和**Getchar**的[通用 C 运行时](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference)函数。 UWP 控制台应用可以发布到 Microsoft Store。 它们在应用列表中有对应条目，并有可以固定到“开始”菜单的主要磁贴。 UWP 控制台应用可以从 "开始" 菜单启动，但你通常会从命令行启动它们。

若要查看其中一个操作，以下是有关创建 UWP 控制台应用的视频。

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>使用 UWP 控制台应用模板 

若要创建 UWP 控制台应用，请首先安装 [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal)中提供的**控制台应用（通用）项目模板**。 然后，在 "**新项目**" 下提供已安装的模板，  >  **Installed**  >  **Other Languages**  >  **Visual C++**  >  **Windows 通用**作为**控制台应用 c + +/WinRT (通用 windows) **和**控制台应用 c + +/cx (通用 windows) **。

## <a name="add-your-code-to-main"></a>将代码添加到 main()

模板添加了 **Program.cpp**，其中包含 `main()` 函数。 这是 UWP 控制台应用中执行开始的位置。 使用 `__argc` 和 `__argv` 形式参数访问命令行实际参数。 控制从 `main()` 返回时，UWP 控制台应用会退出。

下面的 **程序 .cpp** 示例由 **控制台应用 c + +/WinRT** 模板添加：

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>UWP 控制台应用行为

UWP 控制台应用可以从其运行的目录及其下方目录访问文件系统。 这是可能的，因为该模板向应用的 Package.appxmanifest 文件中添加了 [AppExecutionAlias](/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) 扩展。 此扩展还使用户可以从控制台窗口中键入别名，以启动该应用。 该应用不需要在系统路径中启动。

你还可以如[文件访问权限](../files/file-access-permissions.md)中所述，通过添加受限功能 `broadFileSystemAccess`，向 UWP 控制台应用提供对文件系统的广泛访问权限。 此功能适用于 [**Windows.Storage**](/uwp/api/Windows.Storage) 命名空间中的 API。

可以一次运行多个 UWP 控制台应用的实例，因为该模板会向应用的 Package.appxmanifest 文件添加 [SupportsMultipleInstances](multi-instance-uwp.md) 功能。

该模板还会向 Package.appxmanifest 文件添加 `Subsystem="console"` 功能，文件表示此 UWP 应用是控制台应用。 请注意 `desktop4` 和 iot2`namespace` 前缀。 仅桌面和物联网 (IoT) 项目支持 UWP 控制台应用。

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>UWP 控制台应用的其他注意事项

- 只有 c + +/WinRT 和 c + +/CX UWP 应用可以是控制台应用。
- UWP 控制台应用必须针对桌面或 IoT 项目类型。
- UWP 控制台应用不能创建窗口。 它们不能使用 MessageBox ( # A1 或 Location ( # A3，也不能根据任何原因（例如用户同意提示）创建窗口。
- UWP 控制台应用可能不使用后台任务，也不会作为后台任务运行。
- 除[命令行激活](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97)外，UWP 控制台应用不支持激活合约，包括文件关联、协议关联等。
- 尽管 UWP 控制台应用支持多实例，但它们不支持[多实例重定向](multi-instance-uwp.md)
- 有关适用于 UWP 应用的 Win32 API 的列表，请参阅[适用于 UWP 应用的 Win32 和 COM API](/uwp/win32-and-com/win32-and-com-for-uwp-apps)