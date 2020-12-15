---
description: 了解 Project Reunion、它为开发人员提供的好处、现在开发人员可使用的功能以及如何提供反馈。
title: Project Reunion
ms.topic: article
ms.date: 11/17/2020
keywords: windows win32, 桌面开发, project reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 7d5969afacc0f811e3a5c40488f45bb5b421c4b3
ms.sourcegitcommit: cddc595969c658ce30fbc94ded92db4a8ad1bf66
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349352"
---
# <a name="build-windows-apps-with-project-reunion-prerelease"></a>使用 Project Reunion（预发行版）构建 Windows 应用

Project Reunion 0.1 预发行版是一组新的预览版开发人员组件和工具，它们代表着 Windows 应用开发平台的下一步发展。 Project Reunion 提供了一组统一的 API 和工具，各种目标 Windows 10 OS 版本上的任何应用都能够以一致的方式使用它们。 Project Reunion 不会替换现有的 Windows 应用平台和框架，例如 UWP、本机 Win32 以及 .NET， 而是通过一组通用的 API 和工具（开发人员可以在这些平台中使用）来对这些现有平台进行补充。

> [!NOTE]
> Project Reunion 0.1 预发行版是开发人员的早期预览版。 建议在开发环境中试用此版本。 但请注意，从现在的版本到最终的版本，Project Reunion 将会在许多方面发生变化。 生产环境中使用的应用不支持 Project Reunion 0.1 预发行版。 Project Reunion 是一个代号，在未来的版本中可能会更改。

## <a name="goals-of-project-reunion"></a>Project Reunion 的目标

Project Reunion 提供了各种 Windows API，其实现与 OS 分离，并通过 NuGet 包发布给开发人员。 Project Reunion 并不打算替换 Windows SDK。 Windows SDK 将继续按原样工作，并且 Windows 的许多核心组件将通过 API 不断改进，这些 API 通过 OS 和 Windows SDK 版本发布。 建议开发人员按照自己的进度采用 Project Reunion。

Project Reunion 是为支持以下目标而创建的。

#### <a name="unified-api-surface-across-different-types-of-windows-apps"></a>在不同类型的 Windows 应用中实现统一的 API 图面

要创建 Windows 应用的开发人员必须在多个应用平台和框架之间进行选择。 尽管每个平台都提供了许多可供使用其他平台构建的应用使用的功能和 API，但是某些功能和 API 仅供特定平台使用。 Project Reunion 将统一所有 Windows 10 应用对 Windows API 的访问。 无论选择哪种应用模型，你都可以访问 Project Reunion 中提供的同一组 Windows API。

随着时间的推移，我们计划对 Project Reunion 进行进一步的投资，以减少不同应用模型之间的差异。 Project Reunion 将同时提供 WinRT API 和本机 C API。

#### <a name="consistent-support-across-windows-10-versions"></a>在多个 Windows 10 版本中实现一致的支持

随着 Windows API 随新 OS 版本的发展而不断发展，开发人员必须使用[版本自适应代码](/windows/uwp/debug-test-perf/version-adaptive-code)等技术来解决版本中的所有差异，以吸引其应用程序受众。 这可能会增加代码和开发体验的复杂性。 通过 Project Reunion，我们将能够在不同的 OS 版本和所有 Windows 10 设备上统一对 Project Reunion API 访问，从而让我们有机会将更新提供给更多开发人员，而不仅仅是最新版本的 Windows。 当前的计划是让 Project Reunion 支持 Windows 10 版本 1809 以及 Windows 10 的所有更高版本。

#### <a name="faster-release-cadence"></a>加快发布节奏

新的 Windows API 和功能通常与每年以一次或两次的节奏发布的 OS 版本相关。 通过 Project Reunion，我们将能够以更频繁和更灵活的方式发布新的生产质量 API 和功能。

## <a name="get-started"></a>入门

Project Reunion 0.1 预发行版包含针对以下功能区域的新 API。

