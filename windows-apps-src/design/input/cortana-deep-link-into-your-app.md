---
title: 从 Cortana 中的后台应用到前台应用的深层链接 - Cortana UWP 设计和开发
description: 提供从 **Cortana** 中的后台应用程序的深层链接，该应用程序在特定状态或上下文中启动应用程序到前台。
ms.assetid: 6fe5fcc5-9ee4-4c04-92f4-7b1bf7ef5651
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 5096e4897d5a75be70deaf272ec52c151c1ad871
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823491"
---
# <a name="deep-link-from-a-background-app-in-cortana-to-a-foreground-app"></a>从 Cortana 中的后台应用到前台应用的深层链接

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

提供从 **Cortana** 中的后台应用程序的深层链接，该应用程序在特定状态或上下文中启动应用程序到前台。

> [!NOTE]
> 启动前台应用时， **Cortana** 和后台应用服务都会终止。

默认情况下，" **Cortana** 完成" 屏幕上会显示一个深层链接，如下所示 ( "中转到 AdventureWorks" ) ，但你可以在其他屏幕上显示深层链接。

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="即将到来的 Cortana 后台应用完成的屏幕截图":::

> [!NOTE]
> **重要的 API**
>
> - [**Windows.applicationmodel.resources.core. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

## <a name="overview"></a>概述

用户可以通过 **Cortana** 访问你的应用程序，方法是：

- 将其激活为前台应用程序 (参阅 [通过 Cortana) 使用语音命令激活前台应用](cortana-launch-a-foreground-app-with-voice-commands.md) 。
- 公开特定功能作为后台应用服务 (参阅 [使用语音命令) Cortana 激活后台应用](cortana-launch-a-background-app-with-voice-commands.md) 。
- 深层链接到特定页、内容以及状态或上下文。

这里讨论了深层链接。

当 Cortana 和应用服务充当功能齐全的 (应用的网关时，深层链接非常有用，而是要求用户通过 "开始" 菜单) 来启动应用，或者通过 Cortana 提供对应用中更丰富的详细信息和功能的访问权限。 深层链接是提高可用性和升级应用的另一种方法。

提供深层链接的方法有三种：

- &lt;各种 Cortana 屏幕上的 "中转到应用程序 &gt; " 链接。 
- 在各种 **Cortana** 屏幕上的内容磁贴中嵌入的链接。
- 以编程方式从后台应用服务启动前台应用。

## <a name="go-to-ltappgt-deep-link"></a>"中转到 &lt; 应用程序 &gt; " 深层链接

**Cortana** &lt; &gt; 在大多数屏幕上的内容卡下显示 "中转到应用" 深层链接。

:::image type="content" source="images/cortana/cortana-completion-screen.png" alt-text="后台应用完成屏幕上 Cortana 的 &quot;中转到应用&quot; 深层链接的屏幕截图。":::

可以提供此链接的启动参数，该参数在与应用服务相同的上下文中打开应用。 如果未提供启动参数，应用将启动到主屏幕。

在此示例中，通过 **AdventureWorks** 示例的 AdventureWorksVoiceCommandService.cs，将指定的目标 (`destination`) 字符串传递到 SendCompletionMessageForDestination 方法，该方法检索所有匹配行程并提供指向应用的深层链接。

首先，我们将创建一个 [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) (```userMessage```) ，它被 **cortana** 口述，并显示在 **cortana** 画布上。 然后创建一个 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) 列表对象，用于在画布上显示结果卡的集合。

然后，这两个对象将传递到 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)对象的 [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)方法 (`response`) 。 然后，将响应对象的 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 属性值设置为 `destination` 此函数的 passsed 值。 当用户在 Cortana 画布上点击内容磁贴时，参数值将通过响应对象传递到应用程序。

最后，我们调用 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)的 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)方法。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
...
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
...
    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="content-tile-deep-link"></a>内容磁贴深层链接

可以将深层链接添加到各种 **Cortana** 屏幕上的内容卡。

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="使用 AdventureWorks 后端到端 cortana 后台应用程序流的 cortana 画布的屏幕截图，其中":::*包含移交*

与 "中转到 &lt; 应用程序 &gt; " 链接一样，你可以提供一个启动参数来打开你的应用程序，并将其上下文与应用服务相似。 如果未提供启动参数，则内容磁贴不会链接到您的应用程序。

在此示例中，通过 **AdventureWorks** 示例的 AdventureWorksVoiceCommandService.cs，我们将指定的目标传递给 SendCompletionMessageForDestination 方法，该方法检索所有匹配行程，并提供包含应用的深层链接的内容卡。

首先，我们将创建一个  [**VoiceCommandUserMessage**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandUserMessage) (```userMessage```) ，它被 **cortana** 口述，并显示在 **cortana** 画布上。 然后创建一个 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile) 列表对象，用于在画布上显示结果卡的集合。 

然后，这两个对象将传递到 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)对象的 [CreateResponse](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)方法 (```response```) 。 然后，将 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 属性值设置为 voice 命令中目标的值。

