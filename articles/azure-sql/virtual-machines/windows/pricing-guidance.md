---
title: 定價指導方針與管理成本
description: 提供選擇正確 SQL Server 虛擬機器定價模型的最佳做法。
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-machines-sql
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/09/2018
ms.author: mathoma
ms.reviewer: jroth
ms.custom: seo-lt-2019
ms.openlocfilehash: 57e9c82e5685171cff994aca7985f6a4211f00e7
ms.sourcegitcommit: 4295037553d1e407edeb719a3699f0567ebf4293
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/30/2020
ms.locfileid: "96327282"
---
# <a name="pricing-guidance-for-sql-server-on-azure-vms"></a>Azure Vm 上的 SQL Server 定價指導方針
[!INCLUDE[appliesto-sqlvm](../../includes/appliesto-sqlvm.md)]

本文提供 [Azure 虛擬機器上 SQL Server](sql-server-on-azure-vm-iaas-what-is-overview.md)的定價指導方針。 影響成本的選項有數個，而挑選平衡成本和業務需求的正確映像相當重要。

> [!TIP]
> 如果您只需要找出特定 SQL Server edition 和虛擬機器組合 (VM) 大小的成本預估，請參閱 [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) 或 [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux)的定價頁面。 從 [OS/軟體] 清單選取您的平台和 SQL Server 版本。
>
> ![VM 定價頁面上的 UI](./media/pricing-guidance/virtual-machines-pricing-ui.png)
>
> 或者，使用[定價計算機](https://azure.microsoft.com/pricing/#explore-cost)來新增和設定虛擬機器。 

## <a name="free-licensed-sql-server-editions"></a>免費授權的 SQL Server 版本

如果您想要開發、測試或建置某項概念證明，請使用免費授權的 **SQL Server Developer Edition**。 此版本包含 SQL Server Enterprise Edition 的所有功能，可讓您建置並測試任何類型的應用程式。 不過，您無法在生產環境中執行 Developer Edition。 SQL Server Developer Edition VM 只會產生 VM 成本的費用，因為沒有任何相關聯的 SQL Server 授權成本。

如果您希望以生產環境執行輕量型工作負載 (< 4 個核心、< 1 GB 記憶體、< 10 GB/資料庫)，請使用免費授權的 **SQL Server Express Edition**。 SQL Server Express Edition VM 也只會產生 VM 成本的費用。

針對這些開發/測試和輕量型生產環境工作負載，您也可以選擇符合這些工作負載的較小 VM 大小來節省成本。 在某些情況下，DS1v2 可能是理想的選擇。

若要使用這些映射的其中一個來建立執行 SQL Server 2017 的 Azure VM，請參閱下列連結：

| 平台 | 免費授權的映像 |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonWindowsServer2016)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonSLES12SP2)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Developer Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017DeveloperonUbuntuServer1604LTS)<br/>[SQL Server 2017 Express Azure VM](https://portal.azure.com/#create/Microsoft.FreeSQLServerLicenseSQLServer2017ExpressonUbuntuServer1604LTS) |

## <a name="paid-sql-server-editions"></a>付費的 SQL Server 版本

如果您的生產環境工作負載並非輕量型，請使用下列其中一個 SQL Server 版本：

| SQL Server 版本 | 工作負載 |
|-----|-----|
| Web | 小型網站 |
| 標準 | 小型到中型工作負載 |
| Enterprise | 大型或任務關鍵性工作負載|

您有兩個選項來支付這些版本的 SQL Server 授權：*依使用量付費* 或 *自備授權 (BYOL)* 。

## <a name="pay-per-usage"></a>依使用量付費

**依使用量支付 SQL Server 授權費用** (亦稱為 **隨用隨付**) 意謂著執行 Azure VM 的每秒鐘費用都包含 SQL Server 授權的費用。 您可以在 [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows) 或 [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux)的 Azure 虛擬機器定價頁面中，看到不同 SQL Server 版本 (Web、Standard、Enterprise) 的定價。

所有版本的 SQL Server (2012 SP3 至 2019) 的費用都相同。 每秒鐘的授權費用取決於 VM vCPU 數目。

針對下列情況，建議採用依使用量支付 SQL Server 授權費用：

- **暫時或定期的工作負載**。 例如，每年有幾個月需要支援某個事件或每週一需要支援業務分析的應用程式。

- **存留期或規模不明的工作負載**。 例如，有幾個月不需用到或依需求可能需要較多或較少運算能力的應用程式。

若要使用其中一個隨用隨付映射來建立執行 SQL Server 2017 的 Azure VM，請參閱下列連結：

| 平台 | 授權的映像 |
|---|---|
| Windows Server 2016 | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonWindowsServer2016)<br/>[SQL Server 2017 Standard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonWindowsServer2016)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseWindowsServer2016) |
| Red Hat Enterprise Linux | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Standard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonRedHatEnterpriseLinux74)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonRedHatEnterpriseLinux74) |
| SUSE Linux Enterprise Server | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonSLES12SP2)<br/>[SQL Server 2017 Standard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonSLES12SP2)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonSLES12SP2) |
| Ubuntu | [SQL Server 2017 Web Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017WebonUbuntuServer1604LTS)<br/>[SQL Server 2017 Standard Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017StandardonUbuntuServer1604LTS)<br/>[SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.SQLServer2017EnterpriseonUbuntuServer1604LTS) |

