---
ms.assetid: 7b07a6ca-4be1-497c-a901-0a2da3762555
description: 在 Microsoft Store 促销 API 中使用此方法创建、编辑和获取促销性广告活动。
title: 管理广告活动
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 促销 API, 广告活动
ms.localizationpriority: medium
ms.openlocfilehash: 0c985c9a53c233ae0c433567fcd0f88da1428542
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619623"
---
# <a name="manage-ad-campaigns"></a>管理广告活动

在 [Microsoft Store 促销 API](run-ad-campaigns-using-windows-store-services.md) 中使用这些方法来创建、编辑和获取适合你的应用的促销性广告活动。 使用此方法创建的每个活动只能与一个应用关联。

> &nbsp; 注意 &nbsp;你还可以使用合作伙伴中心创建和管理广告活动，并可以在合作伙伴中心访问以编程方式创建的市场活动。 有关在合作伙伴中心管理 ad 市场活动的详细信息，请参阅为 [应用创建 ad 市场活动](./index.md)。

使用这些方法创建或更新活动时，你通常还需要调用以下一种或多种方法来管理与活动关联的 *投放渠道*、*目标市场配置文件* 和 *创意*。 有关活动与投放渠道、目标市场配置文件和创意之间关系的详细信息，请参阅[使用 Microsoft Store 服务开展广告活动](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api)。

* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [管理广告活动的创意](manage-creatives-for-ad-campaigns.md)

## <a name="prerequisites"></a>先决条件

