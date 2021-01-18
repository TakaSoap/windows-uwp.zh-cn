---
description: InfoBar 是应用范围内重要消息的内联通知。
title: InfoBar
template: detail.hbs
ms.date: 11/30/2020
ms.topic: article
keywords: windows 10, winui, uwp
pm-contact: gabilka
design-contact: kimsea
dev-contact: ranjeshj
ms.custom: 20H2
ms.localizationpriority: medium
ms.openlocfilehash: f790e4ed1d16ac42c95f9a835a3b9cc7f3598190
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104538"
---
# <a name="infobar"></a>InfoBar
InfoBar 控件用于向用户显示应用范围内的状态消息，这些消息是高度可见的，但不具有侵入性。 提供有内置的严重性级别，可轻松指示所显示的消息类型，还提供可包含自己的行动号召或超链接按钮的选项。 由于 InfoBar 与其他 UI 内容是内联的，因此可以选择让控件始终可见或由用户关闭。 

**获取 Windows UI 库**

:::row:::
   :::column:::
      ![WinUI 徽标](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      InfoBar 控件需要 Windows UI 库，该库是一个 NuGet 包，其中包含用于 Windows 应用的新控件和 UI 功能。 有关详细信息（包括安装说明），请参阅 [Windows UI 库](/uwp/toolkits/winui/)。
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

> **Windows UI 库 API：** [InfoBar 类](/uwp/api/microsoft.ui.xaml.controls.infobar)

> [!TIP]
> 在本文档中，我们使用 XAML 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们已将此项添加到我们的[页](/uwp/api/windows.ui.xaml.controls.page)元素：`xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>在后面的代码中，我们还使用 C# 中的 **muxc** 别名表示我们已包含在项目中的 Windows UI 库 API。 我们在文件顶部添加了此 **using** 语句：`using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>这是正确的控件吗？
当用户应该被告知更改的应用程序状态、确认它或对它采取行动时，使用 InfoBar 控件。 默认情况下，通知将保留在内容区域中，直到被用户关闭，但不一定会中断用户流。

InfoBar 将占用布局中的空间，其行为与任何其他子元素类似。 它不会覆盖其他内容，也不会在其顶部浮动。

对于时间敏感警报或非重要消息，不要使用 InfoBar 控件来确认或直接响应不会更改应用状态的用户操作。

### <a name="remarks"></a>注解
对于直接影响应用感知或体验的场景，请使用由用户关闭的或当状态被解决时关闭的 InfoBar。


下面是一些示例：
- Internet 连接丢失
- 自动触发保存文档时出错，与特定的用户操作无关
- 尝试录制时未插入麦克风
- 无法连接到手机
- 对应用程序的订阅已过期


对于间接影响应用感知或体验的场景，请使用由用户关闭的 InfoBar

下面是一些示例：
- 通话已开始录音
- 已应用更新，带有指向“发行说明”的链接
- 服务条款已更新并需要确认
- 应用范围的备份已成功地异步完成
- 对应用程序的订阅即将过期

### <a name="when-should-a-different-control-be-used"></a>何时应该使用不同的控件？

在某些情况下，[ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)、[浮出控件](/uwp/api/Windows.UI.Xaml.Controls.Flyout)或 [TeachingTip](/uwp/api/Microsoft.UI.Xaml.Controls.TeachingTip) 可能更适合使用。

- 对于不需要持久性通知的场景，例如在特定 UI 元素的上下文中显示信息，[浮出控件](/uwp/design/controls-and-patterns/dialogs-and-flyouts/flyouts)是更好的选择。 
- 对于应用程序正在确认用户操作，显示用户必须读取的信息的场景，请使用 [ContentDialog](/uwp/design/controls-and-patterns/dialogs-and-flyouts/dialogs)。
  - 此外，如果应用的状态更改非常严重，以至于需要阻止用户进一步与该应用交互的所有功能，请使用 ContentDialog。
- 对于通知是临时教学片段的场景，[TeachingTip](/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip) 是更好的选择。


有关选择正确通知控件的详细信息，请参阅[对话框和浮出控件](/uwp/design/controls-and-patterns/dialogs-and-flyouts/)一文。


## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 XAML 控件库应用，请单击此处<a href="xamlcontrolsgallery:/item/InfoBar">打开此应用，了解 InfoBar 的实际应用</a><strong style="font-weight: semi-bold"></strong>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-an-infobar"></a>创建 InfoBar
下面的 XAML 描述了具有信息通知默认样式的内联 InfoBar。 信息栏可以像任何其他元素一样放置，并遵循基本布局行为。
例如，在垂直 StackPanel 中，InfoBar 将水平展开以填充可用宽度。 


默认情况下，InfoBar 将不可见。 在 XAML 或代码隐藏中将 IsOpen 属性设置为 true，以显示 InfoBar。


```xaml
<muxc:InfoBar x:Name="UpdateAvailableNotification"
    Title="Update available."
    Message="Restart the application to apply the latest update.">
</muxc:InfoBar>
```

```csharp
public MainPage()
{
    this.InitializeComponent();

    if(IsUpdateAvailable())
    {
        UpdateAvailableNotification.IsOpen = true;
    }
}
```

![具有关闭按钮和消息的默认状态下的 InfoBar 示例](images/infobar-default-title-message.png)

### <a name="using-pre-defined-severity-levels"></a>使用预定义的严重性级别
可以通过 Severity 属性设置信息栏的类型，以根据通知的严重性自动设置一致的状态颜色、图标和辅助技术设置。 如果未设置 Severity，则应用默认的信息样式。


```xaml
<muxc:InfoBar x:Name="SubscriptionExpiringNotification"
    Severity="Warning"
    Title="Your subscription is expiring in 3 days."
    Message="Renew your subscription to keep all functionality" />
```
![带有关闭按钮和消息的处于“警告”状态的 InfoBar 示例](images/infobar-warning-title-message.png)

### <a name="programmatic-close-in-infobar"></a>InfoBar 中的编程关闭
用户可以通过“关闭”按钮或以编程方式关闭 InfoBar。 如果通知在状态被解决之前必须处于可见状态，并且你想要删除用户关闭信息栏的功能，则可以将 IsClosable 属性设置为 false。


此属性的默认值为 true，在这种情况下，存在“关闭”按钮且采用“X”的形式。


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working."
    IsClosable="False" />
```
![处于“错误”状态且没有关闭按钮的 InfoBar 示例](images/infobar-error-no-close.png)

### <a name="customization-background-color-and-icon"></a>自定义：背景色和图标
在预定义的严重性级别之外，可以设置 Background 和 IconSource 属性以自定义图标和背景颜色。 InfoBar 将保留已定义的严重性的辅助技术设置，如果未定义，则保留默认设置。


可以通过标准 Background 属性设置自定义背景颜色，此设置将替代通过 Severity设置的颜色。
在设置自己的颜色时，请注意内容的可读性和可访问性。


可以通过 IconSource 属性设置自定义图标。 默认情况下，图标将是可见的（假设控件没有折叠）。
可以通过将 IsIconVisible 属性设置为 false 来删除此图标。 对于自定义图标，建议的图标大小为 20px。


```xaml
<muxc:InfoBar x:Name="CallRecordingNotification"
    Title="Recording started"
    Message="Your call has begun recording."
    Background="{StaticResource LavenderBackgroundBrush}">
    <muxc:InfoBar.IconSource>
        <muxc:SymbolIconSource Symbol="Phone" />
    </muxc:InfoBar.IconSource>
</muxc:InfoBar>
```

![具有自定义背景颜色、自定义图标和关闭按钮的默认状态下的 InfoBar 示例](images/infobar-custom-icon-color.png)

### <a name="add-an-action-button"></a>添加操作按钮

通过定义自己的按钮（继承 [ButtonBase](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ButtonBase)）并在 ActionButton 属性中设置它，可以添加一个附加的操作按钮。 自定义样式将应用于 [Button](/uwp/api/Windows.UI.Xaml.Controls.Button) 和 [HyperlinkButton](/uwp/api/Windows.UI.Xaml.Controls.HyperlinkButton) 类型的操作按钮，以实现一致性和可访问性。 除了 ActionButton 属性之外，还可以通过自定义内容添加其他操作按钮，这些按钮将显示在消息的下方。


```xaml
<muxc:InfoBar x:Name="NoInternetNotification"
    Severity="Error"
    Title="No Internet"
    Message="Reconnect to continue working.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Network Settings" Click="InternetInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![带有单行消息和操作按钮的“错误”状态的 InfoBar 示例](images/infobar-error-action-button.png)

```xaml
<muxc:InfoBar x:Name="TermsAndConditionsUpdatedNotification"
    Title="Terms and Conditions Updated"
    Message="Dismissal of this message implies agreement to the updated Terms and Conditions. Please view the changes on our website.">
    <muxc:InfoBar.ActionButton>
        <HyperlinkButton Content="Terms and Conditions Sep 2020" 
            NavigateUri="https://www.example.com"
            Click="UpdateInfoBarHyperlinkButton_Click" />
    <muxc:InfoBar.ActionButton />
</muxc:InfoBar>
```
![包含一条展开多行的消息和一个超链接的 InfoBar 示例](images/infobar-default-hyperlink.png)

### <a name="content-wrapping"></a>内容换行
如果 InfoBar 内容（不包括自定义内容）无法容纳在一条水平线上，则它们将垂直布局。 Title、Message 和 ActionButton（如果存在）将分别显示在单独的行中。


```xaml
<muxc:InfoBar x:Name="BackupCompleteNotification"
    Severity="Success"
    Title="Backup complete: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua."  
    Message="All documents were uploaded successfully. Ultrices sagittis orci a scelerisque. Aliquet risus feugiat in ante metus dictum at tempor commodo. Auctor augue mauris augue neque gravida.">
    <muxc:InfoBar.ActionButton>
        <Button Content="Action"
            Click="BackupInfoBarButton_Click" />
    </muxc:InfoBar.ActionButton>
</muxc:InfoBar>
```

![“成功”状态下的 InfoBar 示例，具有多行标题和消息](images/infobar-success-content-wrapping.png)


### <a name="custom-content"></a>自定义内容
可以使用 Content 属性将 XAML 内容添加到 InfoBar。 它将出现在其余控件内容下面自己的行中。 InfoBar 将展开以适应定义的内容。


```xaml
<muxc:InfoBar x:Name="BackupInProgressNotification"
    Title="Backup in progress"  
    Message="Your documents are being saved to the cloud"
    IsClosable="False">
    <muxc:InfoBar.Content>
        <muxc:ProgressBar IsIndeterminate="True" Margin="0,0,0,6" MaxWidth="200"/>
    </muxc:InfoBar.Content>
</muxc:InfoBar>
```

![具有不确定进度条的默认状态的 InfoBar 示例](images/infobar-default-custom-content.gif)

### <a name="lightweight-styling"></a>轻型样式设置

可以修改默认 Style 和 ControlTemplate 以使控件具有唯一的外观。 请参阅 InfoBar API 文档的[控件样式和模板](/windows/winui/api/microsoft.ui.xaml.controls.infobar#control-style-and-template)部分，以查看可用主题资源的列表。
有关详细信息，请参阅[设置控件样式](./xaml-styles.md)一文中的[轻量级样式设置部分](./xaml-styles.md#lightweight-styling)。 

例如，以下内容会使页面上所有信息型 InfoBar 的背景色呈蓝色：

```xaml
<Page.Resources>
    <x:SolidColorBrush x:Key="InfoBarInformationalSeverityBackgroundBrush" Color="LightBlue"></x:SolidColorBrush>
</Page.Resources>
```
### <a name="canceling-close"></a>取消关闭
“Closing”事件可用于取消和/或延迟关闭 InfoBar。 这可以用来使 InfoBar 保持打开状态，或者为自定义操作留出时间。 取消关闭 InfoBar 时，IsOpen 将恢复为 true。 此外，还可取消编程关闭。


```xaml
<muxc:InfoBar x:Name="UpdateAvailable"
    Title="Update Available"
    Message="Please close this tip to apply required security updates to this application"
    Closing="InfoBar_Closing">
</muxc:InfoBar>
```

```csharp
public void InfoBar_Closing(InfoBar sender, InfoBarClosingEventArgs args)
{
    if (args.Reason == InfoBarCloseReason.CloseButton) {
        if (!ApplyUpdates()) {
            // could not apply updates - do not close
            args.Cancel = true;
        }
    }   
}
```

## <a name="recommendations"></a>建议

### <a name="enter-and-exit-usability"></a>进入和退出可用性
#### <a name="flashing-content"></a>内容闪现
InfoBar 不应该在视图中快速出现和消失，以防屏幕上出现闪现。 避免对感光性强的人使用闪现视觉效果，提高应用程序的可用性。


对于通过应用状态条件自动进入和退出视图的 InfoBar，建议在应用程序中包含逻辑，防止内容快速/连续多次地出现或消失。 但是，一般来说，此控件应该用于长期存在的状态消息。


#### <a name="updating-the-infobar"></a>更新 InfoBar
控件打开后，对各种属性所做的任何更改（如更新 Message 或 Severity）都不会引发通知事件。 如果你想要通知使用屏幕阅读器的用户，使其知晓 InfoBar 的更新内容，我们建议你关闭并重新打开该控件来触发事件。


#### <a name="inline-messages-offsetting-content"></a>内联消息偏移内容
对于与其他 UI 内容内联的 InfoBar，请注意页面的其余部分将如何响应式地对元素的添加作出反应。


高度值相当大的 InfoBar 会显著改变页面上其他元素的布局。 如果 InfoBar 快速出现或消失，特别是连续出现，用户可能会对不断变化的视觉状态感到困惑。


### <a name="color-and-icon"></a>颜色和图标
当自定义预设 Severity 级别之外的颜色和图标时，请注意用户对标准图标和颜色集的含义的期望。


此外，预设的 Severity 颜色已针对主题更改、高对比度模式、颜色混淆辅助功能以及与前景颜色的对比度进行了设计。 建议尽可能使用这些颜色，并在应用程序中包含自定义逻辑，以适应各种颜色状态和辅助功能。


请查看[标准图标](/windows/win32/uxguide/vis-std-icons)和[颜色](/windows/win32/uxguide/vis-color)的 UX 指南，以确保你的信息传达清晰，便于用户访问。


#### <a name="severity"></a>严重性
 避免为与 Title、Message 或自定义内容中传递的信息不匹配的通知设置 Severity 属性。
 

 随附信息应旨在传达以下信息以使用该 Severity。
 - 错误：发生了错误或问题。
 - 警告：将来可能会引起问题的情况。
 - 成功：长时间运行的任务和/或后台任务已完成。
 - Default：需要用户注意的一般信息。

图标和颜色不应该是唯一表示通知含义的 UI 组件。 应包括通知的 Title 和/或 Message 中的文本以显示信息。


### <a name="message"></a>Message 

通知中的文本并非在所有语言中都是固定长度的。 对于 Title 和 Message 属性，这可能会影响通知是否会扩展到第二行。 我们建议你避免基于消息长度或设置为特定语言的其他 UI 元素进行定位。

将从右到左 (RTL) 或从左到右 (LTR) 的语言作为本地化的源/目标语言时，通知将遵循标准镜像行为。 图标只有在有方向性的情况下才会进行镜像。

请查看[调整布局和字体，并支持 RTL](/uwp/design/globalizing/adjust-layout-and-fonts--and-support-rtl)的指南，获取有关通知中文本本地化的详细信息。

## <a name="related-articles"></a>相关文章

[对话框和浮出控件](./dialogs-and-flyouts/index.md)