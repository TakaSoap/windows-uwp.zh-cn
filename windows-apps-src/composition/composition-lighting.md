---
title: 组合照明
description: 组合照明 Api 可用于向应用程序中添加动态3D 光照。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5531382ef46346a40844a8eb5a5a77c0ad565fbb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154701"
---
# <a name="using-lights-in-windows-ui"></a>在 Windows UI 中使用灯光

使用 Windows UI 组合 Api 可以创建实时动画和效果。 组合照明实现了2D 应用程序中的3D 光照。 在本概述中，我们将学习如何设置组合灯的功能，如何识别要接收每个灯光的视觉对象，并使用各种效果来定义内容内容。

> [!NOTE]
> 若要了解 [XamlLight](/uwp/api/windows.ui.xaml.media.xamllight) 对象如何将 [CompositionLights](/uwp/api/Windows.UI.Composition.CompositionLight) 应用于照亮 xaml UIElements，请参阅 [xaml 照明](xaml-lighting.md)。

使用组合照明，你可以通过允许以下操作创建有趣的 UI：

- 不依赖场景中的其他对象来实现沉浸式方案，如音乐播放场景。
- 能够将一个对象与一个光源配对，以便它们与场景的其余部分一起移动，以[实现流畅的突出显示。](../design/style/reveal.md)
- 以组的形式转换光线和整个场景以创建材料和深度。

组合照明支持三个关键概念： **光源**、 **目标**和 **SceneLightingEffect**。

## <a name="light"></a>亮

[CompositionLight](/uwp/api/windows.ui.composition.compositionlight) 允许您创建各种光源，并将其置于坐标空间中。 这些光源的目标是您希望将视觉对象识别为发亮的视觉对象。

### <a name="light-types"></a>灯光类型

| 类型 | 描述 |
| --- | --- |
| [AmbientLight](/uwp/api/windows.ui.composition.ambientlight) | 发出显示在场景中的所有内容反射的非定向光线的光源。 |
| [DistantLight](/uwp/api/windows.ui.composition.distantlight) | 无限远的远距离光源，它以单个方向发出光。 类似于 sun。 |
| [System.windows.media.media3d.pointlight>](/uwp/api/windows.ui.composition.pointlight) | 光的点，可在所有方向发出光。 类似于灯泡。 |
| [式](/uwp/api/windows.ui.composition.spotlight) | 发出灯光的内部和外部圆锥的光源。 类似于闪光灯。 |

## <a name="targets"></a>Targets

当光源面向视觉对象 (添加到 [目标](/uwp/api/windows.ui.composition.compositionlight.targets) 列表) 时，视觉对象及其所有子对象均可感知和响应此光源。 这可能是一个简单的操作，就像在树的根设置 System.windows.media.media3d.pointlight> 源一样，下面的所有视觉对象都反应点光源方向的动画。

**ExclusionsFromTargets** 使你能够以类似于添加目标的方式删除视觉对象或视觉对象子树的照明。 由所排除的视觉对象以根为根的子树中的子元素不会亮起。

### <a name="sample-targets"></a>示例 (目标) 

在下面的示例中，我们使用 CompositionPointLight 来面向 XAML TextBlock。

```cs
    _pointLight = _compositor.CreatePointLight();
    _pointLight.Color = Colors.White;
    _pointLight.CoordinateSpace = text; //set up co-ordinate space for offset
    _pointLight.Targets.Add(text); //target XAML TextBlock
```

通过将动画添加到点光的偏移量，可轻松实现 shimmering 效果。

```cs
_pointLight.Offset = new Vector3(-(float)TextBlock.ActualWidth, (float)TextBlock.ActualHeight / 2, (float)TextBlock.FontSize);
```

有关详细信息，请参阅 WindowUIDevLabs 示例条样中的完整 [文本 Shimmer](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 14393/TextShimmer) 示例。

## <a name="restrictions"></a>限制

确定 CompositionLight 将发亮的内容时，有几个因素需要考虑。

概念 | 详细信息
--- | ---
**环境光线** | 将非环境光源添加到场景会关闭所有现有光源。  非环境光线所面向的项将显示为黑色。  若要使视觉对象的视觉对象不是面向视觉对象的视觉对象，请将环境光源与其他光源结合使用。
**灯光数量** | 你可以使用任何组合中的任意两个非环境组合光源来面向你的 UI。 不限制环境光线;点光、点光和远处光线是。
**生存期** | CompositionLight 可能会遇到生存期条件 (示例：垃圾回收器可能会在) 使用光对象之前回收该对象。  建议通过将灯光添加为成员来帮助应用程序管理生存期，以保留对灯光的引用。
**转换** | 光源必须位于 UI 之上的节点中，该节点使用视觉对象结构中的 [透视转换](../design/layout/3-d-perspective-effects.md) 等效果来正确绘制。
**目标和协调空间** | CoordinateSpace 是必须在其中设置所有光源属性的视觉空间。 CompositionLight 必须位于 CoordinateSpace 树中。

## <a name="lighting-properties"></a>照明属性

根据所用的光线类型，光可以具有衰减和空间的属性。 并非所有光类型都使用所有属性。

