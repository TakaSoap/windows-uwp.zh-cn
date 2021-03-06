---
title: 设置游戏项目
description: 装配游戏的第一步是在 Microsoft Visual Studio 中设置项目，以便让基础架构正常工作所需的代码量减至最小。
ms.assetid: 9fde90b3-bf79-bcb3-03b6-d38ab85803f2
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 设置, directx
ms.localizationpriority: medium
ms.openlocfilehash: ca91926ec374015eeb88be6d89d3e1741d8b9c6d
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367684"
---
# <a name="set-up-the-game-project"></a>设置游戏项目

本主题介绍如何使用 Visual Studio 中的模板设置简单的 UWP DirectX 游戏。 装配游戏的第一步是在 Microsoft Visual Studio 中设置项目，以便让基础架构正常工作所需的代码量减至最小。 了解如何在使用正确的模板以及专门为游戏开发配置项目时节省设置时间。

## <a name="objectives"></a>目标

* 使用模板在 Visual Studio 中设置 Direct3D 游戏项目
* 通过检查 **App** 源文件了解游戏的主入口点
* 检查项目的 **package.appxmanifest** 文件
* 了解项目包含了哪些游戏开发人员工具和支持

## <a name="how-to-set-up-the-game-project"></a>如何设置游戏项目

如果你不熟悉通用 Windows 平台 (UWP) 的开发，我们建议使用 Visual Studio 中的模板来设置基本代码结构。

>[!Note]
>这篇文章是围绕一个游戏示例编写的教程系列的一部分。 你可以在 [Direct3D 游戏示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX)获取最新代码。 此示例是大型 UWP 功能示例集合的一部分。 有关如何下载示例的说明，请参阅[从 GitHub 获取 UWP 示例](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples)。

### <a name="use-directx-template-to-create-a-project"></a>使用 DirectX 模板创建项目

Visual Studio 模板是一个设置和代码文件集合，专用于基于首选语言和技术的特定类型的应用。 在 Microsoft Visual Studio 2017 中，您会发现多个可以极大地简化游戏和图形应用程序开发的模板。 如果不使用模板，则必须亲自开发大量的基本图形呈现和显示框架，这对于游戏开发新手而言可能是一件苦差事。

用于本教程的模板是标题为 **DirectX 11 应用(通用 Windows)** 的模板。 

在 Visual Studio 中创建 DirectX 11 游戏项目的步骤：
1.  选择**文件...** &gt; **新**&gt; **项目...**
2.  在左窗格中，选择**已安装** &gt; **模板** &gt; **Visual C++**  &gt; **Windows世界**
3.  在中心窗格中，选择 **DirectX 11 应用（通用 Windows）**
4.  为你的游戏项目命名，并单击“确定”  。

![显示如何选择 directx11 模板来创建新游戏项目的屏幕截图](images/simple-dx-game-setup-new-project.png)

此模板为你提供了采用 DirectX 和 C++ 的 UWP 应用的基本框架。 点按 F5 生成并运行应用。 查看浅灰蓝色屏幕。 该模板创建包含使用 DirectX 和 C++ 的 UWP 应用的基本功能的多个代码文件。

## <a name="review-the-apps-main-entry-point-by-understanding-iframeworkview"></a>通过了解 IFrameworkView 查看应用的主入口点

**App** 类继承自 **IFrameworkView** 类。

### <a name="inspect-apph"></a>检查 **App.h**。

让我们快速看一下中的 5 个方法**app.h 中** &mdash; [**初始化**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize)， [ **SetWindow** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow)， [**负载**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load)， [**运行**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run)，以及[**取消初始化**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize)实现时[ **IFrameworkView** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.run)定义的视图提供程序的接口。 这些方法由启动游戏时创建的应用单一实例运行，并加载应用的所有资源以及连接相应的事件处理程序。

```cpp
    // Main entry point for our app. Connects the app with the Windows shell and handle application lifecycle events.
    ref class App sealed : public Windows::ApplicationModel::Core::IFrameworkView
    {
    public:
        App();

        // IFrameworkView Methods.
        virtual void Initialize(Windows::ApplicationModel::Core::CoreApplicationView^ applicationView);
        virtual void SetWindow(Windows::UI::Core::CoreWindow^ window);
        virtual void Load(Platform::String^ entryPoint);
        virtual void Run();
        virtual void Uninitialize();

    protected:
        ...
    };
```

### <a name="inspect-appcpp"></a>检查 **App.cpp**

下面是 **App.cpp** 源文件中的 **main** 方法：

