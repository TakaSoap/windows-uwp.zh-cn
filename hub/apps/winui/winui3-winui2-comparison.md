---
title: WinUI 3 与 WinUI 2 的比较
description: WinUI 3 与 WinUI 2 的快速比较。
ms.topic: article
ms.date: 03/19/2021
keywords: windows 10, uwp, 工具包 sdk, winui, Windows UI 库
ms.openlocfilehash: 8c415b3c08e0f7cd215cd99f2c54fc3cb760d3e0
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730655"
---
# <a name="comparison-of-winui-3-and-winui-2"></a>WinUI 3 与 WinUI 2 的比较

:::image type="content" source="../images/logo-winui2-vs-winui3.png" alt-text="Win UI 2 与 Win UI 3 徽标":::

目前，Windows UI 库 (WinUI) 有两种独特的版本：WinUI 2 和 WinUI 3。 两种版本都基于最新的 [Fluent 设计系统](https://www.microsoft.com/design/fluent)原理和流程提供用户界面和体验，但各自还具有不同的开发目标和范围，具有不同的开发轨迹和发布计划。

[WinUI 2](winui2/index.md) 和 [WinUI 3](winui3/index.md) 均支持 Windows 10 上生产就绪应用的开发，并且均支持使用 C# 或 C++/Win32 生成应用。

这两种版本均处于活跃开发中，并定期发布更新。

## <a name="the-major-differences"></a>主要区别

| WinUI 3                                                                                                                                                                                                                                                                                                        | WinUI 2                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| UX 堆栈和控件库与 OS 和 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 完全分离，包括 UX 堆栈的核心框架层、组合层和输入层。                                                                        | UX 堆栈和控件库与 OS 和 [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk/) 紧密耦合。                                                                                                                                                                                                                                                                                                                                                          |
| WinUI 3 可用于生成生产就绪的 Windows 桌面/Win32 应用。 | WinUI 2 不能用于生成 Windows 桌面/Win32 应用。 |
| WinUI 3 作为 [Project Reunion](../project-reunion/index.md) 框架包的组件提供，在 Project Reunion Visual Studio Extension (VSIX) 中随附 Visual Studio 项目模板。 | WinUI 2的一部分通过操作系统本身（UWP WinRT API 的 Windows.UI.* 系列）提供，一部分作为一个库（“Windows UI 库 2”）并在操作系统本身已包含内容的基础上附带其他控件、元素和最新样式。 对于 WinUI 2，这些功能以可下载的 NuGet 包的形式提供。 但是，UI 堆栈的其他重要部分仍内置于 OS 中，如核心 XAML 框架层、输入层和组合层。 |
| WinUI 3 支持将 C#/.NET 5 用于桌面应用。 | WinUI 2 仅支持 C#/.NET 原生应用。 |
| WinUI 3 对生产就绪 UWP 应用的支持目前为预览版，请参阅 [WinUI 3 - Project Reunion 0.5 Preview](winui3/release-notes/winui3-project-reunion-0.5-preview.md)。                                                                                                                                | 通过将 NuGet 包安装到新的或现有 UWP 项目中，即可将 WinUI 2 并入生产 UWP 应用。 然后，可以在新应用中直接引用 WinUI 控件和样式，也可以在现有应用中将“windows.ui.” 命名空间引用更新为“microsoft.ui.” 来进行引用。                                                                                                                                                                                    |
| WinUI 3 支持基于 Chromium 的 [WebView2](/microsoft-edge/webview2/) 控件 |  WinUI 2 支持 [WebView](/windows/uwp/design/controls-and-patterns/web-view) 控件 |
| WinUI 3 最低支持 Windows 10 2018 年 10 月更新（版本 1809，OS 内部版本 17763）。 | WinUI 2 最低支持 Windows 10 创意者更新（版本 1703，OS 内部版本 15063）。 |

## <a name="see-also"></a>另请参阅

- [使用 Project Reunion 0.5 构建桌面 Windows 应用](../project-reunion/index.md)

- [Windows UI 库 (WinUI)](index.md)

- [Windows UI 库 2.x](winui2/index.md)

- [Windows UI 库 3 - Project Reunion 0.5（2021 年 3 月）](winui3/index.md)
