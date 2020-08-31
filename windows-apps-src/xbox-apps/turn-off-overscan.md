---
title: 如何将 UI 绘制到屏幕的边缘
description: 了解如何关闭置于视区边缘的默认边框，并将您的 UI 绘制到屏幕的边缘。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1adb221f-6f70-4255-9329-2046a486ca45
ms.localizationpriority: medium
ms.openlocfilehash: d34da1bbf129358f4549b3a4a04a7c3f84f872ab
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154841"
---
# <a name="how-to-draw-ui-to-the-edge-of-the-screen"></a>如何将 UI 绘制到屏幕的边缘   
考虑到电视安全区域，应用程序默认会将边框置于视口的边缘（有关详细信息，请参阅[针对 Xbox 和电视进行设计](../design/devices/designing-for-tv.md#tv-safe-area)）。 

我们建议关闭此功能并绘制到屏幕的边缘。 通过添加以下代码，可以在你的应用程序启动时绘制到屏幕的边缘：
   
```
Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().SetDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.UseCoreWindow);
```
   
> [!NOTE]
> C++/DirectX 应用程序无需担心这一问题。 系统始终会将你的应用程序呈现到屏幕的边缘。

## <a name="see-also"></a>另请参阅
- [适用于 Xbox 的最佳做法](tailoring-for-xbox.md)
- [Xbox One 上的 UWP](index.md)
