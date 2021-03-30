---
description: 列表/细节模式可显示项列表和当前选定项的详细信息。 此模式通常用于电子邮件和联系人列表/通讯簿。
title: 列表/细节
ms.assetid: 45C9FE8B-ECA6-44BF-8DDE-7D12ED34A7F7
label: List/details
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5532a0a5a5424d69c94d33db559aaa7afe786d83
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104806082"
---
# <a name="listdetails-pattern"></a>列表/细节模式

列表/细节模式具有一个列表窗格（通常带有[列表视图](lists.md)）和一个内容细节窗格。 当选择列表中的项时，将会更新细节窗格。 此模式通常用于电子邮件和通讯簿。

> **重要的 API**：[ListView 类](/uwp/api/Windows.UI.Xaml.Controls.ListView)、[SplitView 类](/uwp/api/windows.ui.xaml.controls.splitview)

![列表-细节模式的示例](images/list-detail-pattern.png)

> [!TIP]
> 如果要使用为你实现此模式的 XAML 控件，则建议使用 Windows 社区工具包中的 [ListDetailsView XAML 控件](/windows/communitytoolkit/controls/masterdetailsview)。

## <a name="is-this-the-right-pattern"></a>这是正确的模式吗？

如果希望进行以下操作，则适用列表/细节模式：

- 生成电子邮件应用、通讯簿或任何基于列表-细节布局的应用。
- 定位并设置大型内容集合的优先级。
- 允许从列表中快速添加和删除项，并在上下文之间反复执行操作。

## <a name="choose-the-right-style"></a>选择正确的样式

在实现列表/细节模式时，我们建议根据可用屏幕空间量使用堆叠样式或并排样式。

| 可用窗口宽度 | 建议样式 |
|------------------------|-------------------|
| 320 epx-640 epx        | 堆叠           |
| 641 epx 或更宽       | 并排      |

## <a name="stacked-style"></a>堆叠样式

在堆叠样式中，一次只有一个窗格可见：列表窗格或细节窗格。

![堆叠模式下的列表细节](images/patterns-md-stacked.png)

用户从列表窗格开始，然后通过在列表中选择一个项来“深入”到细节窗格。 对用户来说，它表现为列表和细节视图存在于两个单独的页面上。

### <a name="create-a-stacked-listdetails-pattern"></a>创建堆叠的列表/细节模式

创建堆叠的列表/细节模式的一种方法是为列表窗格和细节窗格使用单独的页面。 将列表视图放在一个页面上，细节窗格放在一个单独的页面上。

![堆叠样式列表细节的各个部分](images/patterns-ld-stacked-parts.png)

对于列表视图页面，[列表视图](lists.md)控件适用于显示可包含图像和文本的列表。

对于细节视图页面，使用[内容元素](../layout/layout-panels.md)最为合理。 如果你有大量的单独字段，请考虑使用网格布局将元素排列为一个表单  。

有关页面间的导航，请参阅 [Windows 应用的导航历史记录和向后导航](../basics/navigation-history-and-backwards-navigation.md)。

## <a name="side-by-side-style"></a>并排样式

在并排样式中，列表窗格和细节窗格同时可见。

![列表/细节模式](images/patterns-listdetail-400x227.png)

列表模式中的列表具有一个选项视觉对象，用于指示当前选定的项。 在列表中选择一个新项将更新细节窗格。

### <a name="create-a-side-by-side-listdetails-pattern"></a>创建并排列表/细节模式

创建并排列表/细节模式的一种方法是使用[拆分视图](split-view.md)控件。 将列表视图放在拆分视图窗格中，细节视图放在拆分视图内容中。

![列表细节拆分视图的各个部分](images/patterns-ld-splitview-parts.png)

对于列表窗格，[列表视图](lists.md)控件适用于显示可包含图像和文本的列表。

对于细节内容，使用[内容元素](../layout/layout-panels.md)最为合理。 如果你有大量的单独字段，请考虑使用网格布局将元素排列为一个表单  。

## <a name="adaptive-layout"></a>自适应布局

要实现适用于任何屏幕大小的列表/细节模式，请使用[自适应布局](../layout/layouts-with-xaml.md)创建响应式 UI。

![自适应列表细节布局](images/patterns_listdetail.png)

### <a name="create-an-adaptive-listdetails-pattern"></a>创建自适应列表/细节模式
要创建自适应布局，请为 UI 定义不同的 [VisualStates](/uwp/api/windows.ui.xaml.visualstate)，然后使用 [AdaptiveTriggers](/uwp/api/Windows.UI.Xaml.AdaptiveTrigger) 为不同的状态声明断点   。

## <a name="get-the-sample-code"></a>获取示例代码

以下示例使用自适应布局实现列表/细节模式并演示到静态数据、数据库和在线资源的数据绑定： 
- [大纲/细节示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail) 
- [ListView 和 GridView 示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlListView)
- [Windows Template Studio 大纲/细节示例](https://github.com/Microsoft/WindowsTemplateStudio/tree/master/templates/Uwp/Pages/MasterDetail)
- [客户订单数据库示例](https://github.com/Microsoft/Windows-appsample-customers-orders-database)
- [RSS 阅读器示例](https://github.com/Microsoft/Windows-appsample-rssreader)

> [!TIP]
> 如果要使用为你实现此模式的 XAML 控件，则建议使用 Windows 社区工具包中的 [ListDetailsView XAML 控件](/windows/communitytoolkit/controls/masterdetailsview)。

## <a name="related-articles"></a>相关文章

- [列表](lists.md)
- [搜索](search.md)
- [应用和命令栏](app-bars.md)
- [ListView 类](/uwp/api/Windows.UI.Xaml.Controls.ListView)
- [SplitView 类](/uwp/api/windows.ui.xaml.controls.splitview)
