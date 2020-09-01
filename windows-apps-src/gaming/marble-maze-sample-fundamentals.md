---
title: Marble Maze 示例基础
description: 本文档介绍了 Marble Maze 项目的基本特征；例如如何在 Windows 运行时环境中使用 Visual C++、如何创建和构造，以及如何生成。
ms.assetid: 73329b29-62e3-1b36-01db-b7744ee5b4c3
ms.date: 08/22/2017
ms.topic: article
keywords: windows 10, uwp, 游戏, 示例, directx, 基础知识
ms.localizationpriority: medium
ms.openlocfilehash: 714641be3c5ae6e202f0d6b7da5a1a4c1fc93d86
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89165321"
---
# <a name="marble-maze-sample-fundamentals"></a>Marble Maze 示例基础




本主题介绍了 Marble Maze 项目的基本特征 &mdash; 例如如何在 Windows 运行时环境中使用 Visual C++、如何创建和构造，以及如何生成。 本主题还介绍了代码中使用的一些约定。

> [!NOTE]
> 与本文档对应的示例代码位于 [DirectX Marble Maze 游戏示例](https://github.com/microsoft/Windows-appsample-marble-maze)中。

本文档讨论了计划和开发通用 Windows 平台 (UWP) 游戏时的一些重要事项。

-   在 Visual Studio 中使用 **DirectX 11 应用 (通用 Windows-c + +/cx) ** 模板来创建 DirectX UWP 游戏。
-   Windows 运行时提供了各种类和接口，让你可以用一种更加现代、面向对象的方式开发 UWP 应用。
-   使用包含 hat (^) 符号的对象引用来管理 Windows 运行时变量的生存期，使用 [Microsoft：： WRL：： ComPtr](/cpp/windows/comptr-class) 管理 COM 对象的生存期，使用 [std：： shared \_ ptr](/cpp/standard-library/shared-ptr-class) 或 [std：： unique \_ ptr](/cpp/standard-library/unique-ptr-class) 来管理所有其他堆分配的 c + + 对象的生存期。
-   在大多数情况下，使用异常处理而不是结果代码来处理意外错误。
-   结合使用 [SAL 注释](/visualstudio/code-quality/using-sal-annotations-to-reduce-c-cpp-code-defects) 和代码分析工具来帮助发现应用中的错误。

## <a name="creating-the-visual-studio-project"></a>创建 Visual Studio 项目


如果已下载并解压缩了该示例，则可以在 Visual Studio 中打开 **MarbleMaze_VS2017.sln** 文件（在 **C++** 文件夹中），这些代码会展现在你的面前。

当我们为 Marble Maze 创建了 Visual Studio 项目时，我们从一个现有项目开始。 但是，如果你还没有提供 DirectX UWP 游戏要求的基本功能的现有项目，则建议你在 ** (通用 Windows-c + +/cx) ** 模板的情况下创建基于 Visual Studio DirectX 11 应用的项目，因为它提供了一个基本的工作三维应用程序。 为此，请按照下列步骤进行操作：

1. 在 Visual Studio 2019 中，选择 " **文件" > 新建 > 项目 ...**

2. 在 "新建 **项目** " 窗口中，选择 " **DirectX 11 应用 (通用 Windows-c + +/cx) **。 如果看不到此选项，则可能是由于未安装所需的组件， &mdash; 请 [通过添加或删除工作负载和组件来修改 Visual Studio 2019](/visualstudio/install/modify-visual-studio) ，了解有关如何安装其他组件的信息。

![新建项目](images/vs2019-marble-maze-sample-fundamentals-1.png)

3. 选择 " **下一步**"，然后输入 **项目名称**、要存储的文件的 **位置** 和 **解决方案名称**，然后选择 " **创建**"。



**DirectX 11 应用 (通用 Windows-c + +/cx) **模板中的一个重要项目设置是 **/ZW**选项，它使程序能够使用 Windows 运行时语言扩展。 在使用 Visual Studio 模板时默认已启用此选项。 有关如何在 Visual Studio 中设置编译器选项的详细信息，请参阅[设置编译器选项](/cpp/build/reference/setting-compiler-options)。

> **警告**    **/ZW**选项与 **/clr**等选项不兼容。 **/clr**选项表明无法同时针对 .NET Framework 和 Windows 运行时开发同一个 Visual C++ 项目。

 

从 Microsoft Store 获取的每个 UWP 应用都以应用程序包的形式提供。 应用包中包含一个程序包清单，后者包含有关应用的信息。 例如，你可指定应用的功能（如需要的访问受保护的系统资源或用户数据的能力）。 如果你确定应用需要某些功能，可使用程序包清单来声明所需的功能。 清单还允许指定项目属性，例如支持的设备旋转方向、磁贴图像和初始屏幕。 你可以打开项目中的 **Package.appxmanifest** 来编辑清单。 有关应用包的详细信息，请参阅[打包应用](../packaging/index.md)。

##  <a name="building-deploying-and-running-the-game"></a>生成、部署和运行游戏

在 Visual Studio 顶部下拉菜单中绿色播放按钮的左侧，选择你的部署配置。 我们建议针对你的设备体系结构（**x86** 表示 32 位，**x64** 表示 64 位）将其设置为**调试**，并设置为**本地计算机**。 你还可以在**远程计算机**上进行测试，或者对通过 USB 连接的**设备**进行测试。 然后单击绿色播放按钮以生成并部署到你的设备。

![调试; x64; 本地计算机](images/marble-maze-sample-fundamentals-2.png)

###  <a name="controlling-the-game"></a>控制游戏

可使用触摸、加速计、Xbox One 控制器或鼠标来控制 Marble Maze。

-   使用控制器上的方向板更改活动菜单项。
-   使用触摸、控制器上的 A 或“开始”按钮或鼠标来挑选菜单项。
-   使用触摸、加速计、左操纵杆或鼠标来倾斜迷宫。
-   使用触摸、控制器上的 A 或“开始”按钮或鼠标来关闭菜单，例如高分表。
-   使用控制器上的“开始”按钮或键盘上的 P 键暂停或继续游戏。
-   使用控制器上的“后退”按钮或键盘上的 Home 键重启游戏。
-   高分表可见时，使用控制器上的“后退”按钮或键盘上的 Home 键清除所有分数。

##  <a name="code-conventions"></a>代码转换


Windows 运行时是可用于创建仅在特殊应用程序环境中运行的 UWP 应用的编程接口。 此类应用使用已授权的功能、数据类型和设备，并从 Microsoft Store 分发。 在最低级别上，Windows 运行时由一个应用程序二进制接口 (ABI) 组成。 ABI 是一个低级二进制合约，它使得 Windows 运行时 API 能够访问多种编程语言，例如 JavaScript、.NET 语言和 Visual C++。

为了从 JavaScript 和 .NET 调用 Windows 运行时 API，这些语言需要特定于每种语言环境的投影。 当你从 JavaScript 或 .NET 调用 Windows 运行时 API 时，你调用的是投影，而投影又会调用基础的 ABI 函数。 尽管你可以直接从 C++ 调用 ABI 函数，但 Microsoft 也为 C++ 提供了投影，因为这些投影可让使用 Windows 运行时 API 更简单，同时仍然保持较高的性能。 Microsoft 还提供了明确支持 Windows 运行时投影的 Visual C++ 的语言扩展。 其中很多语言扩展都和 C++/CLI 语言有着类似的语法。 但是，原生应用将此语法用于 Windows 运行时，而不是公共语言运行时 (CLR)。 对象引用或乘幂号 (^) 修饰符是这种新语法的一个重要部分，因为它支持以引用计数的方式自动删除运行时对象。 无需调用 [AddRef](/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref) 和 [Release](/windows/desktop/api/unknwn/nf-unknwn-iunknown-release) 等方法来管理 Windows 运行时对象的生命周期，运行时在没有其他组件引用该对象时删除它，例如在它离开范围或你将所有引用设置为 **nullptr** 时。 使用 Visual C++ 创建 UWP 应用的另一个重要部分是 **ref new** 关键字。 使用 **ref new** 而不是 **new** 来创建引用计数的 Windows 运行时对象。 有关详细信息，请参阅[类型系统 (C++/CX)](/cpp/cppcx/type-system-c-cx)。

> [!IMPORTANT]
> **^** 创建 Windows 运行时对象或创建 Windows 运行时组件时，只需使用和**引用 new** 。 当编写不使用 Windows 运行时的核心应用程序代码时，可以使用标准 C++ 语法。

大理石迷宫 **^** 与 **Microsoft：： WRL：： ComPtr** 一起使用，以管理堆分配的对象并最大程度地减少内存泄漏。 建议使用 ^ 管理 Windows 运行时变量的生存期，使用 **ComPtr** 来管理 COM 变量的生存期 (例如，使用 DirectX) 时，使用 **std：： shared \_ ptr** 或 **std：： unique \_ ptr** 来管理所有其他堆分配的 c + + 对象的生存期。

 

有关可用于 UWP 应用的语言扩展的详细信息，请参阅 [Visual C++ 语言参考 (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx)。

###  <a name="error-handling"></a>错误处理。

Marble Maze 使用异常处理作为处理意外错误的主要方式。 尽管游戏代码在传统上使用日志记录或错误代码（例如 **HRESULT** 值）来表示错误，但异常处理有两个主要优势。 首先，它可以使代码更易于读取和维护。 从代码角度讲，错误处理可将错误更方便地传播到可以处理该错误的例程。 使用错误代码通常需要每个函数明确传播错误。 第二个优势是，可以将 Visual Studio 调试器配置为在发生异常时中断，以便你可以在该错误的位置和上下文处立即停下。 Windows 运行时也广泛使用了异常处理。 因此，通过在你的代码中使用异常处理，你可以将所有错误处理组合到一个模型中。

我们建议在错误处理模型中使用以下约定：

-   使用异常传播意外错误。
-   不使用异常控制代码流。
-   仅捕获你可以安全处理且可以恢复的异常。 否则，不要捕获异常并允许应用终止。
-   当调用返回 **HRESULT** 的 DirectX 例程时，请使用 **DX::ThrowIfFailed** 函数。 [DirectXHelper.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/master/C%2B%2B/Shared/DirectXHelper.h) 中定义了此函数。 如果提供的 **HRESULT** 是错误代码，则 **ThrowIfFailed** 会引发异常。 例如， **E \_ 指针** 会导致 **ThrowIfFailed** 引发 [Platform：： NullReferenceException](/cpp/cppcx/platform-nullreferenceexception-class)。

    使用 **ThrowIfFailed** 时，将 DirectX 调用放在单独一行，以帮助改善代码可读性，如下面的示例所示。

    ```cpp
    // Identify the physical adapter (GPU or card) this device is running on.
    ComPtr<IDXGIAdapter> dxgiAdapter;
    DX::ThrowIfFailed(
        dxgiDevice->GetAdapter(&dxgiAdapter)
        );
    ```

-   尽管我们建议避免使用 **HRESULT** 处理意外错误，但更重要的是避免使用异常处理来控制代码流。 因此，最好在必要时使用 **HRESULT** 返回值来控制代码流。

###  <a name="sal-annotations"></a>SAL 注释

结合使用 SAL 注释和代码分析工具来帮助发现应用中的错误。

通过使用 Microsoft 源代码注释语言 (SAL)，你可注释或描述函数使用其参数的方式。 SAL 注释也可描述返回值。 SAL 注释可与 C/C++ 代码分析工具一起发现 C 和 C++ 源代码中的可能缺陷。 工具报告的常见编码错误包括缓冲区溢出、内存未初始化、null 指针取消引用以及内存和资源泄漏。

请考虑使用 **BasicLoader::LoadMesh** 方法，它在 [BasicLoader.h](https://github.com/Microsoft/Windows-appsample-marble-maze/blob/e62d68a85499e208d591d2caefbd9df62af86809/C%2B%2B/Shared/BasicLoader.h) 中声明。 此方法使用 `_In_` 指定 *filename* 是一个输入参数（因此仅用于读取），使用 `_Out_` 指定 *vertexBuffer* 和 *indexBuffer* 是输出参数（因此仅用于写入），以及使用 `_Out_opt_` 指定 *vertexCount* 和 *indexCount* 是可选的输出参数（可写入）。 因为 *vertexCount* 和 *indexCount* 是可选的输出参数，所以允许它们使用 **nullptr**。 C/C++ 代码分析工具检查对此方法的调用，以确保它传递的参数满足这些条件。

```cpp
void LoadMesh(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    );
```

若要在应用上执行代码分析，在菜单栏上选择**生成 > 对解决方案运行代码分析**。 有关代码分析的详细信息，请参阅[使用代码分析分析 C/C++ 代码质量](/visualstudio/code-quality/analyzing-c-cpp-code-quality-by-using-code-analysis)。

可用注释的完整列表在 sal.h 中定义。 有关详细信息，请参阅 [SAL 注释](/cpp/c-runtime-library/sal-annotations)。

## <a name="next-steps"></a>后续步骤


有关如何构建 Marble Maze 应用程序代码，以及 DirectX UWP 应用的结构与传统桌面应用程序有何不同的信息，请参阅 [Marble Maze 应用程序结构](marble-maze-application-structure.md)。

## <a name="related-topics"></a>相关主题


* [Marble Maze 应用程序结构](marble-maze-application-structure.md)
* [开发 Marble Maze，一款使用 C++ 和 DirectX 的 UWP 游戏](developing-marble-maze-a-windows-store-game-in-cpp-and-directx.md)

 

 