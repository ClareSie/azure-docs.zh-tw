---
title: Azure 虛擬機器擴展集概觀
description: 了解 Azure 虛擬機器擴展集以及如何自動調整應用程式
author: mimckitt
ms.author: mimckitt
ms.topic: overview
ms.service: virtual-machine-scale-sets
ms.subservice: ''
ms.date: 06/30/2020
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: ec96cab0a147e661df48318aadb057f9f6785b1f
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/07/2020
ms.locfileid: "86026520"
---
# <a name="what-are-virtual-machine-scale-sets"></a>什麼是虛擬機器擴展集？
Azure 虛擬機器擴展集可讓您建立和管理一組負載平衡的 VM。 VM 執行個體的數目可以自動增加或減少，以因應需求或已定義的排程。 擴展集可為您的應用程式提供高可用性，並且可讓您集中管理、設定和更新大量的 VM。 透過虛擬機器擴展集，您將可針對計算、巨量資料和容器工作負載等領域建立大規模的服務。


## <a name="why-use-virtual-machine-scale-sets"></a>為何要使用虛擬機器擴展集？
為了提供備援能力及改善效能，我們常會將應用程式分散於多個執行個體間。 客戶可透過將要求分配給其中一個應用程式執行個體的負載平衡器，來存取您的應用程式。 如果您需要執行維護或更新應用程式執行個體，您的客戶就必須被分配到另一個可用的應用程式執行個體。 為了因應更多客戶需求，您可能需要增加執行應用程式的應用程式執行個體數目。

Azure 虛擬機器擴展集可提供對跨 VM 執行的應用程式進行管理的能力，以及[自動調整資源](virtual-machine-scale-sets-autoscale-overview.md)和流量負載平衡等功能。 擴展集具有以下主要優勢：

- **易於建立和管理多個 VM**
    - 如果您有許多 VM 會執行您的應用程式，則務必要在您的環境間維護一致的組態。 您的應用程式若要有穩定的效能，所有 VM 之間的 VM 大小、磁碟組態和應用程式安裝都必須要一致。
    - 透過擴展集，所有 VM 執行個體都會從相同的基礎 OS 映像和組態建立。 此方法可讓您輕鬆地管理數百個 VM，而不需要執行額外的組態工作或網路管理。
    - 擴展集支援使用 [Azure 負載平衡器](../load-balancer/load-balancer-overview.md)以進行基本的第 4 層流量分配，並可使用 [Azure 應用程式閘道](../application-gateway/application-gateway-introduction.md)以進行進階的第 7 層流量分配和 TLS 終止。

- **提供高可用性和應用程式恢復能力**
    - 擴展集可用來執行多個應用程式執行個體。 如果其中一個 VM 執行個體發生問題時，客戶將可在盡可能不中斷的情況下，透過其他 VM 執行個體繼續存取您的應用程式。
    - 如需更高的可用性，您可以使用[可用性區域](../availability-zones/az-overview.md)，將擴展集中的 VM 執行個體自動散發到單一資料中心內或多個資料中心之間。

- **可讓您的應用程式隨著資源需求的變更自動進行調整**
    - 在一天或一週之中，客戶對您應用程式的需求可能會變動。 為了符合客戶的需求，擴展集可在應用程式需求提高時自動增加 VM 執行個體的數目，並在需求降低時減少 VM 執行個體的數目。
    - 自動調整也可在需求偏低時盡可能減少沒有必要執行應用程式的 VM 執行個體，並且在需求增加時自動新增額外的 VM 執行個體，讓客戶仍能繼續享有一定水準的效能。 這項功能有助於降低成本，並且在必要時有效建立 Azure 資源。

- **大規模運作**
    - 擴展集最多支援可 1,000 個 VM 執行個體。 如果您建立並上傳自己的自訂 VM 映像，則限制為 600 個 VM 執行個體。
    - 若要讓生產工作負載達到最佳效能，請使用 [Azure 受控磁碟](../virtual-machines/windows/managed-disks-overview.md)。


## <a name="differences-between-virtual-machines-and-scale-sets"></a>虛擬機器與擴展集的差異
擴展集是從虛擬機器建立的。 透過擴展集將可提供管理和自動化層，以執行及調整您的應用程式。 您可以改以手動方式建立和管理個別的 VM，或整合現有的工具以建立類似的自動化層級。 下表列出擴展集相較於手動管理多個 VM 執行個體所具備的優點。

| 狀況                           | 手動 VM 群組                                                                    | 虛擬機器擴展集 |
|------------------------------------|----------------------------------------------------------------------------------------|---------------------------|
| 新增其他 VM 執行個體        | 以手動程序建立、設定及確保合規性                             | 從集中的組態自動建立 |
| 流量平衡和分配 | 以手動程序建立及設定 Azure 負載平衡器或應用程式閘道      | 自動建立及整合 Azure 負載平衡器或應用程式閘道 |
| 高可用性與備援能力   | 手動建立可用性設定組，或在各個可用性區域間分配及追蹤 VM | 自動將 VM 執行個體分配到各個可用性區域或可用性設定組 |
| VM 的調整                     | 手動監視和 Azure 自動化                                                 | 根據主機計量、客體計量、Application Insights 或排程進行自動調整 |

擴展集不會產生額外費用。 您只需支付基礎計算資源的費用，例如 VM 執行個體、負載平衡器或受控磁碟儲存體。 管理和自動化功能 (例如自動調整和備援能力) 不會產生使用 VM 的額外費用。

## <a name="how-to-monitor-your-scale-sets"></a>如何監視您的擴展集

使用[適用於 VM 的 Azure 監視器](../azure-monitor/insights/vminsights-overview.md)，此監視其提供簡單的上線程序，且會自動從擴展集內的 VM 收集重要的 CPU、記憶體、磁碟和網路效能計數器。 它也提供其他監視功能和預先定義的視覺效果，可協助您專注於擴展集的可用性和效能。

為您的[虛擬機器擴展集應用程式](../azure-monitor/app/azure-vm-vmss-apps.md)啟用監視功能，讓 Application Insights 收集應用程式的詳細資訊，包括頁面檢視、應用程式要求和例外狀況。 設定[可用性測試](../azure-monitor/app/monitor-web-app-availability.md)來模擬使用者流量，進一步確認您應用程式的可用性。

## <a name="next-steps"></a>後續步驟
若要開始使用，請先在 Azure 入口網站中建立虛擬機器擴展集。

> [!div class="nextstepaction"]
> [在 Azure 入口網站中建立擴展集](quick-create-portal.md)
