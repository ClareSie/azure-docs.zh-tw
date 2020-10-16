---
title: 教學課程：設定 ExpressRoute 線路的對等互連 - Azure PowerShell
description: 本教學課程示範如何使用 PowerShell，以 Resource Manager 部署模型建立和管理 ExpressRoute 線路的路由設定。
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/08/2020
ms.author: duau
ms.openlocfilehash: 641d7eeef96af84f0f058aebd19d795083e3567f
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91855339"
---
# <a name="tutorial-create-and-modify-peering-for-an-expressroute-circuit-using-powershell"></a>教學課程：使用 PowerShell 建立和修改 ExpressRoute 線路的對等互連

本教學課程會協助您使用 PowerShell，在 Resource Manager 部署模型中建立和管理 ExpressRoute 線路的路由設定。 您還可以檢查狀態、更新，或是刪除與取消佈建 ExpressRoute 線路的對等互連。 如果您想要對線路使用不同的方法，可選取下列清單中的文章：

> [!div class="op_single_selector"]
> * [Azure 入口網站](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [公用對等互連](about-public-peering.md)
> * [視訊 - 私用對等互連](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [視訊 - Microsoft 對等互連](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (傳統)](expressroute-howto-routing-classic.md)
> 

這些指示只適用於由提供第 2 層連線服務的服務提供者所建立的線路。 如果您使用的服務提供者提供受控第 3 層服務 (通常是 IPVPN，如 MPLS)，連線提供者會為您設定和管理路由。

> [!IMPORTANT]
> 我們目前不會透過服務管理入口網站來公告服務提供者所設定的對等。 我們正努力在近期推出這項功能。 設定 BGP 對等互連之前，請洽詢您的服務提供者。
> 

您可以為 ExpressRoute 線路設定私人對等互連和 Microsoft 對等互連 (針對新線路的 Azure 公用對等互連已被取代)。 您可以依自己選擇的任何順序設定對等互連。 不過，您必須確定一次只完成一個對等的設定。 如需路由網域和對等互連的詳細資訊，請參閱 [ExpressRoute 路由網域](expressroute-circuit-peerings.md)。 如需公用對等互連的詳細資訊，請參閱 [ExpressRoute 公用對等互連](about-public-peering.md)。

在本教學課程中，您會了解如何：
> [!div class="checklist"]
> - 設定、更新及刪除線路的 Microsoft 對等互連
> - 設定、更新及刪除線路的 Azure 私人對等互連

## <a name="prerequisites"></a>必要條件

* 開始設定之前，請確定您已檢閱下列頁面：
    * [先決條件](expressroute-prerequisites.md) 
    * [路由需求](expressroute-routing.md)
    * [工作流程](expressroute-workflows.md)
* 您必須擁有作用中的 ExpressRoute 線路。 繼續之前，請遵循指示來[建立 ExpressRoute 線路](expressroute-howto-circuit-arm.md)，並由您的連線提供者來啟用該線路。 ExpressRoute 線路必須處於已佈建和已啟用狀態，您才能執行本文中的 Cmdlet。

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="microsoft-peering"></a><a name="msft"></a>Microsoft 對等互連

本節將協助您為 ExpressRoute 線路建立、取得、更新和刪除 Microsoft 對等互連設定。

> [!IMPORTANT]
> 在 2017 年 8 月 1 日以前設定之 ExpressRoute 線路的 Microsoft 對等互連，會透過 Microsoft 對等互連公告所有服務首碼，即使未定義路由篩選也一樣。 在 2017 年 8 月 1 日當日或以後設定之 ExpressRoute 線路的 Microsoft 對等互連，不會公告任何首碼，直到路由篩選連結至線路為止。 如需詳細資訊，請參閱[設定 Microsoft 對等互連的路由篩選](how-to-routefilter-powershell.md)。
> 

### <a name="to-create-microsoft-peering"></a>建立 Microsoft 對等

1. 登入並選取訂用帳戶。

   如果您已於本機安裝 PowerShell，請登入。 如果您是使用 Azure Cloud Shell，則可以跳過此步驟。

   ```azurepowershell
   Connect-AzAccount
   ```

   選取您想要建立 ExpressRoute 線路的訂用帳戶。

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionId "<subscription ID>"
   ```
2. 建立 ExpressRoute 線路。

   請遵循指示建立 [ExpressRoute 線路](expressroute-howto-circuit-arm.md) ，並由連線提供者佈建它。 若您的連線提供者提供受控第 3 層服務，您可以要求連線提供者為您啟用 Microsoft 對等互連。 您不需要遵循後續幾節所列的指示。 不過，如果您的連線提供者不管理路由，請在建立線路之後繼續使用後續步驟進行設定。 

3. 檢查 ExpressRoute 線路，以確定已佈建且已啟用線路。 請使用下列範例：

   ```azurepowershell-interactive
   Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```

   回應如下列範例所示：

   ```
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"################################"
   ProvisioningState                : Succeeded
   Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
   CircuitProvisioningState         : Enabled
   ServiceProviderProvisioningState : Provisioned
   ServiceProviderNotes             : 
   ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
   ServiceKey                       : **************************************
   Peerings                         : []
   ```
4. 設定線路的 Microsoft 對等。 繼續之前，請確定您擁有下列資訊。

   * 主要連結的 /30 或 /126 子網路。 地址區塊必須是您所擁有且註冊在 RIR / IRR 中的有效公用 IPv4 或 IPv6 前置詞。
   * 次要連結的 /30 或/126 子網路。 地址區塊必須是您所擁有且註冊在 RIR / IRR 中的有效公用 IPv4 或 IPv6 前置詞。
   * 供建立此對等的有效 VLAN ID。 請確定線路有沒有其他對等使用相同的 VLAN ID。
   * 對等的 AS 編號。 您可以使用 2 位元組和 4 位元組 AS 編號。
   * 公告的首碼：您要提供一份您打算透過 BGP 工作階段公告的所有前置詞清單。 只接受公用 IP 位址首碼。 如果計劃傳送一組首碼，可以傳送以逗號分隔的清單。 這些首碼必須在 RIR / IRR 中註冊給您。 IPv4 BGP 工作階段需要 IPv4 已公告首碼，而 IPv6 BGP 工作階段需要 IPv6 已公告首碼。 
   * 路由登錄名稱：您可以指定可供註冊 AS 編號和首碼的 RIR / IRR。
   * 選擇性：
     * 客戶 ASN：如果您要公告的前置詞未註冊給對等 AS 編號，您可以指定其所註冊的 AS 編號。
     * MD5 雜湊 (如果選擇使用)。

> [!IMPORTANT]
> Microsoft 會驗證指定的「已公告公用首碼」和「對等互連 ASN」(或「客戶 ASN」) 是否已在網際網路路由登錄中指派給您。 如果您要從另一個實體取得公用首碼，以及如果未使用路由登錄來記錄指派，則自動驗證將不會完成，而且需要手動驗證。 如果自動驗證失敗，您會看到 'AdvertisedPublicPrefixesState' 在 "Get-AzExpressRouteCircuitPeeringConfig" 的輸出上為「需要驗證」(請參閱下列區段中的「若要取得 Microsoft 對等互連詳細資訊」)。 
> 
> 如果您看到「需要驗證」訊息，請收集其中顯示實體已將公用前置詞指派給您組織的文件，而此實體列示為路由登錄中的前置詞擁有者，然後開啟支援票證，提交這些文件進行手動驗證。 
> 

   使用下列範例來為線路設定 Microsoft 對等互連：

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv4 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "123.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

   Add-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv6 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "3FFE:FFFF:0:CD31::/120" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

### <a name="to-get-microsoft-peering-details"></a><a name="getmsft"></a>取得 Microsoft 對等互連詳細資料

您可以使用下列範例來取得設定詳細資料：

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt
```

### <a name="to-update-microsoft-peering-configuration"></a><a name="updatemsft"></a>更新 Microsoft 對等組態

您可以使用下列範例來更新設定的任何部分：

```azurepowershell-interactive
Set-AzExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv4 -PrimaryPeerAddressPrefix "123.0.0.0/30" -SecondaryPeerAddressPrefix "123.0.0.4/30" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "124.1.0.0/24" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzExpressRouteCircuitPeeringConfig  -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt -PeeringType MicrosoftPeering -PeerASN 100 -PeerAddressType IPv6 -PrimaryPeerAddressPrefix "3FFE:FFFF:0:CD30::/126" -SecondaryPeerAddressPrefix "3FFE:FFFF:0:CD30::4/126" -VlanId 300 -MicrosoftConfigAdvertisedPublicPrefixes "3FFE:FFFF:0:CD31::/120" -MicrosoftConfigCustomerAsn 23 -MicrosoftConfigRoutingRegistryName "ARIN"

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="azure-private-peering"></a><a name="private"></a>Azure 私用對等互連

本節將協助您為 ExpressRoute 線路建立、取得、更新和刪除 Azure 私用對等互連設定。

### <a name="to-create-azure-private-peering"></a>建立 Azure 私用對等

1. 匯入 ExpressRoute 的 PowerShell 模組。

   從 [PowerShell 資源庫](https://www.powershellgallery.com/)安裝最新的 PowerShell 安裝程式。 然後將 Azure Resource Manager 模組匯入至 PowerShell 工作階段，才能開始使用 ExpressRoute Cmdlet。 您必須以系統管理員身分執行 PowerShell。

   ```azurepowershell-interactive
   Install-Module Az
   ```

   匯入已知語意版本範圍內所有的 Az.\* 模組。

   ```azurepowershell-interactive
   Import-Module Az
   ```

   您也可以只匯入已知語意版本範圍內選取的模組。

   ```azurepowershell-interactive
   Import-Module Az.Network 
   ```

   登入您的帳戶。

   ```azurepowershell-interactive
   Connect-AzAccount
   ```

   選取您想要建立 ExpressRoute 線路的訂用帳戶。

   ```azurepowershell-interactive
   Select-AzSubscription -SubscriptionId "<subscription ID>"
   ```
2. 建立 ExpressRoute 線路。

   請遵循指示建立 [ExpressRoute 線路](expressroute-howto-circuit-arm.md) ，並由連線提供者佈建它。 若您的連線提供者是提供受控第 3 層服務，您可以要求連線提供者為您啟用 Azure 私人對等互連。 您不需要遵循後續幾節所列的指示。 不過，如果您的連線提供者不管理路由，請在建立線路之後繼續使用後續步驟進行設定。

3. 檢查 ExpressRoute 線路，以確定已佈建且已啟用線路。 請使用下列範例：

   ```azurepowershell-interactive
   Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"
   ```

   回應如下列範例所示：

   ```
   Name                             : ExpressRouteARMCircuit
   ResourceGroupName                : ExpressRouteResourceGroup
   Location                         : westus
   Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
   Etag                             : W/"################################"
   ProvisioningState                : Succeeded
   Sku                              : {
                                       "Name": "Standard_MeteredData",
                                       "Tier": "Standard",
                                       "Family": "MeteredData"
                                     }
   CircuitProvisioningState         : Enabled
   ServiceProviderProvisioningState : Provisioned
   ServiceProviderNotes             : 
   ServiceProviderProperties        : {
                                       "ServiceProviderName": "Equinix",
                                       "PeeringLocation": "Silicon Valley",
                                       "BandwidthInMbps": 200
                                     }
   ServiceKey                       : **************************************
   Peerings                         : []
   ```
4. 設定線路的 Azure 私用對等。 繼續執行接下來的步驟之前，請確定您有下列項目：

   * 主要連結的 /30 子網路。 子網路不能在保留給虛擬網路的任何位址空間中。
   * 次要連結的 /30 子網路。 子網路不能在保留給虛擬網路的任何位址空間中。
   * 供建立此對等的有效 VLAN ID。 請確定線路有沒有其他對等使用相同的 VLAN ID。
   * 對等的 AS 編號。 您可以使用 2 位元組和 4 位元組 AS 編號。 您可以將私用 AS 編號用於此對等。 確保您不是使用 65515。
   * 選擇性：
     * MD5 雜湊 (如果選擇使用)。

   使用下列範例來為線路設定 Azure 私用對等互連：

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

   Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
   ```

   如果您選擇使用 MD5 雜湊，請使用下列範例：

   ```azurepowershell-interactive
   Add-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200  -SharedKey "A1B2C3D4"
   ```

   > [!IMPORTANT]
   > 請確定您將 AS 編號指定為對等 ASN，而不是客戶 ASN。
   > 
   >

### <a name="to-get-azure-private-peering-details"></a><a name="getprivate"></a>取得 Azure 私用對等互連詳細資訊

您可以使用下列範例來取得設定詳細資料：

```azurepowershell-interactive
$ckt = Get-AzExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Get-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt
```

### <a name="to-update-azure-private-peering-configuration"></a><a name="updateprivate"></a>更新 Azure 私用對等組態

您可以使用下列範例來更新設定的任何部分。 在此範例中，線路的 VLAN ID 從 100 更新為 500。

```azurepowershell-interactive
Set-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt -PeeringType AzurePrivatePeering -PeerASN 100 -PrimaryPeerAddressPrefix "10.0.0.0/30" -SecondaryPeerAddressPrefix "10.0.0.4/30" -VlanId 200

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="clean-up-resources"></a>清除資源

### <a name="to-delete-microsoft-peering"></a><a name="deletemsft"></a>刪除 Microsoft 對等

您可以執行下列 Cmdlet 來移除對等組態：

```azurepowershell-interactive
Remove-AzExpressRouteCircuitPeeringConfig -Name "MicrosoftPeering" -ExpressRouteCircuit $ckt

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

### <a name="to-delete-azure-private-peering"></a><a name="deleteprivate"></a>刪除 Azure 私用對等

您可以執行下列範例來移除對等互連設定：

> [!WARNING]
> 執行此範例之前，您必須確定已移除所有虛擬網路與 ExpressRoute Global Reach 連線。 
> 

```azurepowershell-interactive
Remove-AzExpressRouteCircuitPeeringConfig -Name "AzurePrivatePeering" -ExpressRouteCircuit $ckt

Set-AzExpressRouteCircuit -ExpressRouteCircuit $ckt
```

## <a name="next-steps"></a>後續步驟

設定 Azure 私人對等互連之後，您可以建立 ExpressRoute 閘道，以將虛擬網路連結至線路。 

> [!div class="nextstepaction"]
> [為 ExpressRoute 設定虛擬網路閘道](expressroute-howto-add-gateway-resource-manager.md)
