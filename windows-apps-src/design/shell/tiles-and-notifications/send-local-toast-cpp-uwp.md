---
description: 了解如何从 c + + UWP 应用程序发送本地 toast 通知，并处理用户单击 toast 的操作。
title: 从 c + + UWP 应用发送本地 toast 通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from a C++ UWP app
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10，uwp，发送 toast 通知，通知，发送通知，toast 通知，操作方法，快速入门，入门，代码示例，演练，cpp，c + +，uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5960d6f2a63ea2aa0aea26392db5958825aebf2b
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804961"
---
# <a name="send-a-local-toast-notification-from-c-uwp-apps"></a>从 c + + UWP 应用发送本地 toast 通知

[!INCLUDE [intro](includes/send-toast-intro.md)]

> [!IMPORTANT]
> 如果正在编写 c + + 非 UWP 应用，请参阅 [c + + WRL](send-local-toast-desktop-cpp-wrl.md) 文档。 如果要编写 c # 应用程序，请参阅 [c # 文档](send-local-toast.md)。



## <a name="step-1-install-nuget-package"></a>步骤1：安装 NuGet 包

[!INCLUDE [nuget package](includes/nuget-package.md)]

我们的代码示例将使用此包。 使用此包可以创建 toast 通知，而无需使用 XML。


## <a name="step-2-add-namespace-declarations"></a>步骤2：添加命名空间声明

```cpp
using namespace Microsoft::Toolkit::Uwp::Notifications;
```


## <a name="step-3-send-a-toast"></a>步骤3：发送 toast

[!INCLUDE [basic toast intro](includes/send-toast-basic-toast-intro.md)]

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)
    ->AddText("Andrew sent you a picture")
    ->AddText("Check this out, The Enchantments in Washington!")
    ->Show();
