---
title: Docker 入门 - 使用容器进行远程开发
description: Windows 或 WSL 上的 Docker Desktop 完整入门指南。 包括 Microsoft 提供的支持和各种 Azure 服务。
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Microsoft, Windows, Docker, WSL, 远程开发, 容器, Docker Desktop, Windows 与 WSL
ms.date: 09/24/2020
ms.openlocfilehash: 09552a6a2a43e414c143ef0a51b6932fa2aaaf20
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824161"
---
# <a name="overview-of-docker-remote-development-on-windows"></a>Windows 上的 Docker 远程开发概述

在 Docker 平台上使用容器进行远程应用程序开发和部署是一种很常用的解决方案，它具有许多优点。 详细了解 Microsoft 工具和服务提供的各种支持，包括适用于 Linux 的 Windows 子系统 (WSL)、Visual Studio、Visual Studio Code、.NET 和各种 Azure 服务。

## <a name="docker-on-windows-10"></a>Windows 10 上的 Docker

:::row:::
    :::column:::
       [![Docker 文档图标](../../images/docker-docs-icon.png)](https://docs.docker.com/docker-for-windows/install/)<br>
        **[安装适用于 Windows 的 Docker Desktop](https://docs.docker.com/docker-for-windows/install/)**<br>
        查找安装步骤、系统要求、安装程序中包含的内容、卸载方式、稳定版与最新版之间的区别，以及 Windows 容器与 Linux 容器之间的切换方法。
    :::column-end:::
    :::column:::
       [![正在运行 Docker 的屏幕截图](../../images/docker-running-screenshot.png)](https://docs.docker.com/get-started/)<br>
        **[Docker 入门](https://docs.docker.com/get-started/)**<br>
        Docker 指导和设置文档，其中包含有关如何入门的分步说明（包括视频演练）。
    :::column-end:::
    :::column:::
       [![Microsoft Learn Docker 课程屏幕截图](../../images/docker-learn-course.png)](/learn/modules/intro-to-docker-containers/)<br>
        **[MS Learn 课程：Docker 容器简介](/learn/modules/intro-to-docker-containers/)**<br>
        Microsoft Learn 不仅提供有关 Docker 入门和连接 Azure 服务的[多种课程](/learn/browse/?terms=docker)，还提供有关 Docker 容器的免费简介课程。
    :::column-end:::
    :::column:::
       [![Docker Desktop WSL2 菜单的屏幕截图](../../images/docker-wsl2.png)](/windows/wsl/tutorials/wsl-containers)<br>
        **[WSL 2 上的 Docker 远程容器入门](/windows/wsl/tutorials/wsl-containers)**<br>
        了解如何使用 WSL 2（适用于 Linux 的 Windows 子系统，版本 2）设置适用于 Windows 的 Docker Desktop，以便与 Linux 命令行（Ubuntu、Debian 和 SUSE 等）一起使用。
    :::column-end:::
:::row-end:::

## <a name="vs-code-and-docker"></a>VS Code 和 Docker

:::row:::
    :::column:::
       [![VS Code 远程容器图形](../../images/vscode-remote-containers.png)](https://code.visualstudio.com/docs/remote/create-dev-container)<br>
        **[使用 VS Code 创建 Docker 容器](https://code.visualstudio.com/docs/remote/containers-tutorial)**<br>
        根据[远程 - 容器扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)中的说明在容器内设置功能齐全的开发环境，并按照相关教程来设置 [NodeJS 容器](https://code.visualstudio.com/docs/containers/quickstart-node)、[Python 容器](https://code.visualstudio.com/docs/containers/quickstart-python)或 [ASP.NET Core 容器](https://code.visualstudio.com/docs/containers/quickstart-aspnet-core)。
    :::column-end:::
    :::column:::
       [![VSCode 附加 Docker 的屏幕截图](../../images/vscode-attach-docker.png)](https://code.visualstudio.com/docs/remote/attach-container)<br>
        **[将 VS Code 附加到 Docker 容器](https://code.visualstudio.com/docs/remote/attach-container)**<br>
        了解如何将 Visual Studio Code 附加到已在运行的 Docker 容器或 [Kubernetes 群集中的容器](https://code.visualstudio.com/docs/remote/attach-container#_attach-to-a-container-in-a-kubernetes-cluster)。
    :::column-end:::
    :::column:::
       [![VSCode 容器菜单的屏幕截图](../../images/vscode-advanced-docker.png)](https://code.visualstudio.com/docs/remote/containers-advanced)<br>
        **[高级容器配置](https://code.visualstudio.com/docs/remote/containers-advanced)**<br>
        了解有关将 Docker 容器与 Visual Studio Code 一起使用的高级设置方案，或阅读本文，了解如何[检查容器](https://code.visualstudio.com/blogs/2019/10/31/inspecting-containers)以使用 VS Code 进行调试。
    :::column-end:::
    :::column:::
       [![VSCode Docker Desktop 与 WSL 一起使用的屏幕截图](../../images/vscode-docker-wsl.png)](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)<br>
        **[在 WSL 2 中使用远程容器](https://code.visualstudio.com/blogs/2020/07/01/containers-wsl)**<br>
        了解如何将 Docker 容器与 WSL 2（适用于 Linux 的 Windows 子系统，版本 2）一起使用，以及如何使用 VS Code 配置相关设置。 还可以了解[其工作原理](https://code.visualstudio.com/blogs/2020/03/02/docker-in-wsl2#_how-it-works)。
    :::column-end:::
:::row-end:::

## <a name="visual-studio-and-docker"></a>Visual Studio 和 Docker

:::row:::
    :::column:::
       [![Visual Studio 图标](../../images/visualstudio.png)](/visualstudio/containers/overview#docker-support-in-visual-studio-1)<br>
        **[Visual Studio 中的 Docker 支持](/visualstudio/containers/overview#docker-support-in-visual-studio-1)**<br>
        除了了解对容器业务流程的支持外，还将了解 Visual Studio 中可用于 ASP.NET 项目、ASP.NET Core 项目、.NET Core 和 .NET Framework 控制台项目的 Docker 支持。
    :::column-end:::
    :::column:::
       [![Visual Studio Docker 菜单](../../images/visualstudio-docker-menu.png)](/visualstudio/containers/container-tools)<br>
        **[快速入门：Visual Studio 中的 Docker](/visualstudio/containers/container-tools)**<br>
        了解如何构建、调试和运行容器化的 .NET、ASP.NET 和 ASP.NET Core 应用，并使用 Visual Studio 将它们发布到 Azure 容器注册表 (ACR)、Docker Hub、Azure 应用服务或自己的容器注册表。
    :::column-end:::
    :::column:::
       [![VS 教程的屏幕截图](../../images/visualstudio-tutorial.png)](/visualstudio/containers/tutorial-multicontainer)<br>
        **[教程：使用 Docker Compose 创建多容器应用](/visualstudio/containers/tutorial-multicontainer)**<br>
        了解如何在 Visual Studio 中使用容器工具时管理多个容器以及如何在它们之间进行通信。 还可以找到教程的链接，例如如何[将 Docker 用于 React 单页应用](/visualstudio/containers/container-tools-react)。
    :::column-end:::
    :::column:::
       [![VS 容器链接](../../images/visualstudio-container-links.png)](/visualstudio/containers)<br>
        **[Visual Studio 中的容器工具](/visualstudio/containers)**<br>
        查找介绍如何在容器中运行生成工具、[调试 Docker 应用](/visualstudio/containers/edit-and-refresh)、对开发工具进行故障排除、部署 Docker 容器以及将 Kubernetes 与 Visual Studio 桥接的主题。
    :::column-end:::
:::row-end:::

![容器、映像和注册表的基本 Docker 分类信息图](../../images/taxonomy-of-docker-terms-and-concepts.png)

## <a name="net-core-and-docker"></a>.NET Core 和 Docker

:::row:::
    :::column:::
       [![.NET 微服务指南封面](../../images/dotnet-microservice-guide.png)](/dotnet/architecture/microservices/)<br>
        **[.NET 指南：微服务应用和容器](/dotnet/architecture/microservices/)**<br>
        通过容器管理的基于微服务的应用简介指南。
    :::column-end:::
    :::column:::
       [![Docker 信息图](../../images/dotnet-docker-infographic.png)](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)<br>
        **[什么是 Docker？](/dotnet/architecture/microservices/container-docker-introduction/docker-defined)**<br>
        Docker 容器的基本说明（包括[将 Docker 容器与虚拟机进行比较](/dotnet/architecture/microservices/container-docker-introduction/docker-defined#comparing-docker-containers-with-virtual-machines)），以及 [Docker 术语和概念的基本分类](/dotnet/architecture/microservices/container-docker-introduction/docker-containers-images-registries)，这些术语和概念解释了容器、映像和注册表之间的区别。
    :::column-end:::
    :::column:::
       [![Docker 分类信息图](../../images/taxonomy-of-docker-terms-and-concepts.png)](/dotnet/core/docker/build-container?tabs=windows)<br>
        **[教程：容器化 .NET Core 应用](/dotnet/core/docker/build-container?tabs=windows)**<br>
        了解如何使用 Docker 容器化 .NET Core 应用程序，包括创建 Dockerfile、执行基本命令以及清理资源。
    :::column-end:::
    :::column:::
       [![使用 Docker 的内部循环开发工作流信息图](../../images/dotnet-docker-workflow.png)](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)<br>
        **[Docker 应用开发工作流](/dotnet/architecture/microservices/docker-application-development-process/docker-app-development-workflow)**<br>
        描述基于 Docker 容器的应用程序的内部循环开发工作流。
    :::column-end:::
:::row-end:::

## <a name="azure-container-services"></a>Azure 容器服务

:::row:::
    :::column:::
       [![Azure 容器实例的屏幕截图](../../images/azure-container-instances.png)](/azure/container-instances/)<br>
        **[Azure 容器实例](/azure/container-instances/)**<br>
        了解如何在托管的无服务器 Azure 环境中按需运行 Docker 容器，包括如何使用 Docker CLI、ARM、Azure 门户进行部署、创建多容器组、在容器之间共享数据、连接到虚拟网络等。
    :::column-end:::
    :::column:::
       [![Azure 容器注册表的屏幕截图](../../images/azure-container-registry-icon.png)](/azure/container-registry)<br>
        **[Azure 容器注册表](/azure/container-registry)**<br>
        了解如何针对各种容器部署类型在专用注册表中生成、存储和管理容器映像和工件。 为现有的容器开发和部署管道创建 Azure 容器注册表，设置自动化任务，并了解如何管理注册表，包括异地复制和最佳做法。
    :::column-end:::
    :::column:::
       [![Azure Service Fabric 的屏幕截图](../../images/azure-service-fabric.png)](/azure/service-fabric)<br>
        **[Azure Service Fabric](/azure/service-fabric)**<br>
        了解 Azure Service Fabric，它是一个分布式系统平台，用于打包、部署和管理可缩放且可靠的微服务和容器。
    :::column-end:::
    :::column:::
       [![Azure 应用服务的屏幕截图](../../images/azure-app-service.png)](/azure/app-service)<br>
        **[Azure 应用服务](/azure/app-service)**<br>
        了解如何在无需管理基础结构的情况下采用所选编程语言构建和托管 Web 应用、移动后端和 RESTful API。 尝试[参加 MS Learn 上的 Azure 应用服务课程](/learn/modules/deploy-run-container-app-service)，基于 Docker 映像部署 Web 应用并配置持续部署。
    :::column-end:::
:::row-end:::

详细了解[支持容器的 Azure 服务](https://azure.microsoft.com/overview/containers/)。

## <a name="docker-containers-explainer-video"></a>Docker 容器说明视频

> [!VIDEO https://www.youtube.com/embed/0oEsMwSxBsk]

## <a name="kubernetes-and-container-orchestration-explainer-video"></a>Kubernetes 和容器业务流程说明视频

> [!VIDEO https://www.youtube.com/embed/3RTvoI-A7UQ]

## <a name="containers-on-windows"></a>Windows 上的容器

:::row:::
    :::column:::
       [![Windows Server 容器图标](../../images/windows-server-containers.png)](/virtualization/windowscontainers)<br>
        **[Windows 上的容器文档](/virtualization/windowscontainers)**<br>
        将应用及其依赖项打包，并利用操作系统级别虚拟化在单个系统上提供快速且完全隔离的环境。 了解 [Windows 容器](/virtualization/windowscontainers/about)，包括快速入门、部署指南和示例。
    :::column-end:::
    :::column:::
       [![常见问题解答图标](../../images/faq.png)](/virtualization/windowscontainers/about/faq)<br>
        **[关于 Windows 容器的常见问题解答](/virtualization/windowscontainers/about/faq)**<br>
        查找有关容器的常见问题解答。 另请参阅 StackOverflow 中有关“[用于 Windows 的 Docker 与 Windows 上的 Docker 有何区别？](https://stackoverflow.com/questions/38464724/whats-the-difference-between-docker-for-windows-and-docker-on-windows/40320748)”的说明
    :::column-end:::
    :::column:::
       [![Windows 容器图标](../../images/windows-container.png)](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)<br>
        **[设置你的环境](/virtualization/windowscontainers/quick-start/set-up-environment?tabs=Windows-10-Client)**<br>
        了解如何设置 Windows 10 或 Windows Server，以创建、运行和部署容器，包括先决条件、安装 Docker 以及使用 [Windows 容器基础映像](/virtualization/windowscontainers/manage-containers/container-base-images)。
    :::column-end:::
    :::column:::
       [![AKS 图标](../../images/kubernettes.png)](/azure/aks/windows-container-cli)<br>
        **[在 Azure Kubernetes 服务 (AKS) 上创建 Windows Server 容器](/azure/aks/windows-container-cli)**<br>
        了解如何使用 Azure CLI 将 Windows Server 容器中的 ASP.NET 示例应用部署到 AKS 群集。
    :::column-end:::
:::row-end:::
