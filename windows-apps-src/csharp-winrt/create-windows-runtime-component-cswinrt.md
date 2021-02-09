---
description: '使用 c #/WinRT 创作 Windows 运行时组件，并从本机应用程序使用它'
title: 创建一个 C# /WinRT 组件并从 C++/WinRT 使用它
ms.date: 01/28/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9f5157f97163a72ccce1ce9fc3f560fb4e16b1df
ms.sourcegitcommit: 61a874d00991f7ca06466a99a557ef0777bd0f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/09/2021
ms.locfileid: "99989642"
---
# <a name="walkthrough-create-a-cwinrt-component-and-consume-it-from-cwinrt"></a>演练：创建 c #/WinRT 组件并从 c + +/WinRT 使用它

> [!NOTE]
> 本文中所述的 c #/WinRT 创作支持目前为预览版，如 c #/WinRT 版本 1.1.2-210208.6。 在此版本中，它仅用于提前反馈和评估。

C #/WinRT 使 .NET 5 开发人员能够使用类库项目在 c # 中创作自己的 Windows 运行时组件。 在本机桌面应用程序中，创作的组件可以作为包引用使用，也可以作为项目引用进行一些修改。

本演练演示如何使用 c #/WinRT 创建简单的 Windows 运行时组件，如何将组件分发为 NuGet 包，以及如何从 c + +/WinRT 控制台应用程序使用该组件。 可在 Github 上的 [此处](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)找到本演练的示例代码。

