---
title: 在 Cortana 中支持更自然的语音命令 - Cortana UWP 设计和开发
description: 使用更灵活和自然的声音命令扩展 Cortana，让用户在命令中的任意位置显示应用的名称。
author: kbridge
label: Conceptual
ms.assetid: c2959c1b-c2f2-4a8d-8f3e-79585f69afcf
ms.date: 01/28/2021
ms.topic: article
keywords: Cortana
ms.openlocfilehash: c6777a0c7ad9a8e2658b759e516b659bbd4eb9a9
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824501"
---
# <a name="support-natural-language-voice-commands-in-cortana"></a>在 Cortana 中支持自然语言形式的语音命令

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

使用更灵活和自然的声音命令扩展 **Cortana** ，让用户在命令中的任意位置显示应用的名称。

> [!NOTE]
> **重要的 API**
>
> - [**Windows.applicationmodel.resources.core. VoiceCommands**](/uwp/api/Windows.ApplicationModel.VoiceCommands)
> - [**VCD 元素和属性 v1.2**](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

使用语音命令，通过应用中的功能来扩展 Cortana，要求用户同时指定应用和要执行的命令或函数。 这通常是通过在语音命令的开头或结尾公告应用名称来实现的。 例如，"艾德公司向拉斯维加斯添加新行程"。

不过，以这种方式指定应用程序名称可能会令 stilted，甚至没有什么意义。 在许多情况下，可以在命令中的其他地方说应用程序名称，这会更加舒适和自然，并有助于使交互更加直观和吸引用户。 前面的示例 "艾德公司向拉斯维加斯添加新行程"。 可能只为 "向拉斯维加斯添加新的艾德公司行程"。 或者，将 "使用艾德公司" 添加到拉斯维加斯的新行程。

你可以设置语音命令，以支持的应用名称为：

- Prefix-在命令短语之前
- 中缀-在命令短语内
- 后缀-在命令短语后面

> [!TIP]
> **先决条件**
>
> 本主题基于 [使用语音命令激活 Cortana 中的后台应用](cortana-launch-a-background-app-with-voice-commands.md)。 接下来，我们将使用名为 " **艾德作品**" 的 "行程计划" 和 "管理" 应用来演示功能。
>
> 如果你还不熟悉通用 Windows 平台 (UWP) 应用开发，请查看这些主题来熟悉此处讨论的技术。
>
> - [创建你的第一个应用](../../get-started/your-first-app.md)
> - 借助[事件和路由事件概述](../../xaml-platform/events-and-routed-events-overview.md)了解事件
>
> **用户体验指南**
>
> 有关如何将应用集成到 **Cortana** 和 [语音交互](speech-interactions.md)的信息，请参阅 [cortana 设计指导原则](cortana-design-guidelines.md)，获取有关设计有用且具有吸引力的语音应用的有用技巧。

## <a name="specify-an-appname-element-in-the-vcd"></a>在 VCD 中指定 **AppName** 元素

**AppName** 元素用于在语音命令中指定应用程序的用户友好名称。

```XML
<AppName>Adventure Works</AppName>
```

## <a name="specify-where-the-app-name-can-be-spoken-in-the-voice-command"></a>指定在语音命令中可以口述应用名称的位置

**ListenFor** 元素具有 **RequireAppName** 属性，该属性指定应用程序名称可以在语音命令中出现的位置。 此属性支持四个值。

1. **BeforePhrase**

   默认。

   指示用户必须在命令短语之前显示应用名称。

   此时，Cortana 会在 "我的旅行到拉斯维加斯时" 侦听 "艾德作品"。

   ```xml
   <ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
   ```

2. **AfterPhrase**

    指示用户必须在命令短语后显示应用名称。

    系统提供了 prepositional 连词的本地化短语列表。 这包括 "使用"、"使用" 和 "打开" 等短语。

    此时，Cortana 会侦听类似于 "向拉斯维加斯工作显示下一次旅程" 和 "使用艾德公司向拉斯维加斯显示下一次旅行" 等命令。

    ```xml
    <ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
    ```

3. **BeforeOrAfterPhrase**

    指示用户必须在命令短语前后显示应用名称。

    对于后缀版本，系统将提供 prepositional 连词的本地化短语列表。 这包括 "使用"、"使用" 和 "打开" 等短语。

    此时，Cortana 会侦听类似于 "艾德作品，显示下一次旅行到拉斯维加斯" 的命令，或 "显示对艾德作品的下一次旅程"。

    ``` xml
    <ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
    ```

4. **ExplicitlySpecified**

    指示用户必须在命令短语中指定的位置精确地说明你的应用名称。 用户无需在短语前后显示应用名称。

    必须使用 **{内置： AppName}** 标记显式引用应用名称。

    此时，Cortana 会侦听类似于 "艾德作品，显示下一次旅行到拉斯维加斯的命令" 或 "向拉斯维加斯显示下一步"。

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
    ```

## <a name="special-cases"></a>特殊情况

当你声明一个 **ListenFor** 元素，其中 **RequireAppName** 为 "AfterPhrase" 或 "ExplicitlySpecified" 时，必须确保满足某些要求：

1. **{内置： AppName}** 必须在 **RequireAppName** 为 "ExplicitlySpecified" 时出现一次且仅出现一次。

    对于此值，系统无法推断应用程序名称在语音命令中出现的位置。 您必须显式指定位置。

2. 不能使用 **PhraseTopic** 元素开始语音命令，该元素通常用于大型词汇语音识别。 至少一个词必须位于它之前。

    这有助于最大程度地降低 **Cortana** 启动应用的机会，前提是命令在查询文本中的任何位置包含应用名称或部分。

    下面是一个无效的声明，如果用户说 "为 Kinect 艾德作品显示评论"，则可能导致 **Cortana** 启动 **艾德作品** 应用。
  
    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
    ```

3. 除了应用名称和对 **PhraseTopic** 元素的引用外， **ListenFor** 字符串中必须至少有两个单词。

    与案例2类似，你需要确保你的命令包含足够的拼音内容来最大程度地降低应用程序的启动几率。

    这可以帮助你将应用程序设置为获得最佳成功，因此当用户指出 "查找 Kinect 艾德作品" 时，应用程序不会错误地启动。

    下面是无效的声明，如果用户显示类似于 "你好艾德作品" 或 "查找 Kinect 艾德公司" 的内容，则这些声明会导致 **Cortana** 启动 **艾德作品** 应用。

    ```xml
    <ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
    <ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
    ```

## <a name="remarks"></a>备注

对于 **Cortana** 中的用户失措语音命令的方式，支持更多的变体也可提高应用的一般可用性。

避免将 "你好 \[ app name \] " 作为 **AppName**。 用户很可能会说 "你好 Cortana" 通过语音激活来调用 Cortana，而 \[ 在查询文本中具有 "你好应用名称 \] " 听起来并不自然。 例如，"你好 Cortana，显示下一次到拉斯维加斯的拉斯维加斯"。

请考虑将中缀/后缀变化添加到现有的语音命令。 正如我们在此处所示，将附加属性添加到现有的 **ListenFor** 元素并支持后缀变体不需要很多精力。 说 "你好 Cortana，向拉斯维加斯工作显示下一次的拉斯维加斯，而不是" 你好的 Cortana，艾德公司，显示我下一次的拉斯维加斯 "。

在语音命令与现有 **Cortana** 功能（ (调用、消息传送等) ）发生冲突的情况下，请考虑使用应用名称作为前缀。 例如，"艾德作品， \[ 关于拉斯维加斯行程的邮件旅行代理 \] "。

## <a name="complete-example"></a>完整示例

下面是一个 VCD 文件，其中演示了提供更自然语言声音命令的各种方式。

> [!NOTE]
> 具有多个 **ListenFor** 元素，每个元素都有不同的 **RequireAppName** 属性值。

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="https://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [Cortana 设计准则](cortana-design-guidelines.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)