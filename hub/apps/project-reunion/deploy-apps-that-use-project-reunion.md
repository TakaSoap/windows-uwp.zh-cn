---
title: 部署使用项目配置的应用
description: 本文提供了有关部署使用项目使用的应用程序的说明。
ms.topic: article
ms.date: 03/19/2021
keywords: windows win32，windows 应用程序开发，项目留尼汪岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 967b38be2ee1485c28175b86c016e2bca1a8ce5b
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730771"
---
# <a name="deploy-apps-that-use-project-reunion"></a>部署使用项目配置的应用

若要将使用项目留尼汪岛0.5 的应用部署到其他计算机，必须使用 [.msix](/windows/msix)打包应用。 在将来的版本中，项目留尼汪岛将支持部署未打包的应用。 有关我们未来计划的详细信息，请参阅我们的 [路线图](https://github.com/microsoft/ProjectReunion/blob/main/docs/roadmap.md)。

默认情况下，当你使用随 Visual Studio 的项目使用扩展插件一起提供的 [WinUI 项目模板](..\winui\winui3\winui-project-templates-in-visual-studio.md) 来创建项目时，你的项目将包含 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net) ，该项目配置为将应用程序构建到 .msix 包。 有关配置此项目以对应用构建 MSIX 包的详细信息，请参阅[在 Visual Studio 中打包桌面或 UWP 应用](/windows/msix/package/packaging-uwp-apps)。

为应用构建 MSIX 包后，可以通过多种方法将其部署到其他计算机。 有关详细信息，请参阅[管理 MSIX 部署](/windows/msix/desktop/managing-your-msix-deployment-overview)。

> [!NOTE]
> 在生产环境中 (c #/.NET 5 或 c + +/Win32) 中，支持在 .MSIX 打包的桌面应用中使用项目留尼汪岛0.5。 使用项目留尼汪岛0.5 的打包桌面应用可发布到 Microsoft Store。

## <a name="dependencies-on-the-project-reunion-framework-package"></a>项目留尼汪岛 framework 包的依赖关系

当你生成使用项目库的应用时，你的应用将引用通过 *框架包* 分发给最终用户的一组项目库的运行时组件。 使用框架包，打包应用程序可以通过用户设备上的单个共享源来访问项目共享组件，而无需将其捆绑到应用程序包中。 框架包还会将其自己的资源（如 Dll 和 API 定义） (COM 和 Windows 运行时注册) 。 这些资源在应用的上下文中运行，因此它们继承应用的功能和特权，而不会对自己的任何功能或特权进行断言。

项目留尼汪岛框架包是通过 Microsoft Store 部署到最终用户的 .MSIX 包。 除了安全和可靠性修补程序外，还可以通过最新版本轻松快速地更新它。 在计算机上使用 "项目共享" 的所有应用都依赖于框架包的共享实例，如下图所示。

![应用如何访问项目空间框架包的示意图](images/framework.png)

配置依赖项的方式取决于你是在应用中使用的是预发行版还是发布版本的项目。 项目在预发布版本和发布版本中提供，其中包括版本的框架包的预发行版本和发布版本。 应用必须确保它们引用适用于所需功能的正确包。

## <a name="configure-dependencies-on-preview-versions-of-the-framework-package"></a>配置框架包预览版本的依赖关系

项目的预览版本用于调查和反馈新功能。 它们未授权在生产环境中使用，不应发布到 Microsoft Store。

当你在开发计算机上安装 Visual Studio 项目的预览版本，或在开发计算机上安装项目留尼汪岛 NuGet 包时，将在生成期间将框架包的预览版本部署为 NuGet 包依赖项。

## <a name="configure-dependencies-on-release-versions-of-the-framework-package"></a>配置框架包发布版本的依赖关系

支持在生产环境中使用的项目留出版本。

当你在开发计算机上安装项目留尼汪岛扩展或项目环境 NuGet 包的发布版本，并使用提供的 WinUI 3 项目模板之一创建项目时，生成的包清单将包含一个 [c y](/uwp/schemas/appxpackage/uapmanifestschema/element-packagedependency) 元素，该元素指定对框架包的依赖关系。

```xml
<Dependencies>
    <PackageDependency Name="Microsoft.ProjectReunion.0.5" MinVersion="0.52103.9000.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />
</Dependencies>
```

但是，如果您手动生成应用程序包，则必须自行将此 **c y** 元素添加到您的包清单，以声明对项目创建框架包的依赖项。

## <a name="updates-and-versioning-of-the-framework-package"></a>框架包的更新和版本控制

发布新版本的 Project 留尼汪岛 framework 包时，所有应用程序都将更新为新版本，而无需重新分发副本。 在发布时，Windows 将更新到最新版本的框架，应用将在重新启动过程中自动引用最新的框架包版本。 旧的框架包版本将不会从系统中删除，直到系统上的应用程序不再运行或正在使用。

![应用如何获取项目空间框架包更新的示意图](images/framework-update.png)

由于应用程序兼容性对 Microsoft 和依赖于项目进行的应用程序兼容性很重要，因此 Project 留尼汪岛 framework 包遵循 [语义版本控制 2.0.0](https://semver.org/) 规则。 这意味着，在我们发布项目的1.0 版后，Project 留尼汪岛 framework 包将保证次要版本和修补程序版本更改之间的兼容性，并且仅在主要版本更新之间进行重大更改。

## <a name="related-topics"></a>相关主题

- [用项目的留尼汪岛构建桌面 Windows 应用](index.md)
- [项目留尼汪岛入门](get-started-with-project-reunion.md)
