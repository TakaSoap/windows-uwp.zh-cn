---
description: 必须先创建一个项目，并在合作伙伴中心定义远程变量，然后才能在通用 Windows 平台 (UWP) 应用中运行试验。
title: 在合作伙伴中心中创建实验项目
ms.assetid: C3809FF1-0A6A-4715-B989-BE9D0E8C9013
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: 19eccb6ebc7556fb3dc2c6c11969361e19f802f3
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032760"
---
# <a name="create-an-experiment-project-in-partner-center"></a>在合作伙伴中心中创建实验项目

若要开始试验，请在合作伙伴中心为你的应用程序创建一个试验 [项目](run-app-experiments-with-a-b-testing.md#terms) ，并定义你的应用程序可以访问的远程变量。

以下说明介绍了用于创建项目的核心步骤。 有关演示如何创建项目、然后运行实验的端到端过程的详细演练，请参阅[通过 A/B 测试来创建并运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)。

## <a name="instructions"></a>Instructions

1. 登录[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 在 **“你的应用”** 下，选择你想为之创建实验的应用。
3. 在导航窗格中，选择 " **服务** "，然后选择 " **试验** "。
4. 在 " **试验** " 页上，单击 " **项目** " 部分中的 " **新建项目** " 按钮。 如果已创建一个或多个项目，则这些项目将在 " **项目** " 部分中列出。
5. 在 " **新建项目** " 页上，输入新项目的名称。
6. 在 " **远程变量** " 部分中，添加要对此项目中的所有实验可用的 [变量](run-app-experiments-with-a-b-testing.md#terms) ，并为每个变量定义默认值。 此处指定的默认值将用于实验的控件组，也将用于没有参与此实验的任何用户。
  1. 如果 " **远程变量** " 部分处于折叠状态，请单击部分标题上的 " **显示** "。
  2. 单击 " **添加变量** "，创建要用于此项目中的任何试验的每个新变量，然后键入变量名称和变量的默认值。
  3. 添加完变量后，单击 " **保存** "。
3. 在 " **SDK 集成** " 部分中，记下 " [项目 ID](run-app-experiments-with-a-b-testing.md#terms) " 值。 当你对 [应用程序进行试验](code-your-experiment-in-your-app.md)时，必须在代码中引用此项目 ID，以便可以接收变体数据和报表视图以及将事件转换为合作伙伴中心。

> [!NOTE]
> 在项目中的实验处于活动状态时，不可编辑、添加或删除远程变量。 此限制有助于保护活动实验的控件组的数据完整性。


## <a name="next-steps"></a>后续步骤

创建项目后，可以[为实验编写应用代码](code-your-experiment-in-your-app.md)以开始检索应用中的变量值，并且可以[在项目中创建实验](define-your-experiment-in-the-dev-center-dashboard.md)。

## <a name="related-topics"></a>相关主题

* [针对实验为你的应用编码](code-your-experiment-in-your-app.md)
* [在合作伙伴中心中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作伙伴中心中管理实验](manage-your-experiment.md)
* [通过 A/B 测试创建和运行你的第一个实验](create-and-run-your-first-experiment-with-a-b-testing.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)
