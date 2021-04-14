---
title: 创建 WinUI 3 项目
description: Visual Studio 中可用的 WinUI 3 项目和项模板的摘要。
ms.date: 03/19/2021
ms.topic: article
ms.openlocfilehash: 823a8698d2d97207a69b31655437d02b70d05857
ms.sourcegitcommit: 99f7edfd79c44768751aca02dab30a03f41d2aae
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106218212"
---
# <a name="winui-3-project-templates-in-visual-studio"></a>Visual Studio 中的 WinUI 3 项目模板

安装 Project Reunion 0.5 VSIX 包后，即可在 Visual Studio 中使用其中一个 WinUI 项目模板创建一个 WinUI 3 应用。 若要在“创建新项目”对话框中访问 WinUI 项目模板，请将语言筛选为“C++”或“C#”，将平台筛选为“Windows”，将项目类型筛选为“WinUI”    。 或者可以搜索“WinUI”并选择一个可用的 C# 或 C++ 模板。

![WinUI 项目模板](images/winui3-csharp-newproject.png)

> [!NOTE] 
> 请参阅最新的[发行说明集](../index.md)，获取有关设置开发环境的指导，了解已知问题等。

Project Reunion 0.5 发行版有两个可用的 Visual Studio 扩展：Project Reunion VSIX 和 Project Reunion 预览 VSIX 。 Project Reunion VSIX 包含可用于生成 MSIX 打包桌面生产应用的项目模板。 Project Reunion 预览 VSIX 包含可用于生成 MSIX 打包桌面应用或 UWP 应用的实验性项目模板。 

## <a name="project-templates-for-winui-3"></a>适用于 WinUI 3 的项目模板

可以使用这些 WinUI 项目模板来创建应用。

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 打包的空白应用（桌面版 WinUI 3） | C# 和 C++ | 使用基于 WinUI 的用户界面创建桌面 NET 5 (C#) 或本机 Win32 (C++) 应用。 生成的项目包括一个基本窗口，该窗口派生自 WinUI 库中的 Microsoft.UI.Xaml.Window 类，可用于生成 UI。 有关此项目类型的详细信息，请参阅[适用于桌面应用的 WinUI 3 入门](get-started-winui3-for-desktop.md)。<p></p>此解决方案还包括一个 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，该项目经配置后可将应用生成为 [MSIX 包](/windows/msix/overview)。 这提供了一种新式部署体验，能够通过包扩展与 Windows 10 功能集成以及更多其他功能。  |
| 实验性空白应用（UWP 版 WinUI 3） | C# 和 C++ | 创建一个具有基于 WinUI 的用户界面的 UWP 应用。 生成的项目包括一个基础页面，该页面派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Page 类，可用于生成 UI。 有关此项目类型的详细信息，请参阅[适用于 UWP 应用的 WinUI 3 入门](get-started-winui3-for-uwp.md)。 |

可以使用这些 WinUI 项目模板来构建可由基于 WinUI 的应用加载和使用的组件。

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 类库（桌面版 WinUI 3） | 仅限 C# | 使用 C# 创建一个 .NET 5 托管类库 (DLL)，使其可由具有基于 WinUI 的用户界面的其他 .NET 5 桌面应用使用。  |
| 实验性类库（UWP 版 WinUI 3）  | 仅限 C# | 使用 C# 创建一个托管类库 (DLL)，使其可由具有基于 WinUI 的用户界面的其他 UWP 应用使用。 |
| Windows 运行时组件 (WinUI 3) | C++ | 创建一个用 C++/WinRT 编写的 [Windows 运行时组件](/windows/uwp/winrt-components/)，任何具有基于 WinUI 的用户界面的 UWP 或桌面应用都可以使用该组件，而无论该应用使用何种编程语言编写。 |
| 实验性 Windows 运行时组件（UWP 版 WinUI 3） | C# | 创建一个用 C# 编写的 [Windows 运行时组件](/windows/uwp/winrt-components/)，使其可由任何具有基于 WinUI 的用户界面的 UWP 应用使用，而不管这些应用使用哪种编程语言编写。 |

## <a name="item-templates-for-winui-3"></a>适用于 WinUI 3 的项模板

下列项模板适合在 WinUI 项目中使用。 若要访问这些 WinUI 项模板，请在“解决方案资源管理器”中右键单击项目节点，选择“添加” -> “新建项”，然后在“添加新项”对话框中单击“WinUI”    。

![WinUI 项模板](images/winui3-addnewitem.png)

| 模板 | Language | 说明 |
|----------|----------|-------------|
| 空白页 (WinUI 3) | C# 和 C++ | 添加一个 XAML 文件和定义了新页面的代码文件，该页面派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Page 类。 |
| 空白窗口（桌面版 WinUI 3） | C# 和 C++ | 添加一个 XAML 文件和定义了新窗口的代码文件，该窗口派生自 WinUI 库中的 Microsoft.UI.Xaml.Window 类。 |
| 自定义控件 (WinUI 3) | C# 和 C++ | 添加用于创建具有默认样式的模板化控件的代码文件。 该模板化控件派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.Control 类。<p></p>有关如何使用该项目模板的演练，请参阅[使用 C++/WinRT 将 UWP 和 WinUI 3 应用的 XAML 控件模板化](xaml-templated-controls-cppwinrt-winui-3.md)和[使用 C# 将 UWP 和 WinUI 3 应用的 XAML 控件模板化](xaml-templated-controls-csharp-winui-3.md)。 有关模板化控件的详细信息，请参阅[自定义 XAML 控件](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
| 资源字典 (WinUI 3) | C# 和 C++ | 添加 XAML 资源的空键控集合。 有关详细信息，请参阅 [ResourceDictionary 和 XAML 资源参考](/windows/uwp/design/controls-and-patterns/resourcedictionary-and-xaml-resource-references)。 |
| 资源文件 (WinUI 3) | C# 和 C++ | 添加用于存储应用的字符串和条件资源的文件。 可以借助此项对应用程序进行本地化。 有关详细信息，请参阅[对 UI 和应用包清单中的字符串进行本地化](/windows/uwp/app-resources/localize-strings-ui-manifest)。 |
| 用户控件 (WinUI 3) | C# 和 C++ | 添加 XAML 文件和用于创建用户控件的代码文件，该用户控件派生自 WinUI 库中的 Microsoft.UI.Xaml.Controls.UserControl 类。 通常，用户控件封装相关的现有控件并提供其自己的逻辑。<p></p>有关用户控件的详细信息，请参阅[自定义 XAML 控件](/archive/msdn-magazine/2019/may/xaml-custom-xaml-controls)。 |
