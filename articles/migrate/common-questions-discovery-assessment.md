---
title: Azure Migrate 中的探索、評估和相依性分析的相關問題
description: 取得 Azure Migrate 中探索、評估和相依性分析的常見問題解答。
ms.topic: conceptual
ms.date: 06/09/2020
ms.openlocfilehash: 7b26d4442f9a84375205e7778ae037b565f53438
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2020
ms.locfileid: "86118829"
---
# <a name="discovery-assessment-and-dependency-analysis---common-questions"></a>探索、評估和相依性分析-常見問題

本文會回答有關 Azure Migrate 中探索、評估和相依性分析的常見問題。 如果您有其他問題，請檢查下列資源：

- 關於 Azure Migrate 的[一般問題](resources-faq.md)
- [Azure Migrate 設備](common-questions-appliance.md)的相關問題
- [伺服器遷移](common-questions-server-migration.md)的相關問題
- 在[Azure Migrate 論壇](https://aka.ms/AzureMigrateForum)中獲得解答


## <a name="what-geographies-are-supported-for-discovery-and-assessment-with-azure-migrate"></a>有哪些地理位置支援使用 Azure Migrate 進行探索和評量？

請檢閱[公用](migrate-support-matrix.md#supported-geographies-public-cloud)和[政府雲端](migrate-support-matrix.md#supported-geographies-azure-government)支援的地理位置。


## <a name="how-many-vms-can-i-discover-with-an-appliance"></a>我可以使用設備探索多少部 Vm？

您可以使用單一設備，探索最多10000個 VMware Vm、最多5000部 Hyper-v Vm，以及最多1000個實體伺服器。 如果您有更多電腦，請參閱[調整 hyper-v 評估](scale-hyper-v-assessment.md)、[調整 VMware 評估](scale-vmware-assessment.md)，或[調整實體伺服器評](scale-physical-assessment.md)量。

## <a name="how-do-i-choose-the-assessment-type"></a>如何選擇評量類型？

- 當您想要評估您的內部部署[VMware vm](how-to-set-up-appliance-vmware.md)、 [hyper-v vm](how-to-set-up-appliance-hyper-v.md)和[實體伺服器](how-to-set-up-appliance-physical.md)以遷移至 Azure VM 時，請使用**Azure VM 評**量。 [深入了解](concepts-assessment-calculation.md)

- 當您想要使用此評量類型來評估內部部署[Vmware vm](how-to-set-up-appliance-vmware.md)以遷移至[Azure VMWARE 解決方案（avs）](../azure-vmware/introduction.md)時，請使用**Azure VMware 解決方案（avs）** 評量。 [深入了解](concepts-azure-vmware-solution-assessment-calculation.md)

- 您只能使用與 VMware 機器相同的通用群組來執行這兩種類型的評量。 請注意，如果您是第一次在 Azure Migrate 中執行 AVS 評量，建議您建立新的 VMware 機器群組。

## <a name="i-cant-see-some-groups-when-i-am-creating-an-azure-vmware-solution-avs-assessment"></a>當我在建立 Azure VMware 解決方案（AVS）評估時，看不到某些群組

- 您可以在只有 VMware 電腦的群組上進行 AVS 評估。 如果要執行 AVS 評估，請從群組中移除所有非 VMware 機器。
- 如果您是第一次在 Azure Migrate 中執行 AVS 評量，建議您建立新的 VMware 機器群組。

## <a name="how-do-i-select-ftt-raid-level-in-avs-assessment"></a>如何? 在 AVS 評估中選取 [FTT-RAID 層級]？

AVS 中使用的儲存引擎是 vSAN。 vSAN 儲存原則會定義虛擬機器的儲存體需求。 這些原則可確保 VM 所需的服務層級，因為原則會決定如何將儲存體配置給 VM。 以下是可用的 FTT-Raid 組合： 

**可容許的失敗 (FTT)** | **RAID 組態** | **最低主機需求** | **大小考量**
--- | --- | --- | --- 
1 | RAID-1 (鏡像) | 3 | 100GB 的 VM 會耗用 200GB。
1 | RAID-5 (抹除編碼) | 4 | 100GB 的 VM 會耗用 133.33GB
2 | RAID-1 (鏡像) | 5 | 100GB 的 VM 會耗用 300GB。
2 | RAID-6 (抹除編碼) | 6 | 100GB 的 VM 會耗用 150GB。
3 | RAID-1 (鏡像) | 7 | 100GB 的 VM 會耗用 400GB。

## <a name="i-cant-see-some-vm-types-in-azure-government"></a>我在 Azure Government 中看不到某些 VM 類型

支援評估和遷移的 VM 類型取決於 Azure Government 位置中的可用性。 您可以在 Azure Government 中[查看和比較](https://azure.microsoft.com/global-infrastructure/services/?regions=usgov-non-regional,us-dod-central,us-dod-east,usgov-arizona,usgov-iowa,usgov-texas,usgov-virginia&products=virtual-machines)VM 類型。

## <a name="the-size-of-my-vm-changed-can-i-run-an-assessment-again"></a>我的 VM 大小已變更。 我可以再次執行評量嗎？

Azure Migrate 設備會持續收集內部部署環境的相關資訊。  評量是內部部署 Vm 的時間點快照集。 如果您在想要評估的 VM 上變更設定，請使用 [重新計算] 選項，以最新的變更來更新評量。

## <a name="how-do-i-discover-vms-in-a-multitenant-environment"></a>如何? 在多租使用者環境中探索 Vm 嗎？

- **VMware**：如果環境是在租使用者之間共用，而您不想要探索租使用者的訂用帳戶中的 vm，請建立 VMware vCenter Server 認證，以僅存取您想要探索的 vm。 然後，當您在 Azure Migrate 設備中啟動探索時，請使用這些認證。
- **Hyper-v**：探索使用 hyper-v 主機認證。 如果 Vm 共用相同的 Hyper-v 主機，目前沒有任何方法可以分隔探索。  

## <a name="do-i-need-vcenter-server"></a>我需要 vCenter Server 嗎？

是，Azure Migrate 需要 VMware 環境中的 vCenter Server 才能執行探索。 Azure Migrate 不支援探索不是由 vCenter Server 管理的 ESXi 主機。

## <a name="what-are-the-sizing-options-in-an-azure-vm-assessment"></a>Azure VM 評量中的調整大小選項有哪些？

使用內部部署調整大小，Azure Migrate 不會考慮要進行評估的 VM 效能資料。 Azure Migrate 會根據內部部署設定來評估 VM 大小。 使用以效能為基礎的調整大小，調整大小取決於使用量資料。

例如，如果內部部署 VM 具有四個核心和 8 GB 的記憶體（50% CPU 使用率和50% 的記憶體使用率）：
- 內部部署調整大小會建議具有四個核心和 8 GB 記憶體的 Azure VM SKU。
- 以效能為基礎的大小調整將會建議有兩個核心和 4 GB 記憶體的 VM SKU，因為會考慮使用率百分比。

同樣地，磁片大小調整取決於調整大小準則和儲存體類型：
- 如果調整大小準則是以效能為基礎，且儲存體類型是自動的，Azure Migrate 會在識別目標磁片類型（標準或高階）時，將磁片的 IOPS 和輸送量值納入考慮。
- 如果調整大小準則是以效能為基礎，且儲存體類型為 Premium，Azure Migrate 會根據內部部署磁片的大小來建議 Premium 磁片 SKU。 當調整大小為 [內部部署] 且儲存體類型為 [標準] 或 [Premium] 時，會將相同的邏輯套用至磁片大小調整。

## <a name="does-performance-history-and-utilization-affect-sizing-in-an-azure-vm-assessment"></a>效能歷程記錄和使用量是否會影響 Azure VM 評估中的大小？

是，效能歷程記錄和使用量會影響 Azure VM 評估中的大小。

### <a name="performance-history"></a>效能歷程記錄

僅針對以效能為基礎的大小調整，Azure Migrate 會收集內部部署機器的效能歷程記錄，然後使用它來建議 Azure 中的 VM 大小和磁片類型：

1. 設備會持續分析內部部署環境，每隔20秒收集一次即時使用方式資料。
2. 設備會匯總所收集的20秒樣本，並使用它們每隔15分鐘建立一個資料點。
3. 若要建立資料點，設備會從所有20秒範例中選取尖峰值。
4. 應用裝置會將資料點傳送至 Azure。

### <a name="utilization"></a>使用率

當您在 Azure 中建立評估時，視效能持續時間和設定的效能歷程百分位數值而定，Azure Migrate 會計算有效的使用率值，然後使用它來調整大小。

例如，如果您將 [效能持續時間] 設為 [一天]，並將 [百分位數] 值設定為第95個百分位數，Azure Migrate 會以遞增的順序排序收集器過去一天所傳送的15分鐘取樣點。 它會挑選第95個百分位數值作為有效使用率。

使用第95個百分位數值可確保忽略極端值。 如果您的 Azure Migrate 使用第99個百分位數，可能會包含極端值。 若要挑選期間的尖峰使用量，但不遺漏任何極端值，請將 Azure Migrate 設定為使用第99個百分位數。


## <a name="how-are-import-based-assessments-different-from-assessments-with-discovery-source-as-appliance"></a>以匯入為基礎的評量與以探索來源作為設備的評估有何不同？

以匯入為基礎的 Azure VM 評量是使用 CSV 檔案匯入 Azure Migrate 的機器所建立的評量。 只有四個欄位是必要的匯入：伺服器名稱、核心、記憶體和作業系統。 以下是一些要注意的事項： 
 - 在開機類型參數上以匯入為基礎的評量中，準備就緒準則較不嚴格。 如果未提供開機類型，系統會假設電腦具有 BIOS 開機類型，且機器未標示為有條件地**備**妥。 在使用探索來源做為設備的評量中，如果遺失開機類型，就會將準備就緒標示為有**條件地備**妥。 這項在就緒計算方面的差異在於，使用者在進行以匯入為基礎的評估時，可能不會在早期的遷移計畫階段中擁有電腦上的所有資訊。 
 - 以效能為基礎的匯入評估會使用使用者所提供的使用率值來進行適當大小的計算。 由於 [使用率] 值是由使用者提供，因此在評估屬性中會停用 [**效能歷程記錄**] 和 [**百分位數使用量**] 選項。 在使用探索來源作為設備的評量中，會從設備所收集的效能資料中挑選所選的百分位數值。

## <a name="why-is-the-suggested-migration-tool-in-import-based-avs-assessment-marked-as-unknown"></a>為什麼匯入型 AVS 評估中的建議遷移工具標示為未知？

對於透過 CSV 檔案匯入的機器，AVS 評估中的預設遷移工具是未知的。 不過，對於 VMware 機器，建議使用 VMWare 混合式雲端擴充功能（HCX）解決方案。 [深入了解](../azure-vmware/hybrid-cloud-extension-installation.md)。


## <a name="what-is-dependency-visualization"></a>什麼是相依性視覺效果？

相依性視覺效果可協助您評估 Vm 群組，以便更安心地進行遷移。 相依性視覺效果會在您執行評量之前，先交叉檢查機器相依性。 它有助於確保不會留下任何內容，並可在您遷移至 Azure 時避免非預期的中斷。 Azure Migrate 使用了 Azure 監視器中的 [服務對應] 解決方案來啟用相依性視覺效果。 [深入了解](concepts-dependency-visualization.md)。

> [!NOTE]
> Azure Government 不提供以代理程式為基礎的相依性分析。 您可以使用無代理程式相依性分析

## <a name="whats-the-difference-between-agent-based-and-agentless"></a>代理程式和無代理程式之間的差異為何？

「無代理程式」視覺效果和以代理程式為基礎的視覺效果之間的差異摘要于表格中。

**需求** | **無代理程式** | **以代理程式為基礎**
--- | --- | ---
支援 | 此選項目前為預覽狀態，且僅適用于 VMware Vm。 [檢查](migrate-support-matrix-vmware.md#dependency-analysis-requirements-agentless)支援的作業系統。 | 正式運作（GA）。
代理程式 | 不需要在您要交叉檢查的電腦上安裝代理程式。 | 要在每個您想要分析的內部部署機器上安裝的代理[程式： Microsoft Monitoring agent （MMA）](../azure-monitor/platform/agent-windows.md)和[Dependency agent](../azure-monitor/platform/agents-overview.md#dependency-agent)。 
必要條件 | 請[參閱](concepts-dependency-visualization.md#agentless-analysis)必要條件和部署需求。 | 請[參閱](concepts-dependency-visualization.md#agent-based-analysis)必要條件和部署需求。
Log Analytics | 不需要。 | Azure Migrate 會使用 [Azure 監視器記錄](../azure-monitor/log-query/log-query-overview.md)中的[服務對應](../azure-monitor/insights/service-map.md)解決方案來實現相依性視覺效果。 [深入了解](concepts-dependency-visualization.md#agent-based-analysis)。
運作方式 | 會在啟用相依性視覺效果的電腦上，捕獲 TCP 連線資料。 探索之後，它會以五分鐘的間隔收集資料。 | 安裝在電腦上的服務對應代理程式會收集有關 TCP 進程的資料，以及每個進程的輸入/輸出連接。
資料 | 來源電腦伺服器名稱、進程、應用程式名稱。<br/><br/> 目的地電腦伺服器名稱、進程、應用程式名稱和埠。 | 來源電腦伺服器名稱、進程、應用程式名稱。<br/><br/> 目的地電腦伺服器名稱、進程、應用程式名稱和埠。<br/><br/> 系統會收集連線、延遲和資料傳輸資訊的數目，並可供 Log Analytics 查詢使用。 
視覺效果 | 單一伺服器的相依性對應可以在一小時到30天的期間內查看。 | 單一伺服器的相依性對應。<br/><br/> 只能在一小時內查看對應。<br/><br/> 伺服器群組的相依性對應。<br/><br/> 從地圖視圖新增和移除群組中的伺服器。
資料匯出 | 過去30天的資料可以 CSV 格式下載。 | 您可以使用 Log Analytics 來查詢資料。


## <a name="do-i-need-to-deploy-the-appliance-for-agentless-dependency-analysis"></a>我是否需要部署設備以進行無代理程式相依性分析？

是，必須部署[Azure Migrate 設備](migrate-appliance.md)。

## <a name="do-i-pay-for-dependency-visualization"></a>我要支付相依性視覺效果嗎？

否。 深入瞭解[Azure Migrate 定價](https://azure.microsoft.com/pricing/details/azure-migrate/)。

## <a name="what-do-i-install-for-agent-based-dependency-visualization"></a>針對代理程式相依性視覺效果，我要安裝什麼？

若要使用以代理程式為基礎的相依性視覺效果，請在您想要評估的每部內部部署機器上，下載並安裝代理程式：

- [Microsoft Monitoring Agent （MMA）](../azure-monitor/platform/agent-windows.md)
- [相依性代理程式](../azure-monitor/platform/agents-overview.md#dependency-agent)
- 如果您的電腦沒有網際網路連線能力，請下載並安裝 Log Analytics 閘道。

只有當您使用以代理程式為基礎的相依性視覺效果時，才需要這些代理程式。

## <a name="can-i-use-an-existing-workspace"></a>我可以使用現有的工作區嗎？

是，若是以代理程式為基礎的相依性視覺效果，您可以將現有的工作區附加至遷移專案，並將其用於相依性視覺效果。 

## <a name="can-i-export-the-dependency-visualization-report"></a>是否可以匯出相依性視覺效果報告？

否，無法匯出以代理程式為基礎的視覺效果中的相依性視覺效果報表。 不過，Azure Migrate 會使用服務對應，而且您可以使用[服務對應 REST API](/rest/api/servicemap/machines/listconnections)來取得 JSON 格式的相依性。

## <a name="can-i-automate-agent-installation"></a>我可以自動安裝代理程式嗎？

針對以代理程式為基礎的相依性視覺效果：

- 使用[腳本來安裝 Dependency agent](../azure-monitor/insights/vminsights-enable-hybrid-cloud.md#installation-script-examples)。
- 針對 MMA，請[使用命令列或自動化](../azure-monitor/platform/log-analytics-agent.md#installation-and-configuration)，或使用[腳本](https://gallery.technet.microsoft.com/scriptcenter/Install-OMS-Agent-with-2c9c99ab)。
- 除了腳本以外，您還可以使用 Microsoft Endpoint Configuration Manager 和[Intigua](https://www.intigua.com/intigua-for-azure-migration)等部署工具來部署代理程式。

## <a name="what-operating-systems-does-mma-support"></a>MMA 支援哪些作業系統？

- 查看[MMA 支援的 Windows 作業系統](../azure-monitor/platform/log-analytics-agent.md#supported-windows-operating-systems)清單。
- 查看[MMA 支援的 Linux 作業系統](../azure-monitor/platform/log-analytics-agent.md#supported-linux-operating-systems)清單。

## <a name="can-i-visualize-dependencies-for-more-than-one-hour"></a>我可以將相依性視覺化超過一小時嗎？

對於以代理程式為基礎的視覺效果，您最多可以將相依性視覺化一小時。 您可以在歷程記錄中返回一個月的特定日期，但視覺效果的最大持續時間為一小時。 例如，您可以使用相依性對應中的持續時間來查看昨天的相依性，但只能查看一個小時範圍內的相依性。 不過，您可以使用 Azure 監視器記錄來[查詢](./how-to-create-group-machine-dependencies.md)較長持續時間的相依性資料。

對於無代理程式視覺效果，您可以從一小時到30天之間的持續時間，查看單一伺服器的相依性對應。

## <a name="can-i-visualize-dependencies-for-groups-of-more-than-10-vms"></a>我可以將超過10個 Vm 的群組相依性視覺化嗎？

您可以將具有多達10個 Vm 的群組的相依性[視覺化](./how-to-create-a-group.md#refine-a-group-with-dependency-mapping)。 如果您有超過10個 Vm 的群組，建議您將群組分割成較小的群組，然後將相依性視覺化。

## <a name="next-steps"></a>後續步驟

閱讀[Azure Migrate 的總覽](migrate-services-overview.md)。
