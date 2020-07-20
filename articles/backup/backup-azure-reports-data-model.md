---
title: Azure 備份診斷事件的資料模型
description: 此資料模型會參考將診斷事件傳送至 Log Analytics （LA）的資源特定模式。
ms.topic: conceptual
ms.date: 10/30/2019
ms.openlocfilehash: af1e4159ff2794f8d4dd11480eb7f1789e034c06
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "84484508"
---
# <a name="data-model-for-azure-backup-diagnostics-events"></a>Azure 備份診斷事件的資料模型

## <a name="coreazurebackup"></a>CoreAzureBackup

下表提供核心備份實體的相關資訊，例如保存庫和備份專案。

| **欄位**                         | **資料類型** | **描述**                                              |
| --------------------------------- | ------------- | ------------------------------------------------------------ |
| ResourceId                        | Text          | 正在收集資料的資源識別碼。 例如，復原服務保存庫資源識別碼。 |
| OperationName                     | Text          | 此欄位代表目前作業的名稱-BackupItem、BackupItemAssociation 或 ProtectedContainer。 |
| 類別                          | Text          | 此欄位代表已推送至 Azure 監視器記錄的診斷資料類別。 例如，CoreAzureBackup。 |
| AgentVersion                      | Text          | 代理程式備份或保護代理程式的版本號碼（如果是 SC DPM 和 MABS） |
| AzureBackupAgentVersion           | Text          | 備份管理伺服器上的 Azure 備份代理程式版本 |
| AzureDataCenter                   | Text          | 保存庫所在的資料中心                       |
| BackupItemAppVersion              | Text          | 備份項目的應用程式版本                       |
| BackupItemFriendlyName            | Text          | 備份專案的易記名稱                             |
| BackupItemName                    | Text          | 備份專案的名稱                                      |
| BackupItemProtectionState         | Text          | 備份項目的保護狀態                          |
| BackupItemFrontEndSize            | Text          | 備份項目的前端大小                            |
| BackupItemType                    | Text          | 備份專案的類型。 例如： VM、Filefolder)             |
| BackupItemUniqueId                | Text          | 備份項目的唯一識別碼                         |
| BackupManagementServerType        | Text          | 備份管理伺服器的類型，如 MABS，SC DPM     |
| BackupManagementServerUniqueId    | Text          | 可唯一識別備份管理伺服器的欄位      |
| BackupManagementType              | Text          | 執行備份作業之伺服器的提供者類型。 例如 IaaSVM、Filefolder |
| BackupManagementServerName        | Text          | 備份管理伺服器的名稱                         |
| BackupManagementServerOSVersion   | Text          | 備份管理伺服器的作業系統版本                   |
| BackupManagementServerVersion     | Text          | 備份管理伺服器的版本                      |
| LatestRecoveryPointLocation       | Text          | 備份項目的最新復原點所在的位置    |
| LatestRecoveryPointTime           | Datetime      | 備份項目的最新復原點的日期時間   |
| OldestRecoveryPointLocation       | Text          | 備份項目的最舊復原點所在的位置    |
| OldestRecoveryPointTime           | Datetime      | 備份項目的最新復原點的日期時間   |
| PolicyUniqueId                    | Text          | 用來識別原則的唯一識別碼                             |
| ProtectedContainerFriendlyName    | Text          | 受保護伺服器的易記名稱                        |
| ProtectedContainerLocation        | Text          | 受保護的容器是否位於內部部署或 Azure 中 |
| ProtectedContainerName            | Text          | 受保護容器的名稱                            |
| ProtectedContainerOSType          | Text          | 受保護容器的 OS 類型                          |
| ProtectedContainerOSVersion       | Text          | 受保護容器的作業系統版本                        |
| ProtectedContainerProtectionState | Text          | 受保護容器的保護狀態                  |
| ProtectedContainerType            | Text          | 受保護的容器是伺服器還是容器  |
| ProtectedContainerUniqueId        | Text          | 用來對所有項目 (使用 DPM、MABS 進行備份的 VM 除外) 的受保護容器進行識別的唯一識別碼 |
| ProtectedContainerWorkloadType    | Text          | 已備份的受保護容器類型。 例如 IaaSVMContainer |
| ProtectionGroupName               | Text          | 備份專案受保護的保護組名，適用于 SC DPM 和 MABS （如果適用） |
| resourceGroupName                 | Text          | 要收集之資料的資源群組（例如復原服務保存庫） |
| schemaVersion                     | Text          | 此欄位代表架構的目前版本，它是**V2** |
| SecondaryBackupProtectionState    | Text          | 是否要為備份項目啟用次要保護  |
| State                             | Text          | 備份專案物件的狀態。 例如，作用中、已刪除 |
| StorageReplicationType            | Text          | 保存庫的儲存體複寫類型。 例如，異地備援 |
| SubscriptionId                    | Text          | 收集資料的資源（例如復原服務保存庫）的訂用帳戶識別碼 |
| VaultName                         | Text          | 保存庫名稱                                            |
| VaultTags                         | Text          | 與保存庫資源相關聯的標記                    |
| VaultUniqueId                     | Text          | 保存庫的唯一識別碼                             |
| SourceSystem                      | Text          | 目前資料的來源系統-Azure                  |

