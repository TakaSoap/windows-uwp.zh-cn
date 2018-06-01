---
author: mcleanbyron
description: 使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一应用程序的购置漏斗数据。
title: 获取应用购置漏斗数据
ms.author: mcleans
ms.date: 08/04/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store 服务, Microsoft Store 分析 API, 购置, 漏斗
ms.localizationpriority: medium
ms.openlocfilehash: 85ffe2219ce7bc1dc47ff92240eeeff1374b5f43
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/17/2018
ms.locfileid: "1663577"
---
# <a name="get-app-acquisition-funnel-data"></a>获取应用购置漏斗数据

使用 Microsoft Store 分析 API 中的此方法，可获取给定日期范围和其他可选筛选器内某一应用程序的购置漏斗数据。 还可以在 Windows 开发人员中心仪表板的[购置报告](../publish/acquisitions-report.md#acquisition-funnel)中获取此信息。

## <a name="prerequisites"></a>先决条件


若要使用此方法，首先需要执行以下操作：

* 如果尚未开始操作，请先完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt;。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  说明      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 你要检索购置漏斗数据的应用的[应用商店 ID](in-app-purchases-and-trials.md#store-ids)。 应用商店 ID 的一个示例是 9WZDNCRFJ3Q8。 |  是  |
| startDate | 日期 | 要检索的购置漏斗数据日期范围中的开始日期。 默认值为当前日期。 |  否  |
| endDate | 日期 | 要检索的购置漏斗数据日期范围中的结束日期。 默认值为当前日期。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 有关详细信息，请参阅下面的[筛选器字段](#filter-fields)部分。 | 否   |

 
### <a name="filter-fields"></a>筛选器字段

请求中的 *filter* 参数包含一条或多条语句，用于在响应中筛选行。 每条语句包含的字段和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。

支持以下筛选字段： *filter* 参数中的字符串值必须使用单引号括起来。

| 字段        |  描述        |
|---------------|-----------------|
| campaignId | 与购置关联的[自定义应用推广活动](../publish/create-a-custom-app-promotion-campaign.md)的 ID 字符串。 |
| market | 包含购置行为所在地市场的 ISO 3166 国家/地区代码的字符串。 |
| deviceType | 用于指定发生购置的设备类型的以下字符串之一：<ul><li><strong>PC</strong></li><li><strong>电话</strong></li><li><strong>控制台</strong></li><li><strong>IoT</strong></li><li><strong>全息</strong></li><li><strong>未知</strong></li></ul> |
| ageGroup | 用于指定完成购置的用户的年龄段的以下字符串之一：<ul><li><strong>0 – 17</strong></li><li><strong>18 – 24</strong></li><li><strong>25 – 34</strong></li><li><strong>35 – 49</strong></li><li><strong>50 或以上</strong></li><li><strong>未知</strong></li></ul> |
| gender | 用于指定完成购置的用户的性别的以下字符串之一：<ul><li><strong>M</strong></li><li><strong>F</strong></li><li><strong>未知</strong></li></ul> |


### <a name="request-example"></a>请求示例

以下示例演示用于获取购置漏斗数据的多个请求。 将 *applicationId* 值替换为应用的应用商店 ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/funnel?applicationId=9NBLGGGZ5QDR&startDate=8/1/2016&endDate=8/31/2016&filter=market eq 'US' and gender eq 'm'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应


### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | array  | 包含应用的购置漏斗数据的对象数组。 有关每个对象中的数据的详细信息，请参阅以下[漏斗值](#funnel-values)部分。                  |
| TotalCount | int    | 在 *Value* 数组中的对象总数。        |


### <a name="funnel-values"></a>漏斗值

在 *Value* 数组中的对象包含以下值。

| 值               | 类型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| MetricType                | string | 指定此对象中包括的[漏斗数据类型的](../publish/acquisitions-report.md#acquisition-funnel)以下字符串之一：<ul><li><strong>PageView</strong></li><li><strong>购置</strong></li><li><strong>安装</strong></li><li><strong>使用</strong></li></ul> |
| UserCount       | string | 执行 *MetricType* 值的漏斗步骤的用户的数量。             |


### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "MetricType": "PageView",
      "UserCount": 100
    },
    {
      "MetricType": "Acquisition",
      "UserCount": 80
    },
    {
      "MetricType": "Install",
      "UserCount": 50
    },
    {
      "MetricType": "Usage",
      "UserCount": 10
    }
  ],
  "TotalCount": 4
}
```

## <a name="related-topics"></a>相关主题

* [购置报告](../publish/acquisitions-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
* [获取应用购置](get-app-acquisitions.md)