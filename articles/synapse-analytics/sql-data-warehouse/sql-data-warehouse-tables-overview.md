---
title: 設計表
description: 在 Azure SQL 資料倉儲中設計資料表的簡介。
services: synapse-analytics
author: XiaoyuMSFT
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 03/15/2019
ms.author: xiaoyul
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: f116897bdaffa765404aa47fda4ae32a49fa99ac
ms.sourcegitcommit: 8a9c54c82ab8f922be54fb2fcfd880815f25de77
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80351275"
---
# <a name="designing-tables-in-azure-sql-data-warehouse"></a>在 Azure SQL 資料倉儲中設計資料表

瞭解在 Azure SQL 資料倉儲中設計資料表的重要概念。 

## <a name="determine-table-category"></a>決定資料表類別 

[星狀結構描述](https://en.wikipedia.org/wiki/Star_schema)會將資料分類為事實和維度資料表。 某些資料表用於資料移至事實或維度資料表之前的整合或暫存。 設計資料表時，請決定資料表的資料將屬於事實、維度還是整合資料表。 此決定將使資料表具有適當的結構和散發機制。 

- **事實資料表**包含通常在事務系統中生成的定量資料，然後載入到資料倉儲中。 例如，零售商每天都會產生銷售交易，然後將資料載入資料倉儲事實資料表中，以進行分析。

- **維度資料表**包含可能會變更、但變更頻率通常不高的屬性資料。 例如，客戶的名稱和地址會儲存在維度資料表中，但只有在客戶的設定檔有所變更時，才會更新。 若要降低大型事實資料表的大小，則不需將客戶的名稱和地址放在事實資料表的每個資料列中。 此時，事實資料表與維度資料表可以共用客戶識別碼。 查詢可以聯結兩個資料表，使客戶的設定檔與交易產生關聯。 

- **整合資料表**可用來整合或暫存資料。 您可以建立整合資料表作為一般資料表、外部資料表或暫存資料表。 例如，您可以將資料載入至暫存資料表、對暫存的資料執行轉換，然後將該資料插入生產資料表中。

## <a name="schema-and-table-names"></a>結構描述和資料表名稱
架構是一種將表分組的好方法，以類似方式一起使用。  如果要將多個資料庫從前置解決方案遷移到 SQL 資料倉儲，最好將所有事實、維度和集成表遷移到 SQL 資料倉儲中的一個架構。 例如，您可以將[WideWorld 導入者DW](/sql/sample/world-wide-importers/database-catalog-wwi-olap)示例資料倉儲中的所有表存儲在一個稱為 wwi 的架構中。 以下代碼創建一個稱為 wwi[的使用者定義的架構](/sql/t-sql/statements/create-schema-transact-sql)。

```sql
CREATE SCHEMA wwi;
```

若要在 SQL 資料倉儲中顯示資料表的組織，可以使用 fact、dim、int 作為資料表名稱的前置詞。 下表顯示 WideWorldImportersDW 的一些結構描述和資料表名稱。  

| WideWorldImportersDW 資料表  | 資料表類型 | SQL 資料倉儲 |
|:-----|:-----|:------|:-----|
| City | 維度 | wwi.DimCity |
| 單 | 事實 | wwi.FactOrder |


## <a name="table-persistence"></a>資料表持續性 

資料表可以在 Azure 儲存體中永久儲存資料、在 Azure 儲存體暫時儲存資料，或將資料儲存在資料倉儲外部的資料存放區中。

### <a name="regular-table"></a>一般資料表

一般資料表會將資料儲存在 Azure 儲存體中，作為資料倉儲的一部分。 無論工作階段是否開啟，資料表和資料都會持續保存。  此範例會建立具有兩個資料行的一般資料表。 

```sql
CREATE TABLE MyTable (col1 int, col2 int );  
```

### <a name="temporary-table"></a>暫存資料表
暫存資料表只存在於工作階段執行期間。 您可以使用臨時表來防止其他使用者看到臨時結果，並減少清理的需要。  臨時表利用本機存放區提供快速性能。  如需詳細資訊，請參閱[暫存資料表](sql-data-warehouse-tables-temporary.md)。

### <a name="external-table"></a>外部資料表
外部資料表會指向位於 Azure 儲存體 blob 或 Azure Data Lake Store 中的資料。 與 CREATE TABLE AS SELECT 陳述式搭配使用時，如果從外部資料表選取，資料將會匯入 SQL 資料倉儲中。 因此，外部資料表對於載入資料有所助益。 如需載入的教學課程，請參閱[使用 PolyBase 從 Azure Blob 儲存體載入資料](load-data-from-azure-blob-storage-using-polybase.md)。

## <a name="data-types"></a>資料類型
SQL 資料倉儲支援最常用的資料類型。 如需支援的資料類型清單，請參閱 CREATE TABLE 參考中 CREATE TABLE 陳述式中的[資料類型](/sql/t-sql/statements/create-table-azure-sql-data-warehouse#DataTypes)。 如需資料類型的使用指引，請參閱[資料類型](sql-data-warehouse-tables-data-types.md)。

## <a name="distributed-tables"></a>分散式資料表
SQL 資料倉儲的一個基本功能是它可以跨[分佈](massively-parallel-processing-mpp-architecture.md#distributions)在表上存儲和操作。  SQL 資料倉儲支援三種用於分發資料的方法，即迴圈（預設）、雜湊和複製。

### <a name="hash-distributed-tables"></a>雜湊分散式資料表
雜湊分散式表根據分佈列中的值分佈行。 雜湊分散式表旨在實現對大型表上的查詢的高性能。 選擇分佈列時，需要考慮幾個因素。 

如需詳細資訊，請參閱[分散式資料表的設計指引](sql-data-warehouse-tables-distribute.md)。

### <a name="replicated-tables"></a>複寫資料表
複寫資料表有一個可在每個計算節點上使用的完整資料表複本。 複寫資料表的聯結不需要移動資料，因此對複寫資料表的查詢得以快速執行。 不過，複寫需要額外的儲存體，且對於大型資料表並不實用。 

如需詳細資訊，請參閱[複寫資料表的設計指引](design-guidance-for-replicated-tables.md)。

### <a name="round-robin-tables"></a>循環配置資源資料表
循環配置資源資料表會將資料表的資料列平均散發於所有散發。 資料列會隨機分散。 資料可以快速載入循環配置資源資料表。  但是，查詢可能需要比其他散發方法更多的資料移動。 

如需詳細資訊，請參閱[分散式資料表的設計指引](sql-data-warehouse-tables-distribute.md)。

### <a name="common-distribution-methods-for-tables"></a>資料表常用的散發方法
資料表類別通常會決定應選擇哪個選項來散發資料表。 

| 資料表類別 | 建議的散發選項 |
|:---------------|:--------------------|
| 事實           | 使用具有叢集資料行存放區索引的雜湊散發。 在相同的散發資料行上聯結兩個雜湊資料表時，可以改善效能。 |
| 維度      | 對較小的資料表使用複寫。 如果資料表太大而無法儲存在每個計算節點上，請使用雜湊散發。 |
| 預備        | 對暫存資料表使用循環配置資源。 使用 CTAS 的載入速度較快。 資料在暫存表中後，請使用 INSERT...選擇以將資料移動到生產表。 |

## <a name="table-partitions"></a>資料表的資料分割
分割的資料表會並根據資料範圍儲存在資料表資料列上，並執行作業。 例如，資料表可能會依日、月或年進行分割。 您可以透過「資料分割消除」將查詢掃描限定於某個資料分割內的資料，進而提升查詢效能。 您也可以透過資料分割切換來維護資料。 由於 SQL 資料倉儲中的資料已散發，資料分割過多可能會降低查詢效能。 如需詳細資訊，請參閱[資料分割指引](sql-data-warehouse-tables-partition.md)。  當分區切換到不為空的表分區時，請考慮在[ALTER TABLE](https://docs.microsoft.com/sql/t-sql/statements/alter-table-transact-sql)語句中使用TRUNCATE_TARGET選項，如果現有資料要截斷。 以下代碼將轉換後的資料轉換為 SalesFact 覆蓋任何現有資料。 

```sql
ALTER TABLE SalesFact_DailyFinalLoad SWITCH PARTITION 256 TO SalesFact PARTITION 256 WITH (TRUNCATE_TARGET = ON);  
```

## <a name="columnstore-indexes"></a>資料行存放區索引
根據預設，SQL 資料倉儲會將資料表儲存為叢集資料行存放區索引。 這種形式的資料儲存對於大型資料表可達到高度的資料壓縮和查詢效能。  叢集資料行存放區索引通常是最佳選擇，但在某些情況下，叢集索引或堆積會是更適當的儲存結構。  堆表對於載入瞬態資料（如轉換為最終表的過渡表）特別有用。

如需資料行存放區功能的清單，請參閱[資料行存放區索引的新功能](/sql/relational-databases/indexes/columnstore-indexes-what-s-new)。 若要改善資料行存放區索引效能，請參閱[盡可能提高資料行存放區索引的資料列群組品質](sql-data-warehouse-memory-optimizations-for-columnstore-compression.md)。

## <a name="statistics"></a>統計資料
查詢最佳化工具在建立執行查詢的計劃時，會使用資料行層級的統計資料。 為了提高查詢性能，對單個列（尤其是查詢聯接中使用的列）進行統計資訊非常重要。 [創建統計資訊](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics#automatic-creation-of-statistic)會自動進行。  但是，更新統計資料不會自動進行。 在新增或變更大量的資料列之後，請更新統計資料。 例如，請在載入之後更新統計資料。 如需詳細資訊，請參閱[統計資料指引](sql-data-warehouse-tables-statistics.md)。

## <a name="primary-key-and-unique-key"></a>主鍵和唯一鍵
僅當同時使用非強制和未執行時，才支援主金鑰。  僅使用"不強制"時支援唯一約束。  檢查[SQL 資料倉儲表約束](sql-data-warehouse-table-constraints.md)。

## <a name="commands-for-creating-tables"></a>建立資料表的命令
您可以將資料表建立為新的空資料表。 您也可以在建立資料表後填入 Select 陳述式的結果。 以下是用來建立資料表的 T-SQL 命令。

| T-SQL 陳述式 | 描述 |
|:----------------|:------------|
| [創建表](/sql/t-sql/statements/create-table-azure-sql-data-warehouse) | 藉由定義所有的資料表資料行和選項，建立空的資料表。 |
| [創建外部表](/sql/t-sql/statements/create-external-table-transact-sql) | 建立外部資料表。 資料表的定義會儲存在 SQL 資料倉儲中。 資料表的資料會儲存在 Azure 儲存體或 Azure Data Lake Store 中。 |
| [CREATE TABLE AS SELECT](/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) | 在新的資料表中填入 Select 陳述式的結果。 資料表資料行和資料類型皆以 Select 陳述式的結果為基礎。 若要匯入資料，此陳述式可從外部資料表進行選取。 |
| [CREATE EXTERNAL TABLE AS SELECT](/sql/t-sql/statements/create-external-table-as-select-transact-sql) | 藉由將 Select 陳述式的結果匯出至外部位置，建立新的外部資料表。  其位置為 Azure Blob 儲存體或 Azure Data Lake Store。 |

## <a name="aligning-source-data-with-the-data-warehouse"></a>找出資料倉儲對應的資料來源

資料倉儲資料表會在您從其他資料來源載入資料時填入。 若要執行成功的載入，來源資料中的資料行在數目和資料類型方面，都必須與資料倉儲中的資料表定義相符。 取得相符的資料，可能是設計資料表時最困難的環節。 

如果資料來自多個資料存放區，您可以將資料放入資料倉儲中，並將其儲存於整合資料表。 資料放入整合資料表後，您可以使用 SQL 資料倉儲的功能執行轉換作業。 資料備妥後，您可以將它插入生產資料表。

## <a name="unsupported-table-features"></a>不支援的資料表功能
SQL 資料倉儲支援其他資料庫所提供的多項 (但並非所有) 資料表功能。  下列清單列出 SQL 資料倉儲不支援的部分資料表功能。

- 外鍵，檢查[表約束](/sql/t-sql/statements/alter-table-table-constraint-transact-sql)
- [計算列](/sql/t-sql/statements/alter-table-computed-column-definition-transact-sql)
- [索引檢視表](/sql/relational-databases/views/create-indexed-views)
- [序列](/sql/t-sql/statements/create-sequence-transact-sql)
- [稀疏列](/sql/relational-databases/tables/use-sparse-columns)
- Surrogate 索引鍵。 使用[身分識別](sql-data-warehouse-tables-identity.md)進行實作。
- [同義字](/sql/t-sql/statements/create-synonym-transact-sql)
- [觸發器](/sql/t-sql/statements/create-trigger-transact-sql)
- [唯一索引](/sql/t-sql/statements/create-index-transact-sql)
- [使用者定義類型](/sql/relational-databases/native-client/features/using-user-defined-types)

## <a name="table-size-queries"></a>資料表大小查詢
識別資料表在每個散發 (共 60 個) 中所耗用的空間和資料列數的其中一種簡單方式，就是使用 [DBCC PDW_SHOWSPACEUSED](/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql)。

```sql
DBCC PDW_SHOWSPACEUSED('dbo.FactInternetSales');
```

不過，使用 DBCC 命令相當受限。  動態管理檢視 (DMV) 會比 DBCC 命令顯示更多詳細資料。 首先請建立此檢視。

```sql
CREATE VIEW dbo.vTableSizes
AS
WITH base
AS
(
SELECT 
 GETDATE()                                                             AS  [execution_time]
, DB_NAME()                                                            AS  [database_name]
, s.name                                                               AS  [schema_name]
, t.name                                                               AS  [table_name]
, QUOTENAME(s.name)+'.'+QUOTENAME(t.name)                              AS  [two_part_name]
, nt.[name]                                                            AS  [node_table_name]
, ROW_NUMBER() OVER(PARTITION BY nt.[name] ORDER BY (SELECT NULL))     AS  [node_table_name_seq]
, tp.[distribution_policy_desc]                                        AS  [distribution_policy_name]
, c.[name]                                                             AS  [distribution_column]
, nt.[distribution_id]                                                 AS  [distribution_id]
, i.[type]                                                             AS  [index_type]
, i.[type_desc]                                                        AS  [index_type_desc]
, nt.[pdw_node_id]                                                     AS  [pdw_node_id]
, pn.[type]                                                            AS  [pdw_node_type]
, pn.[name]                                                            AS  [pdw_node_name]
, di.name                                                              AS  [dist_name]
, di.position                                                          AS  [dist_position]
, nps.[partition_number]                                               AS  [partition_nmbr]
, nps.[reserved_page_count]                                            AS  [reserved_space_page_count]
, nps.[reserved_page_count] - nps.[used_page_count]                    AS  [unused_space_page_count]
, nps.[in_row_data_page_count] 
    + nps.[row_overflow_used_page_count] 
    + nps.[lob_used_page_count]                                        AS  [data_space_page_count]
, nps.[reserved_page_count] 
 - (nps.[reserved_page_count] - nps.[used_page_count]) 
 - ([in_row_data_page_count] 
         + [row_overflow_used_page_count]+[lob_used_page_count])       AS  [index_space_page_count]
, nps.[row_count]                                                      AS  [row_count]
from 
    sys.schemas s
INNER JOIN sys.tables t
    ON s.[schema_id] = t.[schema_id]
INNER JOIN sys.indexes i
    ON  t.[object_id] = i.[object_id]
    AND i.[index_id] <= 1
INNER JOIN sys.pdw_table_distribution_properties tp
    ON t.[object_id] = tp.[object_id]
INNER JOIN sys.pdw_table_mappings tm
    ON t.[object_id] = tm.[object_id]
INNER JOIN sys.pdw_nodes_tables nt
    ON tm.[physical_name] = nt.[name]
INNER JOIN sys.dm_pdw_nodes pn
    ON  nt.[pdw_node_id] = pn.[pdw_node_id]
INNER JOIN sys.pdw_distributions di
    ON  nt.[distribution_id] = di.[distribution_id]
INNER JOIN sys.dm_pdw_nodes_db_partition_stats nps
    ON nt.[object_id] = nps.[object_id]
    AND nt.[pdw_node_id] = nps.[pdw_node_id]
    AND nt.[distribution_id] = nps.[distribution_id]
LEFT OUTER JOIN (select * from sys.pdw_column_distribution_properties where distribution_ordinal = 1) cdp
    ON t.[object_id] = cdp.[object_id]
LEFT OUTER JOIN sys.columns c
    ON cdp.[object_id] = c.[object_id]
    AND cdp.[column_id] = c.[column_id]
WHERE pn.[type] = 'COMPUTE'
)
, size
AS
(
SELECT
   [execution_time]
,  [database_name]
,  [schema_name]
,  [table_name]
,  [two_part_name]
,  [node_table_name]
,  [node_table_name_seq]
,  [distribution_policy_name]
,  [distribution_column]
,  [distribution_id]
,  [index_type]
,  [index_type_desc]
,  [pdw_node_id]
,  [pdw_node_type]
,  [pdw_node_name]
,  [dist_name]
,  [dist_position]
,  [partition_nmbr]
,  [reserved_space_page_count]
,  [unused_space_page_count]
,  [data_space_page_count]
,  [index_space_page_count]
,  [row_count]
,  ([reserved_space_page_count] * 8.0)                                 AS [reserved_space_KB]
,  ([reserved_space_page_count] * 8.0)/1000                            AS [reserved_space_MB]
,  ([reserved_space_page_count] * 8.0)/1000000                         AS [reserved_space_GB]
,  ([reserved_space_page_count] * 8.0)/1000000000                      AS [reserved_space_TB]
,  ([unused_space_page_count]   * 8.0)                                 AS [unused_space_KB]
,  ([unused_space_page_count]   * 8.0)/1000                            AS [unused_space_MB]
,  ([unused_space_page_count]   * 8.0)/1000000                         AS [unused_space_GB]
,  ([unused_space_page_count]   * 8.0)/1000000000                      AS [unused_space_TB]
,  ([data_space_page_count]     * 8.0)                                 AS [data_space_KB]
,  ([data_space_page_count]     * 8.0)/1000                            AS [data_space_MB]
,  ([data_space_page_count]     * 8.0)/1000000                         AS [data_space_GB]
,  ([data_space_page_count]     * 8.0)/1000000000                      AS [data_space_TB]
,  ([index_space_page_count]  * 8.0)                                   AS [index_space_KB]
,  ([index_space_page_count]  * 8.0)/1000                              AS [index_space_MB]
,  ([index_space_page_count]  * 8.0)/1000000                           AS [index_space_GB]
,  ([index_space_page_count]  * 8.0)/1000000000                        AS [index_space_TB]
FROM base
)
SELECT * 
FROM size
;
```

### <a name="table-space-summary"></a>資料表空間摘要

此查詢會傳回資料表的資料列和空格。  它可讓您查看哪些資料表是最大的資料表，及其屬於循環配置資源、複寫還是雜湊分散式資料表。  若為雜湊分散式資料表，則查詢也會顯示散發資料行。  

```sql
SELECT 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
,    COUNT(distinct partition_nmbr) as nbr_partitions
,    SUM(row_count)                 as table_row_count
,    SUM(reserved_space_GB)         as table_reserved_space_GB
,    SUM(data_space_GB)             as table_data_space_GB
,    SUM(index_space_GB)            as table_index_space_GB
,    SUM(unused_space_GB)           as table_unused_space_GB
FROM 
    dbo.vTableSizes
GROUP BY 
     database_name
,    schema_name
,    table_name
,    distribution_policy_name
,      distribution_column
,    index_type_desc
ORDER BY
    table_reserved_space_GB desc
;
```

### <a name="table-space-by-distribution-type"></a>依散發類型的資料表空間

```sql
SELECT 
     distribution_policy_name
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY distribution_policy_name
;
```

### <a name="table-space-by-index-type"></a>依索引類型的資料表空間

```sql
SELECT 
     index_type_desc
,    SUM(row_count)                as table_type_row_count
,    SUM(reserved_space_GB)        as table_type_reserved_space_GB
,    SUM(data_space_GB)            as table_type_data_space_GB
,    SUM(index_space_GB)           as table_type_index_space_GB
,    SUM(unused_space_GB)          as table_type_unused_space_GB
FROM dbo.vTableSizes
GROUP BY index_type_desc
;
```

### <a name="distribution-space-summary"></a>散發空間摘要

```sql
SELECT 
    distribution_id
,    SUM(row_count)                as total_node_distribution_row_count
,    SUM(reserved_space_MB)        as total_node_distribution_reserved_space_MB
,    SUM(data_space_MB)            as total_node_distribution_data_space_MB
,    SUM(index_space_MB)           as total_node_distribution_index_space_MB
,    SUM(unused_space_MB)          as total_node_distribution_unused_space_MB
FROM dbo.vTableSizes
GROUP BY     distribution_id
ORDER BY    distribution_id
;
```

## <a name="next-steps"></a>後續步驟
為您的資料倉儲建立資料表之後，下一個步驟是將資料載入資料表中。  如需載入教學課程，請參閱[載入資料到 SQL 資料倉儲](load-data-wideworldimportersdw.md)。
