---
title: 使用 Azure Key Vault 啟用 SQL TDE
titleSuffix: Azure SQL Database & SQL Managed Instance & Azure Synapse Analytics
description: 瞭解如何設定 Azure SQL Database 和 Azure Synapse Analytics，以開始使用透明資料加密 (TDE) ，以使用 PowerShell 或 Azure CLI 進行靜態加密。
services: sql-database
ms.service: sql-db-mi
ms.subservice: security
ms.custom: seo-lt-2019 sqldbrb=1, devx-track-azurecli
ms.devlang: ''
ms.topic: how-to
author: jaszymas
ms.author: jaszymas
ms.reviewer: vanto
ms.date: 03/12/2019
ms.openlocfilehash: b4480f3d28cb89165a6ba3c5b26b10b1aba9765c
ms.sourcegitcommit: 6a350f39e2f04500ecb7235f5d88682eb4910ae8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2020
ms.locfileid: "96461839"
---
# <a name="powershell-and-the-azure-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell 和 Azure CLI：使用來自 Azure Key Vault 的客戶管理金鑰來啟用透明資料加密
[!INCLUDE[appliesto-sqldb-sqlmi-asa](../includes/appliesto-sqldb-sqlmi-asa.md)]

本文將逐步解說如何使用 Azure Key Vault 的透明資料加密 (TDE) Azure SQL Database 或 Azure Synapse Analytics 的金鑰。 若要深入了解以 Azure Key Vault 整合進行 TDE - 攜帶您自己的金鑰 (BYOK) 的支援，請參閱 [Azure SQL 透明資料加密：「攜帶您自己的金鑰」支援](transparent-data-encryption-byok-overview.md)。

> [!NOTE] 
> Azure SQL 現在支援使用以 TDE 保護裝置形式儲存在受控 HSM 中的 RSA 金鑰。 這項功能處於 **公開預覽** 狀態。 Azure Key Vault 受控 HSM 是完全受控、高可用性、單一租使用者、符合標準的雲端服務，可讓您使用 FIPS 140-2 層級3驗證的 Hsm 來保護雲端應用程式的密碼編譯金鑰。 深入瞭解 [受管理的 hsm](../../key-vault/managed-hsm/index.yml)。

## <a name="prerequisites-for-powershell"></a>PowerShell 的必要條件

- 您必須具有 Azure 訂用帳戶，並且是該訂用帳戶的系統管理員。
- [建議執行的選擇性作業] 備妥硬體安全性模組 (HSM) 或本機金鑰存放區，用來建立 TDE 保護裝置金鑰內容的本機複本。
- 您必須已安裝且正在執行 Azure PowerShell。
- 建立要用於 TDE 的 Azure Key Vault 和金鑰。
  - [使用硬體安全性模組 (HSM) 與 Key Vault 的指示](../../key-vault/keys/hsm-protected-keys.md)
    - 金鑰保存庫必須具有下列屬性才能用於 TDE：
  - 虛[刪除](../../key-vault/general/soft-delete-overview.md)和清除保護
- 金鑰必須具有下列屬性才能用於 TDE：
  - 沒有到期日
  - 未停用
  - 能夠執行 *取得*、*包裝金鑰*、*解除包裝金鑰* 作業
- **預覽版中的 ()** 若要使用受管理的 HSM 金鑰，請依照指示 [使用 Azure CLI 來建立和啟用受控 hsm](../../key-vault/managed-hsm/quick-create-cli.md)

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

如需 Az 模組安裝指示，請參閱[安裝 Azure PowerShell](/powershell/azure/install-az-ps)。 如需特定的 Cmdlet，請參閱[AzureRM。](/powershell/module/AzureRM.Sql/)

如需 Key Vault 的詳細資訊，請參閱 [Key Vault 的 PowerShell 指示](../../key-vault/secrets/quick-create-powershell.md) ，以及 [如何使用 powershell Key Vault 虛刪除](../../key-vault/general/key-vault-recovery.md)。

> [!IMPORTANT]
> PowerShell Azure Resource Manager (RM) 模組仍受支援，但所有未來的開發都是針對 Az.Sql 模組進行的。 AzureRM 模組在至少 2020 年 12 月之前都還會持續收到 Bug 修正。  Az 模組和 AzureRm 模組中命令的引數本質上完全相同。 如需其相容性的詳細資訊，請參閱[新的 Azure PowerShell Az 模組簡介](/powershell/azure/new-azureps-module-az)。

## <a name="assign-an-azure-active-directory-azure-ad-identity-to-your-server"></a>將 Azure Active Directory (Azure AD) 身分識別指派給您的伺服器

