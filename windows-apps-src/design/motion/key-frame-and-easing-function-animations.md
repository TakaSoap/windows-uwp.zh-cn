---
title: 关键帧动画以及缓动函数动画
ms.assetid: D8AF24CD-F4C2-4562-AFD7-25010955D677
description: 线性关键帧动画、具有 KeySpline 值的关键帧动画或缓动函数对于大致相同的情况是三种不同的技术。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e68c667fe1e6d3ef61e60095e7fed02400044d07
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172421"
---
# <a name="key-frame-animations-and-easing-function-animations"></a>关键帧动画以及缓动函数动画



线性关键帧动画、具有 **KeySpline** 值的关键帧动画或缓动函数对于大致相同的情况是三种不同的技术：创建从某个起始状态到结束状态使用非线性动画行为的更为复杂的情节提要动画。

## <a name="prerequisites"></a>必备条件

确保你已经阅读[情节提要动画](storyboarded-animations.md)主题。 本主题在[情节提要动画](storyboarded-animations.md)中解释过的动画概念的基础上生成，并且不会重新复习这些概念。 例如，[情节提要动画](storyboarded-animations.md)介绍了如何定位动画、作为资源的情节提要、[**Timeline**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) 属性值（例如 [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration)、[**FillBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior)）等。

## <a name="animating-using-key-frame-animations"></a>使用关键帧动画创建动画

关键帧动画允许沿动画时间线到达一个点的多个目标值。 换句话说，每个关键帧可以指定一个不同的中间值，并且到达的最后一个关键帧为最终动画值。 通过指定多个值来创建动画，你可以做出更复杂的动画。 关键帧动画还会启用不同的内插逻辑，每个内插逻辑根据动画类型作为不同的 **KeyFrame** 子类实现。 确切地说，每个关键帧动画类型具有其 **KeyFrame** 类的 **Discrete**、**Linear**、**Spline** 和 **Easing** 变体，用于指定其关键帧。 例如，若要指定以 [**Double**](/dotnet/api/system.double) 为目标并使用关键帧的动画，可声明具有 [**DiscreteDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteDoubleKeyFrame)、[**LinearDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.LinearDoubleKeyFrame)、[**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame) 和 [**EasingDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.EasingDoubleKeyFrame) 的关键帧。 你可以在一个 **KeyFrames** 集合中使用任一和所有这些类型，以更改每次新关键帧到达时的内插。

对于内插行为，每个关键帧控制该内插，直至到达其 **KeyTime** 时间。 其 **Value** 也会在该时间到达。 如果有更多关键帧超出范围，则该值将成为序列中下一个关键帧的起始值。

在动画开始时，如果不存在 **KeyTime** 为“0:0:0”的关键帧，则起始值为该属性的任意非动画值。 这类似于中的**from** / **To** / 动画**From****By**的工作方式。

关键帧动画的持续时间为隐式持续时间，它等于其任一关键帧中设置的最高 **KeyTime** 值。 如果需要，你可以设置一个显式 [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration)，但应注意该值不应小于你自己的关键帧中的 **KeyTime**，否则将会截断部分动画。

除了 [**Duration**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration)，你还可以在关键帧动画上设置所有基于 [**Timeline**](/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) 的属性，例如，你可以设置具有 **From**/**To**/**By** 的动画，因为关键帧动画类也派生自 **Timeline**。 它们是：

-   [**AutoReverse**](/uwp/api/windows.ui.xaml.media.animation.timeline.autoreverse)：在到达最后一个关键帧后，从结束位置开始反向重复帧。 这使得动画的显示持续时间加倍。
-   [**BeginTime**](/uwp/api/windows.ui.xaml.media.animation.timeline.begintime)：延迟动画的起始部分。 帧内 **KeyTime** 值的时间线在 **BeginTime** 到达前不开始计数，因此不存在截断帧的风险
-   [**FillBehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.fillbehavior)：控制当到达最后一帧时发生的操作。 **FillBehavior** 不会对任何中间关键帧产生任何影响。
-   [**System.windows.media.animation.timeline.repeatbehavior**](/uwp/api/windows.ui.xaml.media.animation.timeline.repeatbehaviorproperty)：
    -   如果设置为 **Forever**，则关键帧及其时间线将一直重复。
    -   如果设置为一个迭代计数，则时间线将重复该计数多次。
    -   如果设置为 [**Duration**](/uwp/api/Windows.UI.Xaml.Duration)，则时间线在到达该时间前一直重复。 如果该数不是时间线的隐式持续时间的整数倍数，则这可能会截断关键帧序列中的部分动画。
