---
title: 动态修改 Cortana VCD 短语列表-Cortana UWP 设计和开发
description: 使用语音识别结果在运行时访问和更新语音命令定义中 (PhraseList) 元素的列表， (VCD) 文件。
ms.assetid: b497145b-c7a0-454a-8329-6bc1228953bb
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: 1f61a08e9eeb66371ed39b44eb39dacbc1bf3cf5
ms.sourcegitcommit: 8fe992f3a6d8f7975af4911ad88e855bee50083e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/05/2021
ms.locfileid: "99606032"
---
# <a name="dynamically-modify-cortana-vcd-phrase-lists"></a>动态修改 Cortana VCD 短语列表

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

使用语音识别结果在运行时访问和更新语音命令定义中 (**PhraseList**) 元素的列表， (VCD) 文件。

> [!NOTE]
> 语音命令是具有特定意向的单个查询文本，它在语音命令定义 (VCD) 文件中定义，通过 **Cortana** 定向到已安装的应用。
>
> VCD 文件定义一个或多个语音命令，每个命令都具有唯一的意图。
>
> 语音命令定义的复杂性可能会有所不同。 它们可以支持从单个、受约束的查询文本到一系列更灵活的自然语言最谈话的任何内容，所有这些都表示相同的目的。

如果语音命令特定于涉及某种用户定义或暂时性应用数据的任务，则在运行时动态修改短语列表很有用。

> [!NOTE]
> **重要的 API**
>
> - [**Windows.applicationmodel.resources.core. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

例如，假设你有一个旅行应用，用户可在其中输入目标，并且你希望用户能够通过说明应用名称（后跟 "显示行程目标"）来启动应用 &lt; &gt; 。 在 **ListenFor** 元素本身中，你将指定类似于以下内容的内容： `<ListenFor> Show trip to {destination}  </ListenFor>` ，其中 "Destination" 是 **PhraseList** 的 "**标签**" 属性的值。

在运行时更新词组列表，无需为每个可能的目标创建单独的 **ListenFor** 元素。 相反，可以使用用户在输入其路线时指定的目标动态填充 **PhraseList** 。

有关 **PhraseList** 和其他 VCD 元素的详细信息，请参阅 [**VCD 元素和属性 1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2) 参考。

> [!TIP]
> **先决条件**
>
> 如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请查看这些主题来熟悉此处讨论的技术。
>
> - [创建你的第一个应用](/windows/uwp/get-started/your-first-app)
> - 借助[事件和路由事件概述](/windows/uwp/xaml-platform/events-and-routed-events-overview)了解事件
>
> **用户体验指南**
>
> 有关如何将应用集成到 **Cortana** 和 [语音交互](speech-interactions.md)的信息，请参阅 [cortana 设计指导原则](cortana-design-guidelines.md)，获取有关设计有用且具有吸引力的语音应用的有用技巧。

## <a name="identify-the-command-and-update-the-phrase-list"></a>识别命令并更新词组列表

下面是一个示例 VCD 文件，其中定义了一个 **命令** "showTripToDestination" 和一个 **PhraseList** ，用于定义 **艾德公司** 旅行应用中目标的三个选项。 当用户在应用中保存和删除目标时，应用会更新 **PhraseList** 中的选项。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>
```

若要更新 VCD 文件中的 **PhraseList** 元素，请获取包含短语列表的 **CommandSet** 元素。 使用该 **CommandSet** 元素的 **name** 属性， (**名称** 在 VCD 文件中必须唯一) 作为访问 [**InstalledCommandSets**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandManager)属性的键，并获取 [**VoiceCommandSet**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)引用。

确定命令集之后，获取对要修改的短语列表的引用，并调用 [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)方法;使用 **PhraseList** 元素的 **Label** 属性和字符串数组作为短语列表的新内容。

> [!NOTE]
> 如果修改短语列表，则会替换整个短语列表。 如果要将新项插入到短语列表中，则必须在对 [**SetPhraseListAsync**](/uwp/api/Windows.Media.SpeechRecognition.VoiceCommandSet)的调用中同时指定现有项和新项。

在此示例中，我们将上一示例中所示的 **PhraseList** 更新为 Phoenix 的其他目标。

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <a name="remarks"></a>备注

使用 **PhraseList** 来约束识别适用于相对较小的一组或几个词。 如果一组单词太大， (数以百计的单词（例如) 或不应进行约束），请使用 **PhraseTopic** 元素和 **Subject** 元素来优化语音识别结果的相关性，从而提高可伸缩性。

在我们的示例中，我们有了一个 "搜索"**方案** 的 **PhraseTopic** ，并通过 "城市状态" 的 **主题** 进行了进一步改进 \\ 。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <AppName> Adventure Works, </AppName>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [通过 Cortana 使用语音命令激活前台应用](cortana-launch-a-foreground-app-with-voice-commands.md)
- [使用语音命令激活 Cortana 中的后台应用](cortana-launch-a-background-app-with-voice-commands.md)
- [Cortana 设计准则](cortana-design-guidelines.md)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)