## <a name="addonazurebackupalerts"></a>AddonAzureBackupAlerts

下表提供警示相關欄位的詳細資料。

| **欄位**                      | **資料類型** | **描述**                                              |
| :----------------------------- | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | 要收集其相關資料的資源所具備的唯一識別碼。 例如，復原服務保存庫資源識別碼 |
| OperationName                  | Text          | 目前作業的名稱。 例如，Alert            |
| 類別                       | Text          | 推送至 Azure 監視器記錄的診斷資料類別-AddonAzureBackupAlerts |
| AlertCode                      | Text          | 唯一識別警示類型的程式碼                     |
| AlertConsolidationStatus       | Text          | 識別警示是否為合併警示         |
| AlertOccurrenceDateTime        | Datetime      | 建立警示的日期和時間                     |
| AlertRaisedOn                  | Text          | 引發警示的實體類型                        |
| AlertSeverity                  | Text          | 警示的嚴重性。 例如，重大                 |
| AlertStatus                    | Text          | 警示的狀態。 例如，Active                     |
| AlertTimeToResolveInMinutes    | Number        | 解決警示所花費的時間。 作用中警示的時間會呈現為空白。     |
| AlertType                      | Text          | 警示的類型。 例如，備份                           |
| AlertUniqueId                  | Text          | 所產生警示的唯一識別碼                    |
| BackupItemUniqueId             | Text          | 與警示相關聯之備份專案的唯一識別碼 |
| BackupManagementServerUniqueId | Text          | 用來唯一識別備份專案的欄位（如果適用的話） |
| BackupManagementType           | Text          | 執行備份作業之伺服器的提供者類型，例如 IaaSVM、Filefolder) |
| CountOfAlertsConsolidated      | Number        | 如果是合併警示，則顯示合併警示的數目  |
| ProtectedContainerUniqueId     | Text          | 與警示相關聯之受保護伺服器的唯一識別碼 |
| RecommendedAction              | Text          | 建議用來解決警示的動作                      |
| schemaVersion                  | Text          | 結構描述的目前版本，例如 **V2**            |
| State                          | Text          | 警示物件的目前狀態 (例如，作用中、已刪除) |
| StorageUniqueId                | Text          | 用來識別儲存體實體的唯一識別碼                |
| VaultUniqueId                  | Text          | 用來識別與警示相關之保存庫的唯一識別碼    |
| SourceSystem                   | Text          | 目前資料的來源系統 - Azure                    |

## <a name="addonazurebackupprotectedinstance"></a>AddonAzureBackupProtectedInstance

此表格提供基本的受保護執行個體相關欄位。

| **欄位**                      | **資料類型** | **描述**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | 要收集資料之資源的唯一識別碼。 例如，復原服務保存庫資源識別碼 |
| OperationName                  | Text          | 作業的名稱，例如 ProtectedInstance         |
| 類別                       | Text          | 推送至 Azure 監視器記錄的診斷資料類別-AddonAzureBackupProtectedInstance |
| BackupItemUniqueId             | Text          | 備份項目的唯一識別碼                                 |
| BackupManagementServerUniqueId | Text          | 用來唯一識別備份專案的欄位（如果適用的話） |
| BackupManagementType           | Text          | 執行備份作業之伺服器的提供者類型，例如 IaaSVM、Filefolder) |
| ProtectedContainerUniqueId     | Text          | 用來識別執行作業之受保護容器的唯一識別碼 |
| ProtectedInstanceCount         | Text          | 該日期時間內相關聯的備份專案或受保護容器的受保護實例計數 |
| schemaVersion                  | Text          | 結構描述的目前版本，例如 **V2**            |
| State                          | Text          | 備份專案物件的狀態，例如 Active、Deleted |
| VaultUniqueId                  | Text          | 與受保護的實例相關聯之受保護保存庫的唯一識別碼 |
| SourceSystem                   | Text          | 目前資料的來源系統 - Azure                    |

## <a name="addonazurebackupjobs"></a>AddonAzureBackupJobs

下表提供作業相關欄位的詳細資料。

