---
title: 适用于 Windows 10 的 PowerToys 视频会议静音实用工具
description: 一种实用工具，使用户能够通过单次击键将麦克风 (音频) 上，并关闭相机 (视频) ，而不管应用程序在计算机上有哪些焦点。
ms.date: 12/07/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e9a7ec901ffc31e565adb94a1a043fd149064c12
ms.sourcegitcommit: 5dac88ad541b71ebe85b78951e6b357a3db176cc
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97611982"
---
# <a name="video-conference-mute-preview"></a>视频会议静音 (预览) 

> [!IMPORTANT]
> 这是一项预览功能，仅包含在 PowerToys 的预发行版本中。 运行此预发行版需要 Windows 10 版本 1903 (版本 18362) 或更高版本。

将麦克风 (音频) 上，并通过单次击键将相机 (视频) ，无论哪个应用程序在你的计算机上有焦点。

## <a name="usage"></a>使用情况

使用视频会议静音的默认设置为：

- <kbd>⊞ Win</kbd> +<kbd>N</kbd>若要同时切换音频和视频
- <kbd>⊞ Win</kbd> +<kbd>移位</kbd> +用于切换麦克风<kbd>的</kbd>
- <kbd>⊞ Win</kbd> +<kbd>移位</kbd> +<kbd>O</kbd>用于切换视频

![音频和视频静音通知屏幕截图](../images/pt-video-audio-mute-notification.png)

使用 "麦克风" 和/或 "相机切换" 快捷键时，你将看到一个小工具栏窗口显示，告诉你麦克风和摄像机是否设置为打开、关闭或不使用。 可以在 "PowerToys 设置" 的 "视频会议静音" 选项卡中设置此工具栏的位置。

> [!NOTE]
> 请记住，你必须已安装并运行 [PowerToys 的预发行/实验版](https://github.com/microsoft/PowerToys/releases/) ，并在 PowerToys 设置中启用 "视频会议静音" 功能才能使用此实用工具。

## <a name="settings"></a>设置

PowerToys 设置中的视频会议静音选项卡提供以下选项：

- **快捷方式：** 更改用于将麦克风、照相机或两者组合在一起的快捷键。
- **选择的麦克风：** 选择此实用工具将面向的计算机上的麦克风。
- **所选照相机：** 在计算机上选择此实用程序将面向的相机。
- **相机叠加图像：** 选择一个图像，当照相机关闭时，该图像将用作占位符。 默认情况下，当使用此实用程序) 关闭照相机时，将会显示一个黑色屏幕 (。
- **工具栏：** 确定默认情况) 下切换 (右上角时 *，"麦克风打开* " 工具栏上的 "相机打开" 工具栏。
- **显示工具栏：** 选择是否希望在主监视器上只显示工具栏 (默认) 或所有监视器上。
- **照相机和麦克风都 unmuted 时隐藏工具栏**：可以使用一个复选框来切换此选项。

![PowerToys 设置中的视频会议静音选项](../images/pt-video-conference-mute-settings.png)

## <a name="how-does-this-work-under-the-hood"></a>这种情况下的工作原理

应用程序以不同的方式与音频和视频交互。

如果照相机停止工作，使用它的应用程序往往不会恢复，直到 API 进行完整重置。 在应用程序中使用照相机时，若要开启和关闭全局隐私相机，通常会崩溃且不会恢复。

那么，PowerToys 如何处理这种情况，以便能够保持流式传输？

- **音频：** PowerToys 使用 Windows 中的全局麦克风静音 API。  当切换到打开和关闭时，应用应恢复。
- **视频：** PowerToys 具有照相机的虚拟驱动程序。 视频经过驱动程序并返回到应用程序。 选择视频会议静音快捷键会阻止视频进入流式处理，但是应用程序仍会认为它正在接收视频，而视频则会被替换为黑色或已保存在设置中的图像占位符。

### <a name="debug-the-camera-driver"></a>调试照相机驱动程序

若要调试照相机驱动程序，请在计算机上查找此文件夹：

```markdown
C:\Windows\ServiceProfiles\LocalService\AppData\Local\Temp\PowerToysVideoConference.log
```

你还可以 `PowerToysVideoConferenceVerbose.flag` 在同一目录中创建一个空的，以便在驱动程序中启用详细日志记录模式。

## <a name="known-issues"></a>已知问题

若要查看当前在视频会议静音实用程序上打开的所有已知问题，请参阅 [GitHub 上的 PowerToys 跟踪问题 #6246](https://github.com/microsoft/PowerToys/issues/6246)。 PowerToys 开发团队和参与者社区正在努力解决这些问题，并使实用程序在预发布期间保持有效，直到解决了基本问题。

![显示5个参与者和设备设置的 PowerToys 视频会议的屏幕截图](../images/pt-video-conference.png)
