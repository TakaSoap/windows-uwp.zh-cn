---
Description: 提供可使用的应用内产品&\# 8212; 可通过商店商业平台购买、使用并再次购买的商品&\# 8212; 通过商店商业平台，为客户提供可靠且可靠的购买体验。
title: 启用可消费应用内产品购买
ms.assetid: F79EE369-ACFC-4156-AF6A-72D1C7D3BDA4
keywords: uwp, 易耗品, 加载项, 应用内购买, IAP, Windows.ApplicationModel.Store
ms.date: 08/25/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fb4119296b11e805fa72ff027383d13e6fb43818
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363690"
---
# <a name="enable-consumable-in-app-product-purchases"></a>启用可消费应用内产品购买

通过应用商店商业平台提供可消费应用内产品（这些项目可以进行购买、使用和再次购买），以便为客户提供强大可靠的购买体验。 这对游戏内货币（金子、硬币等）等来说尤为有用，可以购买此类货币，然后将其用于购买特定道具。

> [!IMPORTANT]
> 本文介绍如何使用 [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) 命名空间的成员来支持易耗型应用内产品购买。 此命名空间不再更新新功能，我们建议你使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空间。 **Windows 服务**命名空间支持最新的加载项类型，如存储管理的可使用的加载项和订阅，旨在与合作伙伴中心和应用商店支持的未来类型的产品和功能兼容。 **Windows.Services.Store** 命名空间在 Windows 10 版本 1607 中引入，它仅可用于面向 **Windows 10 周年纪念版（10.0；版本 14393）或 Visual Studio** 更高版本的项目中。 有关使用 **Windows.Services.Store** 命名空间支持易耗型应用内产品购买的更多信息，请参阅[本文](enable-consumable-add-on-purchases.md)。

## <a name="prerequisites"></a>先决条件

-   本主题介绍可消费应用内产品的购买和实施报告。 如果你不熟悉应用内产品，请查看[启用应用内产品购买](enable-in-app-product-purchases.md)，了解许可证相关信息以及如何在应用商店中恰当地列出应用内产品。
-   首次编码和测试新的应用内产品时，必须使用 [CurrentAppSimulator](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 对象而不是 [CurrentApp](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 对象。 这样，你可以使用对许可证服务器的模拟调用验证许可证逻辑，而不是调用实时服务器。 要执行此操作，需要在% userprofile% \\ AppData \\ 本地 \\ 包 \\ &lt; 包名称 &gt; \\ LocalState \\ Microsoft \\ Windows Store \\ ApiData 中自定义名为 WindowsStoreProxy.xml 的文件。 Microsoft Visual Studio 仿真器会在你首次运行应用时创建此文件，你也可以在运行时加载一个自定义文件。 有关详细信息，请参阅[将 WindowsStoreProxy.xml 文件与 CurrentAppSimulator 一起使用](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#proxy)。
-   本主题还参考了[应用商店示例](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)中提供的代码示例。 若要获得为通用 Windows 平台 (UWP) 应用提供的不同货币化选项的实际体验，此示例是一个不错的选择。

## <a name="step-1-making-the-purchase-request"></a>步骤 1：提出购买请求

初始购买请求是通过 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 提出的，其方式与通过应用商店提出的任何其他购买相似。 易耗型应用内产品的不同之处在于在成功购买后，客户无法再次购买同一个产品，直到应用通知应用商店之前的购买已成功实施。 应用负责实施购买的消费品，并通知应用商店该项实施。

下面的示例显示了一个可消费应用内产品的购买请求。 你会注意到代码注释，指明应用何时应该为下面两种不同情况的易耗型应用内产品执行本地实施：已成功提出请求的情况和请求因同一产品未实施购买而失败的情况。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="MakePurchaseRequest":::

## <a name="step-2-tracking-local-fulfillment-of-the-consumable"></a>步骤 2：跟踪易耗型应用内产品的本地实施

当授权客户访问易耗型应用内产品时，持续跟踪实施的产品 (*productId*) 以及跟踪与实施相关联的交易 (*transactionId*) 非常重要。

> [!IMPORTANT]
> 应用负责向应用商店准确报告实施情况。 对于维护客户公平、可靠的购买体验来说，此步骤非常重要。

以下示例说明了使用上一步骤中 [RequestProductPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync) 调用的 [PurchaseResults](/uwp/api/Windows.ApplicationModel.Store.PurchaseResults) 属性，标识要实施的已购买产品。 使用一个集合将产品信息存储在可供稍后引用的位置中，以便确认成功完成本地的实施。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GrantFeatureLocally":::

下一个示例显示了如何使用上一个示例的数组来访问产品 ID/交易 ID 对，稍后将使用它们向应用商店报告实施情况。

> [!IMPORTANT]
> 无论应用使用何种方法来跟踪和确认实施，应用都必须恪尽职守，确保客户不会为没有收到的项目付费。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="IsLocallyFulfilled":::

## <a name="step-3-reporting-product-fulfillment-to-the-store"></a>步骤 3：向 Microsoft Store 报告产品实施情况

完成本地实施后，应用必须调用 [ReportConsumableFulfillmentAsync](/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync)，其中包含 *productId* 和包括产品购买在内的交易。

> [!IMPORTANT]
> 如果无法向应用商店报告已实施的易耗型应用内产品，则会导致在上次购买的实施情况得到报告之前，用户无法再次购买该产品。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="ReportFulfillment":::

## <a name="step-4-identifying-unfulfilled-purchases"></a>步骤 4：标识未实施的购买

你的应用可以随时使用 [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 方法来检查是否存在尚未实施的易耗型应用内产品。 你应该定期调用此方法，以便检查是否存在由未参与的应用事件（如网络连接中断或应用终止）所导致的未实施的易耗品。

以下示例说明了如何使用 [GetUnfulfilledConsumablesAsync](/uwp/api/windows.applicationmodel.store.currentapp.getunfulfilledconsumablesasync) 枚举未实施的易耗品，以及应用如何通过此列表进行迭代以便完成本地实施。

> [!div class="tabbedCodeSnippets"]
:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/InAppPurchasesAndLicenses/cs/EnableConsumablePurchases.cs" id="GetUnfulfilledConsumables":::

## <a name="related-topics"></a>相关主题

* [启用应用内产品购买](enable-in-app-product-purchases.md)
* [应用商店示例（演示试用版和应用内购买）](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store)
* [Windows.ApplicationModel.Store](/uwp/api/Windows.ApplicationModel.Store)
 

 
