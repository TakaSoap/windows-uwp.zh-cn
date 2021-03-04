---
title: 与 Cortana 中的后台应用交互 - Cortana UWP 设计和开发
description: 执行语音命令时，通过 Cortana 画布中的语音和文本输入，使用户能够与后台应用交互。
ms.assetid: e42917dc-aece-4880-813f-80b897f9126c
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 6e63d86d8d3764f8ca95dce4c1b8b7de437c95ec
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824491"
---
# <a name="interact-with-a-background-app-in-cortana"></a>在 Cortana 中与后台应用交互

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

执行语音命令时，通过 **Cortana** 画布中的语音和文本输入，使用户能够与后台应用交互。

> [!NOTE]
> **重要的 API**
>
> - [**Windows.applicationmodel.resources.core. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

Cortana 支持通过应用完成的一次性工作流。 此工作流由您的应用程序定义，并可支持以下功能： 

- 成功完成
- 移交
- 进度
- 确认
- 消除歧义
- 错误

## <a name="composing-feedback-strings"></a>撰写反馈字符串

> [!TIP]
> **先决条件**
>
> 如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请查看这些主题来熟悉此处讨论的技术。
>
> - [创建你的第一个应用](../../get-started/your-first-app.md)
> - 借助[事件和路由事件概述](../../xaml-platform/events-and-routed-events-overview.md)了解事件
>
> **用户体验指南**
>
> 有关如何将应用集成到 **Cortana** 和 [语音交互](speech-interactions.md)的信息，请参阅 [cortana 设计指导原则](cortana-design-guidelines.md)，获取有关设计有用且具有吸引力的语音应用的有用技巧。

撰写 **Cortana** 显示和朗读的反馈字符串。

[Cortana 设计指南](cortana-design-guidelines.md)提供了有关为 **Cortana** 编写字符串的建议。

内容卡可为用户提供附加的上下文，并可帮助你使反馈字符串简洁明了。

**Cortana** 支持以下内容卡模板 (在完成屏幕) 上只能使用一个模板：

- 仅标题
- 标题最多包含三行文本
- 带图像的标题
- 带图像的标题和最多三行文本

图像可以是：

- 68w x 68h
- 68w x 92h
- 280w x 140h

你还可以让用户在前台启动你的应用程序，方法是单击卡或应用的文本链接。

## <a name="completion-screen"></a>完成屏幕

完成屏幕为用户提供有关已完成的语音命令任务的信息。

对于你的应用程序作出响应并不需要用户提供额外信息的500任务，无需进一步与  **Cortana** 交互即可完成。 Cortana 只显示完成屏幕。

在这里，我们将使用 **艾德作品** 应用显示语音命令请求的完成屏幕，以显示即将到来的伦敦行程。

:::image type="content" source="images/cortana/cortana-completion-screen-upcomingtrip-small.png" alt-text="即将到来的 Cortana 后台应用完成的屏幕截图":::

语音命令在 AdventureWorksCommands.xml 中定义：

```xml
<Command Name="whenIsTripToDestination">
  <Example> When is my trip to Las Vegas?</Example>
  <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
  <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
  <Feedback> Looking for trip to {destination}</Feedback>
  <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
</Command>
```

AdventureWorksVoiceCommandService.cs 包含完成消息方法：

```csharp
/// <summary>
/// Show details for a single trip, if the trip can be found. 
/// This demonstrates a simple response flow in Cortana.
/// </summary>
/// <param name="destination">The destination, expected to be in the phrase list.</param>
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

## <a name="hand-off-screen"></a>手动关闭屏幕

识别语音命令后， **cortana** 必须调用 ReportSuccessAsync，并在大约500If 内提供反馈。应用服务无法完成500毫秒中的语音命令指定的操作， **Cortana** 会显示一个在应用调用 ReportSuccessAsync 或最长5秒后显示的手写屏。

如果应用服务不调用 ReportSuccessAsync 或任何其他 VoiceCommandServiceConnection 方法，则用户将收到一条错误消息，并且应用服务调用将被取消。

下面是 " **艾德作品** " 应用的手写输入示例。 在此示例中，用户已查询 **Cortana** 进行即将进行的行程。 "移交" 屏幕包括使用应用服务名称、图标和 **反馈** 字符串自定义的消息。 

[!NOTE] 可以在 VCD 文件中声明一个 **反馈** 字符串。 此字符串不会影响 Cortana 画布上显示的 UI 文本，它只会影响 **cortana** 所讲述的文本。

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 后台应用程序的屏幕截图":::

## <a name="progress-screen"></a>进度屏幕

如果应用服务的调用 ReportSuccessAsync 超过500毫秒， **Cortana** 会为用户提供进度屏幕。 将显示应用图标，并且必须提供 GUI 和 TTS 进度字符串，以指示正在主动处理该任务。

**Cortana** 显示最多5秒的进度屏幕。 5秒钟后， **Cortana** 会向用户显示一条错误消息，并关闭应用服务。 如果应用服务需要5秒以上才能完成此操作，它可以继续用进度屏幕更新 **Cortana** 。

下面是 **艾德作品** 应用的进度屏幕示例。 在此示例中，用户取消了拉斯维加斯的行程。 "进度" 屏幕包括为操作、图标和内容磁贴自定义的消息，其中包含要取消的行程的相关信息。

:::image type="content" source="images/cortana/cortana-progress-screen.png" alt-text="带有后台应用进度屏幕的 Cortana 的屏幕截图":::

AdventureWorksVoiceCommandService.cs 包含以下进度消息方法，该方法调用 [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 以显示 **Cortana** 中的进度屏幕。

```    CSharp
/// <summary>
/// Show a progress screen. These should be posted at least every 5 seconds for a 
/// long-running operation.
/// </summary>
/// <param name="message">The message to display, relating to the task being performed.</param>
/// <returns></returns>
private async Task ShowProgressScreen(string message)
{
    var userProgressMessage = new VoiceCommandUserMessage();
    userProgressMessage.DisplayMessage = userProgressMessage.SpokenMessage = message;

    VoiceCommandResponse response = VoiceCommandResponse.CreateResponse(userProgressMessage);
    await voiceServiceConnection.ReportProgressAsync(response);
}
```

## <a name="confirmation-screen"></a>确认屏幕

如果由语音命令指定的操作是不可逆的，会有重大影响，或者识别置信度不高，则应用服务可以请求确认。

下面是一个用于 **艾德 Works** 应用的确认屏幕的示例。 在此示例中，用户已指示应用服务通过 **Cortana** 取消到拉斯维加斯的行程。 应用服务已向 **Cortana** 提供确认屏幕，该屏幕在取消行程之前会提示用户是否有 "是" 或 "否" 回答。

如果用户说 "是" 或 "否"， **Cortana** 将无法确定问题的答案。 在这种情况下， **Cortana** 会提示用户提供应用服务提供的类似问题。

在第二个提示符下，如果用户仍未显示 "是" 或 "否"， **Cortana** 会在第三次使用带有道歉前缀的相同问题提示用户。 如果用户仍未显示 "是" 或 "否"， **Cortana** 将停止侦听语音输入，并要求用户点击其中一个按钮。

确认屏幕包含为操作、图标和内容磁贴自定义的消息，其中包含要取消的行程的相关信息。

:::image type="content" source="images/cortana/cortana-confirmation-screen.png" alt-text="带有后台应用确认屏幕的 Cortana 屏幕截图":::

AdventureWorksVoiceCommandService.cs 包含以下行程取消方法，该方法调用 [**RequestConfirmationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 以在 **Cortana** 中显示确认屏幕。

```    CSharp
/// <summary>
/// Handle the Trip Cancellation task. This task demonstrates how to prompt a user
/// for confirmation of an operation, show users a progress screen while performing
/// a long-running task, and show a completion screen.
/// </summary>
/// <param name="destination">The name of a destination.</param>
/// <returns></returns>
private async Task SendCompletionMessageForCancellation(string destination)
{
    // Begin loading data to search for the target store. 
    // Consider inserting a progress screen here, in order to prevent Cortana from timing out. 
    string progressScreenString = string.Format(
        cortanaResourceMap.GetValue("ProgressLookingForTripToDest", cortanaContext).ValueAsString,
        destination);
    await ShowProgressScreen(progressScreenString);

    Model.TripStore store = new Model.TripStore();
    await store.LoadTrips();

    IEnumerable<Model.Trip> trips = store.Trips.Where(p => p.Destination == destination);
    Model.Trip trip = null;
    if (trips.Count() > 1)
    {
        // If there is more than one trip, provide a disambiguation screen.
        // However, if a significant number of items are returned, you might want to 
        // just display a link to your app and provide a deeper search experience.
        string disambiguationDestinationString = string.Format(
            cortanaResourceMap.GetValue("DisambiguationWhichTripToDest", cortanaContext).ValueAsString,
            destination);
        string disambiguationRepeatString = cortanaResourceMap.GetValue("DisambiguationRepeat", cortanaContext).ValueAsString;
        trip = await DisambiguateTrips(trips, disambiguationDestinationString, disambiguationRepeatString);
    }
    else
    {
        trip = trips.FirstOrDefault();
    }

    var userPrompt = new VoiceCommandUserMessage();
    
    VoiceCommandResponse response;
    if (trip == null)
    {
        var userMessage = new VoiceCommandUserMessage();
        string noSuchTripToDestination = string.Format(
            cortanaResourceMap.GetValue("NoSuchTripToDestination", cortanaContext).ValueAsString,
            destination);
        userMessage.DisplayMessage = userMessage.SpokenMessage = noSuchTripToDestination;

        response = VoiceCommandResponse.CreateResponse(userMessage);
        await voiceServiceConnection.ReportSuccessAsync(response);
    }
    else
    {
        // Prompt the user for confirmation that this is the correct trip to cancel.
        string cancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("CancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userPrompt.DisplayMessage = userPrompt.SpokenMessage = cancelTripToDestination;
        var userReprompt = new VoiceCommandUserMessage();
        string confirmCancelTripToDestination = string.Format(
            cortanaResourceMap.GetValue("ConfirmCancelTripToDestination", cortanaContext).ValueAsString,
            destination);
        userReprompt.DisplayMessage = userReprompt.SpokenMessage = confirmCancelTripToDestination;
        
        response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt);

        var voiceCommandConfirmation = await voiceServiceConnection.RequestConfirmationAsync(response);

        // If RequestConfirmationAsync returns null, Cortana has likely been dismissed.
        if (voiceCommandConfirmation != null)
        {
            if (voiceCommandConfirmation.Confirmed == true)
            {
                string cancellingTripToDestination = string.Format(
               cortanaResourceMap.GetValue("CancellingTripToDestination", cortanaContext).ValueAsString,
               destination);
                await ShowProgressScreen(cancellingTripToDestination);

                // Perform the operation to remove the trip from app data. 
                // As the background task runs within the app package of the installed app,
                // we can access local files belonging to the app without issue.
                await store.DeleteTrip(trip);

                // Provide a completion message to the user.
                var userMessage = new VoiceCommandUserMessage();
                string cancelledTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("CancelledTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = cancelledTripToDestination;
                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
            else
            {
                // Confirm no action for the user.
                var userMessage = new VoiceCommandUserMessage();
                string keepingTripToDestination = string.Format(
                    cortanaResourceMap.GetValue("KeepingTripToDestination", cortanaContext).ValueAsString,
                    destination);
                userMessage.DisplayMessage = userMessage.SpokenMessage = keepingTripToDestination;

                response = VoiceCommandResponse.CreateResponse(userMessage);
                await voiceServiceConnection.ReportSuccessAsync(response);
            }
        }
    }
}
```

## <a name="disambiguation-screen"></a>消除歧义屏幕

当由语音命令指定的操作具有多个可能的结果时，应用服务可以请求用户提供更多的信息。

下面是一个示例，其中显示了 **艾德公司** 应用的消除歧义屏幕。 在此示例中，用户已指示应用服务通过 **Cortana** 取消到拉斯维加斯的行程。 但是，用户在不同日期有两次到拉斯维加斯的行程，应用服务无法完成该操作，用户不需要选择预期的行程。

应用服务为 **Cortana** 提供歧义消除屏幕，提示用户在取消任何操作之前从匹配行程列表中进行选择。

在这种情况下， **Cortana** 会提示用户提供应用服务提供的类似问题。

在第二个提示符下，如果用户仍未说出可用于识别所选内容的内容， **Cortana** 会使用道歉的前缀相同的问题第三次提示用户。 如果用户仍未说出可用于识别所选内容的内容， **Cortana** 将停止侦听语音输入并要求用户点击其中一个按钮。

消除歧义屏幕包含为操作、图标和内容磁贴自定义的消息，其中包含有关要取消的行程的信息。

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="带有后台应用歧义的 Cortana 屏幕屏幕截图":::

AdventureWorksVoiceCommandService.cs 包含以下行程取消方法，该方法调用 [**RequestDisambiguationAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 以显示 **Cortana** 中的消除歧义屏幕。

```csharp
/// <summary>
/// Provide the user with a way to identify which trip to cancel. 
/// </summary>
/// <param name="trips">The set of trips</param>
/// <param name="disambiguationMessage">The initial disambiguation message</param>
/// <param name="secondDisambiguationMessage">Repeat prompt retry message</param>
private async Task<Model.Trip> DisambiguateTrips(IEnumerable<Model.Trip> trips, string disambiguationMessage, string secondDisambiguationMessage)
{
    // Create the first prompt message.
    var userPrompt = new VoiceCommandUserMessage();
    userPrompt.DisplayMessage =
        userPrompt.SpokenMessage = disambiguationMessage;

    // Create a re-prompt message if the user responds with an out-of-grammar response.
    var userReprompt = new VoiceCommandUserMessage();
    userReprompt.DisplayMessage =
        userReprompt.SpokenMessage = secondDisambiguationMessage;

    // Create card for each item. 
    var destinationContentTiles = new List<VoiceCommandContentTile>();
    int i = 1;
    foreach (Model.Trip trip in trips)
    {
        var destinationTile = new VoiceCommandContentTile();

        destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
        destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png"));
        
        // The AppContext can be any arbitrary object.
        destinationTile.AppContext = trip;
        string dateFormat = "";
        if (trip.StartDate != null)
        {
            dateFormat = trip.StartDate.Value.ToString(dateFormatInfo.LongDatePattern);
        }
        else
        {
            // The app allows a trip to have no date.
            // However, the choices must be unique so they can be distinguished.
            // Here, we add a number to identify them.
            dateFormat = string.Format("{0}", i);
        } 

        destinationTile.Title = trip.Destination + " " + dateFormat;
        destinationTile.TextLine1 = trip.Description;

        destinationContentTiles.Add(destinationTile);
        i++;
    }

    // Cortana handles re-prompting if no valid response.
    var response = VoiceCommandResponse.CreateResponseForPrompt(userPrompt, userReprompt, destinationContentTiles);

    // If cortana is dismissed in this operation, null is returned.
    var voiceCommandDisambiguationResult = await
        voiceServiceConnection.RequestDisambiguationAsync(response);
    if (voiceCommandDisambiguationResult != null)
    {
        return (Model.Trip)voiceCommandDisambiguationResult.SelectedItem.AppContext;
    }

    return null;
}
```

## <a name="error-screen"></a>错误屏幕

当无法完成由语音命令指定的操作时，应用服务可以提供错误屏幕。

下面是一个用于 **艾德 Works** 应用程序的错误屏幕的示例。 在此示例中，用户已指示应用服务通过 **Cortana** 取消到拉斯维加斯的行程。 但是，用户不会将任何行程安排到拉斯维加斯。

应用服务为 **Cortana** 提供一个错误屏幕，其中包括为操作自定义的消息、图标和特定的错误消息。

调用 [**ReportFailureAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) ，以在 **Cortana** 中显示错误屏幕。

```csharp
var userMessage = new VoiceCommandUserMessage();
    userMessage.DisplayMessage = userMessage.SpokenMessage = 
      "Sorry, you don't have any trips to Las Vegas";
                
    var response = VoiceCommandResponse.CreateResponse(userMessage);

    response.AppLaunchArgument = "showUpcomingTrips";
    await voiceServiceConnection.ReportFailureAsync(response);
```

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 设计准则](cortana-design-guidelines.md)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)