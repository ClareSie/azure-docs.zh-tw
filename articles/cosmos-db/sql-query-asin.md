---
title: Azure 宇宙 DB 查詢語言中的 ASIN
description: 瞭解 Azure Cosmos DB 中的 Arcsine （ASIN） SQL 系統函數如何返回角度（以弧度為單位），其正則是指定的數位運算式
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 8b70738a439b6c64a84a63adf63c83995530e92e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "78302690"
---
# <a name="asin-azure-cosmos-db"></a>ASIN（Azure 宇宙資料庫）
 傳回角度，以弧度為單位，其正弦函數是指定的數值運算式。 這也稱為反正弦值。  
  
## <a name="syntax"></a>語法
  
```sql
ASIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>引數
  
*numeric_expr*  
   為數值運算式。  
  
## <a name="return-types"></a>傳回類型
  
  傳回數值運算式。  
  
## <a name="examples"></a>範例
  
  下面的示例返回`ASIN`-1。  
  
```sql
SELECT ASIN(-1) AS asin  
```  
  
 以下為結果集。  
  
```json
[{"asin": -1.5707963267948966}]  
```  

## <a name="remarks"></a>備註

此系統功能不會利用索引。

## <a name="next-steps"></a>後續步驟

- [數學函數 Azure 宇宙 DB](sql-query-mathematical-functions.md)
- [系統功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 簡介](introduction.md)
