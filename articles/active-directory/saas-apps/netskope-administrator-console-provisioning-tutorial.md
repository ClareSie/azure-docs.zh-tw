---
title: 教學課程：以 Azure Active Directory 設定 Netskope 使用者驗證來自動佈建使用者 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 來對 Netskope 使用者驗證自動佈建及取消佈建使用者帳戶。
services: active-directory
author: zchia
writer: zchia
manager: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/07/2019
ms.author: Zhchia
ms.openlocfilehash: 46766a7439185714648572f3f1b9d51ef96abba6
ms.sourcegitcommit: 0b9fe9e23dfebf60faa9b451498951b970758103
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/07/2020
ms.locfileid: "94357465"
---
# <a name="tutorial-configure-netskope-user-authentication-for-automatic-user-provisioning"></a>教學課程：設定 Netskope 使用者驗證以自動佈建使用者

本教學課程旨在示範將 Azure AD 設定為可對 Netskope 使用者驗證自動佈建及取消佈建使用者和/或群組時，Netskope 使用者驗證與 Azure Active Directory (Azure AD) 中須執行的步驟。

> [!NOTE]
> 本教學課程會說明建置在 Azure AD 使用者佈建服務之上的連接器。 如需此服務的用途、運作方式和常見問題等重要詳細資訊，請參閱[使用 Azure Active Directory 對 SaaS 應用程式自動佈建和取消佈建使用者](../app-provisioning/user-provisioning.md)。
>
> 此連接器目前為公開預覽版。 如需有關預覽功能的一般 Microsoft Azure 使用規定詳細資訊，請參閱 [Microsoft Azure 預覽版增補使用規定](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)。

## <a name="prerequisites"></a>Prerequisites

本教學課程中概述的案例假設您已經具有下列必要條件：

