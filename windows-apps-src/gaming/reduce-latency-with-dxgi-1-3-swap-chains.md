---
title: 利用 DXGI 1.3 交换链减少延迟
description: 使用 DXGI 1.3 通过等待交换链发信号通知开始呈现新帧的正确时间来减少有效的帧延迟。
ms.assetid: c99b97ed-a757-879f-3d55-7ed77133f6ce
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 延迟, dxgi, 交换链, directx
ms.localizationpriority: medium
ms.openlocfilehash: dd414c3ea65d30253d54cd335ed0b85d151b6dff
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258441"
---
# <a name="reduce-latency-with-dxgi-13-swap-chains"></a>利用 DXGI 1.3 交换链减少延迟



使用 DXGI 1.3 通过等待交换链发信号通知开始呈现新帧的正确时间来减少有效的帧延迟。 游戏通常需要尽可能减少在从收到玩家输入到游戏通过更新显示内容来响应该输入之间的延迟时间。 本主题说明了一项从 Direct3D 11.2 开始可用于将游戏中的有效帧延迟降至最低的技术。

## <a name="how-does-waiting-on-the-back-buffer-reduce-latency"></a>后台缓冲区等待如何减少延迟？


使用翻转模型交换链，当你的游戏调用 [**IDXGISwapChain::Present**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 时，会将后台缓冲区“翻转”进行排队。 当呈现循环调用 Present() 时，系统会阻止线程，直到显示完前一帧为止，从而为在新帧显示之前将其排入队列腾出了空间。 这会导致游戏绘制帧的时间，以及系统允许游戏显示该帧的时间之间的延迟增加。 在很多情况下，系统将会达到一个稳定的平衡状态，在这种状态下，游戏始终会在呈现帧和呈现下一帧之间等待一个完整的额外帧。 最好等到系统准备好接受新帧，然后再根据当前数据呈现该帧并立即将其排队。

使用 DXGI 创建可等待交换链[ **\_交换\_链\_标志\_帧\_延迟\_可等待\_对象**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)标志。 以这种方式创建的交换链可以在系统准备好接收新帧时通知你的呈现循环。 这样就允许你的游戏根据当前数据进行呈现，然后立即将结果放入显示队列中。

## <a name="step-1-create-a-waitable-swap-chain"></a>步骤 1：创建一个可等待的交换链


调用[**CreateSwapChainForCoreWindow**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)时，指定[**DXGI\_交换\_链\_标志\_帧\_延迟\_对象**](https://docs.microsoft.com/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_chain_flag)标志。\_

```cpp
swapChainDesc.Flags = DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT; // Enable GetFrameLatencyWaitableObject().
```

> **请注意**，   与某些标志相反，不能使用[**ResizeBuffers**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)添加或删除此标志。 如果此标记的设置方式与创建交换链时不同，DXGI 会返回一个错误代码。

 

```cpp
// If the swap chain already exists, resize it.
HRESULT hr = m_swapChain->ResizeBuffers(
    2, // Double-buffered swap chain.
    static_cast<UINT>(m_d3dRenderTargetSize.Width),
    static_cast<UINT>(m_d3dRenderTargetSize.Height),
    DXGI_FORMAT_B8G8R8A8_UNORM,
    DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT // Enable GetFrameLatencyWaitableObject().
    );
```

## <a name="step-2-set-the-frame-latency"></a>步骤 2：设置帧延迟


使用 [**IDXGISwapChain2::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-setmaximumframelatency) API 而不是通过调用 [**IDXGIDevice1::SetMaximumFrameLatency**](https://docs.microsoft.com/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) 来设置帧延迟。

默认情况下，可等待交换链的帧延迟设置为 1，这虽然会将延迟降到最低，但也会降低 CPU-GPU 之间的平行关系。 如果你需要将 CPU-GPU 平行度增加到 60 FPS，也就是说，如果 CPU 和 GPU 用于一个帧处理呈现工作的时间都少于 16.7 毫秒，但它们所用时间总和超过 16.7 毫秒，会将帧延迟设置为 2。 这样就使 GPU 能够处理 CPU 在前一帧中排入队列的工作，同时还使 CPU 能够单独为当前帧提交呈现命令。

```cpp
// Swapchains created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT flag use their
// own per-swapchain latency setting instead of the one associated with the DXGI device. The
// default per-swapchain latency is 1, which ensures that DXGI does not queue more than one frame
// at a time. This both reduces latency and ensures that the application will only render after
// each VSync, minimizing power consumption.
//DX::ThrowIfFailed(
//    swapChain2->SetMaximumFrameLatency(1)
//    );
```

## <a name="step-3-get-the-waitable-object-from-the-swap-chain"></a>步骤 3：从交换链获取可等待对象


调用 [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject) 以检索等待句柄。 等待句柄是一个指向可等待对象的指针。 存储此句柄以供你的呈现循环使用。

```cpp
// Get the frame latency waitable object, which is used by the WaitOnSwapChain method. This
// requires that swap chain be created with the DXGI_SWAP_CHAIN_FLAG_FRAME_LATENCY_WAITABLE_OBJECT
// flag.
m_frameLatencyWaitableObject = swapChain2->GetFrameLatencyWaitableObject();
```

## <a name="step-4-wait-before-rendering-each-frame"></a>步骤 4：等待呈现每个帧


你的呈现循环应等待交换链通过可等待对象发送信号来开始呈现每个帧。 这包括使用交换链呈现的第一个帧。 使用 [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)（提供了在步骤 2 中检索的等待句柄）来发信号通知开始每个帧。

以下示例显示来自 DirectXLatency 示例的呈现循环：

```cpp
while (!m_windowClosed)
{
    if (m_windowVisible)
    {
        // Block this thread until the swap chain is finished presenting. Note that it is
        // important to call this before the first Present in order to minimize the latency
        // of the swap chain.
        m_deviceResources->WaitOnSwapChain();

        // Process any UI events in the queue.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

        // Update app state in response to any UI events that occurred.
        m_main->Update();

        // Render the scene.
        m_main->Render();

        // Present the scene.
        m_deviceResources->Present();
    }
    else
    {
        // The window is hidden. Block until a UI event occurs.
        CoreWindow::GetForCurrentThread()->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessOneAndAllPending);
    }
}
```

以下示例显示来自 DirectXLatency 示例的 WaitForSingleObjectEx 调用：

```cpp
// Block the current thread until the swap chain has finished presenting.
void DX::DeviceResources::WaitOnSwapChain()
{
    DWORD result = WaitForSingleObjectEx(
        m_frameLatencyWaitableObject,
        1000, // 1 second timeout (shouldn't ever occur)
        true
        );
}
```

## <a name="what-should-my-game-do-while-it-waits-for-the-swap-chain-to-present"></a>我的游戏在等待交换链显示时应执行哪些操作？


如果你的游戏没有任何任务在呈现循环上被阻止，最好让它等待交换链显示，因为这样可以省电，这在移动设备上尤为重要。 否则，你可以在游戏等待交换链显示期间使用多线程处理来完成工作。 以下是你的游戏可以完成的一些任务：

-   处理网络事件
-   更新 AI
-   基于 CPU 的物理任务
-   延迟的内容呈现（在支持的设备上）
-   资源加载

有关 Windows 中多线程编程的详细信息，请参阅以下相关主题。

## <a name="related-topics"></a>相关主题


* [DirectXLatency 示例](https://code.msdn.microsoft.com/windowsapps/DirectXLatency-sample-a2e2c9c3)
* [**IDXGISwapChain2::GetFrameLatencyWaitableObject**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgiswapchain2-getframelatencywaitableobject)
* [**WaitForSingleObjectEx**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobjectex)
* [**Windows**](https://docs.microsoft.com/uwp/api/Windows.System.Threading)
* [中的异步编程C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps)
* [进程和线程](https://docs.microsoft.com/windows/desktop/ProcThread/processes-and-threads)
* [同步](https://docs.microsoft.com/windows/desktop/Sync/synchronization)
* [使用事件对象（Windows）](https://docs.microsoft.com/windows/desktop/Sync/using-event-objects)

 

 




