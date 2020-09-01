---
title: 将进程外后台任务移植到进程内后台任务
description: 将进程外后台任务移植到前台应用进程内运行的进程内后台任务。
ms.date: 09/19/2018
ms.topic: article
keywords: windows 10，uwp，后台任务，应用服务
ms.assetid: 5327e966-b78d-4859-9b97-5a61c362573e
ms.localizationpriority: medium
ms.openlocfilehash: f500be50c8b57415f5ad2fe62c28cc21f4b57b68
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162651"
---
# <a name="port-an-out-of-process-background-task-to-an-in-process-background-task"></a>将进程外后台任务移植到进程内后台任务

若要将进程外 (OOP) 后台活动移植到进程内活动，最简单的方法是将 [IBackgroundTask](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run?f=255&MSPPError=-2147217396) 方法代码引入应用程序，并从 [OnBackgroundActivated](/uwp/api/windows.ui.xaml.application.onbackgroundactivated)启动该代码。 此处所述的方法不是要从 OOP 后台任务创建填充程序到进程内后台任务;这就是将 (或移植) 的 OOP 版本重写到进程内版本。

如果应用具有多个后台任务，[后台激活示例](https://github.com/Microsoft/Windows-universal-samples/tree/dev/Samples/BackgroundActivation)可显示如何使用 `BackgroundActivatedEventArgs.TaskInstance.Task.Name` 确定启动的任务。

如果当前在后台进程和前台进程之间通信，可删除该状态管理和通信代码。

## <a name="background-tasks-and-trigger-types-that-cannot-be-converted"></a>无法转换的后台任务和触发器类型

* 进程内后台任务不支持激活 VoIP 后台任务。
* 进程内后台任务不支持以下触发器：  [DeviceUseTrigger](/uwp/api/windows.applicationmodel.background.deviceusetrigger?f=255&MSPPError=-2147217396)、 [DeviceServicingTrigger](/uwp/api/windows.applicationmodel.background.deviceservicingtrigger) 和 **IoTStartupTask**