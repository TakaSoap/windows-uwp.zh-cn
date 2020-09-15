---
description: 了解如何使用单选按钮让用户能够从包含两个或更多互斥但相关的选项的集合中选择一个选项。
title: 单选按钮指南
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4953b5adc1953bac83b90271b4042e3b9f13c3f2
ms.sourcegitcommit: 083ddf840ab42bb48b4892fc2876ecbf698e481b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/09/2020
ms.locfileid: "89615530"
---
# <a name="radio-buttons"></a>单选按钮

单选按钮（也称为选项按钮）允许用户从包含两个或更多互斥但相关的选项的集合中选择一个选项。 单选按钮始终成组使用，每个选项都表示为组中的一个单选按钮。

默认状态下，不会选择 RadioButtons 组中的单选按钮。 也就是说，所有单选按钮都处于清除状态。 但是，用户选择单选按钮后，就不能取消选择该按钮以将组还原到其初始清除状态。

RadioButtons 组的单一行为将其与[复选框](checkbox.md)区分开来，后者支持多选、取消选择或清除。

:::image type="content" source="images/controls/radio-button.png" alt-text="RadioButtons 组的示例，其中选择了一个单选按钮":::

**获取 Windows UI 库**

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI 徽标](images/winui-logo-64x64.png) | “RadioButtons”控件作为 Windows UI 库的一部分提供，该库是一个 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](/uwp/toolkits/winui/)。 |

