---
title: 执行地理编码和反向地理编码
description: 本指南演示如何通过在地理编码命名空间中调用 MapLocationFinder 类的方法，将街道地址转换为地理位置 (地理编码) 并将地理位置转换为街道地址 (反向) 。
ms.assetid: B912BE80-3E1D-43BB-918F-7A43327597D2
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, uwp, 地理编码, 地图, 位置
ms.localizationpriority: medium
ms.openlocfilehash: 011a901e2baa9ff4b8f4a5bd8018b9b7790b1852
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162561"
---
# <a name="perform-geocoding-and-reverse-geocoding"></a>执行地理编码和反向地理编码

本指南演示如何[**通过在地理编码命名空间**](/uwp/api/Windows.Services.Maps)中调用[**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)类的方法，将街道地址转换为地理位置 (地理编码) 并将地理位置转换为街道地址 (反向) 。

> [!TIP]
> 若要详细了解如何在应用中使用映射，请从 GitHub 上的[Windows 通用示例](hhttps://github.com/Microsoft/Windows-universal-samples)存储库下载[MapControl](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)示例。

地理编码和反向地理编码中涉及的类按如下方式进行组织。

-   [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)类包含处理地理编码 ([**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)) 并反转地理编码 ([**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)) 的方法。
-   这些方法都返回 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 实例。
-   [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的[**位置**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)属性公开[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)对象的集合。 
-   [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 对象同时具有 [**Address**](/uwp/api/windows.services.maps.maplocation.address) 属性（用于公开表示街道地址的 [**MapAddress**](/uwp/api/Windows.Services.Maps.MapAddress) 对象）和 [**Point**](/uwp/api/windows.services.maps.maplocation.point) 属性（用于公开表示地理位置的 [**Geopoint**](/uwp/api/windows.devices.geolocation.geopoint) 对象）。

> [!IMPORTANT]
> 必须先指定地图验证密钥，才能使用地图服务。 有关详细信息，请参阅[请求地图身份验证密钥](authentication-key.md)。

## <a name="get-a-location-geocode"></a>获取位置（地理编码）

本部分说明如何将街道地址或地点名称转换为地理位置 (地理编码) 。

1.  使用位置名称或街道地址调用[**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder)类的[**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法的重载之一。
2.  [**FindLocationsAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)方法返回[**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)对象。
3.  使用[**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 "[**位置**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)" 属性来公开集合[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)对象。 可能有多个 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 对象，因为系统可能会找到与给定输入对应的多个位置。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void geocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The address or business to geocode.
   string addressToGeocode = "Microsoft";

   // The nearby location to use as a query hint.
   BasicGeoposition queryHint = new BasicGeoposition();
   queryHint.Latitude = 47.643;
   queryHint.Longitude = -122.131;
   Geopoint hintPoint = new Geopoint(queryHint);

   // Geocode the specified address, using the specified reference point
   // as a query hint. Return no more than 3 results.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAsync(
                           addressToGeocode,
                           hintPoint,
                           3);

   // If the query returns results, display the coordinates
   // of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "result = (" +
            result.Locations[0].Point.Position.Latitude.ToString() + "," +
            result.Locations[0].Point.Position.Longitude.ToString() + ")";
   }
}
```

此代码向 `tbOutputText` 文本框显示以下结果。

``` syntax
result = (47.6406099647284,-122.129339994863)
```

## <a name="get-an-address-reverse-geocode"></a>获取地址（反向地理编码）

本部分说明如何将地理位置转换为 (反向地理编码) 的地址。

1.  调用 [**MapLocationFinder**](/uwp/api/Windows.Services.Maps.MapLocationFinder) 类的 [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法。
2.  [**FindLocationsAtAsync**](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync) 方法将返回一个 [**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult) 对象，该对象包含匹配的 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 对象的集合。
3.  使用[**MapLocationFinderResult**](/uwp/api/Windows.Services.Maps.MapLocationFinderResult)的 "[**位置**](/uwp/api/windows.services.maps.maplocationfinderresult.locations)" 属性来公开集合[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)对象。 可能有多个 [**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation) 对象，因为系统可能会找到与给定输入对应的多个位置。
4.  通过每个[**MapLocation**](/uwp/api/Windows.Services.Maps.MapLocation)的[**Address**](/uwp/api/windows.services.maps.maplocation.address)属性访问[**MapAddress**](/uwp/api/Windows.Services.Maps.MapAddress)对象。

```csharp
using Windows.Services.Maps;
using Windows.Devices.Geolocation;
...
private async void reverseGeocodeButton_Click(object sender, RoutedEventArgs e)
{
   // The location to reverse geocode.
   BasicGeoposition location = new BasicGeoposition();
   location.Latitude = 47.643;
   location.Longitude = -122.131;
   Geopoint pointToReverseGeocode = new Geopoint(location);

   // Reverse geocode the specified geographic location.
   MapLocationFinderResult result =
         await MapLocationFinder.FindLocationsAtAsync(pointToReverseGeocode);

   // If the query returns results, display the name of the town
   // contained in the address of the first result.
   if (result.Status == MapLocationFinderStatus.Success)
   {
      tbOutputText.Text = "town = " +
            result.Locations[0].Address.Town;
   }
}
```

此代码向 `tbOutputText` 文本框显示以下结果。

``` syntax
town = Redmond
```

## <a name="related-topics"></a>相关主题

* [UWP 地图示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [UWP 路况应用示例](https://github.com/Microsoft/Windows-appsample-trafficapp)
* [地图设计指南](./display-maps.md)
* [视频：跨 Windows 应用中的手机、平板电脑和 PC 利用地图和位置](https://channel9.msdn.com/Events/Build/2015/2-757)
* [必应地图开发人员中心](https://www.bingmapsportal.com/)
* [**MapLocationFinder** 类](/uwp/api/Windows.Services.Maps.MapLocationFinder)
* [**FindLocationsAsync** 方法](/uwp/api/windows.services.maps.maplocationfinder.findlocationsasync)
* [**FindLocationsAtAsync** 方法](/uwp/api/windows.services.maps.maplocationfinder.findlocationsatasync)