---
Description: 若要添加和管理帐户用户，必须首先将合作伙伴中心帐户与组织的 Azure Active Directory 相关联。
title: 将 Azure Active Directory 与合作伙伴中心帐户关联
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, azure 租户, aad 租户, azure ad 租户, 租户管理, 租户
ms.localizationpriority: medium
ms.openlocfilehash: fa26f4ea3fa91f43dea117ff5ffd2fec87514544
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846757"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>将 Azure Active Directory 与合作伙伴中心帐户关联

若要 [添加和管理帐户用户](add-users-groups-and-azure-ad-applications.md)，必须首先将合作伙伴中心帐户与组织的 Azure Active Directory 相关联。 

[合作伙伴中心](https://partner.microsoft.com/dashboard) 利用多用户帐户访问和管理 Azure AD。 如果你的组织已使用 Microsoft 365 或 Microsoft 的其他业务服务，则你已具有 Azure AD。 否则，你可以在合作伙伴中心内创建新的 Azure AD 租户，无额外费用。

> [!TIP]
> 本主题特定于[合作伙伴中心](https://partner.microsoft.com/dashboard)的 Windows apps 开发人员计划，但关联租户和管理用户的工作方式类似于 Windows 桌面应用程序中的帐户。有关详细信息，请参阅 Windows 桌面应用程序 (查看[windows 桌面应用](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)程序中的详细信息) 和 Windows 硬件开发者计划中的详细信息，请参阅 Windows 硬件开发人员计划中的 "**管理员** **" 角色的**引用 (有关详细信息) ，请参阅[仪表板管理](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration)。

单个 Azure AD 租户可与多个合作伙伴中心帐户关联。 你只需拥有一个与你的合作伙伴中心帐户关联的 Azure AD 租户，以便添加多个帐户用户，但你也可以选择将多个 Azure AD 租户添加到单个合作伙伴中心帐户。 具有合作伙伴中心帐户中的管理员角色的任何用户都可以在该帐户中添加和删除 Azure AD 租户。

> [!IMPORTANT]
> 将合作伙伴中心帐户与 Azure AD 租户关联后，若要添加和管理该租户中的帐户用户，你需要以具有 **管理员** 角色的同一租户中的用户身份登录到合作伙伴中心。


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>将合作伙伴中心帐户与组织的现有 Azure AD 租户关联

如果你的组织已使用 Azure AD，请按照以下步骤来链接你的合作伙伴中心帐户。

1.  在 " [合作伙伴中心](https://partner.microsoft.com/dashboard)" 中，选择 "齿轮" 图标 (靠近仪表板右上角) ，然后选择 " **开发人员设置**"。 在 " **设置** " 菜单中，选择 " **租户**"。
2.  选择 " **将 Azure AD 关联到合作伙伴中心帐户**。
3.  输入要关联的租户的 Azure AD 凭据。
4.  查看 Azure AD 租户的组织名称和域名。 若要完成关联，请选择“确认”。
5.  如果关联成功，即可在合作伙伴中心的“用户”部分中添加和管理帐户用户。

> [!IMPORTANT]
> 若要创建新用户或者对 Azure AD 进行其他更改，你需要使用对 Azure AD 租户拥有[全局管理员权限](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)的帐户登录到该租户。 但是，你不需要全局管理员权限即可关联租户，或者将该租户中已存在的用户添加到合作伙伴中心帐户。


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>创建全新的 Azure AD 以与合作伙伴中心帐户关联

如果需要设置一个新的 Azure AD 以链接到合作伙伴中心帐户，请按照以下步骤操作。

1.  在 " [合作伙伴中心](https://partner.microsoft.com/dashboard)" 中，选择 "齿轮" 图标 (靠近仪表板右上角) ，然后选择 " **开发人员设置**"。 在 " **设置** " 菜单中，选择 " **租户**"。
2.  选择**创建新 Azure AD**。
3.  为新的 Azure AD 输入目录信息：
    - **域名**：我们将用于 Azure AD 域的唯一名称，以及 "onmicrosoft.com"。 例如，如果你输入“example”，则 Azure AD 域将为“example.onmicrosoft.com”。
    - 联系人电子邮件：电子邮件地址，我们可以在必要时就帐户相关事宜与你联系。
    - 全局管理员用户帐户信息：要用于新的全局管理员帐户的名字、姓氏、用户名和密码。
4.  单击 " **创建** " 以确认新的域和帐户信息。
5.  使用新的 Azure AD 全局管理员用户名和密码登录，以开始[添加和管理其他帐户用户](add-users-groups-and-azure-ad-applications.md)。


## <a name="manage-azure-ad-tenant-associations"></a>管理 Azure AD 租户关联

将 Azure AD 租户与合作伙伴中心帐户关联后，可以添加新租户或从 **租户** 页中删除现有租户。


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>向合作伙伴中心帐户添加多个 Azure AD 租户

拥有合作伙伴中心帐户的 **管理员** 角色的任何用户都可以将 Azure AD 租户与该帐户相关联。

若关联新租户，请选择**关联其他 Azure AD 租户**，然后按照上方提示的步骤操作。 请注意，系统会提示你输入要关联的 Azure AD 租户的凭据。


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>从合作伙伴中心帐户中删除 Azure AD 租户

拥有合作伙伴中心帐户的 **管理员** 角色的任何用户都可以从帐户中删除 Azure AD 租户。

> [!IMPORTANT]
> 删除租户后，从该租户添加到合作伙伴中心帐户的所有用户都将无法再登录该帐户。 

若要删除租户，请在 "**帐户设置**") 中的 "**租户**" (页上查找其名称，然后选择 "**删除**"。 系统将提示你确认删除租户。 完成此操作后，该租户中的任何用户都将无法登录到合作伙伴中心帐户，并且将删除为这些用户配置的任何权限。

> [!TIP]
> 如果你当前使用同一个租户中的帐户登录到合作伙伴中心，则无法删除该租户。 若要删除租户，必须以与帐户关联的其他租户的管理员身份登录到合作伙伴中心。 如果只有一个租户与帐户相关联，则只能在使用打开帐户所用的 Microsoft 帐户登录后删除该租户。


