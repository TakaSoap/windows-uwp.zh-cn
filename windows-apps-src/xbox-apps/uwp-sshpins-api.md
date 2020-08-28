---
title: 设备门户 SSH Pin 码 API 参考
description: 了解如何使用/ext/app/sshpins Xbox 设备门户 REST API 以编程方式删除所有受信任的安全外壳 (SSH) pin。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 307af4cdd4e998832f4a2fe7a8f874615fe10ad7
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043529"
---
# <a name="ssh-pins-api-reference"></a>SSH Pin 码 API 参考
可以使用此 REST API 删除开发工具包上的所有受信任 SSH Pin 码。

## <a name="remove-trusted-ssh-pins"></a>删除受信任的 SSH Pin 码

**请求**

方法      | 请求 URI
:------     | :-----
DELETE | /ext/app/sshpins
<br />
**URI 参数**

- 无

**请求标头**

- 无

**请求正文**   

- 无

**响应**   

- 无 

**状态代码**

此 API 具有以下预期状态代码。

HTTP 状态代码      | 说明
:------     | :-----
204 | 清除 Pin 码的请求已成功。
4XX | 错误代码
5XX | 错误代码

<br />
**可用设备系列**

* Windows Xbox

