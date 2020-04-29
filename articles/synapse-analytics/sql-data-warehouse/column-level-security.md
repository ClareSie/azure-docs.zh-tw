---
title: 什麼是適用于 Azure Synapse 的資料行層級安全性？
description: 資料行層級安全性可讓客戶根據使用者的執行內容或群組成員資格來控制資料庫資料表資料行的存取權，簡化應用程式中安全性的設計和編碼，並讓您能夠在資料行存取上執行限制。
services: synapse-analytics
author: julieMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 04/19/2020
ms.author: jrasnick
ms.reviewer: igorstan, carlrab
ms.custom: seo-lt-2019
tags: azure-synapse
ms.openlocfilehash: b0a783ad5db86ca783ff1cebceec8d77ab528047
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "81687929"
---
# <a name="column-level-security"></a>資料行層級安全性

資料行層級安全性可讓客戶根據使用者的執行內容或群組成員資格來控制資料表資料行的存取權。

> [!VIDEO https://www.youtube.com/embed/OU_ESg0g8r8]
由於這段影片已張貼到 Azure Synapse 的資料[列層級安全性](/sql/relational-databases/security/row-level-security?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。

資料行層級安全性可簡化應用程式中安全性的設計和編碼，讓您可以限制資料行存取來保護機密資料。 例如，確保特定使用者只能存取其部門相關資料表的特定資料行。 存取限制邏輯是位於資料庫層，而不是離開這些資料，到另一個應用程式層。 資料庫會在每次嘗試從任何層存取資料時套用存取限制。 這項限制藉由減少整體安全性系統的介面區，讓您的安全性變得更可靠且更穩固。 此外，資料行層級安全性也不需要引入視圖來篩選出資料行，以限制使用者的存取權。

您可以使用[GRANT](/sql/t-sql/statements/grant-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) t-sql 語句來執行資料行層級安全性。 透過這個機制，即可支援 SQL 和 Azure Active Directory (AAD) 驗證。

![cls](./media/column-level-security/cls.png)

## <a name="syntax"></a>語法

```syntaxsql
GRANT <permission> [ ,...n ] ON
    [ OBJECT :: ][ schema_name ]. object_name [ ( column [ ,...n ] ) ]
    TO <database_principal> [ ,...n ]
    [ WITH GRANT OPTION ]
    [ AS <database_principal> ]
<permission> ::=
    SELECT
  | UPDATE
<database_principal> ::=
      Database_user
    | Database_role
    | Database_user_mapped_to_Windows_User
    | Database_user_mapped_to_Windows_Group
```

## <a name="example"></a>範例

下列範例顯示如何限制`TestUser`存取`SSN` `Membership`資料表的資料行：

建立`Membership`含有用來儲存社會安全號碼之 SSN 資料行的資料表：

```sql
CREATE TABLE Membership
  (MemberID int IDENTITY,
   FirstName varchar(100) NULL,
   SSN char(9) NOT NULL,
   LastName varchar(100) NOT NULL,
   Phone varchar(12) NULL,
   Email varchar(100) NULL);
```

除了`TestUser` [SSN] 資料行以外，[允許] 可以存取所有的資料行，其中包含敏感性資料：

```sql
GRANT SELECT ON Membership(MemberID, FirstName, LastName, Phone, Email) TO TestUser;
```

以為執行`TestUser`的查詢若包含 SSN 資料行，將會失敗：

```sql
SELECT * FROM Membership;

-- Msg 230, Level 14, State 1, Line 12
-- The SELECT permission was denied on the column 'SSN' of the object 'Membership', database 'CLS_TestDW', schema 'dbo'.
```

## <a name="use-cases"></a>使用案例

目前如何使用資料行層級安全性的一些範例：

- 某家金融服務公司只允許帳戶管理員能夠存取客戶社會安全號碼 (SSN)、電話號碼和其他個人識別資訊 (PII)。
- 醫療保健提供者只允許醫生和護士存取機密醫療記錄，同時防止帳單部門的成員看到此資料。
