---
ms.assetid: ''
description: 本文说明如何连接到远程照相机，并从每个照相机获取 MediaFrameSourceGroup 来检索帧。
title: 连接到远程摄像头
ms.date: 04/19/2019
ms.topic: article
ms.custom: 19H1
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1fae76aea28ffb63f6cb0ad5af8c5eb6e3fac6e4
ms.sourcegitcommit: 8171695ade04a762f19723f0b88e46e407375800
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89494363"
---
# <a name="connect-to-remote-cameras"></a>连接到远程摄像头

本文说明如何连接到一个或多个远程照相机，并获取一个 [**MediaFrameSourceGroup**](/uwp/api/Windows.Media.Capture.Frames.MediaFrameSourceGroup) 对象，该对象允许您从每个照相机读取帧。 有关从媒体源中读取帧的详细信息，请参阅 [使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)。 有关与设备配对的详细信息，请参阅 [配对设备](../devices-sensors/pair-devices.md)。

> [!NOTE] 
> 本文中所述的功能从 Windows 10 版本1903开始可用。

## <a name="create-a-devicewatcher-class-to-watch-for-available-remote-cameras"></a>创建用于监视可用远程相机的 DeviceWatcher 类

[**DeviceWatcher**](/uwp/api/windows.devices.enumeration.devicewatcher)类监视应用可用的设备，并在添加或删除设备时通知应用。 通过调用[**DeviceInformation**](/uwp/api/windows.devices.enumeration.deviceinformation.createwatcher#Windows_Devices_Enumeration_DeviceInformation_CreateWatcher_System_String_)获取**DeviceWatcher**的实例，并传入高级查询语法 (AQS 标识要监视的设备类型的字符串) 字符串。 指定网络摄像机设备的 AQS 字符串如下：

```syntax
@"System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"" AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True"
```

> [!NOTE] 
> Helper 方法 [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.getdeviceselector) 返回一个 AQS 字符串，该字符串将监视本地连接的和远程网络摄像机。 若要仅监视网络摄像机，应使用上面所示的 AQS 字符串。

当你通过调用[**start**](/uwp/api/windows.devices.enumeration.devicewatcher.start)方法来启动返回的**DeviceWatcher**时，它将为当前可用的每个网络相机引发[**添加**](/uwp/api/windows.devices.enumeration.devicewatcher.added)的事件。 在通过调用 [**stop**](/uwp/api/windows.devices.enumeration.devicewatcher.stop)停止观察程序之前，当新的网络照相机设备可用时，将引发 **添加** 的事件，并且当照相机设备不可用时，将引发 [**已删除**](/uwp/api/windows.devices.enumeration.devicewatcher.removed) 的事件。

传入 **添加** 和 **移除** 事件处理程序的事件参数分别是 [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation) 或 [**DeviceInformationUpdate**](/uwp/api/windows.devices.enumeration.deviceinformationupdate) 对象。 其中每个对象都有一个 **Id** 属性，该属性是触发事件的网络摄像机的标识符。 将此 ID 传递到 [**MediaFrameSourceGroup. FromIdAsync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 方法，以获取可用于从相机检索帧的 [**MediaFrameSourceGroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.fromidasync) 对象。

## <a name="remote-camera-pairing-helper-class"></a>远程相机配对帮助程序类

下面的示例演示一个帮助器类，该类使用**DeviceWatcher**创建和更新**MediaFrameSourceGroup**对象的**ObservableCollection** ，以支持与照相机列表的数据绑定。 典型应用会在自定义模型类中包装 **MediaFrameSourceGroup** 。 请注意，帮助器类维护对应用程序的 [**CoreDispatcher**](/uwp/api/Windows.UI.Core.CoreDispatcher) 的引用，并在对 [**RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) 的调用中更新照相机的集合，以确保在 ui 线程上更新绑定到该集合的 ui。

此外，此示例还处理**已添加**和**已删除**事件之外的[**DeviceWatcher**](/uwp/api/windows.devices.enumeration.devicewatcher.updated)事件。 在 **更新** 的处理程序中，将从中移除关联的远程照相机设备，然后将其添加回集合。

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Frames_Win10/cs/Frames_Win10/RemoteCameraPairingHelper.cs" id="SnippetRemoteCameraPairingHelper":::

```cppwinrt
#include <winrt/Windows.Devices.Enumeration.h>
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Media.Capture.Frames.h>
#include <winrt/Windows.UI.Core.h>
using namespace winrt;
using namespace winrt::Windows::Devices::Enumeration;
using namespace winrt::Windows::Foundation::Collections;
using namespace winrt::Windows::Media::Capture::Frames;
using namespace winrt::Windows::UI::Core;

struct RemoteCameraPairingHelper
{
    RemoteCameraPairingHelper(CoreDispatcher uiDispatcher) :
        m_dispatcher(uiDispatcher)
    {
        m_remoteCameraCollection = winrt::single_threaded_observable_vector<MediaFrameSourceGroup>();
        auto remoteCameraAqs =
            LR"(System.Devices.InterfaceClassGuid:=""{B8238652-B500-41EB-B4F3-4234F7F5AE99}"")"
            LR"(AND System.Devices.InterfaceEnabled:=System.StructuredQueryType.Boolean#True)";
        m_watcher = DeviceInformation::CreateWatcher(remoteCameraAqs);
        m_watcherAddedAutoRevoker = m_watcher.Added(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Added });
        m_watcherRemovedAutoRevoker = m_watcher.Removed(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Removed });
        m_watcherUpdatedAutoRevoker = m_watcher.Updated(winrt::auto_revoke, { this, &RemoteCameraPairingHelper::Watcher_Updated });
        m_watcher.Start();
    }
    ~RemoteCameraPairingHelper()
    {
        m_watcher.Stop();
    }
    IObservableVector<MediaFrameSourceGroup> FrameSourceGroups()
    {
        return m_remoteCameraCollection;
    }
    winrt::fire_and_forget Watcher_Added(DeviceWatcher /* sender */, DeviceInformation args)
    {
        co_await AddDeviceAsync(args.Id());
    }
    winrt::fire_and_forget Watcher_Removed(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
    }
    winrt::fire_and_forget Watcher_Updated(DeviceWatcher /* sender */, DeviceInformationUpdate args)
    {
        co_await RemoveDevice(args.Id());
        co_await AddDeviceAsync(args.Id());
    }
    Windows::Foundation::IAsyncAction AddDeviceAsync(winrt::hstring id)
    {
        auto group = co_await MediaFrameSourceGroup::FromIdAsync(id);
        if (group)
        {
            co_await m_dispatcher;
            m_remoteCameraCollection.Append(group);
        }
    }
    Windows::Foundation::IAsyncAction RemoveDevice(winrt::hstring id)
    {
        co_await m_dispatcher;

        uint32_t ix{ 0 };
        for (auto const&& item : m_remoteCameraCollection)
        {
            if (item.Id() == id)
            {
                m_remoteCameraCollection.RemoveAt(ix);
                break;
            }
            ++ix;
        }
    }

private:
    CoreDispatcher m_dispatcher{ nullptr };
    DeviceWatcher m_watcher{ nullptr };
    IObservableVector<MediaFrameSourceGroup> m_remoteCameraCollection;
    DeviceWatcher::Added_revoker m_watcherAddedAutoRevoker;
    DeviceWatcher::Removed_revoker m_watcherRemovedAutoRevoker;
    DeviceWatcher::Updated_revoker m_watcherUpdatedAutoRevoker;
};
```

## <a name="related-topics"></a>相关主题

* [摄像头](camera.md)
* [使用 MediaCapture 捕获基本的照片、视频和音频](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [相机帧示例](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CameraFrames)
* [使用 MediaFrameReader 处理媒体帧](process-media-frames-with-mediaframereader.md)
