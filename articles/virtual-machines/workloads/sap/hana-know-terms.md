---
title: 認識 SAP HANA on Azure (大型執行個體) 的詞彙 | Microsoft Docs
description: 認識 SAP HANA on Azure (大型執行個體) 的詞彙。
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/21/2020
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6785ae821f701121185f0064c6317c69d50191ab
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "77617057"
---
# <a name="know-the-terms"></a>認識詞彙

《架構和技術部署指南》中廣泛使用數個常見的定義。 請注意下列詞彙及其意義：

- **IaaS**：基礎設施即服務。
- **PaaS**： 平臺即服務。
- **SaaS**： 軟體即服務.
- **SAP 元件**︰個別的 SAP 應用程式，例如 ERP Central Component (ECC)、Business Warehouse (BW)、Solution Manager 或企業版入口網站 (EP)。 SAP 元件可以傳統 ABAP 或 Java 技術為基礎，或以非 NetWeaver 應用程式 (例如商務物件) 為基礎。
- **SAP 環境**︰一或多個以邏輯方式分組的 SAP 元件，可執行像是開發、品質保證、訓練、災害復原或生產等商務功能。
- **SAP 橫向**：指 IT 環境中的整個 SAP 資產。 SAP 環境包含所有生產和非生產環境。
- **SAP 系統**︰DBMS 層與應用程式層的組合，例如 SAP ERP 開發系統、SAP BW 測試系統、SAP CRM 生產系統等。 Azure 部署不支援在內部部署與 Azure 之間分割這兩層。 SAP 系統可以在內部部署或在 Azure 中部署。 您可以將 SAP 環境的不同系統部署到 Azure 或內部部署中。 例如，您可以在 Azure 中部署 SAP CRM 開發和測試系統，而在內部部署 SAP CRM 生產系統。 就 SAP HANA on Azure (大型執行個體) 而言，您應在 VM 中裝載 SAP 系統的 SAP 應用程式層，並在 SAP HANA on Azure (大型執行個體) 戳記中的某個單位上裝載相關的 SAP HANA 執行個體。
- **大型執行個體戳記**︰經 SAP HANA TDI 認證，並專門用來執行 Azure 內的 SAP HANA 執行個體的硬體基礎結構堆疊。
- **SAP HANA on Azure (大型執行個體)**：Azure 中的供應項目的正式名稱；此供應項目可在經 SAP HANA TDI 認證並部署在不同 Azure 區域的「大型執行個體」戳記中的硬體上執行 HANA 執行個體。 相關詞彙 *HANA 大型執行個體*是 *SAP HANA on Azure (大型執行個體)* 的簡稱，廣泛使用於本技術部署指南中。
- **跨單位**：說明將 VM 部署到在內部部署資料中心與 Azure 之間具有站對站、多網站或 Azure ExpressRoute 連線能力的 Azure 訂用帳戶的案例。 在一般 Azure 文件中，這類部署也會描述為跨單位案例。 之所以連線，是為了將內部部署網域、內部部署 Azure Active Directory/OpenLDAP 和內部部署 DNS 擴充到 Azure 中。 內部部署架構會擴充至 Azure 訂用帳戶的 Azure 資產。 透過此擴充，VM 可以是內部部署網域的一部分。 

   內部部署網域的網域使用者可以存取伺服器，並可在這些 VM 上執行服務 (例如 DBMS 服務)。 在內部部署的 VM 與 Azure 部署的 VM 之間可進行通訊和名稱解析。 這是部署大部分 SAP 資產時的典型案例。 有關詳細資訊，請參閱[Azure VPN 閘道](../../../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)，並使用 Azure[門戶創建具有網站到網站連接的虛擬網路](../../../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)。
- **租用戶**：部署於 HANA 大型執行個體戳記的客戶會隔離到*租用戶*中。 每個租用戶的網路、儲存體和計算層會彼此區隔開來。 指派給不同租用戶的儲存體和計算單位無法看到彼此，也無法在 HANA 大型執行個體戳記層級上彼此通訊。 客戶可以選擇部署到不同的租用戶。 即使如此，租用戶彼此之間還是無法在 HANA 大型執行個體戳記層級上進行通訊。
- **SKU 類別**：對於 HANA 大型執行個體，會提供下列兩種 SKU：
    - **I類**： S72、 S72m、 S96、 S144、 S144m、 S192、 S192m、 S192xm、 S224 和 S224m
    - **類型 II 類別**：S384、S384m、S384xm、S384xxm、S576m、S576xm、S768m、S768xm 和 S960m
- **標記**：定義 HANA 大型實例的 Microsoft 內部部署大小。 在部署 HANA 大型實例單元之前，需要在資料中心位置部署由計算、網路和存儲機架組成的 HANA 大型實例戳。 此類部署稱為 HANA 大型實例戳，或從修訂版 4（見下文）使用 **"大型實例行**"的交替術語
- **修訂：HANA**大型實例郵票有兩種不同的郵票修訂版。 這些在體系結構和與 Azure 虛擬機器主機的鄰近性上有所不同
    - "修訂版 3"（修訂版 3）：是 2016 年年中開始部署的原始設計
    - "修訂版 4"（修訂版 4）：是一種新設計，可以更靠近 Azure 虛擬機器主機，並且 Azure VM 和 HANA 大型實例單元之間的網路延遲較低 

您可以透過其他多項資源了解如何在雲端中部署 SAP 工作負載。 如果您想要在 Azure 中執行 SAP HANA 的部署，您必須熟悉並了解 Azure IaaS 的主體及 Azure IaaS 上的 SAP 工作負載部署。 在繼續作業之前，請參閱[在 Azure 虛擬機器上使用 SAP 解決方案](get-started.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)以取得詳細資訊。 

**後續步驟**
- 請參閱 [HLI 認證](hana-certification.md)