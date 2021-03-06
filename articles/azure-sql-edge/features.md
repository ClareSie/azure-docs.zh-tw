---
title: Azure SQL Edge 支援的功能
description: 深入瞭解 Azure SQL Edge 所支援的功能詳細資料。
keywords: SQL Edge 簡介, 什麼是 SQL Edge, SQL Edge 概觀
services: sql-edge
ms.service: sql-edge
ms.topic: conceptual
author: SQLSourabh
ms.author: sourabha
ms.reviewer: sstein
ms.date: 09/03/2020
ms.openlocfilehash: 19dcbbf102a1d8d21f1b14780ea33816a1677c55
ms.sourcegitcommit: 0ce1ccdb34ad60321a647c691b0cff3b9d7a39c8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2020
ms.locfileid: "93392022"
---
# <a name="supported-features-of-azure-sql-edge"></a>Azure SQL Edge 支援的功能 

Azure SQL Edge 是以最新版本的 SQL Database 引擎為基礎。 除了 Linux (上的 SQL Server 2019 或 Windows) 的 SQL Server 中目前不支援或無法使用的某些功能之外，它還支援 Linux 上 SQL Server 2019 支援的功能子集。

如需 Linux 上 SQL Server 支援的功能完整清單，請參閱 [Linux 上的 SQL Server 2019 版本和支援功能](/sql/linux/sql-server-linux-editions-and-components-2019)。 針對 Windows 上 SQL Server 的版本和支援功能，請參閱 [SQL Server 2019 (6.x) 的版本和支援的功能 ](/sql/sql-server/editions-and-components-of-sql-server-version-15)。

## <a name="azure-sql-edge-editions"></a>Azure SQL Edge 版本

Azure SQL Edge 提供兩種不同的版本或軟體方案。 這些版本具有相同的功能集，而且其許可權和可以在主機系統上存取的記憶體和核心數量並無差異。

   |**規劃**  |**說明**  |
   |---------|---------|
   |Azure SQL Edge 開發人員  |  僅供開發之用。 每個 Azure SQL Edge 開發人員容器的最大限制為4個核心和 32 GB 的記憶體。  |
   |Azure SQL Edge    |  適用于生產環境。 每個 Azure SQL Edge 容器限制為最多8個核心和 64 GB 的記憶體。  |

## <a name="operating-system"></a>作業系統

Azure SQL Edge 容器以 Ubuntu 18.04 為基礎，因此僅支援在執行 Ubuntu 18.04 LTS 的 Docker 主機上執行， (建議的) 或 Ubuntu 20.04 LTS。 您可以在其他作業系統主機上執行 Azure SQL Edge 容器，例如，它可以在 Linux 的其他發行版本或使用 Docker CE 或 Docker EE 的 Windows (上執行) ，但 Microsoft 不建議您這麼做，因為這項設定可能未經過廣泛測試。

在 Windows 上執行 Azure SQL Edge 的建議設定是在 Windows 主機上設定 Ubuntu VM，然後在 Linux VM 內執行 Azure SQL Edge。

適用于 Azure SQL Edge 的建議和支援檔案系統為 EXT4 和 XFS。 如果使用持續性磁片區來復原 Azure SQL Edge 資料庫儲存體，則基礎主機檔案系統必須 EXT4 和 XFS。

## <a name="hardware-support"></a>硬體支援

Azure SQL Edge 需要64位處理器 (x64 或 ARM64) ，而且主機上至少有一個處理器和 1 GB 的 RAM。 雖然 Azure SQL Edge 的啟動記憶體使用量接近450MB，但在 Edge 裝置上執行的其他 IoT Edge 模組或進程需要額外的記憶體。 Azure SQL Edge 的實際記憶體和 CPU 需求會根據工作負載的複雜度和所處理的資料量而有所不同。 選擇解決方案的硬體時，Microsoft 建議您執行廣泛的效能測試，以確保符合解決方案所需的效能特性。  

## <a name="azure-sql-edge-components"></a>Azure SQL Edge 元件

Azure SQL Edge 僅支援 database engine。 它不包含 Windows 上的 SQL Server 2019 或 Linux 上的 SQL Server 2019 提供的其他元件支援。 具體而言，Azure SQL Edge 不支援 Analysis Services、Reporting Services、Integration Services、Master Data Services、Machine Learning 服務 (資料庫內) 和 Machine Learning Server (獨立) 等 SQL Server 元件。

## <a name="supported-features"></a>支援的功能

除了支援 Linux 上的 SQL Server 的功能子集之外，Azure SQL Edge 還包含下列新功能的支援： 

- SQL 串流（以支援 Azure 串流分析的相同引擎為基礎）可在 Azure SQL Edge 中提供即時資料串流功能。 
- Time-Series 資料分析的 T-sql 函式呼叫 `Date_Bucket` 。
- 透過 ONNX 執行時間（隨附于 SQL 引擎）的機器學習功能。

