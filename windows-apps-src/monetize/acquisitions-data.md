---
description: 在 Microsoft Store analytics API 中使用此方法，以 JSON 格式获取针对 UWP 应用的聚合获取数据，并使用 xbox 开发人员门户 (XDP) ，并在 XDP Analytics 仪表板中可用。
title: 获取游戏和应用的购置数据
ms.date: 03/06/2019
ms.topic: article
keywords: Windows 10, uwp, 广告网络, 应用元数据
ms.localizationpriority: medium
ms.openlocfilehash: 371f855e644a990191cd8f8b9365553bc2df9693
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349701"
---
# <a name="get-acquisitions-data-for-your-games-and-apps"></a>获取游戏和应用的购置数据 
在 Microsoft Store analytics API 中使用此方法，以 JSON 格式获取针对 UWP 应用的聚合获取数据，并使用 xbox 开发人员门户 (XDP) ，并在 XDP Analytics 仪表板中可用。 

> [!NOTE]
> 此 API 在10月 2016 1 日之前未提供每日聚合数据。 

## <a name="prerequisites"></a>必备条件
若要使用此方法，首先需要执行以下操作： 

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md)（如果尚未这样做）。 
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。 

## <a name="request"></a>请求
### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI |
| --- | --- |
| GET | `https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions` |

### <a name="request-header"></a>请求头

| 标头 | 类型 | 描述 |
| --- | --- | --- |
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 Bearer `<token>`。 |

### <a name="request-parameters"></a>请求参数

| 参数 | 类型 | 描述 | 必需 |
| --- | --- | --- | --- |
| applicationId | 字符串 | 要检索其购置数据的 Xbox One 游戏的产品 ID。 若要获取游戏的产品 ID，请导航到 XDP Analytics 计划中的游戏，并从 URL 中检索产品 ID。 或者，如果您从 "合作伙伴中心分析" 报表下载您的购买数据，则该产品 ID 将包含在 tsv 文件中。  | 是 |
| startDate | 日期 | 要检索的购置数据日期范围中的开始日期。 默认值为当前日期。  | 否 |
| endDate | 日期 | 要检索的购置数据日期范围中的结束日期。 默认值为当前日期。  | 否 |
| filter | 字符串 | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 filter 参数中的字符串值必须使用单引号引起来。 例如，*filter=market eq 'US' and gender eq 'm'*。  <br/> 可以指定响应正文中的以下字段： <ul><li>**acquisitionType**</li><li>**年**</li><li>**storeClient**</li><li>**性别**</li><li>**营销**</li><li>**osVersion**</li><li>**deviceType**</li><li>**sandboxId**</li></ul> | 否 |
| aggregationLevel | 字符串 | 指定用于检索聚合数据的时间范围。 可以是以下字符串之一：**day**、**week** 或 **month**。 如果未指定，默认值为 **day**。  | 否 |
| orderby | 字符串 | 对每个购置的结果数据值进行排序的语句。 语法为 *orderby = field [order]，field [order],...**字段* 参数可以为以下字符串之一： <ul><li>**date**</li><li>**acquisitionType**</li><li>**年**</li><li>**storeClient**</li><li>**性别**</li><li>**营销**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> *order* 参数是可选的，可以是 **asc** 或 **desc**，用于指定每个字段的升序或降序排列。 默认值为 **asc**。 下面是一个 *orderby* 字符串的示例：*orderby=date,market*  | 否 |
| groupby | 字符串 | 仅将数据聚合应用于指定字段的语句。 可以指定的字段如下所示： <ul><li>**date**</li><li>**applicationName**</li><li>**acquisitionType**</li><li>**ageGroup**</li><li>**storeClient**</li><li>**性别**</li><li>**营销**</li><li>**osVersion**</li><li>**deviceType**</li><li>**paymentInstrumentType**</li><li>**sandboxId**</li><li>**xboxTitleIdHex**</li></ul> 返回的数据行会包含 *groupby* 参数中指定的字段，以及以下字段： <ul><li>**date**</li><li>**applicationId**</li><li>**acquisitionQuantity**</li></ul> *groupby* 参数可以与 aggregationLevel 参数结合使用。 例如： *&groupby = ageGroup，marketplace&aggregationLevel = 周*  | 否 |

