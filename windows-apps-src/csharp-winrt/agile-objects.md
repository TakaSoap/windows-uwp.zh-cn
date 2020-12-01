---
description: '敏捷对象是可从任何线程访问的对象。 如果需要以安全方式跨单元封送非 agile 对象，c #/WinRT 提供对 agile 引用的支持。'
title: '具有 c #/WinRT 的 Agile 对象'
ms.date: 11/17/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: a963f1a9918e6732028ad74903b096a38a14ec8f
ms.sourcegitcommit: 3b10880007fe9e29ea2b9305fe62ced239d974be
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/30/2020
ms.locfileid: "96355226"
---
# <a name="agile-objects-in-cwinrt"></a>C # 中的 Agile 对象/WinRT

大多数 Windows 运行时类都是 *敏捷* 类，这意味着可以从不同单元中的任何线程访问这些类。 默认情况下，你创作的 c #/WinRT 类型是 agile，因此不可能为这些类型选择退出此行为。

不过，计划的 c #/WinRT 类型 (，其中包括 Windows SDK 和 WinUI 库提供的 Windows 运行时类型) 可能是 agile，也可能不是 agile。 例如，许多表示 UI 对象的类型是不灵活的。 使用非 agile 类型时，需要考虑它们的线程模型和封送处理行为。 如果需要以安全方式跨单元封送非 agile 对象，c #/WinRT 提供对 agile 引用的支持。

> [!NOTE]
> Windows 运行时基于 COM。 在 COM 环境中，注册的敏捷类使用 `ThreadingModel` = Both。 有关 COM 线程模型和单元的详细信息，请参阅[了解和使用 COM 线程模型](/previous-versions/ms809971(v=msdn.10))。

## <a name="check-for-agile-support"></a>检查是否有敏捷支持

若要检查 Windows 运行时对象是否是 agile 对象，请使用以下代码来确定该对象是否支持 [IAgileObject](/windows/desktop/api/objidl/nn-objidl-iagileobject) 接口。

```csharp
var queryAgileObject = testObject.As<IAgileObject>();

if (queryAgileObject != null) {
    // testObject is agile.
}
```

## <a name="create-an-agile-reference"></a>创建敏捷引用

若要为非 agile 对象创建 agile 引用，可以使用 `AsAgile` 扩展方法。 `AsAgile` 是可应用于任何投影的 c #/WinRT 类型的泛型扩展方法。 如果该类型不是投影类型，则会引发异常。 下面是一个使用 [PopupMenu](/uwp/api/Windows.UI.Popups.PopupMenu) 对象的示例，该对象是 Windows SDK 中的一种非 agile 类型。

```csharp
var nonAgileObj = new Windows.UI.Popups.PopupMenu();
AgileReference<Windows.UI.Popups.PopupMenu> agileReference = nonAgileObj.AsAgile();
```

你现在可以传递 `agileReference` 到其他单元中的线程，并将其用于该线程。

```csharp
await Task.Run(() => {
        Windows.UI.Popups.PopupMenu nonAgileObjAgain = agileReference.Get()
    });
```