> **Windows UI 库 API**：[RadioButtons 类](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)、[SelectedItem 属性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)、[SelectedIndex 属性](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)、[SelectionChanged 事件](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **平台 API**：[RadioButton 类](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)、[IsChecked 属性](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)、[Checked 事件](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **RadioButtons 与 RadioButton**
>
>可以通过两种方法创建单选按钮组。
>
>- 从 WinUI 2.3 开始，我们建议使用 [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 控件。 此控件可简化布局、处理键盘导航和辅助功能，并支持绑定到数据源。
>- 你可以使用由单独的 [RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控件组成的组。 如果你的应用未使用 WinUI 2.3 或更高版本，这是唯一的选择。

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用单选按钮可以让用户从两个或更多互斥的选项中进行选择。

:::image type="content" source="images/radiobutton_basic.png" alt-text="RadioButtons 组，其中选择了一个单选按钮":::

当用户需要在做出选择前查看所有选项时，可使用单选按钮。 单选按钮平等地强调所有选项，这意味着有些选项可能会引起超出必要或所需的关注。

除非所有选项都值得平等关注，否则请考虑使用其他控件。 例如，若要在大多数情况下为大多数用户推荐一个最佳选项，请使用[组合框](combo-box.md)将该最佳选项显示为默认选项。

:::image type="content" source="images/combo_box_collapsed.png" alt-text="显示默认选项的组合框":::

如果只有两个可能的选项，并且这两个选项可以清楚地表示为单个二元选项（例如“开/关”或“是/否”），则可将它们组合为一个[复选框](checkbox.md)或[切换开关](toggles.md)控件。 例如，使用单个复选框“我同意”，而不是两个单选按钮“我同意”和“我不同意”。

请勿使用两个单选按钮来表示单个二元选项：

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="表示二元选项的两个单选按钮":::

改用复选框：

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="复选框是展示二元选项的良好替代方法":::

当用户可以选择多个选项时，请使用[复选框](checkbox.md)。

:::image type="content" source="images/checkbox2.png" alt-text="复选框支持多选":::

当用户的选项为某个值范围（例如，10、20、30...100）时，请使用[滑块](slider.md)控件。

:::image type="content" source="images/controls/slider.png" alt-text="显示某个值范围中的一个值的滑块控件":::

如果有超过八个选项，请使用[组合框](combo-box.md)。

:::image type="content" source="images/combo_box_scroll.png" alt-text="显示多个选项的列表框":::

如果可用选项基于应用的当前上下文或可以根据其他条件动态变化，请使用列表控件。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p>如果已安装 XAML 控件库<strong style="font-weight: semi-bold"></strong>应用，请<a href="xamlcontrolsgallery:/item/RadioButton">将其打开以查看 RadioButtons 控件的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="winui-radiobuttons-overview"></a>WinUI RadioButtons 概述

键盘访问和导航行为已在 [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons) 控件中进行了优化。 这些改进在辅助功能和键盘功能方面有所帮助，能够让用户更快、更轻松地浏览选项列表。

除这些改进外，RadioButtons 组中各个单选按钮的默认可视布局也通过自动化方向、间距和边距设置进行了优化。 使用更原始的组控件（例如 [StackPanel](../layout/layout-panels.md#stackpanel) 或 [Grid](../layout/layout-panels.md#grid)）时可能必须指定这些属性，但在此项优化后无需进行指定。

### <a name="navigating-a-radiobuttons-group"></a>导航 RadioButtons 组

`RadioButtons` 控件具有特殊的导航行为，可以帮助键盘用户更加快速轻松地导航列表。

#### <a name="keyboard-focus"></a>键盘焦点

`RadioButtons` 控件支持两种状态：

- 未选择单选按钮
- 选择了一个单选按钮

下面各节介绍控件在每个状态下的焦点行为。

##### <a name="no-radio-button-is-selected"></a>未选择单选按钮

如果未选择任何单选按钮，则列表中的第一个单选按钮会获得焦点。

> [!NOTE]
> 不会选择从初始选项卡导航接收选项卡焦点的项。

|没有选项卡焦点的列表 | 具有初始选项卡焦点的列表|
|:--:|:--:|
| ![没有选项卡焦点且未选择任何项的列表](images/radiobutton-no-selected-item-no-tab-focus.png) | ![具有初始选项卡焦点但未选择任何项的列表](images/radiobutton-no-selected-item-tab-focus.png)|

##### <a name="one-radio-button-is-selected"></a>选择了一个单选按钮

当用户进入已选择某个单选按钮的列表时，所选单选按钮会获得焦点。

|没有选项卡焦点的列表 | 具有初始选项卡焦点的列表 |
|:--:|:--:|
| ![没有选项卡焦点但选择了某个项的列表](images/radiobutton-selected-item-no-tab-focus.png) | ![具有初始选项卡焦点且选择了某个项的列表](images/radiobutton-selected-item-tab-focus.png)|

#### <a name="keyboard-navigation"></a>键盘导航

有关常规键盘导航行为的详细信息，请参阅[键盘交互 - 导航](../input/keyboard-interactions.md#navigation)。

如果 `RadioButtons` 组中的某个项已具有焦点，则用户可以使用箭头键在组内的各个项之间进行“内部导航”。 可使用向上键和向下键移动到“上一个”或“下一个”逻辑项，就像 XAML 标记中定义的那样。 使用向左键和向右键可进行空间移动。

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>在单列或单行布局中导航

在单列或单行布局中，键盘导航会导致以下行为：

|单列 | “单行”|
|:--|:--|
| ![单列 RadioButtons 组中键盘导航的示例](images/radiobutton-keyboard-navigation-single-column.png)</br>使用向上键和向下键可在项之间移动。</br>使用向左键和向右键不会执行任何操作。 | ![单行 RadioButtons 组中键盘导航的示例](images/radiobutton-keyboard-navigation-single-row.png)<br/>使用向左键和向上键可移动到前一项，使用向右键和向下键可移动到下一项。 |

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>在多列、多行布局中导航

在多列、多行网格布局中，键盘导航会导致以下行为：

|向左键/向右键| 向上键/向下键 |
|:--|:--|
| ![多列/多行 RadioButtons 组中水平键盘导航的示例](images/radiobutton-keyboard-navigation-multi-column-row-1.png)</br>使用向左键和向右键可让焦点在同一行中的项之间水平移动。 | ![多列/多行 RadioButtons 组中垂直键盘导航的示例](images/radiobutton-keyboard-navigation-multi-column-row-2.png)<br/>使用向上键和向下键可让焦点在同一列中的项之间垂直移动。 |
| ![焦点位于某列中最后一项的水平键盘导航示例](images/radiobutton-keyboard-navigation-multi-column-row-3.png)</br> 当焦点位于某列的最后一项并按下向右键或向左键时，焦点会移至下一列或上一列（如果有）中的最后一项。 | ![焦点位于某列中最后一项的垂直键盘导航示例](images/radiobutton-keyboard-navigation-multi-column-row-4.png)<br/>当焦点位于某列的最后一项并按下向下键时，焦点会移至下一列（如果有）的第一项。 当焦点位于某列的第一项且按下向上键时，焦点会移至上一列（如果有）的最后一个项。 |

有关详细信息，请参阅[键盘交互](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items)。

###### <a name="wrapping"></a>总结

RadioButtons 组不会将焦点从第一行或第一列换行到最后一行或最后一列，也不会从最后一行或最后一列换行到第一行或最后一列。 这是因为在用户使用屏幕阅读器时，会失去边界检测和清晰的开始和结束指示，这让有视力障碍的用户难以导航列表。

`RadioButtons` 控件也不支持枚举，因为该控件用于包含合理数量的项（请参阅[这是正确的控件吗？](#is-this-the-right-control)）。

### <a name="selection-follows-focus"></a>选择跟随焦点

使用键盘在 `RadioButtons` 组中的各项之间导航时，随着焦点从一个项移到下一个项，将选择新获得焦点的项，并清除之前的焦点项。

|键盘导航之前 | 键盘导航之后|
|:--|:--|
| ![键盘导航之前的焦点和选择的示例](images/radiobutton-two-selected-before-keyboard-navigation.png)</br>键盘导航之前的焦点和选择的示例 | ![键盘导航之后的焦点和选择的示例](images/radiobutton-three-selected-after-keyboard-navigation.png)<br/>键盘导航之后的焦点和选择的示例，其中通过向下键将焦点移到单选按钮 3，选择它，并清除单选按钮 2 |

通过使用 Ctrl + 箭头键导航，可以在不更改选择的情况下移动焦点。 移动焦点后，可以使用空格键选择当前具有焦点的项。

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>通过 Xbox 游戏板和遥控器导航

如果你使用 Xbox 手柄或遥控器在单选按钮之间移动，则会禁用“在获得焦点后选中”行为，用户必须按“A”按钮才可选择当前具有焦点的单选按钮。

## <a name="accessibility-behavior"></a>辅助功能行为

下表说明讲述人如何处理 `RadioButtons` 组和播放的内容。 此行为取决于用户如何设置讲述人详细信息首选项。

| 操作 | 讲述人播放内容 |
|:--|:--|
| 焦点移动到所选项 | “名称、RadioButton、已选中、第 x 项，共 N 项”   |
|焦点移动到未选中的项<br> （如果使用 Ctrl + 箭头键或 Xbox 手柄导航，<br>则表示不会在获得焦点后选中。） | “名称、RadioButton、未选中、第 x 项，共 N 项”    |

> [!NOTE]
> 讲述人为每个项播放的“名称”是 [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty) 附加属性的值（如果该项具有此属性）；否则，它是项的 [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0) 方法返回的值。
>
> x 是当前项在组中的位置。 N 是组中的总项数。

## <a name="create-a-winui-radiobuttons-group"></a>创建 WinUI RadioButtons 组

`RadioButtons` 控件使用类似于 [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol) 的内容模型。 这意味着你可以：

- 通过直接向 [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items) 集合添加项或将数据绑定到其 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 属性来填充该控件。
- 使用 [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) 或 [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) 属性来获取和设置所选的选项。
- 处理 [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) 事件，以便在选择某个选项时执行操作。

> [!TIP]
> 在本文档中，我们使用 XAML 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们已将此项添加到我们的[页](/uwp/api/windows.ui.xaml.controls.page)元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在后面的代码中，我们还使用 C# 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们在文件顶部添加了此 **using** 语句：`using muxc = Microsoft.UI.Xaml.Controls;`

此处，你声明了一个包含三个选项的简单 `RadioButtons` 控件。 [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header) 属性设置为向组提供标签，`SelectedIndex` 属性设置为提供默认选项。

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

结果类似以下形式：

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="一个包含三个单选按钮的组":::

要在用户选择某个选项时执行操作，请处理 [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) 事件。 在此处，更改名为“ExampleBorder”的 [Border](/uwp/api/windows.ui.xaml.controls.border) 元素的背景色 (`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`)。

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> 还可以从 [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems) 属性获取所选项。 在索引 0 处只有一个所选项，因此可以按如下所示来获取它：`string colorName = e.AddedItems[0] as string;`。

### <a name="selection-states"></a>选择状态

单选按钮有两个状态：已选择或已清除。 当在 `RadioButtons` 组中选择某个选项时，可以从 [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) 属性获取其值，从 [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) 属性获取其在集合中的位置。 如果用户选择了同一组中的另一个单选按钮，则可以清除单选按钮，但如果用户再次选择它，则无法将其清除。 但是，可以通过设置 `SelectedItem = null` 或 `SelectedIndex = -1`，以编程方式清除单选按钮组。 （如果将 `SelectedIndex` 设置为 `Items` 集合范围之外的任何值，则不会选择任何内容。）

### <a name="radiobuttons-content"></a>RadioButtons 内容

前面的示例使用简单的字符串填充了 `RadioButtons` 控件。 该控件提供了单选按钮，并使用字符串作为每个单选按钮的标签。

但是，你可以用任何对象填充 `RadioButtons` 控件。 通常，你希望对象提供可以用作文本标签的字符串表示形式。 在某些情况下，图像可能适合代替文本。

此处，[SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon) 元素用于填充控件。

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="一组带有符号图标的单选按钮":::

还可以使用单独的 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控件来填充 `RadioButtons` 项。 我们稍后将讨论这种特殊情况。 请参阅 [RadioButtons 组中的 RadioButton 控件]()。

能够使用任何对象的好处是可以将 `RadioButtons` 控件绑定到数据模型中的自定义类型。 下一节将对此进行说明。

### <a name="data-binding"></a>数据绑定

`RadioButtons` 控件支持将数据绑定到其 [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource) 属性。 此示例显示如何将控件绑定到自定义数据源。 此示例的外观和功能与前面的背景色示例相同，但此处，颜色画笔存储在数据模型中，而不是在 `SelectionChanged` 事件处理程序中创建。

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>RadioButtons 组中的 RadioButton 控件

可以使用单独的 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控件来填充 `RadioButtons` 项。 你可能会通过此操作来访问某些属性（如 `AutomationProperties.Name`）；或者，你可能已有 `RadioButton` 代码，但想要利用 `RadioButtons` 的布局和导航。

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

使用 `RadioButtons` 组中的 `RadioButton` 控件时，`RadioButtons` 控件知道如何表示 `RadioButton`，因此你最终不会做出两个选择。

但是，你应该注意一些行为。 我们建议你在单个控件或 `RadioButtons` 上处理状态和事件，但不能同时在这两者之上进行处理，以避免冲突。

下表显示了这两个控件上的相关事件和属性。

|RadioButton  |RadioButtons  |
|---------|---------|
|[Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked)、[Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked)、[Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)、[SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

如果在单个 `RadioButton` 上处理事件（例如 `Checked` 或 `Unchecked`），并且同时处理 `RadioButtons.SelectionChanged` 事件，则两个事件都将触发。 `RadioButton` 事件首先发生，`RadioButtons.SelectionChanged` 事件随后发生，这可能会导致冲突。

`IsChecked`、`SelectedItem` 和 `SelectedIndex` 属性保持同步。 对一个属性的更改将更新其他两个属性。

将忽略 [RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname) 属性。 组是由 `RadioButtons` 控件创建的。

### <a name="defining-multiple-columns"></a>定义多列

默认情况下，`RadioButtons` 控件在单个列中垂直排列其单选按钮。 可以设置 [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) 属性，使控件在多个列中排列单选按钮。 （执行此操作时，单选按钮按列主序顺序排列，即各个项按照从上到下、然后从左到右的顺序填充。）

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="两个三列组中的单选按钮":::

> [!TIP]
> 要将项排列在单个水平行中，请将 `MaxColumns` 设置为组中的项数。

## <a name="create-your-own-radiobutton-group"></a>创建自己的 RadioButton 组

> [!Important]
> 除非使用较旧版本的 WinUI，否则建议使用 WinUI `RadioButtons` 控件对 `RadioButton` 元素进行分组。

单选按钮成组工作。 可以通过以下两种方法之一对各 [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton) 控件进行分组：

- 将它们放在同一个父容器内。
- 在每个单选按钮上将 [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) 属性设置为相同的值。

在此示例中，单选按钮的第一组在相同的堆栈面板中进行隐式分组。 第二组分为两个堆栈面板，因此使用 `GroupName` 将它们显式分组为单个组。

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

这两组 `RadioButton` 控件如下所示：

:::image type="content" source="images/radio-button-groups.png" alt-text="两个组中的单选按钮":::

### <a name="radio-button-states"></a>单选按钮状态

单选按钮有两个状态：已选择或已清除。 选择单选按钮时，其 [IsChecked](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) 属性为 `true`。 清除单选按钮时，其 `IsChecked` 属性为 `false`。 如果用户选择了同一组中的另一个单选按钮，则可以清除单选按钮，但如果用户再次选择它，则无法将其清除。 但是，你可以通过将单选按钮的 `IsChecked` 属性设置为 `false`，以编程方式清除它。

### <a name="visuals-to-consider"></a>要考虑的视觉对象

各 `RadioButton` 控件的默认间距与 `RadioButtons` 组提供的间距不同。 要将 `RadioButtons` 间距应用于各 `RadioButton` 控件，请使用 `Margin` 值 `0,0,7,3`，如下所示。

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

下图显示了组中单选按钮的首选间距。

:::image type="content" source="images/radiobutton-layout.png" alt-text="显示一组垂直排列的单选按钮的图像":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="显示单选按钮间距指南的图像":::

> [!NOTE]
> 如果使用 WinUI RadioButtons 控件，则间距、边距和方向都已经过优化。

## <a name="recommendations"></a>建议

- 请确保一组单选按钮的目的和当前状态十分明确。
- 将单选按钮的文本标签限制在一行以内。
- 如果文本标签是动态的，请考虑如何自动调整按钮大小，以及它周围的所有视觉对象将有哪些影响。
- 请使用默认字体，除非你的品牌指南告诉你使用其他字体。
- 不要并排放置两个 RadioButtons 组。 当两个 RadioButtons 组并排放置时，用户很难确定哪些按钮属于哪个组。

## <a name="get-the-sample-code"></a>获取示例代码

- 若要以交互式格式获取所有 XAML 控件，请参阅 [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery)。

## <a name="related-topics"></a>相关主题

- [按钮](buttons.md)
- [切换开关](toggles.md)
- [复选框](checkbox.md)
- [列表和组合框](lists.md)
- [滑块](slider.md)
- [RadioButtons 类](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [RadioButton 类](/uwp/api/windows.ui.xaml.controls.radiobutton)