```


## <a name="step-4-handling-activation"></a>步骤4：处理激活

当用户单击你的通知 (或使用前台激活) 通知上的按钮时，将调用应用 **的** **OnActivated** 。

**App.xaml.cpp**

```cpp
void App::OnActivated(IActivatedEventArgs^ e)
{
    // Handle notification activation
    if (e->Kind == ActivationKind::ToastNotification)
    {
        ToastNotificationActivatedEventArgs^ toastActivationArgs = (ToastNotificationActivatedEventArgs^)e;

        // Obtain the arguments from the notification
        ToastArguments^ args = ToastArguments::Parse(toastActivationArgs->Argument);

        // Obtain any user input (text boxes, menu selections) from the notification
        auto userInput = toastActivationArgs->UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

[!INCLUDE [OnLaunched warning](includes/onlaunched-warning.md)]


## <a name="activation-in-depth"></a>深度激活

使你的通知可操作的第一步是将一些启动参数添加到你的通知，以便你的应用程序可以知道当用户单击通知时要启动的内容 (在这种情况下，我们将包括一些信息，稍后会告诉我们你应该打开一个会话，并且我们知道要打开哪些特定对话) 。

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())

    // Arguments returned when user taps body of notification
    ->AddArgument("action", "viewConversation")
    ->AddArgument("conversationId", 9813)

    ->AddText("Andrew sent you a picture")
    ->Show();
```



## <a name="adding-images"></a>添加图像

可以向通知添加丰富的内容。 我们会将一个内联图像和一个配置文件 (应用徽标覆盖) 映像。

[!INCLUDE [images note](includes/images-note.md)]

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```cpp
// Construct the content and show the toast!
(ref new ToastContentBuilder())
    ...

    // Inline image
    ->AddInlineImage(ref new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    ->AddAppLogoOverride(ref new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop::Circle)
    
    ->Show();
```



## <a name="adding-buttons-and-inputs"></a>添加按钮和输入

你可以添加按钮和输入以使你的通知交互。 按钮可以启动前景应用、协议或后台任务。 我们将添加一个 "答复" 文本框、一个 "赞" 按钮和一个打开该图像的 "视图" 按钮。

<img src="images/toast-notification.png" width="628" alt="Screenshot of a toast notification with inputs and buttons"/>

```cpp
// Construct the content
(ref new ToastContentBuilder())
    ->AddArgument("conversationId", 9813)
    ...

    // Text box for replying
    ->AddInputTextBox("tbReply", "Type a response")

    // Buttons
    ->AddButton((ref new ToastButton())
        ->SetContent("Reply")
        ->AddArgument("action", "reply")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("Like")
        ->AddArgument("action", "like")
        ->SetBackgroundActivation())

    ->AddButton((ref new ToastButton())
        ->SetContent("View")
        ->AddArgument("action", "view"))
    
    ->Show();
```

对前台按钮的激活方式的处理方式与 (应用程序的主 toast 正文相同。将) 调用 .xaml OnActivated。



## <a name="handling-background-activation"></a>处理后台激活

当你对你的 toast（或 toast 内的按钮）指定后台激活时，将执行后台任务而不是激活前台应用。

有关后台任务的详细信息，请参阅[使用后台任务支持应用](../../../launch-resume/support-your-app-with-background-tasks.md)。

如果你的目标版本是 14393 或更高版本，则可以使用进程内后台任务，这样可大大简化操作。 请注意，无法在较旧版本的 Windows 上运行进程内后台任务。 在此代码示例中，将使用进程内后台任务。

```csharp
const string taskName = "ToastBackgroundTask";

// If background task is already registered, do nothing
if (BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name.Equals(taskName)))
    return;

// Otherwise request access
BackgroundAccessStatus status = await BackgroundExecutionManager.RequestAccessAsync();

// Create the background task
BackgroundTaskBuilder builder = new BackgroundTaskBuilder()
{
    Name = taskName
};

// Assign the toast action trigger
builder.SetTrigger(new ToastNotificationActionTrigger());

// And register the task
BackgroundTaskRegistration registration = builder.Register();
```


然后在应用程序中，重写 OnBackgroundActivated 方法。 然后，你可以检索预定义参数和用户输入，类似于前台激活。

**App.xaml.cs**

```csharp
protected override async void OnBackgroundActivated(BackgroundActivatedEventArgs args)
{
    var deferral = args.TaskInstance.GetDeferral();
 
    switch (args.TaskInstance.Task.Name)
    {
        case "ToastBackgroundTask":
            var details = args.TaskInstance.TriggerDetails as ToastNotificationActionTriggerDetail;
            if (details != null)
            {
                string arguments = details.Argument;
                var userInput = details.UserInput;

                // Perform tasks
            }
            break;
    }
 
    deferral.Complete();
}
```


## <a name="set-an-expiration-time"></a>设置过期时间

在 Windows 10 中，所有 toast 通知被用户消除或忽略后将转到操作中心，以便在弹出窗口消失后，用户仍可查看通知。

但是，如果你的通知中的消息仅在一段时间内相关，则应对 toast 通知设置过期时间，让用户不至于看到来自应用的过时信息。 例如，如果升级的有效时间仅为 12 个小时，则将过期时间设置为 12 个小时。 下面的代码中将过期时间设置为 2 天。

> [!NOTE]
> 本地 toast 通知的默认和最长过期时间为 3 天。

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("Expires in 2 days...")
    ->Show(toast =>
    {
        toast->ExpirationTime = DateTime::Now->AddDays(2);
    });
```


## <a name="provide-a-primary-key-for-your-toast"></a>为 toast 提供主键

如要以编程方式删除或替换发送的通知，需使用 Tag 属性（还可选择使用 Group 属性）来为通知提供主键。 然后，你可以在以后使用此主键来删除或替换该通知。

要查看有关替换/删除已发送的 toast 通知的更多详细信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 和 Group 组合充当复合主键。 Group 是两者中较为通用的标识符，你可以用它来分配如“wallPosts”、“messages”、“friendRequests”等组。而 Tag 应该唯一标识组中的通知本身。 使用通用组时，可以使用 [RemoveGroup API](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 删除该组中的所有通知。

```cpp
// Create toast content and show the toast!
(ref new ToastContentBuilder())
    ->AddText("New post on your wall!")
    ->Show(toast =>
    {
        toast.Tag = "18365";
        toast.Group = "wallPosts";
    });
```



## <a name="clear-your-notifications"></a>清除你的通知

应用负责删除和清除自己的通知。 当你的应用启动时，我们不会自动清除你的通知。

仅当用户显式单击通知时，Windows 才会自动删除该通知。

下面是消息传递应用应执行的操作的示例...

1. 用户收到关于对话中新消息的多个 toast
2. 用户点击其中一个 toast 以打开该对话
3. 应用打开该对话，然后清除该对话的所有 toast（方法是对该对话的应用提供的组使用 [RemoveGroup](/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)）
4. 用户的操作中心现在能正确反映通知状态，因为操作中心未留有该对话的过期通知。

若要了解有关清除所有通知或删除特定通知的信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](/previous-versions/windows/apps/dn631260(v=win.10))。

```cpp
ToastNotificationManagerCompat::History->Clear();
```



## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [toast 内容文档](adaptive-interactive-toasts.md)
* [ToastNotification 类](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 类](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)