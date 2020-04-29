---
title: 設定 Azure Front 的 IP 限制 WAF 規則
description: 瞭解如何設定 Web 應用程式防火牆規則，以限制現有 Azure Front 入口端點的 IP 位址。
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: article
ms.date: 03/26/2020
ms.author: tyao
ms.openlocfilehash: 077f127648688b25d45b433fa2bc94ee011b3f2d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/28/2020
ms.locfileid: "80336087"
---
# <a name="configure-an-ip-restriction-rule-with-a-web-application-firewall-for-azure-front-door"></a>使用適用于 Azure Front 的 Web 應用程式防火牆來設定 IP 限制規則

本文說明如何使用 Azure 入口網站、Azure CLI、Azure PowerShell 或 Azure Resource Manager 範本，在 Azure Front 的 Web 應用程式防火牆（WAF）中設定 IP 限制規則。

以 IP 位址為基礎的存取控制規則是自訂的 WAF 規則，可讓您控制對 web 應用程式的存取。 它會以無類別網域間路由（CIDR）格式來指定 IP 位址或 IP 位址範圍的清單來執行這項工作。

根據預設，您的 web 應用程式可以從網際網路存取。 如果您想要從已知的 IP 位址或 IP 位址範圍清單限制用戶端的存取權，您可以建立包含 IP 位址清單的 IP 比對規則做為符合的值，並將運算子設為 "Not" （否定為 true）和要**封鎖**的動作。 套用 IP 限制規則之後，源自此允許清單以外位址的要求會收到403禁止的回應。

## <a name="configure-a-waf-policy-with-the-azure-portal"></a>使用 Azure 入口網站設定 WAF 原則

### <a name="prerequisites"></a>先決條件

遵循[快速入門：建立高可用性全域 web 應用程式的 Front 門](../../frontdoor/quickstart-create-front-door.md)中所述的指示，建立 Azure Front 門板設定檔。

### <a name="create-a-waf-policy"></a>建立 WAF 原則

