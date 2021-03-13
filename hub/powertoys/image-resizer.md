---
title: 适用于 Windows 10 的 PowerToys 映像调整程序实用工具
description: 用于大容量图像大小调整的 Windows shell 扩展
ms.date: 12/02/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e417a70e34ae1e5fdba95e2838a6221b3236e1c6
ms.sourcegitcommit: a1b251971f7ac574275d53bbe3e9ef4a3a9dc15c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103417061"
---
# <a name="image-resizer-utility"></a>图像调整程序实用工具

图像调整器是用于大容量图像大小调整的 Windows shell 扩展。 安装 PowerToys 后，在 "文件资源管理器" 中右键单击一个或多个所选图像文件，然后从菜单中选择 " **调整图片大小** "。

![图像调整调整演示](../images/powertoys-resize-images.gif)

## <a name="drag-and-drop"></a>拖放

使用图像调整调整功能，还可以通过 **使用鼠标右键** 拖放所选文件来调整图像的大小。 这样，便可以在另一个文件夹中快速保存已调整大小的图片。

![图像调整尺寸拖放演示](../images/powertoys-resize-drag-drop.gif)

## <a name="settings"></a>设置

在 PowerToys 的 "图像调整" 选项卡中，可以配置以下设置。

![PowerToys 图像调整大小设置菜单](../images/powertoys-imageresize-settings.png)

### <a name="sizes"></a>大小

添加新的预设大小。 每个大小都可以配置为 "填充"、"适应" 或 "拉伸"。 用于调整大小的维度也可以配置为厘米、英寸、百分比和像素。

#### <a name="fill-vs-fit-vs-stretch"></a>填充与调整

- **填充：** 用图像填充整个指定的大小。 按比例缩放图像。 根据需要裁剪映像。
- **适合：** 将整个图像调整为指定大小。 按比例缩放图像。 不裁剪图像。
- **Stretch：** 用图像填充整个指定的大小。 根据需要拉伸图像 disproportionally。 不裁剪图像

可以交换指定大小的宽度和高度，使其与当前图像的纵向/横向) 方向 (。 若要始终使用指定的宽度和高度，请取消选中： **忽略图片的方向**。


### <a name="fallback-encoding"></a>回退编码

如果文件无法以原始格式保存，则使用回退编码器。 例如，Windows 图元文件 ( .wmf) 映像格式具有一个解码器来读取映像，但没有编码器用于写入新映像。 在这种情况下，无法以原始格式保存图像。 图像调整调整可用于指定后备编码器将使用的格式： PNG、JPEG、TIFF、BMP、GIF 或 WMPhoto 设置。 *这不是文件类型转换工具，而只是作为不受支持的文件格式的回退。*

### <a name="file"></a>文件

可以用以下参数修改已调整大小的图像的文件名：

- `%1`：原始文件名
- `%2`：在 PowerToys 映像调整设置中配置的大小名称 () 
- `%3`：所选宽度
- `%4`：已选择高度
- `%5`：实际高度
- `%6`：实际宽度

例如，将文件名格式设置为： `%1 (%2)` 对文件 `example.png` 和选择 " `Small` 文件大小" 设置，将导致文件名 `example (Small).png` 。

如果将文件的格式设置为 `%1_%4` `example.jpg` ，并选择大小设置， `Medium 1366 x 768px` 将导致文件名为： `example_768.jpg` 。

你还可以选择保留调整大小的图像上的原始 *上次修改* 日期。

### <a name="auto-widthheight"></a>自动宽度/高度

您可以将高度或宽度留空。 这将遵循指定的维度，并将另一维度 "锁定" 到与原始图像纵横比成比例的值。

### <a name="sub-directories"></a>子目录

可以指定文件名格式的目录，将调整大小的图像分组到子目录中。 例如，值为会将已 `%2\%1` 调整大小的图像保存到 `Small\Sample.jpg`
