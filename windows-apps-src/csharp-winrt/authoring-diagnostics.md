---
description: '用于创作组件的 c #/WinRT 诊断错误消息'
title: '诊断 c #/WinRT 组件错误'
ms.date: 01/27/2021
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8d1e5eda34a8cbce70edc812ba27437090393ca0
ms.sourcegitcommit: 457239a6907198bc2948322a07c484dd8a170b33
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99230015"
---
# <a name="diagnose-cwinrt-component-errors"></a>诊断 c #/WinRT 组件错误

本文提供了有关用 c #/WinRT. 编写的 Windows 运行时组件的限制的其他信息 它在作者生成组件时，展开 c #/WinRT 中的错误消息中提供的信息。 对于现有 UWP .NET Native 托管组件，c # WinRT 组件的元数据是使用 [Winmdexp.exe](/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool).net 工具生成的。 现在 Windows 运行时支持与 .NET 分离，c #/WinRT 提供了从组件生成 winmd 文件的工具。 Windows 运行时对代码的约束比 c # 类库多，c #/WinRT 诊断扫描程序会在生成 winmd 文件之前向您发出警报。

本文介绍了 c # 中的生成所报告的错误/WinRT。 本文作为使用 Winmdexp.exe 工具的 [现有 UWP .NET Native 托管组件](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md) 的限制的信息更新版本。

