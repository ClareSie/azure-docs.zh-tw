---
title: 追蹤 Azure 實驗室服務中的實驗室的使用情況 | Microsoft Docs
description: 在本教學課程中，身為實驗室建立者/擁有者的您可以追蹤實驗室的使用情況。
ms.topic: tutorial
ms.date: 06/26/2020
ms.openlocfilehash: a9a9b49b568decc621be1969a8578d61ac7e4861
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "85445027"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>教學課程：追蹤 Azure 實驗室服務中的實驗室的使用情況
本教學課程會說明實驗室建立者/擁有者如何追蹤實驗室的使用情況。

在本教學課程中，您會執行下列動作：

> [!div class="checklist"]
> * 檢視已向實驗室註冊的使用者
> * 檢視實驗室中 VM 的使用情況
> * 管理學生 VM 


## <a name="view-registered-users"></a>檢視已註冊的使用者

1. 瀏覽至 [Azure 實驗室服務網站](https://labs.azure.com)。 
2. 選取 [登入]  ，然後輸入您的認證。 Azure 實驗室服務支援組織帳戶和 Microsoft 帳戶。
3. 在 [我的實驗室]  頁面上，選取您要追蹤其使用情況的實驗室。 
4. 選取左側功能表或 [使用者] 圖格上的 [使用者]。 您會看到已向實驗室註冊的學生。  

    ![已註冊的使用者](./media/tutorial-track-usage/registered-users.png)

    如需為實驗室新增和管理使用者的詳細資訊，請參閱[新增和管理實驗室使用者](how-to-configure-student-usage.md)。

## <a name="view-the-usage-of-vms"></a>檢視 VM 的使用情況

1. 在左側的功能表上選取 [虛擬機器]  。 
2. 確認您有看到 VM 的狀態和 VM 已執行的時數。 實驗室擁有者花在學生 VM 的時間，不會計入最後一個資料行所示的使用時間中。 

    ![VM 使用方式](./media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>管理學生 VM 
在此頁面上，您可以使用 [狀態]  資料行或工具列上的控制項來啟動、停止或重設學生 VM。

![VM 動作](./media/tutorial-track-usage/vm-controls.png)

如需管理實驗室虛擬機器集區的詳細資訊，請參閱[設定和管理虛擬機器集區](how-to-set-virtual-machine-passwords.md)。

> [!NOTE]
> 當教育工作者開啟學生 VM 時，學生的配額不會受到影響。 使用者的配額會指定使用者可在排定的上課時間外使用的實驗室時數。 如需配額的詳細資訊，請參閱[為使用者設定配額](how-to-configure-student-usage.md?#set-quotas-for-users)。

## <a name="next-steps"></a>後續步驟
若要深入了解教室實驗室，請參閱[操作指南](how-to-manage-lab-accounts.md)底下的文章。
