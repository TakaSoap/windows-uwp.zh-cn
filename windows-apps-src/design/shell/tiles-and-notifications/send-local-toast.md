---
description: 了解如何从 UWP 应用发送本地 toast 通知，并处理用户单击 toast 的操作。
title: 从 UWP 应用发送本地 toast 通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from UWP apps
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp, 发送 toast 通知, 通知, 发送通知, toast 通知, 操作方法, 快速入门, 开始使用, 代码示例, 演练
ms.localizationpriority: medium
ms.openlocfilehash: b532e041ffbbcf4a2ecac0e3386430b65d833f2d
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034480"
---
# <a name="send-a-local-toast-notification-from-uwp-apps"></a>从 UWP 应用发送本地 toast 通知


Toast 通知是用户当前未在应用内部时应用可构造并发送给用户的消息。 此快速入门指南将指导你借助新自适应模板和交互式操作完成创建、交付并显示 Windows 10 toast 通知的步骤。 通过本地通知对这些操作进行说明，本地通知是实现起来最简单的通知。

> [!IMPORTANT]
> 桌面应用程序 (包括打包的 [.msix](/windows/msix/desktop/source-code-overview) 应用、使用 [稀疏包](/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) 获取包标识的应用，以及经典非打包桌面应用) 执行发送通知和处理激活的步骤不同。 请参阅[桌面 app](toast-desktop-apps.md) 文档，了解如何实现 toast。

> **重要 API** ： [ToastNotification 类](/uwp/api/Windows.UI.Notifications.ToastNotification)、 [ToastNotificationActivatedEventArgs 类](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)



## <a name="step-1-install-nuget-package"></a>步骤1：安装 NuGet 包

安装 "..." [NuGet 包](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)。 我们的代码示例将使用此包。 本文结束时，我们会提供不使用任何 NuGet 包的 "纯" 代码片段。 使用此包可以创建 toast 通知，而无需使用 XML。


## <a name="step-2-add-namespace-declarations"></a>步骤2：添加命名空间声明

`Windows.UI.Notifications` 包括 toast Api。

```csharp
using Windows.UI.Notifications;
using Microsoft.Toolkit.Uwp.Notifications; // Notifications library
```


## <a name="step-3-send-a-toast"></a>步骤3：发送 toast

在 Windows 10 中，你的 toast 通知内容是使用对于你的通知外观给予了最大程度灵活性的自适应语言描述的。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。

