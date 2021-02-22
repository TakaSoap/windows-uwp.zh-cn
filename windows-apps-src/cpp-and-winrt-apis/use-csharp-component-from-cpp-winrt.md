---
description: 本主题指导你完成将简单的 C# 组件添加到 C++/WinRT 应用的过程
title: 创作 C# Windows 运行时组件，便于通过 C++/WinRT 应用使用
ms.date: 12/30/2020
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, C#
ms.localizationpriority: medium
ms.openlocfilehash: 6c6d766eb757241d6d3f38c425538c62fd057316
ms.sourcegitcommit: b0cedbb65d00e6241a9350c2ee1b1e89b090a2d2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/18/2021
ms.locfileid: "101086938"
---
# <a name="authoring-a-c-windows-runtime-component-for-use-from-a-cwinrt-app"></a>创作 C# Windows 运行时组件，便于通过 C++/WinRT 应用使用

本主题指导你完成将简单的 C# 组件添加到 C++/WinRT 项目的过程。

通过 Visual Studio，可轻松地在使用 C# 或 Visual Basic 编写的 Windows 运行时组件 (WRC) 项目中创作和部署自己的自定义 Windows 运行时类型，然后从 C++ 应用程序项目中引用该 WRC，并从该应用程序中使用这些自定义类型。

在内部，Windows 运行时类型可以使用 UWP 应用程序中允许的任何 .NET 功能。

