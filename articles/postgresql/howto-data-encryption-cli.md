---
title: 資料加密 ─ Azure CLI - 用於後格雷SQL的 Azure 資料庫 - 單一伺服器
description: 瞭解如何使用 Azure CLI 為 PostgreSQL 單一伺服器設置和管理 Azure 資料庫的數據加密。
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: fcdd7c13c9e0a5f9e858309bea50bb0264b7b301
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460676"
---
# <a name="data-encryption-for-azure-database-for-postgresql-single-server-by-using-the-azure-cli"></a>使用 Azure CLI 為後格雷SQL 單個伺服器的 Azure 資料庫的資料加密

瞭解如何使用 Azure CLI 為 PostgreSQL 單個伺服器的 Azure 資料庫設置和管理數據加密。

## <a name="prerequisites-for-azure-cli"></a>Azure CLI 的先決條件

* 您必須具有 Azure 訂用帳戶，並且是該訂用帳戶的系統管理員。
* 建立用於客戶管理的金鑰的金鑰保管庫和密鑰。 還在密鑰保管庫上啟用清除保護和軟刪除。

    ```azurecli-interactive
    az keyvault create -g <resource_group> -n <vault_name> --enable-soft-delete true --enable-purge-protection true
    ```

* 在建立的 Azure 金鑰保管庫中,創建將用於 PostgreSQL 單個伺服器的 Azure 資料庫資料加密的金鑰。

    ```azurecli-interactive
    az keyvault key create --name <key_name> -p software --vault-name <vault_name>
    ```