| 功能 | 描述 |
|---------|-------------|
| [MRT 核心（新式资源管理系统）](mrtcore/mrtcore-overview.md) | MRT 核心是新式 [Windows 资源管理系统](/windows/uwp/app-resources/resource-management-system)的简化版本，作为 Project Reunion 的一部分分发。 |
| [DWriteCore](dwritecore.md) | DWriteCore 是 DirectWrite 的一种形式，可在 Windows 到 Windows 8 的各种版本上运行，并为跨平台使用它奠定了基础。 |

可在[此处](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md)了解有关将其他组件引入 Project Reunion 的未来计划的详细信息。

> [!NOTE]
> 某些其他现有组件（包括 Windows UI 库 (WinUI)、MSIX 和 WebView2）已与 OS 分离，并遵循 Project Reunion 准则（例如，它们在 Windows 10 版本 1809 以及更高版本的 OS 中受支持）。 但是，这些其他组件目前未包含在 Project Reunion NuGet 包内。  

### <a name="set-up-your-development-environment"></a>设置开发环境

如果要试用 Project Reunion 0.1 预发行版，则必须从提供的一个 C++ 示例开始。 这些示例已预先配置为使用 Project Reunion NuGet 包。 此预览版不支持在自己的项目中安装 Project Reunion NuGet 包。 按照以下步骤设置开发环境，以使用其中一个示例。

1. 确保开发计算机上已安装 Windows 10 版本 1809（内部版本 17763）或更高版本的 OS。

2. 安装 [Visual Studio 2019 版本 16.9 预览版 2（或更高版本）](https://visualstudio.microsoft.com/vs/preview/) 安装 Visual Studio 时，必须包括以下工作负载：
    - .NET 桌面开发
    - 通用 Windows 平台开发
    - 使用 C++ 的桌面开发
    - 适用于通用 Windows 平台工作负载的 C++ (v142) 通用 Windows 平台工具可选组件（请参阅安装程序右窗格中“通用 Windows 平台开发”部分下的“安装详细信息”）  

3. 从 Visual Studio Marketplace 安装最新版本的 [C++/WinRT Visual Studio 扩展 (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)。

4. 请确保系统已为 nuget.org 启用了 NuGet 包源。有关详细信息，请参阅[常见 NuGet 配置](/nuget/consume-packages/configuring-nuget-behavior)。

5. 下载并安装 [WinUI 3 预览版 3 VSIX 包](https://aka.ms/winui3/preview3-download)。 仅已配置为使用 WinUI 3 的 Hello World 和 MRT 核心示例需要执行此步骤。 有关如何将 VSIX 包添加到 Visual Studio 的说明，请参阅[查找和使用 Visual Studio 扩展](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box)。

6. 克隆并浏览以下示例：
    - [DWriteCore 库示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery)：此示例应用程序演示了 [DWriteCore](dwritecore.md) API。
    - [MRT 核心示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)：此示例应用程序演示了 [MRT 核心](mrtcore/mrtcore-overview.md) API。
    - [Hello World 示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp)：此示例演示了与 Project Reunion NuGet 包的基本集成。

## <a name="known-issues"></a>已知问题

Project Reunion 0.1 预发行版具有以下限制：

 - 此版本仅支持 MSIX 打包的 C++/Win32 应用。
 - 此版本不支持 C#。
 - 此版本不支持 .NET 5。

## <a name="developer-roadmap"></a>开发人员路线图

有关最新的 Project Reunion 计划，请参阅我们的 [Github 页面](https://github.com/microsoft/ProjectReunion)。

## <a name="give-feedback-and-contribute"></a>提供反馈和参与

我们正在将 Project Reunion 做为开源项目进行构建。 我们在 [Github 页面](https://github.com/microsoft/ProjectReunion)上提供了许多有关我们计划如何实现 Project Reunion 的详细信息，并邀请你参与此开发过程。 查看我们的[参与者指南](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md)，以提出问题、开始讨论或提出功能建议。 我们希望确保 Project Reunion 能够为像你这样的开发人员带来最大的好处。
