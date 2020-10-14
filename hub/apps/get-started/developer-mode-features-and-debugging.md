---
title: 开发人员模式功能和调试
description: 了解有关 Windows 10 中开发人员模式功能的详细信息，以及信息安装错误。
keywords: 入门 开发人员许可证 Visual Studio，开发人员许可证 启用设备
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a8e42b9f35866f07a07122043ced803045bf99d6
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011847"
---
# <a name="developer-mode-features-and-debugging"></a>开发人员模式功能和调试

如果你只是对在应用上安装开发人员模式的基础知识感兴趣，只需按照 [启用设备以便开发](enable-your-device-for-development.md) 入门中概述的说明进行操作。 本文介绍开发人员模式的高级功能、以前版本的 Windows 10 上的开发人员模式以及开发人员模式安装中的调试失败。

## <a name="additional-developer-mode-features"></a>其他开发人员模式功能

对于每个设备系列，可能会提供其他开发人员功能。 仅当在设备上启用了开发人员模式时这些功能才可用，并且可能会因操作系统版本的不同而有所不同。

此图显示了 Windows 10 的开发人员功能：

![开发人员模式选项](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>设备门户

若要了解有关设备门户的详细信息，请参阅 [Windows 设备门户概述](/windows/uwp/debug-test-perf/device-portal.md)。

有关特定于设备的设置说明，请参阅：
- [适用于台式机的设备门户](/windows/uwp/debug-test-perf/device-portal-desktop.md)
- [适用于 HoloLens 的设备门户](/windows/mixed-reality/using-the-windows-device-portal)
- [适用于 IoT 的设备门户](/windows/iot-core/manage-your-device/DevicePortal)
- [适用于移动设备的设备门户](/windows/uwp/debug-test-perf/device-portal-mobile.md)
- [适用于 Xbox 的设备门户](/windows/xbox-apps/device-portal-xbox.md)

如果在启用开发人员模式或设备门户时遇到问题，请参阅[已知问题](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)论坛以查找这些问题的解决方法，或者访问[无法安装开发人员模式程序包](#failure-to-install-developer-mode-package)以获取更多详细信息，并了解可以使用哪些 WSUS KB 来解除阻止开发人员模式程序包。

### <a name="sideload-apps"></a>旁加载应用

> [!IMPORTANT]
> 在最新的 Windows 10 更新中，此设置将不可见，因为默认情况下会启用旁加载。 如果你使用的是以前版本的 Windows 10，则默认设置将仅允许你从 Microsoft Store 运行应用，并且必须启用旁加载才能安装非 Microsoft 源中的应用。

旁加载应用设置通常由需要在未通过 Microsoft Store 认证的托管设备上安装自定义应用的公司或学校使用。 在此情况下，组织通常会强制执行禁用“UWP 应用”设置的策略，如之前的设置页图像中所示。 组织还会提供旁加载应用所需的证书和安装位置。 有关详细信息，请参阅 TechNet 文章：[Windows 10 中的旁加载](/windows/deploy/sideload-apps-in-windows-10)和 [Microsoft Intune 基础知识](/mem/intune/fundamentals/)。

设备系列特定信息

-   对于桌面设备系列：对于桌面设备系列：你可以通过运行使用程序包（“Add-AppDevPackage.ps1”）创建的 Windows PowerShell 脚本，来安装应用包 (.appx) 和运行应用所需的任何证书。 有关详细信息，请参阅[打包 UWP 应用](/windows/msix/package/packaging-uwp-apps)。

-   对于移动设备系列：对于移动设备系列：如果已安装了必需的证书，则可以点击文件以安装任何通过电子邮件收到的或 SD 卡上的 .appx。


**旁加载应用**是比开发人员模式更安全的选项，因为你无法在缺少可信任证书的设备上安装应用。

> [!NOTE]
> 如果旁加载应用，你仍然应该仅从受信任的源安装应用。 安装未经 Microsoft Store 认证的旁加载应用时，即表明你同意已获取旁加载应用所需的所有权限，并且你对任何由安装和运行应用引发的损害负全责。 请参阅此[隐私声明](https://privacy.microsoft.com/privacystatement)的“ Windows &gt; Microsoft Store”部分。


### <a name="ssh"></a>SSH

在设备上启用设备发现时，会启用 SSH 服务。  当设备成为 UWP 应用程序的远程部署目标时，会使用该服务。   服务名称为“SSH Server Broker”和“SSH Server Proxy”。

> [!NOTE]
> 这不是 Microsoft 的 OpenSSH 实现，可以在 [GitHub](https://github.com/PowerShell/Win32-OpenSSH) 上找到该实现。  

为了充分利用 SSH 服务，可以启用设备发现来支持固定配对。 如果想要运行另一个 SSH 服务，可以在其他端口上设置该服务或关闭开发人员模式 SSH 服务。 若要关闭 SSH 服务，请关闭设备发现。  

SSH 登录通过“DevToolsUser”帐户完成，其接受使用密码进行身份验证。  此密码是在按设备发现“配对”按钮后显示在设备上的 PIN，且仅在 PIN 显示时有效。  SFTP 子系统也将启用，用于手动管理从 Visual Studio 安装松散文件部署的 DevelopmentFiles 文件夹。

#### <a name="caveats-for-ssh-usage"></a>SSH 使用的注意事项
在 Windows 中使用的现有 SSH 服务器尚不兼容协议，因此，使用 SFTP 或 SSH 客户端可能需要特殊配置。  特别是，SFTP 子系统在版本 3 或更低版本上运行，因此，任何连接的客户端均应配置为需要旧服务器。  较旧设备上的 SSH 服务器使用 `ssh-dss` 进行公钥身份验证，其已弃用 OpenSSH。  若要连接到此类设备，SSH 客户端必须手动配置为接受 `ssh-dss`。  

### <a name="device-discovery"></a>设备发现

启用设备发现时，将允许你的设备通过 mDNS 对于网络上的其他设备可见。  使用此功能还可以获取用来配对此设备的 SSH PIN，方法是按启用设备发现后出现的“配对”按钮。  该 PIN 提示必须在屏幕上显示，以完成针对设备的首次 Visual Studio 部署。  

![固定配对](images/devmode-pc-pinpair.PNG)

仅当想要使设备成为部署目标时，才应该启用设备发现。 例如，如果使用 Device Portal 将应用部署到手机进行测试，只需要在手机上启用设备发现，并不需要在开发电脑上启用。

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>适用于 Windows 资源管理器、远程桌面和 PowerShell 的优化（仅限桌面设备）

 在桌面设备系列上，**面向开发人员**设置页提供可用于针对开发任务对电脑进行优化的设置的快捷方式。 对于每个设置，可以选中相应复选框，然后单击“应用”或单击“显示设置”链接打开该选项的设置页 。


## <a name="notes"></a>注释
在早期版本的 Windows 10 Mobile 中，“开发人员设置”菜单中有“故障转储”选项。  这已经移到[设备门户](/windows/uwp/debug-test-perf/device-portal.md)，以便可以远程使用而不只是通过 USB。  

有多个工具可用来将应用从 Windows 10 电脑部署到 Windows 10 设备。 两台设备均必须通过有线或无线的连接方式连接到网络的同一子网，或者它们必须通过 USB 进行连接。 所列的两种方法仅会安装应用包 (.appx/.appxbundle)，不安装证书。

-   使用 Windows 10 应用程序部署 (WinAppDeployCmd) 工具。 了解有关 [WinAppDeployCmd 工具](/previous-versions/windows/apps/mt203806(v=vs.140))的详细信息。
-   可以使用[设备门户](/windows/uwp/debug-test-perf/device-portal.md)从浏览器部署到运行 Windows 10 版本 1511 或更高版本的移动设备。 在 Device Portal 中使用 **[应用](/windows/uwp/debug-test-perf/device-portal.md#apps-manager)** 页上传应用包 (.appx) 并在设备上安装它。

## <a name="failure-to-install-developer-mode-package"></a>无法安装开发人员模式程序包。
有时，由于网络或管理问题，开发人员模式无法正确安装。 开发人员模式程序包是**远程**部署到此电脑的必需条件 - 使用来自浏览器的设备门户或设备发现启用 SSH - 但不用于本地部署。  即使遇到这些问题，仍然可以使用 Visual Studio 本地部署应用，或者从本设备向其他设备部署。

请参阅[已知问题](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22)论坛，查找这些问题的解决方法以及其他内容。

> [!NOTE]
> 如果在开发人员模式下无法正常安装，我们建议提出反馈请求。 在“反馈中心”应用中选择“添加新的反馈”，然后选择“开发人员平台”类别和“开发人员模式”子类别   。 提交反馈有助于 Microsoft 解决你所遇到的问题。

### <a name="failed-to-locate-the-package"></a>无法找到该程序包

“无法在 Windows 更新中找到开发人员模式程序包。 错误代码 0x80004005 了解详细信息”   

发生此错误可能是由于网络连接问题、企业设置，或者程序包可能丢失。

若要解决此问题：

1. 确保你的计算机连接到 Internet。
2. 如果你位于加入域的计算机上，请与网络管理员联系。 默认情况下，WSUS 中阻止了开发人员模式程序包，如所有按需功能。
2.1. 为了在当前和之前的版本中解除阻止开发人员模式程序包，应该允许在 WSUS 中使用以下 KB：4016509、3180030、3197985  
3. 在“设置”>“更新和安全”>“Windows 更新”中检查 Windows 更新。
4. 在“设置”>“系统”>“应用和功能”>“管理可选功能”>“添加功能”中验证 Windows 开发人员模式是否存在。 如果缺少，Windows 无法为计算机找到正确的程序包。

在执行上述任意步骤后，禁用并随后重新启用“开发人员模式”以验证是否解决该问题。


### <a name="failed-to-install-the-package"></a>无法安装程序包

“开发人员模式程序包无法安装。 错误代码 0x80004005 了解详细信息”

发生此错误可能是由于 Windows 的内部版本和开发人员模式程序包之间不兼容。

若要解决此问题：

1. 在“设置”>“更新和安全”>“Windows 更新”中检查 Windows 更新。
2. 重启计算机以确保所有更新都已应用。


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>使用组策略或注册表项启用设备

对于大多数开发人员而言，希望使用“设置”应用启用设备进行调试。 在诸如自动执行测试等方案中，可以使用其他方法启用 Windows 10 桌面设备进行开发。  请注意，这些步骤不会启用 SSH 服务器或允许面向设备进行远程部署和调试。

可以使用 gpedit.msc 设置组策略来启用设备，除非你拥有 Windows 10 家庭版。 如果你有 Windows 10 家庭版，则需要使用 regedit 或 PowerShell 命令直接设置注册表项以启用设备。

**使用 gpedit 启用设备**

1.  运行 **Gpedit.msc**。
2.  转到“本地计算机策略”&gt;“计算机配置”&gt;“管理模板”&gt;“Windows 组件”&gt;“应用包部署”
3.  若要启用旁加载，请编辑策略以启用以下项：

    -   **允许安装所有受信任的应用程序**

    或者

    若要启用开发人员模式，请编辑策略以启用以下两项：

    -   **允许安装所有受信任的应用程序**
    -   **允许开发 UWP 应用并从集成开发环境 (IDE) 安装这些应用**

4.  重新启动计算机。

**使用 regedit 启用设备**

1.  运行 **regedit**。
2.  若要启用旁加载，请将此 DWORD 的值设置为 1：

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowAllTrustedApps`

    或者

    若要启用开发人员模式，请将此 DWORD 的值设置为 1：

    -   `HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock\AllowDevelopmentWithoutDevLicense`

**使用 PowerShell 启用设备**

1.  使用管理员权限运行 PowerShell。
2.  若要启用旁加载，请运行此命令：

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowAllTrustedApps" /d "1"
    ```

    或者

    若要启用开发人员模式，请运行此命令：

    ```powershell
    PS C:\WINDOWS\system32> reg add "HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\AppModelUnlock" /t REG_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"
    ```

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>将设备从 Windows 8.1 升级到 Windows 10

当你在 Windows 8.1 设备上创建或旁加载应用时，必须安装开发人员许可证。 如果你将设备从 Windows 8.1 升级到 Windows 10，将保留此信息。 运行以下命令以从已升级的 Windows 10 设备中删除此信息。 如果你直接从 Windows 8.1 升级到版本为 1511 或更高版本的 Windows 10，则无需执行此步骤。

**注销开发人员许可证**

1.  使用管理员权限运行 PowerShell。
2.  运行此命令：`unregister-windowsdeveloperlicense`。

在这之后你需要启用设备用于开发（如本题中所述），以便可以继续在此设备上进行开发。 如果你不执行此操作，则可能在调试应用或为其创建程序包时遇到错误。 以下是此错误的示例：

错误:DEP0700:应用注册失败。