## <a name="unsupported-features"></a>不支援的功能

下列清單包含 Azure SQL Edge 目前不支援的 Linux 功能 SQL Server 2019。

| 區域 | 不支援的功能或服務 |
|-----|-----|
| **資料庫設計** | 記憶體內部 OLTP，以及相關的 DDL 命令和 Transact-sql 函式、類別目錄檢視和動態管理檢視。 |
| &nbsp; | `HierarchyID` 資料類型，以及相關的 DDL 命令和 Transact-sql 函式、類別目錄檢視和動態管理檢視。 |
| &nbsp; | `Spatial` 資料類型，以及相關的 DDL 命令和 Transact-sql 函式、類別目錄檢視和動態管理檢視。 |
| &nbsp; | Stretch DB、相關的 DDL 命令和 Transact-sql 函式、類別目錄檢視和動態管理檢視。 |
| &nbsp; | 全文檢索索引和搜尋，以及相關的 DDL 命令和 Transact-sql 函式、類別目錄檢視和動態管理檢視。|
| &nbsp; | `FileTable`、 `FILESTREAM` 和相關的 DDL 命令和 transact-sql 函式、類別目錄檢視和動態管理檢視。|
| **資料庫引擎** | 複寫。 請注意，您可以將 Azure SQL Edge 設定為複寫拓撲的發送訂閱者。 |
| &nbsp; | Polybase。 請注意，您可以將 Azure SQL Edge 設定為 Polybase 中外部資料表的目標。 |
| &nbsp; | 透過 JAVA 和 Spark 的語言擴充性。 |
| &nbsp; | Active Directory 整合。 |
| &nbsp; | 資料庫自動壓縮。 您可以使用命令來設定資料庫的自動壓縮屬性 `ALTER DATABASE <database_name> SET AUTO_SHRINK ON` ，不過該變更沒有任何作用。 自動壓縮工作將不會針對資料庫執行。 使用者仍然可以使用 ' DBCC ' 命令壓縮資料庫檔案。 |
| &nbsp; | 資料庫快照集。 |
| &nbsp; | 持續性記憶體的支援。 |
| &nbsp; | Microsoft Distributed Transaction Coordinator。 |
| &nbsp; | Resource governor 和 IO 資源管理。 |
| &nbsp; | 緩衝集區延伸。 |
| &nbsp; | 具有協力廠商連接的分散式查詢。 |
| &nbsp; | 連結的伺服器。 |
| &nbsp; | 系統擴充預存程式 (例如 `XP_CMDSHELL`) 。 |
| &nbsp; | CLR 元件，以及相關的 DDL 命令和 Transact-sql 函式、類別目錄檢視和動態管理檢視。 |
| &nbsp; | CLR 相依的 t-sql 函數，例如 `ASSEMBLYPROPERTY` 、、 `FORMAT` `PARSE` 和 `TRY_PARSE` 。 |
| &nbsp; | CLR 相依的日期和時間目錄檢視、函數和查詢子句。 |
| &nbsp; | 緩衝集區延伸。 |
| &nbsp; | Database mail。 |
| &nbsp; | Service Broker |
| &nbsp; | 原則式管理 |
| &nbsp; | 管理資料倉儲 |
| &nbsp; | 自主資料庫 |
| **SQL Server Agent** |  子系統： CmdExec、PowerShell、佇列讀取器、SSIS、SSAS 和 SSRS。 |
| &nbsp; | 警報。 |
| &nbsp; | 受管理的備份。 |
| **高可用性** | Always On 可用性群組。  |
| &nbsp; | 基本可用性群組。 |
| &nbsp; | Always On 容錯移轉叢集實例。 |
| &nbsp; | 資料庫鏡像。 |
| &nbsp; | 熱新增記憶體和 CPU。 |
| **安全性** | 可延伸金鑰管理。 |
| &nbsp; | Active Directory 整合。|
| &nbsp; | 支援安全記憶體保護區。|
| **服務** | SQL Server Browser。 |
| &nbsp; | 透過 R 和 Python Machine Learning。 |
| &nbsp; | StreamInsight. |
| &nbsp; | Analysis Services 中的中繼資料和查詢資料。 |
| &nbsp; | Reporting Services。 |
| &nbsp; | Data Quality Services。 |
| &nbsp; | Master Data Services。 |
| &nbsp; | Distributed Replay。 |
| **管理能力** | SQL Server 公用程式控制點。 |

## <a name="next-steps"></a>後續步驟

- [部署 Azure SQL Edge](deploy-portal.md)
- [設定 Azure SQL Edge](configure.md)
- [使用 SQL Server 用戶端工具連線到 Azure SQL Edge 執行個體](connect.md)
- [Azure SQL Edge 中的備份和還原資料庫](backup-restore.md)