---
title: WinUI 3 预览版 3（2020 年 11 月）
description: WinUI 3 预览版 3 发布概述。
ms.date: 11/17/2020
ms.topic: article
ms.openlocfilehash: ac641036af8505b1e51fb81385f5206a9aa44f40
ms.sourcegitcommit: 29c8999fb7a941fc6e26b49cf10f4cc1fcb69641
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2020
ms.locfileid: "95002912"
---
# <a name="windows-ui-library-3-preview-3-november-2020"></a>Windows UI 库 3 预览版 3（2020 年 11 月）

Windows UI 库 (WinUI) 3 是适用于 Windows 桌面和 UWP 应用的本机用户体验 (UX) 框架。

WinUI 3 预览版 3 引入了新功能和改进功能，还提供显著的 Bug 修复。

**请参阅 [预览版 3 限制和已知问题](#preview-3-limitations-and-known-issues)** 。

> [!Important]
> 此 WinUI 3 预览版用于早期评估以及从开发人员社区收集反馈。 它 **不** 应该用于生产应用。
>
> 我们将继续在 2021 年推出 WinUI 3 的预览版，后续将发布第一个官方版。
>
> 请使用 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml)提供反馈、报告问题并提出建议。

## <a name="install-winui-3-preview-3"></a>安装 WinUI 3 预览版 3

WinUI 3 预览版 3 提供有 Visual Studio 项目模板和 NuGet 包，前者有助于你开始使用基于 WinUI 的用户界面构建应用，后者包含 WinUI 库。 若要安装 WinUI 3 预览版 3，请执行以下步骤。

