---
title: /users/ {ownerId} /people/ {targetid}
assetID: 9dd19e75-3b48-d7e0-fc65-6760c15ddf62
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetid.html
author: KevinAsgari
description: " /users/ {ownerId} /people/ {targetid}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 7693d9e60a9fdf58eba8aecdd8618c0a78ecef44
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3930844"
---
# <a name="usersowneridpeopletargetid"></a>/users/ {ownerId} /people/ {targetid}
调用方的用户集合中访问通过目标 ID 的人员。 这些 Uri 的域是`social.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| ownerId| 字符串| 正在访问其资源的用户的标识符。 必须匹配身份验证的用户。 可能的值为"me"、 xuid({xuid}) 或 gt({gamertag})。| 
| targetid| 字符串| 在从所有者的用户列表中，Xbox 用户 ID (XUID) 或玩家代号检索其数据的用户的标识符。 示例值： xuid(2603643534573581)、 gt(SomeGamertag)。| 
  
<a id="ID4EQB"></a>

 
## <a name="valid-methods"></a>有效的方法

[GET](uri-usersowneridpeopletargetidget.md)

&nbsp;&nbsp;调用方的用户集合中获取目标 ID 由一个人。
 
<a id="ID4E1B"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4E3B"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[统一资源标识符 (URI) 引用](../atoc-xboxlivews-reference-uris.md)

   