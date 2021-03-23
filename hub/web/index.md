---
title: Windows 10 上的 Web 开发
description: Windows 10 上的 web 开发指南，其中包含来自 Microsoft 的工具、api 和各种资源的链接。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: web 开发, web dev, windows 上的 web, api, 边缘
ms.date: 01/06/2021
ms.openlocfilehash: 98b5d1443869baec9dd46796088ff7b71521237d
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804481"
---
# <a name="web-development-on-windows-10"></a>Windows 10 上的 Web 开发

Microsoft 为 web 开发人员提供各种资源，包括支持使用 Windows 10 进行 web 开发的新工具和功能。 本指南介绍了许多可用的工具，并提供了一个[提交反馈](../dev-environment/overview.md#additional-resources)的位置，你可以在其中提交反馈，让 Windows 成为你 web 开发的理想环境。 有关 API 的列表，请参阅[用于 web 开发的 API](/apis.md)。 如需更多帮助以便开始使用，请参阅[在 Windows 10 上设置开发环境](../dev-environment/overview.md)。

## <a name="webview-devtools-pwas"></a>WebView、开发工具、PWA

:::row:::
    :::column:::
        [![WebView 图标](../images/webview2.png)](https://developer.microsoft.com/microsoft-edge/webview2/)<br>
        **[WebView 2](https://developer.microsoft.com/microsoft-edge/webview2/)**<br>
        通过 Microsoft Edge WebView2 在本机应用程序中嵌入 web 内容（HTML、CSS 和 JavaScript）。
        [下载 WebView 2](https://developer.microsoft.com/microsoft-edge/webview2/#download-section)
    :::column-end:::
    :::column:::
        [![Microsoft Edge 开发工具图标](../images/microsoftedge-devtools.png)](/microsoft-edge/devtools-guide-chromium/index.md)<br>
        **[Microsoft Edge 开发工具](/microsoft-edge/devtools-guide-chromium/)**<br>
        Microsoft Edge 开发人员工具是一组直接内置于 Microsoft Edge 浏览器中的检查和调试工具。
    若要打开开发工具，请在 Microsoft Edge 焦点模式下执行以下操作：
        - 右键单击，然后检查
        - 选择 `F12` 键
        - `Ctrl` + `Shift` + `i`
    :::column-end:::
    :::column:::
       [![PWA 图标](../images/pwa-icon.png)](/microsoft-edge/progressive-web-apps-chromium/)<br>
        **[Windows 上的渐进式 Web 应用](/microsoft-edge/progressive-web-apps-chromium/)**<br>
        渐进式 Web 应用 (PWA) 为用户提供针对其设备自定义的类应用本机体验。 它们是进行逐步增强的网站，可在支持的平台上发挥像本机应用那样的功能。<br>
        [开始使用 PWA](/microsoft-edge/progressive-web-apps-chromium/get-started)
    :::column-end:::
:::row-end:::

## <a name="microsoft-edge-browser"></a>Microsoft Edge 浏览器

:::row:::
    :::column:::
       [![Microsoft Edge 图标](../images/microsoftedge.png)](https://www.microsoft.com/en-us/edge)<br>
        **[面向开发人员的 Microsoft Edge](https://developer.microsoft.com/microsoft-edge/)**<br>
        新版 Microsoft Edge 基于 Chromium，可创建更好的 web 兼容性，并减少基础 web 平台的碎片。 发布于 2020 年 1 月 15 日，在 Windows、macOS、iOS 和 Android 上受支持。 <br>
        [安装新版 Microsoft Edge](https://www.microsoft.com/edge)
    :::column-end:::
    :::column:::
        [![Microsoft Edge for Business](../images/microsoftedge-enterprise.png)](/deployedge/)<br>
        **[Microsoft Edge for Business](/deployedge/)**<br>
        Microsoft Edge 基于 Chromium 并提供企业支持。 获取有关如何配置和部署多个可用通道的分步指导。<br>
        [下载 Microsoft Edge 通道](https://www.microsoft.com/edge/business/download)
    :::column-end:::
    :::column:::
        [![Microsoft Edge Insider 图标](../images/microsoftedge-beta.png)](https://www.microsoftedgeinsider.com/whats-new)<br>
        **[Microsoft Edge Insider](https://www.microsoftedgeinsider.com/whats-new)**<br>
        我们每天都在为 Microsoft Edge 生成新内容。 了解我们最近的进度以及如何参与。
        [下载 Microsoft Edge Beta 版本](https://www.microsoftedgeinsider.com/)
    :::column-end:::
    :::column:::
        [![Microsoft Edge 支持图标](../images/microsoftedge-support.png)](https://support.microsoft.com/microsoft-edge)<br>
        **[Microsoft Edge 支持](https://support.microsoft.com/microsoft-edge)**<br>
        获取有关自定义浏览器、添加扩展、跟踪防护、疑难解答等方面的帮助。
        [获取 Microsoft Edge 帮助](https://support.microsoft.com/microsoft-edge)
    :::column-end:::
:::row-end:::

## <a name="debugging-testing-and-accessibility"></a>调试、测试和辅助功能

:::row:::
    :::column:::
       [![VS Marketplace Edge 调试器扩展](../images/visualstudio-edge-debugger.png)](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)<br>
        **[VS Code：适用于 Microsoft Edge 的调试器](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge/)**<br>
        通过供此 VS Code 扩展，在 Microsoft Edge 浏览器中调试 JavaScript 代码。 还可在 Visual Studio 中从 ASP.NET 项目使用它。<br>
        [安装 VS Code - 适用于 Microsoft Edge 的调试器](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)
    :::column-end:::
    :::column:::
       [![虚拟机图标](../images/virtualmachine.png)](https://developer.microsoft.com/microsoft-edge/tools/vms/)<br>
        **[用于测试的虚拟机](https://developer.microsoft.com/microsoft-edge/tools/vms/)**<br>
        使用免费的 Windows 10 虚拟机（本地下载和管理）测试 IE11 和 Microsoft Edge 旧版。<br>
        [下载虚拟机](https://developer.microsoft.com/microsoft-edge/tools/vms/)
    :::column-end:::
    :::column:::
       [![WebHint 图标](../images/webhint.png)](https://webhint.io/)<br>
        **[可提升可访问性的 WebHint](https://webhint.io/)**<br>
        一种可自定义的 lint 分析工具，它通过检查代码以寻找最佳做法和常见错误，帮助提高站点的可访问性、速度、跨浏览器兼容性等。<br>
        [安装 VS Code 扩展](https://webhint.io/docs/user-guide/extensions/vscode-webhint/)<br>
        [安装浏览器扩展](https://webhint.io/docs/user-guide/extensions/extension-browser/)<br>
        [安装 CLI](https://webhint.io/docs/user-guide/)
    :::column-end:::
    :::column:::
       [![WebDriver 图标](../images/webdriver.png)](/microsoft-edge/webdriver-chromium/)<br>
        **[WebDriver](/microsoft-edge/webdriver-chromium/)**<br>
        使用 Microsoft WebDriver 在 Microsoft Edge 中自动测试网站，结束开发人员周期中的循环。<br>
        [安装 WebDriver](https://developer.microsoft.com/microsoft-edge/tools/webdriver/)
    :::column-end:::
:::row-end:::

## <a name="visual-studio-code-editors"></a>Visual Studio Code 编辑器

:::row:::
    :::column:::
       [![VS Code 图标](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        轻量级源代码编辑器，具有对 JavaScript、TypeScript、Node.js 的内置支持；并且还是丰富的扩展（C++、C#、Java、Python、PHP、Go）和运行时（例如 .NET 和 Unity）生态系统。<br>
        [安装 VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio 图标](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio (IDE)](/visualstudio/windows/)**<br>
        集成开发环境，可用于编辑、调试、生成代码和发布应用，包括编译器、IntelliSense 代码完成以及更多功能。<br>
        [安装 Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![VS Code Marketplace 图标](../images/vs-code-marketplace.png)](https://marketplace.visualstudio.com/vscode)<br>
        **[适用于扩展的 VS Code Marketplace](https://marketplace.visualstudio.com/vscode)**<br>
        探索可用于自定义 Visual Studio Code 编辑器的各种扩展。<br>
        [安装扩展](https://marketplace.visualstudio.com/vscode)
    :::column-end:::
    :::column:::
       [![Visual Studio Marketplace 图标](../images/vs-marketplace.png)](https://marketplace.visualstudio.com/vs/)<br>
        **[供应扩展的 Visual Studio Marketplace](https://marketplace.visualstudio.com/vs)**<br>
        探索可用于自定义 Visual Studio 集成开发环境的众多扩展。<br>
        [安装扩展](https://marketplace.visualstudio.com/vs)
    :::column-end:::
:::row-end:::

## <a name="wsl-terminal-package-manager-docker-desktop"></a>WSL、终端、包管理器、Docker Desktop

:::row:::
    :::column:::
       [![WSL 图标](../images/windows-linux-dev-env.png)](/windows/wsl/)<br>
        **[适用于 Linux 的 Windows 子系统](/windows/wsl/)**<br>
        使用与 Windows 完全集成的偏好 Linux 分发版（不再需要双引导）。<br>
        [安装 WSL](/windows/wsl/install-win10)
    :::column-end:::
    :::column:::
       [![Windows 终端图标](../images/terminal.png)](/windows/terminal/)<br>
        **[Windows 终端](/windows/terminal/)**<br>
        自定义终端环境，以使用多个命令行 shell。
        <br>
        [安装终端](https://www.microsoft.com/p/windows-terminal/9n0dx20hk701?rtc=1&activetab=pivot:overviewtab)
    :::column-end:::
    :::column:::
       [![Windows 程序包管理器图标](../images/winget.png)](../package-manager/index.md)<br>
        **[Windows 程序包管理器](../package-manager/index.md)**<br>
        通过命令行使用 winget.exe 客户端在 Windows 10 上安装应用。<br>
        [安装 Windows 程序包管理器（公共预览版）](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![适用于 Windows 的 Docker Desktop 图标](../images/docker-icon.png)](../dev-environment/docker/overview.md)<br>
        **[适用于 Windows 的 Docker Desktop](../dev-environment/docker/overview.md)**<br>
        利用 Visual Studio、VS Code、.NET、适用于 Linux 的 Windows 子系统或各种 Azure 服务的支持来创建远程开发容器。<br>
        [安装用于 Windows 的 Docker 桌面](https://docs.docker.com/docker-for-windows/install/)
    :::column-end:::
:::row-end:::

## <a name="aspnet-typescript-xamarin"></a>ASP.NET、Typescript、Xamarin

:::row:::
    :::column:::
       [![ASP.NET 图标](../images/aspnet.png)](https://dotnet.microsoft.com/apps/aspnet)<br>
        **[ASP.NET](/aspnet/)**<br>
        一个跨平台框架，用于通过 .NET 和 C# 构建 web 应用和服务、物联网 (IoT) 应用或移动后端。 在 Windows、macOS 和 Linux 上使用你喜爱的开发工具。 部署到云或本地。 在 .NET Core 上运行。<br>
        [安装 ASP.NET](https://dotnet.microsoft.com/download)
    :::column-end:::
    :::column:::
       [![Typescript 图标](../images/typescript-icon.png)](https://www.typescriptlang.org/)<br>
        **[Typescript](https://www.typescriptlang.org/)**<br>
        TypeScript 通过向 JavaScript 添加类型来扩展该语言。 例如，JavaScript 提供了 string、number 和 object 等语言基元，但不会检查你对这些基元的赋值是否一致。 而 TypeScript 会这么做。<br>
        [在浏览器中试用](https://www.typescriptlang.org/play/) [本地安装](https://www.typescriptlang.org/#installation)
    :::column-end:::
    :::column:::
       [![Xamarin 存储库图标](../images/xamarin-icon.png)](/xamarin/)<br>
        **[Xamarin](/xamarin/)**<br>
        Xamarin 允许你使用 .NET 代码和特定于平台的用户界面生成适用于 Android、iOS 和 macOS 的本机应用。 Xamarin.Forms 允许你使用采用 C# 或 XAML 编写的共享 UI 代码生成本机应用。
        <br>
        [安装 Xamarin](/xamarin/get-started/installation/)
    :::column-end:::
:::row-end:::

## <a name="open-source-contributions"></a>开源编辑

:::row:::
    :::column:::
       [![开源图标](../images/opensource-icon.png)](https://opensource.microsoft.com/)<br>
        **[Microsoft 开源](https://opensource.microsoft.com/)**<br>
        每天都有数千名 Microsoft 工程师使用、参与和发布开放源代码。 常用项目包括 Visual Studio Code、TypeScript、.NET 和 ChakraCore。<br>
        [参与](https://opensource.microsoft.com/collaborate)
    :::column-end:::
    :::column:::
       [![WinDev 存储库图标](../images/windev-repo.png)](https://github.com/microsoft/WinDev)<br>
        **[Windows 开发人员性能问题存储库](https://github.com/microsoft/WinDev)**<br>
        无论你针对 Windows 还是使用 Windows 进行开发，只要你将其用作跨平台开发计算机，我们都希望了解令你困扰的任何性能问题。
        <br>
        [提交性能问题](https://github.com/microsoft/WinDev/issues)
    :::column-end:::
    :::column:::
       [![文档图标](../images/docs.png)](/contribute/)<br>
        **[参与编辑文档](/contribute/)**<br>
        Microsoft 文档集中大部分是托管在 GitHub 上的开放源代码。 可通过提交问题或创作拉取请求来作出贡献。
        <br>
        [了解操作方法](/contribute/)
    :::column-end:::
:::row-end:::

## <a name="cloud-development-with-azure"></a>使用 Azure 进行云开发

:::row:::
    :::column:::
       [![Azure 图标](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        用于托管现有应用并简化新开发的完整云平台。 Azure 服务集成了开发、测试、部署和管理应用所需的一切。<br>
        [设置 Azure 帐户](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![Azure 认知服务图标](../images/azure-cognitive-services.png)](/azure/cognitive-services/what-are-cognitive-services)<br>
        **[Azure 认知服务](/azure/cognitive-services/what-are-cognitive-services)**<br>
        具有 REST API 和客户端库 SDK 的基于云的服务，可用于帮助你将认知智能构建到应用程序中。<br>
        [试用认知服务](https://azure.microsoft.com/en-us/services/cognitive-services/)
    :::column-end:::
    :::column:::
       [![Azure 开发人员指南图标](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[了解 Azure](/azure/guides/developer/azure-developer-guide)**<br>
        用于托管现有应用并简化新开发的完整云平台。 Azure 服务集成了开发、测试、部署和管理应用所需的一切。<br>
        [设置 Azure 帐户](https://azure.microsoft.com/free/)
    :::column-end:::
:::row-end:::

## <a name="addtional-resources"></a>其他资源

:::row:::
    :::column:::
       [![设置开发环境图标](../images/dev-environment-icon.png)](../dev-environment/overview.md)<br>
        **[在 Windows 10 上设置开发环境](../dev-environment/overview.md)**<br>
        获取帮助以设置开发环境以使用 Python、NodeJS、C#、C、C++，生成 Android 应用，生成 Windows 桌面应用，生成 Docker 容器，运行 PowerShell 脚本等。
        <br>
        [入门](../dev-environment/overview.md)
    :::column-end:::
    :::column:::
       [![适用于 Windows 的 React Native 图标](../images/reactnative-windows.png)](https://microsoft.github.io/react-native-windows/)<br>
        **[适用于 Windows 和 macOS 的 React Native 图标](https://microsoft.github.io/react-native-windows/)**<br>
        为 Windows 10 SDK 和 macOS 10.13 SDK 提供 React Native 支持。 使用 JavaScript 为 Windows 10 支持的所有设备（包括电脑、平板电脑、二合一设备、Xbox、混合现实设备等）以及 macOS 台式机和笔记本电脑生态系统构建本机 Windows 应用程序。
        <br>
        [安装适用于 Windows 的 React Native](https://microsoft.github.io/react-native-windows/docs/getting-started)<br>
        [安装适用于 macOS 的 React Native](https://microsoft.github.io/react-native-windows/docs/rnm-getting-started)
    :::column-end:::
    :::column:::
       [![Learn 图标](../images/learn-icon.png)](/learn/browse/?terms=web)<br>
        **[与 web 开发相关的 Microsoft Learn 课程](/learn/browse/?terms=web)**<br>
        Microsoft Learn 提供免费的在线课程，可帮助你学习各种新技能，并通过分步指南来发现 Microsoft 产品和服务。
        <br>
        [开始学习](/learn/browse/?terms=web)
    :::column-end:::
:::row-end:::

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 和 Windows 之间转换

查看我们的[在 Mac 和 Windows（或适用于 Linux 的 Windows 子系统）之间进行转换的指南](../dev-environment/mac-to-windows.md)。

- [键盘快捷方式](../dev-environment/mac-to-windows.md#keyboard-shortcuts)
- [触控板快捷方式](../dev-environment/mac-to-windows.md#trackpad-shortcuts)
- [终端和 shell 工具](../dev-environment/mac-to-windows.md#command-line-shells-and-terminals)
- [应用和实用程序](../dev-environment/mac-to-windows.md#apps-and-utilities)
- [从 Mac 切换到 Windows 的开发人员的成功案例](../dev-environment/dev-stories.md)