创作运行时组件时，请遵循本文中所述的指导原则和类型限制 [。](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 在内部，组件中的 Windows 运行时类型可以使用 UWP 应用程序中允许的任何 .NET 功能。 有关详细信息，请参阅适用 [于 UWP 应用的 .net](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)。 在外部，类型的成员只能为其参数和返回值公开 Windows 运行时类型。

> [!NOTE]
> 某些 Windows 运行时类型 [映射到 .net 类型](../winrt-components/net-framework-mappings-of-windows-runtime-types.md#uwp-types-that-map-to-net-types-with-a-different-name-andor-namespace)。 可以在 Windows 运行时组件的公共接口中使用这些 .NET 类型，并且会向组件的用户显示为相应 Windows 运行时类型。

## <a name="prerequisites"></a>必备条件

本演练需要以下工具和组件：

- Visual Studio 2019
- .NET 5.0 SDK
- C + + [/WINRT VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) For c + +/WinRT 项目模板

## <a name="create-a-simple-windows-runtime-component-using-cwinrt"></a>使用 c #/WinRT 创建简单的 Windows 运行时组件

首先，在 Visual Studio 2019 中创建一个新项目。 选择类库 **( ".Net Core)** " 项目模板，并将其命名为 **AuthoringDemo**。 你将需要对项目进行以下添加和修改：

1. 更新 `TargetFramework` **AuthoringDemo** 文件中的，并将以下元素添加到 `PropertyGroup` ：

    ```xml
    <PropertyGroup>
        <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
        <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

    对于 `TargetFramework` 元素，你可以使用以下 [目标框架名字对象](/windows/apps/desktop/modernize/desktop-to-uwp-enhance#net-5-use-the-target-framework-moniker-option)之一。
    - **net 5.0-windows 10.0.17763。0**
    - **net 5.0-windows 10.0.18362。0**
    - **net 5.0-windows 10.0.19041。0**

2. 安装最新版本的 [c #/WinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/1.1.2-prerelease.210208.6)。

    a. 在解决方案资源管理器中，右键单击项目节点，然后选择 " **管理 NuGet 包**"。

    b. 搜索 **CsWinRT** NuGet 包，并安装最新版本。 本演练使用 c #/WinRT 版本 1.1.2. 210208.6。

3. 添加一个新 `PropertyGroup` 元素，用于设置多个 c #/WinRT 属性。

    ```xml
    <PropertyGroup>   
        <CsWinRTComponent>true</CsWinRTComponent>
        <CsWinRTWindowsMetadata>10.0.19041.0</CsWinRTWindowsMetadata>
    </PropertyGroup>
      ```

      下面是此示例中的属性的一些详细信息。 有关 c #/WinRT 项目属性的完整列表，请参阅 [c #/WinRT NuGet 文档。](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)

    - `CsWinRTComponent`属性指定项目是 Windows 运行时组件，以便为组件生成 WinMD 文件。
    - `CsWinRTWindowsMetadata`属性提供 Windows 元数据的源。 这是版本1.1.1 所必需的。

4. 您可以使用库 **()** 类文件来编写运行时类。 右键单击 **Class1.cs** 文件，并将其重命名为 **Example.cs**。 将以下代码添加到此文件，这会将公共属性和方法添加到运行时类。 请记得标记要在运行 **时组件中公开的所有** 类。

    ```csharp
    namespace AuthoringDemo
    {
        public sealed class Example
        {
            public int SampleProperty { get; set; }

            public static string SayHello()
            {
                return "Hello from your C# WinRT component";
            }
        }
    }
    ```

5. 现在可以生成项目以生成组件的元数据文件。 在 **解决方案资源管理器** 中右键单击该项目，然后单击 " **生成**"。 你将在 "**生成的文件**" 文件夹下的 **解决方案资源管理器** 中以及在生成输出文件夹中看到生成的 **AuthoringDemo** 文件。

## <a name="generate-a-nuget-package-for-the-component"></a>为组件生成 NuGet 包

接下来，为组件生成 NuGet 包。 生成包时，c #/WinRT 会将组件和托管包中的程序集配置为使用本机应用程序。

可以通过多种方式生成 NuGet 包：

* 如果要在每次生成项目时都生成 NuGet 包，请将以下属性添加到 **AuthoringDemo** 项目文件，然后重新生成项目。

    ```xml
    <PropertyGroup>
        <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>
    ```

* 或者，你可以通过右键单击 **解决方案资源管理器** 中的 **AuthoringDemo** 项目，然后选择 "**打包**" 来生成 NuGet 包。

生成包时，" **生成** " 窗口应指示 `AuthoringDemo.1.0.0.nupkg` 已成功创建 NuGet 包。

## <a name="consume-the-component-in-cwinrt"></a>使用 c + +/WinRT 中的组件

C #/WinRT 创作 Windows 运行时组件可在本机应用程序中使用，只需进行一些修改。 以下步骤演示如何在本机控制台应用程序中调用上面创作的组件。 

1. 向解决方案中添加新的 **c + +/WinRT 控制台应用程序** 项目。 请注意，如果选择此项目，此项目也可以是不同解决方案的一部分。

    a. 在 **解决方案资源管理器** 中，右键单击解决方案节点，然后单击 "**添加**  ->  **新项目**"。

    b. 在 " **添加新项目" 对话框** 中，搜索 " **c + +/WinRT 控制台应用程序** " 项目模板。 选择模板，然后单击 " **下一步**"。

    c. 将新项目命名为 " **CppConsoleApp** "，然后单击 " **创建**"。

2. 将对 AuthoringDemo 组件的引用添加为 NuGet 包或项目引用。

    - **选项 1 (包引用)**：  

        a. 右键单击 " **CppConsoleApp** " 项目，然后选择 " **管理 NuGet 包**"。 可能需要配置包源以添加对 AuthoringDemo NuGet 包的引用。 为此，请单击 "NuGet 包管理器" 中的 " **设置** " 图标，并将包源添加到相应路径。

        ![NuGet 设置](images/nuget-sources-settings.png)

        b. 配置包源后，搜索 **AuthoringDemo** 包，并单击 " **安装**"。

        ![安装 NuGet 包](images/install-authoring-nuget.png)

    - **选项 2 (项目引用)**：
        
        a. 右键单击 " **CppConsoleApp** " 项目，然后选择 "**添加**  ->  **引用**"。 在 " **项目** " 节点下，添加对 **AuthoringDemo** 项目的引用。 从此预览版开始，还需要从 "**浏览**" 节点添加对 **AuthoringDemo** 的文件引用。 可以在 **AuthoringDemo** 项目的输出目录中找到生成的 winmd 文件。

        b. 对于此预览版，还需要将以下属性组添加到 **CppConsoleApp. .vcxproj**。 若要编辑本机应用程序项目文件，请首先右键单击 " **CppConsoleApp** " 项目节点，然后选择 " **卸载项目**"。

        ```xml
        <PropertyGroup>
            <TargetFrameworkVersion>net5.0</TargetFrameworkVersion>
            <TargetFramework>native</TargetFramework>
            <TargetRuntime>Native</TargetRuntime>
        </PropertyGroup>
        ```

3. 若要帮助承载组件，需要在文件和清单文件上添加 runtimeconfig.js。 有关托管组件托管的详细信息，请参阅 [这些托管文档](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)。

    a. 若要将 runtimeconfig.js添加到文件，请右键单击该项目，然后选择 " **添加-> 新项**"。 搜索 " **文本文件** 模板" 并将其命名 **WinRT.Host.runtimeconfig.js**。 粘贴以下内容：

    ```json
    {
        "runtimeOptions": {
            "tfm": "net5.0",
            "rollForward": "LatestMinor",
            "framework": {
                "name": "Microsoft.NETCore.App",
                "version": "5.0.0"
            }
        }
    }
    ```

    请注意，对于该 `tfm` 条目，可以使用 DOTNET_ROOT 环境变量来引用自定义的自包含 .net 5 安装。

    b. 若要添加清单文件，请再次右键单击该项目，然后选择 " **添加-> 新项**"。 搜索 "**文本文件**" 模板，将其命名 **CppConsoleApp.exe。** 粘贴以下内容：

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
        <assemblyIdentity version="1.0.0.0" name="CppConsoleApp"/>
        <file name="WinRT.Host.dll">
            <activatableClass
                name="AuthoringDemo.Example"
                threadingModel="both"
                xmlns="urn:schemas-microsoft-com:winrt.v1" />
        </file>
    </assembly>
    ```

    非打包应用程序需要清单文件。 在此文件中，指定使用可激活的类注册项的运行时类，如上所示。

4. 部署项目时，修改项目以在输出中包括 runtimeconfig.js和清单文件。 对于和 **CppConsoleApp.exe 清单** 文件的 **WinRT.Host.runtimeconfig.js** ，请单击 **解决方案资源管理器** 中的文件，并将 **Content** 属性设置为 **True**。 下面是此示例的示例。

    ![部署内容](images/deploy-content.png)

5. 在项目的标头文件下打开 **pch** ，并添加以下代码行以包含组件。

    ```cpp
    #include <winrt/AuthoringDemo.h>
    ```

6. 在项目的源文件下打开 **main .cpp** ，并将其替换为以下内容。

    ```cpp
    #include "pch.h"
    #include "iostream"

    using namespace winrt;
    using namespace Windows::Foundation;

    int main()
    {
        init_apartment();

        AuthoringDemo::Example ex;
        ex.SampleProperty(42);
        std::wcout << ex.SampleProperty() << std::endl;
        std::wcout << ex.SayHello().c_str() << std::endl;
    }
    ```

7. 生成并运行 **CppConsoleApp** 项目。 现在，应会看到以下输出。

    ![C + +/WinRT 控制台输出](images/consume-component-output.png)

## <a name="related-topics"></a>相关主题

- [代码示例](https://github.com/microsoft/CsWinRT/tree/master/src/Samples/AuthoringDemo)
- [创作组件](https://github.com/microsoft/CsWinRT/blob/master/docs/authoring.md)
- [托管组件托管](https://github.com/microsoft/CsWinRT/blob/master/docs/hosting.md)
