---
title: 设置 DirectX 资源和显示图像
description: 下面我们将为你介绍如何创建 Direct3D 设备、交换链和呈现目标视图，以及如何向屏幕显示呈现的图像。
ms.assetid: d54d96fe-3522-4acb-35f4-bb11c3a5b064
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, directx, 资源, 图像
ms.localizationpriority: medium
ms.openlocfilehash: cced3b5cb6ad9c3e1ffe077887c5f23ce95745bd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159191"
---
# <a name="set-up-directx-resources-and-display-an-image"></a>设置 DirectX 资源和显示图像



下面我们将为你介绍如何创建 Direct3D 设备、交换链和呈现目标视图，以及如何向屏幕显示呈现的图像。

**目标：** 在 C++ 通用 Windows 平台 (UWP) 应用中设置 DirectX 资源并显示纯色。

## <a name="prerequisites"></a>必备条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

**完成所需时间：** 20 分钟。

## <a name="instructions"></a>Instructions

### <a name="1-declaring-direct3d-interface-variables-with-comptr"></a>1. 使用 ComPtr 声明 Direct3D 接口变量

我们使用 Windows 运行时 C++ 模板库 (WRL) 中的 ComPtr [智能指针](/cpp/cpp/smart-pointers-modern-cpp)模板来声明 Direct3D 接口变量，以便可以采用防异常的方式管理这些变量的生存时间。 然后，使用这些变量访问 [**ComPtr class**](/cpp/windows/comptr-class) 及其成员。 例如：

```cpp
    ComPtr<ID3D11RenderTargetView> m_renderTargetView;
    m_d3dDeviceContext->OMSetRenderTargets(
        1,
        m_renderTargetView.GetAddressOf(),
        nullptr // Use no depth stencil.
        );
```

如果将[**ID3D11RenderTargetView**](/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)声明为 ComPtr，则可以使用 ComPtr 的**GetAddressOf**方法获取指向**ID3D11RenderTargetView** (ID3D11RenderTargetView) 的指针的地址，以将其 \* \* 传递给[**ID3D11DeviceContext：： OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets)。 **OMSetRenderTargets** 会将呈现器目标绑定到[输出合并阶段](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-output-merger-stage)，以将呈现器目标指定为输出目标。

在启动示例应用之后，该示例应用将初始化并加载，然后就可以运行了。

### <a name="2-creating-the-direct3d-device"></a>2. 创建 Direct3D 设备

若要使用 Direct3D API 来呈现场景，必须首先创建一个代表显示适配器的 Direct3D 设备。 要创建 Direct3D 设备，需要调用 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 函数。 我们在 [**D3D \_ 功能 \_ 级别**](/windows/desktop/api/d3dcommon/ne-d3dcommon-d3d_feature_level) 值的数组中指定级别9.1 到11.1。 Direct3D 按顺序检查数组并返回支持的最高功能级别。 因此，为了获得最高的可用功能级别，我们列出了 **D3D \_ 功能 \_ 级别** 数组项从最高到最低。 我们会将 [**D3D11 \_ CREATE \_ DEVICE \_ BGRA \_ 支持**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) 标志传递给 *Flags* 参数，使 Direct3D 资源与 Direct2D 进行互操作。 如果我们使用调试版本，还将传递 [**D3D11 \_ 创建 \_ 设备 \_ 调试**](/windows/desktop/api/d3d11/ne-d3d11-d3d11_create_device_flag) 标志。 有关调试应用的详细信息，请参阅[使用调试层来调试应用](/windows/desktop/direct3d11/using-the-debug-layer-to-test-apps)。

通过查询从 [**D3D11CreateDevice**](/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) 返回的 Direct3D 11 设备和设备上下文来获取 Direct3D 11.1 设备 ([**ID3D11Device1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1)) 和设备上下文 ([**ID3D11DeviceContext1**](/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1))。

