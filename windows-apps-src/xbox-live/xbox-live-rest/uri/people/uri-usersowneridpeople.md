---
title: /users/ {ownerId} / 个人
assetID: 9745a93c-720e-606d-bff2-ad0ec610ed98
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeople.html
author: KevinAsgari
description: " /users/ {ownerId} / 个人"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 9778c5519a52291754d08fa8c1f4c4ed163967e1
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3935061"
---
# <a name="usersowneridpeople"></a>/users/ {ownerId} / 个人
访问调用方的用户集合。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 正在访问其资源的用户的标识符。 必须匹配身份验证的用户。 可能的值为"me"、 xuid({xuid}) 或 gt({gamertag})。| 
  
<a id="ID4EOB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersowneridpeopleget.md)

&nbsp;&nbsp;获取调用方的用户集合。
 
<a id="ID4EYB"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E1B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   