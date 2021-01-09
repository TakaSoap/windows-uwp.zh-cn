---
description: 'MRT.LOG 核心组件的概述，以及它们如何工作以 (项目留尼汪岛加载应用程序资源) '
title: MRT.LOG Core (项目留尼汪岛) 简介
ms.topic: article
ms.date: 12/11/2020
keywords: MRT.LOG，MRTCore，pri，makepri.exe，资源，资源加载
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: ce07491dcab2a11738bd5407e9094d8d780ea219
ms.sourcegitcommit: 30d1a27fd78d198cec5c50af5621f9e65c7b965e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/08/2021
ms.locfileid: "98043056"
---
# <a name="introduction-to-mrt-core-project-reunion"></a>MRT.LOG Core (项目留尼汪岛) 简介

MRT.LOG Core 是新式 Windows [资源管理系统](/windows/uwp/app-resources/resource-management-system) 的简化版本，作为 [项目](../index.md)的一部分进行分发。

MRT.LOG 核心提供生成时和运行时功能。 在生成时间，系统创建与你的应用打包在一起的资源的所有不同变体的索引。 此索引为包资源索引 (PRI)，它也包括在你的应用包中。

## <a name="package-resource-index-pri-file"></a>包资源索引 (PRI) 文件

每个应用包都应包含应用中资源的二进制索引。 此索引在生成时创建，并且包含在一个或多个资源 ( .resw) 文件中。

.Resw 文件包含实际的字符串资源，以及引用包中各种文件的一组索引的文件路径。
包通常包含每个语言的单个 .resw 文件，名为 .resw。 实例化 ResourceManager 时，会自动加载每个包的根处的 .resw 文件。

每个 .resw 文件都包含一个命名的资源集合，称为资源映射。 加载包中的 .resw 文件时，将验证资源映射名称以匹配包标识名称。

.Resw 文件仅包含数据，因此它们不会使用) 格式的可移植可执行文件 (。 它们专门设计为仅用于数据。

## <a name="using-mrt-core-to-access-app-resources"></a>使用 MRT.LOG Core 访问应用资源

### <a name="resource-loader-basic-functionality"></a>资源加载器 (基本功能) 

以编程方式访问应用资源的最简单方法是使用 [windows.applicationmodel.resources.core](/windows/winui/api/microsoft.applicationmodel.resources) 命名空间和 windows.applicationmodel.resources.resourceloader 类。 ResourceLoader 为你提供对资源文件集、引用库或其他包的字符串资源的基本访问权限。

### <a name="resource-manager-advanced-functionality"></a>资源管理器 (高级功能) 

[ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)类提供有关资源的其他信息，例如枚举和检查。 这超出了 **ResourceLoader** 类的提供范围。

[ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) 对象表示单个具体的资源值及其限定符，如适用于英语的字符串“Hello World”，或特定于比例-100 分辨率的作为限定符图像资源的“logo.scale-100.jpg”。

对应用提供的资源存储在分层集合中，你可以使用 [ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) 对象进行访问。 **ResourceManager** 类提供应用使用的各种顶级 **ResourceMap** 实例（对应于应用的各种包）的访问权限。 [MainResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap)值与当前应用程序包的资源映射相对应，并排除任何引用的框架包。 每个 **ResourceMap** 都针对在包清单中指定的包名称进行命名。 **Windows.applicationmodel.resources.core.resourcemap** 中的子树 (参阅 [windows.applicationmodel.resources.core.resourcemap. GetSubtree](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap.getsubtree)) 。 子树通常对应于包含资源的资源文件。

**ResourceManager** 不仅支持访问某个应用的字符串资源，它还维护枚举和检查各种文件资源的能力。 为了避免文件和源自该文件内部的其他资源之间发生冲突，索引的文件路径全部驻留在预留的“文件”**ResourceMap** 子树中。 例如，文件 "\Images\logo.png" 对应于资源名称 "Files/images/logo.png"。

### <a name="resourcecontext"></a>ResourceContext

基于作为资源限定符值集合（语言、比例、对比度等）的特定的 [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext) 选择候选资源。 除非覆盖，默认上下文对每个限定符值使用应用的当前配置。 例如，可以针对比例限定图像等资源，具体因不同的监视器而异，因此不同应用程序视图之间也有差异。 出于此原因，每个应用程序视图都有不同的默认上下文。 每当你检索候选资源时，都应该传递 **ResourceContext** 实例，以获取最适合给定视图的值。

### <a name="important-apis"></a>重要的 API

- [ResourceLoader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader)
- [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)
- [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext)

## <a name="sample"></a>示例

有关演示如何使用 MRT.LOG 核心 API 的示例，请参阅 [Mrt.log core 示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)。
