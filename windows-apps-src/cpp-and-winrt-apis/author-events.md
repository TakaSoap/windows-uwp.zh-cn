---
description: 本主题演示了如何创作包含引发事件的运行时类的 Windows 运行时组件。 它还演示了使用该组件并处理事件的应用。
title: 在 C++/WinRT 中创作事件
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 创作, 事件
ms.localizationpriority: medium
ms.openlocfilehash: c70ad8efcb8bb84272a044824d8058813ed30def
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646230"
---
# <a name="author-events-in-cwinrt"></a>在 C++/WinRT 中创作事件

本主题基于 Windows 运行时组件和使用方应用程序（[使用 C++/WinRT 创建 Windows 运行时组件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)主题演示如何生成）。

以下是此主题添加的新功能。
- 更新温度计运行时类以在温度低于冰点时引发事件。
- 更新使用温度计运行时类的核心应用，使其处理该事件。

> [!NOTE]
> 有关安装和使用 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](consume-apis.md) 和[通过 C++/WinRT 创作 API](author-apis.md)。

## <a name="create-thermometerwrc-and-thermometercoreapp"></a>创建 ThermometerWRC 和 ThermometerCoreApp 

如果要按照本主题所示的更新执行操作，以便可以生成和运行代码，则第一步是遵循[使用 C++/WinRT 创建 Windows 运行时组件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)主题中演练的指示。 这样一来，你将拥有 ThermometerWRC Windows 运行时组件和使用它的 ThermometerCoreApp 核心应用 。

## <a name="update-thermometerwrc-to-raise-an-event"></a>更新 ThermometerWRC 以引发事件

更新 `Thermometer.idl`，使其看起来如下所示。 此示例说明了如何使用单精度浮点数的参数声明委托类型为 [EventHandler](/uwp/api/windows.foundation.eventhandler-1) 的事件。

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
        event Windows.Foundation.EventHandler<Single> TemperatureIsBelowFreezing;
    };
}
```

保存该文件。 项目不会在当前状态下完成生成，但在所有情况下立即执行生成可生成 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` 和 `Thermometer.cpp` 存根文件的更新版本。 现在，可在这些文件中看到 TemperatureIsBelowFreezing 事件的存根实现。 在 C++/WinRT 中，IDL 声明事件作为一组重载函数实现（类似于属性作为重载 get 和 set 函数实现的方式）。 一个重载需要注册一个委托，并返回令牌 ([winrt::event_token](/uwp/cpp-ref-for-winrt/event-token))。 另一个重载则需要令牌，并撤销关联代理的注册。

现在，打开 `Thermometer.h` 和 `Thermometer.cpp`，并更新 Thermometer 运行时类的实现。 在 `Thermometer.h` 中，添加两个重载的 TemperatureIsBelowFreezing 函数，以及用于实现这些函数的专用事件数据成员。

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...
        winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler);
        void TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept;

    private:
        winrt::event<Windows::Foundation::EventHandler<float>> m_temperatureIsBelowFreezingEvent;
        ...
    };
}
...
```

如上所示，事件由 [winrt::event](/uwp/cpp-ref-for-winrt/event) 结构模板表示，该结构模板由特定委托类型（其本身可以由 args 类型进行参数化）进行参数化。

在 `Thermometer.cpp` 中，实现两个重载的 TemperatureIsBelowFreezing 函数。

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<float> const& handler)
    {
        return m_temperatureIsBelowFreezingEvent.add(handler);
    }

    void Thermometer::TemperatureIsBelowFreezing(winrt::event_token const& token) noexcept
    {
        m_temperatureIsBelowFreezingEvent.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f) m_temperatureIsBelowFreezingEvent(*this, m_temperatureFahrenheit);
    }
}
```

> [!NOTE]
> 若要详细了解事件自动撤销程序是什么，请参阅[撤销已注册的委托](handle-events.md#revoke-a-registered-delegate)。 可以免费获得适用于你的事件的事件自动撤销程序实现。 换而言之，你无需为事件撤销程序实现重载&mdash;此功能已由 C++/WinRT 投影提供。

其他重载（注册和手动撤销重载）不会合并到投影中。 这是为了让你能够灵活地以最佳方式针对自己的方案来实现它们。 像这些实现中所示那样调用 [**event::add**](/uwp/cpp-ref-for-winrt/event#eventadd-function) 和 [**event::remove**](/uwp/cpp-ref-for-winrt/event#eventremove-function) 是高效且并发/线程安全的默认设置。 但是，如果你有大量事件，那么你可能不希望每个事件都有一个事件字段，而是改为选择某些类型的稀疏实现。

你还可以在上方看到，如果温度低于冰点，AdjustTemperature 函数的实现已更新为引发 TemperatureIsBelowFreezing 事件 。

## <a name="update-thermometercoreapp-to-handle-the-event"></a>更新 ThermometerCoreApp 以处理事件

在 ThermometerCoreApp 项目的 `App.cpp` 中，对代码进行以下更改以注册事件处理程序，然后使温度降至冰点以下。

`WINRT_ASSERT` 是宏定义，并且扩展到 [_ASSERTE](/cpp/c-runtime-library/reference/assert-asserte-assert-expr-macros)。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto &, float temperatureFahrenheit)
        {
            WINRT_ASSERT(temperatureFahrenheit < 32.f); // Put a breakpoint here.
        });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.TemperatureIsBelowFreezing(m_eventToken);
    }
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

