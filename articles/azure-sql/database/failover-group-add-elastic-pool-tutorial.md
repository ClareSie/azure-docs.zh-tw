---
title: 教學課程：將彈性集區新增到容錯移轉群組
description: 使用 Azure 入口網站、PowerShell 或 Azure CLI，將 Azure SQL Database 彈性集區新增至容錯移轉群組。
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: seo-lt-2019 sqldbrb=1
ms.devlang: ''
ms.topic: tutorial
author: stevestein
ms.author: sstein
ms.reviewer: ''
ms.date: 08/27/2019
ms.openlocfilehash: cdbc44158de2f24d7d33d68311979c3b8bdda85d
ms.sourcegitcommit: 1cf157f9a57850739adef72219e79d76ed89e264
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/13/2020
ms.locfileid: "94593972"
---
# <a name="tutorial-add-an-azure-sql-database-elastic-pool-to-a-failover-group"></a>教學課程：將 Azure SQL Database 彈性集區新增至容錯移轉群組
[!INCLUDE[appliesto-sqldb](../includes/appliesto-sqldb.md)]

使用 Azure 入口網站設定 Azure SQL Database 彈性集區的容錯移轉群組，並測試容錯移轉。  在本教學課程中，您將學會如何：

> [!div class="checklist"]
>
> - 建立單一資料庫。
> - 將資料庫新增至彈性集區。
> - 在兩個伺服器之間建立兩個彈性集區的[容錯移轉群組](auto-failover-group-overview.md)。
> - 測試容錯移轉。

## <a name="prerequisites"></a>必要條件

若要完成本教學課程，請確定您具有下列項目︰

