---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: 启用设备进行开发
description: 了解如何通过在 Visual Studio 中启用开发人员模式来支持将 Windows 10 设备用于开发和调试。
keywords: 入门 开发人员许可证 Visual Studio，开发人员许可证 启用设备
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011844"
---
# <a name="enable-your-device-for-development"></a>启用设备进行开发

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>启用开发人员模式、旁加载应用和使用其他开发人员功能

![启用设备进行开发](images/developer-poster.png)

> [!IMPORTANT]
> 如果不是在自己的电脑上自行创建应用程序，则无需启用开发人员模式。 若要尝试解决计算机上遇到的问题，请查看 [Windows 帮助](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10)。 如果是第一次开发，则还需要下载所需的工具来[进行设置](get-set-up.md)。

如果你正在使用计算机进行一般的日常活动，如玩游戏、进行 Web 浏览、收发电子邮件或使用 Office 应用，则不需要激活开发人员模式，并且实际上不应该激活该模式。 此页面上的其余信息对你来说并不重要，你可以放心地重新执行你正在执行的任何操作。 谢谢拜访！

但是，如果你是首次在计算机上使用 Visual Studio 编写软件，则需要在开发电脑和用于测试代码的所有设备上启用开发人员模式。 如果未启用开发人员模式，则打开 UWP 项目将会打开“面向开发人员”设置页，或导致在 Visual Studio 中出现以下对话框：

![启用在 Visual Studio 中显示的开发人员模式对话框](images/latestenabledialog.png)

当看到此对话框时，请单击“开发人员设置”打开“面向开发人员”设置页 。

> [!NOTE]
> 可以随时转到“面向开发人员”页启用或禁用开发人员模式：只需在任务栏中的 Cortana 搜索框中输入“面向开发人员”。

## <a name="accessing-settings-for-developers"></a>访问面向开发人员的设置

若要启用开发人员模式或使用其他设置：

1.  从“面向开发人员”设置对话框中，选择需要的访问级别。
2.  阅读所选设置的免责声明，然后单击“是”以接受更改。

> [!NOTE]
> 启用开发人员模式需要管理员访问权限。 如果设备为组织所有，此选项可能已禁用。

### <a name="developer-mode-features"></a>“开发人员模式”功能

开发人员模式将替换 Windows 8.1 对于开发人员许可证的要求。  除了旁加载外，开发人员模式设置还支持调试和其他部署选项。 这包括启动 SSH 服务允许部署该设备。 为了停止运行此服务，必须禁用开发人员模式。

在桌面上启用开发人员模式时，会安装功能包，其中包括：
- Windows 设备门户。 仅当“启用设备门户”选项打开时，才会启用设备门户，并为它配置防火墙规则。
- 安装允许远程安装应用的 SSH 服务，并为其配置防火墙规则。 启用“设备发现”会打开 SSH 服务器。

有关这些功能的详细信息，或者如果在安装过程中遇到问题，请查看 [开发人员模式功能和调试](developer-mode-features-and-debugging.md)。

## <a name="see-also"></a>另请参阅

* [注册 Windows 帐户](sign-up.md)
* [开发人员模式功能和调试](developer-mode-features-and-debugging.md)。