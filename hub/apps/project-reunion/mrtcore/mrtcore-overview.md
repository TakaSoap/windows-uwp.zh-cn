---
description: 'MRT.LOG 核心组件的概述，以及它们如何工作以 (项目留尼汪岛加载应用程序资源) '
title: '管理资源 MRT.LOG Core (项目留尼汪岛) '
ms.topic: article
ms.date: 03/31/2021
keywords: MRT.LOG，MRTCore，pri，makepri.exe，资源，资源加载
ms.author: hickeys
author: hickeys
ms.localizationpriority: medium
ms.openlocfilehash: c2a0f235e6bd0d8b100572bf3a38fb80440ac440
ms.sourcegitcommit: 86630e2163a87f6d6e02db9598a3e43f2d227cb6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106272977"
---
# <a name="manage-resources-with-mrt-core"></a>使用 MRT Core 管理资源 

MRT.LOG Core 是新式 Windows [资源管理系统](/windows/uwp/app-resources/resource-management-system) 的简化版本，作为 [项目](../index.md)的一部分进行分发。

MRT.LOG 核心提供生成时和运行时功能。 在生成时间，系统创建与你的应用打包在一起的资源的所有不同变体的索引。 此索引为包资源索引 (PRI)，它也包括在你的应用包中。

## <a name="package-resource-index-pri-file"></a>包资源索引 (PRI) 文件

每个应用包都应包含应用中资源的二进制索引。 此索引在生成时创建，并且包含在一个或多个 PRI 文件中。 每个 PRI 文件都包含一个命名资源集合，称为资源映射。

PRI 文件包含实际的字符串资源。 嵌入的二进制文件和文件路径资源直接从项目文件进行索引。 包通常包含每种语言的单个 PRI 文件，名为 "**资源"。** 实例化 [ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)对象时，会自动加载每个包的根处的 **资源 pri** 文件。

PRI 文件仅包含数据，因此它们不使用可移植可执行 (PE) 格式。 它们专门设计为仅用于数据。

> [!NOTE]
> 你必须确保已配置这些资源，以便可以在资源 pri 文件中对这些资源进行索引，然后才能使用 MRT.LOG Core 在使用 c #/.NET 5 的 WinUI 3 项目中检索字符串和图像。 否则，MRT.LOG Core 无法检索这些资源。
>
> * 字符串：确保资源文件)  ( 的 " **生成操作** " 属性设置为 " **PRIResource**"。 如果向项目中添加新的 **资源文件 ()** 项，则会自动设置此属性。
> * 映像：确保将图像文件的 " **生成操作** " 属性设置为 " **内容**"。 如果将现有图像添加到项目中名为 " **资产** " 的文件夹中，则会自动设置此属性。

## <a name="access-app-resources-with-mrt-core"></a>使用 MRT.LOG Core 访问应用资源

MRT.LOG Core 提供多种不同方式来访问应用资源。

### <a name="basic-functionality-with-resourceloader"></a>Windows.applicationmodel.resources.resourceloader 的基本功能

以编程方式访问应用资源的最简单方法是使用 [windows.applicationmodel.resources.resourceloader](/windows/winui/api/microsoft.applicationmodel.resources.resourceloader) 类。 **ResourceLoader** 为你提供对资源文件集、引用库或其他包的字符串资源的基本访问权限。

### <a name="advanced-functionality-with-resourcemanager"></a>带有 ResourceManager 的高级功能

[ResourceManager](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager)类提供有关资源的其他信息，例如枚举和检查。 这超出了 **ResourceLoader** 类的提供范围。

[ResourceCandidate](/windows/winui/api/microsoft.applicationmodel.resources.resourcecandidate) 对象表示单个具体的资源值及其限定符，如适用于英语的字符串“Hello World”，或特定于比例-100 分辨率的作为限定符图像资源的“logo.scale-100.jpg”。

对应用提供的资源存储在分层集合中，你可以使用 [ResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap) 对象进行访问。 **ResourceManager** 类提供应用使用的各种顶级 **ResourceMap** 实例（对应于应用的各种包）的访问权限。 [MainResourceMap](/windows/winui/api/microsoft.applicationmodel.resources.resourcemanager.mainresourcemap)值与当前应用程序包的资源映射相对应，并排除任何引用的框架包。 每个 **ResourceMap** 都针对在包清单中指定的包名称进行命名。 **Windows.applicationmodel.resources.core.resourcemap** 中的子树 (参阅 [windows.applicationmodel.resources.core.resourcemap. GetSubtree](/windows/winui/api/microsoft.applicationmodel.resources.resourcemap.getsubtree)) 。 子树通常对应于包含资源的资源文件。

**ResourceManager** 不仅支持访问某个应用的字符串资源，它还维护枚举和检查各种文件资源的能力。 为了避免文件和源自该文件内部的其他资源之间发生冲突，索引的文件路径全部驻留在预留的“文件”**ResourceMap** 子树中。 例如，文件 "\Images\logo.png" 对应于资源名称 "Files/images/logo.png"。

### <a name="qualify-resource-selection-with-resourcecontext"></a>用 ResourceContext 限定资源选择

基于作为资源限定符值集合（语言、比例、对比度等）的特定的 [ResourceContext](/windows/winui/api/microsoft.applicationmodel.resources.resourcecontext) 选择候选资源。 除非覆盖，默认上下文对每个限定符值使用应用的当前配置。 例如，可以针对比例限定图像等资源，具体因不同的监视器而异，因此不同应用程序视图之间也有差异。 出于此原因，每个应用程序视图都有不同的默认上下文。 每当你检索候选资源时，都应该传递 **ResourceContext** 实例，以获取最适合给定视图的值。

## <a name="sample"></a>示例

有关演示如何使用 MRT.LOG 核心 API 的示例，请参阅 [Mrt.log core 示例](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore)。
