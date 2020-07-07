---
title: Azure VMware Solution by CloudSimple-適用于 CloudSimple 服務的安全性
description: 描述 CloudSimple 服務安全性的共同責任模型
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6d86c90828c081a542fa5574493a46e8a2e44640
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/02/2020
ms.locfileid: "82187472"
---
# <a name="cloudsimple-security-overview"></a>CloudSimple 安全性總覽

本文概述如何藉由 CloudSimple 服務、基礎結構和資料中心，來瞭解如何在 Azure VMware 解決方案上執行安全性。 您將瞭解資料保護和安全性、網路安全性，以及弱點和修補程式的管理方式。

## <a name="shared-responsibility"></a>共同責任

Azure VMware Solution by CloudSimple 會針對安全性使用共同責任模型。 雲端中受信任的安全性是透過客戶和 Microsoft 即服務提供者的共同責任來達成。 此責任矩陣可提供更高的安全性，並消除單一失敗點。

## <a name="azure-infrastructure"></a>Azure 基礎結構

Azure 基礎結構安全性考慮包括資料中心和設備位置。

### <a name="datacenter-security"></a>資料中心安全性

Microsoft 有一整個部門專門用來設計、建立和操作支援 Azure 的實體設施。 這個團隊致力於維護最新的實體安全性。 如需實體安全性的詳細資訊，請參閱[Azure 設施、內部部署和實體安全性](../security/azure-physical-security.md)。

### <a name="equipment-location"></a>設備位置

執行私人雲端的裸機硬體設備會託管于 Azure 資料中心位置。  該設備所在的機殼需要以生物識別為基礎的雙因素驗證，才能取得存取權。

## <a name="dedicated-hardware"></a>專用硬體

作為 CloudSimple 服務的一部分，所有 CloudSimple 客戶都會取得專用的裸機主機，其中包含與其他租使用者硬體實際隔離的本機連接磁片。 具有 vSAN 的 ESXi 管理程式會在每個節點上執行。 節點是透過客戶專用的 VMware vCenter 和 NSX 來管理。 不在租使用者之間共用硬體會提供額外一層的隔離和安全性保護。

## <a name="data-security"></a>資料安全性

客戶會保留其資料的控制和擁有權。 客戶資料的資料監管是客戶的責任。

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>待用資料的資料保護和內部網路內的移動資料

對於私人雲端環境中的待用資料，您可以使用 vSAN 加密。 vSAN 加密可與您自己的虛擬網路或內部部署中的 VMware 認證外部金鑰管理伺服器（KMS）搭配運作。  您可以自行控制資料加密金鑰。 對於在私人雲端中移動的資料，vSphere 支援透過網路加密所有 vmkernel 流量（包括 vMotion 流量）的資料。

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>透過公用網路移動所需資料的資料保護

若要保護透過公用網路移動的資料，您可以為私人雲端建立 IPsec 和 TLS VPN 通道。 支援一般加密方法，包括 128-位元組和256位元組 AES。 傳輸中的資料（包括驗證、系統管理存取權和客戶資料）會使用標準加密機制（SSH、TLS 1.2 和安全 RDP）進行加密。 傳輸機密資訊的通訊會使用標準加密機制。

### <a name="secure-disposal"></a>安全處置

如果您的 CloudSimple 服務過期或已終止，您就必須負責移除或刪除資料。 CloudSimple 會與您合作，以刪除或傳回客戶合約中提供的所有客戶資料，但適用的法律必須有範圍 CloudSimple，才能保留部分或所有的個人資料。 如果需要保留任何個人資料，CloudSimple 會封存資料並執行合理的量值，以防止客戶資料進行任何進一步的處理。

### <a name="data-location"></a>資料位置

設定私人雲端時，您會選擇要在其中部署的 Azure 區域。 除非您執行資料移轉或異地資料備份，否則不會從該實體資料中心移動 VMware 虛擬機器資料。 如果適合您的需求，您也可以裝載工作負載，並將資料儲存在多個 Azure 區域內。

位於私人雲端超交集節點的客戶資料不會在沒有租使用者系統管理員明確動作的情況下，就不會進行位置。 您必須負責以高可用性的方式來執行工作負載。

### <a name="data-backups"></a>資料備份

CloudSimple 不會備份或封存客戶資料。 CloudSimple 會執行 vCenter 和 NSX 資料的定期備份，以提供管理伺服器的高可用性。 在備份之前，所有資料都會使用 VMware Api 在 vCenter 來源進行加密。 加密的資料會傳輸並儲存在 Azure blob 中。 備份的加密金鑰會儲存在 Azure 中執行于 CloudSimple 虛擬網路的高度安全 CloudSimple 受控保存庫中。

## <a name="network-security"></a>網路安全性

CloudSimple 解決方案依賴網路安全性層級。

### <a name="azure-edge-security"></a>Azure edge 安全性

CloudSimple 服務是建置於 Azure 所提供的基礎網路安全性之上。 Azure 會針對與異常輸入或輸出流量模式以及分散式阻斷服務（DDoS）攻擊相關聯的網路型攻擊，套用深度防禦技術來進行偵測和及時回應。 此安全性控制適用于私用雲端環境，以及 CloudSimple 所開發的控制平面軟體。

### <a name="segmentation"></a>分割

CloudSimple 服務會以邏輯方式區隔第2層網路，在您的私用雲端環境中限制對您自己私人網路的存取。 您可以使用防火牆進一步保護您的私人雲端網路。 CloudSimple 入口網站可讓您針對所有網路流量（包括私用雲端流量、私人雲端流量、網際網路的一般流量，以及透過 IPsec VPN 或 ExpressRoute 連線到內部部署的網路流量），定義更多功能和 NS 網路流量控制規則。

## <a name="vulnerability-and-patch-management"></a>弱點和修補程式管理

CloudSimple 負責定期修補受管理的 VMware 軟體（ESXi、vCenter 和 NSX）。

## <a name="identity-and-access-management"></a>身分識別和存取管理

客戶可以使用多重要素驗證或 SSO，依照偏好向其 Azure 帳戶進行驗證（在 Azure AD 中）。 從 Azure 入口網站，您可以啟動 CloudSimple 入口網站，而不需重新驗證認證。

CloudSimple 支援選擇性設定私人雲端 vCenter 的身分識別來源。 您可以使用內部[部署身分識別來源](set-vcenter-identity.md)、私人雲端的新身分識別來源，或[Azure AD](azure-ad.md)。

根據預設，客戶會獲得私用雲端內 vCenter 日常作業所需的許可權。 此許可權等級不包含 vCenter 的系統管理存取權。 如果暫時需要系統管理存取權，您可以在完成系統管理工作時，在有限的期間內[提升您的許可權](escalate-private-cloud-privileges.md)。