最后，我们调用 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)的 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)方法。
在这里，我们将两个具有不同 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)参数值的内容磁贴添加到 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)对象的 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)调用中使用的 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)列表。

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination specified in the voice command.</param>
private async Task SendCompletionMessageForDestination(string destination)
{
    // If this operation is expected to take longer than 0.5 seconds, the task must
    // supply a progress response to Cortana before starting the operation, and
    // updates must be provided at least every 5 seconds.
    string loadingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("LoadingTripToDestination", cortanaContext).ValueAsString,
               destination);
    await ShowProgressScreen(loadingTripToDestination);
    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    // Query for the specified trip. 
    // The destination should be in the phrase list. However, there might be  
    // multiple trips to the destination. We pick the first.
    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);

    var userMessage = new VoiceCommandUserMessage();
    var destinationsContentTiles = new List<VoiceCommandContentTile>();
    if (trips.Count() == 0)
    {
        string foundNoTripToDestination = string.Format(
               cortanaResourceMap.GetValue("FoundNoTripToDestination", cortanaContext).ValueAsString,
               destination);
        userMessage.DisplayMessage = foundNoTripToDestination;
        userMessage.SpokenMessage = foundNoTripToDestination;
    }
    else
    {
        // Set plural or singular title.
        string message = "";
        if (trips.Count() > 1)
        {
            message = cortanaResourceMap.GetValue("PluralUpcomingTrips", cortanaContext).ValueAsString;
        }
        else
        {
            message = cortanaResourceMap.GetValue("SingularUpcomingTrip", cortanaContext).ValueAsString;
        }
        userMessage.DisplayMessage = message;
        userMessage.SpokenMessage = message;

        // Define a tile for each destination.
        foreach (Model.Trip trip in trips)
        {
            int i = 1;
            
            var destinationTile = new VoiceCommandContentTile();

            destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
            destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));

            destinationTile.AppLaunchArgument = trip.Destination;
            destinationTile.Title = trip.Destination;
            if (trip.StartDate != null)
            {
                destinationTile.TextLine1 = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
            }
            else
            {
                destinationTile.TextLine1 = trip.Destination + " " + i;
            }

            destinationsContentTiles.Add(destinationTile);
            i++;
        }
    }

    var response = VoiceCommandResponse.CreateResponse(userMessage, destinationsContentTiles);

    if (trips.Count() > 0)
    {
        response.AppLaunchArgument = destination;
    }

    await voiceServiceConnection.ReportSuccessAsync(response);
}
```

## <a name="programmatic-deep-link"></a>编程深层链接

你还可以使用启动参数以编程方式启动应用程序，以使用与应用服务相似的上下文打开应用。 如果未提供启动参数，应用将启动到主屏幕。

此处，我们将值为 "拉斯维加斯" 的 [**AppLaunchArgument**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)参数添加到 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)对象的 [**RequestAppLaunchAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)调用中使用的 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse)对象。

```CSharp
var userMessage = new VoiceCommandUserMessage();
userMessage.DisplayMessage = "Here are your trips.";
userMessage.SpokenMessage = 
  "You have one trip to Vegas coming up.";

response = VoiceCommandResponse.CreateResponse(userMessage);
response.AppLaunchArgument = “Las Vegas”;
await  VoiceCommandServiceConnection.RequestAppLaunchAsync(response);
```

## <a name="app-manifest"></a>应用部件清单

若要启用对应用的深层链接，必须 `windows.personalAssistantLaunch` 在应用项目的 appxmanifest.xml 文件中声明扩展。

此处，我们声明 `windows.personalAssistantLaunch` **艾德作品** 应用的扩展。

```XML
<Extensions>
  <uap:Extension Category="windows.appService" 
    EntryPoint="AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService">
    <uap:AppService Name="AdventureWorksVoiceCommandService"/>
  </uap:Extension>
  <uap:Extension Category="windows.personalAssistantLaunch"/> 
</Extensions>
```

## <a name="protocol-contract"></a>协议协定

您的应用程序通过统一资源标识符启动到前台， (URI) 使用 [**协议**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind) 协定激活。 应用必须重写应用的 [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application) 事件，并检查是否有 **ActivationKind** 的 **协议**。 有关详细信息，请参阅 [处理 URI 激活](../../launch-resume/handle-uri-activation.md)。

在这里，我们对 [**ProtocolActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) 提供的 URI 进行解码，以访问启动参数。 在此示例中， [**Uri**](/uwp/api/Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs) 设置为 "personalassistantlaunch：？LaunchContext = 内华达州拉斯维加斯 "。

```CSharp
if (args.Kind == ActivationKind.Protocol)
  {
    var commandArgs = args as ProtocolActivatedEventArgs;
    Windows.Foundation.WwwFormUrlDecoder decoder = 
      new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
    var destination = decoder.GetFirstValueByName("LaunchContext");

    navigationCommand = new ViewModel.TripVoiceCommand(
      "protocolLaunch",
      "text",
      "destination",
      destination);

    navigationToPageType = typeof(View.TripDetails);

    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active.
    Window.Current.Activate();
  }
```

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [Cortana 设计准则](cortana-design-guidelines.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)