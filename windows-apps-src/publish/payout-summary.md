---
Description: 支出报表显示你在应用和外接程序中获得的资金的详细信息。 它还使你可以知道你将何时收到付款和收到多少付款。
title: 付款报表
ms.assetid: F0D070BE-8267-4CC9-B0D2-085EBA74AC98
ms.date: 08/02/2019
ms.topic: article
keywords: windows 10, uwp, 付款摘要, 声明, 付款, 收益, 支出, 付款, 收入
ms.localizationpriority: medium
ms.openlocfilehash: 7eab86cc1856f5ad206aa8bbceb2f2e04f5410d2
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/06/2020
ms.locfileid: "91763103"
---
# <a name="payout-reports"></a>付款报表

费用 **摘要** 显示了你在 Microsoft 获得的资金的详细信息。 它还使你可以知道你将何时收到付款和收到多少付款。

如果在 Azure Marketplace 中销售产品，还会在 " **支出摘要**" 中看到有关成功付款的信息。 有关 Azure Marketplace 付款的更多详细信息，请参阅 [Microsoft Azure 应用商店参与策略](/legal/marketplace/participation-policy)和 [Microsoft Azure 应用商店发布者协议](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE3ypvt)。

> [!NOTE]
> 你的收款必须达到 50 美元的[付款阈值](payment-thresholds-methods-and-timeframes.md)，才符合成为付款的条件。 若要详细了解付款阈值，请参阅此页，并查阅应用开发人员协议。

