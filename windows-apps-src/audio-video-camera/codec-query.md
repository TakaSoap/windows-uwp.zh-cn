---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: 查询设备上所安装的音频和视频编码器和解码器。
title: 查询已安装的编解码器
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 编解码器, 编码器, 解码器, 查询
ms.localizationpriority: medium
ms.openlocfilehash: f0f1ddff8336594e62ee26b6bf62b062039bf857
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363990"
---
# <a name="query-for-codecs-installed-on-a-device"></a>查询设备上安装的编解码器
**[CodecQuery](/uwp/api/windows.media.core.codecquery)** 类允许查询在当前设备上安装的编解码器。 文章[支持的编解码器](supported-codecs.md)中给出了 Windows 10 附带的用于不同设备系列的编解码器列表，但是由于用户和应用可以在设备上安装其他编解码器，因此你可能要在运行时查询编解码器支持，以确定当前设备上可用的编解码器。

CodecQuery API 是 **[Windows. Core](/uwp/api/windows.media.core)** 命名空间的成员，因此需要在应用中包含此命名空间。

CodecQuery API 是 **[Windows. Core](/uwp/api/windows.media.core)** 命名空间的成员，因此需要在应用中包含此命名空间。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetCodecQueryUsing":::

通过调用构造函数来初始化 **CodecQuery** 类的新实例。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetNewCodecQuery":::

**[FindAllAsync](/uwp/api/windows.media.core.codecquery.findallasync)** 方法返回与提供的参数相匹配的所有已安装的编解码器。 这些参数包括指定是要查询音频或视频编解码器还是同时查询两者的 **[CodecKind](/uwp/api/windows.media.core.codeckind)** 值， **[CodecCategory](/uwp/api/windows.media.core.codeccategory)** 值指定是要查询编码器还是解码器，并指定表示要查询的媒体编码子类型的字符串，例如：

为子类型值指定空字符串或 null 以返回适用于所有子类型的编解码器。 以下示例列出了设备上安装的所有视频编码器。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetFindAllEncoders":::

传入到 **FindAllAsync** 的子类型字符串可以是系统定义的子类型 GUID 的字符串表示形式或子类型的 FOURCC 代码。 " [音频子类型 guid](/windows/desktop/medfound/audio-subtype-guids) " 和 " [视频子](/windows/desktop/medfound/video-subtype-guids)类型" guid 一文中列出了一组受支持的媒体子类型 guid，但 **[CodecSubtypes](/uwp/api/windows.media.core.codecsubtypes)** 类提供了为每个受支持的子类型返回 GUID 值的属性。 有关 FOURCC 代码的详细信息，请参阅 [FOURCC 代码](/windows/desktop/DirectShow/fourcc-codes) 

以下示例指定 FOURCC 代码“H264”，以确定是否在设备上安装了 H.264 视频解码器。 可以在尝试播放 H.264 视频内容之前执行此查询。 还可以在播放时处理不支持的编解码器。 有关详细信息，请参阅[在打开媒体项时，处理不受支持的编解码器和未知错误](./media-playback-with-mediasource.md#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsH264Supported":::

以下示例中，进行查询的目的是确定当前设备上是否已安装 FLAC 音频编码器。如果已安装，则为子类型创建 **[MediaEncodingProfile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)**，它可用于将音频捕获到文件中或是将音频从另一种格式转码为 FLAC 音频文件。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsFLACSupported":::

## <a name="related-topics"></a>相关主题

* [媒体播放](media-playback.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [转换媒体文件代码](transcode-media-files.md)
* [支持的编解码器](supported-codecs.md)
 

 