请注意对 OnPointerPressed 方法的更改。 现在，每次单击窗口时，都会从温度计的温度中减去 1 华氏度。 现在，该应用正在处理温度低于冰点时引发的事件。 要演示按预期引发事件，请在用于处理 TemperatureIsBelowFreezing 事件的 lambda 表达式内部放置一个断点，运行该应用，然后在窗口内部单击。

## <a name="parameterized-delegates-across-an-abi"></a>跨 ABI 的参数化委托

如果事件必须跨应用程序二进制接口 (ABI) 可访问（例如在组件及其所用应用程序之间），那么该事件必须使用 Windows 运行时委托类型。 上述示例使用 [Windows::Foundation::EventHandler\<T\>](/uwp/api/windows.foundation.eventhandler) Windows 运行时委托类型。 [TypedEventHandler\<TSender, TResult\>](/uwp/api/windows.foundation.eventhandler) 是另一种 Windows 运行时委托类型。

这两种委托类型的类型参数必须跨 ABI，因此类型参数也必须是 Windows 运行时类型。 这包括 Windows 运行时类、第三方运行时类，以及数字和字符串等基本类型。 如果你忘记了此约束，编译器将帮助你处理“必须为 WinRT 类型”错误。

下面是代码列表形式的示例。 从在本主题前面部分创建的 ThermometerWRC 和 ThermometerCoreApp 项目开始，然后编辑这些项目中的代码，使它们看起来与这些列表中的代码相似 。

第一个列表针对 ThermometerWRC 项目。 按如下所示编辑 `ThermometerWRC.idl` 后，生成项目，然后将 `MyEventArgs.h` 和 `.cpp` 复制到项目中（从 `Generated Files` 文件夹），就像之前对 `Thermometer.h` 和 `.cpp` 一样。

```cppwinrt
// ThermometerWRC.idl
namespace ThermometerWRC
{
    [default_interface]
    runtimeclass MyEventArgs
    {
        Single TemperatureFahrenheit{ get; };
    }

    [default_interface]
    runtimeclass Thermometer
    {
        ...
        event Windows.Foundation.EventHandler<ThermometerWRC.MyEventArgs> TemperatureIsBelowFreezing;
        ...
    };
}

// MyEventArgs.h
#pragma once
#include "MyEventArgs.g.h"

namespace winrt::ThermometerWRC::implementation
{
    struct MyEventArgs : MyEventArgsT<MyEventArgs>
    {
        MyEventArgs() = default;
        MyEventArgs(float temperatureFahrenheit);
        float TemperatureFahrenheit();

    private:
        float m_temperatureFahrenheit{ 0.f };
    };
}

// MyEventArgs.cpp
#include "pch.h"
#include "MyEventArgs.h"
#include "MyEventArgs.g.cpp"

namespace winrt::ThermometerWRC::implementation
{
    MyEventArgs::MyEventArgs(float temperatureFahrenheit) : m_temperatureFahrenheit(temperatureFahrenheit)
    {
    }

    float MyEventArgs::TemperatureFahrenheit()
    {
        return m_temperatureFahrenheit;
    }
}

// Thermometer.h
...
struct Thermometer : ThermometerT<Thermometer>
{
...
    winrt::event_token TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler);
...
private:
    winrt::event<Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs>> m_temperatureIsBelowFreezingEvent;
...
}
...

// Thermometer.cpp
#include "MyEventArgs.h"
...
winrt::event_token Thermometer::TemperatureIsBelowFreezing(Windows::Foundation::EventHandler<ThermometerWRC::MyEventArgs> const& handler) { ... }
...
void Thermometer::AdjustTemperature(float deltaFahrenheit)
{
    m_temperatureFahrenheit += deltaFahrenheit;

    if (m_temperatureFahrenheit < 32.f)
    {
        auto args = winrt::make_self<winrt::ThermometerWRC::implementation::MyEventArgs>(m_temperatureFahrenheit);
        m_temperatureIsBelowFreezingEvent(*this, *args);
    }
}
...
```

此列表针对 ThermometerCoreApp 项目。

```cppwinrt
// App.cpp
...
void Initialize(CoreApplicationView const&)
{
    m_eventToken = m_thermometer.TemperatureIsBelowFreezing([](const auto&, ThermometerWRC::MyEventArgs args)
    {
        float degrees = args.TemperatureFahrenheit();
        WINRT_ASSERT(degrees < 32.f); // Put a breakpoint here.
    });
}
...
```

