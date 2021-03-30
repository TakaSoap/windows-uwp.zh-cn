---
description: 了解如何在 Windows 应用中实现向后导航以遍历用户的导航历史记录。
title: 导航历史记录和向后导航
template: detail.hbs
op-migration-status: ready
ms.date: 09/24/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 27250869d02c901825af72fe6fb1a3c0cd887e07
ms.sourcegitcommit: 6661f4d564d45ba10e5253864ac01e43b743c560
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104804411"
---
# <a name="navigation-history-and-backwards-navigation-for-windows-apps"></a>Windows 应用的导航历史记录和向后导航

> **重要的 API**：[BackRequested 事件](/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested)、[SystemNavigationManager 类](/uwp/api/Windows.UI.Core.SystemNavigationManager)、[OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto)

Windows 应用提供了一个一致的后退导航系统，用于遍历用户在应用内和应用之间（具体取决于设备）的导航历史记录。

要在应用中实现向后导航，请在应用的 UI 的左上角放置一个后退按钮。 用户预期后退按钮导航到应用导航历史记录中的上一个位置。 请注意，由你来决定要将哪些导航操作添加到导航历史记录以及如何响应后退按钮按下操作。

对于大多数包含多个页面的应用，建议使用 [NavigationView](../controls-and-patterns/navigationview.md) 控件来为应用提供导航框架。 它适应各种屏幕大小，支持顶部和左侧导航样式。 如果你的应用使用了 `NavigationView` 控件，你可以使用 [NavigationView 的内置后退按钮](../controls-and-patterns/navigationview.md#backwards-navigation)。

> [!NOTE]
> 如果实施导航但不使用 `NavigationView` 控件，应使用本文中的指南和示例。 如果使用 `NavigationView`，则可在此信息中了解有用的背景知识，但你应使用 [NavigationView](../controls-and-patterns/navigationview.md) 一文中的特定指南和示例

## <a name="back-button"></a>“后退”按钮

要创建后退按钮，请将[按钮](../controls-and-patterns/buttons.md)控件与 `NavigationBackButtonNormalStyle` 样式结合使用，并将按钮放在应用的 UI 的左上角（有关详细信息，请参阅下面的 XAML 代码示例）。

![应用的 UI 的左上角的后退按钮](images/back-nav/BackEnabled.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>

        <Button x:Name="BackButton"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>

    </Grid>
</Page>
```

如果你的应用有顶部 [CommandBar](../controls-and-patterns/app-bars.md)，高度为 44px 的 Button 控件将无法很好地与 48px AppBarButtons 对齐。 但是，为了避免不一致，请在 48px 边界内对齐 Button 控件的顶部。

![顶部命令栏上的后退按钮](images/back-nav/CommandBar.png)

```xaml
<Page>
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto"/>
            <RowDefinition Height="*"/>
        </Grid.RowDefinitions>
        
        <CommandBar>
            <CommandBar.Content>
                <Button x:Name="BackButton"
                        Style="{StaticResource NavigationBackButtonNormalStyle}"
                        IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                        ToolTipService.ToolTip="Back" 
                        VerticalAlignment="Top"/>
            </CommandBar.Content>
        
            <AppBarButton Icon="Delete" Label="Delete"/>
            <AppBarButton Icon="Save" Label="Save"/>
        </CommandBar>
    </Grid>
</Page>
```

为了将应用中四处移动的 UI 元素最小化，请在 Backstack 中没有任何内容时显示一个禁用的后退按钮 (`IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}"`)。 但是，如果希望应用永不具有 Backstack，则完全无需显示后退按钮。

![后退按钮状态](images/back-nav/BackDisabled.png)

## <a name="optimize-for-different-devices-and-inputs"></a>针对不同设备和输入进行优化

这种向后导航设计指南适用于各种设备，但如果你针对输入的不同外形规格和方法进行优化，这将有利于你的用户。

若要优化 UI：

- 台式机/中心  ：在应用的 UI 的左上角绘制应用内后退按钮。
- **[平板模式](https://support.microsoft.com/windows/use-your-pc-like-a-tablet-4fbfcca5-f058-814a-4f80-a12e703d7c34)** ：平板电脑上可能有硬件或软件后退按钮，但为了清楚起见，建议绘制应用内后退按钮。
- Xbox/电视：请勿绘制后退按钮，因为这会为 UI 增加不必要的杂乱感。 相反，应依靠手柄 B 按钮进行向后导航。

如果你的应用将在 Xbox 上运行，请[创建用于 Xbox 的自定义视觉触发器](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox)来切换按钮的可见性。 如果你使用 [NavigationView](../controls-and-patterns/navigationview.md) 控件，则当你的应用在 Xbox 上运行时，该控件将自动切换后退按钮的可见性。

除了后退按钮 Click 事件，还建议处理以下事件来支持最常见的向后导航输入。

| 事件 | 输入 |
| --- | --- |
| [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) | Alt+左箭头，<br/>VirtualKey.GoBack |
| [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) | Windows + Backspace，<br/>手柄 B 按钮，<br/>平板模式后退按钮，<br/>硬件后退按钮 |
| [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) | VirtualKey.XButton1<br/>（例如在一些鼠标上找到的后退按钮。） |

## <a name="code-examples"></a>代码示例

本部分演示了如何使用各种输入内容来处理向后导航。

### <a name="back-button-and-back-navigation"></a>后退按钮和向后导航

最低要求是，你需要处理后退按钮 `Click` 事件并提供代码来执行向后导航。 当 Backstack 为空时，还应禁用后退按钮。

此示例代码演示了如何使用后退按钮实现向导航行为。 代码会响应按钮 [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) 事件进行导航。 后退按钮是在 [OnNavigatedTo](/uwp/api/windows.ui.xaml.controls.page.onnavigatedto) 方法中启用或禁用的，该方法将在导航到新页面时调用。

此时显示 `MainPage` 的代码，但你要将此代码添加到支持向后导航的每一页上。 若要避免重复，可以在 `App.xaml.*` 代码隐藏页中将导航相关的代码放在 `App` 类中。

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
        <Button x:Name="BackButton" Click="BackButton_Click"
                Style="{StaticResource NavigationBackButtonNormalStyle}"
                IsEnabled="{x:Bind Frame.CanGoBack, Mode=OneWay}" 
                ToolTipService.ToolTip="Back"/>
...
<Page/>
```

代码隐藏：

```csharp
// MainPage.xaml.cs
private void BackButton_Click(object sender, RoutedEventArgs e)
{
    App.TryGoBack();
}

// App.xaml.cs
//
// Add this method to the App class.
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// MainPage.h
namespace winrt::AppName::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();
 
        void MainPage::BackButton_Click(IInspectable const&, RoutedEventArgs const&)
        {
            App::TryGoBack();
        }
    };
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
};
```

### <a name="support-access-keys"></a>支持访问键

为了确保应用程序非常适合具有不同技能、能力和期望的用户，键盘支持是不可或缺的。 建议同时支持向前和向后导航的快捷键，因为依赖这些导航的用户期待两者都有。 有关详细信息，请查看[键盘说明](..\input\keyboard-interactions.md)和[键盘快捷键](..\input\keyboard-accelerators.md)。

向前和向后导航的常见快捷键是 Alt+向右键（向前）和 Alt+向左键（向后）。 若要支持这些导航快捷键，请处理 [CoreDispatcher.AcceleratorKeyActivated](/uwp/api/windows.ui.core.coredispatcher.acceleratorkeyactivated) 事件。 请处理直接在窗口上的事件（而不是页面上的元素），以便无论焦点在哪个元素上，应用都会响应快捷键。

请将代码添加到 `App` 类来支持快捷键和向前导航，如下所示。 （这假定已添加之前支持后退按钮的代码。）可在“代码示例”部分的末尾一并查看所有 `App` 代码。

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // ...

    }
}

// ...

// Add this code after the TryGoBack method added previously.
// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...
    // Add this code after the TryGoBack method added previously.

private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
 
 
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }
};
```

