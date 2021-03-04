---
title: Cortana 设计准则 - Cortana UWP 设计和开发
description: 这些指导原则和建议描述你的应用程序如何使用 Cortana 与用户交互。
ms.assetid: 332ccb95-0e56-410e-ab63-cc028fce4192
label: Cortana
template: detail.hbs
ms.date: 01/27/2021
ms.topic: article
keywords: cortana，设计
ms.localizationpriority: medium
ms.openlocfilehash: ae5f1ce3c481e833ce80d0ebd52d64f6efba7e78
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101823271"
---
# <a name="cortana-design-guidelines"></a>Cortana 设计指南

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。
>
> 有关 Cortana 如何转换新式生产力体验的 Microsoft 365，请参阅 [cortana in](/microsoft-365/admin/misc/cortana-integration) 。

这些指导原则和建议描述了应用如何最好地使用 **Cortana** 与用户交互，帮助他们完成任务，并清楚地传达一切发生的情况。

**Cortana** 允许在后台运行的应用程序提示用户确认或消除歧义，并在返回时向用户提供有关语音命令状态的反馈。 此过程是轻型、快速的，不会强制用户将 **Cortana** 体验或切换上下文留给应用程序。

尽管用户应该觉得 **cortana** 有助于尽可能地简化过程，但你可能还想让 **cortana** 知道，你的应用程序需要完成该任务。

我们使用名为 " **艾德作品** 集成到 **Cortana** UI" 的行程计划和管理应用，其中显示了我们讨论的许多概念和功能。 有关详细信息，请参阅 [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)。

:::image type="content" source="images/cortana/cortana-overview.png" alt-text="Cortana 画布的屏幕截图":::

## <a name="conversational-writing"></a>会话写入

成功的 **Cortana** 交互需要在 (TTS) 和 GUI 字符串中编制文本到语音转换时遵循一些基本原则。

<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">原则</th>
<th align="left">错误示例</th>
<th align="left">良好的示例</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>低下</dt>
<dd><p>尽可能少地使用字词并将最重要的信息放在最前面。</p>
</dd>
</dl></td>
<td align="left"><p>当然可以，您希望立即搜索什么电影？ 我们有一个很大的集合。</p></td>
<td align="left"><p>当然，您正在寻找什么电影？</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>对应</dt>
<dd><p>提供仅与任务、内容和上下文相关的信息。</p>
</dd>
</dl></td>
<td align="left"><p>我已将此添加到播放列表。 只需知道，电池电量就会变低。</p></td>
<td align="left"><p>我已将此添加到播放列表。</p></td>
</tr>
<tr class="odd">
<td align="left"><p></p>
<dl>
<dt>清除</dt>
<dd><p>避免多义性。 使用日常语言，而不是技术术语。</p>
</dd>
</dl></td>
<td align="left"><p>没有与拉斯维加斯进行查询的结果 &quot; &quot; 。</p></td>
<td align="left"><p>我找不到拉斯维加斯的任何行程。</p></td>
</tr>
<tr class="even">
<td align="left"><p></p>
<dl>
<dt>值得 </dt>
<dd><p>尽可能准确。 对于后台发生的情况是透明的，如果任务尚未完成，请不要说它已经完成。 尊重隐私-不要大声阅读隐私信息。</p>
</dd>
</dl></td>
<td align="left"><p>我找不到该电影，还不得发布。</p></td>
<td align="left"><p>我在目录中找不到此电影。</p></td>
</tr>
</tbody>
</table>

编写人员说话的方式。 不要强调语法准确性。 例如，类似于 "需要" 或 "走" 的耳式口头快捷方式可供 TTS 读出。

尽可能使用隐含的第一人称时态。 例如，"查找下一个艾德公司的工作行程" 表示有人正在进行查找，但不使用单词 "I" 指定。

使用某种变体有助于使应用听起来更加自然。 提供 TTS 和 GUI 字符串的不同版本，以有效地表达相同的内容。 例如，"要查看什么电影？" 可能有一些替代方法，如 "你想要观看哪个电影？"。 每次用户都不会说是完全相同的。 只需确保将 TTS 和 GUI 版本保持同步。