| **欄位**                      | **資料類型** | **描述**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | 正在收集資料的資源識別碼。 例如，復原服務保存庫資源識別碼 |
| OperationName                  | Text          | 此欄位代表目前作業的名稱 - Job    |
| 類別                       | Text          | 此欄位代表已推送至 Azure 監視器記錄的診斷資料分類-AddonAzureBackupJobs |
| AdhocOrScheduledJob            | Text          | 指定作業為臨機操作還是排程作業的欄位           |
| BackupItemUniqueId             | Text          | 用來對儲存體實體的相關備份項目進行識別的唯一識別碼 |
| BackupManagementServerUniqueId | Text          | 用來對儲存體實體的相關備份管理伺服器進行識別的唯一識別碼 |
| BackupManagementType           | Text          | 用於執行本作業所屬之備份的提供者類型 (例如，IaaSVM、FileFolder) |
| DataTransferredInMB            | Number        | 此工作的資料轉送 (以 MB 為單位)                          |
| JobDurationInSecs              | Number        | 總作業持續時間 (以秒為單位)                                |
| JobFailureCode                 | Text          | 由於發生作業失敗而產生的失敗碼字串    |
| JobOperation                   | Text          | 執行工作的作業，例如備份、還原、設定備份 |
| JobOperationSubType            | Text          | 工作作業的子類型。 例如，記錄備份作業的情況下為 ' Log ' |
| JobStartDateTime               | Datetime      | 開始執行作業的日期和時間                       |
| JobStatus                      | Text          | 已完成之作業的狀態 (例如，已完成、失敗)   |
| JobUniqueId                    | Text          | 用來識別作業的唯一識別碼                                |
| ProtectedContainerUniqueId     | Text          | 與作業相關聯之受保護伺服器的唯一識別碼 |
| RecoveryJobDestination         | Text          | 復原作業的目的地，資料會在此處復原   |
| RecoveryJobRPDateTime          | Datetime      | 要復原之復原點的建立日期和時間 |
| RecoveryJobLocation            | Text          | 要復原之復原點的儲存位置 |
| RecoveryLocationType           | Text          | 復原位置的類型                                |
| schemaVersion                  | Text          | 結構描述的目前版本，例如 **V2**            |
| State                          | Text          | 工作物件的目前狀態（例如，作用中、已刪除） |
| VaultUniqueId                  | Text          | 與作業相關聯之受保護保存庫的唯一識別碼 |
| SourceSystem                   | Text          | 目前資料的來源系統 - Azure                    |

## <a name="addonazurebackuppolicy"></a>AddonAzureBackupPolicy

下表提供原則相關欄位的詳細資料。

| **欄位**                       | **資料類型**  | **描述**                                              |
| ------------------------------- | -------------- | ------------------------------------------------------------ |
| ResourceId                      | Text           | 要收集資料之資源的唯一識別碼。 例如，復原服務保存庫資源識別碼 |
| OperationName                   | Text           | 作業的名稱，例如 [原則] 或 [PolicyAssociation] |
| 類別                        | Text           | 推送至 Azure 監視器記錄的診斷資料類別-AddonAzureBackupPolicy |
| BackupDaysOfTheWeek             | Text           | 一週中已排定要備份的日期            |
| BackupFrequency                 | Text           | 執行備份的頻率。 例如，每日、每週 |
| BackupManagementType            | Text           | 執行備份作業之伺服器的提供者類型。 例如，IaaSVM、Filefolder) |
| BackupManagementServerUniqueId  | Text           | 用來唯一識別備份專案的欄位（如果適用的話） |
| BackupTimes                     | Text           | 排定要備份的日期和時間                     |
| DailyRetentionDuration          | 整數   | 所設定備份的總保留期間 (以天為單位)      |
| DailyRetentionTimes             | Text           | 每日保留期的設定日期和時間            |
| DiffBackupDaysOfTheWeek         | Text           | Azure VM 備份中 SQL 的差異備份周中的天數 |
| DiffBackupFormat                | Text           | Azure VM 備份中用於 SQL 差異備份的格式   |
| DiffBackupRetentionDuration     | 十進位數字 | Azure VM 備份中 SQL 的差異備份保留期間 |
| DiffBackupTime                  | 時間           | Azure VM 備份中用於 SQL 差異備份的時間     |
| LogBackupFrequency              | 十進位數字 | SQL 記錄備份的頻率                            |
| LogBackupRetentionDuration      | 十進位數字 | Azure VM 備份中 SQL 記錄備份的保留期間 |
| MonthlyRetentionDaysOfTheMonth  | Text           | 設定每月保留期的月份周數。  例如，First、Last 等等。 |
| MonthlyRetentionDaysOfTheWeek   | Text           | 一週中所選用於每月保留期的日期              |
| MonthlyRetentionDuration        | Text           | 所設定備份的總保留期間 (以月為單位)    |
| MonthlyRetentionFormat          | Text           | 每月保留期的設定類型。 例如，每日以日為基礎，每週以周為基礎 |
| MonthlyRetentionTimes           | Text           | 每月保留期的設定日期和時間           |
| MonthlyRetentionWeeksOfTheMonth | Text           | 設定每月保留期的月份周數。   例如，First、Last 等等。 |
| PolicyName                      | Text           | 所定義原則的名稱                                   |
| PolicyUniqueId                  | Text           | 用來識別原則的唯一識別碼                             |
| PolicyTimeZone                  | Text           | 在記錄中指定原則時間欄位的時區 |
| RetentionDuration               | Text           | 所設定備份的保留期間                    |
| RetentionType                   | Text           | 保留類型                                            |
| schemaVersion                   | Text           | 此欄位代表架構的目前版本，它是**V2** |
| State                           | Text           | 原則物件的目前狀態。 例如，作用中、已刪除 |
| SynchronisationFrequencyPerDay  | 整數   | 在一天中，SC DPM 和 MABS 的檔案備份已同步處理的次數 |
| VaultUniqueId                   | Text           | 此原則所屬之保存庫的唯一識別碼          |
| WeeklyRetentionDaysOfTheWeek    | Text           | 一週中所選用於每週保留期的日期               |
| WeeklyRetentionDuration         | 十進位數字 | 已設定備份的每週保留持續時間總計（以周為單位） |
| WeeklyRetentionTimes            | Text           | 每週保留期的設定日期和時間            |
| YearlyRetentionDaysOfTheMonth   | Text           | 一個月中所選用於每年保留期的日期             |
| YearlyRetentionDaysOfTheWeek    | Text           | 一週中所選用於每年保留期的日期               |
| YearlyRetentionDuration         | 十進位數字 | 所設定備份的總保留期間 (以年為單位)     |
| YearlyRetentionFormat           | Text           | 每年保留的設定類型（例如，每日以日為基礎，以每週為基礎） |
| YearlyRetentionMonthsOfTheYear  | Text           | 一年中所選用於每年保留期的月數             |
| YearlyRetentionTimes            | Text           | 每年保留期的設定日期和時間            |
| YearlyRetentionWeeksOfTheMonth  | Text           | 為每年保留選取的月份周數             |
| SourceSystem                    | Text           | 目前資料的來源系統 - Azure                    |

