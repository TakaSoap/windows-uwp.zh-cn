---
title: 安装 PowerToys
description: 安装 PowerToys，这是一组用于自定义 Windows 10 的实用程序，使用可执行文件或程序包管理器 (WinGet、Chocolatey、优惠) 。
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: 2e6a585a6acd6bc9ec209b07e101daf8d844be34
ms.sourcegitcommit: 77af97719a439f5e73a6109b42fd3110bcb2843b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107218119"
---
# <a name="install-powertoys"></a>安装 PowerToys

> [!WARNING]
> PowerToys v 0.37 及更高版本将需要 Windows 10 v1903 或更高版本。 版本为的 v1 设置（支持旧版本的 Windows 版本）将在 v 0.37 中删除。

建议使用下面链接的 Windows 可执行文件按钮安装 PowerToys，但如果更喜欢使用程序包管理器，也会列出其他安装方法。

## <a name="install-with-windows-executable-file"></a>通过 Windows 可执行文件安装

> [!div class="nextstepaction"]
> [安装 PowerToys](https://aka.ms/installpowertoys)

使用 Windows 可执行文件安装 PowerToys：

1. 请访问 [Microsoft PowerToys GitHub 发行版页面](https://github.com/microsoft/PowerToys/releases/)。
2. 浏览可用 PowerToys 的稳定和实验版本的列表。
3. 选择 " **资产** " 下拉菜单以显示发布的文件。
4. 选择 `PowerToysSetup-0.##.#-x64.exe` 要下载 PowerToys 可执行安装程序的文件。
5. 下载后，打开可执行文件并按照安装提示进行操作。

## <a name="requirements"></a>要求

- Windows 10 1803 (版本 17134) 或更高版本。
- [.Net Core 3.1 桌面运行时](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer)。 PowerToys 安装程序将处理此要求。
- 当前支持 x64 体系结构。 ARM 和 x86 支持将在以后推出。

若要确保计算机满足这些要求，请通过选择 " **⊞ Win** *(windows 键")* R 来检查 Windows 10 版本和内部版本号，  +  然后键入 " **winver**"，然后选择 **"确定"**。 （或者在 Windows 命令提示符下输入 `ver` 命令）。 可以在 "**设置**" 菜单中 [更新到最新的 Windows 版本](ms-settings:windowsupdate)。

## <a name="alternative-install-methods"></a>备用安装方法

<!--  - **[Windows executable .exe file](#install-with-windows-executable-file)** *(Recommended)* -->
- [Windows 程序包管理器](#install-with-windows-package-manager-preview) *(预览)*
- *(不正式支持*[社区驱动的安装工具](#community-driven-install-tools)) 

## <a name="install-with-windows-package-manager-preview"></a>随 Windows 程序包管理器一起安装 (预览) 

若要使用 Windows 包管理器安装 PowerToys (WinGet) 预览版：

1. 从 [Windows 包管理器](https://github.com/microsoft/winget-cli/releases)下载 PowerToys。
2. 在命令行中运行以下命令：

```powershell
WinGet install powertoys
```

## <a name="community-driven-install-tools"></a>社区驱动的安装工具

这些社区驱动的备用安装方法不支持官方，PowerToys 团队不会更新或管理这些包。

### <a name="install-with-chocolatey"></a>通过 Chocolatey 安装

若要使用 [Chocolatey](https://chocolatey.org/)安装 PowerToys，请在命令行/PowerShell 中运行以下命令：

```powershell
choco install powertoys
```

若要升级 PowerToys，请运行：

```powershell
choco upgrade powertoys
```

如果在安装/升级时遇到问题，请访问 [Chocolatey.org 上的 PowerToys 包](https://chocolatey.org/packages/powertoys) ，并按照 [Chocolatey 会审过程](https://chocolatey.org/docs/package-triage-process)进行操作。

### <a name="install-with-scoop"></a>通过优惠安装

若要使用 [优惠](https://scoop.sh/)安装 PowerToys，请在命令行/PowerShell 中运行以下命令：

```powershell
scoop bucket add extras
scoop install powertoys
```

若要更新 PowerToys，请从命令行/PowerShell 运行以下命令：

```powershell
scoop update powertoys
```

如果在安装/更新时遇到问题，请在 [GitHub 上的优惠](https://github.com/lukesampson/scoop/issues)存储库中创建问题。

## <a name="post-install"></a>安装后

成功安装 PowerToys 后，将显示一个概述窗口，其中包含有关每个可用实用工具的介绍性指南。
