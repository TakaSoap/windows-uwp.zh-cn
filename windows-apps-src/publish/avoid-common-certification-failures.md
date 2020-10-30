---
description: 查看此列表以帮助避免频繁地阻止应用通过认证的问题，或者在发布应用后，可能在点检查过程中标识的问题。
title: 避免常见的认证失败
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 672da214582fb6b206d7e16e1e776be40caeec90
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031220"
---
# <a name="avoid-common-certification-failures"></a>避免常见的认证失败


查看此列表以帮助避免频繁地阻止应用通过认证的问题，或者在发布应用后，可能在点检查过程中标识的问题。

> [!NOTE]
> 务必查看 [Microsoft Store 的策略](store-policies.md) ，以确保你的应用满足其中列出的所有要求。

-   仅在应用完成时提交你的应用。 欢迎使用你的应用提要来介绍即将推出的功能，但是请确保你的应用不包含未完成的部分、指向正在构建的网页的链接，或者其他任何会让客户觉得你的应用不完整的内容。

-   在提交应用前[使用 Windows 应用认证工具包测试应用](../debug-test-perf/windows-app-certification-kit.md)。

-   在多个不同配置上测试你的应用，以尽可能确保应用稳定。

-   确保你的应用在没有网络连接时不会崩溃。 即使您的应用需要连接网络才能实际使用，也应该能够在没有网络连接的情况下适当运行。

-   提供使用你的应用时所需的[任何必要信息](notes-for-certification.md)，例如，如果应用要求用户登录某项服务，则需要提供测试帐户的用户名和密码；还可能需要说明访问隐藏的功能或锁定的功能所需的任何步骤。

-   如果应用需要，请包含 [隐私策略 URL](enter-app-properties.md#privacy-policy-url) ;例如，如果你的应用程序以任何方式访问任何类型的个人信息或法律要求。 若要帮助确定应用是否需要隐私策略，请查看 [应用开发人员协议](/legal/windows/agreements/app-developer-agreement) 和 [Microsoft Store 政策](store-policies.md)。

-   请确保你的应用的说明清晰地展示应用的用途。 有关帮助，请参阅[编写出色的应用说明](write-a-great-app-description.md)中提供的相关指南。

-   针对[年龄分级](age-ratings.md)部分中的所有问题提供完整并且准确无误的答案。

-   除非你已经进行专门的工程处理和测试，确定了应用适用于辅助功能方案，否则不要[将应用声明为辅助应用](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines)。

-   如果你的应用使用来自 [**Windows.ApplicationModel.Store**](/uwp/api/Windows.ApplicationModel.Store) 命名空间的商用 API，请确保对应用进行测试并验证它是否可处理常见的异常情况。 此外，请确保你的应用使用 [**CurrentApp**](/uwp/api/Windows.ApplicationModel.Store.CurrentApp) 类（而非 [**CurrentAppSimulator**](/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator) 类，该类仅用于测试）。 （请注意，如果你的应用面向 Windows 10 版本 1607 或更高版本，我们建议使用 [Windows.Services.Store](/uwp/api/windows.services.store) 命名空间的成员，而非使用 Windows.ApplicationModel.Store 命名空间。）


 

 