> [!IMPORTANT]
> 當您在 Azure 入口網站中建立 SQL Server 的虛擬機器時，[ **選擇大小** ] 視窗就會顯示估計的成本。 請務必注意，此預估值僅僅是執行 VM 的計算成本以及任何 OS 授權成本 (Windows 或協力廠商 Linux 作業系統)。
>
> ![選擇 VM 大小刀鋒視窗](./media/pricing-guidance/sql-vm-choose-size-pricing-estimate.png)
>
>不包含其他 SQL Server 的 Web、Standard 和 Enterprise 版本的授權成本。 若要取得最精確的定價估計，請在 [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) 或 [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) 適用的定價頁面上，選取您的作業系統和 SQL Server 版本。

> [!NOTE]
> 現在已可將授權模式從依使用量付費變更成自備授權 (BYOL)，反之亦然。 如需詳細資訊，請參閱[如何變更 SQL Server VM 的授權模式](licensing-model-azure-hybrid-benefit-ahb-change.md)。 

## <a name="bring-your-own-license-byol"></a><a id="byol"></a> 自備授權 (BYOL)

**透過「授權行動性」自備 SQL Server 授權** (也稱為 **BYOL**) 意謂著在 Azure VM 中使用現有的「SQL Server 大量授權」搭配「軟體保證」。 使用 BYOL 的 SQL Server VM 只會收取執行 VM 的費用，不會收取 SQL Server 授權費用，但前提是您已經透過「大量授權」方案取得授權和「軟體保證」。

> [!IMPORTANT]
> BYOL 映像需要具有軟體保證的 Enterprise 合約。 Azure 雲端解決方案合作夥伴 (CSP) 目前不會提供這類映像。 CSP 客戶可以藉由部署隨用隨付映像，然後啟用 [Azure Hybrid Benefit](licensing-model-azure-hybrid-benefit-ahb-change.md)，來攜帶自己的授權。

> [!NOTE]
> BYOL 映像目前只適用於 Windows 虛擬機器。 不過，您可以在僅有 Linux 的 VM 上手動安裝 SQL Server。 請參閱 [LINUX VM 上 SQL Server](../linux/frequently-asked-questions-faq.md)中的指導方針常見問題。

建議您透過授權行動性提供自己的 SQL Server 授權給：

- **持續性工作負載**。 例如，需要全年無休支援業務營運的應用程式。

- **存留期和規模已知的工作負載**。 例如，一整年都需要使用且已預測好需求的應用程式。