灵活地在响应中使用 "确定" 和 "好" 之类的短语。 虽然它们可以提供确认和进度，但如果过于频繁使用且没有变体，它们也可能会重复。

> [!NOTE]
> 仅在 TTS 中使用确认短语。 由于 **Cortana** 画布上的空间有限，请不要在相应的 GUI 字符串中重复这些操作。

在响应中使用缩写，以获得更自然的交互，并在 **Cortana** 画布上节省额外的空间。 例如，"找不到该电影"，而不是 "找不到该电影"。 为 ear 编写，而不是眼睛。

使用系统理解的语言。 用户往往会重复显示这些条款。 了解显示的内容。

通过从备用响应的集合旋转或随机选择，使用响应中的某种变化形式。 例如，"要查看什么电影？" 和 "您希望观看哪个电影？"。 这会使你的应用程序变得更加自然和独特。

## <a name="localization"></a>本地化

若要使用语音命令启动操作，应用必须以用户在其设备上所选的语言（ (设置 &gt; 系统 &gt; 语音 &gt; 语音语言) 注册语音命令。

你应本地化应用程序响应的语音命令以及所有 TTS 和 GUI 字符串。

应避免冗长的 GUI 字符串。 **Cortana** 画布提供三行用于响应，并将截断超过该长度的字符串。

有关详细信息，请参阅 [全球化和本地化部分](../globalizing/guidelines-and-checklist-for-globalizing-your-app.md)。

## <a name="image-resources-and-scaling"></a>图像资源和缩放

通用 Windows 平台 (UWP) 应用可根据特定设置和设备功能自动选择最适合的应用徽标图像， (高对比度、有效像素、区域设置等) 。 你需要做的只是提供映像，并确保在应用项目中对不同资源版本使用适当的命名约定和文件夹组织。 如果不提供建议的资源版本、可访问性、本地化和图像质量，这取决于用户的首选项、功能、设备类型和位置。

若要详细了解高对比度和缩放比例的图像资源，请参阅 [磁贴和图标资产的准则](../../app-resources/images-tailored-for-scale-theme-contrast.md)。

使用限定符命名资源。 资源限定符是文件夹和文件名修饰符，用于标识应在其中使用特定版本资源的上下文。

标准命名约定为 "文件夹名称/qualifiername \[ \_ qualifiername-value \] /filename.qualifiername-value \[ \_ qualifiername \] "。 例如： images/徽标键。 \_ 只需在代码中使用根文件夹和文件名： images/logo.png 来1-100 缩放contrast-white.png。 请参阅 [管理语言和区域](../globalizing/manage-language-and-region.md) 和 [如何使用限定符命名资源](/previous-versions/windows/apps/hh965324(v=win.10))。

建议您将字符串资源 (文件中的默认语言（例如 " \\ .resw" ) ）和 (图像的默认缩放比例（如 "logo.scale-100.png" ) ）标记为默认语言，即使您当前不打算提供本地化或多个解析资源也是如此。 但至少建议为100、200和400规模因素提供资产。

> [!IMPORTANT]
> Cortana 画布的标题区中使用的应用图标是在 "appxmanifest.xml" 文件中指定的 Square44x44Logo 图标。

还可以为用户查询的每个结果磁贴指定图标。 "结果" 图标的有效图像大小为：

- 68w x 68h
- 68w x 92h
- 280w x 140h

## <a name="result-tile-templates"></a>结果磁贴模板

将为 Cortana 画布上显示的结果磁贴提供一组模板。 使用这些模板可指定磁贴标题以及磁贴是否包含文本和结果图标图像。 根据指定的模板，每个图块最多可包含三行文本和一个图像。

以下是支持的模板 (示例) ：

| 名称 | 示例 |
| --- | --- |
| 仅标题  | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titleonly-small.png" alt-text="仅显示标题的 Cortana 画布的屏幕截图"::: |
| 带有文本的标题 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewithtext-small.png" alt-text="显示标题和文本的 Cortana 画布屏幕截图"::: |
| 带有68x68 图标的标题 | 无图像 |
| 带有68x68 图标和文本的标题 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x68iconandtext-small.png" alt-text="显示标题和68x68 图标和文本的 Cortana 画布屏幕截图"::: |
| 带有68x92 图标的标题 | 无图像 |
| 带有68x92 图标和文本的标题 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith68x92iconandtext-small.png" alt-text="显示标题和68x92 图标和文本的 Cortana 画布屏幕截图"::: |
| 带有280x140 图标的标题 | 无图像 |
| 带有280x140 图标和文本的标题 | :::image type="content" source="images/cortana/voicecommandcontenttiletype-titlewith280x140iconandtext-small.png" alt-text="显示标题和280x140 图标和文本的 Cortana 画布屏幕截图"::: |

