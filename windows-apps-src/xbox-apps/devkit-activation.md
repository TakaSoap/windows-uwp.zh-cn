---
title: Xbox One 开发人员模式激活
description: 如何激活开发人员模式，以便你可以在零售模式和开发人员模式之间切换。
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: d93fef1b06fa52616b7e5eddf7b3ac0530856dc2
ms.sourcegitcommit: a15bc17aa0640722d761d0d33f878cb2a822e8ed
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/03/2020
ms.locfileid: "96577099"
---
# <a name="xbox-one-developer-mode-activation"></a>Xbox One 开发人员模式激活

## <a name="how-developer-mode-works"></a>开发人员模式的工作原理
本文仅适用于 Xbox One 和 Xbox 系列 X |通过零售渠道获得的控制台。 有关通过托管开发计划获取的开发工具包硬件，请参阅本文末尾的说明。

Xbox 零售控制台可以有两种模式：零售模式 (1) 和开发人员模式 (2) 。 在零售模式下，控制台处于正常状态：可以播放游戏并运行通过 Xbox 应用商店获取的应用。 在开发人员模式下，你可以为控制台开发和测试软件，但不能播放零售游戏或运行零售版应用。

可以在任何零售版 Xbox 控制台上启用开发人员模式，这可以通过在 Xbox 应用商店中找到的 "零售到开发工具包转换" 应用。 在零售控制台上启用开发人员模式后，可以在零售 (2a) 和开发人员模式之间来回切换 (2b) 。

> [!NOTE]
> 不要在通过 Xbox 托管程序获取的任何 Xbox 开发硬件上运行此转换应用程序 (例如 ID@Xbox) 或者，在开发游戏时可能会导致错误和延迟。 如果你是托管合作伙伴，可以获取有关激活开发硬件的详细信息。 转到 https://developer.microsoft.com/en-us/games/xbox/docs/gdk/provisioning-role。

<br></br>

![Xbox One 模式](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-console"></a>在零售 Xbox 控制台上激活开发人员模式

1.  启动 Xbox 控制台。

2.  从 Xbox One 商店中搜索和安装 **开发人员模式激活** 应用。

    ![安装“开发人员模式激活”应用](images/devkit-activation-1.png)

3.  从 Microsoft Store 页面启动该应用。

    ![开发人员模式激活应用](images/devkit-activation-2.png)

4.  记下“开发人员模式激活”应用中显示的代码。

    ![激活步骤 5](images/activation-step-5.png)  
    
5.  [在合作伙伴中心注册应用开发人员帐户](https://developer.microsoft.com/store/register)。  这也是发布游戏的第一步。

6.  通过有效的当前合作伙伴中心应用开发者帐户登录到 [合作伙伴中心](https://partner.microsoft.com/dashboard) 。  如果左侧导航窗格中未显示多个选项，或者在 "**概述**" 部分中看不到 "**创建新应用**" 选项，以下步骤和激活链接 _将无法工作_;请确保已完全注册了上一步中的应用开发人员帐户。

7.  请参阅 [partner.microsoft.com/xboxconfig/devices](https://partner.microsoft.com/xboxconfig/devices)。

8.  输入“开发人员模式激活”应用中显示的激活代码。 与你的帐户关联的激活次数有限制。 激活开发人员模式后，合作伙伴中心将指示你已使用与你的帐户关联的某个激活。

    ![激活步骤 8](images/activation-step-8-rs2.png)    
    
9.  单击 " **同意并激活**"。 这将导致页面重新加载，并且你将看到你的设备已填充到表中。 可以在 [Xbox One 开发人员模式激活计划](/legal/windows/agreements/xbox-one-developer-mode-activation)中找到 Xbox One 开发人员模式激活计划协议的条款。

10. 输入激活代码之后，主机将显示激活过程的进度屏幕。  
    
11. 完成激活后，打开 Dev 模式激活应用并单击 " **切换"，然后单击 "重启** " 以转到开发人员模式。 请注意，此操作所需的时间比平常更长。

    ![激活步骤 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>在零售模式和开发人员模式之间切换
在控制台上启用开发人员模式后，使用 " **开发** 版" 主页在零售模式和开发人员模式之间切换。 若要详细了解如何启动和使用开发人员主页，请参阅 [Xbox one 工具简介](introduction-to-xbox-tools.md)。

* 若要切换到零售模式，请打开 **开发人员主页**。 在 **快速操作** 下选择 **退出开发人员模式**。 这将在零售模式下重启主机。    

  ![激活步骤 13](images/activation-step-13-rs4.png)  
  
* 若要切换到开发人员模式，请使用“开发人员模式激活”应用。 打开该应用，然后选择 **切换并重启**。 这将在开发人员模式下重新启动你的主机。  

  ![激活步骤 14](images/activation-step-12.png)  

## <a name="see-also"></a>另请参阅
- [Xbox One 开发人员模式停用](devkit-deactivation.md)
- [Xbox One 上的 UWP](index.md)
