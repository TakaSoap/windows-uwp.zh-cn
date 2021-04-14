---
description: 使用 Windows 运行时 API 为 Windows 10 用户增强桌面应用程序。
title: 在桌面应用中调用 Windows 运行时 API
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 754cc0d4d230172ec8fc6b1ee7c253e10404c370
ms.sourcegitcommit: 86630e2163a87f6d6e02db9598a3e43f2d227cb6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106272987"
---
# <a name="call-windows-runtime-apis-in-desktop-apps"></a>在桌面应用中调用 Windows 运行时 API

你可以使用通用 Windows 平台 (UWP) API 向桌面应用添加令 Windows 10 用户感到满意的新式体验。

首先，使用必需的引用设置项目。 然后，从代码中调用 Windows 运行时 API 以为桌面应用带来 Windows 10 体验。 你可以为 Windows 10 用户单独生成，也可以不考虑用户所运行的 Windows 版本而向所有用户分发相同的二进制文件。

某些 Windows 运行时 API 仅在具有[程序包标识符](modernize-packaged-apps.md)的桌面应用中受支持。 有关详细信息，请参阅[可用的 Windows 运行时 API](desktop-to-uwp-supported-api.md)。

## <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>修改 .NET 项目以使用 Windows 运行时 API

有几个用于 .NET 项目的选项：

* 从 .NET 5 开始，可以在项目文件中添加目标框架名字对象 (TFM)，用于访问 WinRT API。 此选项支持面向 Windows 10 1809 版或更高版本的项目。
* 对于早期版本的 .NET，可以安装 `Microsoft.Windows.SDK.Contracts` NuGet 包，以便将所有必要的引用添加到项目中。 此选项支持面向 Windows 10 1803 版或更高版本的项目。
* 如果你的项目同时面向 .NET 5（或更高版本）和早期版本的 .NET 等多个目标，可将项目文件配置为同时使用这两个选项。

### <a name="net-5-use-the-target-framework-moniker-option"></a>.NET 5：使用“目标框架名字对象”选项

