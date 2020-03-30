---
title: 使用 Azure 資源管理器範本創建工作區
titleSuffix: Azure Machine Learning
description: 瞭解如何使用 Azure 資源管理器範本創建新的 Azure 機器學習工作區。
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 03/05/2020
ms.custom: seoapril2019
ms.openlocfilehash: 9403cc05ed5b31f3b76c16c4232506e2ddc5da2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "78402911"
---
[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]
<br>

# <a name="use-an-azure-resource-manager-template-to-create-a-workspace-for-azure-machine-learning"></a>使用 Azure 資源管理器範本為 Azure 機器學習創建工作區

在本文中，您將瞭解使用 Azure 資源管理器範本創建 Azure 機器學習工作區的多種方法。 Resource Manager 範本可讓您輕鬆地以單一、協調的作業建立資源。 範本是 JSON 文件，其定義部署所需的資源。 它也可以指定部署參數。 參數用來在使用範本時提供輸入值。

如需詳細資訊，請參閱 [使用 Azure Resource Manager 範本部署應用程式](../azure-resource-manager/templates/deploy-powershell.md)。

## <a name="prerequisites"></a>Prerequisites

* **Azure 訂閱**。 如果沒有，請嘗試[Azure 機器學習的免費或付費版本](https://aka.ms/AMLFree)。

* 若要從 CLI 使用範本，您需要 [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.2.0) 或 [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)。

## <a name="resource-manager-template"></a>Resource Manager 範本

以下資源管理器範本可用於創建 Azure 機器學習工作區和關聯的 Azure 資源：

[!code-json[create-azure-machine-learning-service-workspace](~/quickstart-templates/101-machine-learning-create/azuredeploy.json)]

此範本會建立下列 Azure 服務：

* Azure 資源群組
* Azure 儲存體帳戶
* Azure 金鑰保存庫
* Azure Application Insights
* Azure Container Registry
* Azure Machine Learning 工作區

資源群組是保存服務的容器。 各種服務是 Azure Machine Learning 工作區的必要項。

範例範本有兩個參數：

* **位置**是將建立資源群組和服務的位置。

    範本將會針對大部分的資源使用您選取的位置。 例外狀況是 Application Insights 服務，該服務在其他服務可用的所有位置都無法使用。 如果您選取的位置無法使用，服務會建立在美國中南部位置。

* **工作區名稱**是 Azure Machine Learning 工作區的易記名稱。

    > [!NOTE]
    > 工作區名稱不區分大小寫。

    其他服務的名稱會隨機產生。

> [!TIP]
> 與本文檔關聯的範本創建新的 Azure 容器註冊表時，還可以創建新工作區，而無需創建容器註冊表。 當您執行需要容器註冊表的操作時，將創建一個操作。 例如，訓練或部署模型。
>
> 您還可以在 Azure 資源管理器範本中引用現有容器註冊表或存儲帳戶，而不是創建新容器註冊表或存儲帳戶。

[!INCLUDE [machine-learning-delete-acr](../../includes/machine-learning-delete-acr.md)]

如需範本的詳細資訊，請參閱下列文章：

* [編寫 Azure Resource Manager 範本](../azure-resource-manager/templates/template-syntax.md)
* [使用 Azure Resource Manager 範本部署應用程式](../azure-resource-manager/templates/deploy-powershell.md)
* [Microsoft.MachineLearningServices resource 類型](https://docs.microsoft.com/azure/templates/microsoft.machinelearningservices/allversions)

### <a name="advanced-template"></a>高級範本

以下示例範本演示如何創建具有三個設置的工作區：

* 為工作區啟用高機密性設置
* 為工作區啟用加密
* 使用現有的 Azure 金鑰保存庫

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "workspaceName": {
      "type": "string",
      "metadata": {
        "description": "Specifies the name of the Azure Machine Learning workspace."
      }
    },
    "location": {
      "type": "string",
      "defaultValue": "southcentralus",
      "allowedValues": [
        "eastus",
        "eastus2",
        "southcentralus",
        "southeastasia",
        "westcentralus",
        "westeurope",
        "westus2"
      ],
      "metadata": {
        "description": "Specifies the location for all resources."
      }
    },
    "sku":{
      "type": "string",
      "defaultValue": "basic",
      "allowedValues": [
        "basic",
        "enterprise"
      ],
      "metadata": {
        "description": "Specifies the sku, also referred to as 'edition' of the Azure Machine Learning workspace."
      }
    },
    "hbi_workspace":{
      "type": "string",
      "defaultValue": "false",
      "allowedValues": [
        "false",
        "true"
      ],
      "metadata": {
        "description": "Specifies that the Azure Machine Learning workspace holds highly confidential data."
      }
    },
    "encryption_status":{
      "type": "string",
      "defaultValue": "Disabled",
      "allowedValues": [
        "Enabled",
        "Disabled"
      ],
      "metadata": {
        "description": "Specifies if the Azure Machine Learning workspace should be encrypted with the customer managed key."
      }
    },
    "cmk_keyvault":{
      "type": "string",
      "metadata": {
        "description": "Specifies the customer managed keyvault Resource Manager ID."
      }
    },
    "resource_cmk_uri":{
      "type": "string",
      "metadata": {
        "description": "Specifies the customer managed keyvault key uri."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat('sa',uniqueString(resourceGroup().id))]",
    "storageAccountType": "Standard_LRS",
    "keyVaultName": "[concat('kv',uniqueString(resourceGroup().id))]",
    "tenantId": "[subscription().tenantId]",
    "applicationInsightsName": "[concat('ai',uniqueString(resourceGroup().id))]",
    "containerRegistryName": "[concat('cr',uniqueString(resourceGroup().id))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "apiVersion": "2018-07-01",
      "name": "[variables('storageAccountName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "[variables('storageAccountType')]"
      },
      "kind": "StorageV2",
      "properties": {
        "encryption": {
          "services": {
            "blob": {
              "enabled": true
            },
            "file": {
              "enabled": true
            }
          },
          "keySource": "Microsoft.Storage"
        },
        "supportsHttpsTrafficOnly": true
      }
    },
    {
      "type": "Microsoft.KeyVault/vaults",
      "apiVersion": "2018-02-14",
      "name": "[variables('keyVaultName')]",
      "location": "[parameters('location')]",
      "properties": {
        "tenantId": "[variables('tenantId')]",
        "sku": {
          "name": "standard",
          "family": "A"
        },
        "accessPolicies": []
      }
    },
    {
      "type": "Microsoft.Insights/components",
      "apiVersion": "2015-05-01",
      "name": "[variables('applicationInsightsName')]",
      "location": "[if(or(equals(parameters('location'),'eastus2'),equals(parameters('location'),'westcentralus')),'southcentralus',parameters('location'))]",
      "kind": "web",
      "properties": {
        "Application_Type": "web"
      }
    },
    {
      "type": "Microsoft.ContainerRegistry/registries",
      "apiVersion": "2017-10-01",
      "name": "[variables('containerRegistryName')]",
      "location": "[parameters('location')]",
      "sku": {
        "name": "Standard"
      },
      "properties": {
        "adminUserEnabled": true
      }
    },
    {
      "type": "Microsoft.MachineLearningServices/workspaces",
      "apiVersion": "2020-01-01",
      "name": "[parameters('workspaceName')]",
      "location": "[parameters('location')]",
      "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",
        "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]",
        "[resourceId('Microsoft.ContainerRegistry/registries', variables('containerRegistryName'))]"
      ],
      "identity": {
        "type": "systemAssigned"
      },
      "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
      },
      "properties": {
        "friendlyName": "[parameters('workspaceName')]",
        "keyVault": "[resourceId('Microsoft.KeyVault/vaults',variables('keyVaultName'))]",
        "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
        "containerRegistry": "[resourceId('Microsoft.ContainerRegistry/registries',variables('containerRegistryName'))]",
        "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]",
         "encryption": {
                "status": "[parameters('encryption_status')]",
                "keyVaultProperties": {
                    "keyVaultArmId": "[parameters('cmk_keyvault')]",
                    "keyIdentifier": "[parameters('resource_cmk_uri')]"
                  }
            },
        "hbi_workspace": "[parameters('hbi_workspace')]"
      }
    }
  ]
}
```

要獲取金鑰保存庫的 ID 以及此範本所需的金鑰 URI，可以使用 Azure CLI。 以下命令是使用 Azure CLI 獲取金鑰保存庫資源識別碼 和 URI 的示例：

```azurecli-interactive
az keyvault show --name mykeyvault --resource-group myresourcegroup --query "[id, properties.vaultUri]"
```

此命令返回類似于以下文本的值。 第一個值是 ID，第二個值是 URI：

```text
[
  "/subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault",
  "https://mykeyvault.vault.azure.net/"
]
```

## <a name="use-the-azure-portal"></a>使用 Azure 入口網站

1. 遵循[從自訂範本部署資源](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-portal#deploy-resources-from-custom-template)的步驟。 當您看見 [編輯範本]____ 畫面時，貼入此文件的範本。
1. 選取 [儲存]____ 以使用範本。 提供下列資訊，並同意列出的條款及條件：

   * 訂閱：選擇要用於這些資源的 Azure 訂閱。
   * 資源組：選擇或創建資源組以包含服務。
   * 工作區名稱：要用於要創建的 Azure 機器學習工作區的名稱。 工作區名稱必須介於 3 到 33 個字元之間。 只能包含英數字元和 '-'。
   * 位置：選擇將創建資源的位置。

如需詳細資訊，請參閱[從自訂範本部署資源](../azure-resource-manager/templates/deploy-portal.md#deploy-resources-from-custom-template)。

## <a name="use-azure-powershell"></a>使用 Azure PowerShell

這個範例假設您已將範本儲存到目前目錄中名為 `azuredeploy.json` 的檔案：

```powershell
New-AzResourceGroup -Name examplegroup -Location "East US"
new-azresourcegroupdeployment -name exampledeployment `
  -resourcegroupname examplegroup -location "East US" `
  -templatefile .\azuredeploy.json -workspaceName "exampleworkspace" -sku "basic"
