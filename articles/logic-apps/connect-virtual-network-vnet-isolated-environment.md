---
title: 使用 ISE 連接到 Azure 虛擬網路
description: 建立可從 Azure Logic Apps 存取 Azure 虛擬網路（Vnet）的整合服務環境（ISE）
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: conceptual
ms.date: 03/12/2020
ms.openlocfilehash: fa63380a8e27dcc8f4de414c483f8d8ed2323e7b
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82234108"
---
# <a name="connect-to-azure-virtual-networks-from-azure-logic-apps-by-using-an-integration-service-environment-ise"></a>透過使用整合服務環境 (ISE) 從 Azure Logic Apps 連線至 Azure 虛擬網路

對於您的邏輯應用程式和整合帳戶需要存取[Azure 虛擬網路](../virtual-network/virtual-networks-overview.md)的案例，請建立[*整合服務環境*（ISE）](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)。 ISE 是一種隔離的環境，會使用專用儲存體和其他與「全域」多租使用者 Logic Apps 服務分開的資源。 這種區隔也可減少任何其他 Azure 租用戶可能對您應用程式效能造成的影響。 ISE 也會為您提供自己的靜態 IP 位址。 這些 IP 位址與公用、多租使用者服務中的邏輯應用程式所共用的靜態 IP 位址不同。

當您建立 ISE 時，Azure 會將 ISE 插入*您的 azure*虛擬網路，然後將 Logic Apps 服務部署到您的虛擬網路。 當您建立邏輯應用程式或整合帳戶時，請選取您的 ISE 作為其位置。 然後，您的邏輯應用程式或整合帳戶就可以直接存取虛擬網路中的資源，例如：虛擬機器 (VM)、伺服器、系統及服務。

![選取整合服務環境](./media/connect-virtual-network-vnet-isolated-environment/select-logic-app-integration-service-environment.png)

> [!IMPORTANT]
> 若要讓邏輯應用程式和整合帳戶在 ISE 中一起工作，兩者都必須使用與其位置*相同的 ISE* 。

ISE 已增加執行持續時間、儲存體保留期、輸送量、HTTP 要求和回應超時、訊息大小，以及自訂連接器要求的限制。 如需詳細資訊，請參閱[Azure Logic Apps 的限制和](../logic-apps/logic-apps-limits-and-config.md)設定。 若要深入瞭解 Ise，請參閱[Azure Logic Apps 的 Azure 虛擬網路資源存取權](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md)。

本文說明如何使用 Azure 入口網站來完成這些工作：

* 啟用 ISE 的存取權。
* 建立您的 ISE。
* 將額外的容量新增至您的 ISE。

