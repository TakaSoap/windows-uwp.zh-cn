---
description: 日期选取器向你提供了一种标准化方式，可使用户通过触摸、鼠标或键盘输入选取本地化格式的日期值。
title: 日期选取器
ms.assetid: d4a01425-4dee-4de3-9a05-3e85c3fc03cb
isNew: true
label: Date picker
template: detail.hbs
ms.date: 04/02/2021
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 51fa98908fa4a2c1c026ce5a4c924b8f800aee8b
ms.sourcegitcommit: 62a6e7b4d35f63c25cedd61c96dfc251ff19c80d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106286596"
---
# <a name="date-picker"></a>日期选取器

日期选取器向你提供了一种标准化方式，可使用户通过触摸、鼠标或键盘输入选取本地化格式的日期值。

![日期选取器示例](images/date-picker-closed.png)

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

> **平台 API：** [DatePicker 类](/uwp/api/Windows.UI.Xaml.Controls.DatePicker)，[SelectedDate 属性](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)

## <a name="is-this-the-right-control"></a>这是正确的控件吗？

使用日期选取器以使用户选取日历上下文不重要的已知日期，例如生日。

如果日历的上下文很重要，请考虑使用[日历日期选取器](calendar-date-picker.md)或[日历视图](calendar-view.md)。

