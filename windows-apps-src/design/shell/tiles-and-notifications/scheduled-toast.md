---
Description: 了解如何计划稍后显示本地 toast 通知。
title: 计划 toast 通知
label: Schedule a toast notification
template: detail.hbs
ms.date: 04/09/2020
ms.topic: article
keywords: windows 10，uwp，计划 toast 通知，scheduledtoastnotification，如何，快速入门，入门，代码示例，演练
ms.localizationpriority: medium
ms.openlocfilehash: 04bbf3da388bf065b2b96684cf3f27cd7534ff51
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "91984733"
---
# <a name="schedule-a-toast-notification"></a>计划 toast 通知

计划 toast 通知允许您将通知计划为稍后显示，而不管应用程序是否正在运行。 这适用于显示提醒或用户的其他后续任务的方案，其中通知的时间和内容是提前知道的。

请注意，计划 toast 通知的传递时段为5分钟。 如果计算机在计划的传递时间内处于关闭状态，并且保持不变超过5分钟，则通知将被 "删除"，因此不再与用户相关。 如果需要有保证的通知送达，而不考虑计算机的持续时间，我们建议使用带有时间触发器的后台任务，如 [此代码示例](https://github.com/WindowsNotifications/quickstart-snoozable-toasts-even-if-computer-is-off)中所示。

> [!IMPORTANT]
> Win32 应用程序 (.MSIX/稀疏包和经典 Win32) 用于发送通知和处理激活的步骤略有不同。 请按照下面的说明进行操作，但 `ToastNotificationManager` 将替换为 `DesktopNotificationManagerCompat` [Win32 应用](toast-desktop-apps.md) 文档中的类。

> **重要 api**： [ScheduledToastNotification 类](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)


## <a name="prerequisites"></a>先决条件

若要完全理解此主题，事先掌握以下内容会很有用...

* Toast 通知术语和概念的应用知识。 有关详细信息，请参阅 [Toast 和操作中心概述](/archive/blogs/tiles_and_toasts/toast-notification-and-action-center-overview-for-windows-10)。
* 熟悉 Windows 10 toast 通知内容。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。
* Windows 10 UWP 应用项目


## <a name="step-1-install-nuget-package"></a>步骤1：安装 NuGet 包

安装 "..." [NuGet 包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。 我们的代码示例将使用此包。 本文结束时，我们会提供不使用任何 NuGet 包的 "纯" 代码片段。 使用此包可以创建 toast 通知，而无需使用 XML。


## <a name="step-2-add-namespace-declarations"></a>步骤2：添加命名空间声明

`Windows.UI.Notifications` 包括 toast Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-schedule-the-notification"></a>步骤3：计划通知

我们将使用一个简单的基于文本的通知来提醒学生当天到期的家庭作业。 构造通知和计划！

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("itemsDueToday", ToastActivationType.Foreground)
    .AddText("ASTR 170B1")
    .AddText("You have 3 items due today!");
    .GetToastContent();

    
// Create the scheduled notification
var toast = new ScheduledToastNotification(content.GetXml(), DateTime.Now.AddSeconds(5));


// Add your scheduled toast to the schedule
ToastNotificationManager.CreateToastNotifier().AddToSchedule(toast);
```


## <a name="provide-a-primary-key-for-your-toast"></a>为 toast 提供主键

如果要以编程方式取消、删除或替换计划的通知，则需要使用 Tag 属性 (，并选择性地) 组属性，以提供通知的主密钥。 然后，你可以在将来使用此主密钥来取消、删除或替换通知。

要查看有关替换/删除已发送的 toast 通知的更多详细信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 和 Group 组合充当复合主键。 Group 是两者中较为通用的标识符，你可以用它来分配如“wallPosts”、“messages”、“friendRequests”等组。而 Tag 应该唯一标识组中的通知本身。 使用通用组时，可以使用 [RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 删除该组中的所有通知。

```csharp
toast.Tag = "18365";
toast.Group = "ASTR 170B1";
```


## <a name="cancel-scheduled-notifications"></a>取消预定通知

若要取消计划的通知，必须首先检索所有计划的通知的列表。

然后，找到与标记 (匹配的计划 toast，还可以选择) 之前指定的组，然后调用 RemoveFromSchedule ( # A3。

```csharp
// Create the toast notifier
ToastNotifier notifier = ToastNotificationManager.CreateToastNotifier();

// Get the list of scheduled toasts that haven't appeared yet
IReadOnlyList<ScheduledToastNotification> scheduledToasts = notifier.GetScheduledToastNotifications();

// Find our scheduled toast we want to cancel
var toRemove = scheduledToasts.FirstOrDefault(i => i.Tag == "18365" && i.Group == "ASTR 170B1");
if (toRemove != null)
{
    // And remove it from the schedule
    notifier.RemoveFromSchedule(toRemove);
}
```


## <a name="activation-handling"></a>激活处理

若要详细了解如何处理激活，请参阅 [发送本地 toast](send-local-toast.md) 文档。 激活计划 toast 通知的方式与激活本地 toast 通知相同。


## <a name="adding-actions-inputs-and-more"></a>添加操作、输入和其他内容

若要详细了解操作和输入等高级主题，请参阅 [发送本地 toast](send-local-toast.md) 文档。 操作和输入在本地 toast 中的工作方式与在计划 toast 中的工作方式相同。


## <a name="resources"></a>资源

* [toast 内容文档](adaptive-interactive-toasts.md)
* [ScheduledToastNotification 类](/uwp/api/Windows.UI.Notifications.ScheduledToastNotification)