## <a name="addonazurebackupstorage"></a>AddonAzureBackupStorage

下表提供儲存體相關欄位的詳細資料。

| **欄位**                      | **資料類型** | **描述**                                              |
| ------------------------------ | ------------- | ------------------------------------------------------------ |
| ResourceId                     | Text          | 正在收集資料的資源識別碼。 例如，復原服務保存庫資源識別碼 |
| OperationName                  | Text          | 此欄位代表目前作業的名稱-儲存體或 StorageAssociation |
| 類別                       | Text          | 此欄位代表已推送至 Azure 監視器記錄的診斷資料分類-AddonAzureBackupStorage |
| BackupItemUniqueId             | Text          | 唯一識別碼，用來識別使用 DPM （MABS）備份之 Vm 的備份專案 |
| BackupManagementServerUniqueId | Text          | 用來唯一識別備份專案的欄位（如果適用的話） |
| BackupManagementType           | Text          | 執行備份作業之伺服器的提供者類型。 例如，IaaSVM、Filefolder) |
| PreferredWorkloadOnVolume      | Text          | 此磁片區為慣用儲存體的工作負載      |
| ProtectedContainerUniqueId     | Text          | 與備份專案相關聯之受保護容器的唯一識別碼 |
| schemaVersion                  | Text          | 架構的版本。 例如， **V2**                   |
| State                          | Text          | 備份專案物件的狀態。 例如，作用中、已刪除 |
| StorageAllocatedInMBs          | Number        | 對應的備份專案在類型磁片的對應儲存體中所配置的儲存空間大小 |
| StorageConsumedInMBs           | Number        | 對應的儲存區中對應的備份專案所使用的儲存體大小 |
| StorageName                    | Text          | 儲存體實體的名稱。 例如，E:\                      |
| StorageTotalSizeInGBs          | Text          | 儲存體實體所耗用的儲存體大小總計 (以 GB 為單位)     |
| StorageType                    | Text          | 儲存體的類型，例如雲端、磁碟區、磁碟             |
| StorageUniqueId                | Text          | 用來識別儲存體實體的唯一識別碼                |
| VaultUniqueId                  | Text          | 用來識別與儲存體實體相關之保存庫的唯一識別碼 |
| VolumeFriendlyName             | Text          | 存放磁片區的易記名稱                          |
| SourceSystem                   | Text          | 目前資料的來源系統 - Azure                    |

## <a name="next-steps"></a>後續步驟

- [瞭解如何將診斷資料傳送至 Log Analytics](https://docs.microsoft.com/azure/backup/backup-azure-diagnostic-events)
- [瞭解如何在資源特定資料表上撰寫查詢](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor#sample-kusto-queries)
