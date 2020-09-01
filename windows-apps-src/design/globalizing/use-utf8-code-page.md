---
Description: 使用 UTF-8 字符编码，以便在 web 应用和其他基于 nix 的平台之间实现最佳兼容性 \* (Unix、Linux 和变型) ，最小化本地化 bug 并降低测试开销。
title: 使用 Windows UTF-8 代码页
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, 全球化, 可本地化性, 本地化
ms.localizationpriority: medium
ms.openlocfilehash: 72e422ee3e1a911658b2fe4957967aeba116c353
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173461"
---
# <a name="use-the-utf-8-code-page"></a>使用 UTF-8 代码页

使用 [utf-8](http://www.utf-8.com/) 字符编码，以便在 web 应用和其他基于 nix 的平台之间实现最佳兼容性 \* (Unix、Linux 和变型) ，最小化本地化 bug 并降低测试开销。

UTF-8 是国际化的通用代码页，可以编码整个 Unicode 字符集。 它在 web 上使用 pervasively，是基于 * nix 的平台的默认值。

> [!NOTE]
> 编码的字符采用1到4个字节。 UTF-8 编码支持较长的字节序列（最多6个字节），但 Unicode 6.0 (U + 10FFFF 的最大码位) 只需要4个字节。

## <a name="-a-vs--w-apis"></a>-A 与-W Api
  
Win32 Api 通常同时支持-A 和-W 变体。

-一个变量识别在系统和支持上配置的 ANSI 代码页 `char*` ，而-W 变体在 utf-16 和支持中运行 `WCHAR` 。

到目前为止，Windows 一直强调了 "Unicode"-W 变体（Api）。 但是，最新版本已使用 ANSI 代码页和-A Api 作为将 UTF-8 支持引入应用的一种方法。 如果 ANSI 代码页配置了 UTF-8，则 Api 在 UTF-8 中运行。 此模型的优点是，支持使用-A Api 生成的现有代码，而无需进行任何代码更改。

## <a name="set-a-process-code-page-to-utf-8"></a>将进程代码页设置为 UTF-8

从 Windows 版本 1903 (2019 更新) ，你可以使用打包应用的 appxmanifest.xml 中的 ActiveCodePage 属性，或者使用未打包的应用的合成清单来强制进程使用 UTF-8 作为过程代码页。

您可以声明此属性，并在早期的 Windows 版本上运行，但您必须像平常一样处理旧的代码页检测和转换。 使用最低目标版本的 Windows 版本1903，进程代码页将始终为 UTF-8，因此可以避免旧的代码页检测和转换。

## <a name="examples"></a>示例

**打包应用的 Appx 清单：**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**未打包的 Win32 应用的合成清单：**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> 使用从命令行向现有的可执行文件添加清单 `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>代码页转换

当 Windows 以 UTF-16 () 运行时 `WCHAR` ，您可能需要将 utf-8 数据转换为 utf-16 (，反之亦然) 以与 Windows api 进行互操作。

[MultiByteToWideChar](/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) 和 [WideCharToMultiByte](/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) 使你能够在 utf-8 和 utf-16 (`WCHAR`)  (和其他代码页) 之间进行转换。 当旧 Win32 API 只能理解时，此方法特别有用 `WCHAR` 。 这些函数允许你将 UTF-8 输入转换为，将其 `WCHAR` 传递到 W API，然后在必要时转换回结果。
在将这些函数与 `CodePage` 设置为时使用时 `CP_UTF8` ，如果使用 `dwFlags` `0` 或，则会发生这种 `MB_ERR_INVALID_CHARS` `ERROR_INVALID_FLAGS` 情况。

> [!NOTE]
> `CP_ACP``CP_UTF8`仅当在 Windows 版本1903上运行时 (可能2019更新) 或更高版本，并且以上所述的 ActiveCodePage 属性设置为 utf-8。 否则，它会采用旧的系统代码页。 建议显式使用 `CP_UTF8` 。

## <a name="related-topics"></a>相关主题

- [代码页](/windows/desktop/Intl/code-pages)
- [代码页标识符](/windows/desktop/Intl/code-page-identifiers)