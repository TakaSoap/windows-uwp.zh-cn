---
title: 适用于 Windows 10 的 PowerToys 颜色选取器实用工具
description: 适用于 Windows 10 的系统范围颜色选取实用程序，使你能够从任何当前正在运行的应用程序中选取颜色，并自动将 HEX 或 RGB 值复制到剪贴板。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 3c76753d455d28dd44045edf9482b3de9ccd97b8
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618566"
---
# <a name="color-picker-utility"></a>颜色选取器实用工具

适用于 Windows 10 的系统范围颜色选取实用程序，使你能够从任何当前正在运行的应用程序中选取颜色，并以可配置格式将其自动复制到剪贴板。

![ColorPicker](../images/pt-colorpicker-hex-editor.png)

## <a name="getting-started"></a>开始使用

### <a name="enable"></a>启用

若要开始使用颜色选取器，需要首先确保在 "PowerToys 设置" (颜色选取器 "部分) 中启用了颜色选取器。

### <a name="activate"></a>激活

启用后，可以选择在启动颜色选取器时执行以下三个行为之一<kbd>，同时激活快捷方式</kbd>按下 + <kbd>Shift</kbd> + <kbd>C</kbd> (请注意，可以在 "设置" 对话框中更改此快捷方式) ：

:::image type="content" source="../images/pt-colorpicker-behaviors.png" alt-text="ColorPicker 行为。":::

- **已启用编辑器模式的颜色选取器** -打开颜色选取器，选择一种颜色后，编辑器将打开，并将所选颜色复制到剪贴板 (默认格式-可在 "设置" 对话框中配置) 。
- **编辑器** -从此处直接打开编辑器，你可以从历史记录中选择一种颜色，微调选定颜色，或者通过打开颜色选取器来捕获一种新颜色。
- **仅限颜色选取器** -仅打开颜色选取器并将所选颜色复制到剪贴板。

### <a name="select-color"></a>选择颜色

激活颜色选取器后，将鼠标光标悬停在要复制的颜色上，并单击鼠标按钮以选择颜色。 若要更详细地查看光标周围的区域，请向上滚动以放大。

复制的颜色将以默认)  (十六进制设置中配置的格式存储在剪贴板中。

![选择颜色](../images/pt-colorpicker.gif)

## <a name="editor-usage"></a>编辑器用法

利用编辑器，您可以查看所选颜色的历史记录 (多达 20) 并以任意预定义的字符串格式复制它们的表示形式。 您可以配置编辑器中显示的颜色格式以及显示的顺序。 可在 PowerToys 设置中找到此配置。

编辑器还允许您微调任何选取的颜色或获取新的类似颜色。 编辑器预览当前选定的颜色2的不同底纹和2个较暗的颜色。

单击任意一种颜色底纹后，会将所选颜色添加到 "颜色历史记录" 列表顶部的 "选取的颜色历史记录" () 。 中间的颜色表示当前从颜色历史记录中选择的颜色。 单击该控件后，将出现微调配置控件，使您可以更改当前颜色的色调或 RGB 值。 按 "确定" 会将新配置的颜色添加到颜色历史记录中。

![ColorPicker 编辑器](../images/pt-colorpicker-editor.gif)

若要从颜色历史记录中删除任何颜色，请右键单击所需的颜色，然后选择 " *删除*"。

## <a name="settings"></a>设置

颜色选取器将允许你更改以下设置：

- 激活快捷方式
- 激活快捷方式的行为
- 复制的颜色的格式 (HEX、RGB 等 ) 
- 编辑器中颜色格式的顺序和外观

![ColorPicker 设置屏幕快照](../images/pt-colorpicker-settings.png)

## <a name="limitations"></a>限制

- 无法在 "开始" 菜单或操作中心顶部显示颜色选取器 (你仍可以选取颜色) 。
- 如果当前聚焦的应用程序是使用管理员提升权限启动 (以管理员身份运行) ，则颜色选取器激活快捷方式将不起作用，除非 PowerToys 也是在管理员提升的情况下启动的。