```cpp
        // First, create the Direct3D device.

        // This flag is required in order to enable compatibility with Direct2D.
        UINT creationFlags = D3D11_CREATE_DEVICE_BGRA_SUPPORT;

#if defined(_DEBUG)
        // If the project is in a debug build, enable debugging via SDK Layers with this flag.
        creationFlags |= D3D11_CREATE_DEVICE_DEBUG;
#endif

        // This array defines the ordering of feature levels that D3D should attempt to create.
        D3D_FEATURE_LEVEL featureLevels[] =
        {
            D3D_FEATURE_LEVEL_11_1,
            D3D_FEATURE_LEVEL_11_0,
            D3D_FEATURE_LEVEL_10_1,
            D3D_FEATURE_LEVEL_10_0,
            D3D_FEATURE_LEVEL_9_3,
            D3D_FEATURE_LEVEL_9_1
        };

        ComPtr<ID3D11Device> d3dDevice;
        ComPtr<ID3D11DeviceContext> d3dDeviceContext;
        DX::ThrowIfFailed(
            D3D11CreateDevice(
                nullptr,                    // Specify nullptr to use the default adapter.
                D3D_DRIVER_TYPE_HARDWARE,
                nullptr,                    // leave as nullptr if hardware is used
                creationFlags,              // optionally set debug and Direct2D compatibility flags
                featureLevels,
                ARRAYSIZE(featureLevels),
                D3D11_SDK_VERSION,          // always set this to D3D11_SDK_VERSION
                &d3dDevice,
                nullptr,
                &d3dDeviceContext
                )
            );

        // Retrieve the Direct3D 11.1 interfaces.
        DX::ThrowIfFailed(
            d3dDevice.As(&m_d3dDevice)
            );

        DX::ThrowIfFailed(
            d3dDeviceContext.As(&m_d3dDeviceContext)
            );
```

### <a name="3-creating-the-swap-chain"></a>3. 创建交换链

接下来，创建设备用于呈现和显示的交换链。 我们声明并初始化一个 [**DXGI \_ 交换 \_ 链 \_ DESC1**](/windows/desktop/api/dxgi1_2/ns-dxgi1_2-dxgi_swap_chain_desc1) 结构，用于描述交换链。 接下来，我们将交换链设置为翻转模型 (即，具有 [**DXGI \_ 交换 \_ 效果 \_ \_ **](/windows/desktop/api/dxgi/ne-dxgi-dxgi_swap_effect) 的交换链) 在 **SwapEffect** 成员中设置的顺序值，并将 **format** 成员设置为 [**DXGI \_ format \_ B8G8R8A8 \_ UNORM**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)。 我们将**SampleDesc**成员指定的[**dxgi \_ 示例 \_ desc**](/windows/desktop/api/dxgicommon/ns-dxgicommon-dxgi_sample_desc)结构的**Count**成员设置为1，将**dxgi \_ 示例 \_ desc**的**Quality**成员设置为零，因为翻转模型不支持 (MSAA) 的多个样本抗失真。 将 **BufferCount** 成员设置为 2，让交换链可以使用前台缓冲区来向显示设备显示，并使用后台缓冲区充当呈现器目标。

通过查询 Direct3D 11.1 设备来获取基本 DXGI 设备。 为了最大程度降低电耗（对于笔记本电脑和平板电脑等使用电池供电的设备，这样做很重要），将 1 作为 DXGI 可排队的后台缓冲区帧的最大数量来调用 [**IDXGIDevice1::SetMaximumFrameLatency**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice1-setmaximumframelatency) 方法。 这样可确保仅在垂直空白之后才呈现应用。

最后，要创建交换链，需要从 DXGI 设备获取父工厂。 调用 [**IDXGIDevice::GetAdapter**](/windows/desktop/api/dxgi/nf-dxgi-idxgidevice-getadapter) 来获取设备的适配器，然后在适配器上调用 [**IDXGIObject::GetParent**](/windows/desktop/api/dxgi/nf-dxgi-idxgiobject-getparent) 来获取父工厂 ([**IDXGIFactory2**](/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2))。 要创建交换链，需要使用交换链描述符和应用的核心窗口来调用 [**IDXGIFactory2::CreateSwapChainForCoreWindow**](/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgifactory2-createswapchainforcorewindow)。