```cpp
//The main function is only used to initialize our IFrameworkView class.
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

此方法从该视图提供程序工厂创建的 Direct3D 视图提供程序实例 (**Direct3DApplicationSource**中定义`App.h`)，并将其传递到应用的单一实例通过调用[ **CoreApplication::Run**](/uwp/api/windows.applicationmodel.core.coreapplication.run)。 Framework 视图的方法 (即**应用程序**在此示例中的类) 的顺序调用**初始化**-**SetWindow** -**负载**-**OnActivated**-**运行**-**取消初始化**。 调用**CoreApplication::Run**启动了该 lifycycle。 您的游戏的主循环的实现的正文中驻留[ **IFrameworkView::Run**方法](/uwp/api/windows.applicationmodel.core.iframeworkview.run)，并在这种情况下，它具有**App::Run**。

滚动以在 **App.cpp** 中查找 **App::Run** 方法。 下面是代码。

```cpp
//This method is called after the window becomes active.
void App::Run()
{
    while (!m_windowClosed)
    {
        if (m_windowVisible)
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            m_main->Update();

            if (m_main->Render())
            {
                m_deviceResources->Present();
            }
        }
        else
        {
            CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
        }
    }
}
```

此方法的作用：如果为您的游戏窗口未关闭，它将所有事件，更新计时器，然后呈现并显示结果的图形管道。 我们将讨论这中更详细地[定义 UWP 应用程序框架](tutorial--building-the-games-uwp-app-framework.md)，[呈现框架实现：对呈现简介](tutorial--assembling-the-rendering-pipeline.md)，和[呈现框架 II:游戏渲染](tutorial-game-rendering.md)。 现在，你应了解 UWP DirectX 游戏的基本代码结构。

## <a name="review-and-update-the-packageappxmanifest-file"></a>检查和更新 package.appxmanifest 文件


代码文件并不是模板的全部内容。 **Package.appxmanifest** 文件包含有关项目的元数据，这些元数据用于打包和启动游戏以及提交到 Microsoft Store。 它还包含玩家系统用于提供对运行游戏所需的系统资源的访问权限的重要信息。

通过在**解决方案资源管理器**中双击 **Package.appxmanifest** 文件启动**清单设计器**。

![package.appx 清单编辑器的屏幕截图。](images/simple-dx-game-setup-app-manifest.png)

有关 **package.appxmanifest** 文件和打包的详细信息，请参阅[清单设计器](https://docs.microsoft.com/previous-versions/br230259(v=vs.140))。 现在，请看一下“功能”  选项卡并查看提供的选项。

![显示 Direct3D 应用的默认功能的屏幕截图。](images/simple-dx-game-setup-capabilities.png)

如果不选择游戏使用的功能（如用于全球高分榜的“Internet”  ），你将无法访问相应的资源或功能。 在创建新游戏时，请确保选择了运行游戏所需的功能！

现在，我们看一下 **DirectX 11 应用（通用 Windows）** 模板附带的其他文件。

## <a name="review-the-included-libraries-and-headers"></a>查看包括的库和头文件

有一些文件我们还没有碰到过。 这些文件提供 Direct3D 游戏开发场景通用的附加工具和支持。 要获取基本 DirectX 游戏项目附带的文件的完整列表，请参阅 [DirectX 游戏项目模板](user-interface.md#template-structure)。

| 模板源文件         | 文件夹            | 描述 |
|------------------------------|------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DeviceResources.h/.cpp       | 通用                 | 定义控制所有 DirectX [设备资源](tutorial--assembling-the-rendering-pipeline.md#resource)的类对象。 它还提供了一个界面，让拥有 DeviceResources 的应用程序在设备丢失或创建时收到通知。                                                |
| DirectXHelper.h              | 通用                 | 实现包括 **DX::ThrowIfFailed**、**ReadDataAsync** 和 **ConvertDipsToPixels 在内的方法。 **DX::ThrowIfFailed** 可将 DirectX Win32 API 返回的错误 HRESULT 值转换为 Windows 运行时异常。 使用此方法为调试 DirectX 错误放置断点。 有关详细信息，请参阅 [ThrowIfFailed](https://github.com/Microsoft/DirectXTK/wiki/ThrowIfFailed)。 **ReadDataAsync** 从二进制文件异步读取。 **ConvertDipsToPixels** 将以与设备无关的像素 (DIP) 为单位的长度转换为以物理像素为单位的长度。 |
| StepTimer.h                  | 通用                 | 定义用于游戏或交互呈现应用的高分辨率计时器。   |
| Sample3DSceneRenderer.h/.cpp | 内容                | 定义用于实例化基本呈现管道的类对象。 它创建用于将 Direct3D 交换链和图形适配器连接到使用 DirectX 的 UWP 的基本呈现器实现。   |
| SampleFPSTextRenderer.h/.cpp | 内容                | 定义使用 Direct2D 和 DirectWrite 在屏幕右下角呈现当前帧/秒 (FPS) 值的类对象。  |
| SamplePixelShader.hlsl       | 内容                | 包含最基本的像素着色器的高级着色器语言 (HLSL) 代码。                                            |
| SampleVertexShader.hlsl      | 内容                | 包含最基本的顶点着色器的高级着色器语言 (HLSL) 代码。                                           |
| ShaderStructures.h           | 内容                | 包含可用于将 MVP 矩阵和每顶点数据发送到顶点着色器的着色器结构。  |
| pch.h/.cpp                   | Main                   | 包含所有 Windows 系统中包括的 Direct3D 应用使用的 API，包括 DirectX 11 API。| 

### <a name="next-steps"></a>后续步骤

现在，你已经学习了如何使用 **DirectX 11 应用（通用 Windows）** 模板创建 UWP DirectX 游戏项目，并且已经引入了几个此项目提供的组件和文件。

下一个部分是[定义游戏的 UWP 框架](tutorial--building-the-games-uwp-app-framework.md)。 我们将检查此游戏的使用情况，并展开介绍该模板所提供的许多概念和组件。

 

 