### <a name="handle-system-back-requests"></a>处理系统后退请求

Windows 设备提供了各种方法供系统用于将向后导航请求传递给你的应用。 一些常见的方式是手柄上的 B 按钮、Windows 键 + Backspace 快捷键，或者平板模式中的系统后退按钮；具体可用选项由设备决定。

可通过注册 [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) 事件的侦听器，支持来自硬件和软件系统后退键的系统支持的后退请求。

下面是添加到 `App` 类的代码，它支持系统提供的后退请求。 （这假定已添加之前支持后退按钮的代码。）可在“代码示例”部分的末尾一并查看所有 `App` 代码。

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // ...

    }
}

// ...
// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for accelerator keys. 
        // ... (Previously added code.)

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }
};
```

#### <a name="system-back-behavior-for-backward-compatibility"></a>针对后向兼容性的系统后退行为

以前，UWP 应用使用 [SystemNavigationManager.AppViewBackButtonVisibility](/uwp/api/windows.ui.core.systemnavigationmanager.appviewbackbuttonvisibility) 来显示或隐藏用于向后导航的系统后退按钮。 （此按钮会引发 [SystemNavigationManager.BackRequested](/api/windows.ui.core.systemnavigationmanager.backrequested) 事件。）此 API 将继续获得支持以确保向后兼容性，但我们不再建议使用由 `AppViewBackButtonVisibility` 公开的后退按钮。 相反如本文所述，你应提供自己的应用内后退按钮。

如果继续使用 [AppViewBackButtonVisibility](/uwp/api/windows.ui.core.appviewbackbuttonvisibility)，系统 UI 会在标题栏中呈现系统后退按钮。 （后退按钮的外观和用户交互与以前版本相比并无变化。）

![标题栏后退按钮](images/nav-back-pc.png)

### <a name="handle-mouse-navigation-buttons"></a>处理鼠标导航按钮

某些鼠标为向前和向后导航提供了硬件导航按钮。 可通过处理 [CoreWindow.PointerPressed](/uwp/api/windows.ui.core.corewindow.pointerpressed) 事件，并检查 [IsXButton1Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton1pressed)（向后）或 [IsXButton2Pressed](/uwp/api/windows.ui.input.pointerpointproperties.isxbutton2pressed)（向前），支持这些鼠标按钮。

下面是添加到 `App` 类来支持鼠标按钮导航的代码。 （这假定已添加之前支持后退按钮的代码。）可在“代码示例”部分的末尾一并查看所有 `App` 代码。

```csharp
// App.xaml.cs
// Add event handler in OnLaunced.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}
```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // Add support for system back requests. 
        // ... (Previously added code.)

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
struct App : AppT<App>
{
    App();

    // ...

private:
    // ...

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

### <a name="all-code-added-to-app-class"></a>添加到 App 类的所有代码
 
```csharp
// App.xaml.cs
//
// (Add event handlers in OnLaunched override.)
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // ...
        // rootFrame.NavigationFailed += OnNavigationFailed;

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window.Current.CoreWindow.Dispatcher.AcceleratorKeyActivated +=
            CoreDispatcher_AcceleratorKeyActivated;

        // Add support for system back requests. 
        SystemNavigationManager.GetForCurrentView().BackRequested 
            += System_BackRequested;

        // Add support for mouse navigation buttons. 
        Window.Current.CoreWindow.PointerPressed += CoreWindow_PointerPressed;

        // ...

    }
}

