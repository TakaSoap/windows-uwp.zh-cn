---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: 你可以使用 Windows.Media.Transcoding API 将视频文件代码从一种格式转换为另一种格式。
title: 转换媒体文件代码
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 927ff8ceb8dc29400f5a7d0ede42b3ee8b703efb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175661"
---
# <a name="transcode-media-files"></a>转换媒体文件代码



可以使用 [**Windows.Media.Transcoding**](/uwp/api/Windows.Media.Transcoding) API 将视频文件代码从一种格式转换为另一种格式。

转换是指从一种格式*转换为数字*媒体文件（如视频或音频文件）的转换。 这通常是通过解码文件，然后再重新编码文件实现的。 例如，你可以将 Windows Media 文件转换为 MP4 文件，以使此文件可以在支持 MP4 格式的便携式设备上播放。 或者，你可以将高清视频文件转换为较低分辨率的文件。 在此情况下，重新编码的文件可能使用与原始文件相同的编解码器，但是其编码配置文件不同。

## <a name="set-up-your-project-for-transcoding"></a>设置你的项目以进行转换代码

除了由默认项目模板引用的命名空间，你还需要引用这些命名空间，以便使用本文中的代码转换媒体文件代码。

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>选择源文件和目标文件

应用确定要转换代码的源文件和目标文件的方式取决于实现。 此示例通过使用 [**FileOpenPicker**](/uwp/api/Windows.Storage.Pickers.FileOpenPicker) 和 [**FileSavePicker**](/uwp/api/Windows.Storage.Pickers.FileSavePicker) 让用户能够选取源文件和目标文件。

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>创建媒体编码配置文件

编码配置文件包含确定如何编码目标文件的设置。 转换文件代码时，此文件中包含的选项最多。

[**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 类提供用于创建预定义编码配置文件的静态方法：

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>用于创建纯音频编码配置文件的方法

方法  |配置文件  |
---------|---------|
[**CreateAlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple 无损音频编解码器 (ALAC) 音频         |
[**CreateFlac**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |免费无损音频编解码器 (FLAC) 音频。         |
[**CreateM4a**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC 音频 (M4A)         |
[**CreateMp3**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3 音频         |
[**CreateWav**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV 音频         |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media 音频 (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>用于创建音频/视频编码配置文件的方法

方法  |配置文件  |
---------|---------|
[**CreateAvi**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |高效率视频编码 (HEVC) 视频，也称为 H.265 视频 |
[**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4 视频（H.264 视频加 AAC 音频） |
[**CreateWmv**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media 视频 (WMV) |


以下代码为 MP4 视频创建配置文件。

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

静态 [**CreateMp4**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) 方法创建 MP4 编码配置文件。 此方法的参数为视频指定目标分辨率。 在这种情况下，[**VideoEncodingQuality.hd720p**](/uwp/api/Windows.Media.MediaProperties.VideoEncodingQuality) 表示在每秒 30 帧情况下的 1280 x 720 像素。 （“720p”表示每帧 720 个逐行扫描行。）用于创建预定义配置文件的其他方法均遵循此模式。

或者，你可以通过使用 [**MediaEncodingProfile.CreateFromFileAsync**](/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createfromfileasync) 方法来创建与现有媒体文件相匹配的配置文件。 或者，如果你知道所需的确切编码设置，则可以创建一个新的 [**MediaEncodingProfile**](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile) 对象，然后填写配置文件详细信息。

## <a name="transcode-the-file"></a>转换文件代码

若要转码文件，请创建新的 [**MediaTranscoder**](/uwp/api/Windows.Media.Transcoding.MediaTranscoder) 对象并调用 [**MediaTranscoder.PrepareFileTranscodeAsync**](/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync) 方法。 传入源文件、目标文件和编码配置文件。 然后，调用从异步转换代码操作中返回的 [**PrepareTranscodeResult**](/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult) 对象上的 [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 方法。

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>响应代码转换进度

你可以注册要在异步进度 [**TranscodeAsync**](/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) 发生更改时响应的事件。 这些事件是通用 Windows 平台 (UWP) 应用的异步编程框架的一部分，且不特定于 API 代码转换操作。

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]