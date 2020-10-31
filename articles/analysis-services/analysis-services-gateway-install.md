---
title: 安裝適用于 Azure Analysis Services 的內部部署資料閘道 |Microsoft Docs
description: 瞭解如何安裝和設定內部部署資料閘道，以從 Azure Analysis Services server 連接到內部部署資料來源。
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/29/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 64bd9e4a4cf78d2628e946af30c2d290ff002cf7
ms.sourcegitcommit: 3bdeb546890a740384a8ef383cf915e84bd7e91e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2020
ms.locfileid: "93081139"
---
# <a name="install-and-configure-an-on-premises-data-gateway"></a>安裝及設定內部部署資料閘道

如果相同區域中有一或多部 Analysis Services 伺服器連線到內部部署資料來源，則需要一個內部部署閘道。  雖然您安裝的閘道與其他服務（例如 Power BI、Power Apps 和 Logic Apps）所使用的相同，但在安裝 Azure Analysis Services 時，還需要完成一些額外的步驟。 這是 **Azure Analysis Services** 專屬的安裝文章。 

若要深入瞭解 Azure Analysis Services 如何與閘道搭配運作，請參閱 [連接至內部部署資料來源](analysis-services-gateway.md)。 若要深入瞭解 advanced 安裝案例和閘道的一般資訊，請參閱內部 [部署資料閘道檔](/data-integration/gateway/service-gateway-onprem)。

## <a name="prerequisites"></a>Prerequisites

**最低需求：**

* .NET 4.5 Framework
* 64位版本的 Windows 8/Windows Server 2012 R2 (或更新版本) 

**建議配備：**

* 8 核心 CPU
* 8 GB 記憶體
* 64位版本的 Windows 8/Windows Server 2012 R2 (或更新版本) 

**重要考慮事項：**

