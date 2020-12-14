---
description: C#/WinRT 是一个为 C# 代码提供 WinRT 投影支持的工具集。
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, 标准, c#, winrt, cswinrt, 投影
ms.localizationpriority: medium
ms.openlocfilehash: ef6fad694dd45e80d462f6a0c5c73ac5539fe16a
ms.sourcegitcommit: c063d0d130944558afa20181dd294ffe7a187a3f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97090681"
---
# <a name="cwinrt"></a>C#/WinRT

C#/WinRT 是一个进行 NuGet 打包的工具包，为 C# 语言提供 Windows 运行时 (WinRT) 投影支持。 “投影”是一个转换层（例如互操作程序集），它以自然而熟悉的方式为目标语言启用编程 WinRT API。 例如，C#/WinRT 投影隐藏了 C# 与 WinRT 接口之间的互操作的详细信息，并提供了从许多 WinRT 类型到相应的 .NET 等效项（例如字符串、URI、公用值类型和泛型集合）的映射。

C#/WinRT 当前支持使用 WinRT 类型，最新版本允许[创建](#create-an-interop-assembly)和[引用](#reference-an-interop-assembly) WinRT 互操作程序集。 将来的 C# /WinRT 版本将支持采用 C# 创作 WinRT 类型。

有关 C#/WinRT 的更多信息，请参阅 [C#/WinRT GitHub 存储库](https://aka.ms/cswinrt/repo)。

## <a name="motivation-for-cwinrt"></a>采用 C#/WinRT 的动机

[.NET Core](/dotnet/core/) 是 .NET 平台的重点，.NET 5 是最新主要版本。 它是一个开源的跨平台运行时，可用于构建设备、云和 IoT 应用程序。

以前版本的 .NET Framework 和 .NET Core 都内置了 WinRT（一种特定于 Windows 的技术）知识。 为了支持 .NET 5 的可移植性和高效性目标，我们从 .NET 编译器和运行时中撤销了 WinRT 投影支持，将其移到了 C#/WinRT 工具包中。 C#/WinRT 的目标是通过旧版 C# 编译器和 .NET 运行时提供的内置 WinRT 支持来提供奇偶校验。 有关详细信息，请参阅 [Windows 运行时类型的 .NET 映射](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)。

C#/WinRT 还支持 WinUI 3.0。 此版本的 WinUI 从操作系统中移除了原生的 Microsoft UI 控件和功能。 这使得应用开发人员能够在 Windows 10 版本 1803 和更高版本上使用最新控件和视觉对象。

最后，C#/WinRT 是一个通用工具包，用于支持在 C# 编译器或 .NET 运行时中无法使用内置 WinRT 支持的其他方案。

## <a name="create-an-interop-assembly"></a>创建一个互操作程序集

WinRT API 在 Windows 元数据 (*.winmd) 文件中定义。 C#/WinRT NuGet 包 ([Microsoft.Windows.CsWinRT](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)) 中包括 C#/WinRT 编译器 cswinrt.exe，可用它来处理 Windows 元数据文件并生成 .NET 5.0 C# 代码。 可以将这些源文件编译为互操作程序集，这与 [C++/WinRT](../cpp-and-winrt-apis/index.md) 为 C++ 语言投影生成头文件的方式类似。 然后，可以将 C#/WinRT 互操作程序集与 C#/WinRT 运行时程序集一起分发，供应用程序引用。

若要在演练中查看如何创建互操作程序集并将其作为 NuGet 包进行分发，请参阅[演练：从 C++/WinRT 组件生成 .NET 5 投影并更新 NuGet](net-projection-from-cppwinrt-component.md)。

### <a name="invoke-cswinrtexe"></a>调用 cswinrt.exe

