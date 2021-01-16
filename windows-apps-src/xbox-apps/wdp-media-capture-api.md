---
title: 媒体捕获 API 参考
description: 了解如何使用 Xbox 设备门户 REST API 捕获当前屏幕的 PNG 表示形式。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 3f92c8fd-4096-4972-97da-01ae5db6423c
ms.localizationpriority: medium
ms.openlocfilehash: 5d5cd87b95f923fc0303d5cd4ed7ecbff70b8209
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254192"
---
# <a name="media-capture-api-reference"></a>媒体捕获 API 参考

## <a name="request"></a>请求

你可以利用以下请求格式捕获当前屏幕，格式为 PNG。

| 方法        | 请求 URI     |
| ------------- |-----------------|
| GET           | /ext/screenshot |

**URI 参数**

可以在请求 URI 上指定以下附加参数：

| URI 参数       | 说明     |
| ------------------- |-----------------|
| 下载（可选） | 用于指示是否应设置 HTTP 响应标头、指示主浏览器是否应将屏幕截图作为附件进行下载而不是在浏览器中对其进行渲染的布尔值。 |

**请求标头**

* 无

**请求正文**

* 无

## <a name="response"></a>响应

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码   | 说明     |
| ------------------ |-----------------|
| 200                | 屏幕截图请求成功，并且捕获已返回 |
| 5XX                | 意外失败的错误代码 |

**可用设备系列**

* Windows Xbox
