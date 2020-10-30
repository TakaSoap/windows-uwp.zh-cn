---
description: 在伙伴中心定义试验并在应用中编写试验代码后，就可以开始活动试验，并使用合作伙伴中心来查看实验结果。
title: 在合作伙伴中心中管理实验
ms.assetid: D48EE0B4-47F2-455C-8FB9-630769AC5ACE
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: dfcd9819940d21dcc81c5ac698b76381adf05af6
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93030550"
---
# <a name="manage-your-experiment-in-partner-center"></a>在合作伙伴中心中管理实验

在 [伙伴中心定义试验](define-your-experiment-in-the-dev-center-dashboard.md) 并 [为应用程序编写代码以进行试验](code-your-experiment-in-your-app.md)后，就可以激活实验，并使用合作伙伴中心来查看试验结果。 在获取所需的全部数据后，可以结束你的实验，然后选择是继续使用你的所有应用的控件变体中的变量值还是切换到使用其他变体之一中的变量值。

> [!NOTE]
> 激活实验后，伙伴中心会立即开始从检测到日志数据的任何应用收集数据以进行试验。 但是，试验数据可能需要几个小时才能显示在合作伙伴中心。

有关演示如何创建并运行实验的端到端过程的演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="activate-your-experiment"></a>激活实验

如果对伙伴中心试验的参数满意，并且已更新应用代码，则可以激活实验，以便开始从应用收集实验数据。 当实验处于活动状态时，您的应用程序可以检索变体值和报表视图，并将事件转换为合作伙伴中心。

1. 登录[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **“你的应用”** 下，通过要激活的实验选择该应用。
3. 在导航窗格中，选择 " **服务** "，然后选择 " **试验** "。
4. 在 " **项目** " 部分的项目表中，展开包含实验的项目，然后执行以下操作之一：
  * 单击试验的 " **激活** " 链接。 你的试验将添加到页面顶部附近的 " **活动试验** " 部分。
  * 单击试验名称，滚动到 "实验" 页的底部，然后单击 " **激活** "。

> [!IMPORTANT]
> 激活实验后，不可再对实验参数进行修改，除非在创建实验时，选中了 **可编辑实验** 复选框。 在激活实验之前，我们建议你在应用中为实验编码。

## <a name="review-the-results-of-your-experiment"></a>查看实验结果

1. 在合作伙伴中心，返回到你的应用程序的 **试验** 页。
2. 在 " **活动试验** " 部分中，单击活动实验的名称以打开 "试验" 页。
3. 有关活动实验或已完成的实验，此页中的前两个部分提供了实验的结果：
  * **“结果摘要”** 部分列出了实验目标和每个变体的转换率百分比。
  * **“结果详细信息”** 部分为实验中的所有目标的每个变体提供了更多详细信息，包括视图、转换、独特用户、转换率、增量百分比、置信度和重要性。 *置信度* 是用来计算错误边距的估计的可靠性统计度量。 *重要性* 是基于示例大小的统计度量，用来确定结果不是偶然得出，而是由于特定原因。

> [!NOTE]
> 合作伙伴中心仅报告24小时内每个用户的第一个转换事件。 如果用户在 24 小时时段内在应用中触发多个转换事件，仅报告第一个转换事件。 这是为了帮助防止触发很多转换事件的单个用户扭曲一组示例用户得出的实验结果。


## <a name="complete-your-experiment"></a>完成实验

1. 在合作伙伴中心，返回试验页。 有关说明，请参阅上一节。
2. 在 " **结果摘要** " 部分中，执行以下操作之一：
  * 如果要结束实验并继续使用应用程序中控件变体中的变量值，请单击 " **保留** "。
  * 如果要结束试验但要在应用的不同变体中使用变量值，请单击要切换到的变体中的 " **切换** "。
3. 单击 **“确定”** 以确认你想要结束该实验。


## <a name="related-topics"></a>相关主题

* [在合作伙伴中心创建项目和定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [针对实验为你的应用编码](code-your-experiment-in-your-app.md)
* [在合作伙伴中心中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [通过 A/B 测试创建和运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)
