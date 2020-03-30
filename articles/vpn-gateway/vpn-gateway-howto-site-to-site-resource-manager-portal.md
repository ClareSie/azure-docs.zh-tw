---
title: 將本地網路連接到 Azure 虛擬網路：網站到網站 VPN：門戶
description: 透過公用網際網路建立從內部部署網路至 Azure 虛擬網路之 IPsec 連線的步驟。 這些步驟可協助您使用入口網站建立跨單位的站對站 VPN 閘道連線。
services: vpn-gateway
titleSuffix: Azure VPN Gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 03/03/2020
ms.author: cherylmc
ms.openlocfilehash: 857b50a04466f43a25cf80d7930cfb4639dc9d65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244429"
---
# <a name="create-a-site-to-site-connection-in-the-azure-portal"></a>在 Azure 入口網站中建立站對站連線

本文說明如何使用 Azure 入口網站來建立從內部部署網路到 VNet 的站對站 VPN 閘道連線。 本文中的步驟適用於 Resource Manager 部署模型。 您也可從下列清單中選取不同的選項，以使用不同的部署工具或部署模型來建立此組態：

> [!div class="op_single_selector"]
> * [Azure 門戶](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
> * [電源外殼](vpn-gateway-create-site-to-site-rm-powershell.md)
> * [Cli](vpn-gateway-howto-site-to-site-resource-manager-cli.md)
> * [Azure 入口網站 (傳統)](vpn-gateway-howto-site-to-site-classic-portal.md)
> 
>

站對站 VPN 閘道連線可用來透過 IPsec/IKE (IKEv1 或 IKEv2) VPN 通道，將內部部署網路連線到 Azure 虛擬網路。 此類型的連線需要位於內部部署的 VPN 裝置，且您已對該裝置指派對外開放的公用 IP 位址。 如需 VPN 閘道的詳細資訊，請參閱[關於 VPN 閘道](vpn-gateway-about-vpngateways.md)。

![站對站 VPN 閘道跨單位連線圖表](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/site-to-site-diagram.png)

## <a name="before-you-begin"></a>開始之前

在開始設定之前，請確認您已符合下列條件：

* 確定您有相容的 VPN 裝置以及能夠對其進行設定的人員。 如需相容 VPN 裝置和裝置組態的詳細資訊，請參閱[關於 VPN 裝置](vpn-gateway-about-vpn-devices.md)。
* 確認您的 VPN 裝置有對外開放的公用 IPv4 位址。
* 如果您不熟悉位於內部部署網路組態的 IP 位址範圍，您需要與能夠提供那些詳細資料的人協調。 當您建立此組態時，您必須指定 IP 位址範圍的首碼，以供 Azure 路由傳送至您的內部部署位置。 內部部署網路的子網路皆不得與您所要連線的虛擬網路子網路重疊。 

### <a name="example-values"></a><a name="values"></a>範例值

本文的範例使用下列值。 您可以使用這些值來建立測試環境，或參考這些值，進一步了解本文中的範例。 有關 VPN 閘道設置的詳細資訊，請參閱有關[VPN 閘道設置](vpn-gateway-about-vpn-gateway-settings.md)。

* **虛擬網路名稱：** VNet1
* **位址空間：** 10.1.0.0/16
* **訂用帳戶：** 您需要使用的訂用帳戶
* **資源組：** TestRG1
* **地區：** 美國東部
* **子網路：** FrontEnd：10.1.0.0/24，BackEnd：10.1.1.0/24 (在此練習中是選擇性的)
* **閘道子網位址範圍：** 10.1.255.0/27
* **虛擬網路閘道名稱：** VNet1GW
* **公用 IP 位址名稱：** VNet1GWpip
* **VPN 類型：** 基於路由
* **連線類型：** 網站到網站 （IPsec）
* **閘道類型：** Vpn
* **本地網路閘道名稱：** 網站1
* **連接名稱：** VNet1toSite1
* **共用的金鑰：** 此範例中，我們會使用 abc123。 但是，您可以使用任何與您 VPN 硬體相容的項目。 值務必符合連線的兩端。

## <a name="1-create-a-virtual-network"></a><a name="CreatVNet"></a>1. 創建虛擬網路

[!INCLUDE [Create a virtual network](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]

## <a name="2-create-the-vpn-gateway"></a><a name="VNetGateway"></a>2. 創建 VPN 閘道

此步驟將帶您建立 VNet 的虛擬網路閘道。 建立閘道通常可能需要 45 分鐘或更久，視選取的閘道 SKU 而定。

[!INCLUDE [About gateway subnets](../../includes/vpn-gateway-about-gwsubnet-portal-include.md)]

### <a name="example-settings"></a>範例設定

* **實例詳細資訊>區域：** 美國東部
* **虛擬網路>虛擬網路：** VNet1
* **>名稱的實例詳細資訊：** VNet1GW
* **閘道類型>實例詳細資訊：** Vpn
* **> VPN 類型的實例詳細資訊：** 基於路由
* **虛擬網路>閘道子網位址範圍**：10.1.255.0/27
* **公共 IP 位址>公共 IP 位址名稱：** VNet1GWpip

[!INCLUDE [Create a vpn gateway](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

[!INCLUDE [NSG warning](../../includes/vpn-gateway-no-nsg-include.md)]


## <a name="3-create-the-local-network-gateway"></a><a name="LocalNetworkGateway"></a>3. 創建本地網路閘道

區域網路閘道通常是指您的內部部署位置。 請賦予網站可供 Azure 參考的名稱，然後指定您想要與其建立連線之內部部署 VPN 裝置的 IP 位址。 也請指定 IP 位址首碼，以供系統透過 VPN 閘道路由至 VPN 裝置。 您指定的位址首碼是位於內部部署網路上的首碼。 如果您的內部部署網路變更，或者您需要變更 VPN 裝置的公用 IP 位址，您稍後可以輕鬆地更新這些值。

**示例值**

* **名稱：** 網站1
* **資源組：** TestRG1
* **位置：** 美國東部


[!INCLUDE [Add a local network gateway](../../includes/vpn-gateway-add-local-network-gateway-portal-include.md)]

## <a name="4-configure-your-vpn-device"></a><a name="VPNDevice"></a>4. 配置 VPN 設備

內部部署網路的站對站連線需要 VPN 裝置。 在此步驟中，設定 VPN 裝置。 在設定 VPN 裝置時，您需要下列項目：

- 共用金鑰。 這個共同金鑰與您建立站對站 VPN 連線時指定的共用金鑰相同。 在我們的範例中，我們會使用基本的共用金鑰。 我們建議您產生更複雜的金鑰以供使用。
- 虛擬網路閘道的公用 IP 位址。 您可以使用 Azure 入口網站、PowerShell 或 CLI 來檢視公用 IP 位址。 要使用 Azure 門戶查找 VPN 閘道的公共 IP 位址，請導航到**虛擬網路閘道**，然後按一下閘道的名稱。

[!INCLUDE [Configure a VPN device](../../includes/vpn-gateway-configure-vpn-device-include.md)]

## <a name="5-create-the-vpn-connection"></a><a name="CreateConnection"></a>5. 創建 VPN 連接

您會在虛擬網路閘道與內部部署 VPN 裝置之間建立站對站 VPN 連線。

[!INCLUDE [Add a site-to-site connection](../../includes/vpn-gateway-add-site-to-site-connection-portal-include.md)]

## <a name="6-verify-the-vpn-connection"></a><a name="VerifyConnection"></a>6. 驗證 VPN 連接

[!INCLUDE [Verify the connection](../../includes/vpn-gateway-verify-connection-portal-include.md)]

## <a name="to-connect-to-a-virtual-machine"></a><a name="connectVM"></a>連線至虛擬機器

[!INCLUDE [Connect to a VM](../../includes/vpn-gateway-connect-vm-s2s-include.md)]

## <a name="how-to-reset-a-vpn-gateway"></a><a name="reset"></a>如何重設 VPN 閘道

如果您遺失一或多個站對站 VPN 通道上的跨單位 VPN 連線，重設 Azure VPN 閘道會很有幫助。 在此情況下，您的所有內部部署 VPN 裝置都會運作正常，但無法使用 Azure VPN 閘道建立 IPsec 通道。 如需相關步驟，請參閱[重設 VPN 閘道](vpn-gateway-resetgw-classic.md)。

## <a name="how-to-change-a-gateway-sku-resize-a-gateway"></a><a name="resize"></a>如何變更閘道 SKU (調整閘道的大小)

如需變更閘道 SKU 的步驟，請參閱[閘道 SKU](vpn-gateway-about-vpn-gateway-settings.md#gwsku)。

## <a name="how-to-add-an-additional-connection-to-a-vpn-gateway"></a><a name="addconnect"></a>如何將其他連線新增至 VPN 閘道

您可以新增其他的連線，前提是連線之間沒有任何位址空間重疊。

1. 若要新增其他連線，請瀏覽至 VPN 閘道，然後按一下 [連線]**** 以開啟 [連線] 頁面。
2. 按一下 [+新增]**** 來新增連線。 調整連線類型以反映 VNet 對 VNet (如果連線到另一個 VNet 閘道) 或站對站。
3. 如果您要使用站對站進行連線，且您尚未建立所需連線之站台的區域網路閘道，您可以新建立一個。
4. 指定您需要使用的共用金鑰，然後按一下 [確定]**** 來建立連線。

## <a name="next-steps"></a>後續步驟

* 如需 BGP 的相關資訊，請參閱 [BGP 概觀](vpn-gateway-bgp-overview.md)和[如何設定 BGP](vpn-gateway-bgp-resource-manager-ps.md)。
* 如需強制通道的相關資訊，請參閱[關於強制通道](vpn-gateway-forced-tunneling-rm.md)。
* 如需高可用性主動-主動連線的相關資訊，請參閱[高可用性跨單位和 VNet 對 VNet 連線能力](vpn-gateway-highlyavailable.md)。
* 如需如何在虛擬網路中限制資源之網路流量的資訊，請參閱[網路安全性](../virtual-network/security-overview.md)。
* 如需 Azure 如何在 Azure、內部部署和網際網路資源間路由流量的資訊，請參閱[虛擬網路流量路由](../virtual-network/virtual-networks-udr-overview.md)。
* 如需使用 Azure Resource Manager 範本建立站對站 VPN 連線的相關資訊，請參閱[建立站對站 VPN 連線](https://azure.microsoft.com/resources/templates/101-site-to-site-vpn-create/)。
* 如需使用 Azure Resource Manager 範本建立 Vnet 對 Vnet VPN 連線的相關資訊，請參閱[部署 HB 異地複寫](https://azure.microsoft.com/resources/templates/101-hdinsight-hbase-replication-geo/)。