您也可以使用[範例 Azure Resource Manager 快速入門範本](https://github.com/Azure/azure-quickstart-templates/tree/master/201-integration-service-environment)，或使用 Logic Apps REST API 來建立 ISE，包括設定客戶管理的金鑰：

* [使用 Logic Apps 建立整合服務環境（ISE） REST API](../logic-apps/create-integration-service-environment-rest-api.md)
* [設定客戶管理的金鑰，以加密 Ise 的待用資料](../logic-apps/customer-managed-keys-integration-service-environment.md)

## <a name="prerequisites"></a>Prerequisites

* Azure 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請先[註冊免費的 Azure 帳戶](https://azure.microsoft.com/free/)。

  > [!IMPORTANT]
  > 邏輯應用程式、內建觸發程式、內建動作，以及在您 ISE 中執行的連接器會使用與以耗用量為基礎的定價方案不同的定價方案。 若要瞭解 Ise 的定價和計費方式，請參閱[Logic Apps 定價模式](../logic-apps/logic-apps-pricing.md#fixed-pricing)。 如需定價費率，請參閱[Logic Apps 定價](../logic-apps/logic-apps-pricing.md)。

* [Azure 虛擬網路](../virtual-network/virtual-networks-overview.md)。 如果您沒有虛擬網路，請了解如何[建立 Azure 虛擬網路](../virtual-network/quick-create-portal.md)。

  * 您的虛擬網路必須有四個*空白*子網，才能在 ISE 中建立及部署資源。 每個子網都支援在您的 ISE 中使用不同的 Logic Apps 元件。 您可以事先建立這些子網，也可以等到建立 ISE，讓您可以同時建立子網。 深入瞭解[子網需求](#create-subnet)。

  * 子網名稱的開頭必須是字母字元或底線，而且不能使用下列字元： `<`、 `>`、 `%`、 `&`、 `\\`、 `?`、 `/`。 
  
  * 如果您想要透過 Azure Resource Manager 範本部署 ISE，請先確定您將一個空白子網委派給 Microsoft. 邏輯/integrationServiceEnvironment。 當您透過 Azure 入口網站部署時，不需要執行此委派。

  * 請確定您的虛擬網路[可存取您的 ise](#enable-access) ，讓您的 ise 能夠正常運作並保持可存取。

  * 如果您使用[ExpressRoute](../expressroute/expressroute-introduction.md)來提供連線提供者所能促成的 Microsoft 雲端服務私人連線，您必須建立具有下列路由的[路由表](../virtual-network/manage-route-table.md)，並將該資料表連結到 ISE 所使用的每個子網：

    **名稱**： <*路由名稱*><br>
    **位址首碼**： 0.0.0.0/0<br>
    **下一個躍點**：網際網路

* 如果您想要針對您的 Azure 虛擬網路使用自訂 DNS 伺服器，請在將 ISE 部署至虛擬網路之前，[遵循下列步驟來設定這些伺服器](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)。 如需管理 DNS 伺服器設定的詳細資訊，請參閱[建立、變更或刪除虛擬網路](../virtual-network/manage-virtual-network.md#change-dns-servers)。

  > [!NOTE]
  > 如果您變更 DNS 伺服器或 DNS 伺服器設定，就必須重新開機 ISE，讓 ISE 能夠收取這些變更。 如需詳細資訊，請參閱[重新開機 ISE](../logic-apps/ise-manage-integration-service-environment.md#restart-ISE)。

<a name="enable-access"></a>

## <a name="enable-access-for-ise"></a>啟用 ISE 的存取

當您搭配 Azure 虛擬網路使用 ISE 時，常見的設定問題是有一或多個已封鎖的埠。 您用來在 ISE 與目的地系統之間建立連線的連接器，可能也會有自己的埠需求。 例如，如果您使用 FTP 連接器與 FTP 系統通訊，就必須提供您在 FTP 系統上使用的埠，例如，用於傳送命令的埠21。

為確保您的 ISE 可供存取，且該 ISE 中的邏輯應用程式可以跨虛擬網路中的每個子網進行通訊，請[為每個子網開啟此表格中所述的埠](#network-ports-for-ise)。 如果有任何必要的埠無法使用，您的 ISE 將無法正確運作。

* 如果您有多個 ISE 實例需要存取具有 IP 限制的其他端點，請將[Azure 防火牆](../firewall/overview.md)或[網路虛擬裝置](../virtual-network/virtual-networks-overview.md#filter-network-traffic)部署至您的虛擬網路，並透過該防火牆或網路虛擬裝置來路由輸出流量。 接著，您可以[設定單一、輸出、公用、靜態和可預測的 IP 位址](connect-virtual-network-vnet-set-up-single-ip-address.md)，讓虛擬網路中的所有 ISE 實例可以用來與目的地系統進行通訊。 如此一來，您就不需要在每個 ISE 的目的地系統上設定額外的防火牆。

   > [!NOTE]
   > 當您的案例需要限制需要存取的 IP 位址數目時，您可以將此方法用於單一 ISE。 請考慮防火牆或虛擬網路設備的額外成本對您的案例是否有意義。 深入瞭解[Azure 防火牆定價](https://azure.microsoft.com/pricing/details/azure-firewall/)。

* 如果您建立新的 Azure 虛擬網路和子網，但沒有任何限制，您就不需要在虛擬網路中設定[網路安全性群組（nsg）](../virtual-network/security-overview.md#network-security-groups)來控制子網之間的流量。

* 在現有的虛擬網路上，您可以藉由[篩選跨子網的網路流量](../virtual-network/tutorial-filter-network-traffic.md)，*選擇性地*設定 nsg。 如果您想要前往此路由，或如果您已經在使用 Nsg，請確定您在已 Nsg 或想要設定 Nsg 的虛擬網路上，[開啟此資料表中的埠](#network-ports-for-ise)。

  > [!NOTE]
  > 如果您使用[NSG 安全性規則](../virtual-network/security-overview.md#security-rules)，則必須*同時*使用 TCP 和 UDP 通訊協定。 NSG 安全性規則會描述您必須針對需要存取這些埠的 IP 位址開啟的埠。 請確定在這些端點之間存在的任何防火牆、路由器或其他專案，也會讓這些埠可以存取這些 IP 位址。

<a name="network-ports-for-ise"></a>

### <a name="network-ports-used-by-your-ise"></a>ISE 使用的網路埠

下表描述您的 ISE 使用的 Azure 虛擬網路中的埠，以及這些埠的使用位置。 為了協助降低建立安全性規則時的複雜性，表格中的[服務標記](../virtual-network/service-tags-overview.md)代表特定 Azure 服務的 IP 位址首碼群組。

> [!IMPORTANT]
> 來源埠是暫時的，因此請確定您已將所有`*`規則都設定為。 請注意，內部 ISE 和外部 ISE[會參考在 ISE 建立時選取的端點](connect-virtual-network-vnet-isolated-environment.md#create-environment)。 如需詳細資訊，請參閱[端點存取](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access)。 

| 目的 | 方向 | 目的地連接埠 | 來源服務標籤 | 目的地服務標記 | 備忘錄 |
|---------|-----------|-------------------|--------------------|-------------------------|-------|
| 虛擬網路內的 Intersubnet 通訊 | 輸入和輸出 | * | 具有 ISE 子網之虛擬網路的位址空間 | 具有 ISE 子網之虛擬網路的位址空間 | 在虛擬網路中的子網*之間*流動的流量所需。 <p><p>**重要**事項：若要在每個子網中的*元件*之間流動流量，請確定您已開啟每個子網內的所有埠。 |
| 與邏輯應用程式的通訊 | 輸入 | 443 | 內部 ISE： <br>VirtualNetwork <p><p>外部 ISE： <br>Internet <br>（請參閱**附注**資料行） | VirtualNetwork | 您可以不使用**網際網路**服務標籤來指定電腦或服務的來源 IP 位址，以呼叫邏輯應用程式中的任何要求觸發程式或 webhook。 <p><p>**重要**：關閉或封鎖此埠可防止 HTTP 呼叫具有要求觸發程式的邏輯應用程式。 |
| 邏輯應用程式執行歷程記錄 | 輸入 | 443 | 內部 ISE： <br>VirtualNetwork <p><p>外部 ISE： <br>Internet <br>（請參閱**附注**資料行） | VirtualNetwork | 您可以指定您想要在其中查看邏輯應用程式執行歷程記錄的電腦或服務的來源 IP 位址，而不是使用 [**網際網路**服務] 標籤。 <p><p>**重要**：雖然關閉或封鎖此埠並不會讓您無法查看執行歷程記錄，但您無法在該執行歷程記錄中查看每個步驟的輸入和輸出。 |
| Logic Apps 設計工具 - 動態屬性 | 輸入 | 454 | LogicAppsManagement | VirtualNetwork | 要求來自該區域的 Logic Apps 存取端點[輸入](../logic-apps/logic-apps-limits-and-config.md#inbound)IP 位址。 |
| 連接器部署 | 輸入 | 454 | AzureConnectors | VirtualNetwork | 部署和更新連接器時的必要。 關閉或封鎖此埠會導致 ISE 部署失敗，並防止連接器更新或修正。 |
| 網路健全狀況檢查 | 輸入 | 454 | LogicApps | VirtualNetwork | 要求來自該區域的[輸入](../logic-apps/logic-apps-limits-and-config.md#inbound)和[輸出](../logic-apps/logic-apps-limits-and-config.md#outbound)IP 位址的 Logic Apps 存取端點。 |
| App Service 管理相依性 | 輸入 | 454、455 | AppServiceManagement | VirtualNetwork | |
| 從 Azure 流量管理員的通訊 | 輸入 | 內部 ISE：454 <p><p>外部 ISE：443 | AzureTrafficManager | VirtualNetwork | |
| API 管理 - 管理端點 | 輸入 | 3443 | APIManagement | VirtualNetwork | |
| 連接器原則部署 | 輸入 | 3443 | APIManagement | VirtualNetwork | 部署和更新連接器時的必要。 關閉或封鎖此埠會導致 ISE 部署失敗，並防止連接器更新或修正。 |
| 從邏輯應用程式進行通訊 | 輸出 | 80、443 | VirtualNetwork | 根據目的地而有所不同 | 邏輯應用程式需要與其通訊之外部服務的端點。 |
| Azure Active Directory | 輸出 | 80、443 | VirtualNetwork | AzureActiveDirectory | |
| 連線管理 | 輸出 | 443 | VirtualNetwork  | AppService | |
| 發佈診斷記錄和計量 | 輸出 | 443 | VirtualNetwork  | AzureMonitor | |
| Azure 儲存體相依性 | 輸出 | 80、443、445 | VirtualNetwork | 儲存體 | |
| Azure SQL 相依性 | 輸出 | 1433 | VirtualNetwork | SQL | |
| Azure 資源健康狀態 | 輸出 | 1886 | VirtualNetwork | AzureMonitor | 將健全狀況狀態發佈至資源健康狀態時的必要 |
| 「記錄到事件中樞」原則和監視代理程式的相依性 | 輸出 | 5672 | VirtualNetwork | EventHub | |
| 針對角色執行個體之間的 Redis 執行個體存取 Azure 快取 | 輸入 <br>輸出 | 6379-6383 | VirtualNetwork | VirtualNetwork | 此外，若要讓 ISE 使用 Azure Cache for Redis，您必須開啟[Azure cache For REDIS 常見問題中所述的這些輸出和輸入埠](../azure-cache-for-redis/cache-how-to-premium-vnet.md#outbound-port-requirements)。 |
||||||

<a name="create-environment"></a>

## <a name="create-your-ise"></a>建立您的 ISE

1. 在 [ [Azure 入口網站](https://portal.azure.com)的主要 Azure 搜尋方塊中，輸入`integration service environments`作為篩選準則，然後選取 [**整合服務環境**]。

   ![尋找並選取 [整合服務環境]](./media/connect-virtual-network-vnet-isolated-environment/find-integration-service-environment.png)

1. 在 [**整合服務環境**] 窗格上，選取 [**新增**]。

   ![尋找並選取 [整合服務環境]](./media/connect-virtual-network-vnet-isolated-environment/add-integration-service-environment.png)

1. 為您的環境提供這些詳細資料，然後選取 [審核] [ **+ 建立**]，例如：

   ![提供環境詳細資料](./media/connect-virtual-network-vnet-isolated-environment/integration-service-environment-details.png)

   | 屬性 | 必要 | 值 | 描述 |
   |----------|----------|-------|-------------|
   | **訂用帳戶** | 是 | <*Azure-訂用帳戶-名稱*> | 要用於環境的 Azure 訂用帳戶 |
   | **資源群組** | 是 | <*Azure-資源群組-名稱*> | 新的或現有的 Azure 資源群組，您想要在其中建立您的環境 |
   | **整合服務環境名稱** | 是 | <*環境名稱*> | 您的 ISE 名稱，其中只能包含字母、數位、連字號`-`（）、底線`_`（）和句點（`.`）。 |
   | **位置** | 是 | <*Azure-datacenter-地區*> | 要用來部署環境的 Azure 資料中心區域 |
   | **SKU** | 是 | **Premium**或**DEVELOPER （無 SLA）** | 要建立及使用的 ISE SKU。 如需這些 Sku 之間的差異，請參閱[ISE sku](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#ise-level)。 <p><p>**重要**事項：只有在 ISE 建立時才可使用此選項，且稍後無法變更。 |
   | **額外容量** | 進階： <br>是 <p><p>員 <br>不適用 | 進階： <br>0到10 <p><p>員 <br>不適用 | 要用於此 ISE 資源的額外處理單位數。 若要在建立後新增容量，請參閱[新增 ISE 容量](../logic-apps/ise-manage-integration-service-environment.md#add-capacity)。 |
   | **存取端點** | 是 | **內部**或**外部** | 要用於 ISE 的存取端點類型。 這些端點會判斷 ISE 中的邏輯應用程式上的要求或 webhook 觸發程式是否可以接收來自您虛擬網路外部的呼叫。 <p><p>您的選擇也會影響您可以在邏輯應用程式執行歷程記錄中查看和存取輸入和輸出的方式。 如需詳細資訊，請參閱[ISE 端點存取](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md#endpoint-access)。 <p><p>**重要**事項：只有在 ISE 建立時才可使用此選項，且稍後無法變更。 |
   | **虛擬網路** | 是 | <*Azure-虛擬網路-名稱*> | 要插入環境的 Azure 虛擬網路，讓該環境中的邏輯應用程式可以存取虛擬網路。 如果您沒有網路，請[先建立 Azure 虛擬網路](../virtual-network/quick-create-portal.md)。 <p><p>**重要**：您*只能*在建立 ISE 時執行此插入。 |
   | **子網路** | 是 | <*子網-資源清單*> | ISE 需要四個*空*的子網，才能在您的環境中建立及部署資源。 若要建立每個子網路，[請遵循此表格底下的步驟](#create-subnet)。 |
   |||||

   <a name="create-subnet"></a>

   **建立子網路**

   若要在您的環境中建立及部署資源，您的 ISE 需要四個未委派給任何服務的*空白*子網。 每個子網都支援在您的 ISE 中使用不同的 Logic Apps 元件。 建立環境之後，您就*無法*變更這些子網位址。 每個子網都必須符合下列需求：

   * 具有以字母字元或底線（沒有數位）開頭的名稱，而且不會使用下列`<`字元：、 `>`、 `%`、 `&`、 `\\`、 `?`、。 `/`

   * 使用無[類別網域間路由（CIDR）格式](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)和類別 B 位址空間。

   * 會使用`/27`位址空間中的，因為每個子網都需要32位址。 例如，有`10.0.0.0/27` 32 個位址，因為 2<sup>（32-27）</sup>是 2<sup>5</sup>或32。 有更多的位址不會提供額外的權益。  若要深入瞭解如何計算位址，請參閱[IPV4 CIDR 區塊](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#IPv4_CIDR_blocks)。

   * 如果您使用[ExpressRoute](../expressroute/expressroute-introduction.md)，您必須建立具有下列路由的[路由表](../virtual-network/manage-route-table.md)，並連結該資料表與您的 ISE 所使用的每個子網：

     **名稱**： <*路由名稱*><br>
     **位址首碼**： 0.0.0.0/0<br>
     **下一個躍點**：網際網路

   1. 在 [**子網**] 清單下，選取 [**管理子網**設定]。

      ![管理子網路設定](./media/connect-virtual-network-vnet-isolated-environment/manage-subnet-configuration.png)

   1. 在 [**子網**] 窗格中，選取 [**子網**]。

      ![新增四個空白子網](./media/connect-virtual-network-vnet-isolated-environment/add-empty-subnets.png)

   1. 在 [新增子網路]**** 窗格中，提供這項資訊。

      * **名稱**：子網的名稱
      * **位址範圍（CIDR 區塊）**：您的虛擬網路中的子網範圍和 CIDR 格式

      ![新增子網路詳細資料](./media/connect-virtual-network-vnet-isolated-environment/provide-subnet-details.png)

   1. 完成後，選取 [確定]  。

   1. 針對其他三個子網路重複這些步驟。

      > [!NOTE]
      > 如果您嘗試建立的子網無效，Azure 入口網站會顯示一則訊息，但不會封鎖您的進度。

   如需有關建立子網的詳細資訊，請參閱[新增虛擬網路子網](../virtual-network/virtual-network-manage-subnet.md)。

1. 在 Azure 成功驗證您的 ISE 資訊之後，請選取 [**建立**]，例如：

   ![驗證成功之後，請選取 [建立]](./media/connect-virtual-network-vnet-isolated-environment/ise-validation-success.png)

   Azure 會開始部署您的環境，這通常會在兩個小時內完成。 有時候，部署可能需要多達四小時。 若要檢查部署狀態，請在 Azure 工具列上選取 [通知] 圖示，這會開啟 [通知] 窗格。

   ![檢查部署狀態](./media/connect-virtual-network-vnet-isolated-environment/environment-deployment-status.png)

   成功完成部署時，Azure 就會顯示此通知：

   ![部署成功](./media/connect-virtual-network-vnet-isolated-environment/deployment-success-message.png)

   否則，請遵循針對部署進行疑難排解的 Azure 入口網站指示。

   > [!NOTE]
   > 如果部署失敗或您刪除了 ISE，Azure「可能」需要最多一小時的時間來釋出您的子網路。 這種延遲表示您可能必須等待，才能在另一個 ISE 中重複使用這些子網。
   >
   > 如果您刪除虛擬網路，Azure 通常需要兩小時的時間，才能釋出您的子網，但此作業可能需要較長的時間。 
   > 刪除虛擬網路時，請確定沒有任何資源仍處於線上狀態。 
   > 請參閱[刪除虛擬網路](../virtual-network/manage-virtual-network.md#delete-a-virtual-network)。

1. 若要查看您的環境，請選取 [**移至資源**] （如果 Azure 在部署完成後不會自動移至您的環境）。

1. 若要檢查 ISE 的網路健康情況，請參閱[管理您的整合服務環境](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)。

1. 若要開始在 ISE 中建立邏輯應用程式和其他構件，請參閱[將資源新增至整合服務環境](../logic-apps/add-artifacts-integration-service-environment-ise.md)。

   > [!IMPORTANT]
   > 您在建立 ISE 之後變成可用的受控 ISE 連接器不會自動出現在邏輯應用程式設計工具的連接器選擇器中。 在您可以使用這些 ISE 連接器之前，您必須手動[將這些連接器新增至您的 ISE](../logic-apps/add-artifacts-integration-service-environment-ise.md#add-ise-connectors-environment) ，使其出現在邏輯應用程式設計工具中。

## <a name="next-steps"></a>後續步驟

* [將資源新增到整合服務環境](../logic-apps/add-artifacts-integration-service-environment-ise.md)
* [管理整合服務環境](../logic-apps/ise-manage-integration-service-environment.md#check-network-health)
* 深入了解 [Azure 虛擬網路](../virtual-network/virtual-networks-overview.md)
* 了解 [Azure 服務的虛擬網路整合](../virtual-network/virtual-network-for-azure-services.md)
