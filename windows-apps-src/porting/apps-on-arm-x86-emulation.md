---
title: x86 和 ARM32 模拟如何在 ARM 上运行
description: 了解 x86 应用的仿真如何使现有 Win32 应用的丰富生态系统在 ARM 设备上可用。
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, 始终连接, ARM 上的 x86 模拟
ms.localizationpriority: medium
ms.openlocfilehash: 61d994a4a022da671b4141ded80c6235ae38bae6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162321"
---
# <a name="how-x86-emulation-works-on-arm"></a>x86 仿真在 ARM 上的工作原理
x86 应用的模拟使得在 ARM 中可以使用丰富的 Win32 应用生态系统。 这使得用户无需对应用进行任何修改，便可获得运行现有 x86 win32 应用的神奇体验。 该应用甚至不知道它在基于 ARM 的 Windows 电脑上运行，除非它调用特定 API ([IsWoW64Process2](/windows/desktop/api/wow64apiset/nf-wow64apiset-iswow64process2))。

Windows 10 的 [WOW64](/windows/desktop/WinProg64/running-32-bit-applications) 层允许 x86 代码在 ARM64 版本的 Windows 10 上运行。 x86 模拟的工作原理是将 x86 指令块编译为 ARM64 指令，并通过优化来提高性能。 服务会缓存这些已转换的代码块，从而减少指令转换开销，并可在代码再次运行时实现优化。 将为每个模块生成缓存，以便于其他应用在初次启动时使用这些缓存。 

有关这些技术的更多详细信息，请参阅[基于 ARM 的 Windows 10](https://channel9.msdn.com/Events/Build/2017/P4171) Channel9 视频。