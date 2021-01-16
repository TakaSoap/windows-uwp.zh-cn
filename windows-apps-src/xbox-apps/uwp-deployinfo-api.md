---
title: Device Portal 部署信息 API 参考
description: 了解如何使用 Xbox 设备门户 REST API deployinfo 来请求一个或多个已安装的包的部署信息。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 2819b21e12d25aca941808e1feeb8a7539750a91
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254222"
---
# <a name="requests-deployment-information-for-one-or-more-installed-packages"></a>请求一个或多个已安装程序包的部署信息。

**请求**

| 方法 | 请求 URI |
|--------|-------------|
| POST | /ext/app/deployinfo |

**URI 参数**

 - 无

**请求标头**

- 无

**请求正文**

以下格式的 JSON 数组：

* DeployInfo
  * PackageFullName - 我们正请求其信息的程序包的名称。
  * OverlayFolder - 重叠文件夹路径的可选路径（如果使用此功能）。

### <a name="response"></a>响应

**响应正文**

以下格式的 JSON 数组（部分字段可选）：

* DeployInfo
  * PackageFullName - 我们正接收相关信息的程序包的名称。
  * DeployType - 部署类型。
  * DeployPathOrSpecifiers - 松散部署的部署路径或者打包部署的安装说明符。
  * DeployDrive - 为适用部署类型将程序包部署到的驱动器。
  * DeploySizeInBytes - 以字节表示的适用部署类型程序包的大小。
  * OverlayFolder - 支持此功能的部署的重叠文件夹。

**状态代码**

此 API 具有以下预期状态代码。

| HTTP 状态代码 | 说明 |
|------------------|-------------|
| 200 | Success |
| 4XX | 错误代码 |
| 5XX | 错误代码 |

**可用设备系列**

* Windows Xbox