### <a name="request-example"></a>请求示例
以下示例演示用于获取 Xbox One 游戏购置数据的多个请求。 用游戏的产品 ID 替换 *applicationId* 值。  

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&top=10&skip=0 HTTP/1.1 
Authorization: Bearer <your access token> 

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/acquisitions?applicationId=9WZDNCRFHXHT&startDate=1/1/2017&endDate=2/1/2019&skip=0&filter=market eq 'US' and gender eq 'm' HTTP/1.1 
Authorization: Bearer <your access token> 
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文
| 值 | 类型 | 描述 |
| --- | --- | --- |
| 值 | 数组 | 包含游戏聚合购置数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[购置值](#acquisition-values)部分。 |
| TotalCount | integer | 查询的数据结果中的行总数。 |

### <a name="acquisition-values"></a>购置值 
*Value* 数组中的元素包含以下值。 

| 值 | 类型 | 描述 |
| --- | --- | --- |
| date | 字符串 | 购置数据的日期范围内的第一个日期。 如果请求指定了某一天，此值就是该日期。 如果请求指定了一周、月或其他日期范围，此值是该日期范围内的第一个日期。 |
| applicationId | 字符串 | 要检索其购置数据的 Xbox One 游戏的产品 ID。 |
| applicationName | 字符串 | 游戏的显示名称。 |
| acquisitionType | 字符串 | 下列字符串之一，用于指示购置类型：  <ul><li>**免费**</li><li>**试用**</li><li>**已付**</li><li>**促销代码**</li><li>**Iap**</li><li>**订阅 Iap**</li><li>**私人受众**</li><li>**前序**</li><li>**Xbox Game Pass**（或者，如果在 2018 年 3 月 23 之前查询数据，则是 **Game Pass**）</li><li>**磁盘**</li><li>**预付码**</li><li>**收费的前序**</li><li>**已取消的前置顺序**</li><li>**失败的前置顺序**</li></ul> |
| age | 字符串 | 用于指示进行购置的用户的年龄段的以下字符串之一： <ul><li>**小于13**</li><li>**13-17**</li><li>**18-24**</li><li>**25-34**</li><li>**35-44**</li><li>**44-55**</li><li>**大于55**</li><li>**Unknown**</li></ul> |
| deviceType | 字符串 | 用于指定完成购置的设备类型的以下字符串之一： <ul><li>**PC**</li><li>**电话**</li><li>**控制台-Xbox One**</li><li>**控制台-Xbox 系列 X**</li><li>**IoT**</li><li>**服务器**</li><li>**平板电脑**</li><li>**Holographic**</li><li>**Unknown**</li></ul> |
| gender | 字符串 | 用于指定进行购置的用户的性别的以下字符串之一： <ul><li>**m**</li><li>**f**</li><li>**Unknown**</li></ul> |
| market | string | 发生购置行为的市场的 ISO 3166 国家/地区代码。 |
| osVersion | 字符串 | 发生购置行为的操作系统版本。 对于此方法，此值始终是 **Windows 10**。 |
| paymentInstrumentType | 字符串 | 用于指示用于购置的付款说明的以下字符串之一： <ul><li>**信用卡**</li><li>**直接借记卡**</li><li>**推断的购买**</li><li>**MS 余额**</li><li>**移动运营商**</li><li>**在线银行转帐**</li><li>**PayPal**</li><li>**分割交易**</li><li>**预付码兑换**</li><li>**已支付金额为零**</li><li>**eWallet**</li><li>**Unknown**</li></ul> |
| sandboxId | 字符串 | 为游戏创建的沙盒的 ID。 此值可以是 " **零售** " 或 "私有沙箱 ID"。 |
| storeClient | 字符串 | 用于指示发生购置的 Microsoft Store 版本的以下字符串之一： <ul><li>**Windows Phone Store (client)**</li><li>**Microsoft Store (client)**（或 **Windows Store (client)**，如果查询 2018 年 3 月 23 日之前的数据） </li><li>**Microsoft Store (web)**（或 **Windows Store (web)**，如果查询 2018 年 3 月 23 日之前的数据） </li><li>**Volume purchase by organizations**</li><li>**其他**</li></ul> |
| xboxTitleId | 字符串 | Xbox 开发人员门户 (XDP) 为支持 Xbox Live 的游戏指定的 Xbox Live 标题 ID（以十六进制值表示）。 |
| acquisitionQuantity | number | 在指定的聚合级别期间发生的购置数。 |
| purchasePriceUSDAmount | number | 客户为购置所付金额，已使用每月汇率转换为美元。 |
| purchaseTaxUSDAmount | number | 购置税额，已转换为美元。 |
| localCurrencyCode | 字符串 | 基于合作伙伴中心帐户的国家/地区的本地货币代码。  |
| xboxProductId | 字符串 | XDP 中产品的 Xbox 产品 ID （如果适用）。  |
| availabilityId | 字符串 | XDP 中产品的可用性 ID （如果适用）。  |
| skuId | 字符串 | XDP 中产品的 SKU ID （如果适用）。  |
| skuDisplayName  | 字符串 | XDP 中产品的 SKU 显示名称（如果适用）。  |
| xboxParentProductId | 字符串 | XDP 中产品的 Xbox 父产品 ID （如果适用）。  |
| parentProductName | 字符串 | XDP 中产品的父产品名称（如果适用）。  |
| productTypeName | 字符串 | XDP 中产品的产品类型名称（如果适用）。  |
| purchaseTaxType | 字符串 | 从 XDP 购买产品的税务类型（如果适用）。  |
| purchasePriceLocalAmount | number | 从 XDP 购买产品的本地价格（如果适用）。  |
| purchaseTaxLocalAmount | number | 如果适用，请从 XDP 购买产品的增值税当地金额。  |

### <a name="response-example"></a>响应示例
以下示例举例说明此请求的 JSON 响应正文。 

```JSON
{ 
    "Value": [ 
        { 
            "date": "2019-01-15T01:00:00.0000000Z", 
            "applicationId": "9WZDNCRFHXHT", 
            "applicationName": null, 
            "acquisitionType": "Paid", 
            "age": null, 
            "deviceType": "Phone", 
            "gender": null, 
            "market": "US", 
            "osVersion": "Windows 10", 
            "paymentInstrumentType": null, 
            "sandboxId": "RETAIL", 
            "storeClient": "Microsoft Store (client)", 
            "xboxTitleId": null, 
            "localCurrencyCode": "USD", 
            "xboxProductId": null, 
            "availabilityId": "B42LRTSZ2MCJ", 
            "skuId": "0010", 
            "skuDisplayName": null, 
            "xboxParentProductId": null, 
            "parentProductName": null, 
            "productTypeName": "Game", 
            "purchaseTaxType": "TaxesNotIncluded", 
            "acquisitionQuantity": 1, 
            "purchasePriceUSDAmount": 3.08, 
            "purchasePriceLocalAmount": 3.08, 
            "purchaseTaxUSDAmount": 0.09, 
            "purchaseTaxLocalAmount": 0.09 
        } 
    ], 

    "@nextLink": null,
    
    "TotalCount": 12221 
} 
```

 
