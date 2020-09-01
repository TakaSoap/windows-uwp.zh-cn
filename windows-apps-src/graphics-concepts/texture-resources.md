---
title: 纹理资源
description: 了解如何使用 Direct3D 纹理资源进行呈现，以及如何使用纹理阶段支持多种纹理混合。
ms.assetid: 016F6CDA-D361-4E6B-BA99-49E915A19364
keywords:
- 纹理资源
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: d82a5525601c98812d6aab97f5f5d4399ceddc91
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89156181"
---
# <a name="texture-resources"></a>纹理资源


纹理是一种用于呈现的资源。

## <a name="span-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanspan-idrendering_with_texture_resourcesspanrendering-with-texture-resources"></a><span id="Rendering_with_Texture_Resources"></span><span id="rendering_with_texture_resources"></span><span id="RENDERING_WITH_TEXTURE_RESOURCES"></span>用纹理资源进行渲染


Direct3D 支持通过纹理层实现多纹理混合的概念。 每个纹理层包括一个纹理以及可在该纹理上执行的操作。 纹理层中的纹理组成当前纹理的集合。 参见[纹理混合](texture-blending.md)。 纹理层中包含了每个纹理的状态。

你还可以通过应用程序设置纹理视角以及纹理筛选状态。 参见[纹理筛选](texture-filtering.md)。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[纹理](textures.md)

 

 




