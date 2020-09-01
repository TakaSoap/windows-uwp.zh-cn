---
title: 纹理块压缩
description: Direct3D 11 中的纹理块压缩 (BC) 支持已经过扩展，以包含 BC6H 和 BC7 算法。
ms.assetid: 63506C46-BF14-464B-B20C-8B8F359E7AFE
keywords:
- 纹理块压缩
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 06c132f23d22dc688f82ff7976bcb93108f2ee1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89158811"
---
# <a name="texture-block-compression"></a>纹理块压缩


Direct3D 11 中的纹理块压缩 (BC) 支持已经过扩展，以包含 BC6H 和 BC7 算法。 BC6H 支持高动态范围颜色源数据，而 BC7 以较少的平均 RGB 源数据项目提供优于平均质量的压缩。

有关 Direct3D 11 之前纹理块压缩算法支持的更多详细信息（包括针对 BC1 至 BC5 格式的支持），请参见[纹理块压缩 (Direct3D 10)](/windows/desktop/direct3d10/d3d10-graphics-programming-guide-resources-block-compression)。

**有关文件格式的说明：  ** BC6H 和 BC7 纹理压缩格式使用 DDS 文件格式存储压缩的纹理数据。 要了解更多信息，请参见 [DDS 编程指南](/windows/desktop/direct3ddds/dx-graphics-dds-pguide)。

## <a name="span-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanspan-idblock_compression_formats_supported_in_direct3d_11spanblock-compression-formats-supported-in-direct3d-11"></a><span id="Block_Compression_Formats_Supported_in_Direct3D_11"></span><span id="block_compression_formats_supported_in_direct3d_11"></span><span id="BLOCK_COMPRESSION_FORMATS_SUPPORTED_IN_DIRECT3D_11"></span>Direct3D 11 中支持的块压缩格式


| 源数据                                  | 数据压缩分辨率最低要求                              | 推荐格式 | 支持的最低功能级别 |
|----------------------------------------------|---------------------------------------------------------------------------|--------------------|---------------------------------|
| alpha 通道可选三个通道颜色       | 三种颜色的通道（5 位：6 位：5 位），alpha 通道为 0 或 1 位  | BC1                | Direct3D 9.1                    |
| alpha 通道可选三个通道颜色       | 三种颜色的通道（5 位：6 位：5 位），alpha 通道为 4 位         | BC2                | Direct3D 9.1                    |
| alpha 通道可选三个通道颜色       | 三种颜色的通道（5 位：6 位：5 位），alpha 通道为 8 位          | BC3                | Direct3D 9.1                    |
| 单通道的颜色                            | 单色通道（8 位）                                                | BC4                | Direct3D 10                     |
| 两个通道的颜色                            | 双色通道（8 位：8 位）                                        | BC5                | Direct3D 10                     |
| 三个信道高动态范围 (HDR) 颜色 | 三个颜色通道 (16 位：16位：16位) 在 "半" 浮点中\* | BC6H               | Direct3D 11                     |
| alpha 通道可选三个通道颜色  | 三种颜色的通道（每个通道为 4 位到 7 位），alpha 通道为 0 至 8 位  | BC7                | Direct3D 11                     |

 

\*"半" 浮点是一个16位值，其中包含一个可选的符号位、5位偏差指数和10或11位尾数。
## <a name="span-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanspan-idbc1__bc2__and_b3_formatsspanbc1-bc2-and-b3-formats"></a><span id="BC1__BC2__and_B3_Formats"></span><span id="bc1__bc2__and_b3_formats"></span><span id="BC1__BC2__AND_B3_FORMATS"></span>BC1、BC2 和 B3 格式


BC1、BC2 以及 BC3 格式相当于 Direct3D 9 DXTn 纹理压缩格式，并且与对应的 Direct3D 10 BC1、BC2 以及 BC3 格式相同。 所有功能级别都需要支持这三种格式 (D3D \_ 功能 \_ 级别 \_ 9 \_ 1、d3d \_ 功能 \_ 级别 \_ 9 \_ 2、d3d 功能 \_ \_ 级别 \_ 9 \_ 3、D3D \_ 功能 \_ 级别 \_ 10 \_ 0、D3D \_ 功能 \_ 级别 \_ 10 \_ 1 和 D3D \_ 功能 \_ 级别 \_ 11 \_ 0) 。

