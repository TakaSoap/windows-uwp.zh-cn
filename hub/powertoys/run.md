---
title: 适用于 Windows 10 的 PowerToys 运行实用工具
description: 适用于包含一些附加功能而不影响性能的高级用户的快速启动程序。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9be1d54946ec2286d95dbe7d4518a631efd471e9
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618526"
---
# <a name="powertoys-run-utility"></a>PowerToys 运行实用工具

对于包含某些附加功能而不牺牲性能的高级用户，PowerToys 运行是快速启动程序。 它是其他插件的开源和模块化。

若要使用 PowerToys 运行，请选择 " <kbd>Alt</kbd> + <kbd>Space</kbd> " 并开始键入！

*如果该快捷方式不符合您的需要，别担心，它在设置中是完全可配置的。*

![PowerToys 运行演示打开应用](../images/pt-powerrun-demo.gif)

## <a name="requirements"></a>要求

- Windows 10 版本1903或更高版本
- 安装之后，必须在后台启用并运行 PowerToys，此实用程序才能正常工作。

## <a name="features"></a>功能

PowerToys 运行功能包括：

- 搜索应用程序、文件夹或文件

- 搜索之前称为 [WindowWalker](https://github.com/betsegaw/windowwalker/) 的正在运行的进程 () 

- 具有键盘快捷方式的可单击按钮 (如 " *以管理员身份打开* " 或 " *打开包含文件夹* ") 

- 例如，使用 (调用 Shell 插件 `>` `> Shell:startup` 将打开 Windows 启动文件夹) 

- 使用计算器执行简单计算

## <a name="settings"></a>设置

"PowerToys 设置" 菜单中提供了以下运行选项。

  | **设置** |**操作** |
  | --- | --- |
  | 打开 PowerToys 运行 | 定义用于打开/隐藏 PowerToys 运行的键盘快捷方式 |
  | 在全屏模式下忽略快捷方式 |  处于全屏 (F11) 时，运行不会与快捷方式结合 |
  | 最大结果数 |  未滚动显示的最大结果数 |
  | 清除上一次启动查询 | 启动后，不会突出显示以前的搜索 |
  | 禁用驱动器检测警告 | 警告，如果所有驱动器未建立索引，将不再可见 |

## <a name="keyboard-shortcuts"></a>键盘快捷方式

  | **快捷方式** | **操作** |
  | --- | --- |
  | Alt + 空格键 | 打开或隐藏 PowerToys 运行 |
  | Esc | 隐藏 PowerToys 运行 |
  | Ctrl+Shift+Enter |  (仅适用于应用程序) 以管理员身份打开所选应用程序 |
  | Ctrl+Shift+E |  (仅适用于应用程序和文件) 在文件资源管理器中打开包含文件夹 |
  | Ctrl+C |  (仅适用于) 复制路径位置的文件夹和文件 |
  | 选项卡 | 导航搜索结果和上下文菜单按钮 |

## <a name="action-key"></a>操作键

这将强制 PowerToys 仅在目标插件中运行。

  | **操作键** | **操作** |
  | --- | --- |
  | `=` | 仅计算器。 示例 `=2+2` |
  | `?` | 仅搜索文件。 `?road`要查找的示例`roadmap.txt` |
  | `.` | 仅安装了应用搜索。 `.code`获取 Visual Studio Code 的示例 |
  | `//` | 仅 Url。 `//docs.microsoft.com`要使默认浏览器继续使用的示例https://docs.microsoft.com |
  | `<` | 仅运行进程。 `<outlook`查找包含 outlook 的所有进程的示例 |
  | `>` | 仅 Shell 命令。 `>ping localhost`执行 ping 查询的示例 |

## <a name="indexer-settings"></a>索引器设置

如果索引器设置未设置为涵盖所有驱动器，您将收到以下警告：

![PowerToys 运行索引器警告](../images/pt-run-warning.png)

你可以在 "PowerToys" 设置中关闭该警告，或选择警告来展开正在索引的驱动器。 选择警告后，将打开 "Windows 10 设置" "搜索窗口" 选项。

![索引设置](../images/pt-run-indexing.png)

在此 "搜索窗口" 菜单中，可以：

- 选择 "增强" 模式，在 Windows 10 计算机上的所有驱动器中启用索引。
- 指定要排除的文件夹路径。
- 选择菜单选项底部附近的 "高级搜索索引器设置" () 设置高级索引设置、添加或删除搜索位置、为加密文件编制索引等。

![高级索引设置](../images/pt-run-indexing-advanced.png)

## <a name="known-issues"></a>已知问题

有关所有已知问题和建议的列表，请参阅 [GitHub 上的 PowerToys 产品](https://github.com/microsoft/PowerToys/issues?q=is%3Aopen+is%3Aissue+label%3AProduct-Launcher)存储库问题。

## <a name="attribution"></a>Attribution

- [Wox](https://github.com/Wox-launcher/Wox/)

- [Beta Tadele 的窗口查看者](https://github.com/betsegaw/windowwalker)
