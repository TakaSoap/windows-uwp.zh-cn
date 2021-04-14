---
description: 本指南展示了如何开始使用 WinUI 3 UI 创建 .NET 和 C++/Win32 桌面应用。
title: 适用于桌面应用的 WinUI 3 入门
ms.date: 03/19/2021
ms.topic: article
keywords: windows 10, uwp, windows 窗体, wpf, xaml 岛
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 8f8462a3645aff1113918f92bf53adb672ae3a61
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730546"
---
# <a name="get-started-with-winui-3-for-desktop-apps"></a>适用于桌面应用的 WinUI 3 入门

WinUI 3 - Project Reunion 0.5 中包含项目模板，便于你创建具有完全基于 WinUI 的用户界面的托管 C#/.NET 5 和本机 C++/Win32 桌面应用。 使用这些项目模板创建应用时，应用程序的整个用户界面都是使用 WinUI 3 提供的窗口、控件和其他 UI 类型实现的。 有关项目模板的完整列表，请参阅[适用于 WinUI 3 的项目模板](winui-project-templates-in-visual-studio.md#project-templates-for-winui-3)。

WinUI 3 作为 Project Reunion 包的一部分提供。 若要详细了解 Project Reunion，请参阅[使用 Project Reunion 0.5（2021 年 3 月）生成桌面 Windows 应用](../../project-reunion/index.md)。

## <a name="prerequisites"></a>先决条件

