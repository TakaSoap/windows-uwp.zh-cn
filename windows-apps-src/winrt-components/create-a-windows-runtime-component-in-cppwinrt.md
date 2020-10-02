---
title: 使用 C++/WinRT 的 Windows 运行时组件
description: 本主题演示如何使用 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 创建和使用 Windows 运行时组件 &mdash; ，该组件可从使用任意 Windows 运行时语言构建的通用 Windows 应用程序调用。
ms.date: 07/06/2020
ms.topic: article
keywords: windows 10、uwp、windows、运行时、组件、组件 Windows 运行时组件、WRC、c + +/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646290"
---
# <a name="windows-runtime-components-with-cwinrt"></a>使用 C++/WinRT 的 Windows 运行时组件

本主题演示如何使用 [c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) 创建和使用 Windows 运行时组件 &mdash; ，该组件可从使用任意 Windows 运行时语言构建的通用 Windows 应用程序调用。

用 c + +/WinRT. 生成 Windows 运行时组件有多种原因。
- 在复杂或计算密集型操作中享有 c + + 的性能优势。
- 用于重用已编写和测试的标准 c + + 代码。
- 若要向编写的通用 Windows 平台 (UWP) 应用公开 Win32 功能，例如 c #。

通常，在创作 c + +/WinRT 组件时，可以使用标准 c + + 库中的类型和内置类型，但在应用程序二进制接口 (ABI) 边界，在这种情况下，你要将数据传入和传入其他包中的代码 `.winmd` 。 在 ABI 上，使用 Windows 运行时类型。 此外，在 c + +/WinRT 代码中，使用委托和事件之类的类型来实现可从你的组件引发的事件，并使用另一种语言进行处理。 有关 c + +/WinRT. 的详细信息，请参阅[c + +/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md)

本主题的其余部分将介绍如何在 c + +/WinRT 中创作 Windows 运行时组件，然后从应用程序使用该组件。

本主题中将生成的 Windows 运行时组件包含表示温度计的运行时类。 本主题还演示了一个使用温度计运行时类的核心应用程序，并调用了一个函数来调整温度。

> [!NOTE]
> 有关安装和使用 [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) Visual Studio 扩展 (VSIX) 和 NuGet 包（两者共同提供项目模板，并生成支持）的信息，请参阅[适用于 C++/WinRT 的 Visual Studio 支持](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)。

> [!IMPORTANT]
> 有关支持你了解如何利用 C++/WinRT 来使用和创作运行时类的基本概述和术语，请参阅[通过 C++/WinRT 使用 API](../cpp-and-winrt-apis/consume-apis.md) 和[通过 C++/WinRT 创作 API](../cpp-and-winrt-apis/author-apis.md)。

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>创建 Windows 运行时组件 (ThermometerWRC) 