-   [**SpeedRatio**](/uwp/api/windows.ui.xaml.media.animation.timeline.speedratioproperty)（不常使用）

### <a name="linear-key-frames"></a>线性关键帧

线性关键帧在帧的 **KeyTime** 达到前，产生该值的简单线性内插。 这种内插行为最**类似于** / **To** / **By** [Storyboarded 动画](storyboarded-animations.md)主题中所述动画的更简单方式。

下面介绍了如何使用关键帧动画来通过线性关键帧缩放矩形的呈现高度。 此示例运行的动画如下：其中矩形的高度在前 4 秒内缓慢地以线性方式增长，然后在最后一秒迅速增长，直至矩形的高度增长至初始高度的两倍。

```xml
<StackPanel>
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimationUsingKeyFrames
              Storyboard.TargetName="myRectangle"
              Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <LinearDoubleKeyFrame Value="1" KeyTime="0:0:0"/>
                <LinearDoubleKeyFrame Value="1.2" KeyTime="0:0:4"/>
                <LinearDoubleKeyFrame Value="2" KeyTime="0:0:5"/>
            </DoubleAnimationUsingKeyFrames>
        </Storyboard>
    </StackPanel.Resources>
</StackPanel>
```

### <a name="discrete-key-frames"></a>离散式关键帧

离散式关键帧根本不使用任何内插。 在 **KeyTime** 到达后，只是简单地应用新的 **Value**。 根据要创建动画的 UI 属性，这种方式常常会产生动画仿佛在“跳”的感觉。 请确保这正是你确实需要的艺术行为。 你可以通过增加声明的关键帧数目来最大程度地减少明显的跳跃感，但如果你需要流畅的动画效果，最好是改为使用线性或样条关键帧。

> [!NOTE]
> 离散式关键帧是为其类型不是 [**Double**](/dotnet/api/system.double)、[**Point**](/uwp/api/Windows.Foundation.Point) 和 [**Color**](/uwp/api/Windows.UI.Color) 的值（具有 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame)）创建动画的唯一方法。 我们将在本主题的后面部分更详细地讨论此方法。

### <a name="spline-key-frames"></a>样条关键帧

