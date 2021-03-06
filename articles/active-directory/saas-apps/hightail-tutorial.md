---
title: 教學課程：Azure Active Directory 單一登入 (SSO) 與 Hightail 整合 | Microsoft Docs
description: 了解如何設定 Azure Active Directory 與 Hightail 之間的單一登入。
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 11/06/2020
ms.author: jeedes
ms.openlocfilehash: 29f2610c371ecd567a18dc70e51b3eb34754d8ff
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "96180848"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hightail"></a>教學課程：Azure Active Directory 單一登入 (SSO) 與 Hightail 整合

在本教學課程中，您將了解如何整合 Hightail 與 Azure Active Directory (Azure AD)。 在整合 Hightail 與 Azure AD 時，您可以︰

* 在 Azure AD 中控制可存取 Hightail 的人員。
* 讓使用者使用其 Azure AD 帳戶自動登入 Hightail。
* 在 Azure 入口網站集中管理您的帳戶。

## <a name="prerequisites"></a>必要條件

若要開始，您需要下列項目：

* Azure AD 訂用帳戶。 如果沒有訂用帳戶，您可以取得[免費帳戶](https://azure.microsoft.com/free/)。
* 已啟用 Hightail 單一登入 (SSO) 的訂用帳戶。

## <a name="scenario-description"></a>案例描述

在本教學課程中，您會在測試環境中設定和測試 Azure AD SSO。

* Hightail 支援由 **SP 和 IDP** 起始的 SSO
* Hightail 支援 **Just In Time** 使用者佈建

## <a name="adding-hightail-from-the-gallery"></a>從資源庫加入 Hightail

若要設定 Hightail 與 Azure AD 整合，您需要從資源庫將 Hightail 加入受控 SaaS 應用程式清單中。

1. 使用公司或學校帳戶或個人的 Microsoft 帳戶登入 Azure 入口網站。
1. 在左方瀏覽窗格上，選取 [Azure Active Directory] 服務。
1. 巡覽至 [企業應用程式]，然後選取 [所有應用程式]。
1. 若要新增應用程式，請選取 [新增應用程式]  。
1. 在 [從資源庫新增]  區段的搜尋方塊中輸入 **Hightail**。
1. 從結果面板選取 [Hightail]  ，然後新增應用程式。 當應用程式新增至您的租用戶時，請等候幾秒鐘。

## <a name="configure-and-test-azure-ad-single-sign-on-for-hightail"></a>設定及測試 Hightail 的 Azure AD 單一登入

以名為 **B.Simon** 的測試使用者，設定及測試與 Hightail 搭配運作的 Azure AD SSO。 若要讓 SSO 能夠運作，您必須建立 Azure AD 使用者與 Hightail 中相關使用者之間的連結關聯性。

若要設定及測試與 Hightail 搭配運作的 Azure AD SSO，請執行下列步驟：

1. **[設定 Azure AD SSO](#configure-azure-ad-sso)** - 讓您的使用者能夠使用此功能。
    * **[建立 Azure AD 測試使用者](#create-an-azure-ad-test-user)** - 使用 B.Simon 測試 Azure AD 單一登入。
    * **[指派 Azure AD 測試使用者](#assign-the-azure-ad-test-user)** - 讓 B.Simon 能夠使用 Azure AD 單一登入。
1. **[設定 Hightail SSO](#configure-hightail-sso)** - 在應用程式端設定單一登入設定。
    * **[建立 Hightail 測試使用者](#create-hightail-test-user)** - 使 Hightail 中對應的 B.Simon 連結到該使用者在 Azure AD 中的代表項目。
1. **[測試 SSO](#test-sso)** - 驗證組態是否能運作。

## <a name="configure-azure-ad-sso"></a>設定 Azure AD SSO

依照下列步驟在 Azure 入口網站中啟用 Azure AD SSO。

1. 在 Azure 入口網站的 [Hightail] 應用程式整合頁面上，尋找 [管理] 區段並選取 [單一登入]。
1. 在 [**選取單一登入方法**] 頁面上，選取 [**SAML**]。
1. 在 [以 SAML 設定單一登入]  頁面上，按一下 [基本 SAML 設定]  的編輯/畫筆圖示，以編輯設定。

   ![編輯基本 SAML 組態](common/edit-urls.png)

1. 在 [基本 SAML 設定]  區段上，如果您想要以 **IDP** 起始模式設定應用程式，請輸入下列欄位的值：

    a. 在 [識別碼 (實體識別碼)]  文字方塊中，輸入 URL：`https://api.spaces.hightail.com/api/v1/saml/consumer`
    
    b. 在 [回覆 URL] 文字方塊中，輸入 URL：`https://api.spaces.hightail.com/api/v1/saml/consumer`

1. 如果您想要以 **SP** 起始模式設定應用程式，請按一下 [設定其他 URL]  ，然後執行下列步驟：

    在 [登入 URL]  文字方塊中，輸入 URL：`https://spaces.hightail.com/corp-login`

1. Hightail 應用程式會預期要有特定格式的 SAML 判斷提示，因此您必須將自訂屬性對應新增至 SAML 權杖屬性設定。 以下螢幕擷取畫面顯示預設屬性清單。

    ![image](common/default-attributes.png)

1. 除了上述屬性外，Hightail 應用程式還需要在 SAML 回應中多傳回幾個屬性，如下所示。 這些屬性也會預先填入，但您可以根據您的需求來檢閱這些屬性。

    | 名稱 | 來源屬性|
    | -------- |-------- |
    | 名字 | user.givenname |
    | 姓氏 | user.surname |
    | 電子郵件 | user.mail |
    | UserIdentity | user.mail |

1. 在 [以 SAML 設定單一登入]  頁面的 [SAML 簽署憑證]  區段中，尋找 [憑證 (Base64)]  並選取 [下載]  ，以下載憑證並將其儲存在電腦上。

    ![憑證下載連結](common/certificatebase64.png)

1. 在 [設定 Hightail]  區段上，根據您的需求複製適當的 URL。

    ![複製組態 URL](common/copy-configuration-urls.png)

> [!NOTE]
> 在 Hightail 應用程式設定單一登入之前，請透過 Hightail 小組將您的電子郵件網域加入允許清單，以便使用此網域的所有使用者都可以使用單一登入功能。

### <a name="create-an-azure-ad-test-user"></a>建立 Azure AD 測試使用者

在本節中，您將在 Azure 入口網站中建立名為 B.Simon 的測試使用者。

1. 在 Azure 入口網站的左窗格中，依序選取 [Azure Active Directory]  、[使用者]  和 [所有使用者]  。
1. 在畫面頂端選取 [新增使用者]  。
1. 在 [使用者]  屬性中，執行下列步驟：
   1. 在 [名稱]  欄位中，輸入 `B.Simon`。  
   1. 在 [使用者名稱]  欄位中，輸入 username@companydomain.extension。 例如： `B.Simon@contoso.com` 。
   1. 選取 [顯示密碼]  核取方塊，然後記下 [密碼]  方塊中顯示的值。
   1. 按一下 [建立]。

### <a name="assign-the-azure-ad-test-user"></a>指派 Azure AD 測試使用者

在本節中，您會將 Hightail 的存取權授與 B.Simon，讓其能夠使用 Azure 單一登入。

1. 在 Azure 入口網站中，選取 [企業應用程式]  ，然後選取 [所有應用程式]  。
1. 在應用程式清單中，選取 [Hightail]  。
1. 在應用程式的概觀頁面中尋找 [管理]  區段，然後選取 [使用者和群組]  。
1. 選取 [新增使用者]  ，然後在 [新增指派]  對話方塊中選取 [使用者和群組]  。
1. 在 [使用者和群組] 對話方塊的 [使用者] 清單中選取 [B.Simon]，然後按一下畫面底部的 [選取] 按鈕。
1. 如果您需要將角色指派給使用者，您可以從 [選取角色] 下拉式清單中選取。 如果未設定此應用程式的角色，您會看到已選取 [預設存取] 角色。
1. 在 [新增指派]  對話方塊中，按一下 [指派]  按鈕。

## <a name="configure-hightail-sso"></a>設定 Hightail SSO

1. 若要自動執行 Hightail 內的設定，您必須按一下 [安裝擴充功能]  來安裝「我的應用程式安全登入瀏覽器擴充功能」  。

    ![我的應用程式擴充功能](common/install-myappssecure-extension.png)

1. 將擴充功能新增至瀏覽器之後，按一下 [設定 Hightail]  便會將您導向到 Hightail 應用程式。 請從該處提供用以登入 Hightail 的管理員認證。 瀏覽器擴充功能會自動為您設定應用程式，並自動執行步驟 3 到 6。

    ![設定組態](common/setup-sso.png)

1. 如果您想要手動設定 Hightail，請在另一個瀏覽器視窗中開啟 [Hightail]  系統管理員入口網站。

1. 按一下頁面右上角的 **使用者圖示**。 

    ![顯示 [使用者] 圖示的螢幕擷取畫面。](./media/hightail-tutorial/configure1.png)

1. 按一下 [View Admin Console] \(檢視管理主控台\)  索引標籤。

    ![顯示使用者 [檢視系統管理員主控台] 按鈕的螢幕擷取畫面。](./media/hightail-tutorial/configure2.png)

1. 在頂端功能表中，按一下 [SAML]  索引標籤，然後執行下列步驟：

    ![顯示 [SAML] 索引標籤的螢幕擷取畫面，您可以在其中輸入 [登入 URL] 和 [SAML 憑證]。](./media/hightail-tutorial/configure3.png)

    a. 在 [Login URL] \(登入 URL\)  文字方塊中，貼上從 Azure 入口網站複製的 [登入 URL]  值。

    b. 在記事本中開啟從 Azure 入口網站下載的 Base-64 編碼憑證，將憑證內容複製到剪貼簿，然後貼到 [SAML Certificate] \(SAML 憑證\)  文字方塊中。

    c. 按一下 [COPY] \(複製\)  以複製執行個體的 SAML 取用者 URL，然後將其貼在 Azure 入口網站上 [基本 SAML 設定]  區段的 [回覆 URL]  文字方塊中。

    d. 按一下 [Save Configurations] \(儲存設定\)  。

### <a name="create-hightail-test-user"></a>建立 Hightail 測試使用者

本節會在 Hightail 中建立名為 Britta Simon 的使用者。 Hightail 支援預設會啟用的 Just-In-Time 使用者佈建。 在這一節沒有您需要進行的動作項目。 如果 Hightail 中還沒有任何使用者存在，在驗證之後就會建立新的使用者。

> [!NOTE]
> 如果您需要手動建立使用者，您必須透過 [Hightail 支援小組](mailto:support@hightail.com)。

## <a name="test-sso"></a>測試 SSO 

在本節中，您會使用下列選項來測試您的 Azure AD 單一登入組態。 

#### <a name="sp-initiated"></a>SP 起始：

* 在 Azure 入口網站中按一下 [測試此應用程式]。 這會重新導向至您可以在其中起始登入流程的 Hightail 登入 URL。  

* 直接移至 Hightail 登入 URL，然後從該處起始登入流程。

#### <a name="idp-initiated"></a>IDP 起始：

* 在 Azure 入口網站中按一下 [測試此應用程式]，您應該會自動登入您已設定 SSO 的 Hightail 

您也可以使用 Microsoft 存取面板，以任何模式測試應用程式。 當您按一下存取面板中的 Hightail 圖格時，如果是在 SP 模式中設定，您會重新導向至 [應用程式登入] 頁面來起始登入流程，如果在 IDP 模式中設定，則應該會自動登入已設定 SSO 的 Hightail。 如需「存取面板」的詳細資訊，請參閱[存取面板簡介](../user-help/my-apps-portal-end-user-access.md)。


## <a name="next-steps"></a>後續步驟

設定 Hightail 後，您可以強制執行工作階段控制項，以即時防止組織的敏感資料遭到外洩和滲透。 工作階段控制項會從條件式存取延伸。 [了解如何使用 Microsoft Cloud App Security 來強制執行工作階段控制項](/cloud-app-security/proxy-deployment-any-app)。