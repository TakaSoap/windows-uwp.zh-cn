---
description: 本文演示了使用包扩展将打包的桌面应用与文件资源管理器集成的不同方法。
title: 将打包桌面应用与文件资源管理器集成
ms.date: 02/08/2021
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: e3e7aaffc86a152530c933291321c30c2e7c507d
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/12/2021
ms.locfileid: "100335188"
---
# <a name="integrate-a-packaged-desktop-app-with-file-explorer"></a>将打包桌面应用与文件资源管理器集成

某些 Windows 应用定义文件资源管理器扩展，这些扩展可添加上下文菜单项，使客户能够执行与应用相关的选项。 旧版 Windows 应用部署方法（如 MSI 和 ClickOnce）通过注册表定义文件资源管理器扩展。 注册表具有一系列控制文件资源管理器扩展和其他类型的 Shell 扩展的配置单元。 这些安装程序通常会创建一系列注册表项，以配置要包含在上下文菜单中的各个项。

如果你使用 [MSIX](/windows/msix/) 打包 Windows 应用，注册表将被虚拟化，因此你的应用无法通过注册表注册文件资源管理器扩展。 而你必须通过在程序包清单中定义的包扩展来定义文件资源管理器扩展。 本文介绍执行此操作的几种方法。

可在 [GitHub](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample) 上找到本文中使用的完整示例代码。

## <a name="add-a-context-menu-entry-that-supports-startup-parameters"></a>添加支持启动参数的上下文菜单项

与文件资源管理器集成的一种最简单的方法是定义一个包扩展，当用户在文件资源管理器中右键单击特定文件类型时，该包扩展会将你的应用添加到上下文菜单中的可用应用列表中。 如果用户打开你的应用程序，你的扩展可将参数传递给你的应用。

此场景具有几个限制：

* 它仅与[文件类型关联功能](/windows/uwp/launch-resume/handle-file-activation)结合使用。 你只能在上下文菜单中为与主应用相关联的文件类型显示附加选项（例如，你的应用支持通过在文件资源管理器中双击文件以将其打开）。
* 只有在将应用设置为该文件类型的默认设置时，上下文菜单中的选项才会显示。
* 唯一支持的操作是启动应用的主要可执行文件（即连接到“开始”菜单项的相同可执行文件）。 但是，每个操作都可指定不同的参数，当应用开始理解哪个操作触发了执行并执行不同的任务时，可以使用这些参数。

尽管有这些限制，但此方法对许多场景而言已经足够。 例如，如果你要生成一个图像编辑器，则可以在上下文菜单中轻松地添加一项以调整图像大小，这将通过一个向导直接启动该图像编辑器以开始大小调整的过程。

### <a name="implement-the-context-menu-entry"></a>实现上下文菜单项

