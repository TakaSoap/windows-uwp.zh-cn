---
title: 使用条形码扫描仪标志
description: 使用 UWP 条形码扫描器 Api，无需手动配置扫描器即可处理条形码符号。
ms.date: 08/29/2018
ms.topic: article
keywords: windows 10, uwp, 服务点, pos
ms.localizationpriority: medium
ms.openlocfilehash: 96014dfeb0b160d9cb94afef5ba4251b3c8bf8aa
ms.sourcegitcommit: 5d34eb13c7b840c05e5394910a22fa394097dc36
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89054377"
---
# <a name="working-with-symbologies"></a>使用标志
[条形码标志](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)是到特定条形码格式的数据映射。 一些常见的符号包括 UPC、代码128、QR 码等。  通用 Windows 平台条形码扫描器 Api 允许应用程序控制扫描仪处理这些符号的方式，而无需手动配置扫描仪。 

## <a name="determine-which-symbologies-are-supported"></a>确定哪些标志受支持 
由于你的应用程序可能要使用来自多个制造商的不同条形码扫描仪型号，你可能需要查询以确定扫描仪支持的标志。  如果你的应用程序需要并非所有扫描仪都支持的特定标志，或你需要启用扫描仪上已手动或以编程方式禁用的标志，这可能很有用。

