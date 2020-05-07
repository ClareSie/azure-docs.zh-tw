---
title: 將資源部署至訂用帳戶
description: 描述如何在 Azure Resource Manager 範本中建立資源群組。 此外也會說明如何將資源部署到 Azure 訂用帳戶範圍。
ms.topic: conceptual
ms.date: 04/30/2020
ms.openlocfilehash: 80fe451f696480ec24b3d8eced64941de9492fef
ms.sourcegitcommit: 50ef5c2798da04cf746181fbfa3253fca366feaa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/30/2020
ms.locfileid: "82610814"
---
# <a name="create-resource-groups-and-resources-at-the-subscription-level"></a>在訂用帳戶層級建立資源群組和資源

若要簡化 Azure 訂用帳戶中的資源管理，您可以在整個訂用帳戶中定義和指派[原則](../../governance/policy/overview.md)或[角色型存取控制](../../role-based-access-control/overview.md)。 使用訂用帳戶層級範本時，您會以宣告方式套用原則，並在訂用帳戶指派角色。 您也可以建立資源群組和部署資源。

若要在訂用帳戶層級部署範本，請使用 Azure CLI、PowerShell 或 REST API。 Azure 入口網站不支援在訂用帳戶層級進行部署。

## <a name="supported-resources"></a>支援的資源

您可以在訂用帳戶層級部署下列資源類型：

* [藍圖](/azure/templates/microsoft.blueprint/blueprints)
* [對應](/azure/templates/microsoft.consumption/budgets)
* [部署](/azure/templates/microsoft.resources/deployments)-適用于部署至資源群組的嵌套範本。
* [eventSubscriptions](/azure/templates/microsoft.eventgrid/eventsubscriptions)
* [peerAsns](/azure/templates/microsoft.peering/2019-09-01-preview/peerasns)
* [policyAssignments](/azure/templates/microsoft.authorization/policyassignments)
* [policyDefinitions](/azure/templates/microsoft.authorization/policydefinitions)
* [policySetDefinitions](/azure/templates/microsoft.authorization/policysetdefinitions)
* [補救](/azure/templates/microsoft.policyinsights/2019-07-01/remediations)
* [resourceGroups](/azure/templates/microsoft.resources/resourcegroups)
* [roleAssignments](/azure/templates/microsoft.authorization/roleassignments)
* [roleDefinitions](/azure/templates/microsoft.authorization/roledefinitions)
* [scopeAssignments](/azure/templates/microsoft.managednetwork/scopeassignments)
* [supportPlanTypes](/azure/templates/microsoft.addons/supportproviders/supportplantypes)
* [標記](/azure/templates/microsoft.resources/tags)

### <a name="schema"></a>結構描述

您用於訂用帳戶層級部署的架構與資源群組部署的架構不同。

針對範本，請使用：

```json
https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#
```

所有部署範圍的參數檔案架構都相同。 針對參數檔案，請使用：

```json
https://schema.management.azure.com/schemas/2019-04-01/deploymentParameters.json#
```

## <a name="deployment-commands"></a>部署命令

訂用帳戶層級部署的命令與資源群組部署的命令不同。

