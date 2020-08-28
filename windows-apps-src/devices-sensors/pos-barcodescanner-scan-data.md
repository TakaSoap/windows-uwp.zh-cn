---
title: 获取并了解条形码数据
description: 了解如何从 BarcodeScannerReport 对象中的条码扫描器获取数据并了解其格式和内容。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 5d2ab873774ce8b48252b82ce6cf7521165554d4
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043439"
---
# <a name="obtain-and-understand-barcode-data"></a>获取并了解条形码数据

设置条形码扫描器后，您需要一种了解所扫描的数据的方式。 在扫描条形码时，将引发 [DataReceived](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.datareceived) 事件。 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)应订阅此事件。 **DataReceived**事件传递一个[BarcodeScannerDataReceivedEventArgs](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)对象，该对象可用于访问条码数据。

## <a name="subscribe-to-the-datareceived-event"></a>订阅 DataReceived 事件

有了 **ClaimedBarcodeScanner**后，让它订阅 **DataReceived** 事件：

```cs
claimedBarcodeScanner.DataReceived += ClaimedBarcodeScanner_DataReceived;
```

事件处理程序将传递给 **ClaimedBarcodeScanner** 和 **BarcodeScannerDataReceivedEventArgs** 对象。 您可以通过此对象的 [报表](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs.report#Windows_Devices_PointOfService_BarcodeScannerDataReceivedEventArgs_Report) 属性（类型为 [BarcodeScannerReport](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)）访问条码数据。

```cs
private async void ClaimedBarcodeScanner_DataReceived(ClaimedBarcodeScanner sender, BarcodeScannerDataReceivedEventArgs args)
{
    // Parse the data
}
```

## <a name="get-the-data"></a>获取数据

有了 **BarcodeScannerReport**后，就可以访问和分析条码数据了。 **BarcodeScannerReport** 有三个属性：

* [ScanData](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandata)：完整的原始条形码数据。
* [ScanDataLabel](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatalabel)：解码的条形码标签，不包括标头、校验和和其他杂项信息。
* [ScanDataType](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport.scandatatype)：已解码的条形码标签类型。 可能的值是在 [BarcodeSymbologies](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies) 类中定义的。

如果要访问 **ScanDataLabel** 或 **ScanDataType**，必须首先将 [IsDecodeDataEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.isdecodedataenabled#Windows_Devices_PointOfService_ClaimedBarcodeScanner_IsDecodeDataEnabled) 设置为 **true**。

```cs
claimedBarcodeScanner.IsDecodeDataEnabled = true;
```

### <a name="get-the-scan-data-type"></a>获取扫描数据类型

获取解码的条形码标签类型相当简单， &mdash; 只需在**ScanDataType**上调用[GetName](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)即可。

```cs
private string GetSymbology(BarcodeScannerDataReceivedEventArgs args)
{
    return BarcodeSymbologies.GetName(args.Report.ScanDataType);
}
```

### <a name="get-the-scan-data-label"></a>获取扫描数据标签

若要获取已解码的条形码标签，需要注意一些事项。 只有某些数据类型包含编码文本，因此您应该首先检查是否可以将该符号转换为字符串，然后将从 **ScanDataLabel** 获取的缓冲区转换为编码的 utf-8 字符串。

```cs
private string GetDataLabel(BarcodeScannerDataReceivedEventArgs args)
{
    uint scanDataType = args.Report.ScanDataType;

    // Only certain data types contain encoded text.
    // To keep this simple, we'll just decode a few of them.
    if (args.Report.ScanDataLabel == null)
    {
        return "No data";
    }

    // This is not an exhaustive list of symbologies that can be converted to a string.
    else if (scanDataType == BarcodeSymbologies.Upca ||
        scanDataType == BarcodeSymbologies.UpcaAdd2 ||
        scanDataType == BarcodeSymbologies.UpcaAdd5 ||
        scanDataType == BarcodeSymbologies.Upce ||
        scanDataType == BarcodeSymbologies.UpceAdd2 ||
        scanDataType == BarcodeSymbologies.UpceAdd5 ||
        scanDataType == BarcodeSymbologies.Ean8 ||
        scanDataType == BarcodeSymbologies.TfStd)
    {
        // The UPC, EAN8, and 2 of 5 families encode the digits 0..9
        // which are then sent to the app in a UTF8 string (like "01234").
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanDataLabel);
    }

    // Some other symbologies (typically 2-D symbologies) contain binary data that
    // should not be converted to text.
    else
    {
        return "Decoded data unavailable.";
    }
}
```

### <a name="get-the-raw-scan-data"></a>获取原始扫描数据

若要从条形码获取完整的原始数据，只需将从 **ScanData** 获取的缓冲区转换为字符串。

```cs
private string GetRawData(BarcodeScannerDataReceivedEventArgs args)
{
    // Get the full, raw barcode data.
    if (args.Report.ScanData == null)
    {
        return "No data";
    }

    // Just to show that we have the raw data, we'll print the value of the bytes.
    else
    {
        return CryptographicBuffer.ConvertBinaryToString(BinaryStringEncoding.Utf8, args.Report.ScanData);
    }
}
```

这些数据通常采用从扫描仪提供的格式。 但会删除消息头和尾部信息，因为它们不包含应用程序的有用信息，并且可能是特定于扫描程序的信息。

常见的标头信息是前缀字符 (如 STX 字符) 。 常见的尾部信息是终止符字符 (例如，ETX 或 CR 字符) ，如果扫描程序生成一个块检查字符，则为块检查字符。

如果扫描 (程序返回一个符号字符，则该属性应包含一个符号字符，例如 **，一个用于 UPC** 的) 。 如果标签出现在标签中并由扫描器返回，则它还应包含检查数字。  (请注意，根据扫描仪的配置，可能存在也可能不存在符号字符和检查数字。 扫描程序将返回它们（如果存在），但如果它们不存在，则不会生成或计算它们。 ) 

某些商品可能使用补充条形码进行标记。 此条形码通常放置在主条形码的右侧，并包含另外两或五个字符的信息。 如果扫描程序读取包含主要和补充条形码的商品，则附加字符将追加到主字符，并将结果作为一个标签传递到应用程序。  (请注意，扫描仪可能支持启用或禁用补充代码读取的配置。 ) 

某些商品可能标记有多个标签，有时称为 *multisymbol 标签* 或 *分层标签*。 这些条形码通常垂直排列，并且可能具有相同或不同的符号。 如果扫描程序读取包含多个标签的商品，则每个条形码作为一个单独的标签传递到应用程序。 这是必需的，因为目前缺乏这些条形码类型的标准化。 无法根据各个条形码数据确定所有变体。 因此，应用程序需要确定基于返回的数据读取多个标签条形码的时间。  (请注意，扫描器可能会也可能不支持读取多个标签。 ) 

在向应用程序引发 **DataReceived** 事件之前设置此值。

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>请参阅
* [条形码扫描仪](pos-barcodescanner.md)
* [ClaimedBarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.getname)
* [BarcodeScannerDataReceivedEventArgs 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerdatareceivedeventargs)
* [BarcodeScannerReport 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescannerreport)
* [BarcodeSymbologies 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