* 為了使用現有的金鑰保管庫,它必須具有以下屬性才能用作客戶管理的密鑰:
  * [軟刪除](../key-vault/general/overview-soft-delete.md)

    ```azurecli-interactive
    az resource update --id $(az keyvault show --name \ <key_vault_name> -o tsv | awk '{print $1}') --set \ properties.enableSoftDelete=true
    ```

  * [清除受保護](../key-vault/general/overview-soft-delete.md#purge-protection)

    ```azurecli-interactive
    az keyvault update --name <key_vault_name> --resource-group <resource_group_name>  --enable-purge-protection true
    ```

* 金鑰必須具有以下屬性才能用作客戶管理的金鑰:
  * 沒有到期日
  * 未停用
  * 執行**取得**、**換行**與**展開**動作

## <a name="set-the-right-permissions-for-key-operations"></a>為金鑰設定正確的權限

1. 有兩種方法可以為 PostgreSQL 單一伺服器獲取 Azure 資料庫的託管標識。

    ### <a name="create-an-new-azure-database-for-mysql-server-with-a-managed-identity"></a>為具有託管標識的 MySQL 伺服器創建新的 Azure 資料庫。

    ```azurecli-interactive
    az postgres server create --name -g <resource_group> --location <locations> --storage-size <size>  -u <user>-p <pwd> --backup-retention <7> --sku-name <sku name> --geo-redundant-backup <Enabled/Disabled>  --assign-identity
    ```

    ### <a name="update-an-existing-the-azure-database-for-mysql-server-to-get-a-managed-identity"></a>更新 MySQL 伺服器的現有 Azure 資料庫以獲取託管標識。

    ```azurecli-interactive
    az postgres server update –name <server name>  -g <resoure_group> --assign-identity
    ```

2. 為**主體**設定**金鑰許可權**(**獲取**、**換行**、**解包**),這是 PostgreSQL 單個伺服器的名稱。

    ```azurecli-interactive
    az keyvault set-policy --name -g <resource_group> --key-permissions get unwrapKey wrapKey --object-id <principal id of the server>
    ```

## <a name="set-data-encryption-for-azure-database-for-postgresql-single-server"></a>為後格雷SQL單一伺服器設定 Azure 資料庫的資料加密

1. 使用 Azure 金鑰保管庫中創建的密鑰為 PostgreSQL 單一伺服器啟用 Azure 資料庫的數據加密。

    ```azurecli-interactive
    az postgres server key create –name  <server name>  -g <resource_group> --kid <key url>
    ```

    金鑰網址:https://YourVaultName.vault.azure.net/keys/YourKeyName/01234567890123456789012345678901>

## <a name="using-data-encryption-for-restore-or-replica-servers"></a>對回復伺服器或複本伺服器使用資料加密

在使用儲存在金鑰保管庫中的客戶託管金鑰對 PostgreSQL 單個伺服器的 Azure 資料庫進行加密後,對新創建的伺服器副本也進行加密。 您可以通過本地或異地還原操作或通過副本(本地/跨區域)操作製作此新副本。 因此,對於加密的 PostgreSQL 單伺服器伺服器,可以使用以下步驟創建加密還原的伺服器。

### <a name="creating-a-restoredreplica-server"></a>建立回復或原或複製伺服器

  *  [建立回復伺服器](howto-restore-server-cli.md) 
  *  [建立讀取複本伺服器](howto-read-replicas-cli.md) 

### <a name="once-the-server-is-restored-revalidate-data-encryption-the-restored-server"></a>還原伺服器後,重新驗證還原的伺服器的資料加密

    ```azurecli-interactive
    az postgres server key create –name  <server name> -g <resource_group> --kid <key url>
    ```

## <a name="additional-capability-for-the-key-being-used-for-the-azure-database-for-postgresql-single-server"></a>在後格雷SQL單個伺服器的 Azure 資料庫的金鑰的其他功能

### <a name="get-the-key-used"></a>使用金鑰

    ```azurecli-interactive
    az mysql server key show --name  <server name>  -g <resource_group> --kid <key url>
    ```

    Key url:  https://YourVaultName.vault.azure.net/keys/YourKeyName/01234567890123456789012345678901>

### <a name="list-the-key-used"></a>列出使用的金鑰

    ```azurecli-interactive
    az postgres server key list --name  <server name>  -g <resource_group>
    ```

### <a name="drop-the-key-being-used"></a>移除使用的金鑰

    ```azurecli-interactive
    az postgres server key delete -g <resource_group> --kid <key url> 
    ```
## <a name="using-an-azure-resource-manager-template-to-enable-data-encryption"></a>使用 Azure 資源管理員樣本開啟資料加密

除了 Azure 門戶之外,您還可以使用新伺服器和現有伺服器的 Azure 資源管理器範本為 PostgreSQL 單台伺服器在 Azure 資料庫上啟用數據加密。

### <a name="for-a-new-server"></a>對新伺服器

使用預先建立的 Azure 資源管理員樣本之一為伺服器預配啟用了資料加密:[資料加密範例](https://github.com/Azure/azure-postgresql/tree/master/arm-templates/ExampleWithDataEncryption)

此 Azure 資源管理器樣本為 PostgreSQL 單一伺服器創建 Azure 資料庫,並使用作為參數傳遞的**KeyVault**和**密鑰**在伺服器上啟用數據加密。

### <a name="for-an-existing-server"></a>對現有伺服器
此外,可以使用 Azure 資源管理器範本在現有的 Azure 資料庫上為 PostgreSQL 單個伺服器啟用數據加密。

* 傳遞之前在屬性物件`Uri`中的屬性下複製的 Azure 密鑰保管庫密鑰的資源 ID。

* 使用*2020-01-01 預覽*作為 API 版本。

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "location": {
      "type": "string"
    },
    "serverName": {
      "type": "string"
    },
    "keyVaultName": {
      "type": "string",
      "metadata": {
        "description": "Key vault name where the key to use is stored"
      }
    },
    "keyVaultResourceGroupName": {
      "type": "string",
      "metadata": {
        "description": "Key vault resource group name where it is stored"
      }
    },
    "keyName": {
      "type": "string",
      "metadata": {
        "description": "Key name in the key vault to use as encryption protector"
      }
    },
    "keyVersion": {
      "type": "string",
      "metadata": {
        "description": "Version of the key in the key vault to use as encryption protector"
      }
    }
  },
  "variables": {
    "serverKeyName": "[concat(parameters('keyVaultName'), '_', parameters('keyName'), '_', parameters('keyVersion'))]"
  },
  "resources": [
    {
      "type": "Microsoft.DBforPostgreSQL/servers",
      "apiVersion": "2017-12-01",
      "kind": "",
      "location": "[parameters('location')]",
      "identity": {
        "type": "SystemAssigned"
      },
      "name": "[parameters('serverName')]",
      "properties": {
      }
    },
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2019-05-01",
      "name": "addAccessPolicy",
      "resourceGroup": "[parameters('keyVaultResourceGroupName')]",
      "dependsOn": [
        "[resourceId('Microsoft.DBforPostgreSQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "resources": [
            {
              "type": "Microsoft.KeyVault/vaults/accessPolicies",
              "name": "[concat(parameters('keyVaultName'), '/add')]",
              "apiVersion": "2018-02-14-preview",
              "properties": {
                "accessPolicies": [
                  {
                    "tenantId": "[subscription().tenantId]",
                    "objectId": "[reference(resourceId('Microsoft.DBforPostgreSQL/servers/', parameters('serverName')), '2017-12-01', 'Full').identity.principalId]",
                    "permissions": {
                      "keys": [
                        "get",
                        "wrapKey",
                        "unwrapKey"
                      ]
                    }
                  }
                ]
              }
            }
          ]
        }
      }
    },
    {
      "name": "[concat(parameters('serverName'), '/', variables('serverKeyName'))]",
      "type": "Microsoft.DBforPostgreSQL/servers/keys",
      "apiVersion": "2020-01-01-preview",
      "dependsOn": [
        "addAccessPolicy",
        "[resourceId('Microsoft.DBforPostgreSQL/servers', parameters('serverName'))]"
      ],
      "properties": {
        "serverKeyType": "AzureKeyVault",
        "uri": "[concat(reference(resourceId(parameters('keyVaultResourceGroupName'), 'Microsoft.KeyVault/vaults/', parameters('keyVaultName')), '2018-02-14-preview', 'Full').properties.vaultUri, 'keys/', parameters('keyName'), '/', parameters('keyVersion'))]"
      }
    }
  ]
}
```

## <a name="next-steps"></a>後續步驟

 要瞭解有關資料加密的詳細資訊,請參閱[Azure 資料庫,用於 PostgreSQL 單一伺服器資料加密,並使用客戶管理的金鑰](concepts-data-encryption-postgresql.md)。
