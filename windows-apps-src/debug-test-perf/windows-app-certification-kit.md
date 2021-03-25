---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Windows 应用认证工具包
description: 若要为你的应用创造在 Microsoft Store 中发布的最佳机会或令其通过 Windows 认证，请在提交应用进行认证之前先在本地进行验证和测试。 本主题显示了如何安装并运行 Windows 应用认证工具包。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 应用认证
ms.localizationpriority: medium
ms.openlocfilehash: 6f63ad960993ade83bdfa52283a33b76e2d80db2
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804811"
---
# <a name="windows-app-certification-kit"></a>Windows 应用认证工具包

若要让应用[获得 Windows 认证](/windows/win32/win_cert/windows-certification-portal)或准备将其[发布到 Microsoft Store](../publish/app-submissions.md)，应首先在本地进行验证和测试。 本主题演示了如何安装并运行 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)，以确保应用安全有效。

## <a name="prerequisites"></a>必备条件

测试通用 Windows 应用的先决条件：

- 必须安装并运行 Windows 10。
- 必须安装 [Windows 应用认证工具包](https://developer.microsoft.com/windows/downloads/windows-10-sdk/)，它包含在适用于 Windows 10 的 Windows 软件开发工具包 (SDK) 中。
- 必须[启用设备进行开发](/windows/apps/get-started/enable-your-device-for-development)。
- 必须将要测试的 Windows 应用部署到计算机。

> [!NOTE]
> **就地升级：** 安装更高版本的 [Windows 应用认证工具包](https://developer.microsoft.com/windows/develop/app-certification-kit)将替换已安装的所有早期版本的工具包。

## <a name="whats-new"></a>新变化

工具包现在支持 Windows [桌面桥应用](/windows/msix/desktop/source-code-overview)测试。 [Windows 桌面桥应用测试](./windows-desktop-bridge-app-tests.md)可为你的应用提供在 Microsoft Store 中发布或通过认证的最佳机会。

工具包现可集成到没有交互式用户会话的自动测试中。

不再支持应用预启动验证测试。

## <a name="known-issues"></a>已知问题

以下是 Windows 应用认证工具包的已知问题列表：

在测试期间，如果安装程序终止，但活动进程或窗口仍保持运行，则该应用认证工具包可以检测到是否仍存在需要该安装程序来完成的任务。 在此种情况下，该工具包将停止运行“进程安装跟踪文件”任务，并且无法继续处理 UI。

**解决方法：** 在安装程序完成后，手动关闭该安装程序衍生的任何活动进程或窗口。

对于 ARM UWA 或不是面向设备系列桌面或 OneCore 的任何 UWA 应用，最终报告中将出现一条消息，指示“并非所有测试均在验证期间进行。 这可能影响你的 Microsoft Store 提交。”。 如果用户未手动取消测试，将不显示此消息。

**解决方法：** 不适用

对于使用 Windows SDK 版本 10.0.15063 的桌面桥应用，请忽略应用程序清单资源测试中任何标记图像不符合预期尺寸的失败，前提是这些尺寸只相差一个像素。 测试应该具有 +/-1 像素容差。 例如 125% 的小磁贴为 88.75x88.75 像素，如果舍入为 89x89 像素，则不符合 88x88px 的大小限制。

**解决方法：** 不适用

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>以交互方式使用 Windows 应用认证工具包验证 Windows 应用

1. 从“开始”  菜单中，搜索“应用”  、找到“Windows 工具包”  ，然后单击“Windows 应用认证工具包”  。

2. 从“Windows 应用认证工具包”中，选择你希望执行的验证类别。 例如：如果你要验证 Windows 应用，请选择“验证 Windows 应用”  。

    你可能会直接浏览到要测试的应用，或从 UI 的列表选择应用。 首次运行 Windows 应用认证工具包时，UI 将列出已安装在计算机上的所有 Windows 应用。 对于任何后续的运行，UI 将显示已验证的最新 Windows 应用。 如果未列出要测试的应用，则可以单击“我的应用未列出”  来获取系统上安装的所有应用的完整列表。

3. 在已输入或选定要测试的应用后，单击“下一步”  。

4. 在下一屏幕中，你将看到与正在测试的应用类型相对应的测试工作流。 如果列表中的某一测试灰显，则表示该测试不适用于你的环境。 例如，如果你正在 Windows 7 上测试 Windows 10 应用，则只有静态测试才能应用到工作流。 请注意，Microsoft Store 可能会应用此工作流程中的所有测试。 选择要运行的测试，然后单击“下一步”  。

    Windows App 认证工具包开始验证该应用。

5. 测试后，在提示符处输入要保存测试报告的文件夹的位置。

    Windows 应用认证工具包将创建一个 HTML 及一个 XML 报告并将它保存在此文件夹中。

6. 打开报告文件并查看测试结果。

> [!NOTE]
> 如果使用的是 Visual Studio，则在创建应用包时可以运行 Windows 应用认证工具包。 请参阅[打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)以了解操作方法。

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>从命令行使用 Windows 应用认证工具包验证 Windows 应用

> [!IMPORTANT]
> Windows 应用认证工具包必须在活动用户会话的上下文中运行。

1. 在命令窗口中，导航到包含 Windows 应用认证工具包的目录。

    **注意**   默认路径为 C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\。

2. 按此顺序输入以下命令，以测试已安装在你的测试计算机上的应用：

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    或者，你也可以使用以下命令（如果未安装应用）。 Windows App 认证工具包将打开相应程序包，然后应用适当的测试工作流：

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. 在测试完成后，打开名为 `[report file name]` 的报告文件并查看测试结果。

**注意**  可以从某个服务运行 Windows 应用认证工具包，但是该服务必须在活动用户会话内启动工具包过程，并且不得在 Session0 中运行。

**注意**   若要详细了解 Windows 应用认证工具包命令行，请输入 `appcert.exe /?` 命令

## <a name="testing-with-a-low-power-computer"></a>使用低能耗电脑进行测试

Windows 应用认证工具包的性能测试阈值基于低能耗电脑的性能。

执行测试的计算机的特性会影响测试结果。 为了确定你的应用性能是否符合 [Microsoft Store 策略](/legal/windows/agreements/store-policies)，我们建议你在低能耗计算机上测试应用，例如基于 Intel Atom 处理器的计算机，屏幕分辨率为 1366x768（或更高），使用旋转硬盘驱动器（相对于固态硬盘驱动器）。

随着低能耗计算机的发展，其性能特征可能会随时间的推移而改变。 请参考最新的 [Microsoft Store 策略](/legal/windows/agreements/store-policies)，并使用最新版本的 Windows 应用认证工具包测试你的应用，以确保你的应用符合最新性能要求。

## <a name="related-topics"></a>相关主题

- [使用 Windows 应用认证工具包](/windows/win32/win_cert/using-the-windows-app-certification-kit)
- [Windows 桌面应用认证要求](/windows/win32/win_cert/certification-requirements-for-windows-desktop-apps)
- [Windows 应用认证工具包测试](windows-app-certification-kit-tests.md)
- [Microsoft Store 策略](/legal/windows/agreements/store-policies)