---
description: '本演练演示如何使用 c #/WinRT 生成 c + +/WinRT 组件的 .NET 5 投影。'
title: 用于从 c + +/WinRT 组件生成 .NET 5 投影和分发 NuGet 的演练
ms.date: 10/12/2020
ms.topic: article
keywords: 'windows 10、c #、winrt、cswinrt、投影'
ms.localizationpriority: medium
ms.openlocfilehash: 2558c37660559bb49263a5708d95ddf9086bf833
ms.sourcegitcommit: 140bbbab0f863a7a1febee85f736b0412bff1ae7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/13/2020
ms.locfileid: "91989141"
---
# <a name="walkthrough-generate-a-net-5-projection-from-a-cwinrt-component-and-distribute-the-nuget"></a>演练：从 c + +/WinRT 组件生成 .NET 5 投影并分发 NuGet

本演练演示如何使用 [c #/WinRT](index.md) 为 c + +/WinRT 组件生成 .net 5 投影，创建关联的 NuGet 包，以及从 .Net 5 c # 控制台应用程序引用 NuGet 包。

可以 [从 GitHub 下载](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)本演练的完整示例。

> [!NOTE]
> 本演练是针对 c #/WinRT (RC2) 的最新预览编写的。 未来的1.0 版本预计开发人员体验会进一步更新和改进。

## <a name="prerequisites"></a>先决条件

本演练和对应的示例需要以下工具和组件：

- 安装了通用 Windows 平台开发工作负荷的[Visual Studio 16.8 Preview 3](https://visualstudio.microsoft.com/vs/preview/) (或更高) 版本。 通用 Windows 平台开发的**安装详细信息**  >  **Universal Windows Platform development**，请参阅**c + + (v14x) 通用 Windows 平台工具**"选项。
- [.Net 5.0 RC2 SDK](https://github.com/dotnet/installer)。
- C + + [/WINRT VSIX extension](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) For c + +/WinRT 项目模板。

## <a name="create-a-simple-cwinrt-runtime-component"></a>创建一个简单的 c + +/WinRT 运行时组件

若要执行本演练，必须首先创建一个 c + +/WinRT 组件，以便为其创建 .NET 5 投影。 本演练[使用 GitHub 的](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample/SimpleMathComponent)相关示例中的**SimpleMathComponent**项目。 这是使用[c + +/WINRT VSIX 扩展](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)创建的**Windows 运行时组件 (c + +/WinRT) **项目。 将项目复制到开发计算机后，在 Visual Studio 2019 Preview 中打开解决方案。

此项目中的代码提供了下面的标头文件中所示的基本数学运算功能。 

```cpp
// SimpleMath.h
...
namespace winrt::SimpleMathComponent::implementation
{
    struct SimpleMath: SimpleMathT<SimpleMath>
    {
        SimpleMath() = default;
        double add(double firstNumber, double secondNumber);
        double subtract(double firstNumber, double secondNumber);
        double multiply(double firstNumber, double secondNumber);
        double divide(double firstNumber, double secondNumber);
    };
}
```

有关创建 c + +/WinRT 组件和生成 winmd 文件的更多详细步骤，请参阅 [使用 c + +/WinRT Windows 运行时组件](https://docs.microsoft.com/windows/uwp/winrt-components/create-a-windows-runtime-component-in-cppwinrt)。

> [!NOTE]
> 如果要在组件中实现 [IInspectable：： GetRuntimeClassName](https://docs.microsoft.com/windows/win32/api/inspectable/nf-inspectable-iinspectable-getruntimeclassname) ，则它 **必须** 返回有效的 WinRT 类名称。 由于 c #/WinRT 使用类名称字符串进行互操作，因此不正确的运行时类名称将引发 **InvalidCastException**。

## <a name="add-a-projection-project-to-the-component-solution"></a>向组件解决方案添加投影项目

如果已从存储库中克隆示例，请先删除 **SimpleMathProjection** 项目，然后按照步骤进行操作。

1. 向解决方案中添加一个新的类库 ** ( .Net Core) ** 项目。

    1. 在**解决方案资源管理器**中，右键单击解决方案节点，然后单击 "**添加**  ->  **新项目**"。
    2. 在 " **添加新项目" 对话框**中，搜索 "类库 ** ( .net Core) ** " 项目模板 "。 选择模板，然后单击 " **下一步**"。
    3. 将新项目命名为 " **SimpleMathProjection** "，然后单击 " **创建**"。

2. 从项目中删除空的 **Class1.cs** 文件。

3. 安装 [c #/WinRT NuGet 包](https://www.nuget.org/packages/Microsoft.Windows.CsWinRT)。

    1. 在 **解决方案资源管理器**中，右键单击 **SimpleMathProjection** 项目，然后选择 " **管理 NuGet 包**"。 
    2. 搜索 **CsWinRT** NuGet 包，并安装最新版本。

4. 将项目引用添加到 **SimpleMathComponent** 项目。 在**解决方案资源管理器**中，右键单击**SimpleMathProjection**项目下的 "**依赖项**" 节点，选择 "**添加项目引用**"，然后选择 " **SimpleMathComponent** " 项目。

    > [!NOTE]
    > 如果你使用的是 Visual Studio 16.8 预览版4或更高版本，则在完成步骤4后，将完成此部分。 如果你使用的是 Visual Studio 16.8 Preview 3，则还必须完成步骤5。

5. 如果使用的是 Visual Studio 16.8 Preview 3：在 **解决方案资源管理器**中，双击 " **SimpleMathProjection** " 节点以在编辑器中打开项目文件，将以下元素添加到该文件中，然后保存并关闭该文件。

    ```xml
    <ItemGroup>
      <PackageReference Include="Microsoft.Net.Compilers.Toolset" Version="3.8.0-4.20472.6" />
    </ItemGroup>

    <PropertyGroup>
      <RestoreSources>
        https://api.nuget.org/v3/index.json;
        https://pkgs.dev.azure.com/dnceng/public/_packaging/dotnet-tools/nuget/v3/index.json
      </RestoreSources>
    </PropertyGroup>
    ```

    这些元素安装所需版本的 **Microsoft.Net** NuGet 包，其中包括最新的 c # 编译器。 本演练已通过这些项目文件引用安装此 NuGet 包，因为默认公共 NuGet 源上可能未提供此包的所需版本。

完成这些步骤后， **解决方案资源管理器** 应类似于此。

![显示投影项目依赖项的解决方案资源管理器](images/projection-dependencies.png)

## <a name="edit-the-project-file-to-execute-cwinrt"></a>编辑项目文件以执行 c #/WinRT

必须先编辑投影项目的项目文件，然后才能调用 **cswinrt.exe** 并生成投影程序集。

1. 在 **解决方案资源管理器**中，双击 " **SimpleMathProjection** " 节点以在编辑器中打开项目文件。

2. 更新 `TargetFramework` 元素以引用 Windows SDK。 这会添加互操作和投影支持所需的程序集 depedencies。 我们的示例针对的是本演练中的最新的 Windows 10 版本， **net 5.0-Windows 10.0.19041.0** (也称为 SDK 版本 2004) 。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. 添加一个新 `PropertyGroup` 元素，用于设置多个 **cswinrt** 属性。

    ```xml
    <PropertyGroup>
      <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
      <CsWinRTIncludes>SimpleMathComponent</CsWinRTIncludes>
      <CsWinRTGeneratedFilesDir>$(OutDir)</CsWinRTGeneratedFilesDir>
    </PropertyGroup>
    ```

    下面是此示例中的设置的一些详细信息：

    - `AllowUnsafeBlocks`元素指定是否使用互操作代码。 
    - `CsWinRTIncludes`属性指定要投影的命名空间。
    - 此 `CsWinRTGeneratedFilesDir` 属性设置生成投影中的文件的输出目录，在下面的部分中，我们将在源中生成。

4. 保存并关闭 **SimpleMathProjection** 文件。

## <a name="build-projects-out-of-source"></a>在源外生成项目

在 [相关的示例](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)中，生成配置了 **属性** 文件。 生成的文件生成 **SimpleMathComponent** 和 **SimpleMathProjection** 项目都显示在解决方案级别的 *_build* 文件夹中。 若要将项目配置为在源外构建，请将下面的 **属性** 文件复制到包含解决方案文件的目录。

```xml
<Project>
  <PropertyGroup>
    <BuildOutDir>$([MSBuild]::NormalizeDirectory('$(SolutionDir)_build', '$(Platform)', '$(Configuration)'))</BuildOutDir>
    <OutDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'bin'))</OutDir>
    <IntDir>$([MSBuild]::NormalizeDirectory('$(BuildOutDir)', '$(MSBuildProjectName)', 'obj'))</IntDir>
  </PropertyGroup>
</Project>
```

尽管此步骤不是生成投影所必需的，但它通过从同一目录中的两个项目生成生成文件并使生成清理变得更加简单，从而简化了这一过程。 请注意，如果不构建源， **SimpleMathComponent** 和互操作程序集 **SimpleMathComponent.dll** 将在其各自的项目文件夹中的不同目录中生成。 下面是在 **SimpleMathProjection** 中引用的这些文件，因此必须相应地更改路径。

## <a name="create-a-nuget-package-from-the-projection"></a>从投影创建 NuGet 包

若要分发和使用互操作程序集，可以在生成解决方案时通过添加一些附加的项目属性来自动创建 NuGet 包。 此包将包括互操作程序集，以及针对所需 c #/WinRT 运行时程序集的 c #/WinRT NuGet 包的依赖项。 此运行时程序集命名为 .NET 5.0 目标 **winrt.runtime.dll** 。

1. 将 NuGet 规范 ( nuspec) 文件添加到 **SimpleMathProjection** 项目。

    1. 在**解决方案资源管理器**中，右键单击**SimpleMathProjection**节点，选择 "**添加**  ->  " "**新建文件夹**"，然后将文件夹命名为 " **nuget**"。 
    2. 右键单击**nuget**文件夹，选择 "**添加**  ->  **新项**"，选择 XML 文件，并将其命名为**SimpleMathProjection. nuspec**。 

2. 将以下项添加到 **SimpleMathProjection** ，以自动生成包。 这些属性指定 `NuspecFile` 和用于生成 NuGet 包的目录。

    ```xml
    <PropertyGroup>
      <GeneratedNugetDir>.\nuget\</GeneratedNugetDir>
      <NuspecFile>$(GeneratedNugetDir)SimpleMathProjection.nuspec</NuspecFile>
      <OutputPath>$(GeneratedNugetDir)</OutputPath>
      <GeneratePackageOnBuild>true</GeneratePackageOnBuild>
    </PropertyGroup>

3. Open the **SimpleMathProjection.nuspec** file to edit the package creation properties. Below is an example of a C++/WinRT component NuGet spec. Notice the `dependency` on CsWinRT for the `net5.0` target framework moniker, as well as the target for `lib\net5.0\SimpleMathProjection.dll`, which points to the projection assembly **SimpleMathComponent.dll** instead of **SimpleMathComponent.winmd**. This behavior is new in .NET 5.0 and enabled by C#/WinRT.

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <package xmlns="http://schemas.microsoft.com/packaging/2012/06/nuspec.xsd">
      <metadata>
        <id>SimpleMathComponent</id>
        <version>0.1.0-prerelease</version>
        <authors>Contoso Math Inc.</authors>
        <description>A simple component with basic math operations</description>
        <dependencies>
          <group targetFramework=".NETCoreApp3.0" />
          <group targetFramework="UAP10.0" />
          <group targetFramework=".NETFramework4.6" />
          <group targetFramework="net5.0">
            <dependency id="Microsoft.Windows.CsWinRT" version="0.8.0" exclude="Build,Analyzers" />
          </group>
        </dependencies>
      </metadata>
      <files>
        <!--Support net46+, netcore3, net5, uap, c++ -->
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\netcoreapp3.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\uap10.0\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.winmd" target="lib\net46\SimpleMathComponent.winmd" />
        <file src="..\..\_build\x64\Debug\SimpleMathProjection\bin\SimpleMathProjection.dll" target="lib\net5.0\SimpleMathProjection.dll" />
        <file src="..\..\_build\x64\Debug\SimpleMathComponent\bin\SimpleMathComponent\SimpleMathComponent.dll" target="runtimes\win10-x64\native\SimpleMathComponent.dll" />
      </files>
    </package>
    ```

## <a name="build-the-solution-to-generate-the-projection-and-nuget-package"></a>生成用于生成投影和 NuGet 包的解决方案

此时，你可以构建解决方案：右键单击解决方案节点，然后选择 " **生成解决方案**"。 这将首先生成组件项目，然后生成投影项目。 除了组件项目中的元数据文件外，还将在输出目录中生成互操作 **.cs** 文件和程序集。 你还将能够在**nuget**文件夹中看到生成的 Nuget 包**simplemathcomponent 0.1.0。**

![显示投影生成的解决方案资源管理器](images/projection-generated-files.png)

## <a name="referencethenugetpackage-inacnet50consoleapplication"></a>在 c # .NET 5.0 控制台应用程序中引用 NuGet 包

若要使用预计的 **SimpleMathComponent**，只需在应用程序中添加对新创建的 NuGet 包的引用。 以下步骤演示了如何通过在单独的解决方案中创建一个简单的控制台应用来实现此目的。

1. 使用 **控制台应用 ( .Net Core) ** 项目创建新解决方案。

    1. 在 Visual Studio 中，选择“文件” -> “新建” -> “项目”。
    2. 在 " **添加新项目" 对话框**中，在 " **.net Core) ** 项目" 模板 ( 搜索控制台应用。 选择模板，然后单击 " **下一步**"。
    3. 将新项目命名为 " **SampleConsoleApp** "，然后单击 " **创建**"。 将此项目创建到新的解决方案中，可以单独还原 **SimpleMathComponent** NuGet 包。

2. 在 **解决方案资源管理器**中，双击 " **SampleConsoleApp** " 节点以打开 **SampleConsoleApp** 项目文件，并更新 "目标框架名字对象" 和 "平台配置"，如以下示例中所示。

    ```xml
    <PropertyGroup>
      <TargetFramework>net5.0-windows10.0.19041.0</TargetFramework>
      <Platforms>x64</Platforms>
    </PropertyGroup>
    ```

3. 将 **SimpleMathComponent** NuGet 包添加到 **SampleConsoleApp** 项目。 还需要 [VCRTForwarders](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/) NuGet 包，这些包在未打包在 .msix 包中的应用程序中是必需的。 若要在生成项目时还原 **SimpleMathComponent** NuGet，可以将 `RestoreSources` 属性与组件解决方案中 **NuGet** 文件夹的路径一起使用。

    ```xml
    <PropertyGroup>
      <RestoreSources>
          https://api.nuget.org/v3/index.json;
          ../../CppWinRTProjectionSample/SimpleMathProjection/nuget
      </RestoreSources>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="Microsoft.VCRTForwarders.140" Version="1.0.6" />
        <PackageReference Include="SimpleMathComponent" Version="0.1.0-prerelease" />
    </ItemGroup>
    ```

    请注意，在本演练中， **SimpleMathComponent** 的 NuGet 还原路径假定两个解决方案文件位于同一个目录中。 或者，你可以向解决方案中 [添加本地 NuGet 包源](https://docs.microsoft.com/nuget/consume-packages/install-use-packages-visual-studio#package-sources) 。

4. 编辑 **Program.cs** 文件，以使用 **SimpleMathComponent**提供的功能。

    ```csharp
    static void Main(string[] args)
    {
        var x = new SimpleMathComponent.SimpleMath();
        Console.WriteLine("Adding 5.5 + 6.5 ...");
        Console.WriteLine(x.add(5.5, 6.5).ToString());
    }
    ```

5. 生成并运行控制台应用。 应会看到以下输出。

    ![控制台 NET5 输出](images/console-output.png)

## <a name="resources"></a>资源

- [此演练的完整代码示例](https://github.com/microsoft/CsWinRT/tree/master/Samples/Net5ProjectionSample)