> [!NOTE]
> 有关详细信息，请参阅[使用 C# 和 Visual Basic 创建 Windows 运行时组件](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)和[适用于 UWP 应用的 .NET 概述](/dotnet/api/index?view=dotnet-uwp-10.0&preserve-view=true)。

在外部，类型的成员只能为其参数和返回值公开 Windows 运行时类型。 在生成解决方案时，Visual Studio 会生成 .NET WRC 项目，然后执行可创建 Windows 元数据 (.winmd) 文件的生成步骤。 这是你的 Windows 运行时组件 (WRC)，即 Visual Studio 在你的应用中包含的组件。

> [!NOTE]
> .NET 自动将某些常用的 .NET 类型（例如基元数据类型和集合类型）映射到其 Windows 运行时等效项。 这些 .NET 类型可在 Windows 运行时组件的公共接口中使用，并且将作为相应的 Windows 运行时类型向该组件的用户显示。 请参阅[使用 C# 和 Visual Basic 创建 Windows 运行时组件](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)。

## <a name="prerequisites"></a>先决条件：

- Windows 10
- [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-blank-app"></a>创建空白应用

在 Visual Studio 中，使用“空白应用(C++/WinRT)”项目模板创建新项目。 请确保使用的是“(C++/WinRT)”模板，而不是“(通用 Windows)”模板。 

将新项目的名称设置为 CppToCSharpWinRT，使文件夹结构与本演练相匹配。

## <a name="add-a-c-windows-runtime-component-to-the-solution"></a>向解决方案添加 C# Windows 运行时组件

在 Visual Studio 中，创建组件项目：在“解决方案资源管理器”中，打开 CppToCSharpWinRT 解决方案的快捷菜单，然后依次选择“添加”和“新建项目”，将新的 C# 项目添加到解决方案 。 在“添加新项目”对话框的“已安装模板”部分中，选择“Visual C#”，然后依次选择“Windows”和“通用”    。 选择“Windows 运行时组件(通用 Windows)”模板，然后输入 SampleComponent 作为项目名称 。 

> [!NOTE]
> 在“新的通用 Windows 平台项目”对话框中，选择“Windows 10 创意者更新(10.0; 内部版本 15063)”作为最低版本 。 有关详细信息，请参阅下面的[应用程序最低版本](#application-minimum-version)部分。

## <a name="add-the-c-getmystring-method"></a>添加 C# GetMyString 方法

在 SampleComponent 项目中，将类的名称从“Class1”更改为“Example”  。 然后，向该类添加两个简单的成员，一个专用 `int` 字段和一个名为 GetMyString 的实例方法：

> ```csharp
>     public sealed class Example
>     {
>         int MyNumber;
> 
>         public string GetMyString()
>         {
>             return $"This is call #: {++MyNumber}";
>         }
>     }
> ```

> [!NOTE]
> 默认情况下，该类被标记为“公共封装”。 必须封装通过你的组件公开的所有 Windows 运行时类。

> [!NOTE]
> 可选：若要为新添加的成员启用 IntelliSense，请在“解决方案资源管理器”中，打开 SampleComponent 项目的快捷菜单，然后选择“生成”。

## <a name="reference-the-c-samplecomponent-from-the-cpptocsharpwinrt-project"></a>从 CppToCSharpWinRT 项目引用 C# SampleComponent

在“解决方案资源管理器”的 C++/WinRT 项目中，打开“引用”的快捷菜单，然后选择“添加引用”，打开“添加引用”对话框  。 依次选择“项目”和“解决方案”。 选中 SampleComponent 项目的复选框，然后选择“确定”来添加引用。

> [!NOTE]
> 可选：若要为 C++/WinRT 项目启用 IntelliSense，请在“解决方案资源管理器”中，打开 CppToCSharpWinRT 项目的快捷菜单，然后选择“生成”。

## <a name="edit-mainpageh"></a>编辑 MainPage.h

在 CppToCSharpWinRT 项目中打开 `MainPage.h`，然后添加两个项。  首先在 `#include` 语句的末尾添加 `#include "winrt/SampleComponent.h"`，然后向 `MainPage` 结构添加 `winrt::SampleComponent::Example` 字段。

```cppwinrt
// MainPage.h
...
#include "winrt/SampleComponent.h"

namespace winrt::CppToCSharpWinRT::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        winrt::SampleComponent::Example myExample;
...
    };
}
```

> [!NOTE]
> 在 Visual Studio 中，`MainPage.h` 列于 `MainPage.xaml` 下。

## <a name="edit-mainpagecpp"></a>编辑 MainPage.cpp

在 `MainPage.cpp` 中，更改 `Mainpage::ClickHandler` 实现以调用 C# 方法 `GetMyString`。

```cppwinrt
void MainPage::ClickHandler(IInspectable const&, RoutedEventArgs const&)
{
    //myButton().Content(box_value(L"Clicked"));

    hstring myString = myExample.GetMyString();

    myButton().Content(box_value(myString));
}
```
## <a name="run-the-project"></a>运行项目

现在可以生成并运行该项目了。 每次单击该按钮时，按钮中的数字将递增。

![C++/WinRT Windows 调入 C# 组件的屏幕截图](images/csharp-cpp-sample.png)

> [!TIP]
> 在 Visual Studio 中，创建组件项目：在“解决方案资源管理器”中，打开 CppToCSharpWinRT 项目的快捷菜单，选择“属性”，然后在“配置属性”下选择“调试”  。 如果要同时调试 C#（托管）和 C++（本机）代码，请将调试器类型设置为“托管和本机”。
> ![C++ 调试属性](images/cpp-debugging-properties.png)

## <a name="application-minimum-version"></a>应用程序最低版本

C# 项目版本的[应用程序最低版本](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version)将控制用于编译应用程序的 .NET 版本。 例如，选择“Windows 10 Fall Creators Update (10.0; 内部版本 16299)”或更高版本将启用 .NET Standard 2.0 和 Windows ARM64 处理器支持。 

> [!TIP]
> 如果不需要 .NET Standard 2.0 或 ARM64 支持，建议使用低于 16299 的应用程序最低版本以避免额外的生成配置。

## <a name="configure-for-windows-10-fall-creators-update-100-build-16299"></a>针对 Windows 10 Fall Creators Update（10.0；内部版本 16299）进行配置

按照以下步骤操作，在从 C++/WinRT 项目引用的 C# 项目中启用 .NET Standard 2.0 或 Windows ARM64 支持。 

在 Visual Studio 中，转到“解决方案资源管理器”并打开 CppToCSharpWinRT 项目的快捷菜单。  选择“属性”，并将通用 Windows 应用最低版本设置为“Windows 10 Fall Creators Update (10.0; 内部版本 16299)”（或更高版本） 。 对 SampleComponent 项目执行相同的操作。

在 Visual Studio 中，打开 CppToCSharpWinRT 项目的快捷菜单，然后选择“卸载项目”以在文本编辑器中打开 `CppToCSharpWinRT.vcxproj`。 

将以下 XML 复制并粘贴到 `CPPWinRTCSharpV2.vcxproj` 中的第一个 `PropertyGroup`。 

```xml
   <!-- Start Custom .NET Native properties -->
   <DotNetNativeVersion>2.2.9-rel-29512-01</DotNetNativeVersion>
   <DotNetNativeSharedLibary>2.2.8-rel-29512-01</DotNetNativeSharedLibary>
   <UWPCoreRuntimeSdkVersion>2.2.11</UWPCoreRuntimeSdkVersion>
   <!--<NugetPath>$(USERPROFILE)\.nuget\packages</NugetPath>-->
   <NugetPath>$(ProgramFiles)\Microsoft SDKs\UWPNuGetPackages</NugetPath>
   <!-- End Custom .NET Native properties -->
```

`DotNetNativeVersion`、`DotNetNativeSharedLibary` 和 `UWPCoreRuntimeSdkVersion` 的值可能因 Visual Studio 版本而异。  若要将它们设置为正确的值，请打开 `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages` 并查看下表中每个值的子目录。  例如，`%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` 目录下将有一个名为 `2.2.9-rel-29512-01` 的目录。

> | MSBuild 变量 | Directory | 示例
> |-| - | -
> | DotNetNativeVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.Compiler` | `2.2.9-rel-29512-01`
> | DotNetNativeSharedLibary | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.Native.SharedLibrary` | `2.2.8-rel-29512-01`
> | UWPCoreRuntimeSdkVersion | `%ProgramFiles(x86)%\Microsoft SDKs\UWPNuGetPackages\Microsoft.Net.UWPCoreRuntimeSdk` | `2.2.11`

接下来，紧接着第一个 `PropertyGroup` 之后，添加以下内容（未更改）。

```xml
  <!-- Start Custom .NET Native targets -->
  <!-- Import all of the .NET Native / CoreCLR props at the beginning of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.props" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.props" />
  <!-- End Custom .NET Native targets -->
```

在项目文件的末尾，就在结束 `Project` 标记之前，添加以下内容（未更改）。

```xml
  <!-- Import all of the .NET Native / CoreCLR targets at the end of the project -->
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x86.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-x64.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk\$(UWPCoreRuntimeSdkVersion)\build\runtime.win10-arm.Microsoft.Net.UWPCoreRuntimeSdk.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x86.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-x64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.Compiler\$(DotNetNativeVersion)\build\runtime.win10-arm64.Microsoft.Net.Native.Compiler.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x86.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-x64.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm.Microsoft.Net.Native.SharedLibrary.targets" />
  <Import Condition="'$(WindowsTargetPlatformMinVersion)' &gt;= '10.0.16299.0'" Project="$(NugetPath)\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary\$(DotNetNativeSharedLibary)\build\runtime.win10-arm64.Microsoft.Net.Native.SharedLibrary.targets" />
  <!-- End Custom .NET Native targets -->
```

在 Visual Studio 中重载该项目文件。 为此，请在 Visual Studio 解决方案资源管理器中，打开 CppToCSharpWinRT 项目的快捷菜单，然后选择“重载项目”。

## <a name="building-for-net-native"></a>针对 .NET Native 进行生成

建议使用针对 .NET Native 生成的 C# 组件来生成和测试应用程序。 在 Visual Studio 中，打开 CppToCSharpWinRT 项目的快捷菜单，然后选择“卸载项目”以在文本编辑器中打开 `CppToCSharpWinRT.vcxproj`。 

接下来，在 C++ 项目文件的“版本和 ARM64 配置”中，将 `UseDotNetNativeToolchain` 属性设置为 `true`。

在 Visual Studio 解决方案资源管理器中，打开 CppToCSharpWinRT 项目的快捷菜单，然后选择“重载项目”。 

```xml
  <PropertyGroup Condition="'$(Configuration)'=='Release'" Label="Configuration">
...
    <UseDotNetNativeToolchain>true</UseDotNetNativeToolchain>
  </PropertyGroup>
  <PropertyGroup Condition="$(Platform)'='ARM64'" Label="Configuration">
    <UseDotNetNativeToolchain Condition="'$(UseDotNetNativeToolchain)'==''">true</UseDotNetNativeToolchain>
  </PropertyGroup>
```

## <a name="related-topics"></a>相关主题
* [使用 C# 和 Visual Basic 创建 Windows 运行时组件](/windows/uwp/winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md)
* [使用 C++/WinRT 创建 Windows 运行时组件](/windows-apps-src/winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)