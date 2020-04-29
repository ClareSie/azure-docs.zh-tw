---
title: 疑難排解
description: 針對 Azure Synapse 分析進行疑難排解。
services: synapse-analytics
author: kevinvngo
manager: craigg
ms.service: synapse-analytics
ms.topic: conceptual
ms.subservice: ''
ms.date: 02/04/2019
ms.author: kevin
ms.reviewer: jrasnick
ms.custom: azure-synapse
ms.openlocfilehash: 973d2339db1e55f2cca45025f2d678e5126f4317
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80743660"
---
# <a name="troubleshooting-sql-analytics-in-azure-synapse"></a>針對 Azure Synapse 中的 SQL 分析進行疑難排解

本文列出常見的疑難排解問題。

## <a name="connecting"></a>Connecting

| 問題                                                        | 解決方案                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 使用者 'NT AUTHORITY\ANONYMOUS LOGON' 登入失敗。 (Microsoft SQL Server，錯誤：18456) | 當 Azure AD 使用者嘗試連接到 master 資料庫，但在 master 中沒有使用者時，就會發生此錯誤。  若要更正此問題，請在連接時指定您想要連接的 SQL 集區，或將使用者新增至 master 資料庫。  如需詳細資訊，請參閱[安全性總覽](sql-data-warehouse-overview-manage-security.md)一文。 |
| 伺服器主體 "MyUserName" 在目前的資訊安全內容下無法存取「主要」資料庫。 無法開啟使用者預設資料庫。 登入失敗。 使用者 'MyUserName' 登入失敗。 (Microsoft SQL Server，錯誤：916) | 當 Azure AD 使用者嘗試連接到 master 資料庫，但在 master 中沒有使用者時，就會發生此錯誤。  若要更正此問題，請在連接時指定您想要連接的 SQL 集區，或將使用者新增至 master 資料庫。  如需詳細資訊，請參閱[安全性總覽](sql-data-warehouse-overview-manage-security.md)一文。 |
| CTAIP 錯誤                                                  | 在 SQL server master 資料庫上建立登入，而不是在 SQL 資料庫中，就會發生這個錯誤。  如果您遇到這個錯誤，請查看 [安全性概觀](sql-data-warehouse-overview-manage-security.md) 一文。  本文說明如何在 master 上建立登入和使用者，以及如何在 SQL 資料庫中建立使用者。 |
| 遭到防火牆封鎖                                          | SQL 集區受防火牆保護，以確保只有已知的 IP 位址能夠存取資料庫。 防火牆預設將會受到保護，因此您在可以連線之前，必須明確啟用單一 IP 位址或位址範圍。  若要設定防火牆的存取，請遵循[佈建指示](create-data-warehouse-portal.md)中[設定用戶端 IP 的伺服器防火牆存取](create-data-warehouse-portal.md)的步驟。 |
| 無法與工具或驅動程式連線                           | Synapse SQL 集區建議您使用[SSMS](/sql/ssms/download-sql-server-management-studio-ssms?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)、[適用于 Visual Studio 的 SSDT](sql-data-warehouse-install-visual-studio.md)，或使用[sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md)來查詢您的資料。 如需驅動程式和連接到 Azure Synapse 的詳細資訊，請參閱[Azure Synapse 的驅動程式](sql-data-warehouse-connection-strings.md)和[連接到 azure Synapse](sql-data-warehouse-connect-overview.md)文章。 |

## <a name="tools"></a>工具

