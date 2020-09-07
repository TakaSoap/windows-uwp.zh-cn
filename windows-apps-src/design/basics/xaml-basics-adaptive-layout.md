---
title: “创建自适应布局”教程
description: 了解如何在 XAML 中使用自适应布局功能创建在任何窗口大小都美观的应用。
keywords: XAML, UWP, 入门
ms.date: 08/20/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8e1498836772c3c279a1b9d85d76070b29593f5e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174471"
---
# <a name="tutorial-create-adaptive-layouts"></a>教程：创建自适应布局

本教程介绍关于使用 XAML 的自适应布局功能的基础知识，利用这些功能，可以创建以任何大小显示均具有良好外观的应用。 你将了解如何添加窗口断点、创建新的 DataTemplate 以及使用 VisualStateManager 类定制应用的布局。 你将使用这些工具针对较小的窗口大小优化图像编辑程序。

图像编辑程序有两个页面。 主页显示照片库视图，以及关于每个图像文件的一些信息。

![MainPage](../basics/images/xaml-basics/mainpage.png)

详细信息页  显示选定的单张照片。 利用浮出编辑菜单，可以修改、重命名和保存照片。

![DetailPage](../basics/images/xaml-basics/detailpage.png)

## <a name="prerequisites"></a>必备条件

+ Visual Studio 2019：[下载 Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)（社区版本是免费的。）
+ Windows 10 SDK（10.0.17763.0 或更高版本）：[下载最新的 Windows SDK（免费）](https://developer.microsoft.com/windows/downloads/windows-10-sdk)
+ Windows 10 版本 1809 或更高版本

## <a name="part-0-get-the-starter-code-from-github"></a>第 0 部分：从 github 获取起始代码

在本教程中，我们将从一个简化版的 PhotoLab 示例开始。

1. 转到 GitHub 页查看示例：[https://github.com/Microsoft/Windows-appsample-photo-lab](https://github.com/Microsoft/Windows-appsample-photo-lab)。
2. 接下来，需要克隆或下载示例。 单击“克隆或下载”  按钮。 一个子菜单将出现。
    ![PhotoLab 示例在 GitHub 页上的“克隆或下载”菜单](images/xaml-basics/clone-repo.png)

    **如果你不熟悉 GitHub:**

    a. 单击“下载 ZIP”  并在本地保存文件。 这将下载一个包含你需要的所有项目文件的 .zip 文件。

    b. 将该文件解压缩。 使用文件资源管理器浏览到刚下载的 .zip 文件，右键单击它，然后选择“全部解压缩…”。

    c. 浏览到示例的本地副本，并转到 `Windows-appsample-photo-lab-master\xaml-basics-starting-points\adaptive-layout` 目录。

    **如果你熟悉 GitHub：**

    a. 在本地克隆 repo 的主分支。

    b. 浏览到 `Windows-appsample-photo-lab\xaml-basics-starting-points\adaptive-layout` 目录。

3. 在 Visual Studio 中双击 `Photolab.sln` 打开解决方案。

## <a name="part-1-define-window-breakpoints"></a>第 1 部分：定义窗口断点

运行应用。 它很适合全屏显示，但是当你把窗口缩小得太小时，用户界面 (UI) 就不理想了。 通过使用 [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) 类使 UI 适应不同窗口大小，你可以确保最终用户体验始终感觉良好。

![小窗口：之前](../basics/images/xaml-basics/adaptive-layout-small-before.png)

有关应用布局的更多信息，请参阅文档的[布局](../layout/index.md)部分。

### <a name="add-window-breakpoints"></a>添加窗口断点

第一步是定义应用不同视觉状态的“断点”。 请参阅[屏幕大小和断点](../layout/screen-sizes-and-breakpoints-for-responsive-design.md)，了解有关小、中和大屏幕断点的详细信息。

从解决方案资源管理器中打开 App.xaml，并在结束 `</ResourceDictionary>` 标记之前的 `MergedDictionaries` 后面添加以下代码。

```xaml
    <!--  Window width adaptive breakpoints.  -->
    <x:Double x:Key="MinWindowBreakpoint">0</x:Double>
    <x:Double x:Key="MediumWindowBreakpoint">641</x:Double>
    <x:Double x:Key="LargeWindowBreakpoint">1008</x:Double>
```

这将创建 3 个断点，使你能够为 3 个窗口大小范围创建新的视觉状态：

+ 小（宽度为 0 - 640 像素）
+ 中等（宽度为 641 - 1007 像素）
+ 大（宽度大于 1007 像素）

在本例中，只为小窗口大小创建新外观。 中窗口和大窗口大小使用相同的外观。

## <a name="part-2-add-a-data-template-for-small-window-sizes"></a>第 2 部分：为小窗口大小添加数据模板

若要使此应用即使在小窗口中也外观良好，你可以创建一个新的数据模板，以优化用户缩小窗口时图像库视图中图像的显示方式。

### <a name="create-a-new-datatemplate"></a>创建新的 DataTemplate

 从解决方案资源管理器中打开 MainPage.xaml，并在 `Page.Resources` 标记内添加以下代码。

```xaml
<DataTemplate x:Key="ImageGridView_SmallItemTemplate"
              x:DataType="local:ImageFileInfo">

    <!-- Create image grid -->
    <Grid Height="{Binding ItemSize, ElementName=page}"
          Width="{Binding ItemSize, ElementName=page}">

        <!-- Place image in grid, stretching it to fill the pane-->
        <Image x:Name="ItemImage"
               Source="{x:Bind ImageSource, Mode=OneWay}"
               Stretch="UniformToFill">
        </Image>

    </Grid>
</DataTemplate>
```

此库模板通过消除图像周围的边框、除去每个缩略图下方的图像元数据（文件名、分级等），从而节省屏幕空间。 每个缩略图改为以简单的正方形显示。

### <a name="add-metadata-to-a-tooltip"></a>将元数据添加到工具提示

你仍然希望用户能够访问每个图像的元数据，因此将为每个图像项目添加一个工具提示。 在刚刚创建的 DataTemplate 的 `Image` 标记中添加以下代码。

```xaml
    <!-- Add a tooltip to the image that displays metadata -->
    <ToolTipService.ToolTip>
        <ToolTip x:Name="tooltip">

            <!-- Arrange tooltip elements vertically -->
            <StackPanel Orientation="Vertical"
                        Grid.Row="1">

                <!-- Image title -->
                <TextBlock Text="{x:Bind ImageTitle, Mode=OneWay}"
                           HorizontalAlignment="Center"
                           Style="{StaticResource SubtitleTextBlockStyle}" />

                <!-- Arrange elements horizontally -->
                <StackPanel Orientation="Horizontal"
                            HorizontalAlignment="Center">

                    <!-- Image file type -->
                    <TextBlock Text="{x:Bind ImageFileType}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}" />

                    <!-- Image dimensions -->
                    <TextBlock Text="{x:Bind ImageDimensions}"
                               HorizontalAlignment="Center"
                               Style="{StaticResource CaptionTextBlockStyle}"
                               Margin="8,0,0,0" />
                </StackPanel>
            </StackPanel>
        </ToolTip>
    </ToolTipService.ToolTip>
```

将鼠标悬停在缩略图上（或者长按触摸屏）时，会显示图像的标题、文件类型和尺寸。

## <a name="part-3-define-visual-states"></a>第 3 部分：定义视觉状态

你现在已经为你的数据创建了新布局，但该应用目前无法知道何时使用此布局而非默认样式。 若要解决此问题，需要添加 [VisualStateManager](/uwp/api/windows.ui.xaml.visualstatemanager) 和 [VisualState](/uwp/api/windows.ui.xaml.visualstate) 定义。

### <a name="add-a-visualstatemanager"></a>添加 VisualStateManager

将以下代码添加到 `RelativePanel` 页面根元素中。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

### <a name="create-statetriggers-to-apply-the-visual-state"></a>创建 StateTriggers 以应用视觉状态

接下来，创建对应于每个吸附点的 `StateTriggers`。 在 MainPage.xaml 中，将以下代码添加到在第 2 部分创建的 `VisualStateManager` 中。

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
    ...

        <!-- Large window VisualState -->
        <VisualState x:Key="LargeWindow">

            <!-- Large window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource LargeWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Medium window VisualState -->
        <VisualState x:Key="MediumWindow">

            <!-- Medium window trigger -->
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="{StaticResource MediumWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

        <!-- Small window VisualState -->
        <VisualState x:Key="SmallWindow">

            <!-- Small window trigger -->
            <VisualState.StateTriggers >
                <AdaptiveTrigger MinWindowWidth="{StaticResource MinWindowSnapPoint}"/>
            </VisualState.StateTriggers>

        </VisualState>

    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

当每个视觉状态触发时，该应用将使用分配到活动 `VisualState` 的任何布局属性。

### <a name="set-properties-for-each-visual-state"></a>为每个视觉状态设置属性

最后，为每个视觉状态设置属性，以告诉 `VisualStateManager` 在触发状态时要应用哪些属性。 每个资源库都以特定 XAML 元素的一个属性为目标，并将其设置为给定值。 在 `StateTriggers` 之后，将此代码添加到刚刚创建的 `SmallWindow` 视觉状态中。

```xaml
    <!-- Small window setters -->
    <VisualState.Setters>

        <!-- Apply small template and styles -->
        <Setter Target="ImageGridView.ItemTemplate"
                Value="{StaticResource ImageGridView_SmallItemTemplate}" />
        <Setter Target="ImageGridView.ItemContainerStyle"
                Value="{StaticResource ImageGridView_SmallItemContainerStyle}" />

        <!-- Adjust the zoom slider to fit small windows-->
        <Setter Target="ZoomSlider.Minimum"
                Value="80" />
        <Setter Target="ZoomSlider.Maximum"
                Value="180" />
        <Setter Target="ZoomSlider.TickFrequency"
                Value="20" />
        <Setter Target="ZoomSlider.Value"
                Value="100" />
    </VisualState.Setters>
```

这些资源库将图像库的 `ItemTemplate` 设置为在上一部分中创建的新的 `DataTemplate`。 它们还会调整缩放滑块以更好地适应小屏幕。

### <a name="run-the-app"></a>运行应用

运行应用。 加载该应用时，请尝试更改窗口的大小。 将窗口缩小为小尺寸时，你应该会看到应用切换为你在第 2 部分中创建的小布局。

![小窗口：之后](../basics/images/xaml-basics/adaptive-layout-small-after.png)

## <a name="going-further"></a>深入探索

现在，你已完成本实验，并且拥有足够的自适应布局知识，可以自行进行进一步的实验。 作为一项更大的挑战，请尝试针对较大的屏幕大小优化布局（例如 Surface Hub）。 若要测试 Surface Hub 布局，请参阅[使用 Visual Studio 测试 Surface Hub 应用](../../debug-test-perf/test-surface-hub-apps-using-visual-studio.md)。

如果遇到问题，可以在[使用 XAML 定义页面布局](../layout/layouts-with-xaml.md)的以下部分中找到更多指南。

+ [视觉状态和状态触发器](../layout/layouts-with-xaml.md#visual-states-and-state-triggers)
+ [定制布局](../layout/layouts-with-xaml.md#tailored-layouts)

或者，如果你想要了解有关如何构建初始照片编辑应用的详细信息，请查看这些有关 XAML [用户界面](../basics/xaml-basics-ui.md)和[数据绑定](../../data-binding/xaml-basics-data-binding.md)的教程。

## <a name="get-the-final-version-of-the-photolab-sample"></a>获取 PhotoLab 示例的最终版本

本教程并不生成完整的照片编辑应用，因此，请务必查看[最终版本](https://github.com/Microsoft/Windows-appsample-photo-lab)以了解诸如自定义动画和样式等其他功能。