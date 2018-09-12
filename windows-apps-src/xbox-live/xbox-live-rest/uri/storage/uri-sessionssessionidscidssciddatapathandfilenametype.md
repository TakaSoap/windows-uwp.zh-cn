---
title: /sessions/ {sessionId} {scid} /scids/ /data/ {pathAndFileName} {类型}
assetID: f5e91763-0704-8526-77d4-c55dfec3b5a5
permalink: en-us/docs/xboxlive/rest/uri-sessionssessionidscidssciddatapathandfilenametype.html
author: KevinAsgari
description: " /sessions/ {sessionId} {scid} /scids/ /data/ {pathAndFileName} {类型}"
ms.author: kevinasg
ms.date: 20-12-2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: xbox live, xbox, 游戏, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: f0ce084fed46cce407c712ee42b782565c1174d4
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/12/2018
ms.locfileid: "3930936"
---
# <a name="sessionssessionidscidssciddatapathandfilenametype"></a>/sessions/ {sessionId} {scid} /scids/ /data/ {pathAndFileName} {类型}
下载文件。 这些 Uri 的域是`titlestorage.xboxlive.com`。
 
  * [URI 参数](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>URI 参数
 
| 参数| 类型| 说明| 
| --- | --- | --- | 
| sessionId| 字符串| 若要查找会话的 ID。| 
| scid| guid| 要查找的服务配置 ID。| 
| pathAndFileName| 字符串| 要访问的项的路径和文件名。 有效的字符 （达且包括最终正斜杠） 的路径部分包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)，下划线 (_) 和正斜杠 （/）。路径部分可能为空。有效的字符的文件名部分 （最终正斜杠后面的所有内容） 包含大写字母 (A-Z)、 小写字母 (a-z)、 数字 (0-9)，下划线 (_)，句点 （.） 和连字符 （-）。 文件名称不能为空，以句号结尾或包含两个连续句点。| 
| type| 字符串| 数据的格式。 可能的值为二进制文件或 json。| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>有效的方法

[删除 (/sessions/ {sessionId} {scid} /scids/ /data/ {pathAndFileName} {类型})](uri-sessionssessionidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;删除文件。 

[获取 (/sessions/ {sessionId} {scid} /scids/ /data/ {pathAndFileName} {类型})](uri-sessionssessionidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;下载文件。

[PUT (/sessions/ {sessionId} {scid} /scids/ /data/ {pathAndFileName} {类型})](uri-sessionssessionidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;将文件上传。 可在其中的数据和元数据发送一条消息，或作为其中的数据和元数据发送一系列的较小的块多块上载完整上载上传数据。 可以在一个消息发送仅小于 4 兆字节的文件。 数据类型 json 不支持多块上传。 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>另请参阅
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 的子磁盘） 

[标题存储 Uri](atoc-reference-storagev2.md)

   