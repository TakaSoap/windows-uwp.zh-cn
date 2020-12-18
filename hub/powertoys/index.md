---
title: Microsoft PowerToys
description: Microsoft PowerToys 是一套用于自定义 Windows 10 的实用工具。 其中包括颜色选取器（单击任何位置即可抓取颜色值）、FancyZones（用于在网格布局中放置窗口的快捷键）、文件资源浏览器加载项（预览 SVG 或 Markdown 文件）、图像大小调整器（简单右键单击一下即可调整一张或多张图像的大小）、键盘管理器（重新映射键或创建自己的快捷键）、PowerRename（使用搜索和替换进行批量重命名）、PowerToys Run（使用 Alt + 空格来启动应用）、快捷键指南，还有很多即将提供。
ms.date: 12/02/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 1f441c7ef9fc4268b35c041f1100cb7f116318a5
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97611827"
---
# <a name="microsoft-powertoys-utilities-to-customize-windows-10"></a>Microsoft PowerToys：用于自定义 Windows 10 的实用工具

Microsoft PowerToys 是一组实用工具，可帮助高级用户调整和简化其 Windows 10 体验，从而提高工作效率。

> [!div class="nextstepaction"]
> [安装 PowerToys](install.md)

## <a name="processor-support"></a>处理器支持