我们将从一个简单的基于文本的通知开始。 使用通知库) 构造通知内容 (并显示通知！

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    .AddToastActivationInfo("picOfHappyCanyon", ToastActivationType.Foreground)
    .AddText("Andrew sent you a picture")
    .AddText("Check this out, Happy Canyon in Utah!")
    .GetToastContent();

// Create the notification
var notif = new ToastNotification(content.GetXml());

// And show it!
ToastNotificationManager.CreateToastNotifier().Show();
```

## <a name="step-4-handling-activation"></a>步骤4：处理激活

当用户单击你的通知 (或使用前台激活) 通知上的按钮时，将调用应用的 **App.xaml.cs** **OnActivated** 。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle notification activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {
        // Obtain the arguments from the notification
        string args = toastActivationArgs.Argument;

        // Obtain any user input (text boxes, menu selections) from the notification
        ValueSet userInput = toastActivationArgs.UserInput;
 
        // TODO: Show the corresponding content
    }
}
```

> [!IMPORTANT]
> 必须按 **OnLaunched** 代码那样初始化框架和激活窗口。 **如果用户单击你的 toast ，则不会调用 OnLaunched** ，即使你的应用已关闭并是首次启动也是如此。 通常建议将 **OnLaunched** 和 **OnActivated** 合并到你自己的 `OnLaunchedOrActivated` 方法中，因为二者中均需执行相同的初始化。


## <a name="activation-in-depth"></a>深度激活

使你的通知可操作的第一步是将一些启动参数添加到你的通知，以便你的应用程序可以知道当用户单击通知时要启动的内容 (在这种情况下，我们将包括一些信息，稍后会告诉我们你应该打开一个会话，并且我们知道要打开哪些特定对话) 。

建议安装 [QueryString.NET](https://www.nuget.org/packages/QueryString.NET/) NuGet 包，以帮助构造和分析通知参数的查询字符串，如下所示。

```csharp
using Microsoft.QueryStringDotNET; // QueryString.NET

int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()

    // Arguments returned when user taps body of notification
    .AddToastActivationInfo(new QueryString() // Using QueryString.NET
    {
        { "action", "viewConversation" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), ToastActivationType.Foreground)

    .AddText("Andrew sent you a picture")
    ...
```


下面是一个更复杂的示例，说明如何处理激活 .。。

**App.xaml.cs**

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Get the root frame
    Frame rootFrame = Window.Current.Content as Frame;
 
    // TODO: Initialize root frame just like in OnLaunched
 
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs toastActivationArgs)
    {            
        // Parse the query string (using QueryString.NET)
        QueryString args = QueryString.Parse(toastActivationArgs.Argument);
 
        // See what action is being requested 
        switch (args["action"])
        {
            // Open the image
            case "viewImage":
 
                // The URL retrieved from the toast args
                string imageUrl = args["imageUrl"];
 
                // If we're already viewing that image, do nothing
                if (rootFrame.Content is ImagePage && (rootFrame.Content as ImagePage).ImageUrl.Equals(imageUrl))
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ImagePage), imageUrl);
                break;
                             
 
            // Open the conversation
            case "viewConversation":
 
                // The conversation ID retrieved from the toast args
                int conversationId = int.Parse(args["conversationId"]);
 
                // If we're already viewing that conversation, do nothing
                if (rootFrame.Content is ConversationPage && (rootFrame.Content as ConversationPage).ConversationId == conversationId)
                    break;
 
                // Otherwise navigate to view it
                rootFrame.Navigate(typeof(ConversationPage), conversationId);
                break;
        }
 
        // If we're loading the app for the first time, place the main page on
        // the back stack so that user can go back after they've been
        // navigated to the specific page
        if (rootFrame.BackStack.Count == 0)
            rootFrame.BackStack.Add(new PageStackEntry(typeof(MainPage), null, null));
    }
 
    // TODO: Handle other types of activation
 
    // Ensure the current window is active
    Window.Current.Activate();
}
```


## <a name="adding-images"></a>添加图像

可以向通知添加丰富的内容。 我们会将一个内联图像和一个配置文件 (应用徽标覆盖) 映像。

> [!NOTE]
> 图像可来自于应用包、应用的本地存储或来自 Web。 自 Fall Creators Update 起，正常连接上的 Web 图像的大小限制提升至 3 MB，按流量计费的连接上的限制提升至 1 MB。 在尚未运行 Fall Creators Update 的设备上，Web 图像的大小不得超过 200 KB。

> [!IMPORTANT]
> Http 映像仅在其清单中具有 internet 功能的 UWP/.MSIX/稀疏应用中受支持。 桌面非 .MSIX/稀疏应用不支持 http 映像;必须将映像下载到本地应用数据，并在本地引用它。

<img alt="Toast with images" src="images/send-toast-02.png" width="364"/>

```csharp
// Construct the content
var content = new ToastContentBuilder()
    ...

    // Inline image
    .AddInlineImage(new Uri("https://picsum.photos/360/202?image=883"))

    // Profile (app logo override) image
    .AddAppLogoOverride(new Uri("ms-appdata:///local/Andrew.jpg"), ToastGenericAppLogoCrop.Circle)
    
    .GetToastContent();
    
...
```



## <a name="adding-buttons-and-inputs"></a>添加按钮和输入

你可以添加按钮和输入以使你的通知交互。 按钮可以启动前景应用、协议或后台任务。 我们将添加一个 "答复" 文本框、一个 "赞" 按钮和一个打开该图像的 "视图" 按钮。

<img alt="Toast with images and buttons" src="images/send-toast-03.png" width="364"/>

```csharp
int conversationId = 384928;

// Construct the content
var content = new ToastContentBuilder()
    ...

    // Text box for replying
    .AddInputTextBox("tbReply", placeHolderContent: "Type a response")

    // Reference the text box's ID in order to place this button next to the text box
    .AddButton("tbReply", "Reply", ToastActivationType.Background, new QueryString()
    {
        { "action", "reply" },
        { "conversationId", conversationId.ToString() }
    }.ToString(), imageUri: new Uri("Assets/Reply.png", UriKind.Relative))

    .AddButton("Like", ToastActivationType.Background, new QueryString()
    {
        { "action", "like" },
        { "conversationId", conversationId.ToString() }
    }.ToString())

    .AddButton("View", ToastActivationType.Foreground, new QueryString()
    {
        { "action", "viewImage" },
        { "imageUrl", image.ToString() }
    }.ToString())
    
    .GetToastContent();
    
...
```

将按照与主 toast 正文相同的方式来处理前景按钮的激活方式， (App.xaml.cs OnActivated 将被称为) 。



## <a name="handling-background-activation"></a>处理后台激活

当你对你的 toast（或 toast 内的按钮）指定后台激活时，将执行后台任务而不是激活前台应用。

有关后台任务的详细信息，请参阅[使用后台任务支持应用](/windows/uwp/launch-resume/support-your-app-with-background-tasks)。

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


然后在 App.xaml.cs 中，重写 OnBackgroundActivated 方法。 然后，你可以检索预定义参数和用户输入，类似于前台激活。

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

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("Expires in 2 days...")
    .GetToastContent();

// Set expiration time
var notif = new ToastNotification(content.GetXml())
{
    ExpirationTime = DateTime.Now.AddDays(2)
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```


