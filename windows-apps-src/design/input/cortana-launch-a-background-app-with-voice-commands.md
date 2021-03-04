---
title: 使用语音命令激活 Cortana 中的后台应用 | Cortana UWP 设计和开发
description: 使用语音命令) ，使用应用中的功能扩展 Cortana (作为后台任务。
ms.assetid: e2c7eae3-6beb-4156-92a5-474bba53451e
ms.date: 09/24/2019
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 987a8f4146e5f97c0338c6a8492ace050fbaa3d0
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824631"
---
# <a name="activate-a-background-app-in-cortana-using-voice-commands"></a>使用语音命令激活 Cortana 中的后台应用  

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

除了使用 **Cortana** 中的语音命令访问系统功能外，还可以使用指定要运行的操作或命令的语音命令) ，使用应用中的特性和功能扩展 **Cortana** (作为后台任务。 当应用程序在后台处理声音命令时，它不会获得焦点。 相反，它会通过 **cortana** 画布和 **cortana** 语音返回所有反馈和结果。  

> [!NOTE]
> **重要的 API**
>
> - [**Windows.applicationmodel.resources.core. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

应用可能会被激活到前台 (应用将焦点) 或在后台激活 (**Cortana** 会根据交互的复杂程度保留) 。 例如，需要额外的上下文或用户输入的语音命令 (例如，将消息发送到特定联系人) 最好在前台应用程序中进行处理，而基本命令 (如列出即将发生的行程) 可以通过后台应用在 **Cortana** 中处理。  

如果要使用声音命令激活应用到前台，请参阅 [通过 Cortana 使用语音命令激活前台应用](cortana-launch-a-foreground-app-with-voice-commands.md)。  

> [!NOTE]
> 语音命令是具有特定意向的单个查询文本，它在语音命令定义 (VCD) 文件中定义，通过 **Cortana** 定向到已安装的应用。
>
> VCD 文件定义一个或多个语音命令，每个命令都具有唯一的意图。
>
> 语音命令定义的复杂性可能会有所不同。 它们可以支持从单个、受约束的查询文本到一系列更灵活的自然语言最谈话的任何内容，所有这些都表示相同的目的。

我们使用名为 " **艾德作品** 集成到 **Cortana** UI" 的行程计划和管理应用，其中显示了我们讨论的许多概念和功能。 有关详细信息，请参阅 [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)。

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 启动前台应用程序的屏幕截图":::

若要查看无 **Cortana** 的 **艾德公司** 行程，用户将启动该应用，并导航到 **即将** 发生的行程页面。  

通过 **Cortana** 使用声音命令在后台启动你的应用程序，用户可能只是说 `Adventure Works, when is my trip to Las Vegas?` 。 应用程序将处理命令， **Cortana** 会显示结果以及应用图标和其他应用信息（如果提供）。  

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="在后台使用 AdventureWorks 应用的基本查询和结果屏幕显示 Cortana 的屏幕截图":::

以下基本步骤使用语音或键盘输入添加语音命令功能并使用应用的后台功能扩展 **Cortana** 。

1. 创建应用服务 (请参阅在后台调用 **Cortana**) 的 [**windows.applicationmodel.resources.core. AppService。**](/uwp/api/Windows.ApplicationModel.AppService)  
2. 创建一个 VCD 文件。 VCD 文件是一个 XML 文档，该文档定义用户在激活应用程序时可能会说启动操作或调用命令的所有口头命令。 请参阅 [**VCD 元素和属性**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)。  
3. 当应用程序启动时，在 VCD 文件中注册命令集。  
4. 处理应用服务的后台激活和语音命令的运行。  
5. 向 **Cortana** 内的语音命令显示和朗读相应的反馈。  

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

## <a name="create-a-new-solution-with-a-primary-project-in-visual-studio"></a>在 Visual Studio 中使用主项目创建新的解决方案  

1. 启动 Microsoft Visual Studio 2015。  
    此时将显示 "Visual Studio 2015 起始页"。  

2. 在“文件”菜单中，选择“新建” > “项目”。  
    此时将显示 " **新建项目** " 对话框。 在对话框的左窗格中，可以选择要显示的模板的类型。  

3. 在左侧窗格中，展开 " **已安装的 > 模板 > Visual C \# > Windows**"，然后选择 " **通用** 模板" 组。 对话框的中心窗格显示通用 Windows 平台 (UWP) 应用程序的项目模板的列表。  
4. 在中心窗格中，选择 " **空白应用 (通用 Windows)** 模板。  
    " **空白应用程序** " 模板创建一个编译和运行的最小 UWP 应用。 **空白应用** 模板不包含用户界面控件或数据。 使用此页作为指南将控件添加到应用。  

5. 在 " **名称** " 文本框中，键入项目名称。 示例：使用 `AdventureWorks` 。  
6. 单击 " **确定"** 按钮创建项目。  
    Microsoft Visual Studio 创建项目并将其显示在 **解决方案资源管理器** 中。  

## <a name="add-image-assets-to-primary-project-and-specify-them-in-the-app-manifest"></a>将图像资产添加到主项目，并在应用程序清单中指定它们  

UWP 应用应自动选择最适合的映像。 选择基于特定设置和设备功能， (高对比度、有效像素、区域设置等) 。 必须提供映像，并确保在应用项目中对不同资源版本使用适当的命名约定和文件夹组织。  
如果未提供推荐的资源版本，则用户体验可能会受到以下方式的影响。

- 辅助功能
- 本地化  
- 图像质量  
资源版本用于改编用户体验中的以下更改。  
- 用户首选项  
- 功能  
- 设备类型  
- 位置  

若要详细了解高对比度和缩放比例的图像资源，请访问位于 [msdn.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-app-assets](../../app-resources/images-tailored-for-scale-theme-contrast.md)的磁贴和图标资产页面的指导原则。  

必须使用限定符命名资源。 资源限定符是文件夹和文件名修饰符，用于标识应在其中使用特定版本资源的上下文。  

标准命名约定是 `foldername/qualifiername-value[_qualifiername-value]/filename.qualifiername-value[_qualifiername-value].ext` 。  
例如： `images/logo.scale-100_contrast-white.png` ，这可能是使用根文件夹和文件名：引用代码 `images/logo.png` 。  
有关详细信息，请参阅位于 [msdn.microsoft.com/library/windows/apps/xaml/hh965324.aspx](/previous-versions/windows/apps/hh965324(v=win.10))的如何使用限定符命名资源页。  

Microsoft 建议您将字符串资源文件上的默认语言 (例如 `en-US\resources.resw`) 和 () 之类的图像上的默认缩放比例进行标记 `logo.scale-100.png` ，即使您当前不打算提供本地化或多个解析资源。 不过，Microsoft 建议你至少为100、200和400规模因素提供资产。  

>[!IMPORTANT]
> **Cortana** 画布的标题区中使用的应用图标是在文件中指定的 Square44x44Logo 图标 `Package.appxmanifest` 。  
> 你还可以为 **Cortana** 画布的 "内容" 区域中的每个条目指定一个图标。 结果图标的有效图像大小为：
>
> - 68w x 68h
> - 68w x 92h  
> - 280w x 140h  

在将 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 对象传递给 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 类之前，不会验证内容磁贴。 如果将 [**VoiceCommandResponse**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 对象传递到 **Cortana** ，其中包含的内容磁贴带有不符合这些大小比例的图像，则可能会出现异常。  

示例：**艾德作品** 应用 (`VoiceCommandService\\AdventureWorksVoiceCommandService.cs`) `GreyTile.png` 使用 [**TitleWith68x68IconAndText**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType)图块模板指定 [**VoiceCommandContentTile**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTile)类上的一个简单的灰色正方形 () 。 徽标变量位于 `VoiceCommandService\\Images` ，并使用 [**GetFileFromApplicationUriAsync**](/uwp/api/Windows.Storage.StorageFile) 方法进行检索。

```csharp
var destinationTile = new VoiceCommandContentTile();  

destinationTile.ContentTileType = VoiceCommandContentTileType.TitleWith68x68IconAndText;
destinationTile.Image = await StorageFile.GetFileFromApplicationUriAsync(
    new Uri("ms-appx:///AdventureWorks.VoiceCommands/Images/GreyTile.png")
);  
```

## <a name="create-an-app-service-project"></a>创建应用服务项目

1. 右键单击解决方案名称，选择 "新建" " **> 项目**"。  
2. 在 " **已安装 > 模板 > Visual C \# > Windows > 通用**" 下，选择 " **Windows 运行时组件**"。 **Windows 运行时组件** 是 ([**AppService**](/uwp/api/Windows.ApplicationModel.AppService)) 实现应用服务的组件。  
3. 键入项目的名称，然后单击 **"确定"** 按钮。  
    示例：`VoiceCommandService`。  
4. 在 **解决方案资源管理器** 中，选择该 `VoiceCommandService` 项目并重命名 `Class1.cs` 由 Visual Studio 生成的文件。
    示例： **艾德公司** 使用 `AdventureWorksVoiceCommandService.cs` 。  
5. 单击 " **是"** 按钮;当系统询问你是否要重命名出现的所有项时 `Class1.cs` 。  
6. 在 `AdventureWorksVoiceCommandService.cs` 文件中：
    1. 添加以下 using 指令。  
        `using Windows.ApplicationModel.Background;`  
    2. 创建新项目时，项目名称将用作所有文件中的默认根命名空间。 重命名命名空间，以便在主项目下嵌套应用服务代码。
        示例：`namespace AdventureWorks.VoiceCommands`。  
    3. 在解决方案资源管理器中右键单击应用服务项目名称，然后选择 " **属性**"。  
    4. 在 " **库** " 选项卡上，将 " **默认命名空间** " 字段更新为此相同的值。  
        示例：`AdventureWorks.VoiceCommands`）。  
    5. 创建一个用于实现 [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 接口的新类。 此类需要 [**Run**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 方法，这是 Cortana 识别语音命令时的入口点。  

    示例：来自 **艾德作品** 应用的基本后台任务类。  

    >[!NOTE]
    > 后台任务类本身以及后台任务项目中的所有类都必须是密封的公共类。  

    ```csharp
    namespace AdventureWorks.VoiceCommands
    {
        ...
        
        /// <summary>
        /// The VoiceCommandService implements the entry point for all voice commands.
        /// The individual commands supported are described in the VCD xml file. 
        /// The service entry point is defined in the appxmanifest.
        /// </summary>
        public sealed class AdventureWorksVoiceCommandService : IBackgroundTask
        {
            ...

            /// <summary>
            /// The background task entrypoint. 
            /// 
            /// Background tasks must respond to activation by Cortana within 0.5 second, and must 
            /// report progress to Cortana every 5 seconds (unless Cortana is waiting for user
            /// input). There is no running time limit on the background task managed by Cortana,
            /// but developers should use plmdebug (https://msdn.microsoft.com/library/windows/hardware/jj680085%28v=vs.85%29.aspx)
            /// on the Cortana app package in order to prevent Cortana timing out the task during
            /// debugging.
            /// 
            /// The Cortana UI is dismissed if Cortana loses focus. 
            /// The background task is also dismissed even if being debugged. 
            /// Use of Remote Debugging is recommended in order to debug background task behaviors. 
            /// Open the project properties for the app package (not the background task project), 
            /// and enable Debug -> "Do not launch, but debug my code when it starts". 
            /// Alternatively, add a long initial progress screen, and attach to the background task process while it runs.
            /// </summary>
            /// <param name="taskInstance">Connection to the hosting background service process.</param>
            public void Run(IBackgroundTaskInstance taskInstance)
            {
              //
              // TODO: Insert code 
              //
              //
        }
      }
    }
    ```  

7. 将后台任务声明为应用程序清单中的 **AppService** 。  
    1. 在 **解决方案资源管理器** 中，右键单击该 `Package.appxmanifest` 文件，然后选择 " **查看代码**"。  
    2. 查找 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素。  
    3. 向 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素添加元素 [`Application`](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 。  
    4. 向 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 元素添加元素 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 。  
    5. 将 `Category` 属性添加到 `uap:Extension` 元素，并将属性的值设置 `Category` 为 `windows.appService` 。  
    6. 将 `EntryPoint` 属性添加到 `uap: Extension` 元素，并将属性的值设置 `EntryPoint` 为实现的类的名称 [`IBackgroundTask`](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) 。  
        示例：`AdventureWorks.VoiceCommands.AdventureWorksVoiceCommandService`。  
    7. 向 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 元素添加元素 `uap:Extension` 。  
    8. 将 `Name` 属性添加到 [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) 元素，并将属性的值设置 `Name` 为应用服务的名称（在本例中为） `AdventureWorksVoiceCommandService` 。  
    9. 将第二个 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 元素添加到 [`Extensions`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素。  
    10. 将 `Category` 属性添加到此 [`uap:Extension`](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extension) 元素，并将属性的值设置 `Category` 为 `windows.personalAssistantLaunch` 。  

    示例：来自艾德作品应用的清单。

    ```xml
    <Package>
        <Applications>
            <Application>
            
                <Extensions>
                    <uap:Extension Category="windows.appService" EntryPoint="CortanaBack1.VoiceCommands.AdventureWorksVoiceCommandService">
                        <uap:AppService Name="AdventureWorksVoiceCommandService"/>
                    </uap:Extension>
                    <uap:Extension Category="windows.personalAssistantLaunch"/>
                </Extensions>
                
            <Application>
        <Applications>
    </Package>
    ```  

8. 将此应用服务项目添加为主项目中的引用。  
    1. 右键单击 " **引用**"。  
    2. 选择 " **添加引用 ...**"。  
    3. 在 " **引用管理器** " 对话框中，展开 " **项目** "，然后选择 "应用服务" 项目。  
    4. 单击 **"确定"** 按钮。  

## <a name="create-a-vcd-file"></a>创建 VCD 文件

1. 在 Visual Studio 中，右键单击主项目名称，选择 " **添加 > 新项**"。 添加 **XML 文件**。  
2. 键入 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 文件的名称。  
    示例：`AdventureWorksCommands.xml`。
3. 单击 " **添加** " 按钮。  
4. 在 **解决方案资源管理器** 中，选择 " [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) " 文件。  
5. 在 " **属性** " 窗口中，将 " **生成操作** " 设置为 " **内容**"，然后将 " **复制到输出目录** " 设置为 "要 **复制**"  

## <a name="edit-the-vcd-file"></a>编辑 VCD 文件  

1. 添加一个 `VoiceCommands` 具有指向的 `xmlns` 特性的元素 `https://schemas.microsoft.com/voicecommands/1.2` 。  
2. 对于您的应用程序支持的每种语言，创建一个 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 包含您的应用程序所支持的语音命令的元素。  
    您可以声明多个 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 元素，每个元素都有一个不同的 [`xml:lang`](/previous-versions/windows/dn722331(v=win.10)) 属性，因此您的应用程序可在不同的市场中使用。 例如，适用于美国的应用程序可能具有 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 适用于英语和 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 西班牙语的。  

    >[!IMPORTANT]
    > 若要激活应用程序并使用语音命令启动操作，应用程序必须注册一个 VCD 文件，其中包含的 [`CommandSet`](/previous-versions/windows/dn722331(v=win.10)) 元素具有与用户的设备中所指示的语言相匹配的语言。 语音语言位于 "设置" **> System > speech > 语音语言**。  

3. `Command`为要支持的每个命令添加一个元素。  
    `Command`在 [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)文件中声明的每个都必须包含以下信息：  
    - `Name`应用程序在运行时用于标识语音命令的属性。  
    - 一个 `Example` 元素，其中包含描述用户如何调用命令的短语。 当用户显示 `What can I say?` 、 `Help` 或点击 "**查看更多**" 时，Cortana 会显示该示例。  
    - 一个 `ListenFor` 元素，其中包含应用将其识别为命令的单词或短语。 每个 `ListenFor` 元素都可能包含对一个或多个 `PhraseList` 元素的引用，这些元素包含与命令相关的特定词。  

       >[!NOTE]
       > `ListenFor` 不能以编程方式修改元素。 但是， `PhraseList` `ListenFor` 可以通过编程方式修改与元素关联的元素。 应用程序应 `PhraseList` 基于用户使用应用时生成的数据集来修改运行时元素的内容。
       >
       > 有关详细信息，请参阅 [动态修改 CORTANA VCD 短语列表](cortana-dynamically-modify-voice-command-definition-vcd-phrase-lists.md)。  

    - 一个 `Feedback` 元素，其中包含 **Cortana** 要显示的文本，并在启动应用程序时进行口述。  

一个 `Navigate` 元素，用于指示语音命令激活应用程序到前台。 在此示例中，该 ```showTripToDestination``` 命令是前台任务。  

一个 `VoiceCommandService` 元素，指示语音命令在后台激活应用。 `Target`此元素的属性的值应与 `Name` [`uap:AppService`](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-appservice) appxmanifest.xml 文件中元素的属性的值相匹配。 在此示例中， `whenIsTripToDestination` 和 `cancelTripToDestination` 命令是将应用服务的名称指定为的后台任务 `AdventureWorksVoiceCommandService` 。  

有关更多详细信息，请参阅 [**VCD 元素和属性 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 参考。  

示例： [**VCD**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 文件的一部分，用于定义 `en-us` **艾德公司** 应用的语音命令。  

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

>[!NOTE]
> 不会在应用安装中保留语音命令数据。 若要确保你的应用程序的语音命令数据保持不变，请考虑在每次启动或激活应用时初始化 VCD 文件，或维护一个设置以指示当前是否已安装了 VCD。  

在 `app.xaml.cs` 文件中：  

1. 添加以下 using 指令：

   ```csharp
   using Windows.Storage;
   ```

2. `OnLaunched`用 async 修饰符标记方法。  

   ```csharp
   protected async override void OnLaunched(LaunchActivatedEventArgs e)
   ```  

3. [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager)在处理程序中调用方法 [`OnLaunched`](/uwp/api/Windows.UI.Xaml.Application) ，以注册应识别的语音命令。  
    示例：艾德作品应用定义一个 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) 对象。  
    示例：调用 [`GetFileAsync`](/uwp/api/Windows.Storage.StorageFolder) 方法以初始化 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) 包含文件的对象 `AdventureWorksCommands.xml` 。  
    然后，将 [`StorageFile`](/uwp/api/Windows.Storage.StorageFile) 对象传递给 [`InstallCommandDefinitionsFromStorageFileAsync`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager) 方法。  

   ```csharp
   try {
      // Install the main VCD. 
      StorageFile vcdStorageFile = await Package.Current.InstalledLocation.GetFileAsync(
            @"AdventureWorksCommands.xml"
      );
       
      await Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.InstallCommandDefinitionsFromStorageFileAsync(vcdStorageFile);
     
      // Update phrase list.
      ViewModel.ViewModelLocator locator = App.Current.Resources["ViewModelLocator"] as ViewModel.ViewModelLocator;
      if(locator != null) {
            await locator.TripViewModel.UpdateDestinationPhraseList();
        }
    }
    catch (Exception ex) {
        System.Diagnostics.Debug.WriteLine("Installing Voice Commands Failed: " + ex.ToString());
    }
    ```  

## <a name="handle-activation"></a>处理激活  

指定应用响应后续语音命令激活的方式。

>[!NOTE]
> 安装 voice 命令集之后，必须至少启动一次应用。  

1. 确认你的应用已通过语音命令激活。  

    重写 [`Application.OnActivated`](/uwp/api/Windows.UI.Xaml.Application) 事件并检查 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)。[**类型**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs) 为 [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.Activation.ActivationKind)。  

2. 确定命令名称以及所讲述的内容。  

    [`VoiceCommandActivatedEventArgs`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs)从 [**IActivatedEventArgs**](/uwp/api/Windows.ApplicationModel.Activation.IActivatedEventArgs)获取对象的引用并查询 [`Result`](/uwp/api/Windows.ApplicationModel.Activation.VoiceCommandActivatedEventArgs) 对象的属性 [`SpeechRecognitionResult`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。  

    若要确定用户所说的内容，请检查字典中已识别的短语的 [**文本**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 值或语义属性 [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 。  

3. 在应用中采取适当的措施，例如导航到所需的页面。  

    >[!NOTE]
    > 如果需要参阅 VCD，请访问 [编辑 Vcd 文件](#edit-the-vcd-file) 部分。

    接收到语音命令的语音识别结果后，可以从数组中的第一个值获取命令名称 [`RulePath`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。 由于 VCD 文件定义了多个可能的语音命令，因此必须验证该值是否与 VCD 中的命令名称匹配，并采取适当的措施。  

    对于应用程序而言，最常见的操作是导航到一个页面，其中包含与声音命令上下文相关的内容。  
    示例：打开 **TripPage** 页面并传入 voice 命令的值、命令输入方式以及可识别的目标短语 (（如果适用）) 。 或者，当导航到 **TripPage** 页面时，应用可能会将导航参数发送到 [**SpeechRecognitionResult**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 。  

    您可以通过 [`SpeechRecognitionSemanticInterpretation.Properties`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 使用 **commandMode** 键来确定启动应用程序的语音命令是否实际被口述，或是否以文本形式键入。 该键的值将为 `voice` 或 `text` 。 如果键的值为，请 `voice` 考虑在应用中使用语音合成 ([**SpeechSynthesis**](/uwp/api/Windows.Media.SpeechSynthesis)) 为用户提供口头反馈。  

    使用 [**SpeechRecognitionSemanticInterpretation**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 可查看元素的或约束中所讲述的内容 `PhraseList` `PhraseTopic` `ListenFor` 。 字典键是 `Label` 或元素的特性的值 `PhraseList` `PhraseTopic` 。
    示例：以下代码介绍了如何访问 **{destination}** 短语的值。  

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
    protected override void OnActivated(IActivatedEventArgs args) {
        base.OnActivated(args);
        
        Type navigationToPageType;
        ViewModel.TripVoiceCommand? navigationCommand = null;
        
        // Voice command activation.
        if (args.Kind == ActivationKind.VoiceCommand) {
            // Event args may represent many different activation types. 
            // Cast the args so that you only get useful parameters out.
            var commandArgs = args as VoiceCommandActivatedEventArgs;
            
            Windows.Media.SpeechRecognition.SpeechRecognitionResult speechRecognitionResult = commandArgs.Result;
            
            // Get the name of the voice command and the text spoken.
            // See VoiceCommands.xml for supported voice commands.
            string voiceCommandName = speechRecognitionResult.RulePath[0];
            string textSpoken = speechRecognitionResult.Text;
            
            // commandMode indicates whether the command was entered using speech or text.
            // Apps should respect text mode by providing silent (text) feedback.
            string commandMode = this.SemanticInterpretation("commandMode", speechRecognitionResult);
            
            switch (voiceCommandName) {
                case "showTripToDestination":
                    // Access the value of {destination} in the voice command.
                    string destination = this.SemanticInterpretation("destination", speechRecognitionResult);
                    
                    // Create a navigation command object to pass to the page.
                    navigationCommand = new ViewModel.TripVoiceCommand(
                        voiceCommandName,
                        commandMode,
                        textSpoken,
                        destination
                    );
              
                    // Set the page to navigate to for this voice command.
                    navigationToPageType = typeof(View.TripDetails);
                    break;
                default:
                    // If not able to determine what page to launch, then go to the default entry point.
                    navigationToPageType = typeof(View.TripListView);
                    break;
            }
        }
        // Protocol activation occurs when a card is selected within Cortana (using a background task).
        else if (args.Kind == ActivationKind.Protocol) {
            // Extract the launch context. In this case, use the destination from the phrase set (passed
            // along in the background task inside Cortana), which makes no attempt to be unique. A unique id or 
            // identifier is ideal for more complex scenarios. The destination page is left to check if the 
            // destination trip still exists, and navigate back to the trip list if it does not.
            var commandArgs = args as ProtocolActivatedEventArgs;
            Windows.Foundation.WwwFormUrlDecoder decoder = new Windows.Foundation.WwwFormUrlDecoder(commandArgs.Uri.Query);
            var destination = decoder.GetFirstValueByName("LaunchContext");
            
            navigationCommand = new ViewModel.TripVoiceCommand(
                "protocolLaunch",
                "text",
                "destination",
                destination
            );
            
            navigationToPageType = typeof(View.TripDetails);
        }
        else {
            // If launched using any other mechanism, fall back to the main page view.
            // Otherwise, the app will freeze at a splash screen.
            navigationToPageType = typeof(View.TripListView);
        }
        
        // Repeat the same basic initialization as OnLaunched() above, taking into account whether
        // or not the app is already active.
        Frame rootFrame = Window.Current.Content as Frame;
        
        // Do not repeat app initialization when the Window already has content,
        // just ensure that the window is active.
        if (rootFrame == null) {
            // Create a frame to act as the navigation context and navigate to the first page.
            rootFrame = new Frame();
            App.NavigationService = new NavigationService(rootFrame);
            
            rootFrame.NavigationFailed += OnNavigationFailed;
            
            // Place the frame in the current window.
            Window.Current.Content = rootFrame;
        }
        
        // Since the expectation is to always show a details page, navigate even if 
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
    private string SemanticInterpretation(string interpretationKey, SpeechRecognitionResult speechRecognitionResult) {
        return speechRecognitionResult.SemanticInterpretation.Properties[interpretationKey].FirstOrDefault();
    }
    ```  

## <a name="handle-the-voice-command-in-the-app-service"></a>在应用服务中处理语音命令  

在应用服务中处理语音命令。  

1. 将以下 using 指令添加到语音命令服务文件中。  
    示例：`AdventureWorksVoiceCommandService.cs`。  

    ```csharp
        using Windows.ApplicationModel.VoiceCommands;
        using Windows.ApplicationModel.Resources.Core;
        using Windows.ApplicationModel.AppService;
    ```  

2. 采用服务延迟，以便在处理语音命令时不终止应用服务。  
3. 确认后台任务正在以由语音命令激活的应用服务的形式运行。  
    1. 将 TriggerDetails 强制 [**转换为**](/uwp/api/Windows.ApplicationModel.AppService.AppServiceTriggerDetails) [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 。  
    2. 检查 [**IBackgroundTaskInstance.TriggerDetails.Name**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskRegistration) 是否为文件中应用服务的名称 `Package.appxmanifest` 。  
4. 使用 [**TriggerDetails 创建 IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 的 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) **来检索** 语音命令。
5. 为 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)注册事件处理程序。  [**VoiceCommandCompleted**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) 在应用服务由于用户取消而关闭时接收通知。  
6. 为 [**IBackgroundTaskInstance**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance) 注册事件处理程序，以便在应用服务由于意外故障而关闭时收到通知。  
7. 确定命令名称以及所讲述的内容。  
    1. 请使用 [**VoiceCommand**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand)。[**CommandName**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommand) 属性来确定语音命令的名称。  
    2. 若要确定用户所说的内容，请检查字典中已识别的短语的 [**文本**](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionResult) 值或语义属性 [`SpeechRecognitionSemanticInterpretation`](/uwp/api/Windows.Media.SpeechRecognition.SpeechRecognitionSemanticInterpretation) 。  
8. 在应用服务中采取适当的措施。  
9. 使用 **Cortana** 显示语音命令并将其反馈给语音命令。  
    1. 确定希望 **Cortana** 显示的字符串，并为响应语音命令和创建对象而向用户发出对话 [`VoiceCommandResponse`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandResponse) 。 有关如何选择 **cortana** 显示和讲述的反馈字符串的指导，请参阅 [cortana 设计指南](cortana-design-guidelines.md)。  
    2. 使用 [**VoiceCommandServiceConnection**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)实例，通过对对象调用 [**ReportProgressAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)或 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)来向 **Cortana** 报告进度或完成情况 `VoiceCommandServiceConnection` 。  

    >[!NOTE]
    > 如果需要参阅 VCD，请访问 [编辑 Vcd 文件](#edit-the-vcd-file) 部分。  

    ```csharp
    public sealed class VoiceCommandService : IBackgroundTask {
        private BackgroundTaskDeferral serviceDeferral;
        VoiceCommandServiceConnection voiceServiceConnection;
        
        public async void Run(IBackgroundTaskInstance taskInstance) {
            //Take a service deferral so the service isn&#39;t terminated.
            this.serviceDeferral = taskInstance.GetDeferral();
            
            taskInstance.Canceled += OnTaskCanceled;
            
            var triggerDetails = taskInstance.TriggerDetails as AppServiceTriggerDetails;
    
            if (triggerDetails != null &amp;&amp; 
                triggerDetails.Name == "AdventureWorksVoiceServiceEndpoint") {
                try {
                    voiceServiceConnection = 
                    VoiceCommandServiceConnection.FromAppServiceTriggerDetails(
                        triggerDetails);
                    voiceServiceConnection.VoiceCommandCompleted += 
                    VoiceCommandCompleted;
                    
                    VoiceCommand voiceCommand = await 
                    voiceServiceConnection.GetVoiceCommandAsync();
                    
                    switch (voiceCommand.CommandName) {
                        case "whenIsTripToDestination":
                            {
                                var destination = 
                                voiceCommand.Properties["destination"][0];
                                SendCompletionMessageForDestination(destination);
                                break;
                            }
                            
                            // As a last resort, launch the app in the foreground.
                        default:
                            LaunchAppInForeground();
                            break;
                    }
                }
                finally {
                    if (this.serviceDeferral != null) {
                        // Complete the service deferral.
                        this.serviceDeferral.Complete();
                    }
                }
            }
        }
        
        private void VoiceCommandCompleted(VoiceCommandServiceConnection sender,
            VoiceCommandCompletedEventArgs args) {
            if (this.serviceDeferral != null) {
                // Insert your code here.
                // Complete the service deferral.
                this.serviceDeferral.Complete();
            }
        }
        
        private async void SendCompletionMessageForDestination(
            string destination) {
            // Take action and determine when the next trip to destination
            // Insert code here.
            
            // Replace the hardcoded strings used here with strings 
            // appropriate for your application.
            
            // First, create the VoiceCommandUserMessage with the strings 
            // that Cortana will show and speak.
            var userMessage = new VoiceCommandUserMessage();
            userMessage.DisplayMessage = "Here's your trip.";
            userMessage.SpokenMessage = "Your trip to Vegas is on August 3rd.";
            
            // Optionally, present visual information about the answer.
            // For this example, create a VoiceCommandContentTile with an 
            // icon and a string.
            var destinationsContentTiles = new List<VoiceCommandContentTile>();
            
            var destinationTile = new VoiceCommandContentTile();
            destinationTile.ContentTileType = 
                VoiceCommandContentTileType.TitleWith68x68IconAndText;
            // The user taps on the visual content to launch the app. 
            // Pass in a launch argument to enable the app to deep link to a 
            // page relevant to the item displayed on the content tile.
            destinationTile.AppLaunchArgument = 
                string.Format("destination={0}", "Las Vegas");
            destinationTile.Title = "Las Vegas";
            destinationTile.TextLine1 = "August 3rd 2015";
            destinationsContentTiles.Add(destinationTile);
            
            // Create the VoiceCommandResponse from the userMessage and list    
            // of content tiles.
            var response = VoiceCommandResponse.CreateResponse(
                userMessage, destinationsContentTiles);
            
            // Cortana displays a "Go to app_name" link that the user 
            // taps to launch the app. 
            // Pass in a launch to enable the app to deep link to a page 
            // relevant to the voice command.
            response.AppLaunchArgument = string.Format(
                "destination={0}", "Las Vegas");
            
            // Ask Cortana to display the user message and content tile and 
            // also speak the user message.
            await voiceServiceConnection.ReportSuccessAsync(response);
        }
        
        private async void LaunchAppInForeground() {
            var userMessage = new VoiceCommandUserMessage();
            userMessage.SpokenMessage = "Launching Adventure Works";
            
            var response = VoiceCommandResponse.CreateResponse(userMessage);
            
            // When launching the app in the foreground, pass an app 
            // specific launch parameter to indicate what page to show.
            response.AppLaunchArgument = "showAllTrips=true";
            
            await voiceServiceConnection.RequestAppLaunchAsync(response);
        }
    }
    ```  

激活后，应用服务会有0.5 秒的调用 [**ReportSuccessAsync**](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection)。 **Cortana** 显示并显示一个反馈字符串。  

>[!NOTE]
> 可以在 VCD 文件中声明一个 **反馈** 字符串。 该字符串不会影响 Cortana 画布上显示的 UI 文本，而只会影响 **cortana** 所讲述的文本。  

如果此应用程序的调用时间超过0.5 秒， **Cortana** 将插入一个手写屏幕，如下所示。 **Cortana** 会在应用程序调用 **ReportSuccessAsync** 或最长5秒之前显示手写。 如果应用服务未调用 **ReportSuccessAsync**，或者为 [`VoiceCommandServiceConnection`](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandServiceConnection) **Cortana** 提供信息的任何方法，则用户将收到一条错误消息，并取消应用服务。  

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="Cortana 的屏幕截图，以及在后台使用 AdventureWorks 应用的进度和结果屏幕的基本查询":::

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [通过 Cortana 使用语音命令激活前台应用](cortana-launch-a-foreground-app-with-voice-commands.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 设计准则](cortana-design-guidelines.md)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)