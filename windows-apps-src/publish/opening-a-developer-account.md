---
ms.assetid: 284EBA1F-BFB4-4CDA-9F05-4927CDACDAA7
title: 开设开发者帐户
description: 下面概述了如何在合作伙伴中心为 Microsoft Store 和其他 Microsoft 程序注册 Windows 开发人员帐户。
ms.date: 3/30/2020
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: e16446321ccb8dc2494f59feec88fbb0a52aed94
ms.sourcegitcommit: a4ca8ba143862411cd1104515cfeb98f1bcdb780
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96857408"
---
# <a name="opening-a-developer-account"></a>开设开发者帐户

本文介绍如何在 [合作伙伴中心](https://partner.microsoft.com/dashboard)注册 Windows 开发人员帐户。

> [!NOTE]
> 当你注册开发人员帐户时，我们将使用你在联系人信息中提供的电子邮件地址来发送与你的帐户相关的消息。 有时，这些信息可能包含有关我们的程序的信息。 如果你选择不[opt out](https://account.microsoft.com/account/Account?ru=https%3A%2F%2Faccount.microsoft.com%2Fprofile%2Fcontact-info&destrt=profile-landing)使用这些信息性电子邮件，请注意，我们仍会向你发送事务性消息 (例如，通知你的应用已通过认证，或者) 的付款方式。 这些事务性电子邮件是帐户的必需部分，除非你关闭帐户，否则你将继续接收这些电子邮件。

## <a name="the-account-signup-process"></a>帐户注册流程

> [!NOTE]
> 在某些情况下，注册开发人员帐户时看到的屏幕和字段可能与以下步骤中所述的内容略有不同。 但基本信息和过程将匹配这些步骤所描述的内容。

> [!NOTE]
> 有一个已知问题，某些区域设置中的用户可能无法完成其注册。 在我们确认它已解决之前，建议你在 partner.microsoft.com 上开始注册过程后，手动将浏览器的区域设置标记更改为 **en-us** 。

1.  请在 [注册页](https://developer.microsoft.com/store/register)**上**，选择 "注册"。
2.  如果您尚未使用 Microsoft 帐户登录，请在此登录或创建一个新的 Microsoft 帐户。 此处使用的 Microsoft 帐户用于登录开发人员帐户。
3.  选择您所在的 [国家/地区](account-types-locations-and-fees.md#developer-account-and-app-submission-markets) 或业务所在的位置。 此后您将无法更改此信息。
4.  选择[开发者帐户类型](account-types-locations-and-fees.md)（个人或公司）。 此后您将无法更改此信息，因此请确保选择的帐户类型正确无误。
5.  输入要使用 (50 个字符或更少) 的 **发布者显示名称** 。 客户在浏览应用时将看到此名称，并通过此名称了解应用，因此请谨慎选择该名称。 对于公司帐户，请务必使用贵组织已注册的业务名称或商业名称。 如果输入其他人已选择的名称，或其他人有权使用该名称，则不允许使用该名称。

    > [!NOTE]
    > 请确保您有权使用在此处输入的名称。 如果您选择的名称已被其他人用作商标或申请版权，则您的帐户可能被关闭。 有关详细信息，请参阅 [应用开发人员协议](/legal/windows/agreements/app-developer-agreement) 。 如果其他人正在使用您已获得商标权或其他法律权利的发布者显示名称，请[联系 Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html)。    

6.  输入要用于你的开发人员帐户的联系人信息。

    > [!NOTE]
    > 此信息将用于就帐户相关事宜与您取得联系。 例如，你将在完成注册后收到电子邮件确认。 完成此操作后，我们将在我们向你发送消息时发送消息，或者你需要通过你的帐户进行修复。 我们可能还会发送信息性电子邮件（如前文所述），除非你选择不接收非事务性电子邮件。

    如果要注册为公司，还需要输入将批准公司帐户的人员的姓名、电子邮件地址和电话号码。

7.  选择 " **下一步** " 转到 " **付款** " 部分。

8.  输入一次性注册费用的付款信息。 如果有抵扣注册费用的促销代码，可以在此处输入该代码。 否则，请在支持的市场) 提供信用卡信息 (或 PayPal 信息。 请注意，预付款的信用卡无法用于此项购买。 完成后，选择 " **下一步** " 转到 " **查看** " 屏幕。

9.  查看帐户信息，并确认所有内容都正确无误。 然后，阅读并接受[应用开发人员协议](/legal/windows/agreements/app-developer-agreement)的条款和条件。 选中相应的复选框，以指示已阅读并接受这些条款。

10.  选择“完成”以确认注册。 将处理你的付款，我们将向你的电子邮件地址发送一条确认消息。

注册后，你的帐户将经历验证。 对于个人帐户，我们将进行检查，确保其他用户尚未使用您的发布者显示名称。 对于公司帐户，过程所花时间会长一点，因为我们还需要确认用户得到授权，可设置公司帐户。 此验证可能需要几天到几周的时间，并且通常包含公司电话呼叫。 您可以在 " **帐户设置** " 页上检查您的验证状态。


## <a name="additional-guidelines-for-company-accounts"></a>关于公司帐户的其他准则

> [!IMPORTANT]
> 若要允许多个用户访问你的开发人员帐户，建议使用 Azure Active Directory (Azure AD) 将角色分配给单个用户，而不是共享对 Microsoft 帐户的访问权限。 然后，每个用户都可以通过使用其个人 Azure AD 凭据登录到合作伙伴中心来访问开发人员帐户。 有关详细信息，请参阅[管理帐户用户](manage-account-users.md)。

如果希望让多名用户使用打开 (该帐户的 Microsoft 帐户登录，而不是将单个用户添加到帐户) ，请参阅以下指南：

-   电子邮件所有权验证主要联系人（主电子邮件）的地址是否有效。 主要联系人电子邮件地址必须是受监视并可以发送/接收电子邮件的工作帐户。 合作伙伴 **不** 应使用： (1) 与公司域无关的个人电子邮件地址，或 (2) 租户用户登录未关联到电子邮件 (例如， jsmith@testcompany.onmicrosoft.com) 。
-   将此 Microsoft 帐户的访问权限限制为尽可能少的用户数量。
-   设置公司电子邮件通讯组列表，其中包括需要访问开发人员帐户的所有用户。 将此电子邮件地址添加到 [与 Microsoft 帐户关联的安全信息](https://account.microsoft.com/security)。 此方法允许列表中的所有员工接收发送到此别名的安全代码。 如果设置通讯组列表不可行，可以将个人电子邮件地址添加到安全信息。 但在出现 (提示时，该电子邮件地址的所有者将是唯一可以访问和共享安全代码的人，如将新的安全信息添加到该帐户时或从新设备访问该帐户时) 。
-   将公司电话号码添加到 Microsoft 帐户的安全信息。 尝试使用不需要扩展且可供关键团队成员访问的数字。
-   鼓励开发人员使用 [受信任的设备](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device) 登录公司的开发人员帐户。 所有主要团队成员应有权访问这些受信任的设备。 这种安排降低了团队成员访问帐户时要发送安全代码的需要。 每个帐户每周可以生成的代码数有限制。
-   如果需要允许从不受信任的电脑访问该帐户，则限制最多只有五个开发人员可以进行该访问。 理想情况下，这些开发人员应从共用同一地理位置和网络位置的计算机访问该帐户。
-   经常在 https://account.microsoft.com/security 中查看公司的安全信息以确保这些信息都是最新的。


## <a name="microsoft-account-security"></a>Microsoft 帐户安全性

我们通过将 Microsoft 帐户与多种形式的身份验证相结合，使用所提供的安全信息来提高 Microsoft 帐户的安全级别。 这样一来，未经授权访问 Microsoft 帐户（和开发者帐户）的难度将大大提高。 此外，如果你忘记了密码，或者其他人尝试访问你的帐户，我们将能够联系你确认所有权，并/或重新建立适当的帐户控制。

你的 Microsoft 帐户上必须至少有两个电子邮件地址或电话号码。 我们建议添加尽可能多的信息。 请记住，必须对某些安全信息进行确认，它才能生效。 此外，请确保经常查看安全信息并验证它是否是最新的。 你可以通过转到 https://account.microsoft.com/security 并使用 Microsoft 帐户登录来管理你的安全信息。 有关详细信息，请参阅 [帐户安全信息 & 验证代码](https://support.microsoft.com/help/12428/microsoft-account-security-info-verification-codes)。

当你通过 Microsoft 帐户登录到合作伙伴中心时，系统可能会提示你通过发送安全代码来验证你的身份，你必须输入该安全代码才能完成登录过程。 建议将经常使用的电脑指定为 *受信任的设备*。 当你从受信任的设备登录时，通常不会提示你输入代码，但在特定情况下可能偶尔会提示你，或者你在很长一段时间内未登录到该设备。 有关详细信息，请参阅 [将受信任的设备添加到 Microsoft 帐户](https://support.microsoft.com/help/12369/microsoft-account-add-a-trusted-device)。


## <a name="closing-your-account"></a>关闭帐户

开发人员帐户不会过期，因此无需续订你的帐户即可将其保持打开状态。 如果你决定完全关闭你的帐户，你可以通过联系技术支持人员来执行此操作。

当关闭你的帐户时，务必了解你在 Microsoft Store 中发布的任何应用会发生什么情况：

-   应用的当前客户仍可使用该应用。 但是，它们不能进行应用内购买。
-   即使应用仍可供以前获取该应用的客户使用，你的应用列表也会从 Microsoft Store 中删除。 任何新客户都无法获得您的应用程序。
-   将释放你的应用的名称，以供其他开发人员使用。
-   如果由于之前的应用销售额而产生余额，则即使到期金额不满足标准支付阈值，也可以请求支付此余额。