```cpp
            // If the swap chain does not exist, create it.
            DXGI_SWAP_CHAIN_DESC1 swapChainDesc = {0};

            swapChainDesc.Stereo = false;
            swapChainDesc.BufferUsage = DXGI_USAGE_RENDER_TARGET_OUTPUT;
            swapChainDesc.Scaling = DXGI_SCALING_NONE;
            swapChainDesc.Flags = 0;

            // Use automatic sizing.
            swapChainDesc.Width = 0;
            swapChainDesc.Height = 0;

            // This is the most common swap chain format.
            swapChainDesc.Format = DXGI_FORMAT_B8G8R8A8_UNORM;

            // Don't use multi-sampling.
            swapChainDesc.SampleDesc.Count = 1;
            swapChainDesc.SampleDesc.Quality = 0;

            // Use two buffers to enable the flip effect.
            swapChainDesc.BufferCount = 2;

            // We recommend using this swap effect for all applications.
            swapChainDesc.SwapEffect = DXGI_SWAP_EFFECT_FLIP_SEQUENTIAL;


            // Once the swap chain description is configured, it must be
            // created on the same adapter as the existing D3D Device.

            // First, retrieve the underlying DXGI Device from the D3D Device.
            ComPtr<IDXGIDevice2> dxgiDevice;
            DX::ThrowIfFailed(
                m_d3dDevice.As(&dxgiDevice)
                );

            // Ensure that DXGI does not queue more than one frame at a time. This both reduces
            // latency and ensures that the application will only render after each VSync, minimizing
            // power consumption.
            DX::ThrowIfFailed(
                dxgiDevice->SetMaximumFrameLatency(1)
                );

            // Next, get the parent factory from the DXGI Device.
            ComPtr<IDXGIAdapter> dxgiAdapter;
            DX::ThrowIfFailed(
                dxgiDevice->GetAdapter(&dxgiAdapter)
                );

            ComPtr<IDXGIFactory2> dxgiFactory;
            DX::ThrowIfFailed(
                dxgiAdapter->GetParent(IID_PPV_ARGS(&dxgiFactory))
                );

            // Finally, create the swap chain.
            CoreWindow^ window = m_window.Get();
            DX::ThrowIfFailed(
                dxgiFactory->CreateSwapChainForCoreWindow(
                    m_d3dDevice.Get(),
                    reinterpret_cast<IUnknown*>(window),
                    &swapChainDesc,
                    nullptr, // Allow on all displays.
                    &m_swapChain
                    )
                );
```

### <a name="4-creating-the-render-target-view"></a>4. 创建呈现器目标视图

要将图形呈现到窗口，需要创建一个呈现器目标视图。 调用 [**IDXGISwapChain::GetBuffer**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-getbuffer) 来获取交换链接的后台缓冲区，以便在创建呈现器目标视图时使用。 将后台缓冲区指定为一个 2D 纹理 ([**ID3D11Texture2D**](/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d))。 要创建呈现器目标视图，需要使用交换链的后台缓冲区调用 [**ID3D11Device::CreateRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createrendertargetview)。 必须通过指定 view 端口 ([**D3D11 \_ 视区**](/windows/desktop/api/d3d11/ns-d3d11-d3d11_viewport) 来将其绘制到整个核心窗口，) 为交换链的后台缓冲区的完整大小。 在对 [**ID3D11DeviceContext::RSSetViewports**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-rssetviewports) 的调用中使用视区将视区绑定到管道的[光栅化阶段](/windows/desktop/direct3d11/d3d10-graphics-programming-guide-rasterizer-stage)。 光栅化阶段将向量信息转换为光栅图像。 此情况下不要求转换，因为我们只是显示纯色。

