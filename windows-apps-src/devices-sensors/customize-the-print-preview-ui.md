---
ms.assetid: 88132B6F-FB50-4B03-BC21-233988746230
title: 自定义打印预览 UI
description: 本部分介绍了如何在打印预览 UI 中自定义打印选项和设置。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10，uwp，打印
ms.localizationpriority: medium
ms.openlocfilehash: 6b73f595dc4dd6a255a439eecfccf7fce1d61204
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89175471"
---
# <a name="customize-the-print-preview-ui"></a>自定义打印预览 UI



**重要的 API**

-   [**"打印"**](/uwp/api/Windows.Graphics.Printing)
-   [**Windows.UI.Xaml.Printing**](/uwp/api/Windows.UI.Xaml.Printing)
-   [**PrintManager**](/uwp/api/Windows.Graphics.Printing.PrintManager)

本部分介绍了如何在打印预览 UI 中自定义打印选项和设置。 有关打印的详细信息，请参阅[从应用打印](print-from-your-app.md)。

**提示**   本主题中的大多数示例都基于 "打印" 示例。 若要查看完整的代码，请从 GitHub 上的 [Windows-universal-samples 存储库](https://github.com/Microsoft/Windows-universal-samples)中下载[通用 Windows 平台 (UWP) 打印示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)。

 

## <a name="customize-print-options"></a>自定义打印选项

默认情况下，打印预览 UI 将显示 [**ColorMode**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.colormode)、[**Copies**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.copies) 和 [**Orientation**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.orientation) 打印选项。 除上述打印选项外，还有其他几种常见打印机选项，你也可以将它们添加到打印预览 UI：

-   [**绑定**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.binding)
-   [**归类**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.collation)
-   [**双工**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.duplex)
-   [**HolePunch**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.holepunch)
-   [**InputBin**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.inputbin)
-   [**MediaSize**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize)
-   [**MediaType**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediatype)
-   [**NUp**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.nup)
-   [**PrintQuality**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.printquality)
-   [**装订**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.staple)

这些选项在 [**StandardPrintTaskOptions**](/uwp/api/Windows.Graphics.Printing.StandardPrintTaskOptions) 类中进行定义。 你可以将选项添加到打印预览 UI 中显示的选项列表，或者从中删除选项。 你还可以更改它们的显示顺序，并设置显示给用户的默认设置。

但是，通过使用此方法所进行的修改将仅影响打印预览 UI。 通过在打印预览 UI 中点击“更多设置”****，用户始终可以访问打印机支持的所有选项。

**注意**   虽然您的应用程序可以指定要显示的任何打印选项，但打印预览 UI 中只显示了所选打印机支持的选项。 打印 UI 不会显示所选打印机不支持的选项。

 

### <a name="define-the-options-to-display"></a>定义要显示的选项

加载应用的屏幕时，该屏幕将注册打印合约。 定义 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 事件处理程序是注册的一部分。 会将用于自定义在打印预览 UI 中显示的选项的代码添加到 **PrintTaskRequested** 事件处理程序中。

修改 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 事件处理程序以包含 [**printTask.options**](/uwp/api/windows.graphics.printing.printtask.options) 说明，以便配置需要在打印预览 UI 中显示的打印设置。对于希望显示打印选项自定义列表的应用的屏幕，请覆盖帮助程序类中的 **PrintTaskRequested** 事件处理程序，以包含指定打印此屏幕时要显示的选项的代码。

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         IList<string> displayedOptions = printTask.Options.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.MediaSize);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Collation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Duplex);

         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;

         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