搜索错误消息文本 (省略占位符) 或消息编号的特定值。 如果在此处找不到所需的信息，可以通过本文末尾的 "反馈" 按钮，帮助我们改进文档。 请在反馈中提供错误消息。 或者，你可以在 [c #/WinRT](https://github.com/microsoft/CsWinRT)存储库中提交 bug。

本文按方案组织错误消息。

## <a name="implementing-interfaces-that-arent-valid-windows-runtime-interfaces"></a>实现的接口不是有效的 Windows 运行时接口

C #/WinRT 组件无法实现某些 Windows 运行时接口，如表示异步操作或操作的 Windows 运行时接口 (**IAsyncAction**、 **\<TProgress\> iasyncactionwithprogress<tprogress>**、 **iasyncoperation<tresult> \<TResult\>** 或 **IAsyncOperationWithProgress \<TResult,TProgress\>**) 。 相反，请使用 **system.runtime.interopservices.windowsruntime.asyncinfo** 类在 Windows 运行时组件中生成异步操作。 注意：这些不是唯一的无效接口，例如类 **无法实现 system.exception。**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1008</p></td>
<td><p>Windows 运行时组件类型 {0} 无法实现接口 {1} ，因为接口不是有效的 Windows 运行时接口</p></td>
</tr>
</tbody>
</table>

## <a name="attribute-related-errors"></a>与属性相关的错误

在 Windows 运行时中，只有当一个重载指定为默认重载时，重载方法才可以具有相同数量的参数。 使用特性 **DefaultOverload** (CsWinRT1015，1016) 。

当数组作为函数或属性中的输入或输出使用时，它们必须为只读或只写 (CsWinRT 1025) 。 提供 **InteropServices. WindowsRuntime. ReadOnlyArray** 和 **InteropServices** . WindowsRuntime. WriteOnlyArray。是为你使用而提供的。 提供的属性仅可用于数组类型 (CsWinRT1026) 的参数，并且每个参数 (CsWinRT1023) 仅应用一个。

不需要将任何属性应用 **于标记为的数组** 参数，因为假定它是只写的。 如果在这种情况下将其视为只读，则会出现一条错误消息， (CsWinRT1024) 。

不应在 (CsWinRT1021，1022) 的任何类型的参数上使用特性 **InteropServices。 InAttribute** 和 **InteropServices。**

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1015</p></td>
<td><p>在类中 {2} ： {0} "" 的多参数重载 {1} 通过 windows.foundation.metadata.defaultoverloadattribute 进行修饰。 特性只能应用于方法的一个重载。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1016</p></td>
<td><p>在类中 {2} ：的 {0} -参数重载 {1} 必须有一个指定为默认重载的方法，方法是使用特性 windows.foundation.metadata.defaultoverloadattribute 进行修饰。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1021</p></td>
<td><p>方法 " {0} " 具有作为数组的参数 "" {1} ，并且该参数具有 InteropServices 或 InAttribute 或 InteropServices。 system.runtime.interopservices.outattribute。 在 Windows 运行时中，数组参数必须具有 ReadOnlyArray 或 WriteOnlyArray。 请删除这些属性，或使用相应的 Windows 运行时属性替换它们（如有必要）。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1022</p></td>
<td><p>方法 " {0} " 的参数 "" 具有 {1} InteropServices 或 InAttribute 或 InteropServices。 Windows 运行时 system.runtime.interopservices.outattribute 不支持将参数标记为 InteropServices 或 InAttribute。 InteropServices. system.runtime.interopservices.outattribute.. 或。 请考虑删除 System.Runtime.InteropServices.InAttribute，并改为使用“out”修饰符替换 System.Runtime.InteropServices.OutAttribute。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1023</p></td>
<td><p>方法 " {0} " 具有作为数组的参数 "" {1} ，并且它同时具有 ReadOnlyArray 和 WriteOnlyArray。 在 Windows 运行时中，数组参数内容必须是可读或可写。 请从“{1}”中删除其中一个属性。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1024</p></td>
<td><p>方法 " {0} " 的输出参数 "" {1} 是一个数组，但它具有 ReadOnlyArray 特性。 在 Windows 运行时中，可写入输出数组的内容。  请从“{1}”中删除该属性。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1025</p></td>
<td><p>方法 " {0} " 具有作为数组的参数 " {1} "。 在 Windows 运行时中，数组参数的内容必须是可读或可写。 请将 ReadOnlyArray 或 WriteOnlyArray 应用于 " {1} "。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1026</p></td>
<td><p>方法 " {0} " 的参数 " {1} " 不是数组，并且具有 ReadOnlyArray 特性或 WriteOnlyArray 特性。 Windows 运行时不支持用 ReadOnlyArray 或 WriteOnlyArray 标记非数组参数。 "</p></td>
</tr>
</tbody>
</table>

## <a name="namespace-errors-and-invalid-names-for-the-output-file"></a>输出文件的命名空间错误和无效名称

在 Windows 运行时中，Windows 元数据 ( winmd) 文件中的所有公共类型必须位于共享 winmd 文件名的命名空间中，或位于文件名的子命名空间中。 例如，如果你的 Visual Studio 项目 (命名为 Class2，则 Windows 运行时组件为 a.class3) ，它可以包含公共类 d.class4 和，但不能包含或等公共类。）的。

> [!NOTE]
> 这些限制仅适用于公共类型，不适用于在实现中使用的私有类型。

如果是 A.class3，则可以将 A.class3 移到另一个命名空间，或将 Windows 运行时组件的名称更改为 winmd。 在之前的示例中，调用 A.B.winmd 的代码将无法找到 A.Class3。

如果是 D.class4，则不能在 D.class4 命名空间中同时包含和类，因此不能选择更改 Windows 运行时组件的名称。 你可以将 D.class4 移动到另一个命名空间，或将其放在另一个 Windows 运行时组件中。

文件系统无法区分大小写，因此不允许 (CsWinRT1002) 区分大小写不同的命名空间。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1001</p></td>
<td><p>公共类型具有命名空间 ( " {1} " ) ，该命名空间与其他命名空间 ( "" ) 不共享公共前缀 {0} 。 Windows 元数据文件中的所有类型必须存在于由文件名暗示的命名空间的子命名空间中。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1002</p></td>
<td><p>找到多个名称为 "" 的命名空间 {0} ; 命名空间名称不能仅在 Windows 运行时中区分大小写。</p></td>
</tr>
</tbody>
</table>

## <a name="exporting-types-that-arent-valid-windows-runtime-types"></a>导出不是有效的 Windows 运行时类型的类型

组件的公共接口必须仅公开 Windows 运行时类型。 但是，.NET 提供了许多常用类型的映射，这些类型在 .NET 和 Windows 运行时中稍有不同。 这使 .NET 开发人员能够使用熟悉的类型，而不是学习新的类型。 你可以在组件的公共接口中使用这些映射的 .NET Framework 类型。 有关详细信息，请参阅 [在 Windows 运行时组件中声明类型](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#declaring-types-in-windows-runtime-components)、 [将 Windows 运行时类型传递给托管代码](../winrt-components/creating-windows-runtime-components-in-csharp-and-visual-basic.md#passing-windows-runtime-types-to-managed-code)和 [Windows 运行时类型的 .net 映射](../winrt-components/net-framework-mappings-of-windows-runtime-types.md)。

其中许多映射都是接口。 例如，IList \<T\> 映射到 Windows 运行时接口 IVector \<T\> 。 如果使用 List \<string\> 而不是 IList \<string\> 作为参数类型，c #/WinRT 将提供一系列替代项列表，其中包括由列表实现的所有映射接口 \<T\> 。 如果使用嵌套的泛型类型，如 List \<Dictionary\<int, string\> \> ，则 c #/WinRT 为每个嵌套级别提供选择。 这些列表会非常长。

通常情况下，最好选择最接近类型的接口。 例如，对于字典 \<int, string\> ，最佳选择是最可能的 IDictionary \<int, string\> 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1006</p></td>
<td><p>成员 " {0} " 的签名中具有类型 "" {1} 。 类型 " {1} " 不是有效的 Windows 运行时类型。 但类型 (或其泛型参数) 实现有效 Windows 运行时类型的接口。 请考虑将成员签名中的类型 "更改 {1} 为以下类型之一： {2} 。</p></td>
</tr>
</tbody>
</table>

在 Windows 运行时中，成员签名中的数组必须是下限为 0 (零) 的一维。 `myArray[][]`不允许使用嵌套数组类型，如 (CsWinRT1017) 和 `myArray[,]` (CsWinRT1018) 。

> [!NOTE]
> 此限制不适用于在实现内部使用的数组。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1017</p></td>
<td><p>方法的 {0} 签名中包含类型的嵌套数组 {1} 。 不能嵌套 Windows 运行时方法签名中的数组。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1018</p></td>
<td><p>方法 " {0} " {1} 在其签名中具有类型为 "" 的多维数组。 Windows 运行时方法签名中的数组必须是一维数组。</p></td>
</tr>
</tbody>
</table>

## <a name="structures-that-contain-fields-of-disallowed-types"></a>包含禁止类型的字段的结构

在 Windows 运行时中，结构只能包含字段，并且只有结构可以包含字段。 这些字段必须是公共字段。 有效字段类型包括枚举、结构和基元类型。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1007</p></td>
<td><p>结构 {0} 不包含公共字段。 Windows 运行时结构必须包含至少一个公共字段。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1011</p></td>
<td><p>结构 {0} 具有非公共字段。 对于 Windows 运行时结构，所有字段都必须是公共的。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1012</p></td>
<td><p>结构 {0} 包含 const 字段。 常量只能出现在 Windows 运行时枚举中。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1013</p></td>
<td><p>结构 {0} 具有类型为的字段 {1} ; {1} 不是有效的 Windows 运行时字段类型。 Windows 运行时结构中的每个字段仅可以是 UInt8、Int16、UInt16、Int32、UInt32、Int64、UInt64、单精度、双精度、布尔值、字符串、Enum，或其本身就是结构。</p></td>
</tr>
</tbody>
</table>

## <a name="parameter-name-conflicts-with-generated-code"></a>参数名称与生成的代码冲突

在 Windows 运行时中，返回值被视为输出参数，并且参数的名称必须是唯一的。 默认情况下，c #/WinRT 为返回值提供名称 `__retval` 。 如果你的方法有一个名为的参数 `__retval` ，则将收到错误 CsWinRT1010。 若要更正此错误，请为参数指定一个以外的名称 `__retvalue` 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1010</p></td>
<td><p>方法中的参数名称与 {1} {0} 生成的 c #/WinRT 互操作中使用的返回值参数名称相同; 请使用不同的参数名称。</p></td>
</tr>
</tbody>
</table>

## <a name="miscellaneous"></a>杂项

C #/WinRT 创作组件中的其他限制包括：

- 不能在公共类型上公开重载运算符。
- 类和接口不能是泛型的。
- 类必须是密封的。
- 参数不能通过引用传递，例如，使用 **ref** 关键字。
- 属性必须具有公共 get 方法。
- 组件的命名空间中必须至少有一个公共类型 (类或接口) 。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1014</p></td>
<td><p>" {0} " 是运算符重载。 托管的类型无法在 Windows 运行时中公开运算符重载。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1004</p></td>
<td><p>类型 {0} 为泛型。 Windows 运行时类型不能为泛型。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1005</p></td>
<td><p>CsWinRT 中不支持导出非密封类型。 请将类型标记 {0} 为密封。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1020</p></td>
<td><p>方法 " {0} " 已标记参数 "" {1} `ref` 。 Windows 运行时中不允许使用引用参数。</p></td>
</tr>
<tr class="odd">
<td><p>CsWinRT1000</p></td>
<td><p>属性 " {0} " 没有公共 getter 方法。 Windows 运行时不支持仅限 setter 的属性。</p></td>
</tr>
<tr class="even">
<td><p>CsWinRT1003</p></td>
<td><p>Windows 运行时组件必须至少具有一个公共类型</p></td>
</tr>
</tbody>
</table>

在 Windows 运行时中，一个类只能有一个具有给定数量的参数的构造函数。 例如，您不能让一个构造函数具有一个 **string** 类型的参数，另一个构造函数具有类型为 **int** 的单一参数。唯一的解决方法是对每个构造函数使用不同数量的参数。

<table>
<colgroup>
<col style="width: 50%" />
<col style="width: 50%" />
</colgroup>
<thead>
<tr class="header">
<th><p>错误号</p></th>
<th><p>消息正文</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>CsWinRT1009</p></td>
<td><p>在 Windows 运行时中，类不能有多个具有相同 arity 的构造函数，类 {0} 有多个 {1} arity 构造函数。</p></td>
</tr>
</tbody>
</table>