1. 在 [Azure 入口網站上，選取 [**建立資源**]，在搜尋方塊中輸入**web 應用程式防火牆**，然後選取 [ **WEB 應用程式防火牆（WAF）**]。
2. 選取 [建立]  。
3. 在 [**建立 WAF 原則**] 頁面上，使用下列值來完成 [**基本**] 索引標籤：
   
   |設定  |值  |
   |---------|---------|
   |的原則     |全球 WAF （Front）|
   |訂用帳戶     |選取您的訂用帳戶|
   |資源群組     |選取您的前門所在的資源群組。|
   |原則名稱     |輸入原則的名稱|
   |原則狀態     |啟用|

   選取**下一步：原則設定**

1. 在 [**原則設定**] 索引標籤上，選取 [**預防**]。 針對 [**封鎖回應主體**]，輸入*您已被封鎖！* 因此，您可以看到您的自訂規則已生效。
2. 選取 **[下一步：受控規則]**。
3. 選取 **[下一步：自訂規則]**。
4. 選取 [**新增自訂規則**]。
5. 在 [**新增自訂規則**] 頁面上，使用下列測試值來建立自訂規則：

   |設定  |值  |
   |---------|---------|
   |自訂規則名稱     |FdWafCustRule|
   |狀態     |啟用|
   |規則型別     |比對|
   |優先順序    |100|
   |相符類型     |IP 位址|
   |Match 變數|RemoteAddr|
   |作業|不包含|
   |IP 位址或範圍|10.10.10.0/24|
   |結果為|拒絕流量|

   :::image type="content" source="../media/waf-front-door-configure-ip-restriction/custom-rule.png" alt-text="自訂規則":::

   選取 [新增]  。
6. 選取 **[下一步：關聯]**。
7. 選取 [**新增前端主機**]。
8. 針對 [**前端主機**]，選取您的前端主機，然後選取 [**新增**]。
9. 選取 [檢閱 + 建立]  。
10. 通過原則驗證之後，請選取 [**建立**]。

### <a name="test-your-waf-policy"></a>測試您的 WAF 原則

1. WAF 原則部署完成後，請流覽至您的 Front 前端主機名稱。
2. 您應該會看到您的自訂封鎖訊息。

   :::image type="content" source="../media/waf-front-door-configure-ip-restriction/waf-rule-test.png" alt-text="WAF 規則測試":::

   > [!NOTE]
   > 私人 IP 位址是刻意用於自訂規則，以確保規則會觸發。 在實際部署中，針對您的特定情況，使用 IP 位址建立*允許*和*拒絕*規則。

## <a name="configure-a-waf-policy-with-the-azure-cli"></a>使用 Azure CLI 設定 WAF 原則

### <a name="prerequisites"></a>先決條件
在您開始設定 IP 限制原則之前，請先設定您的 CLI 環境，並建立 Azure Front profile。

#### <a name="set-up-the-azure-cli-environment"></a>設定 Azure CLI 環境
1. 安裝[Azure CLI](/cli/azure/install-azure-cli)，或使用 Azure Cloud Shell。 Azure Cloud Shell 是免費的 Bash Shell，您可以直接在 Azure 入口網站內執行。 它具有預先安裝和設定的 Azure CLI，可與您的帳戶搭配使用。 在接下來的 CLI 命令中選取 [**試試看**] 按鈕，然後在開啟的 Cloud Shell 會話中登入您的 Azure 帳戶。 會話啟動之後，請輸入`az extension add --name front-door`以新增 Azure Front 門板擴充功能。
 2. 如果您是在 Bash 中于本機使用 CLI，請使用`az login`登入 Azure。

#### <a name="create-an-azure-front-door-profile"></a>建立 Azure Front 門板設定檔
遵循[快速入門：建立高可用性全域 web 應用程式的 Front 門](../../frontdoor/quickstart-create-front-door.md)中所述的指示，建立 Azure Front 門板設定檔。

### <a name="create-a-waf-policy"></a>建立 WAF 原則

使用[az network front WAF-policy create](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-create)命令來建立 WAF 原則。 在接下來的範例中，將原則名稱*IPAllowPolicyExampleCLI*取代為唯一的原則名稱。

```azurecli-interactive 
az network front-door waf-policy create \
  --resource-group <resource-group-name> \
  --subscription <subscription ID> \
  --name IPAllowPolicyExampleCLI
  ```
### <a name="add-a-custom-ip-access-control-rule"></a>新增自訂 IP 存取控制規則

使用[az network front waf-policy custom-rule create](/cli/azure/ext/front-door/network/front-door/waf-policy/rule?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-rule-create)命令，為您剛建立的 waf 原則新增自訂 IP 存取控制規則。

在下列範例中：
-  將*IPAllowPolicyExampleCLI*取代為您稍早建立的唯一原則。
-  以您自己的範圍取代*ip-位址範圍 1*， *ip 位址範圍為 2* 。

首先，為上一個步驟所建立的原則建立 IP 允許規則。 
> [!NOTE]
> **--延遲**是必要的，因為規則必須在下一個步驟中加入比對條件。

```azurecli
az network front-door waf-policy rule create \
  --name IPAllowListRule \
  --priority 1 \
  --rule-type MatchRule \
  --action Block \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI --defer
```
接下來，將符合條件新增至規則：

```azurecli
az network front-door waf-policy rule match-condition add \
--match-variable RemoteAddr \
--operator IPMatch \
--values "ip-address-range-1" "ip-address-range-2" \
--negate true \
--name IPAllowListRule \
  --resource-group <resource-group-name> \
  --policy-name IPAllowPolicyExampleCLI 
  ```
                                                   
### <a name="find-the-id-of-a-waf-policy"></a>尋找 WAF 原則的識別碼 
使用[az network front WAF-policy show](/cli/azure/ext/front-door/network/front-door/waf-policy?view=azure-cli-latest#ext-front-door-az-network-front-door-waf-policy-show)命令來尋找 WAF 原則的識別碼。 以您稍早建立的唯一原則取代下列範例中的*IPAllowPolicyExampleCLI* 。

   ```azurecli
   az network front-door  waf-policy show \
     --resource-group <resource-group-name> \
     --name IPAllowPolicyExampleCLI
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-front-end-host"></a>將 WAF 原則連結至 Azure Front 前端主機

使用[az network Front update](/cli/azure/ext/front-door/network/front-door?view=azure-cli-latest#ext-front-door-az-network-front-door-update)命令，將 Azure Front *WebApplicationFirewallPolicyLink* ID 設定為原則識別碼。 將*IPAllowPolicyExampleCLI*取代為您稍早建立的唯一原則。

   ```azurecli
   az network front-door update \
     --set FrontendEndpoints[0].WebApplicationFirewallPolicyLink.id=/subscriptions/<subscription ID>/resourcegroups/resource-group-name/providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/IPAllowPolicyExampleCLI \
     --name <frontdoor-name>
     --resource-group <resource-group-name>
   ```
在此範例中，WAF 原則會套用至**FrontendEndpoints [0]**。 您可以將 WAF 原則連結至您的任何前端。
> [!Note]
> 您只需要將**WebApplicationFirewallPolicyLink**屬性設定為一次，即可將 WAF 原則連結到 Azure Front 門板前端。 後續的原則更新會自動套用至前端。

## <a name="configure-a-waf-policy-with-azure-powershell"></a>使用 Azure PowerShell 設定 WAF 原則

### <a name="prerequisites"></a>先決條件
在您開始設定 IP 限制原則之前，請先設定您的 PowerShell 環境，並建立 Azure Front 門板設定檔。

#### <a name="set-up-your-powershell-environment"></a>設定 PowerShell 環境
Azure PowerShell 提供一組 Cmdlet，可使用[Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)模型來管理 Azure 資源。

您可以在本機電腦上安裝 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview)，並將其用於任何 PowerShell 工作階段。 依照頁面上的指示，使用您的 Azure 認證登入 PowerShell，然後安裝 Az 模組。

1. 使用下列命令連線至 Azure，然後使用互動式對話方塊來登入。
    ```
    Connect-AzAccount
    ```
 2. 在安裝 Azure Front 門板模組之前，請確定您已安裝最新版本的 PowerShellGet 模組。 執行下列命令，然後重新開啟 PowerShell。

    ```
    Install-Module PowerShellGet -Force -AllowClobber
    ``` 

3. 使用下列命令安裝 FrontDoor 模組。 
    
    ```
    Install-Module -Name Az.FrontDoor
    ```
### <a name="create-an-azure-front-door-profile"></a>建立 Azure Front 門板設定檔
遵循[快速入門：建立高可用性全域 web 應用程式的 Front 門](../../frontdoor/quickstart-create-front-door.md)中所述的指示，建立 Azure Front 門板設定檔。

### <a name="define-an-ip-match-condition"></a>定義 IP 比對條件
使用[AzFrontDoorWafMatchConditionObject](/powershell/module/az.frontdoor/new-azfrontdoorwafmatchconditionobject)命令來定義 IP 比對條件。
在下列範例中，以您自己的範圍取代*ip 位址範圍 1*， *ip 位址範圍為 2* 。    
```powershell
$IPMatchCondition = New-AzFrontDoorWafMatchConditionObject `
-MatchVariable  RemoteAddr `
-OperatorProperty IPMatch `
-MatchValue "ip-address-range-1", "ip-address-range-2"
-NegateCondition 1
```
     
### <a name="create-a-custom-ip-allow-rule"></a>建立自訂 IP 允許規則

使用[AzFrontDoorWafCustomRuleObject](/powershell/module/Az.FrontDoor/New-azfrontdoorwafcustomruleobject)命令來定義動作並設定優先順序。 在下列範例中，將會封鎖不是來自用戶端 Ip 且符合清單的要求。

```azurepowershell
$IPAllowRule = New-AzFrontDoorWafCustomRuleObject `
-Name "IPAllowRule" `
-RuleType MatchRule `
-MatchCondition $IPMatchCondition `
-Action Block -Priority 1
```

### <a name="configure-a-waf-policy"></a>設定 WAF 原則
使用`Get-AzResourceGroup`來尋找包含 Azure Front 設定檔的資源組名。 接下來，使用[AzFrontDoorWafPolicy](/powershell/module/az.frontdoor/new-azfrontdoorwafpolicy)來設定具有 IP 規則的 WAF 原則。

```azurepowershell
  $IPAllowPolicyExamplePS = New-AzFrontDoorWafPolicy `
    -Name "IPRestrictionExamplePS" `
    -resourceGroupName <resource-group-name> `
    -Customrule $IPAllowRule`
    -Mode Prevention `
    -EnabledState Enabled
   ```

### <a name="link-a-waf-policy-to-an-azure-front-door-front-end-host"></a>將 WAF 原則連結至 Azure Front 前端主機

將 WAF 原則物件連結至現有的前端主機，並更新 Azure Front 門板屬性。 首先，使用[AzFrontDoor](/powershell/module/Az.FrontDoor/Get-AzFrontDoor)來取得 Azure Front 物件。 接下來，使用[AzFrontDoor](/powershell/module/Az.FrontDoor/Set-AzFrontDoor)命令，將**WebApplicationFirewallPolicyLink**屬性設定為上一個步驟中所建立 *$IPAllowPolicyExamplePS*的資源識別碼。

```azurepowershell
  $FrontDoorObjectExample = Get-AzFrontDoor `
    -ResourceGroupName <resource-group-name> `
    -Name $frontDoorName
  $FrontDoorObjectExample[0].FrontendEndpoints[0].WebApplicationFirewallPolicyLink = $IPBlockPolicy.Id
  Set-AzFrontDoor -InputObject $FrontDoorObjectExample[0]
```

> [!NOTE]
> 在此範例中，WAF 原則會套用至**FrontendEndpoints [0]**。 您可以將 WAF 原則連結至您的任何前端。 您只需要將**WebApplicationFirewallPolicyLink**屬性設定為一次，即可將 WAF 原則連結到 Azure Front 門板前端。 後續的原則更新會自動套用至前端。


## <a name="configure-a-waf-policy-with-a-resource-manager-template"></a>使用 Resource Manager 範本來設定 WAF 原則
若要查看建立 Azure Front policy 原則的範本，以及具有自訂 IP 限制規則的 WAF 原則，請移至[GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/201-front-door-waf-clientip)。


## <a name="next-steps"></a>後續步驟

- 瞭解如何[建立 Azure Front 門板設定檔](../../frontdoor/quickstart-create-front-door.md)。
