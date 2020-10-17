---
title: 增强现有 ScrollViewer 体验
description: 了解如何使用 XAML ScrollViewer 和 ExpressionAnimations 来创建动态的输入驱动运动体验。
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, 动画
ms.localizationpriority: medium
ms.openlocfilehash: 438f108a07349da6515443e64bd4494529b8e6a0
ms.sourcegitcommit: fe21402578a1f434769866dd3c78aac63dbea5ea
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/17/2020
ms.locfileid: "92152406"
---
# <a name="enhance-existing-scrollviewer-experiences"></a>增强现有 ScrollViewer 体验

本文介绍了如何使用 XAML ScrollViewer 和 ExpressionAnimation 来创建动态的输入驱动运动体验。

## <a name="prerequisites"></a>必备条件

我们在此假设你熟悉这些文章中所述的概念：

- [输入驱动的动画](input-driven-animations.md)
- [基于关系的动画](relation-animations.md)

## <a name="why-build-on-top-of-scrollviewer"></a>为什么要在 ScrollViewer 之上生成？

通常情况下，可以使用现有 XAML ScrollViewer 为应用内容创建可滚动和可缩放的图面。 引入 Fluent 设计语言后，现在应该着重考虑如何使用滚动或缩放图面的操作来驱动其他运动体验。 例如，使用滚动来推动后台的模糊动画或推动“粘滞标头”的位置。

在这些情况下，将充分利用滚动和缩放等行为或操作体验，使应用的其他部分更为动态。 这些体验反过来也让应用感觉更一致，同时在最终用户看来也更为让人难忘。 通过使应用 UI 让人难忘，最终用户与应用互动的频率将更高，时间也更长。

## <a name="what-can-you-build-on-top-of-scrollviewer"></a>在 ScrollViewer 之上能构建什么内容？

你可以利用 ScrollViewer 的位置生成大量动态体验：

- 视差 – 使用 ScrollViewer 位置以相对滚动位置的速率移动背景或前景内容。
- 粘滞标头 – 使用 ScrollViewer 的位置来实现动画效果，并将标头“粘”在一个位置。
- 输入驱动的效果 – 使用 Scrollviewer 的位置来实现 Composition 效果，如模糊。

一般情况下，通过引用 ExpressionAnimation 的 ScrollViewer 位置，可以创建相对于滚动量动态变化的动画。

![含视差效果的列表视图](images/animation/parallax.gif)

![害羞标头](images/animation/shy-header.gif)

## <a name="using-scrollviewermanipulationpropertyset"></a>使用 ScrollViewerManipulationPropertySet

若要使用 XAML ScrollViewer 创建这些动态体验，必须能够在动画中引用滚动位置。 这是通过从名为 ScrollViewerManipulationPropertySet 的 XAML ScrollViewer 访问 CompositionPropertySet 来完成的。
ScrollViewerManipulationPropertySet 包含一个名为 "转换" 的 System.numerics.vector2 属性，该属性提供对 ScrollViewer 的滚动位置的访问。 然后可以在 ExpressionAnimation 中像引用任何其他 CompositionPropertySet 一样引用此属性。

一般的开始步骤：

1. 通过 ElementCompositionPreview 访问 ScrollViewerManipulationPropertySet。
    - `ElementCompositionPreview.GetScrollViewerManipulationPropertySet(ScrollViewer scroller)`
1. 创建引用 PropertySet 的 Translation 属性的 ExpressionAnimation。
    - 不要忘了设置 Reference 参数！
1. 将 CompositionObject 的属性作为 ExpressionAnimation 的操作目标。

> [!NOTE]
> 建议将从 GetScrollViewerManipulationPropertySet 方法返回的 PropertySet 分配给类变量。 这将确保该属性集不会作为垃圾回收，因此对于引用了它的 ExpressionAnimation 不会产生任何影响。 ExpressionAnimation 不维护对等式中使用的任何对象的强引用。

## <a name="example"></a>示例

看一下以上显示的 Parallax 示例是如何组合出来的。 为便于参考，[GitHub 上的窗口 UI 开发实验室存储库](https://github.com/microsoft/WindowsCompositionSamples)提供该应用的所有代码。

第一件事是获取对 ScrollViewerManipulationPropertySet 的引用。

```csharp
_scrollProperties =
    ElementCompositionPreview.GetScrollViewerManipulationPropertySet(myScrollViewer);
```

下一步创建利用 ScrollViewer 的滚动位置定义等式的 ExpressionAnimation。

```csharp
_parallaxExpression = compositor.CreateExpressionAnimation();
_parallaxExpression.SetScalarParameter("StartOffset", 0.0f);
_parallaxExpression.SetScalarParameter("ParallaxValue", 0.5f);
_parallaxExpression.SetScalarParameter("ItemHeight", 0.0f);
_parallaxExpression.SetReferenceParameter("ScrollManipulation", _scrollProperties);
_parallaxExpression.Expression = "(ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight)) * ParallaxValue - (ScrollManipulation.Translation.Y + StartOffset - (0.5 * ItemHeight))";
```

> [!NOTE]
> 此外，还可以利用 ExpressionBuilder 帮助程序类构造这个 Expression，无需 String：

> ```csharp
> var scrollPropSet = _scrollProperties.GetSpecializedReference<ManipulationPropertySetReferenceNode>();
> var parallaxValue = 0.5f;
> var parallax = (scrollPropSet.Translation.Y + startOffset);
> _parallaxExpression = parallax * parallaxValue - parallax;
> ```

最后，使用此 ExpressionAnimation 并以需要视差处理的 Visual 为目标。 在本例中，就是每个列表项的图像。

```csharp
Visual visual = ElementCompositionPreview.GetElementVisual(image);
visual.StartAnimation("Offset.Y", _parallaxExpression);
```
