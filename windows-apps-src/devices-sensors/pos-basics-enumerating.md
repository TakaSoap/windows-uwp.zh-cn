---
title: 枚举 PointOfService 设备
description: 了解四种使用设备选择器来查询和枚举可用于系统的 PointOfService 设备的方法。
ms.date: 10/08/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6d1767e49e14f424b7d21424fbfc02b76acd546a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89159671"
---
# <a name="enumerating-point-of-service-devices"></a>枚举服务点设备
在此部分，你将了解如何[定义设备选择器](./build-a-device-selector.md)（用于向系统提供查询设备），以及如何通过以下方法之一使用此选择器枚举服务点设备：

**方法1：** [使用设备选取器](#method-1-use-a-device-picker)
<br/>
显示设备选取器 UI，并让用户选择连接的设备。 此方法处理在附加和删除设备时更新列表，并且比其他方法更简单、更安全。

**方法2：** [获取第一个可用设备](#method-2-get-first-available-device)<br />使用 [GetDefaultAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync) 访问特定服务点设备类中的第一个可用设备。

**方法3：** [设备快照](#method-3-snapshot-of-devices)<br />枚举系统中在给定时间点存在的服务点的快照。 当你想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI 时这会很有用。 在整个枚举完成之前， [FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)将保留返回结果。

**方法4：** [枚举和监视](#method-4-enumerate-and-watch)<br />[DeviceWatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher) 是一个功能更强大的灵活枚举模型，可用于枚举当前存在的设备，还可在系统中添加或删除设备时收到通知。  当你想要在后台维护当前的设备列表以显示在 UI 中而不是等待快照发生时，这非常有用。

## <a name="define-a-device-selector"></a>定义设备选择器
设备选择器将使你可以在枚举设备时限制要搜索的设备。  这将允许你只获得相关结果，并缩短枚举所需设备所需的时间。

您可以为要查找的设备类型使用 **GetDeviceSelector** 方法，以获取该类型的设备选择器。 例如，使用 [PosPrinter](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector#Windows_Devices_PointOfService_PosPrinter_GetDeviceSelector) 将为你提供一个选择器，用于枚举附加到系统的所有 [POSPRINTERS](/uwp/api/windows.devices.pointofservice.posprinter) ，包括 USB、网络和蓝牙 POS 打印机。

```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector();
```

不同设备类型的 **GetDeviceSelector** 方法为：

* [BarcodeScanner.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.barcodescanner.getdeviceselector)
* [CashDrawer.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.cashdrawer.getdeviceselector)
* [LineDisplay.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.linedisplay.getdeviceselector)
* [MagneticStripeReader.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.magneticstripereader.getdeviceselector)
* [PosPrinter.GetDeviceSelector](/uwp/api/windows.devices.pointofservice.posprinter.getdeviceselector)

使用将[PosConnectionTypes](/uwp/api/windows.devices.pointofservice.posconnectiontypes)值作为参数的**GetDeviceSelector**方法，你可以限制选择器来枚举本地、网络或蓝牙连接的 POS 设备，从而减少完成查询所用的时间。  下面的示例演示如何使用此方法来定义仅支持本地连接的 POS 打印机的选择器。

 ```Csharp
using Windows.Devices.PointOfService;

string selector = POSPrinter.GetDeviceSelector(PosConnectionTypes.Local);
```

> [!TIP]
> 请参阅[生成设备选择器](./build-a-device-selector.md)了解如何生成更高级的筛选器字符串。

## <a name="method-1-use-a-device-picker"></a>方法1：使用设备选取器

使用 [DevicePicker](/uwp/api/windows.devices.enumeration.devicepicker) 类，可以显示一个选取器飞出控件，其中包含供用户选择的设备列表。 您可以使用 " [筛选器](/uwp/api/windows.devices.enumeration.devicepicker.filter) " 属性来选择要在选取器中显示的设备类型。 此属性的类型为 [DevicePickerFilter](/uwp/api/windows.devices.enumeration.devicepickerfilter)。 可以使用 [SupportedDeviceClasses](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceclasses) 或 [SupportedDeviceSelectors](/uwp/api/windows.devices.enumeration.devicepickerfilter.supporteddeviceselectors) 属性将设备类型添加到筛选器。

准备好显示设备选取器时，可以调用 [PickSingleDeviceAsync](/uwp/api/windows.devices.enumeration.devicepicker.picksingledeviceasync) 方法，该方法将显示选取器 UI 并返回所选设备。 需要指定一个 [矩形](/uwp/api/windows.foundation.rect) 来确定弹出窗口出现的位置。 此方法将返回 [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) 对象，因此，若要将其与服务 api 点一起使用，需要对所需的特定设备类使用 **FromIdAsync** 方法。 将 [DeviceInformation.Id](/uwp/api/windows.devices.enumeration.deviceinformation.id) 属性作为方法的 *deviceId* 参数传递，并获取设备类的实例作为返回值。

以下代码片段将创建一个 **DevicePicker**，向其中添加一个条形码扫描器筛选器，使用户选择一个设备，然后根据设备 ID 创建 **BarcodeScanner** 对象：

```cs
private async Task<BarcodeScanner> GetBarcodeScanner()
{
    DevicePicker devicePicker = new DevicePicker();
    devicePicker.Filter.SupportedDeviceSelectors.Add(BarcodeScanner.GetDeviceSelector());
    Rect rect = new Rect();
    DeviceInformation deviceInformation = await devicePicker.PickSingleDeviceAsync(rect);
    BarcodeScanner barcodeScanner = await BarcodeScanner.FromIdAsync(deviceInformation.Id);
    return barcodeScanner;
}
```

## <a name="method-2-get-first-available-device"></a>方法2：获取第一个可用设备

获取服务点设备的最简单方法是使用 **GetDefaultAsync** 在服务设备类中获取第一个可用设备。 

下面的示例演示如何将 [GetDefaultAsync](/uwp/api/windows.devices.pointofservice.barcodescanner.getdefaultasync#Windows_Devices_PointOfService_BarcodeScanner_GetDefaultAsync) 用于 [BarcodeScanner](/uwp/api/windows.devices.pointofservice.barcodescanner)。 对于所有服务点类，代码模式都是类似的。

```Csharp
using Windows.Devices.PointOfService;

BarcodeScanner barcodeScanner = await BarcodeScanner.GetDefaultAsync();
```

> [!CAUTION]
> 必须谨慎使用**GetDefaultAsync** ，因为它可能会将不同的设备从一个会话返回到下一个会话。 许多事件可能影响此枚举，从而导致出现不同的第一个可用设备，包括： 
> - 连接到计算机的摄像头更改 
> - 连接到你的计算机的服务设备的更改点
> - 网络上可用的网络连接点服务设备的更改
> - 计算机范围内的蓝牙服务设备的更改 
> - 对服务配置点的更改 
> - 安装驱动程序或 OPOS 服务对象
> - 安装服务点扩展
> - Windows 操作系统更新

## <a name="method-3-snapshot-of-devices"></a>方法3：设备快照

在有些情况下，你可能想要生成自己的 UI 或需要枚举设备而无需向用户显示 UI。  在这些情况下，你可以枚举当前使用 [DeviceInformation.FindAllAsync](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) 连接或与系统配对的设备的快照。  此方法将保留任何结果直到整个枚举完成。

> [!TIP]
> 建议在使用**FindAllAsync**时，将**GetDeviceSelector**方法与**PosConnectionTypes**参数一起使用，以将查询限制为所需的连接类型。  网络和蓝牙连接可能会延迟结果，因为在返回 **FindAllAsync** 结果之前，必须完成其枚举。

> [!CAUTION] 
> **FindAllAsync** 返回一组设备。  此数组的顺序可能随会话改变，因此，不建议通过使用数组的硬编码索引来依赖特定顺序。  使用 [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) 属性可以筛选结果，也可以提供 UI 供用户从中进行选择。

此示例使用上面定义的选择器拍摄使用 **FindAllAsync** 的设备的快照，然后枚举集合返回的每个项，并将设备名称和 ID 写入调试输出。 

```Csharp
using Windows.Devices.Enumeration;

DeviceInformationCollection deviceCollection = await DeviceInformation.FindAllAsync(selector);

foreach (DeviceInformation devInfo in deviceCollection)
{
    Debug.WriteLine("{0} {1}", devInfo.Name, devInfo.Id);
}
```

> [!TIP] 
> 在使用 [Windows.Devices.Enumeration](/uwp/api/Windows.Devices.Enumeration) API 时，你经常需要使用 [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) 对象来获取有关特定设备的信息。 例如， [DeviceInformation.ID](/uwp/api/windows.devices.enumeration.deviceinformation.id) 属性可用于恢复和重复使用同一设备（如果该设备在将来的会话中可用），并且 [DeviceInformation.Name](/uwp/api/windows.devices.enumeration.deviceinformation.name) 属性可用于在应用中显示。  请参阅 [DeviceInformation](/uwp/api/windows.devices.enumeration.deviceinformation) 参考页面了解有关其他可用属性的信息。

## <a name="method-4-enumerate-and-watch"></a>方法4：枚举和监视

一种更强大更灵活的枚举设备的方法就是创建 [DeviceWatcher](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)。  设备观察程序动态枚举设备，以便应用程序在完成初始枚举后在添加、删除或者更改设备的情况下收到通知。  通过 **DeviceWatcher** ，你可以检测网络连接设备何时进入联机状态、Bluetooth 设备处于范围内以及本地连接的设备是否已拔下，以便在应用程序中执行适当的操作。

此示例使用上面定义的选择器创建 **DeviceWatcher** ，并为 [已添加](/uwp/api/windows.devices.enumeration.devicewatcher.added)、 [已删除](/uwp/api/windows.devices.enumeration.devicewatcher.removed)和 [已更新](/uwp/api/windows.devices.enumeration.devicewatcher.updated) 的通知定义事件处理程序。 你需要填写希望对每个通知采取的措施的详细信息。

```Csharp
using Windows.Devices.Enumeration;

DeviceWatcher deviceWatcher = DeviceInformation.CreateWatcher(selector);
deviceWatcher.Added += DeviceWatcher_Added;
deviceWatcher.Removed += DeviceWatcher_Removed;
deviceWatcher.Updated += DeviceWatcher_Updated;

void DeviceWatcher_Added(DeviceWatcher sender, DeviceInformation args)
{
    // TODO: Add the DeviceInformation object to your collection
}

void DeviceWatcher_Removed(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Remove the item in your collection associated with DeviceInformationUpdate
}

void DeviceWatcher_Updated(DeviceWatcher sender, DeviceInformationUpdate args)
{
    // TODO: Update your collection with information from DeviceInformationUpdate
}
```

> [!TIP]
> 有关使用**DeviceWatcher**的更多详细信息，请参阅[列举和监视设备]( ./enumerate-devices.md#enumerate-and-watch-devices)。

## <a name="see-also"></a>另请参阅
* [服务点入门](pos-basics.md)
* [DeviceInformation 类](/uwp/api/windows.devices.enumeration.deviceinformation)
* [PosPrinter 类](/uwp/api/windows.devices.pointofservice.posprinter)
* [PosConnectionTypes 枚举](/uwp/api/windows.devices.pointofservice.posconnectiontypes)
* [BarcodeScanner 类](/uwp/api/windows.devices.pointofservice.barcodescanner)
* [DeviceWatcher 类](/uwp/api/Windows.Devices.Enumeration.DeviceWatcher)

[!INCLUDE [feedback](./includes/pos-feedback.md)]