---
title: 使用 Azure CLI 的 Azure 流量管理員子網覆寫 |Microsoft Docs
description: 本文將協助您瞭解如何使用流量管理員子網覆寫來覆寫流量管理員設定檔的路由方法，以透過預先定義的 IP 範圍，將流量導向至端點對應的使用者 IP 位址。
services: traffic-manager
documentationcenter: ''
author: rohinkoul
manager: twooley
ms.topic: article
ms.service: traffic-manager
ms.date: 09/18/2019
ms.author: rohink
ms.openlocfilehash: 818b692884bd9d31efd08663a582ebcfec2032e9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "76938479"
---
# <a name="traffic-manager-subnet-override-using-azure-cli"></a>使用 Azure CLI 流量管理員子網覆寫

流量管理員子網覆寫可讓您變更設定檔的路由方法。  新增覆寫將會根據使用者的 IP 位址，以預先定義的 IP 範圍將流量導向端點對應。 

## <a name="how-subnet-override-works"></a>子網覆寫的運作方式

將子網覆寫新增至流量管理員設定檔時，流量管理員會先檢查是否有使用者 IP 位址的子網覆寫。 如果找到，則會將使用者的 DNS 查詢導向至對應的端點。  如果找不到對應，流量管理員會回到設定檔的原始路由方法。 

IP 位址範圍可以指定為 CIDR 範圍（例如，1.2.3.0/24）或做為位址範圍（例如，1.2.3.4-5.6.7.8）。 與每個端點相關聯的 IP 範圍，對該端點而言必須是唯一的。 不同端點間的任何 IP 範圍重迭，會導致流量管理員拒絕設定檔。

有兩種類型的路由設定檔支援子網覆寫：

* **地理**-如果流量管理員找到 DNS 查詢之 IP 位址的子網覆寫，則會將該查詢路由至端點的健康情況所在的端點。
* **效能**-如果流量管理員找到 DNS 查詢之 IP 位址的子網覆寫，它只會將流量路由傳送至端點（如果狀況良好）。  如果子網覆寫端點狀況不良，流量管理員將會切換回效能路由啟發學習法。

## <a name="create-a-traffic-manager-subnet-override"></a>建立流量管理員子網覆寫

若要建立流量管理員子網覆寫，您可以使用 Azure CLI 將覆寫的子網新增至流量管理員端點。

## <a name="azure-cli"></a>Azure CLI

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

如果您選擇在本機安裝和使用 CLI，在執行本教學課程時，您必須執行 Azure CLI 2.0.28 版或更新版本。 若要尋找版本，請執行 `az --version`。 如果您需要安裝或升級，請參閱[安裝 Azure CLI]( /cli/azure/install-azure-cli)。

## <a name="update-the-traffic-manager-endpoint-with-subnet-override"></a>使用子網覆寫來更新流量管理員端點。
使用 Azure CLI，透過[az network 流量-manager 端點更新](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update)來更新端點。

```azurecli

### Add a range of IPs ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 1.2.3.4-5.6.7.8 \
    --type AzureEndpoints

### Add a subnet ###
az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --subnets 9.10.11.0:24 \
    --type AzureEndpoints

```

您可以使用 **--remove**選項執行[az network 流量-manager 端點更新](https://docs.microsoft.com/cli/azure/network/traffic-manager/endpoint?view=azure-cli-latest#az-network-traffic-manager-endpoint-update)，以移除 IP 位址範圍。

```azurecli

az network traffic-manager endpoint update \
    --name MyEndpoint \
    --profile-name MyTmProfile \
    --resource-group MyResourceGroup \
    --remove subnets \
    --type AzureEndpoints

```
## <a name="next-steps"></a>後續步驟
深入了解「流量管理員」的 [流量路由方法](traffic-manager-routing-methods.md)。

瞭解[子網流量路由方法](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-routing-methods#subnet-traffic-routing-method)