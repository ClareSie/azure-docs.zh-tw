---
title: 使用 PowerShell 備份和恢復 Azure VM
description: 介紹如何使用使用 PowerShell 的 Azure 備份備份和恢復 Azure VM
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 733a06a84aa170f1361ea74d126ec9752586fce2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247978"
---
# <a name="back-up-and-restore-azure-vms-with-powershell"></a>使用 PowerShell 備份和恢復 Azure VM

本文介紹如何使用 PowerShell Cmdlet 在[Azure 備份](backup-overview.md)恢復服務保存庫中備份和還原 Azure VM。

在本文中，您將了解如何：

> [!div class="checklist"]
>
> * 建立復原服務保存庫並設定保存庫內容。
> * 定義備份原則
> * 套用備份原則以保護多部虛擬機器
> * 觸發受保護虛擬機器的隨選備份作業。在備份 (或保護) 虛擬機器之前，您必須先完成[先決條件](backup-azure-arm-vms-prepare.md)來備妥保護 VM 的環境。

## <a name="before-you-start"></a>開始之前

* [瞭解有關](backup-azure-recovery-services-vault-overview.md)恢復服務保存庫的更多詳細資訊。
* [查看](backup-architecture.md#architecture-built-in-azure-vm-backup)Azure VM 備份的體系結構，[瞭解](backup-azure-vms-introduction.md)備份過程，並[查看](backup-support-matrix-iaas.md)支援、限制和先決條件。
* 查看恢復服務的 PowerShell 物件層次結構。

## <a name="recovery-services-object-hierarchy"></a>復原服務物件階層

物件層次結構總結如下圖。

![復原服務物件階層](./media/backup-azure-vms-arm-automation/recovery-services-object-hierarchy.png)

查看 Azure 庫中的**Az.恢復服務** [Cmdlet 引用](https://docs.microsoft.com/powershell/module/Az.RecoveryServices/?view=azps-1.4.0)。

## <a name="set-up-and-register"></a>設置和註冊

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

開始：

1. [下載最新版本的 PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps)

2. 輸入下列命令，以找到可用的 Azure 備份 PowerShell Cmdlet：

    ```powershell
    Get-Command *azrecoveryservices*
    ```

    將顯示 Azure備份、Azure Site Recovery 與復原服務保存庫的別名與 Cmdlet。 下圖是您將看到的範例。 它不是 Cmdlet 的完整清單。

    ![復原服務清單](./media/backup-azure-vms-automation/list-of-recoveryservices-ps.png)

3. 使用 **Connect-AzAccount** 登入您的 Azure 帳戶。 此 Cmdlet 會開啟網頁，提示您輸入帳戶認證：

    * 或者，您可以使用 **-Credential** 參數，以參數形式將您的帳戶認證加入 **Connect-AzAccount** Cmdlet。
    * 如果您是代表租用戶工作的 CSP 合作夥伴，請使用客戶的 tenantID 或租用戶主要網域名稱將客戶指定為租用戶。 例如：**連接-AzAccount - 租戶"fabrikam.com"**

4. 由於一個帳戶可以有多個訂用帳戶，因此您必須將要使用的訂用帳戶與帳戶建立關聯：

    ```powershell
    Select-AzSubscription -SubscriptionName $SubscriptionName
    ```

5. 如果首次使用 Azure 備份，則必須使用**[註冊-AzResourceProvider](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** Cmdlet 向訂閱註冊 Azure 恢復服務提供程式。

    ```powershell
    Register-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

6. 您可以使用下列命令，確認提供者已成功註冊：

    ```powershell
    Get-AzResourceProvider -ProviderNamespace "Microsoft.RecoveryServices"
    ```

    在命令輸出中，**RegistrationState** 應該變更為 **Registered**。 如果沒有，只需再次運行**[註冊-Az資來源提供者](https://docs.microsoft.com/powershell/module/az.resources/register-azresourceprovider)** Cmdlet。

## <a name="create-a-recovery-services-vault"></a>建立復原服務保存庫

下列步驟將引導您完成建立復原服務保存庫。 復原服務保存庫不同於備份保存庫。

1. 復原服務保存庫是一項 Resource Manager 資源，因此您必須將它放在資源群組內。 您可以使用現有資源組，或使用**[New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)** Cmdlet 創建資源組。 建立資源群組時，請指定資源群組的名稱與位置。  

    ```powershell
    New-AzResourceGroup -Name "test-rg" -Location "West US"
    ```

2. 使用 [New-AzRecoveryServicesVault](https://docs.microsoft.com/powershell/module/az.recoveryservices/new-azrecoveryservicesvault?view=azps-1.4.0) Cmdlet 來建立復原服務保存庫。 請務必為保存庫指定與用於資源群組相同的位置。

    ```powershell
    New-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "test-rg" -Location "West US"
    ```

3. 指定要使用的儲存體備援類型；您可以使用[本地備援儲存體 (LRS)](../storage/common/storage-redundancy-lrs.md) 或[異地備援儲存體 (GRS)](../storage/common/storage-redundancy-grs.md)。 下列範例顯示 testvault 的 -BackupStorageRedundancy 選項設為 GeoRedundant。

    ```powershell
    $vault1 = Get-AzRecoveryServicesVault -Name "testvault"
    Set-AzRecoveryServicesBackupProperty  -Vault $vault1 -BackupStorageRedundancy GeoRedundant
    ```

   > [!TIP]
   > 許多 Azure 備份 Cmdlet 都需要將復原服務保存庫物件當做輸入。 基於這個理由，將備份復原服務保存庫物件儲存在變數中會是方便的做法。
   >
   >

## <a name="view-the-vaults-in-a-subscription"></a>在訂用帳戶中檢視保存庫

要查看訂閱中的所有保存庫，請使用[獲取-AzRecovery 服務Vault](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesvault?view=azps-1.4.0)：

```powershell
Get-AzRecoveryServicesVault
```

輸出會類似下列的範例，請注意，會提供相關聯的 ResourceGroupName 與位置。

```output
Name              : Contoso-vault
ID                : /subscriptions/1234
Type              : Microsoft.RecoveryServices/vaults
Location          : WestUS
ResourceGroupName : Contoso-docs-rg
SubscriptionId    : 1234-567f-8910-abc
Properties        : Microsoft.Azure.Commands.RecoveryServices.ARSVaultProperties
```

## <a name="back-up-azure-vms"></a>備份 Azure VM

使用復原服務保存庫來保護您的虛擬機器。 在您套用保護之前，請設定保存庫內容 (保存庫中所保護的資料類型)，並確認保護原則。 保護原則是備份工作何時執行，以及每個備份快照之保留時間長度的排程。

### <a name="set-vault-context"></a>設定保存庫內容

在 VM 上啟用保護之前，請使用[Set-AzRecoveryServicesVaultCoNtext](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesvaultcontext?view=azps-1.4.0)來設置保存庫上下文。 保存庫內容設定之後就會套用至所有後續的 Cmdlet。 下列範例會設定保存庫 *testvault* 的保存庫內容。

```powershell
Get-AzRecoveryServicesVault -Name "testvault" -ResourceGroupName "Contoso-docs-rg" | Set-AzRecoveryServicesVaultContext
```

### <a name="fetch-the-vault-id"></a>獲取保存庫 ID

我們計畫根據 Azure PowerShell 準則棄用保存庫上下文設置。 相反，您可以存儲或提取保存庫 ID，並將其傳遞給相關命令。 因此，如果您尚未設置保存庫上下文或想要指定為特定保存庫運行的命令，請將保存庫 ID 作為"-vaultID"傳遞給所有相關命令，如下所示：

```powershell
$targetVault = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault"
$targetVault.ID
```

Or

```powershell
$targetVaultID = Get-AzRecoveryServicesVault -ResourceGroupName "Contoso-docs-rg" -Name "testvault" | select -ExpandProperty ID
```

### <a name="modifying-storage-replication-settings"></a>修改存儲複製設置

使用[Set-Az 恢復服務備份屬性](https://docs.microsoft.com/powershell/module/az.recoveryservices/Set-AzRecoveryServicesBackupProperty)命令將保存庫的存儲複製配置設置為 LRS/GRS

```powershell
Set-AzRecoveryServicesBackupProperty -Vault $targetVault -BackupStorageRedundancy GeoRedundant/LocallyRedundant
```

> [!NOTE]
> 只有當保存庫中沒有受保護的備份項目時，才可以修改儲存體備援。

### <a name="create-a-protection-policy"></a>建立保護原則

當您建立復原服務保存庫時，它會隨附預設的保護和保留原則。 預設保護原則會在每天的指定時間觸發備份作業。 預設保留原則會將每日復原點保留 30 天。 您可以使用預設原則來快速地保護 VM，並在之後編輯原則的各種詳細資料。

使用**[獲取恢復服務備份保護原則](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy)** 查看保存庫中可用的保護原則。 您可以使用此 Cmdlet 來取得特定的原則，或檢視與工作負載類型相關聯的原則。 下列範例會取得工作負載類型 AzureVM 的原則。

```powershell
Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM" -VaultId $targetVault.ID
```

輸出類似於下列範例：

```output
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
DefaultPolicy        AzureVM            AzureVM              4/14/2016 5:00:00 PM
```

> [!NOTE]
> PowerShell 中 BackupTime 欄位的時區是 UTC。 不過，當備份時間顯示在 Azure 入口網站中時，系統會根據您的當地時區調整時間。
>
>

備份保護原則至少與一個保留原則相關聯。 保留原則定義復原點在刪除之前保留的時間。

* 使用 [Get-AzRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupretentionpolicyobject) 檢視預設保留原則。
* 同樣，您可以使用[獲取恢復服務備份計畫策略物件](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupschedulepolicyobject)來獲取預設計畫策略。
* [New-AzRecoveryServicesBackupProtectionPolicy](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupprotectionpolicy) Cmdlet 會建立 PowerShell 物件來保存備份原則資訊。
* 排程和保留原則物件可當作 New-AzRecoveryServicesBackupProtectionPolicy Cmdlet 的輸入。

預設情況下，在計畫策略物件中定義開始時間。 使用以下示例將開始時間更改為所需的開始時間。 所需的開始時間也應為 UTC。 下面的示例假定每日備份所需的開始時間為 01：00 UTC AM。

```powershell
$schPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM" -VaultId $targetVault.ID
$UtcTime = Get-Date -Date "2019-03-20 01:00:00Z"
$UtcTime = $UtcTime.ToUniversalTime()
$schpol.ScheduleRunTimes[0] = $UtcTime
```

> [!IMPORTANT]
> 您只需提供 30 分鐘倍數的開始時間。 在上面的示例中，它只能是"01：00：00"或"02：30：00"。 開始時間不能為"01：15：00"

下列範例會將排程原則和保留原則儲存在變數中。 這個範例在建立保護原則 *NewPolicy* 時會使用這些變數來定義參數。

```powershell
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM" -VaultId $targetVault.ID
New-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -WorkloadType "AzureVM" -RetentionPolicy $retPol -SchedulePolicy $schPol -VaultId $targetVault.ID
```

輸出類似於下列範例：

```output
Name                 WorkloadType       BackupManagementType BackupTime                DaysOfWeek
----                 ------------       -------------------- ----------                ----------
NewPolicy           AzureVM            AzureVM              4/24/2016 1:30:00 AM
```

### <a name="enable-protection"></a>啟用保護。

在定義保護原則之後，您仍然必須對項目啟用此原則。 使用[啟用-Az 恢復服務備份保護](https://docs.microsoft.com/powershell/module/az.recoveryservices/enable-azrecoveryservicesbackupprotection)啟用保護。 啟用保護需要兩個物件：項目和原則。 一旦原則與保存庫相關聯，備份工作流程將依照原則排程定義的時間觸發。

> [!IMPORTANT]
> 使用 PS 同時為多個 VM 啟用備份時，請確保單個策略與其關聯的 VM 不超過 100 個。 這是[建議的最佳做法](https://docs.microsoft.com/azure/backup/backup-azure-vm-backup-faq#is-there-a-limit-on-number-of-vms-that-can-beassociated-with-a-same-backup-policy)。 目前，如果有超過 100 部 VM，但已規劃在未來新增檢查，則 PS 用戶端不會明確封鎖。

下列範例會使用原則 NewPolicy 來對項目 V2VM 啟用保護。 這些範例根據 VM 是否加密以及加密的類型而有所不同。

在**非加密 Resource Manager VM** 上啟用保護：

```powershell
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1" -VaultId $targetVault.ID
```

若要在加密的 VM 上 (使用 BEK 與 KEK 加密) 啟用保護，您必須提供權限讓 Azure 備份服務讀取金鑰保存庫中的金鑰與祕密。

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToKeys backup,get,list -PermissionsToSecrets get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1" -VaultId $targetVault.ID
```

若要在**加密的 VM 上 (只使用 BEK 加密)** 啟用保護，您必須提供權限讓 Azure 備份服務讀取金鑰保存庫中的密碼。

```powershell
Set-AzKeyVaultAccessPolicy -VaultName "KeyVaultName" -ResourceGroupName "RGNameOfKeyVault" -PermissionsToSecrets backup,get,list -ServicePrincipalName 262044b1-e2ce-469f-a196-69ab7ada62d3
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Policy $pol -Name "V2VM" -ResourceGroupName "RGName1" -VaultId $targetVault.ID
```

> [!NOTE]
> 如果使用 Azure 政府雲，請使用值 ff281ffe-705c-4f53-9f37-a40e6f2c68f3 作為[Set-AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/az.keyvault/set-azkeyvaultaccesspolicy) Cmdlet 中的參數"服務主體名稱"。
>

## <a name="monitoring-a-backup-job"></a>監視備份工作

您不必透過 Azure 入口網站就可以監視長時間執行的作業，例如備份作業。 要獲取正在進行的作業的狀態，請使用[獲取-Az 恢復服務備份作業](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupjob)Cmdlet。 此 Cmdlet 會取得特定保存庫 (在保存庫內容中指定) 的備份作業。 下列範例會以陣列形式取得進行中作業的狀態，並將狀態儲存在 $joblist 變數中。

```powershell
$joblist = Get-AzRecoveryservicesBackupJob –Status "InProgress" -VaultId $targetVault.ID
$joblist[0]
```

輸出類似於下列範例：

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM             Backup               InProgress            4/23/2016                5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

使用[等待-AzRecoveryServicesBackupJobCmdlet，](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob)而不是輪詢這些作業以完成這些作業（這是不必要的附加代碼）。 此 Cmdlet 會暫停執行，直到工作完成，或達到指定的逾時值為止。

```powershell
Wait-AzRecoveryServicesBackupJob -Job $joblist[0] -Timeout 43200 -VaultId $targetVault.ID
```

## <a name="manage-azure-vm-backups"></a>管理 Azure VM 備份

### <a name="modify-a-protection-policy"></a>修改保護原則

要修改保護原則，請使用[集-AzRecoveryServices 備份保護原則](https://docs.microsoft.com/powershell/module/az.recoveryservices/set-azrecoveryservicesbackupprotectionpolicy)來修改計畫策略或保留原則物件。

#### <a name="modifying-scheduled-time"></a>修改計畫時間

創建保護原則時，預設情況下會為其分配啟動時間。 以下示例演示如何修改保護原則的開始時間。

````powershell
$SchPol = Get-AzRecoveryServicesBackupSchedulePolicyObject -WorkloadType "AzureVM"
$UtcTime = Get-Date -Date "2019-03-20 01:00:00Z" (This is the time that the customer wants to start the backup)
$UtcTime = $UtcTime.ToUniversalTime()
$SchPol.ScheduleRunTimes[0] = $UtcTime
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Set-AzRecoveryServicesBackupProtectionPolicy -Policy $pol  -SchedulePolicy $SchPol -VaultId $targetVault.ID
````

#### <a name="modifying-retention"></a>修改保留

下列範例會將復原點保留變更為 365 天。

```powershell
$retPol = Get-AzRecoveryServicesBackupRetentionPolicyObject -WorkloadType "AzureVM"
$retPol.DailySchedule.DurationCountInDays = 365
$pol = Get-AzRecoveryServicesBackupProtectionPolicy -Name "NewPolicy" -VaultId $targetVault.ID
Set-AzRecoveryServicesBackupProtectionPolicy -Policy $pol  -RetentionPolicy $RetPol -VaultId $targetVault.ID
```

#### <a name="configuring-instant-restore-snapshot-retention"></a>配置即時還原快照保留

> [!NOTE]
> 從 Az PS 版本 1.6.0 開始，可以使用 Powershell 更新策略中的即時還原快照保留期

````powershell
$bkpPol = Get-AzRecoveryServicesBackupProtectionPolicy -WorkloadType "AzureVM" -VaultId $targetVault.ID
$bkpPol.SnapshotRetentionInDays=7
Set-AzRecoveryServicesBackupProtectionPolicy -policy $bkpPol -VaultId $targetVault.ID
````

預設值為 2，使用者可以將值設置為最小值 1，最大值為 5。 對於每週備份策略，期間設置為 5，不能更改。

### <a name="trigger-a-backup"></a>觸發備份

使用[備份-Az 恢復服務備份專案](https://docs.microsoft.com/powershell/module/az.recoveryservices/backup-azrecoveryservicesbackupitem)觸發備份作業。 如果是初始備份，則會是完整備份。 後續的備份會採用增量複本。 下面的示例採用 VM 備份以保留 60 天。

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM" -VaultId $targetVault.ID
$item = Get-AzRecoveryServicesBackupItem -Container $namedContainer -WorkloadType "AzureVM" -VaultId $targetVault.ID
$endDate = (Get-Date).AddDays(60).ToUniversalTime()
$job = Backup-AzRecoveryServicesBackupItem -Item $item -VaultId $targetVault.ID -ExpiryDateTimeUTC $endDate
```

輸出類似於下列範例：

```output
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   ----------
V2VM              Backup              InProgress          4/23/2016                  5:00:30 PM                cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

> [!NOTE]
> PowerShell 中 StartTime 和 EndTime 欄位的時區是 UTC。 不過，當時間顯示在 Azure 入口網站中時，系統會根據您的當地時區調整時間。
>
>

### <a name="change-policy-for-backup-items"></a>更改備份項的策略

使用者可以修改現有策略或將備份項的策略從策略 1 更改為策略 2。 要切換備份項的策略，請獲取相關策略並備份項，並使用啟用[-AzRecoveryServices](https://docs.microsoft.com/powershell/module/az.recoveryservices/Enable-AzRecoveryServicesBackupProtection?view=azps-1.5.0)命令，並將備份項作為參數。

````powershell
$TargetPol1 = Get-AzRecoveryServicesBackupProtectionPolicy -Name <PolicyName> -VaultId $targetVault.ID
$anotherBkpItem = Get-AzRecoveryServicesBackupItem -WorkloadType AzureVM -BackupManagementType AzureVM -Name "<BackupItemName>" -VaultId $targetVault.ID
Enable-AzRecoveryServicesBackupProtection -Item $anotherBkpItem -Policy $TargetPol1 -VaultId $targetVault.ID
````

該命令等待配置備份完成並返回以下輸出。

```powershell
WorkloadName     Operation            Status               StartTime                 EndTime                   JobID
------------     ---------            ------               ---------                 -------                   -----
TestVM           ConfigureBackup      Completed            3/18/2019 8:00:21 PM      3/18/2019 8:02:16 PM      654e8aa2-4096-402b-b5a9-e5e71a496c4e
```

### <a name="stop-protection"></a>停止保護

#### <a name="retain-data"></a>保留資料

如果使用者希望停止保護，他們可以使用[禁用-AzRecoveryServices 備份保護](https://docs.microsoft.com/powershell/module/az.recoveryservices/Disable-AzRecoveryServicesBackupProtection?view=azps-1.5.0)PS Cmdlet。 這將停止計畫備份，但備份到現在的資料將永久保留。

````powershell
$bkpItem = Get-AzRecoveryServicesBackupItem -BackupManagementType AzureVM -WorkloadType AzureVM -Name "<backup item name>" -VaultId $targetVault.ID
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID
````

#### <a name="delete-backup-data"></a>刪除備份資料

為了完全刪除保存庫中存儲的備份資料，只需將"-刪除復原點"標誌/開關添加到["禁用"保護命令](#retain-data)。

````powershell
Disable-AzRecoveryServicesBackupProtection -Item $bkpItem -VaultId $targetVault.ID -RemoveRecoveryPoints
````

## <a name="restore-an-azure-vm"></a>還原 Azure VM

使用 Azure 入口網站還原 VM 和使用 PowerShell 還原 VM 之間有一個重要差異。 使用 PowerShell 時，建立磁碟和復原點組態資訊之後，還原作業即完成。 還原作業不會建立虛擬機器。 若要從磁碟建立虛擬機器，請參閱[從還原的磁碟建立 VM](backup-azure-vms-automation.md#create-a-vm-from-restored-disks)一節。 如果您不想還原整個 VM，但想要從 Azure VM 備份還原或復原幾個檔案，請參閱[檔案復原](backup-azure-vms-automation.md#restore-files-from-an-azure-vm-backup)一節。

> [!Tip]
> 還原作業不會建立虛擬機器。
>
>

下圖顯示從 RecoveryServicesVault 到 BackupRecoveryPoint 的物件階層。

![顯示 BackupContainer 的復原服務物件階層](./media/backup-azure-vms-arm-automation/backuprecoverypoint-only.png)

若要還原備份資料，請識別已備份的項目和保存時間點資料的復原點。 使用[還原-AzRecovery 服務備份專案](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem)將資料從保存庫還原到您的帳戶。

還原 Azure VM 的基本步驟如下︰

* 選取 VM。
* 選擇復原點。
* 還原磁碟。
* 從預存的磁碟建立 VM。

### <a name="select-the-vm"></a>選取 VM

若要取得可識別正確備份項目的 PowerShell 物件，請從保存庫中的容器開始，向下深入物件階層。 要選擇表示 VM 的容器，請使用[獲取-AzRecovery 服務備份容器](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer)Cmdlet 和管道，該管道到[獲取-Az 恢復服務備份專案](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem)Cmdlet。

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM" -VaultId $targetVault.ID
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM" -VaultId $targetVault.ID
```

### <a name="choose-a-recovery-point"></a>選擇復原點

使用 [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) Cmdlet 來列出備份項目的所有復原點。 接下來選擇要還原的復原點。 如果您不確定要使用哪一個復原點，在清單中選擇最近的 RecoveryPointType = AppConsistent 點是好的做法。

在下面的腳本中，變數 **$rp**是過去七天所選備份項的復原點陣列。 陣列是以相反時間順序排序，最新復原點位於索引 0。 使用標準 PowerShell 陣列索引來挑選復原點。 在範例中，$rp[0] 會選取最新的復原點。

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime() -VaultId $targetVault.ID
$rp[0]
```

輸出類似於下列範例：

```output
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="restore-the-disks"></a>還原磁碟

使用[還原-AzRecovery 服務備份專案](https://docs.microsoft.com/powershell/module/az.recoveryservices/restore-azrecoveryservicesbackupitem)Cmdlet 將備份項的資料和配置還原到復原點。 在您識別復原點之後，請使用它作為 **-RecoveryPoint** 參數的值。 在上面的範例中，**$rp[0]** 是要使用的復原點。 在接下來的範例程式碼中，**$rp[0]** 是要用來還原磁碟的復原點。

還原磁碟和設定資訊：

```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -VaultId $targetVault.ID
$restorejob
```

#### <a name="restore-managed-disks"></a>還原受控磁碟

> [!NOTE]
> 如果備份的 VM 含有受控磁碟，而您想要將它們還原成受控磁碟，我們從 Azure PowerShell RM 模組 v 6.7.0 開始便已導入該功能 。
>
>

請提供額外的參數 **TargetResourceGroupName**以指定將作為受控磁碟還原目的地的 RG。

> [!NOTE]
> 強烈建議使用 **TargetResourceGroupName** 參數來還原受控磁碟，因為這會導致效能顯著改善。 此外，從 Azure Powershell Az 模組 1.0 起，這是還原受控磁碟時的必要參數。
>
>

```powershell
$restorejob = Restore-AzRecoveryServicesBackupItem -RecoveryPoint $rp[0] -StorageAccountName "DestAccount" -StorageAccountResourceGroupName "DestRG" -TargetResourceGroupName "DestRGforManagedDisks" -VaultId $targetVault.ID
```

**VMConfig.JSON** 檔案將會還原至儲存體帳戶，而受控磁碟則會還原至指定的目標 RG。

輸出類似於下列範例：

```output
WorkloadName     Operation          Status               StartTime                 EndTime            JobID
------------     ---------          ------               ---------                 -------          ----------
V2VM              Restore           InProgress           4/23/2016 5:00:30 PM                        cf4b3ef5-2fac-4c8e-a215-d2eba4124f27
```

使用[等待-Az 恢復服務備份作業](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob)Cmdlet 等待還原作業完成。

```powershell
Wait-AzRecoveryServicesBackupJob -Job $restorejob -Timeout 43200
```

還原作業完成後，請使用[獲取-AzRecovery 服務備份作業詳細資訊](https://docs.microsoft.com/powershell/module/az.recoveryservices/wait-azrecoveryservicesbackupjob)Cmdlet 獲取還原操作的詳細資訊。 JobDetails 屬性具有重建 VM 所需的資訊。

```powershell
$restorejob = Get-AzRecoveryServicesBackupJob -Job $restorejob -VaultId $targetVault.ID
$details = Get-AzRecoveryServicesBackupJobDetails -Job $restorejob -VaultId $targetVault.ID
```

還原磁碟之後，請繼續下一節來建立 VM。

## <a name="replace-disks-in-azure-vm"></a>替換 Azure VM 中的磁片

要替換磁片和配置資訊，請執行以下步驟：

* 第 1 步：[復原磁碟](backup-azure-vms-automation.md#restore-the-disks)
* 第 2 步：[使用 PowerShell 分離資料磁片](https://docs.microsoft.com/azure/virtual-machines/windows/detach-disk#detach-a-data-disk-using-powershell)
* 第 3 步：[使用 PowerShell 將資料磁片連接到 Windows VM](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-ps)

## <a name="create-a-vm-from-restored-disks"></a>從還原的磁碟建立 VM

在您還原磁碟之後，使用下列步驟從磁碟建立及設定虛擬機器。

> [!NOTE]
>
> 1. AzureAz 模組 3.0.0 或更高版本是必需的。 <br>
> 2. 若要從預存的磁碟建立加密的 VM，您的 Azure 角色必須具備可執行 **Microsoft.KeyVault/vaults/deploy/action** 動作的權限。 如果您的角色並沒有此權限，請使用此動作來建立自訂角色。 如需詳細資訊，請參閱 [Azure RBAC 中的自訂角色](../role-based-access-control/custom-roles.md)。 <br>
> 3. 還原磁碟之後，現在即可取得部署範本，您可以直接用來建立新的虛擬機器。 沒有其他不同的 PS Cmdlet 可建立加密/解密的受控/非受控虛擬機器。<br>
> <br>

### <a name="create-a-vm-using-the-deployment-template"></a>使用部署範本創建 VM

結果工作詳細資料提供可查詢和部署的範本 URI。

```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $templateBlobURI = $properties["Template Blob Uri"]
```

此範本位於客戶的儲存體帳戶和指定的容器下，因此無法直接存取。 我們需要完整的 URL (以及暫時的 SAS 權杖)，才能存取此範本。

1. 首先從範本BlobURI中提取範本名稱。 格式如下所述。 您可以使用 Powershell 中的拆分操作從此 URL 中提取最終範本名稱。

```http
https://<storageAccountName.blob.core.windows.net>/<containerName>/<templateName>
```

2. 然後，完整的 URL 可以生成，如下[所述](https://docs.microsoft.com/azure/azure-resource-manager/templates/secure-template-with-sas-token?tabs=azure-powershell#provide-sas-token-during-deployment)。

```powershell
Set-AzCurrentStorageAccount -Name $storageAccountName -ResourceGroupName <StorageAccount RG name>
$templateBlobFullURI = New-AzStorageBlobSASToken -Container $containerName -Blob <templateName> -Permission r -FullUri
```

3. 部署範本以創建新的 VM，如[此處](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy)所述。

```powershell
New-AzResourceGroupDeployment -Name ExampleDeployment ResourceGroupName ExampleResourceGroup -TemplateUri $templateBlobFullURI -storageAccountType Standard_GRS
```

### <a name="create-a-vm-using-the-config-file"></a>使用設定檔創建 VM

下節列出使使用「VMConfig」檔案建立虛擬機器所需的步驟。

> [!NOTE]
> 強烈建議使用以上詳述的部署範本來建立虛擬機器。 本節 (1-6 點) 很快就會淘汰。

1. 查詢工作詳細資料的已還原磁碟內容。

   ```powershell
   $properties = $details.properties
   $storageAccountName = $properties["Target Storage Account Name"]
   $containerName = $properties["Config Blob Container Name"]
   $configBlobName = $properties["Config Blob Name"]
   ```

2. 設定 Azure 儲存體內容，並還原為 JSON 組態檔。

   ```powershell
   Set-AzCurrentStorageAccount -Name $storageaccountname -ResourceGroupName "testvault"
   $destination_path = "C:\vmconfig.json"
   Get-AzStorageBlobContent -Container $containerName -Blob $configBlobName -Destination $destination_path
   $obj = ((Get-Content -Path $destination_path -Raw -Encoding Unicode)).TrimEnd([char]0x00) | ConvertFrom-Json
   ```

3. 使用 JSON 組態檔來建立 VM 組態。

   ```powershell
   $vm = New-AzVMConfig -VMSize $obj.'properties.hardwareProfile'.vmSize -VMName "testrestore"
   ```

4. 連接作業系統磁碟與資料磁碟。 此步驟提供各種受控和已加密 VM 設定的範例。 使用適合您的 VM 設定的範例。

* **非受控與未加密的 VM** - 如果是非受控、未加密的 VM，請使用下列範例。

```powershell
       Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
       $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
       foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
       {
        $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
       }
```

* **含 Azure AD 的非受控已加密 VM (僅限 BEK)** - 對於含 Azure AD 的非受控已加密 VM (僅限使用 BEK 加密)，您需要先將密碼還原至金鑰保存庫，才可以連結磁碟。 如需詳細資訊，請參閱[從 Azure 備份復原點還原已加密的虛擬機器](backup-azure-restore-key-secret.md)。 下列範例示範如何將 OS 和資料磁碟連結至已加密的 VM。 設定 OS 磁碟時，請務必提及相關的 OS 類型。

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $dekUrl = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows/Linux
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
      {
       $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
```

* **含 Azure AD 的非受控已加密 VM (BEK 和 KEK)** - 對於含 Azure AD 的非受控已加密 VM (使用 BEK 和 KEK 加密)，您需要先將金鑰與密碼還原到金鑰保存庫，才可以連結磁碟。 如需詳細資訊，請參閱[從 Azure 備份復原點還原已加密的虛擬機器](backup-azure-restore-key-secret.md)。 下列範例示範如何將 OS 和資料磁碟連結至已加密的 VM。

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net:443/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net:443/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.storageProfile'.osDisk.vhd.uri -DiskEncryptionKeyUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -CreateOption "Attach" -Windows
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.storageProfile'.osDisk.osType
      foreach($dd in $obj.'properties.storageProfile'.dataDisks)
     {
     $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
     }
```

* **不含 Azure AD 的非受控已加密 VM (僅限 BEK)** - 對於不含 Azure AD 的非受控已加密 VM (僅限使用 BEK 加密)，如果**無法使用來源 keyVault/密碼**，會使用[從 Azure 備份復原點還原未加密的虛擬機器](backup-azure-restore-key-secret.md)中的程序，將密碼還原至金鑰保存庫。 然後執行下列指令碼，對於還原的 OS blob 設定加密詳細資料 (資料 blob 不需要此步驟)。 可以從還原的 keyVault 擷取 $dekurl。

只有在沒有來源 keyVault/密碼可用時，才需要執行下列指令碼。

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
```

有**密碼可用**，而且也對於 OS Blob 設定加密詳細資料後，可使用下列指令碼附加磁碟。

如果來源 keyVault/密碼可供使用，則無法執行上述指令碼。

```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
```

* **不含 Azure AD 的非受控已加密 VM (BEK 和 KEK)** - 對於不含 Azure AD 的非受控已加密 VM (僅限使用 BEK 和 KEK 加密)，如果**無法使用來源 keyVault/金鑰/密碼**，會使用[從 Azure 備份復原點還原未加密的虛擬機器](backup-azure-restore-key-secret.md)中的程序，將金鑰和密碼還原至金鑰保存庫。 然後執行下列指令碼，對於還原的 OS blob 設定加密詳細資料 (資料 blob 不需要此步驟)。 可以從還原的 keyVault 擷取 $dekurl 和 $kekurl。

只有在沒有來源 keyVault/金鑰/密碼可用時，才需要執行下列指令碼。

```powershell
      $dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
      $kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
      $keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
      $encSetting = "{""encryptionEnabled"":true,""encryptionSettings"":[{""diskEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""secretUrl"":""$dekUrl""},""keyEncryptionKey"":{""sourceVault"":{""id"":""$keyVaultId""},""keyUrl"":""$kekUrl""}}]}"
      $osBlobName = $obj.'properties.StorageProfile'.osDisk.name + ".vhd"
      $osBlob = Get-AzStorageBlob -Container $containerName -Blob $osBlobName
      $osBlob.ICloudBlob.Metadata["DiskEncryptionSettings"] = $encSetting
      $osBlob.ICloudBlob.SetMetadata()
```

有**金鑰/密碼可用**，而且對於 OS Blob 設定加密詳細資料後，可使用下列指令碼附加磁碟。

如果來源 keyVault/金鑰/密碼可供使用，則無法執行上述指令碼。

```powershell
      Set-AzVMOSDisk -VM $vm -Name "osdisk" -VhdUri $obj.'properties.StorageProfile'.osDisk.vhd.Uri -CreateOption "Attach"
      $vm.StorageProfile.OsDisk.OsType = $obj.'properties.StorageProfile'.OsDisk.OsType
      foreach($dd in $obj.'properties.StorageProfile'.DataDisks)
      {
      $vm = Add-AzVMDataDisk -VM $vm -Name "datadisk1" -VhdUri $dd.vhd.Uri -DiskSizeInGB 127 -Lun $dd.Lun -CreateOption "Attach"
      }
```

* **受控與未加密的 VM** - 針對受控、未加密的 VM，請連結已還原的受控磁碟。 如需深入的資訊，請參閱[使用 PowerShell 將資料磁碟連結至 Windows VM](../virtual-machines/windows/attach-disk-ps.md)。

* **含 Azure AD 的受控已加密 VM (僅限 BEK)** - 對於含 Azure AD 的受控已加密的 VM (僅限使用 BEK 加密)，請連結已還原的受控磁碟。 如需深入的資訊，請參閱[使用 PowerShell 將資料磁碟連結至 Windows VM](../virtual-machines/windows/attach-disk-ps.md)。

* **含 Azure AD 的受控已加密 VM (BEK 與 KEK)** - 對於含 Azure AD 的受控已加密的 VM (使用 BEK 與 KEK 加密)，請連結已還原的受控磁碟。 如需深入的資訊，請參閱[使用 PowerShell 將資料磁碟連結至 Windows VM](../virtual-machines/windows/attach-disk-ps.md)。

* **託管和加密 VM 沒有 Azure AD （僅限 BEK）** - 對於沒有 Azure AD 的託管、加密 VM（僅使用 BEK 加密），如果源**金鑰庫/機密不可用**，請使用 Azure[備份復原點還原非加密虛擬機器](backup-azure-restore-key-secret.md)的過程將機密還原到金鑰保存庫。 然後執行下列指令碼，對於還原的 OS 磁碟設定加密詳細資料 (資料磁碟不需要此步驟)。 可以從還原的 keyVault 擷取 $dekurl。

只有在沒有來源 keyVault/密碼可用時，才需要執行下列指令碼。  

```powershell
$dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
$keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
$diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
$encryptionSettingsElement = New-Object Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement
$encryptionSettingsElement.DiskEncryptionKey = New-Object Microsoft.Azure.Management.Compute.Models.KeyVaultAndSecretReference
$encryptionSettingsElement.DiskEncryptionKey.SourceVault = New-Object Microsoft.Azure.Management.Compute.Models.SourceVault
$encryptionSettingsElement.DiskEncryptionKey.SourceVault.Id = $keyVaultId
$encryptionSettingsElement.DiskEncryptionKey.SecretUrl = $dekUrl
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings = New-Object System.Collections.Generic.List[Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement]
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings.Add($encryptionSettingsElement)
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettingsVersion = "1.1"
Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
```

有密碼可用，而且也對於 OS 磁碟設定加密詳細資料後，若要附加已還原的受控磁碟，請參閱[使用 PowerShell 將資料磁碟連結至 Windows VM](../virtual-machines/windows/attach-disk-ps.md)。

* **沒有 Azure AD（BEK 和 KEK） 的託管和加密 VM** - 對於沒有 Azure AD 的託管加密 VM（使用 BEK & KEK 加密），如果源**金鑰庫/金鑰/金鑰/機密不可用**，請使用還原[Azure 備份復原點中的程式](backup-azure-restore-key-secret.md)將金鑰和金鑰保存庫機密還原到金鑰保存庫。 然後執行以下腳本以在還原的 OS 磁片上設置加密詳細資訊（資料磁片不需要此步驟）。 可以從還原的 keyVault 擷取 $dekurl 和 $kekurl。

只有在沒有來源 keyVault/金鑰/密碼可用時，才需要執行下列指令碼。

```powershell
$dekUrl = "https://ContosoKeyVault.vault.azure.net/secrets/ContosoSecret007/xx000000xx0849999f3xx30000003163"
$kekUrl = "https://ContosoKeyVault.vault.azure.net/keys/ContosoKey007/x9xxx00000x0000x9b9949999xx0x006"
$keyVaultId = "/subscriptions/abcdedf007-4xyz-1a2b-0000-12a2b345675c/resourceGroups/ContosoRG108/providers/Microsoft.KeyVault/vaults/ContosoKeyVault"
$diskupdateconfig = New-AzDiskUpdateConfig -EncryptionSettingsEnabled $true
$encryptionSettingsElement = New-Object Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement
$encryptionSettingsElement.DiskEncryptionKey = New-Object Microsoft.Azure.Management.Compute.Models.KeyVaultAndSecretReference
$encryptionSettingsElement.DiskEncryptionKey.SourceVault = New-Object Microsoft.Azure.Management.Compute.Models.SourceVault
$encryptionSettingsElement.DiskEncryptionKey.SourceVault.Id = $keyVaultId
$encryptionSettingsElement.DiskEncryptionKey.SecretUrl = $dekUrl
$encryptionSettingsElement.KeyEncryptionKey = New-Object Microsoft.Azure.Management.Compute.Models.KeyVaultAndKeyReference
$encryptionSettingsElement.KeyEncryptionKey.SourceVault = New-Object Microsoft.Azure.Management.Compute.Models.SourceVault
$encryptionSettingsElement.KeyEncryptionKey.SourceVault.Id = $keyVaultId
$encryptionSettingsElement.KeyEncryptionKey.KeyUrl = $kekUrl
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings = New-Object System.Collections.Generic.List[Microsoft.Azure.Management.Compute.Models.EncryptionSettingsElement]
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettings.Add($encryptionSettingsElement)
$diskupdateconfig.EncryptionSettingsCollection.EncryptionSettingsVersion = "1.1"
Update-AzDisk -ResourceGroupName "testvault" -DiskName $obj.'properties.StorageProfile'.osDisk.name -DiskUpdate $diskupdateconfig
```

有金鑰/密碼可用，而且對於 OS 磁碟設定加密詳細資料後，若要附加已還原的受控磁碟，請參閱[使用 PowerShell 將資料磁碟連結至 Windows VM](../virtual-machines/windows/attach-disk-ps.md)。

5. 設定網路設定。

    ```powershell
    $nicName="p1234"
    $pip = New-AzPublicIpAddress -Name $nicName -ResourceGroupName "test" -Location "WestUS" -AllocationMethod Dynamic
    $virtualNetwork = New-AzVirtualNetwork -ResourceGroupName "test" -Location "WestUS" -Name "testvNET" -AddressPrefix 10.0.0.0/16
    $virtualNetwork | Set-AzVirtualNetwork
    $vnet = Get-AzVirtualNetwork -Name "testvNET" -ResourceGroupName "test"
    $subnetindex=0
    $nic = New-AzNetworkInterface -Name $nicName -ResourceGroupName "test" -Location "WestUS" -SubnetId $vnet.Subnets[$subnetindex].Id -PublicIpAddressId $pip.Id
    $vm=Add-AzVMNetworkInterface -VM $vm -Id $nic.Id
    ```

6. 建立虛擬機器。

    ```powershell  
    New-AzVM -ResourceGroupName "test" -Location "WestUS" -VM $vm
    ```

7. 推送 ADE 擴充功能。
   如果未推送 ADE 擴展，則資料磁片將標記為未加密，因此執行以下步驟必須執行：

   * **對於含 Azure AD 的 VM** - 使用下列命令，以手動方式啟用資料磁碟的加密  

     **僅限 BEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

     **BEK 和 KEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -AadClientID $aadClientID -AadClientSecret $aadClientSecret -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId  -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -VolumeType Data
      ```

   * **對於不含 Azure AD 的 VM** - 使用下列命令，以手動方式啟用資料磁碟的加密。

     如果在命令執行期間請求 AADClientID，則需要更新 Azure PowerShell。

     **僅限 BEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

      **BEK 和 KEK**

      ```powershell  
      Set-AzVMDiskEncryptionExtension -ResourceGroupName $RG -VMName $vm.Name -DiskEncryptionKeyVaultUrl $dekUrl -DiskEncryptionKeyVaultId $keyVaultId -KeyEncryptionKeyUrl $kekUrl -KeyEncryptionKeyVaultId $keyVaultId -SkipVmBackup -VolumeType "All"
      ```

> [!NOTE]
> 確保手動刪除作為加密 VM 復原磁碟進程的一部分創建的 JASON 檔。

## <a name="restore-files-from-an-azure-vm-backup"></a>從 Azure VM 備份還原檔案

除了還原磁碟，您也可以從 Azure VM 備份還原個別檔案。 還原檔案功能提供復原點中所有檔案的存取權。 透過檔案總管管理檔案，就像一般的檔案一樣。

從 Azure VM 備份還原檔案的基本步驟如下：

* 選取 VM
* 選擇復原點
* 掛接復原點的磁碟
* 複製所需的檔案
* 將磁碟取消掛接

### <a name="select-the-vm"></a>選取 VM

若要取得可識別正確備份項目的 PowerShell 物件，請從保存庫中的容器開始，向下深入物件階層。 要選擇表示 VM 的容器，請使用[獲取-AzRecovery 服務備份容器](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupcontainer)Cmdlet 和管道，該管道到[獲取-Az 恢復服務備份專案](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackupitem)Cmdlet。

```powershell
$namedContainer = Get-AzRecoveryServicesBackupContainer  -ContainerType "AzureVM" -Status "Registered" -FriendlyName "V2VM" -VaultId $targetVault.ID
$backupitem = Get-AzRecoveryServicesBackupItem -Container $namedContainer  -WorkloadType "AzureVM" -VaultId $targetVault.ID
```

### <a name="choose-a-recovery-point"></a>選擇復原點

使用 [Get-AzRecoveryServicesBackupRecoveryPoint](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprecoverypoint) Cmdlet 來列出備份項目的所有復原點。 接下來選擇要還原的復原點。 如果您不確定要使用哪一個復原點，在清單中選擇最近的 RecoveryPointType = AppConsistent 點是好的做法。

在下面的腳本中，變數 **$rp**是過去七天所選備份項的復原點陣列。 陣列是以相反時間順序排序，最新復原點位於索引 0。 使用標準 PowerShell 陣列索引來挑選復原點。 在範例中，$rp[0] 會選取最新的復原點。

```powershell
$startDate = (Get-Date).AddDays(-7)
$endDate = Get-Date
$rp = Get-AzRecoveryServicesBackupRecoveryPoint -Item $backupitem -StartDate $startdate.ToUniversalTime() -EndDate $enddate.ToUniversalTime() -VaultId $targetVault.ID
$rp[0]
```

輸出類似於下列範例：

```output
RecoveryPointAdditionalInfo :
SourceVMStorageType         : NormalStorage
Name                        : 15260861925810
ItemName                    : VM;iaasvmcontainer;RGName1;V2VM
RecoveryPointId             : /subscriptions/XX/resourceGroups/ RGName1/providers/Microsoft.RecoveryServices/vaults/testvault/backupFabrics/Azure/protectionContainers/IaasVMContainer;iaasvmcontainer;RGName1;V2VM/protectedItems/VM;iaasvmcontainer; RGName1;V2VM/recoveryPoints/15260861925810
RecoveryPointType           : AppConsistent
RecoveryPointTime           : 4/23/2016 5:02:04 PM
WorkloadType                : AzureVM
ContainerName               : IaasVMContainer;iaasvmcontainer; RGName1;V2VM
ContainerType               : AzureVM
BackupManagementType        : AzureVM
```

### <a name="mount-the-disks-of-recovery-point"></a>掛接復原點的磁碟

使用[獲取-Az 恢復服務備份RPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/get-azrecoveryservicesbackuprpmountscript) Cmdlet 獲取腳本以裝載復原點的所有磁片。

> [!NOTE]
> 磁碟會以 iSCSI 連接磁碟的形式掛接至執行指令碼所在的機器。 掛接會立即發生，您不會產生任何費用。
>
>

```powershell
Get-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0] -VaultId $targetVault.ID
```

輸出類似於下列範例：

```output
OsType  Password        Filename
------  --------        --------
Windows e3632984e51f496 V2VM_wus2_8287309959960546283_451516692429_cbd6061f7fc543c489f1974d33659fed07a6e0c2e08740.exe
```

在您要復原檔案的機器上執行此指令碼。 若要執行指令碼，您必須輸入提供的密碼。 連結磁碟之後，使用 Windows 檔案總管瀏覽新的磁碟區與檔案。 有關詳細資訊，請參閱備份文章"[從 Azure 虛擬機器備份恢復檔](backup-azure-restore-files-from-vm.md)"。

### <a name="unmount-the-disks"></a>將磁碟取消掛接

複製所需檔後，請使用[禁用-AzRecovery 服務備份RPMountScript](https://docs.microsoft.com/powershell/module/az.recoveryservices/disable-azrecoveryservicesbackuprpmountscript)來卸載磁片。 請務必取消掛接磁碟，以便移除對復原點檔案的存取權。

```powershell
Disable-AzRecoveryServicesBackupRPMountScript -RecoveryPoint $rp[0] -VaultId $targetVault.ID
```

## <a name="next-steps"></a>後續步驟

如果您偏好使用 PowerShell 來與 Azure 資源互動，請參閱 PowerShell 文章：[部署和管理 Windows Server 的備份](backup-client-automation.md)。 如果您管理 DPM 備份，請參閱[部署及管理 DPM 的備份](backup-dpm-automation.md)一文。
