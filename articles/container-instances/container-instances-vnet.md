---
title: 將容器組部署到 Azure 虛擬網路
description: 了解如何將容器群組部署至新的或現有的 Azure 虛擬網路。
ms.topic: article
ms.date: 01/06/2020
ms.author: danlep
ms.openlocfilehash: 318576e9b5c5b32bbc993ea16494c938b74bd2f4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "77200056"
---
# <a name="deploy-container-instances-into-an-azure-virtual-network"></a>將容器執行個體部署至 Azure 虛擬網路

[Azure 虛擬網路](../virtual-network/virtual-networks-overview.md)為 Azure 和本地資源提供安全的私人網路絡。 透過將容器群組部署至 Azure 虛擬網路，您的容器可在虛擬網路中安全地與其他資源通訊。

部署至 Azure 虛擬網路的容器群組可供您進行下列案例：

* 直接在同一個子網路中的容器群組間通訊
* 在虛擬網路中，將[以工作為基礎](container-instances-restart-policy.md)的工作負載輸出從容器執行個體傳送至資料庫
* 從虛擬網路中的[服務端點](../virtual-network/virtual-network-service-endpoints-overview.md)為容器執行個體擷取內容
* 容器可在虛擬網路中與虛擬機器通訊
* 容器可透過 [VPN 閘道](../vpn-gateway/vpn-gateway-about-vpngateways.md)或 [ExpressRoute](../expressroute/expressroute-introduction.md) 與內部部署資源通訊

> [!IMPORTANT]
> 到虛擬網路的容器組部署通常僅適用于以下區域的生產工作負載：**美國東部、美國中南部和美國西部 2**。 在此功能可用的其他區域，虛擬網路部署當前處於預覽狀態，並計畫在不久的將來提供一般部署。 若您同意[補充的使用規定][terms-of-use]，即可取得預覽。 


## <a name="virtual-network-deployment-limitations"></a>虛擬網路部署限制

當您將容器群組部署至虛擬網路時，會有特定限制。

* 若要將容器群組部署至子網路，該子網路不能包含任何其他資源類型。 在將容器群組部署至子網路之前，請先將所有現有資源從現有的子網路移除，或是建立新的子網路。
* 不能在部署到虛擬網路的容器組中使用[託管標識](container-instances-managed-identity.md)。
* 無法在部署到虛擬網路的容器組中啟用[即時探測](container-instances-liveness-probe.md)或[就緒探測](container-instances-readiness-probe.md)。
* 由於涉及其他網路資源，將容器組部署到虛擬網路通常比部署標準容器實例慢。

[!INCLUDE [container-instances-vnet-limits](../../includes/container-instances-vnet-limits.md)]

容器資源限制會隨著這些區域中非網路的容器執行個體其限制不同而異。 目前僅支援 Linux 容器的這項功能。 已計劃支援 Windows。

### <a name="unsupported-networking-scenarios"></a>不支援的網路方案 

* **Azure 負載等化器**- 不支援將 Azure 負載等化器放在網路容器組中的容器實例前面
* **虛擬網路對等互連**
  * 如果 ACI VNet 對等網路對等使用公共 IP 空間，則 VNet 對等互連不適用於 ACI。 對等網路需要 RFC 1918 專用 IP 空間，以便 VNet 對等互連工作。 
  * 您只能將 VNet 與另一個 VNet 對等
* **虛擬網路流量路由**- 無法圍繞公共 IP 設置自訂路由。 路由可以在部署 ACI 資源的委派子網的專用 IP 空間內設置 
* **網路安全性群組**- 當前未強制執行應用於委派給 Azure 容器實例的子網的 NSG 中的出站安全規則 
* **公共 IP 或 DNS 標籤**- 部署到虛擬網路的容器組當前不支援使用公共 IP 位址或完全限定的功能變數名稱將容器直接公開到 Internet
* **內部名稱解析**- 不支援通過內部 Azure DNS 在虛擬網路中為 Azure 資源提供名稱解析

