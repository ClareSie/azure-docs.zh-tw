---
title: 將 SQL Server 備份至 Azure 做為 DPM 工作負載
description: 使用 Azure 備份服務備份 SQL Server 資料庫的簡介
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: ef8ffcb2445a7be27f7fd3da2115f76fe961fd74
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/06/2020
ms.locfileid: "87876303"
---
# <a name="back-up-sql-server-to-azure-as-a-dpm-workload"></a>將 SQL Server 備份至 Azure 做為 DPM 工作負載

本文會引導您完成使用 Azure 備份來備份 SQL Server 資料庫的設定步驟。

若要將 SQL Server 資料庫備份至 Azure，您需要 Azure 帳戶。 如果您沒有帳戶，只需要幾分鐘的時間就可以建立免費帳戶。 如需詳細資訊，請參閱[建立您的 Azure 免費帳戶](https://azure.microsoft.com/pricing/free-trial/)。

若要將 SQL Server 資料庫備份至 Azure，並從 Azure 復原：

1. 建立備份原則來保護 Azure 中 SQL Server 資料庫。
1. 在 Azure 中建立隨選備份複本。
1. 從 Azure 復原資料庫。

>[!NOTE]
>DPM 2019 UR2 支援使用叢集共用磁片區 (CSV) ， (FCI) SQL Server 容錯移轉叢集實例。

## <a name="prerequisites-and-limitations"></a>必要條件和限制

* 如果您有一個資料庫上其檔案位於遠端的檔案共用，則保護將會失敗，錯誤識別碼為 104。 DPM 不支援保護遠端檔案共用上的 SQL Server 資料。
* DPM 無法保護儲存在遠端 SMB 共用上的資料庫。
* 請確定[可用性群組複本已設定為唯讀](/sql/database-engine/availability-groups/windows/configure-read-only-access-on-an-availability-replica-sql-server?view=sql-server-ver15)。
* 您必須明確地將系統帳戶**NTAuthority\System**新增至 SQL Server 上的 Sysadmin 群組。
* 當您執行部分自主資料庫的替代位置復原時，您必須確定目標 SQL 實例已啟用「自主[資料庫](/sql/relational-databases/databases/migrate-to-a-partially-contained-database?view=sql-server-ver15#enable)」功能。
* 當您針對檔案資料流程資料庫執行替代位置復原時，必須確定目標 SQL 實例已啟用「檔案[資料流程資料庫](/sql/relational-databases/blob/enable-and-configure-filestream?view=sql-server-ver15)」功能。
* 保護 SQL Server AlwaysOn：
  * DPM 在保護群組建立之際執行查詢時，會偵測可用性群組。
  * DPM 會偵測容錯移轉和資料庫的連續保護。
  * DPM 支援 SQL Server 執行個體的多站台叢集設定。
* 當您保護使用 AlwaysOn 功能的資料庫時，DPM 有下列限制：
  * DPM 將會根據備份喜好設定，採用 SQL Server 中設定的可用性群組備份原則，如下所示：
    * 慣用次要：除非主要複本是唯一的線上複本，否則應在次要複本上執行備份。 如果有多個次要複本可用，則會選取具有最高備份優先權的節點進行備份。 如果只有主要複本可用，則應該在主要複本上進行備份。
    * 僅次要：不應在主要複本上執行備份。 如果主要複本是唯一的線上複本，則不應進行備份。
    * 主要備份：您應該一律在主要複本上進行備份。
    * 任何複本：備份可以在可用性群組中的任何可用性複本上執行。 作為備份來源的節點將依據每個節點的備份優先順序而定。
  * 請注意：
    * 備份可能會從任何可讀取的複本（也就是主要、同步次要、非同步次要）進行。
    * 如果從備份中排除任何複本，例如 [**排除複本**] 已啟用或標示為 [無法讀取]，則不會在任何選項下選取該複本進行備份。
    * 如果有多個複本可用且可讀取，則會選取備份優先順序最高的節點進行備份。
    * 如果在選取的節點上備份失敗，則備份作業會失敗。
    * 不支援復原到原始位置。
* SQL Server 2014 或以上的備份問題：
  * SQL server 2014 新增了新功能，可為[Windows Azure Blob 儲存體中的內部部署 SQL Server 建立資料庫](/sql/relational-databases/databases/sql-server-data-files-in-microsoft-azure?view=sql-server-ver15)。 DPM 無法用來保護此設定。
  * SQL AlwaysOn 選項的「偏好次要」備份喜好設定有一些已知問題。 DPM 一律會從次要複本進行備份。 如果找不到次要複本，則備份會失敗。

## <a name="before-you-start"></a>開始之前

開始之前，請確定您已符合使用 Azure 備份來保護工作負載的[必要條件](backup-azure-dpm-introduction.md#prerequisites-and-limitations)。 以下是一些必要的工作：

* 建立備份保存庫。
* 下載保存庫認證。
* 安裝 Azure 備份代理程式。
* 向保存庫註冊伺服器。

## <a name="create-a-backup-policy"></a>建立備份原則

若要保護 Azure 中 SQL Server 資料庫，請先建立備份原則：

1. 在 [Data Protection Manager (DPM) 伺服器上，選取 [**保護**] 工作區。
1. 選取 [**新增**] 以建立保護群組。

    ![建立保護群組](./media/backup-azure-backup-sql/protection-group.png)
1. 在 [開始] 頁面上，檢查有關建立保護群組的指引。 然後選取 [下一步]。
1. 選取 [**伺服器**]。

    ![選取伺服器保護群組類型](./media/backup-azure-backup-sql/pg-servers.png)
1. 展開您要備份的資料庫所在的 SQL Server 虛擬機器。 您會看到可從該伺服器備份的資料來源。 展開 [**所有 SQL 共用**]，然後選取您要備份的資料庫。 在此範例中，我們選取 ReportServer $ MSDPM2012 和 ReportServer $ MSDPM2012TempDB。 然後選取 [下一步]。

    ![選取 SQL Server 資料庫](./media/backup-azure-backup-sql/pg-databases.png)
1. 將保護群組命名為，然後選取 [**我想要線上保護**]。

    ![選擇資料保護方法-短期磁片保護或線上 Azure 保護](./media/backup-azure-backup-sql/pg-name.png)
1. 在 [**指定短期目標**] 頁面上，包含建立磁片備份點所需的輸入。

    在此範例中，[**保留範圍**] 設定為*5 天*。 備份**同步處理頻率**會設定為每隔*15 分鐘*一次。 **快速完整備份**已設定為*8:00 PM*。

    ![設定備份保護的短期目標](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > 在此範例中，每天下午8:00 建立備份點。 自前一天 8:00 PM 備份點以來已修改過的資料會被轉移。 這個程序稱為 [快速完整備份] ****。 雖然交易記錄會每隔15分鐘同步處理一次，但如果我們需要在下午9:00 復原資料庫，則會從上一個快速完整備份點重新執行記錄（在此範例中為 8:00 PM）來建立該點。
   >
   >

1. 選取 [下一步]  。 DPM 會顯示可用的整體儲存空間。 它也會顯示潛在的磁碟空間使用量。

    ![設定磁片配置](./media/backup-azure-backup-sql/pg-storage.png)

    根據預設，DPM 會為每個資料來源建立一個磁片區 (SQL Server 資料庫) 。 磁片區會用於初始備份複本。 在此設定中，邏輯磁片管理員 (LDM) 會將 DPM 保護限制為300資料來源 (SQL Server 資料庫) 。 若要因應這項限制，請選取 [將資料共置在 DPM 存放集區中]****。 如果您使用此選項，DPM 會針對多個資料來源使用單一磁片區。 此設定可讓 DPM 保護最多 2000 SQL Server 的資料庫。

    如果您選取 [**自動擴大磁片**區]，則 DPM 會在生產資料成長時，考慮增加的備份磁片區。 如果您未選取 **[自動擴大磁片**區]，DPM 會將備份存放裝置限制為保護群組中的資料來源。

1. 如果您是系統管理員，您可以選擇透過**網路自動**傳輸此初始備份，然後選擇傳輸時間。 或選擇**手動**傳輸備份。 然後選取 [下一步]。

    ![選擇複本建立方法](./media/backup-azure-backup-sql/pg-manual.png)

    初始備份複本要求將整個資料來源 (傳送 SQL Server 資料庫) 。 備份資料會從實際執行伺服器 (SQL Server 電腦) 移到 DPM 服務器。 如果此備份很大，則透過網路傳送資料可能會造成頻寬擁塞。 基於這個理由，系統管理員可以選擇使用卸載式媒體**手動**傳輸初始備份。 或者，他們可以在指定時間透過**網路自動**傳輸資料。

    完成初始備份之後，備份會在初始備份複本上以累加方式繼續進行。 增量備份通常都非常小，因此有利於透過網路傳輸。

1. 選擇執行一致性檢查的時機。 然後選取 [下一步]。

    ![選擇執行一致性檢查的時機](./media/backup-azure-backup-sql/pg-consistent.png)

    DPM 可以執行備份點完整性的一致性檢查。 它會計算生產伺服器上備份檔案的總和檢查碼， (此範例中的 SQL Server 電腦) 和 DPM 中該檔案的備份資料。 如果檢查找到衝突，則會假設 DPM 中的備份檔案已損毀。 DPM 會傳送對應至總和檢查碼不符的區塊，以修正備份的資料。 由於一致性檢查是需要大量效能的作業，因此系統管理員可以選擇排程一致性檢查或自動執行。

1. 選取要在 Azure 中保護的資料來源。 然後選取 [下一步]。

    ![選取要在 Azure 中保護的資料來源](./media/backup-azure-backup-sql/pg-sqldatabases.png)
1. 如果您是系統管理員，您可以選擇符合組織原則的備份排程和保留原則。

    ![選擇排程和保留原則](./media/backup-azure-backup-sql/pg-schedule.png)

    在此範例中，每天下午12:00 和 8:00 PM 會執行備份。

    > [!TIP]
    > 若要快速復原，請在磁片上保留幾個短期復原點。 這些復原點可用於操作復原。 Azure 可作為良好的離站位置，以提供更高的 Sla 和保證的可用性。
    >
    > 在本機磁片備份完成之後，使用 DPM 來排程 Azure 備份。 當您遵循這種做法時，最新的磁片備份會複製到 Azure。
    >

1. 選擇保留原則排程。 如需保留原則運作方式的詳細資訊，請參閱[使用 Azure 備份來取代您的磁帶基礎結構](backup-azure-backup-cloud-as-tape.md)。

    ![選擇保留原則](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    在此範例中：

    * 每天下午12:00 和下午8:00 執行備份。 它們會保留180天。
    * 星期六下午12:00 的備份會保留104周。
    * 當月最後一個星期六下午12:00 時，會保留60個月的備份。
    * 3月最後一個星期六下午12:00 的備份會保留10年。

    選擇保留原則之後，請選取 **[下一步]**。

1. 選擇如何將初始備份複本傳輸至 Azure。

    * [**自動透過網路**] 選項會遵循您的備份排程，將資料傳輸至 Azure。
    * 如需**離線備份**的詳細資訊，請參閱[離線備份的總覽](offline-backup-overview.md)。

    選擇傳輸機制之後，請選取 **[下一步]**。

1. 在 [**摘要**] 頁面上，檢查原則詳細資料。 然後選取 [**建立群組**]。 您可以選取 [**關閉**]，並在 [**監視**] 工作區中監看作業進度。

    ![保護群組建立的進度](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="create-on-demand-backup-copies-of-a-sql-server-database"></a>建立 SQL Server 資料庫的隨選備份複本

當第一次進行備份時，就會建立復原點。 您可以手動觸發復原點的建立，而不是等待排程執行：

1. 在 [保護] 群組中，確認資料庫狀態為 **[確定]**。

    ![顯示資料庫狀態的保護群組](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
1. 以滑鼠右鍵按一下資料庫，然後選取 [**建立復原點**]。

    ![選擇建立線上復原點](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
1. 在下拉式功能表中，選取 [**線上保護**]。 然後選取 **[確定]** ，開始在 Azure 中建立復原點。

    ![開始在 Azure 中建立復原點](./media/backup-azure-backup-sql/sqlbackup-azure.png)
1. 您可以在 [**監視**] 工作區中查看作業進度。

    ![在監視主控台中查看工作進度](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a>從 Azure 復原 SQL Server 資料庫

若要從 Azure 復原受保護的實體（例如 SQL Server 資料庫）：

1. 開啟 DPM 服務器管理主控台。 移至 [復原 **] 工作區**，查看 DPM 備份的伺服器。 選取在此範例中為 ReportServer $ MSDPM2012) 的資料庫 (。 選取以「**線上**」結束的復原**時間**。

    ![選取復原點](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
1. 以滑鼠右鍵按一下資料庫名稱，然後選取 [**復原**]。

    ![從 Azure 復原資料庫](./media/backup-azure-backup-sql/sqlbackup-recover.png)
1. DPM 會顯示復原點的詳細資料。 選取 [下一步]  。 若要覆寫資料庫，請選取復原類型 [復原到原始的 SQL Server 執行個體] ****。 然後選取 [下一步]。

    ![將資料庫復原到其原始位置](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    在此範例中，DPM 允許將資料庫復原到另一個 SQL Server 實例或獨立的網路資料夾。
1. 在 [**指定復原選項**] 頁面上，您可以選取 [復原選項]。 例如，您可以選擇 [**網路頻寬使用節流**設定] 來調節復原所使用的頻寬。 然後選取 [下一步]。
1. 在 [**摘要**] 頁面上，您會看到目前的復原設定。 選取 [**復原**]。

    復原狀態會顯示正在復原的資料庫。 您可以選取 [**關閉**] 以關閉嚮導，並在 [**監視**] 工作區中查看進度。

    ![開始復原程式](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    復原完成時，還原的資料庫會與應用程式一致。

## <a name="next-steps"></a>後續步驟

如需詳細資訊，請參閱[AZURE 備份常見問題](backup-azure-backup-faq.md)。
