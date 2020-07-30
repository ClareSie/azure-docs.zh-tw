---
title: ML Studio （傳統）：內部部署 SQL Server-Azure
description: 使用來自 SQL Server 資料庫的資料，以 Azure Machine Learning Studio （傳統）執行 advanced analytics。
services: machine-learning
ms.service: machine-learning
ms.subservice: studio
ms.topic: how-to
author: likebupt
ms.author: keli19
ms.custom: seodec18
ms.date: 03/13/2017
ms.openlocfilehash: 133de2c6b4fd4f970595a0e46c24167ab10c0aea
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2020
ms.locfileid: "87432124"
---
# <a name="perform-analytics-with-azure-machine-learning-studio-classic-using-a-sql-server-database"></a>使用 SQL Server 資料庫，以 Azure Machine Learning Studio （傳統）執行分析

**適用物件：** ![是 ](../../../includes/media/aml-applies-to-skus/yes.png) Machine Learning Studio （傳統） ![ 否](../../../includes/media/aml-applies-to-skus/no.png)[Azure Machine Learning](../compare-azure-ml-to-studio-classic.md)  


使用內部部署資料的企業通常會想要針對他們的機器學習服務工作負載來利用雲端的範圍和靈活度。 但是他們又不想因為將內部部署資料移至雲端而中斷目前的商務程序和工作流程。 Azure Machine Learning Studio （傳統）現在支援從 SQL Server 資料庫讀取您的資料，然後使用此資料對模型進行定型和評分。 您不再需要手動複製和同步處理雲端與內部部署伺服器之間的資料。 相反地，Azure Machine Learning Studio （傳統）中的匯**入資料**模組現在可以直接從您的 SQL Server 資料庫讀取，以便進行定型和評分作業。

本文概要說明如何將 SQL Server 資料輸入 Azure Machine Learning Studio （傳統）。 它假設您已熟悉 Studio （傳統）概念，例如工作區、模組、資料集、實驗*等。*

