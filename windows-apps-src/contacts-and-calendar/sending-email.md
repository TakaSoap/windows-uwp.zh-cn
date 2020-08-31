---
description: 显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。
title: 发送电子邮件
ms.assetid: 74511E90-9438-430E-B2DE-24E196A111E5
keywords: 联系人, 电子邮件, 发送
ms.date: 10/11/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 47d07fa1932aae87704f7922762a8f0b7e430444
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154661"
---
# <a name="send-email"></a>发送电子邮件

显示如何启动撰写电子邮件对话框以允许用户发送电子邮件。 你可以在显示该对话框之前，使用数据预填充电子邮件的字段。 该消息将在用户点击发送按钮后发出。

**本文内容**

-   [启动撰写电子邮件对话框](#launch-the-compose-email-dialog)
-   [摘要和后续步骤](#summary-and-next-steps)
-   [相关主题](#related-topics)

## <a name="launch-the-compose-email-dialog"></a>启动撰写电子邮件对话框

创建新 [**EmailMessage**](/uwp/api/Windows.ApplicationModel.Email.EmailMessage) 对象，并设置你要在撰写电子邮件对话框中预填充的数据。 调用 [**ShowComposeNewEmailAsync**](/uwp/api/windows.applicationmodel.email.emailmanager.showcomposenewemailasync) 显示对话框。

``` cs
private async Task ComposeEmail(Windows.ApplicationModel.Contacts.Contact recipient,
    string subject, string messageBody)
{
    var emailMessage = new Windows.ApplicationModel.Email.EmailMessage();
    emailMessage.Body = messageBody;

    var email = recipient.Emails.FirstOrDefault<Windows.ApplicationModel.Contacts.ContactEmail>();
    if (email != null)
    {
        var emailRecipient = new Windows.ApplicationModel.Email.EmailRecipient(email.Address);
        emailMessage.To.Add(emailRecipient);
        emailMessage.Subject = subject;
    }

    await Windows.ApplicationModel.Email.EmailManager.ShowComposeNewEmailAsync(emailMessage);
}
```

>[!NOTE]
> 使用 [EmailAttachment](/uwp/api/windows.applicationmodel.email.emailattachment) 类添加到电子邮件中的附件仅会显示在邮件应用程序中。 如果用户将任何其他邮件程序配置为其默认邮件程序，则将显示 "撰写" 窗口而不显示附件。 这是一个已知问题。

## <a name="summary-and-next-steps"></a>总结和后续步骤

本主题已向你展示如何启动撰写电子邮件对话框。 有关选择用作电子邮件接收方联系人的信息，请参阅[选择联系人](selecting-contacts.md)。 请参阅 [**PickSingleFileAsync**](/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) 以选择要用作电子邮件附件的文件。

## <a name="related-topics"></a>相关主题

* [选择联系人](selecting-contacts.md)
 