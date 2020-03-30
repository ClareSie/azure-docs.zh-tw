---
title: 使用 Azure Cosmos DB 中的日期
description: 瞭解如何在 Azure Cosmos DB 中存儲、索引和查詢 DataTime 物件
ms.service: cosmos-db
author: SnehaGunda
ms.author: sngun
ms.topic: conceptual
ms.date: 03/03/2020
ms.openlocfilehash: 92fa35fbe8e5eef4dbdc8b6c47a9055affd449a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "78273193"
---
# <a name="working-with-dates-in-azure-cosmos-db"></a>使用 Azure Cosmos DB 中的日期

Azure Cosmos DB 透過原生 [JSON](https://www.json.org) 資料模型，提供結構描述的彈性和豐富的索引編製功能。 所有 Azure Cosmos DB 資源 (包括資料庫、容器、文件及預存程序) 都會建立模型，並以 JSON 文件的形式儲存。 為了滿足可攜性需求，JSON (和 Azure Cosmos DB) 僅支援一小組基本類型︰字串、數字、布林值、陣列、物件及 Null。 不過，JSON 具有彈性，讓開發人員和架構可以使用這些基本類型及加以組合為物件或陣列，來表示更複雜的類型。

除了基本類型，許多應用程式需要 DateTime 類型來表示日期和時間戳記。 此文章說明開發人員如何使用 .NET SDK 來儲存、擷取、查詢 Azure Cosmos DB 中的日期。

## <a name="storing-datetimes"></a>儲存 DateTimes

Azure Cosmos DB 支援 JSON 類型，如字串、數位、布林、空、陣列、物件。 它不直接支援日期時間類型。 目前，Azure Cosmos DB 不支援日期當地語系化。 因此，您需要將 DateTimes 存儲為字串。 Azure Cosmos DB 中 DateTime 字串`YYYY-MM-DDThh:mm:ss.sssZ`的建議格式遵循 ISO 8601 UTC 標準。 建議將 Azure Cosmos DB 中的所有日期存儲為 UTC。 將日期字串轉換為此格式將允許按字典順序對日期進行排序。 如果存儲了非 UTC 日期，則必須在用戶端處理邏輯。 要將本地日期時間轉換為 UTC，偏移量必須已知/存儲為 JSON 中的屬性，並且用戶端可以使用偏移量計算 UTC DateTime 值。

大部分應用程式可以使用 DateTime 的預設字串表示法，原因如下︰

* 可以比較字串，且在將 DateTime 值轉換成字串時會保留這些字元的相對順序。
* 這個方法不需要任何自訂程式碼或屬性來進行 JSON 轉換。
* 儲存在 JSON 中的日期是人類可閱讀的。
* 此方法可利用 Azure Cosmos DB 的索引來提升查詢效能。

例如，下列程式碼片段使用 .NET SDK 將 `Order` 物件 (包含 `ShipDate` 和 `OrderDate` 兩個 DateTime 屬性) 儲存為文件︰

```csharp
    public class Order
    {
        [JsonProperty(PropertyName="id")]
        public string Id { get; set; }
        public DateTime OrderDate { get; set; }
        public DateTime ShipDate { get; set; }
        public double Total { get; set; }
    }

    await container.CreateItemAsync(
        new Order
        {
            Id = "09152014101",
            OrderDate = DateTime.UtcNow.AddDays(-30),
            ShipDate = DateTime.UtcNow.AddDays(-14), 
            Total = 113.39
        });
```

這份文件會在 Azure Cosmos DB 中以下列方式儲存：

```json
    {
        "id": "09152014101",
        "OrderDate": "2014-09-15T23:14:25.7251173Z",
        "ShipDate": "2014-09-30T23:14:25.7251173Z",
        "Total": 113.39
    }
```  

或者，您可以將 DateTimes 儲存為 Unix 時間戳記，也就是用一個數字來表示自 1970 年 1 月 1 日之後經過的秒數。 Azure Cosmos DB 的內部時間戳記 (`_ts`) 屬性便是使用此方法。 您可以使用 [UnixDateTimeConverter](https://msdn.microsoft.com/library/azure/microsoft.azure.documents.unixdatetimeconverter.aspx) 類別將 DateTime 序列化為數字。

## <a name="querying-datetimes-in-linq"></a>用 LINQ 查詢 DateTime

SQL .NET SDK 會自動支援透過 LINQ 查詢儲存在 Azure Cosmos DB 中的資料。 例如，以下程式碼片段顯示 LINQ 查詢，該查詢篩選最近三天內發貨的訂單：

```csharp
    IQueryable<Order> orders = container.GetItemLinqQueryable<Order>(allowSynchronousQueryExecution: true).Where(o => o.ShipDate >= DateTime.UtcNow.AddDays(-3));
```

已轉換為以下 SQL 語句並在 Azure Cosmos DB 上執行：

```sql
    SELECT * FROM root WHERE (root["ShipDate"] >= "2016-12-18T21:55:03.45569Z")
```

您可以在[LINQ 中查詢 Cosmos DB](sql-query-linq-to-sql.md)中的 LINQ 資料庫，瞭解有關 Azure Cosmos DB 的 SQL 查詢語言和 LINQ 提供程式的更多資訊。

## <a name="indexing-datetimes-for-range-queries"></a>索引 DateTime 以進行範圍查詢

查詢與日期時間值是常見的。 要有效地執行這些查詢，必須在查詢篩選器中的任何屬性上定義索引。

您可以參閱 [Azure Cosmos DB 索引編製原則](index-policy.md)，來深入了解如何設定索引編製原則。 

## <a name="next-steps"></a>後續步驟

* 下載並執行 [GitHub 上的程式碼範例](https://github.com/Azure/azure-documentdb-dotnet/tree/master/samples/code-samples)
* 深入了解 [SQL 查詢](sql-query-getting-started.md)
* 深入了解 [Azure Cosmos DB 編製索引原則](index-policy.md)
