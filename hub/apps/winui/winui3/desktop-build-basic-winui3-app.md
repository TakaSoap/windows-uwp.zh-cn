---
description: 使用 WinUI 3 UI 生成 .NET 和 C++/Win32 桌面应用。
title: 构建基本 WinUI 3 桌面版应用
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.author: kbridge
author: Karl-Bridge-Microsoft
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: e7453f620b7c82c46a44e293a36a9eb51bfb36ae
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730734"
---
# <a name="build-a-basic-winui-3---project-reunion-05-desktop-app"></a>生成基本的 WinUI 3 - Project Reunion 0.5 桌面应用

在本文中，我们将逐步介绍如何使用完全由 WinUI 3 控件和功能组成的用户界面生成基本的托管 C#/.NET 5 和原生 C++/Win32 WinUI 3 - Project Reunion 0.5 桌面应用程序。

## <a name="prerequisites"></a>先决条件

1. 设置开发环境。 请参阅 [Project Reunion 入门](../../project-reunion/get-started-with-project-reunion.md)。
1. 通过创建用于 C# 和 .NET 5 项目的第一款 WinUI 3 桌面应用来测试配置。 请参阅[适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)。

:::image type="content" source="images/build-basic/template-app.png" alt-text="初始模板应用，正在运行中。":::<br/>
初始模板应用，正在运行中。

## <a name="initial-template-app"></a>初始模板应用

我们将从初始模板应用程序开始生成，但在开始之前，我们先来看看模板应用的结构和内容。

### <a name="the-application-solution"></a>应用程序解决方案

默认情况下，该解决方案包含两个项目：应用程序本身以及用于创建 [.msix](/windows/msix) 应用包的另一个项目。

> [!NOTE]
> 当前需要 MSIX 应用包才能将使用 Project Reunion 0.5 的应用部署到其他计算机。 但是，未来版本的 Project Reunion 将支持部署未打包应用。

:::image type="content" source="images/build-basic/template-app-solution-explorer.png" alt-text="显示初始模板应用文件结构的解决方案资源管理器。":::<br/>
显示初始模板应用文件结构的解决方案资源管理器。

### <a name="the-application-project"></a>应用程序项目

现在，双击应用程序项目文件（或右键单击并选择“编辑项目文件”），这样便会在 XML 文本编辑器中打开该文件。

下面是来自初始模板应用的项目文件，其中包含需要注意的两个项目：

- `TargetFramework` 是 [.NET 5](/dotnet/core/dotnet-five)，今后这是 .NET 的主要实现。
- 各种 Project Reunion 功能的 NuGet `PackageReference` 元素，包括 WinUI。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
    <TargetPlatformMinVersion>10.0.17763.0</TargetPlatformMinVersion>
    <RootNamespace>WinUIApp2</RootNamespace>
    <Platforms>x86;x64;arm64</Platforms>
    <RuntimeIdentifiers>win10-x86;win10-x64;win10-arm64</RuntimeIdentifiers>
    <NoWin32Manifest>true</NoWin32Manifest>
    <ApplicationIcon />
    <StartupObject />
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.ProjectReunion" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.Foundation" Version="0.5.0-prerelease" />
    <PackageReference Include="Microsoft.ProjectReunion.WinUI" Version="0.5.0-prerelease" />
    <Manifest Include="$(ApplicationManifest)" />
  </ItemGroup>
</Project>
```

### <a name="the-mainwindowxaml-file"></a>MainWindow.xaml 文件

凭借 WinUI 3，你现在可以通过 XAML 标记创建 [Window](/windows/winui/api/microsoft.ui.xaml.window) 类的实例。

XAML Window 类已扩展为支持桌面窗口，从而转换为 UWP 和桌面应用模型所使用的每个低级别窗口实现的抽象。 具体而言，UWP 使用 CoreWindow，而 Win32 使用窗口句柄（或称 HWND）和相应的 Win32 API。

以下代码示例是初始模板应用的 Mainwindow.xaml 文件，该文件使用 Window 类作为应用的根元素。

```xaml
<Window
    x:Class="WinUIApp2.MainWindow"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:WinUIApp2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Orientation="Horizontal" HorizontalAlignment="Center" VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
    </StackPanel>