只可在使用 .NET 5（或更高版本）和面向 Windows 10 版本 1809 或更高版本操作系统的项目中使用此选项。 有关此方案的更多背景信息，请参阅[此博客文章](https://blogs.windows.com/windowsdeveloper/2020/09/03/calling-windows-apis-in-net5/)。

1. 在 Visual Studio 中打开项目后，在“解决方案资源管理器”中右键单击该项目，然后选择“编辑项目文件” 。 项目文件的呈现效果与此类似。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>net5.0</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. 将 TargetFramework 元素的值替换为以下字符串之一：

    * **net5.0-windows10.0.17763.0**：如果应用面向 Windows 10 1809 版，请使用此值。
    * **net5.0-windows10.0.18362.0**：如果应用面向 Windows 10 1903 版，请使用此值。
    * **net5.0-windows10.0.19041.0**：如果应用面向 Windows 10 2004 版，请使用此值。

    例如，以下元素适用于面向 Windows 10 2004 版的项目。

    ```xml
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    ```

3. 保存更改并关闭项目文件。

### <a name="earlier-versions-of-net-install-the-microsoftwindowssdkcontracts-nuget-package"></a>早期版本的 .NET：安装 Microsoft.Windows.SDK.Contracts NuGet 包

如果应用使用 .NET Core 3.x 或 .NET Framework，请使用此选项。 此选项支持面向 Windows 10 1803 版或更高版本操作系统的项目。

1. 确保已启用[包引用](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，单击“工具”->“NuGet 程序包管理器”->“程序包管理器设置”。
    2. 确保为“默认包管理格式”选择 PackageReference 。

2. 在 Visual Studio 中打开项目后，在“解决方案资源管理器”中右键单击该项目，然后选择“管理 NuGet 包” 。

3. 在“NuGet 包管理器”窗口中，选择“浏览”选项卡，然后搜索 `Microsoft.Windows.SDK.Contracts` 。

4. 找到 `Microsoft.Windows.SDK.Contracts` 包后，在“NuGet 包管理器”窗口的右窗格中，根据要面向的 Windows 10 版本，选择要安装的包的“版本” ：

    * **10.0.19041.xxxx**：对于 Windows 10 版本 2004，请选择此版本。
    * **10.0.18362.xxxx**：对于 Windows 10 版本 1903，请选择此版本。
    * **10.0.17763.xxxx**：对于 Windows 10 版本 1809，请选择此版本。
    * **10.0.17134.xxxx**：对于 Windows 10 版本 1803，请选择此版本。

5. 单击“安装” 。

### <a name="configure-projects-that-multi-target-different-versions-of-net"></a>配置面向多个不同 .NET 版本的项目

如果你的项目同时面向 .NET 5（或更高版本）和早期版本（包括 .NET Core 3.x 和 .NET Framework），可配置项目文件，让其使用目标框架名字对象自动拉取 .NET 5 的 WinRT API 参考，并为早期版本使用 `Microsoft.Windows.SDK.Contracts` NuGet 包。

1. 在 Visual Studio 中打开项目后，在“解决方案资源管理器”中右键单击该项目，然后选择“编辑项目文件” 。 下面的示例展示使用 .NET Core 3.1 的应用的项目文件。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFramework>netcoreapp3.1</TargetFramework>
        <UseWindowsForms>true</UseWindowsForms>
      </PropertyGroup>
    </Project>
    ```

2. 将文件中的 TargetFramework 元素替换为 TargetFrameworks 元素（请注意复数形式）。 在此元素中，为要面向的所有 .NET 版本（以分号分隔）指定目标框架名字对象。 

    * 对于 .NET 5 或更高版本，请使用以下目标框架名字对象之一：
        * **net5.0-windows10.0.17763.0**：如果应用面向 Windows 10 1809 版，请使用此值。
        * **net5.0-windows10.0.18362.0**：如果应用面向 Windows 10 1903 版，请使用此值。
        * **net5.0-windows10.0.19041.0**：如果应用面向 Windows 10 2004 版，请使用此值。
    * 对于 .NET Core 3.x，请使用 netcoreapp 3.0 或 netcoreapp 3.1。
    * 对于 .NET Framework，请使用 net46。

    下面的示例演示如何同时面向 .NET Core 3.1 和 .NET 5（适用于 Windows 10 2004 版）。

    ```xml
    <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
    ```

3. 在 PropertyGroup 元素后面，添加一个 PackageReference 元素，它包含一个条件语句，该语句将为你的应用面向的任何 .NET Core 3.x 版本或 .NET Framework 安装 NuGet 包 `Microsoft.Windows.SDK.Contracts` 。 PackageReference 元素必须是 ItemGroup 元素的子元素。 下面的示例演示如何面向 .NET Core 3.1 执行此操作。

    ```xml
    <ItemGroup>
      <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                        Include="Microsoft.Windows.SDK.Contracts"
                        Version="10.0.19041.0" />
    </ItemGroup>
    ```

    完成后，项目文件的呈现效果与此类似。

    ```xml
    <Project Sdk="Microsoft.NET.Sdk.WindowsDesktop">
      <PropertyGroup>
        <OutputType>WinExe</OutputType>
        <TargetFrameworks>netcoreapp3.1;net5.0-windows10.0.19041.0</TargetFrameworks>
        <UseWPF>true</UseWPF>
      </PropertyGroup>
      <ItemGroup>
        <PackageReference Condition="'$(TargetFramework)' == 'netcoreapp3.1'"
                         Include="Microsoft.Windows.SDK.Contracts"
                         Version="10.0.19041.0" />
      </ItemGroup>
    </Project>
    ```

4. 保存更改并关闭项目文件。

## <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>修改 C++ Win32 项目以使用 Windows 运行时 API

通过 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/) 来使用 Windows 运行时 API。 C++/WinRT 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。

若要为 C++/WinRT 配置项目，请执行以下操作：

* 对于新项目，你可以安装 [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)，并使用该扩展中包含的其中一个 C++/WinRT 项目模板。
* 对于现有项目，可以在项目中安装 [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) NuGet 包。

有关这些选项的更多详细信息，请参阅[本文](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

## <a name="add-windows-10-experiences"></a>添加 Windows 10 体验

现在一切已准备就绪，可以添加用户在 Windows 10 上运行你的应用程序时可享受的新式体验。 使用此设计流。

:white_check_mark:**首先，确定要添加的体验**

有很多选择。 例如，可通过使用[盈利 API](/windows/uwp/monetize) 来简化你的采购订单流，或在要分享有趣的内容时（例如其他用户发布了新图片）[吸引用户对应用程序的注意](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)。

![Toast 通知](images/desktop-to-uwp/toast.png)

即使用户忽略或关闭你的消息，他们仍可在操作中心中再次看到该消息，然后单击该消息打开你的应用。 这可以加强用户与应用程序的互动，并使你的应用程序看似已与操作系统深度集成。 稍后，我们将在本文中向你演示用于实现该体验的代码。

请访问 [UWP 文档](/windows/uwp/get-started/)以获取更多创意。

:white_check_mark:**决定是增强还是扩展**

你经常会听到我们使用术语“增强”和“扩展”，因此我们需要花些时间来说明一下这两个术语的确切含义 。

我们使用术语“增强”来描述可以直接从桌面应用对其进行调用的 Windows 运行时 API（无论你是否选择将应用程序打包到 MSIX 包中）。 当你选择 Windows 10 体验后，请确定创建它所需的 API，然后查看该 API 是否出现在此[列表](desktop-to-uwp-supported-api.md)中。 这是你可以直接从桌面应用中调用的 API 的列表。 如果你的 API 未出现在此列表中，那是因为与该 API 关联的功能只在 UWP 进程内运行。 通常情况下，其中包括呈现 UWP XAML（例如 UWP 地图控件或 Windows Hello 安全提示）的 API。

> [!NOTE]
> 尽管通常不能直接从桌面调用呈现 UWP XAML 的 API，但你可以使用其他方法。 如果要托管 UWP XAML 控件或其他自定义视觉体验，则可以使用 [XAML 岛](xaml-islands.md)（从 Windows 10 版本 1903 开始）和[可视化层](visual-layer-in-desktop-apps.md)（从 Windows 10 版本 1803 开始）。 可以在打包或未打包的桌面应用中使用这些功能。

如果已选择将桌面应用打包到 MSIX 包中，则另一种选择是通过向解决方案中添加 UWP 项目来“扩展”应用程序。 桌面项目仍是应用程序的入口点，但 UWP 项目使你可以访问[此列表](desktop-to-uwp-supported-api.md)中未显示的所有 API。 桌面应用可以使用应用服务来与 UWP 进程通信，我们可针对如何进行相关设置提供很多指导。 如果你要添加的体验需要 UWP 项目，请参阅[使用 UWP 组件进行扩展](desktop-to-uwp-extend.md)。

:white_check_mark:**引用 API 协定**

如果你可以直接从桌面应用中调用 API，请打开浏览器并搜索该 API 的引用主题。
在 API 的摘要下，你会找到一个描述用于该 API 的 API 协定的表。 下面是该表的一个示例：

![API 协定表](images/desktop-to-uwp/contract-table.png)

如果你有基于 .NET 的桌面应用，请添加对该 API 协定的引用，然后将该文件的 Copy Local 属性设置为 False 。 如果你有一个基于 C++ 的项目，请将包含此协定的文件夹的路径添加到“附加包含目录”中。

:white_check_mark:**调用 API 以添加你的体验**

以下代码用于显示我们之前看到的通知窗口。 此[列表](desktop-to-uwp-supported-api.md)中显示了这些 API，因此你可以将此代码添加到桌面应用中并立即执行此代码。

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```cppwinrt
#include <sstream>
#include <winrt/Windows.Data.Xml.Dom.h>
#include <winrt/Windows.UI.Notifications.h>

using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::System;
using namespace winrt::Windows::UI::Notifications;
using namespace winrt::Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    std::wstring const title = L"featured picture of the day";
    std::wstring const content = L"beautiful scenery";
    std::wstring const image = L"https://picsum.photos/360/180?image=104";
    std::wstring const logo = L"https://picsum.photos/64?image=883";

    std::wostringstream xmlString;
    xmlString << L"<toast><visual><binding template='ToastGeneric'>" <<
        L"<text>" << title << L"</text>" <<
        L"<text>" << content << L"</text>" <<
        L"<image src='" << image << L"'/>" <<
        L"<image src='" << logo << L"'" <<
        L" placement='appLogoOverride' hint-crop='circle'/>" <<
        L"</binding></visual></toast>";

    XmlDocument toastXml;

    toastXml.LoadXml(xmlString.str().c_str());

    ToastNotificationManager::CreateToastNotifier().Show(ToastNotification(toastXml));
}
```

```cppcx
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```

若要了解有关通知的详细信息，请参阅[自适应和交互式 toast 通知](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts)。

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>支持 Windows XP、Windows Vista 和 Windows 7/8 安装库

你可以为适用于 Windows 10 的应用程序增加新式体验，而无需创建新分支和维护不同代码库。

如果要为 Windows 10 用户生成单独的二进制文件，请使用条件编译。 如果你希望生成要部署到所有 Windows 用户的一组二进制文件，请使用运行时检查。

让我们快速查看一下每个选项。

### <a name="conditional-compilation"></a>条件编译

你可以保留一个代码库，并编译一组仅面向 Windows 10 用户的二进制文件。

首先，向项目中添加新的生成配置。

![生成配置](images/desktop-to-uwp/build-config.png)

对于该生成配置，请创建一个常量以标识调用 Windows 运行时 API 的代码。  

对于基于 .NET 的项目，该常量称为“条件编译常量”。

![条件编译常量](images/desktop-to-uwp/compilation-constants.png)

对于基于 C++ 的项目，该常量称为“预处理器定义”。

![预处理器定义常量](images/desktop-to-uwp/pre-processor.png)

在任意 UWP 代码块前添加该常量。

```csharp
[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}
```

```C++
#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif
```

仅当在活动生成配置中定义了该常量时，编译器才会生成该代码。

### <a name="runtime-checks"></a>运行时检查

可以不考虑用户所运行的 Windows 版本而为所有 Windows 用户编译一组二进制文件。 仅当用户在 Windows 10 上以打包的应用程序形式运行应用程序时，应用程序才会调用 Windows 运行时 API。

向代码添加运行时检查最简单的方法是安装以下 Nuget 包：[桌面桥帮助程序](https://www.nuget.org/packages/DesktopBridge.Helpers/)，然后使用 ``IsRunningAsUWP()`` 方法关闭调用 Windows 运行时 API 的所有代码。 有关更多详细信息，请参阅此博客文章：[桌面桥 - 标识应用程序的上下文](/archive/blogs/appconsult/desktop-bridge-identify-the-applications-context)。

## <a name="related-samples"></a>相关示例

* [Hello World 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [辅助磁贴](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Microsoft Store API 示例](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [实现 UWP UpdateTask 的 WinForms 应用程序](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [UWP 示例的桌面应用桥](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>查找问题的答案

有问题？ 请在 Stack Overflow 上向我们提问。 我们的团队会监视这些[标记](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge)。 你还可以在[此处](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D)提问。