* 在設定期間，向 Azure 註冊您的閘道時，系統會選取您訂用帳戶的預設區域。 您可以選擇不同的訂用帳戶和區域。 如果您有伺服器位於多個區域中，您必須針對每個區域安裝一個閘道。 
* 閘道無法安裝在網域控制站上。
* 一部電腦只能安裝一個閘道。
* 請在電源維持開啟且不會進入睡眠狀態的電腦上安裝閘道。
* 請勿在具有無線網路連線的電腦上安裝閘道。 效能會因此降低。
* 安裝閘道時，您用來登入電腦的使用者帳戶必須具有「登入為服務」權限。 安裝完成時，內部部署資料閘道服務會使用 NT SERVICE\PBIEgwService 帳戶登入為服務。 在安裝期間可以指定不同的帳戶，或是可在安裝完成後於服務中指定。 在安裝時請確定群組原則設定允許您用於登入的帳戶，且您選擇的服務帳戶具有「登入為服務」權限。
* 如果[租用戶](/previous-versions/azure/azure-services/jj573650(v=azure.100)#what-is-an-azure-ad-tenant)與您要註冊閘道的訂用帳戶相同，請以 Azure AD 中的帳戶登入 Azure。 安裝和註冊閘道時不支援 Azure B2B (來賓) 帳戶。
* 如果資料來源位於 Azure 虛擬網路 (VNet) 上，您必須設定 [AlwaysUseGateway](analysis-services-vnet-gateway.md) 伺服器屬性。

## <a name="download"></a>下載

 [下載閘道](https://go.microsoft.com/fwlink/?LinkId=820925&clcid=0x409)

## <a name="install"></a>安裝

1. 執行安裝程式。

2. 選取內部 **部署資料閘道** 。

   ![選取](media/analysis-services-gateway-install/aas-gateway-installer-select.png)

2. 選取位置，接受條款，然後按一下 [安裝]  。

   ![安裝位置和授權條款](media/analysis-services-gateway-install/aas-gateway-installer-accept.png)

3. 登入 Azure。 此帳戶必須位於租用戶的 Azure Active Directory 中。 此帳戶用於閘道管理員。 安裝和註冊閘道時不支援 Azure B2B (來賓) 帳戶。

   ![登入 Azure](media/analysis-services-gateway-install/aas-gateway-installer-account.png)

   > [!NOTE]
   > 如果您使用網域帳戶來登入，該帳戶會對應至 Azure AD 中的組織帳戶。 您的組織帳戶會當作閘道管理員。

## <a name="register"></a>註冊

若要在 Azure 中建立閘道資源，您必須向閘道雲端服務註冊您安裝的本機執行個體。 

1.  選取 [在這部電腦上註冊新的閘道]  。

    ![醒目顯示 [在這部電腦上註冊新閘道] 選項的螢幕擷取畫面。](media/analysis-services-gateway-install/aas-gateway-register-new.png)

2. 輸入您的閘道名稱和復原金鑰。 根據預設，閘道會使用您訂用帳戶的預設區域。 如果您需要選取不同的區域，請選取 [變更區域]  。

    > [!IMPORTANT]
    > 將您的修復金鑰儲存在安全的地方。 必須要有修復金鑰，才能接管、移轉或還原閘道。 

   ![註冊](media/analysis-services-gateway-install/aas-gateway-register-name.png)


## <a name="create-an-azure-gateway-resource"></a>建立 Azure 閘道資源

在您安裝並註冊閘道之後，您需要在 Azure 中建立閘道資源。 使用您用於註冊閘道的相同帳戶登入 Azure。

1. 在 Azure 入口網站中，按一下 [ **建立資源** ]，然後搜尋 **內部部署資料閘道** ，然後按一下 [ **建立** ]。

   ![建立閘道資源](media/analysis-services-gateway-install/aas-gateway-new-azure-resource.png)

2. 在 [建立連線閘道]  中，輸入下列設定：

   * **名稱** ：輸入閘道資源的名稱。 

   * **訂用帳戶** ︰選取要與閘道資源關聯的 Azure 訂用帳戶。 
   
     預設的訂用帳戶會由您用來登入的 Azure 帳戶來決定。

   * **資源群組** ：建立資源群組，或選取現有的資源群組。

   * **位置** ：選取您註冊閘道的區域。

   * **安裝名稱** ：如果尚未選取您的閘道安裝，請選取您在電腦上安裝並註冊的閘道。 

     完成之後，請按一下 [建立]  。

## <a name="connect-gateway-resource-to-server"></a>將閘道資源連線到伺服器

> [!NOTE]
> 入口網站不支援從伺服器連線至不同訂用帳戶中的閘道資源，但使用 PowerShell 支援此功能。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

1. 在 Azure Analysis Services 伺服器概觀中，按一下 [內部部署資料閘道]  。

   ![將伺服器連線至閘道](media/analysis-services-gateway-install/aas-gateway-connect-server.png)

2. 在 [挑選要連線的內部部署資料閘道]  中，選取您的閘道資源，然後按一下 [連線選取的閘道]  。

   ![將伺服器連線至閘道資源](media/analysis-services-gateway-install/aas-gateway-connect-resource.png)

    > [!NOTE]
    > 如果您的閘道並未出現在清單中，您的伺服器可能不在註冊閘道時所指定的相同區域中。

    當您的伺服器和閘道資源之間的連線成功時，狀態將會顯示為 **已連線** 。


    ![將伺服器連線到閘道資源成功](media/analysis-services-gateway-install/aas-gateway-connect-success.png)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用 [get-azresource](/powershell/module/az.resources/get-azresource) 來取得閘道 ResourceID。 然後，在 [AzAnalysisServicesServer](/powershell/module/az.analysisservices/set-azanalysisservicesserver)或 [AzAnalysisServicesServer](/powershell/module/az.analysisservices/new-azanalysisservicesserver)中指定 **-GatewayResourceID** ，將閘道資源連線到現有或新的伺服器。

若要取得閘道資源識別碼：

```azurepowershell-interactive
Connect-AzAccount -Tenant $TenantId -Subscription $subscriptionIdforGateway -Environment "AzureCloud"
$GatewayResourceId = $(Get-AzResource -ResourceType "Microsoft.Web/connectionGateways" -Name $gatewayName).ResourceId  

```

若要設定現有的伺服器：

```azurepowershell-interactive
Connect-AzAccount -Tenant $TenantId -Subscription $subscriptionIdforAzureAS -Environment "AzureCloud"
Set-AzAnalysisServicesServer -ResourceGroupName $RGName -Name $servername -GatewayResourceId $GatewayResourceId

```
---

就這麼簡單。 如果您需要開啟連接埠，或進行疑難排解，請務必簽出[內部部署資料閘道](analysis-services-gateway.md)。

## <a name="next-steps"></a>下一步

* [ Analysis Services](analysis-services-manage.md)   
* [從 Azure Analysis Services 取得資料](analysis-services-connect.md)   
* [使用 Azure 虛擬網路上的資料來源閘道](analysis-services-vnet-gateway.md)