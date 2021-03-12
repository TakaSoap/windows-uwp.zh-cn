---
description: 了解 Project Reunion、它为开发人员提供的好处、现在开发人员可使用的功能以及如何提供反馈。
title: Project Reunion
ms.topic: article
ms.date: 03/09/2021
keywords: windows win32, 桌面开发, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 2354e7f42ab9c487275c66c9f709f8791ba5005e
ms.sourcegitcommit: 539b428bcf3d72c6bda211893df51f2a27ac5206
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/11/2021
ms.locfileid: "102629242"
---
# <a name="build-desktop-windows-apps-with-project-reunion-05-preview-march-2021"></a>使用 Project Reunion 0.5 预览版（2021 年 3 月）构建桌面 Windows 应用

Project Reunion 是一组新的开发人员组件和工具，它们代表着 Windows 应用开发平台的下一步发展。 Project Reunion 提供了一组统一的 API 和工具，各种目标 Windows 10 OS 版本上的任何桌面应用都能够一致地使用它们。 

Project Reunion 不会替代现有的桌面 Windows 应用平台和框架，例如 .NET（包括 Windows 窗体和 WPF）和 C++/Win32。 而是通过一组通用的 API 和工具（开发人员可以在这些平台中使用）来对这些现有平台进行补充。

> [!NOTE]
> Project Reunion 0.5 预览版是开发人员预览版。 建议在开发环境中试用此版本。 但请注意，从现在的版本到 1.0 版本，Project Reunion 将会在许多方面发生变化。 生产环境中使用的应用不支持 Project Reunion 0.5 预览版。 Project Reunion 是一个代号，在未来的版本中可能会更改。

## <a name="benefits-of-project-reunion-for-windows-app-developers"></a>面向 Windows 应用开发人员的 Project Reunion 的优势

Project Reunion 提供了各种 Windows API，其实现与 OS 分离，并通过 NuGet 包发布给开发人员。 Project Reunion 并不打算替换 Windows SDK。 Windows SDK 将继续按原样工作，并且 Windows 的许多核心组件将通过 API 不断改进，这些 API 通过 OS 和 Windows SDK 版本发布。 建议开发人员按照自己的进度采用 Project Reunion。

#### <a name="unified-api-surface-across-desktop-app-platforms"></a>在不同的桌面应用平台中实现统一的 API 图面

想要创建桌面 Windows 应用的开发人员必须在多个应用平台和框架之间进行选择。 尽管每个平台都提供了许多可供使用其他平台构建的应用使用的功能和 API，但是某些功能和 API 仅供特定平台使用。 Project Reunion 统一了所有桌面 Windows 10 应用对 Windows API 的访问。 无论选择哪种应用模型，你都可以访问 Project Reunion 中提供的同一组 Windows API。

随着时间的推移，我们计划对 Project Reunion 进行进一步的投资，以减少不同应用模型之间的差异。 Project Reunion 将同时提供 WinRT API 和本机 C API。

#### <a name="consistent-support-across-windows-10-versions"></a>在多个 Windows 10 版本中实现一致的支持

随着 Windows API 随新 OS 版本的发展而不断发展，开发人员必须使用[版本自适应代码](/windows/uwp/debug-test-perf/version-adaptive-code)等技术来解决版本中的所有差异，以吸引其应用程序受众。 这可能会增加代码和开发体验的复杂性。

Project Reunion API 适用于 Windows 10 版本 1809 以及 Windows 10 的所有更高版本。 这意味着，只要你的客户使用的是 Windows 10 版本 1809 或任何更高版本，就可以在新的 Project Reunion API 和功能发布后立即使用，而无需编写版本自适应代码。

#### <a name="faster-release-cadence"></a>加快发布节奏

新的 Windows API 和功能通常与每年以一次或两次的节奏发布的 OS 版本相关。 Project Reunion 将以更快的节奏发布更新，使你能够在创建 Windows 开发平台后更早、更快地访问这些创新。

## <a name="components-available-with-project-reunion"></a>Project Reunion 包含的组件

Project Reunion 0.5 预览版包含以下组件。