有关选择正确日期控件的详细信息，请参阅[日期和时间控件](date-and-time.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 XAML 控件库应用，请单击此处<a href="xamlcontrolsgallery:/item/DatePicker">打开此应用，了解 DatePicker 的实际应用</a><strong style="font-weight: semi-bold"></strong>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

入口点显示选定的日期，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 日期选取器会覆盖其他 UI；它不会将其他 UI 推开。

![日期选取器展开示例](images/controls_datepicker_expand.png)

## <a name="create-a-date-picker"></a>创建日期选取器

本示例演示如何创建附带标头的简单日期选取器。

```xaml
<DatePicker x:Name="birthDatePicker" Header="Date of birth"/>
```

```csharp
DatePicker birthDatePicker = new DatePicker();
birthDatePicker.Header = "Date of birth";
```

生成的日期选取器如下所示：

![日期选取器示例](images/date-picker-closed.png)

### <a name="formatting-the-date-picker"></a>设置日期选取器的格式

默认情况下，日期选取器按年、月、日的顺序显示日期。 如果你的日期选取器方案不需要所有字段，你可隐藏不需要的字段。 若要隐藏字段，请将其相应字段的 Visible 属性设置为 `false`：[DayVisible](/uwp/api/windows.ui.xaml.controls.datepicker.dayvisible)、[MonthVisible](/uwp/api/windows.ui.xaml.controls.datepicker.monthvisible) 或 [YearVisible](/uwp/api/windows.ui.xaml.controls.datepicker.yearvisible)。

此处只需显示年，因此隐藏日期和月份字段。

```xaml
<DatePicker x:Name="yearDatePicker" Header="In what year was Microsoft founded?" 
            MonthVisible="False" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-year-only.png" alt-text="隐藏了日期字段和月份字段的日期选取器。":::

`DatePicker` 中每个 `ComboBox` 的字符串内容是通过 [DateTimeFormatter](/uwp/api/windows.globalization.datetimeformatting.datetimeformatter) 创建的。 通过提供字符串（格式模板或格式模式）来指示 `DateTimeFormatter` 如何设置日期值的格式 。 有关详细信息，请查看 [DayFormat](/uwp/api/windows.ui.xaml.controls.datepicker.dayformat)、[MonthFormat](/uwp/api/windows.ui.xaml.controls.datepicker.monthformat) 和 [YearFormat](/uwp/api/windows.ui.xaml.controls.datepicker.yearformat) 属性。

在这里，使用格式模式来将月份显示为整数和缩写形式。 可向格式模式添加文本字符串，例如在月份缩写两边加括号：`({month.abbreviated})`。

```xaml
<DatePicker MonthFormat="{}{month.integer(2)} ({month.abbreviated})" DayVisible="False"/>
```

:::image type="content" source="images/date-time/date-picker-day-hidden.png" alt-text="隐藏了日期字段的日期选取器。":::

### <a name="date-values"></a>日期值

日期选取器控件同时包含 [Date](/uwp/api/windows.ui.xaml.controls.datepicker.date)/[DateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.datechanged) 和 [SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate)/[SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged) API。 这两者的区别是，`Date` 不可为 null，而 `SelectedDate` 可以为 null。

`SelectedDate` 的值用于填充日期选取器，它默认为 `null`。 如果 `SelectedDate` 是 `null`，则 `Date` 属性设置为 1600/12/31；否则，`Date` 值与 `SelectedDate` 值同步。 当 `SelectedDate` 是 `null`时，不设置选取器，它显示字段名称而不是日期。

:::image type="content" source="images/date-time/date-picker-no-selected-date.png" alt-text="未选择任何日期的日期选取器。":::

可设置 [MinYear](/uwp/api/windows.ui.xaml.controls.datepicker.minyear) 和 [MaxYear](/uwp/api/windows.ui.xaml.controls.datepicker.maxyear) 属性来限制选取器中的日期值。 默认情况下，`MinYear` 设置为比当前日期减去 100 年，`MaxYear` 设置为当期日期加 100 年。

如果仅设置了 `MinYear` 或 `MaxYear`，则需要确保有效值范围介于你设置的日期和另一日期的默认值之间；否则，选取器中将没有日期可供选择。 例如，仅设置 `yearDatePicker.MaxYear = new DateTimeOffset(new DateTime(900, 1, 1));` 会生成一个具有 `MinYear` 默认值的无效日期范围。

#### <a name="initializing-a-date-value"></a>初始化日期值

日期属性不可设置为 XAML 特性字符串，因为 Windows 运行时 XAML 解析器不具有用于将字符串转换为日期（作为 [DateTime](/uwp/api/windows.foundation.datetime) / [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true) 对象）的转换逻辑。 下面是一些建议的方法，可通过它们在代码中定义这些对象，并将其设置为当前日期以外的日期。

- [DateTime](/uwp/api/windows.foundation.datetime)：将 [Windows.Globalization.Calendar](/uwp/api/windows.globalization.calendar) 对象实例化（它会初始化为当前日期）。 设置[年份](/uwp/api/windows.globalization.calendar.year)或调用 [AddYears](/uwp/api/windows.globalization.calendar.addyears) 来调整日期。 然后，调用 [Calendar.GetDateTime](/uwp/api/windows.globalization.calendar.getdatetime) 并使用返回的 `DateTime` 来设置日期属性。
- [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true)：调用构造函数。 对于内部 [System.DateTime](/dotnet/api/system.datetime?view=dotnet-uwp-10.0&preserve-view=true)，请使用 构造函数签名。 或者，构造默认的 [DateTimeOffset](/dotnet/api/system.datetimeoffset?view=dotnet-uwp-10.0&preserve-view=true)（它会初始化为当前日期）并调用 [AddYears](/dotnet/api/system.datetimeoffset.addyears?view=dotnet-uwp-10.0&preserve-view=true)。

另一个可行的方法是定义可用作数据对象或在数据上下文中可用的日期，然后将日期属性设置为引用 [\{Binding\} 标记扩展](/windows/uwp/xaml-platform/binding-markup-extension)的 XAML 特定，以便可作为数据访问该日期。

> [!NOTE]
> 有关日期值的重要信息，请参阅日期和时间控件一文中的 [Datetime 和日历值](date-and-time.md#datetime-and-calendar-values)。

此示例演示了在不同的 `DatePicker` 控件上设置 `SelectedDate`、`MinYear` 和 `MaxYear` 属性的情况。

```xaml
<DatePicker x:Name="yearDatePicker" MonthVisible="False" DayVisible="False"/>
<DatePicker x:Name="arrivalDatePicker" Header="Arrival date"/>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    // Set minimum year to 1900 and maximum year to 1999.
    yearDatePicker.SelectedDate = new DateTimeOffset(new DateTime(1950, 1, 1));
    yearDatePicker.MinYear = new DateTimeOffset(new DateTime(1900, 1, 1));
    // Using a different DateTimeOffset constructor.
    yearDatePicker.MaxYear = new DateTimeOffset(1999, 12, 31, 0, 0, 0, new TimeSpan());

    // Set minimum to the current year and maximum to five years from now.
    arrivalDatePicker.MinYear = DateTimeOffset.Now;
    arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
}
```

### <a name="using-the-date-values"></a>使用日期值

若要在应用中使用日期值，通常需要对 [SelectedDate](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddate) 属性使用数据绑定，或者处理 [SelectedDateChanged](/uwp/api/windows.ui.xaml.controls.datepicker.selecteddatechanged) 事件。

> 有关结合使用 `DatePicker` 和 `TimePicker` 来更新单个 `DateTime` 值的示例，请查看[日历、日期和时间控件 - 将日期选取器和时间选取器结合使用](/windows/uwp/design/controls-and-patterns/date-and-time#use-a-date-picker-and-time-picker-together)。

此处使用 `DatePicker` 来让用户选择其抵达日期。 需要处理 `SelectedDateChanged` 事件来更新名为 `arrivalDateTime` 的 [DateTime](/uwp/api/windows.foundation.datetime) 实例。

```xaml
<StackPanel>
    <DatePicker x:Name="arrivalDatePicker" Header="Arrival date"
                DayFormat="{}{day.integer} ({dayofweek.abbreviated})"
                SelectedDateChanged="arrivalDatePicker_SelectedDateChanged"/>
    <Button Content="Clear" Click="ClearDateButton_Click"/>
    <TextBlock x:Name="arrivalText" Margin="0,12"/>
</StackPanel>
```

```csharp
public sealed partial class MainPage : Page
{
    DateTime arrivalDateTime;

    public MainPage()
    {
        this.InitializeComponent();

        // Set minimum to the current year and maximum to five years from now.
        arrivalDatePicker.MinYear = DateTimeOffset.Now;
        arrivalDatePicker.MaxYear = DateTimeOffset.Now.AddYears(5);
    }

    private void arrivalDatePicker_SelectedDateChanged(DatePicker sender, DatePickerSelectedValueChangedEventArgs args)
    {
        if (arrivalDatePicker.SelectedDate != null)
        {
            arrivalDateTime = new DateTime(args.NewDate.Value.Year, args.NewDate.Value.Month, args.NewDate.Value.Day);
        }
        arrivalText.Text = arrivalDateTime.ToString();
    }

    private void ClearDateButton_Click(object sender, RoutedEventArgs e)
    {
        arrivalDatePicker.SelectedDate = null;
        arrivalText.Text = string.Empty;
    }
}
```

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-articles"></a>相关文章

- [日期和时间控件](date-and-time.md)
- [日历日期选取器](calendar-date-picker.md)
- [日历视图](calendar-view.md)
- [时间选取器](time-picker.md)
