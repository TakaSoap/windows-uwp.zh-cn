---
title: 在 Windows 10 上创建开发环境
description: 帮助你在 Windows 上设置开发环境并安装首选工具和代码语言的指南。 无论你是否选择使用 Python、NodeJS、VS Code、Git、Bash、Linux 工具和命令、Android Studio，我们都会为你提供功能强大的新工具（例如 Windows 终端和 WSL）。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: 设置 windows, 开发环境, 开发工具, 开发路径, Microsoft, Windows, 开发人员, 使用技巧, 性能, WSL, 终端, nodejs, python
ms.localizationpriority: medium
ms.date: 07/24/2020
ms.openlocfilehash: 3a577ca4241f102bcff2a84419a0e3523f812b74
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254641"
---
# <a name="set-up-your-development-environment-on-windows-10"></a>在 Windows 10 上设置开发环境

本指南将帮助你开始安装和设置在 Windows 或适用于 Linux 的 Windows 子系统上进行开发所需的语言和工具。

## <a name="development-paths"></a>开发路径

:::row:::
    :::column:::
       [![JavaScrip NodeJS 图标](../images/nodejs-logo.png)](../nodejs/index.yml)<br>
        **[NodeJS 入门](../nodejs/index.yml)**<br>
        在 Windows 或适用于 Linux 的 Windows 子系统上安装 NodeJS 并设置开发环境。
    :::column-end:::
    :::column:::
       [!["Python" 图标](../images/python-logo.png)](../python/index.yml)<br>
        **[Python 入门](../python/index.yml)**<br>
        在 Windows 或适用于 Linux 的 Windows 子系统上安装 Python 并设置开发环境。
    :::column-end:::
    :::column:::
       [![Android 图标](../images/android-logo.png)](/windows/android)<br>
        **[Android 入门](/windows/android)**<br>
        安装 Android Studio，或选择 Xamarin、React 或 Cordova 等跨平台解决方案，然后在 Windows 上设置开发环境。
    :::column-end:::
    :::column:::
       [![Windows 桌面图标](../images/windows-logo.png)](../apps/index.yml)<br>
        **[Windows 桌面入门](../apps/index.yml)**<br>
        开始使用 UWP、Win32、WPF、Windows Forms 生成适用于 Windows 10 的桌面应用，或者使用 MSIX 和 XAML Islands 更新和部署现有桌面应用。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![C/C++](../images/c-logo.png)](/cpp/)<br>
        **[C++ 和 C 入门](/cpp/)**<br>
        开始使用 C++、C 和程序集开发应用、服务和工具。
    :::column-end:::
    :::column:::
       [![C# 图标](../images/csharp-logo.png)](/dotnet/csharp/)<br>
        **[C# 入门](/dotnet/csharp/)**<br>
        开始使用 C# 和 .NET Core 生成应用。
    :::column-end:::
    :::column:::
       [![适用于 Windows 的 Docker Desktop 图标](../images/docker-logo.png)](../dev-environment/docker/overview.md)<br>
        **[适用于 Windows 的 Docker Desktop 入门](../dev-environment/docker/overview.md)**<br>
        利用 Visual Studio、VS Code、.NET、适用于 Linux 的 Windows 子系统或各种 Azure 服务的支持来创建远程开发容器。
    :::column-end:::
    :::column:::
       [![PowerShell 图标](../images/powershell.png)](/powershell/)<br>
        **[PowerShell 入门](/powershell/)**<br>
        开始使用 PowerShell（一种命令行 shell 和脚本语言）自动完成跨平台任务和管理配置。
    :::column-end:::
:::row-end:::

:::row:::
    :::column:::
       [![Rust 图标](../images/rust-icon.png)](./rust/index.yml)<br>
        **[Rust 入门](./rust/index.yml)**<br>
        开始使用 Rust 编程 &mdash; 其中包括如何使用 Windows crate 设置 Rust for Windows。
    :::column-end:::
:::row-end:::

## <a name="tools-and-platforms"></a>工具和平台

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
        将 winget.exe 客户端（一种综合程序包管理器）与命令行配合使用，在 Windows 10 上安装应用程序。<br>
        [安装 Windows 程序包管理器（公共预览版）](../package-manager/winget/index.md#install-winget)
    :::column-end:::
    :::column:::
       [![PowerToys 图标](../images/powertoys.png)](https://github.com/microsoft/PowerToys)<br>
        **[Windows PowerToys](https://github.com/microsoft/PowerToys)**<br>
        通过这组高级用户实用程序，调整并简化 Windows 体验，以提高工作效率。<br>
        [安装 PowerToys（公共预览版）](https://github.com/microsoft/PowerToys#installing-and-running-microsoft-powertoys)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
       [![VS Code 图标](../images/Vscode.png)](https://code.visualstudio.com/docs)<br>
        **[VS Code](https://code.visualstudio.com/docs)**<br>
        轻量级源代码编辑器，具有对 JavaScript、TypeScript、Node.js 的内置支持；并且还是丰富的扩展（C++、C#、Java、Python、PHP、Go）和运行时（例如 .NET 和 Unity）生态系统。<br>
        [安装 VS Code](https://code.visualstudio.com/download)
    :::column-end:::
    :::column:::
       [![Visual Studio 图标](../images/visualstudio.png)](/visualstudio/windows/)<br>
        **[Visual Studio](/visualstudio/windows/)**<br>
        集成开发环境，可用于编辑、调试、生成代码和发布应用，包括编译器、IntelliSense 代码完成以及更多功能。<br>
        [安装 Visual Studio](/visualstudio/install/install-visual-studio)
    :::column-end:::
    :::column:::
       [![Azure 图标](../images/Azure.png)](/azure/guides/developer/azure-developer-guide)<br>
        **[Azure](/azure/guides/developer/azure-developer-guide)**<br>
        用于托管现有应用并简化新开发的完整云平台。 Azure 服务集成了开发、测试、部署和管理应用所需的一切。<br>
        [设置 Azure 帐户](https://azure.microsoft.com/free/)
    :::column-end:::
    :::column:::
       [![.NET 图标](../images/net.png)](https://dotnet.microsoft.com/)<br>
        **[.NET](/dotnet/standard/get-started/)**<br>
        带有工具和库的开源开发平台，可用于生成任何类型的应用，包括 Web、移动、桌面、游戏、IoT、云和微服务。<br>
        [安装 .NET](https://dotnet.microsoft.com/download)
    :::column-end:::
:::row-end:::

<br>

## <a name="run-windows-and-linux"></a>运行 Windows 和 Linux

适用于 Linux 的 Windows 子系统 (WSL) 允许开发人员同时运行 Linux 操作系统和 Windows。 两者共享同一硬盘驱动器（可以访问对方的文件），剪贴板自然地支持二者之间的复制和粘贴，不需要双启动。 WSL 使你能够使用 BASH，并提供 Mac 用户最熟悉的环境。
- 在 [WSL 文档](/windows/wsl)中或通过[第 9 频道的 WSL 视频](https://channel9.msdn.com/Search?term=wsl&lang-en=true)了解详细信息。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-can-I-do-with-WSL--One-Dev-Question/player?format=ny]

你还可以使用 Windows 终端，通过多个选项卡或多个窗格在同一窗口中打开所有喜欢的命令行工具（PowerShell、Windows 命令提示符、Ubuntu、Debian、Azure CLI、Oh-my-Zsh、Git Bash 或以上所有工具）。

在 [Windows 终端文档](/windows/terminal)中或通过[第 9 频道上的 Windows 终端视频](https://channel9.msdn.com/Search?term=windows%20terminal&lang-en=true)详细信息。

> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/What-are-the-main-features-of-the-new-Terminal--One-Dev-Question/player?format=ny]

## <a name="transitioning-between-mac-and-windows"></a>在 Mac 和 Windows 之间转换

查看我们的[在 Mac 和 Windows（或适用于 Linux 的 Windows 子系统）之间进行转换的指南](./mac-to-windows.md)。 它可以帮助确定以下项之间的区别：

- [键盘快捷方式](./mac-to-windows.md#keyboard-shortcuts)
- [触控板快捷方式](./mac-to-windows.md#trackpad-shortcuts)
- [终端和 shell 工具](./mac-to-windows.md#command-line-shells-and-terminals)
- [应用和实用程序](./mac-to-windows.md#apps-and-utilities)

![Office 图像](../images/flashy-office3.png)

## <a name="additional-resources"></a>其他资源

- [改进工作流的技巧](./tips.md)
- [从 Mac 切换到 Windows 的开发人员的成功案例](./dev-stories.md)
- [热门教程、课程和代码示例](./tutorials.md)
- [Microsoft Game Stack 文档](/gaming/)
