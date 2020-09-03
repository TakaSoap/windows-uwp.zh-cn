---
description: 本主题介绍了可用于在 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 和 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 对象之间进行转换的两个帮助程序函数。
title: 实现 C++/WinRT 与 C++/CX 之间的互操作
ms.date: 10/09/2018
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 端口, 迁移, 互操作, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 8ef3b45222b5e9324dc76d7a81a8d096a569595d
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157371"
---
# <a name="interop-between-cwinrt-and-ccx"></a>实现 C++/WinRT 与 C++/CX 之间的互操作

在阅读本主题之前，你需要先了解[从 C++/CX 移动到 C++/WinRT](./move-to-winrt-from-cx.md) 主题中的信息。 该主题介绍了将 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 项目移植到 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 的两个主要策略选项。

- 一次性移植整个项目。 对于不太大的项目，这是最简单的选项。 如果你有 Windows 运行时组件项目，则此策略是你唯一的选项。
- 逐步移植项目（代码库的大小或复杂性可能会使其成为一种必需策略）。 但是，此策略要求你遵循移植过程，在此过程中的某段时间，C++/CX 和 C++/WinRT 代码将在同一项目中并存。 对于 XAML 项目，无论何时，均要求 XAML 页面类型要么完全是 C++/WinRT，要么完全是 C++/CX。

本互操作主题适用于第二种策略，即，需要逐步移植项目的情况&mdash;。 本主题介绍了可用于在同一项目中将 C++/CX 对象转换为 C++/WinRT 对象（反之亦然）的两个帮助程序函数。

将代码从 C++/CX 逐步移植到 C++/WinRT 时，这些帮助程序函数将非常有用。 或者，你可以选择在同一项目中同时使用 C++/WinRT 和 C++/CX 语言投影（无论是否进行移植），并使用这些帮助程序函数在两者之间进行互操作。

阅读完本主题之后，若要了解演示如何在同一项目中并行支持 PPL 任务和协同程序（例如，从任务链调用协同程序）的信息和代码示例，请参阅更高级的主题[实现 C++/WinRT 与 C++/CX 之间的异步和互操作](./interop-winrt-cx-async.md)。

## <a name="the-from_cx-and-to_cx-functions"></a>from_cx 和 to_cx 函数

下面是名为 `interop_helpers.h` 的头文件的源代码列表，其中包含两个转换帮助程序函数。 在逐步移植项目时，项目的一部分内容仍位于 C++/CX 中，一部分内容已移植到 C++/WinRT。 可以使用这些帮助程序函数在这两个部分之间的边界点在 C++/CX 和 C++/WinRT 之间来回转换项目中的对象。

代码列表后面的部分介绍了这两个函数，以及如何在项目中创建和使用头文件。

```cppwinrt
// interop_helpers.h
#pragma once

template <typename T>
T from_cx(Platform::Object^ from)
{
    T to{ nullptr };

    winrt::check_hresult(reinterpret_cast<::IUnknown*>(from)
        ->QueryInterface(winrt::guid_of<T>(), winrt::put_abi(to)));

    return to;
}

template <typename T>
T^ to_cx(winrt::Windows::Foundation::IUnknown const& from)
{
    return safe_cast<T^>(reinterpret_cast<Platform::Object^>(winrt::get_abi(from)));
}
```

### <a name="the-from_cx-function"></a>from_cx 函数

from_cx 帮助程序函数可将 C++/CX 对象转换为等效的 C++/WinRT 对象。 该函数将 C++/CX 对象强制转换为其基础 [IUnknown****](/windows/win32/api/unknwn/nn-unknwn-iunknown) 接口指针。 然后，它对该指针调用 [QueryInterface****](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)) 来查询 C++/WinRT 对象的默认接口。 QueryInterface 是 C++/CX `safe_cast` 扩展的 Windows 运行时应用程序二进制接口 (ABI) 等效项。 此外，[winrt::put_abi****](/uwp/cpp-ref-for-winrt/put-abi) 函数将检索 C++/WinRT 对象的基础 IUnknown**** 接口指针的地址，使该地址能够设置为其他值。

### <a name="the-to_cx-function"></a>to_cx 函数

