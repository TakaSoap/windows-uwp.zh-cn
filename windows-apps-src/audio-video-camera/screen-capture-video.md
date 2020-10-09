---
title: 屏幕捕获到视频
description: 本文介绍如何使用 Windows 将从屏幕捕获的帧编码为视频文件。
ms.date: 07/28/2020
ms.topic: article
dev_langs:
- csharp
keywords: windows 10、uwp、屏幕捕获、视频
ms.localizationpriority: medium
ms.openlocfilehash: 9f95e310fb93292db7dc348493487fada9c6d66e
ms.sourcegitcommit: 83eb36047380501fd1e4d023d593904ad783365b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/08/2020
ms.locfileid: "91852374"
---
# <a name="screen-capture-to-video"></a>屏幕捕获到视频

本文介绍如何使用 Windows 将从屏幕捕获的帧编码为视频文件。 有关屏幕捕获静态图像的信息，请参阅 [Screeen 捕获](./screen-capture.md)。 有关利用本文中所述的概念和技术的简单的端到端示例应用，请参阅 [SimpleRecorder](https://github.com/MicrosoftDocs/SimpleRecorder/)。

## <a name="overview-of-the-video-capture-process"></a>视频捕获过程概述
本文提供了一个示例应用的演练，该应用将窗口内容记录到视频文件中。 虽然实现此方案可能需要大量代码，但屏幕录制器应用程序的高级结构非常简单。 屏幕捕获进程使用三个主要 UWP 功能：

- [GraphicsCapture](/uwp/api/windows.graphics.capture) api 执行的工作实际上是从屏幕捕获像素。 [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem)类表示正在捕获的窗口或显示。 [GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) 用于启动和停止捕获操作。 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool)类维护要将屏幕内容复制到其中的帧的缓冲区。
- [MediaStreamSource](/uwp/api/windows.media.core.mediastreamsource)类接收捕获的帧并生成视频流。
- [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder)类接收由**MediaStreamSource**生成的流，并将其编码为视频文件。

本文中所示的示例代码可以分为几个不同的任务：

- **初始化** -这包括配置上述 UWP 类，初始化图形设备接口，选取要捕获的窗口，并设置编码参数，如分辨率和帧速率。
- **事件处理程序和线程处理**-主捕获循环的主要驱动程序是通过[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)事件定期请求帧的**MediaStreamSource** 。 此示例使用事件在示例的不同组件之间协调新帧的请求。 同步对于允许同时捕获和编码帧很重要。
- **复制帧** -从捕获帧缓冲区将帧复制到可传递到 **MediaStreamSource** 的单独 Direct3D 面，以便在编码时不覆盖资源。 Direct3D Api 用于快速执行此复制操作。 