如果您有現有的 [伺服器](logical-servers.md)，請使用下列程式在伺服器中新增 Azure Active Directory (Azure AD) 身分識別：

   ```powershell
   $server = Set-AzSqlServer -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -AssignIdentity
   ```

如果您要建立伺服器，請使用 [>new-azsqlserver](/powershell/module/az.sql/new-azsqlserver) 指令程式搭配標記身分識別，在伺服器建立期間新增 Azure AD 身分識別：

   ```powershell
   $server = New-AzSqlServer -ResourceGroupName <SQLDatabaseResourceGroupName> -Location <RegionName> `
       -ServerName <LogicalServerName> -ServerVersion "12.0" -SqlAdministratorCredentials <PSCredential> -AssignIdentity
   ```

## <a name="grant-key-vault-permissions-to-your-server"></a>為您的伺服器授與 Key Vault 權限

使用 [>set-azkeyvaultaccesspolicy](/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) 指令程式，將金鑰保存庫的存取權授與您的伺服器，然後再使用該金鑰保存庫中的金鑰來進行 TDE。

   ```powershell
   Set-AzKeyVaultAccessPolicy -VaultName <KeyVaultName> `
       -ObjectId $server.Identity.PrincipalId -PermissionsToKeys get, wrapKey, unwrapKey
   ```
若要在受控 HSM 上將許可權新增至您的伺服器，請將「受管理的 HSM 加密服務加密」本機 RBAC 角色新增至伺服器。 這可讓伺服器針對受管理的 HSM 中的金鑰執行 get、wrap key、解除包裝金鑰操作。
[在受控 HSM 上布建伺服器存取的指示](../../key-vault/managed-hsm/role-management.md)

## <a name="add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>將 Key Vault 金鑰新增至伺服器，並設定 TDE 保護裝置

- 使用 [AzKeyVaultKey](/powershell/module/az.keyvault/get-azkeyvaultkey?view=azps-2.4.0) 指令 Cmdlet 從 key vault 取出金鑰識別碼
- 使用 [AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey) 指令 Cmdlet 將 Key Vault 中的金鑰新增至伺服器。
- 使用 [AzSqlServerTransparentDataEncryptionProtector 指令程式](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) ，將金鑰設定為所有伺服器資源的 TDE 保護裝置。
- 使用 [AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/get-azsqlservertransparentdataencryptionprotector) 指令程式來確認已依照預期設定 TDE 保護裝置。

> [!NOTE]
> **預覽版中的 ()** 針對受管理的 HSM 金鑰，請使用 Az. Sql 2.11.1 版本的 PowerShell。

> [!NOTE]
> 金鑰保存庫名稱和金鑰名稱的合併長度不可超過 94 個字元。

> [!TIP]
> Key Vault 中的範例 KeyId： <br/>https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
> 受控 HSM 的範例 KeyId：<br/>https://contosoMHSM.managedhsm.azure.net/keys/myrsakey

```powershell
# add the key from Key Vault to the server
Add-AzSqlServerKeyVaultKey -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> -KeyId <KeyVaultKeyId>

# set the key as the TDE protector for all resources under the server
Set-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName> `
   -Type AzureKeyVault -KeyId <KeyVaultKeyId>

# confirm the TDE protector was configured as intended
Get-AzSqlServerTransparentDataEncryptionProtector -ResourceGroupName <SQLDatabaseResourceGroupName> -ServerName <LogicalServerName>
```

## <a name="turn-on-tde"></a>開啟 TDE

使用 [AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) 指令 Cmdlet 來開啟 TDE。

```powershell
Set-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName> -State "Enabled"
```

現在，資料庫或資料倉儲已使用 Key Vault 中的加密金鑰啟用 TDE。

## <a name="check-the-encryption-state-and-encryption-activity"></a>檢查加密狀態和加密活動

使用 [AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryption) 取得加密狀態，並使用 [AzSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/az.sql/get-azsqldatabasetransparentdataencryptionactivity) 檢查資料庫或資料倉儲的加密進度。

```powershell
# get the encryption state
Get-AzSqlDatabaseTransparentDataEncryption -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName> `

# check the encryption progress for a database or data warehouse
Get-AzSqlDatabaseTransparentDataEncryptionActivity -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> -DatabaseName <DatabaseName>  
```

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

若要安裝所需版本的 Azure CLI (2.0 版或更新版本) 並連接到您的 Azure 訂用帳戶，請參閱 [安裝和設定 Azure 跨平臺 Command-Line 介面 2.0](/cli/azure/install-azure-cli)。

