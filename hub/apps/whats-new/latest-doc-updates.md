---
description: 发现 Windows 开发人员文档的最新新增内容。
title: Windows 开发人员文档的最新更新
ms.topic: article
ms.date: 11/05/2020
ms.localizationpriority: medium
ms.author: quradic
author: QuinnRadich
ms.openlocfilehash: a4d05e92399fa0246add4751323439303647d531
ms.sourcegitcommit: 98888dede3c59ee12c64ab0521a85b5fa60cb771
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/06/2020
ms.locfileid: "93765580"
---
# <a name="latest-updates-to-the-windows-developer-docs"></a>Windows 开发人员文档的最新更新

Windows 开发人员文档会定期更新有新的、经过改进的信息和内容。 下面是截至 2020 年 11 月 5 日的更改摘要。

注意：有关作为 Windows 10 版本 19041（也称为 2004）的一部分添加的特定 API 列表，请参阅[此列表](/windows/uwp/whats-new/windows-10-build-19041-api-diff)。


## <a name="news"></a>新闻

本月，我们将另一批绝对链接转换为了相对链接，以加快导航速度，并改进离线阅读体验。 如果你发现链接被破坏，请继续告诉我们：你可以直接在大多数页面中提供反馈，也可以使用句柄 [@WindowsDocs](https://www.twitter.com/windowsdocs) 在 Twitter 上联系我们。

本月要点包括：


### <a name="new-topics"></a>新主题

* [经典控制台 API 与虚拟终端序列](https://docs.microsoft.com/windows/console/classic-vs-vt)
* [Windows 控制台和终端生态系统路线图](https://docs.microsoft.com/windows/console/ecosystem-roadmap)
* [Windows API 集](https://docs.microsoft.com/windows/win32/apiindex/windows-apisets)
* [API 集操作加载程序](https://docs.microsoft.com/windows/win32/apiindex/api-set-loader-operation)
* [检测 API 集可用性](https://docs.microsoft.com/windows/win32/apiindex/detect-api-set-availability)
* [Windows 伞库](https://docs.microsoft.com/windows/win32/apiindex/windows-umbrella-libraries)


### <a name="new-and-updated-samples"></a>新示例和更新的示例

* [演练：从 C++/WinRT 组件生成 .NET 5 投影并分发 NuGet](https://docs.microsoft.com/windows/uwp/csharp-winrt/net-projection-from-cppwinrt-component)


### <a name="other-content-of-interest"></a>其他感兴趣的内容

* [Azure 通信服务](https://docs.microsoft.com/azure/communication-services/overview)
* [Azure 应用配置 Python 快速入门](https://docs.microsoft.com/azure/azure-app-configuration/quickstart-python)

### <a name="deprecated-content"></a>已弃用的内容

* [BarcodeScanner.GetSupportedProfiles](https://docs.microsoft.com/uwp/api/windows.devices.pointofservice.barcodescanner.getsupportedprofiles?view=winrt-19041)

### <a name="updated-documentation"></a>已更新的文档

* [控制台文档](https://github.com/MicrosoftDocs/Console-Docs)
* [MIDL 3.0 概念内容](https://docs.microsoft.com/uwp/midl-3/intro#parameters)
* [Docker 概述入门](https://docs.microsoft.com/windows/dev-environment/docker/overview)
* [关于在 WSL 2 中安装 Linux 磁盘的入门](https://docs.microsoft.com/windows/wsl/wsl2-mount-disk)

以下 API 参考主题在过去一个月中有了重大更新：

### <a name="win32-api-reference"></a>Win32 API 参考
<ul>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-createmappedbitmap">CreateMappedBitmap 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-createtoolbarex">CreateToolbarEx 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-getwindowsubclass">GetWindowSubclass 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_addmasked">ImageList_AddMasked 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_create">ImageList_Create 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_draw">ImageList_Draw 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_replaceicon">ImageList_ReplaceIcon 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commctrl/nf-commctrl-imagelist_setoverlayimage">ImageList_SetOverlayImage 函数 (commctrl.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-commdlgextendederror">CommDlgExtendedError 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-findtexta">FindTextA 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-findtextw">FindTextW 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getopenfilenamea">GetOpenFileNameA 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getopenfilenamew">GetOpenFileNameW 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getsavefilenamea">GetSaveFileNameA 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/commdlg/nf-commdlg-getsavefilenamew">GetSaveFileNameW 函数 (commdlg.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dde/nf-dde-unpackddelparam">UnpackDDElParam 函数 (dde.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_clone">DPA_Clone 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_createex">DPA_CreateEx 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_destroycallback">DPA_DestroyCallback 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_enumcallback">DPA_EnumCallback 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_getptr">DPA_GetPtr 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_getptrindex">DPA_GetPtrIndex 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_grow">DPA_Grow 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_insertptr">DPA_InsertPtr 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_search">DPA_Search 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_setptr">DPA_SetPtr 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dpa_sort">DPA_Sort 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_create">DSA_Create 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_deleteitem">DSA_DeleteItem 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_destroycallback">DSA_DestroyCallback 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_enumcallback">DSA_EnumCallback 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_getitem">DSA_GetItem 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_getitemptr">DSA_GetItemPtr 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_insertitem">DSA_InsertItem 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/dpa_dsa/nf-dpa_dsa-dsa_setitem">DSA_SetItem 函数 (dpa_dsa.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-bindmoniker">BindMoniker 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-codosdatetimetofiletime">CoDosDateTimeToFileTime 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-cofiletimetodosdatetime">CoFileTimeToDosDateTime 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-cofreealllibraries">CoFreeAllLibraries 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-cofreelibrary">CoFreeLibrary 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-coinitialize">CoInitialize 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-coloadlibrary">CoLoadLibrary 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createantimoniker">CreateAntiMoniker 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createdataadviseholder">CreateDataAdviseHolder 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createdatacache">CreateDataCache 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createobjrefmoniker">CreateObjrefMoniker 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-createpointermoniker">CreatePointerMoniker 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/objbase/nf-objbase-getrunningobjecttable">GetRunningObjectTable 函数 (objbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole/nf-ole-oledraw">OleDraw 函数 (ole.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole/nf-ole-oleloadfromstream">OleLoadFromStream 函数 (ole.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole/nf-ole-olesavetostream">OleSaveToStream 函数 (ole.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-createdataadviseholder">CreateDataAdviseHolder 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-createoleadviseholder">CreateOleAdviseHolder 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olecreateembeddinghelper">OleCreateEmbeddingHelper 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olecreatefromdata">OleCreateFromData 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olecreatemenudescriptor">OleCreateMenuDescriptor 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olegetautoconvert">OleGetAutoConvert 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olegetclipboard">OleGetClipboard f函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olegeticonofclass">OleGetIconOfClass 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleinitialize">OleInitialize 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleiscurrentclipboard">OleIsCurrentClipboard 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olequerycreatefromdata">OleQueryCreateFromData 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleregenumverbs">OleRegEnumVerbs 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olesavetostream">OleSaveToStream 函数 (ole2.h)</a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-olesetmenudescriptor">OleSetMenuDescriptor 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-oleuninitialize">OleUninitialize 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/ole2/nf-ole2-writefmtusertypestg">WriteFmtUserTypeStg 函数 (ole2.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-createpropertysheetpagea">CreatePropertySheetPageA 函数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-createpropertysheetpagew">CreatePropertySheetPageW 函数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-propertysheeta">PropertySheetA 函数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/prsht/nf-prsht-propertysheetw">PropertySheetW 函数 (prsht.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupcloseinffile">SetupCloseInfFile 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupdigetdevicepropertyw">SetupDiGetDevicePropertyW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupdiopendeviceinfow">SetupDiOpenDeviceInfoW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupfindnextline">SetupFindNextLine 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetfieldcount">SetupGetFieldCount 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinecounta">SetupGetLineCountA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinecountw">SetupGetLineCountW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinetexta">SetupGetLineTextA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetlinetextw">SetupGetLineTextW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetstringfielda">SetupGetStringFieldA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetstringfieldw">SetupGetStringFieldW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupgetthreadlogtoken">SetupGetThreadLogToken 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupiteratecabineta">SetupIterateCabinetA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupiteratecabinetw">SetupIterateCabinetW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setuplogerrora">SetupLogErrorA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setuplogerrorw">SetupLogErrorW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupopeninffilea">SetupOpenInfFileA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupopenlog">SetupOpenLog 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupverifyinffilea">SetupVerifyInfFileA 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupverifyinffilew">SetupVerifyInfFileW 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/setupapi/nf-setupapi-setupwritetextlog">SetupWriteTextLog 函数 (setupapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-assoccreateforclasses">AssocCreateForClasses 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-duplicateicon">DuplicateIcon 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-extracticonexa">ExtractIconExA 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-extracticonexw">ExtractIconExW 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shell_notifyiconw">Shell_NotifyIconW 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shgetimagelist">SHGetImageList 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shgetstockiconinfo">SHGetStockIconInfo 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shellapi/nf-shellapi-shsetlocalizedname">SHSetLocalizedName 函数 (shellapi.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-dad_autoscroll">DAD_AutoScroll 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-dad_dragenterex2">DAD_DragEnterEx2 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-dad_setdragimage">DAD_SetDragImage 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilcreatefrompath">ILCreateFromPath 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilcreatefrompatha">ILCreateFromPathA 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilcreatefrompathw">ILCreateFromPathW 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-ilsavetostream">ILSaveToStream 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-pickicondlg">PickIconDlg 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-restartdialog">RestartDialog 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shaddtorecentdocs">SHAddToRecentDocs 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shchangenotify">SHChangeNotify 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shcreateshellitem">SHCreateShellItem 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shdodragdrop">SHDoDragDrop 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shell_getimagelists">Shell_GetImageLists 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shgetrealidl">SHGetRealIDL 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shgetsetsettings">SHGetSetSettings 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shilcreatefrompath">SHILCreateFromPath 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shlimitinputedit">SHLimitInputEdit 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/shlobj_core/nf-shlobj_core-shopenwithdialog">SHOpenWithDialog 函数 (shlobj_core.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-backupeventloga">BackupEventLogA 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-backupeventlogw">BackupEventLogW 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-cleareventloga">ClearEventLogA 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-cleareventlogw">ClearEventLogW 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-closeeventlog">CloseEventLog 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-deleteumsthreadcontext">DeleteUmsThreadContext 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-encryptfilea">EncryptFileA 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-encryptfilew">EncryptFileW 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-enterumsschedulingmode">EnterUmsSchedulingMode 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getcurrenthwprofilea">GetCurrentHwProfileA 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getcurrenthwprofilew">GetCurrentHwProfileW 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getnumberofeventlogrecords">GetNumberOfEventLogRecords 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-getumscompletionlistevent">GetUmsCompletionListEvent 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-openbackupeventloga">OpenBackupEventLogA 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-openbackupeventlogw">OpenBackupEventLogW 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-queryumsthreadinformation">QueryUmsThreadInformation 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readencryptedfileraw">ReadEncryptedFileRaw 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readeventloga">ReadEventLogA 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-readeventlogw">ReadEventLogW 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winbase/nf-winbase-umsthreadyield">UmsThreadYield 函数 (winbase.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winnls/nf-winnls-notifyuilanguagechange">NotifyUILanguageChange 函数 (winnls.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winperf/nc-winperf-pm_collect_proc">PM_COLLECT_PROC (winperf.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safercreatelevel">SaferCreateLevel 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safergetlevelinformation">SaferGetLevelInformation 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safergetpolicyinformation">SaferGetPolicyInformation 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-saferidentifylevel">SaferIdentifyLevel 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-saferrecordeventlogentry">SaferRecordEventLogEntry 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safersetlevelinformation">SaferSetLevelInformation 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winsafer/nf-winsafer-safersetpolicyinformation">SaferSetPolicyInformation 函数 (winsafer.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-addclipboardformatlistener">AddClipboardFormatListener 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-adjustwindowrectex">AdjustWindowRectEx 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-animatewindow">AnimateWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-appendmenua">AppendMenuA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-appendmenuw">AppendMenuW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-attachthreadinput">AttachThreadInput 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-bringwindowtotop">BringWindowToTop 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-callmsgfiltera">CallMsgFilterA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-callmsgfilterw">CallMsgFilterW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-changedisplaysettingsa">ChangeDisplaySettingsA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-changedisplaysettingsw">ChangeDisplaySettingsW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-changewindowmessagefilterex">ChangeWindowMessageFilterEx 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-checkdlgbutton">CheckDlgButton 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-checkmenuitem">CheckMenuItem 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-checkmenuradioitem">CheckMenuRadioItem 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-childwindowfrompoint">ChildWindowFromPoint 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-childwindowfrompointex">ChildWindowFromPointEx 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-closedesktop">CloseDesktop 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-closewindowstation">CloseWindowStation 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createcaret">CreateCaret 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdesktopa">CreateDesktopA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdesktopw">CreateDesktopW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdialogindirectparama">CreateDialogIndirectParamA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createdialogindirectparamw">CreateDialogIndirectParamW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-createiconindirect">CreateIconIndirect 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-defrawinputproc">DefRawInputProc 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-defwindowproca">DefWindowProcA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-defwindowprocw">DefWindowProcW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-deletemenu">DeleteMenu 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-destroycaret">DestroyCaret 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-destroywindow">DestroyWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dialogboxindirectparama">DialogBoxIndirectParamA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dialogboxindirectparamw">DialogBoxIndirectParamW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dispatchmessage">DispatchMessage 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dispatchmessagea">DispatchMessageA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-dispatchmessagew">DispatchMessageW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawedge">DrawEdge 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawicon">DrawIcon 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawiconex">DrawIconEx 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtexta">DrawTextA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtextexa">DrawTextExA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtextexw">DrawTextExW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-drawtextw">DrawTextW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enablemenuitem">EnableMenuItem 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enablescrollbar">EnableScrollBar 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-endmenu">EndMenu 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-endpaint">EndPaint 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesa">EnumDisplayDevicesA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-enumdisplaydevicesw">EnumDisplayDevicesW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-fillrect">FillRect 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-findwindowa">FindWindowA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-findwindoww">FindWindowW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-flashwindowex">FlashWindowEx 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getancestor">GetAncestor 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getcapture">GetCapture 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getcaretblinktime">GetCaretBlinkTime 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfoa">GetClassInfoA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfoexa">GetClassInfoExA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfoexw">GetClassInfoExW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassinfow">GetClassInfoW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclasslongptra">GetClassLongPtrA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclasslongptrw">GetClassLongPtrW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassname">GetClassName 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassnamea">GetClassNameA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassnamew">GetClassNameW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclassword">GetClassWord 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclipboardformatnamea">GetClipboardFormatNameA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclipboardformatnamew">GetClipboardFormatNameW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getclipboardviewer">GetClipboardViewer 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getcursorpos">GetCursorPos 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdesktopwindow">GetDesktopWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdlgitemint">GetDlgItemInt 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdlgitemtexta">GetDlgItemTextA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdlgitemtextw">GetDlgItemTextW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getdoubleclicktime">GetDoubleClickTime 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getfocus">GetFocus 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getforegroundwindow">GetForegroundWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getkeyboardstate">GetKeyboardState 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getlastactivepopup">GetLastActivePopup 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenudefaultitem">GetMenuDefaultItem 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuinfo">GetMenuInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuitemcount">GetMenuItemCount 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuitemid">GetMenuItemID 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuiteminfoa">GetMenuItemInfoA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenuiteminfow">GetMenuItemInfoW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmenustate">GetMenuState 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmessageextrainfo">GetMessageExtraInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getmessagetime">GetMessageTime 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getparent">GetParent 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointerdevices">GetPointerDevices 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointerframeinfohistory">GetPointerFrameInfoHistory 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointerframetouchinfo">GetPointerFrameTouchInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getpointertype">GetPointerType 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getprocessdefaultlayout">GetProcessDefaultLayout 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getrawinputdevicelist">GetRawInputDeviceList 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getscrollbarinfo">GetScrollBarInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getscrollinfo">GetScrollInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getshellwindow">GetShellWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getsubmenu">GetSubMenu 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getsystemmetrics">GetSystemMetrics 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getsystemmetricsfordpi">GetSystemMetricsForDpi 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-gettitlebarinfo">GetTitleBarInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-gettopwindow">GetTopWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-gettouchinputinfo">GetTouchInputInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindow">GetWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtexta">GetWindowTextA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtextlengtha">GetWindowTextLengthA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtextlengthw">GetWindowTextLengthW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowtextw">GetWindowTextW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-getwindowthreadprocessid">GetWindowThreadProcessId 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-initializetouchinjection">InitializeTouchInjection 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-injecttouchinput">InjectTouchInput 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insendmessage">InSendMessage 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenua">InsertMenuA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenuitema">InsertMenuItemA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenuitemw">InsertMenuItemW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-insertmenuw">InsertMenuW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-invalidaterect">InvalidateRect 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-invertrect">InvertRect 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isclipboardformatavailable">IsClipboardFormatAvailable 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isdialogmessagea">IsDialogMessageA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isdialogmessagew">IsDialogMessageW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isdlgbuttonchecked">IsDlgButtonChecked 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-ishungappwindow">IsHungAppWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-isiconic">IsIconic 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-ismenu">IsMenu 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-iswindowunicode">IsWindowUnicode 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-iswineventhookinstalled">IsWinEventHookInstalled 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-iszoomed">IsZoomed 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-loadicona">LoadIconA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-loadiconw">LoadIconW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-loadimagea">LoadImageA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-locksetforegroundwindow">LockSetForegroundWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-lockworkstation">LockWorkStation 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-logicaltophysicalpoint">LogicalToPhysicalPoint 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-mapwindowpoints">MapWindowPoints 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messagebeep">MessageBeep 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messagebox">MessageBox 函数 (winuser.h)</a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxa">MessageBoxA 函数 (winuser.h)</a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxindirecta">MessageBoxIndirectA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxindirectw">MessageBoxIndirectW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-messageboxw">MessageBoxW 函数 (winuser.h)</a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-modifymenua">ModifyMenuA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-modifymenuw">ModifyMenuW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-monitorfrompoint">MonitorFromPoint 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-monitorfromrect">MonitorFromRect 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-monitorfromwindow">MonitorFromWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-oemtocharbuffa">OemToCharBuffA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-oemtocharbuffw">OemToCharBuffW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-openinputdesktop">OpenInputDesktop 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-peekmessagea">PeekMessageA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-peekmessagew">PeekMessageW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-physicaltologicalpoint">PhysicalToLogicalPoint 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-postmessagea">PostMessageA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-postmessagew">PostMessageW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-privateextracticonsa">PrivateExtractIconsA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-privateextracticonsw">PrivateExtractIconsW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-querydisplayconfig">QueryDisplayConfig 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-realgetwindowclassw">RealGetWindowClassW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-redrawwindow">RedrawWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerclipboardformata">RegisterClipboardFormatA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerclipboardformatw">RegisterClipboardFormatW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerdevicenotificationa">RegisterDeviceNotificationA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-registerdevicenotificationw">RegisterDeviceNotificationW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-removemenu">RemoveMenu 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-removepropa">RemovePropA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-removepropw">RemovePropW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-scrolldc">ScrollDC 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-scrollwindow">ScrollWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-scrollwindowex">ScrollWindowEx 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-senddlgitemmessagea">SendDlgItemMessageA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-senddlgitemmessagew">SendDlgItemMessageW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-sendmessagetimeouta">SendMessageTimeoutA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-sendmessagetimeoutw">SendMessageTimeoutW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setcaretblinktime">SetCaretBlinkTime 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setclipboarddata">SetClipboardData 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setcoalescabletimer">SetCoalescableTimer 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setcursorpos">SetCursorPos 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setdlgitemtexta">SetDlgItemTextA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setdlgitemtextw">SetDlgItemTextW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setdoubleclicktime">SetDoubleClickTime 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setfocus">SetFocus 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setforegroundwindow">SetForegroundWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setlayeredwindowattributes">SetLayeredWindowAttributes 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmenuinfo">SetMenuInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmenuiteminfoa">SetMenuItemInfoA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmenuiteminfow">SetMenuItemInfoW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setmessageextrainfo">SetMessageExtraInfo 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setparent">SetParent 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setprocesswindowstation">SetProcessWindowStation 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setpropa">SetPropA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setpropw">SetPropW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowdisplayaffinity">SetWindowDisplayAffinity 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlonga">SetWindowLongA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlongptra">SetWindowLongPtrA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlongptrw">SetWindowLongPtrW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowlongw">SetWindowLongW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowpos">SetWindowPos 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowshookexa">SetWindowsHookExA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowshookexw">SetWindowsHookExW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowtexta">SetWindowTextA 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-setwindowtextw">SetWindowTextW 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-showscrollbar">ShowScrollBar 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-showwindow">ShowWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-showwindowasync">ShowWindowAsync 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-shutdownblockreasoncreate">ShutdownBlockReasonCreate 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-shutdownblockreasondestroy">ShutdownBlockReasonDestroy 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-systemparametersinfoa">SystemParametersInfoA 函数 (winuser.h)</a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-systemparametersinfow">SystemParametersInfoW 函数 (winuser.h)</a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-trackmouseevent">TrackMouseEvent 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-updatelayeredwindow">UpdateLayeredWindow 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-waitforinputidle">WaitForInputIdle 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-waitmessage">WaitMessage 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/winuser/nf-winuser-windowfromdc">WindowFromDC 函数 (winuser.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsdisconnectsession">WTSDisconnectSession 函数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsfreememory">WTSFreeMemory 函数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsfreememoryexa">WTSFreeMemoryExA 函数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsfreememoryexw">WTSFreeMemoryExW 函数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtslogoffsession">WTSLogoffSession 函数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsvirtualchannelopenex">WTSVirtualChannelOpenEx 函数 (wtsapi32.h) </a></li>
<li><a href="https://docs.microsoft.com/windows/win32/api/wtsapi32/nf-wtsapi32-wtsvirtualchannelquery">WTSVirtualChannelQuery 函数 (wtsapi32.h) </a></li>
</ul>

### <a name="uwp-api-reference"></a>UWP API 参考

<ul>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothcachemode">Windows.Devices.Bluetooth.BluetoothCacheMode</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.fromidasync">Windows.Devices.Bluetooth.BluetoothLEDevice.FromIdAsync(System.String)</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.gattserviceschanged">Windows.Devices.Bluetooth.BluetoothLEDevice.GattServicesChanged</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.devices.bluetooth.bluetoothledevice.namechanged">Windows.Devices.Bluetooth.BluetoothLEDevice.NameChanged</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.peers.patterninterface">Windows.UI.Xaml.Automation.Peers.PatternInterface</a></li>
<li><a href="https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.listviewbase.candragitems">Windows.UI.Xaml.Controls.ListViewBase.CanDragItems</a></li>
</ul>