// ...

// (Add these methods to the App class.)
public static bool TryGoBack()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}

// Perform forward navigation if possible.
private bool TryGoForward()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoForward)
    {
        rootFrame.GoForward();
        return true;
    }
    return false;
}

// Invoked on every keystroke, including system keys such as Alt key combinations.
// Used to detect keyboard navigation between pages even when the page itself
// doesn't have focus.
private void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher sender, AcceleratorKeyEventArgs e)
{
    // When Alt+Left are pressed navigate back.
    // When Alt+Right are pressed navigate forward.
    if (e.EventType == CoreAcceleratorKeyEventType.SystemKeyDown
        && (e.VirtualKey == VirtualKey.Left || e.VirtualKey == VirtualKey.Right)
        && e.KeyStatus.IsMenuKeyDown == true
        && !e.Handled)
    {
        if (e.VirtualKey == VirtualKey.Left)
        {
            e.Handled = TryGoBack();
        }
        else if (e.VirtualKey == VirtualKey.Right)
        {
            e.Handled = TryGoForward();
        }
    }
}

// Handle system back requests.
private void System_BackRequested(object sender, BackRequestedEventArgs e)
{
    if (!e.Handled)
    {
        e.Handled = TryGoBack();
    }
}

// Handle mouse back button.
private void CoreWindow_PointerPressed(CoreWindow sender, PointerEventArgs e)
{
    // For this event, e.Handled arrives as 'true', so invert the value.
    if (e.CurrentPoint.Properties.IsXButton1Pressed
        && e.Handled)
    {
        e.Handled = !TryGoBack();
    }
    else if (e.CurrentPoint.Properties.IsXButton2Pressed
            && e.Handled)
    {
        e.Handled = !TryGoForward();
    }
}


```

```cppwinrt
// App.cpp
void App::OnLaunched(LaunchActivatedEventArgs const& e)
{
    // ...
    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == nullptr)
    {
        // ...
        // rootFrame.NavigationFailed({ this, &App::OnNavigationFailed });

        // Add support for accelerator keys. 
        // Listen to the window directly so the app responds
        // to accelerator keys regardless of which element has focus.
        Window::Current().CoreWindow().Dispatcher().
            AcceleratorKeyActivated({ this, &App::CoreDispatcher_AcceleratorKeyActivated });

        // Add support for system back requests. 
        SystemNavigationManager::GetForCurrentView().
            BackRequested({ this, &App::System_BackRequested });

        // Add support for mouse navigation buttons. 
        Window::Current().CoreWindow().
            PointerPressed({ this, &App::CoreWindow_PointerPressed });

        // ...
    }
}

// App.h
#include "winrt/Windows.UI.Core.h"
#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"
 
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Input;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;

struct App : AppT<App>
{
    App();

    // ...

