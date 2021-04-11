---
description: Web 视图控件将一个视图嵌入你的应用中，以便使用 Microsoft Edge 呈现引擎来呈现 Web 内容。 超链接也可以在 Web 视图控件中显示并正常工作。
title: Web 视图
ms.assetid: D3CFD438-F9D6-4B72-AF1D-16EF2DFC1BB1
label: Web view
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3bab93eca2318d7253df5acb16d866ac81d8ae6c
ms.sourcegitcommit: 4ea59d5d18f79800410e1ebde28f97dd5e45eb26
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101824431"
---
# <a name="web-view"></a>Web 视图

Web 视图控件将一个视图嵌入你的应用中，以便使用 Microsoft Edge 呈现引擎来呈现 Web 内容。 超链接也可以在 Web 视图控件中显示并正常工作。

> **重要 API**：[WebView 类](/uwp/api/Windows.UI.Xaml.Controls.WebView)

## <a name="is-this-the-right-control"></a>这是合适的控件吗？

使用 Web 视图控件从远程 Web 服务器、动态生成的代码或者应用程序包中的内容文件显示格式丰富的 HTML 内容。 丰富的内容还可以包含脚本代码，并在脚本和你的应用代码之间通信。

## <a name="examples"></a>示例

<table>
<th align="left">XAML 控件库<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>如果已安装 <strong style="font-weight: semi-bold">XAML 控件库</strong>应用，请单击此处<a href="xamlcontrolsgallery:/item/WebView">打开此应用，了解 WebView 的实际应用</a>。</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">获取 XAML 控件库应用 (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">获取源代码 (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="create-a-web-view"></a>创建 Web 视图

**修改 Web 视图的外观**

[WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 不是 [Control](/uwp/api/Windows.UI.Xaml.Controls.Control) 子类，因此它不具有控件模板。 但是，你可以设置各种属性来控制 Web 视图的某些可视部分。
- 若要限制显示区域，请设置 [Width](/uwp/api/windows.ui.xaml.frameworkelement.width) 和 [Height](/uwp/api/windows.ui.xaml.frameworkelement.height) 属性。 
- 若要转换、缩放、扭曲和旋转 Web 视图，请使用 [RenderTransform](/uwp/api/windows.ui.xaml.uielement.rendertransform) 属性。
- 若要控制 Web 视图的不透明度，请设置 [Opacity](/uwp/api/windows.ui.xaml.uielement.opacity) 属性。
- 若要在 HTML 内容不指定颜色的情况下指定一种用作 Web 页面背景的颜色，请设置 [DefaultBackgroundColor](/uwp/api/windows.ui.xaml.controls.webview.defaultbackgroundcolor) 属性。 

**获取 Web 页面标题**

你可以使用 [DocumentTitle](/uwp/api/windows.ui.xaml.controls.webview.documenttitle) 属性获取 Web 视图中当前显示的 HTML 文档标题。 

**输入事件和 Tab 键顺序**

尽管 WebView 不是 Control 子类，但它可接收键盘输入焦点，并加入 Tab 键序列。 它提供 [Focus](/uwp/api/windows.ui.xaml.controls.webview.focus) 方法，以及 [GotFocus](/uwp/api/windows.ui.xaml.uielement.gotfocus) 和 [LostFocus](/uwp/api/windows.ui.xaml.uielement.lostfocus) 事件，但它不具有 Tab 键相关属性。 它在 Tab 键序列中的位置与在 XAML 文档顺序中的位置相同。 Tab 键序列包括 Web 视图内容中的所有元素，这些元素可以接收输入焦点。 

如 [WebView](/uwp/api/Windows.UI.Xaml.Controls.WebView) 类页上的“事件”表中所示，Web 视图不支持继承自 [UIElement](/uwp/api/Windows.UI.Xaml.UIElement) 的大部分用户输入事件，如 [KeyDown](/uwp/api/windows.ui.xaml.uielement.keydown)、[KeyUp](/uwp/api/windows.ui.xaml.uielement.keyup) 和 [PointerPressed](/uwp/api/windows.ui.xaml.uielement.pointerpressed)。 可以改为将 [InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) 与 JavaScript **eval** 函数结合使用来使用 HTML 事件处理程序，并通过 HTML 事件处理程序中的 **window.external.notify** 以使用 [WebView.ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) 通知应用程序。

### <a name="navigating-to-content"></a>导航到内容

Web 视图提供多个 API 以进行基本导航：[GoBack](/uwp/api/windows.ui.xaml.controls.webview.goback)、[GoForward](/uwp/api/windows.ui.xaml.controls.webview.goforward)、[Stop](/uwp/api/windows.ui.xaml.controls.webview.stop)、[Refresh](/uwp/api/windows.ui.xaml.controls.webview.refresh)、[CanGoBack](/uwp/api/windows.ui.xaml.controls.webview.cangoback) 和 [CanGoForward](/uwp/api/windows.ui.xaml.controls.webview.cangoforward)。 这些 API 可用于向你的应用添加典型的 Web 浏览功能。 

若要设置 Web 视图的初始内容，请设置 XAML 中的 [Source](/uwp/api/windows.ui.xaml.controls.webview.source) 属性。 XAML 分析程序会自动将字符串转换为 [URI](/uwp/api/Windows.Foundation.Uri)。 

```xaml
<!-- Source file is on the web. -->
<WebView x:Name="webView1" Source="http://www.contoso.com"/>

<!-- Source file is in local storage. -->
<WebView x:Name="webView2" Source="ms-appdata:///local/intro/welcome.html"/>

<!-- Source file is in the app package. -->
<WebView x:Name="webView3" Source="ms-appx-web:///help/about.html"/>
```

可以使用代码设置 Source 属性，但若不这样做，通常可以在代码中使用其中一个 Navigate 方法来加载内容。 

若要加载 Web 内容，请将 [Navigate](/uwp/api/windows.ui.xaml.controls.webview.navigate) 方法与使用 http 或 https 方案的 **Uri** 结合使用。 

```csharp
webView1.Navigate("http://www.contoso.com");
```

若要使用 POST 请求和 HTTP 标头导航到 URI，请使用 [NavigateWithHttpRequestMessage](/uwp/api/windows.ui.xaml.controls.webview.navigatewithhttprequestmessage) 方法。 此方法仅支持针对 [HttpRequestMessage.Method](/uwp/api/windows.web.http.httprequestmessage.method) 属性值的 [HttpMethod.Post](/uwp/api/windows.web.http.httpmethod.post) 和 [HttpMethod.Get](/uwp/api/windows.web.http.httpmethod.get)。 

若要加载应用的 [LocalFolder](/uwp/api/windows.storage.applicationdata.localfolder) 或 [TemporaryFolder](/uwp/api/windows.storage.applicationdata.temporaryfolder) 数据存储中的未压缩和未加密内容，请将 **Navigate** 方法与使用 [ms-appdata scheme](../../app-resources/uri-schemes.md) 的 **Uri** 结合使用。 Web 视图对此方案的支持要求你将子文件夹中的内容置于本地文件夹或临时文件夹下。 这样便可以导航到 URI，例如 ms-appdata:///local/folder/file.html 和 ms-appdata:///temp/folder/file.html   。 （若要加载压缩或加密文件，请参阅 [NavigateToLocalStreamUri](/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri)。） 

其中的每个第一级别的子文件夹独立于其他第一级别子文件夹中的内容。 例如，可以导航到 ms-appdata:///temp/folder1/file.html，但在此文件中不能链接到 ms-appdata:///temp/folder2/file.html。 但仍可以使用 ms-appx-web scheme 链接到应用程序包中的 HTML 内容，也可以使用 http 和 https URI 方案链接到 Web 内容。

```csharp
webView1.Navigate("ms-appdata:///local/intro/welcome.html");
```

若要加载来自应用程序包中内容，请将 **Navigate** 方法与使用 [ms-appx-web scheme](/previous-versions/windows/apps/jj655406(v=win.10)) 的 **Uri** 结合使用。 

```csharp
webView1.Navigate("ms-appx-web:///help/about.html");
```

使用 [NavigateToLocalStreamUri](/uwp/api/windows.ui.xaml.controls.webview.navigatetolocalstreamuri) 方法，通过自定义解析程序加载本地内容。 这样既可以支持高级方案（如下载和缓存基于 Web 的内容供脱机使用），也可以支持从压缩文件中提取内容。

### <a name="responding-to-navigation-events"></a>响应导航事件

Web 视图控件提供的多个事件可用于响应导航和内容加载状态。 对于根 Web 视图内容，事件发生顺序如下所示：[NavigationStarting](/uwp/api/windows.ui.xaml.controls.webview.navigationstarting)、[ContentLoading](/uwp/api/windows.ui.xaml.controls.webview.contentloading)、[DOMContentLoaded](/uwp/api/windows.ui.xaml.controls.webview.domcontentloaded)、[NavigationCompleted](/uwp/api/windows.ui.xaml.controls.webview.navigationcompleted)


**NavigationStarting** - 在 Web 视图导航到新内容之前发生。 通过将 WebViewNavigationStartingEventArgs.Cancel 属性设置为 true，可以在处理程序中取消针对此事件的导航。 

```csharp
webView1.NavigationStarting += webView1_NavigationStarting;

private void webView1_NavigationStarting(object sender, WebViewNavigationStartingEventArgs args)
{
    // Cancel navigation if URL is not allowed. (Implemetation of IsAllowedUri not shown.)
    if (!IsAllowedUri(args.Uri))
        args.Cancel = true;
}
```

**ContentLoading** - 在 Web 视图已开始加载新内容时发生。 

```csharp
webView1.ContentLoading += webView1_ContentLoading;

private void webView1_ContentLoading(WebView sender, WebViewContentLoadingEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Loading content for " + args.Uri.ToString();
    }
}
```

**DOMContentLoaded** - 在 Web 视图已完成当前 HTML 内容分析时发生。 

```csharp
webView1.DOMContentLoaded += webView1_DOMContentLoaded;

private void webView1_DOMContentLoaded(WebView sender, WebViewDOMContentLoadedEventArgs args)
{
    // Show status.
    if (args.Uri != null)
    {
        statusTextBlock.Text = "Content for " + args.Uri.ToString() + " has finished loading";
    }
}
```

**NavigationCompleted** - 在 Web 视图已完成当前内容加载或者导航失败时发生。 若要确定导航是否已失败，请检查 [WebViewNavigationCompletedEventArgs](/uwp/api/Windows.UI.Xaml.Controls.WebViewNavigationCompletedEventArgs) 类的 [IsSuccess](/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.issuccess) 和 [WebErrorStatus](/uwp/api/windows.ui.xaml.controls.webviewnavigationcompletedeventargs.weberrorstatus) 属性。 

```csharp
webView1.NavigationCompleted += webView1_NavigationCompleted;

private void webView1_NavigationCompleted(WebView sender, WebViewNavigationCompletedEventArgs args)
{
    if (args.IsSuccess == true)
    {
        statusTextBlock.Text = "Navigation to " + args.Uri.ToString() + " completed successfully.";
    }
    else
    {
        statusTextBlock.Text = "Navigation to: " + args.Uri.ToString() + 
                               " failed with error " + args.WebErrorStatus.ToString();
    }
}
```

对于 Web 视图内容中的每个 iframe，类似的事件按同样顺序发生： 
- [FrameNavigationStarting](/uwp/api/windows.ui.xaml.controls.webview.framenavigationstarting) - 在 Web 视图中的框架导航到新内容之前发生。 
- [FrameContentLoading](/uwp/api/windows.ui.xaml.controls.webview.framecontentloading) - 在 Web 视图中的框架已开始加载新内容时发生。 
- [FrameDOMContentLoaded](/uwp/api/windows.ui.xaml.controls.webview.framedomcontentloaded) - 在 Web 视图中的框架已完成分析其当前 HTML 内容时发生。 
- [FrameNavigationCompleted](/uwp/api/windows.ui.xaml.controls.webview.framenavigationcompleted) - 在 Web 视图中的框架已完成加载其内容时发生。 

### <a name="responding-to-potential-problems"></a>应对潜在问题

你可以应对内容的潜在问题，例如脚本长时间运行、Web 视图不能加载内容和不安全内容的警告。 

脚本运行期间，你的应用可能不做任何响应。 [LongRunningScriptDetected](/uwp/api/windows.ui.xaml.controls.webview.longrunningscriptdetected) 事件在 Web 视图执行 JavaScript 期间定期发生，从而提供中断该脚本运行的机会。 若要确定脚本已运行的时间，请检查 [WebViewLongRunningScriptDetectedEventArgs](/uwp/api/Windows.UI.Xaml.Controls.WebViewLongRunningScriptDetectedEventArgs) 的 [ExecutionTime](/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.executiontime) 属性。 若要停止该脚本，请将事件参数 [StopPageScriptExecution](/uwp/api/windows.ui.xaml.controls.webviewlongrunningscriptdetectedeventargs.stoppagescriptexecution) 属性设置为 **true**。 除非在后续 Web 视图导航期间重新加载已停止执行的脚本，否则不会再次执行该脚本。 

Web 视图控件无法承载任意文件类型。 尝试加载 Web 视图无法承载的内容时，将发生 [UnviewableContentIdentified](/uwp/api/windows.ui.xaml.controls.webview.unviewablecontentidentified) 事件。 你可以处理此事件并通知用户，或者使用 [Launcher](/uwp/api/Windows.System.Launcher) 类将该文件重定向到外部浏览器或其他应用。

同样地，当在 Web 内容中调用不受支持的 URI 方案（如 fbconnect:// 或 mailto://）时会发生 [UnsupportedUriSchemeIdentified](/uwp/api/windows.ui.xaml.controls.webview.unsupportedurischemeidentified) 事件。 你可以处理此事件来提供自定义行为，而不是允许默认系统启动器启动 URI。

当 Web 视图显示由 SmartScreen 筛选器报告为不安全内容的警告页时，会发生 [UnsafeContentWarningDisplayingevent](/uwp/api/windows.ui.xaml.controls.webview.unsafecontentwarningdisplaying) 事件。 如果用户选择继续导航，随后导航到该页面将不显示警告，也不会触发该事件。

### <a name="handling-special-cases-for-web-view-content"></a>处理 Web 视图内容的特殊情况

你可以使用 [ContainsFullScreenElement](/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelement) 属性和 [ContainsFullScreenElementChanged](/uwp/api/windows.ui.xaml.controls.webview.containsfullscreenelementchanged) 事件来检测、响应和支持 Web 内容中的全屏体验，例如全屏视频播放。 例如，你可能会使用 ContainsFullScreenElementChanged 事件来调整 Web 视图的大小以占据整个应用视图，或者（如以下示例所示）在需要全屏 Web 体验时，以全屏模式显示应用的窗口。

```csharp
// Assume webView is defined in XAML
webView.ContainsFullScreenElementChanged += webView_ContainsFullScreenElementChanged;

private void webView_ContainsFullScreenElementChanged(WebView sender, object args)
{
    var applicationView = ApplicationView.GetForCurrentView();

    if (sender.ContainsFullScreenElement)
    {
        applicationView.TryEnterFullScreenMode();
    }
    else if (applicationView.IsFullScreenMode)
    {
        applicationView.ExitFullScreenMode();
    }
}
```

你可以使用 [NewWindowRequested](/uwp/api/windows.ui.xaml.controls.webview.newwindowrequested) 事件来处理托管 web 内容请求显示新窗口的情况，例如弹出窗口。 你可以使用另一个 WebView 控件来显示请求窗口的内容。

[PermissionRequested](/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) 事件用于启用需要特殊功能的 Web 功能。 当前这些功能包括：地理位置、IndexedDB 存储以及用户音频和视频（例如，来自麦克风或摄像头）。 如果你的应用访问用户位置或用户媒体，仍需要在应用清单中声明此功能。 例如，使用地理位置的应用在 Package.appxmanifest 中至少需要声明以下功能：

```xml
  <Capabilities>
    <Capability Name="internetClient" />
    <DeviceCapability Name="location" />
  </Capabilities>
```

除了由应用处理 [PermissionRequested](/uwp/api/windows.ui.xaml.controls.webview.permissionrequested) 事件，用户还需要批准请求位置或媒体功能的应用的标准系统对话框，以便启用这些功能。

下面是应用如何在必应地图中支持地理位置的示例：

```csharp
// Assume webView is defined in XAML
webView.PermissionRequested += webView_PermissionRequested;

private void webView_PermissionRequested(WebView sender, WebViewPermissionRequestedEventArgs args)
{
    if (args.PermissionRequest.PermissionType == WebViewPermissionType.Geolocation &&
        args.PermissionRequest.Uri.Host == "www.bing.com")
    {
        args.PermissionRequest.Allow();
    }
}
```

如果你的应用需要用户输入或其他异步操作来响应权限请求，则使用 [WebViewPermissionRequest](/uwp/api/Windows.UI.Xaml.Controls.WebViewPermissionRequest) 的 [Defer](/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer) 方法创建一个可以稍后执行的 [WebViewDeferredPermissionRequest](/uwp/api/Windows.UI.Xaml.Controls.WebViewDeferredPermissionRequest)。 请参阅 [WebViewPermissionRequest.Defer](/uwp/api/windows.ui.xaml.controls.webviewpermissionrequest.defer)。 

如果用户必须安全地注销 Web 视图中承载的网站，或在其他安全性非常重要的情况下，请调用静态方法 [ClearTemporaryWebDataAsync](/uwp/api/windows.ui.xaml.controls.webview.cleartemporarywebdataasync) 清除 Web 视图会话中本地缓存的内容。 这可以防止恶意用户访问敏感数据。 

### <a name="interacting-with-web-view-content"></a>与 Web 视图内容交互

你可以与 Web 视图内容进行交互，方法为使用 [InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) 方法调用脚本或将脚本注入 Web 视图内容，并使用 [ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) 事件从 Web 视图内容取回信息。

若要调用 Web 视图内容中的 JavaScript，请使用 [InvokeScriptAsync](/uwp/api/windows.ui.xaml.controls.webview.invokescriptasync) 方法。 调用的脚本只可以返回字符串值。 

例如，如果名为 `webView1` 的 Web 视图内容包含一个名为 `setDate` 的函数（采用 3 个参数），你可以按如下方式调用该函数。 

```csharp
string[] args = {"January", "1", "2000"};
string returnValue = await webView1.InvokeScriptAsync("setDate", args);
```


你可以将 InvokeScriptAsync 与 JavaScript eval 函数结合使用，以便将内容注入到 Web 页面。

此处，XAML 文本框中的文本 (`nameTextBox.Text`) 将写入 `webView1` 中承载的 HTML 页面中的 div。 

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    string functionString = String.Format("document.getElementById('nameDiv').innerText = 'Hello, {0}';", nameTextBox.Text);
    await webView1.InvokeScriptAsync("eval", new string[] { functionString });
}
```

Web 视图内容中的脚本可以使用带有字符串参数的 window.external.notify 将信息发送回你的应用。 若要接收这些消息，请处理 [ScriptNotify](/uwp/api/windows.ui.xaml.controls.webview.scriptnotify) 事件。 

若要在调用 window.external.notify 时，允许外部网页引发 ScriptNotify 事件，必须在应用清单的 ApplicationContentUriRules 部分包含该页面的 URI。 （你可以在 Microsoft Visual Studio 中的 Package.appxmanifest 设计器的“内容 URI”选项卡上执行此操作。）此列表中的 URI 必须使用 HTTPS，可以包含子域通配符（例如 `https://*.microsoft.com`），但不能包含域通配符（例如 `https://*.com` 和 `https://*.*`）。 该部件清单要求不适用于源自应用包的内容、使用 ms-local-stream:// URI 的内容或使用 [NavigateToString](/uwp/api/windows.ui.xaml.controls.webview.navigatetostring) 加载的内容。 

### <a name="accessing-the-windows-runtime-in-a-web-view"></a>访问 Web 视图中的 Windows 运行时

你可以使用 [AddWebAllowedObject](/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject) 方法将本机类的实例从 Windows 运行时组件注入 Web 视图的 JavaScript 上下文。 这支持对该 Web 视图内 JavaScript 内容中的对象的本机方法、属性和事件进行完全访问。 必须使用 [AllowForWeb](/uwp/api/Windows.Foundation.Metadata.AllowForWebAttribute) 属性修饰该类。 

例如，此代码将一个从 Windows 运行时组件导入的 `MyClass` 的实例注入到 Web 视图。

```csharp
private void webView_NavigationStarting(WebView sender, WebViewNavigationStartingEventArgs args) 
{ 
    if (args.Uri.Host == "www.contoso.com")  
    { 
        webView.AddWebAllowedObject("nativeObject", new MyClass()); 
    } 
}
```

有关详细信息，请参阅 [WebView.AddWebAllowedObject](/uwp/api/windows.ui.xaml.controls.webview.addweballowedobject)。 

此外，还可以允许 Web 视图中受信任的 JavaScript 内容直接访问 Windows 运行时 API。 这为 Web 视图中承载的 Web 应用提供了强大的本机功能。 若要启用此功能，必须在 Package.appxmanifest 中将受信任内容的 URI 列入应用的 ApplicationContentUriRules 中的也许列表，并将 WindowsRuntimeAccess 明确设置为“all”。 

此示例显示应用清单的一部分。 此处，本地 URI 可访问 Windows 运行时。 

```csharp
  <Applications>
    <Application Id="App"
      ...

      <uap:ApplicationContentUriRules>
        <uap:Rule Match="ms-appx-web:///Web/App.html" WindowsRuntimeAccess="all" Type="include"/>
      </uap:ApplicationContentUriRules>
    </Application>
  </Applications>
```

### <a name="options-for-web-content-hosting"></a>用于 Web 内容托管的选项

你可以使用 [WebView.Settings](/uwp/api/windows.ui.xaml.controls.webview.settings) 属性（属于类型 [WebViewSettings](/uwp/api/Windows.UI.Xaml.Controls.WebViewSettings)）对是否启用 JavaScript 和 IndexedDB 进行控制。 例如，当 Web 视图用于只显示静态内容时，你需要禁用 JavaScript 以获得最佳性能。

### <a name="capturing-web-view-content"></a>捕获 Web 视图内容

若要允许与其他应用共享 Web 视图内容，请使用 [CaptureSelectedContentToDataPackageAsync](/uwp/api/windows.ui.xaml.controls.webview.captureselectedcontenttodatapackageasync) 方法，该方法会将选定的内容作为 [DataPackage](/uwp/api/Windows.ApplicationModel.DataTransfer.DataPackage) 返回。 此方法是异步的，因此必须使用延迟来阻止 [DataRequested](/uwp/api/windows.applicationmodel.datatransfer.datatransfermanager.datarequested) 事件处理程序在异步调用完成之前返回。 

若要获取 Web 视图当前内容的预览图像，请使用 [CapturePreviewToStreamAsync](/uwp/api/windows.ui.xaml.controls.webview.capturepreviewtostreamasync) 方法。 此方法会创建当前内容的图像，并将其写入指定的流。 

### <a name="threading-behavior"></a>线程行为

默认情况下，Web 视图内容承载于属于桌面设备系列的设备的 UI 线程上，不会承载在所有其他设备的 UI 线程上。 你可以使用 [WebView.DefaultExecutionMode](/uwp/api/windows.ui.xaml.controls.webview.defaultexecutionmode) 静态属性来查询当前客户端的默认线程行为。 如有必要，可以使用 [WebView(WebViewExecutionMode)](/uwp/api/windows.ui.xaml.controls.webview.-ctor#Windows_UI_Xaml_Controls_WebView__ctor_Windows_UI_Xaml_Controls_WebViewExecutionMode_) 构造函数替代此行为。 

> **注意**&nbsp;&nbsp;在移动设备的 UI 线程上托管内容时可能存在性能问题，因此在更改 DefaultExecutionMode 时，请确保在所有目标设备上进行测试。

在 UI 线程外托管内容的 Web 视图与需要手势才能从 Web 视图控件向上传播到父级的父控件不兼容，例如 [FlipView](/uwp/api/Windows.UI.Xaml.Controls.FlipView)、[ScrollViewer](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) 和其他相关控件。 这些控件将不能接收在线程外的 Web 视图中发起的手势。 此外，并不直接支持打印线程外的 Web 内容，应改为使用 [WebViewBrush](/uwp/api/Windows.UI.Xaml.Controls.WebViewBrush) 填充打印元素。

## <a name="recommendations"></a>建议


-   确保针对设备正确地格式化加载的网站，并使用与应用的其他部分一致的颜色、版式和导航。
-   应相应地调整输入字段的大小。 用户可能不会意识到他们可以放大以输入文本。
-   如果 Web 视图的外观与应用其他部分有差异，请考虑使用替代控件或方法完成相关任务。 如果你的 Web 视图与应用的其余部分相匹配，用户会将其视作一个整体无缝体验。

## <a name="get-the-sample-code"></a>获取示例代码

- [XAML 控件库示例](https://github.com/Microsoft/Xaml-Controls-Gallery) - 以交互式格式查看所有 XAML 控件。

## <a name="related-topics"></a>相关主题

- [WebView 类](/uwp/api/Windows.UI.Xaml.Controls.WebView)
