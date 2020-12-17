---
title: 适用于 Windows 10 的 PowerToys 管理员模式
description: 要使 PowerToys 能够使用在提升的管理员模式下运行的应用，还必须在管理员模式下运行 PowerToys。
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2eb514c25c135f8d58641232523a32f5d35153d4
ms.sourcegitcommit: 46a7e9db64e17a645ee6e888f62a9b04632c56af
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97618536"
---
# <a name="powertoys-running-with-administrator-elevated-permissions"></a>PowerToys 以管理员提升的权限运行

如果以管理员身份运行任何应用程序 (也称为提升的权限) ，则在提升的应用程序处于焦点或尝试与 FancyZones 之类的 PowerToys 功能进行交互时，PowerToys 可能会无法正常工作。 还可以通过以管理员身份运行 PowerToys 来解决此问题。

## <a name="options"></a>选项

PowerToys 的两个选项可用于支持以管理员身份运行的应用程序 () 提升的权限：

- **[推荐]**：当检测到提升的进程时，PowerToys 将显示一个提示。 打开 **PowerToys 设置**。 在 " **常规** " 选项卡中，选择 "以管理员身份重新启动"。

- 在 **PowerToys 设置** 中启用 "始终以管理员身份运行"。

## <a name="run-as-administrator-elevated-processes-explained"></a>以管理员身份运行的已提升进程

默认情况下，Windows 应用程序在 **用户模式下** 运行。 若要在 **管理模式下** 运行应用程序或以 *提升的进程* 运行，则表示应用将使用对操作系统的其他访问权限运行。

在管理模式下运行应用程序或程序的最简单方法是右键单击程序，然后选择 "以 **管理员身份运行**"。 如果当前用户不是管理员，则 Windows 将要求提供管理员用户名和密码。

大多数应用程序不需要以提升的权限运行。 但需要管理员权限的常见方案是运行某些 PowerShell 命令或编辑注册表。

如果 (用户访问控制提示) 出现此提示，则该应用程序将请求管理员级别提升的权限：

![Windows 提升权限提示屏幕截图](../images/pt-admin-prompt.png)

在提升的命令行的情况下，通常会在标题栏中追加标题 "管理员"。

![Windows 管理员命令行屏幕截图](../images/pt-admin-terminal.png)

## <a name="support-for-admin-mode-with-powertoys"></a>支持具有 PowerToys 的管理模式

与在管理员模式下运行的其他应用程序交互时，PowerToys 只需要提升的管理员权限。 如果这些应用程序处于焦点，则 PowerToys 可能不起作用，除非它已提升。

下面是我们将无法使用的两种方案：

- 截获某些类型的键盘笔划
- 调整大小/移动窗口

### <a name="affected-powertoys-utilities"></a>受影响的 PowerToys 实用工具

在以下情况下，可能需要管理员模式权限：

- FancyZones
  - 将提升的窗口 (例如，命令提示符) 到别致的区域
  - 将提升的窗口移到另一个区域
- 快捷方式指南
  - 显示快捷方式
- 键盘 remapper
  - 密钥到密钥的重新映射
  - 全局级别快捷方式重映射
  - 应用目标快捷方式重映射
- PowerToys 运行
  - 显示快捷方式
