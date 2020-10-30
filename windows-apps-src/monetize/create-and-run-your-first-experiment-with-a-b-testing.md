---
description: 在本操作实例中，通过 A/B 测试创建、运行和管理你的第一个实验。
title: 创建并运行你的第一个实验
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, Microsoft Store Services SDK, A/B 测试, 实验
ms.localizationpriority: medium
ms.openlocfilehash: 8f7f58e32194be5d73a6a946e4a49efafcfba572
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033240"
---
# <a name="create-and-run-your-first-experiment"></a>创建并运行你的第一个实验

本演练中的操作：
* 在合作伙伴中心创建试验项目，该 [项目](run-app-experiments-with-a-b-testing.md#terms) 定义几个表示应用按钮的文本和颜色的远程变量。
* 使用检索远程变量值的代码创建一个应用，使用此数据更改按钮的背景色，并将日志视图和转换事件数据返回到合作伙伴中心。
* 在项目中创建用于测试成功更改应用按钮的背景色是否会增加按钮单击数的实验。
* 运行该应用以收集实验数据。
* 查看合作伙伴中心的试验结果，选择要为应用程序的所有用户启用的变化形式，并完成试验。

有关使用合作伙伴中心进行 A/B 测试的概述，请参阅 [使用 a/b 测试运行应用试验](run-app-experiments-with-a-b-testing.md)。

## <a name="prerequisites"></a>先决条件

若要执行本演练，必须具有合作伙伴中心帐户，并且必须按照 [使用 a/B 测试运行应用试验](run-app-experiments-with-a-b-testing.md)中所述配置开发计算机。

## <a name="create-a-project-with-remote-variables-in-partner-center"></a>在合作伙伴中心创建包含远程变量的项目

1. 登录[合作伙伴中心](https://partner.microsoft.com/dashboard)。
2. 如果合作伙伴中心中已有要用于创建试验的应用，请在 "合作伙伴中心" 中选择该应用。 如果合作伙伴中心还没有应用，请 [通过保留名称创建一个新应用](../publish/create-your-app-by-reserving-a-name.md) ，并在合作伙伴中心选择该应用。
3. 在导航窗格中，单击 " **服务** "，然后单击 " **试验** "。
4. 在下一页的 " **项目** " 部分中，单击 " **新建项目** " 按钮。
5. 在 " **新建项目** " 页上，输入 "名称" **按钮，单击** 新项目的 "试验"。
6. 展开 " **远程变量** " 部分，并单击 "添加四次 **变量** "。 现在应该有四个空变量行。
  * 在第一行中，键入 " **buttonText** " 作为 "变量名称"，然后在 " **默认值** " 列中键入 **灰色按钮** 。
  * 在第二行中，在 " **默认值** " 列中键入 **r** 作为变量名，然后键入 " **128** "。
  * 在第三行中，键入 **g** 作为变量名，在 " **默认值** " 列中键入 **128** 。
  * 在第四行中，在 " **默认值** " 列中键入 **b** 作为变量名称并键入 " **128** "。
7. 单击 " **保存** "，记下 " **SDK 集成** " 部分中显示的 " [项目 ID](run-app-experiments-with-a-b-testing.md#terms) " 值。 在下一部分中，你将更新你的应用代码，并在你的代码中引用此值。

## <a name="code-the-experiment-in-your-app"></a>在应用中为实验编码

1. 在 Visual Studio 中，使用 Visual C# 创建新的通用 Windows 平台项目。 将该项目命名为 **SampleExperiment** 。
2. 在“解决方案资源管理器”中，展开项目节点、右键单击 **“引用”** ，然后选择 **“添加引用”** 。
3. 在 **引用管理器** 中，展开 " **通用 Windows** "，然后单击 " **扩展** "。
4. 在 Sdk 列表中，选中 " **Microsoft Engagement 框架** " 旁边的复选框，然后单击 **"确定"** 。
5. 在 **解决方案资源管理器** 中，双击 "MainPage" 以打开应用中主页的设计器。
6. 将 **“按钮”** 从 **“工具箱”** 拖动到该页。
7. 在设计器上双击按钮以打开代码文件，并为 **Click** 事件添加事件处理程序。  
8. 将代码文件的所有内容替换为以下代码。 将该 ```projectId``` 变量分配给你在上一部分中从合作伙伴中心获取的 " [项目 ID](run-app-experiments-with-a-b-testing.md#terms) " 值。
    :::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreSDKSamples/cs/ExperimentPage.xaml.cs" id="SampleExperiment":::

9. 保存代码文件并生成项目。

## <a name="create-the-experiment-in-partner-center"></a>在合作伙伴中心创建试验

1. 返回到 "合作伙伴中心" 中的 **按钮单击试验** 项目页面。
2. 在 " **试验** " 部分，单击 " **新建试验** " 按钮。
3. 在 " **试验详细信息** " 部分中，在 " **实验名称** " 字段中键入 " **优化" 按钮** 的名称。
4. 在 " **查看事件** " 部分中，在 " **查看事件名称** " 字段中键入 **userViewedButton** 。 请注意，此名称与在上一部分添加的代码中记录的视图事件字符串相匹配。
5. 在 " **目标和转换事件** " 部分中，输入以下值：
  * 在 **“目标名称”** 字段中，键入 **“提高按钮单击量”** 。
  * 在 " **转换事件名称** " 字段中，键入名称 **userClickedButton** 。 请注意，此名称与在上一部分添加的代码中记录的转换事件字符串相匹配。
  * 在 " **目标** " 字段中，选择 " **最大化** "。
6. 在 " **远程变量和变体** " 部分中，确认已选中 " **平均分布** " 复选框，以便将变体平均分布于应用。
7. 将变量添加到你的实验：
    1. 单击下拉控件，选择 " **buttonText** "，然后单击 " **添加变量** "。 字符串 **灰色按钮** 应自动出现在 **变体** 中 (此值从项目设置) 派生的列。 在 " **变体 B** " 列中，键入 **蓝色按钮** 。
    2. 再次单击下拉控件，选择 " **r** "，然后单击 " **添加变量** "。 字符串 **128** 应自动出现在 **变体** 中的列中。 在 " **变体 B** " 列中，键入 **1** 。
    3. 再次单击下拉控件，选择 " **g** "，然后单击 " **添加变量** "。 字符串 **128** 应自动出现在 **变体** 中的列中。 在 " **变体 B** " 列中，键入 **1** 。  
    4. 再次单击下拉控件，选择 **b** ，然后单击 " **添加变量** "。 字符串 **128** 应自动出现在 **变体** 中的列中。 在 " **变体 B** " 列中，键入 **255** 。  
8. 单击 " **保存** "，然后单击 " **激活** "。

> [!IMPORTANT]
> 激活实验后，不可再对实验参数进行修改，除非在创建实验时，选中了 **可编辑实验** 复选框。 通常我们建议你在激活实验之前，在应用中为实验编码。

## <a name="run-the-app-to-gather-experiment-data"></a>运行该应用以收集实验数据

1. 运行你在之前创建的 **SampleExperiment** 应用。
2. 确认你看到的是灰色按钮还是蓝色按钮。 单击该按钮，然后关闭应用。
3. 在相同计算机上重复上述步骤几次，确认应用显示相同的按钮色。

## <a name="review-the-results-and-complete-the-experiment"></a>查看结果并完成实验

完成上一部分后至少等待几小时，然后遵循以下步骤查看实验结果并完成实验。

> [!NOTE]
> 一旦激活实验，伙伴中心就会立即开始从检测到日志数据的任何应用收集数据以进行试验。 但是，试验数据可能需要几个小时才能显示在合作伙伴中心。

1. 在合作伙伴中心，返回到你的应用程序的 **试验** 页。
2. 在 " **活动试验** " 部分中，单击 " **优化按钮单击** " 以切换到此试验的页面。
3. 确认 " **结果摘要** " 和 " **结果详细信息** " 部分中显示的结果与您希望看到的内容相符。 有关这些部分的详细信息，请参阅 [在合作伙伴中心管理试验](manage-your-experiment.md#review-the-results-of-your-experiment)。
    > [!NOTE]
    > 合作伙伴中心仅报告24小时内每个用户的第一个转换事件。 如果用户在 24 小时时段内在应用中触发多个转换事件，仅报告第一个转换事件。 这是为了帮助防止触发很多转换事件的单个用户扭曲一组示例用户得出的实验结果。

4. 现在，你已准备好结束实验了。 在 **结果摘要** 部分的 **变体 B** 列中，单击 **“切换”** 。 这会将应用的所有用户切换到蓝色按钮。
5. 单击 **“确定”** 以确认你想要结束该实验。
6. 运行你在上一部分中创建的 **SampleExperiment** 应用。
7. 确认你看到了一个蓝色的按钮。 请注意，你的应用接收更新的变体分配最多需要两分钟。

## <a name="related-topics"></a>相关主题

* [在合作伙伴中心创建项目和定义远程变量](create-a-project-and-define-remote-variables-in-the-dev-center-dashboard.md)
* [针对实验为你的应用编码](code-your-experiment-in-your-app.md)
* [在合作伙伴中心中定义实验](define-your-experiment-in-the-dev-center-dashboard.md)
* [在合作伙伴中心中管理实验](manage-your-experiment.md)
* [通过 A/B 测试运行应用实验](run-app-experiments-with-a-b-testing.md)
