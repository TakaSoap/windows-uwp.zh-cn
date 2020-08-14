---
description: C++/WinRT 是适用于 Windows 运行时 (WinRT) API 的完全标准的现代 C++17 语言投影，以基于标头文件的库的形式实现。
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影
ms.localizationpriority: medium
ms.openlocfilehash: 986ba55404ed934960041a0eb720dbe88316e8b7
ms.sourcegitcommit: a9f44bbb23f0bc3ceade3af7781d012b9d6e5c9a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/13/2020
ms.locfileid: "88180782"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 是 Windows 运行时 (WinRT) API 的完全标准新式 C++17 语言投影，以基于标头文件的库的形式实现，旨在为你提供对新式 Windows API 的一流访问。 利用 C++/WinRT，你可以采用任何符合标准的 C++17 编译器创作和使用 Windows 运行时 API。 Windows SDK 包含 C++/WinRT；它已在版本 10.0.17134.0（Windows 10，版本 1803）中引用。

C++/WinRT 适合有兴趣编写适用于 Windows 的美观、快速的代码的任何开发人员。 原因如下。

## <a name="the-case-for-cwinrt"></a>针对 C++/WinRT 的案例
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

C++ 编程语言适用于企业*和*独立软件供应商 (ISV) 行业中重视高水平的正确性、质量和性能的应用场合。 例如：系统编程；资源受限的嵌入式和移动系统；游戏和图形；设备驱动程序；以及工业、科学和医疗应用，等等。

从语言的角度来说，C++ 一直专注于创作和使用类型丰富且轻量的抽象。 但是，由于原始的指针、原始的循环、耗时耗力的内存分配以及 C++98 的发布，该语言已经发生了根本性的改变。 新式 C++（从 C++11 起）可以清楚地表达想法，具有简便性和可读性并且引入 bug 可能性低得多。

对于采用 C++ 编写和使用 Windows 运行时 API，有 C++/WINRT 可用。 这是 Microsoft 推荐的用于替代 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) 语言投影和 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live) 的替代品。

当采用 C++/WinRT 时，你将使用标准 C++ 数据类型、算法和关键字。 投影确实有自己的自定义数据类型，但在大多数情况下，你无需了解它们，因为它们将提供到/自标准类型的相应转换。 这样，你就可以继续使用已用惯的标准 C++ 语言功能和已拥有的源代码。 C++/WinRT 使得在任何 C++ 应用程序（从 Win32 到 UWP）中调用 Windows 运行时 API 变得轻而易举。

与适用于 Windows 运行时的任何其他语言选择相比，C++/WinRT 的表现更好，生成的二进制文件更小。 它的表现甚至超过了直接使用 ABI 接口手动编写的代码。 这是因为抽象使用了 Visual C++ 编译器能够优化的新式 C++ 习惯用语。 这包括神奇静态变量、空基类、**strlen** 删除以及最新版本的 Visual C++ 中的很多专门用于改善 C++/WinRT 的性能的更新的优化。

可以通过多种方式将 C++/WinRT 逐步引入项目。 可以使用 [Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)，也可以通过 C++/CX 进行互操作。 有关详细信息，请参阅[实现 C++/WinRT 与 C++/CX 之间的互操作](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)。

要详细了解如何移动到 C++/WinRT，请参阅以下资源。

