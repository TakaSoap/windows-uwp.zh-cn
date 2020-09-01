---
title: XAML 属性动画
description: 了解如何使用通用 Windows 平台 (UWP) 组合动画直接对 XAML UIElement 上的属性进行动画处理。
ms.date: 09/13/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: stmoy
design-contact: jeffarn
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 0a7ab119df407b45fb391aebf50df5d85f89ec0f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238292"
---
# <a name="animating-xaml-elements-with-composition-animations"></a>用撰写动画为 XAML 元素制作动画效果

本文介绍了一些新属性，这些属性使你可以使用组合动画的性能对 XAML UIElement 进行动画处理，并轻松设置 XAML 属性。

在 Windows 10 1809 版之前，可以选择在 UWP 应用中生成动画：

- 使用[Storyboarded 动画](storyboarded-animations.md)等 XAML 构造，或 ThemeTransition 命名空间中的 _*_ 和 _* ThemeAnimation_ [类。](/uwp/api/windows.ui.xaml.media.animation)
- 使用 [带有 XAML 的可视化层](../../composition/using-the-visual-layer-with-xaml.md)中所述的组合动画。

与使用 XAML 构造相比，使用可视层可以提供更好的性能。 但使用 [ElementCompositionPreview](/uwp/api/Windows.UI.Xaml.Hosting.ElementCompositionPreview) 获取元素的基础组合 [视觉](/uwp/api/windows.ui.composition.visual) 对象，然后使用组合动画对视觉对象进行动画处理，使用起来更复杂。

从 Windows 10 版本1809开始，你可以使用组合动画直接对 UIElement 上的属性进行动画处理，而无需获取基础组合视觉对象。

> [!NOTE]
> 若要在 UIElement 上使用这些属性，UWP 项目目标版本必须为1809或更高版本。 有关配置项目版本的详细信息，请参阅 [版本自适应应用](../../debug-test-perf/version-adaptive-apps.md)。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon.png" alt="XAML controls gallery" width="168"></img></td>
<td>
    <p>如果安装了 <strong style="font-weight: semi-bold">XAML 控件库</strong> 应用，请单击此处 <a href="xamlcontrolsgallery:/item/XamlCompInterop">打开应用，并查看操作中的动画互操作</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="new-rendering-properties-replace-old-rendering-properties"></a>新的呈现属性替换旧的呈现属性

此表显示可用于修改 UIElement 呈现的属性，还可以使用 [CompositionAnimation](/uwp/api/windows.ui.composition.compositionanimation)对其进行动画处理。

| 属性 | 类型 | 说明 |
| -- | -- | -- |
| [不透明度](/uwp/api/windows.ui.xaml.uielement.opacity) | Double | 对象的不透明度 |
| [翻译](/uwp/api/windows.ui.xaml.uielement.translation) | Vector3 | 移动元素的 X/Y/Z 位置 |
| [TransformMatrix](/uwp/api/windows.ui.xaml.uielement.transformmatrix) | Matrix4x4 | 要应用到元素的转换矩阵 |
| [缩放](/uwp/api/windows.ui.xaml.uielement.scale) | Vector3 | 缩放元素，居中于 CenterPoint |
| [旋转](/uwp/api/windows.ui.xaml.uielement.rotation) | Float | 围绕 RotationAxis 和 CenterPoint 旋转元素 |
| [RotationAxis](/uwp/api/windows.ui.xaml.uielement.rotationaxis) | Vector3 | 旋转轴 |
| [CenterPoint](/uwp/api/windows.ui.xaml.uielement.centerpoint) | Vector3 | 刻度和旋转的中心点 |

TransformMatrix 属性值按下面的顺序与 "缩放"、"旋转" 和 "平移" 属性结合： "TransformMatrix"、"缩放"、"旋转" 和 "转换"。

这些属性不会影响元素的布局，因此修改这些属性不会导致新的[度量值](/uwp/api/windows.ui.xaml.uielement.measure) / [排列](/uwp/api/windows.ui.xaml.uielement.arrange)处理过程。

这些属性与组合 [视觉对象](/uwp/api/windows.ui.composition.visual) 上的命名属性具有相同的用途和行为， (除了翻译以外，不是在 Visual) 上。

