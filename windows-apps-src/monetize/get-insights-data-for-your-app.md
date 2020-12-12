---
description: 在 Microsoft Store analytics API 中使用此方法可获取应用的见解数据。
title: 获取见解数据
ms.date: 07/31/2018
ms.topic: article
keywords: windows 10，uwp，应用商店服务，Microsoft Store analytics API，见解
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cebd415b00268c9a5e8febbe175347345d830c23
ms.sourcegitcommit: 368753aea2792984857f6a57a22daed1035f1a33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97349719"
---
# <a name="get-insights-data"></a>获取见解数据

使用 Microsoft Store analytics API 中的此方法获取给定日期范围内应用的收购、运行状况和使用情况指标以及其他可选筛选器的相关信息。 合作伙伴中心的 [Insights 报告](../publish/insights-report.md) 中也提供了此信息。

## <a name="prerequisites"></a>必备条件


若要使用此方法，首先需要执行以下操作：

* 完成 Microsoft Store 分析 API 的所有[先决条件](access-analytics-data-using-windows-store-services.md#prerequisites)（如果尚未这样做）。
* [获取 Azure AD 访问令牌](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token)，以供在此方法的请求标头中使用。 获取访问令牌后，在它到期前，你有 60 分钟的使用时间。 该令牌到期后，可以获取新的令牌。

## <a name="request"></a>请求


### <a name="request-syntax"></a>请求语法

| 方法 | 请求 URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights``` |


### <a name="request-header"></a>请求头

| 标头        | 类型   | 描述                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| 授权 | 字符串 | 必需。 Azure AD 访问令牌的格式为 **Bearer** &lt;*token*&gt; 。 |


### <a name="request-parameters"></a>请求参数

| 参数        | 类型   |  描述      |  必需  
|---------------|--------|---------------|------|
| applicationId | 字符串 | 要为其检索 insights 数据的应用的 [存储 ID](in-app-purchases-and-trials.md#store-ids) 。 如果未指定此参数，响应正文将包含注册到你的帐户的所有应用的 insights 数据。  |  否  |
| startDate | 日期 | 要检索的 insights 数据的日期范围的开始日期。 默认值为当前日期之前 30 天。 |  否  |
| endDate | 日期 | 要检索的 insights 数据的日期范围的结束日期。 默认值为当前日期。 |  否  |
| filter | 字符串  | 在响应中筛选行的一条或多条语句。 每条语句包含的响应正文中的字段名称和值使用 **eq** 或 **ne** 运算符进行关联，并且语句可以使用 **and** 或 **or** 进行组合。 *filter* 参数中的字符串值必须使用单引号引起来。 例如， *filter = dataType eq ' 采集 '*。 <p/><p/>您可以指定以下筛选器字段：<p/><ul><li><strong>价格</strong></li><li><strong>性能</strong></li><li><strong>使用情况</strong></li></ul> | 是   |

### <a name="request-example"></a>请求示例

下面的示例演示获取见解数据的请求。 将 *applicationId* 值替换为你的应用的 Store ID。

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/insights?applicationId=9NBLGGGZ5QDR&startDate=6/1/2018&endDate=6/15/2018&filter=dataType eq 'acquisition' or dataType eq 'health' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>响应

### <a name="response-body"></a>响应正文

| 值      | 类型   | 描述                  |
|------------|--------|-------------------------------------------------------|
| 值      | 数组  | 包含应用的见解数据的对象数组。 有关每个对象中的数据的详细信息，请参阅下面的 " [见解值](#insight-values) " 一节。                                                                                                                      |
| TotalCount | int    | 查询的数据结果中的行总数。                 |


### <a name="insight-values"></a>见解值

*Value* 数组中的元素包含以下值。

| 值               | 类型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| applicationId       | 字符串 | 要为其检索 insights 数据的应用的存储 ID。     |
| insightDate                | 字符串 | 标识特定度量值的更改的日期。 此日期表示一周结束时间，在该时间内我们检测到，与之前的一周相比，此指标的增减量明显增加或降低。 |
| dataType     | 字符串 | 以下字符串之一，指定此见解介绍的常规分析区域：<p/><ul><li><strong>价格</strong></li><li><strong>性能</strong></li><li><strong>使用情况</strong></li></ul>   |
| insightDetail          | 数组 | 表示当前见解的详细信息的一个或多个 [InsightDetail 值](#insightdetail-values) 。    |


### <a name="insightdetail-values"></a>InsightDetail 值

| 值               | 类型   | 描述                           |
|---------------------|--------|-------------------------------------------|
| FactName           | 字符串 | 以下值之一，指示当前见解或当前维度基于 **dataType** 值描述的指标。<ul><li>对于 **运行状况**，此值始终为 **点击次数**。</li><li>对于 **购置**，此值始终为 **AcquisitionQuantity**。</li><li>对于 **用法**，此值可以为以下字符串之一：<ul><li><strong>DailyActiveUsers</strong></li><li><strong>EngagementDurationMinutes</strong></li><li><strong>DailyActiveDevices</strong></li><li><strong>DailyNewUsers</strong></li><li><strong>DailySessionCount</strong></li></ul></ul>  |
| SubDimensions         | 数组 |  一个或多个对象，这些对象描述了见解的单个指标。   |
| PercentChange            | 字符串 |  整个客户群指标变化的百分比。  |
| DimensionName           | 字符串 |  当前维度中描述的度量值的名称。 示例包括 **事件**=、 **市场**、 **DeviceType**、 **PackageVersion**、 **AcquisitionType**、 **AgeGroup** 和 **性别**。   |
| DimensionValue              | 字符串 | 当前维度中描述的度量值。 例如，如果 **DimensionName** 为 **DimensionValue** **，则可能** 是 **故障** 或 **挂起**。   |
| FactValue     | 字符串 | 检测到见解时的日期的绝对值。  |
| 方向 | 字符串 |  更改的方向 (**正面** 或 **负**) 。   |
| 日期              | 字符串 |  确定与当前见解或当前维度相关的更改的日期。   |

### <a name="response-example"></a>响应示例

以下示例举例说明此请求的 JSON 响应正文。

```json
{
  "Value": [
    {
      "applicationId": "9NBLGGGZ5QDR",
      "insightDate": "2018-06-03T00:00:00",
      "dataType": "health",
      "insightDetail": [
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "21",
              "DimensionValue:": "DE",
              "FactValue": "109",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "crash",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
        {
          "FactName": "HitCount",
          "SubDimensions": [
            {
              "FactName:": "HitCount",
              "PercentChange": "71",
              "DimensionValue:": "JP",
              "FactValue": "112",
              "Direction": "Positive",
              "Date": "6/3/2018 12:00:00 AM",
              "DimensionName": "Market"
            }
          ],
          "DimensionValue": "hang",
          "Date": "6/3/2018 12:00:00 AM",
          "DimensionName": "EventType"
        },
      ],
      "insightId": "9CY0F3VBT1AS942AFQaeyO0k2zUKfyOhrOHc0036Iwc="
    }
  ],
  "@nextLink": null,
  "TotalCount": 2
}
```

## <a name="related-topics"></a>相关主题

* [洞察报告](../publish/insights-report.md)
* [使用 Microsoft Store 服务访问分析数据](access-analytics-data-using-windows-store-services.md)
