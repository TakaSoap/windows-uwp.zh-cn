---
description: 了解如何为经过打包的 .NET 和本机 C++/Win32 应用获取特定类型的激活信息
title: 获取打包应用的激活信息
ms.date: 09/17/2020
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 748646ce335e9e68ee7a22131ebd305db94e9801
ms.sourcegitcommit: 5d7168ebc9f43aa13051446aff45a46600e6aafe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90799844"
---
# <a name="get-activation-info-for-packaged-apps"></a>获取打包应用的激活信息

从 Windows 10 1809 版开始，经过打包的桌面应用可以调用 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法，在启动过程中检索某些类型的应用激活信息。 例如，可以调用此方法获取通过打开文件、单击交互式 toast 或使用协议激活应用的相关信息。 从 Windows 10 2004 版开始，使用[稀疏包](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps)的应用中也支持此功能。

> [!NOTE]
> 除了按本文所述使用 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法检索特定类型的激活信息外，还可以通过定义 COM 类来检索后台任务的激活信息。 有关详细信息，请参阅[创建和注册 winmain COM 后台任务](/windows/uwp/launch-resume/create-and-register-a-winmain-background-task)。

## <a name="code-example"></a>代码示例

下面的代码示例演示如何通过 Windows 窗体应用中的 Main 函数调用 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法。 对于应用支持的每个激活类型，将 `args` 返回值强制转换为相应的事件 args 类型。 此代码示例假定 `Handlexxx` 方法是在其他地方定义的专用激活处理程序代码。

```csharp
static void Main()
{
    Application.EnableVisualStyles();
    Application.SetCompatibleTextRenderingDefault(false);

    var args = AppInstance.GetActivatedEventArgs();
    switch (args.Kind)
    {
        case ActivationKind.Launch:
            HandleLaunch(args as LaunchActivatedEventArgs);
            break;
        case ActivationKind.ToastNotification:
            HandleToastNotification(args as ToastNotificationActivatedEventArgs);
            break;
        case ActivationKind.VoiceCommand:
            HandleVoiceCommand(args as VoiceCommandActivatedEventArgs);
            break;
        case ActivationKind.File:
            HandleFile(args as FileActivatedEventArgs);
            break;
        case ActivationKind.Protocol:
            HandleProtocol(args as ProtocolActivatedEventArgs);
            break;
        case ActivationKind.StartupTask:
            HandleStartupTask(args as StartupTaskActivatedEventArgs);
            break;
        default:
            HandleLaunch(null);
            break;
    }
```

## <a name="supported-activation-types"></a>支持的激活类型

可以使用 [AppInstance.GetActivatedEventArgs](/uwp/api/windows.applicationmodel.appinstance.getactivatedeventargs) 方法从下表中列出的一组受支持的事件 args 对象中检索激活信息。 其中一些激活类型要求使用程序包清单中的一个程序包扩展。

仅 Windows 10 2004 版及更高版本支持 [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) 激活信息。 Windows 10 1809 版及更高版本支持所有其他激活信息类型。

| 事件 args 类型 | 程序包扩展 | 相关文档 | 
|-------------------|-----------------|-----------------------|
| [ShareTargetActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.sharetargetactivatedeventargs) | [uap:ShareTarget](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-sharetarget) | [使桌面应用程序成为共享目标](/windows/apps/desktop/modernize/desktop-to-uwp-extend#making-your-desktop-application-a-share-target) |
| [ProtocolActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.protocolactivatedeventargs) | [uap:Protocol](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol) | [使用协议启动应用程序](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-your-application-by-using-a-protocol) |
| [ToastNotificationActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.toastnotificationactivatedeventarg) | desktop:ToastNotificationActivation | [来自桌面应用的 Toast 通知](/windows/uwp/design/shell/tiles-and-notifications/toast-desktop-apps)。 |
| [StartupTaskActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.startuptaskactivatedeventargs)  | desktop:StartupTask | [用户登录 Windows 时启动可执行文件](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#start-an-executable-file-when-users-log-into-windows) |
| [FileActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.fileactivatedeventargs) | [uap:FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) | [将打包的应用程序与一组文件类型相关联](/windows/apps/desktop/modernize/desktop-to-uwp-extensions#associate-your-packaged-application-with-a-set-of-file-types) |
| [VoiceCommandActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.voicecommandactivatedeventargs) | 无 | [处理激活和执行语音命令](/cortana/voice-commands/launch-a-foreground-app-with-voice-commands-in-cortana) |
| [LaunchActivatedEventArgs](/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs) | 无 |  |
