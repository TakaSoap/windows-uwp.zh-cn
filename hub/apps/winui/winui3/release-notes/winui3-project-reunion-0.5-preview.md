---
title: Windows UI 库 3 - Project Reunion 0.5 预览版（2021 年 3 月）发行说明
description: Windows UI 库 3 - Project Reunion 0.5 预览版（2021 年 3 月）发行说明。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: e6d6f1a2007ab09b0295de24b1761d3dbb0c9094
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730646"
---
# <a name="windows-ui-library-3---project-reunion-05-preview-march-2021-release-notes"></a>Windows UI 库 3 - Project Reunion 0.5 预览版（2021 年 3 月）发行说明

Windows UI 库 (WinUI) 3 是用于构建新式 Windows 应用的原生用户体验 (UX) 平台。 WinUI 3 的此预览版既适用于桌面/Win32 应用，也适用于 UWP 应用，并且包含 Visual Studio 项目模板和 NuGet 包，前者有助于你开始使用基于 WinUI 的用户界面构建应用，后者包含 WinUI 库。

WinUI 3 - Project Reunion 0.5 预览版是 WinUI 3 的第一个版本，在 Project Reunion 包中提供。 除了这项更改之外，此预览版本还包含一些关键的 bug 修复、更高的稳定性以及其他一些常规改进（请参阅[WinUI 3 - Project Reunion 0.5 预览版中引入的功能](#major-changes-introduced-in-this-release)）。

> [!Important]
> 此 WinUI 3 预览版用于早期评估以及从开发人员社区收集反馈。 它 **不** 应该用于生产应用。
>
> 我们预计将在 3 月下旬发布 Project Reunion 0.5，其中包含 WinUI 3 的第一个稳定受支持版本。
>
> 请使用 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml)提供反馈、报告问题并提出建议。

## <a name="install-winui-3---project-reunion-05-preview"></a>安装 WinUI 3 - Project Reunion 0.5 预览版

此版本的 WinUI 3 作为 Project Reunion 0.5 预览版的一部分提供。 

若要安装，请按照 Project Reunion 的[设置开发环境](../../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)指南中的说明进行操作。 

与 WinUI 3 以前的预览版本相比，你将下载 Project Reunion VSIX 包，而不是 WinUI VSIX 包。 Project Reunion VSIX 包含你要用于构建 WinUI 3 应用的 [WinUI 项目模板](../winui-project-templates-in-visual-studio.md)。 完成安装后，WinUI 3 应用的开发体验不会改变。