> [!NOTE]
> 你还可克隆并生成 [XAML 控件库](#xaml-controls-gallery-winui-3-preview-3-branch)的 WinUI 3 预览版 3 版本。

1. 确保你的开发计算机上已安装 Windows 10 版本 1803（内部版本 17134）或更高版本。

2. 安装 [Visual Studio 2019 版本 16.9 预览版](https://visualstudio.microsoft.com/vs/preview/)

    安装 Visual Studio 时，必须包括以下工作负载：
    - .NET 桌面开发（这也将安装 .NET 5）
    - 通用 Windows 平台开发

    若要生成 C++ 应用，还必须包括以下工作负载：
    - 使用 C++ 的桌面开发
    - 适用于通用 Windows 平台工作负载的 C++ (v142) 通用 Windows 平台工具可选组件（请参阅右窗格中“通用 Windows 平台开发”部分下的“安装详细信息”）
3. 请确保系统已为 nuget.org 启用了 NuGet 包源。有关详细信息，请参阅[常见 NuGet 配置](/nuget/consume-packages/configuring-nuget-behavior)。[Windows 社区工具包](#windows-community-toolkit)

4. 下载并安装 [WinUI 3 预览版 3 VSIX 包](https://aka.ms/winui3/preview3-download)。 它将 WinUI 3 项目模板和包含 WinUI 3 库的 NuGet 包添加到 Visual Studio 2019。

    有关如何将 VSIX 包添加到 Visual Studio 的说明，请参阅[查找和使用 Visual Studio 扩展](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。

#### <a name="webview2"></a>WebView2

如果你正在应用中使用 WebView2 控件，请从 [Microsoft Edge Insider Channels](https://www.microsoftedgeinsider.com/en-us/download) 安装 Microsoft Edge 浏览器的 Dev 渠道版本。 请务必卸载Microsoft Edge Beta、Microsoft Edge Dev 和 Microsoft Edge WebView2 运行时的所有现有实例。

#### <a name="windows-community-toolkit"></a>Windows 社区工具包

如果使用的是 Windows 社区工具包，请[下载最新版本](https://aka.ms/wct-winui3)。

## <a name="create-winui-projects"></a>创建 WinUI 项目

安装 WinUI 3 预览版 3 VSIX 包后，即可使用 Visual Studio 中的一个 WinUI 项目模板创建一个新项目。 若要在“创建新项目”对话框中访问 WinUI 项目模板，请将语言筛选为“C++”或“C#”，将平台筛选为“Windows”，将项目类型筛选为“WinUI”    。 或者可以搜索“WinUI”并选择一个可用的 C# 或 C++ 模板。

![WinUI 项目模板](images/winui-projects-csharp.png)

有关 WinUI 项目模板入门的详细信息，请参阅以下文章：

- [适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)
- [适用于 UWP 应用的 WinUI 3 入门](get-started-winui3-for-uwp.md)

除[限制和已知问题](#preview-3-limitations-and-known-issues)外，使用 WinUI 项目生成应用类似于使用 XAML 和 WinUI 2.x 生成 UWP 应用。 因此，有关 UWP 应用和 Windows SDK 中的 Windows.UI WinRT 命名空间的大多数[指南文档](/windows/uwp/design/)均适用。

在此版本中，我们还添加了 [WinUI 3 API 参考文档](/windows/winui/api/)，它适用于移植到 WinUI 3 的所有 WinRT API。

如果使用 WinUI 3 预览版 2 创建了一个项目，可升级该项目以使用预览版 3。 请参阅 [WinUI GitHub 存储库](https://aka.ms/winui3/upgrade-instructions)了解详细说明。

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

为了充分利用添加到 WinUI 3 预览版 3 中的最新工具功能，例如热重载、实时可视化树和实时属性资源浏览器，你必须结合最新的 WinUI 3 预览版使用 Visual Studio 的最新预览版。 下表显示了未来版本与 WinUI 3 预览版 3 的兼容性：

| VS 版本  | WinUI 3 预览版 3  |
|---|---|
| 16.8 RTM  | 否   |
| 16.9 预览版  | 是  | 
| 16.9 RTM  | 否   |
| 16.10 预览版  | 是   |


## <a name="new-features-and-capabilities-in-preview-3"></a>预览版 3 中的新特性和功能

- ARM64 支持
- 在应用内部和外部进行拖放
- RenderTargetBitmap（目前仅限 XAML 内容 - 无 SwapChainPanel 内容）
- 对我们工具/开发人员体验的改进：
  - 实时可视化树、热重载、实时属性资源管理器及类似工具
  - Intellisense 现适用于 WinUI 3 
- MRT 核心支持
  - 这可使应用在启动时速度更快、更轻质，还能加快资源查找速度。
- 自定义光标支持
- 线程外输入
- 自预览版 2 以来的性能改进
- 桌面应用中的多个窗口 - 自预览版 2 以来改进了支持


## <a name="new-features-and-capabilities-introduced-in-past-winui-3-previews"></a>之前的 WinUI 3 预览版中引入的新特性和功能

以下特性和功能是在 WinUI 3 预览版 1 和 2 中引入的，并且在 WinUI 3 预览版 3 中继续受支持。

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
- 开放源代码迁移所需的改进

若要详细了解 WinUI 3 的优势和 WinUI 路线图，请参阅 GitHub 上的 [Windows UI 库路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md)。

### <a name="provide-feedback-and-suggestions"></a>提供反馈和建议

欢迎在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中提供反馈。

### <a name="whats-coming-next"></a>即将推出的内容有哪些？

请查看我们详细的[功能路线图](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md#winui-30-feature-roadmap)，了解何时将在 WinUI 3 中引入特定功能。 

## <a name="preview-3-limitations-and-known-issues"></a>预览版 3 的限制和已知问题

预览版 3 版本只是一个预览版。 围绕桌面应用制定的方案尤其新颖。 Bug、限制和其他问题是少不了的。

以下各项是 WinUI 3 预览版 3 的一些已知问题。 如果发现下面未列出的问题，请在 [WinUI GitHub 存储库](https://github.com/microsoft/microsoft-ui-xaml/issues/new/choose)中，通过为现有问题贡献内容或提交新问题来告知我们。

### <a name="platform-and-os-support"></a>平台和 OS 支持

WinUI 3 预览版 3与运行 Windows 10 2018 年 4 月更新（版本 1803 - 内部版本 17134）及更高版本的电脑兼容。

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

### <a name="known-issues"></a>已知问题

- Alt+F4 不会关闭桌面应用窗口。

-   如果你正在应用中使用 WebView2，但它未呈现或加载，那么你运行的 Microsoft Edge 浏览器的版本可能不兼容。 请[下载 Microsoft Edge Dev 渠道](https://www.microsoftedgeinsider.com/en-us/download)，且务必卸载Microsoft Edge Beta、Microsoft Edge Dev 和 Microsoft Edge WebView2 运行时的所有现有实例。

- 不得在 .NET 5 WinUI 应用中使用 Marshal 函数，理由是它们不能与 C#/WinRT 正确互操作。 有关更多详细信息，请参阅[本页面](https://github.com/microsoft/CsWinRT/blob/master/docs/interop.md)。

- 如果在设置 URI 属性时遇到故障，例如在 Xaml 标记中使用 `{Binding}` 时，可使用 `{x:Bind}` 或使用 C#/WinRT 的预览版来暂时解决此问题。 为此，请在 .csproj 文件中添加以下行：

  ```xml
  <ItemGroup>
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        RuntimeFrameworkVersion="10.0.18362.11-preview" />
    <FrameworkReference Update="Microsoft.Windows.SDK.NET.Ref" 
                        TargetingPackVersion="10.0.18362.11-preview" />
  </ItemGroup>
  ```

- 对于 C# UWP 应用：

  WinUI 3 框架是一组 WinRT 组件，可从 C++（使用 C++/WinRT）或 C# 使用它们。 使用 C# 时，存在两个版本的 .NET，具有由应用模型决定：在 UWP 应用中使用 WinUI 3 时，你使用的是 .NET Native；在桌面应用中使用时，你使用的是 .NET 5（和 C#/WinRT）。

  在 UWP 中对 WinUI 3 应用使用 C# 时，与 WinUI 3 桌面应用或 C# WinUI 2 应用中的 C# 相比，API 命名空间方面存在一些差异：一些类型位于 `Microsoft` 命名空间中，而不是在 `System` 命名空间中。 例如，`INotifyPropertyChanged` 接口在 `Microsoft.UI.Xaml.Data` 命名空间中，而不是在 `System.ComponentModel` 命名空间中。 

  这适用于：
    - `INotifyPropertyChanged`（及相关类型）
    - `INotifyCollectionChanged`
    - `ICommand`

  `System` 命名空间版本仍然存在，但不可用于 WinUI 3。 这意味着 `ObservableCollection` 在 WinUI 3 C# UWP 应用中不按原样工作。 有关暂时解决方案，请参阅 [XAML 控件库示例](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)中的 [CollectionsInterop 示例](https://github.com/microsoft/Xaml-Controls-Gallery/blob/winui3preview/XamlControlsGallery/CollectionsInterop.cs)。

## <a name="xaml-controls-gallery-winui-3-preview-3-branch"></a>XAML 控件库（WinUI 3 预览版 3 分支）

请参阅 [XAML 控件库的 WinUI 3 预览版 3 分支](https://github.com/microsoft/Xaml-Controls-Gallery/tree/winui3preview)，了解包含所有 WinUI 3 预览版 3 控件和功能的示例应用。

![WinUI 3 预览版 3 XAML 控件库应用](images/WinUI3XamlControlsGalleryP3.PNG)<br/>
*WinUI 3 预览版 3 XAML 控件库应用示例*

要下载该示例，请使用以下命令克隆 winui3preview 分支：

```
git clone --single-branch --branch winui3preview https://github.com/microsoft/Xaml-Controls-Gallery.git
```

克隆后，请确保在本地 Git 环境中切换到 winui3preview 分支： 

```
git checkout winui3preview
```
