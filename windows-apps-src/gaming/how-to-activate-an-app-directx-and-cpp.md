---
title: 如何激活应用（DirectX 和 C++）
description: 本主题介绍了如何为通用 Windows 平台 (UWP) DirectX 应用定义激活体验。
ms.assetid: b07c7da1-8a5e-5b57-6f77-6439bf653a53
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, directx, 激活
ms.localizationpriority: medium
ms.openlocfilehash: 839cfc718e6225beb14df535bc48f9ba6f3f6dc5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173141"
---
# <a name="how-to-activate-an-app-directx-and-c"></a>如何激活应用（DirectX 和 C++）



本主题介绍了如何为通用 Windows 平台 (UWP) DirectX 应用定义激活体验。

## <a name="register-the-app-activation-event-handler"></a>注册应用激活事件处理程序


首先，注册以处理 [**CoreApplicationView::Activated**](/uwp/api/windows.applicationmodel.core.coreapplicationview.activated) 事件，当启动你的应用并由操作系统对你的应用进行初始化时会引发该事件。

将此代码添加到你的查看提供程序（在此示例中称为 **MyViewProvider**）的 [**IFrameworkView::Initialize**](/uwp/api/windows.applicationmodel.core.iframeworkview.initialize) 方法的实现中：

```cpp
void App::Initialize(CoreApplicationView^ applicationView)
{
    // Register event handlers for the app lifecycle. This example includes Activated, so that we
    // can make the CoreWindow active and start rendering on the window.
    applicationView->Activated +=
        ref new TypedEventHandler<CoreApplicationView^, IActivatedEventArgs^>(this, &App::OnActivated);
  
  //...

}
```

## <a name="activate-the-corewindow-instance-for-the-app"></a>为应用激活 CoreWindow 实例


当你的应用启动时，你必须为你的应用获取对 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow) 的引用。 **CoreWindow** 包含你的应用用于处理窗口事件的窗口事件消息调度程序。 通过调用 [**CoreWindow::GetForCurrentThread**](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) 在你的回调中为应用激活事件获取此引用。 获取此引用后，通过调用 [**CoreWindow::Activate**](/uwp/api/windows.ui.core.corewindow.activate) 激活主应用窗口。

```cpp
void App::OnActivated(CoreApplicationView^ applicationView, IActivatedEventArgs^ args)
{
    // Run() won't start until the CoreWindow is activated.
    CoreWindow::GetForCurrentThread()->Activate();
}
```

## <a name="start-processing-event-message-for-the-main-app-window"></a>为主屏窗口启动处理事件消息


对于应用的 [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)，你的回调作为由 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 处理的事件消息发生。 如果你没有从你的应用的主回路调用 [**CoreDispatcher::ProcessEvents**](/uwp/api/windows.ui.core.coredispatcher.processevents)（在你的查看提供程序的 [**IFrameworkView::Run**](/uwp/api/windows.applicationmodel.core.iframeworkview.run) 方法中实现），那么将不会调用此回调。

``` syntax
// This method is called after the window becomes active.
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

## <a name="related-topics"></a>相关主题


* [如何暂停应用（DirectX 和 C++）](how-to-suspend-an-app-directx-and-cpp.md)
* [如何恢复应用（DirectX 和 C++）](how-to-resume-an-app-directx-and-cpp.md)

 

 