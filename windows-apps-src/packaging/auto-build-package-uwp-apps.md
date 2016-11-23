---
author: rmpablos
title: "设置 UWP 应用的自动生成"
description: "如何将自动生成配置为生成旁加载和/或存储软件包。"
translationtype: Human Translation
ms.sourcegitcommit: 1e30c20b38b9e6b00cd4f56bf9ffb3752ceed6a9
ms.openlocfilehash: 681019d708b859d117992cde6d2ad3d37fa4833a

---
# 设置 UWP 应用的自动生成

可使用 Visual Studio Team Services (VSTS) 为 UWP 项目创建自动生成。 在本文中，我们将讨论执行该操作的方法。  我们还将介绍如何使用命令行执行这些任务，以便你可以与其他生成系统（AppVeyor）集成。 

## 选择正确类型的生成代理

选择你希望 VSTS 在其执行生成过程时使用的生成代理类型。 托管生成代理与大多数常见工具和 SDK 一起部署，并且适用于大多数方案，请参阅[托管生成服务器上的软件](https://www.visualstudio.com/en-us/docs/build/admin/agents/hosted-pool#software)文章。 但是，如果你需要对生成步骤的更多控制权，可创建自定义生成代理。 可使用下表帮助你进行该决策。

|**方案**|**自定义代理**|**托管生成代理**|
-------------|----------------|----------------------|
|基本 UWP 版本（包括 .NET 本机）|:white_check_mark:|:white_check_mark:|
|生成软件包用于旁加载|:white_check_mark:|:white_check_mark:|
|生成软件包用于应用商店提交|:white_check_mark:|:white_check_mark:|
|使用自定义证书|:white_check_mark:||
|面向自定义 Windows SDK 生成|:white_check_mark:||
|运行单元测试|:white_check_mark:||
|使用增量生成|:white_check_mark:||

>注意：如果你计划面向 Windows 周年更新 SDK（版本 14393），你将需要设置自定义生成代理，因为托管生成池仅支持 SDK 10586 和 10240。 用于[选择 UWP 版本](https://msdn.microsoft.com/en-us/windows/uwp/updates-and-versions/choose-a-uwp-version)的更多信息

#### 创建自定义生成代理（可选）

如果你选择创建自定义生成代理，你将需要通用 Windows 平台工具。 这些工具是 Visual Studio 的一部分。 可使用 Visual Studio 的社区版本。

若要了解详细信息，请参阅[在 Windows 上部署代理。](https://www.visualstudio.com/en-us/docs/build/admin/agents/v2-windows) 

若要运行 UWP 单元测试，你必须执行以下操作：• 部署并启动应用。 • 在交互模式下运行 VSTS 代理。 • 将代理配置为在重启后自动登录。

现在我们将讨论如何设置自动生成。

## 设置自动生成
我们将从 VSTS 中提供的默认 UWP 生成定义开始，然后介绍如何配置该定义，以便你可以完成更高级的生成任务。

**将项目证书添加到源代码存储库**

VSTS 适用于基于 TFS 和 GIT 的代码存储库。  
如果使用 Git 存储库，请将项目的证书文件添加到存储库，以便生成代理可以对 appx 包签名。 如果不执行此操作，则 Git 存储库将忽略证书文件。 若要将证书文件添加到存储库，请在解决方案资源管理器中右键单击该证书文件，然后在快捷菜单中选择“将忽略的文件添加到源控件”命令。 

![如何包含证书](images/building-screen1.png)

我们将在本指南的后面部分讨论[高级证书管理](#certificates-best-practices)。 

若要在 VSTS 中创建你的第一个生成定义，请导航到“生成”选项卡，然后选择 + 按钮。

![创建生成定义](images/building-screen2.png)

在生成定义模板列表中，选择“通用 Windows 平台”模板。

![选择 UWP 生成](images/building-screen3.png)

此生成定义包含以下生成任务：

- NuGet 还原 **\*.sln
- 生成解决方案 **\*.sln
- 发布符号
- 发布项目：drop

#### 配置 NuGet 还原生成任务

此任务还原在项目中定义的 NuGet 包。 某些软件包需要 NuGet.exe 的自定义版本。 如果你使用的是需要自定义版本的软件包，请在存储库中参考该版本 NuGet.exe，然后在 *Path to NuGet.exe* 高级属性中引用它。

![默认生成定义](images/building-screen4.png)

#### 配置生成解决方案生成任务

此任务将工作文件夹中的任何解决方案编译为二进制文件，并生成输出 AppX 文件。 此任务使用 MSbuild 参数。  你必须指定这些参数的值。 使用下表作为指南。 

|**MSBuild 参数**|**值**|**说明**|
|--------------------|---------|---------------|
|AppxPackageDir|$(Build.ArtifactStagingDirectory)\AppxPackages|定义要存储生成的项目的文件夹。|
|AppxBundlePlatforms|$(Build.BuildPlatform)|允许你定义要包含在程序包中的平台。|
|AppxBundle|始终|为指定平台创建带有 appx 文件的 appxbundle。|
|**UapAppxPackageBuildMode**|StoreUpload|定义要生成的 appx 包类型。 （默认不包含）|


如果你希望通过使用命令行或使用任何其他生成系统生成解决方案，请使用这些参数运行 msbuild。

```
/p:AppxPackageDir="$(Build.ArtifactStagingDirectory)\AppxPackages\\"  
/p:UapAppxPackageBuildMode=StoreUpload 
/p:AppxBundlePlatforms="$(Build.BuildPlatform)"
/p:AppxBundle=Always
```

使用 $() 语法定义的参数是在生成定义中定义的变量，并且将在其他生成系统中更改。

![默认变量](images/building-screen5.png)

若要查看所有预定义的变量，请参阅[使用生成变量](https://www.visualstudio.com/docs/build/define/variables)。

#### 配置发布项目生成任务 
此任务将生成的项目存储在 VSTS 中。 可以在生成结果页面的“项目”选项卡中看到它们。 VSTS 使用我们之前定义的 `$Build.ArtifactStagingDirectory)\AppxPackages` 文件夹。

![项目](images/building-screen6.png)

由于我们已将 `UapAppxPackageBuildMode` 属性设置为 `StoreUpload`，因此项目文件夹包含你上传到应用商店的软件包 (appxupload) 以及启用旁加载的软件包 (appxbundle)。


>注意：默认情况下，VSTS 代理保留最新的 appx 生成的软件包。 如果你希望仅存储当前生成的项目，请将该生成配置为清除二进制文件目录。 若要执行此操作，请添加一个名为 `Build.Clean` 的变量，然后将其设置为值 `all`。 若要了解详细信息，请参阅[指定存储库。](https://www.visualstudio.com/en-us/docs/build/define/repository#how-can-i-clean-the-repository-in-a-different-way)

#### 自动生成的类型
接下来，你将使用生成定义创建自动生成。 下表介绍可创建的每种类型的自动生成。 

|**生成类型**|**项目**|**建议的频率**|**说明**|
|-----------------|------------|-------------------------|---------------|
|连续集成|生成日志、测试结果|每个提交|此类型的生成很快，并且一天运行多次。|
|用于旁加载的连续部署生成|部署包|每天 |此类型的生成包含单元测试，但需要较长时间。 它允许手动测试，并且你可以将其与其他工具（如 HockeyApp）集成。|
|将软件包提交到应用商店的连续部署生成|发布软件包|按需|此类型的生成创建可发布到应用商店的软件包。|

让我们来查看如何配置其中每一个。


## 设置连续集成 (CI) 生成 
此类型的生成帮助你快速诊断与代码相关的问题。 通常仅为单个平台执行它们，并且不需要 .NET 本机工具链处理它们。 此外，使用 CI 生成，你可以运行单元测试，该测试生成测试结果报告。  

如果要将 UWP 单元测试作为 CI 生成的一部分运行，你将需要使用自定义生成代理，而不是托管生成代理。

>请注意：如果将多个应用捆绑在同一个解决方案中，你可能收到错误。 有关解决该错误的帮助，请参阅以下主题：[解决将多个应用捆绑在同一个解决方案中时出现的错误。](#bundle-errors) 


### 配置 CI 生成定义
使用默认 UWP 模板创建生成定义。 然后，配置在每次签入时执行的触发器。  

![CI 触发器](images/building-screen7.png)

由于 CI 生成不会部署到用户，因此保留不同的版本控制编号以避免与其他 CD 生成混淆是一个好主意。 例如：`$(BuildDefinitionName)_0.0.$(DayOfYear)$(Rev:.r)`。


#### 为单元测试配置自定义生成代理

1.首先，在电脑上启用开发人员模式。 请参阅启用设备进行开发。 2.允许服务作为交互式进程运行。 请参阅在 Windows 上部署代理。 3.将签名证书部署到代理。

若要执行该操作，请双击该 .cer 文件、选择“本地计算机”，然后选择“受信任人存储”。

<span id="uwp-unit-tests" />
### 配置生成定义以运行 UWP 单元测试
若要执行单元测试，请使用 Visual Studio 测试生成步骤。


![添加单元测试](images/building-screen8.png)

UWP 单元测试在给定 appx 文件的上下文中执行，因此你无法使用生成的程序包。 此外，你必须指定具体平台 appx 文件的路径。 例如：

```
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp.UnitTest\x86\MyUWPApp.UnitTest_$(AppxVersion)_x86.appx
```

>注意：使用以下命令从命令行本地执行单元测试：
`"%ProgramFiles(x86)%\Microsoft Visual Studio 14.0\Common7\IDE\CommonExtensions\Microsoft\TestWindow\vstest.console.exe"`

#### 访问测试结果
在 VSTS 中，生成摘要页显示执行单元测试的每个生成的测试结果。  从该处，你可以打开“测试结果”页以查看关于测试结果的更多详细信息。 

![测试结果](images/building-screen9.png)

#### 提高 CI 生成的速度
如果要将 CI 生成仅用于监视签入的质量，可以缩短生成时间。

#### 提高 CI 生成的速度
1.  仅针对单个平台生成。
2.  编辑 BuildPlatform 变量以仅使用 x86。 ![配置 CI](images/building-screen10.png) 
3.  在生成步骤中，将 /p:AppxBundle=Never 添加到 MSBuild Arguments 属性，然后设置 Platform 属性。 ![配置平台](images/building-screen11.png)
4.  在单元测试项目中，禁用 .NET 本机。 

若要执行此操作，请打开项目文件，然后在项目属性中将 `UseDotNetNativeToolchain` 属性设置为 `false`。

>注意。 使用 .NET 本机工具链仍然是工作流的重要部分，因此你应使用它测试版本生成。 

<span id="bundle-errors" />
#### 解决将多个应用捆绑在同一个解决方案中时出现的错误。 
如果将多个 UWP 项目添加到解决方案，然后尝试创建程序包，则可能收到如下错误： 

```
MakeAppx(0,0): Error : Error info: error 80080204: The package with file name "AppOne.UnitTests_0.1.2595.0_x86.appx" and package full name "8ef641d1-4557-4e33-957f-6895b122f1e6_0.1.2595.0_x86__scrj5wvaadcy6" is not valid in the bundle because it has a different package family name than other packages in the bundle
```

出现此错误是因为，在解决方案级别上，哪个应用应出现在程序包中不明确。 若要解决此问题，请打开每个项目文件，并在第一个 `<PropertyGroup>` 元素的末尾添加以下属性：

|**项目**|**属性**|
|-------|----------|
|应用|`<AppxBundle>Always</AppxBundle>`|
|UnitTests|`<AppxBundle>Never</AppxBundle>`|

然后，从生成步骤中删除 `AppxBundle` msbuild 参数。

## 设置连续部署生成用于旁加载
当此类型的生成完成时，用户可以从生成结果页的项目部分下载 appxbundle 文件。 如果要通过创建更完整的分配来对应用进行 beta 测试，可使用 HockeyApp 服务。 此服务提供用于 beta 测试、用户分析和崩溃诊断的高级功能。


### 将版本号应用到生成

清单文件包含应用版本号。  更新源控件存储库中的清单文件以更改版本号。 更新应用版本号的另一个方法是使用 VSTS 生成的生成号，然后在编译应用前修改应用清单。 不要将这些更改提交到源代码存储库。

在编译前，你必须在生成定义中定义版本控制生成号格式，然后使用所生成的版本号更新 AppxManifest 和（可选）AssemblyInfo.cs 文件。

在生成定义的“常规”选项卡中定义生成号格式：

![生成版本](images/building-screen12.png) 

例如，如果将生成号格式设置为以下值：  
``` 
$(BuildDefinitionName)_1.1.$(DayOfYear)$(Rev:r).0 
```

VSTS 生成如下版本号：
```
CI_MyUWPApp_1.1.2501.0
```

>注意：应用商店将要求版本中的最后一个数字为 0。

以便你可以提取版本号并将其应用到清单和/或 `AssemblyInfo` 文件，使用自定义 PowerShell 脚本（在[此处](https://go.microsoft.com/fwlink/?prd=12560&pver=14&plcid=0x409&clcid=0x9&ar=DevCenter&sar=docs)提供）。 该脚本从环境变量 `BUILD_BUILDNUMBER` 中读取版本号，然后修改 AssemblyInfo 和 AppxManifest 文件。 确保将此脚本添加到源存储库，然后配置 PowerShell 生成任务，如下所示：


![更新版本](images/building-screen13.png) 


            `$(AppxVersion)` 变量包含版本号。 可在其他生成步骤中使用该编号。 


#### 可选：与 HockeyApp 集成
首先，安装 [HockeyApp](https://marketplace.visualstudio.com/items?itemName=ms.hockeyapp) Visual Studio 扩展。 

>注意：你必须以 VSTS 管理员身份安装此扩展。 


![hockey app](images/building-screen14.png) 

接下来，使用本指南配置 HockeyApp 连接：[如何将 HockeyApp 与 Visual Studio Team Services (VSTS) 或 Team Foundation Server (TFS) 一起使用。](https://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs) 可使用 Microsoft 帐户、社交媒体帐户或仅仅一个电子邮件地址设置 HockeyApp 帐户。 免费计划附带两个应用、一个所有者，并且没有数据限制。

然后，可以手动或通过上传现有 appx 包文件创建 HockeyApp 应用。 若要了解详细信息，请参阅[如何创建新应用。](https://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app)  

若要使用现有 appx 包文件，请添加一个生成步骤，并设置该生成步骤的 Binary File Path 参数。 

![配置 hockey app](images/building-screen15.png) 

若要设置此参数，请将应用名称、AppxVersion 变量和受支持的平台一起组合成为如下的单个字符串：

``` 
$(Build.ArtifactStagingDirectory)\AppxPackages\MyUWPApp_$(AppxVersion)_Test\MyUWPApp_$(AppxVersion)_x86_x64_ARM.appxbundle
```

>注意：尽管 HockeyApp 任务允许你指定符号文件的路径，但最佳做法是将符号（appxsym 文件）包含在程序包中。

我们将在本指南[后面部分](#sideloading-best-practices)中帮助你安装并运行旁加载软件包。 

## 设置将软件包提交到应用商店的连续部署生成 

若要生成应用商店提交软件包，请在 Visual Studio 中使用应用商店关联向导将应用与应用商店关联起来。

![关联到应用商店](images/building-screen16.png) 

>注意：此向导生成名为 Package.StoreAssociation.xml 的文件，该文件包含应用商店关联信息。 如果你将源代码存储在公用存储库（如 GitHub）中，则此文件将包含该帐户的所有应用保留名称。 可在公开前排除或删除此文件。

如果你无权访问用于发布该应用的开发人员中心帐户，则可以按照以下文档中的说明操作：[正在为第三方生成应用？如何打包它们的应用商店应用。](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/#e35YzR5aRG6uaBqK.97) 

然后你需要验证生成步骤是否包含以下参数：

```
/p:UapAppxPackageBuildMode=StoreUpload 
```

这将生成可提交到应用商店的 appxupload 文件。


#### 配置自动应用商店提交

为 Windows 应用商店使用 Visual Studio Team Services 扩展以便与应用商店 API 集成，并将 appxupload 包发送到应用商店。

你需要将开发人员帐户与 Azure Active Directory (AD) 连接起来，然后在 AD 中创建一个应用以对请求进行身份验证。 可按照扩展页中的指南完成该操作。 

配置该扩展后，可添加生成任务，并使用应用 ID 和 appxupload 文件的位置配置它。

![配置开发人员中心](images/building-screen17.png) 

其中 `Package File` 参数的值将是：

```
$(Build.ArtifactStagingDirectory)\
AppxPackages\MyUWPApp__$(AppxVersion)_x86_x64_ARM_bundle.appxupload
```

>注意。 你必须手动激活此生成。 可使用它更新现有应用，但无法将其用于首次提交到应用商店。 有关详细信息，请参阅[使用 Windows 应用商店服务创建和管理应用商店提交。](https://msdn.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services)

## 最佳做法

<span id="sideloading-best-practices"/>
### 旁加载应用的最佳做法

如果要分配应用而不将其发布到应用商店，可以将应用直接旁加载到设备，前提是这些设备信任用于对应用包签名的证书。 

使用 `Add-AppDevPackage.ps1` PowerShell 脚本安装应用。 此脚本将证书添加到本地计算机的受信任的根证书部分，然后将安装或更新 appx 文件。

#### 使用 Windows10 周年更新旁加载应用
在 Windows10 周年更新中，你可以双击 appxbundle 文件，然后通过在对话框中选择安装按钮来安装应用。 


![在 rs1 中旁加载](images/building-screen18.png) 

>注意：此方法不安装证书或关联的依赖项。

如果要从 VSTS 或 HockeyApp 之类的网站分配 appx 包，你需要在浏览器中将该站点添加到受信任的站点列表。 否则，Windows 将该文件标记为锁定。 

<span id="certificates-best-practices"/>
### 为证书签名的最佳做法 
Visual Studio 为每个项目都生成证书。 这使维护有效证书的特选列表变得困难。 如果你计划创建多个应用，可创建单个证书来对所有应用签名。 然后，信任你的证书的每台设备都将能够旁加载你的任意应用，而无需安装其他证书。 若要了解详细信息，请参阅[如何创建应用包签名证书。](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)


#### 创建签名证书
使用 [MakeCert.exe](https://msdn.microsoft.com/en-us/library/windows/desktop/ff548309(%09v=vs.85).aspx) 工具创建证书。 以下示例使用 MakeCert.exe 工具创建证书。

```
MakeCert /n publisherName /r /h 0 /eku "1.3.6.1.5.5.7.3.3,1.3.6.1.4.1.311.10.3.13" /e expirationDate /sv MyKey.pvk MyKey.cer
```

然后，你可以使用 Pvk2Pfx 工具生成包含受密码保护的私钥的 PFX 文件。

向每个计算机角色提供以下证书：

|**计算机**|**用途**|**证书**|**证书存储**|
|-----------|---------|---------------|---------------------|
|开发人员/生成计算机|对生成签名|MyCert.PFX|当前用户/个人|
|开发人员/生成计算机|运行|MyCert.cer|本地计算机/受信任人|
|用户|运行|MyCert.cer|本地计算机/受信任人|

>注意：你还可以使用已经受用户信任的企业证书。

#### 为 UWP 应用签名
Visual Studio 和 MSBuild 为管理用于为应用签名的证书提供不同的选项：

一个方法是将证书包含在解决方案中的私钥中（通常采用 PFX 文件的格式），然后在项目文件中引用该 PFX 文件。 可使用清单编辑器的“软件包”选项卡管理它。


![创建证书](images/building-screen19.png) 

另一个选项是将证书安装到生成计算机上（当前用户/个人），然后使用“证书存储”选项中的“选取”。 这指定项目文件中证书的指纹，因此证书应安装在将用于生成项目的所有计算机中。

#### 信任目标设备中的签名证书
目标设备必须信任证书，然后才能在该设备上安装应用。 

在本地计算机证书存储中的受信任人或信任根位置注册证书的公钥。

注册证书的最简单方法是在 .cer 文件中双击，然后按照向导中的步骤将证书保存在本地计算机和受信任人存储中。

## 相关主题
* [为 Windows 生成 .NET 应用](https://www.visualstudio.com/en-us/docs/build/get-started/dot-net) 
* [打包 UWP 应用](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps)
* [在 Windows10 中旁加载 LOB 应用](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10)
* [如何创建应用包签名证书](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx)



<!--HONumber=Nov16_HO1-->

