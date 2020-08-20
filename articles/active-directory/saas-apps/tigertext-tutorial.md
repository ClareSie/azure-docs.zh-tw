---
title: 教學課程：Azure Active Directory 與 TigerText Secure Messenger 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 TigerText Secure Messenger 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 9dff60767e923bad1322b689acd98e69eb9c2ac6
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2020
ms.locfileid: "88516855"
---
# <a name="tutorial-azure-active-directory-integration-with-tigertext-secure-messenger"></a>教學課程：Azure Active Directory 與 TigerText Secure Messenger 整合

在本教學課程中，您將了解如何整合 TigerText Secure Messenger 與 Azure Active Directory (Azure AD)。

將 TigerText Secure Messenger 與 Azure AD 整合可提供下列優點：

* 您可以在 Azure AD 中控制可存取 TigerText Secure Messenger 的人員。
* 您可以讓使用者使用其 Azure AD 帳戶自動登入 TigerText Secure Messenger (單一登入)。
* 您可以集中管理您的帳戶：Azure 入口網站。

如需軟體即服務 (SaaS) 應用程式與 Azure AD 整合的詳細資訊，請參閱[什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)。

## <a name="prerequisites"></a>Prerequisites

若要設定 Azure AD 與 TigerText Secure Messenger 整合，您需要下列項目：

