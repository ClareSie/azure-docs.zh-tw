---
title: 為 Azure HDInsight 規劃虛擬網路
description: 瞭解如何規劃 Azure 虛擬網路部署，以將 HDInsight 連線至其他雲端資源或資料中心內的資源。
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/21/2020
ms.openlocfilehash: d421811c18ac63952432cd853a6928db7c81f3db
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "82182424"
---
# <a name="plan-a-virtual-network-for-azure-hdinsight"></a>為 Azure HDInsight 規劃虛擬網路

本文提供搭配 Azure HDInsight 使用[Azure 虛擬網路](../virtual-network/virtual-networks-overview.md)（vnet）的背景資訊。 它也會討論在您可以為 HDInsight 叢集執行虛擬網路之前，必須進行的設計和實施決策。 規劃階段完成後，您可以繼續為 Azure HDInsight 叢集[建立虛擬網路](hdinsight-create-virtual-network.md)。 如需有關正確設定網路安全性群組（Nsg）和使用者定義路由所需之 HDInsight 管理 IP 位址的詳細資訊，請參閱[hdinsight 管理 ip 位址](hdinsight-management-ip-addresses.md)。

使用 Azure 虛擬網路可啟用下列案例：

* 從內部部署網路直接連線至 HDInsight。
* 將 HDInsight 連線至 Azure 虛擬網路中的資料存放區。
* 直接存取無法透過網際網路公開使用的 Apache Hadoop 服務。 例如，Apache Kafka API 或 Apache HBase Java API。

> [!IMPORTANT]
> 在 VNET 中建立 HDInsight 叢集將會建立數個網路資源，例如 Nic 和負載平衡器。 請勿**刪除這些**網路資源，因為您的叢集需要它們才能正確地與 VNET 搭配運作。
>
> 2019年2月28日之後，在 VNET 中建立的新 HDInsight 叢集的網路資源（例如 Nic、磅等等）將會布建在相同的 HDInsight 叢集資源群組中。 先前，這些資源已布建在 VNET 資源群組中。 目前執行中的叢集和未使用 VNET 建立的叢集並沒有任何變更。

## <a name="planning"></a>規劃

規劃在虛擬網路中安裝 HDInsight 時，您必須回答的問題如下：

