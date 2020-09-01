---
title: 为帐户用户设置角色或自定义权限
description: 了解如何在将用户添加到合作伙伴中心帐户时设置标准角色或自定义权限。
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, 用户角色, 用户权限, 自定义角色, 用户访问权限, 自定义权限, 标准角色
ms.localizationpriority: medium
ms.openlocfilehash: 10c75d117320a947ce33ebd732c1956a9b3ae0e6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172851"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>为帐户用户设置角色或自定义权限

当你 [将用户添加到你的合作伙伴中心帐户](add-users-groups-and-azure-ad-applications.md)时，你将需要指定他们在帐户中的访问权限。 可以通过将适用于整个帐户的[标准角色](#roles)分配给用户以实现此目的，或者可以选择[自定义权限](#custom)，向用户提供相应的访问权限级别。 有些自定义权限适用于整个帐户，有些权限限制于一个或多个特定产品（如果你愿意，可授予所有产品）。

> [!NOTE] 
> 无论是添加用户、组还是添加 Azure AD 应用程序，均可应用同一角色和权限。

在确定应用什么角色或权限时，请注意： 
-   用户 (包括组和 Azure AD 应用程序) 将能够使用与分配的角色关联的权限 () 访问整个合作伙伴中心帐户，除非你 [自定义权限](#custom) 并分配 [产品级别权限](#product-level-permissions) ，以便他们只能使用特定应用和/或外接程序。
-   可以通过选择多个角色或使用自定义权限授予想要授予的访问权限，允许用户、组或 Azure AD 应用程序访问多个角色的功能。
-   具有某个角色（或自定义权限集）的用户还可加入具有其他角色（或权限集）的组。 在该情况下，用户可访问与组和个人帐户关联的所有功能。

> [!TIP]
> 本主题特定于 [合作伙伴中心](https://partner.microsoft.com/dashboard)的 Windows apps 开发人员计划。 有关硬件开发人员计划中用户角色的信息，请参阅[管理用户角色](/windows-hardware/drivers/dashboard/managing-user-roles)。 有关 Windows 桌面应用程序计划中的用户角色的信息，请参阅 [Windows 桌面应用程序计划](/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users)。


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>将角色分配给帐户用户

默认情况下，会向你提供一组标准角色，供你在将用户、组或 Azure AD 应用程序添加到你的合作伙伴中心帐户时进行选择。 每个角色都有一组特定的权限，以便在帐户内执行某些功能。 

除非选择定义[自定义权限](#custom)（方法为选中**自定义权限**），否则添加到帐户的每个用户、组或 Azure AD 应用程序都必须分配以下标准角色中的至少一个角色。 

> [!NOTE]
> 该帐户的**所有者**是第一个使用 Microsoft 帐户创建它的人员（而不是任何通过 Azure AD 添加的用户）。 此帐户所有者是唯一具有该帐户的完整访问权限的人员，其中包括删除应用、创建和编辑所有帐户用户以及更改所有财务和帐户设置的功能。 


| 角色                 | 描述              |
|----------------------|--------------------------|
| Manager              | 具有对该帐户的完整访问权限，除了更改税收和付款设置。 这包括在合作伙伴中心管理用户，但是请注意，在 Azure AD 租户中创建和删除用户的能力取决于帐户在 Azure AD 中的权限。 也就是说，如果为某个用户分配了管理员角色，但在组织的 Azure AD 中没有全局管理员权限，则他们将无法创建新用户或从目录中删除用户 (不过，他们可以更改用户的合作伙伴中心角色) 。 <p> 请注意，如果合作伙伴中心帐户与多个 Azure AD 租户相关联，则管理器无法查看用户 (的完整详细信息，包括名字、姓氏、密码恢复电子邮件以及这些用户是否是 Azure AD 全局管理员) ，除非他们使用具有该租户的全局管理员权限的帐户登录到同一租户。 但是，用户可以在与合作伙伴中心帐户关联的任何租户中添加和删除用户。 |
| 开发人员            | 可以上传程序包并提交应用和加载项，并且可以查看[使用情况报告](usage-report.md)获取遥测详细信息。 可以访问 [跨设备体验](https://developer.microsoft.com/windows/project-rome) 功能。 无法查看财务信息或帐户设置。   |
| 业务参与者 | 可查看[运行状况](health-report.md)和[使用情况](usage-report.md)报告。 无法创建或提交产品、更改帐户设置或查看财务信息。   |
| 财务参与者  | 可查看[付款报告](payout-summary.md)、财务信息和购置报告。 无法对应用、加载项或帐户设置进行任何更改。    |
| 营销人员             | 可以[回复客户评论](respond-to-customer-reviews.md)和查看非财务[分析报告](analytics.md)。 无法对应用、加载项或帐户设置进行任何更改。      |

下表显示了每个角色（和帐户所有者）可用的一些特定功能。

|                                                       |    帐户所有者                 |    Manager                       |    开发人员                     |    业务参与者    |    财务参与者    |    营销人员                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    采集报表 (包括近乎实时的数据)  |    可以查看                      |    可以查看                      |    不允许访问                     |    不允许访问               |    可以查看               |    不允许访问                     |
|    反馈报告/响应                          |    可以查看和发送反馈    |    可以查看和发送反馈    |    可以查看和发送反馈    |    不允许访问               |    不允许访问              |    可以查看和发送反馈    |
|    运行状况报告 (包括近乎实时的数据)       |    可以查看                      |    可以查看                      |    可以查看                      |    可以查看                |    不允许访问              |    不允许访问                     |
|    使用情况报告                                       |    可以查看                      |    可以查看                      |    可以查看                      |    可以查看                |    不允许访问              |    不允许访问                     |
|    付款帐户                                     |    可以更新                    |    不允许访问                     |    不允许访问                     |    不允许访问               |    可以更新             |    不允许访问                     |
|    税务资料                                        |    可以更新                    |    不允许访问                     |    不允许访问                     |    不允许访问               |    可以更新             |    不允许访问                     |
|    付款摘要                                     |    可以查看                      |    不允许访问                     |    不允许访问                     |    不允许访问               |    可以查看               |    不允许访问                     |

如果没有合适的标准角色，或者希望限制访问特定应用和/或加载项，可选中**自定义权限**，将自定义权限授予用户，如下所述。


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>将自定义权限分配至帐户用户

若要分配自定义权限而非标准角色，可在添加或编辑用户帐户时在**角色**部分中单击**自定义权限**。 

若要为用户启用某项权限，请将框切换为相应设置。 

![访问权限设置指南](images/permission_key.png)

- **无法访问**：用户没有所指示的权限。
- **只读**：用户有权访问与指示区域相关的功能，但无法进行更改。 
- **读/写**：用户有权更改和查看与区域关联的功能。
- **Mixed**：不能直接选择此选项，但 **混合** 指示器将显示是否允许对该权限的访问进行组合。 例如，如果你向**所有产品**的**定价和可用性**授予**只读**访问权限，但随后对某个特定产品的定价和**可用性**授予**读/写**访问权限，则**所有产品**的**定价和可用性**指标都将显示为 "Mixed"。 如果某些产品没有权限 **访问** 权限，但其他产品具有 **读/写** 和/或 **只读** 访问权限，则这一点同样适用。

对于某些权限（如与查看分析数据相关的权限），只能授予 **只读** 访问权限。 请注意，在当前实现中，某些权限不区分 **只读** 访问和 **读/写** 访问。 请查看每项权限的详细信息，了解**只读**和/或**读/写**访问权限授予的特定功能。

下表介绍了有关每项权限的特定详细信息。

## <a name="account-level-permissions"></a>帐户级权限

此部分中权限不限于特定产品。 向这些权限之一授予访问权限可支持用户访问整个帐户。

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">权限名称</th>
    <th align="left">只读</th>
    <th align="left">读取/写入</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>帐户设置</b>                    </td><td align="left">  可以查看 " <b>帐户设置</b> " 部分中的所有页面，包括 <a href="managing-your-profile.md">联系信息</a>。       </td><td align="left">  可以在 " <b>帐户设置</b> " 部分查看所有页。 可更改<a href="/windows/uwp/publish/manage-account-settings-and-profile">联系人信息</a>和其他页面，但无法更改付款帐户或税务配置文件（除非单独授予该权限）。            </td></tr>
<tr><td align="left">    <b>帐户用户</b>                       </td><td align="left">  可在<b>用户</b>部分查看已添加到帐户的用户。          </td><td align="left">  可将用户添加到帐户，并在<b>用户</b>部分更改现有的用户。             </td></tr>
<tr><td align="left">    <b>帐户级别 ad 性能报表</b> </td><td align="left">  可查看帐户级的<a href="advertising-performance-report.md">广告性能报告</a>。      </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    <b>广告活动</b>                        </td><td align="left">  可查看在帐户中创建的<a href="create-an-ad-campaign-for-your-app.md">广告市场活动</a>。      </td><td align="left">  可在帐户中创建、管理和查看<a href="create-an-ad-campaign-for-your-app.md">广告市场活动</a>。          </td></tr>
<tr><td align="left">    <b>Ad 中介</b>                        </td><td align="left">  可查看帐户中所有产品的广告中介配置。    </td><td align="left">  可查看和更改帐户中所有产品的广告中介配置。        </td></tr>
<tr><td align="left">    <b>Ad 采集报表</b>                </td><td align="left">  可查看帐户中所有产品的<a href="/windows/uwp/publish/advertising-performance-report">广告中介报告</a>。    </td><td align="left">  不适用    </td></tr>
<tr><td align="left">    <b>Ad 性能报告</b>              </td><td align="left">  可查看帐户中所有产品的<a href="advertising-performance-report.md">广告性能报告</a>。       </td><td align="left">  不适用         </td></tr>
<tr><td align="left">    <b>Ad 单位</b>                            </td><td align="left">  可查看为帐户创建的<a href="in-app-ads.md">广告单元</a>。    </td><td align="left">  可创建、管理和查看帐户<a href="in-app-ads.md">广告单元</a>。             </td></tr>
<tr><td align="left">    <b>关联广告</b>                       </td><td align="left">  可查看帐户中所有产品的<a href="/windows/uwp/publish/in-app-ads">联盟广告</a>使用情况。    </td><td align="left">  可管理和查看帐户中所有产品的<a href="/windows/uwp/publish/in-app-ads">联盟广告</a>使用情况。                </td></tr>
<tr><td align="left">    <b>子公司性能报表</b>      </td><td align="left">  可查看帐户中所有产品的<a href="/windows/uwp/publish/advertising-performance-report">联盟性能报告</a>。   </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    <b>应用安装广告报表</b>             </td><td align="left">  可查看<a href="promote-your-app-report.md">广告活动报告</a>。           </td><td align="left">  不适用   </td></tr>
<tr><td align="left">    <b>社区广告</b>                       </td><td align="left">  可查看帐户中所有产品的免费<a href="about-community-ads.md">社区广告</a>使用情况。          </td><td align="left">  可创建、管理和查看帐户中所有产品的免费<a href="about-community-ads.md">社区广告</a>使用情况。               </td></tr>
<tr><td align="left">    <b>联系人信息</b>                        </td><td align="left">  可查看“帐户设置”部分中的<a href="/windows/uwp/publish/manage-account-settings-and-profile">联系人信息</a>。        </td><td align="left">  可编辑和查看“帐户设置”部分中的<a href="/windows/uwp/publish/manage-account-settings-and-profile">联系人信息</a>。            </td></tr>
<tr><td align="left">    <b>COPPA 合规性</b>                    </td><td align="left">  可查看帐户中所有产品的 <a href="in-app-ads.md#coppa-compliance">COPPA 合规性</a>选择（指示产品是否面向年龄在 13 岁以下的儿童）。                                            </td><td align="left">  可编辑和查看帐户中所有产品的 <a href="in-app-ads.md#coppa-compliance">COPPA 合规性</a> 选择（指示产品是否面向年龄在 13 岁以下的儿童）。         </td></tr>
<tr><td align="left">    <b>客户组</b>                     </td><td align="left">  可以查看 <a href="create-customer-groups.md">客户组</a> (段和已知用户组) 。      </td><td align="left">  可以创建、编辑和查看)  (段和已知用户组的 <a href="create-customer-groups.md">客户组</a> 。       </td></tr>
<tr><td align="left">    <b>管理产品组</b>&nbsp;*                            </td><td align="left">  可查看新产品组创建页面，但实际上无法创建新产品组。    </td><td align="left">  可以创建并编辑产品组。     </td></tr>
<tr><td align="left">    <b>新应用</b>                            </td><td align="left">  可查看新应用创建页面，但实际上无法在帐户中创建新应用。    </td><td align="left">  在帐户中预留新应用名称可<a href="create-your-app-by-reserving-a-name.md">创建新应用</a>，并可创建新提交，然后将应用提交到应用商店。     </td></tr>
<tr><td align="left">    <b>新捆绑</b>&nbsp;*                       </td><td align="left">  可查看新捆绑包创建页面，但实际上无法在帐户中创建新捆绑包。     </td><td align="left">  可创建产品的新捆绑包。          </td></tr>
<tr><td align="left">    <b>合作伙伴服务</b>&nbsp;*                  </td><td align="left">  可查看检索 XTokens 服务的安装证书。     </td><td align="left">  可管理和查看检索 XTokens 服务的安装证书。       </td></tr>
<tr><td align="left">    <b>帐户支出</b>                      </td><td align="left">  可以在<b>帐户设置</b>中查看帐户的<a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">帐户信息</a>。     </td><td align="left">  可以在<b>帐户设置</b>中编辑和查看帐户的<a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">帐户信息</a>。       </td></tr>
<tr><td align="left">    <b>支出摘要</b>                      </td><td align="left">  可查看<a href="payout-summary.md">付款摘要</a>，访问并下载付款报告信息。       </td><td align="left">  可查看<a href="payout-summary.md">付款摘要</a>，访问并下载付款报告信息。   </td></tr>
<tr><td align="left">    <b>信赖方</b>&nbsp;*                   </td><td align="left">  可查看依赖方，检索 XTokens。    </td><td align="left">  可管理和查看依赖方，检索 XTokens。     </td></tr>
<tr><td align="left">    <b>沙箱</b>&nbsp;*                         </td><td align="left">  可以访问 <b>沙箱</b> 页面并在帐户中查看沙盒，并查看这些沙箱的所有适用配置。 无法查看每个沙盒的产品和提交，除非授予相应的产品级别权限。 </td><td align="left">  可以访问 <b>沙盒</b> 页面并查看和管理帐户中的沙盒，包括创建和删除沙箱并管理其配置。 无法查看每个沙盒的产品和提交，除非授予相应的产品级别权限。    </td></tr>
<tr><td align="left">    <b>商店销售活动</b>&nbsp;*                            </td><td align="left">  不适用    </td><td align="left">  可以将选项配置为在 Microsoft Store 促销活动中自动加入产品。     </td></tr>
<tr><td align="left">    <b>税务配置文件</b>                         </td><td align="left">  可以在<b>帐户设置</b>中查看<a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">税务配置文件信息和窗体</a>。     </td><td align="left">  可以填写纳税形式并在<b>帐户设置</b>中更新<a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">税务概要信息</a>。     </td></tr>
<tr><td align="left">    <b>测试帐户</b>&nbsp;*                     </td><td align="left">  可查看测试 Xbox Live 配置的帐户。      </td><td align="left">  可创建、管理和查看测试 Xbox Live 配置的帐户。      </td></tr>
<tr><td align="left">    <b>Xbox 设备</b>                        </td><td align="left">  可以在 " <b>帐户设置</b> " 部分中查看为该帐户启用的 Xbox 开发控制台。       </td><td align="left">  可以在 " <b>帐户设置</b> " 部分中添加、删除和查看为该帐户启用的 Xbox 开发控制台。     </td></tr>
    </tbody>
    </table>

\* 用星号标记的权限 ( * ) 授予对所有帐户不可用的功能的访问权限。 如果帐户尚未启用这些功能，则选择这些权限不会有任何效果。   


## <a name="product-level-permissions"></a>产品级别的权限

本部分中的权限可授予帐户中所有产品，或者进行自定义，支持仅向一个或多个特定产品授予权限。 

产品级别权限分为四个类别：**分析**、**盈利**、**发布**和 **Xbox Live**。 可展开这些类别，查看每个类别中的各个权限。 还可以选择启用一个或多个特定产品的**所有权限**。

若要向帐户中每个产品授予权限，请在标为**所有产品**的行中选择该权限（切换框，指示**只读**或**读/写**）。 
 
> [!TIP]
> 为**所有产品**选择的内容将应用到当前在帐户中的所有产品和在帐户中创建的任何未来产品。 若要防止将权限应用到未来产品，则分别选择所有产品，而不是选择**所有产品**。

在 " **所有产品** " 行下方，你将看到帐户中的每个产品都列在单独的行中。 若仅为特定产品授予权限，请在该产品行中选择该权限。

每个外接程序都列在其父产品下方的单独行中，以及 **所有外接程序** 行。 对 **所有外接程序** 所做的选择将应用于该产品的所有当前加载项以及为该产品创建的任何未来加载项。

请注意，无法为加载项设置某些权限。 这是因为它们不应用于外接程序 (例如，) **或者在父** 产品级别授予的权限适用于该产品的所有外接程序 (例如， **促销代码**) 。 但请注意，适用于加载项的任何权限必须单独设置；加载项不会继承为父产品选择的内容。 例如，如果你希望允许用户为外接程序提供定价和可用性选择，则需要为外接程序 (或**所有外**接程序) 启用**定价和可用性**权限，无论是否已向父产品授予**定价和可用性**权限。 


### <a name="analytics"></a>Analytics

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b> (包含</b> 近乎实时的数据)  </td><td>    可查看产品的<a href="acquisitions-report.md">购置</a>和<a href="add-on-acquisitions-report.md">加载项购置</a>报告。        </td><td>    不适用    </td><td>    N/A (父产品的设置包括 **外接程序收购** 报告)         </td><td>    不适用                         </td></tr>
    <tr><td align="left">    <b>使用情况</b> </td><td>    可查看产品的<a href="usage-report.md">使用情况报告</a>。     </td><td>    不适用       </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    <b>运行状况</b> (包括近乎实时的数据)  </td><td>    可查看产品的<a href="health-report.md">运行状况报告</a>。    </td><td>    不适用     </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    <b>客户反馈</b>    </td><td>    可查看产品的<a href="reviews-report.md">评论</a>和<a href="feedback-report.md">反馈</a>报告。       </td><td>    N/A (要对反馈或评论做出响应，则必须授予 <b>Contact customer</b> 权限)    </td><td>    不适用     </td><td>    不适用         </td></tr>
    <tr><td align="left">    <b>Xbox analytics</b> </td><td>    可以查看产品的 <a href="xbox-analytics-report.md">Xbox analytics 报告</a> 。    </td><td>    不适用   </td><td>    不适用       </td><td>    不适用          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>盈利

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>促销代码</b>     </td><td>    可查看产品及其加载项的<a href="generate-promotional-codes.md">促销代码</a>订单和使用信息，并可查看使用情况信息。         </td><td>    可查看、管理和创建产品及其加载项的<a href="generate-promotional-codes.md">促销代码</a>，还可查看使用信息。          </td><td>    不适用（对于父产品的设置适用于所有加载项）     </td><td>    不适用（对于父产品的设置适用于所有加载项）     </td></tr>
    <tr><td align="left">    <b>目标产品</b>     </td><td>    可查看产品的<a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">定向优惠</a>。         </td><td>    可查看、管理和创建产品的<a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">定向优惠</a>。          </td><td>    不适用     </td><td>    不适用      </td></tr>
    <tr><td align="left">    <b>联系客户</b>  </td><td>    只要已获得<b>客户反馈</b>权限，就可以查看<a href="respond-to-customer-feedback.md">客户反馈</a>和<a href="respond-to-customer-reviews.md">对客户评论的答复</a>。 还可查看为产品创建的<a href="send-push-notifications-to-your-apps-customers.md">目标通知</a>。    </td><td>    只要<b>客户反馈</b>权限已被授予，就可以对<a href="respond-to-customer-feedback.md">客户反馈进行响应</a>并对<a href="respond-to-customer-reviews.md">客户评论做出回应</a>。 还可为产品<a href="send-push-notifications-to-your-apps-customers.md">创建和发送目标通知</a>。                   </td><td>    不适用         </td><td>    不适用                          </td></tr>
    <tr><td align="left">    <b>试验</b></td><td>    可查看产品的<a href="../monetize/run-app-experiments-with-a-b-testing.md">实验（A/B 测试）</a>和实验数据。   </td><td>    可为产品创建、管理和查看<a href="../monetize/run-app-experiments-with-a-b-testing.md">实验（A/B 测试）</a>，还可查看实验数据。     </td><td>    不适用  </td><td>    不适用                 </td></tr>
    <tr><td align="left">    <b>商店销售活动</b>&nbsp;*</td><td>    可以查看产品的促销活动状态。   </td><td>    可以将产品添加到促销活动并配置折扣。      </td><td>    可以查看产品的促销活动状态。   </td><td>    可以将产品添加到促销活动并配置折扣。      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>发布 

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>产品设置</b>  </td><td>    可以查看产品的产品设置页。     </td><td>    可以查看和编辑产品的产品设置页。 </td><td>    可以查看外接程序的 "产品设置" 页。   </td><td>    可以查看和编辑产品设置页面外接程序。          </td></tr>
    <tr><td align="left">    <b>定价和可用性</b>  </td><td>    可以查看产品的 <a href="set-app-pricing-and-availability.md">定价和可用性</a> 页面。     </td><td>    可以查看和编辑产品的 <a href="set-app-pricing-and-availability.md">定价和可用性</a> 页面。 </td><td>    可以查看外接程序的 " <a href="set-add-on-pricing-and-availability.md">定价和可用性</a> " 页。   </td><td>    可以查看和编辑外接程序的 " <a href="set-add-on-pricing-and-availability.md">定价和可用性</a> " 页。          </td></tr>
    <tr><td align="left">    <b>属性</b>   </td><td>    可以查看产品的 " <a href="enter-app-properties.md">属性</a> " 页。      </td><td>    可以查看和编辑产品的 " <a href="enter-app-properties.md">属性</a> " 页。       </td><td>    可以查看外接程序的 " <a href="enter-add-on-properties.md">属性</a> " 页。     </td><td>    可以查看和编辑加载项的 " <a href="enter-add-on-properties.md">属性</a> " 页。               </td></tr>
    <tr><td align="left">    <b>年龄分级</b>    </td><td>    可以查看产品的 " <a href="age-ratings.md">年龄分级</a> " 页。       </td><td>    可以查看和编辑产品的 " <a href="age-ratings.md">年龄分级</a> " 页。    </td><td>    可以查看外接程序的 "年龄分级" 页。          </td><td>     可以查看和编辑加载项的 "年龄分级" 页。       </td></tr>
    <tr><td align="left">    <b>包</b>        </td><td>    可以查看产品的 " <a href="upload-app-packages.md">包</a> " 页面。  </td><td>    可以查看和编辑产品的 " <a href="upload-app-packages.md">包</a> " 页，包括上载包。     </td><td>   如果适用) ，可以查看加载项 (的 " <a href="upload-app-packages.md">包</a> " 页。   </td><td>     如果适用) ，可以查看和编辑加载项 (的 <a href="upload-app-packages.md">包</a> 页。             </td></tr>
    <tr><td align="left">    <b>商店列表</b>  </td><td>    可以查看 <a href="create-app-store-listings.md">应用商店列表页 () </a> 产品。  </td><td>    可以查看和编辑 <a href="create-app-store-listings.md">商店列表页 (</a> 的产品) ，还可以为不同的语言添加新的商店节目表。     </td><td>    可以查看 <a href="create-add-on-store-listings.md">应用商店列表页 () </a> 外接程序。            </td><td>    可以查看和编辑 <a href="create-add-on-store-listings.md">应用商店列表页 () </a> 加载项，还可以添加不同语言的商店节目表。                 </td></tr>
    <tr><td align="left">    <b>应用商店提交</b>     </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。           </td><td>    可将产品提交到应用商店，并查看认证报告。 包含全新和更新的提交。 </td><td>如果“无法访问”设置为“只读”，则将授予此权限。     </td><td>    可将加载项提交到应用商店，并查看认证报告。 包含全新和更新的提交。</td></tr>
    <tr><td align="left">    <b>创建新的提交</b>       </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。        </td><td>    可为产品创建新<a href="app-submissions.md">提交</a>。  </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。   </td><td>    可为加载项创建新<a href="add-on-submissions.md">提交</a>。        </td></tr>
    <tr><td align="left">    <b>新加载项</b>    </td><td>    如果“无法访问”设置为“只读”，则将授予此权限。 </td><td>    可为产品<a href="set-your-add-on-product-id.md">创建新加载项</a>。 </td><td>    不适用    </td><td>    不适用        </td></tr>
    <tr><td align="left">    <b>名称保留</b>   </td><td>    可查看产品的<a href="manage-app-names.md">管理应用名称</a>页面。</td><td>    可查看和编辑产品的<a href="manage-app-names.md">管理应用名称</a>页面，包括预留其他名称和删除预留的名称。 </td><td>   可查看加载项的预留名称。    </td><td>   可查看和编辑加载项的预留名称。          </td></tr>
    <tr><td align="left">    <b>光盘请求</b>   </td><td>    可以查看 "请求" 页。 </td><td>    可以创建光盘请求。 </td><td>   不适用    </td><td>   不适用          </td></tr>
    <tr><td align="left">    <b>光盘版税 </b>   </td><td>    可以查看光盘的版税页面。</td><td>    可以创造光盘版税。 </td><td>   不适用    </td><td>   不适用          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">权限&nbsp;名称</th>
    <th align="left">只&nbsp;读</th>
    <th align="left">读取/写入</th>
    <th align="left">只&nbsp;读&nbsp;（加载&#8209;项） </th>
    <th align="left">读取&#8209;写入&nbsp;（加载&#8209;项）</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>信赖方</b>&nbsp;*</td><td>    可以查看帐户的 "信赖方" 页。   </td><td>    可以查看和编辑帐户的 "信赖方" 页。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>合作伙伴服务</b>&nbsp;*</td><td>    可以查看帐户的 "Web 服务" 页。  </td><td>    可以查看和编辑帐户的 "Web 服务" 页。      </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>Xbox 测试帐户</b>&nbsp;*</td><td>    可以查看帐户的 "Xbox Test 帐户" 页。  </td><td>    可以查看和编辑帐户的 "Xbox Test 帐户" 页。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>每个沙箱的 Xbox Test 帐户</b>&nbsp;*</td><td>    只能查看帐户的指定沙箱的 "Xbox 测试帐户" 页。  </td><td>    可以查看和编辑 Xbox 测试。   <tr><td align="left">    <b>帐户的 "帐户" 页仅限帐户的指定沙箱    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>Xbox 设备</b>&nbsp;*</td><td>    可以查看帐户的 Xbox one 开发控制台页。  </td><td>    可以查看和编辑帐户的 "Xbox one 开发控制台" 页。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>每个沙箱的 Xbox 设备</b>&nbsp;*</td><td>    可以仅查看帐户的指定沙箱的 "Xbox one 开发控制台" 页。  </td><td>    只能为帐户的指定沙箱查看和编辑 Xbox one 开发控制台页。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>应用通道</b>&nbsp;*</td><td>    不适用  </td><td>    可通过 OneGuide 将促销视频频道发布到 Xbox 主机以供观看。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>服务配置</b>&nbsp;*</td><td>    可以查看产品的 "Xbox Live 服务配置" 页。  </td><td>    可以查看和编辑产品的 "Xbox Live 服务配置" 页。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
    <tr><td align="left">    <b>工具访问</b>&nbsp;*</td><td>    可以对产品运行 Xbox Live 工具来仅查看数据。  </td><td>    可以对产品运行 Xbox Live 工具来查看和编辑数据。    </td><td>    不适用    </td><td>    不适用                      </td></tr>
</tbody>
</table>

\* 用星号标记的权限 ( * ) 授予对所有帐户不可用的功能的访问权限。 如果帐户尚未启用这些功能，则选择这些权限不会有任何效果。