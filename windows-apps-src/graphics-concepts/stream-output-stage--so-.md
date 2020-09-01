---
title: 流输出 (SO) 阶段
description: 流输出 (SO) 阶段可将顶点数据从之前的有效阶段输出（或流式传输）至内存中的一个或多个缓冲区。 流出到内存的数据可以作为输入数据再次循环回到管道，或者从 CPU 读回。
ms.assetid: DE89E99F-39BC-4B34-B80F-A7D373AA7C0A
keywords:
- 流输出 (SO) 阶段
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: f56036ecc083d72f552954860d04750c1c83b8b6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156201"
---
# <a name="stream-output-so-stage"></a>流输出 (SO) 阶段


流输出 (SO) 阶段可将顶点数据从之前的有效阶段输出（或流式传输）至内存中的一个或多个缓冲区。 流出到内存的数据可以作为输入数据再次循环回到管道，或者从 CPU 读回。

## <a name="span-idpurpose_and_usesspanspan-idpurpose_and_usesspanspan-idpurpose_and_usesspanpurpose-and-uses"></a><span id="Purpose_and_uses"></span><span id="purpose_and_uses"></span><span id="PURPOSE_AND_USES"></span>目的和使用


![流输出阶段在管道中的位置图](images/d3d10-pipeline-stages-so.png)

流输出阶段可将管道中的基元数据沿着到光栅器的路线流到内存中。 之前阶段的数据可流出到内存，和/或传入光栅器。 流出到内存的数据可以作为输入数据再次循环回到管道，或者从 CPU 读回。

流出到内存的数据可在后续渲染传递中读回到管道，也可复制到暂存资源（以便由 CPU 读取）中。 流出的数据量各有不同；Direct3D 旨在处理数据，无需查询 (GPU) 写入数据量。--&gt;

将流输出数据馈送至管道的方式有两种：

-   可将流输出数据回送至输入汇编程序 (IA) 阶段。
-   流输出数据可由可编程的着色器使用[加载](/windows/desktop/direct3dhlsl/dx-graphics-hlsl-to-load)功能读取。

## <a name="span-idinputspanspan-idinputspanspan-idinputspaninput"></a><span id="Input"></span><span id="input"></span><span id="INPUT"></span>送


来自之前着色器阶段的顶点数据。

## <a name="span-idoutputspanspan-idoutputspanspan-idoutputspanoutput"></a><span id="Output"></span><span id="output"></span><span id="OUTPUT"></span>输出


流输出 (SO) 阶段连续地将来自前一活动阶段的顶点数据（例如几何着色器 (GS) 阶段）输出（或流入）到内存中的一个或多个缓冲区。 如果 (GS) 阶段的几何图形着色器处于非活动状态，则流输出 (因此) 阶段会持续从域着色器将顶点数据输出 (DS) 阶段到内存中的缓冲区 (或者 DS 也处于非活动状态，从顶点着色器 (VS) 阶段) 。

三角形或直线带与输入汇编程序 (IA) 阶段绑定时，每个带都会在流出之前转换为列表。顶点始终展开写成完整基元（例如，三角形一次为 3 个顶点）；不完整的基元绝不会流出。相邻的基元类型会在数据流出之前放弃相邻数据。

流输出阶段支持最多同时 4 个缓冲区。

-   如果你将数据传入多个缓冲区，那么每个缓冲区仅可采集一个逐顶点元素（最多 4 个组件），隐含数据步幅等于各缓冲区中的元素宽度（兼容针对着色器阶段输入绑定单元素缓冲区的方式）。 此外，如果缓冲区的大小不同，那么在任何一个缓冲区的容量满时，写入即会停止。
-   如果你将数据流式传入单个缓冲区，那么该缓冲区最多可采集 64 个逐顶点数据标量组件（256 字节或更少），而顶点步幅最高为 2048 字节。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[图形管道](graphics-pipeline.md)

 

 