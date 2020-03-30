---
title: 泛型服務層
description: 瞭解 Azure SQL 資料庫通用層
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 02/07/2019
ms.openlocfilehash: 7c57755ae63f8af5a2a4faa4764bc6a9597e8c2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255882"
---
# <a name="general-purpose-service-tier---azure-sql-database"></a>一般用途服務層級 - Azure SQL Database

> [!NOTE]
> 基於 vCore 的採購模型中的泛型服務層稱為基於 DTU 的採購模型中的標準服務層。 如需以虛擬核心為基礎的購買模型與以 DTU 為基礎的購買模型的比較，請參閱 [Azure SQL Database 購買模型和資源](sql-database-purchase-models.md)。

Azure SQL Database 是以會針對雲端環境調整的 SQL Server 資料庫引擎架構為基礎，以確保 99.99% 的可用性 (即使在基礎結構失敗的情況下)。 Azure SQL Database 中使用三個服務層級，每個服務層級具有不同的架構模型。 這些服務層級為：

- 一般用途
- 業務關鍵
- 超大規模資料庫

泛型服務層的體系結構模型基於計算和存儲的分離。 此架構模型仰賴 Azure Blob 儲存體的高可用性和可靠性，可以透明的方式複寫資料庫檔案，並保證當基礎結構發生失敗時也不會遺失資料。

下圖顯示標準架構模型中具有分隔計算和儲存體層的四個節點。

![分隔計算與儲存體](media/sql-database-managed-instance/general-purpose-service-tier.png)

在泛型服務層的體系結構模型中，有兩個層：

- 無狀態計算層，執行 `sqlservr.exe` 流程並且僅包含暫時性和快取資料 (例如計畫快取、緩衝集區、列儲存集區)。 此無狀態 SQL Server 節點是由 Azure Service Fabric 操作，可初始化流程、控制節點的健康情況，並在必要時執行故障轉移至其他位置。
- 具狀態資料層，包含儲存在 Azure Blob 儲存體中的資料庫檔案 (.mdf/.ldf)。 Azure Blob 可確保任何資料庫檔案中放置的任何記錄都不會遺失資料。 Azure 儲存體具有內建的資料可用性/備援，即使 SQL Server 流程損毀，也可以確保保留資料檔案中記錄檔或頁面中的每項記錄。

每當升級資料庫引擎或作業系統，部份的基礎結構失敗，或者在 SQL Server 流程中偵測到某些關鍵問題時，Azure Service Fabric 都會將無狀態 SQL Server 流程移至另一個無狀態計算節點。 主要節點發生容錯移轉時，會有一組備用節點等候執行新的計算服務，以便將容錯移轉的時間縮到最短。 Azure 儲存體層中的資料不受影響，而資料/記錄檔會附加到新初始化的 SQL Server 流程。 此流程可保證 99.99％ 的可用性，但由於轉換時間和新 SQL Server 節點以冷快取啟動，可能會對正在執行的繁重工作負載產生一些效能影響。

## <a name="when-to-choose-this-service-tier"></a>選擇此服務層級的時機

一般目的服務層級是 Azure SQL Database 中的預設服務層級，專供大部分的一般工作負載使用。 如果需要具有 99.99% SLA 的完全託管資料庫引擎，在大多數情況下，存儲延遲在 5 到 10 毫秒之間與 Azure SQL IaaS 匹配，則通用層是您的最佳選擇。

## <a name="next-steps"></a>後續步驟

- 在[託管實例](sql-database-managed-instance-resource-limits.md#service-tier-characteristics)中查找通用/標準層的資源特徵（核心數、IO、記憶體數），[在 vCore 模型](sql-database-vcore-resource-limits-single-databases.md#general-purpose---provisioned-compute---gen4)或[DTU 模型中](sql-database-dtu-resource-limits-single-databases.md#single-database-storage-sizes-and-compute-sizes)查找單個資料庫，或在[vCore 模型](sql-database-vcore-resource-limits-elastic-pools.md#general-purpose---provisioned-compute---gen4)和[DTU 模型中](sql-database-dtu-resource-limits-elastic-pools.md#standard-elastic-pool-limits)查找彈性池。
- 了解[業務關鍵](sql-database-service-tier-business-critical.md)和[超大規模資料庫](sql-database-service-tier-hyperscale.md)層。
- 瞭解[服務結構](../service-fabric/service-fabric-overview.md)。
- 有關高可用性和災害復原的更多選項，請參閱[業務連續性](sql-database-business-continuity.md)。