to_cx 帮助程序函数可将 C++/WinRT 对象转换为等效的 C++/CX 对象。 [winrt::get_abi****](/uwp/cpp-ref-for-winrt/get-abi) 函数将检索指向 C++/WinRT 对象的基础 IUnknown**** 接口的指针。 在使用 C++/CX `safe_cast` 扩展查询请求的 C++/CX 类型之前，此函数会将该指针强制转换为 C++/CX 对象。

### <a name="the-interop_helpersh-header-file"></a>`interop_helpers.h` 头文件

若要在项目中使用这两个帮助程序函数，请执行以下步骤。

- 向项目添加一个新的头文件 (.h) 项，并将其命名为 `interop_helpers.h`。
- 将 `interop_helpers.h` 中的内容替换为上面列出的代码。
- 将这些包含内容添加到 `pch.h`。

```cppwinrt
// pch.h
...
#include <unknwn.h>
// Include C++/WinRT projected Windows API headers here.
...
#include <interop_helpers.h>
```

## <a name="taking-a-ccx-project-and-adding-cwinrt-support"></a>采用 C++/CX 项目并添加 C++/WinRT 支持

本部分介绍在你决定采用现有 C++/CX 项目、向其添加 C++/WinRT 支持并在该项目中进行移植工作时要执行的操作。 另请参阅 [Visual Studio 对 C++/WinRT 的支持](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

若要在 C++/CX 项目中混合使用 C++/CX 和 C++/WinRT，包括在项目中使用 from_cx 和 to_cx 帮助程序函数，需要手动向项目添加 C++/WinRT 支持&mdash;&mdash;。

首先，在 Visual Studio 中打开 C++/CX 项目，确保项目属性“常规”\>“目标平台版本”设置为 10.0.17134.0（Windows 10 版本 1803）或更高版本。

### <a name="install-the-cwinrt-nuget-package"></a>安装 C++/WinRT NuGet 程序包

[Microsoft.Windows.CppWinRT NuGet 程序包](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/)提供 C++/WinRT 生成支持（MSBuild 属性和目标）。 若要安装它，请单击菜单项“项目”\>“管理 NuGet 程序包...”\>“浏览”，在搜索框中键入或粘贴 Microsoft.Windows.CppWinRT，在搜索结果中选择该项，然后单击“安装”以安装该项目的包。 

> [!IMPORTANT]
> 安装 C++/WinRT NuGet 程序包会导致在项目中禁用对 C++/CX 的支持。 如果你打算进行一次性移植，最好使该支持保持禁用状态，这样生成消息便可帮助你查找（和移植）C++/CX 上的所有依赖项（最终将纯 C++/CX 项目转换为纯 C++/WinRT 项目）。 但是，若要了解有关如何重新启用该支持的信息，请参阅下一部分。

### <a name="turn-ccx-support-back-on"></a>重新启用 C++/CX 支持

如果你要进行一次性移植，则无需执行此操作。 但是，如果需要进行逐步移植，则此时需要在项目中重新启用 C++/CX 支持。 在项目属性中，“C/C++”\>“常规”\>“使用 Windows 运行时扩展”\>“是(/ZW)”。

或者（例如 XAML 项目），可以在 Visual Studio 中使用 C++/WinRT 项目属性页来添加 C++/CX 支持。 在项目属性中，“公共属性”\>“C++/WinRT”\>“项目语言”\>“C++/CX”。 这样做会将以下属性添加到 `.vcxproj` 文件中。

```xml
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
```

> [!IMPORTANT]
> 每当你需要通过生成过程将 Midl 文件 (.idl) 的内容处理为存根文件时，都需要将“项目语言”改回为“C++/WinRT”  。 在生成过程生成这些存根之后，将“项目语言”改回为“C++/CX”。

如需类似自定义选项（用于优化 `cppwinrt.exe` 工具的行为）的列表，请参阅 Microsoft.Windows.CppWinRT NuGet 包[自述文件](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing)。

### <a name="include-cwinrt-header-files"></a>包含 C++/WinRT 头文件

至少应在预编译的头文件（通常为 `pch.h`）中包含 `winrt/base.h`，如下所示。

```cppwinrt
// pch.h
...
#include <winrt/base.h>
...
```

但是，几乎可以肯定的是，你需要 winrt::Windows::Foundation 命名空间中的类型。 你可能已经知道所需的其他命名空间。 因此，请包含与此类命名空间相对应的 C++/WinRT 投影 Windows API 标头（现在无需显式包含 `winrt/base.h`，因为它将自动为你包含在内）。

```cppwinrt
// pch.h
...
#include <winrt/Windows.Foundation.h>
// Include any other C++/WinRT projected Windows API headers here.
...
```

另请参阅下一部分（采用 C++/WinRT 项目并添加 C++/CX 支持）中的代码示例，了解使用命名空间别名 `namespace cx` 和 `namespace winrt` 的方法。 此方法可用于处理 C++/WinRT 投影和 C++/CX 投影之间潜在的命名空间冲突。

### <a name="add-interop_helpersh-to-the-project"></a>将 `interop_helpers.h` 添加到项目中

现在，可以将 from_cx 和 to_cx 函数添加到 C++/CX 项目中。 有关如何执行此操作的说明，请参阅上面的 [from_cx 和 to_cx 函数](#the-from_cx-and-to_cx-functions)部分。

## <a name="taking-a-cwinrt-project-and-adding-ccx-support"></a>采用 C++/WinRT 项目并添加 C++/CX 支持

本部分介绍在你决定创建新的 C++/WinRT 项目并在该项目中进行移植工作时要执行的操作。

若要在 C++/WinRT 项目中混合使用 C++/WinRT 和 C++/CX，包括在项目中使用 from_cx 和 to_cx 帮助程序函数，需要手动向项目添加 C++/CX 支持&mdash;&mdash;。

- 使用 C++/WinRT 项目模板之一在 Visual Studio 中创建一个新的 C++/WinRT 项目（请参阅 [Visual Studio 对 C++/WinRT 的支持](./intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)）。
- 启用对 C++/CX 的项目支持。 在项目属性中，“C/C++”**“常规”**\>**“使用 Windows 运行时扩展”**\>**“是(/ZW)”**\>****。

### <a name="an-example-cwinrt-project-showing-the-two-helper-functions-in-use"></a>演示正在使用的两个帮助程序函数的示例 C++/WinRT 项目

在本部分中，可以创建一个示例 C++/WinRT 项目来演示如何使用 from_cx 和 to_cx。 它还说明如何使用不同代码岛的命名空间别名来处理 C++/WinRT 投影与 C++/CX 投影之间潜在的命名空间冲突。

- 创建“Visual C++”**“Windows 通用”**\>**“核心应用(C++/WinRT)”**\>**** 项目。
- 在项目属性中，“C/C++”**“常规”** \> **“使用 Windows 运行时扩展”** \> **“是(/ZW)”** \> ****。
- 将 `interop_helpers.h` 添加到项目。 有关如何执行此操作的说明，请参阅上面的 [from_cx 和 to_cx 函数](#the-from_cx-and-to_cx-functions)部分。
- 将 `App.cpp` 中的内容替换为下面列出的代码，然后生成并运行。

`WINRT_ASSERT` 是宏定义，并且扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
// App.cpp
#include "pch.h"
#include <sstream>

namespace cx
{
    using namespace Windows::Foundation;
}

namespace winrt
{
    using namespace Windows;
    using namespace Windows::ApplicationModel::Core;
    using namespace Windows::Foundation;
    using namespace Windows::Foundation::Numerics;
    using namespace Windows::UI;
    using namespace Windows::UI::Core;
    using namespace Windows::UI::Composition;
}

struct App : winrt::implements<App, winrt::IFrameworkViewSource, winrt::IFrameworkView>
{
    winrt::CompositionTarget m_target{ nullptr };
    winrt::VisualCollection m_visuals{ nullptr };
    winrt::Visual m_selected{ nullptr };
    winrt::float2 m_offset{};

    winrt::IFrameworkView CreateView()
    {
        return *this;
    }

    void Initialize(winrt::CoreApplicationView const &)
    {
    }

    void Load(winrt::hstring const&)
    {
    }

    void Uninitialize()
    {
    }

    void Run()
    {
        winrt::CoreWindow window = winrt::CoreWindow::GetForCurrentThread();
        window.Activate();

        winrt::CoreDispatcher dispatcher = window.Dispatcher();
        dispatcher.ProcessEvents(winrt::CoreProcessEventsOption::ProcessUntilQuit);
    }

    void SetWindow(winrt::CoreWindow const & window)
    {
        winrt::Compositor compositor;
        winrt::ContainerVisual root = compositor.CreateContainerVisual();
        m_target = compositor.CreateTargetForCurrentView();
        m_target.Root(root);
        m_visuals = root.Children();

        window.PointerPressed({ this, &App::OnPointerPressed });
        window.PointerMoved({ this, &App::OnPointerMoved });

        window.PointerReleased([&](auto && ...)
        {
            m_selected = nullptr;
        });
    }

    void OnPointerPressed(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        winrt::float2 const point = args.CurrentPoint().Position();

        for (winrt::Visual visual : m_visuals)
        {
            winrt::float3 const offset = visual.Offset();
            winrt::float2 const size = visual.Size();

            if (point.x >= offset.x &&
                point.x < offset.x + size.x &&
                point.y >= offset.y &&
                point.y < offset.y + size.y)
            {
                m_selected = visual;
                m_offset.x = offset.x - point.x;
                m_offset.y = offset.y - point.y;
            }
        }

        if (m_selected)
        {
            m_visuals.Remove(m_selected);
            m_visuals.InsertAtTop(m_selected);
        }
        else
        {
            AddVisual(point);
        }
    }

    void OnPointerMoved(IInspectable const &, winrt::PointerEventArgs const & args)
    {
        if (m_selected)
        {
            winrt::float2 const point = args.CurrentPoint().Position();

            m_selected.Offset(
            {
                point.x + m_offset.x,
                point.y + m_offset.y,
                0.0f
            });
        }
    }

    void AddVisual(winrt::float2 const point)
    {
        winrt::Compositor compositor = m_visuals.Compositor();
        winrt::SpriteVisual visual = compositor.CreateSpriteVisual();

        static winrt::Color colors[] =
        {
            { 0xDC, 0x5B, 0x9B, 0xD5 },
            { 0xDC, 0xED, 0x7D, 0x31 },
            { 0xDC, 0x70, 0xAD, 0x47 },
            { 0xDC, 0xFF, 0xC0, 0x00 }
        };

        static unsigned last = 0;
        unsigned const next = ++last % _countof(colors);
        visual.Brush(compositor.CreateColorBrush(colors[next]));

        float const BlockSize = 100.0f;

        visual.Size(
        {
            BlockSize,
            BlockSize
        });

        visual.Offset(
        {
            point.x - BlockSize / 2.0f,
            point.y - BlockSize / 2.0f,
            0.0f,
        });

        m_visuals.InsertAtTop(visual);

        m_selected = visual;
        m_offset.x = -BlockSize / 2.0f;
        m_offset.y = -BlockSize / 2.0f;
    }
};

int __stdcall wWinMain(HINSTANCE, HINSTANCE, PWSTR, int)
{
    winrt::init_apartment();

    winrt::Uri uri(L"http://aka.ms/cppwinrt");
    std::wstringstream wstringstream;
    wstringstream << L"C++/WinRT: " << uri.Domain().c_str() << std::endl;

    // Convert from a C++/WinRT type to a C++/CX type.
    cx::Uri^ cx = to_cx<cx::Uri>(uri);
    wstringstream << L"C++/CX: " << cx->Domain->Data() << std::endl;
    ::OutputDebugString(wstringstream.str().c_str());

    // Convert from a C++/CX type to a C++/WinRT type.
    winrt::Uri uri_from_cx = from_cx<winrt::Uri>(cx);
    WINRT_ASSERT(uri.Domain() == uri_from_cx.Domain());
    WINRT_ASSERT(uri == uri_from_cx);

    winrt::CoreApplication::Run(winrt::make<App>());
}
```

## <a name="important-apis"></a>重要的 API
* [IUnknown 接口](/windows/win32/api/unknwn/nn-unknwn-iunknown)
* [QueryInterface 函数](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))
* [winrt::get_abi 函数](/uwp/cpp-ref-for-winrt/get-abi)
* [winrt::put_abi 函数](/uwp/cpp-ref-for-winrt/put-abi)

## <a name="related-topics"></a>相关主题
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [从 C++/CX 移动到 C++/WinRT](./move-to-winrt-from-cx.md)
* [实现 C++/WinRT 与 C++/CX 之间的异步和互操作](./interop-winrt-cx-async.md)