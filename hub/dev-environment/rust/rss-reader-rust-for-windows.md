---
title: RSS 读者教程（适用于 Windows 的 Rust 与 VS Code）
description: 演练了如何编写从 RSS 源下载博客文章标题的简单应用。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, 了解 Rust, 初学者在 Windows 上使用 Rust 进行开发, 结合使用 Rust 与 VS Code, Rust for Windows
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: b55faf6d44395989cb7eec39fb9cdd1ee7128aa7
ms.sourcegitcommit: bcdec8bda3106cd5588464531e582101d52dcc80
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/06/2021
ms.locfileid: "102254549"
---
# <a name="rss-reader-tutorial-rust-for-windows-with-vs-code"></a>RSS 读者教程（适用于 Windows 的 Rust 与 VS Code）

上一主题介绍了 [Rust for Windows 与 windows crate](rust-for-windows.md)。

现在，让我们尝试利用 Rust for Windows 来编写一个简单的应用，用于从真正简单的整合 (RSS) 源下载博客文章的标题。

1. 启动命令提示符（适用于 VS 的 x64 本机工具命令提示符或任何 `cmd.exe`），并使用 `cd` 转到要用来保存 Rust 项目的文件夹中。

2. 然后，通过 Cargo 新建一个名为“rss_reader” 的 Rust 项目，并使用 `cd` 转到此项目新创建的文件夹中。

   ```console
   cargo new rss_reader
   cd rss_reader
   ```

3. 现在&mdash;还是通过 Cargo&mdash;，我们将新建一个名为“bindings”的子项目。 如以下命令所示，这个新项目是一个库，它将作为我们绑定到要调用的 Windows API 的媒介。 在生成时，bindings 库子项目将生成为 crate（这是 Rust 对二进制文件或库的术语）。 我们将在 rss_reader 项目中使用这个 crate，稍后我们会加以演示。

   ```console
   cargo new --lib bindings
   ```

   将 bindings 嵌套为 crate 意味着，在生成 rss_reader 时，bindings 将能够缓存我们导入的任何绑定的结果。

4. 然后，在 VS Code 中打开 rss_reader 项目。

   ```console
   code .
   ```

5. 让我们先来处理 bindings 库。

   在 VS Code 的资源管理器中，依次打开 `bindings` > `Cargo.toml` 文件。

   ![在项目创建后，Cargo.toml 文件在 VS Code 中打开](../../images/rust-rss-reader-1.png)

   `Cargo.toml` 文件是描述 Rust 项目（包括它所具有的任何依赖项）的文本文件。

   目前，`dependencies` 部分是空的。 所以现在就来编辑此部分（同时添加 `[build-dependencies]` 部分），如下所示。

   ```toml
   # bindings\Cargo.toml
   ...

   [dependencies]
   windows="0.3.1"

   [build-dependencies]
   windows="0.3.1"
   ```

   我们刚刚在 windows crate 上为 bindings 库及其生成脚本同时添加了依赖项。 这样，Cargo 就可以包的形式下载、生成和缓存 Windows 支持。 将版本号设置为最新版本 &mdash; 你将能够在 [windows crate](https://crates.io/crates/windows) 的网页上看到此信息。

6. 现在，可以添加生成脚本本身，这就是我们将生成最终依赖的绑定的地方。 在 VS Code 中，右键单击“bindings”文件夹，然后单击“新建文件”。 键入名称“build.rs”，然后按 Enter。 按下面所示编辑 `build.rs`。

   ```rust
   // bindings\build.rs
   fn main() { 
       windows::build!(windows::web::syndication::SyndicationClient);
   }
   ```

   `windows::build!` 宏负责以 `.winmd` 文件的形式解析任何依赖项，并直接从元数据生成所选类型的绑定。 我们本可以要求生成一个完整的命名空间（使用 `windows::web::syndication::*` 而不是 `windows::web::syndication::SyndicationClient`）。 但在此示例中，我们要求仅为 [SyndicationClient](/uwp/api/windows.web.syndication.syndicationclient) 类型生成绑定。 通过这种方式，你可以根据需要导入任意量的代码，并避免等待生成和编译你永远不会需要的代码。
   
   此外，build 宏会自动查找显式列出的类型的所有依赖项。 在此示例中，SyndicationClient 包含需要 [Uri](/uwp/api/windows.foundation.uri) 类型参数的方法。 因此，build 宏包含 Uri 的定义，这样你就可以调用相应方法了。 其他类型是 windows crate 本身的一部分。 例如，windows::Result 是由 windows crate 定义的，因此它始终可用。 你会发现，[Windows.Foundation](/uwp/api/windows.foundation) 命名空间中需要的大部分内容都自动包含在其中。

7. 依次打开 `bindings` > `src` > `lib.rs` 源代码文件。 若要添加上一步中生成的绑定，请将 `lib.rs` 中的默认代码替换为以下代码。

   ```rust
   // bindings\src\lib.rs
   ::windows::include_bindings!();
   ```

   `windows::include_bindings!` 宏包含生成脚本在上一步中生成的源代码。 现在，无论何时需要访问其他 API，只需在生成脚本 (`build.rs`) 中将其列出即可。

8. 现在，让我们实现主项目 rss_reader。 首先，打开项目根目录下的 `Cargo.toml` 文件，然后在内部 bindings crate 上添加以下依赖项。

   ```toml
   # Cargo.toml
   ...

   [dependencies] 
   bindings = { path = "bindings" }
   ```

9. 最后，依次打开 rss_reader 项目的 `src` > `main.rs` 源代码文件。 其中有一个简单的代码，用于输出“Hello, world!” 消息。 将以下代码添加到 `main.rs` 的开头。

   ```rust
   // src\main.rs
   use bindings::{ 
       windows::foundation::Uri,
       windows::web::syndication::SyndicationClient,
       windows::Result,
   };

   fn main() {
       println!("Hello, world!");
   }
   ```

   `use` 声明缩短了我们将要使用的类型的路径。 其有我们前面提到过的 Uri 类型。 windows::Result 可以帮助我们进行错误传播和简洁的错误处理。

10. 若要新建 [Uri](/uwp/api/windows.foundation.uri)，请将以下代码添加到 main 函数中。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;

       Ok(())
   }
   ```

   请注意，我们使用 windows::Result 作为 main 函数的返回类型。 这会让操作变得更简单，因为处理来自操作系统 (OS) API 的错误是很常见的。

   你可以在创建 Uri 的代码行末尾看到问号运算符。 简而言之，我们这样做是为了利用 Rust 的错误传播和短路逻辑。 也就是说，我们不需要对这个简单的示例进行大量的手动错误处理。 若要详细了解 Rust 的这项功能，请参阅[利用 ? 运算符简化错误处理](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html)。

11. 为了下载此 RSS 源，我们将新建一个 SyndicationClient 对象。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;

       Ok(())
   }
   ```

   新函数是 Rust 中等效于默认构造函数的函数。

