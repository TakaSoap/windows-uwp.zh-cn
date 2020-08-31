---
Description: 可以记录 UWP 应用中的自定义事件，并在合作伙伴中心的使用情况报告中查看这些事件。
title: 记录合作伙伴中心的自定义事件
ms.date: 06/01/2018
ms.topic: article
keywords: windows 10, uwp, Microsoft Store Services SDK, 日志事件
ms.assetid: 4aa591e0-c22a-4c90-b316-0b5d0410af19
ms.localizationpriority: medium
ms.openlocfilehash: ec4bee888d055b5331252e91bfd979d81b976f3c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158431"
---
# <a name="log-custom-events-for-partner-center"></a>记录合作伙伴中心的自定义事件

使用 "合作伙伴中心" 中的 " [使用情况" 报告](../publish/usage-report.md) ，可以获取有关在通用 WINDOWS 平台 (UWP) 应用中定义的自定义事件的信息。 自定义事件是表示应用中的某个事件或活动的任意字符串。 例如，游戏可能定义名为 *firstLevelPassed*、*secondLevelPassed* 等的自定义事件，用户在游戏中通过每个关卡时记录这些事件。

若要记录应用中的自定义事件，请将自定义事件字符串传递到 Microsoft Store Services SDK 提供的 [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 可以在合作伙伴中心的 "[使用情况报告](../publish/usage-report.md)" 的 "**自定义事件**" 部分中查看自定义事件的总次数。

> [!NOTE]
> 你记录到合作伙伴中心的自定义事件与 [Windows 事件](/windows/desktop/Events/windows-events)无关，它们不显示在 **事件查看器**中。

## <a name="prerequisites"></a>必备条件

你的应用必须在应用商店中发布，然后才能在合作伙伴中心内的应用的 **使用情况报告** 中查看自定义日志记录事件。

## <a name="how-to-log-custom-events"></a>如何记录自定义事件

1. 如果先前尚未这样做，请在开发计算机上[安装 Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk)。

2. 在 Visual Studio 中打开项目。

3. 在解决方案资源管理器中，右键单击项目的 " **引用** " 节点，然后单击 " **添加引用**"。

4. 在 **引用管理器**中，展开 " **通用 Windows** "，然后单击 " **扩展**"。

5. 在 Sdk 列表中，单击 " **Microsoft Engagement 框架** " 旁边的复选框，然后单击 **"确定"**。

6. 将以下语句添加到要记录自定义事件的每个代码文件的顶部。
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#EngagementNamespace)]

7. 在要记录自定义事件的每个代码部分中，获取 [StoreServicesCustomEventLogger](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 对象，然后调用 [Log](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log) 方法。 将自定义事件字符串传递到该方法。
    [!code-csharp[EventLogger](./code/StoreSDKSamples/cs/LogEvents.cs#Log)]

    > [!NOTE]
    > 如果应用记录了很多名称很长的自定义事件，则[使用情况报告](../publish/usage-report.md)的加载时间可能很长。 建议为自定义事件使用简短的名称。 

## <a name="related-topics"></a>相关主题

* [使用情况报告](../publish/usage-report.md)
* [记录方法](/uwp/api/microsoft.services.store.engagement.storeservicescustomeventlogger.log)
* [Microsoft Store Services SDK](./microsoft-store-services-sdk.md)