* Azure AD 訂用帳戶。 如果您沒有 Azure 訂用帳戶，請在開始之前先[建立免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用單一登入的 TigerText Secure Messenger 訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD 單一登入，並整合 TigerText Secure Messenger 與 Azure AD。

TigerText Secure Messenger 支援由 SP 起始的單一登入 (SSO)。

## <a name="add-tigertext-secure-messenger-from-the-azure-marketplace"></a>從 Azure Marketplace 新增 TigerText Secure Messenger

若要進行將 TigerText Secure Messenger 整合到 Azure AD 中的設定，您必須從 Azure Marketplace 將 TigerText Secure Messenger 新增至受控 SaaS 應用程式清單：

1. 登入 [Azure 入口網站](https://portal.azure.com?azure-portal=true)。
1. 在左窗格中，選取 [Azure Active Directory]  。

    ![Azure Active Directory 選項](common/select-azuread.png)

1. 移至 [企業應用程式]  ，然後選取 [所有應用程式]  。

    ![[企業應用程式] 窗格](common/enterprise-applications.png)

1. 若要新增應用程式，請選取窗格頂端的 [+ 新增應用程式]  。

    ![新增應用程式選項](common/add-new-app.png)

1. 在搜尋方塊中，輸入 **TigerText Secure Messenger**。 在搜尋結果中選取 [TigerText Secure Messenger]  ，然後選取 [新增]  以新增應用程式。

    ![結果清單中的 TigerText Secure Messenger](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>設定和測試 Azure AD 單一登入

在本節中，您會以名為 **Britta Simon** 的測試使用者為基礎，設定及測試與 TigerText Secure Messenger 搭配運作的 Azure AD 單一登入。 若要讓單一登入能夠運作，您必須建立 Azure AD 使用者與 TigerText Secure Messenger 中相關使用者之間的連結。

若要設定及測試與 TigerText Secure Messenger 搭配運作的 Azure AD 單一登入，您需要完成下列建構元素：

1. **[設定 Azure AD 單一登入](#configure-azure-ad-single-sign-on)** ，讓您的使用者能夠使用此功能。
1. **[設定 TigerText Secure Messenger 單一登入](#configure-tigertext-secure-messenger-single-sign-on)** ，以在應用程式端設定單一登入設定。
1. **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** ，以使用 Britta Simon 測試 Azure AD 單一登入。
1. **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** ，讓 Britta Simon 能夠使用 Azure AD 單一登入。
1. **[建立 TigerText Secure Messenger 測試使用者](#create-a-tigertext-secure-messenger-test-user)** ，使 TigerText Secure Messenger 中有名為 Britta Simon 的使用者連結至名為 Britta Simon 的 Azure AD 使用者。
1. **[測試單一登入](#test-single-sign-on)** 驗證組態是否能運作。

### <a name="configure-azure-ad-single-sign-on"></a>設定 Azure AD 單一登入

在本節中，您會在 Azure 入口網站中啟用 Azure AD 單一登入。

若要設定與 TigerText Secure Messenger 搭配運作的 Azure AD 單一登入，請執行下列步驟：

1. 在 [Azure 入口網站](https://portal.azure.com/)的 [TigerText Secure Messenger]  應用程式整合頁面上，選取 [單一登入]  。

    ![設定單一登入選項](common/select-sso.png)

1. 在 [選取單一登入方法]  窗格上選取 [SAML/WS-Fed]  模式，以啟用單一登入。

    ![單一登入選取模式](common/select-saml-option.png)

1. 在 [以 SAML 設定單一登入]  窗格上選取 [編輯]  (鉛筆圖示)，以開啟 [基本 SAML 組態]  窗格。

    ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 組態]  窗格中，執行下列步驟：

    ![TigerText Secure Messenger 網域和 URL 單一登入資訊](common/sp-identifier.png)

    1. 在 [登入 URL]  方塊中，輸入 URL：

       `https://home.tigertext.com`

    1. 在 [識別碼 (實體識別碼)]  方塊中，以下列模式輸入 URL：

       `https://saml-lb.tigertext.me/v1/organization/<instance ID>`

    > [!NOTE]
    > [識別碼 (實體識別碼)]  值並非實際值。 請使用實際的識別碼來更新此值。 若要取得此值，請連絡 [TigerText Secure Messenger 支援小組](mailto:prosupport@tigertext.com)。 您也可以參考 Azure 入口網站中 [基本 SAML 組態]  窗格所示的模式。

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，選取 [下載]  ，以從指定的選項下載 [同盟中繼資料 XML]  並儲存在電腦上。

    ![同盟中繼資料 XML 下載選項](common/metadataxml.png)

1. 在 [設定 TigerText Secure Messenger]  區段中，複製您所需的一或多個 URL：

   * **登入 URL**
   * **Azure AD 識別碼**
   * **登出 URL**

    ![複製組態 URL](common/copy-configuration-urls.png)

### <a name="configure-tigertext-secure-messenger-single-sign-on"></a>設定 TigerText Secure Messenger 單一登入

若要在 TigerText Secure Messenger 端設定單一登入，您必須將從 Azure 入口網站下載的 [同盟中繼資料 XML] 和複製的適當 URL 傳送給 [TigerText Secure Messenger 支援小組](mailto:prosupport@tigertext.com)。 TigerText Secure Messenger 小組會確定兩端的 SAML SSO 連線已正確設定。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您會在 Azure 入口網站中建立名為 Britta Simon 的測試使用者。

1. 在 Azure 入口網站中，於左窗格選取 [Azure Active Directory]     > [使用者]   > [所有使用者]  。

    ![[使用者] 和 [所有使用者] 選項](common/users.png)

1. 選取畫面頂端的 [+ 新增使用者]  。

    ![新增使用者選項](common/new-user.png)

1. 在 [使用者]  窗格中，執行下列步驟：

    ![[使用者] 窗格](common/user-properties.png)

    1. 在 [名稱]  方塊中，輸入 **BrittaSimon**。
  
    1. 在 [使用者名稱] 方塊中，輸入 **BrittaSimon\@\<yourcompanydomain>.\<extension>** 。 例如 **BrittaSimon\@contoso.com**。

    1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。

    1. 選取 [建立]  。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 TigerText Secure Messenger 的存取權授與 Britta Simon，讓她能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]   > [所有應用程式]   > [TigerText Secure Messenger]  。

    ![企業應用程式窗格](common/enterprise-applications.png)

1. 在應用程式清單中，選取 [TigerText Secure Messenger]  。

    ![應用程式清單中的 TigerText Secure Messenger](common/all-applications.png)

1. 在左側窗格中，選取 [管理]  之下的 [使用者和群組]  。

    ![[使用者和群組] 選項](common/users-groups-blade.png)

1. 選取 [+ 新增使用者]，然後在 [新增指派] 窗格中選取 [使用者和群組]。

    ![[新增指派] 窗格](common/add-assign-user.png)

1. 在 [使用者和群組] 窗格中，選取 [使用者] 清單中的 [Britta Simon]，然後選擇窗格底部的 [選取]。

1. 如果您預期使用 SAML 判斷提示中的角色值，則在 [選取角色]  窗格的清單中選取適當的使用者角色。 選擇窗格底部的 [選取]  。

1. 在 [新增指派]  窗格中，選取 [指派]  。

### <a name="create-a-tigertext-secure-messenger-test-user"></a>建立 TigerText Secure Messengerr 測試使用者

在本節中，您會在 TigerText Secure Messenger 中建立名為 Britta Simon 的使用者。 請與 [TigerText Secure Messenger 支援小組](mailto:prosupport@tigertext.com)合作，在 TigerText Secure Messenger 中新增 Britta Simon 作為使用者。 您必須先建立和啟動使用者，然後才能使用單一登入。

### <a name="test-single-sign-on"></a>測試單一登入

在本節中，您會使用 MyApps 入口網站來測試您的 Azure AD 單一登入組態。

當您在「我的應用程式」入口網站中選取 [TigerText Secure Messenger]  時，應該會自動登入您已設定單一登入的 TigerText Secure Messenger 訂用帳戶。 如需 My Apps 入口網站的詳細資訊，請參閱[在 My Apps 入口網站上存取和使用應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)。

## <a name="additional-resources"></a>其他資源

* [用於整合 SaaS 應用程式與 Azure Active Directory 的教學課程清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

* [什麼是搭配 Azure Active Directory 的應用程式存取和單一登入？](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

* [什麼是 Azure Active Directory 中的條件式存取？](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
