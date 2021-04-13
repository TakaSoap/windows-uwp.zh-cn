---
title: 适用于 Windows 10 的 PowerToys 运行实用工具
description: 适用于包含一些附加功能而不影响性能的高级用户的快速启动程序。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d9c67b38f9a7c7729c0f4839a327a2527aa116d
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107218131"
---
# <a name="powertoys-run-utility"></a>PowerToys 运行实用工具

对于包含某些附加功能而不牺牲性能的高级用户，PowerToys 运行是快速启动程序。 对其他插件来说，它是开源和模块化的。

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
  | 禁用驱动器检测警告 | 警告：如果所有驱动器未编制索引，将不再可见。 |

## <a name="keyboard-shortcuts"></a>键盘快捷方式

  | **快捷方式** | **操作** |
  | --- | --- |
  | Alt + 空格键 | 打开或隐藏 PowerToys 运行 |
  | Esc | 隐藏 PowerToys 运行 |
  | Ctrl+Shift+Enter |  (仅适用于应用程序) 以管理员身份打开所选应用程序 |
  | Ctrl+Shift+E |  (仅适用于应用程序和文件) 在文件资源管理器中打开包含文件夹 |
  | Ctrl+C |  (仅适用于) 复制路径位置的文件夹和文件 |
  | 选项卡 | 导航搜索结果和上下文菜单按钮 |

## <a name="action-keys"></a>操作键

这些默认激活短语将强制 PowerToys 仅在目标插件中运行。

  | **操作键** | **操作** |
  | --- | --- |
  | `=` | 仅计算器。 示例 `=2+2` 。 |
  | `?` | 仅搜索文件。 `?road`要查找 `roadmap.txt` 的示例。 |
  | `.` | 仅适用于已安装的程序。 `.code`获取 Visual Studio Code 的示例。 有关向程序的启动添加参数的选项，请参阅 [程序参数](#program-parameters) 。 |
  | `//` | 仅 Url。 `//docs.microsoft.com`要使默认浏览器继续使用的示例 https://docs.microsoft.com 。 |
  | `<` | 仅运行进程。 `<outlook`查找包含 outlook 的所有进程的示例。 |
  | `>` | 仅 Shell 命令。 `>ping localhost`执行 ping 查询的示例。 |
  | `:` | 仅限注册表项。 `:hkcu`搜索 HKEY_CURRENT_USER 注册表项的示例。 |
  | `!` | 仅限 Windows 服务。 `!alg`搜索要启动或停止的应用程序层网关服务的示例。 |
  | `{` | Visual Studio Code 以前打开的工作区、远程计算机 (SSH 或 Codespaces) 和容器。 `{powertoys`用于在其路径中搜索包含 "powertoys" 的工作区的示例。 此插件默认情况下处于关闭状态。

## <a name="system-commands"></a>系统命令

PowerToys 运行启用一组可执行的系统级操作。

  | **操作键**   |   **操作** |
  | ------------------ | ---------------------------------------------------------------------------------|
  | `Shutdown` | 关闭计算机 |
  | `Restart` | 重新启动计算机 |
  | `Sign Out` | 注销当前用户 |
  | `Lock` | 锁定计算机 |
  | `Sleep` | 睡眠计算机 |
  | `Hibernate` | 休眠计算机 |
  | `Empty Recycle Bin` | 清空回收站 |

## <a name="plugin-manager"></a>插件管理器

PowerToys 运行设置菜单包含一个插件管理器，可用于启用/禁用当前可用的各种插件。 通过选择和扩展部分，可以自定义每个插件使用的激活短语。 此外，还可以选择全局结果中是否显示某个插件，还可以设置可用的其他插件选项。 

## <a name="program-parameters"></a>程序参数

PowerToys Run program 插件允许在启动应用程序时添加程序自变量。 程序参数必须符合程序的命令行接口定义的预期格式。

例如，当启动 Visual Studio Code 时，您可以指定要打开的文件夹：

`Visual Studio Code -- C:\myFolder`

Visual Studio Code 还支持一组 [命令行参数](https://code.visualstudio.com/docs/editor/command-line)，这些参数可在 PowerToys 运行到的相应参数中使用，例如，查看文件之间的差异：

`Visual Studio Code -d C:\foo.txt C:\bar.txt` 

如果未选择程序插件的选项 "在全局结果中包含"，请确保在默认情况下包含激活短语 `.` 以调用插件的行为：

`.Visual Studio Code -- C:\myFolder`

## <a name="monitor-positioning"></a>监视定位

如果正在使用多个监视器，则可以通过在 "设置" 菜单中配置相应的启动行为，在所需的监视器上启动 PowerToys 运行。 选项包括打开：

- 主监视器
- 用鼠标光标监视
- 用聚焦窗口监视

![PowerToys 运行监视器选定内容](../images/pt-run-monitor.png)

## <a name="windows-search-settings"></a>Windows 搜索设置

如果未将 Windows Search 插件设置为涵盖所有驱动器，则将收到以下警告：

![PowerToys 运行索引器警告](../images/pt-run-warning.png)

你可以在 "Windows 搜索" 的 "PowerToys 运行插件管理器" 选项中关闭该警告，或选择警告来展开正在索引的驱动器。 选择警告后，将打开 "Windows 10 设置" "搜索窗口" "选项" 菜单。

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
