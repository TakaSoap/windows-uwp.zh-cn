---
title: 获取并了解磁条数据
description: 了解如何从磁条获取和解释数据。
ms.date: 10/04/2018
ms.topic: article
keywords: windows 10，uwp，服务点，pos，磁条纹读取器
ms.localizationpriority: medium
ms.openlocfilehash: eb4238d35497bdd90671dd0b6fa63d589c0ec740
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173261"
---
# <a name="obtain-and-understand-magnetic-stripe-data"></a>获取并了解磁条数据

使用 [服务点](pos-basics.md)入门中所述的步骤在应用程序中设置磁条阅读器后，便可以开始获取数据了。

## <a name="subscribe-to-datareceived-events"></a>订阅 * DataReceived 事件

只要读者识别重击卡，它就会引发三个事件之一：

* [AamvaCardDataReceived 事件](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)：当电动机卡为重击时出现。
* [BankCardDataReceived 事件](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.aamvacarddatareceived)：在银行卡为重击时发生。
* [VendorSpecificDataReceived 事件](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader.vendorspecificdatareceived)：在特定于供应商的卡为重击时发生。

应用程序只需订阅磁条纹读取器支持的事件。 可以查看 [MagneticStripeReader](/uwp/api/windows.devices.pointofservice.magneticstripereader.supportedcardtypes)支持的卡类型。

以下代码演示了如何订阅三个 ***DataReceived** 事件：

```cs
private void SubscribeToEvents(ClaimedMagneticStripeReader claimedReader, MagneticStripeReader reader)
{
    foreach (var type in reader.SupportedCardTypes)
    {
        if (item == MagneticStripeReaderCardTypes.Aamva)
        {
            claimedReader.AamvaCardDataReceived += Reader_AamvaCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.Bank)
        {
            claimedReader.BankCardDataReceived += Reader_BankCardDataReceived;
        }
        else if (item == MagneticStripeReaderCardTypes.ExtendedBase)
        {
            claimedReader.VendorSpecificDataReceived += Reader_VendorSpecificDataReceived;
        }
    }
}
```

将向事件处理程序传递 [ClaimedMagneticStripeReader](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader) 和 *参数* 对象，其类型将因事件而异：

* **AamvaCardDataReceived** 事件： [MagneticStripeReaderAamvaCardDataReceivedEventArgs 类](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* **BankCardDataReceived** 事件： [MagneticStripeReaderBankCardDataReceivedEventArgs 类](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* **VendorSpecificDataReceived** 事件： [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 类](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)

## <a name="get-the-data"></a>获取数据

对于 **AamvaCardDataReceived** 和 **BankCardDataReceived** 事件，可以直接从 *args* 对象获取一些数据。 下面的示例演示如何获取一些属性并将其分配给成员变量：

```cs
private string _accountNumber;
private string _expirationDate;
private string _firstName;

private void Reader_BankCardDataReceived(
    ClaimedMagneticStripeReader sender, 
    MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    _accountNumber = args.AccountNumber;
    _expirationDate = args.ExpirationDate;
    _firstName = args.FirstName;
    // etc...
}
```

但是，某些数据（包括 **VendorSpecificDataReceived** 事件中的所有数据）必须通过 **报表** 对象进行检索，该对象是 *参数参数的* 属性。 这是 [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)类型。

您可以使用 [CardType](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.cardtype) 属性来确定已重击的卡类型，然后使用该属性来通知您如何解释 [Track1](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track1)、 [Track2](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track2)、 [Track3](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track3)和 [Track4](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport.track4)中的数据。

每个轨迹的数据表示为 [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata) 对象。 从此类中，你可以获取以下类型的数据：

* [数据](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.data)：原始或已解码的数据。
* [DiscretionaryData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.discretionarydata)：任意数据。 
* [EncryptedData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata.encrypteddata)：已加密的数据。

以下代码段将获取报表和跟踪数据，然后检查卡类型：

```cs
private void GetTrackData(MagneticStripeReaderBankCardDataReceivedEventArgs args)
{
    MagneticStripeReaderReport report = args.Report;
    IBuffer track1 = report.Track1.Data;
    IBuffer track2 = report.Track2.Data;
    IBuffer track3 = report.Track3.Data;
    IBuffer track4 = report.Track4.Data;

    if (report.CardType == MagneticStripeReaderCardTypes.Aamva)
    {
        // Card type is AAMVA
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Bank)
    {
        // Card type is bank card
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.ExtendedBase)
    {
        // Card type is vendor-specific
    }
    else if (report.CardType == MagneticStripeReaderCardTypes.Unknown)
    {
        // Card type is unknown
    }
}
```

[!INCLUDE [feedback](./includes/pos-feedback.md)]

## <a name="see-also"></a>另请参阅

* [磁条读取器](pos-magnetic-stripe-reader.md)
* [ClaimedMagneticStripeReader 类](/uwp/api/windows.devices.pointofservice.claimedmagneticstripereader)
* [MagneticStripeReaderAamvaCardDataReceivedEventArgs 类](/uwp/api/windows.devices.pointofservice.magneticstripereaderaamvacarddatareceivedeventargs)
* [MagneticStripeReaderBankCardDataReceivedEventArgs 类](/uwp/api/windows.devices.pointofservice.magneticstripereaderbankcarddatareceivedeventargs)
* [MagneticStripeReaderVendorSpecificCardDataReceivedEventArgs 类](/uwp/api/windows.devices.pointofservice.magneticstripereadervendorspecificcarddatareceivedeventargs)
* [MagneticStripeReaderReport](/uwp/api/windows.devices.pointofservice.magneticstripereaderreport)
* [MagneticStripeReaderTrackData](/uwp/api/windows.devices.pointofservice.magneticstripereadertrackdata)