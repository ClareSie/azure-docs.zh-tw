---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/09/2020
ms.author: tamram
ms.openlocfilehash: 27617da97ced9ac775beae71e4c25202944b2aba
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "78940947"
---
下表描述資料表儲存體的容量、延展性和效能目標。

| 資源 | 目標 |
|----------|---------------|
| Azure 儲存體帳戶中的資料表數目 | 僅受限於儲存體帳戶的容量 |
| 資料表中的資料分割數目 | 僅受限於儲存體帳戶的容量 |
| 資料分割中的實體數目 | 僅受限於儲存體帳戶的容量 |
| 單一資料表的大小上限 | 500 TiB |
| 單一實體的大小上限，包括所有屬性值 | 1 MiB |
| 資料表實體中的屬性數目上限 | 255 (包括 3 個系統屬性：**PartitionKey**、**RowKey** 和 **Timestamp**) |
| 實體中個別屬性的大小總和上限 | 依屬性類型而有所不同。 如需詳細資訊，請參閱[了解資料表服務的資料模型](/rest/api/storageservices/understanding-the-table-service-data-model)中的**屬性類型**。 |
| **PartitionKey** | 大小最多 1 KiB 的字串 |
| **RowKey** | 大小最多 1 KiB 的字串 |
| 實體群組交易的大小 | 交易最多可以包含 100 個實體，而承載大小必須小於 4 MiB。 實體群組交易只能包含實體的更新一次。 |
| 每個資料表的預存存取原則的最大數目 | 5 |
| 每一儲存體帳戶的要求率上限 | 每秒 20,000 筆交易，假設實體大小為 1 KiB |
| 單一資料表分割的目標輸送量 (1 KiB 實體) | 每秒最多 2,000 個實體 |