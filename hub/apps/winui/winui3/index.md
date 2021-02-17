---
title: WinUI 3 预览版 4（2021 年 2 月）
description: WinUI 3 预览版 4 发布概述。
ms.date: 02/09/2021
ms.topic: article
ms.openlocfilehash: 7bbc5c4983f77080366942ecaf702e7e1f844886
ms.sourcegitcommit: 884318ec5118cade85a31f4d5644436614e9f272
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/15/2021
ms.locfileid: "100524993"
---
# <a name="windows-ui-library-3-preview-4-february-2021"></a>Windows UI 库 3 预览版 4（2021 年 2 月）

Windows UI 库 (WinUI) 3 是适用于 Windows 桌面和 UWP 应用的本机用户体验 (UX) 框架。

WinUI 3 预览版 4 是稳定预览版本，其中包含重要的 bug 修复和其他常规改进（请参阅[预览版 4 中推出的功能](#capabilities-introduced-in-preview-4)）。

> [!Important]
> 此 WinUI 3 预览版用于早期评估以及从开发人员社区收集反馈。 它 **不** 应该用于生产应用。
>
> 我们将继续在 2021 年推出 WinUI 3 的预览版，后续将在 2021 年 3 月发布第一个官方的受支持版本。
>
> 请使用 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml)提供反馈、报告问题并提出建议。

## <a name="install-winui-3-preview-4"></a>安装 WinUI 3 预览版 4

WinUI 3 预览版 4 提供有 Visual Studio 项目模板和 NuGet 包，前者有助于你开始使用基于 WinUI 的用户界面构建应用，后者包含 WinUI 库。 若要安装 WinUI 3 预览版 4，请执行以下步骤。

