---
title: Azure 宇宙 DB 查詢語言中的 SIN
description: 在 Azure 宇宙 DB 中瞭解 SQL 系統函數 SIN。
author: ginamr
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: girobins
ms.custom: query-reference
ms.openlocfilehash: 25e7cf66fdd55a0b641c35443e38b0a67cbe365d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "78303098"
---
# <a name="sin-azure-cosmos-db"></a>SIN（Azure 宇宙資料庫）
 在指定運算式中傳回指定角度的三角正弦函數 (以弧度為單位)。  
  
## <a name="syntax"></a>語法
  
```sql
SIN(<numeric_expr>)  
```  
  
## <a name="arguments"></a>引數
  
*numeric_expr*  
   為數值運算式。  
  
## <a name="return-types"></a>傳回類型
  
  傳回數值運算式。  
  
## <a name="examples"></a>範例
  
  下面的示例計算`SIN`指定角度。  
  
```sql
SELECT SIN(45.175643) AS sin  
```  
  
 以下為結果集。  
  
```json
[{"sin": 0.929607286611012}]  
```  

## <a name="remarks"></a>備註

此系統功能不會利用索引。

## <a name="next-steps"></a>後續步驟

- [數學函數 Azure 宇宙 DB](sql-query-mathematical-functions.md)
- [系統功能 Azure 宇宙 DB](sql-query-system-functions.md)
- [Azure 宇宙 DB 簡介](introduction.md)