12. 现在，我们可以使用 SyndicationClient 对象来检索源。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       Ok(())
   }
   ```

由于 [RetrieveFeedAsync](/uwp/api/windows.web.syndication.syndicationclient.retrievefeedasync) 是异步 API，因此可以使用阻塞性 get 函数（如上所示）。 此外，也可以在 `async` 函数内使用 `await` 运算符（以协作方式等待结果），就像在 C# 或 C++ 中所做的那样。

13. 现在，我们可以简单地循环访问结果项，然后只打印标题。

   ```rust
   // src\main.rs
   ...

   fn main() -> Result<()> {
       let uri = Uri::create_uri("https://blogs.windows.com/feed")?;
       let client = SyndicationClient::new()?;
       let feed = client.retrieve_feed_async(uri)?.get()?;

       for item in feed.items()? {
           println!("{}", item.title()?.text()?);
       }

       Ok(())
   }
   ```

14. 现在，依次单击“运行” > “运行但不调试”（或按 Ctrl+F5），以确认能否生成和运行。 文本编辑器中也嵌入了“调试”和“运行”命令。 或者，也可以从命令提示符提交命令 `cargo run`（先使用 `cd` 转到 `rss_reader` 文件夹），这将执行生成和运行操作。

   ![文本编辑器中嵌入的“调试”和“运行”命令](../../images/rust-rss-reader-2.png)

   在下面的“终端”窗格中，可以看到 Cargo 成功地下载并编译了 windows crate，缓存结果，并使用它们在更短时间内完成后续的生成操作。 然后，它生成并运行示例，以显示博客文章标题列表。

   ![博客文章标题列表](../../images/rust-rss-reader-3.png)

这和为 Windows 编写 Rust 程序一样简单。 但实际上，很多开发人员都喜欢生成这个工具，这样 Rust 既可以在编译时解析基于 [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/)（公共语言基础结构 (CLI)）的 `.winmd` 文件，也可以在运行时如实地使用基于 COM 的应用程序二进制接口 (ABI)，同时兼顾安全性和效率。

## <a name="related"></a>相关内容

* [适用于 Windows 的 Rust 与 Windows 包装箱](rust-for-windows.md)
* [ECMA-335](https://www.ecma-international.org/publications-and-standards/standards/ecma-335/)
* [利用 ? 运算符简化错误处理](https://doc.rust-lang.org/edition-guide/rust-2018/error-handling-and-panics/the-question-mark-operator-for-easier-error-handling.html)
