---
title: Azure App Service 存取限制
description: 瞭解如何藉由指定存取限制，在 Azure App Service 中保護您的應用程式。
author: ccompy
ms.assetid: 3be1f4bd-8a81-4565-8a56-528c037b24bd
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 61ccc0231989589836e00088b9ca03d0cb49baca
ms.sourcegitcommit: 400f473e8aa6301539179d4b320ffbe7dfae42fe
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/28/2020
ms.locfileid: "92790945"
---
# <a name="azure-app-service-access-restrictions"></a>Azure App Service 存取限制

存取限制可讓您定義優先順序排序的允許/拒絕清單，以控制對您應用程式的網路存取。 此清單可以包含 IP 位址或 Azure 虛擬網路子網。 當有一或多個專案時，清單結尾會有隱含的「拒絕全部」。

存取限制功能適用于所有 App Service 託管工作負載，包括：web 應用程式、API 應用程式、Linux 應用程式、Linux 容器應用程式和功能。

對您的應用程式提出要求時，會根據存取限制清單中的 IP 位址規則來評估 [寄件者] 位址。 如果 [寄件者] 位址位於設定為 [服務端點] 的子網中，則會將來源子網與存取限制清單中的虛擬網路規則進行比較。 如果根據清單中的規則，不允許該位址進行存取，則服務會回覆 [HTTP 403](https://en.wikipedia.org/wiki/HTTP_403) 狀態碼。

存取限制功能是在 App Service 前端角色中執行，而這些角色是您的程式碼執行所在的背景工作主機上游。 因此，存取限制實際上是網路 Acl。

將 web 應用程式從 Azure 虛擬網路存取許可權制 (VNet) 的能力稱為「 [服務端點][serviceendpoints]」。 服務端點可讓您限制從選取的子網存取多租使用者服務。 將流量限制在 App Service 環境中裝載的應用程式時，並不會有任何作用。 如果您在 App Service 環境中，您可以使用 IP 位址規則來控制對您應用程式的存取。

> [!NOTE]
> 您必須在網路端和其所啟用的 Azure 服務上啟用服務端點。 如需支援服務端點的 Azure 服務清單，請參閱 [虛擬網路服務端點](../virtual-network/virtual-network-service-endpoints-overview.md)。
>

![存取限制流程](media/app-service-ip-restrictions/access-restrictions-flow.png)

## <a name="adding-and-editing-access-restriction-rules-in-the-portal"></a>在入口網站中新增和編輯存取限制規則 ##

若要將存取限制規則新增至應用程式，請使用功能表來開啟 [ **網路** > **存取限制** ]，然後按一下 [ **設定存取限制** ]

![App Service 網路功能選項](media/app-service-ip-restrictions/access-restrictions.png)  

從存取限制 UI，您可以查看為您的應用程式定義的存取限制規則清單。

![Azure 入口網站中 [存取限制] 畫面的螢幕擷取畫面，其中顯示針對所選應用程式定義的存取限制規則清單。](media/app-service-ip-restrictions/access-restrictions-browse.png)

此清單會顯示您應用程式上目前的所有限制。 如果您的應用程式上有 VNet 限制，則資料表會顯示是否已啟用 Microsoft 的服務端點。 當您的應用程式沒有定義的限制時，您的應用程式就可以從任何地方存取。  

## <a name="adding-ip-address-rules"></a>新增 IP 位址規則

您可以按一下 **[+] 新增規則** 來新增存取限制規則。 當您新增規則之後，該規則會立即生效。 規則會依據優先順序強制執行，從最小數字開始往上增加。 當您新增規則時 (即使只新增一個規則)，隱含的「拒絕所有」語句就會發生作用。

建立規則時，您必須選取 [允許]/[拒絕] 和 [規則類型]。 您也必須提供優先權值和您要限制存取的內容。  您可以選擇性地將名稱和描述新增至規則。  

![新增 IP 存取限制規則](media/app-service-ip-restrictions/access-restrictions-ip-add.png)

若要設定以 IP 位址為基礎的規則，請選取 IPv4 或 IPv6 類型。 針對 IPv4 和 IPv6 位址，CIDR 標記法中必須指定 IP 位址標記法。 若要指定正確的位址，您可以使用類似 1.2.3.4/32 的位址，其中前四個八位元代表您的 IP 位址，而 /32 是遮罩。 適用於所有位址的 IPv4 CIDR 標記法是 0.0.0.0/0。 若要深入了解 CIDR 標記法，您可以參閱[無類別網域間路由](https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing)。 

## <a name="service-endpoints"></a>服務端點

服務端點可讓您限制對選取的 Azure 虛擬網路子網的存取。 若要限制對特定子網的存取，請建立具有虛擬網路類型的限制規則。 您可以選擇您想要允許或拒絕存取的訂用帳戶、VNet 和子網。 如果尚未針對您選取的子網啟用服務端點，則會自動為您啟用，除非您核取此方塊。 如果您有在子網上啟用服務端點的許可權，您會想要在應用程式上啟用它，而非子網的情況下，您會有相關的情況。 如果您需要讓其他人在子網上啟用服務端點，您可以核取該方塊，並將您的應用程式設定為服務端點，以在稍後的子網上啟用。 

![新增 VNet 存取限制規則](media/app-service-ip-restrictions/access-restrictions-vnet-add.png)

服務端點無法用來限制存取在 App Service 環境中執行的應用程式。 當您的應用程式在 App Service 環境時，您可以使用 IP 存取規則來控制應用程式的存取權。 

透過服務端點，您可以使用應用程式閘道或其他 WAF 裝置來設定應用程式。 您也可以使用安全後端來設定多層式應用程式。 如需有關某些可能性的詳細資訊，請參閱 [網路功能和 App Service](networking-features.md) ，以及 [應用程式閘道與服務端點的整合](networking/app-gateway-with-service-endpoints.md)。

> [!NOTE]
> - 使用 IP SSL 虛擬 IP (VIP) 的 web 應用程式目前不支援服務端點。
> - IP 或服務端點限制有512個數據列的限制。 如果您需要超過512列的限制，建議您查看獨立的安全性產品，例如 Azure Front Door、Azure App 閘道，或 (WAF) 的 Web 應用程式防火牆。
>

## <a name="managing-access-restriction-rules"></a>管理存取限制規則

您可以按一下任何資料列，以編輯現有的存取限制規則。 編輯內容與優先順序的變更都會立即生效。

![[編輯 IP 限制] 對話方塊的螢幕擷取畫面，其中 Azure 入口網站顯示現有存取限制規則的欄位。](media/app-service-ip-restrictions/access-restrictions-ip-edit.png)

當您編輯規則時，無法在 IP 位址規則與虛擬網路規則之間變更類型。 

![[編輯 IP 限制] 對話方塊的螢幕擷取畫面，顯示虛擬網路規則的設定 Azure 入口網站。](media/app-service-ip-restrictions/access-restrictions-vnet-edit.png)

若要刪除規則，請按一下規則上的 [ **...** ]，然後按一下 [ **移除** ]。

![刪除存取限制規則](media/app-service-ip-restrictions/access-restrictions-delete.png)

## <a name="blocking-a-single-ip-address"></a>封鎖單一 IP 位址 ##

新增您的第一個 IP 限制規則時，服務會新增明確 **拒絕的所有** 規則，優先順序為2147483647。 在實務上，「明確 **拒絕全部** 」規則將會是最後一個執行的規則，而且將會封鎖存取未使用 **允許** 規則明確允許的任何 IP 位址。

如果使用者想要明確封鎖單一 IP 位址或 IP 位址區塊，但允許其他所有存取權，則必須新增明確的 **允許所有** 規則。

![封鎖單一 ip 位址](media/app-service-ip-restrictions/block-single-address.png)

## <a name="scm-site"></a>SCM 網站 

除了能夠控制對您應用程式的存取，您也可以限制對您應用程式所使用的 scm 網站的存取。 Scm 網站是 web deploy 端點，也是 Kudu 主控台。 您可以從應用程式將存取限制分開指派給 scm 網站，或針對應用程式和 scm 網站使用相同的集合。 當您核取此方塊與您的應用程式具有相同的限制時，所有專案都會遮蔽。如果您取消核取此方塊，則會套用先前在 scm 網站上的任何設定。 

![Azure 入口網站中 [存取限制] 畫面的螢幕擷取畫面，顯示未針對 scm 網站或應用程式設定任何存取限制。](media/app-service-ip-restrictions/access-restrictions-scm-browse.png)

## <a name="programmatic-manipulation-of-access-restriction-rules"></a>以程式設計方式操作存取限制規則 ##

[Azure CLI](/cli/azure/webapp/config/access-restriction?view=azure-cli-latest) 和 [Azure PowerShell](/powershell/module/Az.Websites/Add-AzWebAppAccessRestrictionRule?view=azps-3.1.0) 支援編輯存取限制。 使用 Azure CLI 新增存取限制的範例：

```azurecli-interactive
az webapp config access-restriction add --resource-group ResourceGroup --name AppName \
    --rule-name 'IP example rule' --action Allow --ip-address 122.133.144.0/24 --priority 100
```
使用 Azure PowerShell 新增存取限制的範例：

```azurepowershell-interactive
Add-AzWebAppAccessRestrictionRule -ResourceGroupName "ResourceGroup" -WebAppName "AppName"
    -Name "Ip example rule" -Priority 100 -Action Allow -IpAddress 122.133.144.0/24
```

您也可以使用 [Azure REST API](/rest/api/azure/) PUT 作業，在 Resource Manager 的應用程式設定上手動設定值，或使用 Azure Resource Manager 範本。 例如，您可以使用 resources.azure.com，並編輯 ipSecurityRestrictions 區塊來新增必要的 JSON。

您可以在資源管理員中的以下位置找到此資訊：

management.azure.com/subscriptions/ **subscription ID** /resourceGroups/ **resource groups** /providers/Microsoft.Web/sites/ **web app name** /config/web?api-version=2018-02-01

先前的 JSON 語法範例為：
```json
{
  "properties": {
    "ipSecurityRestrictions": [
      {
        "ipAddress": "122.133.144.0/24",
        "action": "Allow",
        "priority": 100,
        "name": "IP example rule"
      }
    ]
  }
}
```

## <a name="azure-functions-access-restrictions"></a>Azure Functions 存取限制

存取限制也適用于具有 App Service 方案相同功能的函式應用程式。 啟用存取限制將會停用任何不允許的 Ip 的入口網站程式碼編輯器。

## <a name="next-steps"></a>後續步驟
[Azure Functions 的存取限制](../azure-functions/functions-networking-options.md#inbound-ip-restrictions)

[應用程式閘道與服務端點的整合](networking/app-gateway-with-service-endpoints.md)

<!--Links-->
[serviceendpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
