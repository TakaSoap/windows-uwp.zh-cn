---
description: 了解有关如何优化 ad 单位的 viewability 的准则，以及如何通过 "广告性能" 报告衡量 viewability 指标。
title: 优化广告单元的可见性
ms.date: 02/18/2020
ms.topic: article
keywords: windows 10, uwp, 广告, 指南, 可见性
ms.localizationpriority: medium
ms.openlocfilehash: 1653651c41128d359fcacddf0c0d7d408e798d07
ms.sourcegitcommit: 8e0e4cac79554e86dc7f035c4b32cb1f229142b0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/26/2020
ms.locfileid: "88942787"
---
# <a name="optimize-the-viewability-of-your-ad-units"></a>优化广告单元的可见性

>[!WARNING]
> 从2020年6月1日起，将关闭适用于 Windows UWP 应用的 Microsoft Ad 盈利平台。 [了解详细信息](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/db8d44cb-1381-47f7-94d3-c6ded3fea36f/microsoft-ad-monetization-platform-shutting-down-june-1st?forum=aiamgr)

[广告效果报告](../publish/advertising-performance-report.md)包括广告单元的可见性指标。 广告行业日趋重视可视化展示，而不只限于提供展示，所以可见性是一项重要指标。 广告厂商倾向于对可视化展示出价，因为他们的广告有更多机会被用户看到。  

根据 IAB 可见性指南，横幅广告展示如果满足以下条件将被视为可查看：

* 像素要求：广告中像素的 50% 或以上位于应用的可查看空间。
* 时间要求：像素要满足的时间要求是在广告呈现后持续一秒或以上。

视频广告展示如果满足以下条件被视为可查看：

* 像素要求：广告中像素的 50% 或以上位于应用的可查看部分。
* 时间要求：视频满足像素要求并在广告呈现后持续播放两秒。

可见性使用以下公式计算:

**可见性 = [查看的展示数] * 100 / [总广告展示数]**

## <a name="guidelines-to-improve-ad-unit-viewability"></a>改进广告单元可见性指南

若要优化广告单元的可视化展示，请按照以下指南操作。

1. **衡量性能** &nbsp; &nbsp;使用 "[广告性能报表](../publish/advertising-performance-report.md)" 查看每个 ad 单位的 viewability 指标。
2.  **Position your ad control appropriately** &nbsp; 适当 &nbsp; 定位广告控件通常情况下，广告最适合的位置就在折叠 (，即用户可以看到的页面可见部分的底部，无需) 滚动。 显示在页面顶部的广告不可以查看。 请考虑检查页面的每个元素，以了解如何优化应用中的可见空间。 请确保不要将广告控件放置在应用后台。
3.  **管理页面长度** &nbsp; &nbsp;页面内容较短，导致 viewability 较高。 如果你的应用页面需要更长长度，应实现无限滚动。
4.  **修复广告位置** &nbsp; &nbsp;请确保广告在用户滚动时仍处于固定位置。 这将帮助你获得更多的查看时间并增加可见性比率。
5.  **设置不透明度** &nbsp; &nbsp;确保包含广告控件的容器的不透明度为100%。
6.  **实现延迟加载** &nbsp; &nbsp;在用户滚动到包含广告控件的视图之前，请不要加载广告。 这将确保广告显示更久以供用户查看。
7.  **试验各种广告大小** &nbsp; &nbsp;选择几个 ad 大小，并使用[广告性能报表](../publish/advertising-performance-report.md)来度量每个 ad 大小的 viewability。 选择最适合你的那一个。
8.  重新**访问应用设计** &nbsp; &nbsp;改进应用页面设计以减少页面加载时间。 用户常常在加载更快的页面上呆得更久。
