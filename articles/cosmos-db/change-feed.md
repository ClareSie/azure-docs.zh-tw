---
title: 使用 Azure Cosmos DB 中的變更摘要支援
description: 使用 Azure Cosmos DB 更改源支援來跟蹤文檔中的更改、基於事件的處理（如觸發器）以及使緩存和分析系統保持最新
author: TheovanKraay
ms.author: thvankra
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 11/25/2019
ms.reviewer: sngun
ms.custom: seodec18
ms.openlocfilehash: 898dfe7a619981b93af98effa942fdecbeb42dde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79368123"
---
# <a name="change-feed-in-azure-cosmos-db---overview"></a>Azure Cosmos DB 中的變更摘要 - 概觀

Azure Cosmos DB 中的變更摘要支援是靠接聽 Azure Cosmos 容器的任何變更而運作。 然後變更摘要會輸出已排序的文件清單，這些文件已依其修改的順序變更過。 變更會保存，可進行非同步累加處理，而輸出可配送給一或多個取用者進行平行處理。 

Azure Cosmos DB 非常適合用於 IoT、遊戲、零售，以及操作記錄應用程式。 這類應用程式常用的設計模式，是利用資料的變更觸發其他動作。 其他動作的範例包括：

* 於插入或修改項目時觸發通知或呼叫 API。
* IoT 的即時串流處理或在操作資料上的即時分析處理。
* 藉由與快取、搜尋引擎、或資料倉儲進行同步處理，或將資料封存到冷儲存體，進行額外的資料移動。

Azure Cosmos DB 中的變更摘要可讓您針對每一個模式建置有效率且可調整的解決方案，如下圖所示：

![使用 Azure Cosmos DB 變更摘要來提供即時分析和事件導向的計算案例](./media/change-feed/changefeedoverview.png)

## <a name="supported-apis-and-client-sdks"></a>支援的 API 和用戶端 SDK

此功能目前受到下列 Cosmos DB API 和用戶端 SDK 支援。

| **用戶端驅動程式** | **Azure CLI** | **SQL API** | **Azure 宇宙 DB 的卡珊多拉 API** | **Azure 宇宙 DB 的蒙戈DB API** | **格雷姆林 API**|**表 API** |
| --- | --- | --- | --- | --- | --- | --- |
| .NET | NA | 是 | 是 | 是 | 是 | 否 |
|Java|NA|是|是|是|是|否|
|Python|NA|是|是|是|是|否|
|節點/JS|NA|是|是|是|是|否|

## <a name="change-feed-and-different-operations"></a>變更摘要和不同的作業

現在，您會看到變更摘要中的所有作業。 可讓您控制變更摘要的功能還無法使用，只能執行特定作業，例如更新，無法執行像插入等作業。 您可以在專案上添加"軟標記"以進行更新，並在處理更改源中的項時基於該標記進行篩選。 當前更改源不會記錄刪除。 您可以像先前的範例那樣，在正在刪除的項目上新增軟標記，可以在項目中新增稱為 "deleted" 的屬性，並將它設定為 "true"，然後在項目上設定 TTL，讓它可以被自動刪除。 您可以讀取歷史專案的更改源（與專案對應的最新更改，不包括中間更改），例如，五年前添加的專案。 如果項目未刪除，您可以讀取至原始容器的變更摘要。

### <a name="sort-order-of-items-in-change-feed"></a>變更摘要中項目的排序順序

變更摘要項目會按修改時間順序排列。 此排序次序保證每個邏輯分區鍵。

### <a name="consistency-level"></a>一致性級別

在"最終一致性"級別使用更改源時，後續更改源讀取操作之間可能存在重複事件（一個讀取操作的最後一個事件顯示為下一個讀取操作中的第一個事件）。

### <a name="change-feed-in-multi-region-azure-cosmos-accounts"></a>多重區域 Azure Cosmos 帳戶中的變更摘要

在多重區域 Azure Cosmos 帳戶，中，如果寫入區域進行容錯移轉，變更摘要會跨手動容錯移轉作業運作，而且會連續運作。

### <a name="change-feed-and-time-to-live-ttl"></a>變更摘要和存留時間 (TTL)

如果項目上的 TTL (存留時間) 屬性設為 -1，變更摘要會永久保存。 如果未刪除資料，即會一直保留在變更摘要中。  

### <a name="change-feed-and-_etag-_lsn-or-_ts"></a>變更摘要和 _etag、_lsn 或 _ts

_etag 格式是作為內部之用，因為該格式可以隨時變更，請別依賴它。 _ts 是修改或建立時間戳記。 您可以使用 _ts，依時間順序進行比較。 _lsn是僅為更改源添加的批次處理 ID;它表示事務 ID。 許多項目可能有相同的 _lsn。 FeedResponse 上的 ETag 與您在項目上看到的 _etag 不同。 _etag 是內部識別碼，而且用於並行控制，它會指示項目版本，而使用 ETag 來排序摘要。

## <a name="change-feed-use-cases-and-scenarios"></a>變更摘要使用個案和案例

變更摘要可有效處理具有大量寫入的大型資料集。 變更摘要也提供了替代方式，以查詢整個資料集來識別已變更的項目。

### <a name="use-cases"></a>使用案例

例如，您可以利用變更摘要有效率地執行下列工作︰

* 透過儲存在 Azure Cosmos DB 中的資料，來更新快取、更新搜尋索引，或更新資料倉儲。

