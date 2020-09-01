---
description: 唯一标识对象元素，可方便从代码隐藏或一般代码中访问已实例化的对象。
title: xName 属性
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b5fd9c2b482eb79fcdace2c068afa21bad673de2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89161671"
---
# <a name="xname-attribute"></a>x:Name 属性


唯一标识对象元素，可方便从代码隐藏或一般代码中访问已实例化的对象。 应用于支持的编程模型之后，**x:Name** 可视为等效于持有一个对象引用（由一个构造函数返回）的变量。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 描述 |
|------|-------------|
| XAMLNameValue | 一个符合 XamlName 语法限制的字符串。 |

##  <a name="xamlname-grammar"></a>XamlName 语法

以下是在此 XAML 实现中作为密钥使用的字符串的规范语法：

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   字符限制为较小的 ASCII 范围，更专用于罗马字母大写和小写字母、数字和下划线 (\_) 字符。
-   不支持 Unicode 字符范围。
-   名称不能以数字开头。 如果用户将数字作为初始字符提供，则某些工具实现会将下划线 (\_) 添加到字符串，或者工具会根据包含数字的其他值自动生成 **x：Name** 值。

## <a name="remarks"></a>备注

当处理 XAML 时，指定的 **x:Name** 变成一个在基础代码中创建的字段的名称，该字段持有该对象的引用。 创建此字段的过程由 MSBuild 目标步骤执行，这些步骤还负责连接一个 XAML 文件和它的代码隐藏的分部类。 此行为不一定是由 XAML 语言指定的，它是通用 Windows 平台 (UWP) XAML 编程的特定实现，应用在其编程和应用程序模型中使用 **x:Name**。

每个已定义的 **x:Name** 在一个 XAML 名称范围中必须是唯一的。 通常，XAML 名称范围在所加载页面的根元素级别定义，其中包含单个 XAML 页面中该元素下面的所有元素。 其他 XAML 名称范围由在该页面上定义的任何控件模板或数据模板定义。 在运行时，其他 XAML 名称范围是为从所应用的控件模板创建的对象树的根，以及由从 [**XamlReader.Load**](/uwp/api/windows.ui.xaml.markup.xamlreader.load) 调用创建的对象树定义的。 有关详细信息，请参阅 [XAML 空间范围](xaml-namescopes.md)。

在将元素引入设计界面时，设计工具常常会自动为它们生成 **x:Name** 值。 该自动生成架构因使用的设计器不同而不同，但一种典型的架构是生成一个字符串，它以支持该元素的类名开头，后跟一个延长的整数。 例如，如果向设计器引入第一个 [**Button**](/uwp/api/Windows.UI.Xaml.Controls.Button) 元素，则可以在 XAML 中看到，此元素拥有 **x:Name** 属性值“Button1”。

无法在 XAML 属性元素语法中或在代码中使用 [**SetValue**](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) 设置 **x:Name**。 只能在元素上使用 XAML 属性语法来设置 **x:Name**。

**注意**   专用于 c + +/CX 应用，不会为 XAML 文件或页面的根元素创建**x：Name**引用的支持字段。 如果你需要从 C++ 代码隐藏来引用根对象，请使用其他 API 或树形遍历。 例如，你可以为已知的命名子元素调用 [**FindName**](/uwp/api/windows.ui.xaml.frameworkelement.findname)，然后调用 [**Parent**](/uwp/api/windows.ui.xaml.frameworkelement.parent)。

### <a name="xname-and-other-name-properties"></a>x:Name 和其他 Name 属性

UWP XAML 中使用的一些类型还具有一个名为 **Name** 的属性。 例如，[**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) 和 [**TextElement.Name**](/uwp/api/windows.ui.xaml.documents.textelement.name)。

如果 **Name** 可用作一个元素上的可设置属性，**Name** 和 **x:Name** 可在 XAML 中交替使用，但如果在相同元素上指定了这两个属性，会发生错误。 有时，会存在一个只读的 **Name** 属性（如 [**VisualState.Name**](/uwp/api/windows.ui.xaml.visualstate.name)）。 如果出现这种情况，请在 XAML 中始终使用 **x:Name** 对该元素进行命名，而且对于一些少见的代码方案使用只读的 **Name**。

**注意**  [**FrameworkElement.Name**](/uwp/api/windows.ui.xaml.frameworkelement.name) 通常不应当用于更改最初由 **x:Name** 设置的值，但这个一般规则有一些例外的场景。 在典型的场景中，XAML 名称范围的创建和定义是一个 XAML 处理器操作。 在运行时修改 **FrameworkElement.Name** 可能会导致不一致的 XAML 名称范围/专用字段命名对齐，这种不一致在代码隐藏文件中很难跟踪。

### <a name="xname-and-xkey"></a>x:Name 和 x:Key

**x:Name** 可以作为一个属性应用到 [**ResourceDictionary**](/uwp/api/Windows.UI.Xaml.ResourceDictionary) 内的元素，以充当 [x:Key 属性](x-key-attribute.md)的替代属性。 （通常，要求 **ResourceDictionary** 中的所有元素都必须具有一个 x:Key 或 x:Name 属性。）这常见于[情节提要动画](../design/motion/storyboarded-animations.md)。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源引用](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)部分。