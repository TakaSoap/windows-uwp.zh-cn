---
description: 时间选取器提供了一种标准化途径，可使用户使用触摸、鼠标或键盘输入选取时间值。
title: 时间选取器
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4c3d5391e3d738e7de81362a5fd0e42dc6d007dd
ms.sourcegitcommit: cc871be2508f52509b6a947fe879aeec360d0fd2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106270222"
---
# <a name="time-picker"></a>时间选取器
 
时间选取器提供了一种标准化途径，可使用户使用触摸、鼠标或键盘输入选取时间值。

![时间选取器示例](images/time-picker-closed.png)

**获取 Windows UI 库**

:::row:::
   :::column:::
      ![WinUI 徽标](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
       Windows UI 库 2.2 或更高版本包含此控件的使用圆角的新模板。 有关详细信息，请参阅[圆角半径](../style/rounded-corner.md)。 WinUI 是一种 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **平台 API**：[TimePicker 类](/uwp/api/Windows.UI.Xaml.Controls.TimePicker)，[SelectedTime 属性](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？
使用时间选取器使用户可以选取单个时间值。

有关选择正确控件的详细信息，请参阅[日期和时间控件](date-and-time.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/TimePicker">打开此应用，了解 TimePicker 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

入口点显示选定的时间，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 时间选取器会覆盖其他 UI；它不会将其他 UI 推开。

![时间选取器展开示例](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>创建时间选取器

本示例显示如何创建带有标题的简单时间选取器。

```xaml
<TimePicker x:Name="arrivalTimePicker" Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

生成的时间选取器如下所示：

![时间选取器示例](images/time-picker-closed.png)

### <a name="formatting-the-time-picker"></a>设置时间选取器的格式

默认情况下，时间选取器显示 12 小时制并提供“上午/下午”选择器。 可将 [ClockIdentifier](/uwp/api/windows.ui.xaml.controls.timepicker.clockidentifier) 属性设置为“24HourClock”，改为按 24 小时制显示时间。

```xaml
<TimePicker Header="12HourClock" SelectedTime="14:30"/>
<TimePicker Header="24HourClock" SelectedTime="14:30" ClockIdentifier="24HourClock"/>
```

:::image type="content" source="images/date-time/time-picker-clocks.png" alt-text="一个时间选取器显示 12 小时制，一个选取器显示 24 小时制。":::

可设置 [MinuteIncrement](/uwp/api/windows.ui.xaml.controls.timepicker.minuteincrement) 属性来指示分钟选取器中显示的时间增量。 例如，数字 15 指定 `TimePicker` 分钟控件仅显示 00、15、30 和 45。

```xaml
<TimePicker MinuteIncrement="15"/>
```

:::image type="content" source="images/date-time/time-picker-minute-increment.png" alt-text="一个时间选取器显示 15 分钟增量。":::

### <a name="time-values"></a>时间值

时间选取器控件同时具有 [Time](/uwp/api/windows.ui.xaml.controls.timepicker.time)/[TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged) 和 [SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime)/[SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged) API。 这两者的区别是，`Time` 不可为 null，而 `SelectedTime` 可以为 null。

`SelectedTime` 的值用于填充时间选取器，它默认为 `null`。 如果 `SelectedTime` 是 `null`，则 `Time` 属性设置为 [TimeSpan](/dotnet/api/system.timespan?view=dotnet-uwp-10.0&preserve-view=true) 0；否则，`Time` 值与 `SelectedTime` 值同步。 当 `SelectedTime` 是 `null`时，不设置选取器，它显示字段名称而不是时间。

:::image type="content" source="images/date-time/time-picker-no-selected-time.png" alt-text="没有选择任何时间的时间选取器。":::

#### <a name="initializing-a-time-value"></a>初始化时间值

在代码中，可将时间属性初始化为类型 `TimeSpan` 的值：

```csharp
TimePicker timePicker = new TimePicker
{
    SelectedTime = new TimeSpan(14, 15, 00) // Seconds are ignored.
};
```

可在 XAML 中将时间值设置为特性。 如果已在 XAML 中声明 `TimePicker` 对象，且未对时间值使用绑定，则这种方法可能最简单。 使用 Hh:Mm 格式的字符串；其中，Hh 是小时数，可介于 0 到 23 之间，Mm 是分钟数，可介于 0 到 59 之间  。

```xaml
<TimePicker SelectedTime="14:15"/>
```

> [!NOTE]
> 有关日期和时间值的重要信息，请参阅 *日期和时间控件* 一文中的 [DateTime 和日历值](date-and-time.md#datetime-and-calendar-values)。

### <a name="using-the-time-values"></a>使用时间值

若要在应用中使用时间值，通常需要对 [SelectedTime](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtime) 或 [Time](/uwp/api/windows.ui.xaml.controls.timepicker.time) 属性使用数据绑定，直接在代码中使用时间属性，或者处理 [SelectedTimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.selectedtimechanged) 或 [TimeChanged](/uwp/api/windows.ui.xaml.controls.timepicker.timechanged) 事件。

> 有关结合使用 `DatePicker` 和 `TimePicker` 来更新单个 `DateTime` 值的示例，请查看[日历、日期和时间控件 - 将日期选取器和时间选取器结合使用](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together)。

在这里，`SelectedTime` 属性用于将所选时间与当前时间进行比较。

请注意，由于 `SelectedTime` 属性可为 null，因此必须将其显式强制转换为 `DateTime`，如下所示：`DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);`。 不过，可使用 `Time` 而不强制转换，如下所示：`DateTime myTime = DateTime.Today + checkTimePicker.Time;`。

:::image type="content" source="images/date-time/time-picker-check.png" alt-text="时间选取器、按钮和文本标签。":::

```xaml
<StackPanel>
    <TimePicker x:Name="checkTimePicker"/>
    <Button Content="Check time" Click="{x:Bind CheckTime}"/>
    <TextBlock x:Name="resultText"/>
</StackPanel>
```

```csharp
private void CheckTime()
{
    // Using the Time property.
    // DateTime myTime = DateTime.Today + checkTimePicker.Time;
    // Using the SelectedTime property (nullable requires cast to DateTime).
    DateTime myTime = (DateTime)(DateTime.Today + checkTimePicker.SelectedTime);
    if (DateTime.Now >= myTime)
    {
        resultText.Text = "Your selected time has already past.";
    }
    else
    {
        string hrs = (myTime - DateTime.Now).Hours.ToString();
        string mins = (myTime - DateTime.Now).Minutes.ToString();
        resultText.Text = string.Format("Your selected time is {0} hours, {1} minutes from now.", hrs, mins);
    }
}
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

- [日期和时间控件](date-and-time.md)
- [日历日期选取器](calendar-date-picker.md)
- [日历视图](calendar-view.md)
- [日期选取器](date-picker.md)