属性 | 描述
--- | ---
**颜色** | 光的 [颜色](/uwp/api/windows.ui.color) 。 照明颜色值由 [D3D](../graphics-concepts/light-properties.md) 漫射、环境和镜面定义，用于定义要发出的颜色。 光源使用 RGBA 值用于光源;不使用 alpha 颜色部分。
**方向** | 光的方向。 光源的方向相对于其 [CoordinateSpace](/uwp/api/windows.ui.composition.distantlight.coordinatespace) 视觉对象指定。
**坐标空间** | 每个视觉对象都有一个隐式三维坐标空间。 X 方向从左到右。 Y 方向从上到下。 Z 方向是平面的点。 此坐标的原始点为视觉对象的左上角，单位为与设备无关的像素 (DIP) 。 此坐标中定义的光线偏移量。
**内部和外部圆锥** | 聚光发出的光锥包括两个部分：一个明亮的内锥和一个外锥。 组合允许您控制内部和外部圆锥度和颜色。
**Offset** | 光源相对于其坐标空间视觉对象的偏移量。

> [!NOTE]
> 当多个光源碰到同一视觉对象时，或者当光源的颜色值大到足以超过1.0 时，光源的颜色可能会改变，因为光源颜色通道的钳位。

### <a name="advanced-lighting-properties"></a>高级照明属性

属性 | 描述
--- | ---
**光** | 控制光的亮度。
**衰减** | 衰减控制光的强度如何随距离属性指定的最大距离而减弱。  可以使用常量、Quadradic 和线性衰减属性。

## <a name="getting-started-with-lighting"></a>带有照明的入门

请按照以下常规步骤添加光源：

- 创建和放置光源：创建光源并将其放在指定的坐标空间中。
- 确定要光源的对象：目标光线位于相关视觉对象上。
- 可有可无定义各个对象对光源的反应方式：将 SceneLightingEffect 与 EffectBrush 配合使用，以自定义用于显示 SpriteVisual 的光源反射。 反射默认值支持光源的 CoordinateSpace 的子级照明。  使用 SceneLightingEffect 绘制的视觉对象将覆盖该视觉对象的默认光照。

## <a name="scenelightingeffect"></a>SceneLightingEffect

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect)用于修改应用于[CompositionLight](/uwp/api/windows.ui.composition.compositionlight)的目标[SpriteVisual](/uwp/api/Windows.UI.Composition.SpriteVisual)内容的默认照明。

[SceneLightingEffect](/uwp/api/Windows.UI.Composition.Effects.SceneLightingEffect) 经常用于创建材料。 当您想要实现更复杂的操作时（例如，启用图像上的反射属性）和/或使用普通地图提供深度反射时，使用 SceneLightingEffect。 SceneLightingEffect 提供了使用反射和漫射量等照明属性自定义 UI 的功能。 您可以通过 "效果" 管道的其余部分进一步自定义光照效果，使您可以单独与内容混合和组合不同的照明反应。

> [!NOTE]
> 场景照明不产生阴影;这是一种侧重于2D 呈现的效果。  它不考虑包含实照明模型（包括阴影）的3D 光照方案。


属性 | 描述
--- | ---
**普通地图** | "NormalMaps" 创建纹理效果，在该纹理中，指向光源的法线将更亮，而正常的指针会变暗。 若要将 NormalMap 添加到目标视觉对象，请使用 LoadedImageSurface 的 [CompositionSurfaceBrush](/uwp/api/Windows.UI.Composition.CompositionSurfaceBrush) 来加载 NormalMap 资产。
**环境** | 环境属性主要用于控制总体颜色反射。
**反射** | 反射反射在对象上创建突出显示，使它们显得发亮。 您可以控制反射反射的级别以及闪光度。  这些属性用于创建材料效果，如 shinny 金属或光泽纸。
**漫射** | 散射反射在所有方向上 scatters 光。
**Reflectance 模型** | [Reflectance 模型](/uwp/api/windows.ui.composition.effects.scenelightingeffectreflectancemodel) 允许在 [Blinn 冯氏](/visualstudio/designers/how-to-create-a-basic-phong-shader) 和基于物理的 Blinn 冯氏之间进行选择。  当您想要进行简洁反射高光时，应选择 "基于物理的 Blinn 冯氏"。

### <a name="sample-scenelightingeffect"></a>示例 (SceneLightingEffect) 

下面的示例演示如何向 SceneLightingEffect 添加普通地图。

```cs
CompositionBrush CreateNormalMapBrush(ICompositionSurface normalMapImage)
{
    var colorSourceEffect = new ColorSourceEffect()
    {
        Color = Colors.White
    };
    var sceneLightingEffect = new SceneLightingEffect()
    {
        NormalMapSource = new CompositionEffectSourceParameter("NormalMap")
    };

    var compositeEffect = new ArithmeticCompositeEffect()
    {
        Source1 = colorSourceEffect,
        Source2 = sceneLightingEffect,
    };

    var factory = _compositor.CreateEffectFactory(sceneLightingEffect);

    var normalMapBrush = _compositor.CreateSurfaceBrush();
    normalMapBrush.Surface = normalMapImage;
    normalMapBrush.Stretch = CompositionStretch.Fill;

    var brush = factory.CreateBrush();
    brush.SetSourceParameter("NormalMap", normalMapBrush);

    return brush;
}
```

## <a name="related-articles"></a>相关文章

- [在可视层中创建材料和光源](https://blogs.windows.com/buildingapps/2017/08/04/creating-materials-lights-visual-layer/)
- [照明概述](../graphics-concepts/lighting-overview.md)
- [CompositionCapabilities API](/uwp/api/windows.ui.composition.compositioncapabilities)
- [数学数学](../graphics-concepts/mathematics-of-lighting.md)
- [SceneLightingEffect](/uwp/api/windows.ui.composition.effects.scenelightingeffect)
- [WindowsUIDevLabs GitHub 存储库](https://github.com/microsoft/WindowsCompositionSamples)