若要对本文所述的桌面项目模板使用 WinUI 3，请配置开发计算机，并安装 Project Reunion 0.5 Visual Studio 扩展。 有关详细信息，请参阅[设置开发环境](../../project-reunion/get-started-with-project-reunion.md#set-up-your-development-environment)。

## <a name="create-a-winui-3-desktop-app-for-c-and-net-5"></a>创建适用于 C# 和 .NET 5 的 WinUI 3 桌面应用

1. 在 Visual Studio 2019 中，依次选择“文件” -> “新建” -> “项目”。  

2. 在“项目”下拉筛选器中，分别选择“C#”、“Windows”和“WinUI”。

3. 选择“打包的空白应用(桌面版 WinUI 3)”项目类型，然后单击“下一步”。

    ![“新建项目”向导的屏幕截图，其中突出显示了“打包的空白应用(桌面版 Win UI)”选项。](images/WinUI3-csharp-newproject.png)

4. 输入项目名称，根据需要选择任何其他选项，然后单击“创建”。

5. 在下面的对话框中，将“目标版本”设置为 Windows 10 版本 2004（内部版本 19041），并将“最低版本”设置为 Windows 10 版本 1809（内部版本 17763），然后单击“确定”  。

    ![目标版本和最低版本](images/WinUI3-minversion.png)

6. 此时，Visual Studio 生成两个项目：

    * **项目名称(桌面)** ：此项目包含应用的代码。 App.xaml 文件和 App.xaml.cs 和代码隐藏文件定义了一个 `Application` 类，它表示你的应用实例 。 MainWindow.xaml 文件和 MainWindow.xaml.cs 代码隐藏文件定义了一个 `MainWindow` 类，它表示你的应用显示的主窗口 。 这些类派生自 WinUI 提供的 **Microsoft.UI.Xaml** 命名空间中的类型。

        ![Visual Studio 的屏幕截图，其中显示“解决方案资源管理器”窗格以及 MainWindow.xaml.cs 文件的内容。](images/WinUI-csharp-appproject.png)

    * **项目名称(程序包)** ：这是一个 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，已配置该项目以将应用生成到 [MSIX 包](/windows/msix/overview)中。 这提供了一种新式部署体验、通过包扩展与 Windows 10 功能集成的功能以及更多其他功能。 此项目包含应用的[程序包清单](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，默认情况下它是你的解决方案的启动项目。

        ![Visual Studio 的屏幕截图，其中显示“解决方案资源管理器”窗格以及 Package.appxmanifest 文件的内容。](images/WinUI-csharp-packageproject.png)

7. 若要向应用项目中添加新项，请在 **解决方案资源管理器** 中右键单击“项目名称(桌面)”项目节点，然后选择“添加” -> “新项”。  在“添加新项”对话框中，选择“WinUI”选项卡，选择要添加的项，然后单击“添加”。 有关可用项的更多详细信息，请参阅[适用于 WinUI 3 的项模板](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3)。

    ![“添加新项”对话框的屏幕截图，其中已选中“已安装”>“Visual C# 项”>“WinUI”，并且突出显示了“空白页”选项。](images/winui3-addnewitem.png)

8. 生成并运行解决方案，确认应用运行时不会出错。

## <a name="create-a-winui-3-desktop-app-for-cwin32"></a>为 C++/Win32 创建 WinUI 3 桌面应用

1. 在 Visual Studio 2019 中，依次选择“文件” -> “新建” -> “项目”。  

2. 在“项目”下拉筛选器中，选择“C++”、“Windows”和“WinUI”。

3. 选择“打包的空白应用(桌面版 WinUI 3)”项目类型，然后单击“下一步”。

    ![“新建项目”向导的另一个屏幕截图，其中突出显示了“打包的空白应用(桌面版 WinUI)”选项。](images/WinUI3-newproject-cpp.png)

4. 输入项目名称，根据需要选择任何其他选项，然后单击“创建”。

5. 在下面的对话框中，将“目标版本”设置为 Windows 10 版本 20004（内部版本 19041），并将“最低版本”设置为 Windows 10 版本 1809（内部版本 17763），然后单击“确定”。

    ![目标版本和最低版本](images/WinUI3-minversion.png)

6. 此时，Visual Studio 生成两个项目：

    * **项目名称(桌面)** ：此项目包含应用的代码。 **App.xaml** 和各种 **应用** 代码文件定义一个表示应用实例的 `Application` 类，**MainWindow.xaml** 和各种 **MainWindow** 代码文件定义一个表示应用显示的主窗口的 `MainWindow` 类。 这些类派生自 WinUI 提供的 **Microsoft.UI.Xaml** 命名空间中的类型。

        ![Visual Studio 的屏幕截图，其中显示“解决方案资源管理器”窗格以及 MainWindow.xaml 文件的内容。](images/WinUI-csharp-appproject.png)

    * **项目名称(程序包)** ：这是一个 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，已配置该项目以将应用生成到 [MSIX 包](/windows/msix/overview)中。 这提供了一种新式部署体验、通过包扩展与 Windows 10 功能集成的功能以及更多其他功能。 此项目包含应用的[程序包清单](/uwp/schemas/appxpackage/uapmanifestschema/schema-root)，默认情况下它是你的解决方案的启动项目。

        ![Visual Studio 的另一个屏幕截图，其中显示“解决方案资源管理器”窗格以及 Package.appxmanifest 文件的内容。](images/WinUI-cpp-packageproject.png)

7. 若要向应用项目中添加新项，请在 **解决方案资源管理器** 中右键单击“项目名称(桌面)”项目节点，然后选择“添加” -> “新项”。  在“添加新项”对话框中，选择“WinUI”选项卡，选择要添加的项，然后单击“添加”。 有关可用项的更多详细信息，请参阅[适用于 WinUI 3 的项模板](winui-project-templates-in-visual-studio.md#item-templates-for-winui-3)。

    ![新项](images/winui3-addnewitem-cpp.png)

8. 生成并运行解决方案，确认应用运行时不会出错。

   > [!NOTE]
   > 将仅启动打包的项目，因此请确保将其设置为启动项目。


## <a name="localizing-your-winui-desktop-app"></a>将 WinUI 桌面应用本地化

若要在 WinUI 桌面应用中支持多种语言，并确保将打包的项目适当本地化，请将适当的资源添加到项目中（参阅[应用资源和资源管理系统](/windows/uwp/app-resources/)），并在项目的 `package.appxmanifest` 文件中声明每种受支持的语言。 生成项目时，指定的语言将添加到生成的应用清单 (`AppxManifest.xml`) 中，并将使用相应的资源。

1. 在文本编辑器中打开 .wapproj 的 `package.appxmanifest`，找到以下部分：

    ```xml
    <Resources>
        <Resource Language="x-generate"/>
    </Resources>
    ```

2. 对于你支持的每种语言，请将 `<Resource Language="x-generate">` 替换为 `<Resource />` 元素。 例如，以下标记指定“en-US”和“es-ES”本地化资源可用：

    ```xml
    <Resources>
        <Resource Language="en-US"/>
        <Resource Language="es-ES"/>
    </Resources>
    ```


## <a name="known-issues-and-limitations"></a>已知问题和限制

请参阅 [Windows UI 库 3 - Project Reunion 0.5](index.md) 的[限制和已知问题](index.md#limitations-and-known-issues)部分。

## <a name="related-topics"></a>相关主题

[Windows UI 库 3 - Project Reunion 0.5](index.md)
