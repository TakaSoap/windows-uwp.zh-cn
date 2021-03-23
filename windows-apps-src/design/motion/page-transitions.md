---
title: 页面过渡
description: 了解如何使用通用 Windows 平台 (UWP) 页面过渡向用户提供应用中各页面间关系的反馈。
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0b2bdb95c6383559899e6baf06c391bbd250f3ca
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804981"
---
# <a name="page-transitions"></a>页面过渡

页面过渡可将用户导航到应用中的各个页面，并提供反馈作为页面之间的关系。 页面过渡可帮助用户了解他们是否处于导航层次结构的顶部、在同级页面之间移动或导航到页面层次结构的更深层。

为应用、*页面刷新* 和 *钻取* 内的页面之间的导航提供了两个不同的动画，由 [**NavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.navigationtransitioninfo) 的子类表示。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong> 应用，请单击此处 <a href="xamlcontrolsgallery:/item/PageTransition">打开应用，并查看操作中的页面过渡</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="page-refresh"></a>页面刷新

页面刷新是传入内容的上滑动画与淡入动画的组合。 请在用户转到导航堆栈的顶部时使用页面刷新，例如选项卡或左侧导航栏项目之间的导航。

所需的感觉是用户已重新开始。

![页面刷新动画](images/page-refresh.gif)

页面刷新动画由 [**EntranceNavigationTransitionInfoClass**](/uwp/api/windows.ui.xaml.media.animation.entrancenavigationtransitioninfo)表示。

```csharp
// Explicitly play the page refresh animation
myFrame.Navigate(typeof(Page2), null, new EntranceNavigationTransitionInfo());

```

**注意**：[**帧**](/uwp/api/windows.ui.xaml.controls.frame)自动使用 [**NavigationThemeTransition**](/uwp/api/windows.ui.xaml.media.animation.navigationthemetransition) 对两个页面之间的导航进行动画处理。 默认情况下，动画是页面刷新。

## <a name="drill"></a>钻取

请在用户导航到应用的更深层时使用钻取，例如在选择一个项目后显示更多信息。

所需的感觉是用户已进入应用的更深层。

![钻取动画](images/drill.gif)

钻取动画由 [**DrillInNavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.drillinnavigationtransitioninfo) 类表示。

```csharp
// Play the drill in animation
myFrame.Navigate(typeof(Page2), null, new DrillInNavigationTransitionInfo());
```

## <a name="horizontal-slide"></a>水平滑动

使用水平滑动显示同级页面彼此相邻显示。 [NavigationView](../controls-and-patterns/navigationview.md)控件自动将此动画用于顶部导航，但如果您要构建自己的水平导航体验，则可以使用 SlideNavigationTransitionInfo 来实现水平滑动。

所需的感觉是，用户在彼此相邻的页面之间导航。 

```csharp
// Navigate to the right, ie. from LeftPage to RightPage
myFrame.Navigate(typeof(RightPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromRight } );

// Navigate to the left, ie. from RightPage to LeftPage
myFrame.Navigate(typeof(LeftPage), null, new SlideNavigationTransitionInfo() { Effect = SlideNavigationTransitionEffect.FromLeft } );
```

## <a name="suppress"></a>取消

若要避免在导航过程中播放任何动画，请使用 [**SuppressNavigationTransitionInfo**](/uwp/api/windows.ui.xaml.media.animation.suppressnavigationtransitioninfo) 来代替其他 **NavigationTransitionInfo** 子类型。

```csharp
// Suppress the default animation
myFrame.Navigate(typeof(Page2), null, new SuppressNavigationTransitionInfo());
```

如果要使用[连接的动画](connected-animation.md)构建自己的过渡或显示/隐藏动画，那么禁止显示动画很有用。

## <a name="backwards-navigation"></a>向后导航

要在向后导航时播放特定过渡，可以使用 `Frame.GoBack(NavigationTransitionInfo)`.

当基于屏幕大小动态修改导航行为时，这可能很有用;例如，在响应式列表/详细情况下。

## <a name="related-topics"></a>相关主题

- [在两个页面之间导航](../basics/navigate-between-two-pages.md)
- [UWP 应用中运动](index.md)
