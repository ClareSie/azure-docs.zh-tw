---
title: 教學課程︰以 Azure Active Directory 設定 Box 來自動佈建使用者 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Box 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 1c959595-6e57-4954-9c0d-67ba03ee212b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/26/2017
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1397b4189a9c2c15e3878687ea8c67c1da7567f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "77058564"
---
# <a name="tutorial-configure-box-for-automatic-user-provisioning"></a>教學課程︰設定 Box 來自動佈建使用者

本教學課程旨在說明您需要在 Box 和 Azure AD 中執行的步驟，以將使用者帳戶從 Azure AD 自動佈建和取消佈建至 Box。

> [!NOTE]
> 本教學課程會說明建置在 Azure AD 使用者佈建服務之上的連接器。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../app-provisioning/user-provisioning.md)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 Box 整合，您需要下列項目：

- Azure AD 租用戶
- Box 商務方案或更佳方案

> [!NOTE]
> 當您測試本教學課程中的步驟時，「不」** 建議您使用生產環境。

若要測試本教學課程中的步驟，請遵循下列建議：

- 除非必要，否則請勿使用生產環境。
- 如果您沒有 Azure AD 試用環境，您可以[取得一個月試用](https://azure.microsoft.com/pricing/free-trial/)。

## <a name="assigning-users-to-box"></a>將使用者指派給 Box 

Azure Active Directory 會使用稱為「指派」的概念，來判斷哪些使用者應接收對指定應用程式的存取權。 在自動使用者帳戶佈建的內容中，只有「已指派」至 Azure AD 中的應用程式之使用者和群組會進行同步處理。

在設定並啟用佈建服務之前，您必須決定 Azure AD 中的哪些使用者及/或群組代表需要 Box 應用程式存取權的使用者。 一旦決定後，您可以依照此處的指示，將這些使用者指派給 Box 應用程式︰

[將使用者或群組指派給企業應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-assign-user-azure-portal)

## <a name="assign-users-and-groups"></a>指派使用者和群組
Azure 入口網站的 [Box] > [使用者和群組]**** 索引標籤可讓您指定應該授與哪些使用者和群組 Box 的存取權。 指派使用者或群組會導致下列事項發生︰

* Azure AD 允許指派的使用者 (直接指派或群組成員資格) 驗證 Box。 如果使用者未經指派，則 Azure AD 不會允許他們登入 Box，並會在 Azure AD 登入分頁傳回錯誤。
* Box 的應用程式圖格會加入使用者的 [應用程式啟動程式](../manage-apps/end-user-experiences.md)中。
* 如已啟用自動佈建，則指派的使用者及/或群組就會加入佈建佇列進行自動佈建。
  
  * 如果只設定要佈建使用者物件，則所有直接指派的使用者都會放在佈建佇列中，而任何指派群組成員的所有使用者也會放在佈建佇列中。 
  * 如果設定要佈建的是群組物件，則所有指派的群組物件以及這些群組成員的所有使用者都會佈建到 Box。 群組和使用者成員資格都會保留寫入 Box。

您可以使用 [屬性] > [單一登入]**** 索引標籤來設定，在 SAML 驗證期間，要向 Box 呈現哪些使用者屬性 (或宣告)；使用 [屬性] > [佈建]**** 索引標籤來設定，在佈建作業期間，使用者及群組屬性如何從 Azure AD 流向 Box。

### <a name="important-tips-for-assigning-users-to-box"></a>將使用者指派給 Box 的重要秘訣 

*   建議將單一 Azure AD 使用者指派給 Box，以測試佈建的設定。 其他使用者及/或群組可能會稍後再指派。

*   將使用者指派給 Box 時，您必須選取有效的使用者角色。 「預設存取」角色不適用於佈建。

## <a name="enable-automated-user-provisioning"></a>啟用自動使用者佈建

本節會引導您將 Azure AD 連線至 Box 的使用者帳戶佈建 API，以及根據 Azure AD 中的使用者和群組指派，設定佈建服務以在 Box 中建立、更新和停用指派的使用者帳戶。

如已啟用自動佈建，則指派的使用者及/或群組就會加入佈建佇列進行自動佈建。
    
 * 如果只設定要佈建使用者物件，則直接指派的使用者會放在佈建佇列中，而任何指派群組成員的所有使用者也會放在佈建佇列中。 
    
 * 如果設定要佈建的是群組物件，則所有指派的群組物件以及這些群組成員的所有使用者都會佈建到 Box。 群組和使用者成員資格都會保留寫入 Box。

> [!TIP] 
> 您也可以選擇啟用 Box 的 SAML 型單一登入，請遵循 [Azure 入口網站](https://portal.azure.com)中提供的指示。 可以獨立設定自動佈建的單一登入，雖然這兩個功能彼此補充。

### <a name="to-configure-automatic-user-account-provisioning"></a>若要設定自動使用者帳戶佈建：

本節的目的是要說明如何對 Box 啟用 Active Directory 使用者帳戶的佈建。

1. 在[Azure 門戶](https://portal.azure.com)中，流覽到**Azure 活動目錄>企業應用>所有應用程式**部分。

2. 如果您已經設定單一登入的 Box，使用 [搜尋] 欄位搜尋您的 Box 執行個體。 否則，請選取 [新增]****，並在應用程式庫中搜尋 [Box]****。 從搜尋結果中選取 Box，並將它新增至您的應用程式清單。

3. 選取您的 Box 執行個體，然後選取 [佈建]**** 索引標籤。

4. 將**預配模式**設置為 **"自動**"。 

    ![佈建](./media/box-userprovisioning-tutorial/provisioning.png)

5. 在 [管理員認證]**** 區段底下，按一下 [授權]**** 以在新的瀏覽器視窗中開啟 Box 登入對話方塊。

6. 在 [登入以授與 Box 存取權]**** 頁面上，提供必要的認證，然後按一下 [授權]****。 
   
    ![啟用自動使用者佈建](./media/box-userprovisioning-tutorial/IC769546.png "啟用自動使用者佈建")

7. 按一下 [授與 Box 存取權] **** ，以授權進行此作業並返回 Azure 入口網站。 
   
    ![啟用自動使用者佈建](./media/box-userprovisioning-tutorial/IC769549.png "啟用自動使用者佈建")

8. 在 Azure 入口網站中，按一下 [測試連線]**** 以確保 Azure AD 可以連線到您的 Box 應用程式。 如果連線失敗，請確定您的 Box 帳戶具有小組系統管理員權限，並再試一次「授權」**** 步驟。

9. 在 [通知電子郵件]**** 欄位中，輸入應收到佈建錯誤通知的個人或群組之電子郵件地址，然後勾選核取方塊。

10. 按一下"**保存"。**

11. 在 [對應] 區段中，選取 [同步處理 Azure Active Directory 使用者至 Box]****。

12. 在 [屬性對應]**** 區段中，檢閱從 Azure AD 同步至 Box 的使用者屬性。 選取為 [比對]**** 屬性的屬性會用來比對 Box 中的使用者帳戶以進行更新作業。 選取 [儲存] 按鈕以認可任何變更。

13. 若要啟用 Box 的 Azure AD 佈建服務，在 [設定] 區段中，將 [佈建狀態]**** 變更為 [開啟]****

14. 按一下"**保存"。**

這會對 [使用者和群組] 區段中指派給 Box 的任何使用者和/或群組，啟動首次同步處理。 初始同步處理會比後續同步處理花費更多時間執行，只要服務正在執行，這大約每 40 分鐘便會發生一次。 您可以使用 [同步處理詳細資料]**** 區段來監視進度，並依循連結前往佈建活動記錄，此記錄會描述您 Box 應用程式上佈建服務所執行的所有動作。

如需如何讀取 Azure AD 佈建記錄的詳細資訊，請參閱[關於使用者帳戶自動佈建的報告](../app-provisioning/check-status-user-account-provisioning.md)。

在您的 Box 租用戶中，同步處理而來的使用者會列在 [管理主控台]**** 的 [受管理的使用者]**** 中。

![整合狀態](./media/box-userprovisioning-tutorial/IC769556.png "整合狀態")


## <a name="additional-resources"></a>其他資源

* [管理企業應用的使用者帳戶預配](tutorial-list.md)
* [什麼是使用 Azure 活動目錄的應用程式訪問和單一登入？](../manage-apps/what-is-single-sign-on.md)
* [配置單點登錄](box-tutorial.md)
