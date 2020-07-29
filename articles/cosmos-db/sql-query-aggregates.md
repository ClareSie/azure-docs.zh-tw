---
title: Azure Cosmos DB 中的彙總函式
description: 瞭解 SQL 彙總函式語法、Azure Cosmos DB 支援的彙總函式類型。
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/16/2020
ms.author: tisande
ms.openlocfilehash: 24acd1e9c13320244ff4c27abd13abeda6f70b2b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "79464456"
---
# <a name="aggregate-functions-in-azure-cosmos-db"></a>Azure Cosmos DB 中的彙總函式

彙總函式會對子句中的一組值執行計算 `SELECT` ，並傳回單一值。 例如，下列查詢會傳回容器內的專案計數 `Families` ：

## <a name="examples"></a>範例

```sql
    SELECT COUNT(1)
    FROM Families f
```

結果如下：

```json
    [{
        "$1": 2
    }]
```

您也可以使用 VALUE 關鍵字，只傳回匯總的純量值。 例如，下列查詢會以單一數字傳回值的計數：

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
```

結果如下：

```json
    [ 2 ]
```

您也可以將匯總與篩選結合在一起。 例如，下列查詢會傳回位址狀態為的專案計數 `WA` 。

```sql
    SELECT VALUE COUNT(1)
    FROM Families f
    WHERE f.address.state = "WA"
```

結果如下：

```json
    [ 1 ]
```

## <a name="types-of-aggregate-functions"></a>彙總函式的類型

SQL API 支援下列彙總函式。 `SUM`和會對 `AVG` 數值執行運算，和、和會 `COUNT` `MIN` `MAX` 處理數位、字串、布林值和 null。

| 函式 | 說明 |
|-------|-------------|
| COUNT | 以運算式傳回項目的數目。 |
| SUM   | 以運算式傳回所有值的總和。 |
| 最小值   | 傳回運算式中的最小值。 |
| 最大值   | 傳回運算式中的最大值。 |
| 平均   | 以運算式傳回值的平均。 |

您也可以匯總陣列反復專案的結果。

> [!NOTE]
> 在 Azure 入口網站的資料總管中，匯總查詢可能只會匯總一個查詢頁面上的部分結果。 SDK 會產生所有頁面的單一累計值。 若要使用程式碼執行匯總查詢，您需要 .NET SDK 1.12.0、.NET Core SDK 1.1.0 或 JAVA SDK 1.9.5 或更新版本。

## <a name="remarks"></a>備註

這些匯總系統函數將受益于[範圍索引](index-policy.md#includeexclude-strategy)。 如果您預期會在 `COUNT` 屬性上執行、、 `SUM` `MIN` 、 `MAX` 或， `AVG` 您應該[在索引編制原則中包含相關的路徑](index-policy.md#includeexclude-strategy)。

## <a name="next-steps"></a>後續步驟

- [Azure Cosmos DB 簡介](introduction.md)
- [系統函數](sql-query-system-functions.md)
- [使用者定義函數](sql-query-udfs.md)