若要支持此场景，可向程序包清单添加类别为 `windows.fileTypeAssociation` 的 [Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-extension) 元素。 此元素必须添加为 [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素下 [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素的子元素。

以下示例演示了一个应用的注册，该应用为扩展是 `.foo` 的文件启用上下文菜单。 此示例指定了 `.foo` 扩展，因为这是一个虚设扩展，通常不会注册到任何给定计算机上的其他应用。 如果需要管理一个可能已被采用的文件类型（如 .txt 或 .jpg），请记住，在将应用设置为该文件类型的默认设置之前，你无法看到该选项。 此示例摘自 GitHub 上相关示例中的 [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) 文件。

```xml
<Extensions>
  <uap3:Extension Category="windows.fileTypeAssociation">
    <uap3:FileTypeAssociation Name="foo" Parameters="&quot;%1&quot;">
      <uap:SupportedFileTypes>
        <uap:FileType>.foo</uap:FileType>
      </uap:SupportedFileTypes>
      <uap2:SupportedVerbs>
        <uap3:Verb Id="Resize" Parameters="&quot;%1&quot; /p">Resize file</uap3:Verb>
      </uap2:SupportedVerbs>
    </uap3:FileTypeAssociation>
  </uap3:Extension>
</Extensions>
```

此示例假设在清单的根 `<Package>` 元素中声明了以下命名空间和别名。

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap uap2 uap3 rescap">
  ...
</Package>
```

[FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 元素将你的应用与你想要支持的文件类型相关联。 有关更多详细信息，请参阅[将打包的应用程序与一组文件类型相关联](desktop-to-uwp-extensions.md#associate-your-packaged-application-with-a-set-of-file-types)。 下面是与此元素相关的最重要的项。

| 特性或元素 | 描述 |
|----------------------|-------------|
| `Name` 特性 | 与要注册的扩展名匹配，去掉点号（在前面的示例中为 `foo`）。 |
| `Parameters` 特性 | 包含当用户双击具有此扩展的文件时要传递给应用程序的参数。 通常，至少要传递 `%1`，这是一个包含所选文件的路径的特殊参数。 这样，在你双击某个文件时，应用程序就可知道它的完整路径，并可以加载它。 |
| [SupportedFileTypes](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-supportedfiletypes) 元素 | 指定要注册的扩展名，包括点号（本示例中为 `.foo`）。 可根据需要指定多个 `<FileType>` 项以支持更多文件类型。 |

若要定义上下文菜单集成，还必须添加 [SupportedVerbs](/uwp/schemas/appxpackage/uapmanifestschema/element-uap2-supportedverbs) 子元素。 此元素包含一个或多个 [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素，这些元素定义了当用户在文件资源管理器中右键单击扩展为 .foo 的文件时将列出的选项。 有关更多详细信息，请参阅[向具有特定文件类型的文件的上下文菜单添加选项](desktop-to-uwp-extensions.md#add-options-to-the-context-menus-of-files-that-have-a-certain-file-type)。 下面是与 [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素相关的最重要的项。

| 特性或元素 | 描述 |
|----------------------|-------------|
| `Id` 特性 | 指定操作的唯一标识符。|
| `Parameters` 特性 | 与 [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 元素类似，[Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素的此特性包含在用户单击上下文菜单项时传递给应用程序的参数。 通常，除了用于获取所选文件路径的特殊 `%1` 参数之外，还会传递一个或多个参数来获取上下文。 这使应用能够理解它是从一个上下文菜单项打开的。  |
| 元素值 | [Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-uap3-verb) 元素的值包含要在上下文菜单项中显示的标签（在本例中为“调整文件大小”）。 |

### <a name="access-the-startup-parameters-in-your-app-code"></a>访问应用代码中的启动参数

应用接收参数的方式取决于所创建的应用的类型。 例如，WPF 应用通常在 `App` 类的 `OnStartup` 方法中处理启动事件参数。 可检查是否存在启动参数，并根据结果采取最适当的操作（如打开应用程序的特定窗口，而不是主窗口）。

```csharp
public partial class App : Application
{
    protected override void OnStartup(StartupEventArgs e)
    {
        if (e.Args.Contains("Resize"))
        {
            // Open a specific window of the app.
        }
        else
        {
            MainWindow main = new MainWindow();
            main.Show();
        }
    }
}
```

以下屏幕截图演示了前面的示例创建的“调整文件大小”上下文菜单项。

![快捷菜单中的“调整文件大小”命令的屏幕截图](images/file-explorer/resize-file.png)

## <a name="support-generic-files-or-folders-and-perform-complex-tasks"></a>支持泛型文件或文件夹并执行复杂任务

虽然在程序包清单中使用 [FileTypeAssociation](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation) 扩展（如前一部分所述）对于许多情况来说已经足够了，但你可能会发现它存在局限性。 最大的两项挑战如下：

* 你只能处理与你关联的文件类型。 例如，你无法处理泛型文件夹。
* 你只能通过一系列参数来启动应用。 你无法执行高级操作，如在不打开主应用的情况下启动另一个可执行文件或执行任务。

若要实现这些目标，必须创建一个 [Shell 扩展](/windows/win32/shell/shell-exts)，它提供了更强大的方法来与文件资源管理器集成。 在此场景中，你创建一个 DLL，其中包含管理文件上下文菜单所需的所有内容，包括标签、图标、状态和要执行的任务。 由于此功能是在 DLL 中实现的，因此你几乎可执行所有你在普通应用中可执行的操作。 实现 DLL 后，必须通过在程序包清单中定义的扩展来注册它。

> [!NOTE]
> 本部分中所述的过程有一个限制。 在目标计算机上安装了包含该扩展的 MSIX 包后，必须重启文件资源管理器，然后才能加载 Shell 扩展。 为此，用户可以重启计算机，或者可使用任务管理器重启 explorer.exe 进程 。

### <a name="implement-the-shell-extension"></a>实现 Shell 扩展

Shell 扩展以 [COM（组件对象模型）](/windows/win32/com/component-object-model--com--portal)为基础。 DLL 公开了一个或多个在系统注册表中注册的 COM 对象。 Windows 发现这些 COM 对象并将你的扩展与文件资源管理器集成。 因为你要将代码与 Windows Shell 集成，所以性能和内存占用情况很重要。 因此，这些类型的扩展通常是用 C++ 生成的。

有关演示如何实现 Shell 扩展的示例代码，请参阅 GitHub 上相关示例中的 [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) 项目。 此项目以 Windows 桌面示例中的[此示例](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb)为基础，它有几个版本，使示例更易于与最新版本的 Visual Studio 一起使用。

此项目包含许多用于不同任务的样本代码，如动态和静态菜单以及 DLL 的手动注册。 如果你要使用 MSIX 打包应用，则大部分代码是不需要的，因为打包支持将为你处理这些任务。 [ExplorerCommandVerb.cpp](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/ExplorerCommandVerb.cpp) 文件包含上下文菜单的实现，这是本演练中令人感兴趣的主要代码文件。

关键函数是 `CExplorerCommandVerb::Invoke`。 这是用户在上下文菜单中单击该项时调用的函数。 在本示例中，为了最大程度地降低对性能的影响，该操作针对另一个线程进行执行，这样你实际可在 `CExplorerCommandVerb::_ThreadProc` 中找到真正的实现。

```cpp
DWORD CExplorerCommandVerb::_ThreadProc()
{
    IShellItemArray* psia;
    HRESULT hr = CoGetInterfaceAndReleaseStream(_pstmShellItemArray, IID_PPV_ARGS(&psia));
    _pstmShellItemArray = NULL;
    if (SUCCEEDED(hr))
    {
        DWORD count;
        psia->GetCount(&count);

        IShellItem2* psi;
        HRESULT hr = GetItemAt(psia, 0, IID_PPV_ARGS(&psi));
        if (SUCCEEDED(hr))
        {
            PWSTR pszName;
            hr = psi->GetDisplayName(SIGDN_DESKTOPABSOLUTEPARSING, &pszName);
            if (SUCCEEDED(hr))
            {
                WCHAR szMsg[128];
                StringCchPrintf(szMsg, ARRAYSIZE(szMsg), L"%d item(s), first item is named %s", count, pszName);

                MessageBox(_hwnd, szMsg, L"ExplorerCommand Sample Verb", MB_OK);

                CoTaskMemFree(pszName);
            }

            psi->Release();
        }
        psia->Release();
    }

    return 0;
}
```

当用户右键单击某个文件或文件夹时，此函数将显示一个消息框，其中包含所选文件或文件夹的完整路径。 如果要以其他方式自定义 Shell 扩展，可在示例中扩展以下函数：

- 可更改 [GetTitle](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettitle) 函数，以自定义上下文菜单中该项的标签。
- 可更改 [GetIcon](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-geticon) 函数，以自定义上下文菜单中该项附近显示的图标。
- 可更改 [GetTooltip](/windows/win32/api/shobjidl_core/nf-shobjidl_core-iexplorercommand-gettooltip) 函数，以自定义将鼠标悬停在上下文菜单中的该项时显示的工具提示。

### <a name="register-the-shell-extension"></a>注册 Shell 扩展

由于 Shell 扩展以 COM 为基础，因此 DLL 实现必须作为 COM 服务器公开，使 Windows 可以将其与文件资源管理器集成。 通常，通过向 COM 服务器分配一个唯一 ID（称为 CLSID）并将其注册到系统注册表的特定配置单元中来实现此操作。 在 [ExplorerCommandVerb](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/tree/main/Docs-ContextMenuSample/ExplorerCommandVerb) 项目中，在 [Dll.h](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ExplorerCommandVerb/Dll.h) 文件中定义了 `CExplorerCommandVerb` 扩展的 CLSID。

```cpp
class __declspec(uuid("CC19E147-7757-483C-B27F-3D81BCEB38FE")) CExplorerCommandVerb;
```

在 MSIX 包中打包 Shell 扩展 DLL 时，可遵循类似的方法。 但是，GUID 必须在程序包清单而不是注册表中注册，如[此处](https://blogs.windows.com/windowsdeveloper/2017/04/13/com-server-ole-document-support-desktop-bridge/)所述。

在程序包清单中，首先向 Package 元素添加以下命名空间。

```xml
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  xmlns:desktop5="http://schemas.microsoft.com/appx/manifest/desktop/windows10/5"
  xmlns:com="http://schemas.microsoft.com/appx/manifest/com/windows10" 
  IgnorableNamespaces="desktop desktop4 desktop5 com">
    
    ...
</Package>
```

若要注册 CLSID，可向程序包清单添加类别为 `windows.comServer` 的 [com.Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) 元素。 此元素必须添加为 [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素下 [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素的子元素。 此示例摘自 GitHub 上相关示例中的 [Package.appxmanifest](https://github.com/microsoft/Windows-AppConsult-Samples-DesktopBridge/blob/main/Docs-ContextMenuSample/ContextMenuSample.Package/Package.appxmanifest) 文件。

```xml
<com:Extension Category="windows.comServer">
  <com:ComServer>
    <com:SurrogateServer DisplayName="ContextMenuSample">
      <com:Class Id="CC19E147-7757-483C-B27F-3D81BCEB38FE" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
    </com:SurrogateServer>
  </com:ComServer>
</com:Extension>
```

[com:Class](/uwp/schemas/appxpackage/uapmanifestschema/element-com-surrogateserver-class) 元素中有两个要配置的关键特性。

| Attribute | 描述 |
|----------------------|-------------|
| `Id` 特性 | 这必须与要注册的对象的 CLSID 匹配。 在本示例中，这是在与 `CExplorerCommandVerb` 类关联的 `Dll.h` 文件中声明的 CLSID。 |
| `Path` 特性 | 这必须包含公开 COM 对象的 DLL 的名称。 此示例在包的根中包含了 DLL，因此它可以只指定 `ExplorerCommandVerb` 项目生成的 DLL 的名称。 |

接下来，添加注册文件上下文菜单的另一个扩展。 为此，可向程序包清单添加类别为 `windows.fileExplorerContextMenus` 的 [desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 元素。 此元素还必须添加为 [Application](/uwp/schemas/appxpackage/uapmanifestschema/element-application) 元素下 [Extensions](/uwp/schemas/appxpackage/uapmanifestschema/element-1-extensions) 元素的子元素。

```xml
<desktop4:Extension Category="windows.fileExplorerContextMenus">
  <desktop4:FileExplorerContextMenus>
    <desktop5:ItemType Type="Directory">
      <desktop5:Verb Id="Command1" Clsid="CC19E147-7757-483C-B27F-3D81BCEB38FE" />
    </desktop5:ItemType>
  </desktop4:FileExplorerContextMenus>
</desktop4:Extension>
```

[desktop4:Extension](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-extension) 元素下有两个要配置的关键特性。

| 特性或元素 | 描述 |
|----------------------|-------------|
| [desktop5:ItemType](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-itemtype) 的 `Type` 特性 | 这定义要与上下文菜单关联的项的类型。 如果你想对所有的文件显示，它可以是一个星号 (`*`)；它可以是一个特定的文件扩展 (`.foo`)；或者它可以用于文件夹 (`Directory`)。 |
| [desktop5:Verb](/uwp/schemas/appxpackage/uapmanifestschema/element-desktop5-verb) 的 `Clsid` 特性 | 这必须与之前在程序包清单文件中注册为 COM 服务器的 CLSID 匹配。 |

### <a name="configure-the-dll-in-the-package"></a>在包中配置 DLL

在 MSIX 包的根中包含实现 Shell 扩展的 DLL（在本示例中为 ExplorerCommandVerb.dll）。 如果你要使用 [Windows 应用程序打包项目](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)，最简单的解决方案是将 DLL 复制并粘贴到该项目中，并确保将 DLL 文件属性的“复制到输出目录”选项设置为“如果较新则复制” 。

若要确保包始终包含最新版本的 DLL，可向 Shell 扩展项目添加[后期生成事件](/visualstudio/ide/specifying-custom-build-events-in-visual-studio)，以便每次生成时，DLL 都被复制到 Windows 应用程序打包项目中。

### <a name="restart-file-explorer"></a>重启文件资源管理器

安装 Shell 扩展包后，必须重启文件资源管理器，然后才能加载 Shell 扩展。 这是通过 MSIX 包部署和注册的 Shell 扩展的一个限制。

若要测试 Shell 扩展，请重启电脑或使用“任务管理器”重启 explorer.exe 进程 。 完成此操作后，你应该能够在上下文菜单中看到该项。

![自定义上下文菜单项的屏幕截图](images/file-explorer/folder-context-menu.png)

如果你单击它，`CExplorerCommandVerb::_ThreadProc` 函数将被调用以显示包含所选文件夹路径的消息框。

![自定义弹出窗口的屏幕截图](images/file-explorer/popup.png)