```

如需詳細資訊，請參閱[使用 Resource Manager 範本與 Azure PowerShell 來部署資源](../azure-resource-manager/templates/deploy-powershell.md)和[使用 SAS 權杖和 Azure PowerShell 部署私用 Resource Manager 範本](../azure-resource-manager/templates/secure-template-with-sas-token.md)。

## <a name="use-the-azure-cli"></a>使用 Azure CLI

這個範例假設您已將範本儲存到目前目錄中名為 `azuredeploy.json` 的檔案：

```azurecli-interactive
az group create --name examplegroup --location "East US"
az group deployment create \
  --name exampledeployment \
  --resource-group examplegroup \
  --template-file azuredeploy.json \
  --parameters workspaceName=exampleworkspace location=eastus sku=basic
```

如需詳細資訊，請參閱[使用 Resource Manager 範本與 Azure CLI 來部署資源](../azure-resource-manager/templates/deploy-cli.md)和[使用 SAS 權杖和 Azure CLI 部署私用 Resource Manager 範本](../azure-resource-manager/templates/secure-template-with-sas-token.md)。

## <a name="troubleshooting"></a>疑難排解

### <a name="resource-provider-errors"></a>資來源提供者錯誤

[!INCLUDE [machine-learning-resource-provider](../../includes/machine-learning-resource-provider.md)]

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure 金鑰保存庫訪問策略和 Azure 資源管理器範本

使用 Azure 資源管理器範本創建工作區和相關資源（包括 Azure 金鑰保存庫）時，請多次使用。 例如，在持續集成和部署管道中多次使用具有相同參數的範本。

通過範本創建資源的大多數操作都是冪等的，但每次使用範本時，金鑰保存庫都會清除訪問策略。 清除訪問策略會破壞對正在使用它的任何現有工作區的金鑰保存庫的訪問。 例如，停止/創建 Azure 筆記本 VM 的功能可能會失敗。  

為了避免此問題，我們建議採用以下方法之一：

* 不要多次為相同的參數部署範本。 或者在使用範本重新創建現有資源之前刪除這些資源。

* 檢查金鑰保存庫訪問策略，然後使用這些策略設置範本`accessPolicies`的屬性。 要查看訪問策略，請使用以下 Azure CLI 命令：

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query properties.accessPolicies
    ```

    有關使用範本`accessPolicies`部分的詳細資訊，請參閱[AccessPolicyentry 物件引用](https://docs.microsoft.com/azure/templates/Microsoft.KeyVault/2018-02-14/vaults#AccessPolicyEntry)。

* 檢查金鑰保存庫資源是否已存在。 如果是，請不要通過範本重新創建它。 例如，要使用現有的金鑰保存庫而不是創建新金鑰保存庫，請使用以下更改範本：

    * **添加**接受現有金鑰保存庫資源的 ID 的參數：

        ```json
        "keyVaultId":{
          "type": "string",
          "metadata": {
            "description": "Specify the existing Key Vault ID."
          }
        }
      ```

    * **刪除**創建金鑰保存庫資源的部分：

        ```json
        {
          "type": "Microsoft.KeyVault/vaults",
          "apiVersion": "2018-02-14",
          "name": "[variables('keyVaultName')]",
          "location": "[parameters('location')]",
          "properties": {
            "tenantId": "[variables('tenantId')]",
            "sku": {
              "name": "standard",
              "family": "A"
            },
            "accessPolicies": [
            ]
          }
        },
        ```

    * **Remove**從工作區`"[resourceId('Microsoft.KeyVault/vaults', variables('keyVaultName'))]",``dependsOn`部分刪除行。 此外**Change**，更改`keyVault`工作區`properties`部分中的條目以傳址參數`keyVaultId`：

        ```json
        {
          "type": "Microsoft.MachineLearningServices/workspaces",
          "apiVersion": "2019-11-01",
          "name": "[parameters('workspaceName')]",
          "location": "[parameters('location')]",
          "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
            "[resourceId('Microsoft.Insights/components', variables('applicationInsightsName'))]"
          ],
          "identity": {
            "type": "systemAssigned"
          },
          "sku": {
            "tier": "[parameters('sku')]",
            "name": "[parameters('sku')]"
          },
          "properties": {
            "friendlyName": "[parameters('workspaceName')]",
            "keyVault": "[parameters('keyVaultId')]",
            "applicationInsights": "[resourceId('Microsoft.Insights/components',variables('applicationInsightsName'))]",
            "storageAccount": "[resourceId('Microsoft.Storage/storageAccounts/',variables('storageAccountName'))]"
          }
        }
        ```

    進行這些更改後，您可以在運行範本時指定現有金鑰保存庫資源的 ID。 然後，範本將通過將工作區`keyVault`的屬性設置為其 ID 來重用金鑰保存庫。

    要獲取金鑰保存庫的 ID，可以引用運行的原始範本的輸出或使用 Azure CLI。 以下命令是使用 Azure CLI 獲取金鑰保存庫資源識別碼 的示例：

    ```azurecli-interactive
    az keyvault show --name mykeyvault --resource-group myresourcegroup --query id
    ```

    此命令會傳回類似下列文字的值：

    ```text
    /subscriptions/{subscription-guid}/resourceGroups/myresourcegroup/providers/Microsoft.KeyVault/vaults/mykeyvault
    ```

## <a name="next-steps"></a>後續步驟

* [使用資源管理器範本和資源管理器 REST API 部署資源](../azure-resource-manager/templates/deploy-rest.md)。
* [通過視覺化工作室創建和部署 Azure 資源組](../azure-resource-manager/templates/create-visual-studio-deployment-project.md)。