| 問題                                                        | 解決方案                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| Azure AD 使用者缺少 Visual Studio 的物件瀏覽器           | 這是已知的問題。  解決方法是在 [sys.database_principals](/sql/relational-databases/system-catalog-views/sys-database-principals-transact-sql?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest) 中檢視使用者。  若要深入瞭解如何搭配使用 Azure Active Directory 與 Synapse SQL 集區，請參閱[Azure Synapse 的驗證](sql-data-warehouse-authentication.md)。 |
| 手動撰寫腳本、使用腳本處理嚮導，或透過 SSMS 連接會變慢、沒有回應或產生錯誤 | 請確定已在主要資料庫中建立使用者。 在 [腳本選項] 中，也請確定 engine 版本設定為 "Microsoft Azure SQL 資料倉儲版"，而引擎類型為 "Microsoft Azure SQL Database"。 |
| 無法在 SSMS 中產生指令碼                               | 如果 [產生相依物件的腳本] 選項設定為 "True"，則產生 Synapse SQL 集區的腳本會失敗。 因應措施是，使用者必須手動移至 [**工具]-[> 選項]-[>SQL Server 物件總管-> 產生相依選項的腳本，並將設定為 false** |

## <a name="performance"></a>效能

| 問題                                                        | 解決方案                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 查詢效能疑難排解                            | 如果您正試著針對特定查詢進行疑難排解，請從[了解如何監視查詢](sql-data-warehouse-manage-monitor.md#monitor-query-execution)開始。 |
| TempDB 空間問題 | [監視 TempDB](sql-data-warehouse-manage-monitor.md#monitor-tempdb)空間使用量。  TempDB 空間不足的常見原因如下：<br>-配置給查詢的資源不足，導致資料溢出到 TempDB。  請參閱[工作負載管理](resource-classes-for-workload-management.md) <br>-統計資料遺失或過期，因而造成過多的資料移動。  如需如何建立統計資料的詳細資訊，請參閱[維護資料表統計資料](sql-data-warehouse-tables-statistics.md)<br>-每個服務層級配置了 TempDB 空間。  將[您的 SQL 集區調整](sql-data-warehouse-manage-compute-overview.md#scaling-compute)為較高的 DWU 設定，會配置更多 TempDB 空間。|
| 查詢效能和計劃不佳通常是因為遺漏統計資料 | 效能不佳最常見的原因是缺乏資料表的統計資料。  如需有關如何建立統計資料，以及這對於效能為何重要，請參閱[維護資料表統計資料](sql-data-warehouse-tables-statistics.md)。 |
| 並行存取低落/排入佇列的查詢偏少                             | 為了瞭解如何平衡記憶體配置與並行存取，必須先了解[工作負載管理](resource-classes-for-workload-management.md)。 |
| 如何實作最佳作法                              | [SYNAPSE SQL 集區最佳做法](sql-data-warehouse-best-practices.md)一文是學習改善查詢效能的最佳起點。 |
| 如何透過調整來提升效能                      | 有時候改善效能的解決方案是藉由[調整您的 SQL 集](sql-data-warehouse-manage-compute-overview.md)區，為您的查詢增加更多計算能力。 |
| 索引品質不佳導致查詢效能不佳     | 有時，查詢會因為[資料行存放區索引品質不佳](sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality)而變慢。  請參閱這篇文章，以取得詳細資訊及如何[重建索引以提升區段品質](sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality)。 |

## <a name="system-management"></a>系統管理

| 問題                                                        | 解決方案                                                   |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| 訊息 40847：無法執行這項作業，因為伺服器可能會超過允許的資料庫交易單位配額 45000。 | 減少您正在嘗試建立的資料庫 [DWU](what-is-a-data-warehouse-unit-dwu-cdwu.md)，或是[要求增加配額](sql-data-warehouse-get-started-create-support-ticket.md)。 |
| 調查空間使用量                              | 請參閱[資料表大小](sql-data-warehouse-tables-overview.md#table-size-queries)，以了解您系統的空間使用量。 |
| 協助管理資料表                                    | 請參閱[資料表概觀](sql-data-warehouse-tables-overview.md)一文，以協助管理您的資料表。  本文還包含更詳細主題的連結，例如[資料表的資料類型](sql-data-warehouse-tables-data-types.md)、[散發資料表](sql-data-warehouse-tables-distribute.md)、[編製資料表的索引](sql-data-warehouse-tables-index.md)、[分割資料表](sql-data-warehouse-tables-partition.md)、[維護資料表統計資料](sql-data-warehouse-tables-statistics.md)和[暫存資料表](sql-data-warehouse-tables-temporary.md)。 |
| 透明資料加密（TDE）進度列未在 Azure 入口網站中更新 | 您可以透過 [PowerShell (英文)](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json) 檢視 TDE 的狀態。 |

## <a name="differences-from-sql-database"></a>與 SQL Database 不同之處

| 問題                                 | 解決方案                                                   |
| :------------------------------------ | :----------------------------------------------------------- |
| 不支援的 SQL Database 功能     | 請參閱[不支援的資料表功能](sql-data-warehouse-tables-overview.md#unsupported-table-features)。 |
| 不支援的 SQL Database 資料類型   | 請參閱[不支援的資料類型](sql-data-warehouse-tables-data-types.md#identify-unsupported-data-types)。        |
| DELETE 和 UPDATE 限制         | 請參閱 [UPDATE 因應措施](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-update-statements)、[DELETE 因應措施](sql-data-warehouse-develop-ctas.md#ansi-join-replacement-for-delete-statements)和[使用 CTAS 來解決不支援 UPDATE 和 DELETE 語法的問題](sql-data-warehouse-develop-ctas.md)。 |
| 不支援 MERGE 陳述式      | 請參閱 [MERGE 因應措施](sql-data-warehouse-develop-ctas.md#replace-merge-statements)。                  |
| 預存程序限制          | 請參閱[預存程序限制](sql-data-warehouse-develop-stored-procedures.md#limitations)，以了解預存程序的一些限制。 |
| UDF 不支援 SELECT 陳述式 | 這是 UDF 目前的限制。  關於我們支援的語法，請參閱 [CREATE FUNCTION](/sql/t-sql/statements/create-function-sql-data-warehouse?toc=/azure/synapse-analytics/sql-data-warehouse/toc.json&bc=/azure/synapse-analytics/sql-data-warehouse/breadcrumb/toc.json&view=azure-sqldw-latest)。 |

## <a name="next-steps"></a>後續步驟

如需更多尋找問題解決方案的協助，以下是您可以嘗試的其他資源。

* [網路](https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/)
* [功能要求](https://feedback.azure.com/forums/307516-sql-data-warehouse)
* [影片](https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse)
* [建立支援票證](sql-data-warehouse-get-started-create-support-ticket.md)
* [MSDN 論壇](https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse)
* [Stack Overflow 論壇](https://stackoverflow.com/questions/tagged/azure-sqldw)
* [Twitter](https://twitter.com/hashtag/SQLDW)
