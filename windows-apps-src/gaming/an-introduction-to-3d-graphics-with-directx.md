---
title: DirectX 游戏的基本 3D 图形
description: 我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。
ms.assetid: 2989c91f-7b45-7377-4e83-9daa0325e92e
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, 游戏, directx, 图形
ms.localizationpriority: medium
ms.openlocfilehash: 68ee694c036d4bc92f76ea75f7c5e5e530e26334
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173191"
---
# <a name="basic-3d-graphics-for-directx-games"></a>DirectX 游戏的基本 3D 图形



我们将介绍如何使用 DirectX 编程来实现 3D 图形的基本概念。

**目标：** 了解如何编写 3D 图形应用。

## <a name="prerequisites"></a>必备条件


我们假定你熟悉 C++。 你还需要具有图形编程概念方面的基本经验。

**完成所需总时间：** 30 分钟。

## <a name="where-to-go-from-here"></a>下一步


在这里，我们将讨论如何通过 DirectX 和 c + + Cx 开发3D 图形 \\ 。 此教程包含五个部分，向你介绍 [Direct3D](/windows/desktop/direct3d) API 以及在其他许多 DirectX 示例中也会用到的概念和代码。 这些部分从介绍如何为 UWP C++ 应用配置 DirectX，到如何设置基元的纹理和添加效果，循序渐进，逐层深入。

> **注意**   本教程使用具有列矢量的右手坐标系。 很多 DirectX 示例和应用都使用具有行向量的左手坐标系。 为了获得更完整的图形数学解决方案以及支持具有行向量的左手坐标系的解决方案，请考虑使用 [DirectXMath](/windows/desktop/dxmath/directxmath-portal)。 有关详细信息，请参阅[将 DirectXMath 与 Direct3D 结合使用](/windows/desktop/dxmath/pg-xnamath-migration-d3dx)。

 

我们将向你展示如何：

-   使用 Windows 运行时初始化 [Direct3D](/windows/desktop/direct3d) 接口。
-   应用每顶点着色器操作
-   设置几何图形
-   将场景栅格化（将 3D 场景扁平化为 2D 投影）
-   剔除隐藏的表面

> **注意**  

 

接下来，我们将创建 Direct3D 设备、交换链和呈现器目标视图，并向屏幕显示呈现的图像。

[快速入门：设置 DirectX 资源和显示图像](setting-up-directx-resources.md)

## <a name="related-topics"></a>相关主题


* [Direct3D 11 图形](/windows/desktop/direct3d11/atoc-dx-graphics-direct3d-11)
* [DXGI](/windows/desktop/direct3ddxgi/dx-graphics-dxgi)
* [HLSL](/windows/desktop/direct3dhlsl/dx-graphics-hlsl)

 

 