針對 Azure CLI，請使用[az deployment sub create](/cli/azure/deployment/sub?view=azure-cli-latest#az-deployment-sub-create)。 下列範例會部署範本來建立資源群組：

```azurecli-interactive
az deployment sub create \
  --name demoSubDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" \
  --parameters rgName=demoResourceGroup rgLocation=centralus
```

針對 PowerShell 部署命令，請使用[new-azdeployment](/powershell/module/az.resources/new-azdeployment)或**AzSubscriptionDeployment**。 下列範例會部署範本來建立資源群組：

```azurepowershell-interactive
New-AzSubscriptionDeployment `
  -Name demoSubDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/emptyRG.json" `
  -rgName demoResourceGroup `
  -rgLocation centralus
```

針對 REST API，請使用 [[部署-在訂用帳戶範圍建立](/rest/api/resources/deployments/createorupdateatsubscriptionscope)]。

## <a name="deployment-location-and-name"></a>部署位置和名稱

針對訂用帳戶層級部署，您必須提供部署的位置。 部署的位置與您部署的資源位置不同。 部署位置會指定部署資料的儲存位置。

您可以提供部署的名稱，或使用預設的部署名稱。 預設名稱是範本檔案的名稱。 例如，部署名為 **azuredeploy.json** 的範本會建立預設的部署名稱 **azuredeploy**。

針對每個部署名稱，此位置是不可變的。 當不同位置有相同名稱的現有部署時，您無法在一個位置建立部署。 如果您收到錯誤代碼 `InvalidDeploymentLocation`，請使用不同的名稱或與先前該名稱部署相同的位置。

## <a name="use-template-functions"></a>使用範本函式

針對訂用帳戶層級部署，使用範本函式時有一些重要考量：

* **不**支援 [resourceGroup()](template-functions-resource.md#resourcegroup) 函式。
* 支援 [reference()](template-functions-resource.md#reference) 和 [list()](template-functions-resource.md#list) 函式。
* 使用[subscriptionResourceId （）](template-functions-resource.md#subscriptionresourceid)函數來取得在訂用帳戶層級部署之資源的資源識別碼。

  例如，若要取得原則定義的資源識別碼，請使用：
  
  ```json
  subscriptionResourceId('Microsoft.Authorization/roleDefinitions/', parameters('roleDefinition'))
  ```
  
  傳回的資源識別碼具有下列格式：

  ```json
  /subscriptions/{subscriptionId}/providers/{resourceProviderNamespace}/{resourceType}/{resourceName}
  ```

## <a name="create-resource-groups"></a>建立資源群組

若要在 Azure Resource Manager 範本中建立資源群組，請搭配資源群組的名稱和位置定義 [Microsoft.Resources/resourceGroups](/azure/templates/microsoft.resources/allversions) 資源。 您可以建立資源群組，並將資源部署至相同範本中的該資源群組。

下列範本會建立空的資源群組。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2018-05-01",
      "name": "[parameters('rgName')]",
      "location": "[parameters('rgLocation')]",
      "properties": {}
    }
  ],
  "outputs": {}
}
```

搭配資源群組使用 [copy 元素](copy-resources.md)來建立一個以上的資源群組。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgNamePrefix": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "instanceCount": {
      "type": "int"
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2018-05-01",
      "location": "[parameters('rgLocation')]",
      "name": "[concat(parameters('rgNamePrefix'), copyIndex())]",
      "copy": {
        "name": "rgCopy",
        "count": "[parameters('instanceCount')]"
      },
      "properties": {}
    }
  ],
  "outputs": {}
}
```

如需資源反復專案的詳細資訊，請參閱[在 Azure Resource Manager 範本中部署資源的多個實例](./copy-resources.md)和[教學課程：使用 Resource Manager 範本建立多個資源實例](./template-tutorial-create-multiple-instances.md)。

## <a name="resource-group-and-resources"></a>資源群組和資源

若要建立資源群組並對它部署資源，請使用巢狀範本。 巢狀範本能定義要部署至該資源群組的資源。 將巢狀範本設定為資源群組的相依項目，以確保該資源群組在部署資源之前確實存在。

下列範例會建立資源群組，並將儲存體帳戶部署至該資源群組。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "rgName": {
      "type": "string"
    },
    "rgLocation": {
      "type": "string"
    },
    "storagePrefix": {
      "type": "string",
      "maxLength": 11
    }
  },
  "variables": {
    "storageName": "[concat(parameters('storagePrefix'), uniqueString(subscription().id, parameters('rgName')))]"
  },
  "resources": [
    {
      "type": "Microsoft.Resources/resourceGroups",
      "apiVersion": "2018-05-01",
      "location": "[parameters('rgLocation')]",
      "name": "[parameters('rgName')]",
      "properties": {}
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2018-05-01",
      "name": "storageDeployment",
      "resourceGroup": "[parameters('rgName')]",
      "dependsOn": [
        "[resourceId('Microsoft.Resources/resourceGroups/', parameters('rgName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {},
          "variables": {},
          "resources": [
            {
              "type": "Microsoft.Storage/storageAccounts",
              "apiVersion": "2017-10-01",
              "name": "[variables('storageName')]",
              "location": "[parameters('rgLocation')]",
              "sku": {
                "name": "Standard_LRS"
              },
              "kind": "StorageV2"
            }
          ],
          "outputs": {}
        }
      }
    }
  ],
  "outputs": {}
}
```

## <a name="azure-policy"></a>Azure 原則

### <a name="assign-policy-definition"></a>指派原則定義

下列範例會將現有原則定義指派給訂用帳戶。 如果原則定義接受參數，請提供它們做為物件。 如果原則定義不接受參數，請使用預設的空白物件。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "policyDefinitionID": {
      "type": "string"
    },
    "policyName": {
      "type": "string"
    },
    "policyParameters": {
      "type": "object",
      "defaultValue": {}
    }
  },
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-03-01",
      "name": "[parameters('policyName')]",
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[parameters('policyDefinitionID')]",
        "parameters": "[parameters('policyParameters')]"
      }
    }
  ]
}
```