若要从项目中调用 cswinrt.exe，请安装最新的 [C#/WinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT/)。 然后，可在 C# 库中设置 C#/WinRT 特定的项目属性来生成互操作程序集。 以下项目片段演示了对 **cswinrt** 的简单调用，该调用为 Contoso 命名空间中的类型生成投影源。 然后，这些源会包含在项目生成中。

```xml
<PropertyGroup>
  <CsWinRTIncludes>Contoso</CsWinRTIncludes>
</PropertyGroup>
```

在此项目中，你还需要引用 CsWinRT NuGet 包以及想要投射的项目特定的 .winmd 文件（无论是通过 NuGet 包、项目引用还是直接引用来进行投射）。 默认情况下，不会投射 Windows 和 Microsoft 命名空间 。 有关 CsWinRT 项目属性的完整列表，请参阅 [CsWinRT NuGet 文档](https://github.com/microsoft/CsWinRT/blob/master/nuget/readme.md)。

### <a name="distribute-the-interop-assembly"></a>分发互操作程序集

互操作程序集通常以 NuGet 包的形式分发，并且依赖于必需的 C#/WinRT 运行时程序集 WinRT.Runtime.dll 的 C#/WinRT NuGet 包。

若要确保为 .NET 5.0 应用程序部署了正确版本的 C#/WinRT 运行时，请在依赖 C#/WinRT NuGet 包的 .nuspec 文件中包含一个 `targetFramework` 条件。

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework="net5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="1.0.1" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> .NET 5.0 的目标框架名字对象将从“.NETCoreApp5.0”移到“net5.0”。

## <a name="reference-an-interop-assembly"></a>引用互操作程序集

通常，C#/WinRT 互操作程序集由应用程序项目引用。 但是，中间互操作程序集也可能引用它们。 例如，WinUI 互操作程序集将引用 Windows SDK 互操作程序集。

若要使用投影的 C#/WinRT 类型，请向项目中添加对相应 C#/WinRT 互操作 NuGet 程序包的引用。 这会导致将互操作程序集和 C#/WinRT 运行时程序集都添加到项目中。

如果你分发没有正式互操作程序集的第三方 WinRT 组件，则应用程序项目可能会按照[创建互操作程序集](#create-an-interop-assembly)的过程来生成其自己的专用投影源。 不建议使用此方法，因为它可能会在一个进程内生成同一类型的冲突投影。 设计了遵循[语义版本控制](https://semver.org)方案的 NuGet 打包来防止出现此情况。 最好使用正式的第三方互操作程序集。

### <a name="winrt-type-activation"></a>WinRT 类型激活

C#/WinRT 支持激活由操作系统承载的 WinRT 类型以及第三方组件（例如 [Win2D](https://www.nuget.org/packages/Win2D.uwp/)）。 桌面应用程序中对第三方组件的激活支持是通过[注册免费 WinRT 激活](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/)（在 Windows 10 版本 1903 及更高版本中提供）启用的。 如果组件是面向 UWP 应用构建的，则可能还需要使用 [VCRT 转发器](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/)程序包。

如果 Windows 未能如上所述激活该类型，则可使用 C#/WinRT 提供的激活回退路径。 在这种情况下，C#/WinRT 会尝试根据完全限定的类型名称查找一个原生实现 DLL，并逐步删除各个元素。 例如，回退逻辑会尝试按顺序从以下模块激活 Contoso.Controls.Widget 类型：

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

C#/WinRT 使用 [LoadLibrary 备用搜索顺序](/windows/win32/dlls/dynamic-link-library-search-order#alternate-search-order-for-desktop-applications)查找一个实现 DLL。 依赖于此回退行为的应用应将该实现 DLL 与应用模块一起打包。

## <a name="common-errors-with-net-5"></a>.NET 5+ 的常见错误

在使用版本低于其任何依赖项的 .NET SDK 构建的项目中，你可能会遇到以下错误或警告。

| 错误或警告消息 | 原因 |
|--------------------------|--------|
| 警告 MSB3277：发现不同版本的 WinRT.Runtime 或 Microsoft.Windows.SDK.NET 之间存在无法解决的冲突。 | 引用在其 API 表面公开 Windows SDK 类型的库时，会出现此生成警告。 |
| [错误 CS1705](/dotnet/csharp/language-reference/compiler-messages/cs1705)：程序集“AssemblyName1”使用“TypeName”，后者的版本比引用的程序集“AssemblyName2”的版本高 | 引用和使用已在库中公开的 Windows SDK 类型时，会出现此生成编译器错误。 |
| System.IO.FileLoadException | 在不公开 Windows SDK 类型的库中调用某些 API 时，可能会出现此运行时错误。 |

要解决这些错误，请将 .NET SDK 更新到最新版本。 这样做将确保你的应用程序使用的运行时和 Windows SDK 程序集与所有依赖项兼容。 使用 .NET 5 SDK 的早期服务/功能更新时可能会出现这些错误，原因是运行时修补程序可能要求程序集版本更新。

## <a name="known-issues"></a>已知问题

已知问题和中断性变更记录在 [C#/WinRT GitHub 存储库](https://aka.ms/cswinrt/repo)中。

如果在 C#/WinRT NuGet 程序包、cswinrt.exe 编译器或生成的投影源中遇到任何功能问题，请通过 [C#/WinRT 问题页](https://github.com/microsoft/CsWinRT/issues)将问题提交给我们。

## <a name="additional-resources"></a>其他资源

* [C#/WinRT GitHub 存储库](https://aka.ms/cswinrt/repo)
