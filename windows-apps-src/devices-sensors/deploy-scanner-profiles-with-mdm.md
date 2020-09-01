---
title: 使用 MDM 部署条形码扫描仪配置文件
description: 可以使用 MDM 服务器部署条形码扫描仪配置文件。
ms.assetid: 99ED3BD8-022C-40C2-9C65-F599186548FE
ms.date: 09/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1bac497ec52dd0897af8c6c606bcdc041007c579
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173271"
---
# <a name="deploy-barcode-scanner-profiles-with-mdm"></a>使用 MDM 部署条形码扫描仪配置文件

**注意**   此功能需要 Windows 10 移动版或更高版本。

可以使用 MDM 服务器部署条形码扫描仪配置文件。 若要部署配置文件，请使用[ENTERPRISEEXTFILESYSTEM CSP](/windows/client-management/mdm/enterpriseextfilessystem-csp)中的*OemProfile*将其放入 \\ 数据 \\ SharedData \\ OEM \\ 公共 \\ 配置文件文件夹。 然后，驱动程序制造商可以使用这些扫描仪配置文件配置不会通过 API 图面公开的设置。

Microsoft 未定义扫描仪配置文件的细节或实现方法。

## <a name="related-topics"></a>相关主题
- [EnterpriseExtFileSystem 云解决方案提供商](/windows/client-management/mdm/enterpriseextfilessystem-csp)
- [条形码扫描仪设备支持](./pos-device-support.md#barcode-scanner)