有关 Cortana 模板的详细信息，请参阅 [VoiceCommandContentTileType](/uwp/api/Windows.ApplicationModel.VoiceCommands.VoiceCommandContentTileType) 。

## <a name="example"></a>示例

此示例演示 **Cortana** 中的后台应用程序的端到端任务流。 我们正在使用 **艾德作品** 应用取消到拉斯维加斯的行程。 此示例使用 "标题和68x68 图标和文本" 模板。

:::image type="content" source="images/cortana/e2e-canceltrip.png" alt-text="适用于端到端 Cortana 后台应用流的 Cortana 画布屏幕截图":::

下面是此图中概述的步骤：

1. 用户点击麦克风启动 **Cortana**。
2. 用户将 "取消我的艾德工作行程" 显示为在后台启动 **艾德公司** 应用。 应用使用 **Cortana** 语音和画布来与用户交互。
3. **Cortana** 转换为提交屏幕，该屏幕向用户提供确认反馈， ( "我将会获得艾德作品。") 、状态栏和 "取消" 按钮。
4. 在这种情况下，用户有多个与查询匹配的行程，因此该应用程序提供了消除了所有匹配结果的消除，并询问 "您要取消哪一个？"
5. 用户指定 "会议会议" 项。
6. 由于取消操作无法撤消，应用会提供确认屏幕，要求用户确认其意图。
7. 用户显示 "是"。
8. 然后，该应用程序会提供一个完成屏幕，其中显示操作的结果。

我们将在此处更详细地探讨这些步骤。

### <a name="handoff"></a>Handoff

:::image type="content" source="images/cortana/cortana-backgroundapp-result.png" alt-text="Cortana 画布的屏幕截图，适用于端到端 cortana 后台应用流使用 adventureworks，无":::切换 *AdventureWorks* ，无移交

:::image type="content" source="images/cortana/cortana-backgroundapp-progress-result.png" alt-text="使用 AdventureWorks 后端到端 cortana 后台应用程序流的 cortana 画布的屏幕截图，其中":::*包含移交*

对于您的应用程序而言，不需要500毫秒的任务会进行响应，并且无需用户提供其他信息，无需再通过 **Cortana** 进一步参与，而只需显示完成屏幕。

如果你的应用程序需要超过500毫秒的响应， **Cortana** 会提供一个移交屏幕。 将显示应用程序图标和名称，并且必须提供 GUI 和 TTS 切换字符串，以指示已正确理解语音命令。 将显示最多5秒的移交屏幕;如果应用在这段时间内未响应， **Cortana** 会显示一般错误屏幕。

### <a name="gui-and-tts-guidelines-for-handoff-screens"></a>切换屏幕的 GUI 和 TTS 指导原则

明确指出任务正在进行中。

使用现在时时态。

使用用于确认正在启动的任务并引用特定实体的操作谓词。

使用不会提交到请求的不完整操作的一般谓词。 例如，"寻找行程" 而不是 "取消行程"。 在这种情况下，如果未返回任何结果，则用户听不到类似的 "正在取消你的拉斯维加斯 ... .。。 我找不到拉斯维加斯的旅行。

请注意，如果应用仍需要解析请求的实体，则该任务尚未发生。 例如，请注意我们如何说 "寻找行程"，而不是 "取消行程"，因为可以匹配零个或多个行程，并且我们还不知道结果。

GUI 和 TTS 字符串可以相同，但不需要。 尝试保持 GUI 字符串的简短，以避免截断和重复其他视觉资产。

| TTS | GUI |
| --- | --- |
| 正在寻找下一个艾德作品。 | 正在寻找下一个行程 .。。 |
| 搜索艾德公司的行程会落在城市。 | 正在搜索落为城市的行程 .。。 |

### <a name="progress"></a>进度