當您將容器群組部署至虛擬網路後，便需要[其他步驟](#delete-network-resources)來**刪除網路資源**。

## <a name="required-network-resources"></a>必要的網路資源

將容器群組部署至虛擬網路需要三項 Azure 虛擬網路資源：[虛擬網路](#virtual-network)本身、虛擬網路中的[委派子網路](#subnet-delegated)及[網路設定檔](#network-profile)。 

### <a name="virtual-network"></a>虛擬網路

虛擬網路可定義您建立一或多個子網路所在的位址空間。 接著，您可以將 Azure 資源 (例如容器群組) 部署到虛擬網路中的子網路。

### <a name="subnet-delegated"></a>子網路 (委派)

子網路會將虛擬網路分割成位址空間，供放在其中的 Azure 資源使用。 您可以在虛擬網路中建立一或多個子網路。

您用於容器群組的子網路只能包含容器群組。 當您首次將容器群組部署至子網路時，Azure 會將該子網路委派至 Azure 容器執行個體。 委派之後，子網路只能用於容器群組。 如果您嘗試將容器群組以外的資源部署至委派子網路，該作業會失敗。

### <a name="network-profile"></a>網路設定檔

網路設定檔是 Azure 資源的網路設定範本。 其會為資源指定特定的網路屬性，例如應該部署該資源的子網路。 當您首次使用 [az container create][az-container-create] 命令將容器群組部署至子網路 (也因此部署至虛擬網路) 時，Azure 會為您建立網路設定檔。 然後您可以在日後部署至子網路時，使用該網路設定檔。 

若要使用 Resource Manager 範本、YAML 檔案或程式設計方法將容器群組部署至子網路，您必須提供網路設定檔的完整 Resource Manager 資源識別碼。 您可以使用先前使用 [az container create][az-container-create] 建立的設定檔，或使用 Resource Manager 範本來建立設定檔 (請參閱[範本範例](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet)及[參考](https://docs.microsoft.com/azure/templates/microsoft.network/networkprofiles))。 若要取得先前所建設定檔的識別碼，請使用 [az network profile list][az-network-profile-list] 命令。 

在下圖中，多個容器群組已部署到委派至 Azure 容器執行個體的子網路。 當您將一個容器群組部署至子網路後，就可立即指定相同的網路設定檔來部署其他容器群組。

![虛擬網路中的容器群組][aci-vnet-01]

## <a name="deployment-scenarios"></a>部署案例

您可以使用 [az container create][az-container-create] 將容器群組部署至新的虛擬網路，並允許 Azure 為您建立必要的網路資源，或是部署至現有的虛擬網路。 

### <a name="new-virtual-network"></a>新的虛擬網路

若要部署至新的虛擬網路並讓 Azure 自動為您建立網路資源，請在執行 [az container create][az-container-create] 時指定下列項目：

* 虛擬網路名稱
* CIDR 格式的虛擬網路位址首碼
* 子網路名稱
* CIDR 格式的子網路位址首碼

虛擬網路及子網路位址首碼分別指定了虛擬網路及子網路的位址空間。 這些值會使用無類別網域間路由選擇 (CIDR) 標記法呈現，例如：`10.0.0.0/16`。 如需使用子網路的詳細資訊，請參閱[新增、變更或刪除虛擬網路子網路](../virtual-network/virtual-network-manage-subnet.md)。

當您利用此方法部署了第一個容器群組後，就可以透過指定虛擬網路及子網路名稱，或是 Azure 自動為您建立的網路設定檔，來部署至同一個子網路。 因為 Azure 將該子網路委派至 Azure 容器執行個體，所以您「只」** 能將容器群組部署至子網路。

### <a name="existing-virtual-network"></a>現有的虛擬網路

將容器群組部署至現有的虛擬網路：

1. 在現有虛擬網路中創建子網，使用已部署容器組的現有子網，或使用清空*所有其他*資源的現有子網
1. 使用 [az container create][az-container-create] 部署容器群組，並指定下列其中一項：
   * 虛擬網路名稱及子網路名稱
   * 虛擬網路資源識別碼與子網路資源識別碼，其允許使用不同資源群組中的虛擬網路
   * 網路設定檔名稱或識別碼，您可使用 [az network profile list][az-network-profile-list] 取得

當您將第一個容器群組部署至現有的子網路後，Azure 會將該子網路委派至 Azure 容器執行個體。 除了容器群組之外，您不可再將其他資源部署至該子網路。

## <a name="deployment-examples"></a>部署範例

下列章節說明如何利用 Azure CLI 將容器群組部署至虛擬網路。 該命令範例已處理成 **Bash** 殼層格式。 如果您慣用其他殼層，例如 PowerShell 或命令提示字元，請相應調整行接續字元。

### <a name="deploy-to-a-new-virtual-network"></a>部署至新的虛擬網路

首先，部署容器群組，並為新的虛擬網路及子網路指定參數。 當您指定這些參數時，Azure 會建立虛擬網路及子網路，將子網路委派至 Azure 容器執行個體，以及建立網路設定檔。 當您建立這些資源後，您的容器群組就會部署至子網路。

請執行下列 [az container create][az-container-create] 命令，為新的虛擬網路及子網路指定設定。 您需要提供在虛擬網路中的容器組部署[可用的](#virtual-network-deployment-limitations)區域中創建的資源組的名稱。 此命令部署運行為靜態網頁提供服務的小型 Node.js Webserver 的公共 Microsoft [aci-helloworld][aci-helloworld]容器。 在下一節中，您會對同一個子網路部署第二個容器群組，並測試兩個容器執行個體之間的通訊。

```azurecli
az container create \
    --name appcontainer \
    --resource-group myResourceGroup \
    --image mcr.microsoft.com/azuredocs/aci-helloworld \
    --vnet aci-vnet \
    --vnet-address-prefix 10.0.0.0/16 \
    --subnet aci-subnet \
    --subnet-address-prefix 10.0.0.0/24
```

當您使用此方法部署至新的虛擬網路時，部署可能會花費幾分鐘建立網路資源。 經過初始部署後，其他容器群組部署會更快完成。

### <a name="deploy-to-existing-virtual-network"></a>部署至現有的虛擬網路

既然您已將一個容器群組部署至新的虛擬網路，請再對同一個子網路部署第二個容器群組，然後驗證這兩個容器執行個體之間的通訊。

首先，取得您部署的第一個容器群組 IP 位址 *appcontainer*：

```azurecli
az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
```

輸出應會顯示私人子網路中容器群組的 IP 位址：

```console
$ az container show --resource-group myResourceGroup --name appcontainer --query ipAddress.ip --output tsv
10.0.0.4
```

現在，將 `CONTAINER_GROUP_IP` 設定為您用 `az container show` 命令擷取的 IP，然後執行下列 `az container create` 命令。 第二個容器 *commchecker*會執行以 Alpine Linux 為基礎的映像，並對第一個容器群組的私人子網路 IP 位址執行 `wget`。

```azurecli
CONTAINER_GROUP_IP=<container-group-IP-here>

az container create \
    --resource-group myResourceGroup \
    --name commchecker \
    --image alpine:3.5 \
    --command-line "wget $CONTAINER_GROUP_IP" \
    --restart-policy never \
    --vnet aci-vnet \
    --subnet aci-subnet
```

當第二個容器部署完成之後，請提取其記錄，您便可以看見其執行的 `wget` 命令輸出：

```azurecli
az container logs --resource-group myResourceGroup --name commchecker
```

如果第二個容器已成功與第一個容器通訊，輸出應會如下所示：

```console
$ az container logs --resource-group myResourceGroup --name commchecker
Connecting to 10.0.0.4 (10.0.0.4:80)
index.html           100% |*******************************|  1663   0:00:00 ETA
```

記錄輸出應會顯示，`wget` 可以使用其區域子網路的私人 IP 位址從第一個容器連線及下載索引檔案。 兩個容器群組之間的網路流量會保留在虛擬網路中。

### <a name="deploy-to-existing-virtual-network---yaml"></a>部署至現有的虛擬網路 - YAML

還可以通過使用 YAML 檔、資源管理器範本或其他程式設計方法（如使用 Python SDK）將容器組部署到現有虛擬網路。 若要部署至虛擬網路中的子網路，您要在 YAML 中額外指定幾項屬性：

* `ipAddress`：容器群組的 IP 位址設定。
  * `ports`：要開啟的連接埠 (如果有)。
  * `protocol`：已開啟連接埠的通訊協定 (TCP 或 UDP)。
* `networkProfile`：為 Azure 資源指定網路設定，像是虛擬網路及子網路。
  * `id`：`networkProfile` 的完整 Resource Manager 資源識別碼。

若要使用 YAML 檔案將容器群組部署至虛擬網路，您需要先取得網路設定檔的識別碼。 請執行 [az network profile list][az-network-profile-list] 命令，指定包含您虛擬網路及委派子網路的資源群組名稱。

``` azurecli
az network profile list --resource-group myResourceGroup --query [0].id --output tsv
```

命令的輸出會顯示完整的網路設定檔識別碼：

```console
$ az network profile list --resource-group myResourceGroup --query [0].id --output tsv
/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-aci-subnet
```

當您取得網路設定檔識別碼後，請將下列 YAML 複製到名為 *vnet-deploy-aci.yaml* 的新檔案。 在 `networkProfile` 之下，用您剛擷取的識別碼來取代 `id` 值，然後儲存檔案。 此 YAML 會在虛擬網路中建立名為 *appcontaineryaml* 的容器群組。

```YAML
apiVersion: '2018-09-01'
location: westus
name: appcontaineryaml
properties:
  containers:
  - name: appcontaineryaml
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld
      ports:
      - port: 80
        protocol: TCP
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  ipAddress:
    type: Private
    ports:
    - protocol: tcp
      port: '80'
  networkProfile:
    id: /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkProfiles/aci-network-profile-aci-vnet-subnet
  osType: Linux
  restartPolicy: Always
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

使用 [az container create][az-container-create] 命令來部署容器群組，並為 `--file` 參數指定 YAML 檔案名稱：

```azurecli
az container create --resource-group myResourceGroup --file vnet-deploy-aci.yaml
```

部署完成後，請執行 [az container show][az-container-show] 命令來顯示其狀態：

```console
$ az container show --resource-group myResourceGroup --name appcontaineryaml --output table
Name              ResourceGroup    Status    Image                                       IP:ports     Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  ------------------------------------------  -----------  ---------  ---------------  --------  ----------
appcontaineryaml  myResourceGroup  Running   mcr.microsoft.com/azuredocs/aci-helloworld  10.0.0.5:80  Private    1.0 core/1.5 gb  Linux     westus
```

## <a name="clean-up-resources"></a>清除資源

### <a name="delete-container-instances"></a>刪除容器執行個體

當您使用完所建立的容器執行個體後，請使用下列命令將其刪除：

```azurecli
az container delete --resource-group myResourceGroup --name appcontainer -y
az container delete --resource-group myResourceGroup --name commchecker -y
az container delete --resource-group myResourceGroup --name appcontaineryaml -y
```

### <a name="delete-network-resources"></a>刪除網路資源

此功能當前需要多個其他命令來刪除您之前創建的網路資源。 如果您使用了本文前幾節的範例命令來建立虛擬網路及子網路，您可以使用下列指令碼來刪除這些網路資源。 該腳本假定資源組包含具有單個網路設定檔的單個虛擬網路。

在執行指令碼之前，請將變數 `RES_GROUP` 設定為包含應刪除虛擬網路及子網路的資源群組名稱。 如果您未使用前面建議的名稱，`aci-vnet`請更新虛擬網路的名稱。 此指令碼會針對 Bash 殼層加以格式化。 如果您慣用其他殼層，例如 PowerShell 或是命令提示字元，您需要相應調整變數指派及存取子。

> [!WARNING]
> 此指令碼會刪除資源！ 它會刪除虛擬網路及內含的所有子網路。 在執行此指令碼之前，請先確認您已不再需要虛擬網路中的「任何」** 資源，包括內含的任何子網路。 一旦您刪除後，**這些資源就無法復原**。

```azurecli
# Replace <my-resource-group> with the name of your resource group
# Assumes one virtual network in resource group
RES_GROUP=<my-resource-group>

# Get network profile ID
# Assumes one profile in virtual network
NETWORK_PROFILE_ID=$(az network profile list --resource-group $RES_GROUP --query [0].id --output tsv)

# Delete the network profile
az network profile delete --id $NETWORK_PROFILE_ID -y

# Delete virtual network
az network vnet delete --resource-group $RES_GROUP --name aci-vnet
```

## <a name="next-steps"></a>後續步驟

若要使用 Resource Manager 範本部署新的虛擬網路、子網路、網路設定檔及容器群組，請參閱 [Create an Azure container group with VNet](https://github.com/Azure/azure-quickstart-templates/tree/master/101-aci-vnet
) (使用 VNet 建立 Azure 容器群組)。

雖然本文簡短，但仍討論了幾項虛擬網路資源及功能。 Azure 虛擬網路文件涵蓋這些主題的詳細說明：

* [虛擬網路](../virtual-network/manage-virtual-network.md)
* [子](../virtual-network/virtual-network-manage-subnet.md)
* [服務終結點](../virtual-network/virtual-network-service-endpoints-overview.md)
* [VPN 閘道](../vpn-gateway/vpn-gateway-about-vpngateways.md)
* [快速路線](../expressroute/expressroute-introduction.md)

<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[aci-helloworld]: https://hub.docker.com/_/microsoft-azuredocs-aci-helloworld
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-network-profile-list]: /cli/azure/network/profile#az-network-profile-list