## <a name="simple-signals-across-an-abi"></a>跨 ABI 的简单信号

如果无需连同事件传递任何形参或实参，则可以定义自己的简单 Windows 运行时委托类型。 以下示例展示 Thermometer 运行时类的更简易版本。 它声明名为 SignalDelegate 的委托类型，然后使用该类型来引发信号类型事件，而不是具有参数的事件。

```idl
// ThermometerWRC.idl
namespace ThermometerWRC
{
    delegate void SignalDelegate();

    runtimeclass Thermometer
    {
        Thermometer();
        event ThermometerWRC.SignalDelegate SignalTemperatureIsBelowFreezing;
        void AdjustTemperature(Single value);
    };
}
```

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

        winrt::event_token SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler);
        void SignalTemperatureIsBelowFreezing(winrt::event_token const& token);
        void AdjustTemperature(float deltaFahrenheit);

    private:
        winrt::event<ThermometerWRC::SignalDelegate> m_signal;
        float m_temperatureFahrenheit{ 0.f };
    };
}
```

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    winrt::event_token Thermometer::SignalTemperatureIsBelowFreezing(ThermometerWRC::SignalDelegate const& handler)
    {
        return m_signal.add(handler);
    }

    void Thermometer::SignalTemperatureIsBelowFreezing(winrt::event_token const& token)
    {
        m_signal.remove(token);
    }

    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
        if (m_temperatureFahrenheit < 32.f)
        {
            m_signal();
        }
    }
}
```

```cppwinrt
// App.cpp
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    winrt::event_token m_eventToken;
    ...
    
    void Initialize(CoreApplicationView const &)
    {
        m_eventToken = m_thermometer.SignalTemperatureIsBelowFreezing([] { /* ... */ });
    }
    ...

    void Uninitialize()
    {
        m_thermometer.SignalTemperatureIsBelowFreezing(m_eventToken);
    }
    ...

    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(-1.f);
        ...
    }
    ...
};
```

## <a name="parameterized-delegates-simple-signals-and-callbacks-within-a-project"></a>项目中的参数化委托、简单信号和回调

如果所需事件是 Visual Studio 项目内部的（未跨二进制文件），而在内部这些事件不限于 Windows 运行时类型，则仍可使用 [winrt::event](/uwp/cpp-ref-for-winrt/event)\<Delegate\> 类模板。 请直接使用 [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) 而不是实际的 Windows 运行时委托类型，因为 **winrt::delegate** 也支持非 Windows 运行时参数。

以下示例先显示不采用任何参数的委托签名（本质上即简单信号），然后显示采用字符串的委托签名。

```cppwinrt
winrt::event<winrt::delegate<>> signal;
signal.add([] { std::wcout << L"Hello, "; });
signal.add([] { std::wcout << L"World!" << std::endl; });
signal();

winrt::event<winrt::delegate<std::wstring>> log;
log.add([](std::wstring const& message) { std::wcout << message.c_str() << std::endl; });
log.add([](std::wstring const& message) { Persist(message); });
log(L"Hello, World!");
```

注意如何向事件添加尽可能多的订阅委托。 但会产生一些与事件相关的开销。 如果只需仅具有一个订阅委托的简单回调，则你可以独立使用 [winrt::delegate&lt;…T&gt;](/uwp/cpp-ref-for-winrt/delegate)。

```cppwinrt
winrt::delegate<> signalCallback;
signalCallback = [] { std::wcout << L"Hello, World!" << std::endl; };
signalCallback();

winrt::delegate<std::wstring> logCallback;
logCallback = [](std::wstring const& message) { std::wcout << message.c_str() << std::endl; }f;
logCallback(L"Hello, World!");
```

如果你正在从项目内部使用事件和代理的 C++/CX 基本代码移植，winrt::delegate 将帮助你复制 C++/WinRT 中的这个模式。

## <a name="design-guidelines"></a>设计指南

建议将事件而不是委托作为函数参数传递。 [winrt::event](/uwp/cpp-ref-for-winrt/event) 的 add 函数是例外，因为在这种情况下必须传递委托 。 这条指导原则的原因是，委托在各种 Windows 运行时语中可以具有各种形式（根据支持一个还是多个客户端注册）。 事件及其多个订阅者模型构成更可预测且更一致的选项。

事件处理程序委托的签名应该由以下两个形参组成：发送方 (IInspectable) 和实参（[RoutedEventArgs](/uwp/api/windows.ui.xaml.routedeventargs) 等事件实参类型）。

注意，如果设计的是内部 API，这些指导原则不一定适用。 但随时间推移，内部 API 通常会公开。

## <a name="related-topics"></a>相关主题
* [使用 C++/WinRT 创作 API](author-apis.md)
* [通过 C++/WinRT 使用 API](consume-apis.md)
* [在 C++/WinRT 中使用委托处理事件](handle-events.md)
* [使用 C++/WinRT 创建 Windows 运行时组件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)