若要使用 Azure CLI 部署此範本，請使用：

```azurecli-interactive
# Built-in policy definition that accepts parameters
definition=$(az policy definition list --query "[?displayName=='Allowed locations'].id" --output tsv)

az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" \
  --parameters policyDefinitionID=$definition policyName=setLocation policyParameters="{'listOfAllowedLocations': {'value': ['westus']} }"
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell-interactive
$definition = Get-AzPolicyDefinition | Where-Object { $_.Properties.DisplayName -eq 'Allowed locations' }

$locations = @("westus", "westus2")
$policyParams =@{listOfAllowedLocations = @{ value = $locations}}

New-AzSubscriptionDeployment `
  -Name policyassign `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policyassign.json" `
  -policyDefinitionID $definition.PolicyDefinitionId `
  -policyName setLocation `
  -policyParameters $policyParams
```

### <a name="create-and-assign-policy-definitions"></a>建立並指派原則定義

您可以在相同的範本中[定義](../../governance/policy/concepts/definition-structure.md)和指派原則定義。

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2018-05-01/subscriptionDeploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {},
  "resources": [
    {
      "type": "Microsoft.Authorization/policyDefinitions",
      "apiVersion": "2018-05-01",
      "name": "locationpolicy",
      "properties": {
        "policyType": "Custom",
        "parameters": {},
        "policyRule": {
          "if": {
            "field": "location",
            "equals": "northeurope"
          },
          "then": {
            "effect": "deny"
          }
        }
      }
    },
    {
      "type": "Microsoft.Authorization/policyAssignments",
      "apiVersion": "2018-05-01",
      "name": "location-lock",
      "dependsOn": [
        "locationpolicy"
      ],
      "properties": {
        "scope": "[subscription().id]",
        "policyDefinitionId": "[resourceId('Microsoft.Authorization/policyDefinitions', 'locationpolicy')]"
      }
    }
  ]
}
```

若要在您的訂用帳戶中建立原則定義，並將它指派給訂用帳戶，請使用下列 CLI 命令：

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
New-AzSubscriptionDeployment `
  -Name definePolicy `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/policydefineandassign.json"
```

## <a name="azure-blueprints"></a>Azure 藍圖

### <a name="create-blueprint-definition"></a>建立藍圖定義

您可以從範本[建立](../../governance/blueprints/tutorials/create-from-sample.md)藍圖定義。

:::code language="json" source="~/quickstart-templates/subscription-level-deployments/blueprints-new-blueprint/azuredeploy.json":::

若要在您的訂用帳戶中建立藍圖定義，請使用下列 CLI 命令：

```azurecli
az deployment sub create \
  --name demoDeployment \
  --location centralus \
  --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-level-deployments/blueprints-new-blueprint/azuredeploy.json"
```

若要使用 PowerShell 部署此範本，請使用：

```azurepowershell
New-AzSubscriptionDeployment `
  -Name demoDeployment `
  -Location centralus `
  -TemplateUri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/subscription-level-deployments/blueprints-new-blueprint/azuredeploy.json"
```

## <a name="template-samples"></a>範本範例

* [建立資源群組、將其鎖定並授與許可權](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments/create-rg-lock-role-assignment)。
* [建立資源群組、原則和原則指派](https://github.com/Azure/azure-docs-json-samples/blob/master/subscription-level-deployment/azuredeploy.json)。

## <a name="next-steps"></a>後續步驟

* 若要瞭解如何指派角色，請參閱[使用 RBAC 和 Azure Resource Manager 範本來管理 Azure 資源的存取權](../../role-based-access-control/role-assignments-template.md)。
* 如需針對 Azure 資訊安全中心部署工作區設定的範例，請參閱 [deployASCwithWorkspaceSettings.json](https://github.com/krnese/AzureDeploy/blob/master/ARM/deployments/deployASCwithWorkspaceSettings.json)。
* 您可以在[GitHub](https://github.com/Azure/azure-quickstart-templates/tree/master/subscription-level-deployments)找到範例範本。
* 您也可以在[管理群組層級](deploy-to-management-group.md)和[租使用者層級](deploy-to-tenant.md)部署範本。