样条关键帧根据 **KeySpline** 属性的值在值之间创建可变的过渡。 此属性指定贝塞尔曲线的第一个控制点和第二个控制点，可描述动画的加速。 基本上，[**KeySpline**](/uwp/api/Windows.UI.Xaml.Media.Animation.KeySpline) 定义了一个时间函数关系，其中函数-时间图形采用贝塞尔曲线的形状。 你通常在 XAML 速记属性字符串中指定一个 **KeySpline** 值，该字符串具有四个以空格或逗号分隔的 [**Double**](/dotnet/api/system.double) 值。 这些值是用作贝塞尔曲线的两个控制点的“X,Y”对。 “X”是时间，而“Y”是对值的函数修饰符。 每个值应始终介于 0 到 1 之间（包含这两个值）。 如果不将控制点修改为 **KeySpline**，则从 0,0 到 1,1 的直线是线性内插的时间函数的表示形式。 控制点更改该曲线的形状，并因此更改样条动画的时间函数的行为。 最好是在图形上以可视化方式查看此方法。 你可以在浏览器中运行 [Silverlight 主曲线可视化工具示例](https://samples.msdn.microsoft.com/Silverlight/SampleBrowser/index.htm#/?sref=KeySplineExample)以查看控制点如何修改曲线以及示例动画在将其用作 **KeySpline** 值时如何运行。

此下一示例展示应用于一个动画的三个不同关键帧，其最后一帧为 [**Double**](/dotnet/api/system.double) 值的主曲线动画 ([**SplineDoubleKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.SplineDoubleKeyFrame))。 请注意应用于 **KeySpline** 的字符串“0.6,0.0 0.9,0.00”。 这会生成一条曲线，其中动画在开始时缓慢运行，但随后在刚刚到达 **KeyTime** 前快速到达该值。

```xml
<Storyboard x:Name="myStoryboard">
    <!-- Animate the TranslateTransform's X property
        from 0 to 350, then 50,
        then 200 over 10 seconds. -->
    <DoubleAnimationUsingKeyFrames
        Storyboard.TargetName="MyAnimatedTranslateTransform"
        Storyboard.TargetProperty="X"
        Duration="0:0:10" EnableDependentAnimation="True">

        <!-- Using a LinearDoubleKeyFrame, the rectangle moves 
            steadily from its starting position to 500 over 
            the first 3 seconds.  -->
        <LinearDoubleKeyFrame Value="500" KeyTime="0:0:3"/>

        <!-- Using a DiscreteDoubleKeyFrame, the rectangle suddenly 
            appears at 400 after the fourth second of the animation. -->
        <DiscreteDoubleKeyFrame Value="400" KeyTime="0:0:4"/>

        <!-- Using a SplineDoubleKeyFrame, the rectangle moves 
            back to its starting point. The
            animation starts out slowly at first and then speeds up. 
            This KeyFrame ends after the 6th second. -->
        <SplineDoubleKeyFrame KeySpline="0.6,0.0 0.9,0.00" Value="0" KeyTime="0:0:6"/>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

### <a name="easing-key-frames"></a>缓动关键帧

缓动关键帧是这样一种关键帧：其中应用了插入，并且插入的随时间变化的函数由多个预定义数学公式控制。 实际上，您可以使用样条关键帧产生的结果与一些缓动函数类型所产生的结果相同，但也有一些缓动函数（如 [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase)）不能用样条再现。

若要将缓动函数应用到缓动关键帧，则应在该关键帧的 XAML 中将 **EasingFunction** 属性设置为属性元素。 对于值，指定缓动函数类型之一的对象元素。

此示例应用了 [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase)，然后将 [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) 作为连续关键帧应用于 [**DoubleAnimation**](/uwp/api/Windows.UI.Xaml.Media.Animation.DoubleAnimation) 以创建一种回弹效果。

```xml
<Storyboard x:Name="myStoryboard">
    <DoubleAnimationUsingKeyFrames Duration="0:0:10"
        Storyboard.TargetProperty="Height"
        Storyboard.TargetName="myEllipse">

        <!-- This keyframe animates the ellipse up to the crest 
            where it slows down and stops. -->
        <EasingDoubleKeyFrame Value="-300" KeyTime="00:00:02">
            <EasingDoubleKeyFrame.EasingFunction>
                <CubicEase/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>

        <!-- This keyframe animates the ellipse back down and makes
            it bounce. -->
        <EasingDoubleKeyFrame Value="0" KeyTime="00:00:06">
            <EasingDoubleKeyFrame.EasingFunction>
                <BounceEase Bounces="5"/>
            </EasingDoubleKeyFrame.EasingFunction>
        </EasingDoubleKeyFrame>
    </DoubleAnimationUsingKeyFrames>
</Storyboard>
```

这仅仅是一个缓动函数示例。 我们将在下一节对此进行更多讨论。

## <a name="easing-functions"></a>缓动函数

缓动函数允许将自定义的数学公式应用到动画。 数学运算通常对于制作在 2-D 坐标系中模拟真实物理效果的动画非常有用。 例如，用户可能希望某个对象逼真地弹跳或表现出像在弹簧上一样。 您可以使用关键**帧甚至** / **To** / **通过**动画来接近这些效果，但这会占用大量的工作，而且动画比使用数学公式要精确得多。

缓动函数可以以三种方式应用于动画：

-   按照上一节中的说明，通过在关键帧动画中使用缓动关键帧。 使用 [**EasingColorKeyFrame.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingcolorkeyframe.easingfunction)、[**EasingDoubleKeyFrame.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingdoublekeyframe.easingfunction) 或 [**EasingPointKeyFrame.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.easingpointkeyframe.easingfunction)。
-   通过在 **From**/**To**/**By** 动画类型之一上设置 **EasingFunction** 属性。 使用 [**ColorAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.coloranimation.easingfunction)、[**DoubleAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.doubleanimation.easingfunction) 或 [**PointAnimation.EasingFunction**](/uwp/api/windows.ui.xaml.media.animation.pointanimation.easingfunction)。
-   通过将 [**GeneratedEasingFunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) 设置为 [**VisualTransition**](/uwp/api/Windows.UI.Xaml.VisualTransition) 的一部分。 这种方式专用于定义控件的视觉状态；对于详细信息，请参阅 [**GeneratedEasingFunction**](/uwp/api/windows.ui.xaml.visualtransition.generatedeasingfunction) 或[视觉状态的情节提要](/previous-versions/windows/apps/jj819808(v=win.10))。

下面是缓动函数的列表：

-   [**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase)：动画开始在指定路径上运动前稍微收缩动画的运行。
-   [**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase)：创建回弹效果。
-   [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase)：使用圆函数创建加速或减速的动画。
-   [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase)：使用函数 f(t) = t3 创建加速或减速的动画。
-   [**ElasticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ElasticEase)：创建一个动画，模拟弹簧的来回振荡运动，直到它达到停止状态。
-   [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase)：使用指数公式创建加速或减速的动画。
-   [**PowerEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase)：使用公式 f(t) = tp 创建加速或减速的动画，其中 p 等于 [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) 属性。
-   [**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase)：使用函数 f(t) = t2 创建加速或减速的动画。
-   [**QuarticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuarticEase)：使用函数 f(t) = t4 创建加速或减速的动画。
-   [**QuinticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuinticEase)：使用函数 f(t) = t5 创建加速或减速的动画。
-   [**SineEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.SineEase)：使用正弦公式创建加速或减速的动画。

某些缓动函数具有其自己的属性。 例如，[**BounceEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BounceEase) 具有两个属性（[**Bounces**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounces) 和 [**Bounciness**](/uwp/api/windows.ui.xaml.media.animation.bounceease.bounciness)），用于修改该特定 **BounceEase** 的时间函数行为。 其他缓动函数（例如 [**CubicEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CubicEase)）不具有除所有缓动函数共享的 [**EasingMode**](/uwp/api/windows.ui.xaml.media.animation.easingfunctionbase.easingmode) 属性之外的任何属性，并且始终产生相同的时间函数行为。

根据你在具有多个属性的缓动函数上设置的属性，这些缓动函数中的某些函数会部分重叠。 例如，[**QuadraticEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.QuadraticEase) 与其 [**Power**](/uwp/api/windows.ui.xaml.media.animation.powerease.power) 等于 2 的 [**PowerEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.PowerEase) 完全相同。 并且，基本上 [**CircleEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.CircleEase) 就是具有默认值的 [**ExponentialEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.ExponentialEase)。

[**BackEase**](/uwp/api/Windows.UI.Xaml.Media.Animation.BackEase) 缓动函数是唯一的，因为它可以更改正常范围之外的值（在由 **From**/**To** 设置时）或关键帧的值。 它通过以从正常到行为的预期方向更改**值来启动**动画 / **To** ，并再次返回到**from**或起始值，然后以正常方式运行动画。

在前面的示例中，我们展示了如何为关键帧动画声明缓动函数。 下一个示例**From** / **To** / **通过**动画将缓动函数应用到。

```xml
<StackPanel x:Name="LayoutRoot" Background="White">
    <StackPanel.Resources>
        <Storyboard x:Name="myStoryboard">
            <DoubleAnimation From="30" To="200" Duration="00:00:3" 
                Storyboard.TargetName="myRectangle" 
                Storyboard.TargetProperty="(UIElement.RenderTransform).(ScaleTransform.ScaleY)">
                <DoubleAnimation.EasingFunction>
                    <BounceEase Bounces="2" EasingMode="EaseOut" 
                                Bounciness="2"/>
                </DoubleAnimation.EasingFunction>
            </DoubleAnimation>
        </Storyboard>
    </StackPanel.Resources>
    <Rectangle x:Name="myRectangle" Fill="Blue" Width="200" Height="30"/>
</StackPanel>
```

当缓动函数由动画应用于**From** / **时** / **By** ，它将更改在动画[**持续时间**](/uwp/api/windows.ui.xaml.media.animation.timeline.duration)内值在**from**和**to**值之间的外插值的函数优先特性。 如果没有缓动函数，则该内插将为线性内插。

## <a name="span-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspanspan-iddiscrete_object_value_animationsspandiscrete-object-value-animations"></a><span id="Discrete_object_value_animations"></span><span id="discrete_object_value_animations"></span><span id="DISCRETE_OBJECT_VALUE_ANIMATIONS"></span>离散对象值动画

有一种类型的动画值得特别提出，因为它是可以将动画化的值应用于其类型不是 [**Double**](/dotnet/api/system.double)、[**Point**](/uwp/api/Windows.Foundation.Point) 或 [**Color**](/uwp/api/Windows.UI.Color) 的属性的唯一方法。 它就是关键帧动画 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames)。 使用 [**Object**](/dotnet/api/system.object) 值的动画非常不同，因为不可能在帧之间内插值。 当帧的 [**KeyTime**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.keytime) 到达时，动画化的值将立即设置为关键帧的 **Value** 中指定的值。 由于没有任何内插，因此只有一种关键帧用于 **ObjectAnimationUsingKeyFrames** 关键帧集合：[**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame)。

[**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) 的 [**Value**](/uwp/api/windows.ui.xaml.media.animation.objectkeyframe.value) 通常使用属性元素语法设置，因为你尝试设置的对象值通常不可表示为字符串以采用属性语法填充 **Value**。 如果你使用引用，例如 [StaticResource](../../xaml-platform/staticresource-markup-extension.md)，则仍可以使用属性语法。

你将发现默认模板中使用的 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 的一个情况是在模板属性引用 [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) 资源时。 这些资源是 [**SolidColorBrush**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush) 对象，而不仅仅是 [**Color**](/uwp/api/Windows.UI.Color) 值，并且它们使用定义为系统主题 ([**ThemeDictionaries**](/uwp/api/windows.ui.xaml.resourcedictionary.themedictionaries)) 的资源。 可以将它们直接分配给 **Brush** 类型的值，例如 [**TextBlock.Foreground**](/uwp/api/windows.ui.xaml.controls.textblock.foreground)，并且不需要使用间接目标。 但由于 **SolidColorBrush** 不是 [**Double**](/dotnet/api/system.double)、[**Point**](/uwp/api/Windows.Foundation.Point) 或 **Color**，因此你必须使用 **ObjectAnimationUsingKeyFrames** 才能使用该资源。

```xml
<Style x:Key="TextButtonStyle" TargetType="Button">
    <Setter Property="Template">
        <Setter.Value>
            <ControlTemplate TargetType="Button">
                <Grid Background="Transparent">
                    <TextBlock x:Name="Text"
                        Text="{TemplateBinding Content}"/>
                    <VisualStateManager.VisualStateGroups>
                        <VisualStateGroup x:Name="CommonStates">
                            <VisualState x:Name="Normal"/>
                            <VisualState x:Name="PointerOver">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPointerOverForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
                            <VisualState x:Name="Pressed">
                                <Storyboard>
                                    <ObjectAnimationUsingKeyFrames Storyboard.TargetName="Text" Storyboard.TargetProperty="Foreground">
                                        <DiscreteObjectKeyFrame KeyTime="0" Value="{StaticResource ApplicationPressedForegroundThemeBrush}"/>
                                    </ObjectAnimationUsingKeyFrames>
                                </Storyboard>
                            </VisualState>
...
                       </VisualStateGroup>
                    </VisualStateManager.VisualStateGroups>
                </Grid>
            </ControlTemplate>
        </Setter.Value>
    </Setter>
</Style>
```

你还可以使用 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 设置使用枚举值的属性的动画。 下面是来自 Windows 运行时默认模板的命名样式中的另一个示例。 请留意它如何设置获取 [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility) 枚举常量的 [**Visibility**](/uwp/api/windows.ui.xaml.uielement.visibility) 属性。 在这种情况下，你可以使用属性语法设置该值。 你仅需要枚举中的非限定常量名称，用以使用枚举值设置属性，例如“Collapsed”。

```xml
<Style x:Key="BackButtonStyle" TargetType="Button">
    <Setter Property="Template">
      <Setter.Value>
        <ControlTemplate TargetType="Button">
          <Grid x:Name="RootGrid">
            <VisualStateManager.VisualStateGroups>
              <VisualStateGroup x:Name="CommonStates">
              <VisualState x:Name="Normal"/>
...           <VisualState x:Name="Disabled">
                <Storyboard>
                  <ObjectAnimationUsingKeyFrames Storyboard.TargetName="RootGrid" Storyboard.TargetProperty="Visibility">
                    <DiscreteObjectKeyFrame Value="Collapsed" KeyTime="0"/>
                  </ObjectAnimationUsingKeyFrames>
                </Storyboard>
              </VisualState>
            </VisualStateGroup>
...
          </VisualStateManager.VisualStateGroups>
        </Grid>
      </ControlTemplate>
    </Setter.Value>
  </Setter>
</Style>
```

你可以将多个 [**DiscreteObjectKeyFrame**](/uwp/api/Windows.UI.Xaml.Media.Animation.DiscreteObjectKeyFrame) 用于一个 [**ObjectAnimationUsingKeyFrames**](/uwp/api/Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames) 帧集。 作为其中多个对象值可能有用的示例方案，这是通过创建 [**Image.Source**](/uwp/api/windows.ui.xaml.controls.image.source) 的值的动画来创建“幻灯片放映”动画的一种有趣方法。

 ## <a name="related-topics"></a>相关主题

* [Property-path 语法](../../xaml-platform/property-path-syntax.md)
* [依赖属性概述](../../xaml-platform/dependency-properties-overview.md)
* [**情节提要**](/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
* [**Storyboard.TargetProperty**](/uwp/api/windows.ui.xaml.media.animation.storyboard.targetpropertyproperty)