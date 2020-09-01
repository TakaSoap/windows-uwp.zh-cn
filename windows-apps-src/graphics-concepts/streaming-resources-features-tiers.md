---
title: 流式资源功能层
description: 访问有关 Direct3D 流式处理资源的三层功能功能的文章，以前称为平铺资源。
ms.assetid: 6AE7EA72-3929-4BB4-8780-F0CF26192D87
keywords:
- 流式资源功能层
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: ee27244c4d4c2797b71c9d5c8c2c5185a99596b5
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173051"
---
# <a name="streaming-resources-features-tiers"></a>流式资源功能层


Direct3D 具有三层功能，可为流式资源提供支持。

第 1 层提供了用于流式资源的基本功能。

第 2 层在第 1 层基础上增加了一些功能，如在大小至少为一个标准磁贴形状时保证非环绕纹理的 mipmap；具有用于固定详细信息级别 (LOD) 及用于获取着色器操作相关状态的着色器指令；此外，可读取将采样值视为零的 NULL 映射磁贴。

第 3 层功能在第 2 层的基础上增加了 Texture3D 功能。

Direct3D 版本提供查询功能，可验证支持流式资源的硬件和驱动程序及其所处的层级别。

## <a name="span-idin-this-sectionspanin-this-section"></a><span id="in-this-section"></span>本部分中的内容


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">主题</th>
<th align="left">描述</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="tier-1.md">第 1 层</a></p></td>
<td align="left"><p>本部分将介绍第 1 层支持。</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="tier-2.md">第 2 层</a></p></td>
<td align="left"><p>第 2 层流式资源支持在第 1 层基础上增加了一些功能，如在大小至少为一个标准磁贴形状时保证非环绕纹理的 mipmap；具有用于固定详细信息级别 (LOD) 及用于获取着色器操作相关状态的着色器指令；此外，可读取将采样值视为零的 NULL 映射磁贴。</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="tier-3.md">第 3 层</a></p></td>
<td align="left"><p>第3层除了 <a href="tier-2.md">第2层</a> 功能外，还添加了对 Texture3D for 流式处理资源的支持。</p></td>
</tr>
</tbody>
</table>

 

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源](streaming-resources.md)

 

 




