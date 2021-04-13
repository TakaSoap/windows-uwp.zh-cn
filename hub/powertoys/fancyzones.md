---
title: 适用于 Windows 10 的 PowerToys FancyZones 实用工具
description: 用于将窗口排列并对齐到高效布局的窗口管理器实用工具
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4e85193aca4ffda4f863cca7321ca870a08694a3
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107219002"
---
# <a name="fancyzones-utility"></a>FancyZones 实用程序

FancyZones 是一种窗口管理器实用工具，用于将窗口排列和对齐到高效的布局中，以便快速提高工作流和还原布局的速度。 FancyZones 允许用户为要拖动 windows 的目标的桌面定义一组窗口位置。  当用户将窗口拖动到区域时，将调整窗口的大小并重新定位以填充该区域。  

![FancyZones 屏幕截图](../images/pt-fancy-zones2.png)

## <a name="getting-started"></a>入门

### <a name="enable"></a>启用

若要开始使用 FancyZones，需要在 PowerToys 设置中启用该实用工具，然后调用 FancyZones 编辑器 UI。  

### <a name="launch-zones-editor"></a>启动区域编辑器

使用 "PowerToys 设置" 菜单中的按钮或按<kbd>Win</kbd> (启动区域编辑器 + <kbd>`</kbd> ，请注意，可以在 "设置" 对话框) 中更改此快捷方式。  

![FancyZones 设置 UI](../images/pt-fancyzones-settings.png) 

### <a name="elevated-permission-admin-apps"></a>提升的权限管理应用

如果你的应用程序已提升，请在管理员模式下运行，请阅读 [PowerToys 并以管理员身份运行](administrator.md) 以获取详细信息。

## <a name="choose-your-layout-layout-editor"></a>选择布局 (布局编辑器) 

首次启动时，区域编辑器会显示一个布局列表，可根据监视器上的窗口数进行调整。 选择布局会在监视器上显示该布局的预览。 选定布局会自动应用。  

![FancyZones 选取器屏幕快照](../images/pt-fancyzones-picker.png)

如果正在使用多个显示，编辑器将检测可用的监视器并显示它们，以便用户在之间进行选择。 选定的监视器将成为所选布局的目标。

![FancyZones 选取器多个监视器](../images/pt-fancyzones-multimon.png)

### <a name="space-around-zones"></a>区域周围的空间

"在 **区域间显示空间** " 切换功能使你能够确定边框或边距的显示方式将环绕每个 FancyZone 窗口。 " **区域** " 字段周围的空间使你可以为边框的宽度设置自定义值。

**突出显示相邻区域的距离** 使你可以为 FancyZone 窗口之间的空间量设置自定义值，直到它们合并在一起，或者在两者都突出显示之前，使它们合并在一起。

区域编辑器打开后，在更改值后，选中并取消选中 "在 **区域周围显示空间** " 框以查看应用的新值。

![区域周围的 FancyZones 空间屏幕快照](../images/pt-fancyzones-spacearound.png)

### <a name="creating-a-custom-layout"></a>创建自定义布局

区域编辑器还支持创建和保存自定义布局。 选择右下角的 " **+ 创建新布局** " 按钮。
  
可以通过两种方法创建自定义区域布局： **网格** 布局和 **画布** 布局。 它们也可以被视为 subtractive 和附加模型。  

Subtractive **网格** 模型以三列网格开头，并允许通过拆分和合并区域来创建区域，并根据需要调整区域之间的装订线大小。

若要合并两个区域，请选择并按住鼠标左键并拖动鼠标，直到选择第二个区域，然后松开按钮，随即显示一个弹出菜单。

![FancyZones 表编辑器模式](../images/pt-fancyzones-grideditor.png)

添加 **画布** 模型以空白布局开头，并支持添加可进行拖动和调整大小与 windows 相似的区域。

![FancyZones 窗口编辑器模式](../images/pt-fancyzones-canvaseditor.png)

### <a name="quickly-changing-between-layouts"></a>在布局之间快速更改

使用自定义布局，可以将此布局配置为用户定义的热键，以快速将其应用到所需的桌面。 可以通过打开自定义布局的 "编辑" 菜单来设置热键。 设置后，可以通过按绑定来应用自定义布局 `Win ⊞ + Ctrl + Alt + [hotkey]` 。 还可以通过在拖动窗口时按热键来应用布局。

在下面的演示中，我们从应用于屏幕的默认模板开始，为分配热键（共2个）。 然后，使用 `Win ⊞ + Ctrl + Alt + [hotkey]` 绑定应用第一个自定义布局，并将窗口绑定到该布局。 最后，在拖动窗口时应用第二个自定义布局，并将窗口绑定到它。

![FancyZones Quick-Swap 布局](../images/pt-fancyzones-quickswap.gif) 

## <a name="snapping-a-window-to-two-or-more-zones"></a>使窗口与两个或多个区域对齐

如果两个区域相邻，则可以将窗口与它们所在区域的总和对齐 (舍入到包含) 的最小矩形。 当鼠标光标位于两个区域的公共边缘附近时，两个区域同时激活，使您可以将窗口拖放到这两个区域中。  

还可以与任意数量的区域对齐：首先拖动窗口直到激活一个区域，然后按住 `Control` 键的同时拖动窗口以选择多个区域。

若要仅使用键盘将窗口对齐到多个区域，请先打开两个选项 `Override Windows Snap hotkeys (Win+Arrow) to move between zones` 和 `Move windows based on their position` 。 将窗口对齐到一个区域后，使用<kbd>Win</kbd>  +  <kbd>Control</kbd>  +  <kbd>Alt</kbd> + 箭头将窗口展开到多个区域。

![两个区域激活屏幕截图](../images/pt-fancyzones-twozones.png)

## <a name="shortcut-keys"></a>快捷键

| 快捷方式      | 操作 |
| ----------- | ----------- |
| Win ⊞ + \` |Windows 键 + 反撇号 (⊞ + \` ) 启动编辑器 (此快捷方式可在 "设置" 对话框中编辑)  |
| Win ⊞ + 向左键/向右键 | 仅在启用了设置的情况下，在区域之间移动聚焦的窗口 (仅当 `Override Windows Snap hotkeys` 启用了 "设置" 时，才 `Windows ⊞ key + Left Arrow` `Windows key ⊞ + Right Arrow` 会重写和，同时 `Win ⊞ + Up Arrow` `Win ⊞ + Down Arrow` 保持正常运行)  |

FancyZones 不会重写 Windows 10 `Win ⊞ + Shift + Arrow` 以将窗口快速移动到相邻的监视器。

## <a name="settings"></a>设置

| 设置 | 描述 |
| --------- | ------------- |
| 配置区域编辑器热键 | 若要更改默认热键，请单击文本框， (不需要选择或删除文本) 然后按键盘上所需的组合键 |
| 按住 Shift 键以在拖动时激活区域 | 在拖动和手动 snap 模式期间，通过在按住 shift 键的同时按下 shift 键，在自动对齐模式和手动对齐模式间切换，启用对齐 |
| 使用非主鼠标按钮切换区域激活 | 启用此选项后，单击非主鼠标按钮会切换区域激活 |
| 覆盖 Windows Snap 热键 (Win ⊞ + 箭头) 在区域之间移动 | 当此选项处于启用状态并且 FancyZones 正在运行时，它将覆盖两个 Windows Snap 键： `Win ⊞ + Left Arrow` 和 `Win ⊞ + Right Arrow` |
| 根据窗口的位置移动窗口 | 允许使用 Win ⊞ + 向上/向下/向右键根据窗口相对于区域布局的位置来对齐窗口 |
| 跨所有监视器跨区域之间移动窗口 | 当此选项处于关闭状态时，Win ⊞ + 箭头的步进将通过当前监视器上的区域循环窗口，当处于开启状态时，它将通过所有监视器上的所有区域循环窗口 |
| 当屏幕分辨率发生变化时，将窗口保留在其区域中 | 屏幕分辨率发生更改后，如果启用此设置，FancyZones 将调整 windows 的大小并将其重定位到前面的区域 |
| 在区域布局更改过程中，分配到区域的 windows 将与新大小/位置匹配 | 如果启用此选项，FancyZones 将通过维护每个窗口的以前区域编号位置来调整窗口的大小并将其放置到新的区域布局中 |
| 将新创建的窗口移到最后一个已知区域 | 自动将新打开的窗口移到应用程序所在的最后一个区域位置 |
| 将新创建的窗口移到当前活动监视器 [实验] | 如果选择了此选项，并且 "将新创建的 windows 移动到最后一个已知区域" 处于关闭状态，或者应用程序没有上一个已知区域，则会将应用程序保留在当前活动监视器上 |
| 在 unsnapping 时还原 windows 的原始大小 | 如果启用此选项，则 unsnapping 在快照之前，窗口将还原其大小 |
| 在多监视器环境中启动编辑器时跟随鼠标光标而不是焦点 | 当此选项处于打开状态时，编辑器热键将在显示器上的鼠标光标所在位置启动，当此选项为 off 时，编辑器热键将在当前活动窗口所在的监视器上启动编辑器  |
| 拖动窗口时在所有监视器上显示区域 | 默认情况下，FancyZones 仅显示当前监视器上可用的区域，这项功能在打开时可能会影响性能 |
| 允许区域跨监视器 (所有监视器必须具有相同的 DPI 缩放)  | 此选项允许将所有连接的监视器视为一个大屏幕。 若要正常工作，要求所有监视器都具有相同的 DPI 缩放比例 |
| 使拖动的窗口透明 | 当区域被激活时，所拖动的窗口将变为透明，以改善区域可见性 |
| 区域突出显示颜色 (默认 #008CFF)  | 在窗口拖动过程中，区域成为活动放置目标的区域的颜色 |
| 区域非活动颜色 (默认 #F5FCFF)  | 在窗口拖动过程中，区域变为非活动状态时的颜色 |
| 区域边框颜色 (默认 #FFFFFF)  | 活动区域和非活动区域的边框颜色 |
| 区域不透明度 (% )  (默认 50% )  | 活动区域和非活动区域的不透明度百分比 |
| 排除应用程序与区域的对齐 | 添加应用程序名称或名称的一部分， (例如，添加 `Notepad` 将匹配 `Notepad.exe` 和 `Notepad++.exe` ，以仅匹配 `Notepad.exe` 添加 `.exe` 扩展)  |

![FancyZones 设置靠下屏幕截图](../images/pt-fancyzones-settings2.png)
