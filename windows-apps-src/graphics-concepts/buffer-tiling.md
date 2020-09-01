---
title: 缓冲区平铺
description: 一个缓冲区资源分为多个 64KB 的磁贴，如果最后一个磁贴的大小不是 64KB 的倍数，其中会有一些空白空间。
ms.assetid: 577DC6B0-F373-4748-AD80-2784C597C366
keywords:
- 缓冲区平铺
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: fdb78dc2cff6ccedec58acbc946068e14511fdc2
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156351"
---
# <a name="buffer-tiling"></a>缓冲区平铺


[缓冲区](introduction-to-buffers.md)资源分为64kb 个磁贴，最后一个磁贴中的某些空白空间（如果大小不是64kb 的倍数）。

结构化的缓冲区对要平铺的步幅不能有约束。 但在第一时间将其平铺会牺牲使用 [**StructuredBuffers**](/windows/desktop/direct3dhlsl/sm5-object-structuredbuffer) 时可能的硬件性能优化。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[流式资源区域的平铺方式](how-a-streaming-resource-s-area-is-tiled.md)

 

 