* 您需要將 HDInsight 安裝到現有虛擬網路嗎？ 或者，您要建立新的網路嗎？

    如果您使用現有的虛擬網路，您可能需要先修改網路設定，才能安裝 HDInsight。 如需詳細資訊，請參閱[將 HDInsight 新增至現有虛擬網路](#existingvnet)一節。

* 您要將包含 HDInsight 的虛擬網路連線至另一個虛擬網路或內部部署網路嗎？

    若要輕鬆地跨網路使用資源，您可能需要建立自訂 DNS，並設定 DNS 轉送。 如需詳細資訊，請參閱[連線多個網路](#multinet)一節。

* 您要將輸入或輸出流量限制/重新導向至 HDInsight 嗎？

    HDInsight 必須具有 Azure 資料中心內特定 IP 位址的無限制通訊。 另外還有數個連接埠必須允許通過防火牆才能進行用戶端通訊。 如需詳細資訊，請參閱[控制網路流量](#networktraffic)一節。

## <a name="add-hdinsight-to-an-existing-virtual-network"></a><a id="existingvnet"></a>將 HDInsight 新增至現有虛擬網路

使用本節中的步驟，以了解如何將新的 HDInsight 新增至現有 Azure 虛擬網路。

> [!NOTE]  
> 您無法在虛擬網路中新增現有的 HDInsight 叢集。

1. 您要使用虛擬網路的傳統或 Resource Manager 部署模型嗎？

    HDInsight 3.4 和更新版本需要 Resource Manager 虛擬網路。 舊版 HDInsight 需要傳統虛擬網路。

    如果您的現有網路是傳統虛擬網路，則必須建立 Resource Manager 虛擬網路，然後連線兩者。 [將傳統 VNet 連線至新的 VNet](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md)。

    加入之後，Resource Manager 網路中所安裝的 HDInsight 就可以與傳統網路中的資源互動。

2. 您使用網路安全性群組、使用者定義路由或虛擬網路設備來限制傳入或傳出虛擬網路的流量嗎？

    HDInsight 是一個受控服務，需要 Azure 資料中心內數個 IP 位址的無限制存取權。 若要允許與這些 IP 位址的通訊，請更新任何現有網路安全性群組或使用者定義路由。

    HDInsight 會裝載多個使用各種連接埠的服務。 不要封鎖這些埠的流量。 如需允許通過虛擬設備防火牆的連接埠清單，請參閱「安全性」一節。

    若要尋找現有安全性設定，請使用下列 Azure PowerShell 或 Azure CLI 命令：

    * 網路安全性群組

        將`RESOURCEGROUP`取代為包含虛擬網路的資源組名，然後輸入命令：

        ```powershell
        Get-AzNetworkSecurityGroup -ResourceGroupName  "RESOURCEGROUP"
        ```

        ```azurecli
        az network nsg list --resource-group RESOURCEGROUP
        ```

        如需詳細資訊，請參閱[為網路安全性群組疑難排解](../virtual-network/diagnose-network-traffic-filter-problem.md)文件。

        > [!IMPORTANT]  
        > 會根據規則優先順序依序套用網路安全性群組規則。 會套用第一個符合流量模式的規則，而且未針對該流量套用其他規則。 排序從最寬鬆到最嚴格權限的規則。 如需詳細資訊，請參閱[使用網路安全性群組來篩選網路流量](../virtual-network/security-overview.md)文件。

    * 使用者定義的路由

        將`RESOURCEGROUP`取代為包含虛擬網路的資源組名，然後輸入命令：

        ```powershell
        Get-AzRouteTable -ResourceGroupName "RESOURCEGROUP"
        ```

        ```azurecli
        az network route-table list --resource-group RESOURCEGROUP
        ```

        如需詳細資訊，請參閱[為路由疑難排解](../virtual-network/diagnose-network-routing-problem.md)文件。

3. 建立 HDInsight 叢集，並在設定期間選擇 Azure 虛擬網路。 使用下列文件中的步驟，以了解叢集建立程序：

    * [使用 Azure 入口網站建立 HDInsight](hdinsight-hadoop-create-linux-clusters-portal.md)
    * [使用 Azure PowerShell 建立 HDInsight](hdinsight-hadoop-create-linux-clusters-azure-powershell.md)
    * [使用 Azure 傳統 CLI 建立 HDInsight](hdinsight-hadoop-create-linux-clusters-azure-cli.md)
    * [使用 Azure Resource Manager 範本建立 HDInsight](hdinsight-hadoop-create-linux-clusters-arm-templates.md)

   > [!IMPORTANT]  
   > 將 HDInsight 新增至虛擬網路是選擇性的設定步驟。 請務必在設定叢集時選取虛擬網路。

## <a name="connecting-multiple-networks"></a><a id="multinet"></a>連線多個網路

多網路設定的最大挑戰是網路之間的名稱解析。

Azure 會針對安裝於虛擬網路中的 Azure 服務提供名稱解析。 這個內建名稱解析允許 HDInsight 使用完整網域名稱 (FQDN) 連線至下列資源：

* 網際網路上的任何可用資源。 例如，microsoft.com、windowsupdate.com。

* 相同 Azure 虛擬網路中的任何資源，方法是使用資源的「內部 DNS 名稱」____。 例如，使用預設名稱解析時，以下是指派給 HDInsight 背景工作角色節點的內部 DNS 名稱範例：

  * wn0-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net
  * wn2-hdinsi.0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net

    這些節點可以使用內部 DNS 名稱彼此直接通訊，以及與 HDInsight 中的其他節點通訊。

預設名稱解析「不」____ 允許 HDInsight 解析加入虛擬網路之網路中的資源名稱。 例如，通常會將內部部署網路加入虛擬網路。 根據預設名稱解析，HDInsight 無法依名稱存取內部部署網路中的資源。 相反地，內部部署網路中的資源無法依名稱存取虛擬網路中的資源。

> [!WARNING]  
> 您必須建立自訂 DNS 伺服器，並設定虛擬網路使用它，再建立 HDInsight 叢集。

若要啟用虛擬網路與所加入網路中資源之間的名稱解析，您必須執行下列動作：

1. 在要安裝 HDInsight 的 Azure 虛擬網路中建立自訂 DNS 伺服器。

2. 將虛擬網路設定為使用自訂 DNS 伺服器。

3. 尋找虛擬網路的 Azure 指派 DNS 尾碼。 此值與 `0owcbllr5hze3hxdja3mqlrhhe.ex.internal.cloudapp.net` 類似。 如需尋找 DNS 尾碼的資訊，請參閱[範例：自訂 DNS](hdinsight-create-virtual-network.md#example-dns) 一節。

4. 設定 DNS 伺服器之間的轉送。 設定取決於遠端網路類型。

   * 如果遠端網路是內部部署網路，請設定 DNS，如下所示：

     * 「自訂 DNS」____(在虛擬網路中)：

         * 將虛擬網路 DNS 尾碼的要求轉送至 Azure 遞迴解析程式 (168.63.129.16)。 Azure 會處理虛擬網路中資源的要求

         * 將所有其他要求轉送至內部部署 DNS 伺服器。 內部部署 DNS 會處理所有其他名稱解析要求，即使是網際網路資源 (例如 Microsoft.com) 的要求也是一樣。

     * 內部部署 DNS____：將虛擬網路 DNS 尾碼的要求轉送至自訂 DNS 伺服器。 自訂 DNS 伺服器接著會轉送至 Azure 遞迴解析程式。

       此設定會將完整網域名稱包含虛擬網路 DNS 尾碼的要求路由傳送至自訂 DNS 伺服器。 內部部署 DNS 伺服器會處理所有其他要求 (即使是針對公用網際網路位址)。

   * 如果遠端網路是另一個 Azure 虛擬網路，請設定 DNS，如下所示：

     * 「自訂 DNS」____(在每個虛擬網路中)：

         * 將虛擬網路 DNS 尾碼的要求轉送至自訂 DNS 伺服器。 每個虛擬網路中的 DNS 會負責解析其網路內的資源。

         * 將所有其他要求轉送至 Azure 遞迴解析程式。 遞迴解析程式負責解析本機和網際網路資源。

       根據 DNS 尾碼，每個網路的 DNS 伺服器都會將要求轉送至另一個。 其他要求是使用 Azure 遞迴解析程式進行解析。

     如需每個設定的範例，請參閱[範例：自訂 DNS](hdinsight-create-virtual-network.md#example-dns) 一節。

如需詳細資訊，請參閱 [VM 和角色執行個體的名稱解析](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md)文件。

## <a name="directly-connect-to-apache-hadoop-services"></a>直接連線至 Apache Hadoop 服務

您可以連線至位於 `https://CLUSTERNAME.azurehdinsight.net` 的叢集。 此位址會使用公用 IP，如果您已使用 NSG 限制來自網際網路的傳入流量，則可能無法觸達該 IP。 此外，當您在 VNet 中部署叢集時，可以使用私人端點 `https://CLUSTERNAME-int.azurehdinsight.net` 來存取該叢集。 此端點會解析至 VNet 內的私人 IP，以便存取叢集。

若要透過虛擬網路連線至 Apache Ambari 和其他網頁，請使用下列步驟：

1. 若要探索 HDInsight 叢集節點的內部完整網域名稱 (FQDN)，請使用下列其中一種方法：

    將`RESOURCEGROUP`取代為包含虛擬網路的資源組名，然後輸入命令：

    ```powershell
    $clusterNICs = Get-AzNetworkInterface -ResourceGroupName "RESOURCEGROUP" | where-object {$_.Name -like "*node*"}

    $nodes = @()
    foreach($nic in $clusterNICs) {
        $node = new-object System.Object
        $node | add-member -MemberType NoteProperty -name "Type" -value $nic.Name.Split('-')[1]
        $node | add-member -MemberType NoteProperty -name "InternalIP" -value $nic.IpConfigurations.PrivateIpAddress
        $node | add-member -MemberType NoteProperty -name "InternalFQDN" -value $nic.DnsSettings.InternalFqdn
        $nodes += $node
    }
    $nodes | sort-object Type
    ```

    ```azurecli
    az network nic list --resource-group RESOURCEGROUP --output table --query "[?contains(name,'node')].{NICname:name,InternalIP:ipConfigurations[0].privateIpAddress,InternalFQDN:dnsSettings.internalFqdn}"
    ```

    在所傳回的節點清單中，尋找前端節點的 FQDN 並使用這些 FQDN 來連線至 Ambari 和其他 Web 服務。 例如，使用 `http://<headnode-fqdn>:8080` 存取 Ambari。

    > [!IMPORTANT]  
    > 前端節點上裝載的某些服務一次只會在一個節點上作用。 如果您嘗試在一個前端節點上存取服務，但傳回 404 錯誤，請切換至其他的前端節點。

2. 若要判斷可提供服務的節點和連接埠，請參閱 [Hadoop 服務在 HDInsight 上所使用的連接埠](./hdinsight-hadoop-port-settings-for-services.md)文件。

## <a name="controlling-network-traffic"></a><a id="networktraffic"></a> 控制網路流量

### <a name="techniques-for-controlling-inbound-and-outbound-traffic-to-hdinsight-clusters"></a>控制 HDInsight 叢集之輸入和輸出流量的技術

Azure 虛擬網路中的網路流量可以使用下列方法進行控制：

* **網路安全性群組** (NSG) 可讓您篩選輸入和輸出網路流量。 如需詳細資訊，請參閱[使用網路安全性群組來篩選網路流量](../virtual-network/security-overview.md)文件。

* **網路虛擬裝置**（NVA）只能與輸出流量搭配使用。 Nva 會複寫防火牆和路由器等裝置的功能。 如需詳細資訊，請參閱[網路設備](https://azure.microsoft.com/solutions/network-appliances)文件。

HDInsight 是受控服務，針對來自 VNET 的傳入和傳出流量，需要不受限制地存取 HDInsight 健全狀況和管理服務。 使用 Nsg 時，您必須確定這些服務仍然可以與 HDInsight 叢集通訊。

![在 Azure 自訂 VNET 中建立的 HDInsight 實體圖表](./media/hdinsight-plan-virtual-network-deployment/hdinsight-vnet-diagram.png)

### <a name="hdinsight-with-network-security-groups"></a>具有網路安全性群組的 HDInsight

如果您計畫使用**網路安全性群組**來控制網路流量，請在安裝 HDInsight 之前執行下列動作：

1. 識別您要用於 HDInsight 的 Azure 區域。

2. 識別 HDInsight 針對您的區域所需的服務標記。 如需詳細資訊，請參閱[Azure HDInsight 的網路安全性群組（NSG）服務標記](hdinsight-service-tags.md)。

3. 建立或修改您打算安裝 HDInsight 之子網的網路安全性群組。

    * __網路安全性群組__：允許連接埠 __443__ 上來自 IP 位址的「輸入」____ 流量。 這可確保 HDInsight 管理服務可以從虛擬網路外部連線到叢集。

如需網路安全性群組的詳細資訊，請參閱[網路安全性群組的總覽](../virtual-network/security-overview.md)。

### <a name="controlling-outbound-traffic-from-hdinsight-clusters"></a>控制來自 HDInsight 叢集的輸出流量

如需控制 HDInsight 叢集輸出流量的詳細資訊，請參閱[設定 Azure HDInsight 叢集的輸出網路流量限制](hdinsight-restrict-outbound-traffic.md)。

#### <a name="forced-tunneling-to-on-premises"></a>強制通道至內部部署

強制通道是一種使用者定義路由設定，其中來自子網路的所有流量都會強制流向特定網路或位置，例如內部部署網路。 HDInsight 不__支援將__流量強制通道傳送至內部部署網路。

## <a name="required-ip-addresses"></a><a id="hdinsight-ip"></a> 所需的 IP 位址

如果您使用網路安全性群組或使用者定義的路由來控制流量，請參閱[HDInsight 管理 IP 位址](hdinsight-management-ip-addresses.md)。

## <a name="required-ports"></a><a id="hdinsight-ports"></a> 所需連接埠

如果您打算使用**防火牆**並從從外部透過特定連接埠存取叢集，則可能需要在您案例所需的連接埠上允許流量。 根據預設，只要上一節所述的 Azure 管理流量允許在埠443上連線到叢集，就不需要任何特殊的埠允許清單。

如需特定服務的連接埠清單，請參閱 [HDInsight 上 Apache Hadoop 服務所使用的連接埠](hdinsight-hadoop-port-settings-for-services.md)文件。

如需虛擬設備防火牆規則的詳細資訊，請參閱[虛擬設備案例](../virtual-network/virtual-network-scenario-udr-gw-nva.md)文件。

## <a name="load-balancing"></a>負載平衡

當您建立 HDInsight 叢集時，也會建立負載平衡器。 此負載平衡器的類型位於[基本 SKU 層級](../load-balancer/concepts-limitations.md#skus)，其具有特定的條件約束。 其中一個條件約束是，如果您在不同的區域中有兩個虛擬網路，您就無法連線至基本負載平衡器。 如需詳細資訊，請參閱[虛擬網路常見問題：全域 vnet 對等互連的條件約束](../virtual-network/virtual-networks-faq.md#what-are-the-constraints-related-to-global-vnet-peering-and-load-balancers)。

## <a name="next-steps"></a>後續步驟

* 如需建立 Azure 虛擬網路的程式碼範例和範例，請參閱[建立 Azure HDInsight 叢集的虛擬網路](hdinsight-create-virtual-network.md)。
* 如需設定 HDInsight 連線至內部部署網路的端對端範例，請參閱[將 HDInsight 連線至內部部署網路](./connect-on-premises-network.md)。
* 如需 Azure 虛擬網路的詳細資訊，請參閱 [Azure 虛擬網路概觀](../virtual-network/virtual-networks-overview.md)。
* 如需網路安全性群組的詳細資訊，請參閱[網路安全性群組](../virtual-network/security-overview.md)。
* 如需使用者定義路由的詳細資訊，請參閱[使用者定義的路由和 IP 轉送](../virtual-network/virtual-networks-udr-overview.md)。