    // Perform back navigation if possible.
    static bool TryGoBack()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoBack())
            {
                rootFrame.GoBack();
                return true;
            }
        }
        return false;
    }
private:
    // Perform forward navigation if possible.
    bool TryGoForward()
    {
        Frame rootFrame{ nullptr };
        auto content = Window::Current().Content();
        if (content)
        {
            rootFrame = content.try_as<Frame>();
            if (rootFrame.CanGoForward())
            {
                rootFrame.GoForward();
                return true;
            }
        }
        return false;
    }
  
    // Invoked on every keystroke, including system keys such as Alt key combinations.
    // Used to detect keyboard navigation between pages even when the page itself
    // doesn't have focus.
    void CoreDispatcher_AcceleratorKeyActivated(CoreDispatcher const& /* sender */, AcceleratorKeyEventArgs const& e)
    {
        // When Alt+Left are pressed navigate back.
        // When Alt+Right are pressed navigate forward.
        if (e.EventType() == CoreAcceleratorKeyEventType::SystemKeyDown
            && (e.VirtualKey() == Windows::System::VirtualKey::Left || e.VirtualKey() == Windows::System::VirtualKey::Right)
            && e.KeyStatus().IsMenuKeyDown
            && !e.Handled())
        {
            if (e.VirtualKey() == Windows::System::VirtualKey::Left)
            {
                e.Handled(TryGoBack());
            }
            else if (e.VirtualKey() == Windows::System::VirtualKey::Right)
            {
                e.Handled(TryGoForward());
            }
        }
    }

    // Handle system back requests.
    void System_BackRequested(IInspectable const& /* sender */, BackRequestedEventArgs const& e)
    {
        if (!e.Handled())
        {
            e.Handled(TryGoBack());
        }
    }

    // Handle mouse forward and back buttons.
    void CoreWindow_PointerPressed(CoreWindow const& /* sender */, PointerEventArgs const& e)
    {
        // For this event, e.Handled arrives as 'true', so invert the value. 
        if (e.CurrentPoint().Properties().IsXButton1Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoBack());
        }
        else if (e.CurrentPoint().Properties().IsXButton2Pressed()
            && e.Handled())
        {
            e.Handled(!TryGoForward());
        }
    }
};
```

## <a name="guidelines-for-custom-back-navigation-behavior"></a>自定义后退导航行为指南

如果你选择提供你自己的后退堆栈导航，则体验应与其他应用保持一致。 我们建议你遵循以下导航操作模式：

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">导航操作</th>
<th align="left">添加到导航历史记录？</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>页面到页面，不同的对等组</strong></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在此图中，用户从应用的级别 1 导航到级别 2，并且跨对等组，因此该导航将添加到导航历史记录。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two and the back to group one." /></p>
<p>在下图中，用户在两个同级别的对等组之间导航，并且再次跨对等组，因此该导航将添加到导航历史记录。</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Diagram of navigation across peer groups showing the user navigating from group one to group two then on to group three and back to group two." /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>页面到页面，同一对等组，无屏幕导航元素</strong>
<p>用户从一个页面导航到同一对等组内的另一个页面。 没有可用于直接导航到两个页面的屏幕导航元素（如 <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>）。</p></td>
<td style="vertical-align:top;"><strong>是</strong>
<p>在下图中，用户在同一对等组中的两个页面之间导航，并且导航应添加到导航历史记录。</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>页面到页面，同一对等组，带有屏幕导航元素</strong>
<p>用户从一个页面导航到同一对等组内的另一个页面。 两个页面显示在相同的导航元素中，如 <a href="/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>。</p></td>
<td style="vertical-align:top;"><strong>视情况而定</strong>
<p>是的，添加到导航历史记录，有两个明显例外。 如果预计应用的用户经常在对等组中的页面之间切换，或者希望保留导航层次结构，则不要添加到导航历史记录。 在这种情况下，当用户按下后退时，将在用户导航到当前对等组之前返回到上一个页面。 </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>显示瞬态 UI</strong>
<p>应用显示弹出窗口或子窗口（例如对话框、初始屏幕或屏幕键盘），或应用进入特殊模式（例如多重选择模式）。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>当用户按下后退按钮时，取消瞬态 UI（隐藏屏幕键盘、取消对话框等）并返回到生成瞬态 UI 的页面。</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>枚举项目</strong>
<p>应用显示屏幕项目的内容，例如列表/细节列表中的选定项目的详细信息。</p></td>
<td style="vertical-align:top;"><strong>否</strong>
<p>枚举项目与在对等组内导航类似。 当用户按下后退时，导航到位于当前页面前面的具有项目枚举的页面。</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>恢复中

当用户切换到其他应用并返回到你的应用时，我们建议返回到导航历史记录中的最后一页。

## <a name="related-articles"></a>相关文章

- [导航基础知识](navigation-basics.md)