:::image type="content" source="images/cortana/e2e-canceltrip-progress.png" alt-text="适用于端到端 cortana 后台应用流的 cortana 画布屏幕截图使用 adventureworks 取消行程进度":::*AdventureWorks "取消行程" 进度*

当任务在步骤之间需要一段时间时，你的应用程序需要进入并在进度屏幕上更新用户。 将显示应用图标，并且必须提供 GUI 和 TTS 进度字符串来指示任务正在进行。

应该提供指向应用的链接，其中包含启动参数，以在适当的状态启动应用。 这允许用户查看或完成该任务。 **Cortana** 提供链接文本 (如 "中转到艾德作品" ) 。

进度屏幕将显示5秒，之后它们必须后跟另一个屏幕，否则任务将超时。

这些屏幕可以跟踪进度屏幕：

- 进度
- 确认 (显式，稍后将对此进行介绍) 
- 消除歧义
- Completion

### <a name="gui-and-tts-guidelines-for-progress-screens"></a>面向进度屏幕的 GUI 和 TTS 指导原则

使用现在时时态。

使用用于确认任务正在进行的操作谓词。

**GUI**：如果显示了实体，请使用对它的引用 ( "取消此行程 ..." ) ;如果未显示任何实体，请明确地 ( "取消" "正在取消 '"，并 ) "。

**TTS**：在第一个 "进度" 屏幕上只应包含一个 TTS 字符串。 如果需要进一步的进度屏幕，请发送一个空字符串（ {} 作为 TTS 字符串），并仅提供一个 GUI 字符串。

| 条件  | TTS | GUI |
| --- | --- | --- |
| 在显示时打开的实体上的实体读取     | 正在取消此行程 .。。          | 正在取消此行程 .。。          |
| 在显示时未读取实体 | 正在取消你的拉斯维加斯 .。。 | 正在取消此行程 .。。          |
| 实体未在先前打开/未显示时读取        | 正在取消你的拉斯维加斯 .。。 | 正在取消你的拉斯维加斯 .。。 |

### <a name="confirmation"></a>确认

:::image type="content" source="images/cortana/e2e-canceltrip-confirmation.png" alt-text="适用于端到端 cortana 后台应用流的 cortana 画布屏幕截图使用 adventureworks 取消行程确认":::*AdventureWorks "取消行程" 确认*

某些任务可以通过用户命令的性质隐式确认;其他人可能更敏感，要求明确确认。 下面是有关何时使用显式或隐式确认的一些准则。

确认屏幕上的 GUI 和 TTS 字符串都是由您的应用程序指定的，并且将显示应用程序图标（如果提供）而不是 **Cortana** 头像。

客户响应确认后，你的应用程序必须在 500 ms 内提供下一个屏幕，以避免进入进度屏幕。

使用 explicit 的时间 .。。

- 内容将离开用户 (如文本消息、电子邮件或社交帖子) 
- 操作不能撤消 (例如，购买或删除某些内容) 
- 结果可能是尴尬 (例如，调用错误的人员) 
- 需要更复杂的识别 (例如，开放的脚本) 

使用隐式 when .。。

- 只为用户保存内容 (例如，注释到) 
- 可以通过一种简单的方法来 (例如，打开或关闭警报) 
- 任务需要快速 (例如，在忘记之前快速捕获观点) 
- 准确性很高 (例如，简单菜单) 

### <a name="gui-and-tts-guidelines-for-confirmation-screens"></a>用于确认屏幕的 GUI 和 TTS 准则

使用现在时时态。

向用户询问可以使用 "是" 或 "否" 回答的明确问题。 问题应该明确确认用户正在尝试执行的操作，而不应有其他明显的选项。

如果第一次未理解语音命令，请提供重新提示问题的变化形式。

**GUI**：如果显示了实体，请使用对其的引用。 如果未显示任何实体，则显式调用实体。

**TTS**：为了清晰起见，请始终引用特定项或实体，除非它在上一次打开时被系统读取。

