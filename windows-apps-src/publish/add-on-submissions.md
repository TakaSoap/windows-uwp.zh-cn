---
description: 了解如何提交外接程序（也称为应用内产品）作为客户可购买的应用的补充项。
title: 加载项提交
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, iap, 应用内购买, 应用内产品, iap 提交
ms.localizationpriority: medium
ms.openlocfilehash: 25246d192f40bae096c33fae00feb1bfc2a4c530
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162142"
---
# <a name="add-on-submissions"></a>加载项提交

加载项（有时称为应用内产品）是应用的补充项，可供客户购买。 外接程序可能是一项有趣的新功能，也可能是新游戏级别，或者你认为的任何其他内容会使用户保持联系。 加载项不但是赚钱的绝佳方法，它们还有助于促进客户互动和参与。

外接程序通过 [合作伙伴中心](https://partner.microsoft.com/dashboard)发布，并要求你拥有有效的 [开发人员帐户](https://developer.microsoft.com/store/register)。 你还需要在你的应用代码中[启用加载项](../monetize/in-app-purchases-and-trials.md)。

外接程序提交过程的第一步是通过 [定义产品类型和产品 ID](set-your-add-on-product-id.md)在合作伙伴中心创建外接程序。 之后，你将创建一个提交，以便可通过 Microsoft Store 购买你的外接程序。 你可以在[提交应用](app-submissions.md)的同时提交加载项，或者可以单独处理它。 并且你可以在应用在 Store 中上架后[更新](#updating-an-add-on-after-publication)加载项，而无需重新提交该应用。

> [!NOTE]
> 文档的本节介绍如何在合作伙伴中心提交加载项。 此外，你也可以使用 [Microsoft Store 提交 API](../monetize/create-and-manage-submissions-using-windows-store-services.md) 自动执行加载项提交。


## <a name="checklist-for-submitting-an-add-on"></a>加载项提交清单

以下是你在创建加载项提交时提供的信息的列表。 下面标出了你需要提供的项目。 其中某些是选填的，或已提供你可以按需更改的默认值。


### <a name="create-a-new-add-on-page"></a>创建新加载项页面

| 字段名称                    | 说明                            |
|-------------------------------|----------------------------------|
| [**产品类型**](set-your-add-on-product-id.md#product-type)      | 必需 |  
| [**产品 ID**](set-your-add-on-product-id.md#product-id)          | 必需 |        


### <a name="properties-page"></a>“属性”页

| 字段名称                    | 说明                              |   
|-------------------------------|------------------------------------|
| [**产品生命周期**](enter-add-on-properties.md#product-lifetime)  | 如果产品类型为 **持久性**，则为必需。 不适用于其他产品类型。 |
| [**数量**](enter-add-on-properties.md#quantity)  | 如果产品类型为 **存储托管的可耗用项**，则为必需项。 不适用于其他产品类型。 |
| [**订阅期限**](enter-add-on-properties.md#subscription-period)          | 在产品类型为**订阅**时必填。 不适用于其他产品类型。       |  
| [**免费试用版**](enter-add-on-properties.md#free-trial)          | 在产品类型为**订阅**时必填。 不适用于其他产品类型。       |
| [**内容类型**](enter-add-on-properties.md#content-type)          | 必需    |               
| [**关键字**](enter-add-on-properties.md#keywords)                  | 选填（最多 10 个关键字，每个限 30 个字符） |
| [**自定义开发人员数据**](enter-add-on-properties.md#custom-developer-data)   | 选填（限 3000 个字符）            |


### <a name="pricing-and-availability-page"></a>定价和可用性页面

| 字段名称                    | 说明                                       |
|-------------------------------|---------------------------------------------|
| [**市场**](set-add-on-pricing-and-availability.md#markets)  | 默认：所有可能的市场 |
| [**能见度**](set-add-on-pricing-and-availability.md#visibility)   | 默认：可供购买。 可能在应用一览中显示 |
| [**计划**](set-add-on-pricing-and-availability.md#schedule)    | 默认：尽快发布
| [**定价**](set-add-on-pricing-and-availability.md#pricing)                | 必需                                    |
| [**销售定价**](put-apps-and-add-ons-on-sale.md)               | 可选                    |


### <a name="store-listings"></a>应用商店一览

需要一个 Store 一览。 建议为应用支持的每种 [语言](create-add-on-store-listings.md#store-listing-languages) 提供存储列表。

| 字段名称                    | 说明                                       |
|-------------------------------|---------------------------------------------|
| [**标题**](create-add-on-store-listings.md#title)                    | 必填（限 100 个字符）           |
| [**说明**](create-add-on-store-listings.md#description)       | 选填（限 200 个字符）            |
| [**按钮**](create-add-on-store-listings.md#icon)                    | 可选（.png、300x300 像素）            |


完成输入此信息后，单击 **"提交到应用商店"**。 在大多数情况下，认证过程需要大约一个小时。 然后，你的加载项将发布到 Store 并且可供客户购买。

> [!NOTE]
> 加载项还必须在应用的代码中实现。 有关详细信息，请参阅[应用内购买和试用](../monetize/in-app-purchases-and-trials.md)。


## <a name="updating-an-add-on-after-publication"></a>在发布后更新加载项

你可以随时更改已发布的加载项。 外接程序更改是独立于您的应用程序提交和发布的，因此您通常不需要更新整个应用程序即可更改加载项，例如更新其价格或说明。

若要提交更新，请在合作伙伴中心中转到 "外接程序" 页，然后单击 " **更新**"。 这将为外接程序创建新的提交，并使用之前提交的信息作为起点。 进行所需的更改，然后单击 **"提交到应用商店"**。

如果你希望删除之前提供的加载项，可通过创建新提交并通过**停止获取**选项将[分发和可见性](set-add-on-pricing-and-availability.md)选项更改为**在 Microsoft Store 中隐藏**。 请确保根据需要更新应用程序的代码，以删除对外接程序的引用 (尤其是以前发布的应用程序支持 Windows 8.1 之前的情况;此可见性设置不适用于) 的客户。

> [!IMPORTANT]
> 如果你以前发布的应用可供 Windows 8.x 上的客户使用，你将需要创建并发布新的应用程序提交，以使加载项更新对这些客户可见。 同样，如果你在应用发布后向面向 Windows 8.x 的应用添加新的加载项，你将需要更新应用的代码来引用这些加载项，然后才能重新提交应用。 否则，使用 Windows 8.x 的客户将无法看到新的加载项。
