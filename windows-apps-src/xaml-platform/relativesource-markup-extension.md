---
description: 提供一种方式，在运行时对象图中以相对关系形式指定绑定来源。
title: RelativeSource 标记扩展
ms.assetid: B87DEF36-BE1F-4C16-B32E-7A896BD09272
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d8d81adab37f942f804fadb3d8d1ec0cefd20380
ms.sourcegitcommit: 249100d990cd5cf2854c59fa66803b7f83d5db96
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "105938962"
---
# <a name="relativesource-markup-extension"></a>{RelativeSource} 标记扩展


提供一种方式，在运行时对象图中以相对关系形式指定绑定来源。

## <a name="xaml-attribute-usage-self-mode"></a>XAML 属性使用方法（Self 模式）

``` syntax
<Binding RelativeSource="{RelativeSource Self}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource Self} ...}" .../>
```

## <a name="xaml-attribute-usage-templatedparent-mode"></a>XAML 属性使用方法（TemplatedParent 模式）

``` syntax
<Binding RelativeSource="{RelativeSource TemplatedParent}" .../>
-or-
<object property="{Binding RelativeSource={RelativeSource TemplatedParent} ...}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 说明 |
|------|-------------|
| {RelativeSource Self} | 生成 <strong>Self</strong> 的 [<strong>Mode</strong>](/uwp/api/windows.ui.xaml.data.relativesource.mode) 值。 目标元素应用作此绑定的来源。 这对于将一个元素的属性绑定到相同元素上的另一个属性很有用。 |
| {RelativeSource TemplatedParent} | 生成将应用为此绑定来源的 [<strong>ControlTemplate</strong>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)。 这对于在模板级别向绑定应用运行时信息很有用。 |

## <a name="remarks"></a>备注

[**Binding**](/uwp/api/Windows.UI.Xaml.Data.Binding) 可以将 [**Binding.RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 设置为 **Binding** 对象元素的属性或者 [{Binding} 标记扩展](binding-markup-extension.md)内部的组件。 这就是显示两种不同 XAML 语法的原因。

**RelativeSource** 类似于 [{Binding} 标记扩展](binding-markup-extension.md)。  它是一个能够返回自身实例的标记扩展，支持一种基于字符串的结构，该结构在本质上会将一个参数传递给构造函数。 在本例中，传递的参数是 [**Mode**](/uwp/api/windows.ui.xaml.data.relativesource.mode) 值。

**Self** 模式对于将元素的一个属性绑定到相同元素上的另一个属性很有用，并且该模式是 [**ElementName**](/uwp/api/windows.ui.xaml.data.binding.elementname) 绑定上的一个变体，但不需要命名和自引用元素。 如果将元素的一个属性绑定到相同元素上的另一个属性，那么两个属性都必须使用相同的属性类型，或者你还必须在绑定上使用 [**Converter**](/uwp/api/windows.ui.xaml.data.binding.converter) 来转换这些值。 例如，可以使用 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 作为 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 的源而不进行转换，但是你将需要一个转换器来将 [**IsEnabled**](/uwp/api/windows.ui.xaml.controls.control.isenabled) 用作 [**Visibility**](/uwp/api/Windows.UI.Xaml.Visibility) 的源。

下面是一个示例。 此 [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) 使用 [{Binding} 标记扩展](binding-markup-extension.md)，以便 [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) 和 [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) 始终相等，并以正方形形式呈现它。 仅将 Height 设置为固定值。 对于此 **Rectangle**，其默认 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 是 **null**，而不是 **this**。 因此，若要创建将成为对象本身的数据上下文源（并支持绑定到它的其他属性），我们使用 {Binding} 标记扩展用法中的 `RelativeSource={RelativeSource Self}` 参数。

```XML
<Rectangle
  Fill="Orange" Width="200"
  Height="{Binding RelativeSource={RelativeSource Self}, Path=Width}"
/>
```

`RelativeSource={RelativeSource Self}` 的另一个用途是作为将对象的 [**DataContext**](/uwp/api/windows.ui.xaml.frameworkelement.datacontext) 设置为其自身的一种方式。  例如，你可能会在一些 SDK 示例中看到此方法，其中 [**页面**](/uwp/api/Windows.UI.Xaml.Controls.Page) 类已使用自定义属性进行了扩展，该自定义属性已经为自己的数据绑定提供现成的视图模型，例如： `<common:LayoutAwarePage ... DataContext="{Binding DefaultViewModel, RelativeSource={RelativeSource Self}}">`

**注意****RelativeSource** 的 XAML 用法仅显示它应有的用法：作为绑定表达式的一部分，在 XAML 中设置 [**Binding.RelativeSource**](/uwp/api/windows.ui.xaml.data.binding.relativesource) 的值。 在理论上，如果将一个属性的值设置为 [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)，也可能还有其他用法。

## <a name="related-topics"></a>相关主题

* [XAML概述](xaml-overview.md)
* [深入了解数据绑定](../data-binding/data-binding-in-depth.md)
* [{Binding} 标记扩展](binding-markup-extension.md)
* [**绑定**](/uwp/api/Windows.UI.Xaml.Data.Binding)
* [**RelativeSource**](/uwp/api/Windows.UI.Xaml.Data.RelativeSource)