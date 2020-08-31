---
description: 通过计算对来源于自定义资源查找实现的资源的引用，为任何 XAML 属性提供值。 资源查找是通过 CustomXamlResourceLoader 类实现执行的。
title: CustomResource 标记扩展
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 094a2a957493787acef8e97b49b61778fc1ec42f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155101"
---
# <a name="customresource-markup-extension"></a>{CustomResource} 标记扩展


通过计算对来源于自定义资源查找实现的资源的引用，为任何 XAML 属性提供值。 资源查找由 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 类实现执行。

## <a name="xaml-attribute-usage"></a>XAML 属性使用方法

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>XAML 值

| 术语 | 描述 |
|------|-------------|
| key | 所请求资源的密钥。 键的初始分配方式特定于当前注册使用的 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 类的实现。 |

## <a name="remarks"></a>备注

**CustomResource** 这种技术可获取在自定义资源存储库中的其他地方定义的值。 此技术相对比较高级，大多数 Windows 运行时应用方案都没有使用此技术。

本主题中不介绍 **CustomResource** 解析资源词典的方法，因为解析方法会随 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 实现方法的不同而有很大的不同。

只要在标记中遇到使用 `{CustomResource}` 的情况，就由 Windows 运行时 XAML 分析程序调用 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 实现的 [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 方法。 传递给 **GetResource** 的 *resourceId* 来自 *key* 参数，其他输入参数来自上下文（例如使用情况所应用的属性）。

默认情况下，`{CustomResource}` 使用情况不起作用。（[**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 的基本实现不完整）。 要进行有效的 `{CustomResource}` 引用，必须执行以下每一个步骤：

1.  从 [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) 派生自定义类，并替代 [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) 方法。 不要在实现中调用基类。
2.  设置 [**CustomXamlResourceLoader.Current**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) 以在初始化逻辑中引用你的类。 必须在加载包括 `{CustomResource}` 扩展在内的任何页面级 XAML 之前执行此操作。 设置 **CustomXamlResourceLoader.Current** 的一个位置是，App.xaml 代码隐藏模板中为你生成的 [**Application**](/uwp/api/Windows.UI.Xaml.Application) 子类构造函数。
3.  现在你可以在你的应用加载为页面的 XAML 中，或从 XAML 资源词典内使用 `{CustomResource}` 扩展。

**CustomResource** 是标记扩展。 当要求转义特性值应为非文本值或非处理程序名称时，通常会实现标记扩展，相对于只在某些类型或属性上放置类型转换器而言，此需求更具有全局性。 XAML 中的所有标记扩展在 \{ 其特性语法中使用 "" 和 " \} " 字符，这是 xaml 处理器识别标记扩展必须处理特性的约定。

## <a name="related-topics"></a>相关主题

* [ResourceDictionary 和 XAML 资源引用](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md)
* [**CustomXamlResourceLoader**](/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)