## <a name="provide-a-primary-key-for-your-toast"></a>为 toast 提供主键

如要以编程方式删除或替换发送的通知，需使用 Tag 属性（还可选择使用 Group 属性）来为通知提供主键。 然后，你可以在以后使用此主键来删除或替换该通知。

要查看有关替换/删除已发送的 toast 通知的更多详细信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))。

Tag 和 Group 组合充当复合主键。 Group 是两者中较为通用的标识符，你可以用它来分配如“wallPosts”、“messages”、“friendRequests”等组。而 Tag 应该唯一标识组中的通知本身。 使用通用组时，可以使用 [RemoveGroup API](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_) 删除该组中的所有通知。

```csharp
// Create toast content
var content = new ToastContentBuilder()
    .AddText("New post on your wall!")
    .GetToastContent();

// Set tag/group
new ToastNotification(content.GetXml())
{
    Tag = "18365",
    Group = "wallPosts"
};

// And show it!
ToastNotificationManager.CreateToastNotifier().Show(notif);
```



## <a name="clear-your-notifications"></a>清除你的通知

UWP 应用负责删除和清除它们自己的通知。 当你的应用启动时，我们不会自动清除你的通知。

仅当用户显式单击通知时，Windows 才会自动删除该通知。

下面是消息传递应用应执行的操作的示例...

1. 用户收到关于对话中新消息的多个 toast
2. 用户点击其中一个 toast 以打开该对话
3. 应用打开该对话，然后清除该对话的所有 toast（方法是对该对话的应用提供的组使用 [RemoveGroup](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotificationHistory#Windows_UI_Notifications_ToastNotificationHistory_RemoveGroup_System_String_)）
4. 用户的操作中心现在能正确反映通知状态，因为操作中心未留有该对话的过期通知。

若要了解有关清除所有通知或删除特定通知的信息，请参阅[快速入门：在操作中心 (XAML) 中管理 toast 通知](https://docs.microsoft.com/previous-versions/windows/apps/dn631260(v=win.10))。

```csharp
ToastNotificationManager.History.Clear();
```


## <a name="plain-code-snippets"></a>普通代码片段

如果使用的不是 NuGet 中的通知库，可以按如下所示手动构造 XML，以创建 [ToastNotification](/uwp/api/Windows.UI.Notifications.ToastNotification)。

```csharp
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;

// In a real app, these would be initialized with actual data
string title = "Andrew sent you a picture";
string content = "Check this out, Happy Canyon in Utah!";
string image = "http://blogs.msdn.com/cfs-filesystemfile.ashx/__key/communityserver-blogs-components-weblogfiles/00-00-01-71-81-permanent/2727.happycanyon1_5B00_1_5D00_.jpg";
string logo = "ms-appdata:///local/Andrew.jpg";
 
// TODO: all values need to be XML escaped
 
// Construct the visuals of the toast
string toastVisual =
$@"<visual>
  <binding template='ToastGeneric'>
    <text>{title}</text>
    <text>{content}</text>
    <image src='{image}'/>
    <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
  </binding>
</visual>";

// In a real app, these would be initialized with actual data
int conversationId = 384928;
 
// Generate the arguments we'll be passing in the toast
string argsReply = $"action=reply&conversationId={conversationId}";
string argsLike = $"action=like&conversationId={conversationId}";
string argsView = $"action=viewImage&imageUrl={Uri.EscapeDataString(image)}";
 
// TODO: all args need to be XML escaped
 
string toastActions =
$@"<actions>
 
  <input
      type='text'
      id='tbReply'
      placeHolderContent='Type a response'/>
 
  <action
      content='Reply'
      arguments='{argsReply}'
      activationType='background'
      imageUri='Assets/Reply.png'
      hint-inputId='tbReply'/>
 
  <action
      content='Like'
      arguments='{argsLike}'
      activationType='background'/>
 
  <action
      content='View'
      arguments='{argsView}'/>
 
</actions>";

// Now we can construct the final toast content
string argsLaunch = $"action=viewConversation&conversationId={conversationId}";
 
// TODO: all args need to be XML escaped
 
string toastXmlString =
$@"<toast launch='{argsLaunch}'>
    {toastVisual}
    {toastActions}
</toast>";
 
// Parse to XML
XmlDocument toastXml = new XmlDocument();
toastXml.LoadXml(toastXmlString);
 
// Generate toast
var toast = new ToastNotification(toastXml);
```


## <a name="resources"></a>资源

* [GitHub 上的完整代码示例](https://github.com/WindowsNotifications/quickstart-sending-local-toast-win10)
* [toast 内容文档](adaptive-interactive-toasts.md)
* [ToastNotification 类](/uwp/api/Windows.UI.Notifications.ToastNotification)
* [ToastNotificationActivatedEventArgs 类](/uwp/api/Windows.ApplicationModel.Activation.ToastNotificationActivatedEventArgs)