首先在 Microsoft Visual Studio 中创建新项目。 ** (c + +/WinRT) **项目中创建 Windows 运行时组件，并将其命名为 "温度计 Windows 运行时组件" ) 的*ThermometerWRC* (。 请确保未选中“将解决方案和项目放在同一目录中”。 面向 Windows SDK 的最新正式发布（非预览）版本。 将项目命名为 *ThermometerWRC* 将为你提供本主题中其他步骤的最简单体验。 

暂时不要生成该项目。

该新建项目包含一个名为 `Class.idl` 的文件。 在解决方案资源管理器中，将该文件重命名为 `Thermometer.idl`（重命名 `.idl` 文件还会自动重命名从属的 `.h` 和 `.cpp` 文件）。 将 `Thermometer.idl` 中的内容替换为下表。

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

保存该文件。 此时，该项目不会生成到完成，但现在生成是一项有用的操作，因为它会生成要在其中实现 **温度计** 运行时类的源代码文件。 因此，继续生成（此阶段可能发生的生成错误与找不到 `Class.h` 和 `Class.g.h` 有关）。

在生成过程中，`midl.exe` 工具会运行以创建组件的 Windows 运行时元数据文件（即 `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd`）。 然后，`cppwinrt.exe` 工具运行（具有 `-component` 选项）以生成源代码文件，从而为你在创作组件时提供支持。 这些文件包括存根，可让你开始实现在 IDL 中声明的 **温度计** 运行时类。 这些存根是 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` 和 `Thermometer.cpp`。

右键单击项目节点，然后单击“打开文件资源管理器中的文件夹”。 执行此操作，将在文件资源管理器中打开项目文件夹。 将存根文件 `Thermometer.h` 和 `Thermometer.cpp` 从文件夹 `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` 复制到包含项目文件的文件夹（即 `\ThermometerWRC\ThermometerWRC\`），并替换目标中的文件。 现在，让我们打开 `Thermometer.h` 和 `Thermometer.cpp` 并实现运行时类。 在中 `Thermometer.h` ，将新的私有成员添加到实现， (*不* 是 **温度计**的工厂实现) 。

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

在中 `Thermometer.cpp` ，实现 **AdjustTemperature** 方法，如下列表所示。

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

还需要删除这两个文件中的 `static_assert`。

如果任何警告阻止你进行生成，请处理这些警告或将项目属性“C/C++” > “常规” > “将警告视为错误”设置为“否(/WX-)”，然后重新生成该项目   。

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>创建核心应用 (ThermometerCoreApp) 测试 Windows 运行时组件

现在 (在 *ThermometerWRC* 解决方案中创建新项目，或在新项目) 中创建。 ** (c + +/WinRT) **项目中创建一个核心应用，并将其命名为*ThermometerCoreApp*。 如果这两个项目处于同一个解决方案中，则将 *ThermometerCoreApp* 设置为启动项目。

> [!NOTE]
> 如前文所述，在文件夹中创建了你 Windows 运行时组件的 Windows 运行时元数据文件， (其项目命名为 *ThermometerWRC*) `\ThermometerWRC\Debug\ThermometerWRC\` 。 该路径的第一段是包含解决方案文件的文件夹的名称；下一段是名为 `Debug` 的子目录；最后一段是为 Windows 运行时组件命名的子目录。 如果你未将项目命名为 *ThermometerWRC*，则元数据文件将位于文件夹中 `\<YourProjectName>\Debug\<YourProjectName>\` 。

现在，在核心应用程序项目中 (*ThermometerCoreApp*) ，添加引用，并浏览到 `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` (或添加项目到项目的引用（如果两个项目位于同一解决方案) 中）。 单击“添加”，然后单击“确定” 。 现在生成 *ThermometerCoreApp*。 在极少数情况下，你看到了负载文件 `readme.txt` 不存在的错误，请从 Windows 运行时组件项目中排除该文件，重新生成该文件，然后重新生成 *ThermometerCoreApp*。

生成过程期间,`cppwinrt.exe` 工具会运行以将引用的 `.winmd` 文件处理到包含投影类型的源代码文件中,从而为你在使用组件时提供支持。 组件的运行时类的投影类型的标头&mdash;名为 `ThermometerWRC.h`&mdash;将生成在文件夹 `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\` 中。

`App.cpp` 中包含该标头。

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

此外，在中 `App.cpp` ，添加以下代码以实例化 **温度计** 对象 (使用投影类型的默认构造函数) ，并对温度计对象调用方法。

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

每次单击窗口时，会增加温度计对象的温度。 如果要单步执行代码以确认应用程序确实正在调用 Windows 运行时组件，则可以设置断点。

## <a name="next-steps"></a>后续步骤

若要将更多功能或新 Windows 运行时类型添加到 c + +/WinRT Windows 运行时组件，可以遵循上面所示的相同模式。 首先，使用 IDL 定义要公开的功能。 然后在 Visual Studio 中生成项目以生成存根实现。 然后，根据需要完成实现。 IDL 中定义的任何方法、属性和事件都对使用 Windows 运行时组件的应用程序可见。 有关 IDL 的详细信息，请参阅 [Microsoft 接口定义语言3.0 的简介](/uwp/midl-3/intro)。

有关如何向 Windows 运行时组件添加事件的示例，请参阅 [在 c + + 中创作事件/WinRT](../cpp-and-winrt-apis/author-events.md)。

## <a name="troubleshooting"></a>疑难解答

| 症状 | 纠正方法 |
|---------|--------|
|在 C++/WinRT 应用中，当使用利用 XAML 的 [C# Windows 运行时组件](./creating-windows-runtime-components-in-csharp-and-visual-basic.md)时，编译器会生成一个错误，格式为："'MyNamespace_XamlTypeInfo': 不是 'winrt::MyNamespace' 的成员"&mdash;其中 MyNamespace 是 Windows 运行时组件命名空间的名称。 | 在 C++/WinRT 应用中的 `pch.h` 中，根据需要添加 `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>`&mdash; 来替换 MyNamespace。 |
