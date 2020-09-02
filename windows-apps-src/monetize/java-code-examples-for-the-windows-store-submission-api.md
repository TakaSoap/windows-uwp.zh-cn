---
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: 使用此部分中的 Java 代码示例了解有关使用 Microsoft Store 提交 API 的详细信息。
title: Java 示例 - 应用、加载项和外部测试版的提交
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, Microsoft Store 提交 API, 代码示例, java
ms.localizationpriority: medium
ms.openlocfilehash: d10390dbb5364ff4f05de211167551d91dfab858
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363930"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Java 示例：应用、加载项和外部测试版的提交

本文提供 Java 代码示例演示如何使用 [Microsoft Store 提交 API](create-and-manage-submissions-using-windows-store-services.md) 执行以下任务：

* [获取 Azure AD 访问令牌](#token)
* [创建加载项](#create-add-on)
* [创建软件包外部测试版](#create-package-flight)
* [创建应用提交](#create-app-submission)
* [创建加载项提交](#create-add-on-submission)
* [创建软件包外部测试版提交](#create-flight-submission)

你可以查看每个示例，了解有关它所演示的任务的详细信息，也可以将本文中的所有代码示例生成到控制台应用程序。 有关完整的代码列表，请参阅本文末尾的[代码列表](java-code-examples-for-the-windows-store-submission-api.md#code-listing)部分。

## <a name="prerequisites"></a>先决条件

这些示例使用以下库：

* [Apache Commons Logging 1.2](https://commons.apache.org/proper/commons-logging/)  (commons-logging-1.2.jar)。
* [Apache HttpComponents Core 4.4.5 和 Apache HttpComponents Client 4.5.2](https://hc.apache.org/)（httpcore-4.4.5.jar 和 httpclient-4.5.2.jar）。
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) 和 [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4)（javax.json-api-1.0.jar 和 javax.json-1.0.4.jar）。

## <a name="main-program-and-imports"></a>主程序和导入

以下示例显示了所有代码示例使用的导入语句，并示例了可调用其他示例方法的命令行程序。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/MainExample.java" range="1-64":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>获取 Azure AD 访问令牌

以下示例演示如何[获取 Azure AD 访问令牌](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token)，你可以使用此令牌在 Microsoft Store 提交 API 中调用方法。 获取令牌后，可以在 60 分钟的令牌有效期内，使用该令牌调用“Microsoft Store 提交 API”。 该令牌到期后，可以重新生成一个。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="65-95":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>创建加载项

以下示例演示如何[创建](create-an-add-on.md)再[删除](delete-an-add-on.md)加载项。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="310-345":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>创建软件包外部测试版

以下示例演示如何[创建](create-a-flight.md) 和[删除](delete-a-flight.md) 软件包外部测试版。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="185-221":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>创建应用提交

以下示例介绍如何使用 Microsoft Store 提交 API 中的多种方法创建应用提交。 为此，此 `SubmitNewApplicationSubmission` 方法会创建新的提交，作为上次发布的提交的克隆，然后更新并将克隆的提交内容提交到合作伙伴中心。 具体来说，`SubmitNewApplicationSubmission` 方法执行以下任务：

1. 首先，此方法[获取指定应用的数据](get-an-app.md)。
2. 接下来，此方法会[删除应用的挂起提交](delete-an-app-submission.md)（如果存在）。
3. 然后，此方法会[创建新的应用提交](create-an-app-submission.md)（新提交时是上次发布的提交副本）。
4. 它会更改新提交的部分详细信息并将新的提交包上载到 Azure Blob 存储。
5. 接下来，它将 [更新](update-an-app-submission.md) 并 [提交](commit-an-app-submission.md) 到合作伙伴中心的新提交。
6. 最后，它会定期[检查新提交的状态](get-status-for-an-app-submission.md)，直到其成功提交。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="97-183":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>创建加载项提交

以下示例介绍如何使用 Microsoft Store 提交 API 中的多种方法创建加载项提交。 为此，此 `SubmitNewInAppProductSubmission` 方法会创建一个新的提交，作为上次发布的提交的克隆，然后将克隆的提交更新并提交到合作伙伴中心。 具体来说，`SubmitNewInAppProductSubmission` 方法执行以下任务：

1. 首先，此方法[获取指定加载项的数据](get-an-add-on.md)。
2. 接下来，此方法会[删除加载项的挂起提交](delete-an-add-on-submission.md)（如果存在）。
3. 然后，该方法会 [创建新的加载项提交](create-an-add-on-submission.md)（新提交是上次发布的提交的副本）。
4. 该方法会将包含提交图标的 ZIP 存档上传至 Azure Blob 存储。
5. 接下来，它将 [更新](update-an-add-on-submission.md) 并 [提交](commit-an-add-on-submission.md) 到合作伙伴中心的新提交。
6. 最后，它会定期[检查新提交的状态](get-status-for-an-add-on-submission.md)，直到其成功提交。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="347-431":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>创建软件包外部测试版提交

以下示例介绍如何使用 Microsoft Store 提交 API 中的多种方法创建软件包外部测试版提交。 为此，此 `SubmitNewFlightSubmission` 方法会创建一个新的提交，作为上次发布的提交的克隆，然后将克隆的提交更新并提交到合作伙伴中心。 具体来说，`SubmitNewFlightSubmission` 方法执行以下任务：

1. 首先，此方法[获取指定软件包外部测试版的数据](get-a-flight.md)。
2. 接下来，此方法会[删除软件包外部测试版的挂起提交](delete-a-flight-submission.md)（如果存在）。
3. 然后，此方法[会创建新的软件包外部测试版提交](create-a-flight-submission.md)（新提交时是上次发布的提交副本）。
4. 它会将新的提交程序包上载到 Azure Blob 存储。
5. 接下来，它将 [更新](update-a-flight-submission.md) 并 [提交](commit-a-flight-submission.md) 到 PartnerCenter 的新提交。
6. 最后，它会定期[检查新提交的状态](get-status-for-a-flight-submission.md)，直到其成功提交。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="223-308":::

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>上传提交文件和处理请求响应的实用程序方法

这些实用程序方法演示了以下任务：

* 如何将包含新的应用或加载项提交资源的 ZIP 存档上载到 Azure Blob 存储。 有关将 ZIP 存档上载到应用和加载项提交的 Azure Blob 存储的详细信息，请参阅[创建应用提交](manage-app-submissions.md#create-an-app-submission)、[创建加载项提交](manage-add-on-submissions.md#create-an-add-on-submission)和[创建软件包外部测试版提交](manage-flight-submissions.md#create-a-package-flight-submission)中的相关说明。
* 如何处理请求响应。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="433-490":::

<span id="code-listing" />

## <a name="complete-code-listing"></a>完整代码清单

以下代码列表包含组织到一个源文件中的所有以前示例。

:::code language="java" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/java/CompleteExample.java" range="1-491":::

## <a name="related-topics"></a>相关主题

* [使用 Microsoft Store 服务创建和管理提交](create-and-manage-submissions-using-windows-store-services.md)