> [!NOTE]
> 此功能不適用於免費的工作區。 如需機器學習服務定價和層級的詳細資訊，請參閱 [Azure 機器學習服務定價](https://azure.microsoft.com/pricing/details/machine-learning/)。
>
>

<!-- -->



## <a name="install-the-data-factory-self-hosted-integration-runtime"></a>安裝 Data Factory 自我裝載整合執行階段
若要存取 Azure Machine Learning Studio （傳統）中的 SQL Server 資料庫，您必須下載並安裝 Data Factory 自我裝載 Integration Runtime，先前稱為資料管理閘道。 當您在 Machine Learning Studio （傳統）中設定連線時，您有機會使用 [**下載並註冊資料閘道**] 對話方塊來下載並安裝 INTEGRATION RUNTIME （IR），如下所述。


您也可以從 [Microsoft 下載中心](https://www.microsoft.com/download/details.aspx?id=39717)下載及執行 MSI 安裝程式套件，藉此提前安裝 IR。MSI 也可用來將現有的 IR 升級到最新版本，並保留所有設定。

Data Factory 自我裝載整合執行階段的必要條件如下：

* Data Factory 自我裝載整合需要 64 位元的作業系統，以及 .NET Framework 4.6.1 以上。
* 支援的 Windows 作業系統版本包括 Windows 10、Windows Server 2012、Windows Server 2012 R2、Windows Server 2016。 
* 建議的 IR 電腦組態為至少 2 GHz、4 核心 CPU、8 GB RAM 和 80GB 磁碟。
* 如果主機電腦處於休眠狀態，則 IR 不會回應資料要求。 因此，安裝 IR 之前，請先在電腦上設定適當的電源計劃。 如果電腦已設定為休眠，安裝 IR 時會顯示訊息。
* 由於複製活動會以特定的頻率發生，因此，電腦上的資源使用量 (CPU、記憶體) 也會遵循與尖峰和閒置時間相同的模式。 資源使用率也仰賴要移動的資料量。 如果有多個複製作業正在進行，您將可觀察到資源使用量會在尖峰時段增加。 雖然上述的基本組態在技術上來說即已足夠，但是根據資料移動的特定負載而定，您的組態最好具備比基本組態還要多的資源。

在安裝及使用 Data Factory 自我裝載整合執行階段時，請考量下列事項：

* 一部電腦上只能安裝一個 IR 執行個體。
* 您可以在多個內部部署資料來源使用單一 IR。
* 您可以將不同電腦上的多個 IR 連線到同一個內部部署資料來源。
* 您一次只能為一個工作區設定一個「ir」。 目前，無法在工作區之間共用 IRs。
* 您可以為單一工作區設定多個 IR。 例如，當您準備好要讓時，您可能會想要使用在開發期間連接到測試資料來源的 IR，以及生產環境 IR。
* IR 不一定要在與資料來源相同的電腦上。 但是，愈接近資料來源，可縮短閘道連線到資料來源的時間。 建議在不同的電腦上安裝 IR 及裝載內部部署資料來源，這樣閘道和資料來源才不會爭搶資源。
* 如果您的電腦上已安裝 IR 來提供 Power BI 或 Azure Data Factory 案例，請在另一部電腦上安裝個別的 IR for Azure Machine Learning Studio （傳統）。

  > [!NOTE]
  > 您無法在同一部電腦上執行 Data Factory 自我裝載整合執行階段和 Power BI Gateway。
  >
  >
* 即使您使用 Azure ExpressRoute 作為其他資料，您還是需要使用適用于 Azure Machine Learning Studio （傳統）的 Data Factory 自我裝載 Integration Runtime。 即使使用 ExpressRoute，您也應該將資料來源視為內部部署資料來源 (亦即在防火牆後面)。 使用 Data Factory 自我裝載整合執行階段建立 Machine Learning 與資料來源之間的連線。

您可以在 [Data Factory 中的整合執行階段](../../data-factory/concepts-integration-runtime.md)一文中，找到安裝必要條件、安裝步驟和疑難排解秘訣的詳細資訊。

## <a name="span-idusing-the-data-gateway-step-by-step-walk-classanchorspan-id_toc450838866-classanchorspanspaningress-data-from-your-sql-server-database-into-azure-machine-learning"></a><span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>將資料從您的 SQL Server 資料庫輸入 Azure Machine Learning
在此逐步解說中，您將在 Azure Machine Learning 工作區中設定 Azure Data Factory Integration Runtime、加以設定，然後從 SQL Server 資料庫讀取資料。

> [!TIP]
> 開始之前，請先停用瀏覽器的快顯封鎖程式 `studio.azureml.net` 。 如果您使用 Google Chrome 瀏覽器，請下載並安裝數個可在 Google Chrome 線上應用程式商店 [Click Once 應用程式擴充功能](https://chrome.google.com/webstore/search/clickonce?_category=extensions)上取得的外掛程式之一。
>
> [!NOTE]
> Azure Data Factory自我裝載整合執行階段原名為資料管理閘道。 逐步教學課程會繼續稱之為閘道。  

### <a name="step-1-create-a-gateway"></a>步驟 1：建立閘道器
第一個步驟是建立並設定閘道，以存取您的 SQL 資料庫。

1. 登入[Azure Machine Learning Studio （傳統）](https://studio.azureml.net/Home/) ，然後選取您想要使用的工作區。
2. 按一下左側的 [設定]**** 刀鋒視窗，然後按一下頂端的 [資料閘道]**** 索引標籤。
3. 按一下螢幕底部的 [新增資料閘道] **** 。

    ![新增資料閘道](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)
4. 在 [新增資料閘道]**** 對話方塊中，輸入 [閘道名稱]**** 並選擇性地新增 [描述]****。 按一下右下角的箭號，以移至組態的下一個步驟。

    ![輸入閘道名稱和描述](./media/use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)
5. 在 [下載並註冊資料閘道] 對話方塊中，將閘道註冊金鑰複製到剪貼簿。

    ![下載並註冊資料閘道](./media/use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)
6. <span id="note-1" class="anchor"></span>如果您還沒有下載並安裝 Microsoft 資料管理閘道，則可按一下 [下載資料管理閘道]****。 這會帶您前往 Microsoft 下載中心，您可以在其中選取所需的閘道器版本，然後下載並安裝它。 您可以在 [利用資料管理閘道在內部部署來源和雲端之間移動資料](../../data-factory/tutorial-hybrid-copy-portal.md)一文的前幾節中，找到有關安裝必要條件、安裝步驟及疑難排解秘訣的詳細資訊。
7. 安裝閘道之後，隨即開啟 [資料管理閘道組態管理員]，並顯示 [註冊閘道]**** 對話方塊。 貼上您複製到剪貼簿的**閘道註冊金鑰**，然後按一下 [註冊]****。
8. 如果您已經安裝閘道，請執行「資料管理閘道組態管理員」。 按一下 [變更金鑰]****、貼上您在上一個步驟中複製到剪貼簿的**閘道註冊金鑰**，然後按一下 [確定]****。
9. 安裝完成時，便會隨即顯示「Microsoft 資料管理閘道組態管理員」的 [註冊閘道] **** 對話方塊。 貼上您在上一個步驟中複製到剪貼簿的「閘道註冊金鑰」，然後按一下 [註冊]****。

    ![註冊閘道](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)
10. 當「Microsoft 資料管理閘道組態管理員」中的 [常用] **** 索引標籤中設定了下列值時，閘道組態即已完成︰

    * [閘道名稱]**** 和 [執行個體名稱]**** 設定為閘道的名稱。
    * [註冊]**** 設定為 [已註冊]****。
    * [狀態]**** 設定為 [已啟動]****。
    * 底部的狀態列會顯示 [**已連線到資料管理閘道雲端服務**] 和一個綠色的核取記號。

      ![資料管理閘道管理員](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

      當註冊成功時，Azure Machine Learning Studio （傳統）也會更新。

    ![閘道註冊成功](./media/use-data-from-an-on-premises-sql-server/gateway-registered.png)
11. 在 [下載並註冊資料閘道] **** 對話方塊中，按一下核取記號以完成安裝。 [設定] **** 頁面會將閘道狀態顯示為「線上」。 在右側窗格中，您將會發現狀態和其他有用的資訊。

    ![閘道設定](./media/use-data-from-an-on-premises-sql-server/gateway-status.png)
12. 在 [Microsoft 資料管理閘道] 中 Configuration Manager 切換至 [**憑證**] 索引標籤。在此索引標籤上指定的憑證，可用來加密/解密您在入口網站中指定的內部部署資料存放區的認證。 此憑證是預設的憑證。 Microsoft 建議將此憑證變更為您自己在憑證管理系統中備份的憑證。 按一下 [變更] **** 改為使用您自己的憑證。

    ![變更閘道憑證](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-certificate.png)
13. (選擇性) 如果您想啟用詳細資訊記錄功能來為閘道問題進行疑難排解，請在 [Microsoft 資料管理閘道組態管理員] 中切換到 [診斷]**** 索引標籤，然後選取 [啟用詳細資訊記錄以進行疑難排解]**** 選項。 您可以在 Windows 事件檢視器的 [**應用程式及服務記錄**檔]  - &gt; **資料管理閘道**] 節點底下找到記錄資訊。 您也可以使用 [診斷] **** 索引標籤，使用閘道來測試與內部部署資料來源的連線。

    ![啟用詳細資訊記錄](./media/use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-verbose-logging.png)

這會完成 Azure Machine Learning Studio （傳統）中的閘道設定程式。
您現在可以開始使用內部部署資料。

您可以針對每個工作區，在 Studio （傳統）中建立及設定多個閘道。 例如，您可能想要有一個可在開發期間連接到測試資料來源的閘道器，以及另一個適用於生產資料來源的閘道器。 Azure Machine Learning Studio （傳統）可讓您根據公司環境，彈性地設定多個閘道。 目前您無法在工作區之間共用閘道，而且單一電腦只能安裝一個閘道。 如需詳細資訊，請參閱[利用資料管理閘道在內部部署來源和雲端之間移動資料](../../data-factory/tutorial-hybrid-copy-portal.md)。

### <a name="step-2-use-the-gateway-to-read-data-from-an-on-premises-data-source"></a>步驟 2︰使用閘道器讀取來自內部部署資料來源的資料
設定閘道之後，您可以將匯**入資料**模組新增至從 SQL Server 資料庫輸入資料的實驗。

1. 在 [Machine Learning Studio （傳統）] 中，選取 [**實驗**] 索引標籤，按一下左下角的 [ **+ 新增**]，然後選取 [**空白實驗**] \ （或選取數個可用的範例實驗之一）。
2. 找出 **匯入模型** 模組，並將它拖曳到實驗畫布。
3. 按一下畫布下方的 [另存新檔] **** 。 輸入「Azure Machine Learning Studio （傳統）內部部署 SQL Server 教學課程」做為實驗名稱，選取工作區，然後按一下 [**確定]** 核取記號。

   ![以新名稱儲存實驗](./media/use-data-from-an-on-premises-sql-server/experiment-save-as.png)
4. 按一下 [匯入資料]**** 模組來選取它，並在畫布右邊的 [屬性]**** 窗格中，然後選取 [資料來源]**** 下拉式清單中的 [內部部署 SQL Database]。
5. 選取您安裝並註冊的 **資料閘道** 。 您可以藉由選取 [(新增資料閘道...)] 來設定其他閘道。

   ![從匯入資料模組選取資料閘道](./media/use-data-from-an-on-premises-sql-server/import-data-select-on-premises-data-source.png)
6. 輸入 SQL **Database 伺服器名稱**和**資料庫名稱**，以及您想要執行的 SQL **Database 查詢**。
7. 按一下 [使用者名稱和密碼]**** 下方的 [輸入值]****，然後輸入資料庫認證。 您可以根據 SQL Server 的設定方式，使用 Windows 整合式驗證或 SQL Server 驗證。

   ![輸入資料庫認證](./media/use-data-from-an-on-premises-sql-server/database-credentials.png)

   「需要值」訊息會變更為「已設定值」並具有綠色核取記號。 除非資料庫資訊或密碼變更，否則您只需輸入認證一次。 Azure Machine Learning Studio （傳統）會使用您在安裝閘道時所提供的憑證，以加密雲端中的認證。 Azure 永遠不會在沒有加密的情況下儲存內部部署認證。

   ![匯入資料模組屬性](./media/use-data-from-an-on-premises-sql-server/import-data-properties-entered.png)
8. 按一下 [執行] **** 來執行實驗。

實驗完成執行之後，您可以按一下 [匯**入資料**] 模組的輸出埠，然後選取 [**視覺化**]，將您從資料庫匯入的資料視覺化。

當實驗完成開發之後，您就能部署和操作您的模型。 使用批次執行服務時，將會讀取匯**入資料**模組中所設定之 SQL Server 資料庫的資料，並將其用於計分。 雖然您可以使用要求回應服務來對內部部署資料進行評分，但是 Microsoft 建議改用 [Excel 增益集](excel-add-in-for-web-services.md) 。 目前，在您的實驗或發行的 web 服務中，不支援透過**匯出資料**寫入 SQL Server 資料庫。