| 条件 | TTS | GUI |
| --- | --- | --- |
| 在显示时未读取实体 | 是否要取消拉斯维加斯的技术大会？ | 取消此行程？                             |
| 实体未在先前打开/未显示时读取        | 是否要取消拉斯维加斯的技术大会？ | 取消拉斯维加斯的技术大会？                 |
| 实体在以前的关闭时读取/不显示实体            | 是否要取消此行程？             | 取消此行程？                             |
| 显示实体的重新提示                              | 是否要取消此行程？            | 是否要取消此行程？             |
| 未显示实体的重新提示                          | 是否要取消此行程？            | 是否要取消拉斯维加斯的技术大会？ |

### <a name="disambiguation"></a>消除歧义

:::image type="content" source="images/cortana/cortana-disambiguation-screen.png" alt-text="Cortana 画布对于端到端 cortana 后台应用流的屏幕截图使用 AdventureWorks 取消行程消除":::了 *adventureworks "取消行程" 消除歧义*

某些任务可能要求用户从实体列表中进行选择以完成任务。

消除歧义屏幕上的 GUI 和 TTS 字符串都是由您的应用程序指定的，并且将显示应用程序图标（如果提供）而不是 **Cortana** 头像。

客户回答消除歧义问题后，应用程序必须在 500 ms 内提供下一个屏幕，以避免进入进度屏幕。

### <a name="gui-and-tts-guidelines-for-disambiguation-screens"></a>消除歧义屏幕的 GUI 和 TTS 准则

使用现在时时态。

询问用户一个明确的问题，该问题可以在显示的任何实体的标题或文本行中回答。

最多可显示10个实体。

每个实体都应具有唯一的标题。

如果第一次未理解语音命令，请提供重新提示问题的变化形式。

**TTS**：为清楚起见，除非已在上一轮上口述，否则始终引用特定的项或实体。

**TTS**：请不要阅读实体列表，除非有三个或更少的，它们都很短。

| 条件                 | TTS                                                                            | GUI                              |
|----------------------------|--------------------------------------------------------------------------------|----------------------------------|
| PROMPT-3 个或更少项  | 您想要取消哪种旅程？ 拉斯维加斯的技术大会或聚会 | 要取消哪一项？ |
| PROMPT-超过3个项目 | 您想要取消哪种旅程？                                          | 要取消哪一项？ |
| 重新提示                   | 您希望取消哪种                                         | 要取消哪一项？ |

### <a name="completion"></a>Completion

:::image type="content" source="images/cortana/e2e-canceltrip-completion.png" alt-text="Cortana 画布，适用于端到端 cortana 后台应用流使用 AdventureWorks 的屏幕截图取消行程完成":::*AdventureWorks "取消行程" 完成*

成功完成任务后，应用应通知用户已成功完成请求的任务。

完成屏幕上的 GUI 和 TTS 字符串都是由您的应用程序指定的，并且将显示应用程序图标（如果提供），而不是 **Cortana** 头像。

应该提供指向应用的链接，其中包含启动参数，以在适当的状态启动应用。 这允许用户查看或完成该任务。 **Cortana** 提供链接文本 (如 "中转到艾德作品" ) 。

### <a name="gui-and-tts-guidelines-for-completion-screens"></a>完成屏幕的 GUI 和 TTS 指导原则

使用过去的时态。

使用操作谓词显式声明任务已完成。

如果显示了该实体，或在之前引用了它，则只引用它。

| 条件                                       | TTS                                             | GUI                                |
|--------------------------------------------------|-------------------------------------------------|------------------------------------|
| 已显示实体/实体在之前打开         | 我已经取消此行程。                       | 已取消此行程。               |
| 之前未显示实体/实体 | 我已经取消了您的拉斯维加斯技术会议。 | 已取消 "会议大会"。 |

### <a name="error"></a>错误

:::image type="content" source="images/cortana/e2e-canceltrip-error.png" alt-text="Cortana 画布对于端到端 cortana 后台应用流使用 adventureworks 的屏幕截图取消行程错误":::*AdventureWorks "取消行程" 错误*

出现以下错误之一时， **Cortana** 会显示相同的一般错误消息。

- 应用服务意外终止。
- **Cortana** 无法与应用服务通信。
- 应用程序在 **Cortana** 显示切换屏幕或进度屏幕后无法提供5秒的屏幕。

## <a name="related-articles"></a>相关文章

- [Windows 应用中的 Cortana 交互](cortana-interactions.md)
- [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)
- [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)