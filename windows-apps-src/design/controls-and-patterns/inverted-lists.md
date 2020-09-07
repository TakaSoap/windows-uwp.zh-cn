---
title: 反转列表
description: 了解如何创建反转列表，并使用它在通用 Windows 平台 (UWP) 应用的 ListView 控件底部添加新项。
label: Inverted lists
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 40e99ecb4719569e95b55a8cafb411df26ed265e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89169881"
---
# <a name="inverted-lists"></a>反转列表

 

在采用外观不同的项表示发送方/接收方的聊天体验中，你可以使用列表视图表示对话。  使用不同的颜色和水平对齐方式将来自发送方/接收方的消息分隔开来有助于用户在对话中快速适应。

> **重要的 API**：[ListView 类](/uwp/api/windows.ui.xaml.controls.listview)、[ItemsStackPanel 类](/uwp/api/windows.ui.xaml.controls.itemsstackpanel)、[ItemsUpdatingScrollMode 属性](/uwp/api/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode)
 
你通常需要将列表呈现为看起来从底部向上增长，而不是从顶部向下增长。  当新消息到达并添加到末尾时，之前的消息会向上滑动，以留出空间，从而将用户的注意力吸引到最新到达的内容上。  但是，如果用户向上滚动以查看之前的回复，则新消息的到达不得引起会干扰用户注意力的视觉转移。

![带倒排列表的聊天应用](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>创建反转列表

若要创建反转列表，请使用将 [ItemsStackPanel](/uwp/api/windows.ui.xaml.controls.itemsstackpanel) 作为其项目面板的列表视图。 在 ItemsStackPanel 上，将 [ItemsUpdatingScrollMode](/uwp/api/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode) 设置为 [KeepLastItemInView](/uwp/api/windows.ui.xaml.controls.itemsupdatingscrollmode)。

> [!IMPORTANT]
> 从 Windows 10（版本 1607）开始可使用 **KeepLastItemInView** 枚举值。 当你的应用在较早版本的 Windows 10 上运行时，你无法使用此值。

此示例显示如何将列表视图的项对齐到底部，并指示当项发生更改时，最后一项应保留在视图中。
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>应做事项和禁止事项

- 将来自发送方/接收方的消息在两边对齐，使用户可以清楚地了解对话流程。
- 如果用户已经在对话末尾等待下一条消息，则以动画方式将现有消息移开，以显示最新消息。
- 如果用户未在对话末尾阅读，请不要移动项，以免干扰用户注意力。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 自下而上列表示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)