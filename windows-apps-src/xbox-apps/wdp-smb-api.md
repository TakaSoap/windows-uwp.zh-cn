---
title: Device Portal SMB API 参考
description: 了解如何使用 Xbox 设备门户 REST API/ext/smb/developerfolder 通过文件资源管理器访问 Xbox One 控制台上的开发人员文件夹。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 1f0eb76e-fe3e-4674-a27e-229beec7e63d
ms.localizationpriority: medium
ms.openlocfilehash: 80a49d324c27754a2686ba4d954b47e7529df330
ms.sourcegitcommit: eb725a47c700131f5975d737bd9d8a809e04943b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/27/2020
ms.locfileid: "88970205"
---
# <a name="developer-folder-api-reference"></a>开发人员文件夹 API 参考

你可以使用标准文件资源管理器来访问 Xbox One 中与开发相关的文件。 这使你可以轻松查看文件，并将文件从电脑重新放置到主机。

**请求**

你可以利用以下请求来访问开发人员文件夹。 请求将返回：

* 文件共享的位置。 此位置可以输入到文件资源管理器的地址栏中。
* 用于访问文件共享的用户名。
* 用于访问文件共享的密码。

方法      | 请求 URI
:------     | :-----
GET | /ext/smb/developerfolder

**URI 参数**

- 无

**请求标头**

- 无

**请求正文**

- 无

**回复**   
路径 - 指向文件开发人员文件共享的路径。   
用户名 - 访问开发人员文件共享所需的用户名。   
密码 - 访问开发人员文件共享所需的密码。   

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
200 | 已授予访问文件共享凭据的请求。
4XX | 错误代码
5XX | 错误代码

**可用设备系列**

* Windows Xbox