- [从 C++/CX 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx)
- [从 WRL 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-wrl)
- [从 C# 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp)

### <a name="topics-about-cwinrt"></a>有关 C++/WinRT 的主题

| 主题 | 说明 |
| - | - |
| [C++/WinRT 简介](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) | 对 C++/WinRT（一种适用于 Windows 运行时 API 的标准 C++ 语言投影）的介绍。 |
| [C++/WinRT 入门](/windows/uwp/cpp-and-winrt-apis/get-started) | 为了帮助你更快地开始使用 C++/WinRT，本主题详细介绍了一个简单的代码示例。 |
| [C++/WinRT 中的新增功能](/windows/uwp/cpp-and-winrt-apis/news) | C++/WinRT 的新增功能和更改。 |
| [常见问题解答](/windows/uwp/cpp-and-winrt-apis/faq) | 对你可能存疑的关于通过 C++/WinRT 创作和使用 Windows 运行时 API 的问题的解答。 |
| [故障排除](/windows/uwp/cpp-and-winrt-apis/troubleshooting) | 无论你是要削减新代码还是要移植现有应用，本主题中的症状排查和补救措施表都可能对你有帮助。 |
| [照片编辑器 C++/WinRT 示例应用程序](/windows/uwp/cpp-and-winrt-apis/photo-editor-sample) | 照片编辑器是一个 UWP 示例应用程序，其通过 C++/WinRT 语言投影展示开发。 此示例应用程序允许你从**图片**库检索照片，然后使用分类的照片效果编辑选择的图像。 | 
| [字符串处理](/windows/uwp/cpp-and-winrt-apis/strings) | 利用 C++/WinRT，你可以使用标准 C++ 宽字符串类型来调用 Windows 运行时 API，也可以使用 [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) 类型。 |
| [标准 C++ 数据类型和 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types) | 利用 C++/WinRT，你可以使用标准 C++ 数据类型调用 Windows 运行时 API。 |
| [将标量值装箱到 IInspectable 和从 IInspectable 取消标量值装箱](/windows/uwp/cpp-and-winrt-apis/boxing) | 标量值需要先封装到引用类对象内，然后再传递到需要 **IInspectable** 的函数。 该封装过程称为对值进行*装箱*。 |
| [通过 C++/WinRT 使用 API](/windows/uwp/cpp-and-winrt-apis/consume-apis) | 本主题介绍了如何使用 C++/WinRT API，无论它们是由 Windows、第三方组件供应商还是由你自己实现的。 |
| [使用 C++/WinRT 创作 API](/windows/uwp/cpp-and-winrt-apis/author-apis) | 本主题展示了如何直接或间接使用 **winrt::implements** 基结构来创作 C++/WinRT API。 |
| [C++/WinRT 的错误处理](/windows/uwp/cpp-and-winrt-apis/error-handling) | 本主题讨论了处理使用 C++/WinRT 编程时出现的错误的策略。 |
| [使用代理处理事件](/windows/uwp/cpp-and-winrt-apis/handle-events) | 本主题介绍了如何使用 C++/WinRT 注册和撤销事件处理委托。 |
| [创作事件](/windows/uwp/cpp-and-winrt-apis/author-events) | 本主题演示了如何创作包含引发事件的运行时类的 Windows 运行时组件。 它还演示了使用该组件并处理事件的应用。 |
| [使用 C++/WinRT 的集合](/windows/uwp/cpp-and-winrt-apis/collections) | C++/WinRT 提供了函数和基类，当你希望实现并/或传递集合时，它们可以为你节省大量的时间和精力。 |
| [并发和异步操作](/windows/uwp/cpp-and-winrt-apis/concurrency) | 本主题介绍了你可通过 C++/WinRT 创建和使用 Windows 运行时异步对象的方式。 |
| [更高级的并发和异步](/windows/uwp/cpp-and-winrt-apis/concurrency-2) | 使用 C++/WinRT 的具有并发性和异步性的更高级方案。 |
| [XAML 控件; 绑定到 C++/WinRT 属性](/windows/uwp/cpp-and-winrt-apis/binding-property) | 可有效地绑定到 XAML 项目控件的属性称为*可观测*属性。 本主题介绍了如何实现和使用可观测属性以及如何将 XAML 控件绑定到该属性。 |
| [XAML 项目控件; 绑定到 C++/WinRT 集合](/windows/uwp/cpp-and-winrt-apis/binding-collection) | 可有效地绑定到 XAML 项目控件的集合称为*可观测*集合。 本主题介绍了如何实现和使用可观测集合以及如何将 XAML 项目控件绑定到该集合。 |
| [XAML 自定义（模板化）控件与 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl) | 本主题引导你完成使用 C++/WinRT 创建简单的自定义控件的步骤。 你可以基于此处的信息创建你自己的功能丰富且可自定义的 UI 控件。 |
| [将参数传递到 ABI 边界](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi) | C++/ WinRT 通过提供针对常见情况的自动转换，简化了将参数传递到 ABI 边界。 |
| [通过 C++/WinRT 使用 COM 组件](/windows/uwp/cpp-and-winrt-apis/consume-com) | 本主题通过一个完整的 Direct2D 代码示例展示了如何通过 C++/WinRT 来使用 COM 类和接口。 |
| [通过 C++/WinRT 创作 COM 组件](/windows/uwp/cpp-and-winrt-apis/author-coclasses) | C++/WinRT 可以帮助你创作经典 COM 组件，就像它可以帮助你创作 Windows 运行时类一样。 |
| [从 C++/CX 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-cx) | 本主题介绍将 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节。 |
| [实现 C++/WinRT 与 C++/CX 之间的互操作](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx) | 本主题介绍了可用于在 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 对象之间进行转换的两个帮助程序函数。 |
| [实现 C++/WinRT 与 C++/CX 之间的异步和互操作](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx-async) | 这是与从 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 逐步移动到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 相关的高级主题。 它显示并行模式库 (PPL) 任务和协同例程如何在同一项目中并行存在。 |
| [从 WRL 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-wrl) | 本主题介绍了如何将 [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) 代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 中的等效项。 |
| [将 Clipboard 示例从 C# 移植到C++&mdash;案例研究](/windows/uwp/cpp-and-winrt-apis/clipboard-to-winrt-from-csharp) | 本主题提供了一个有关将[通用 Windows 平台 (UWP) 应用示例](https://github.com/microsoft/Windows-universal-samples)之一从 [C#](/visualstudio/get-started/csharp) 移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 的案例研究。 可以通过按照本演练并自行移植示例，获取移植实践和体验。 |
| [从 C# 移动到 C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-csharp) | 本主题对 [C#](/visualstudio/get-started/csharp) 项目中的源代码移植到 [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) 项目中的等效项时所涉及的技术细节进行了全面分类。 |
| [实现 C++/WinRT 与 ABI 之间的互操作](/windows/uwp/cpp-and-winrt-apis/interop-winrt-abi) | 本主题介绍了如何在应用程序二进制接口 (ABI) 和 C++/WinRT 对象之间进行转换。 |
| [C++/WinRT 中的弱引用](/windows/uwp/cpp-and-winrt-apis/weak-references) | Windows 运行时是引用在其中占有重要地位的一个系统；在这样的系统中，了解强引用与弱引用的意义和区别非常重要。 |
| [敏捷对象](/windows/uwp/cpp-and-winrt-apis/agile-objects) | 敏捷对象是可从任何线程访问的对象。 C++/WinRT 类型默认情况下是敏捷对象，但你可以选择退出。 |
| [诊断直接分配](/windows/uwp/cpp-and-winrt-apis/diag-direct-alloc) | 本主题深入探讨了一种 C++/WinRT 2.0 功能，该功能可帮助你诊断在堆栈上创建实现类型对象的错误，而无需使用 [**WinRT:: make**](/uwp/cpp-ref-for-winrt/make) 系列的帮助程序。 |
| [用于实现类型的扩展点](/windows/uwp/cpp-and-winrt-apis/details-about-destructors) | 使用 C++/WinRT 2.0 中的这些扩展点可以延迟实现类型的析构、在析构期间安全地查询，以及与投影方法的入口和出口挂钩。 |
| [一个简单的 C++WinRT Windows UI 库示例](/windows/uwp/cpp-and-winrt-apis/simple-winui-example) | 本主题将指导你完成在 C++/WinRT 项目内添加对 WinUI 的简单支持的过程。 |
| [使用 C++/WinRT 创建 Windows 运行时组件](/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt) | 本主题展示了如何使用 C++/WinRT 创建和使用 Windows 运行时组件 &mdash; 一种可以从使用任何 Windows 运行时语言构建的通用 Windows 应用调用的组件。 |

### <a name="topics-about-the-c-language"></a>有关 C++ 语言的主题

| 主题 | 说明 |
| - | - |
| [值类别以及对它们的引用](/windows/uwp/cpp-and-winrt-apis/cpp-value-categories) | 本主题介绍了 C++ 中存在的各种值类别。 你肯定听说过左值和右值，但还有其他类型。 |

## <a name="important-apis"></a>重要的 API
* [winrt 命名空间](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Windows 运行时 C++ 模板库 (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
