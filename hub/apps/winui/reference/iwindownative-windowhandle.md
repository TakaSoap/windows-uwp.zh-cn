---
title: IWindowNative.WindowHandle 属性
description: 用于获取请求的窗口 HWND 的 WinUI COM 属性。
ms.topic: reference
ms.date: 03/09/2021
keywords: winui, Windows UI 库
ms.openlocfilehash: 9c8278b5b19f4e7eeee447ed4a4261064f36194d
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730647"
---
# <a name="iwindownativewindowhandle-property-microsoftuixamlwindowh"></a>IWindowNative.WindowHandle 属性 (microsoft.ui.xaml.window.h)

为窗口获取请求的句柄。

## <a name="syntax"></a>语法

<!--
[
    object,
    uuid( EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB ),
    local,
    pointer_default(unique)
]
interface IWindowNative: IUnknown
{
    [propget] HRESULT WindowHandle([out, retval] HWND* hWnd);
};
-->

```cpp
HRESULT WindowHandle(
  HWND* hWnd
);
```

## <a name="parameters"></a>参数

*hWnd* [out]

类型：**HWND***

窗口的句柄。

## <a name="return-value"></a>返回值

类型：HRESULT

如果该方法成功，则返回 S_OK。 否则，它将返回 HRESULT 错误代码。

## <a name="remarks"></a>备注

## <a name="examples"></a>示例

尝试以下示例之前，请查看以下主题：

- 若要对桌面项目模板使用 WinUI 3，请配置开发计算机并[设置开发环境](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。
- 根据[桌面应用的 WinUI 3 入门](../winui3/get-started-winui3-for-desktop.md)中所述，通过创建并运行初始模板应用来确认开发环境按预期方式工作。

### <a name="customized-window-icon"></a>自定义窗口图标

在下面的示例中，我们先使用初始的“桌面 C#/.NET 5 中的 WinUI”模板代码，并演示如何使用 WindowHandle 来自定义用于应用窗口的图标 。

#### <a name="mainwindowxamlcs"></a>MainWindow.xaml.cs

1. 首先，添加以下 using 指令：

    - [System.Runtime.InteropServices](/dotnet/api/system.runtime.interopservices)：提供对 COM 互操作和平台调用服务的支持。 在本例中，必须具有它才能使用 PInvoke 功能。
    - [WinRT](/uwp/cpp-ref-for-winrt/winrt)：提供属于 C++/WinRT 的自定义数据类型。 在本例中，必须具有它才能使用 IWindowNative COM 接口。

    ```csharp
    using System.Runtime.InteropServices;
    using WinRT;
    ```

1. 然后，我们在项目中添加一个 .ico 文件 (Images/windowIcon.ico)，并将该文件的“生成操作”设置为“内容”（右键单击文件并选择“属性”）。

1. 在 MainWindow 方法中，我们使用在上一步中添加的对 .ico 文件的引用添加对 `LoadIcon("Images/windowIcon.ico");` 函数的调用（详见下一步）。

    ```csharp
    public MainWindow()
    {
        LoadIcon("Images/windowIcon.ico");
    
        this.InitializeComponent();
    }
    ```

1. 接下来，我们添加一个 `LoadIcon(string iconName)` 函数，它会获取应用程序的句柄并使用各种 [PInvoke](/dotnet/standard/native-interop/pinvoke) 功能（包括 [LoadImage](/windows/win32/api/winuser/nf-winuser-loadimagew) 和 [SendMessage](/windows/win32/api/winuser/nf-winuser-sendmessage)）来设置应用程序图标。

    ```csharp
    private void LoadIcon(string iconName)
    {
        //Get the Window's HWND
        var hwnd = this.As<IWindowNative>().WindowHandle;
    
        IntPtr hIcon = PInvoke.User32.LoadImage(
            IntPtr.Zero, 
            iconName,
            PInvoke.User32.ImageType.IMAGE_ICON, 
            16, 16, 
            PInvoke.User32.LoadImageFlags.LR_LOADFROMFILE);
    
        PInvoke.User32.SendMessage(
            hwnd, 
            PInvoke.User32.WindowMessage.WM_SETICON, 
            (IntPtr)0, 
            hIcon);
    }    
    ```

1. 最后，我们嵌入来自公共 IWindowNative 接口的类型信息，并从运行时程序集创建类的实例。 有关更多详细信息，请查看[嵌入托管程序集中的类型](/dotnet/standard/assembly/embed-types-visual-studio)。

    ```csharp
    [ComImport]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    [Guid("EECDBF0E-BAE9-4CB6-A68E-9598E1CB57BB")]
    internal interface IWindowNative
    {
        IntPtr WindowHandle { get; }
    }
    ```

1. 如果你已在自己的应用中执行这些步骤，请生成和运行应用。 你应会看到一个如下所示的应用程序窗口（带有自定义应用图标）：

    :::image type="content" source="../winui3/images/build-basic/template-app-windowhandle.png" alt-text="显示有自定义应用程序图标的模板应用。":::<br/>*显示有自定义应用程序图标的模板应用。*

## <a name="applies-to"></a>适用于

| 产品 | 版本 |
| --- | --- |
| WinUI | 3.0.0-project-reunion-0.5, 3.0.0-project-reunion-preview-0.5 |

## <a name="see-also"></a>另请参阅

[IWindowNative 接口](iwindownative.md)