</Window>
```

## <a name="basic-managed-cnet-5-app"></a>基本托管 C#/.NET 5 应用

对于第一个示例，让我们从 User32.dll 使用 [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow) API，从而以编程方式使窗口最大化。

1. 首先，若要从 User32.dll 调用 Win32 API，请将 PInvoke.User32 NuGet 包添加到你的项目中（从 Visual Studio 菜单中，选择“工具”->“NuGet 包管理器”->“管理解决方案的 NuGet 包…”）。有关更多详细信息，请参阅[从托管代码调用原生函数](/cpp/dotnet/calling-native-functions-from-managed-code) 。
1. 添加包后，打开 Mainwindow.xaml 代码隐藏文件，并将 `MyButton_Click` 事件处理程序替换为以下代码：

    ```csharp
            private void MyButton_Click(object sender, RoutedEventArgs e)
            {
                myButton.Content = "Clicked";
    
                IntPtr hwnd = (App.Current as App).MainWindowWindowHandle;
                PInvoke.User32.ShowWindow(hwnd, PInvoke.User32.WindowShowStyle.SW_MAXIMIZE);
            }
    ```

    [PInvoke](/dotnet/standard/native-interop/pinvoke) [ShowWindow](/windows/win32/api/winuser/nf-winuser-showwindow) 方法将窗口句柄 (`hwnd`) 用作第一个参数，并使用第二个参数指定应使窗口最大化。 

1. 若要获取窗口句柄，请使用 [GetActiveWindow](/windows/win32/api/winuser/nf-winuser-getactivewindow) 方法。 此方法返回当前活动窗口的窗口句柄（在激活目标窗口之后调用此方法）。

    `MainWindow` 对象（参阅 mainwindow.xaml）在 App.xaml.cs 代码隐藏文件中的 `OnLaunched` 事件处理程序中进行创建、实例化和激活。 将 `OnLaunched` 事件替换为以下代码：

    ```csharp
        protected override void OnLaunched(Microsoft.UI.Xaml.LaunchActivatedEventArgs args)
        {
            m_window = new MainWindow();
            m_window.Activate();
            m_windowhandle = PInvoke.User32.GetActiveWindow();
        }

        IntPtr m_windowhandle;
        public IntPtr MainWindowWindowHandle { get { return m_windowhandle; } }
    ```

1. 编译并运行应用。
1. 最后，按“单击我”按钮，应用窗口应最大化。

## <a name="full-trust-desktop-app"></a>“完全信任”桌面应用

WinUI 3 提供了一种功能，使应用程序能够在 [AppContainer](/windows/win32/secauthz/appcontainer-for-legacy-applications-) 的安全沙盒以外以“完全信任权限”运行。

托管 C#/.NET 5 和原生 C++/Win32 WinUI 3 - Project Reunion 0.5 桌面应用程序可在不受限制的情况下调用 .NET 5 API。 在以下示例（源自上一示例中使用的初始模板应用）中，我们展示了如何查询当前进程并获取所有已加载模块的列表（对 UWP 应用行不通）。

1. 在 Mainwindow.xaml 文件中，添加 [ContentDialog](/windows/winui/api/microsoft.ui.xaml.controls.contentdialog) 元素。

    ```xaml
    <StackPanel 
        Orientation="Horizontal" 
        HorizontalAlignment="Center" 
        VerticalAlignment="Center">
        <Button x:Name="myButton" Click="MyButton_Click">Click Me</Button>
        <ContentDialog x:Name="contentDialog" CloseButtonText="Close">
            <StackPanel>
                <TextBlock Name="cdTextBlock"/>
            </StackPanel>
        </ContentDialog>
    </StackPanel>
    ```

1. 在 MainWindow.xaml.cs 代码隐藏文件中，从 [System.Diagnostics](/dotnet/api/system.diagnostics) 调用 .NET API 以获取[当前进程](/dotnet/api/system.diagnostics.process.getcurrentprocess)中加载的模块。 在本例中，我们只循环访问 [Process.Modules](/dotnet/api/system.diagnostics.process.modules) 对象中的每个 [ProcessModule](/dotnet/api/system.diagnostics.processmodule)。

    ```csharp
    private async void MyButton_Click(object sender, RoutedEventArgs e)
    {
        myButton.Content = "Clicked";

        var description = new System.Text.StringBuilder();
        var process = System.Diagnostics.Process.GetCurrentProcess();
        foreach (System.Diagnostics.ProcessModule module in process.Modules)
        {
            description.AppendLine(module.FileName);
        }

        cdTextBlock.Text = description.ToString();
        await contentDialog.ShowAsync();
    }
    ```
1. 编译并运行应用。
1. 最后，按“单击我”按钮，对话框应显示进程列表。

    :::image type="content" source="images/build-basic/template-app-full-trust.png" alt-text="完全信任应用程序的示例。":::<br/>完全信任应用程序的示例。

## <a name="summary"></a>总结

在本主题中，我们介绍了如何访问基础窗口实现（本例中为 Win32 和 HWND），并在应用中使用 Win32 API 以及 Windows 10 的 WinRT API。 这让你可在创建新 WinUI 3 桌面应用时使用大部分现有桌面应用程序代码。

另外还介绍了如何将 .NET 5 API 与 WinUI 3 一起使用作 UI 框架。

## <a name="see-also"></a>另请参阅

- [Windows UI 库 3 - Project Reunion 0.5（2021 年 3 月）](index.md)
- [Project Reunion 入门](../../project-reunion/get-started-with-project-reunion.md)
- [适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)
