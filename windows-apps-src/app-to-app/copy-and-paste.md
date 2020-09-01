---
description: 本文介绍如何支持在通用 Windows 平台 (UWP) 应用中通过使用剪贴板进行复制和粘贴。
title: 复制和粘贴
ms.assetid: E882DC15-E12D-4420-B49D-F495BB484BEE
ms.date: 12/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 117c09eb2dd0f24a330060da9b9766cb33e90d58
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175761"
---
# <a name="copy-and-paste"></a>复制和粘贴

本文介绍如何支持在通用 Windows 平台 (UWP) 应用中通过使用剪贴板进行复制和粘贴。 复制和粘贴是在应用之间或在应用内交换数据的传统方法，并且在一定程度上，几乎每个应用都可以支持剪贴板操作。 有关演示多种不同复制和粘贴方案的完整代码示例，请参阅 [剪贴板示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)。

## <a name="check-for-built-in-clipboard-support"></a>检查内置剪贴板支持

许多情况下，你无需编写用于支持剪贴板操作的代码。 可用于创建应用的许多默认 XAML 控件已经支持剪贴板操作。 

## <a name="get-set-up"></a>准备工作

首先，将 [**Windows.ApplicationModel.DataTransfer**](/uwp/api/Windows.ApplicationModel.DataTransfer) 命名空间包含在你的应用中。 然后，添加一个 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 对象实例。 该对象同时包含用户希望复制的数据和你希望包含的所有属性（如描述）。

```cs
DataPackage dataPackage = new DataPackage();
```

<!-- AuthenticateAsync-->

## <a name="copy-and-cut"></a>复制和剪切

复制和剪切（也称为*移动*）的工作原理几乎完全相同。 使用 [**RequestedOperation**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 属性选择所需操作。

```cs
// copy 
dataPackage.RequestedOperation = DataPackageOperation.Copy;
// or cut
dataPackage.RequestedOperation = DataPackageOperation.Move;
```

## <a name="set-the-copied-content"></a>设置复制的内容

接下来，你可以将用户已选择的数据添加到 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 对象。 如果此数据受**DataPackage**类支持，则可以使用**DataPackage**对象的相应[方法](/uwp/api/windows.applicationmodel.datatransfer.datapackage#methods)之一。 下面介绍了如何使用 [**SetText**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.settext) 方法添加文本：

```cs
dataPackage.SetText("Hello World!");
```

最后一步是通过调用静态 [**SetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent) 方法将 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 添加到剪贴板。

```cs
Clipboard.SetContent(dataPackage);
```

## <a name="paste"></a>粘贴

若要获取剪贴板的内容，请调用静态 [**GetContent**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent) 方法。 此方法将返回一个包含该内容的 [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView)。 该对象几乎与 [**DataPackage**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 对象完全相同，只不过其内容为只读。 通过该对象，你可以使用 [**AvailableFormats**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats) 或 [**Contains**](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains) 方法来确定哪些格式可用。 然后，你可以调用相应的 [**DataPackageView**](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackageView) 方法来获取数据。

```cs
async void OutputClipboardText()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="track-changes-to-the-clipboard"></a>跟踪对剪贴板的更改

除了复制和粘贴命令以外，你可能还想要跟踪剪贴板更改。 可通过处理剪贴板的 [**ContentChanged**](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged) 事件来执行此操作。

```cs
Clipboard.ContentChanged += async (s, e) => 
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        string text = await dataPackageView.GetTextAsync();
        // To output the text from this example, you need a TextBlock control
        TextOutput.Text = "Clipboard now contains: " + text;
    }
}
```

## <a name="see-also"></a>另请参阅

* [剪贴板示例](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/Clipboard)
* [应用到应用的通信](index.md)
* [DataTransfer](/uwp/api/windows.applicationmodel.datatransfer)
* [DataPackage](/uwp/api/windows.applicationmodel.datatransfer.datapackage)
* [DataPackageView](/uwp/api/windows.applicationmodel.datatransfer.datapackageview)
* [DataPackagePropertySet]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataPackagePropertySet)
* [DataRequest](/uwp/api/windows.applicationmodel.datatransfer.datarequest) 
* [DataRequested]( /uwp/api/Windows.ApplicationModel.DataTransfer.DataTransferManager)
* [FailWithDisplayText](/uwp/api/windows.applicationmodel.datatransfer.datarequest.failwithdisplaytext)
* [ShowShareUi](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.showshareui)
* [RequestedOperation](/uwp/api/windows.applicationmodel.datatransfer.datapackage.requestedoperation) 
* [ControlsList](../design/controls-and-patterns/index.md)
* [SetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.setcontent)
* [GetContent](/uwp/api/windows.applicationmodel.datatransfer.clipboard.getcontent)
* [AvailableFormats](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.availableformats)
* [包含](/uwp/api/windows.applicationmodel.datatransfer.datapackageview.contains)
* [ContentChanged](/uwp/api/windows.applicationmodel.datatransfer.clipboard.contentchanged)