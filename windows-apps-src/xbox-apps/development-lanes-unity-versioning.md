---
title: Unity - 对你的 UWP 项目进行版本控制
description: 了解如何使用通用 Windows 平台 (UWP) 为 Xbox 游戏使用版本控制。
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 67c4ec927d83ecba1257eb73fec451e3281333b2
ms.sourcegitcommit: b0a82c2a132212eb5fb72b67f0789cac1014642f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254162"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity：对你的 UWP 项目进行版本控制

仍未使用通用 Windows 平台 (UWP) 为 Xbox 生成 Unity 游戏？  请先参阅[将 Unity 游戏引入 Xbox 上的 UWP](development-lanes-unity.md)。

关于为何要将你生成的 UWP 目录的各个部分添加到版本控制有多种不同的原因，其中一个原因是为了添加依赖项（例如 Xbox Live SDK）。  我们将在此教程中使用此方案作为示例，希望它能帮助你解决你的项目的个别需求。

***免责声明：我们将使用 Git 作为版本控制解决方案。 如果二者不同，则这些概念仍应翻译。** _

若要刷新内存，我们的游戏（ _*_ScrapyardPhoenix_*_）的目录如下所示：

![生成目标文件夹](images/build-destination.png)

这是我们的 UWP 目录的外观：

![UWP 与解决方案](images/uwp-vs-solution.png)

在此目录中，只关心一个文件夹， _*_ScrapyardPhoenix_*_ (将游戏名称插入此处) 文件夹。  在版本控制中，可以忽略其他所有内容。

_*_不熟悉 .gitignore 文件是什么？ 请参阅 [.gitignore](https://git-scm.com/docs/gitignore)。_*_

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep... (this line will be modified and removed further down)
!/UWP/ScrapyardPhoenix/
```

我们将从 **UWP/ScrapyardPhoenix** 文件夹内选择一些不同的文件和文件夹添加到我们的版本控制。  首先让我们详细查看完整内容：

![UWP 生成目录](images/uwp-build-directory.png)  

## <a name="folders"></a>文件夹  

| 文件夹名称 | 设置 | 说明 |
|-------------|---------|-------------|
| `Assets` | **_包括_* _ | 包含 Microsoft Store 映像 |
| `Data` | _*_忽略_*_ | Unity 将项目编译到 (场景、着色器、脚本、Prototyping 等 )  |
| `Dependencies` | _*_包括_*_ | 此文件夹是创建的，用于将所有 UWP 依赖项保存在 (例如，XboxLiveSDK.dll)  |
| `Properties` | _*_包括_*_ | 包含开发人员可以修改的更高级的设置 |
| `Unprocessed` | _*_忽略_*_ | 包含 Unity `.dll` 和 `.pdb` 文件 |

## <a name="files"></a>文件  

| 文件夹名称 | 设置 | 说明 |
|-------------|---------|-------------|
| `App.cs` | _*_包括_*_ | UWP 应用程序的入口点;可以通过其他源文件修改和扩展此文件 |
| `Package.appxmanifest` | _*_包括_*_ | .Msix 或 .appx 包的应用包清单源文件 |
| `project.json` | _*_包括_*_ | 描述你依赖的 NuGet 包 `_.csproj` |
| `ScrapyardPhoenix.csproj` | ***包括** _ | 描述 UWP 生成目标;如果将其他依赖项添加到 UWP 项目中，此 `_.csproj` 文件将包含此信息 |
| `ScrapyardPhoenix.csproj.user` | ***忽略** _ | 此文件包含本地用户信息 |

## <a name="resulting-gitignore"></a>生成的 .gitignore

```console
##################################################################
# The original .gitignore file can be found at
# https://github.com/github/gitignore/blob/master/Unity.gitignore
##################################################################

# standard ignores for a Unity Project
...

# ignore the whole UWP directory
/UWP/_*

# except we want to keep...
!/UWP/ScrapyardPhoenix/Assets/*
!/UWP/ScrapyardPhoenix/Dependencies/*
!/UWP/ScrapyardPhoenix/Properties/*
!/UWP/ScrapyardPhoenix/App.cs
!/UWP/ScrapyardPhoenix/Package.appxmanifest
!/UWP/ScrapyardPhoenix/project.json
!/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj
```

于是，现在你的团队成员将与你生成的 UWP 项目同步。 现在你可以向你的 UWP 项目随意添加其他资源、源和依赖项。

有关对 UWP 文件夹进行版本控制的一些更多示例可以在[这些示例](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview)中找到。

## <a name="adding-dependencies-to-your-uwp-app"></a>向 UWP 应用添加依赖项

通过将依赖项放置在 **Plugins** 文件夹下的 **Unity Assets** 文件夹中将其添加到 DLL 和 WINMD，然后选择它们并在检查器中相应地设置其目标平台设置。

![UWP 解决方案](images/uwp-solution.PNG)

**_ScrapyardPhoenix (通用 Windows)_** 是你要添加对其的引用的项目，例如 XBOX Live SDK。

## <a name="see-also"></a>另请参阅

- [将现有游戏移植到 Xbox](development-lanes-landing.md)
- [Xbox One 上的 UWP](index.md)