如需 Key Vault 的詳細資訊，請參閱 [使用 cli 2.0 管理 Key Vault](../../key-vault/general/manage-with-cli2.md) 以及 [如何搭配 cli 使用 Key Vault 虛刪除](../../key-vault/general/key-vault-recovery.md)。

## <a name="assign-an-azure-ad-identity-to-your-server"></a>將 Azure AD 身分識別指派給您的伺服器

```azurecli
# create server (with identity) and database
az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
az sql db create --name <dbname> --server <servername> --resource-group <rgname>
```

> [!TIP]
> 防止"principalID" 建立伺服器，它是用於在下一步中指派金鑰保存庫使用權限的物件識別碼

## <a name="grant-key-vault-permissions-to-your-server"></a>為您的伺服器授與 Key Vault 權限

```azurecli
# create key vault, key and grant permission
az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
az keyvault key create --name <keyname> --vault-name <kvname> --protection software
az keyvault set-policy --name <kvname>  --object-id <objectid> --resource-group <rgname> --key-permissions wrapKey unwrapKey get
```

> [!TIP]
> 請保留新金鑰的金鑰 URI 或 keyID 以在下一個步驟中使用，例如：https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h

## <a name="add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>將 Key Vault 金鑰新增至伺服器，並設定 TDE 保護裝置

```azurecli
# add server key and update encryption protector
az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>
```

> [!NOTE]
> 金鑰保存庫名稱和金鑰名稱的合併長度不可超過 94 個字元。

## <a name="turn-on-tde"></a>開啟 TDE

```azurecli
# enable encryption
az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled
```

現在，資料庫或資料倉儲已使用 Azure Key Vault 中客戶管理的加密金鑰啟用 TDE。

## <a name="check-the-encryption-state-and-encryption-activity"></a>檢查加密狀態和加密活動

```azurecli
# get encryption scan progress
az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

# get whether encryption is on or off
az sql db tde show --database <dbname> --server <servername> --resource-group <rgname>
```

* * *

## <a name="useful-powershell-cmdlets"></a>PowerShell Cmdlet

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

- 使用 [AzSqlDatabaseTransparentDataEncryption](/powershell/module/az.sql/set-azsqldatabasetransparentdataencryption) 指令 Cmdlet 來關閉 TDE。

   ```powershell
   Set-AzSqlDatabaseTransparentDataEncryption -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName> `
      -DatabaseName <DatabaseName> -State "Disabled"
   ```

- 使用 [AzSqlServerKeyVaultKey 指令程式](/powershell/module/az.sql/get-azsqlserverkeyvaultkey) 可傳回已新增至伺服器的 Key Vault 索引鍵清單。

   ```powershell
   # KeyId is an optional parameter, to return a specific key version
   Get-AzSqlServerKeyVaultKey -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

- 使用 [AzSqlServerKeyVaultKey](/powershell/module/az.sql/remove-azsqlserverkeyvaultkey) 從伺服器移除 Key Vault 金鑰。

   ```powershell
   # the key set as the TDE Protector cannot be removed
   Remove-AzSqlServerKeyVaultKey -KeyId <KeyVaultKeyId> -ServerName <LogicalServerName> -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```

# <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

- 如需一般的資料庫設定，請參閱 [az sql](/cli/azure/sql)。

- 針對保存庫金鑰設定，請參閱 [az sql server key](/cli/azure/sql/server/key)。

- 如需 TDE 設定，請參閱 [az sql server TDE-key](/cli/azure/sql/server/tde-key) 和 [az sql db TDE](/cli/azure/sql/db/tde)。

* * *

## <a name="troubleshooting"></a>疑難排解

如果發生問題，請進行下列檢查：

- 如果找不到金鑰保存庫，請確定您是在正確的訂用帳戶中。

   # <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

   ```powershell
   Get-AzSubscription -SubscriptionId <SubscriptionId>
   ```

   # <a name="the-azure-cli"></a>[Azure CLI](#tab/azure-cli)

   ```powershell
   az account show - s <SubscriptionId>
   ```

   * * *

- 如果新的金鑰無法新增至伺服器，或新的金鑰無法更新為 TDE 保護裝置，請進行下列檢查：
   - 金鑰不應有到期日
   - 金鑰必須已啟用 *取得*、*包裝金鑰* 和 *解除包裝金鑰* 作業。

## <a name="next-steps"></a>後續步驟

- 瞭解如何輪替伺服器的 TDE 保護裝置，以符合安全性需求： [使用 PowerShell 輪替透明資料加密保護](transparent-data-encryption-byok-key-rotation.md)裝置。
- 如果有安全性風險，請瞭解如何移除可能遭盜用的 TDE 保護裝置： [移除可能遭盜用的金鑰](transparent-data-encryption-byok-remove-tde-protector.md)。