若要使用这些方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 促销 API 的所有[先决条件](run-ad-campaigns-using-windows-store-services.md#prerequisites)。

  > &nbsp; 注意 &nbsp;作为先决条件的一部分，请确保在[合作伙伴中心创建至少一个付费广告活动](./index.md)，并为合作伙伴中心中的广告营销活动至少添加一个付款方式。 使用此 API 创建的 ad 市场活动的传递行会自动对在合作伙伴中心的 " **ad 市场活动** " 页上选择的默认付款方式进行计费。

* [获取 Azure AD 访问令牌](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在这些方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。


## <a name="request"></a>请求

这些方法具有以下 URI。

| 方法类型 | 请求 URI                                                      |  说明  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  创建新广告活动。  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  编辑通过 *campaignId* 指定的广告活动。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/{campaignId}``` |  获取通过 *campaignId* 指定的广告活动。  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign``` |  查询广告活动。 请参阅[参数](#parameters)部分了解支持的查询参数。  |


### <a name="header"></a>标头

| 标头        | 类型   | 说明         |
|---------------|--------|---------------------|
| 授权 | 字符串 | 必需。 Azure AD 的访问令牌，采用的格式为 **持有** 者 &lt; *令牌* &gt; 。 |
| 跟踪 ID   | GUID   | 可选。 跟踪调用流的 ID。                                  |


<span id="parameters"/> 

### <a name="parameters"></a>参数

用于查询广告活动的 GET 方法支持以下可选的查询参数。

| 名称        | 类型   |  说明      |    
|-------------|--------|---------------|
| skip  |  int   | 要在查询中跳过的行数。 使用此参数分页浏览数据集。 例如，fetch=10 和 skip=0，将检索前 10 行数据；top=10 和 skip=10，将检索之后的 10 行数据，依此类推。    |       
| “etch  |  int   | 要在请求中返回的数据行数。    |       
| campaignSetSortColumn  |  string   | 将响应正文中的[活动](#campaign)对象按指定字段排序。 语法为 <em>CampaignSetSortColumn=field</em>，其中的 <em>field</em> 参数可以是以下字符串之一：</p><ul><li><strong>id</strong></li><li><strong>createdDateTime</strong></li></ul><p>默认为 **createdDateTime**。     |     
| isDescending  |  布尔   | 将响应正文中的[活动](#campaign)对象按升序或降序排列。   |         
| storeProductId  |  string   | 使用此值只返回与具有指定[应用商店 ID](in-app-purchases-and-trials.md#store-ids) 的应用关联的广告活动。 产品应用商店 ID 示例：9nblggh42cfd。   |         
| label  |  string   | 使用此值只返回包含在 [活动](#campaign)对象中指定的 *label* 的广告活动。    |


### <a name="request-body"></a>请求正文

POST 和 PUT 方法需要一个 JSON 请求正文，其中包含[活动](#campaign)对象的必填字段以及你要设置或更改的任何其他字段。


### <a name="request-examples"></a>请求示例

下面的示例演示如何调用 POST 方法来创建广告活动。

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign",
    "storeProductId": "9nblggh42cfd",
    "configuredStatus": "Active",
    "objective": "DriveInstalls",
    "type": "Community"
}
```

下面的示例演示如何调用 GET 方法来检索特定的广告活动。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign/31043481  HTTP/1.1
Authorization: Bearer <your access token>
```

下面的示例演示如何调用 GET 方法查询一组广告活动（按创建日期排序）。

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/campaign?storeProductId=9nblggh42cfd&fetch=100&skip=0&campaignSetSortColumn=createdDateTime HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>响应

这些方法返回一个 JSON 响应正文，其中包含一个或多个[活动](#campaign)对象，具体视你调用的方法而定。 下面的示例演示特定广告活动的 GET 方法的响应正文。

```json
{
    "Data": {
        "id": 31043481,
        "name": "Contoso App Campaign",
        "createdDate": "2017-01-17T10:12:15Z",
        "storeProductId": "9nblggh42cfd",
        "configuredStatus": "Active",
        "effectiveStatus": "Active",
        "effectiveStatusReasons": [
            "{\"ValidationStatusReasons\":null}"
        ],
        "labels": [],
        "objective": "DriveInstalls",
        "type": "Paid",
        "lines": [
            {
                "id": 31043476,
                "name": "Contoso App Campaign - Paid Line"
            }
        ]
    }
}
```


<span id="campaign"/>

## <a name="campaign-object"></a>活动对象

这些方法的请求和响应正文包含以下字段。 这张表列出了 POST 方法请求正文中的哪些字段是只读字段（意味着不能在 PUT 方法中更改它们）以及哪些字段是必填字段。

| 字段        | 类型   |  说明      |  只读  | 默认  | POST 必填字段 |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  广告市场活动的 ID。     |   是    |      |  否     |       
|  name   |  string   |   广告活动的名称。    |    否   |      |  是     |       
|  configuredStatus   |  string   |  以下值之一，用于指定开发人员指定的广告活动的状态： <ul><li>**活动**</li><li>**非活动**</li></ul>     |  否     |  活动    |   是    |       
|  effectiveStatus   |  string   |   以下值之一，用于根据系统验证情况指定广告活动的有效状态： <ul><li>**活动**</li><li>**非活动**</li><li>**Processing**</li></ul>    |    是   |      |   否      |       
|  effectiveStatusReasons   |  array   |  以下值中的一个或多个，用于指定广告活动处于此有效状态的原因： <ul><li>**AdCreativesInactive**</li><li>**BillingFailed**</li><li>**AdLinesInactive**</li><li>**ValidationFailed**</li><li>**已失败**</li></ul>      |  是     |     |    否     |       
|  storeProductId   |  string   |  与广告活动关联的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。 产品应用商店 ID 示例：9nblggh42cfd。     |   是    |      |  是     |       
|  标签   |  array   |   一个或多个字符串，代表活动的自定义标签。 这些标签用于搜索和标记活动。    |   否    |  null    |    否     |       
|  type   | 字符串    |  以下值之一，用于指定活动类型： <ul><li>**已付**</li><li>**楔形**</li><li>**社区**</li></ul>      |   是    |      |   是    |       
|  目标   |  string   |  以下值之一，用于指定活动的目标： <ul><li>**DriveInstall**</li><li>**DriveReengagement**</li><li>**DriveInAppPurchase**</li></ul>     |   否    |  DriveInstall    |   是    |       
|  lines   |  array   |   标识与广告活动关联的[投放渠道](manage-delivery-lines-for-ad-campaigns.md#line)的一个或多个对象。 此字段中的每个对象都由 *id* 和 *name*（分别指定投放渠道的 ID 和名称）字段组成。     |   否    |      |    否     |       
|  createdDate   |  string   |  广告活动的创建日期和时间（ISO 8601 格式）。     |  是     |      |     否    |


## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务开展广告市场活动](run-ad-campaigns-using-windows-store-services.md)
* [管理广告活动的投放渠道](manage-delivery-lines-for-ad-campaigns.md)
* [管理广告活动的目标市场配置文件](manage-targeting-profiles-for-ad-campaigns.md)
* [管理广告活动的创意](manage-creatives-for-ad-campaigns.md)
* [获取广告市场活动性能数据](get-ad-campaign-performance-data.md)