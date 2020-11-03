---
description: 列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: 控件模式和接口
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 92f2337848f3689aa8f2f6f73dd92dbbe1313257
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93032530"
---
# <a name="control-patterns-and-interfaces"></a>控件模式和接口  



列出 Microsoft UI 自动化控件模式、客户端用于访问这些模式的类以及提供程序用于实现这些模式的接口。

本主题中的表描述了 Microsoft UI 自动化控件模式。 此表还列出了 UI 自动化客户端用于访问控件模式的类和 UI 自动化提供程序用来实现这些模式的接口。 从 UI 自动化客户端的角度来看，“控件模式”列显示模式名称， [**Control Pattern Availability Property Identifiers**](/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids) 中将该名称列为常数值。 从 UI 自动化提供程序的角度来看，其中每个模式都是一个 [**PatternInterface**](/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface) 常量名称。 “类提供程序接口”列显示提供程序为向自定义 XAML 控件提供此模式而实现的接口名称。

有关如何实现用于公开控件模式和实现接口的自定义自动化对等的详细信息，请参阅[自定义自动化对等](custom-automation-peers.md)。

在实现控件模式时，还应查阅 UI 自动化提供程序文档，该文档说明了客户端在控件模式下将达到的部分预期，无论使用哪个 UI 框架实现控件模式，均不会影响这些预期。 常规 UI 自动化提供程序文档中列出的部分信息将影响对等实现的方式及正确为该模式提供支持的方式。 请参阅[实现 UI 自动化控件模式](/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns)，并查看记录了你计划实现的模式的页面。

| 控件模式 | 类提供程序接口 | 说明 |
|-----------------|--------------------------|-------------|
| **批注** | [**IAnnotationProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | 用于公开文档注释的属性。 |
| **程序坞** | [**IDockProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | 用于可在停靠容器中停靠的控件。 例如，工具栏或工具调色板。 |
| **拖动** | [**IDragProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | 用于支持可拖动控件或包含可拖动项目的控件。 |
| **DropTarget** | [**IDropTargetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | 用于支持可作为拖放操作目标的控件。 |
| **ExpandCollapse** | [**IExpandCollapseProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | 用于支持通过直观展开来显示更多内容并通过折叠来隐藏内容的控件。 |
| **网格** | [**IGridProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | 用于支持网格功能（如调整大小和移动到指定单元格）的控件。 请注意，网格本身并不实现此模式，因为它虽然提供布局，却不是控件。 |
| **GridItem** | [**IGridItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | 用于在网格内具有单元格的控件。 |
| **Invoke** | [**IInvokeProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | 用于可被调用的控件，如  [**按钮**](/uwp/api/Windows.UI.Xaml.Controls.Button)。 |
| **ItemContainer** | [**IItemContainerProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | 支持应用程序查找容器（例如虚拟化列表）中的元素。 |
| **MultipleView** | [**IMultipleViewProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | 用于可在同一组信息、数据或子级的多个表示形式之间切换的控件。 |
| **ObjectModel** | [**IObjectModelProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | 用于公开指向文档基础对象模型的指针。 |
| **RangeValue** | [**IRangeValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | 用于具有一系列可应用于该控件的值的控件。 例如，包含年份的微调框控件的数据范围可能介于 1900 至当前年份之间，而另一个呈现月份的微调框控件的数据范围介于 1 至 12 之间。 |
| **滚动** | [**IScrollProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | 用于可滚动的控件。 例如，一个控件其所具有的滚动条在控件的可视区域中存在的信息超过了可被显示的信息时，便处于活动状态。 |
| **ScrollItem** | [**IScrollItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | 用于一种控件，该控件具有可滚动列表中的各个项。 例如，一个列表控件，该控件具有滚动列表中的各个项，如组合框控件。 |
| **选择** | [**ISelectionProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | 用于选择容器控件。 例如， [**ListBox**](/uwp/api/Windows.UI.Xaml.Controls.ListBox) 和 [**ComboBox**](/uwp/api/Windows.UI.Xaml.Controls.ComboBox)。 |
| **SelectionItem** | [**ISelectionItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | 用于选择容器控件中的各个项，如列表框和组合框。 |
| **电子表格** | [**ISpreadsheetProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | 用于公开电子表格或其他基于网格的文档的内容。 |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | 用于公开电子表格或其他基于网格的文档的单元格属性。 |
| **样式** | [**IStylesProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | 用于描述具有特定样式、填充颜色、填充图案或形状的 UI 元素。 |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | 启用 UI 自动化客户端应用，将鼠标或键盘输入定向到特定的 UI 元素。 |
| **表** | [**ITableProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | 用于具有网格以及标头信息的控件。 例如，表格日历控件。 |
| **TableItem** | [**ITableItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | 用于表中的项。 |
| **Text** | [**ITextProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | 用于可公开文本信息的编辑控件和文档。 另请参阅 [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) 和 [**ITextProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider2)。 |
| **TextChild** | [**ITextChildProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | 用于访问距元素最近的支持 **Text** 控件模式的上级元素。 |
| **TextEdit** | 无可用的托管类 | 访问用于修改文本的控件，例如通过输入法编辑器 (IME) 执行自动更正或启用输入组合的控件。 |
| **TextRange** | [**ITextRangeProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | 访问实现 [**ITextProvider**](/uwp/api/windows.ui.xaml.automation.provider.itextprovider) 的文本容器中的一段连续文本。 另请参阅 [**ITextRangeProvider2**](/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2)。 |
| **切换** | [**IToggleProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | 用于在其中可切换状态的控件。 例如，可选中的 [**CheckBox**](/uwp/api/Windows.UI.Xaml.Controls.CheckBox) 和菜单项。 |
| **转换** | [**ITransformProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | 用于可调整大小、移动和旋转的控件。 Transform 控件模式通常用于设计器、窗体、图形编辑器和绘图应用程序。 |
| **值** | [**IValueProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | 允许客户端在不支持某个值范围的控件上获取或设置值。 |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | 公开容器中已虚拟化并需要作为 UI 自动化元素可完全进行访问的项目。 |
| **窗口** | [**IWindowProvider**](/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | 向 Microsoft Windows 操作系统公开特定于窗口的信息（一种基本概念）。 用作窗口的控件示例包括子窗口和对话框。 |

> [!NOTE]
> 现有的 XAML 控件中可能并不会包含所有这些模式的实现。 其中部分模式具有接口，目的仅在于通过模式的常规 UI 自动化框架定义来支持奇偶校验，以及支持需要使用纯粹自定义实现来支持该模式的自动化对等方案。

> [!NOTE]
> Windows Phone 应用商店应用不支持此处列出的所有 UI 自动化控件模式。 **Annotation** 、 **Dock** 、 **Drag** 、 **DropTarget** 、 **ObjectModel** 是一些不受支持的模式。

<span id="related_topics"/>

## <a name="related-topics"></a>相关主题  
* [自定义的自动化对等](custom-automation-peers.md)
* [辅助功能](accessibility.md)
