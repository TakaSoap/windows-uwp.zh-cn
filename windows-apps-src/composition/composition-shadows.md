---
title: 复合阴影
description: 影子 Api 允许向 UI 内容添加动态自定义阴影。
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 29ca04ac3faf3a2884bcc2346177f49222cf786e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166421"
---
# <a name="shadows-in-windows-ui"></a>Windows UI 中的阴影

[DropShadow](/uwp/api/Windows.UI.Composition.DropShadow)类提供了创建可配置的阴影的方法，可将其应用于视觉对象) 的[SpriteVisual](/uwp/api/windows.ui.composition.spritevisual)或[LayerVisual](/uwp/api/windows.ui.composition.layervisual) (子树。 与可视层中的对象一样，DropShadow 的所有属性都可以使用 CompositionAnimations 进行动画处理。

## <a name="basic-drop-shadow"></a>基本投影

若要创建基本阴影，只需创建一个新的 DropShadow，并将其与您的视觉对象相关联。 默认情况下，阴影为矩形。 可使用一组标准属性来调整阴影的外观。

```cs
var basicRectVisual = _compositor.CreateSpriteVisual();
basicRectVisual.Brush = _compositor.CreateColorBrush(Colors.Blue);
basicRectVisual.Offset = new Vector3(100, 100, 20);
basicRectVisual.Size = new Vector2(300, 300);

var basicShadow = _compositor.CreateDropShadow();
basicShadow.BlurRadius = 25f;
basicShadow.Offset = new Vector3(20, 20, 20);

basicRectVisual.Shadow = basicShadow;
```

![带有基本 DropShadow 的矩形视觉对象](images/rectangular-dropshadow.png)

## <a name="shaping-the-shadow"></a>定形阴影

有几种方法可以定义 DropShadow 的形状：

- **使用默认值** -默认情况下，DropShadow 形状由 CompositionDropShadowSourcePolicy 上的 "默认" 模式定义。 对于 SpriteVisual，除非提供了掩码，否则默认值为矩形。 对于 LayerVisual，默认值是使用视觉对象画笔的 alpha 继承掩码。
- **设置掩码** –可以将 [mask](/uwp/api/windows.ui.composition.dropshadow.mask) 属性设置为定义阴影的不透明蒙板。
- **指定使用继承的掩码** –将 [SourcePolicy](/uwp/api/windows.ui.composition.dropshadow.sourcepolicy) 属性设置为使用 [CompositionDropShadowSourcePolicy](/uwp/api/windows.ui.composition.compositiondropshadowsourcepolicy)。 InheritFromVisualContent 使用从视觉对象画笔的 alpha 生成的掩码。

## <a name="masking-to-match-your-content"></a>用于匹配内容的屏蔽

如果你希望你的阴影与视觉对象的内容相匹配，则可以将视觉对象的画笔用于阴影蒙版属性，或将阴影设置为自动从内容继承掩码。 如果使用的是 LayerVisual，则默认情况下，阴影将继承掩码。

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = imageBrush;
// or use shadow.SourcePolicy = CompositionDropShadowSourcePolicy.InheritFromVisualContent;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![已连接的包含遮蔽投影的 web 映像](images/ms-brand-web-dropshadow.png)

## <a name="using-an-alternative-mask"></a>使用备用掩码

在某些情况下，可能需要对阴影进行形状调整，使其与视觉对象的内容不匹配。 若要实现此效果，需要使用 alpha 画笔来显式设置掩码属性。

在下面的示例中，我们将加载两个图面：一个用于视觉内容，另一个用于阴影蒙版：

```cs
var imageSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myImage.png"));
var imageBrush = _compositor.CreateSurfaceBrush(imageSurface);

var circleSurface = LoadedImageSurface.StartLoadFromUri(new Uri("ms-appx:///Assets/myCircleImage.png"));
var customMask = _compositor.CreateSurfaceBrush(circleSurface);

var imageSpriteVisual = _compositor.CreateSpriteVisual();
imageSpriteVisual.Size = new Vector2(400,400);
imageSpriteVisual.Offset = new Vector3(100, 500, 20);
imageSpriteVisual.Brush = imageBrush;

var shadow = _compositor.CreateDropShadow();
shadow.Mask = customMask;
shadow.BlurRadius = 25f;
shadow.Offset = new Vector3(20, 20, 20);

imageSpriteVisual.Shadow = shadow;
```

![已连接的 web 图像（带圆形掩码投影）](images/ms-brand-web-masked-dropshadow.png)

## <a name="animating"></a>制作

与可视层中的标准一样，DropShadow 属性可使用撰写动画进行动画处理。 下面，我们修改上面的 sprinkles 示例中的代码，对阴影的模糊半径进行动画处理。

```cs
ScalarKeyFrameAnimation blurAnimation = _compositor.CreateScalarKeyFrameAnimation();
blurAnimation.InsertKeyFrame(0.0f, 25.0f);
blurAnimation.InsertKeyFrame(0.7f, 50.0f);
blurAnimation.InsertKeyFrame(1.0f, 25.0f);
blurAnimation.Duration = TimeSpan.FromSeconds(4);
blurAnimation.IterationBehavior = AnimationIterationBehavior.Forever;
shadow.StartAnimation("BlurRadius", blurAnimation);
```

## <a name="shadows-in-xaml"></a>XAML 中的阴影

如果要将影子添加到更复杂的框架元素，可通过几种方法在 XAML 和组合之间使用阴影：

1. 使用 Windows 社区工具包中的 [DropShadowPanel](https://github.com/windows-toolkit/WindowsCommunityToolkit/blob/master/Microsoft.Toolkit.Uwp.UI.Controls/DropShadowPanel/DropShadowPanel.Properties.cs) 。 有关如何使用的详细信息，请参阅 [DropShadowPanel 文档](/windows/uwpcommunitytoolkit/controls/DropShadowPanel) 。
1. 创建一个视觉对象，将其用作影子宿主 & 将其与 XAML 讲义视觉对象关联。
1. 使用撰写示例库的 [SamplesCommon](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SamplesCommon/SamplesCommon) 自定义 CompositionShadow 控件。 有关用法，请参阅此处的示例。

## <a name="performance"></a>性能

尽管视觉对象层具有很多优化功能，以使效果高效且可供使用，但根据您设置的选项，生成阴影可能是一个相对较昂贵的操作。 下面是不同类型的阴影的高级别 "成本"。 请注意，尽管某些阴影的成本可能很高，但在某些情况下，它们可能仍适合使用。

阴影特征| Cost
------------- | -------------
“矩形”    | 低
影子掩码      | 高
CompositionDropShadowSourcePolicy.InheritFromVisualContent | 高
静态模糊半径 | 低
对模糊半径进行动画处理 | 高

## <a name="additional-resources"></a>其他资源

- [组合 DropShadow API](/uwp/api/Windows.UI.Composition.DropShadow)
- [WindowsUIDevLabs GitHub 存储库](https://github.com/microsoft/WindowsCompositionSamples)