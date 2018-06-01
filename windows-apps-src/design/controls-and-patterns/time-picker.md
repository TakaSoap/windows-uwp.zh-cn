---
author: muhsinking
Description: The time picker gives you a standardized way to let users pick a time value using touch, mouse, or keyboard input.
title: 时间选取器
ms.assetid: 5124ecda-09e6-449e-9d4a-d969dca46aa3
label: Time picker
template: detail.hbs
ms.author: mukin
ms.date: 05/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kisai
design-contact: ksulliv
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: b54716798b41bb7f7ac0da53f895cf91735e4e31
ms.sourcegitcommit: 4b522af988273946414a04fbbd1d7fde40f8ba5e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2018
ms.locfileid: "1494064"
---
# <a name="time-picker"></a>时间选取器
 

时间选取器提供了一种标准化途径，可使用户使用触摸、鼠标或键盘输入选取时间值。 

> **重要 API**: [TimePicker 类](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.aspx)，[时间属性](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.timepicker.time.aspx)


## <a name="is-this-the-right-control"></a>这是正确的控件吗？
使用时间选取器使用户可以选取单个时间值。

有关选择正确控件的详细信息，请参阅[日期和时间控件](date-and-time.md)文章。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/TimePicker">打开此应用，了解 TimePicker 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

入口点显示选定的时间，当用户选择该入口点时，会从中间垂直展开一个选取器图面以供用户进行选择。 时间选取器会覆盖其他 UI；它不会将其他 UI 推开。

![时间选取器展开示例](images/controls_timepicker_expand.png)

## <a name="create-a-time-picker"></a>创建时间选取器

本示例显示如何创建带有标题的简单时间选取器。

```xaml
<TimePicker x:Name=arrivalTimePicker Header="Arrival time"/>
```

```csharp
TimePicker arrivalTimePicker = new TimePicker();
arrivalTimePicker.Header = "Arrival time";
```

生成的时间选取器如下所示：

![时间选取器示例](images/time-picker-closed.png)

> [!NOTE]
> 有关日期和时间值的重要信息，请参阅*日期和时间控件*一文中的[DateTime 和日历值](date-and-time.md#datetime-and-calendar-values)。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

- [日期和时间控件](date-and-time.md)
- [日历日期选取器](calendar-date-picker.md)
- [日历视图](calendar-view.md)
- [日期选取器](date-picker.md)