> [!NOTE]
> 你还可克隆并生成 [XAML 控件库](#xaml-controls-gallery-winui-3-preview-4-branch)的 WinUI 3 预览版 4 版本。

1. 确保你的开发计算机上已安装 Windows 10 版本 1803（内部版本 17134）或更高版本。

2. 安装 [Visual Studio 2019 版本 16.9 预览版](https://visualstudio.microsoft.com/vs/preview/)。 下载最新预览版，以确保获取工作负载所需的所有更新（如 .NET 5）。

    安装 Visual Studio 时，必须包括以下工作负载：
    - 通用 Windows 平台开发

    若要生成 .NET 应用，还必须包括以下工作负载：
    - .NET 桌面开发（这还会安装你所需的最新版本的 .NET 5）

    若要生成 C++ 应用，还必须包括以下工作负载：
    - 使用 C++ 的桌面开发
    - 适用于通用 Windows 平台工作负载的 C++ (v142) 通用 Windows 平台工具可选组件（请参阅右窗格中“通用 Windows 平台开发”部分下的“安装详细信息”）

3. 请确保系统已为 nuget.org 启用了 NuGet 包源。有关详细信息，请参阅[常见 NuGet 配置](/nuget/consume-packages/configuring-nuget-behavior)。

4. 下载并安装 [WinUI 3 预览版 4 VSIX 包](https://aka.ms/winui3/preview4-download)。 它将 WinUI 3 项目模板和包含 WinUI 3 库的 NuGet 包添加到 Visual Studio 2019。

    有关如何将 VSIX 包添加到 Visual Studio 的说明，请参阅[查找和使用 Visual Studio 扩展](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。

5. 若要使用 WinUI 3 工具（如“实时可视化树”、“热重载”和“实时属性资源管理器”），必须按照[此处说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述，启用 Visual Studio Preview 功能中的 WinUI 3 工具。

#### <a name="webview2"></a>WebView2
若要将 WebView2 用于 WinUI 3 预览版 4，请下载[此页](https://developer.microsoft.com/microsoft-edge/webview2/)上的 Evergreen Bootstrapper 或 Evergreen Standalone Installer。 

#### <a name="windows-community-toolkit"></a>Windows 社区工具包

如果使用的是 Windows 社区工具包，请[下载最新版本](https://aka.ms/wct-winui3)。

## <a name="create-winui-projects"></a>创建 WinUI 项目

安装 WinUI 3 预览版 4 VSIX 包后，即可使用 Visual Studio 中的一个 WinUI 项目模板创建一个新项目。 若要在“创建新项目”对话框中访问 WinUI 项目模板，请将语言筛选为“C++”或“C#”，将平台筛选为“Windows”，将项目类型筛选为“WinUI”    。 或者可以搜索“WinUI”并选择一个可用的 C# 或 C++ 模板。

![WinUI 项目模板](images/winui-projects-csharp.png)

有关 WinUI 项目模板入门的详细信息，请参阅以下文章：

- [适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)
- [适用于 UWP 应用的 WinUI 3 入门](get-started-winui3-for-uwp.md)

除[限制和已知问题](#limitations-and-known-issues)外，使用 WinUI 项目生成应用类似于使用 XAML 和 WinUI 2.x 生成 UWP 应用。 因此，有关 UWP 应用和 Windows SDK 中的 Windows.UI WinRT 命名空间的大多数[指南文档](/windows/uwp/design/)均适用。

即将推出此版本的 API 参考文档。 参考文档推出时，相关链接将在此处提供。 在此期间，请查看[适用于预览版 3 的 WinUI 3 API 参考文档](/windows/winui/api/)。

如果使用 WinUI 3 预览版 3 创建了一个项目，可升级该项目以使用预览版 4。 请参阅 [WinUI GitHub 存储库](https://aka.ms/winui3/upgrade-instructions)了解详细说明。

### <a name="project-templates-for-winui-3"></a>适用于 WinUI 3 的项目模板

可以使用这些 WinUI 项目模板来创建应用。

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 打包的空白应用（桌面版 WinUI） | C# 和 C++ | 使用基于 WinUI 的用户界面创建桌面 NET 5 (C#) 或本机 Win32 (C++) 应用。 生成的项目包括一个基本窗口，该窗口派生自 WinUI 库中的 Microsoft.UI.Xaml.Window 类，可用于生成 UI。 有关此项目类型的详细信息，请参阅[适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)。<p></p>此解决方案还包括一个 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目经配置后可将应用生成为 [MSIX 包](/windows/msix/overview)。 这提供了一种新式部署体验，能够通过包扩展与 Windows 10 功能集成以及更多其他功能。  |
| 空白应用（UWP 版 WinUI）  | C# 和 C++ | 创建一个具有基于 WinUI 的用户界面的 UWP 应用。 生成的项目包括一个基础页面，该页面派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Page 类，可用于生成 UI。 有关此项目类型的详细信息，请参阅[适用于 UWP 应用的 WinUI 3 入门](get-started-winui3-for-uwp.md)。 |

可以使用这些 WinUI 项目模板来构建可由基于 WinUI 的应用加载和使用的组件。

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 类库（桌面版 WinUI） | 仅限 C# | 使用 C# 创建一个 .NET 5 托管类库 (DLL)，使其可由具有基于 WinUI 的用户界面的其他 .NET 5 桌面应用使用。  |
| 类库（UWP 版 WinUI）  | 仅限 C# | 使用 C# 创建一个托管类库 (DLL)，使其可由具有基于 WinUI 的用户界面的其他 UWP 应用使用。 |
| Windows 运行时组件 (WinUI) | C++ | 创建一个用 C++/WinRT 编写的 [Windows 运行时组件](/windows/uwp/winrt-components/)，使其可由任何具有基于 WinUI 的用户界面的 UWP 或桌面应用使用，而不管这些应用使用哪种编程语言编写。 |
| Windows 运行时组件 (UWP) | C# | 创建一个用 C# 编写的 [Windows 运行时组件](/windows/uwp/winrt-components/)，使其可由任何具有基于 WinUI 的用户界面的 UWP 应用使用，而不管这些应用使用哪种编程语言编写。 |

### <a name="item-templates-for-winui-3"></a>适用于 WinUI 3 的项模板

下列项模板适合在 WinUI 项目中使用。 若要访问这些 WinUI 项模板，请在“解决方案资源管理器”中右键单击项目节点，选择“添加” -> “新建项”，然后在“添加新项”对话框中单击“WinUI”    。

![WinUI 项模板](images/winui-items-csharp.png)

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 空白页 (WinUI) | C# 和 C++ | 添加一个 XAML 文件和定义了新页面的代码文件，该页面派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Page 类。 |
| 空白窗口（桌面版 WinUI） | C# 和 C++ | 添加一个 XAML 文件和定义了新窗口的代码文件，该窗口派生自 WinUI 库中的 Microsoft.UI.Xaml.Window 类。 |
| 自定义控件 (WinUI) | C# 和 C++ | 添加用于创建具有默认样式的模板化控件的代码文件。 该模板化控件派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Control 类。<p></p>有关如何使用该项目模板的演练，请参阅[使用 C++/WinRT 将 UWP 和 WinUI 3 应用的 XAML 控件模板化](xaml-templated-controls-cppwinrt-winui-3.md)和[使用 C# 将 UWP 和 WinUI 3 应用的 XAML 控件模板化](xaml-templated-controls-csharp-winui-3.md)。 有关模板化控件的详细信息，请参阅[自定义 XAML 控件](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
| 资源字典 (WinUI) | C# 和 C++ | 添加 XAML 资源的空键控集合。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源参考](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。 |
| 资源文件 (WinUI) | C# 和 C++ | 添加用于存储应用的字符串和条件资源的文件。 可以借助此项对应用程序进行本地化。 有关详细信息，请参阅[对 UI 和应用包清单中的字符串进行本地化](/windows/uwp/app-resources/localize-strings-ui-manifest)。 |
| 用户控件 (WinUI) | C# 和 C++ | 添加 XAML 文件和用于创建用户控件的代码文件，该用户控件派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.UserControl 类。 通常，用户控件封装相关的现有控件并提供其自己的逻辑。<p></p>有关用户控件的详细信息，请参阅[自定义 XAML 控件](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |

### <a name="visual-studio-support"></a>Visual Studio 支持

为了充分利用添加到 WinUI 3 预览版 4 中的最新工具功能（例如热重载、实时可视化树和实时属性资源浏览器），必须结合最新的 WinUI 3 预览版使用 Visual Studio 的最新预览版，并确保启用 Visual Studio Preview 功能中的 WinUI 工具，如[此处说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述。 下表显示了未来版本与 WinUI 3 预览版 4 的兼容性：

| VS 版本  | WinUI 3 预览版 4  |
|---|---|
| 16.8 RTM  | 否   |
| 16.9 预览版  | 是  | 
| 16.9 RTM  | 否   |
| 16.10 预览版  | 是   |

## <a name="capabilities-introduced-in-preview-4"></a>预览版 4 中推出的功能

- 与 WinUI 2.5 的奇偶校验（包括 InfoBar 控件、ProgressRing 和 NavigationView 中的新功能以及 bug 修补程序）
- 自定义标题栏功能：新的 Window.ExtendsContentIntoTitleBar 和 Window.SetTitleBar API，支持开发人员在桌面应用中创建自定义标题栏。
- VirtualSurfaceImageSource  支持

## <a name="list-of-bugs-fixed-in-preview-4"></a>预览版 4 中已修复的 bug 列表

下面是自预览版 3 以来团队已修复的面向用户的 bug 的列表。 围绕稳定性和改进测试，还有很多工作要做。

- 此版本已采用新版本的 CS/WinRT 和 Windows SDK，对以下 bug 进行了修复：
  - 使用 {Binding} 绑定到 URI 属性时出现故障
  - C#/WinRT 封送函数不能与 .NET 5 正确互操作

- 在 Windows 预览体验内部版本上运行时出现 WinUI 3 故障
  - 感谢多个社区参与者在 GitHub 上报告此 bug！ 
- WebView2 不将主机应用的语言/区域设置应用于 CoreWebView2Environment
- Windows 社区工具包 DataGrid 控件在启动/显示滚动条时，使应用发生故障
  - 感谢多个社区参与者在 GitHub 上报告此 bug！
- 当显示模式发生变化时，页面呈现进入错误状态
- 使用 CalendarView 中的语言复合框时出现故障
- WinUI 3 桌面：无法按 tab 键退出 WebView2
- WinUI 3 桌面：具有派生的 TreeViewNodes 的 TreeView 出现故障 
  - 感谢 @eleanorleffler [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2699)！
- WinUI 3 桌面：无法在 ContentDialog 中的文本框内输入文本 
  - 感谢 @eleanorleffler [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2704)！
- WinUI 3 桌面：ALT 和 F6 不起作用
- 旧的已删除的 SwapChainPanel 在新的 SwapChain 上呈现
  - 感谢 @dotMorten [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2942)！
- WinUI 3 桌面：无法在触控板中滚动
- 将 NavigationView 控件用于同一线程上的多个窗口时出现故障
- 辅助功能问题：WinUI 桌面应用启动时显示定焦矩形
- 在 DataGrid 中滚动时出现访问冲突
  - 感谢 @TroelsL [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2946)！
- WinUI 3 桌面：Tab 键循环不起作用 
- 在具有 WinUI Xaml Islands 的桌面应用程序中，针对 GridView 的拖放操作失败
  - 感谢 @smk2007 [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3871)！
- 辅助功能问题：无法在 WinUI 3 桌面上使用 PageUp/PageDown 键滚动
- WebView2 具有错误的视区大小
- 打开 MenuFlyout 后单击时 WebView2 出现故障
  - 感谢 @sudongg [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3729)！
- WinUI 3 桌面：尝试关闭 DropDownButton 或 SplitButton 的浮出控件会导致应用故障
- WebView2：右键双击鼠标会导致故障
- 单击 ToggleSplitButton 将导致应用程序故障
  - 感谢 @lhak [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3641)！
- WinUI 3 桌面：任务栏上显示空的 DesktopWindowXamlSource 窗口
  - 感谢 @bridgesquared [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3698)！
- WinUI 3 桌面：DataGrid 不显示
  - 感谢 @eleanorleffler [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2703)！
- WinUI 3 桌面：无法将文件拖放到网格上 
  - 感谢 @eleanorleffler [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/2715)！
- WinUI 3 桌面：WinUI 3 预览版 2 中的 ItemsRepeater 出现故障 
  - 感谢 @hshristov [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3007)！
- 更新绑定时引发 AccessViolationException
  - 感谢 @WamWooWam [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3680)！
- WinUI 3 桌面：应用在滚动 NavigationView 上出现故障
  - 感谢 @Berkunath [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3598)！
- 动态添加或删除 ItemsControl 的 ItemsSource 集合中的项时，ItemsControl 不会更新。 
  - 感谢 @VigneshRameshh [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3517)！
- 如果启用 C++ 一致性模式，App.xaml.g.h 中会发生编译错误 C2760 
  - 感谢 @boostafazoo [在 GitHub 上提交了此问题](https://github.com/microsoft/microsoft-ui-xaml/issues/3716)！


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>之前的 WinUI 3 预览版中引入的新特性和功能

以下特性和功能是在 WinUI 3 预览版 1 -3 中引入的，并且在 WinUI 3 预览版 4 中继续受支持。

- 能够创建采用 WinUI 的桌面应用，包括适用于 Win32 应用的 [.NET 5](https://github.com/dotnet/core/tree/master/release-notes/5.0)
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

若要详细了解 WinUI 3 的优势和 WinUI 路线图，请参阅 GitHub 上的 [Windows UI 库路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供反馈和建议

欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中提供反馈。

### <a name="whats-coming-next"></a>即将推出的内容有哪些？

请查看我们详细的[功能路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)，了解何时将在 WinUI 3 中引入特定功能。 

## <a name="limitations-and-known-issues"></a>限制和已知问题

预览版 4 版本只是一个预览版。 围绕桌面应用制定的方案尤其新颖。 Bug、限制和其他问题是少不了的。

以下各项是 WinUI 3 预览版 4 的一些已知问题。 如果发现下面未列出的问题，请在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中，通过为现有问题贡献内容或提交新问题来告知我们。

### <a name="platform-and-os-support"></a>平台和 OS 支持

WinUI 3 预览版 4 与运行 Windows 10 2018 年 4 月更新（版本 1803 - 内部版本 17134）及更高版本的电脑兼容。

### <a name="developer-tools"></a>开发人员工具

- 仅支持 C# 和 C++/WinRT 应用
- 桌面应用支持 .NET 5 和 C# 9，而且必须在 MSIX 应用中打包
- UWP 应用支持 .NET Native 和 C# 7.3
- 在 Visual Studio 16.8 版本中，开发人员工具和 Intellisense 可能无法正常工作。
- 不提供 XAML 设计器支持
- 不支持新的 C++/CX 应用，不过，现有应用可继续运行（请尽快迁移到 C++/WinRT）
- 现在应用中提供对多个窗口的支持，但该项支持尚不完整，也不稳定。
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

#### <a name="corewindow-applicationview-coreapplicationview-and-coredispatcher-in-desktop-apps"></a>桌面应用中的 CoreWindow、ApplicationView、CoreApplicationView 和 CoreDispatcher

预览版 4 中的新增功能 [CoreWindow](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow)、[ApplicationView](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationView)、[CoreApplicationView](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.CoreApplicationView)
[CoreDispatcher](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) 及其依赖项在桌面应用中不可用。

例如，[Window.Dispatcher](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window.Dispatcher) 属性始终为 null，但 Window.DispatcherQueue 属性可用作一种替代方法。

这些 API 仅适用于 UWP 应用。
在过去的预览版中，它们在桌面应用中也可以部分使用，但在预览版 4 中它们被完全禁用。
这些 API 是针对 UWP 情况设计的，其中每个线程只适用于一个窗口，WinUI3 的功能之一是启用多个窗口。

有一些 API 在内部依赖于这些 API 的存在，因此在桌面应用中不受支持。 这些 API 通常具有静态 `GetForCurrentView` 方法。 例如 [UIViewSettings.GetForCurrentView](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.UIViewSettings.GetForCurrentView)。


### <a name="known-issues"></a>已知问题

- Alt+F4 不会关闭桌面应用窗口。

- 由于 [CoreWindow](https://docs.microsoft.com/uwp/api/windows.ui.core.corewindow) 的更改，以下 WinRT API 可能不再像预期的那样适用于桌面应用：
  - [`ApplicationView`](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationview) 和所有相关 API 将不再适用。
  - [`CoreApplicationView`](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplicationview) 和所有相关 API 将不再适用。
  - 可能并不支持所有 `GetForCurrentView` API，例如 [`CoreInputView.GetForCurrentView`](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.Core.CoreInputView.GetForCurrentView)。
  - [`CoreWindow.GetForCurrentThread`](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow.GetForCurrentThread) 现在将返回 null。

  有关在 WinUI 3 桌面应用中使用 WinRT API 的详细信息，请参阅[可用于桌面应用的 Windows Runtime API](https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-supported-api
)。

- 桌面应用中不再支持 [UISettings.ColorValuesChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.colorvalueschanged)和 [AccessibilitySettings.HighContrastChanged 事件](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.accessibilitysettings.highcontrastchanged)。 如果使用它来检测 Windows 主题中的更改，可能会导致问题。 

- 此版本包含一些试验性 API。 这些尚未经过团队全面测试，并且可能存在未知问题。 如果遇到任何问题，请在我们的存储库中[提交 bug](https://github.com/microsoft/microsoft-ui-xaml/issues/new?assignees=&labels=&template=bug_report.md&title=)。 

- 以前，如果要获取 CompositionCapabilities 实例，需要调用 [CompositionCapabilites.GetForCurrentView()](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities.getforcurrentview)。 但是，从此调用返回的功能不依赖于视图。 为了解决并反映此问题，我们已在此版本中删除 GetForCurrentView() 静态，因此现在可以直接创建 [CompositionCapabilties](https://docs.microsoft.com/uwp/api/windows.ui.composition.compositioncapabilities) 对象。

- 对于 C# UWP 应用：

  WinUI 3 框架是一组 WinRT 组件，可从 C++（使用 C++/WinRT）或 C# 使用它们。 使用 C# 时，存在两个版本的 .NET，具有由应用模型决定：在 UWP 应用中使用 WinUI 3 时，你使用的是 .NET Native；在桌面应用中使用时，你使用的是 .NET 5（和 C#/WinRT）。

  在 UWP 中对 WinUI 3 应用使用 C# 时，与 WinUI 3 桌面应用或 C# WinUI 2 应用中的 C# 相比，API 命名空间方面存在一些差异：一些类型位于 `Microsoft` 命名空间中，而不是在 `System` 命名空间中。 例如，`INotifyPropertyChanged` 接口在 `Microsoft.UI.Xaml.Data` 命名空间中，而不是在 `System.ComponentModel` 命名空间中。 

  这适用于：
    - `INotifyPropertyChanged`（及相关类型）
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 命名空间版本仍然存在，但不可用于 WinUI 3。 这意味着 `ObservableCollection` 在 WinUI 3 C# UWP 应用中不按原样工作。 有关暂时解决方案，请参阅 [XAML 控件库示例](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)中的 [CollectionsInterop 示例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。

## <a name="xaml-controls-gallery-winui-3-preview-4-branch"></a>XAML 控件库（WinUI 3 预览版 4 分支）

请参阅 [XAML 控件库的 WinUI 3 预览版 4 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)，了解包含所有 WinUI 3 预览版 4 控件和功能的示例应用。

![WinUI 3 预览版 4 XAML 控件库应用](images/WinUI3XamlControlsGallery.PNG)<br/>
*WinUI 3 预览版 4 XAML 控件库应用示例*

要下载该示例，请使用以下命令克隆 winui3preview 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

克隆后，请确保在本地 Git 环境中切换到 winui3preview 分支： 

```
git checkout winui3preview
```