若要搭配 SQL Server VM 使用 BYOL，您必須具備 SQL Server Standard 或 Enterprise 的授權及[軟體保證](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default.aspx#tab=1)，這是透過某些大量授權方案時的必要選項，也是搭配其他方案時的選購項目。 根據合約的類型及數量和 (或) 對 SQL Server 之承諾的不同，透過「大量授權」方案提供的定價層級也會不同。 但根據經驗法則，自備授權可為持續性生產環境工作負載提供下列優點：

| BYOL 優點 | 描述 |
|-----|-----|
| **節省成本** | [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-benefit/) 可讓您省下高達 55% 的成本。 如需詳細資訊，請參閱[切換授權模型](licensing-model-azure-hybrid-benefit-ahb-change.md) |
| **免費的被動次要複本** | 自備授權的另一個優點是每一 SQL Server 可享有[一個免費的被動次要複本授權](https://azure.microsoft.com/pricing/licensing-faq/)，以用於提供高可用性。 這讓高可用性 SQL Server 部署 (例如使用「永遠開啟可用性群組」) 的授權費用得以砍半。 執行被動次要複本的權限是透過「容錯移轉伺服器軟體保證」權益來提供。 |

若要使用下列其中一個自備授權映射來建立執行 SQL Server 2017 的 Azure VM，請查看前面加上 "{BYOL}" 的 Vm：

- [SQL Server 2017 Enterprise Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017EnterpriseWindowsServer2016)
- [SQL Server 2017 Standard Azure VM](https://portal.azure.com/#create/Microsoft.BYOLSQLServer2017StandardonWindowsServer2016)

> [!IMPORTANT]
> 請在 10 天內告訴我們您將在 Azure 中使用多少個 SQL Server 授權。 上述映像的連結包含如何這麼做的相關指示。

> [!NOTE]
> 現在已可將授權模式從依使用量付費變更成自備授權 (BYOL)，反之亦然。 如需詳細資訊，請參閱[如何變更 SQL Server VM 的授權模式](licensing-model-azure-hybrid-benefit-ahb-change.md)。 



## <a name="reduce-costs"></a>降低成本

若要避免不必要的成本，請選擇最佳的虛擬機器大小，並考慮針對非連續工作負載間歇性進行關機。

### <a name="correctly-size-your-vm"></a><a id="machinesize"></a> 選擇適當的 VM 大小

SQL Server 的授權成本與 vCPU 數目直接相關。 選擇符合預期的 CPU、記憶體、儲存和 I/O 頻寬需求的 VM 大小。 如需機器大小選項的完整清單，請參閱 [Windows VM 大小](../../../virtual-machines/sizes.md)和 [Linux VM 大小](../../../virtual-machines/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。

有一些新的機器大小適用於特定類型的 SQL Server 工作負載。 這些機器的大小可維持高等級的記憶體、儲存以及 I/O 頻寬，但是它們具有較低的虛擬化核心計數。 例如，請考慮下列範例：

| VM 大小 | vCPU | 記憶體 | 磁碟上限 | 最大 I/O 輸送量 | SQL Server 授權成本 | 總成本 (計算 + 授權)  |
|---|---|---|---|---|---|---|
| **Standard_DS14v2** | 16 | 112 GB | 32 | 51,200 IOPS 或 768 MB/秒 | | |
| **Standard_DS14-4v2** | 4 | 112 GB | 32 | 51,200 IOPS 或 768 MB/秒 | 降低 75% | 降低 57% |

> [!IMPORTANT]
> 這是時間點的範例。 如需最新規格，請參閱 [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) 和 [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) 的機器大小文章和 Azure 定價頁面。

在上述範例中，您可以看到 **Standard_DS14v2** 和 **Standard_DS14 4v2** 的規格除了 vCPU 之外完全相同。 **Standard_DS14 4v2** 機器大小尾端的尾碼 **-4v2**，表示有效 vCPU 的數目。 由於 SQL Server 授權成本與 vCPU 數目有關，因此這會大幅降低不需要額外 vCPU 時的 VM 成本。 這是一個範例，而且有許多機器大小，具有使用此尾碼模式識別的受限制 vCPU。 如需詳細資訊，請參閱部落格文章[針對更多符合成本效益的資料庫工作推出新的 Azure VM 大小](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/)。

### <a name="shut-down-your-vm-when-possible"></a>若可能則關閉 VM

如果您要使用任何不會持續執行的工作負載，請考慮在非使用中的期間關閉虛擬機器。 您只需依據使用量付費。

例如，如果您只是要在 Azure VM 上試用 SQL Server，您就不會希望不小心讓它持續執行數週而產生費用。 其中一個解決方案就是使用[自動關閉功能](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/)。

![SQL Server VM 自動關機](./media/pricing-guidance/sql-vm-auto-shutdown.png)

自動關閉是 [Azure DevTest Labs](https://azure.microsoft.com/services/devtest-lab) 所提供的較大一組相似功能的一部分。

針對其他工作負載，請考慮使用指令碼解決方案 (例如 [Azure 自動化](https://azure.microsoft.com/services/automation/)) 來自動關閉並重新啟動 Azure VM。

> [!IMPORTANT]
> 將 VM 關閉並解除配置是唯一可以避免產生費用的方式。 只是停止或使用電源選項來關閉 VM 仍然會產生使用費。

## <a name="next-steps"></a>後續步驟

如需一般的 Azure 定價指導方針，請參閱[使用 Azure 計費與成本管理避免非預期的成本](../../../cost-management-billing/cost-management-billing-overview.md)。 如需最新的 Azure 虛擬機器定價（包括 SQL Server），請參閱適用于 [Windows vm](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) 和 [Linux Vm](https://azure.microsoft.com/pricing/details/virtual-machines/linux/)的 azure 虛擬機器定價頁面。

如需 Azure 虛擬機器上 SQL Server 的總覽，請參閱下列文章：

- [Windows VM 上的 SQL Server 概觀](sql-server-on-azure-vm-iaas-what-is-overview.md)
- [Linux VM 上的 SQL Server 概觀](../linux/sql-server-on-linux-vm-what-is-iaas-overview.md)