**重要提示**   调用[**displayedOptions**](/uwp/api/windows.graphics.printing.printtaskoptions.displayedoptions) ( # A1 将从打印预览 UI 中删除所有打印选项，包括 "**其他设置**" 链接。 请务必附加你希望在打印预览 UI 上显示的选项。

### <a name="specify-default-options"></a>指定默认选项

你还可以在打印预览 UI 中设置选项的默认值。 在上一示例的以下代码行中，设置 [**MediaSize**](/uwp/api/windows.graphics.printing.standardprinttaskoptions.mediasize) 选项的默认值。

``` csharp
         // Preset the default value of the printer option
         printTask.Options.MediaSize = PrintMediaSize.NorthAmericaLegal;
```         

## <a name="add-new-print-options"></a>添加新的打印选项

本部分显示了如何创建一个新打印选项，定义该选项支持的值列表，然后将该选项添加到打印预览。 像上一部分那样，在 [**PrintTaskRequested**](/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_#Windows_Foundation_IAsyncOperationWithProgress_2_Progress) 事件处理程序中添加新的打印选项。

首先，获取一个 [**PrintTaskOptionDetails**](/uwp/api/Windows.Graphics.Printing.OptionDetails.PrintTaskOptionDetails) 对象。 这用于向打印预览 UI 添加新的打印选项。 然后，清除在打印预览 UI 中显示的选项列表，并添加当用户想要从应用打印时你希望显示的选项。 之后，创建新打印选项并初始化选项值列表。 最后，添加新的选项并为 **OptionChanged** 事件分配一个处理程序。

``` csharp
protected override void PrintTaskRequested(PrintManager sender, PrintTaskRequestedEventArgs e)
{
   PrintTask printTask = null;
   printTask = e.Request.CreatePrintTask("C# Printing SDK Sample", sourceRequestedArgs =>
   {
         PrintTaskOptionDetails printDetailedOptions = PrintTaskOptionDetails.GetFromPrintTaskOptions(printTask.Options);
         IList<string> displayedOptions = printDetailedOptions.DisplayedOptions;

         // Choose the printer options to be shown.
         // The order in which the options are appended determines the order in which they appear in the UI
         displayedOptions.Clear();

         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Copies);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.Orientation);
         displayedOptions.Add(Windows.Graphics.Printing.StandardPrintTaskOptions.ColorMode);

         // Create a new list option
         PrintCustomItemListOptionDetails pageFormat = printDetailedOptions.CreateItemListOption("PageContent", "Pictures");
         pageFormat.AddItem("PicturesText", "Pictures and text");
         pageFormat.AddItem("PicturesOnly", "Pictures only");
         pageFormat.AddItem("TextOnly", "Text only");

         // Add the custom option to the option list
         displayedOptions.Add("PageContent");

         printDetailedOptions.OptionChanged += printDetailedOptions_OptionChanged;

         // Print Task event handler is invoked when the print job is completed.
         printTask.Completed += async (s, args) =>
         {
            // Notify the user when the print operation fails.
            if (args.Completion == PrintTaskCompletion.Failed)
            {
               await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
               {
                     MainPage.Current.NotifyUser("Failed to print.", NotifyType.ErrorMessage);
               });
            }
         };

         sourceRequestedArgs.SetSource(printDocumentSource);
   });
}
```

这些选项按追加它们的相同顺序显示在打印预览 UI 中，第一个选项显示在窗口的顶部。 在此示例中，会将自定义选项追加到最后，这样它便显示在选项列表的底部。 但是，你可以将其放在列表中的任意位置；自定义打印选项无需添加到最后位置。

当用户在你的自定义选项中更改选定选项时，将更新打印预览图像。 调用 [**InvalidatePreview**](/uwp/api/windows.ui.xaml.printing.printdocument.invalidatepreview) 方法以在打印预览 UI 中重绘图像，如下所示。

``` csharp
async void printDetailedOptions_OptionChanged(PrintTaskOptionDetails sender, PrintTaskOptionChangedEventArgs args)
{
   // Listen for PageContent changes
   string optionId = args.OptionId as string;
   if (string.IsNullOrEmpty(optionId))
   {
         return;
   }

   if (optionId == "PageContent")
   {
         await scenarioPage.Dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () =>
         {
            printDocument.InvalidatePreview();
         });
   }
}
```

## <a name="related-topics"></a>相关主题

* [打印的设计指南](./printing-and-scanning.md)
* [//Build 2015 视频：开发可在 Windows 10 中打印的应用](https://channel9.msdn.com/Events/Build/2015/2-94)
* [UWP 打印示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Printing)