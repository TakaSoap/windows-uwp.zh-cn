---
ms.openlocfilehash: 9508a15492b2b2d6a27d042ddea323f142eb8103
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "104804301"
---
> [!IMPORTANT]
> 必须按 **OnLaunched** 代码那样初始化框架和激活窗口。 **如果用户单击你的 toast ，则不会调用 OnLaunched**，即使你的应用已关闭并是首次启动也是如此。 通常建议将 **OnLaunched** 和 **OnActivated** 合并到你自己的 `OnLaunchedOrActivated` 方法中，因为二者中均需执行相同的初始化。