- Azure 訂用帳戶。 [建立免費帳戶](https://azure.microsoft.com/free/) (如果您還沒有帳戶的話)。

## <a name="1---create-a-single-database"></a>1 - 建立單一資料庫

[!INCLUDE [sql-database-create-single-database](../includes/sql-database-create-single-database.md)]

## <a name="2---add-the-database-to-an-elastic-pool"></a>2 - 將資料庫新增至彈性集區

在此步驟中，您將建立彈性集區，並在其中新增您的資料庫。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

使用 Azure 入口網站建立彈性集區。

1. 在 Azure 入口網站的左側功能表中，選取 [Azure SQL]。 如果 **Azure SQL** 不在清單中，請選取 [所有服務]，然後在搜尋方塊中鍵入 "Azure SQL"。 (選用) 選取 **Azure SQL** 旁的星號將其設為最愛，並新增為左側導覽中的項目。
1. 選取 [+ 新增] 以開啟 [選取 SQL 部署選項] 頁面。 您可以選取 [資料庫] 圖格上的 [顯示詳細資料]，以查看不同資料庫的其他資訊。
1. 從 [SQL Database] 圖格的 [資源類型] 下拉式清單中，選取 [彈性集區]。 選取 [建立] 以建立彈性集區。

    ![選取彈性集區](./media/failover-group-add-elastic-pool-tutorial/select-azure-sql-elastic-pool.png)

1. 使用下列值設定您的彈性集區：
   - **名稱** ：提供彈性集區的唯一名稱，例如 `myElasticPool`。
   - 訂用帳戶：從下拉式清單中選取訂用帳戶。
   - **資源群組** ：從下拉式清單中，選取您在第 1 節中建立的資源群組 `myResourceGroup`。
   - **伺服器** ：從下拉式清單中，選取您在第 1 節中建立的伺服器。  

       ![為彈性集區建立新的伺服器](./media/failover-group-add-elastic-pool-tutorial/use-existing-server-for-elastic-pool.png)

   - **計算 + 儲存體** ：選取 [設定彈性集區] 以設定您的計算和儲存體，並將您的單一資料庫新增至彈性集區。 在 [集區設定] 索引標籤上保留預設值 Gen5，以及 2 個虛擬核心和 32gb。

1. 在 [設定] 頁面上選取 [資料庫] 索引標籤，然後選擇 [新增資料庫]。 選擇您在第 1 節中建立的資料庫，然後選取 [套用] 將其新增至您的彈性集區。 再次選取 [套用] 以套用您的彈性集區設定，然後關閉 [設定] 頁面。

    ![將資料庫新增至彈性集區](./media/failover-group-add-elastic-pool-tutorial/add-database-to-elastic-pool.png)

1. 選取 [檢閱 + 建立] 以檢閱您的彈性集區設定，然後選取 [建立] 以建立彈性集區。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用 PowerShell 建立彈性集區和次要伺服器。

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   $poolName = "myElasticPool"
   $databaseName = "mySampleDatabase"
   $drLocation = "West US"
   $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # The ip address range that you want to allow to access your server
   # Leaving at 0.0.0.0 will prevent outside-of-azure connections
   # $startIp = "0.0.0.0"
   # $endIp = "0.0.0.0"

   # Show randomized variables
   Write-host "DR Server name is" $drServerName
   Write-host "Failover group name is" $failoverGroupName

   # Create primary Gen5 elastic 2 vCore pool
   Write-host "Creating elastic pool..."
   $elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -ElasticPoolName $poolName `
       -Edition "GeneralPurpose" `
       -vCore 2 `
       -ComputeGeneration Gen5
   $elasticPool

   # Add single db into elastic pool
   Write-host "Creating elastic pool..."
   $addDatabase = Set-AzSqlDatabase -ResourceGroupName $resourceGroupName `
       -ServerName $serverName `
       -DatabaseName $databaseName `
       -ElasticPoolName $poolName
   $addDatabase
   ```

本教學課程的這個部分會使用下列 PowerShell Cmdlet：

| Command | 注意 |
|---|---|
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | 建立 Azure SQL Database 的彈性資料庫集區。|
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | 設定資料庫的屬性，或將現有資料庫移到彈性集區中。 |

---

## <a name="3---create-the-failover-group"></a>3 - 建立容錯移轉群組

在此步驟中，您將在現有的伺服器與另一個區域中的新伺服器之間建立[容錯移轉群組](auto-failover-group-overview.md)。 然後，將彈性集區新增至容錯移轉群組。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

使用 Azure 入口網站建立您的容錯移轉群組。

1. 在 [Azure 入口網站](https://portal.azure.com)的左側功能表中，選取 [Azure SQL]。 如果 **Azure SQL** 不在清單中，請選取 [所有服務]，然後在搜尋方塊中鍵入 Azure SQL。 (選用) 選取 **Azure SQL** 旁的星號將其設為最愛，並新增為左側導覽中的項目。
1. 選取在上一節中建立的彈性集區，例如 `myElasticPool`。
1. 在 [概觀] 窗格中選取 [伺服器名稱] 下的伺服器名稱，以開啟伺服器的設定。
  
    ![開啟彈性集區的伺服器](./media/failover-group-add-elastic-pool-tutorial/server-for-elastic-pool.png)

1. 選取 [設定] 窗格底下的 [容錯移轉群組]，然後選取 [新增群組]，以建立新的容錯移轉群組。

    ![新增容錯移轉群組](./media/failover-group-add-elastic-pool-tutorial/elastic-pool-failover-group.png)

1. 在 [容錯移轉群組] 頁面上輸入或選取下列值，然後選取 [建立]：
    - **容錯移轉群組名稱** ：輸入唯一的容錯移轉群組名稱，例如 `failovergrouptutorial`。
    - **次要伺服器** ：選取 [設定必要設定] 選項，然後選擇 [建立新的伺服器]。 或者，您也可以選擇已存在的伺服器作為次要伺服器。 在為您新的次要伺服器輸入下列值之後，選取 [選取]。
        - **伺服器名稱** ：輸入次要伺服器的唯一名稱，例如 `mysqlsecondary`。
        - **伺服器管理員登入** ：輸入 `azureuser`
        - **密碼** ：輸入符合密碼需求的複雜密碼。
        - **位置** ：從下拉式清單中選擇位置，例如 `East US`。 此位置不可與主要伺服器的位置相同。

       > [!NOTE]
       > 伺服器登入和防火牆設定必須符合主要伺服器的設定。

       ![建立容錯移轉群組的次要伺服器](./media/failover-group-add-elastic-pool-tutorial/create-secondary-failover-server.png)

1. 選取 [群組中的資料庫]，然後選取您在第 2 節中建立的彈性集區。 此時應該會出現警告，提示您在次要伺服器上建立彈性集區。 選取警告，然後選取 [確定]，以在次要伺服器上建立彈性集區。 

   ![將彈性集區新增至容錯移轉群組](./media/failover-group-add-elastic-pool-tutorial/add-elastic-pool-to-failover-group.png)

1. 選取 [選取]，將您的彈性集區設定套用至容錯移轉群組，然後選取 [建立] 以建立容錯移轉群組。 將彈性集區新增至容錯移轉群組，將會自動啟動異地複寫程序。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用 PowerShell 建立容錯移轉群組。

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $poolName = "myElasticPool"
   # $databaseName = "mySampleDatabase"
   # $drLocation = "West US"
   # $drServerName = "mysqlsecondary-$(Get-Random)"
   $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Create a secondary server in the failover region
   Write-host "Creating a secondary server in the failover region..."
   New-AzSqlServer -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -Location $drLocation `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential `
         -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
   Write-host "Secondary server =" $drServerName

   # Create a server firewall rule that allows access from the specified IP range
   Write-host "Configuring firewall for secondary server..."
   New-AzSqlServerFirewallRule -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   Write-host "Firewall configured"

   # Create secondary Gen5 elastic 2 vCore pool
   Write-host "Creating secondary elastic pool..."
   $elasticPool = New-AzSqlElasticPool -ResourceGroupName $resourceGroupName `
       -ServerName $drServerName `
       -ElasticPoolName $poolName `
       -Edition "GeneralPurpose" `
       -vCore 2 `
       -ComputeGeneration Gen5
   $elasticPool

   # Create a failover group between the servers
   Write-host "Creating failover group..."
   New-AzSqlDatabaseFailoverGroup `
     –ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -PartnerServerName $drServerName  `
      –FailoverGroupName $failoverGroupName `
      –FailoverPolicy Automatic `
      -GracePeriodWithDataLossHours 2
   Write-host "Failover group created successfully."

   # Add elastic pool to the failover group
   Write-host "Enumerating databases in elastic pool...."
   $FailoverGroup = Get-AzSqlDatabaseFailoverGroup `
                    -ResourceGroupName $resourceGroupName `
                    -ServerName $serverName `
                    -FailoverGroupName $failoverGroupName
   $databases = Get-AzSqlElasticPoolDatabase `
                  -ResourceGroupName $resourceGroupName `
                  -ServerName $serverName `
                  -ElasticPoolName $poolName
   Write-host "Adding databases to failover group..."
   $failoverGroup = $failoverGroup | Add-AzSqlDatabaseToFailoverGroup `
                                     -Database $databases
   $failoverGroup
   ```

本教學課程的這個部分會使用下列 PowerShell Cmdlet：

| Command | 注意 |
|---|---|
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | 建立裝載資料庫和彈性集區的伺服器。 |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | 建立伺服器的防火牆規則。 |
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | 建立 Azure SQL Database 的彈性集區。|
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | 建立新的容錯移轉群組。 |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | 將一或多個 Azure SQL 資料庫新增至容錯移轉群組。 |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 取得或列出 Azure SQL Database 容錯移轉群組。 |

---

## <a name="4---test-failover"></a>4 - 測試容錯移轉

在此步驟中，您會將容錯移轉群組容錯移轉到次要伺服器，然後使用 Azure 入口網站進行容錯回復。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

使用 Azure 入口網站測試容錯移轉群組的容錯移轉。

1. 在 [Azure 入口網站](https://portal.azure.com)的左側功能表中，選取 [Azure SQL]。 如果 **Azure SQL** 不在清單中，請選取 [所有服務]，然後在搜尋方塊中鍵入 Azure SQL。 (選用) 選取 **Azure SQL** 旁的星號將其設為最愛，並新增為左側導覽中的項目。
1. 選取在上一節中建立的彈性集區，例如 `myElasticPool`。
1. 在 [伺服器名稱] 底下選取伺服器的名稱，以開啟伺服器的設定。

    ![開啟彈性集區的伺服器](./media/failover-group-add-elastic-pool-tutorial/server-for-elastic-pool.png)

1. 選取 [設定] 窗格下的 [容錯移轉群組]，然後選擇您在第 2 節中建立的容錯移轉群組。
  
   ![從入口網站選取容錯移轉群組](./media/failover-group-add-elastic-pool-tutorial/select-failover-group.png)

1. 檢查哪個伺服器是主要的，哪個伺服器是次要的。
1. 從工作窗格中選取 [容錯移轉]，以容錯移轉包含彈性集區的容錯移轉群組。
1. 在通知您 TDS 工作階段將中斷的警告上，選取 [是]。

   ![容錯移轉包含您資料庫的容錯移轉群組](./media/failover-group-add-elastic-pool-tutorial/failover-sql-db.png)

1. 檢查哪個伺服器是主要的，哪個伺服器是次要的。 如果容錯移轉成功，這兩部伺服器應該已交換角色。
1. 再次選取 [容錯移轉]，將容錯移轉群組容錯回復至原始設定。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用 PowerShell 測試容錯移轉群組的容錯移轉。

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $poolName = "myElasticPool"
   # $databaseName = "mySampleDatabase"
   # $drLocation = "West US"
   # $drServerName = "mysqlsecondary-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Check role of secondary replica
   Write-host "Confirming the secondary server is secondary...."
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole

   # Failover to secondary server
   Write-host "Failing over failover group to the secondary..."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failover group failed over to" $drServerName
   ```

將容錯移轉群組容錯移轉至次要伺服器，然後使用 PowerShell 進行容錯回復。

   ```powershell-interactive
   # Set variables for your server and database
   # $subscriptionId = '<SubscriptionID>'
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"
   # $location = "East US"
   # $adminLogin = "azureuser"
   # $password = "PWD27!"+(New-Guid).Guid
   # $serverName = "mysqlserver-$(Get-Random)"
   # $poolName = "myElasticPool"
   # $databaseName = "mySampleDatabase"
   # $drLocation = "West US"
   # $drServerName = "mysqlsecondary-$(Get-Random)"
   # $failoverGroupName = "failovergrouptutorial-$(Get-Random)"

   # Check role of secondary replica
   Write-host "Confirming the secondary server is now primary"
   (Get-AzSqlDatabaseFailoverGroup `
      -FailoverGroupName $failoverGroupName `
      -ResourceGroupName $resourceGroupName `
      -ServerName $drServerName).ReplicationRole

   # Revert failover to primary server
   Write-host "Failing over failover group to the primary...."
   Switch-AzSqlDatabaseFailoverGroup `
      -ResourceGroupName $resourceGroupName `
      -ServerName $serverName `
      -FailoverGroupName $failoverGroupName
   Write-host "Failover group failed over to" $serverName
   ```

本教學課程的這個部分會使用下列 PowerShell Cmdlet：

| Command | 注意 |
|---|---|
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 取得或列出 Azure SQL Database 容錯移轉群組。 |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| 執行 Azure SQL Database 容錯移轉群組的容錯移轉。 |

---

## <a name="clean-up-resources"></a>清除資源

刪除資源群組，即可清除資源。

# <a name="portal"></a>[入口網站](#tab/azure-portal)

1. 瀏覽至您在 [Azure 入口網站](https://portal.azure.com)中的資源群組。
1. 選取 [刪除資源群組] 以刪除群組中的所有資源，以及資源群組本身。
1. 在文字方塊中輸入資源群組的名稱 (`myResourceGroup`)，然後選取 [刪除]，即可刪除該資源群組。

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

使用 PowerShell 清除您的資源。

   ```powershell-interactive
   # Set variables for your server and database
   # $resourceGroupName = "myResourceGroup-$(Get-Random)"

   # Clean up resources by removing the resource group
   Write-host "Removing resource group..."
   Remove-AzResourceGroup -ResourceGroupName $resourceGroupName
   Write-host "Resource group removed =" $resourceGroupName
   ```

本教學課程的這個部分會使用下列 PowerShell Cmdlet：

| Command | 注意 |
|---|---|
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 移除資源群組 |

---

> [!IMPORTANT]
> 如果您想要保留資源群組，但要刪除次要資料庫，請先將其從容錯移轉群組中移除，然後再將其刪除。 將次要資料庫從容錯移轉群組中移除之前先將其刪除，可能會導致無法預期的行為。

## <a name="full-script"></a>完整指令碼

# <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

[!code-powershell-interactive[main](../../../powershell_scripts/sql-database/failover-groups/add-elastic-pool-to-failover-group-az-ps.ps1 "Add elastic pool to a failover group")]

此指令碼會使用下列命令。 下表中的每個命令都會連結至命令特定的文件。

| Command | 注意 |
|---|---|
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | 建立用來存放所有資源的資源群組。 |
| [New-AzSqlServer](/powershell/module/az.sql/new-azsqlserver) | 建立裝載資料庫和彈性集區的伺服器。 |
| [New-AzSqlServerFirewallRule](/powershell/module/az.sql/new-azsqlserverfirewallrule) | 建立伺服器的防火牆規則。 |
| [New-AzSqlDatabase](/powershell/module/az.sql/new-azsqldatabase) | 建立資料庫。 |
| [New-AzSqlElasticPool](/powershell/module/az.sql/new-azsqlelasticpool) | 建立 Azure SQL Database 的彈性資料庫集區。|
| [Set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) | 設定資料庫的屬性，或將現有資料庫移到彈性集區中。 |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) | 建立新的容錯移轉群組。 |
| [Get-AzSqlDatabase](/powershell/module/az.sql/get-azsqldatabase) | 取得 SQL Database 中的一或多個資料庫。 |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup) | 將一或多個 Azure SQL 資料庫新增至容錯移轉群組。 |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | 取得或列出 Azure SQL Database 容錯移轉群組。 |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup)| 執行 Azure SQL Database 容錯移轉群組的容錯移轉。 |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | 移除資源群組 |

# <a name="portal"></a>[入口網站](#tab/azure-portal)

沒有可供 Azure 入口網站使用的指令碼。

---

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已將 Azure SQL Database 彈性集區新增至容錯移轉群組，並已測試容錯移轉。 您已了解如何︰

> [!div class="checklist"]
>
> - 建立單一資料庫。
> - 將資料庫新增至彈性集區。
> - 在兩個伺服器之間建立兩個彈性集區的[容錯移轉群組](auto-failover-group-overview.md)。
> - 測試容錯移轉。

請前進到下一個關於如何使用 DMS 進行移轉的教學課程。

> [!div class="nextstepaction"]
> [教學課程：使用 DMS 將 SQL Server 遷移至集區資料庫](../../dms/tutorial-sql-server-to-azure-sql.md?toc=/azure/sql-database/toc.json)