在通过使用 [BarcodeScanner.FromIdAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.fromidasync) 获得 [BarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner) 对象后，调用 [GetSupportedSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedsymbologiesasync#Windows_Devices_PointOfService_BarcodeScanner_GetSupportedSymbologiesAsync) 获取设备支持的标志的列表。

下面的示例获取符号的条形码扫描器列表，并将其显示在文本块中：

```cs
private void DisplaySupportedSymbologies(BarcodeScanner barcodeScanner, TextBlock textBlock) 
{
    var supportedSymbologies = await barcodeScanner.GetSupportedSymbologiesAsync();

    foreach (uint item in supportedSymbologies)
    {
        string symbology = BarcodeSymbologies.GetName(item);
        textBlock.Text += (symbology + "\n");
    }
}
```

## <a name="determine-if-a-specific-symbology-is-supported"></a>确定是否支持特定标志
若要确定扫描程序是否支持特定的符号，可以调用 [IsSymbologySupportedAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.issymbologysupportedasync#Windows_Devices_PointOfService_BarcodeScanner_IsSymbologySupportedAsync_System_UInt32_)。

以下示例检查条形码扫描器是否支持 **Code32** 条码符号：

```cs
bool symbologySupported = await barcodeScanner.IsSymbologySupportedAsync(BarcodeSymbologies.Code32);
```

## <a name="change-which-symbologies-are-recognized"></a>更改识别的符号
在某些情况下，你可能需要使用条形码扫描仪支持的标志的子集。  若要阻止你不打算在应用程序中使用的标志，这尤为有用。 例如，为确保用户扫描正确的条形码，你可以在获取项目 SKU 时将扫描限制为 UPC 或 EAN，在获取序列号时将扫描限制为 Code 128。

在你知道了扫描仪支持的标志后，你可以设置希望扫描仪识别的标志。  这可以在使用[ClaimScannerAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.claimscannerasync#Windows_Devices_PointOfService_BarcodeScanner_ClaimScannerAsync)建立[ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)对象之后完成。 可以调用 [SetActiveSymbologiesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setactivesymbologiesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_SetActiveSymbologiesAsync_Windows_Foundation_Collections_IIterable_System_UInt32__) 来启用一组特定的标志，列表中略去的标志将禁用。

下面的示例将声明的条形码扫描器的活动符号设置为 [Code39](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39#Windows_Devices_PointOfService_BarcodeSymbologies_Code39) 和 [Code39Ex](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies.code39ex)：

```cs
private async void SetSymbologies(ClaimedBarcodeScanner claimedBarcodeScanner) 
{
    var symbologies = new List<uint>{ BarcodeSymbologies.Code39, BarcodeSymbologies.Code39Ex };
    await claimedBarcodeScanner.SetActiveSymbologiesAsync(symbologies);
}
```

## <a name="barcode-symbology-attributes"></a>条码符号特性
不同的条形码符号可以具有不同的属性，例如支持多个解码长度、将支票数字作为原始数据的一部分传输到主机以及检查数字验证。 使用 [BarcodeSymbologyAttributes](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes) 类，可以获取和设置给定 [ClaimedBarcodeScanner](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner) 和条码符号的这些特性。

可以使用 [GetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.getsymbologyattributesasync#Windows_Devices_PointOfService_ClaimedBarcodeScanner_GetSymbologyAttributesAsync_System_UInt32_)获取给定的符号的属性。 下面的代码段获取 **ClaimedBarcodeScanner**的 Upca 的符号特性。

```cs
BarcodeSymbologyAttributes barcodeSymbologyAttributes = 
    await claimedBarcodeScanner.GetSymbologyAttributesAsync(BarcodeSymbologies.Upca);
```

完成修改属性并准备好对其进行设置后，可以调用 [SetSymbologyAttributesAsync](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner.setsymbologyattributesasync)。 此方法返回一个 **布尔**值，如果成功设置特性，则为 **true** 。

```cs
bool success = await claimedBarcodeScanner.SetSymbologyAttributesAsync(
    BarcodeSymbologies.Upca, barcodeSymbologyAttributes);
```

### <a name="restrict-scan-data-by-data-length"></a>限制扫描数据的数据长度
某些标志的长度是可变的，如 Code 39 或 Code 128。  这些符号的条形码可以彼此接近，其中包含不同的数据，通常具有特定长度。 设置所需的特定数据长度可以防止无效扫描。

设置解码长度之前，请检查条形码符号是否支持多个长度为 [IsDecodeLengthSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.isdecodelengthsupported#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_IsDecodeLengthSupported)的长度。 知道受支持后，可以设置 [DecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelengthkind#Windows_Devices_PointOfService_BarcodeSymbologyAttributes_DecodeLengthKind)类型为 [BarcodeSymbologyDecodeLengthKind](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)。 此属性可以是下列任意值：

* **AnyLength**：任意数字的解码长度。
* **离散**：解码长度为 [DecodeLength1](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength1) 或 [DecodeLength2](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.decodelength2) 单字节字符。
* **范围**：解码长度介于 **DecodeLength1** 和 **DecodeLength2** 单字节字符之间。 **DecodeLength1**和**DecodeLength2**的顺序并不重要 (可以高于或低于其他) 。

最后，可以将 **DecodeLength1** 和 **DecodeLength2** 的值设置为控制所需数据的长度。

下面的代码段演示如何设置解码长度：

```cs
private async Task<bool> SetDecodeLength(
    ClaimedBarcodeScanner scanner,
    uint symbology, 
    BarcodeSymbologyDecodeLengthKind kind, 
    uint decodeLength1, 
    uint decodeLength2)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsDecodeLengthSupported)
    {
        attributes.DecodeLengthKind = kind;
        attributes.DecodeLength1 = decodeLength1;
        attributes.DecodeLength2 = decodeLength2;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-transmission"></a>检查数字传输

可在符号上设置的另一个属性是检查数字是否将作为原始数据的一部分传输到主机。 在设置此设置之前，请确保该符号支持带 [IsCheckDigitTransmissionSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionsupported)的检查数字传输。 然后，设置是否通过 [IsCheckDigitTransmissionEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigittransmissionenabled)启用了检查数字传输。

下面的代码段演示如何设置检查数字传输：

```cs
private async Task<bool> SetCheckDigitTransmission(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitTransmissionSupported)
    {
        attributes.IsCheckDigitTransmissionEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

### <a name="check-digit-validation"></a>检查数字验证

还可以设置是否验证条形码检查数字。 在设置此设置之前，请确保该符号支持带有 [IsCheckDigitValidationSupported](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationsupported)的检查数字验证。 然后，设置是否通过 [IsCheckDigitValidationEnabled](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes.ischeckdigitvalidationenabled)启用检查数字验证。

下面的代码段演示如何设置检查数字验证：

```cs
private async Task<bool> SetCheckDigitValidation(ClaimedBarcodeScanner scanner, uint symbology, bool isEnabled)
{
    bool success = false;
    BarcodeSymbologyAttributes attributes = await scanner.GetSymbologyAttributesAsync(symbology);

    if (attributes.IsCheckDigitValidationSupported)
    {
        attributes.IsCheckDigitValidationEnabled = isEnabled;
        success = await scanner.SetSymbologyAttributesAsync(symbology, attributes);
    }

    return success;
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另请参阅

* [条形码扫描仪](pos-barcodescanner.md)
* [BarcodeSymbologies 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologies)
* [BarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner)
* [ClaimedBarcodeScanner 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.claimedbarcodescanner)
* [BarcodeSymbologyAttributes 类](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologyattributes)
* [BarcodeSymbologyDecodeLengthKind 枚举](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodesymbologydecodelengthkind)