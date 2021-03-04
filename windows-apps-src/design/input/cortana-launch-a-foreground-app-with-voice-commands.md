---
title: 通过 Cortana 使用语音命令激活前台应用 - Cortana UWP 设计和开发
description: 使用语音命令激活应用到前台，并在应用中执行操作或命令。
ms.assetid: e4bf3714-6f62-466f-9e7c-3b03ee86a117
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 4b17a93d950d48209637cedf89bb49759ba0cf1f
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824321"
---
# <a name="activate-a-foreground-app-with-voice-commands-through-cortana"></a>通过 Cortana 使用语音命令激活前台应用

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

除了使用 **Cortana** 中的语音命令访问系统功能外，还可以使用应用中的特性和功能扩展 **cortana** 。 使用语音命令，可以将应用程序激活到前台，并在应用程序中执行一个操作或命令。

> [!NOTE]
> **重要的 API**
>
> - [**Windows.applicationmodel.resources.core. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

当应用程序在前台处理声音命令时，它会获得焦点并消除 Cortana。 如果愿意，可以激活应用程序并执行命令作为后台任务。 在这种情况下，Cortana 保留焦点，应用通过 **cortana** 画布和 **cortana** 语音返回所有反馈和结果。

需要额外的上下文或用户输入 (例如向特定联系人发送消息的语音命令) 最好在前台应用程序中进行处理，而基本命令 (如列出即将发生的行程) 可以通过后台应用在 **Cortana** 中处理。

如果要使用声音命令在后台激活应用，请参阅 [使用语音命令激活 Cortana 中的后台应用](cortana-launch-a-background-app-with-voice-commands.md)。

> [!NOTE]
> 语音命令是具有特定意向的单个查询文本，它在语音命令定义 (VCD) 文件中定义，通过 **Cortana** 定向到已安装的应用。
>
> VCD 文件定义一个或多个语音命令，每个命令都具有唯一的意图。
>
> 语音命令定义的复杂性可能会有所不同。 它们可以支持从单个、受约束的查询文本到一系列更灵活的自然语言最谈话的任何内容，所有这些都表示相同的目的。

为了演示前景应用功能，我们将使用来自 [Cortana voice 命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)的名为 "**艾德作品**" 的行程计划和管理应用。

若要在不使用 **Cortana** 的情况下创建新的 **艾德作品**，用户将启动该应用并导航到 **新的行程** 页面。 若要查看现有行程，用户将启动该应用程序，导航到即将发生的 **行程** 页面，然后选择行程。

通过 **Cortana** 使用语音命令，用户可以直接说 "艾德作品添加行程" 或 "在艾德作品上添加行程" 以启动应用程序并导航到 **新的行程** 页面。 反过来，说 "艾德公司向伦敦显示行程"，将启动该应用，并导航到 " **行程** 详细信息" 页面，如下所示。

:::image type="content" source="images/cortana/cortana-foreground-with-adventureworks.png" alt-text="Cortana 启动前台应用程序的屏幕截图":::

下面是使用语音或键盘输入添加语音命令功能并将 Cortana 与应用集成的基本步骤：

1. 创建一个 VCD 文件。 这是一个 XML 文档，用于定义用户在激活应用程序时可用于启动操作或调用命令的所有讲述命令。 请参阅 [**VCD 元素和属性**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)。
2. 当应用程序启动时，在 VCD 文件中注册命令集。
3. 处理激活语音命令，在应用内导航并执行命令。

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

## <a name="create-a-new-solution-with-project-in-visual-studio"></a>使用 Visual Studio 中的项目创建新的解决方案

1. 启动 Microsoft Visual Studio 2015。

    此时将显示 "Visual Studio 2015 起始页"。

2. 在“文件”菜单中，选择“新建” > “项目”。

    此时将显示 " **新建项目** " 对话框。 在对话框的左窗格中，可以选择要显示的模板的类型。

3. 在左侧窗格中，展开 " **已安装的 > 模板 > Visual C \# > Windows**"，然后选择 " **通用** 模板" 组。 对话框的中心窗格显示通用 Windows 平台 (UWP) 应用程序的项目模板的列表。
4. 在中心窗格中，选择 " **空白应用 (通用 Windows)** 模板。

    " **空白应用程序** " 模板创建一个编译和运行的最小 UWP 应用，但不包含用户界面控件或数据。 在本教程的过程中，您将向应用程序添加控件。

5. 在 " **名称** " 文本框中，键入项目名称。 在此示例中，我们使用 "AdventureWorks"。
6. 单击“确定”以创建该项目  。

    Microsoft Visual Studio 创建项目并将其显示在 **解决方案资源管理器** 中。

## <a name="add-image-assets-to-project-and-specify-them-in-the-app-manifest"></a>将图像资产添加到项目，并在应用程序清单中指定它们

UWP 应用可以根据特定的设置和设备功能自动选择最适合的图像 (高对比度、有效像素、区域设置等) 。 你需要做的只是提供映像，并确保在应用项目中对不同资源版本使用适当的命名约定和文件夹组织。 如果不提供建议的资源版本、可访问性、本地化和图像质量，这取决于用户的首选项、功能、设备类型和位置。

若要详细了解高对比度和缩放比例的图像资源，请参阅 [磁贴和图标资产的准则](../../app-resources/images-tailored-for-scale-theme-contrast.md)。

使用限定符命名资源。 资源限定符是文件夹和文件名修饰符，用于标识应在其中使用特定版本资源的上下文。

标准命名约定是 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` 。 例如， `images/logo.scale-100_contrast-white.png` 可在代码中只使用根文件夹和文件名进行引用： `images/logo.png` 。 请参阅 [如何使用限定符命名资源](/previous-versions/windows/apps/hh965324(v=win.10))。

建议您将字符串资源文件上的默认语言 (例如 `en-US\resources.resw`) 和图像上的默认缩放比例 (如 `logo.scale-100.png`) ，即使您当前不打算提供本地化或多个解析资源。 但至少建议为100、200和400规模因素提供资产。

> [!IMPORTANT]
> **Cortana** 画布的标题区中使用的应用图标是在 "appxmanifest.xml" 文件中指定的 Square44x44Logo 图标。

## <a name="create-a-vcd-file"></a>创建 VCD 文件

1. 在 Visual Studio 中，右键单击主项目名称，选择 " **添加 > 新项**"。 添加 **XML 文件**。
2. 在此示例中键入 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 文件的名称， ("AdventureWorksCommands.xml" ) ，并单击 "添加"。
3. 在 **解决方案资源管理器** 中，选择 " [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) " 文件。
4. 在 " **属性** " 窗口中，将 " **生成操作** " 设置为 " **内容**"，然后将 " **复制到输出目录** " 设置为 "要 **复制**"

## <a name="edit-the-vcd-file"></a>编辑 VCD 文件

添加一个 **VoiceCommands** 元素，该元素具有指向的 **xmlns** 属性 `https://schemas.microsoft.com/voicecommands/1.2` 。

1. 对于您的应用程序支持的每种语言，请创建一个 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 元素，其中包含应用程序支持的语音命令。

   可以声明多个 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 元素，每个元素都有不同的 [**xml： lang**](/previous-versions/windows/dn722331(v=win.10)) 特性，以便在不同的市场中使用应用。 例如，美国的应用程序可能具有英语的 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 和西班牙语的 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 。

   > [!CAUTION]
   > 若要激活应用程序并使用语音命令启动操作，应用程序必须注册一个 VCD 文件，其中包含的 [**CommandSet**](/previous-versions/windows/dn722331(v=win.10)) 的语言与用户为其设备选择的语言匹配。 语音语言位于 "设置" **> System > speech > 语音语言**。

2. 为要支持的每个命令添加 **command** 元素。 在 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)文件中声明的每个 **命令** 都必须包含以下信息：

   - 应用程序在运行时用于标识语音命令的 **AppName** 特性。
   - 一个 **示例** 元素，其中包含描述用户如何调用命令的短语。 当用户显示 "我可以说什么？"、"帮助" 或点击 **查看详细信息** 时， **Cortana** 会显示此示例。
   - 一个 **ListenFor** 元素，其中包含应用将其识别为命令的单词或短语。 每个 **ListenFor** 元素都可以包含对一个或多个 **PhraseList** 元素的引用，这些元素包含与命令相关的特定词。
  
> [!NOTE]
> 无法以编程方式修改 **ListenFor** 元素。 但是，可以通过编程方式修改与 **ListenFor** 元素关联的 **PhraseList** 元素。 应用程序应基于用户使用应用时生成的数据集在运行时修改 **PhraseList** 的内容。 请参阅 [动态修改 CORTANA VCD 短语列表](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)。

一个 **反馈** 元素，其中包含 **Cortana** 用于在启动应用程序时显示和讲话的文本。

一个 **导航** 元素，用于指示语音命令激活应用程序到前台。 在此示例中，该 `showTripToDestination` 命令是前台任务。

**VoiceCommandService** 元素指示语音命令在后台激活应用。 此元素的 **Target** 属性的值应与 appxmanifest.xml 文件中 [**uap： AppService**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice)元素的 **Name** 属性的值相匹配。 在此示例中， `whenIsTripToDestination` 和 `cancelTripToDestination` 命令是将应用服务的名称指定为 "AdventureWorksVoiceCommandService" 的后台任务。

有关更多详细信息，请参阅 [**VCD 元素和属性 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 参考。

下面是用于定义 **艾德作品** 应用的 en-us 声音命令的 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)文件的一部分。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.2">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> Show trip to London </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> show [my] trip to {destination} </ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> show [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate />
    </Command>

    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> when is [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> when is [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Looking for trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>
    
    <Command Name="cancelTripToDestination">
      <Example> Cancel my trip to Las Vegas </Example>
      <ListenFor RequireAppName="BeforeOrAfterPhrase"> cancel [my] trip to {destination}</ListenFor>
      <ListenFor RequireAppName="ExplicitlySpecified"> cancel [my] {builtin:AppName} trip to {destination} </ListenFor>
      <Feedback> Cancelling trip to {destination}</Feedback>
      <VoiceCommandService Target="AdventureWorksVoiceCommandService"/>
    </Command>

    <PhraseList Label="destination">
      <Item>London</Item>
      <Item>Las Vegas</Item>
      <Item>Melbourne</Item>
      <Item>Yosemite National Park</Item>
    </PhraseList>
  </CommandSet>
```

## <a name="install-the-vcd-commands"></a>安装 VCD 命令

您的应用程序必须运行一次才能安装 VCD。

> [!NOTE]
> 不会在应用安装中保留语音命令数据。 若要确保你的应用程序的语音命令数据保持不变，请考虑在每次启动或激活应用时初始化 VCD 文件，或维护一个设置以指示当前是否已安装了 VCD。

在 "app.xaml.cs" 文件中：

1. 添加以下 using 指令：

    ```csharp
    using Windows.Storage;
    ```

1. 用 async 修饰符标记 "OnLaunched" 方法。  

    ```csharp
    protected async override void OnLaunched(LaunchActivatedEventArgs e)
    ```

1. 在 [**OnLaunched**](/uwp/api/Windows.UI.Xaml.Application)处理程序中调用 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) ，以注册系统应识别的语音命令。

  在艾德作品示例中，我们首先定义一个 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 对象。

  然后，调用 [**document.getfileasync**](/uwp/api/Windows.Storage.StorageFolder) 以通过 "AdventureWorksCommands.xml" 文件对其进行初始化。

  然后将此 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 对象传递给 [**InstallCommandDefinitionsFromStorageFileAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)。

```csharp
try
{
  // Install the main VCD. 
  StorageFile vcdStorageFile = 
  await Package.Current.InstalledLocation.GetFileAsync(
  @"AdventureWorksCommands.xml");

  await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);

  // Update phrase list.
  ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
  if(locator != null)
  {
     await locator.TripViewModel.UpdateDestinationPhraseList();
  }
}
catch (Exception ex)
{
  System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
}
```

## <a name="handle-activation-and-execute-voice-commands"></a>处理激活和执行语音命令

指定应用程序在一次启动后 (响应后续语音命令激活的方式，并) 安装语音命令集。

1. 确认你的应用已通过语音命令激活。

    重写 [**OnActivated**](/uwp/api/Windows.UI.Xaml.Application) 事件并检查 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)。[**类型**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 为 [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。

2. 确定命令名称以及所讲述的内容。

    从 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)获取对 [**VoiceCommandActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)对象的引用并查询 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)对象的 [**Result**](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)属性。

    若要确定用户所说的内容，请检查 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)字典中已识别的短语的 [**Text**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult)或语义属性的值。

3. 在应用中采取适当的措施，例如导航到所需的页面。

在此示例中，我们将返回到步骤3：编辑 VCD 文件中的 VCD。

获取语音命令的语音识别结果后，将从 [**RulePath**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 数组中的第一个值获取命令名称。 当 VCD 文件定义了多个可能的语音命令时，需要将值与 VCD 中的命令名进行比较，并采取适当的措施。

应用程序最常见的操作是导航到包含与声音命令上下文相关的内容的页面。 在此示例中，我们将导航到 **TripPage** 页，并传入 voice 命令的值、命令的输入方式以及可识别的 "destination" 短语 (（如果适用）) 。 或者，当导航到页面时，应用程序可以向 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 发送导航参数。

可以通过使用 **commandMode** 键的 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)字典，查明启动应用程序的语音命令是否确实被口述，或是否以文本方式键入。 该键的值将是 "voice" 或 "text"。 如果键的值为 "voice"，请考虑在应用中使用语音合成 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) 为用户提供口头反馈。

使用 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation)可查看 **ListenFor** 元素的 **PhraseList** 或 **PhraseTopic** 约束中讲述的内容。 字典键是 **PhraseList** 或 **PhraseTopic** 元素的 **Label** 特性的值。 在这里，我们将演示如何访问 **{destination}** 短语的值。

```csharp
/// <summary>
/// Entry point for an application activated by some means other than normal launching. 
/// This includes voice commands, URI, share target from another app, and so on. 
/// 
/// NOTE:
/// A previous version of the VCD file might remain in place 
/// if you modify it and update the app through the store. 
/// Activations might include commands from older versions of your VCD. 
/// Try to handle these commands gracefully.
/// </summary>
/// <param name="args">Details about the activation method.</param>
protected override void OnActivated(IActivatedEventArgs args)
{
    base.OnActivated(args);

    Type navigationToPageType;
    ViewModel.TripVoiceCommand? navigationCommand = null;

    // Voice command activation.
    if (args.Kind == ActivationKind.VoiceCommand)
    {
        // Event args can represent many different activation types. 
        // Cast it so we can get the parameters we care about out.
        var commandArgs = args as VoiceCommandActivatedEventArgs;

        Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;

        // Get the name of the voice command and the text spoken. 
        // See VoiceCommands.xml for supported voice commands.
        string voiceCommandName = speechRecognitionResult.RulePath[0];
        string textSpoken = speechRecognitionResult.Text;

        // commandMode indicates whether the command was entered using speech or text.
        // Apps should respect text mode by providing silent (text) feedback.
        string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
        
        switch (voiceCommandName)
        {
            case "showTripToDestination":
                // Access the value of {destination} in the voice command.
                string destination = this.SemanticInterpretation("destination", speechRecognitionResult);

                // Create a navigation command object to pass to the page. 
                navigationCommand = new ViewModel.TripVoiceCommand(
                    voiceCommandName,
                    commandMode,
                    textSpoken,
                    destination);

                // Set the page to navigate to for this voice command.
                navigationToPageType = typeof(View.TripDetails);
                break;
            default:
                // If we can't determine what page to launch, go to the default entry point.
                navigationToPageType = typeof(View.TripListView);
                break;
        }
    }
    // Protocol activation occurs when a card is clicked within Cortana (using a background task).
    else if (args.Kind == ActivationKind.Protocol)
    {
        // Extract the launch context. In this case, we're just using the destination from the phrase set (passed
        // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
        // identifier is ideal for more complex scenarios. We let the destination page check if the 
        // destination trip still exists, and navigate back to the trip list if it doesn't.
        var commandArgs = args as ProtocolActivatedEventArgs;
        Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
        var destination = decoder.GetFirstValueByName("LaunchContext");

        navigationCommand = new ViewModel.TripVoiceCommand(
                                "protocolLaunch",
                                "text",
                                "destination",
                                destination);

        navigationToPageType = typeof(View.TripDetails);
    }
    else
    {
        // If we were launched via any other mechanism, fall back to the main page view.
        // Otherwise, we'll hang at a splash screen.
        navigationToPageType = typeof(View.TripListView);
    }

    // Repeat the same basic initialization as OnLaunched() above, taking into account whether
    // or not the app is already active.
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active.
    if (rootFrame == null)
    {
        // Create a frame to act as the navigation context and navigate to the first page.
        rootFrame = new Frame();
        App.NavigationService = new NavigationService(rootFrame);

        rootFrame.NavigationFailed += OnNavigationFailed;

        // Place the frame in the current window.
        Window.Current.Content = rootFrame;
    }

    // Since we're expecting to always show a details page, navigate even if 
    // a content frame is in place (unlike OnLaunched).
    // Navigate to either the main trip list page, or if a valid voice command
    // was provided, to the details page for that trip.
    rootFrame.Navigate(navigationToPageType, navigationCommand);

    // Ensure the current window is active
    Window.Current.Activate();
}

/// <summary>
/// Returns the semantic interpretation of a speech result. 
/// Returns null if there is no interpretation for that key.
/// </summary>
/// <param name="interpretationKey">The interpretation key.</param>
/// <param name="speechRecognitionResult">The speech recognition result to get the semantic interpretation from.</param>
/// <returns></returns>
private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult)
{
  return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
}
```

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [使用语音命令激活 Cortana 中的后台应用](cortana-launch-a-background-app-with-voice-commands.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 设计准则](cortana-design-guidelines.md)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)