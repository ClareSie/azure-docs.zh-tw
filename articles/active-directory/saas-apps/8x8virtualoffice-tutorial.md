---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 8x8 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 8x8 之間的單一登入。
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b34a6edf-e745-4aec-b0b2-7337473d64c5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/20/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c598222978a1c831be6f5e9db9eb87b2d6b6b96
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/24/2020
ms.locfileid: "78968680"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-8x8"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 8x8 整合

在本教學課程中，您會了解如何將 8x8 與 Azure Active Directory (Azure AD) 進行整合。 在整合 8x8 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 8x8 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 8x8。
* 在 Azure 入口網站集中管理您的帳戶。

若要深入了解 SaaS 應用程式與 Azure AD 整合，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)。

## <a name="prerequisites"></a>Prerequisites

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 8x8 訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* 8x8 支援 **SP 和 IDP** 起始的 SSO

* 設定 8x8 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)。

> [!NOTE]
> 此應用程式的識別碼是固定的字串值，因此一個租用戶中只能設定一個執行個體。

## <a name="adding-8x8-from-the-gallery"></a>從資源庫新增 8x8

若要設定將 8x8 整合到 Azure AD 中，您需要將 8x8 從資源庫新增到受控 SaaS 應用程式清單。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory]  服務。
1. 巡覽至 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **8x8**。
1. 從結果面板選取 [8x8]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-8x8"></a>設定及測試 8x8 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 8x8 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 8x8 中相關使用者之間的連結關聯性。

若要設定及測試與 8x8 搭配運作的 Azure AD SSO，請完成下列建置組塊：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 8x8 SSO](#configure-8x8-sso)** - 在應用程式端設定單一登入設定。
    1. **[建立 8x8 測試使用者](#create-8x8-test-user)** - 使 8x8 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [8x8]  應用程式整合頁面上，尋找 [管理]  區段並選取 [單一登入]  。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  區段上，執行下列步驟：

    a. 在 [識別碼]  文字方塊中，鍵入 URL：`https://sso.8x8.com/saml2`

    b. 在 [回覆 URL]  文字方塊中，鍵入 URL：`https://sso.8x8.com/saml2`

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。 在本教學課程中，您稍後會在 [設定 8x8 SSO]  區段中用到此憑證。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 8x8]  區段上複製 URL，您稍後會在本教學課程中用到這些 URL 值。

    ![複製組態 URL](./media/8x8virtualoffice-tutorial/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下頁面底部的 [新增]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 8x8 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [8x8]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。

   ![[使用者和群組] 連結](common/users-groups-blade.png)

1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。

    ![[新增使用者] 連結](common/add-assign-user.png)

1. 在 [使用者和群組]  對話方塊的 [使用者] 清單中選取 [B.Simon]  ，然後按一下畫面底部的 [選取]  按鈕。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-8x8-sso"></a>設定 8x8 SSO

本教學課程的下一個部分取決於您要用於 8x8 的訂用帳戶種類。

* 若為使用 Configuration Manager 來進行系統管理的 8x8 版本和 X 系列客戶，請參閱[設定 8x8 Configuration Manager](#configure-8x8-configuration-manager)。
* 若為使用帳戶管理員來進行系統管理的 Virtual Office 客戶，請參閱[設定 8x8 帳戶管理員](#configure-8x8-account-manager)。

### <a name="configure-8x8-configuration-manager"></a>設定 8x8 Configuration Manager

1. 登入 8x8 [Configuration Manager](https://vo-cm.8x8.com/)。

1. 從首頁按一下 [身分識別管理]  。

    ![8x8 Configuration Manager](./media/8x8virtualoffice-tutorial/configure1.png)

1. 核取 [單一登入 (SSO)]  ，然後選取 [Microsoft Azure AD]  。

    ![8x8 Configuration Manager](./media/8x8virtualoffice-tutorial/configure2.png)

1. 將 Azure AD 中 [設定使用 SAML 的單一登入]  頁面內的三個 URL 和簽署憑證，複製到 8x8 Configuration Manager 中的 [Microsoft Azure AD SAML 設定]  區段。

    ![8x8 Configuration Manager](./media/8x8virtualoffice-tutorial/configure3.png)

    a. 將 [登入 URL]  複製到 [IDP 登入 URL]  。

    b. 將 [Azure AD 識別碼]  複製到 [IDP 簽發者 URL/URN]  。

    c. 將 [登出 URL]  複製到 [IDP 登出 URL]  。

    d. 下載 [憑證 (Base64)]  ，並上傳至 [憑證]  。

    e. 按一下 [檔案]  。

### <a name="configure-8x8-account-manager"></a>設定 8x8 帳戶管理員

1. 以系統管理員身分登入 8x8 Virtual Office 租用戶。

1. 在 [應用程式面板] 上選取 [Virtual Office 帳戶管理員]  。

    ![在應用程式端設定](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_001.png)

1. 選取 [商務]  帳戶以進行管理，並按一下 [登入]  按鈕。

    ![在應用程式端設定](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_002.png)

1. 按一下功能表清單中的 [帳戶]  索引標籤。

    ![在應用程式端設定](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_003.png)

1. 按一下 [帳戶] 清單中的 [單一登入]  。

    ![在應用程式端設定](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_004.png)

1. 選取 [驗證方法] 底下的 [單一登入]  ，然後按一下 [SAML]  。

    ![在應用程式端設定](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_005.png)

1. 在 [SAML 單一登入]  區段中，執行下列步驟：

    ![在應用程式端設定](./media/8x8virtualoffice-tutorial/tutorial_8x8virtualoffice_006.png)

    a. 在 [登入 URL]  文字方塊中，貼上您從 Azure 入口網站複製的**登入 URL** 值。

    b. 在 [登出 URL]  文字方塊中，貼上您從 Azure 入口網站複製的**登出 URL** 值。

    c. 在 [簽發者 URL]  文字方塊中，貼上您從 Azure 入口網站複製的 **Azure AD 識別碼**值。

    d. 按一下 [瀏覽]  按鈕來上傳您從 Azure 入口網站下載的憑證。

    e. 按一下 [儲存]  按鈕。

### <a name="create-8x8-test-user"></a>建立 8x8 測試使用者

在本節中，您會在 8x8 中建立名為 Britta Simon 的使用者。 請與 [8x8 支援小組](https://www.8x8.com/about-us/contact-us)合作，在 8x8 平台中新增使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

## <a name="test-sso"></a>測試 SSO

在本節中，您會使用存取面板來測試您的 Azure AD 單一登入設定。

當您在存取面板中按一下 [8x8] 圖格時，應該會自動登入您已設定 SSO 的 8x8。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

- [如何與 Azure Active Directory 整合 SaaS 應用程式的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [嘗試搭配 Azure AD 使用 8x8](https://aad.portal.azure.com/)

- [什麼是 Microsoft Cloud App Security 中的工作階段控制項？](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [如何使用進階可見性和控制項保護 8x8](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)