### <a name="example-setting-the-scale-property"></a>示例：设置刻度属性

此示例演示如何设置按钮的 Scale 属性。

```xaml
<Button Scale="2,2,1" Content="I am a large button" />
```

```csharp
var button = new Button();
button.Content = "I am a large button";
button.Scale = new Vector3(2.0f,2.0f,1.0f);
```

### <a name="mutual-exclusivity-between-new-and-old-properties"></a>新属性和旧属性之间的相互独占性

> [!NOTE]
> **Opacity**属性不强制执行本部分所述的独占性。 无论使用的是 XAML 动画还是组合动画，都可以使用相同的 Opacity 属性。

可以使用 CompositionAnimation 进行动画处理的属性是对多个现有 UIElement 属性的替换：

- [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform)
- [System.windows.uielement.rendertransformorigin](/uwp/api/windows.ui.xaml.uielement.rendertransformorigin)
- [投影](/uwp/api/windows.ui.xaml.uielement.projection)
- [Transform3D](/uwp/api/windows.ui.xaml.uielement.transform3d)

设置 (或) 任何新属性进行动画处理时，不能使用旧属性。 相反，如果将 (或) 任何旧属性设置为动画，则不能使用新属性。

如果使用 ElementCompositionPreview 通过以下方法来获取和管理视觉对象，则也无法使用新属性：

- [ElementCompositionPreview.GetElementVisual](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.getelementvisual)
- [ElementCompositionPreview.SetIsTranslationEnabled](/uwp/api/windows.ui.xaml.hosting.elementcompositionpreview.setistranslationenabled)

> [!IMPORTANT]
> 尝试混合使用这两个属性集将导致 API 调用失败并生成错误消息。

可以通过清除属性从一组属性中进行切换，但是为了简单起见，不建议这样做。 如果属性由 DependencyProperty (（例如，UIElement）支持，则为 UIElement。 ProjectionProperty) 支持投影，然后调用 System.windows.dependencyobject.clearvalue 将其还原到 "未使用" 状态。 否则 (例如，Scale 属性) ，请将属性设置为其默认值。

## <a name="animating-uielement-properties-with-compositionanimation"></a>用 CompositionAnimation 对 UIElement 属性进行动画处理

您可以使用 CompositionAnimation 对表中列出的呈现属性进行动画处理。 [ExpressionAnimation](/uwp/api/windows.ui.composition.expressionanimation)也可以引用这些属性。

使用 UIElement 上的 [StartAnimation](/uwp/api/windows.ui.xaml.uielement.startanimation) 和 [StopAnimation](/uwp/api/windows.ui.xaml.uielement.stopanimation) 方法对 uielement 属性进行动画处理。

### <a name="example-animating-the-scale-property-with-a-vector3keyframeanimation"></a>示例：使用 Vector3KeyFrameAnimation 对 Scale 属性进行动画处理

此示例演示如何对按钮刻度进行动画处理。

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateVector3KeyFrameAnimation();

animation.InsertKeyFrame(1.0f, new Vector3(2.0f,2.0f,1.0f));
animation.Duration = TimeSpan.FromSeconds(1);
animation.Target = "Scale";

button.StartAnimation(animation);
```

### <a name="example-animating-the-scale-property-with-an-expressionanimation"></a>示例：使用 ExpressionAnimation 对 Scale 属性进行动画处理

页面有两个按钮。 第二个按钮通过缩放) 作为第一个按钮，动画处理为大型 (的两倍。

```xaml
<Button x:Name="sourceButton" Content="Source"/>
<Button x:Name="destinationButton" Content="Destination"/>
```

```csharp
var compositor = Window.Current.Compositor;
var animation = compositor.CreateExpressionAnimation("sourceButton.Scale*2");
animation.SetExpressionReferenceParameter("sourceButton", sourceButton);
animation.Target = "Scale";
destinationButton.StartAnimation(animation);
```

## <a name="related-topics"></a>相关主题

- [情节提要动画](storyboarded-animations.md)
- [将可视化层与 XAML 结合使用](../../composition/using-the-visual-layer-with-xaml.md)
- [转换概述](../layout/transforms.md)
