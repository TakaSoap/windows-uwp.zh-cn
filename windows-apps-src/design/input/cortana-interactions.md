---
description: 使用在 Windows 应用程序中启动和执行单个操作的语音命令扩展 **Cortana** 的基本功能。
title: Windows 应用中的 Cortana 交互
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
keywords: Cortana, Cortana 画布, Cortana 设计, 用户界面, 语音命令, VCD
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fca4da482585ddd4b7f9d54008c1905e372ca030
ms.sourcegitcommit: d51c3dd64d58c7fa9513ba20e736905f12df2a9a
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/28/2021
ms.locfileid: "98988729"
---
# <a name="cortana-interactions-in-windows-apps"></a>Windows 应用中的 Cortana 交互

>[!WARNING]
> 从 Windows 10 2020 更新 (版本2004，codename "20H1" ) 中不再支持此功能。

使用在 Windows 应用程序中启动和执行单个操作的语音命令扩展 **Cortana** 的基本功能。

可以在应用程序获得焦点时在前台启动目标应用程序，) 或在后台激活 **cortana** (**cortana** 会保留焦点，但会根据交互的复杂程度从应用) 提供结果 (。 通常，需要其他上下文或用户输入的语音命令最好在前台应用程序中进行处理，而基本命令可通过后台应用在 **Cortana** 中进行处理。 

通过集成你的应用程序的基本功能，并为用户提供一个中心入口点来完成大部分任务，而无需直接打开应用， **Cortana** 就成为了你的应用程序和用户之间的联系。 将此快捷方式提供给应用功能并减少切换应用的需要，可以节省用户大量时间和精力。

> [!NOTE]
> 语音命令是具有特定意向的单个查询文本，它在语音命令定义 (VCD) 文件中定义，通过 **Cortana** 定向到已安装的应用。
>
> VCD 文件定义一个或多个语音命令，每个命令都具有唯一的意图。
>
> 语音命令定义的复杂性可能会有所不同。 它们可以支持从单个、受约束的查询文本到一系列更灵活的自然语言最谈话的任何内容，所有这些都表示相同的目的。

## <a name="other-speech-and-conversation-components"></a>其他语音和对话组件

### <a name="speech-voice-and-conversation-in-windows-10"></a>Windows 10 中的语音、语音和对话

有关各种 Windows 开发框架如何为生成 Windows 应用程序的开发人员提供语音识别、语音合成和会话支持的信息，请参阅 [Windows 10 中的语音、语音和对话](/windows/apps/speech) 。

### <a name="cortana-skills-kit"></a>Cortana 技能套件

如果希望通过添加自己的技能使用户能够通过 Cortana 与你的 **服务** 进行交互，请参阅 [cortana 技能工具包](/cortana/skills/)。 [**弃用通知：** 作为我们目标的一部分，通过将 Cortana 嵌入到 Microsoft 365 来转换新式工作效率体验，我们正在淘汰开发人员平台 (的 cortana 技能套件) 以及在此平台上构建的所有技能。]

## <a name="related-articles"></a>相关文章

* [VCD 元素和属性 v1.2](/uwp/schemas/voicecommands/voice-command-elements-and-attributes-1-2)

### <a name="designers"></a>设计器

* [Cortana 设计准则](cortana-design-guidelines.md)

### <a name="samples"></a>示例

* [Cortana 语音命令示例](https://go.microsoft.com/fwlink/p/?LinkID=619899)
