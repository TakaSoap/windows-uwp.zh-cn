---
description: 本主题是与从 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 逐步移植到 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 相关的高级主题。 它说明并行模式库 (PPL) 任务和协同程序如何在同一项目中并存。
title: 实现 C++/WinRT 与 C++/CX 之间的异步和互操作
ms.date: 08/06/2020
ms.topic: article
keywords: windows 10, uwp, 标准, c++, cpp, winrt, 投影, 移植, 迁移, 互操作, C++/CX, PPL, 任务, 协同程序
ms.localizationpriority: medium
ms.openlocfilehash: 1beff7fe5595a2601d56d65b52ca51eacedee47f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157391"
---
# <a name="asynchrony-and-interop-between-cwinrt-and-ccx"></a>实现 C++/WinRT 与 C++/CX 之间的异步和互操作

> [!TIP]
> 尽管我们建议从头开始阅读本主题，但你可以直接跳到[将 C++/CX 异步移植到 C++/WinRT 概述](#overview-of-porting-ccx-async-to-cwinrt)部分中的互操作方法摘要。

本主题是与从 [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) 逐步移植到 [C++/WinRT](./intro-to-using-cpp-with-winrt.md) 相关的高级主题。 本主题紧接[实现 C++/WinRT 与 C++/CX 之间的互操作](./interop-winrt-cx.md)讨论的内容。

如果代码库的大小或复杂性使得有必要逐步移植项目，则需要一个移植过程，在此过程中的某段时间，C++/CX 和 C++/WinRT 代码将在同一项目中并存。 如果你拥有异步代码，则可能需要在逐步移植源代码的过程中，让并行模式库 (PPL) 任务链和协同程序在项目中并存。 本主题重点介绍在 C++/CX 异步代码和 C++/WinRT 异步代码之间互操作的方法。 你可以单独使用这些方法，也可以组合使用这些方法。 利用这些方法，你可以在移植整个项目的过程中逐步进行受控的本地更改，而不会使每项更改在整个项目中不受控制地级联。

在阅读本主题前，最好先阅读[实现 C++/WinRT 与 C++/CX 之间的互操作](./interop-winrt-cx.md)。 该主题演示如何为逐步移植准备项目。 它还引入了两个帮助程序函数，可用于将 C++/CX 对象转换为 C++/WinRT 对象（反之亦然）。 本主题的异步内容基于这些信息，并使用这些帮助程序函数。

> [!NOTE]
> 从 C++/CX 逐步移植到 C++/WinRT 存在一些限制。 如果拥有 [Windows 运行时组件](../winrt-components/create-a-windows-runtime-component-in-cppwinrt.md)项目，则无法逐步移植，需要一次性移植项目。 对于 XAML 项目，无论何时，均要求 XAML 页面类型要么完全是 C++/WinRT，要么完全是 C++/CX。 有关详细信息，请参阅[从 C++/CX 移动到 C++/WinRT](./move-to-winrt-from-cx.md)这一主题。

## <a name="the-reason-an-entire-topic-is-dedicated-to-asynchronous-code-interop"></a>整个主题专门介绍异步代码互操作的原因

从 C++/CX 到 C++/WinRT 的移植通常很简单，但从[并行模式库 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 任务迁移到协同程序的情况例外。 其模型不同。 从 PPL 任务到协同程序没有自然的一对一映射，也没有适用于所有情况的机械移植代码的简单方法。

好消息是，从任务到协同程序的转换大幅简化了这一过程。 开发团队通常会报告说，一旦克服了移植异步代码的障碍，其余的移植工作大部分都是机械式的。

通常情况下，最初是为了适应同步 API 而编写算法。 其随后转换为任务和显式延续 &mdash; 结果通常是对底层逻辑的无意识混淆。 例如，循环变为递归；if-else 分支变成嵌套任务树（任务链）；共享变量变为 shared_ptr。 若要析构通常非自然的 PPL 源代码结构，我们建议先退一步，了解原始代码的意图（即发现原始同步版本）。 然后将 `co_await`（协同等待）插入适当位置。

因此，如果具有要从其开始进行移植的异步代码的 C#（而不是 C++/CX）版本，则可以获得更轻松的体验和更干净的移植。 C# 代码使用 `await`。 因此，C# 代码本质上已遵循从同步版本开始，然后在适当位置插入 `await` 的理念。

如果没有项目的 C# 版本，则可以使用本主题中所述的方法。 移植到 C++/WinRT 后，异步代码的结构将更易于移植到 C#（如果你想这样做）。

## <a name="some-background-in-asynchronous-programming"></a>异步编程的一些背景

既然我们已经有了异步编程概念和术语的常用参考框架，接下来让我们简单设置有关 Windows 运行时异步编程的一般场景，以及两种 C++ 语言投影如何以各自不同的方式在此之上放置。

项目中包含异步工作的方法，主要分为两种。

- 通常，你需要等待异步工作完成，然后才能执行其他操作。 返回异步操作对象的方法即可以等待异步工作完成的方法。
- 但有时你不希望或不需要等待工作异步完成。 在这种情况下，异步方法不返回异步操作对象，会使效率更高。 你无需等待的异步方法称为发后不理 (fire-and-forget) 方法。

### <a name="windows-runtime-async-objects-iasyncxxx"></a>Windows 运行时异步对象 (IAsyncXxx)

**Windows::Foundation** Windows 运行时命名空间包含四种类型的异步操作对象。

- [**IAsyncAction**](/uwp/api/windows.foundation.iasyncaction)；
- [**IAsyncActionWithProgress&lt;TProgress&gt;**](/uwp/api/windows.foundation.iasyncactionwithprogress-1)；
- [**IAsyncOperation&lt;TResult&gt;**](/uwp/api/windows.foundation.iasyncoperation-1)；
- [**IAsyncOperationWithProgress&lt;TResult, TProgress&gt;**](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)。

在本主题中，当我们使用方便的速记形式 IAsyncXxx 时，要么我们在统指这些类型；要么我们在谈论这四种类型之一，但无需具体指定哪一种类型。

### <a name="ccx-async"></a>C++/CX 异步

C++/CX 异步代码使用[并行模式库 (PPL)](/cpp/parallel/concrt/parallel-patterns-library-ppl) 任务。 PPL 任务由 [concurrency::task](/cpp/parallel/concrt/reference/task-class) 类表示。

通常情况下，C++/CX 异步方法通过配合使用 lambda 函数和 [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task) 及 [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then) 来将 PPL 任务链接在一起。 每个 lambda 函数都会返回一个任务，该任务完成时，会生成一个值，该值随后传入任务延续的 lambda 中。

此外，C++/CX 异步方法可以调用 [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) 来创建 IAsyncXxx\^，而不是调用 create_task 来创建任务。

因此，C++/CX 异步方法的返回类型可以是 PPL 任务，也可以是 IAsyncXxx\^。

在任一情况下，方法本身都使用 `return` 关键字返回异步对象，该对象完成时，会生成调用方实际需要的值（可能是文件、字节数组或布尔值）。

> [!NOTE]
> 如果 C++/CX 异步方法返回 IAsyncXxx\^，则 TResult（如果有）限制为 Windows 运行时类型 。 例如，布尔值是 Windows 运行时类型；但 C++/CX 投影类型（例如，Platform::Array<byte>^）不是。

### <a name="cwinrt-async"></a>C++/WinRT 异步

C++/WinRT 将 C++ 协同程序集成到编程模型中。 协同程序和 `co_await` 语句提供一种协同等待结果的自然方法。

每个 IAsyncXxx 类型都投影为 winrt::Windows::Foundation C++/WinRT 命名空间中的相应类型。 我们将其称为 winrt::IAsyncXxx（相对于 C++/CX 的 IAsyncXxx\^） 。

C++/WinRT 协同程序的返回类型为 winrt::IAsyncXxx 或 [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)。 协同程序不使用 `return` 关键字来返回异步对象，而是使用 `co_return` 关键字来协同返回调用方实际需要的值（可能是文件、字节数组或布尔值）。

如果某个方法包含至少一个 `co_await` 语句（或至少一个 `co_return` 或 `co_yield`），则该方法因此属于协同程序。

详细信息和代码示例，请参阅[利用 C++/WinRT 实现的并发和异步运算](./concurrency.md)。

## <a name="the-direct3d-game-sample-simple3dgamedx"></a>Direct3D 游戏示例 (Simple3DGameDX)

本主题包含多个特定编程方法的演练，用于说明如何逐步移植异步代码。 作为案例研究，我们将使用 [Direct3D 游戏示例](/samples/microsoft/windows-universal-samples/simple3dgamedx/)的 C++/CX 版本（称为 Simple3DGameDX）。 我们将演示一些示例，说明如何采用该项目中的原始 C++/CX 源代码并将其异步代码逐步移植到 C++/WinRT。

- 从上述链接下载 ZIP 文件，然后解压缩。
- 在 Visual Studio 中打开 C++/CX 项目（位于名为 `cpp` 的文件夹中）。
- 然后，你需要向项目添加 C++/WinRT 支持。 [采用 C++/CX 项目并添加 C++/WinRT 支持](./interop-winrt-cx.md#taking-a-ccx-project-and-adding-cwinrt-support)中介绍了执行此操作的步骤。 在该部分中，将 `interop_helpers.h` 头文件添加到项目中的步骤尤其重要，因为在本主题中我们将依赖于这些帮助程序函数。
- 最后，将 `#include <pplawait.h>` 添加到 `pch.h` 中。 此操作提供对 PPL 的协同程序支持（下一部分中将详细介绍该支持）。

暂时不要生成，否则你将收到关于 byte 不明确的错误。 解决方法如下。

- 打开 `BasicLoader.cpp` 并注释掉 `using namespace std;`。
- 在同一源代码文件中，随后需要将 shared_ptr 限定为 std::shared_ptr。 为此，你可以在该文件中执行“搜索并替换”操作。
- 然后将 vector 限定为 std::vector，将 string 限定为 std::string。

现在，将再次生成项目，其具有 C++/WinRT 支持，并包含 from_cx 和 to_cx 互操作帮助程序函数。

现在，你已准备好 Simple3DGameDX 项目，可用于本主题中的代码演练。

## <a name="overview-of-porting-ccx-async-to-cwinrt"></a>将 C++/CX 异步移植到 C++/WinRT 概述

简而言之，在移植过程中，我们将把 PPL 任务链更改为对 `co_await` 的调用。 我们将把方法的返回值从 PPL 任务更改为 C++/WinRT winrt::IAsyncXxx 对象。 我们还将把任何 IAsyncXxx\^ 更改为 C++/WinRT winrt::IAsyncXxx 。

你会记得，协同程序是调用 `co_xxx` 的任何方法。 C++/WinRT 协同程序使用 `co_return` 协同返回其值。 借助对 PPL 的协同程序支持（由 `pplawait.h` 提供），还可以使用 `co_return` 从协同程序返回 PPL 任务。 你还可以 `co_await` 任务和 IAsyncXxx。 但不能使用 `co_return` 返回 IAsyncXxx\^。 下表描述对图片中使用 `pplawait.h` 的各种异步方法之间的互操作的支持。

|方法|能否 `co_await` 它？|能否从中 `co_return`？|
|-|-|-|
|方法返回 task\<void\>|是|是|
|方法返回 task\<T\>|否|是|
|方法返回 IAsyncXxx^|是|否。 但是，你可以让 create_async 环绕使用 `co_return` 的任务。|
|方法返回 winrt::IAsyncXxx|是|是|

使用下一个表直接跳至本主题中介绍相关互操作方法的部分，或继续从此处阅读。

|异步互操作方法|本主题中的部分|
|-|-|
|使用 `co_await` 在发后不理方法或构造函数中等待 task\<void\> 方法。|[在发后不理方法中等待 task\<void\>](#await-taskvoid-within-a-fire-and-forget-method)|
|使用 `co_await` 在 task\<void\> 方法中等待 task\<void\> 方法。|[在 task\<void\> 方法中等待 task\<void\>](#await-taskvoid-within-a-taskvoid-method)|
|使用 `co_await` 在 task\<T\> 方法中等待 task\<void\> 方法。|[在 task\<T\> 方法中等待 task\<void\>](#await-taskvoid-within-a-taskt-method)|
|使用 `co_await` 等待 IAsyncXxx^ 方法。|[在 task 方法中等待 IAsyncXxx^，项目其余部分保持不变](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|在 task\<void\> 方法中使用 `co_return`。|[在 task\<void\> 方法中等待 task\<void\>](#await-taskvoid-within-a-taskvoid-method)|
|在 task\<T\> 方法中使用 `co_return`。|[在 task 方法中等待 IAsyncXxx^，项目其余部分保持不变](#await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged)|
|让 create_async 环绕使用 `co_return` 的任务。|[让 create_async 环绕使用 `co_return` 的任务](#wrap-create_async-around-a-task-that-uses-co_return)|
|移植 concurrency::wait。|[将 concurrency::wait 移植到 `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after)|
|返回 winrt::IAsyncXxx，而不是 task\<void\>。|[将 task\<void\> 返回类型移植到 winrt::IAsyncXxx](#port-a-taskvoid-return-type-to-winrtiasyncxxx)|
|将 winrt::IAsyncXxx\<T\>（T 为基元）转换为 task\<T\> 。|[将 winrt::IAsyncXxx\<T\>（T 为基元）转换为 task\<T\>](#convert-a-winrtiasyncxxxt-t-is-primitive-to-a-taskt) |
|将 winrt::IAsyncXxx\<T\>（T 为 Windows 运行时类型）转换为 task\<T^\> 。|[将 winrt::IAsyncXxx\<T\>（T 为 Windows 运行时类型）转换为 task\<T^\>](#convert-a-winrtiasyncxxxt-t-is-a-windows-runtime-type-to-a-taskt) |

以下是一个演示部分支持的简短代码示例。

```cppcx
#include <ppltasks.h>
#include <pplawait.h>
#include <winrt/Windows.Foundation.h>

concurrency::task<bool> TaskAsync()
{
    co_return true;
}

Windows::Foundation::IAsyncOperation<bool>^ IAsyncXxxCppCXAsync()
{
    // co_return true; // Error! Can't do that. But you can do
    // the following.
    return concurrency::create_async([=]() -> concurrency::task<bool> {
        co_return true;
        });
}

winrt::Windows::Foundation::IAsyncOperation<bool> IAsyncXxxCppWinRTAsync()
{
    co_return true;
}

concurrency::task<bool> CppCXAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    co_return co_await IAsyncXxxCppWinRTAsync();
}

winrt::fire_and_forget CppWinRTAsync()
{
    bool b1 = co_await TaskAsync();
    bool b2 = co_await IAsyncXxxCppCXAsync();
    bool b3 = co_await IAsyncXxxCppWinRTAsync();
}
```

> [!IMPORTANT]
> 即使拥有这些出色的互操作选项，逐步移植仍然取决于选择可以准确进行而不影响项目其余部分的更改。 我们希望避免任意抓住一头不放，因此拆解了整个项目的结构。 为此，我们必须按照特定顺序执行操作。 接下来，我们将仔细查看一些进行此类异步相关移植/互操作更改的示例。

## <a name="await-a-taskvoid-method-leaving-the-rest-of-the-project-unchanged"></a>等待 task\<void\>方法，项目其余部分保持不变

返回 task\<void\> 的方法异步执行工作，并返回异步操作对象，但它最终不会生成值。 我们可以 `co_await` 这样的方法。

因此，最好从找到调用此类方法的位置开始逐步移植异步代码。 这些位置将涉及到创建和/或返回任务。 它们还可能涉及到没有任何值从各个任务传递到其延续的任务链。 如我们所见，在类似位置，可以直接将异步代码替换为 `co_await` 语句。

> [!NOTE]
> 随着本主题的深入，你将看到此策略的好处。 通过 `co_await` 专门调用特定 task\<void\> 方法后，你便可以自由地将该方法移植到 C++/WinRT，并使其返回 winrt::IAsyncXxx 。

让我们来看一些示例。 打开 Simple3DGameDX 项目（请参阅 [Direct3D 游戏示例](#the-direct3d-game-sample-simple3dgamedx)）。

> [!IMPORTANT]
> 在下面的示例中，当你看到方法的实现更改时，请记住，我们不需要改变要更改的方法的调用方。 这些更改已本地化，它们不会在项目中级联。

### <a name="await-taskvoid-within-a-fire-and-forget-method"></a>在发后不理方法中等待 task\<void\>

让我们从在发后不理方法中等待 task\<void\> 开始，因为这是最简单的情况。 这些方法是异步工作的方法，但方法的调用方不会等待该工作完成。 尽管实际上是异步完成的，但你只需调用方法并置之不理。

查看项目依赖项关系图的根目录，查找包含 create_task 的 `void` 方法和/或仅调用 task\<void\> 方法的任务链。

在 Simple3DGameDX 中，你将在 GameMain::Update 方法的实现中找到类似代码。 它位于源代码文件 `GameMain.cpp` 中。

#### <a name="gamemainupdate"></a>**GameMain::Update**

以下是方法的 C++/CX 版本的摘录，其中显示方法的两个异步完成的部分。

```cppcx
void GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    case UpdateEngineState::Dynamics:
        ...
        m_game->LoadLevelAsync().then([this]()
        {
            m_game->FinalizeLoadLevel();
            m_updateState = UpdateEngineState::ResourcesLoaded;
        }, task_continuation_context::use_current());
        ...
    ...
}
```

可以看到对 Simple3DGame::LoadLevelAsync 方法（返回 PPL task\<void\>）的调用。 之后是执行一些同步工作的延续。 LoadLevelAsync 是异步的，但它不返回值。 因此，没有任何值从任务传递到延续。

我们可采用相同的方式对这两个位置的代码进行更改。 代码在以下列表之后进行说明。 我们本可在此讨论在类成员协同程序中访问 this 指针的安全方法。 但是，让我们将其推迟到后面的部分进行讨论（[ `co_await` 和 this 稍后进行讨论](#the-deferred-discussion-about-co_await-and-the-this-pointer)）&mdash;目前，此代码可以正常工作。

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    ...
    case UpdateEngineState::WaitingForPress:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    case UpdateEngineState::Dynamics:
        ...
        co_await m_game->LoadLevelAsync();
        m_game->FinalizeLoadLevel();
        m_updateState = UpdateEngineState::ResourcesLoaded;
        ...
    ...
}
```

如你所见，因为 LoadLevelAsync 返回任务，所有我们可以 `co_await` 它。 而且，我们不需要显式延续 &mdash; `co_await` 之后的代码仅在 LoadLevelAsync 完成时执行。

引入 `co_await` 会将方法转换为协同程序，因此不能让它返回 `void`。 这是发后不理方法，因此我们将其更改为返回 [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)。

你还需要编辑 `GameMain.h`。 在此处的声明中将 GameMain::Update 的返回类型从 `void` 更改为 winrt::fire_and_forget 。

可以对项目的副本进行此更改，游戏仍会照样生成并运行。 从根本上说，源代码仍然是 C++/CX，但它现在使用的模式与 C++/WinRT 相同，因此，让我们距离能够机械移植其余代码更近了一步。

#### <a name="gamemainresetgame"></a>**GameMain::ResetGame**

GameMain::ResetGame 是另一种发后不理方法；它也会调用 LoadLevelAsync。 因此，如果你想练习，可在此处更改相同的代码。

#### <a name="gamemainondevicerestored"></a>**GameMain::OnDeviceRestored**

事情在 GameMain::OnDeviceRestored 中变得更加有趣，因为它更深入地嵌套了异步代码，包括无操作任务。 以下是方法的异步部分的概览（使用省略号表示不太相关的同步代码）。

```cppcx
void GameMain::OnDeviceRestored()
{
    ...
    create_task([this]()
    {
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            ...
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ...
    }, task_continuation_context::use_current());
}
```

首先，将 `GameMain.h` 和 `.cpp` 中 GameMain::OnDeviceRestored 的返回类型从 `void` 更改为 winrt::fire_and_forget。 还需要打开 `DeviceResources.h`，并对 IDeviceNotify::OnDeviceRestored 的返回类型进行相同的更改。

若要移植异步代码，请删除所有 create_task 和 then 调用及其大括号，然后将方法简化为一系列简单语句。

将任何 `return`（返回任务）更改为 `co_await`。 你将得到一个 `return`，它不返回任何内容，因此只需删除它即可。 完成后，无操作任务将消失，方法异步部分的概览如下所示。 同样，不太相关的同步代码已省略。

```cppcx
winrt::fire_and_forget GameMain::OnDeviceRestored()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

如你所见，这种形式的异步结构更简单，且更易于阅读。

#### <a name="gamemaingamemain"></a>**GameMain::GameMain**

GameMain::GameMain 构造函数异步执行工作，并且项目的任何部分都不会等待该工作完成。 同样，此列表概要列出异步部分。

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    create_task([this]()
    {
        ...
        return m_renderer->CreateGameDeviceResourcesAsync(m_game);
    }).then([this]()
    {
        ...
        if (m_updateState == UpdateEngineState::WaitingForResources)
        {
            return m_game->LoadLevelAsync().then([this]()
            {
                ...
            }, task_continuation_context::use_current());
        }
        else
        {
            return create_task([]()
            {
                // Return a no-op task.
            });
        }
    }, task_continuation_context::use_current()).then([this]()
    {
        ....
    }, task_continuation_context::use_current());
}
```

但是，构造函数无法返回 winrt::fire_and_forget，因此，我们将异步代码移至新的 GameMain::ConstructInBackground 发后不理方法中，并将代码平展为 `co_await` 语句，然后从构造函数中调用新方法。 下面是结果。

```cppcx
GameMain::GameMain(...) : ...
{
    ...
    ConstructInBackground();
}

winrt::fire_and_forget GameMain::ConstructInBackground()
{
    ...
    co_await m_renderer->CreateGameDeviceResourcesAsync(m_game);
    ...
    if (m_updateState == UpdateEngineState::WaitingForResources)
    {
        ...
        co_await m_game->LoadLevelAsync();
        ...
    }
    ...
}
```

现在，GameMain 中的所有发后不理方法（实际上是所有异步代码）都变成了协同程序。 如果你有这样的倾向，也许可​​以在其他类中寻找发后不理方法，并进行类似更改。

### <a name="the-deferred-discussion-about-co_await-and-the-this-pointer"></a>关于 `co_await` 和 this 指针的推迟讨论

当我们对 GameMain::Update 进行更改时，我推迟了关于 this 指针的讨论。 让我们在这里进行讨论。

这适用于我们到目前为止更改的所有方法；它适用于所有协同程序，而不仅仅是发后不理协同程序。 将 `co_await` 引入方法会引入暂停点。 因此，我们必须小心使用 this 指针，当然，每次访问类成员时，我们都会在暂停点之后使用它。

简而言之，解决方案是调用 [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)。 但是，有关该问题和解决方案的完整讨论，请参阅[在类成员协同程序中安全访问 this 指针](./weak-references.md#safely-accessing-the-this-pointer-in-a-class-member-coroutine)。

你只能在派生自 [winrt::implements](/uwp/cpp-ref-for-winrt/implements) 的类中调用 implements::get_strong。

#### <a name="derive-gamemain-from-winrtimplements"></a>从 winrt::implements 派生 GameMain

我们需要在 `GameMain.h` 中进行第一个更改。

```cppcx
class GameMain :
    public DX::IDeviceNotify
```

GameMain 将继续实现 DX::IDeviceNotify，但我们将其更改为派生自 [winrt::implements](/uwp/cpp-ref-for-winrt/implements)。

```cppwinrt
class GameMain : 
    public winrt::implements<GameMain, winrt::Windows::Foundation::IInspectable>,
    DX::IDeviceNotify
```

接下来，在 `App.cpp` 中，你将找到此方法。

```cppcx
void App::Load(Platform::String^)
{
    if (!m_main)
    {
        m_main = std::unique_ptr<GameMain>(new GameMain(m_deviceResources));
    }
}
```

但是，既然 GameMain 派生自 winrt::implements，我们就需要以不同的方式构造它。 在这种情况下，我们将使用 [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self) 函数模板。 有关详细信息，请参阅[实例化和返回实现类型和接口](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces)。

将上述代码行替换为以下代码行。

```cppwinrt
    ...
    m_main = winrt::make_self<GameMain>(m_deviceResources);
    ...
```

若要关闭该更改的循环，我们还需要更改 m_main 的类型。 在 `App.h` 中，你将找到此代码。

```cppcx
ref class App sealed :
    public Windows::ApplicationModel::Core::IFrameworkView
{
    ...
private:
    ...
    std::unique_ptr<GameMain> m_main;
};
```

将 m_main 的声明作如下更改。

```cppwinrt
    ...
    winrt::com_ptr<GameMain> m_main;
    ...
```

#### <a name="we-can-now-call-implementsget_strong"></a>现在，我们可以调用 implements::get_strong

对于 GameMain::Update，以及我们添加了 `co_await` 的任何其他方法，下面介绍如何在协同程序开始时调用 get_strong，以确保强引用保留到协同程序完成为止。

```cppcx
winrt::fire_and_forget GameMain::Update()
{
    auto strong_this{ get_strong() }; // Keep *this* alive.
    ...
        co_await ...
    ...
}
```

### <a name="await-taskvoid-within-a-taskvoid-method"></a>在 task\<void\> 方法中等待 task\<void\>

下一个最简单的情况是在本身返回 task\<void\> 的方法中等待 task\<void\>。 因为我们可以 `co_await` task\<void\>，并且可以从其 `co_return`。

在 Simple3DGame::LoadLevelAsync 方法的实现中，你会找到一个非常简单的示例。 它位于源代码文件 `Simple3DGame.cpp` 中。

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    return m_renderer->LoadLevelResourcesAsync();
}
```

这只是一些同步代码，然后返回由 GameRenderer::LoadLevelResourcesAsync 创建的任务。

我们不返回该任务，而是 `co_await`，然后 `co_return` 生成的 `void`。

```cppcx
task<void> Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

这似乎不是个重大变化。 但现在，我们通过 `co_await` 调用 GameRenderer::LoadLevelResourcesAsync 后，就可以自由对其进行移植，以返回 winrt::IAsyncXxx 而不是任务 。 稍后我们将在[将 task\<void\> 返回类型移植到 winrt::IAsyncXxx](#port-a-taskvoid-return-type-to-winrtiasyncxxx) 部分中执行此操作 。

### <a name="await-taskvoid-within-a-taskt-method"></a>在 task\<T\> 方法中等待 task\<void\>

尽管在 Simple3DGameDX 中找不到合适的示例，但我们可以设计一个仅用于演示模式的假设示例。

以下代码示例中的第一行演示 task\<void\> 的简单 `co_await`。 然后，为了满足 task\<T\> 返回类型的要求，我们需要以异步方式返回 StorageFile\^ 。 为此，我们需要 `co_await` Windows 运行时 API，并 `co_return` 生成文件。

```cppcx
task<StorageFile^> Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder^ location,
    Platform::String^ filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location->GetFileAsync(filename);
}
```

我们甚至可采用相同的方式将更多方法移植到 C++/WinRT。

```cppcx
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
Simple3DGame::LoadLevelAndRetrieveFileAsync(
    StorageFolder location,
    std::wstring filename)
{
    co_await m_renderer->LoadLevelResourcesAsync();
    co_return co_await location.GetFileAsync(filename);
}
```

在该示例中，m_renderer 数据成员仍为 C++/CX。

## <a name="await-an-iasyncxxx-in-a-task-method-leaving-the-rest-of-the-project-unchanged"></a>在 task 方法中等待 IAsyncXxx^，项目其余部分保持不变

我们已了解可以如何 `co_await` task\<void\>。 还可以 `co_await` 返回 IAsyncXxx 的方法，无论这是项目中的方法还是异步 Windows API（例如我们在上一部分中协同等待的 [StorageFolder.GetFileAsync](/uwp/api/windows.storage.storagefolder.getfileasync)）。

有关进行此类代码更改的示例，请查看 BasicReaderWriter::ReadDataAsync（你将发现它在 `BasicReaderWriter.cpp` 中实现）。

以下是原始 C++/CX 版本。

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

以下代码列表显示我们可以 `co_await` 返回 IAsyncXxx^ 的 Windows API。 不仅如此，我们还可以 `co_return` BasicReaderWriter::ReadDataAsync 异步返回的值（在本例中为字节数组）。 第一步演示如何进行这些更改；我们将在下一部分中实际将 C++/CX 代码移植到 C++/WinRT。

```cppcx
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
)
{
    StorageFile^ file = co_await m_location->GetFileAsync(filename);
    IBuffer^ buffer = co_await FileIO::ReadBufferAsync(file);
    auto fileData = ref new Platform::Array<byte>(buffer->Length);
    DataReader::FromBuffer(buffer)->ReadBytes(fileData);
    co_return fileData;
}
```

同样，我们不需要改变要更改的方法的调用方，因为我们没有更改返回类型。

### <a name="port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged"></a>将 ReadDataAsync（大部分）移植到 C++/WinRT，项目其余部分保持不变

我们可以更进一步，将方法几乎完全移植到 C++/WinRT，而无需更改项目的任何其他部分。

此方法对项目其余部分的唯一依赖项是 BasicReaderWriter::m_location 数据成员，它是 C++/CX StorageFolder^。 若要使该数据成员保持不变，并使参数类型和返回类型保持不变，我们只需要执行几次转换即可 &mdash; 一次在方法开始时，一次在方法结束时。 为此，我们可以使用 from_cx 和 to_cx 互操作帮助程序函数。

BasicReaderWriter::ReadDataAsync 在将其实现基本移植到 C++/WinRT 后的外观如下所示。 这是逐步移植的良好示例。 此方法处于我们可以不再将其视为使用某些 C++/WinRT 方法的 C++/CX 方法，而是将其视为与 C++/CX 互操作的 C++/WinRT 方法的阶段。

```cppwinrt
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.Storage.Streams.h>
#include <robuffer.h>
...
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

> [!NOTE]
> 在上述 ReadDataAsync 中，我们构造并返回了新的 C++/CX 数组。 当然，我们这样做是为了满足该方法的返回类型的要求（这样就不必更改项目的其余部分）。
>
> 你可能会在自己的项目中遇到其他示例，在这些示例中，你在移植后到达方法的结尾，但仅拥有 C++/WinRT 对象。 若要 `co_return` 它，只需调用 to_cx 进行转换即可。 有关此内容的详细信息和示例，请参阅下一部分。

## <a name="convert-a-winrtiasyncxxxt-to-a-taskt"></a>将 winrt::IAsyncXxx\<T\> 转换为 task\<T\> 

本部分将介绍以下情境：将异步方法移植到 C++/WinRT（以便它返回 winrt::IAsyncXxx\<T\>），但 C++/CX 代码仍调用该方法，就像它仍返回任务一样。

- 一种情况是 T 为基元，不需要进行任何转换。
- 另一种情况是 T 为 Windows 运行时类型，在这种情况下，需要将其转换为 T^ 。

### <a name="convert-a-winrtiasyncxxxt-t-is-primitive-to-a-taskt"></a>将 winrt::IAsyncXxx\<T\>（T 为基元）转换为 task\<T\> 

在异步返回基元值时，适用此部分中的模式（我们将使用布尔值来进行说明）。 假设移植到 C++/WinRT 的方法具有此签名。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<bool>
MyClass::GetBoolMemberFunctionAsync()
{
    bool value = ...
    co_return value;
}
```

你可以将对此方法的调用转换为如下任务。

```cppcx
task<bool> MyClass::RetrieveBoolTask()
{
    co_return co_await GetBoolMemberFunctionAsync();
}
```

或如下任务。

```cppcx
task<bool> MyClass::RetrieveBoolTask()
{
    return concurrency::create_task(
        [this]() -> concurrency::task<bool> {
            auto result = co_await GetBoolMemberFunctionAsync();
            co_return result;
        });
}
```

请注意，lambda 函数的任务返回类型是显式的，因为编译器无法推导它。

我们还可以从任意任务链中调用方法，如下所示。 同样，使用显式 lambda 返回类型。

```cppcx
...
.then([this]() -> concurrency::task<bool> {
    co_return co_await GetBoolMemberFunctionAsync();
}).then([this](bool result) {
    ...
});
...
```

### <a name="convert-a-winrtiasyncxxxt-t-is-a-windows-runtime-type-to-a-taskt"></a>将 winrt::IAsyncXxx\<T\>（T 为 Windows 运行时类型）转换为 task\<T^\> 

在异步返回 Windows 运行时值时，适用此部分中的模式（我们将使用 StorageFile 值来进行说明）。 假设移植到 C++/WinRT 的方法具有此签名。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::StorageFile>
MyClass::GetStorageFileMemberFunctionAsync()
{
    co_return co_await winrt::Windows::Storage::StorageFile::GetFileFromPathAsync
    (L"MyFile.txt");
}
```

下一列表显示如何将对此方法的调用转换为任务。 注意，我们需要调用 to_cx 互操作帮助程序函数，才能将返回的 C++/WinRT 对象转换为 C++/CX 句柄（也称为 hat）对象。

```cppcx
task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    winrt::Windows::Storage::StorageFile storageFile =
        co_await GetStorageFileMemberFunctionAsync();
    co_return to_cx<Windows::Storage::StorageFile>(storageFile);
}
```

下面是一个更简洁的版本。

```cppcx
task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    co_return to_cx<Windows::Storage::StorageFile>(GetStorageFileMemberFunctionAsync());
}
```

你甚至可以选择将该模式包装为可重复使用的函数模板，并像通常返回任务那样 `return` 它。

```cppcx
template<typename ResultTypeCX, typename Awaitable>
concurrency::task<ResultTypeCX^> to_task(Awaitable awaitable)
{
    co_return to_cx<ResultTypeCX>(co_await awaitable);
}

task<Windows::Storage::StorageFile^> RetrieveStorageFileTask()
{
    return to_task<Windows::Storage::StorageFile>(GetStorageFileMemberFunctionAsync());
}
```

如果你喜欢此想法，我们建议你将 to_task 添加到 `interop_helpers.h`。

## <a name="wrap-create_async-around-a-task-that-uses-co_return"></a>让 create_async 环绕使用 `co_return` 的任务

无法直接 `co_return` IAsyncXxx\^，但可以实现类似目的。 如果你有一个协同返回值的任务，可以将该任务包装在对 [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async) 的调用中。

以下是一个假设示例，因为没有可以从 Simple3DGameDX 中提取的示例。

```cppcx
Windows::Foundation::IAsyncOperation<bool>^ MyClass::RetrieveBoolAsync()
{
    return concurrency::create_async(
        [this]() -> concurrency::task<bool> {
            bool result = co_await GetBoolMemberFunctionAsync();
            co_return result;
        });
}
```

如你所见，可通过可以 `co_await` 的任何方法获得返回值。

## <a name="port-concurrencywait-to-co_await-winrtresume_after"></a>将 concurrency::wait 移植到 `co_await winrt::resume_after`

在几个位置，Simple3DGameDX 使用 [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait) 将线程短时间暂停。 下面是一个示例。

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int InitialLoadingDelay = 2000;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]()
    {
        wait(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

concurrency::wait 的 C++/WinRT 版本为 winrt::resume_after 结构。 我们可以在 PPL 任务中 `co_await` 该结构。 下面是代码示例。

```
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto InitialLoadingDelay = 2000ms;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::CreateGameDeviceResourcesAsync(_In_ Simple3DGame^ game)
{
    std::vector<task<void>> tasks;
    ...
    tasks.push_back(create_task([]() -> task<void>
    {
        co_await winrt::resume_after(GameConstants::InitialLoadingDelay);
    }));
    ...
}
```

请注意我们必须进行的另外两个更改。 我们将 GameConstants::InitialLoadingDelay 的类型更改为 std::chrono::duration，并明确指明 lambda 函数的返回类型，因为编译器不再能够推导它。

## <a name="port-a-taskvoid-return-type-to-winrtiasyncxxx"></a>将 task\<void\> 返回类型移植到 winrt::IAsyncXxx

### <a name="simple3dgameloadlevelasync"></a>**Simple3DGame::LoadLevelAsync**

在使用 Simple3DGameDX 的这一阶段，项目中所有调用 Simple3DGame::LoadLevelAsync 的位置都使用 `co_await` 来调用它。

这意味着只需将该方法的返回类型从 task\<void\> 更改为 winrt::Windows::Foundation::IAsyncAction （保持其余部分不变）。

```cppcx
winrt::Windows::Foundation::IAsyncAction Simple3DGame::LoadLevelAsync()
{
    m_level[m_currentLevel]->Initialize(m_objects);
    m_levelDuration = m_level[m_currentLevel]->TimeLimit() + m_levelBonusTime;
    co_return co_await m_renderer->LoadLevelResourcesAsync();
}
```

现在应该能够通过相当机械的方式将该方法的其余部分及其依赖项（例如 m_level 等）移植到 C++/WinRT。

### <a name="gamerendererloadlevelresourcesasync"></a>**GameRenderer::LoadLevelResourcesAsync**

以下是 GameRenderer::LoadLevelResourcesAsync 的原始 C++/CX 版本。

```cppcx
// GameConstants.h
namespace GameConstants
{
    ...
    static const int LevelLoadingDelay = 500;
    ...
}

// GameRenderer.cpp
task<void> GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;

    return create_task([this]()
    {
        wait(GameConstants::LevelLoadingDelay);
    });
}
```

Simple3DGame::LoadLevelAsync 是项目中唯一调用 GameRenderer::LoadLevelResourcesAsync 的位置，并且它已经使用 `co_await` 来调用它。

因此，不再需要 GameRenderer::LoadLevelResourcesAsync 返回任务 &mdash; 它可以改为返回 winrt::Windows::Foundation::IAsyncAction。 实现本身足够简单，可以完全移植到 C++/WinRT。 这涉及进行我们在[将 concurrency::wait 移植到 `co_await winrt::resume_after`](#port-concurrencywait-to-co_await-winrtresume_after) 中进行的相同更改。 对于项目的其余部分，没有任何需要担心的重要依赖项。

因此，方法在完全移植到 C++/WinRT 之后的外观如下所示。

```cppwinrt
// GameConstants.h
namespace GameConstants
{
    using namespace std::literals::chrono_literals;
    ...
    static const auto LevelLoadingDelay = 500ms;
    ...
}

// GameRenderer.cpp
winrt::Windows::Foundation::IAsyncAction GameRenderer::LoadLevelResourcesAsync()
{
    m_levelResourcesLoaded = false;
    co_return co_await winrt::resume_after(GameConstants::LevelLoadingDelay);
}
```

### <a name="the-goalmdashfully-port-a-method-to-cwinrt"></a>目标&mdash;将方法完全移植到 C++/WinRT

让我们通过将方法 BasicReaderWriter::ReadDataAsync 完全移植到 C++/WinRT，以一个最终目标示例结束本演练。

我们上次查看此方法时（在[将 ReadDataAsync（大部分）移植到 C++/WinRT，项目其余部分保持不变](#port-readdataasync-mostly-to-cwinrt-leaving-the-rest-of-the-project-unchanged)部分中），大部分都已移植到 C++/WinRT 中。 但它仍然返回 Platform::Array\<byte\>^ 的任务。

```cppwinrt
task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename)
{
    auto location_from_cx = from_cx<winrt::Windows::Storage::StorageFolder>(m_location);

    auto file = co_await location_from_cx.GetFileAsync(filename->Data());
    auto buffer = co_await winrt::Windows::Storage::FileIO::ReadBufferAsync(file);
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));

    co_return ref new Platform::Array<byte>(bytes, buffer.Length());
}
```

我们会将其更改为返回 IAsyncOperation，而不是返回任务。 我们将返回 C++/WinRT [IBuffer](/uwp/api/windows.storage.streams.ibuffer) 对象，而不是通过该 IAsyncOperation 返回字节数组 。 我们将看到，此操作还需要对调用站点上的代码进行少许更改。

在移植其实现、其参数和 m_location 数据成员以使用 C++/WinRT 语法和对象之后，方法的外观如下所示。

```cppwinrt
winrt::Windows::Foundation::IAsyncOperation<winrt::Windows::Storage::Streams::IBuffer>
BasicReaderWriter::ReadDataAsync(
    _In_ winrt::hstring const& filename)
{
    StorageFile file{ co_await m_location.GetFileAsync(filename) };
    co_return co_await FileIO::ReadBufferAsync(file);
}

winrt::array_view<byte> BasicLoader::GetBufferView(
    winrt::Windows::Storage::Streams::IBuffer const& buffer)
{
    byte* bytes;
    auto byteAccess = buffer.as<Windows::Storage::Streams::IBufferByteAccess>();
    winrt::check_hresult(byteAccess->Buffer(&bytes));
    return { bytes, bytes + buffer.Length() };
}
```

如你所见，BasicReaderWriter::ReadDataAsync 本身要简单得多，因为我们已将从缓冲区中检索字节的同步逻辑纳入其自己的方法。

但现在我们需要从 C++/CX 的此类结构中移植调用站点。

```cppcx
task<void> BasicLoader::LoadTextureAsync(...)
{
    return m_basicReaderWriter->ReadDataAsync(filename).then(
        [=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(...);
    });
}
```

C++/WinRT 中的这种模式。

```cppwinrt
winrt::Windows::Foundation::IAsyncAction BasicLoader::LoadTextureAsync(...)
{
    auto textureBuffer = co_await m_basicReaderWriter.ReadDataAsync(filename);
    auto textureData = GetBufferView(textureBuffer);
    CreateTexture(...);
}
```

## <a name="important-apis"></a>重要的 API

* [IAsyncAction](/uwp/api/windows.foundation.iasyncaction)
* [IAsyncActionWithProgress&lt;TProgress&gt;](/uwp/api/windows.foundation.iasyncactionwithprogress-1)
* [IAsyncOperation&lt;TResult&gt;](/uwp/api/windows.foundation.iasyncoperation-1)
* [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](/uwp/api/windows.foundation.iasyncoperationwithprogress-2)
* [implements::get_strong](/uwp/cpp-ref-for-winrt/implements#implementsget_strong-function)
* [concurrency::create_async](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_async)
* [concurrency::create_task](/cpp/parallel/concrt/reference/concurrency-namespace-functions#create_task)
* [concurrency::task](/cpp/parallel/concrt/reference/task-class)
* [concurrency::task::then](/cpp/parallel/concrt/reference/task-class#then)
* [concurrency::wait](/cpp/parallel/concrt/reference/concurrency-namespace-functions#wait)
* [winrt::fire_and_forget](/uwp/cpp-ref-for-winrt/fire-and-forget)
* [winrt::make_self](/uwp/cpp-ref-for-winrt/make-self)

## <a name="related-topics"></a>相关主题

* [从 C++/CX 移动到 C++/WinRT](./move-to-winrt-from-cx.md)
* [实现 C++/WinRT 与 C++/CX 之间的互操作](./interop-winrt-cx.md)
* [利用 C++/WinRT 实现的并发和异步操作](./concurrency.md)
* [C++/WinRT 中的弱引用](./weak-references.md)
* [使用 C++/WinRT 创作 API](./author-apis.md)