---
title: 在 Windows 上通过 Rust 进行开发的概述
description: 面向想要使用 Rust 在 Windows 上进行开发的初学者的概述。
author: stevewhims
ms.author: stwhi
manager: jken
ms.topic: article
keywords: Rust, Windows 10, Microsoft, 了解 Rust, 初学者在 Windows 上使用 Rust 进行开发, 结合使用 Rust 与 VS Code
ms.localizationpriority: medium
ms.date: 03/04/2021
ms.openlocfilehash: 7d3d2cbe392fe54a82af982a23b866a745e993ae
ms.sourcegitcommit: 85b9a5fc16f4486bc23b4ec8f4fae5ab6211a066
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/05/2021
ms.locfileid: "102194127"
---
# <a name="overview-of-developing-on-windows-with-rust"></a>在 Windows 上通过 Rust 进行开发的概述

入门 [Rust](https://www.rust-lang.org/) 并不难。 如果你是想要使用 Windows 10 学习 Rust 的初学者，建议遵循本分步指南中的每个细节。 它介绍了要安装什么，以及如何创建开发环境。

> [!TIP]
> 如果你已经很热衷于 Rust 并且创建了 Rust 环境，而只是想要开始调用 Windows API，则可以随时跳转到 [Rust for Windows 与 windows crate](rust-for-windows.md) 主题。

## <a name="what-is-rust"></a>什么是 Rust？

Rust 是一种系统编程语言，因此可用于编写系统（如操作系统）。 但它也可用于编写性能和可信度很重要的应用程序。 Rust 语言语法可以与 C++ 语法相媲美，提供了与新式 C++ 相当的性能；对于许多有经验的开发人员来说，Rust 在编译和运行时模型、类型系统和确定性终止化方面都是正确的。

此外，Rust 的设计保证了内存安全，而不需要进行垃圾回收。

那么，我们为什么要选择 Rust 作为 Windows 的最新语言投影呢？ 其中一个因素是，Stack Overflow 的[年度开发人员调查](https://insights.stackoverflow.com/survey)显示，Rust 是目前为止年复一年最受欢迎的编程语言。 虽然你可能会发现此语言有陡峭的学习曲线，但一旦你越过了这个峰，就很难不爱上它了。

此外，Microsoft 还是 [Rust 基金会](https://foundation.rust-lang.org/)的创始成员。 此基金会是一个独立的非营利组织，以一种新的方式来维持和发展大型的、参与式的开放源代码生态系统。

## <a name="the-pieces-of-the-rust-development-toolsetecosystem"></a>Rust 开发工具集/生态系统的组成部分

在这一节中，我们将介绍一些 Rust 工具和术语。 你可以随时再回来查看，以回顾任何相关介绍。

* crate 是 Rust 编译和链接单元。 crate 可以源代码形式存在，然后能够被处理成以二进制可执行文件（简称“二进制文件”）或二进制库（简称“库”）形式存在的 crate。
* Rust 项目称为“包”。 一个包可以包含一个或多个 crate，以及描述如何生成这些 crate 的 `Cargo.toml` 文件。
* `rustup` 是 Rust 工具链的安装程序和更新程序。
* Cargo 是 Rust 包管理工具的名称。
* `rustc` 是 Rust 编译器。 大多数情况下，你不会直接调用 `rustc`，而是通过 Cargo 间接调用它。
* crates.io (`https://crates.io/`) 是 Rust 社区的 crate 注册表。

## <a name="setting-up-your-development-environment"></a>设置开发环境

下一主题将介绍如何[在 Windows 上创建 Rust 开发环境](setup.md)。

## <a name="related"></a>相关内容

* [Rust 网站](https://www.rust-lang.org/)
* [适用于 Windows 的 Rust 与 Windows 包装箱](rust-for-windows.md)
* [Stack Overflow 年度开发人员调查](https://insights.stackoverflow.com/survey)
* [Rust 基金会](https://foundation.rust-lang.org/)
* [crates.io](https://crates.io/)
* [在 Windows 上针对 Rust 设置开发环境](setup.md)
