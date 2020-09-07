---
title: 在 UWP 应用中使用 MongoDB 数据库
description: 了解如何将通用 Windows 平台 (UWP) 应用直接连接到 MongoDB 数据库，并以编程方式测试连接。
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MongoDB, 数据库
ms.localizationpriority: medium
ms.openlocfilehash: aaa035393e49d6bdad49faa806485cc51d21bb84
ms.sourcegitcommit: cb5af00af05e838621c270173e7fde1c5d2168ef
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/28/2020
ms.locfileid: "89043509"
---
# <a name="use-a-mongodb-database"></a>使用 MongoDB 数据库
本文包含启用在 UWP 应用中使用 MongoDB 数据库功能所需的步骤。 另外，本文还包含一个小的代码片段，演示如何在代码中与数据库交互。

## <a name="set-up-your-solution"></a>设置解决方案

要将应用直接连接到 MongoDB 数据库，请确保项目的最低版本以 Fall Creators Update（版本 16299）为目标。  可以在 UWP 项目的属性页中找到该信息。

![VisualStudio 中“目标属性”窗格的图片，显示目标版本和最低版本已设置为 Fall Creators Update](images/min-version-fall-creators.png)

打开“程序包管理器控制台”  （“视图”->“其他窗口”->“程序包管理器控制台”）。 使用 **Install-Package MongoDB.Driver** 命令安装 MongoDB 的驱动程序。 这样就可以通过编程方式访问 MongoDB 数据库。

## <a name="test-your-connection-using-sample-code"></a>使用示例代码测试连接
以下示例代码从远程 MongoDB 客户端获取一个集合，然后向该集合添加新文档。 然后，它使用 MongoDB API 检索集合的新大小和插入的文档，并将其输出。请注意，需自定义 IP 地址和数据库名称。

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```
