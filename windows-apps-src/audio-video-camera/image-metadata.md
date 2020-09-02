---
ms.assetid: D5D98044-7221-4C2A-9724-56E59F341AB0
description: 本文介绍如何读写图像元数据属性以及如何使用 GeotagHelper 实用程序类标注文件。
title: 图像元数据
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c020a2ca66c81bee81813402e546fc01ce77c7f3
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362560"
---
# <a name="image-metadata"></a>图像元数据



本文介绍如何使用 [**GeotagHelper**](/uwp/api/Windows.Storage.FileProperties.GeotagHelper) 实用程序类读取和写入图像元数据属性以及如何 geotag 文件。

## <a name="image-properties"></a>映像属性

[**StorageFile.Properties**](/uwp/api/windows.storage.storagefile.properties) 属性返回对关于文件的相关内容信息提供访问的 [**StorageItemContentProperties**](/uwp/api/Windows.Storage.FileProperties.StorageItemContentProperties) 对象。 通过调用 [**GetImagePropertiesAsync**](/uwp/api/windows.storage.fileproperties.storageitemcontentproperties.getimagepropertiesasync) 获取特定于图像的属性。 返回的 [**ImageProperties**](/uwp/api/Windows.Storage.FileProperties.ImageProperties) 对象公开包含基本图像元数据字段（如图像的标题和捕获日期）的成员。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetImageProperties":::

若要访问较大的文件元数据集，请使用 Windows 属性系统，一组可以使用唯一的字符串标识符进行检索的文件元数据。 创建字符串列表，并为想要检索的每个属性添加标识符。 [**ImageProperties.RetrievePropertiesAsync**](/uwp/api/windows.storage.fileproperties.imageproperties.retrievepropertiesasync) 方法采用此字符串列表，并返回键/值对的字典，其中键是属性标识符，值是属性值。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetWindowsProperties":::

-   有关 Windows 属性的完整列表（包括标识符和每个属性的类型），请参阅 [Windows 属性](/windows/desktop/properties/props)。

-   某些属性仅适用于某些文件容器和图像编解码器。 有关每种图像类型支持的图像元数据的列表，请参阅[照片元数据策略](/windows/desktop/wic/photo-metadata-policies)。

-   由于不受支持的属性在检索时可能会返回 Null 值，因此在使用返回的元数据值之前，始终检查 Null。

## <a name="geotag-helper"></a>地理标签帮助程序

GeotagHelper 是一个实用工具类，可以方便地直接使用 [**Windows.Devices.Geolocation**](/uwp/api/Windows.Devices.Geolocation) API 标记带有地理数据的图像，而不必手动分析或构建元数据格式。

如果你已拥有表示要在图像（来自先前使用的地理位置 API 或某些其他源）中标记的位置的 [**Geopoint**](/uwp/api/Windows.Devices.Geolocation.Geopoint) 对象，你可以通过调用 [**GeotagHelper.SetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagasync) 并传入 [**StorageFile**](/uwp/api/Windows.Storage.StorageFile) 和 **Geopoint** 来设置地理标签数据。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSetGeoDataFromPoint":::

若要使用设备的当前位置设置地理标签数据，则创建一个新的 [**Geolocator**](/uwp/api/Windows.Devices.Geolocation.Geolocator) 对象，并调用传入 **Geolocator** 和要标记文件的 [**GeotagHelper.SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetSetGeoDataFromGeolocator":::

-   必须在应用部件清单中包含 **location** 设备功能，以便使用 [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) API。

-   你必须在调用 [**SetGeotagFromGeolocatorAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.setgeotagfromgeolocatorasync) 之前调用 [**RequestAccessAsync**](/uwp/api/windows.devices.geolocation.geolocator.requestaccessasync)，以确保用户已授予你的应用使用他们的位置的权限。

-   有关地理位置 API 的详细信息，请参阅[地图和位置](../maps-and-location/index.md)。

若要获取表示图像文件的已加注地理标签位置的 GeoPoint，请调用 [**GetGeotagAsync**](/uwp/api/windows.storage.fileproperties.geotaghelper.getgeotagasync)。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetGetGeoData":::

## <a name="decode-and-encode-image-metadata"></a>解码和编码图像元数据

使用图像数据的最先进方式是使用 [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder) 或 [BitmapEncoder](bitmapencoder-options-reference.md) 在流级别上读写属性。 对于这些操作，你可以使用 Windows 属性来指定正在读写的数据，但你也可以使用由 Windows 图像处理组件 (WIC) 提供的元数据查询语言为请求的属性指定路径。

使用此技术读取图像元数据要求你拥有使用源图像文件流创建的 [**BitmapDecoder**](/uwp/api/Windows.Graphics.Imaging.BitmapDecoder)。 有关如何执行此操作的详细信息，请参阅[图像处理](imaging.md)。

安装解码器后，创建一个字符串列表，并为要使用 Windows 属性标识符字符串或 WIC 元数据查询进行检索的每个元数据属性添加一个新项。 调用解码器的 [**BitmapProperties**](/uwp/api/Windows.Graphics.Imaging.BitmapProperties) 成员上的 [**BitmapPropertiesView.GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) 方法以请求指定的属性。 在包含属性名称或路径和属性值的键/值对的字典中将返回属性。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetReadImageMetadata":::

-   有关 WIC 元数据查询语言和支持的属性的信息，请参阅 [WIC 图像格式本机元数据查询](/windows/desktop/wic/-wic-native-image-format-metadata-queries)。

-   许多元数据属性仅受图形类型的一个子集支持。 如果请求的属性中的某一个属性不受与解码器关联的图像支持，[**GetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmappropertiesview.getpropertiesasync) 将失败，错误代码为 0x88982F41；如果图像完全不支持元数据，错误代码为 0x88982F81。 与这些错误代码关联的常量为 WINCODEC \_ err \_ PROPERTYNOTSUPPORTED 和 WINCODEC \_ err \_ UNSUPPORTEDOPERATION，并在 winerror.h 头文件中定义。
-   由于不确定图像是否包含特定属性的值，在尝试访问它之前，请使用 **IDictionary.ContainsKey** 来验证属性是否出现在结果中。

将图像元数据写入流需要与图像输出文件关联的 **BitmapEncoder**。

创建 [**BitmapPropertySet**](/uwp/api/Windows.Graphics.Imaging.BitmapPropertySet) 对象以包含要设置的属性值。 创建 [**BitmapTypedValue**](/uwp/api/Windows.Graphics.Imaging.BitmapTypedValue) 对象以表示属性值。 此对象将 **object** 用作值和定义值类型的 [**PropertyType**](/uwp/api/Windows.Foundation.PropertyType) 枚举的成员。 将 **BitmapTypedValue** 添加到 **BitmapPropertySet**，然后调用 [**BitmapProperties.SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) 会导致编码器将属性写入流中。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/ImagingWin10/cs/MainPage.xaml.cs" id="SnippetWriteImageMetadata":::

-   有关图像文件类型支持的属性的详细信息，请参阅 [Windows 属性](/windows/desktop/properties/props)、[照片元数据策略](/windows/desktop/wic/photo-metadata-policies)和 [WIC 图像格式本机元数据查询](/windows/desktop/wic/-wic-native-image-format-metadata-queries)。

-   如果请求的属性之一不受与编码器关联的图像支持，[**SetPropertiesAsync**](/uwp/api/windows.graphics.imaging.bitmapproperties.setpropertiesasync) 将失败，错误代码为 0x88982F41。

## <a name="related-topics"></a>相关主题

* [映像](imaging.md)
 

 
