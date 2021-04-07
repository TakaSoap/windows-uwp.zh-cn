---
title: WinUI 3 Project Reunion 0.5（2021 年 3 月）
description: WinUI 3 Project Reunion 0.5 的概述。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 92437934b7a07c6409d5d44325094f8dd024f443
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730459"
---
# <a name="windows-ui-library-3---project-reunion-05-march-2021"></a>Windows UI 库 3 - Project Reunion 0.5（2021 年 3 月）

Windows UI 库 (WinUI) 3 是用于构建新式 Windows 应用的原生用户体验 (UX) 框架。  它独立于 Windows 操作系统，作为 [Project Reunion](../../project-reunion/index.md) 的一部分提供。  Project Reunion 0.5 版本提供 [Visual Studio 项目模板](https://aka.ms/projectreunion/vsixdownload)，可帮助你开始使用基于 WinUI 3 的用户界面构建应用。

WinUI 3 Project Reunion 0.5 是 WinUI 3 的第一个稳定的支持版本，可用于创建可发布到 Microsoft Store 的生产应用。 此版本包含了稳定性更新和常规改进，使 WinUI 3 能够前向兼容，并可用于生产。


## <a name="install-winui-3---project-reunion-05"></a>安装 WinUI 3 Project Reunion 0.5

此新版 WinUI 3 作为 Project Reunion 0.5 的一部分提供。 若要安装，请参阅：

[Project Reunion 0.5 的安装说明](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)

现在，WinUI 作为 Project Reunion 的一部分提供，你需下载 Project Reunion Visual Studio 扩展 (VSIX) 以开始操作，该扩展中包含一组开发人员工具和组件。 有关 Project Reunion 包的详细信息，请参阅[部署使用 Project Reunion 的应用](../../project-reunion/deploy-apps-that-use-project-reunion.md)。 Project Reunion VSIX 包含你要用于构建 WinUI 3 应用的 [WinUI 项目模板](winui-project-templates-in-visual-studio.md)。 

> [!NOTE]
> 若要查看 WinUI 3 控件和功能的运行情况，可从 GitHub 克隆并生成 WinUI 3 版本的 [XAML 控件库](#winui-3-controls-gallery)。

> [!NOTE]
> 若要使用 WinUI 3 工具（如“实时可视化树”、“热重载”和“实时属性资源管理器”），必须按照[此处说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述，启用 Visual Studio Preview 功能中的 WinUI 3 工具。

设置开发环境后，请参阅 [Visual Studio 中的 WinUI 3 项目模板](winui-project-templates-in-visual-studio.md)，熟悉可用的 Visual Studio 项目和项模板。 

有关开始构建 WinUI 3 应用的详细信息，请参阅以下文章：

- [适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)
- [构建基本 WinUI 3 桌面版应用](desktop-build-basic-winui3-app.md)

除[限制和已知问题](#limitations-and-known-issues)外，使用 WinUI 项目生成应用类似于使用 XAML 和 WinUI 2.x 生成 UWP 应用。 因此，有关 UWP 应用和 Windows SDK 中的 Windows.UI WinRT 命名空间的大多数[指南文档](/windows/uwp/design/)均适用。

WinUI 3 API 参考文档请参阅此处：[WinUI 3 API 参考](/windows/winui/api)

### <a name="webview2"></a>WebView2

要将 WebView2 与此 WinUI 3 版本一起使用，请下载在[此页](https://developer.microsoft.com/microsoft-edge/webview2/)上的 Evergreen Bootstrapper 或 Evergreen Standalone Installer（如果尚未安装 WebView2 运行时）。 

### <a name="windows-community-toolkit"></a>Windows 社区工具包

如果使用的是 Windows 社区工具包，请[下载最新版本](https://aka.ms/wct-winui3)。

### <a name="visual-studio-support"></a>Visual Studio 支持

为了利用已添加到 WinUI 3 中的最新工具功能（例如热重载、实时可视化树和实时属性资源管理器），必须使用最新的 Visual Studio 预览版，并确保启用 Visual Studio 预览功能中的 WinUI 工具，如[此处的说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。 下表显示了 Visual Studio 2019 版与 WinUI 3 Project Reunion 0.5 的兼容性：

| VS 版本  | WinUI 3 - Project Reunion 0.5  |
|---|---|
| 16.8  | 否   |
| 16.9  | 是，但没有热重载、实时可视化树或实时属性资源管理器功能  |
| 16.10 预览版  | 是，具有所有 WinUI 3 工具   |

## <a name="updating-your-existing-winui-3-app"></a>更新现有的 WinUI 3 应用

可更新使用预览版 WinUI 3 的应用，以使用 WinUI 3 的这个新的支持版本。 请根据你的应用类型参阅以下说明。

> [!NOTE] 
> 由于每个应用的单独场景的唯一性，这些说明可能会有问题。 请谨慎地遵循这些说明进行操作，如果发现问题，请[在 GitHub 存储库上报告错误](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)。 

### <a name="updating-a-winui-3-preview-4-app-to-use-winui-3---project-reunion-05"></a>更新 WinUI 3 预览版 4 应用以使用 WinUI 3 Project Reunion 0.5

在开始之前，确保已安装了所有 WinUI 3 Project Reunion 0.5 必备组件，包括 Project Reunion VSIX 和 NuGet 包。 请参阅[此处的安装说明](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。

首先，请务必逐个执行下列步骤： 
- 在 .wapproj 文件中，如果 TargetPlatformMinVersion 低于 10.0.17763.0，请将其更改为 10.0.17763.0 

- 如果应用使用 `Application.Suspending` 事件，请确保删除或更改该行，因为 `Application.Suspending` 不再针对桌面应用进行调用。 有关详细信息，请参阅 [API 参考文档](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true)。

  请注意，C++ 和 C# 应用的默认项目模板包含以下行。 如果代码中仍存在这些行，请确保删除这些行：

    C#：`this.Suspending += OnSuspending;`
  
    C++：`Suspending({ this, &App::OnSuspending });` 

现在，对项目进行一些更改： 
  
1. 在 Visual Studio 中，转到“工具” -> “NuGet 包管理器” -> “包管理器控制台”  。
2. 键入 ```uninstall-package Microsoft.WinUI -ProjectName {yourProject}```
3. 键入 ```install-package Microsoft.ProjectReunion -Version 0.5.0 -ProjectName {yourProjectName}```
4. 在应用程序 (package).wapproj 中进行以下更改：

    添加以下部分：

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]">
        <IncludeAssets>build</IncludeAssets>
      </PackageReference>
    </ItemGroup>
    ```

    然后，删除以下行：

    ```xml
    <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
    ```

    ```xml
    <Import Project="$(AppxTargetsLocation)Microsoft.WinUI.AppX.targets" />
    ```

5. 在项目的 {YourProject}(package)/build/ 文件夹下，删除现有的 `Microsoft.WinUI.AppX.targets` 文件。

### <a name="updating-a-winui-3---project-reunion-05-preview-app-to-use-winui-3---project-reunion-05-stable"></a>更新 WinUI 3 Project Reunion 0.5 预览版应用以使用 WinUI 3 Project Reunion 0.5（稳定版）

在开始之前，确保已安装了所有 WinUI 3 Project Reunion 0.5 必备组件，包括 Project Reunion VSIX 和 NuGet 包。 请参阅[此处的安装说明](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。

首先，请务必逐个执行下列步骤：
- 在 .wapproj 文件中，如果 TargetPlatformMinVersion 低于 10.0.17763.0，请将其更改为 10.0.17763.0 

- 如果应用使用 `Application.Suspending` 事件，请确保删除或更改该行，因为 `Application.Suspending` 不再针对桌面应用进行调用。 有关详细信息，请参阅 [API 参考文档](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true)。

  请注意，C++ 和 C# 应用的默认项目模板包含以下行。 如果代码中仍存在这些行，请确保删除这些行：

    C#：`this.Suspending += OnSuspending;`
  
    C++：`Suspending({ this, &App::OnSuspending });` 

现在，对项目进行一些更改： 

1. 在 Visual Studio 中，转到“工具” -> “NuGet 包管理器” -> “包管理器控制台”  。
2. 键入 ```uninstall-package Microsoft.ProjectReunion -ProjectName {yourProject}```
3. 键入 ```uninstall-package Microsoft.ProjectReunion.Foundation -ProjectName {yourProject}```
4. 键入 ```uninstall-package Microsoft.ProjectReunion.WinUI -ProjectName {yourProject}```
5. 键入 ```install-package Microsoft.ProjectReunion -Version 0.5.0 -ProjectName {yourProjectName}```
6. 在应用程序 (package).wapproj 中进行以下更改：
  
    添加以下部分：

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0]">
        <IncludeAssets>build</IncludeAssets>
      </PackageReference>
    </ItemGroup>
    ```
    然后，删除以下行（如果存在）：
    ```xml
    <AppxTargetsLocation Condition="'$(AppxTargetsLocation)'==''">$(MSBuildThisFileDirectory)build\</AppxTargetsLocation>
    ```

    ```xml
    <Import Project="$(Microsoft_ProjectReunion_AppXReference_props)" />
    <Import Project="$(Microsoft_WinUI_AppX_targets)" />
    ```

    删除此项组：

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.ProjectReunion" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
        <ExcludeAssets>all</ExcludeAssets>
      </PackageReference>
      <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="[0.5.0-prerelease]" GeneratePathProperty="true">
        <ExcludeAssets>all</ExcludeAssets>
      </PackageReference>
    </ItemGroup>
    ```

5. 在项目的 {YourProject}(package)/build/ 文件夹下，删除现有的 `Microsoft.WinUI.AppX.targets` 文件。

## <a name="major-changes-introduced-in-this-release"></a>此版本中引入的重大更改

### <a name="stable-features"></a>稳定版功能

此版本提供了使 WinUI 3 适用于可发布到 Microsoft Store 的生产应用的稳定性和支持。 它包括对过去的预览版中引入的大多数功能的支持和前向兼容性：

- 创建使用 WinUI 的桌面应用的功能，包括适用于 Win32 应用的 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)
- [RadialGradientBrush](/windows/uwp/design/style/brushes#radial-gradient-brushes)
- [TabView 更新](/windows/uwp/design/controls-and-patterns/tab-view)
- 深色主题更新
- 对 [WebView2](/microsoft-edge/hosting/webview2) 的改进和更新
  - 支持高 DPI
  - 支持调整窗口大小和移动窗口
  - 已更新为面向较新版本的 Edge
  - 不再需要引用 WebView2 特定的 Nuget 程序包
- SwapChainPanel
- MRT 核心支持
  - 这可使应用在启动时速度更快、更轻质，还能加快资源查找速度。
- ARM64 支持
- 在应用内部和外部进行拖放
- RenderTargetBitmap（目前仅限 XAML 内容 - 无 SwapChainPanel 内容）
- 自定义光标支持
- 线程外输入
- 对我们工具/开发人员体验的改进：
  - 实时可视化树、热重载、实时属性资源管理器及类似工具
  - Intellisense for WinUI 3
- 开放源代码迁移所需的改进
- 自定义标题栏功能：新的 [Window.ExtendsContentIntoTitleBar](/windows/winui/api/microsoft.ui.xaml.window.extendscontentintotitlebar) 和 [Window.SetTitleBar](/windows/winui/api/microsoft.ui.xaml.window.settitlebar) API，支持开发人员在桌面应用中创建自定义标题栏。
- 虚拟图面图像源支持
- 应用内 Acrylic

### <a name="preview-features"></a>预览功能

由于这是一个稳定的版本，因此已从此版本的 WinUI 3 中删除了预览功能。 你仍可以使用 [WinUI 3 的当前预览版](release-notes/winui3-project-reunion-0.5-preview.md)来访问这些功能。 请注意，以下主要功能仍处于预览状态，我们正在努力使这些功能稳定：

- UWP 支持
  - 这意味着你不能使用 WinUI 3 Project Reunion 0.5 VSIX 构建或运行 UWP 应用。 需要使用 [WinUI 3 - Project Reunion 0.5 预览版 VSIX](https://aka.ms/projectreunion/previewdownload)，并按照 [Project Reunion 入门](../../project-reunion/get-started-with-project-reunion.md)中有关设置开发环境的其余说明进行操作。 有关详细信息，请参阅 [Windows UI 库 3 - Project Reunion 0.5 预览版（2021 年 3 月）发行说明](release-notes/winui3-project-reunion-0.5-preview.md)。

- 桌面应用中的多窗口支持

- 输入验证

### <a name="provide-feedback-and-suggestions"></a>提供反馈和建议

欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中提供反馈。

### <a name="whats-coming-next"></a>即将推出的内容有哪些？

若要详细了解规划特定功能的时间，请参阅 GitHub 上的[功能路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)。

## <a name="limitations-and-known-issues"></a>限制和已知问题

以下各项是 WinUI 3 Project Reunion 0.5 的一些已知问题。 如果发现下面未列出的问题，请在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中，通过为现有问题贡献内容或提交新问题来告知我们。

### <a name="platform-and-os-support"></a>平台和 OS 支持

WinUI 3 Project Reunion 0.5 与运行 Windows 10 2018 年 10 月更新（版本 1809 - 内部版本 17763）及更高版本的电脑兼容。

### <a name="developer-tools"></a>开发人员工具

- 仅支持 C# 和 C++/WinRT 应用
- 桌面应用支持 .NET 5 和 C# 9，而且必须在 MSIX 应用中打包
- 不提供 XAML 设计器支持
- 不支持新的 C++/CX 应用，不过，现有应用可继续运行（请尽快迁移到 C++/WinRT）
- 不支持未打包的桌面部署
- 使用 F5 运行桌面应用时，请确保你运行的是打包项目。 在应用项目上按 F5 将运行未打包的应用，而 WinUI 3 尚不支持此类应用。

### <a name="missing-platform-features"></a>缺少的平台功能

- Xbox 支持
- HoloLens 支持
- 窗口式弹出窗口
  - 更具体地说，无论属性值如何，`ShouldConstrainToRootBounds` 属性表现得如同被设置为 `true` 一样。
- 墨迹支持，包括：
  - [InkCanvas](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.InkCanvas)
  - [HandwritingView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.HandwritingView)
  - [InkPresenter](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking.InkPresenter)
- 背景 Acrylic
- MediaElement 和 MediaPlayerElement
- MapControl
- SwapChainPanel 不支持透明度
- 全局展示使用回退行为，即纯色画笔
- 此版本不支持 XAML 岛
- 直接在现有的非 WinUI 桌面应用中使用 WinUI 3 具有以下限制：用于迁移现有应用的当前可用路径是将新的 WinUI 3 项目添加到解决方案中，并根据需要调整或重构逻辑。

- 不会在桌面应用中调用 Application.Suspending。 有关更多详细信息，请参阅有关 [Application.Suspending 事件](https://docs.microsoft.com/windows/winui/api/microsoft.ui.xaml.application.suspending?view=winui-3.0-preview&preserve-view=true )的 API 参考文档。 

- 桌面应用中不支持 CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher 及其依赖项（请参阅下文）

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>桌面应用中的 CoreWindow、ApplicationView、CoreApplicationView 和 CoreDispatcher

WinUI 3 预览版 4 以及后续标准版中的新增功能 [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、[ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、[CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView) [CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 及其依赖项在桌面应用中不可用。 例如，[Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 属性始终为 null，但 Window.DispatcherQueue 属性可用作替代项 。

这些 API 仅适用于 UWP 应用。 在过去的预览版中，它们也部分地在桌面应用中工作，但自预览版 4 开始，它们被完全禁用。 这些 API 是针对 UWP 情况设计的，在这种情况下，每个线程只有一个窗口，而未来版 WinUI3 的一个功能就是启用多个窗口。

有一些 API 在内部依赖于这些 API 的存在，因此在桌面应用中不受支持。 这些 API 通常具有静态 `GetForCurrentView` 方法。 例如 [UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)。

有关受影响 API 以及这些 API 的变通方法和替代方法的详细信息，请参阅[适用于桌面应用的 WinRT API 更改](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)

### <a name="known-issues"></a>已知问题

- 按 Alt+F4 不会关闭桌面应用窗口。

- 桌面应用中不再支持 [UISettings.ColorValuesChanged 事件](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged)和 [AccessibilitySettings.HighContrastChanged 事件](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged)。 如果使用它来检测 Windows 主题中的更改，可能会导致问题。 

- 以前，如果要获取 CompositionCapabilities 实例，需要调用 [CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)。 但是，从此调用返回的功能不依赖于视图。 为了解决并反映此问题，我们已在此版本中删除 GetForCurrentView() 静态，因此现在可以直接创建 [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) 对象。

- Acrylic 画笔呈透明显示。 

- 由于某个 C#/WinRT 问题，订阅某些框架元素事件和页面导航可能会导致内存泄漏。 

- 在此版本中，你可能还会遇到其他 C#/WinRT 问题，例如 GC/ObjectDisposedExceptions、封送处理值和可以为 null 的类型（TimeSpan、IReference<Vector3> 等）。 
  - 这些问题将在即将于 4 月中旬推出的 .NET 5 SDK 服务版本中得到修复。 可通过以下方法获得此更新：通过显式下载并安装它（例如，在生成管道中）或通过 Visual Studio 更新隐式下载并安装它。

## <a name="winui-3-controls-gallery"></a>WinUI 3 控件库

查看 WinUI 3 控件库（以前称为 XAML 控件库 - WinUI 3 版本）以获取示例应用，该示例应用包含属于 WinUI 3 Project Reunion 0.5 的所有控件和功能。

![WinUI 3 控件库应用](images/WinUI3XamlControlsGallery.png)<br/>
WinUI 3 控件库应用的示例

可通过克隆 GitHub 存储库来下载该示例。 为此，请使用以下命令克隆 winui3 分支：

```
git clone --single-branch --branch winui3 https://github.com/microsoft/Xaml-Controls-Gallery.git
```

克隆后，请确保在本地 Git 环境中切换到 winui3 分支： 

```
git checkout winui3
```
