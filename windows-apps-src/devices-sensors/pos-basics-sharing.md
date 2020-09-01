---
title: PointOfService 设备共享
description: 了解如何在多台电脑依赖于共享外设的环境中，与其他计算机共享网络或 Bluetooth 连接的外围设备。
ms.date: 06/14/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 4fad9bc75ed0ff79be1596a3c99445c7e9f97b1f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163261"
---
# <a name="pointofservice-device-sharing"></a>PointOfService 设备共享

了解如何在多台电脑依赖于共享外围设备（而不是连接到每台计算机的专用外围设备）的环境中，与其他计算机共享网络或蓝牙连接的外围网络。

## <a name="device-sharing"></a>设备共享

网络和蓝牙连接的 PointOfService 外设通常用于 wheere 多个客户端设备在一整天共享同一外围设备的环境中使用。  在繁忙的零售或食品服务环境中，客户端设备连接到外围设备的任何延迟都会影响到相关工作效率，即关联可以与客户关闭交易，然后继续下一步。 在 "快速服务餐厅" 方案中，使用收据打印机作为厨房打印机，以将客户订单的详细信息传送到厨房进行准备，其中有多个客户端设备从客户那里获得订单。  当订单完成后，每个客户端设备应该能够声明共享打印机，并立即打印厨房的顺序。

在这些环境中，应用程序必须完全 **释放** 设备对象，以便另一台设备可以声明同一设备，这一点非常重要。

在 "using" 块的末尾释放 PosPrinter

```Csharp 
using Windows.Devices.PointOfService;
using(PosPrinter printer = await PosPrinter.FromIdAsync("Device ID"))
{
    if (printer != null)
    {
        // Exercise the printer.
    }

    // When leaving this scope, printer.Dispose() is automatically invoked, 
    // releasing the session we have with the printer.
}
```


通过显式调用 Dispose ( 释放 PosPrinter

```Csharp 
using Windows.Devices.PointOfService;

PosPrinter printer = await PosPrinter.FromIdAsync("Device ID");
if (printer != null)
{
    // Exercise the printer, then dispose of the printer explicitly.
    printer.Dispose();
}
```

## <a name="api-methods-used"></a>使用的 API 方法 

+ [BarcodeScanner](/uwp/api/windows.devices.pointofservice.barcodescanner.dispose) 
+ [CashDrawer](/uwp/api/windows.devices.pointofservice.cashdrawer.dispose) 
+ [LineDisplay](/uwp/api/windows.devices.pointofservice.linedisplay.dispose) 
+ [MagneticStripeReader](/uwp/api/windows.devices.pointofservice.magneticstripereader.dispose)  
+ [PosPrinter](/uwp/api/windows.devices.pointofservice.posprinter.dispose) 


[!INCLUDE [feedback](./includes/pos-feedback.md)]