---
title: " (CS) 阶段的计算着色器"
description: 计算着色器 (CS) 阶段提供高速通用计算，并利用了图形处理单元 (GPU) 中的大量并行处理器。
ms.assetid: 300D4C0C-5450-45F8-9F29-E1A101D38F73
keywords:
- " (CS) 阶段的计算着色器"
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 2dfdebc0a5219d48853cab08845fe4cfb45ee94f
ms.sourcegitcommit: 21eb13a50402bf5442a5f0a4bf34800d1dc679c4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/19/2020
ms.locfileid: "90804737"
---
# <a name="compute-shader-cs-stage"></a> (CS) 阶段的计算着色器

计算着色器 (CS) 阶段提供高速通用计算，并利用了图形处理单元 (GPU) 中的大量并行处理器。 计算着色器阶段提供内存共享和线程同步功能，以允许更有效的并行编程方法。

计算着色器可以在多个线程上并行运行。

计算着色器是一种 [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)的可编程着色器阶段，它扩展了 Direct3D 以外的图形编程。 计算着色器技术也称为 *DirectCompute* 技术。 有关详细信息，另请参阅 [计算着色器概述](/windows/win32/direct3d11/direct3d-11-advanced-stages-compute-shader)。

## <a name="related-topics"></a>相关主题

[计算管道](compute-pipeline.md) 
[图形管道](graphics-pipeline.md)
