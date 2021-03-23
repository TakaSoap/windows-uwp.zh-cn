---
ms.openlocfilehash: 62470070b726aa7101b8299296dc1f4ef67f30e4
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804306"
---
当用户单击任意通知 (或通知) 上的按钮时，将发生以下情况 .。。

**如果你的应用当前正在运行**.。。

1. 将在后台线程上调用 **ToastNotificationManagerCompat OnActivated** 事件。

**如果你的应用程序当前已关闭**.。。

1. 将启动你的应用程序的 EXE，并 `ToastNotificationManagerCompat.WasCurrentProcessToastActivated()` 返回 true 以指示进程由于新式激活而启动，并将很快调用事件处理程序。
1. 然后，将在后台线程上调用 **ToastNotificationManagerCompat OnActivated** 事件。