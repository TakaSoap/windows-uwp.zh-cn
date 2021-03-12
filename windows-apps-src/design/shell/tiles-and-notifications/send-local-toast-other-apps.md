---
description: 了解如何从其他类型的未打包应用发送本地 toast 通知，并处理用户单击 toast 的操作。
title: 从其他类型的未打包应用发送本地 toast 通知
ms.assetid: E9AB7156-A29E-4ED7-B286-DA4A6E683638
label: Send a local toast notification from other types of unpackaged apps
template: detail.hbs
ms.date: 11/17/2020
ms.topic: article
keywords: windows 10，发送 toast 通知，通知，发送通知，toast 通知，操作方法，快速入门，入门，代码示例，演练，其他类型的应用，未打包
ms.localizationpriority: medium
ms.openlocfilehash: 71c0facc0cd77383ac6682f4934334a7356eb0e8
ms.sourcegitcommit: 5e718720d1032a7089dea46a7c5aefa6cda3385f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2021
ms.locfileid: "103231536"
---
# <a name="send-a-local-toast-notification-from-other-types-of-unpackaged-apps"></a>从其他类型的未打包应用发送本地 toast 通知

如果正在开发的应用不使用 .MSIX/UWP 或稀疏签名包，并且不是 c # 或 c + +，则可以使用此页！

Toast 通知是用户当前未在应用内部时应用可构造并发送给用户的消息。 本快速入门指导完成创建、传递和显示 Windows 10 toast 通知的步骤。 这些快速入门使用本地通知，这是实现最简单的通知。

> [!IMPORTANT]
> 如果要编写 c # 应用程序，请参阅 [c # 文档](send-local-toast.md)。 如果要编写 c + + 应用程序，请参阅 [c + + UWP](send-local-toast-cpp-uwp.md) 或 [c + + WRL](send-local-toast-desktop-cpp-wrl.md) 文档。



## <a name="step-1-register-your-app-in-the-registry"></a>步骤1：在注册表中注册应用程序

首先需要在注册表中注册应用的信息，包括标识应用的唯一 AUMID、应用的显示名称、图标和 COM 激活器的 GUID。

```xml
<registryKey keyName="HKEY_LOCAL_MACHINE\Software\Classes\AppUserModelId\<YOUR_AUMID>">
    <registryValue
        name="DisplayName"
        value="My App"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconUri"
        value="C:\icon.png"
        valueType="REG_EXPAND_SZ" />
    <registryValue
        name="IconBackgroundColor"
        value="AARRGGBB"
        valueType="REG_SZ" />
    <registryValue
        name="CustomActivator"
        value="{YOUR COM ACTIVATOR GUID HERE}"
        valueType="REG_SZ" />
</registryKey>
```

## <a name="step-2-set-up-your-com-activator"></a>步骤2：设置 COM 激活器

即使在应用程序未运行时，也可以在任意时间点单击通知。 因此，通过 COM 激活器来处理通知激活。 你的 COM 类必须实现 `INotificationActivationCallback` 接口。 COM 类的 GUID 必须与在注册表 CustomActivator 值中指定的 GUID 匹配。

```cpp
struct callback : winrt::implements<callback, INotificationActivationCallback>
{
    HRESULT __stdcall Activate(
        LPCWSTR app,
        LPCWSTR args,
        [[maybe_unused]] NOTIFICATION_USER_INPUT_DATA const* data,
        [[maybe_unused]] ULONG count) noexcept final
    {
        try
        {
            std::wcout << this_app_name << L" has been called back from a notification." << std::endl;
            std::wcout << L"Value of the 'app' parameter is '" << app << L"'." << std::endl;
            std::wcout << L"Value of the 'args' parameter is '" << args << L"'." << std::endl;
            return S_OK;
        }
        catch (...)
        {
            return winrt::to_hresult();
        }
    }
};
```



## <a name="step-3-send-a-toast"></a>步骤3：发送 toast

在 Windows 10 中，你的 toast 通知内容是使用对于你的通知外观给予了最大程度灵活性的自适应语言描述的。 有关详细信息，请参阅 [toast 内容文档](adaptive-interactive-toasts.md)。

我们将从一个简单的基于文本的通知开始。 使用通知库) 构造通知内容 (并显示通知！

> [!IMPORTANT]
> 发送通知时，必须使用 AUMID，以便在应用中显示通知。

<img alt="Simple text notification" src="images/send-toast-01.png" width="364"/>

```cpp
// Construct the toast template
XmlDocument doc;
doc.LoadXml(L"<toast>\
    <visual>\
        <binding template=\"ToastGeneric\">\
            <text></text>\
            <text></text>\
        </binding>\
    </visual>\
</toast>");

// Populate with text and values
doc.SelectSingleNode(L"//text[1]").InnerText(L"Andrew sent you a picture");
doc.SelectSingleNode(L"//text[2]").InnerText(L"Check this out, The Enchantments in Washington!");

// Construct the notification
ToastNotification notif{ doc };

// And send it! Use the AUMID you specified earlier.
ToastNotificationManager::CreateToastNotifier(L"MyPublisher.MyApp").Show(notif);
```


## <a name="step-4-handling-activation"></a>步骤4：处理激活

当你单击通知时，将激活你的 COM 激活器。


## <a name="more-details"></a>更多详细信息

### <a name="aumid-restrictions"></a>AUMID 限制

AUMID 最长为129个字符。 如果 AUMID 超过129个字符，则计划 toast 通知将无法工作-添加计划的通知时，你将收到以下异常： *传递到系统调用的数据区域太小。 (0x8007007A)*。