---
title: IWindowNative 接口
description: WinUI COM 接口，用于提供 XAML 和本机窗口之间的互操作性。
ms.topic: reference
ms.date: 03/09/2021
keywords: winui，Windows UI 库
ms.openlocfilehash: e641ab73a87e993fddb3f6aa8b90a0149744b99f
ms.sourcegitcommit: 7f2a09e8d5d37cb5860a5f2ece5351ea6907b94c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/29/2021
ms.locfileid: "105730635"
---
# <a name="iwindownative-interface-microsoftuixamlwindowh"></a>IWindowNative 接口 (microsoft.ui.xaml.window.h)

实现 XAML 与本机窗口之间的互操作性。

此接口由[窗口](/windows/winui/api/microsoft.ui.xaml.window)实现，桌面应用可使用此接口获取窗口的基础 HWND。

## <a name="inheritance"></a>继承

IWindowNative 接口从 IUnknown 接口继承。 IWindowNative 还具有下列类型的成员：

- [属性](#properties)

## <a name="properties"></a>属性

IWindowNative 接口具有这些属性。

| 属性 | 描述 |
| --- | --- |
| [IWindowNative::WindowHandle](iwindownative-windowhandle.md) | 获取请求的窗口 HWND。 |

## <a name="applies-to"></a>适用于

| 产品 | 版本 |
| --- | --- |
| WinUI | 3.0.0-project-reunion-0.5，3.0.0-project-reunion-preview-0.5 |

## <a name="see-also"></a>另请参阅

[Windows UI 库 (WinUI)](../index.md)