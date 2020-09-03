---
Description: 使用标签向用户指示他们应向邻近控件输入的内容。 还可以为一组相关控件添加标签，或在一组相关控件旁显示说明文本。
title: 标签
ms.assetid: CFACCCD4-749F-43FB-947E-2591AE673804
label: Labels
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 68b062dc4bd70c81b1b8b57808fad8e9c7498d75
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172611"
---
# <a name="labels"></a>标签

 

标签是一个控件或一组相关控件的名称或标题。

> **重要的 API**：Header 属性，[TextBlock 类](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

在 XAML 中，许多控件都具有用于显示标签的内置 Header 属性。 对于没有 Header 属性的控件，或要为多组控件添加标签，你可以改用 [TextBlock](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)。

![说明标准标签控件的屏幕截图](images/label-standard.png)

## <a name="recommendations"></a>建议


-   使用标签向用户指示他们应向邻近控件输入的内容。 还可以为一组相关控件添加标签，或在一组相关控件旁显示说明文本。
-   在为控件添加标签时，请将标签写成名词或简洁的名词短语，不要写成一个句子，也不要写成说明文本。 避免使用冒号或其他标点符号。
-   当标签中确实有说明文本时，文本字符串长度可以增加，并且可以使用标点符号。


## <a name="get-the-sample-code"></a>获取示例代码
* [XAML UI 基本示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics)

## <a name="related-topics"></a>相关主题
* [文本控件](text-controls.md)
* [TextBox.Header 属性](/uwp/api/windows.ui.xaml.controls.textbox.header)
* [PasswordBox.Header 属性](/uwp/api/windows.ui.xaml.controls.passwordbox.header)
* [ToggleSwitch.Header 属性](/uwp/api/windows.ui.xaml.controls.toggleswitch.header)
* [DatePicker.Header 属性](/uwp/api/windows.ui.xaml.controls.datepicker.header)
* [TimePicker.Header 属性](/uwp/api/windows.ui.xaml.controls.timepicker.header)
* [Slider.Header 属性](/uwp/api/windows.ui.xaml.controls.slider.header)
* [ComboBox.Header 属性](/uwp/api/windows.ui.xaml.controls.combobox.header)
* [RichEditBox.Header 属性](/uwp/api/windows.ui.xaml.controls.richeditbox.header)
* [TextBlock 类](/uwp/api/Windows.UI.Xaml.Controls.TextBlock)

 

 