| 纹理块压缩格式 | DXGI 格式                                                                           | 与 Direct3D 9 对应的格式                               | 每个 4x4 像素块的字节数 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------------------------------------|---------------------------|
| BC1                      | DXGI \_ format \_ BC1 \_ UNORM，dxgi \_ format \_ BC1 \_ UNORM \_ SRGB，dxgi \_ format \_ BC1 无格式 \_ | D3DFMT \_ DXT1，FourCC = "DXT1"                                | 8                         |
| BC2                      | DXGI \_ format \_ BC2 \_ UNORM，dxgi \_ format \_ BC2 \_ UNORM \_ SRGB，dxgi \_ format \_ BC2 无格式 \_ | D3DFMT \_ DXT2 \* ，FourCC = "DXT2"，D3DFMT \_ DXT3，FOURCC = "DXT3" | 16                        |
| BC3                      | DXGI \_ format \_ BC3 \_ UNORM，dxgi \_ format \_ BC3 \_ UNORM \_ SRGB，dxgi \_ format \_ BC3 无格式 \_ | D3DFMT \_ DXT4 \* ，FourCC = "DXT4"，D3DFMT \_ DXT5，FOURCC = "DXT5" | 16                        |

 

\*这些压缩方案 (DXT2 和 DXT4) 不区分 Direct3D 9 预乘 alpha 格式和标准 alpha 格式。 在呈现时，不必须使用可编程的着色器进行此区分。

## <a name="span-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanspan-idbc4_and_bc5_formatsspanbc4-and-bc5-formats"></a><span id="BC4_and_BC5_Formats"></span><span id="bc4_and_bc5_formats"></span><span id="BC4_AND_BC5_FORMATS"></span>BC4 和 BC5 格式


| 纹理块压缩格式 | DXGI 格式                                                                     | 与 Direct3D 9 对应的格式 | 每个 4x4 像素块的字节数 |
|--------------------------|---------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC4                      | DXGI \_ format \_ BC4 \_ UNORM，dxgi \_ FORMAT \_ BC4 \_ SNORM，dxgi \_ format \_ BC4 \_ 无格式 | FourCC="ATI1"                | 8                         |
| BC5                      | DXGI \_ format \_ BC5 \_ UNORM，dxgi \_ FORMAT \_ BC5 \_ SNORM，dxgi \_ format \_ BC5 \_ 无格式 | FourCC="ATI2"                | 16                        |

 

## <a name="span-idbc6h_formatspanspan-idbc6h_formatspanspan-idbc6h_formatspanbc6h-format"></a><span id="BC6H_Format"></span><span id="bc6h_format"></span><span id="BC6H_FORMAT"></span>BC6H 格式


如需了解有关此格式的更多详细信息，请参见 [BC6H 格式](/windows/desktop/direct3d11/bc6h-format)文档。

| 纹理块压缩格式 | DXGI 格式                                                                      | 与 Direct3D 9 对应的格式 | 每个 4x4 像素块的字节数 |
|--------------------------|----------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC6H                     | DXGI \_ format \_ BC6H \_ UF16，dxgi \_ FORMAT \_ BC6H \_ SF16，dxgi \_ format \_ BC6H \_ 无格式 | 不适用                          | 16                        |

 

BC6H 格式可以针对每个 4x4 像素块选择不同的编码模式。 总共有 14 种不同的编码模式可选，在每种模式下最终显示的纹理的视觉效果都略有不同。 选择合适的模式有助于硬件快速解码（根据源内容选择或调整质量级别），但这也极大地增加了搜索空间的复杂性。

## <a name="span-idbc7_formatspanspan-idbc7_formatspanspan-idbc7_formatspanbc7-format"></a><span id="BC7_Format"></span><span id="bc7_format"></span><span id="BC7_FORMAT"></span>BC7 格式


如需了解有关此格式的更多详细信息，请参见 [BC7 格式](/windows/desktop/direct3d11/bc7-format)文档。

| 纹理块压缩格式 | DXGI 格式                                                                           | 与 Direct3D 9 对应的格式 | 每个 4x4 像素块的字节数 |
|--------------------------|---------------------------------------------------------------------------------------|------------------------------|---------------------------|
| BC7                      | DXGI \_ format \_ BC7 \_ UNORM，dxgi \_ format \_ BC7 \_ UNORM \_ SRGB，dxgi \_ format \_ BC7 无格式 \_ | 不适用                          | 16                        |

 

BC7 格式可以针对每个 4x4 像素块选择不同的编码模式。 总共有 8 种不同的编码模式可选，在每种模式下最终显示的纹理的视觉效果都略有不同。 选择合适的模式有助于硬件快速解码（根据源内容选择或调整质量级别），但这也极大地增加了搜索空间的复杂性。

## <a name="span-idrelated-topicsspanrelated-topics"></a><span id="related-topics"></span>相关主题


[附录](appendix.md)

[纹理](/windows/desktop/direct3d11/overviews-direct3d-11-resources-textures)

 

 