---
title: Xbox One 上 UWP 应用和游戏的系统资源
description: 了解 Xbox 上运行的 UWP 应用与系统和其他应用共享资源的方式，以及 UWP 应用或游戏的资源要求。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 12e87019-4315-424e-b73c-426d565deef9
ms.localizationpriority: medium
ms.openlocfilehash: dbc6af56867508e3178ddd8b731af49270faf3d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174681"
---
# <a name="system-resources-for-uwp-apps-and-games-on-xbox-one"></a>Xbox One 上 UWP 应用和游戏的系统资源

Xbox One 上运行的 UWP 应用与系统和其他应用共享资源。 Xbox One 上的 UWP 可以使用的资源取决于你提交为应用还是提交为 Xbox Live 创意者计划游戏。

* 在前台运行时的最大可用内存：
    * 应用：1 GB
    * 游戏：5 GB

后台运行的应用最多可使用 128 MB 内存。 背景模式仅适用于并发应用程序，如后台音乐播放器。  游戏会暂停，并在后台终止。


超过限制将导致内存分配故障。 有关监视内存使用情况的详细信息，请参阅 [MemoryManager 类](/uwp/api/windows.system.memorymanager)参考。

> [!NOTE]
> 在 Visual Studio 调试器中运行应用程序或游戏时，这些内存约束不适用。 此限制仅在调试模式下处于未运行状态时才适用。

* CPU
    * 应用：共享 2-4 CPU 内核具体取决于系统上运行的应用和游戏数量。
    * 游戏：4 个独占 CPU 内核，2 个共享 CPU 内核。

* GPU
    * 应用：共享 45% 的 GPU 具体取决于系统上运行的应用和游戏数量。
    * 游戏：对可用 GPU 周期的完全访问权限。

* DirectX 支持
    * 应用：DirectX 11 功能级别 10。
    * 游戏：DirectX 12 以及 DirectX 11 功能级别 10。

* 所有应用和游戏必须面向 x64 体系结构以进行开发或提交到 Xbox 应用商店。  

对于**应用程序开发**，与标准电脑相比，可用资源可能会受到限制，并因系统上运行的应用和游戏数而有所变化。

对于**游戏开发**，Xbox One 与其他游戏主机一样，是一个专业化的硬件，它需要专门的基于硬件的开发工具来访问其全部潜能。 如果正在使用的游戏需要访问 Xbox one 硬件的最大潜能，请考虑向 [ID@Xbox](https://www.xbox.com/Developers/id) 程序注册以获取对 Xbox one 开发工具包的访问权限。

## <a name="see-also"></a>另请参阅
- [Xbox One 上的 UWP](index.md)
- [Xbox Live 创意者计划入门](/gaming/xbox-live/get-started-with-creators/creators-program)
- [Xbox one 上的 DirectX 和 UWP](https://walbourn.github.io/)