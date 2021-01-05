---
title: WinUI 2.5 发行说明
description: WinUI 2.5（包括新功能和 Bug 修复）的发行说明。
ms.date: 12/01/2020
ms.topic: reference
ms.openlocfilehash: 1fe64ea547b3e8966ea0ec085e77225a3a1895df
ms.sourcegitcommit: e252ccb4a76c4883c7073083d73007957202b84d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97657664"
---
# <a name="windows-ui-library-25"></a>Windows UI 库 2.5

WinUI 2.5 是 Windows UI 库 (WinUI) 的最新官方版本。

WinUI 是托管在 GitHub 的 [Windows UI 库存储库](https://aka.ms/winui)中的开源项目。 请在此存储库中注册所有 Bug 报告、提交功能请求和贡献社区代码。

WinUI 版本：[GitHub 发布页](https://github.com/microsoft/microsoft-ui-xaml/releases)

可以通过 NuGet 包管理器将 WinUI 包添加到 Visual Studio 项目中。 有关详细信息，请参阅 [Windows UI 库入门](../getting-started.md)。

NuGet 包下载：[Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>新功能

### <a name="infobar"></a>InfoBar

[InfoBar](/windows/uwp/design/controls-and-patterns/infobar) 控件用于显示对用户高度可见的应用范围内的状态消息，但这些消息也不具有侵入性。 该控件包含用于指示所显示消息类型的 Severity 属性，以及用于指定你自己的行动号召或超链接按钮的选项。 由于 InfoBar 与其他 UI 内容内联，你还可以指定控件是始终可见还是可以由用户关闭。

此示例显示了默认状态的 InfoBar，其中有一个关闭按钮和消息。

:::image type="content" source="../images/infobar-default-title-message.png" alt-text="带有关闭按钮和消息的默认状态的 InfoBar 示例。":::

此动画示例显示了包含各种严重性状态和自定义消息的 InfoBar。

:::image type="content" source="../images/infobar-severity-animated.gif" alt-text="InfoBar 严重性状态和自定义消息的动画示例。":::

[使用准则](/windows/uwp/design/controls-and-patterns/infobar)

[API 参考](/windows/winui/api/microsoft.ui.xaml.controls.infobar)

### <a name="determinate-progressring"></a>确定的 ProgressRing

[ProgressRing](/windows/uwp/design/controls-and-patterns/progress-controls) 的确定状态显示任务完成的百分比。 在已知持续时间且操作进度不应阻止用户与应用交互的操作期间，应使用此控件。

以下动画图像演示了确定的 ProgressRing 控件。

:::image type="content" source="../images/progressring-determinate-mode-animated.gif" alt-text="确定的 ProgressRing 控件的动画示例。":::<br>

[使用准则](/windows/uwp/design/controls-and-patterns/progress-controls#progress-controls-best-practices)

[API 参考](/windows/winui/api/microsoft.ui.xaml.controls.progressring)


### <a name="navigationview-footermenuitems"></a>NavigationView FooterMenuItems

使用 NavigationView 控件的 FooterMenuItems 属性将导航项放置在导航窗格的末尾（与将项目放置在窗格的开头的 MenuItems 属性对比）。

下图显示了页脚菜单中包含“帐户”、“购物车”和“帮助”导航项的 NavigationView  。

:::image type="content" source="../images/navigationview-footermenuitems.png" alt-text="页脚菜单中包含“帐户”、“购物车”和“帮助”导航项的 NavigationView 示例。":::

[使用准则](/windows/uwp/design/controls-and-patterns/navigationview?#footer-menu-items)

[API 参考](/windows/winui/api/microsoft.ui.xaml.controls.navigationview.footermenuitems)

## <a name="samples"></a>示例

“XAML 控件库”示例应用包括这些 WinUI 功能和控件的示例。

如果已安装“XAML 控件库”应用并将其更新到最新版本：

- 请参阅操作中的 [InfoBar](xamlcontrolsgallery:/item/InfoBar)。
- 请参阅操作中的 [ProgressRing](xamlcontrolsgallery:/item/ProgressRing)。
- 请参阅操作中的 [NavigationView](xamlcontrolsgallery:/item/NavigationView)。

如果尚未安装 XAML 控件库应用，请从 [Microsoft Store](https://aka.ms/xamlgalleryapp) 获取它。

还可以在 [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) 中查看、克隆和生成 XAML 控件库源代码。

## <a name="other-updates"></a>其他更新

请参阅[显著更改](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.5.0)列表，了解本版本中解决的许多 GitHub 问题。