> [!NOTE]
> 你还可以克隆并构建 WinUI 3 预览版本的 [XAML 控件库](#xaml-controls-gallery-winui-3-preview-branch)。

> [!NOTE]
> 若要使用 WinUI 3 工具（如“实时可视化树”、“热重载”和“实时属性资源管理器”），必须按照[此处说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述，启用 Visual Studio Preview 功能中的 WinUI 3 工具。

设置开发环境后，请查看[创建 WinUI 3 项目](../winui-project-templates-in-visual-studio.md)，熟悉可用的 Visual Studio 项目和项模板。 

有关 WinUI 项目模板入门的详细信息，请参阅以下文章：

- [适用于桌面应用的 WinUI 3 入门](../get-started-winui3-for-desktop.md)
- [适用于 UWP 应用的 WinUI 3 入门（预览）](../get-started-winui3-for-uwp.md)

除[限制和已知问题](#limitations-and-known-issues)外，使用 WinUI 项目生成应用类似于使用 XAML 和 WinUI 2.x 生成 UWP 应用。 因此，有关 UWP 应用和 Windows SDK 中的 Windows.UI WinRT 命名空间的大多数[指南文档](/windows/uwp/design/)均适用。

WinUI 3 API 参考文档请参阅此处：[WinUI 3 API 参考](/windows/winui/api)

如果之前创建了使用 WinUI 3 预览版 4 的项目，可将该项目升级到使用 Project Reunion 0.5 预览版。 请参阅 [WinUI GitHub 存储库](https://aka.ms/winui3/upgrade-instructions)了解详细说明。

### <a name="webview2"></a>WebView2
要将 WebView2 与此 WinUI 3 预览版一起使用，请下载在[此页](https://developer.microsoft.com/microsoft-edge/webview2/)上找到的常青引导程序或常青独立安装程序（如果尚未安装 WebView2 运行时）。 

### <a name="windows-community-toolkit"></a>Windows 社区工具包

如果使用的是 Windows 社区工具包，请[下载最新版本](https://aka.ms/wct-winui3)。

### <a name="visual-studio-support"></a>Visual Studio 支持

为了充分利用添加到 WinUI 3 中的最新工具功能（例如热重载、实时可视化树和实时属性资源管理器），必须结合使用最新的 Visual Studio 预览版和最新的 WinUI 3 预览版，并确保启用 Visual Studio 预览功能中的 WinUI 工具，如[此处的说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。 下表显示了未来版本与 WinUI 3 - Project Reunion 0.5 预览版的兼容性：

| VS 版本  | WinUI 3 - Project Reunion 0.5 预览版  |
|---|---|
| 16.8 RTM  | 否   |
| 16.9 预览版  | 是，有工具  | 
| 16.9 RTM  | 是，但没有热重载、实时可视化树和实时属性资源管理器   |
| 16.10 预览版  | 是，有工具   |

## <a name="major-changes-introduced-in-this-release"></a>此版本中引入的重大更改

- WinUI 3 现在作为 Project Reunion 包的一部分发布，这也是我们未来受支持版本的发布机制。

- 现在支持应用内 acrylic。

- 透视控件不再受支持，已在 WinUI 3 中弃用。 建议使用 [NavigationView 控件](/windows/uwp/design/controls-and-patterns/navigationview)实现应用内导航。

- 只有 Windows 10 版本 1809（内部版本 17763）或更高版本才支持 WinUI 3 和 Project Reunion。

- 预览功能现在标记为试验功能。 
  - 预览功能是指 WinUI 3 预览版中继续包含，但下一个 WinUI 3 受支持版本不会包含的任何功能。 
  - 预览功能还包括 WinUI 2.6 预览版中包含的所有试验 API。
  - 构建使用预览功能的应用时，该应用会发出警告。 


## <a name="list-of-bugs-fixed-in-winui-3---project-reunion-05-preview"></a>WinUI 3 - Project Reunion 0.5 预览版中已修复 bug 的列表

下面是自预览版 3 以来团队已修复的面向用户的 bug 的列表。 围绕稳定性和改进测试，还有很多工作要做。

- WinUI 3 错误消息需要改写：“无法解析‘Windows.metadata’。  请安装 Windows 软件开发工具包。 Windows SDK 已随 Visual Studio 一起安装。”
- 选择 Windows 默认主题时，应用不会响应 Windows 中的主题更改，重启后才会响应
- 调用 XamlDirect.CreateInstance 时发生异常
  - 感谢 @BorzillaR [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3509)！
- 进度栏不显示“暂停”和“错误”选项之间的区别
- 标准 UI 控件列表视图项浮出控件显示在错误位置。
- 尝试通过触摸重新排列列表视图项时，桌面 XAML 控件库崩溃
  - 感谢 @j0shuams [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3694)！
- 按住格式文本块会使浮出控件放置在错误位置
- 按向左/向右箭头时焦点不会在各单选按钮之间移动
  - 感谢 @vmadurga [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3385)！
- 当用户按向下/向上箭头键选择日期选取器中的下一个/上一个月/年/日时，讲述人保持沉默

- 导航视图轻型消除在 WinUI 3 中不起作用


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>之前的 WinUI 3 预览版中引入的新特性和功能

以下特性和功能在 WinUI 3 预览版 1-4 中引入，在 WinUI 3 - Project Reunion 0.5 预览版中继续受支持。

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

若要详细了解 WinUI 3 的优势和 WinUI 路线图，请参阅 GitHub 上的 [Windows UI 库路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供反馈和建议

欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中提供反馈。

### <a name="whats-coming-next"></a>即将推出的内容有哪些？

请查看我们详细的[功能路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)，了解何时将在 WinUI 3 中引入特定功能。 

## <a name="limitations-and-known-issues"></a>限制和已知问题

WinUI 3 - Project Reunion 0.5 预览版只是一个预览版。 围绕桌面应用制定的方案尤其新颖。 Bug、限制和其他问题是少不了的。

以下各项是 WinUI 3 - Project Reunion 0.5 预览版的一些已知问题。 如果发现下面未列出的问题，请在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中，通过为现有问题贡献内容或提交新问题来告知我们。

### <a name="platform-and-os-support"></a>平台和 OS 支持

WinUI 3 - Project Reunion 0.5 预览版与运行 Windows 10 2018 年 4 月更新（版本 1809 - 内部版本 17763）及更高版本的电脑兼容。

### <a name="developer-tools"></a>开发人员工具

- 仅支持 C# 和 C++/WinRT 应用
- 桌面应用支持 .NET 5 和 C# 9，而且必须在 MSIX 应用中打包
- UWP 应用支持 .NET Native 和 C# 7.3
- 在 Visual Studio 中，开发人员工具和 Intellisense 可能无法正常工作。
- 不提供 XAML 设计器支持
- 不支持新的 C++/CX 应用，不过，现有应用可继续运行（请尽快迁移到 C++/WinRT）
- 现在，桌面应用中提供对多个窗口的支持，但该功能尚不完整，也不稳定。
  - 如果发现多窗口行为出现新问题或性能下降，请在我们的存储库中提交 Bug。
- 不支持未打包的桌面部署
- 使用 F5 运行桌面应用时，请确保你运行的是打包项目。 在应用项目上按 F5 将运行未打包的应用，而 WinUI 3 尚不支持此类应用。

### <a name="missing-platform-features"></a>缺少的平台功能

- Xbox 支持
- HoloLens 支持
- 窗口式弹出窗口
  - 更具体地说，无论属性值如何，`ShouldConstrainToRootBounds` 属性表现得如同被设置为 `true` 一样。
- 墨迹书写支持
- 亚克力
- MediaElement 和 MediaPlayerElement
- MapControl
- SwapChainPanel 和非 XAML 内容的 RenderTargetBitmap
- SwapChainPanel 不支持透明度
- 全局展示使用回退行为，即纯色画笔
- 此版本不支持 XAML 岛
- 第三方生态系统库将无法完全正常运行
- IME 不起作用
- 桌面应用中不支持 CoreWindow、ApplicationView、CoreApplicationView、CoreDispatcher 及其依赖项（请参阅下文）

### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>桌面应用中的 CoreWindow、ApplicationView、CoreApplicationView 和 CoreDispatcher

预览版 4 以及后续标准版中的新增功能 [CoreWindow](/uwp/api/Windows.UI.Core.CoreWindow)、[ApplicationView](/uwp/api/Windows.UI.ViewManagement.ApplicationView)、[CoreApplicationView](/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
[CoreDispatcher](/uwp/api/Windows.UI.Core.CoreDispatcher) 及其依赖项在桌面应用中不可用。 例如，[Window.Dispatcher](/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 属性始终为 null，但 Window.DispatcherQueue 属性可用作一种替代方法。

这些 API 仅适用于 UWP 应用。 在过去的预览版中，它们在桌面应用中也可以部分使用，但在预览版 4 中它们被完全禁用。 这些 API 是针对 UWP 情况设计的，其中每个线程只适用于一个窗口，WinUI3 的功能之一是启用多个窗口。

有一些 API 在内部依赖于这些 API 的存在，因此在桌面应用中不受支持。 这些 API 通常具有静态 `GetForCurrentView` 方法。 例如 [UIViewSettings.GetForCurrentView](/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)。

有关受影响 API 以及这些 API 的变通方法和替代方法的详细信息，请参阅[适用于桌面应用的 WinRT API 更改](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/winrt-apis-for-desktop.md)

### <a name="known-issues"></a>已知问题

- 存在导致 UWP 应用无法在 Windows 10 版本 1809 上启动的问题。 团队正在努力修复此 bug，希望在下一个预览版本中解决此问题。

- 按 Alt+F4 不会关闭桌面应用窗口。

- 桌面应用中不再支持 [UISettings.ColorValuesChanged 事件](/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged)和 [AccessibilitySettings.HighContrastChanged 事件](/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged)。 如果使用它来检测 Windows 主题中的更改，可能会导致问题。 

- 此版本包含一些试验 API，使用这些 API 时会出现版本警告。 这些尚未经过团队全面测试，并且可能存在未知问题。 如果遇到任何问题，请在我们的存储库中[提交 bug](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=)。 

- 以前，如果要获取 CompositionCapabilities 实例，需要调用 [CompositionCapabilites.GetForCurrentView()](/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)。 但是，从此调用返回的功能不依赖于视图。 为了解决并反映此问题，我们已在此版本中删除 GetForCurrentView() 静态，因此现在可以直接创建 [CompositionCapabilties](/uwp/api/windows.ui.composition.compositioncapabilities) 对象。

- 对于 C# UWP 应用：

  WinUI 3 框架是一组 WinRT 组件，可从 C++（使用 C++/WinRT）或 C# 使用它们。 使用 C# 时，存在两个版本的 .NET，具体由应用模型决定：在 UWP 应用中使用 WinUI 3 时，你使用的是 .NET Native；在桌面应用中使用时，你使用的是 .NET 5（和 C#/WinRT）。

  在 UWP 中对 WinUI 3 应用使用 C# 时，与 WinUI 3 桌面应用或 C# WinUI 2 应用中的 C# 相比，API 命名空间方面存在一些差异：一些类型位于 `Microsoft` 命名空间中，而不是在 `System` 命名空间中。 例如，`INotifyPropertyChanged` 接口在 `Microsoft.UI.Xaml.Data` 命名空间中，而不是在 `System.ComponentModel` 命名空间中。 

  这适用于：
    - `INotifyPropertyChanged`（及相关类型）
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 命名空间版本仍然存在，但不可用于 WinUI 3。 这意味着 `ObservableCollection` 在 WinUI 3 C# UWP 应用中不按原样工作。 有关暂时解决方案，请参阅 [XAML 控件库示例](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)中的 [CollectionsInterop 示例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。

## <a name="xaml-controls-gallery-winui-3-preview-branch"></a>XAML 控件库（WinUI 3 预览版分支）

请参阅 [WinUI 3 预览版分支的 XAML 控件库](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)获取示例应用，该示例应用包含属于 WinUI 3 - Project Reunion 0.5 预览版的所有控件和功能。

:::image type="content" source="../images/WinUI3XamlControlsGallery.png" alt-text="WinUI 3 预览版XAML 控件库应用":::<br/>
WinUI 3 预览版 XAML 控件库应用示例

要下载该示例，请使用以下命令克隆 winui3preview 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

克隆后，请确保在本地 Git 环境中切换到 winui3preview 分支： 

```
git checkout winui3preview
```