| 组件 | 说明 |
|---------|-------------|
| [Windows UI 库 3](../winui/winui3/index.md) | Windows UI 库 (WinUI) 3 是既适用于桌面应用（.NET 和 C++/Win32）又适用于 UWP 应用的下一代 Windows 用户体验 (UX) 平台。 此版本包含 Visual Studio 项目模板和 NuGet 包，前者有助于你开始[使用基于 WinUI 的用户界面构建应用](..\winui\winui3\index.md#create-winui-projects)，后者包含 WinUI 库。  |
| [使用 MRT Core 管理资源](mrtcore/mrtcore-overview.md) | MRT Core 提供 API 来加载和管理你的应用所使用的资源。 MRT Core 是新式 [Windows 资源管理系统](/windows/uwp/app-resources/resource-management-system)的简化版本。 |
| [使用 DWriteCore 呈现文本](dwritecore.md) | 通过 DWriteCore，你可以获取用于呈现文本的所有当前 DirectWrite 功能，包括与设备无关的文本布局系统、硬件加速文本、多格式文本和广泛的语言支持。  |

可在[此处](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)了解有关将其他组件引入 Project Reunion 的未来计划的详细信息。

## <a name="set-up-your-development-environment"></a>设置开发环境

1. 确保开发计算机上已安装 Windows 10 版本 1809（内部版本 17763）或更高版本的 OS。

2. 如果尚未安装 [Visual Studio 2019 版本 16.10 预览版](https://visualstudio.microsoft.com/vs/preview/)（或更高版本），请先安装。

    安装 Visual Studio 时，必须包含以下组件：
    - 在“工作负载”选项卡上，确保选中“通用 Windows 平台开发” 。
    - 在“单个组件”选项卡上，确保在“SDK、库和框架”部分选择了“Windows 10 SDK (10.0.19041.0)”  。

    要构建 .NET 应用，还必须包含以下组件：
    - .NET 桌面开发工作负载。

    要构建 C++ 应用，还必须包含以下组件：
    - 使用 C++ 进行桌面开发工作负载。
    - 用于通用 Windows 平台开发的 C++ (v142) 通用 Windows 平台工具可选组件工作负载 。

3. 如果之前通过早期的 WinUI 3 预览版本安装了 [WinUI 3 预览版扩展](https://marketplace.visualstudio.com/items?itemName=Microsoft-WinUI.WinUIProjectTemplates)，请卸载该扩展。 有关如何卸载扩展的详细信息，请参阅[管理适用于 Visual Studio 的扩展](/visualstudio/ide/finding-and-using-visual-studio-extensions)。

4. 请确保系统已为 nuget.org 启用了 NuGet 包源。有关详细信息，请参阅[常见 NuGet 配置](/nuget/consume-packages/configuring-nuget-behavior)。

5. 在 Visual Studio 2019 中，单击“扩展” > “管理扩展”，搜索 Project Reunion，然后安装 Project Reunion 扩展   。 或者，你可以直接从 Visual Studio Marketplace 下载并安装 [Project Reunion 扩展](https://marketplace.visualstudio.com/items?itemName=ProjectReunion.MicrosoftProjectReunion) 。 有关如何将 VSIX 包添加到 Visual Studio 的说明，请参阅[管理适用于 Visual Studio 的扩展](/visualstudio/ide/finding-and-using-visual-studio-extensions)。

6. 若要使用 WinUI 3 工具（如“实时可视化树”、“热重载”和“实时属性资源管理器”），必须按照[此处说明](https://github.com/microsoft/microsoft-ui-xaml/issues/4140)所述，启用 Visual Studio Preview 功能中的 WinUI 3 工具。

## <a name="get-started-developing-with-project-reunion"></a>Project Reunion 开发入门

你可以在使用 Project Reunion 扩展随附的 WinUI 3 项目模板创建的新应用中使用 Project Reunion 0.5 预览版，也可以通过安装 NuGet 包在现有项目中使用 Project Reunion 组件。

> [!NOTE]
> Project Reunion 0.5 预览版仅支持 MSIX 打包的应用。

#### <a name="create-a-new-project-that-uses-project-reunion"></a>创建使用 Project Reunion 的新项目

适用于 Visual Studio 2019 的 Project Reunion 0.5 预览版扩展包括项目模板，通过这些模板，你可以生成具有基于 WinUI 3 的 UI 层的项目，并使用所有其他 Project Reunion API。 你可以使用这些模板来构建使用 Project Reunion 的 MSIX 打包桌面应用（C#/.NET 5 或 C++/WinRT）或 UWP 应用。 有关可用项目模板的详细信息，请参阅[创建 WinUI 项目](..\winui\winui3\index.md#create-winui-projects)。

创建使用 Project Reunion 0.5 预览版的新项目的方法：

1. 按照以下文章中的说明进行操作：

    - [适用于桌面应用的 WinUI 3 入门](..\winui\winui3\get-started-winui3-for-desktop.md)
    - [适用于 UWP 应用的 WinUI 3 入门](..\winui\winui3\get-started-winui3-for-uwp.md)

2. 创建项目后，除了桌面和 UWP 应用常用的所有其他 Windows 和 .NET API 之外，你还可以使用以下 Project Reunion API 和组件。

    - [Windows UI 库 3](../winui/winui3/index.md)
    - [使用 MRT Core 管理资源](mrtcore/mrtcore-overview.md)
    - [使用 DWriteCore 呈现文本](dwritecore.md)

要确认你的新项目使用 Project Reunion，请在解决方案资源管理器中展开自己项目下的“依赖项” > “包”节点  。 你应该会看到此节点下列出了若干 Microsoft.ProjectReunion 包，如下图所示。

![解决方案资源管理器窗格中的 Project Reunion 包的屏幕截图](images/reunion-packages.png)

#### <a name="use-project-reunion-in-an-existing-project"></a>在现有项目中使用 Project Reunion

如果你希望在现有桌面项目（C#/.NET 5 或 C++/WinRT）中使用 Project Reunion，可以在项目中安装 Project Reunion 0.5 预览版 NuGet 包。 

> [!NOTE]
> 在这种情况下，只能在项目中使用 Project Reunion 包含的非 WinUI 3 组件。 要使用 WinUI 3，必须使用上一节中介绍的其中一个 WinUI 3 项目模板创建一个新项目。

1. 在 Visual Studio 2019 中，打开现有桌面项目（C#/.NET 5 或 C++/WinRT）或 UWP 项目。

2. 确保已启用[包引用](/nuget/consume-packages/package-references-in-project-files)：

    1. 在 Visual Studio 中，单击“工具”->“NuGet 程序包管理器”->“程序包管理器设置”。
    2. 确保为“默认包管理格式”选择 PackageReference 。

3. 在“解决方案资源管理器”  中，右键单击你的项目并选择“管理 NuGet 包”  。

4. 在“NuGet 包管理器”窗口中，选择“浏览”选项卡，然后搜索 `Microsoft.ProjectReunion` 。

5. 找到 Microsoft.ProjectReunion 包后，在“NuGet 包管理器”窗口的右窗格中，单击“安装”  。

6. 安装包后，可以在项目中使用以下 Project Reunion API 和组件：

    - [使用 MRT Core 管理资源](mrtcore/mrtcore-overview.md)
    - [使用 DWriteCore 呈现文本](dwritecore.md)

#### <a name="deploying-apps-that-use-project-reunion"></a>部署使用 Project Reunion 的应用

当前，使用 Project Reunion 的应用必须使用 [MSIX](/windows/msix) 进行打包。 默认情况下，当你使用适用于 Visual Studio 的 Project Reunion 扩展随附的其中一个 WinUI 项目模板创建项目时，你的项目将包含 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目配置为将应用程序构建到 MSIX 包中。 有关配置此项目以对应用构建 MSIX 包的详细信息，请参阅[在 Visual Studio 中打包桌面或 UWP 应用](/windows/msix/package/packaging-uwp-apps)。

为应用构建 MSIX 包后，可以通过多种方法将其部署到其他计算机。 有关详细信息，请参阅[管理 MSIX 部署](/windows/msix/desktop/managing-your-msix-deployment-overview)。

> [!NOTE]
> 使用 Project Reunion 0.5 预览版的应用不能发布到 Microsoft Store。

## <a name="samples"></a>示例

目前，你可以浏览以下 Project Reunion 示例。

- [DWriteCore 库示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery)：此示例应用程序演示了 [DWriteCore](dwritecore.md) API。
- [MRT 核心示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)：此示例应用程序演示了 [MRT 核心](mrtcore/mrtcore-overview.md) API。
- [Hello World 示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp)：此示例演示了与 Project Reunion NuGet 包的基本集成。

## <a name="limitations-and-known-issues"></a>限制和已知问题

- 生产环境中使用的应用不支持此版本。 bug、限制和其他问题是少不了的。
- 此版本只能在 MSIX 打包的桌面应用（C#/.NET 5 或 C++/Win32）中使用。 不能在未打包的桌面应用中使用。
- [WinUI 3 的工具限制](..\winui\winui3\index.md#developer-tools)还适用于使用 Project Reunion 0.5 预览版的任何项目。

## <a name="developer-roadmap"></a>开发人员路线图

有关最新的 Project Reunion 计划，请参阅我们的 [Github 页面](https://github.com/microsoft/ProjectReunion)。

## <a name="give-feedback-and-contribute"></a>提供反馈和参与

我们正在将 Project Reunion 做为开源项目进行构建。 我们在 [Github 页面](https://github.com/microsoft/ProjectReunion)上提供了许多有关我们计划如何实现 Project Reunion 的详细信息，并邀请你参与此开发过程。 查看我们的[参与者指南](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)，以提出问题、开始讨论或提出功能建议。 我们希望确保 Project Reunion 能够为像你这样的开发人员带来最大的好处。