* 實作應用程式層級的資料階層處理和封存，例如在 Azure Cosmos DB 中儲存「熱資料」，並將「冷資料」轉存到其他儲存系統，例如 [Azure Blob 儲存體](../storage/common/storage-introduction.md)。

* 執行零停機時間移轉至另一個 Azure Cosmos 帳戶或具有不同邏輯分割區索引鍵的另一個 Azure Cosmos 容器。

* 使用 Azure Cosmos DB 實作[lambda 架構](https://blogs.technet.microsoft.com/msuspartner/2016/01/27/azure-partner-community-big-data-advanced-analytics-and-lambda-architecture/)，其中 Azure Cosmos DB 同時支援即時、批次和服務層，因此可支援具有低 TCO 的 lambda 架構。

* 接收與儲存來自裝置、感應器、基礎結構和應用程式的事件資料，並即時處理這些事件，例如使用 [Spark](../hdinsight/spark/apache-spark-overview.md)。  下圖顯示如何透過變更摘要使用 Azure Cosmos DB 實作 lambda 架構：

![運用 Azure Cosmos DB 進行擷取和查詢的 lambda 管線](./media/change-feed/lambda.png)

### <a name="scenarios"></a>案例

以下是您可以使用變更摘要輕鬆實作的一些案例：

* 在您的 [serverless](https://azure.microsoft.com/solutions/serverless/) Web 和行動應用程式內，您可以追蹤像是對客戶設定檔、喜好設定或其位置等所做之所有變更的事件，並觸發特定動作，例如使用 [Azure Functions](change-feed-functions.md) 傳送推播通知到客戶裝置。

* 例如，如果您使用 Azure Cosmos DB 來建置遊戲，就可以根據完成遊戲的分數，使用變更摘要來實作即時排行榜。


## <a name="working-with-change-feed"></a>使用變更摘要

您可以搭配變更摘要使用下列選項：

* [搭配使用變更摘要與 Azure Functions](change-feed-functions.md)
* [將更改饋送與更改饋送處理器一起使用](change-feed-processor.md) 

容器內每個邏輯分割區索引鍵都可使用變更摘要，因此可以配送給一或多個取用者以進行平行處理，如下圖所示。

![Azure Cosmos DB 變更摘要的分散式處理](./media/change-feed/changefeedvisual.png)

## <a name="features-of-change-feed"></a>變更摘要的功能

* 根據預設，所有 Azure Cosmos 帳戶都會啟用變更摘要。

* 和任何其他 Azure Cosmos DB 作業一樣，您可以在與您的 Azure Cosmos 資料庫關聯的任何區域中，使用[佈建輸送量](request-units.md)從變更摘要讀取。

* 變更摘要包含對容器內項目進行的插入與更新作業。 您可以擷取刪除項目，方法是在您的項目 (例如文件) 內設定「虛刪除」旗標來取代刪除動作。 或者，您可以使用 [TTL 功能](time-to-live.md)為項目設定有限的逾期期限。 例如 24 小時，並使用該屬性的值擷取刪除項目。 使用此解決方案，您必須在比 TTL 逾期期限更短的時間間隔內處理變更。 

* 對項目所做的每項變更皆會在變更摘要中出現一次，且用戶端必須管理檢查點邏輯。 如果要避免管理檢查點的複雜性，更改饋送處理器提供自動檢查點和"至少一次"語義。 請參閱[將更改進給與更改饋送處理器一起使用](change-feed-processor.md)。

* 變更記錄檔中只會包含指定項目的最新變更。 中繼變更可能無法使用。

* 變更摘要會依照各個邏輯分割區索引鍵值內的修改順序排序。 跨分割區索引鍵值順序不會是固定的。

* 變更可以從任何時間點同步處理，也就是說，可用變更沒有固定的資料保留期限。

* Azure Cosmos 容器所有邏輯分割區索引鍵的變更都是以平行方式提供使用。 這項功能可讓大型容器的變更由多個取用者平行處理。

* 應用程式可以同時請求同一容器上的多個更改源。 ChangeFeedOptions.StartTime 可用來提供初始的開始點。 例如，尋找與指定時鐘時間相對應的接續權杖。 ContinuationToken 如經指定，會優先於 StartTime 和 StartFromBeginning 值。 ChangeFeedOptions.StartTime 的精確度為 5 秒內。 

## <a name="change-feed-in-apis-for-cassandra-and-mongodb"></a>更改卡珊多拉和蒙戈DB的 API 中源

更改源功能在 MongoDB API 中顯示為更改流，在 Cassandra API 中使用謂詞顯示。 要瞭解有關 MongoDB API 的實現詳細資訊的詳細資訊，請參閱[MongoDB 的 Azure Cosmos DB API 中的更改流](mongodb-change-streams.md)。

本機 Apache Cassandra 提供更改資料捕獲 （CDC），這是一種機制，用於標記存檔的特定表，以及在達到 CDC 日誌的可配置磁片大小後拒絕對這些表的寫入。 Cassandra 的 Azure Cosmos DB API 中的更改源功能增強了通過 CQL 使用謂詞查詢更改的能力。 要瞭解有關實現詳細資訊的詳細資訊，請參閱[Cassandra 的 Azure Cosmos DB API 中的更改源](cassandra-change-feed.md)。

## <a name="next-steps"></a>後續步驟

您現在可以在下列文章中繼續深入了解變更摘要：

* [讀取變更摘要的選項](read-change-feed.md)
* [搭配使用變更摘要與 Azure Functions](change-feed-functions.md)
* [使用變更摘要處理器](change-feed-processor.md)