```cpp
        // Once the swap chain is created, create a render target view.  This will
        // allow Direct3D to render graphics to the window.

        ComPtr<ID3D11Texture2D> backBuffer;
        DX::ThrowIfFailed(
            m_swapChain->GetBuffer(0, IID_PPV_ARGS(&backBuffer))
            );

        DX::ThrowIfFailed(
            m_d3dDevice->CreateRenderTargetView(
                backBuffer.Get(),
                nullptr,
                &m_renderTargetView
                )
            );


        // After the render target view is created, specify that the viewport,
        // which describes what portion of the window to draw to, should cover
        // the entire window.

        D3D11_TEXTURE2D_DESC backBufferDesc = {0};
        backBuffer->GetDesc(&backBufferDesc);

        D3D11_VIEWPORT viewport;
        viewport.TopLeftX = 0.0f;
        viewport.TopLeftY = 0.0f;
        viewport.Width = static_cast<float>(backBufferDesc.Width);
        viewport.Height = static_cast<float>(backBufferDesc.Height);
        viewport.MinDepth = D3D11_MIN_DEPTH;
        viewport.MaxDepth = D3D11_MAX_DEPTH;

        m_d3dDeviceContext->RSSetViewports(1, &viewport);
```

### <a name="5-presenting-the-rendered-image"></a>5. 显示呈现的图像

我们将进入一个不断呈现和显示场景的无限循环。

在此循环中，我们调用：

1.  [**ID3D11DeviceContext::OMSetRenderTargets**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-omsetrendertargets) 以将呈现器目标指定为输出目标。
2.  [**ID3D11DeviceContext::ClearRenderTargetView**](/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-clearrendertargetview) 以将呈现目标清除为纯色。
3.  [**IDXGISwapChain::Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 以向窗口显示呈现的图像。

由于我们之前将最大帧延迟设置为 1，因此，Windows 通常会将呈现循环减慢至屏幕刷新速率，通常大约为 60 Hz。 Windows 通过在应用调用 [**Present**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-present) 时使应用进入睡眠状态来减慢呈现循环。 在刷新屏幕之前，Windows 会将应用保持为睡眠状态。

```cpp
        // Enter the render loop.  Note that UWP apps should never exit.
        while (true)
        {
            // Process events incoming to the window.
            m_window->Dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

            // Specify the render target we created as the output target.
            m_d3dDeviceContext->OMSetRenderTargets(
                1,
                m_renderTargetView.GetAddressOf(),
                nullptr // Use no depth stencil.
                );

            // Clear the render target to a solid color.
            const float clearColor[4] = { 0.071f, 0.04f, 0.561f, 1.0f };
            m_d3dDeviceContext->ClearRenderTargetView(
                m_renderTargetView.Get(),
                clearColor
                );

            // Present the rendered image to the window.  Because the maximum frame latency is set to 1,
            // the render loop will generally be throttled to the screen refresh rate, typically around
            // 60 Hz, by sleeping the application on Present until the screen is refreshed.
            DX::ThrowIfFailed(
                m_swapChain->Present(1, 0)
                );
        }
```

### <a name="6-resizing-the-app-window-and-the-swap-chains-buffer"></a>6. 调整应用窗口和交换链的缓冲区的大小

如果应用窗口大小发生改变，则应用必须调整交换链的缓冲区的大小，重新创建呈现器目标视图，然后显示大小调整后的呈现图像。 要调整交换链缓冲区的大小，需要调用 [**IDXGISwapChain::ResizeBuffers**](/windows/desktop/api/dxgi/nf-dxgi-idxgiswapchain-resizebuffers)。 在此调用中，我们将 *BufferCount* 参数的缓冲区数和缓冲区格式保持不变 (两个，并将 *NewFormat* 参数设置为 [**DXGI \_ format \_ B8G8R8A8 \_ UNORM**](/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)) 。 使交换链的后台缓冲区的大小与调整后的窗口大小相同。 在调整了交换链的缓冲区的大小之后，创建新的呈现器目标并显示新呈现的图像（与初始化应用时类似）。

```cpp
            // If the swap chain already exists, resize it.
            DX::ThrowIfFailed(
                m_swapChain->ResizeBuffers(
                    2,
                    0,
                    0,
                    DXGI_FORMAT_B8G8R8A8_UNORM,
                    0
                    )
                );
```

## <a name="summary-and-next-steps"></a>总结和后续步骤


我们创建了一个 Direct3D 设备、交换链和呈现器目标视图，并向显示器显示了呈现的图像。

接下来，我们还要在屏幕上绘制一个三角形。

[创建着色器和绘制基元](creating-shaders-and-drawing-primitives.md)

 

 