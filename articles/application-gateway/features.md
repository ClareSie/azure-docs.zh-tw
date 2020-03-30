---
title: Azure 應用程式閘道功能
description: 瞭解 Azure 應用程式閘道功能
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 03/04/2020
ms.author: victorh
ms.openlocfilehash: 550d9f4f5396b2165260e39cd28222b083dd6756
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279984"
---
# <a name="azure-application-gateway-features"></a>Azure 應用程式閘道功能

[Azure 應用程式閘道](overview.md)是網路流量負載平衡器，可讓您管理 Web 應用程式的流量。

![應用程式閘道概念](media/overview/figure1-720.png)

應用程式閘道包括以下功能：

- [安全通訊端層 (SSL/TLS) 終止](#secure-sockets-layer-ssltls-termination)
- [自動縮放](#autoscaling)
- [區域冗余](#zone-redundancy)
- [靜態 VIP](#static-vip)
- [Web 應用程式防火牆](#web-application-firewall)
- [AKS 的輸入控制器](#ingress-controller-for-aks)
- [URL 型路由](#url-based-routing)
- [多網站裝載](#multiple-site-hosting)
- [重新導向](#redirection)
- [會話相關性](#session-affinity)
- [Websocket 和 HTTP/2 流量](#websocket-and-http2-traffic)
- [連接排水](#connection-draining)
- [自訂錯誤頁面](#custom-error-pages)
- [重新撰寫 HTTP 標頭](#rewrite-http-headers)
- [調整大小](#sizing)

## <a name="secure-sockets-layer-ssltls-termination"></a>安全通訊端層 (SSL/TLS) 終止

應用程式閘道支援在閘道上終止 SSL/TLS，之後流量通常會以未加密狀態流至後端伺服器。 這項功能可讓 Web 伺服器不必再負擔昂貴的加密和解密成本。 但有時與伺服器進行未加密的通信不是可接受的選項。 這可能是因為安全性需求、合規性需求，或應用程式可能只接受安全連線。 對於這些應用程式，應用程式閘道可支援端對端 SSL/TLS 加密。

有關詳細資訊，請參閱[SSL 終止概述以及使用應用程式閘道端到端 SSL 的概述](ssl-overview.md)

## <a name="autoscaling"></a>自動調整

應用程式閘道Standard_v2支援自動縮放，並可根據不斷變化的流量負載模式進行擴展或縮減。 自動調整規模也可讓您在佈建時，無須選擇部署大小或執行個體計數。 

有關應用程式閘道Standard_v2功能的詳細資訊，請參閱[自動縮放 v2 SKU](application-gateway-autoscaling-zone-redundant.md)。

## <a name="zone-redundancy"></a>區域備援

Standard_v2應用程式閘道可以跨多個可用性區域，提供更好的故障恢復能力，並無需在每個區域中預配單獨的應用程式閘道。

## <a name="static-vip"></a>靜態 VIP

SKUStandard_v2應用程式閘道專門支援靜態 VIP 類型。 這可確保與應用程式閘道相關聯的 VIP 即使在應用程式閘道的存留期結束後也不會變更。

## <a name="web-application-firewall"></a>Web 應用程式防火牆

Web 應用程式防火牆 （WAF） 是一種服務，它為您的 Web 應用程式提供集中保護，防止常見的漏洞和漏洞。 WAF 會根據 [OWASP (Open Web Application Security Project) 核心規則集](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) 3.1 (僅限 WAF_v2)、3.0 或 2.2.9 中的規則提供保護。 

Web 應用程式已逐漸成為利用常見已知弱點的惡意攻擊目標。 這些攻擊中最常見的是 SQL 插入式攻擊、跨網站指令碼攻擊等等。 想要防止應用程式的程式碼受到這類攻擊會非常困難，而且可能需要對許多層次的應用程式拓撲執行嚴格的維護、修補和監視工作。 集中式 Web 應用程式防火牆有助於簡化安全性管理作業，且更加確保應用程式管理員能夠對抗威脅或入侵。 相較於保護每個個別的 Web 應用程式，WAF 方案還可透過在中央位置修補已知弱點，更快地因應安全性威脅。 現有應用程式閘道可以輕鬆地轉換為啟用 Web 應用程式防火牆的應用程式閘道。

如需詳細資訊，請參閱[什麼是 Azure Web 應用程式防火牆？](../web-application-firewall/overview.md)。

## <a name="ingress-controller-for-aks"></a>AKS 的輸入控制器
應用程式閘道輸入控制器 (AGIC) 可讓您使用應用程式閘道作為 [Azure Kubernetes Service (AKS)](https://azure.microsoft.com/services/kubernetes-service/) 叢集的輸入。 

入口控制器作為 AKS 群集中的窗格運行，並消耗[Kubernetes 入口資源](https://kubernetes.io/docs/concepts/services-networking/ingress/)並將其轉換為應用程式閘道配置，這允許閘道將流量載入到 Kubernetes pod。 入口控制器僅支援應用程式閘道Standard_v2和WAF_v2 SKU。 

如需詳細資訊，請參閱[應用程式閘道輸入控制器 (AGIC)](ingress-controller-overview.md)。

## <a name="url-based-routing"></a>URL 型路由

URL 路徑型路由可讓您根據要求的 URL 路徑，將流量路由傳送至後端伺服器集區。 有一個案例是將對於不同內容類型的要求路由傳送至不同的集區。

例如，對 `http://contoso.com/video/*` 的要求會路由傳送至 VideoServerPool，而對 `http://contoso.com/images/*` 的要求則會路由傳送至 ImageServerPool。 如果沒有任何路徑模式相符，則會選取 DefaultServerPool。

有關詳細資訊，請參閱基於[URL 路徑的路由概述](url-route-overview.md)。

## <a name="multiple-site-hosting"></a>多網站裝載

多網站裝載可讓您在相同的應用程式閘道執行個體上設定多個網站。 此功能允許您通過將多達 100 個網站添加到一個應用程式閘道（以實現最佳性能）來為部署配置更高效的拓撲。 每個網站都可以導向到自己的集區。 例如，應用程式閘道可以從兩個伺服器集區 (名為 ContosoServerPool 和 FabrikamServerPool) 為 `contoso.com` 和 `fabrikam.com` 處理流量。

對 `http://contoso.com` 的要求會路由傳送至 ContosoServerPool，而對 `http://fabrikam.com` 的要求則會路由傳送至 FabrikamServerPool。

同樣地，相同父系網域的兩個子網域也可以裝載在相同的應用程式閘道部署上。 使用子網域的範例可能包括單一應用程式閘道部署上裝載的 `http://blog.contoso.com` 和 `http://app.contoso.com`。

有關詳細資訊，請參閱[應用程式閘道多個網站託管](multiple-site-overview.md)。

## <a name="redirection"></a>重新導向

許多 Web 應用程式的常見案例是支援自動 HTTP 至 HTTPS 的重新導向，以確保應用程式與其使用者之間的通訊會透過加密的路徑進行。

在過去，您會使用一些技巧 (例如建立專屬集區)，其唯一目的是要將其在 HTTP 上接收的要求重新導向至 HTTPS。 應用程式閘道支援在應用程式閘道上重新導向流量的功能。 這可簡化應用程式組態、將資源使用量最佳化，並支援新的重新導向案例，包括全域和路徑式重新導向。 應用程式閘道重新導向支援不僅止於 HTTP 至 HTTPS 的重新導向。 這是一般重新導向機制，因此您可以從使用規則定義的任何連接埠重新導向，或是重新導向至使用規則定義的任何連接埠。 它也支援重新導向至外部網站。

應用程式閘道重新導向提供下列功能：

- 從閘道上的一個連接埠到另一個連接埠的全域重新導向。 這允許在網站上進行 HTTP 至 HTTPS 重新導向。
- 路徑式重新導向。 這類型的重新導向只允許在特定網站區域上進行 HTTP 至 HTTPS 重新導向，例如以 `/cart/*` 表示的購物車區域。
- 重新導向至外部網站。

有關詳細資訊，請參閱[應用程式閘道重定向概述](redirect-overview.md)。

## <a name="session-affinity"></a>工作階段親和性

當您想要在同一個後端保留使用者工作階段時，以 Cookie 為基礎的工作階段親和性非常有用。 使用受閘道管理的 Cookie，應用程式閘道即可將來自使用者工作階段的後續流量導向至同一部伺服器進行處理。 當使用者工作階段的工作階段狀態儲存在伺服器本機時，這項功能很重要。

有關詳細資訊，請參閱[應用程式閘道的工作原理](how-application-gateway-works.md#modifications-to-the-request)。

## <a name="websocket-and-http2-traffic"></a>Websocket 和 HTTP/2 流量

應用程式閘道可對 WebSocket 和 HTTP/2 通訊協定提供原生支援。 使用者無法進行設定來選擇要啟用或停用 WebSocket 支援。

WebSocket 和 HTTP/2 通訊協定都可透過長時間執行的 TCP 連線，讓伺服器與用戶端之間能進行全雙工通訊。 此功能可讓網頁伺服器和用戶端之間進行互動性更高的通訊，此通訊可以是雙向的，而不需要像 HTTP 型實作所要求的進行輪詢。 這些協定具有較低的開銷，與 HTTP 不同，它可以為多個請求/回應重用相同的 TCP 連接，從而更有效地利用資源。 這些通訊協定設計為透過傳統 HTTP 連接埠 80 和 443 進行運作。

如需詳細資訊，請參閱 [WebSocket 支援](application-gateway-websocket.md) \(機器翻譯\) 與 [HTTP/2 支援](configuration-overview.md#http2-support) \(機器翻譯\)。

## <a name="connection-draining"></a>清空連線

清空連線可協助您在已規劃的服務更新期間，毫無錯誤地移除後端集區成員。 此設定會透過後端 http 設定啟用，而且可以在規則建立期間套用至後端集區的所有成員。 啟用後，應用程式閘道可確保後端池的所有登出實例不會收到任何新請求，同時允許現有請求在配置的時間限制內完成。 這適用於透過使用者設定變更從後端集區中確實移除的後端執行個體，以及根據健康情況探查的判斷，而回報為狀況不良的後端執行個體。 唯一的例外是綁定用於取消註冊實例的請求，這些請求已顯式取消註冊，因為閘道託管的會話關聯性並繼續接近取消註冊實例。

有關詳細資訊，請參閱[應用程式閘道配置概述](configuration-overview.md#connection-draining)。

## <a name="custom-error-pages"></a>自訂錯誤頁面

應用程式閘道可讓您建立自訂的錯誤頁面，而不是顯示預設的錯誤頁面。 您可以使用自訂錯誤頁面來搭配您自己的商標和版面配置。

如需相關資訊，請參閱[自訂錯誤](custom-error.md)。

## <a name="rewrite-http-headers"></a>重新撰寫 HTTP 標頭

HTTP 標頭允許用戶端和伺服器透過要求或回應傳遞其他資訊。 重寫這些 HTTP 標頭可協助您完成幾個重要的案例，例如：

- 新增安全性相關的標頭欄位，例如 HSTS/ X-XSS-Protection。
- 移除可能會顯示機密資訊的回應標頭欄位。
- 從 X-Forwarded-For 標頭中移除連接埠資訊。

Application Gateway 支援在要求及回應封包於用戶端與後端應用程式之間移動時，新增、移除或更新 HTTP 要求及回應標頭的功能。 它還為您提供了新增條件的功能，以確保僅在符合特定條件時才重新寫入指定的標頭。

如需詳細資訊，請參閱[重新撰寫 HTTP 標頭](rewrite-http-headers.md)。

## <a name="sizing"></a>調整大小

應用程式閘道Standard_v2可以配置為自動縮放或固定大小部署。 此 SKU 不提供不同的實例大小。 如需 v2 效能和定價的詳細資訊，請參閱[自動調整 v2 SKU](application-gateway-autoscaling-zone-redundant.md#pricing)。

應用程式閘道標準有三種尺寸：**小**、**中**、**大**。 小型執行個體大小是針對開發和測試案例。

如需應用程式閘道限制的完整清單，請瀏覽[應用程式閘道服務限制](../azure-resource-manager/management/azure-subscription-service-limits.md?toc=%2fazure%2fapplication-gateway%2ftoc.json#application-gateway-limits)。

下表顯示每個應用程式閘道 v1 執行個體，在啟用 SSL 卸載時的平均效能輸送量：

| 平均後端頁面回應大小 | 小型 | 中 | 大型 |
| --- | --- | --- | --- |
| 6 KB |7.5 Mbps |13 Mbps |50 Mbps |
| 100 KB |35 Mbps |100 Mbps |200 Mbps |

> [!NOTE]
> 這些值是應用程式閘道輸送量的近似值。 實際的輸送量會依據不同的環境詳細資料而有所不同，例如平均頁面大小、後端執行個體位置，以及提供一個頁面所需的處理時間。 如需實際效能數字，您需自行執行測試。 這些值僅供容量規劃指引使用。

## <a name="next-steps"></a>後續步驟

- 瞭解應用程式閘道的工作原理 -[應用程式閘道的工作原理](how-application-gateway-works.md)