- **x64**：支持
- **x86**：正在开发中（请参见[问题 #602](https://github.com/microsoft/PowerToys/issues/602)）
- **ARM**：正在开发中（请参见[问题 #490](https://github.com/microsoft/PowerToys/issues/490)）

## <a name="current-powertoy-utilities"></a>当前 PowerToy 实用工具

当前可用的实用工具包括：

### <a name="color-picker"></a>颜色选取器

:::row:::
    :::column:::
        [![颜色选取器屏幕截图](../images/pt-color-picker.png)](color-picker.md)
    :::column-end:::
    :::column span="2":::
        [颜色选取器](color-picker.md)是一种系统范围的颜色选取实用工具，通过 <kbd>Win</kbd>+<kbd>Shift</kbd>+<kbd>C</kbd> 进行激活。 从当前正在运行的任何应用程序中选取颜色，然后选取器会自动将颜色按可配置的格式复制到剪贴板中。 颜色选取器还包含一个编辑器，其中显示了之前选取的颜色的历史记录，你可用它来微调所选颜色并复制不同的字符串表示形式。 该代码基于[马丁·克尔赞的颜色选取器](https://github.com/martinchrzan/ColorPicker)。
    :::column-end:::
:::row-end:::

### <a name="fancy-zones"></a>FancyZones

:::row:::
    :::column:::
        [![FancyZones 屏幕截图](../images/pt-fancy-zones.png)](fancyzones.md)
    :::column-end:::
    :::column span="2":::
        [FancyZones](fancyzones.md) 是一种窗口管理器，可用于轻松创建复杂的窗口布局，并将窗口快速放入到这些布局中。
    :::column-end:::
:::row-end:::

### <a name="file-explorer-add-ons"></a>文件资源浏览器加载项

:::row:::
    :::column:::
        [![文件资源浏览器屏幕截图](../images/pt-file-explorer.png)](file-explorer.md)
    :::column-end:::
    :::column span="2":::
        通过[文件资源浏览器](file-explorer.md)加载项，可在文件资源浏览器中实现预览窗口呈现，从而显示 SVG 图标 (.svg) 和 Markdown (.md) 文件预览。 若要启用预览窗格，请文件资源浏览器中选择“视图”选项卡，然后选择“预览窗格”。
    :::column-end:::
:::row-end:::

### <a name="image-resizer"></a>图像大小调整器

:::row:::
    :::column:::
        [![图像大小调整器屏幕截图](../images/pt-image-resizer.png)](image-resizer.md)
    :::column-end:::
    :::column span="2":::
        [图像大小调整器](image-resizer.md)是一种用于快速调整图像大小的 Windows Shell 扩展。  只需在文件资源浏览器中简单右键单击一下，立即就能调整一张或多张图像的大小。 此代码基于 [Brice Lambson 的图像大小调整器](https://github.com/bricelam/ImageResizer)。
    :::column-end:::
:::row-end:::

### <a name="keyboard-manager"></a>键盘管理器

:::row:::
    :::column:::
        [![键盘管理器屏幕截图](../images/pt-keyboard-manager.png)](keyboard-manager.md)
    :::column-end:::
    :::column span="2":::
        通过[键盘管理器](keyboard-manager.md)，可重新映射键和创建自己的键盘快捷方式，从而自定义键盘来提高工作效率。 此版 PowerToy 需要使用 Windows 10 1903（版本 18362）或更高版本。
    :::column-end:::
:::row-end:::

### <a name="powerrename"></a>PowerRename

:::row:::
    :::column:::
        [![PowerRename 屏幕截图](../images/pt-rename.png)](powerrename.md)
    :::column-end:::
    :::column span="2":::
        通过 [PowerRename](powerrename.md)，可执行批量重命名，搜索和替换文件名称。 它附带高级功能，例如使用正则表达式、面向特定文件类型、预览预期结果和撤消更改的能力。 此代码基于 [Chris Davis 的 SmartRename](https://github.com/chrdavis/SmartRename)。
    :::column-end:::
:::row-end:::

### <a name="powertoys-run"></a>PowerToys Run

:::row:::
    :::column:::
        [![PowerToys Run 屏幕截图](../images/pt-run.png)](run.md)
    :::column-end:::
    :::column span="2":::
        [PowerToys Run](run.md) 可帮助你搜索应用并立即启动它 - 只需按快捷键 <kbd>Alt</kbd>+<kbd>空格</kbd>，然后开始键入即可。 对其他插件来说，它是开源和模块化的。 现在还包含窗口切换器。 此版 PowerToy 需要使用 Windows 10 1903（版本 18362）或更高版本。
    :::column-end:::
:::row-end:::

### <a name="shortcut-guide"></a>快捷键指南

:::row:::
    :::column:::
        [![快捷键指南屏幕截图](../images/pt-shortcut-guide.png)](shortcut-guide.md)
    :::column-end:::
    :::column span="2":::
        当用户按下 Windows 键超过 1 秒时，会出现 [Windows 键快捷键指南](shortcut-guide.md)，它还显示桌面当前状态的可用快捷方式。
    :::column-end:::
:::row-end:::

## <a name="powertoys-video-walk-through"></a>PowerToys 视频演练

在该视频中，PowerToys 的项目经理 Clint Rutkas 演示了如何安装和使用各种提供的实用工具，还分享了一些提示，介绍了如何参与等等。

> [!VIDEO https://channel9.msdn.com/Shows/Tabs-vs-Spaces/PowerToys-Utilities-to-customize-Windows-10/player?format=ny]

## <a name="future-powertoy-utilities"></a>今后的 PowerToy 实用工具

### <a name="experimental-powertoys"></a>实验性 PowerToys

安装 PowerToys 的预发行试验版，试用最新的实验性实用工具，包括：

#### <a name="video-conference-mute-experimental"></a>视频会议静音（实验性）

:::row:::
    :::column:::
        [![视频会议静音屏幕截图](../images/pt-video-conference-mute.png)](video-conference-mute.md)
    :::column-end:::
    :::column span="2":::
        [视频会议静音](video-conference-mute.md)是在会议通话期间使用 <kbd>⊞ Win</kbd>+<kbd>N</kbd> 对麦克风和相机“全局”静音的一种快捷方式，它不考虑当前聚焦在哪个应用程序上。 此功能仅包含在 [PowerToys 的预发行版/实验版](https://github.com/microsoft/PowerToys/releases/)中，且需要 Windows 10 1903（版本 18362）或更高版本。
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>已知问题

请访问 GitHub 的 PowerToys 存储库，在[问题](https://github.com/microsoft/PowerToys/issues)选项卡中搜索已知问题或提交新问题。

## <a name="contribute-to-powertoys-open-source"></a>参与 PowerToys（开源）

欢迎你参与 PowerToys！ PowerToys 开发团队很高兴与高级用户社区合作，共同构建可帮助用户充分利用 Windows 的工具。 可通过多种方式进行参与：

- 编写[技术规范](https://codeburst.io/on-writing-tech-specs-6404c9791159)
- 提交[设计概念或建议](https://www.microsoft.com/design/inclusive/)
- [参与编辑文档](https://docs.microsoft.com/contribute/)
- 识别和修复[源代码](https://github.com/microsoft/PowerToys/tree/master/src)中的 bug
- [对新功能和 PowerToy 实用工具编码](https://github.com/microsoft/PowerToys/tree/master/doc/devdocs)

在开始致力于你想要参与的功能之前，请阅读[参与者指南](https://github.com/microsoft/PowerToys/blob/master/CONTRIBUTING.md)。 PowerToys 团队很高兴与你协作，共同找出最佳方法、在功能开发的整个过程中提供指导和辅导，并帮助避免任何浪费精力或重复工作。

## <a name="powertoys-release-notes"></a>PowerToys 发行说明

有关 PowerToys [发行说明](https://github.com/microsoft/PowerToys/releases/)，可查看 GitHub 存储库的安装页面。 若要参考，还可在 PowerToys 维基百科上查找[版本清单](https://github.com/microsoft/PowerToys/wiki/Release-check-list)。

## <a name="powertoys-history"></a>PowerToys 历史记录

灵感来自 [Windows 95 era PowerToys 项目](https://en.wikipedia.org/wiki/Microsoft_PowerToys)，这次重启向高级用户提供了多种方式来使用 Windows 10 shell 提高工作效率并针对各个工作流进行自定义。  可在[此处](https://socket3.wordpress.com/2016/10/22/using-windows-95-powertoys/)找到 Windows 95 PowerToys 的简要概述。

## <a name="powertoys-roadmap"></a>PowerToys 路线图

PowerToys 是一个快速孵化的开源团队，旨在向高级用户提供多种方式来使用 Windows 10 shell 提高工作效率并针对各个工作流进行自定义。 我们将始终如一地检查、重新评估和调整工作优先级，目的是提高用户工作效率。

- [New specs for possible PowerToys](https://github.com/microsoft/PowerToys/wiki/Specs)（适用于可能的 PowerToys 的新规范）
- [积压工作 (backlog) 优先级列表](https://github.com/microsoft/PowerToys/wiki/Roadmap#backlog-priority-list-in-order)
- [1.0 版策略规范](https://github.com/microsoft/PowerToys/wiki/Version-1.0-Strategy)，2020 年 2 月