> [!NOTE]
> 若要寻求付款方面的支持（包括配置付款帐户、付款丢失、暂停付款或其他任何方面），请单击[此处](https://developer.microsoft.com/windows/support)联系支持人员。

## <a name="access-the-payout-summary-pages"></a>访问“付款摘要”页

若要打开“付款摘要”页之一，请执行以下操作：

1. 选择右上角的“付款”图标。
2. 选择“交易历史记录”、“付款”或“导出数据”。

## <a name="transaction-history-page"></a>“交易历史记录”页

此页显示你的所有个人收入，包括每次交易的日期、类型和收入。 可以选择要查看的时间段，也可以按“合约 ID”、“计划”、“付款 ID”、“收入类型”、“控制杆”和“状态”进行筛选。 本会计年度（7 月 1 日至 6 月 30 日）和前两个会计年度的数据都可查看。

若要查看关于收入的更多详情，请选择页面右侧的向下箭头。 这会显示控制杆、收入金额和产品。 如果出于某种原因，此数据无法使用，但你需要访问它，请与 [支持](https://developer.microsoft.com/windows/support)人员联系。 如果收益是调整的结果，而不是交易的，则不会显示 "产品" 字段。

若要导出此页上的任何事务数据，请选择 "导出"，然后按照 "导出数据" 页上的说明操作。 从 "事务历史记录" 页导出的文件以交易货币显示数据，按交易币种和美元计算收入，并按货币支付付薪值。

## <a name="payments-page"></a>“付款”页

此页上的总计代表你参与的所有计划。 可以按“参与者 ID”、“计划”、“付款 ID”和“收入类型”进行筛选。 金额以美元为单位。 付款值还以付款币种显示。

| 区域                   | 说明                                                                                  |
|------------------------|----------------------------------------------------------------------------------------------|
| 今年付款总额   | 你的所有计划在今年（美元）支付的总金额。       |
| 下次估计付款 | 即使有其他人即将) ，我们也会向你收取一次下一笔支付 (。 |
| 上次付款           | 金额 (美元) 、计划名称和最新付款计划。           |
| 付款(按来源)     | 过去12个月内按计划表示的付款金额，单位为美元。           |
| 支付               | 选择 "付费" 或 "挂起"，然后根据需要进行排序。 如需了解特定付款的更多详情，请选择“查看”。 若要下载付款汇款对帐单的副本，请选择“下载”。 请注意，事务历史记录数据可能需要长达24小时才能显示，因此你可能看不到相关收入。 |

若要导出此页上的任何数据，请选择 "导出"，然后按照 "导出数据" 页上的说明进行操作。

## <a name="payment-status"></a>付款状态

| 收入状态           | 原因                                                                                                                                      | 合作伙伴是否需要采取行动？                                   |
|--------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------|
| 未处理              | 收入符合成为付款的条件。 按照激励计划的计划指南中的规定，它会在这种状态下度过“冷却”期。 | 否                                                         |
| 即将处理                 | 支付订单在处理付款之前生成待定内部评审。                                                               | 否                                                         |
| 等待纳税发票      | 您的纳税发票不完整或无效。                                                                                                  | 必须更新纳税发票，才能收到付款 |
| 审查期间被拒绝   | 付款在评审期间被拒绝。                                                                                                     | 有关详细信息，请联系 [Microsoft 支持人员](https://developer.microsoft.com/windows/support)                      |
| 失败                   | 由于 Microsoft 系统错误，付款失败。                                                                                         | 有关详细信息，请联系 [Microsoft 支持人员](https://developer.microsoft.com/windows/support)                      |
| 正在进行              | 付款正在进行。                                                                                                                 | 否                                                         |
| 付款不正确        | 付款 recouping 正在进行。                                                                                                       | 否                                                         |
| 已发送                     | 已将付款发送到银行。                                                                                                     | 否                                                         |
| 正在重新处理             | 付款遇到 Microsoft 系统错误，正在重新处理。                                                                  | 否                                                         |
| Reversed                 | 付款已被银行冲销，并将在下一个付款周期再次发送。                                                     | 否                                                         |
| 纳税发票被拒绝     | 纳税发票在审查期间被拒绝了。 在纳税发票审查完成之前，所有挂起的付款都将处于暂停状态。                 | 有关详细信息，请联系 [Microsoft 支持人员](https://developer.microsoft.com/windows/support)                      |
| 正在审查纳税发票 | 正在审查纳税发票。 一旦纳税发票获准，就会发放付款。                                   | 否                                                         |
| 已拒绝                 | 银行拒绝付款。                                                                                                      | 有关详细信息，请联系银行。                             |

## <a name="export-data-page"></a>“导出数据”页

按照此页上的说明导出所需的数据。

说明：

- “导出数据”页本身不会刷新。 你可能需要手动刷新此页，才能看到最新数据。
- 筛选器可能会导致“无可用数据”错误出现。 这可能意味着，你已将默认时间段设置为三个月的时间，然后从超出该时间段的收入中选择付款 ID。 请延长时间段，然后重试。

## <a name="payments"></a>付款

![导出付款](images/pc-export-payments.png)

使用此选项，可以下载你的银行收到的给定计划的付款、相关税款和总收入金额。 因为此报告用于许多合作伙伴中心计划，所以你的报告中可能没有某些列。 下面介绍了这些列。

| 列名称              | 说明                                                                                                                               |
|--------------------------|-----------------------------------------------------------------------------------------------------------------------------------------  |
| participantID            | 计划下合作伙伴收入的主要标识                                                                             |
| participantIdType        | 应用商店计划的激励计划和卖方 ID 的程序 id                                                                |
| participantName          | 收入合作伙伴的名称                                                                                                               |
| programName              | 激励计划/应用商店计划的名称                                                                                                              |
| earned                   | 计划/participantID 赚取的金额（以付款币种为单位）                                                                       |
| earnedUSD                | 计划/participantID 赚取的金额（以美元为单位）                                                                                      |
| withheldTax              | 计划/participantID 的预扣税款金额（以付款币种为单位）                                                               |
| salesTax                 | 计划/participantID 的销售税总金额（以付款币种为单位）（仅适用于激励计划）                   |
| serviceFeeTax            | 计划/participantID 的 serviceFeeTax 总金额（以付款币种为单位）（仅适用于应用商店计划和 Azure 市场） |
| totalPayment             | 计划/participantID 的总付款（以本国币种为单位），不含预扣税款，但包括销售税（若有）   |
| currencyCode             | 付款币种代码                                                                                                                      |
| paymentMethod            | 用于支付合作伙伴的方法例如，电子银行转帐、贷方说明                                                     |
| paymentID                | 付款的唯一标识符。 此数字通常在银行报表中可见。 仅适用于 SAP 支付的 ()               |
| paymentStatus            | 付款状态                                                                                                                            |
| paymentStatusDescription | 付款状态的易记说明                                                                                                    |
| paymentDate              | Microsoft 发放付款的日期                                                                                                      |

## <a name="transaction-history"></a>交易历史记录

![导出交易历史记录](images/pc-export-transaction.png)

使用此选项，可以下载“交易历史记录”页中的每个收入行项，即收入类型、日期、相关交易金额、客户、产品和其他适用于计划的交易详细信息。

| 列名称                    | 说明                                                                                                                              | 是否适用于激励/应用商店/Azure 市场           |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| earningId                      | 每笔收入的唯一标识符                                                                                                       | All                                                            |
| participantID                  | 计划下合作伙伴收入的主要标识                                                                            | All                                                            |
| participantIdType              | 主要是激励计划的计划 ID，以及应用商店计划和 Azure 市场的卖家 ID                                          | All                                                            |
| participantName                | 收入合作伙伴的名称                                                                                                              | All                                                            |
| partnerCountryCode             | 收益合作伙伴所在国家/地区                                                                                                  | All                                                            |
| programName                    | 激励计划/应用商店计划的名称                                                                                                             | All                                                            |
| transactionId                  | 交易的唯一标识符                                                                                                    | All                                                            |
| transactionCurrency            | 原始客户交易发生时使用的币种（这不是合作伙伴所在位置的币种）                                     | All                                                            |
| transactionDate                | 交易日期。 适用于多次交易促成一笔收入的计划                                           | All                                                            |
| transactionExchangeRate        | 用于显示相应交易金额（以美元为单位）的汇率日期                                                                 | All                                                            |
| transactionAmount              | 交易金额（以产生收入的原始交易币种为单位）                                              | All                                                            |
| transactionAmountUSD           | 交易金额（以美元为单位）                                                                                                                | All                                                            |
| lever                          | 表示收入的业务规则                                                                                                  | All                                                            |
| earningRate                    | 用于交易金额以生成收入的激励率                                                                      | All                                                            |
| quantity                       | 因计划而异。 表示交易计划的已计费数量                                                            | All                                                            |
| quantityType                   | 指示数量的类型，例如，计费数量、MAU                                                                             | All                                                            |
| earningType                    | 指示是费用、回扣、市场活动、销售等。                                                                                          | All                                                            |
| earningAmount                  | 收入金额（以原始交易币种为单位）                                                                                      | All                                                            |
| earningAmountUSD               | 收入金额（以美元为单位）                                                                                                                    | All                                                            |
| earningDate                    | 收入日期                                                                                                                      | All                                                            |
| calculationDate                | 在系统中计算收入的日期                                                                                            | All                                                            |
| earningExchangeRate            | 用于显示相应金额（以美元为单位）的汇率                                                                                  | All                                                            |
| exchangeRateDate               | 用于计算 EarningAmount（以美元为单位）的汇率日期                                                                                   | All                                                            |
| paymentAmountWOTax             | 只为 "已发送" 付款支付的金额为 "已发送" 的货币而不包含税金)  (收益                                                                 | All                                                            |
| paymentCurrency                | 合作伙伴在付款配置文件中选择的付款币种。 仅针对状态为“已发送”的付款显示                                                   | All                                                            |
| paymentExchangeRate            | 用于使用 ExchangeRateDate 计算 paymentAmountWOTax（以付款币种为单位）的汇率                                            | All                                                            |
| claimId                        | 索赔的唯一标识符                                                                                                              | 激励 - 仅某些计划                                |
| planId                         | 计划的唯一标识符                                                                                                               | 激励 - 仅某些计划                                |
| paymentId                      | 付款的唯一标识符。 此编号通常显示在银行对帐单中                                                 | 仅适用于 SAP 付款                                              |
| paymentStatus                  | 付款状态                                                                                                                           | All                                                            |
| paymentStatusDescription       | 付款状态的易记说明                                                                                                   | All                                                            |
| customerId                     | 始终为空白                                                                                                                     | 仅激励计划（例外：OEM）和 Azure 市场 |
| customerName                   | 始终为空白                                                                                                                     | 仅激励计划（例外：OEM）和 Azure 市场 |
| partNumber                     | 始终为空白                                                                                                                     | 某些激励计划和应用商店计划以及 Azure 市场        |
| productName                    | 关联到交易的产品名称                                                                                                       | All                                                            |
| productId                      | 唯一产品标识符                                                                                                                | 应用商店和 Azure 市场                                    |
| parentProductId                | 唯一父产品标识符。 请注意：如果没有用来交易的父产品，则父产品 ID = 产品 ID。 | 应用商店和 Azure 市场                                    |
| parentProductName              | 父产品名称。 请注意：如果没有用来交易的父产品，则父产品名称 = 产品名称。   | 应用商店和 Azure 市场                                    |
| productType                    | 产品的类型（如应用、加载项、游戏等）                                                                                        | 应用商店和 Azure 市场                                    |
| invoiceNumber                  | 发票编号（仅适用于 EA）                                                                                                  | 激励和 Azure 市场 - 仅某些计划           |
| subscriptionId                 | 与客户关联的订阅标识符                                                                                         | 激励 - 仅某些计划                                 |
| subscriptionStartDate          | 订阅开始日期                                                                                                                  | 激励 - 仅某些计划                                 |
| subscriptionEndDate            | 订阅结束日期                                                                                                                    | 激励 - 仅某些计划                                 |
| resellerId                     | 经销商标识符                                                                                                                      | 激励 - 仅某些计划                                 |
| resellerName                   | 经销商名称                                                                                                                            | 激励 - 仅某些计划                                 |
| distributorId                  | 分销商标识符                                                                                                                   | 激励 - 仅某些计划                                 |
| distributorName                | 分销商名称                                                                                                                         | 激励 - 仅某些计划                                 |
| agreementNumber                | 协议编号                                                                                                                         | 激励 - 仅某些计划                                 |
| AgreementStartDate             | 协议开始日期                                                                                                                     | 激励 - 仅某些计划                                 |
| AgreementEndDate               | 协议结束日期                                                                                                                       | 激励 - 仅某些计划                                 |
| workload                       | 工作负荷                                                                                                                                 | 激励 - 仅某些计划                                 |
| TransactionType                | 交易的类型（如购买、退款、冲销、拒付等）                                                               | 应用商店和 Azure 市场                                    |
| localProviderSeller            | 记录的本地提供商/卖家                                                                                                          | 仅应用商店                                                     |
| taxRemitted                    | 减免税额（销售税、使用税或增值税/消费税）。                                                                                   | 应用商店和 Azure 市场                                    |
| taxRemitModel                  | 免税方（销售税、使用税或增值税/消费税）。                                                                    | 仅应用商店                                                     |
| storeFee                       | Microsoft 为使应用或外接程序在应用商店中可用而保留的金额。                                           | 仅应用商店                                                     |
| transactionPaymentMethod       | 客户用于交易的付款方式（如信用卡、移动运营商结算、PayPal 等）                                | 应用商店和 Azure 市场                                    |
| tpan                           | 表示第三方广告网络                                                                                                     | 应用商店 - 仅广告                                               |
| customerCountry                | 客户所在国家/地区                                                                                                                         | 应用商店和 Azure 市场                                    |
| customerCity                   | 客户所在城市                                                                                                                            | 应用商店和 Azure 市场                                    |
| customerState                  | 客户所在省/自治区/直辖市                                                                                                                           | 应用商店和 Azure 市场                                    |
| customerZip                    | 客户邮政编码                                                                                                                 | 应用商店和 Azure 市场                                    |
| purchaseTypeCode               | 始终为空白                                                                                                                     | 激励计划 - CRI                                        |
| purchaseOrderType              | 始终为空白                                                                                                                     | 激励计划 - CRI                                        |
| purchaseOrderCoverageStartDate | 始终为空白                                                                                                                     | 激励计划 - CRI                                        |
| purchaseOrderCoverageEndDate   | 始终为空白                                                                                                                     | 激励计划 - CRI                                        |
| programOfferingLevel           |                                                                                                                                          | 激励计划 - CRI                                        |
| TenantID                       |                                                                                                                                          | 激励计划                                             |
| externalReferenceId            | 计划的唯一标识符                                                                                                        | 直接付款计划（激励和应用商店）                      |
| externalReferenceIdLabel       | 唯一标识符标签                                                                                                                  | 直接付款计划（激励和应用商店）                      |

## <a name="historical-statements"></a>历史对帐单

![导出历史对帐单](images/pc-export-statements.png)

2019 年 7 月 1 日之前的交易历史记录另行处理。 对帐单将使用以下字段，而不是当前字段。

> [!NOTE]
> 旧式交易历史记录中有一个名为“预留”的列，它对应于新式历史记录中的“收入”列，区别在于它不包括状态为“付款已发送”的所有收入。

> [!NOTE]
> 筛选器 (3M、6分钟、12M 等 ) 将不会应用于 " **历史语句** " 部分。

| 字段名称              | 说明                                                                                                                                                             |
|-------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 收入来源          | 收入的来源，基于交易发生的位置（例如 Microsoft Store、Windows Phone 应用商店、Windows 应用商店 8、广告等）                  |
| 订单 ID                | 唯一订单标识符。 此 ID 允许你通过各自的非购买交易（如退款、拒付等）标识购买交易。 两者具有相同的订单 ID。 此外，在拆分付费（已针对单个购买使用了多种付款方式）时，它将允许你链接购买交易。 |
| Transaction ID          | 唯一交易标识符。                                                                                                                                          |
| 交易日期/时间   | 交易发生的日期和时间 (UTC)。                                                                                                                       |
| 父产品 ID       | 唯一父产品标识符。 请注意：如果没有用来交易的父产品，则父产品 ID = 产品 ID。                                |
| 产品 ID              | 唯一产品标识符。                                                                                                                                              |
| 父产品名称     | 父产品名称。 请注意：如果没有用来交易的父产品，则父产品名称 = 产品名称。                                  |
| 产品名称            | 产品的名称。                                                                                                                                                    |
| 产品类型            | 产品的类型（如应用、加载项、游戏等）                                                                                                                       |
| 数量                | 当“收入来源”为“适用于企业的 Microsoft Store”时，“数量”表示购买的许可证数量。 对于其他所有收入来源，“数量”始终为“1”。 注意：即使当单笔交易因使用了两种不同的付款方式而拆分为两个明细项目时，每个明细项目都将显示数量为 1。 |
| 事务类型        | 交易的类型（如购买、退款、冲销、拒付等）                                                                                              |
| 付款方式          | 客户用于交易的付款方式（如信用卡、移动运营商结算、PayPal 等）                                                               |
| 国家/地区        | 进行交易的国家/地区。                                                                                                                          |
| 本地提供商/卖家 | 记录的本地提供商/卖家。                                                                                                                                        |
| 交易币种    | 交易的币种。                                                                                                                                            |
| 交易金额      | 交易的金额。                                                                                                                                              |
| 汇出税款            | 减免税额（销售税、使用税或增值税/消费税）。                                                                                                                  |
| 净收入            | 交易金额减去免除的税收。                                                                                                                                   |
| 应用商店费用               | 由 Microsoft 扣除的净收入百分比将用作支付在应用商店中提供应用或加载项的费用。                                                      |
| 应用收款            | 净收入减去应用商店费用。                                                                                                                                       |
| 预扣税款          | 所得税扣缴金额。 **保留**的 .csv 文件中未包含 (。 )                                                                                                 |
| 付款                 | 应用收款减去任何相应所得税预扣金额（金额以交易币种显示）。 **保留**的 .csv 文件中未包含 (。 )                                |
| 外汇汇率                 | 用于将交易币种兑换为付款货币的外汇汇率。                                                                                         |
| 付款币种        | 付款时所使用的货币。                                                                                                                                       |
| 转换后的付款       | 使用外汇汇率，将付款金额兑换为付款货币。                                                                                                         |
| 税款汇出模型         | 免税方（销售税、使用税或增值税/消费税）。                                                                                                   |
| 资格日期/时间   | 交易收款符合成为付款的条件的日期和时间 (UTC)。 在创建付款时，这包括资格日期时间早于付款创建日期的交易收益。 （仅包括在“已保留”**** 的 .csv 文件中。） |
| Charges                 | 显示“交易金额”列汇总的所有费用明细的细目。 （仅包括在 Azure Marketplace 中；不包括在“已保留”**** 的 .csv 文件中。） |