* Azure AD 租用戶
* [Netskope 使用者驗證租用戶](https://www.netskope.com/)
* Netskope 使用者驗證中具有管理員權限的使用者帳戶。

## <a name="assigning-users-to-netskope-user-authentication"></a>將使用者指派給 Netskope 使用者驗證

Azure Active Directory 使用所謂「指派」的概念，決定應該授權哪些使用者存取選取的應用程式。 在自動使用者佈建的內容中，只有已指派至 Azure AD 中應用程式的使用者和/或群組會進行同步處理。

設定並啟用自動使用者佈建之前，您應先決定 Azure AD 中的哪些使用者和/或群組需要存取 Netskope 使用者驗證。 一旦決定後，您可以依照此處的指示，將這些使用者和/或群組指派給 Netskope 使用者驗證：
* [將使用者或群組指派給企業應用程式](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-netskope-user-authentication"></a>將使用者指派給 Netskope 使用者驗證的重要秘訣

* 建議將單一 Azure AD 使用者指派給 Netskope 使用者驗證，以測試自動使用者佈建的設定。 其他使用者及/或群組可能會稍後再指派。

* 將使用者指派給 Netskope 使用者驗證時，您必須在 [指派] 對話方塊中選取任何有效的應用程式特有角色 (如果有的話)。 具有 **預設存取** 角色的使用者會從佈建中排除。

## <a name="set-up-netskope-user-authentication-for-provisioning"></a>設定 Netskope 使用者驗證以進行佈建

1. 登入您的 [Netskope 使用者驗證管理主控台](https://netskope.goskope.com/)。 瀏覽至 [首頁] > [設定]。

    ![Netskope 使用者驗證管理主控台](media/netskope-administrator-console-provisioning-tutorial/admin.png)

2.  瀏覽至 [工具]。 在 [工具] 功能表底下，瀏覽至 [目錄工具] > [SCIM 整合]。

    ![Netskope 使用者驗證工具](media/netskope-administrator-console-provisioning-tutorial/tools.png)

    ![Netskope 使用者驗證新增 SCIM](media/netskope-administrator-console-provisioning-tutorial/directory.png)

3. 向下捲動並按一下 [新增權杖] 按鈕。 在 [新增 OAuth 用戶端名稱] 對話方塊中，提供 **用戶端名稱**，然後按一下 [儲存] 按鈕。

    ![Netskope 使用者驗證新增權杖](media/netskope-administrator-console-provisioning-tutorial/add.png)

    ![Netskope 使用者驗證用戶端名稱](media/netskope-administrator-console-provisioning-tutorial/clientname.png)

3.  複製 **SCIM 伺服器 URL** 和 **權杖**。 這些值會分別輸入到 Azure 入口網站中 Netskope 使用者驗證應用程式 [佈建] 索引標籤中的 [租用戶 URL] 和 [祕密權杖] 欄位。

    ![Netskope 使用者驗證建立權杖](media/netskope-administrator-console-provisioning-tutorial/token.png)

## <a name="add-netskope-user-authentication-from-the-gallery"></a>從資源庫新增 Netskope 使用者驗證

將 Netskope 使用者驗證設定為可使用 Azure AD 自動佈建使用者之前，您必須將 Netskope 使用者驗證從 Azure AD 應用程式庫新增至您的受控 SaaS 應用程式清單。

**若要從 Azure AD 應用程式庫新增 Netskope 使用者驗證，請執行下列步驟：**

1. 在 **[Azure 入口網站](https://portal.azure.com)** 的左方瀏覽窗格中，選取 [Azure Active Directory]。

    ![Azure Active Directory 按鈕](common/select-azuread.png)

2. 移至 [企業應用程式]，然後選取 [所有應用程式]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

3. 若要新增新的應用程式，請選取窗格頂端的 [新增應用程式] 按鈕。

    ![新增應用程式按鈕](common/add-new-app.png)

4. 在搜尋方塊中，輸入 **Netskope 使用者驗證**，從結果面板中選取 [Netskope 使用者驗證]，然後按一下 [新增] 按鈕來新增應用程式。

    ![結果清單中的 Netskope 使用者驗證](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-netskope-user-authentication"></a>對 Netskope 使用者驗證設定自動使用者佈建 

本節將引導您逐步設定 Azure AD 佈建服務，以根據 Azure AD 中的使用者和/或群組指派，在 Netskope 使用者驗證中建立、更新和停用使用者和/或群組。

> [!TIP]
> 建議您選擇為 Netskope 使用者驗證啟用 SAML 型單一登入，請遵循 [Netskope 使用者驗證單一登入教學課程](./netskope-cloud-security-tutorial.md)中提供的指示。 雖然自動使用者佈建和單一登入這兩個功能互相補充，您還是可以將這兩者分開設定。

> [!NOTE]
> 若要深入了解 Netskope 使用者驗證的 SCIM 端點，請參閱[本文](https://docs.google.com/document/d/1n9P_TL98_kd1sx5PAvZL2HS6MQAqkQqd-OSkWAAU6ck/edit#heading=h.prxq74iwdpon)。

### <a name="to-configure-automatic-user-provisioning-for-netskope-user-authentication-in-azure-ad"></a>若要在 Azure AD 中對 Netskope 使用者驗證設定自動使用者佈建：

1. 登入 [Azure 入口網站](https://portal.azure.com)。 選取 [企業應用程式]，然後選取 [所有應用程式]。

    ![企業應用程式刀鋒視窗](common/enterprise-applications.png)

2. 在應用程式清單中，選取 [Netskope 使用者驗證]  。

    ![應用程式清單中的 Netskope 使用者驗證](common/all-applications.png)

3. 選取 [佈建]  索引標籤。

    ![[管理] 選項的螢幕擷取畫面，並已指出 [佈建] 選項。](common/provisioning.png)

4. 將 [佈建模式]  設定為 [自動]  。

    ![[佈建模式] 下拉式清單的螢幕擷取畫面，並已指出 [自動] 選項。](common/provisioning-automatic.png)

5. 在 [管理員認證] 區段下，輸入稍早在 [租用戶 URL] 中擷取的 **SCIM 伺服器 URL** 值。 輸入稍早在 [祕密權杖] 中所擷取的 **權杖** 值。 按一下 [測試連線]，以確保 Azure AD 可連線到 Netskope 使用者驗證。 如果連線失敗，請確定您的 Netskope 使用者驗證帳戶具有管理員權限並再試一次。

    ![租用戶 URL + 權杖](common/provisioning-testconnection-tenanturltoken.png)

6. 在 [通知電子郵件]  欄位中，輸入應該收到佈建錯誤通知的個人或群組電子郵件地址，然後選取 [發生失敗時傳送電子郵件通知]  核取方塊。

    ![通知電子郵件](common/provisioning-notification-email.png)

7. 按一下 [檔案]  。

8. 在 [對應] 區段中，選取 [將 Azure Active Directory 使用者同步至 Netskope 使用者驗證]。

    ![Netskope 使用者驗證使用者對應](media/netskope-administrator-console-provisioning-tutorial/usermappings.png)

9. 在 [屬性對應] 區段中，檢閱從 Azure AD 同步至 Netskope 使用者驗證的使用者屬性。 選取為 [比對] 屬性 (Property) 的屬性 (Attribute) 會用來比對 Netskope 使用者驗證中的使用者帳戶，以進行更新作業。 選取 [儲存]  按鈕以認可所有變更。

    ![Netskope 使用者驗證使用者屬性](media/netskope-administrator-console-provisioning-tutorial/userattributes.png)

10. 在 [對應] 區段中，選取 [將 Azure Active Directory 群組同步至 Netskope 使用者驗證]。

    ![Netskope 使用者驗證群組對應](media/netskope-administrator-console-provisioning-tutorial/groupmappings.png)

11. 在 [屬性對應] 區段中，檢閱從 Azure AD 同步至 Netskope 使用者驗證的群組屬性。 選取為 [比對] 屬性 (Property) 的屬性 (Attribute) 會用來比對 Netskope 使用者驗證中的群組，以進行更新作業。 選取 [儲存]  按鈕以認可所有變更。

    ![Netskope 使用者驗證群組屬性](media/netskope-administrator-console-provisioning-tutorial/groupattributes.png)

12. 若要設定範圍篩選，請參閱[範圍篩選教學課程](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md)中提供的下列指示。

13. 若要對 Netskope 使用者驗證啟用 Azure AD 佈建服務，請在 [設定] 區段中，將 [佈建狀態] 變更為 [開啟]。

    ![佈建狀態已切換為開啟](common/provisioning-toggle-on.png)

14. 透過在 [設定] 區段的 [範圍] 中選擇需要的值，可定義要佈建到 Netskope 使用者驗證的使用者和/或群組。

    ![佈建範圍](common/provisioning-scope.png)

15. 當您準備好要佈建時，按一下 [儲存]  。

    ![儲存雲端佈建設定](common/provisioning-configuration-save.png)

此作業會對在 [設定]  區段的 [範圍]  中定義的所有使用者和/或群組，啟動首次同步處理。 初始同步處理會比後續同步處理花費更多時間執行，只要 Azure AD 佈建服務正在執行，這大約每 40 分鐘便會發生一次。 您可以使用 [同步詳細資料] 區段來監視進度，並遵循連結來取得佈建活動報告；當中會描述 Netskope 使用者驗證上 Azure AD 佈建服務所執行的所有動作。

如需如何讀取 Azure AD 佈建記錄的詳細資訊，請參閱[關於使用者帳戶自動佈建的報告](../app-provisioning/check-status-user-account-provisioning.md)。

## <a name="additional-resources"></a>其他資源

* [管理企業應用程式的使用者帳戶佈建](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>後續步驟

* [瞭解如何針對佈建活動檢閱記錄和取得報告](../app-provisioning/check-status-user-account-provisioning.md)