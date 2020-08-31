---
description: 将一个控件模板中的属性值链接到模板控件上的某个其他公开的属性的值。 TemplateBinding 只能在 XAML 中的 ControlTemplate 定义中使用。
title: TemplateBinding 标记扩展
ms.assetid: FDE71086-9D42-4287-89ED-8FBFCDF169DC
ms.date: 10/29/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3b368ca2f429d52674ba1cb3493012d54dc0848a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154921"
---
# <a name="templatebinding-markup-extension"></a>{TemplateBinding} 标记扩展

将一个控件模板中的属性值链接到模板控件上的某个其他公开的属性的值。 **TemplateBinding** 只能在 XAML 的 [**system.windows.controls.controltemplate>**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定义中使用。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object propertyName="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-attribute-usage-for-setter-property-in-template-or-style"></a>XAML 属性使用方法（针对模板或样式中的 Setter 属性）

``` syntax
<Setter Property="propertyName" Value="{TemplateBinding sourceProperty}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 描述 |
|------|-------------|
| propertyName | 在 setter 语法中设置的属性的名称。 这必须是一个依赖属性。 |
| sourceProperty | 存在于模板化的类型上的其他依赖属性的名称。 |

## <a name="remarks"></a>备注

如果你是自定义控件的作者或者要替换现有控件的控件模板，使用 **TemplateBinding** 是如何定义控件模板的基础部分。 有关详细信息，请参阅[快速入门：控件模板](/previous-versions/windows/apps/hh465374(v=win.10))。

*propertyName* 和 *targetProperty* 常常使用相同的属性名称。 在此情况下，一个控件可以在自身定义一个属性，并将该属性转发到其组件部分之一的直观命名的现有属性。 例如，在其复合中合并了 [**TextBlock**](/uwp/api/Windows.UI.Xaml.Controls.TextBlock) 的控件（用于显示控件自身的 **Text** 属性），可以将此 XAML 包含为控件模板中的一部分： `<TextBlock Text="{TemplateBinding Text}" .... />`

用作源属性值和目标属性值的类型必须匹配。 使用 **TemplateBinding** 时，无法引入转换器。 解析 XAML 时，无法匹配值将导致错误。 如果需要转换器，可以对模板绑定使用 verbose 语法，例如： `{Binding RelativeSource={RelativeSource TemplatedParent}, Converter="..." ...}`

尝试在 XAML 中的 [**ControlTemplate**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) 定义外使用 **TemplateBinding** 将导致分析器错误。

在模板化父值还会因为其他绑定而推迟的情况下可以使用 **TemplateBinding**。 **TemplateBinding** 的评估可以等待任何所需运行时绑定都具有值。

**TemplateBinding** 始终是单向绑定。 所涉及的两个属性都必须是依赖项属性。

**TemplateBinding** 是标记扩展。 当要求转义特性值应为非文本值或非处理程序名称时，通常会实现标记扩展，相对于只在某些类型或属性上放置类型转换器而言，此需求更具有全局性。 XAML 中的所有标记扩展在其属性语法中都使用“{”和“}”字符，通过此约定，XAML 处理器可以知道标记扩展必须处理属性。

**注意**   在 Windows 运行时 XAML 处理器实现中，没有适用于**TemplateBinding**的支持类表示形式。 **TemplateBinding** 专用于 XAML 标记中。 无法通过一种简单的方式来重现代码中的行为。

### <a name="xbind-in-controltemplate"></a>System.windows.controls.controltemplate> 中的 x:Bind

> [!NOTE]
> 在 System.windows.controls.controltemplate> 中使用 x:Bind 需要 Windows 10 版本 1809 ([SDK 17763](https://developer.microsoft.com/windows/downloads/windows-10-sdk)) 或更高版本。 有关目标版本的详细信息，请参阅[版本自适应代码](../debug-test-perf/version-adaptive-code.md)。

从 Windows 10 版本1809开始，可以在[**system.windows.controls.controltemplate>**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)中使用**TemplateBinding**的任何位置使用**x:Bind**标记扩展。 

使用**x:Bind**时， [system.windows.controls.controltemplate>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)上不需要使用[TargetType](/uwp/api/windows.ui.xaml.controls.controltemplate.targettype)属性 (可选) 。

使用**x:Bind**支持，可以在[system.windows.controls.controltemplate>](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)中同时使用两种[函数绑定](../data-binding/function-bindings.md)和双向绑定。

在此示例中， **TextBlock** 属性的计算结果为 **按钮. content-type**。 System.windows.controls.controltemplate> 上的 TargetType 作为数据源，并以与 TemplateBinding 到父级相同的结果完成。

```xaml
<ControlTemplate TargetType="Button">
    <Grid>
        <TextBlock Text="{x:Bind Content, Mode=OneWay}"/>
    </Grid>
</ControlTemplate>
```

## <a name="related-topics"></a>相关主题

* [快速入门：控件模板](/previous-versions/windows/apps/hh465374(v=win.10))
* [深入了解数据绑定](../data-binding/data-binding-in-depth.md)
* [**System.windows.controls.controltemplate>**](/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate)
* [XAML概述](xaml-overview.md)
* [依赖项属性概述](dependency-properties-overview.md)
 