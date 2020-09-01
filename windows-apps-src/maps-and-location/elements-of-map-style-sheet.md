---
description: 了解如何使用地图样式表来定义地图的外观，例如在 Windows 应用商店应用程序的 MapControl 中显示的地图。
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: 地图样式表参考
ms.date: 03/19/2017
ms.topic: article
keywords: windows 10, uwp, 地图, 地图样式表
ms.localizationpriority: medium
ms.openlocfilehash: f40f3e64e4fe08d5216a08d125828a96b56cc2af
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155681"
---
# <a name="map-style-sheet-reference"></a>地图样式表参考

Microsoft 映射技术使用 [地图样式表](/BingMaps/styling/map-style-sheets) 来定义地图的外观，包括在 Windows 应用商店应用程序的 [MapControl](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol)中显示的。  使用 JavaScript 对象表示法 (JSON) 通过 [MapStyleSheet. ParseFromJson](/uwp/api/windows.ui.xaml.controls.maps.mapstylesheet.parsefromjson#Windows_UI_Xaml_Controls_Maps_MapStyleSheet_ParseFromJson_System_String_) 方法定义地图样式表。

样式表包含其属性被重写以更改地图最终外观的 [条目](/BingMaps/styling/map-style-sheet-entries) 。

例如，可以使用以下 JSON 使水源显示为红色，水标签显示为绿色，而土地区显示为蓝色：

```json
    {"version":"1.*",
        "settings":{"landColor":"#0000FF"},
        "elements":{"water":{"fillColor":"#FF0000","labelColor":"#00FF00"}}
    }
```

可以使用 " [地图样式表编辑器](https://www.microsoft.com/p/map-style-sheet-editor/9nbhtcjt72ft) " 应用程序以交互方式创建样式表。