---
title: 在 Windows 上针对 Rust 设置开发环境
description: 为想要在 Windows 上使用 Rust 进行开发的初学者创建开发环境。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, 了解 Rust, 初学者在 Windows 上使用 Rust 进行开发, 结合使用 Rust 与 VS Code
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 5aac8dd9b9f760f6e1ed49ff0246e44c400d72c4
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194137"
---
# <a name="set-up-your-dev-environment-on-windows-for-rust"></a>在 Windows 上针对 Rust 设置开发环境

[关于在 Windows 上使用 Rust 进行开发的概述](overview.md)主题介绍了 Rust，并讨论了它是什么及其主要组成部分。 在本主题中，我们将创建开发环境。

建议在 Windows 上进行 Rust 开发。 但是，如果你计划在 Linux 上进行本地编译和测试，也可以选择在[适用于 Linux 的 Windows 子系统 (WSL)](/windows/wsl/about) 上使用 Rust 进行开发。

## <a name="install-visual-studio-recommended-or-the-microsoft-c-build-tools"></a>安装 Visual Studio（推荐）或 Microsoft C++ 生成工具

在 Windows 上，Rust 需要某些 C++ 生成工具。

你可以下载 [Microsoft C++ 生成工具](https://visualstudio.microsoft.com/visual-cpp-build-tools/)，也可以（推荐）首选直接安装 [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)。

> [!NOTE]
> 我们将使用 Visual Studio Code 作为 Rust 的集成开发环境 (IDE)，而不是 Visual Studio。 但你仍然可以免费安装 Visual Studio。 社区版是可用的 &mdash; 它对学生、开放源代码参与者和个人都是免费的。

安装 Visual Studio 时，建议选择几个 Windows 工作负载 &mdash; .NET 桌面开发、使用 C++ 的桌面开发和通用 Windows 平台开发。 你可能认为不需要全部三种，但很有可能会出现某依赖项需要全部三种，因此我们认为选择全部三种会更简单。

新的 Rust 项目默认使用 Git。 因此，还要将独立组件 Git for Windows 添加到组合中（使用搜索框按名称搜索它）。

![.NET 桌面开发、使用 C++ 的桌面开发和通用 Windows 平台开发](../../images/rust-vs-workloads.png)

## <a name="install-rust"></a>安装 Rust

接下来，[从 Rust 网站安装 Rust](https://www.rust-lang.org/tools/install)。 此网站检测到你运行的是 Windows，并提供适用于 Windows 的 `rustup` 工具的 64 位和 32 位安装程序，以及有关将 Rust 安装到[适用于 Linux 的 Windows 子系统 (WSL)](/windows/wsl/about) 的说明。

> [!TIP]
> Rust 在 Windows 上运行得非常好；因此不需要使用 WSL 路由（除非你打算在 Linux 上进行本地编译和测试）。 由于你运行的是 Windows，因此建议直接运行适用于 64 位 Windows 的 `rustup` 安装程序。 然后，你就可以使用 Rust 为 Windows 编写应用了。

当 Rust 安装程序完成后，你就可以使用 Rust 进行编程了。 你还没有方便使用的 IDE（下一部分 &mdash; [安装 Visual Studio Code](#install-visual-studio-code) 中将讨论这个问题）。 此外，你也还不能调用 Windows API。 但你可以启动命令提示符（适用于 VS 的 x64 本机工具命令提示符或任何 `cmd.exe`），并能发出命令 `cargo --version`。 如果你看到版本号打印出来，则可以确认 Rust 已正确安装。

如果你对上面使用的 `cargo` 关键字感到好奇，请了解 Cargo 是 Rust 开发环境中用于管理和生成项目（更恰当地说是包）及其依赖项的工具的名称。

如果你真的现在就想钻研一些编程（即使没有方便使用的 IDE），那么可以阅读 Rust 网站上《Rust 编程语言》一书中的 [Hello, World!](https://doc.rust-lang.org/book/ch01-02-hello-world.html) 章节。

## <a name="install-visual-studio-code"></a>安装 Visual Studio Code

通过使用 Visual Studio Code (VS Code) 作为文本编辑器/集成开发环境 (IDE)，可以利用诸如代码完成、语法突出显示、格式设置和调试等语言服务。

VS Code 还包含[内置终端](https://code.visualstudio.com/docs/editor/integrated-terminal)，便于你发出命令行参数（例如向 Cargo 发出命令）。

1. 首先，下载并安装[适用于 Windows 的 Visual Studio Code](https://code.visualstudio.com)。

2. 安装 VS Code 后，安装 rust-analyzer 扩展。 可以[从 Visual Studio Marketplace 安装 rust-analyzer 扩展](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)，也可以打开 VS Code，然后在“扩展”菜单 (Ctrl+Shift+X) 中搜索“rust-analyzer”。

3. 若要获得调试支持，请安装 CodeLLDB 扩展。 可以[从 Visual Studio Marketplace 安装 CodeLLDB 扩展](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)，也可以打开 VS Code，然后在“扩展”菜单 (Ctrl+Shift+X) 中搜索“CodeLLDB”。

   > [!NOTE]
   > 除了使用 CodeLLDB 扩展获得调试支持外，还可以使用 Microsoft C/C++ 扩展。 C/C++ 扩展不像 CodeLLDB 那样与 IDE 集成得很好。 但 C/C++ 扩展提供了上级调试信息。 所以你最好把它准备好，以防万一。
   >
   > 可以[从 Visual Studio Marketplace 安装 C/C++ 扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)，也可以打开 VS Code，然后在“扩展”菜单 (Ctrl+Shift+X) 中搜索“C/C++”。

4. 若要在 VS Code 中打开终端，请依次选择“视图” > “终端”，或使用快捷键 Ctrl+`（使用反撇号字符）。 默认终端是 PowerShell。

## <a name="hello-world-tutorial-rust-with-vs-code"></a>Hello, world! 教程（Rust 与 VS Code）

让我们通过一个简单的“Hello, world!” 应用来了解一下 Rust。

1. 首先，启动命令提示符（适用于 VS 的 x64 本机工具命令提示符或任何 `cmd.exe`），并使用 `cd` 转到要用来保存 Rust 项目的文件夹中。

2. 然后，使用以下命令让 Cargo 为你新建一个 Rust 项目。

   ```console
   cargo new first_rust_project
   ```

   传递给 `cargo new` 命令的参数是你希望 Cargo 创建的项目的名称。 在此示例中，项目名称为 first_rust_project。 建议使用蛇形命名法来命名 Rust 项目（在这种命名法中，单词都是小写的，每个空格都用下划线字符替代）。

   Cargo 为你创建采用你提供的名称的项目。 事实上，Cargo 的新项目包含一个非常简单的应用的源代码，此应用会输出“Hello, world!” 消息，我们稍后就会看到。 除了创建 first_rust_project 项目外，Cargo 还创建了一个名为 first_rust_project 的文件夹，并将此项目的源代码文件放入其中。

3. 现在，使用 `cd` 转到此文件夹，然后在此文件夹的上下文中启动 VS Code。

   ```console
   cd first_rust_project
   code .
   ```

4. 在 VS Code 的资源管理器中，打开 `src` > `main.rs` 文件，这是包含应用入口点（名为 main 的函数）的 Rust 源代码文件。 这是它的外观。

   ```rust
   // main.rs
   fn main() {
     println!("Hello, world!");
   }
   ```

   > [!NOTE]
   > 在 VS Code 中打开第一个 `.rs` 文件时，你会看到一条通知，提示你一些 Rust 组件没有安装，并询问你是否要安装这些组件。 单击“是”，然后 VS Code 就会安装 Rust 语言服务器。

   通过浏览 `main.rs` 中的代码，可以判断 main 是一个函数定义，它打印字符串“Hello, world!”。 如需了解更多语法详情，请参阅 Rust 网站上的 [Rust 程序剖析](https://doc.rust-lang.org/book/ch01-02-hello-world.html#anatomy-of-a-rust-program)。

5. 现在，让我们尝试使用调试器来运行此应用。 在第 2 行放置一个断点，然后依次单击“运行” > “开始调试”（或按 F5）。 文本编辑器中也嵌入了“调试”和“运行”命令。

   > [!NOTE]
   > 当你首次使用调试器来运行应用时，将会看到显示“无法启动调试，因为没有提供启动配置”的对话框。 单击“确定”后，就会看到第二个对话框，其中显示“在此工作区中检测到 Cargo.toml。 是否要为其目标生成启动配置?”。 单击 **“是”** 。 然后，关闭 launch.json 文件，并重新开始调试。

6. 可以看到，调试器在第 2 行中断。 按 F5 继续运行，应用将一直运行到完成。 在“终端”窗格中，你会看到预期的输出“Hello, world!”。

## <a name="rust-for-windows"></a>适用于 Windows 的 Rust

不仅可以在 Windows 上使用 Rust，还可以使用 Rust 为 Windows 编写应用。 通过 windows crate，可以调用任何过去、现在和将来的 Windows API。 如需了解更多详情和代码示例，请参阅 [Rust for Windows 与 windows crate](rust-for-windows.md) 主题。

## <a name="related"></a>相关内容

* [适用于 Windows 的 Rust 与 Windows 包装箱](rust-for-windows.md)
* [适用于 Linux 的 Windows 子系统 (WSL)](/windows/wsl/about)
* [Microsoft C++ 生成工具](https://visualstudio.microsoft.com/visual-cpp-build-tools/)
* [Microsoft Visual Studio](https://visualstudio.microsoft.com/downloads/)
* [适用于 Windows 的 Visual Studio Code](https://code.visualstudio.com)
* [rust-analyzer 扩展](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer)
* [CodeLLDB 扩展](https://marketplace.visualstudio.com/items?itemName=vadimcn.vscode-lldb)
* [扩展](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools)