### <a name="about-the-direct3d-apis"></a>关于 Direct3D Api
如上所述，每个捕获的帧的复制可能是本文中所示的实现中最复杂的部分。 在较低的级别，此操作使用 Direct3D 完成。 在此示例中，我们使用 [SharpDX](http://sharpdx.org/) 库从 c # 中执行 Direct3D 操作。 不再正式支持此库，但已选择此库，因为它是低级别复制操作的性能，非常适合于这种情况。 我们已尝试使 Direct3D 操作尽可能离散，使你可以更轻松地替代你自己的代码或其他库来执行这些任务。

## <a name="setting-up-your-project"></a>设置项目
本演练中的示例代码是使用空白应用在 Visual Studio 2019 中 ** (通用 Windows) ** c # 项目模板创建的。 若要在 **您的应用** 程序中使用 appxmanifest.xml api，必须在您的项目的包文件中包含 **图形捕获** 功能。 此示例将生成的视频文件保存到设备上的视频库。 若要访问此文件夹，必须包含 **视频库** 功能。

若要安装 SharpDX Nuget 包，请在 Visual Studio 中选择 " **管理 Nuget 包**"。 在 "浏览" 选项卡中搜索 "SharpDX. Direct3D11" 包，然后单击 " **安装**"。

请注意，为了减小本文中代码列表的大小，以下演练中的代码省略了显式命名空间引用，并使用前导下划线 "_" 命名了 MainPage 类成员变量的声明。

## <a name="setup-for-encoding"></a>编码设置

本节中所述的 **SetupEncoding** 方法初始化一些主要对象，这些对象将用于捕获和编码视频帧，并为捕获的视频设置编码参数。 此方法可通过编程方式或以响应用户交互（如按钮单击）的方式进行调用。 下面显示了 **SetupEncoding** 的代码列表，如下所示。

- **检查捕获支持。** 在开始捕获过程之前，需要调用 [GraphicsCaptureSession IsSupported](/uwp/api/windows.graphics.capture.graphicscapturesession.issupported) 以确保当前设备支持屏幕捕获功能。

- **初始化 Direct3D 接口。** 此示例使用 Direct3D 将从屏幕捕获的像素复制为编码为视频帧的纹理。 本文的稍后部分将演示用于初始化 Direct3D 接口 **CreateD3DDevice** 和 **CreateSharpDXDevice**的帮助器方法。

- **初始化 GraphicsCaptureItem。** [GraphicsCaptureItem](/uwp/api/windows.graphics.capture.graphicscaptureitem)表示屏幕上将捕获的一项（窗口或整个屏幕）。 允许用户通过创建 [GraphicsCapturePicker](/uwp/api/windows.graphics.capture.graphicscapturepicker) 并调用 [PickSingleItemAsync](/uwp/api/windows.graphics.capture.graphicscapturepicker.picksingleitemasync)来选择要捕获的项。

- **创建组合纹理。** 创建将用于复制每个视频帧的纹理资源和关联的呈现目标视图。 只有在创建 **GraphicsCaptureItem** 并知道其维度后，才能创建此纹理。 请参阅 **WaitForNewFrame** 的说明，了解如何使用此撰写纹理。 本文后面还显示了用于创建此纹理的帮助器方法。

- **创建 MediaEncodingProfile 和 VideoStreamDescriptor。** [MediaStreamSource](/uwp/api/windows.media.core.mediastreamsource)类的实例将获取从屏幕捕获的图像，并将其编码到视频流中。 然后， [MediaTranscoder](/uwp/api/windows.media.transcoding.mediatranscoder) 类将视频流转码到视频文件中。 [VideoStreamDecriptor](/uwp/api/windows.media.core.videostreamdescriptor)为**MediaStreamSource**提供编码参数，如分辨率和帧速率。 使用[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)指定**MediaTranscoder**的视频文件编码参数。 请注意，用于视频编码的大小不必与正在捕获的窗口大小相同，但是为了使此示例简单，编码设置是硬编码的，以使用捕获项的实际维度。

- **创建 MediaStreamSource 和 MediaTranscoder 对象。** 如上所述， **MediaStreamSource** 对象将单个帧编码到视频流中。 调用此类的构造函数，并传入在上一步中创建的 **MediaEncodingProfile** 。 将缓冲时间设置为零，并注册[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)事件[的处理](/uwp/api/windows.media.core.mediastreamsource.starting)程序，这将在本文的后面部分进行说明。 接下来，构造 **MediaTranscoder** 类的新实例，并启用硬件加速。

- **创建输出文件** 此方法中的最后一步是创建一个文件，该文件将转码视频。 在此示例中，我们将只在设备上的视频库文件夹中创建一个唯一命名的文件。 请注意，若要访问此文件夹，你的应用程序必须在应用程序清单中指定 "视频库" 功能。 创建文件后，将其打开以进行读取和写入，并将生成的流传递到 **EncodeAsync** 方法中，接下来将显示该方法。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SetupEncoding":::

## <a name="start-encoding"></a>开始编码
既然已经初始化了主对象，就会实现 **EncodeAsync** 方法以启动捕获操作。 此方法首先检查以确保未记录，如果不是，则会调用 helper 方法 **StartCapture** 从屏幕开始捕获帧。 此方法将在本文的后面部分显示。 接下来，调用[PrepareMediaStreamSourceTranscodeAsync](/uwp/api/windows.media.transcoding.mediatranscoder.preparemediastreamsourcetranscodeasync) ，以使用**MediaTranscoder**我们在上一节中创建的编码配置文件将**MediaStreamSource**对象生成的视频流转码到输出文件流。 准备好转码器后，调用 [TranscodeAsync](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 开始转码。 有关使用 **MediaTranscoder**的详细信息，请参阅 [转码 media files](./transcode-media-files.md)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_EncodeAsync":::

## <a name="handle-mediastreamsource-events"></a>处理 MediaStreamSource 事件
**MediaStreamSource**对象采用从屏幕捕获的帧，并将其转换为可使用**MediaTranscoder**保存到文件的视频流。 我们通过对象的事件的处理程序将帧传递到 **MediaStreamSource** 。

为新的视频帧准备好**MediaStreamSource**时，将引发[SampleRequested](/uwp/api/windows.media.core.mediastreamsource.samplerequested)事件。 确保当前正在记录，将调用帮助程序方法 **WaitForNewFrame** ，以获取从屏幕捕获的新帧。 本文后面所示，此方法将返回包含捕获帧的 [ID3D11Surface](/uwp/api/Windows.Graphics.DirectX.Direct3D11.IDirect3DSurface) 对象。 在此示例中，我们将 **IDirect3DSurface** 接口包装在帮助程序类中，此类还存储捕获帧的系统时间。 帧和系统时间都传递到 CreateFromDirect3D11Surface 工厂方法中，生成的[MediaStreamSample](/uwp/api/windows.media.core.mediastreamsample)设置为[MediaStreamSourceSampleRequestedEventArgs](/uwp/api/windows.media.core.mediastreamsourcesamplerequestedeventargs)的[MediaStreamSample](/uwp/api/windows.media.core.mediastreamsample.createfromdirect3d11surface) [属性。](/uwp/api/windows.media.core.mediastreamsourcesamplerequest.sample) 这就是将捕获的帧提供给 **MediaStreamSource**的方式。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceSampleRequested":::

在 [开始](/uwp/api/windows.media.core.mediastreamsource.starting) 事件的处理程序中，我们调用 **WaitForNewFrame**，但只将帧捕获到的系统时间传递到 [SetActualStartPosition](/uwp/api/windows.media.core.mediastreamsourcestartingrequest.setactualstartposition) 方法， **MediaStreamSource** 使用该方法对后续帧的计时进行正确编码。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnMediaStreamSourceStarting":::

## <a name="start-capturing"></a>开始捕获
此步骤中显示的 **StartCapture** 方法是从上一步中所示的 **EncodeAsync** helper 方法调用的。 首先，此方法初始化一组用于控制捕获操作流的事件对象。

- **_multithread** 是一种帮助器类，用于包装 SharpDX 库的多 **线程** 对象，此类对象将用于确保其他线程在复制时无法访问 SharpDX 纹理。
- **_frameEvent** 用于指示已捕获新帧并可传递到 **MediaStreamSource**
- **_closedEvent** 指示记录已停止，并且我们不应等待任何新帧。

将帧事件和已关闭事件添加到一个数组中，以便我们可以在捕获循环中等待其中任一项。

**StartCapture**方法的其余部分将设置 Windows。捕获将执行实际屏幕捕获的 api。 首先，为 **CaptureItem** 事件注册事件。 接下来，创建一个 [Direct3D11CaptureFramePool](/uwp/api/windows.graphics.capture.direct3d11captureframepool) ，它允许一次缓冲多个捕获的帧。 [CreateFreeThreaded](/uwp/api/windows.graphics.capture.direct3d11captureframepool.createfreethreaded)方法用于创建框架池，以便在池自己的工作线程而不是应用程序的主线程上调用[FrameArrived](/uwp/api/windows.graphics.capture.direct3d11captureframepool.framearrived)事件。 接下来，为 **FrameArrived** 事件注册处理程序。 最后，为所选**CaptureItem**创建[GraphicsCaptureSession](/uwp/api/windows.graphics.capture.graphicscapturesession) ，并通过调用[StartCapture](/uwp/api/windows.graphics.capture.graphicscapturesession)启动帧捕获。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_StartCapture":::

## <a name="handle-graphics-capture-events"></a>处理图形捕获事件
在上一步中，我们为图形捕获事件注册了两个处理程序，并设置了一些事件以帮助管理捕获循环的流。

当**Direct3D11CaptureFramePool**具有新的捕获帧时，将引发**FrameArrived**事件。 在此事件的处理程序中，调用发送方上的 [TryGetNextFrame](/uwp/api/windows.graphics.capture.direct3d11captureframepool.trygetnextframe) 以获取下一个捕获的帧。 检索到帧后，我们设置 **_frameEvent** 以便捕获循环知道有新帧可用。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnFrameArrived":::

在 **关闭** 的事件处理程序中，我们向 **_closedEvent** 发出信号，以便捕获循环知道何时停止。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_OnClosed":::

## <a name="wait-for-new-frames"></a>等待新帧
此部分中所述的 **WaitForNewFrame** helper 方法是发生大量捕获循环的位置。 请记住，每当**MediaStreamSource**准备好将新帧添加到视频流时，将从**OnMediaStreamSourceSampleRequested**事件处理程序调用此方法。 在高级别，此函数只是将每个屏幕捕获的视频帧从一个 Direct3D surface 面复制到另一个，以便在捕获新帧时可以将其传递到 **MediaStreamSource** 进行编码。 此示例使用 SharpDX 库来执行实际复制操作。

在等待新帧之前，方法会释放类变量中存储的任何以前的帧， **_currentFrame**，并重置 **_frameEvent**。 然后，该方法将等待 **_frameEvent** 或要发出信号的 **_closedEvent** 。 如果设置了已关闭的事件，应用程序将调用帮助程序方法来清理捕获资源。 此方法将在本文的后面部分显示。

如果设置了帧事件，我们知道已经调用了上一步中定义的 **FrameArrived** 事件处理程序，并且我们开始将捕获的帧数据复制到将传递到 **MediaStreamSource**的 Direct3D 11 图面。

此示例使用 helper 类**SurfaceWithInfo**，它只允许我们将帧的视频帧和系统时间传递给 MediaStreamSource，这两个对象都是**MediaStreamSource** 。 帧复制过程的第一步是实例化此类，并设置系统时间。

接下来的步骤是此示例中特别依赖于 SharpDX 库的部分。 本文末尾定义了此处使用的帮助程序函数。 首先，我们使用 **MultiThreadLock** 来确保在进行复制时没有其他线程访问视频帧缓冲区。 接下来，我们调用 helper 方法 **CreateSharpDXTexture2D** 从视频帧创建 SharpDX **Texture2D** 对象。 这将是复制操作的源纹理。 

接下来，我们将在上一步中创建的 **Texture2D** 对象复制到我们之前在此过程中创建的撰写纹理。 此撰写纹理充当交换缓冲区，以便在捕获下一帧时编码过程可以对像素进行运算。 若要执行复制，请清除与复合纹理关联的呈现目标视图，然后在要复制的纹理中定义区域（在这种情况下为整个纹理），然后我们调用 **CopySubresourceRegion** 以实际将像素复制到撰写纹理。

我们创建纹理说明的副本，以便在创建目标纹理时使用，但会修改说明，将 **BindFlags** 设置为 **RenderTarget** ，使新纹理具有写入访问权限。 如果将 **CpuAccessFlags** 设置为 **None** ，则系统将优化复制操作。 纹理说明用于创建新的纹理资源，并通过调用 **CopyResource**将合成纹理资源复制到此新资源中。 最后，调用 **CreateDirect3DSurfaceFromSharpDXTexture** 来创建从该方法返回的 **IDirect3DSurface** 对象。


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_WaitForNewFrame":::

## <a name="stop-capture-and-clean-up-resources"></a>停止捕获并清理资源
**Stop**方法提供一种停止捕获操作的方法。 应用可以通过编程方式或响应用户交互（如按钮单击）来调用此。 此方法只是设置 **_closedEvent**。 在前面的步骤中定义的 **WaitForNewFrame** 方法将查找此事件，如果已设置，则将关闭捕获操作。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Stop":::

**清理**方法用于正确释放在复制操作过程中创建的资源。 这包括：

- 捕获会话使用的 **Direct3D11CaptureFramePool** 对象
- **GraphicsCaptureSession**和**GraphicsCaptureItem**
- Direct3D 和 SharpDX 设备
- 复制操作中使用的 SharpDX 纹理和呈现目标视图。
- 用于存储当前帧的 **Direct3D11CaptureFrame** 。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_Cleanup":::

## <a name="helper-wrapper-classes"></a>Helper 包装类
以下帮助器类定义为有助于处理本文中的示例代码。

**MultithreadLock** helper 类**包装 SharpDX 多线程类**，以确保其他线程在复制时不会访问纹理资源。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_MultithreadLock":::

**SurfaceWithInfo** 用于将 **IDirect3DSurface** 关联到表示捕获的帧的 **SystemRelativeTime** ，以及分别捕获捕获的帧的时间。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/MainPage.xaml.cs" id="snippet_SurfaceWithInfo":::

## <a name="direct3d-and-sharpdx-helper-apis"></a>Direct3D 和 SharpDX helper Api
以下帮助器 Api 定义为抽象出 Direct3D 和 SharpDX 资源的创建。 有关这些技术的详细说明不在本文的讨论范围内，但此处提供了代码，以使你能够实现本演练中所示的示例代码。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ScreenRecorderExample/cs/Direct3D11Helpers.cs" id="snippet_Direct3D11Helpers":::

## <a name="see-also"></a>请参阅

* [Windows.Graphics.Capture 命名空间](/uwp/api/windows.graphics.capture)
* [屏幕捕获](screen-capture.md)