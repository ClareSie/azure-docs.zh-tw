---
title: Azure Cosmos DB 中的 WHERE 子句
description: 瞭解適用于 Azure Cosmos DB 的 SQL WHERE 子句
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/06/2020
ms.author: tisande
ms.openlocfilehash: 483a0533eafc81ef8698d260a753062ae074f6d4
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "78898759"
---
# <a name="where-clause-in-azure-cosmos-db"></a>Azure Cosmos DB 中的 WHERE 子句

選擇性的 WHERE 子句（`WHERE <filter_condition>`）指定來源 JSON 專案必須滿足的條件，查詢才能將其包含在結果中。 JSON 專案必須評估要視為結果的指定`true`條件。 索引層使用 WHERE 子句來判斷可為結果一部分之來源專案的最小子集。
  
## <a name="syntax"></a>語法
  
```sql  
WHERE <filter_condition>  
<filter_condition> ::= <scalar_expression>  
  
```  
  
## <a name="arguments"></a>引數

- `<filter_condition>`  
  
   指定要傳回的文件必須滿足的條件。  
  
- `<scalar_expression>`  
  
   表示要計算之值的運算式。 如需詳細資訊，請參閱純量[運算式](sql-query-scalar-expressions.md)。  
  
## <a name="remarks"></a>備註
  
  為了傳回文件，指定為篩選條件的運算式必須評估為 True。 只有布林值`true`才會滿足條件，任何其他值：未定義、null、False、數位、陣列或物件將無法滿足條件。

  如果您在`WHERE`子句中包含分割區索引鍵做為相等篩選準則的一部分，則您的查詢將會自動篩選為僅相關的分割區。

## <a name="examples"></a>範例

下列查詢會要求包含`id`屬性的專案，其值為`AndersenFamily`。 它會排除沒有`id`屬性或值不相符`AndersenFamily`的任何專案。

```sql
    SELECT f.address
    FROM Families f
    WHERE f.id = "AndersenFamily"
```

結果如下：

```json
    [{
      "address": {
        "state": "WA",
        "county": "King",
        "city": "Seattle"
      }
    }]
```

### <a name="scalar-expressions-in-the-where-clause"></a>WHERE 子句中的純量運算式

前一個範例已顯示簡單的相等查詢。 SQL API 也支援各種純量[運算式](sql-query-scalar-expressions.md)。 最常用的是二元和一元運算式。 來源 JSON 物件中的屬性參考也是有效的運算式。

您可以使用下列支援的二元運算子：  

|**運算子類型**  | **值** |
|---------|---------|
|算術 | +,-,*,/,% |
|位元    | \|, &, ^, <<, >>, >>> (zero-fill right shift) |
|邏輯    | AND、OR、NOT      |
|比較 | =, !=, &lt;, &gt;, &lt;=, &gt;=, <> |
|字串     |  \|\| (串連) |

下列查詢會使用二元運算子：

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE c.grade % 2 = 1     -- matching grades == 5, 1

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade ^ 4 = 1    -- matching grades == 5

    SELECT *
    FROM Families.children[0] c
    WHERE c.grade >= 5    -- matching grades == 5
```

您也可以使用一元運算子 +,-, ~，而不是查詢中的，如下列範例所示：

```sql
    SELECT *
    FROM Families.children[0] c
    WHERE NOT(c.grade = 5)  -- matching grades == 1

    SELECT *
    FROM Families.children[0] c
    WHERE (-c.grade = -5)  -- matching grades == 5
```

您也可以在查詢中使用屬性參考。 例如， `SELECT * FROM Families f WHERE f.isRegistered`會傳回包含屬性`isRegistered`且值等於的 JSON 專案。 `true` `false`任何其他值（例如、 `null`、 `Undefined`、 `<number>` `<string>` `<object>`、、或`<array>`）都會將專案從結果中排除。

## <a name="next-steps"></a>後續步驟

- [開始使用](sql-query-getting-started.md)
- [IN 關鍵字](sql-query-keywords.md#in)
- [FROM 子句](sql-query-from.md)