---
title: 登入後，應用程式頁面上顯示錯誤訊息 |Microsoft Docs
description: 如何解決應用程式傳回錯誤訊息時，Azure AD 登入的問題。
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 07/11/2017
ms.author: kenwith
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 779286a43f8b20ce9a9a528e14eaa930763d82b4
ms.sourcegitcommit: 8e7316bd4c4991de62ea485adca30065e5b86c67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/17/2020
ms.locfileid: "94651578"
---
# <a name="an-app-page-shows-an-error-message-after-the-user-signs-in"></a>應用程式頁面會在使用者登入之後顯示錯誤訊息

在此案例中，Azure Active Directory (Azure AD) 會將使用者登入。 但應用程式會顯示錯誤訊息，而且不會讓使用者完成登入流程。 問題在於應用程式不接受 Azure AD 發出的回應。

應用程式不接受來自 Azure AD 的回應有幾個可能的原因。 如果錯誤訊息無法清楚地找出回應中缺少的內容，請嘗試下列動作：

-   如果應用程式是 Azure AD 資源庫，請確認您已遵循 [如何在 Azure AD 中對應用程式進行 SAML 型單一登入](./debug-saml-sso-issues.md)的步驟。

-   使用 [Fiddler](https://www.telerik.com/fiddler) 之類的工具來捕捉 SAML 要求、回應和權杖。

-   將 SAML 回應傳送給應用程式廠商，並要求他們遺漏什麼。

## <a name="attributes-are-missing-from-the-saml-response"></a>SAML 回應中缺少屬性

若要在將于 Azure AD 回應中傳送的 Azure AD 設定中新增屬性，請遵循下列步驟：

1. 開啟 [**Azure 入口網站**](https://portal.azure.com/) ，然後以全域系統管理員或共同管理員身分登入。

2. 在左側導覽窗格的頂端，選取 [ **所有服務** ] 以開啟 Azure AD 延伸模組。

3. 在篩選搜尋方塊中輸入 **Azure Active Directory** ，然後選取 [ **Azure Active Directory**]。

4. 在 Azure AD 流覽窗格中選取 [ **企業應用程式** ]。

5. 選取 [ **所有應用程式** ] 以查看您的應用程式清單。

   > [!NOTE]
   > 如果您沒有看到想要的應用程式，請使用 [**所有應用程式] 清單** 頂端的 [**篩選器**] 控制項。 將 [ **顯示** ] 選項設定為 [所有應用程式]。

6. 選取您要設定單一登入的應用程式。

7. 應用程式載入之後，請在流覽窗格中選取 [ **單一登入** ]。

8. 在 [ **使用者屬性** ] 區段中，選取 [ **View] 和 [編輯所有其他使用者屬性**]。 在此，您可以在使用者登入時，變更要在 SAML 權杖中傳送給應用程式的屬性。

   若要新增屬性︰

   1. 選取 [ **加入屬性**]。 輸入 **名稱**，然後從下拉式清單中選取 **值** 。

   1.  選取 [儲存]。 您會在資料表中看到新的屬性。

9. 儲存組態。

   下次當使用者登入應用程式時，Azure AD 會在 SAML 回應中傳送新的屬性。

## <a name="the-app-doesnt-identify-the-user"></a>應用程式不會識別使用者

登入應用程式失敗，因為 SAML 回應缺少角色之類的屬性。 或它會失敗，因為應用程式預期 **NameID** (使用者識別碼) 屬性有不同的格式或值。

如果您使用 [Azure AD 自動化使用者](../app-provisioning/user-provisioning.md) 布建來建立、維護及移除應用程式中的使用者，請確認使用者已布建至 SaaS 應用程式。 如需詳細資訊，請參閱 [未將任何使用者布建至 Azure AD 資源庫應用程式](../app-provisioning/application-provisioning-config-problem-no-users-provisioned.md)。

## <a name="add-an-attribute-to-the-azure-ad-app-configuration"></a>將屬性新增至 Azure AD 應用程式設定

若要變更使用者識別碼的值，請遵循下列步驟︰

1. 開啟 [**Azure 入口網站**](https://portal.azure.com/) ，然後以全域系統管理員或共同管理員身分登入。

2. 選取左側導覽窗格頂端的 [ **所有服務** ]，以開啟 Azure AD 延伸模組。

3. 在篩選搜尋方塊中輸入 **Azure Active Directory** ，然後選取 [ **Azure Active Directory**]。

4. 在 Azure AD 流覽窗格中選取 [ **企業應用程式** ]。

5. 選取 [ **所有應用程式** ] 以查看您的應用程式清單。

   > [!NOTE]
   > 如果您沒有看到想要的應用程式，請使用 [**所有應用程式] 清單** 頂端的 [**篩選器**] 控制項。 將 [ **顯示** ] 選項設定為 [所有應用程式]。

6. 選取您要為 SSO 設定的應用程式。

7. 應用程式載入之後，請在流覽窗格中選取 [ **單一登入** ]。

8. 在 [ **使用者屬性**] 下，從 [ **使用者識別碼** ] 下拉式清單中選取使用者的唯一識別碼。

## <a name="change-the-nameid-format"></a>變更 NameID 格式

如果應用程式需要 **NameID** (使用者識別碼) 屬性的另一個格式，請參閱 [編輯 NameID](../develop/active-directory-saml-claims-customization.md#editing-nameid) 以變更 nameid 格式。

Azure AD 會根據選取的值或 SAML AuthRequest 中的應用程式所要求的格式，來選取 **NameID** 屬性的格式 (使用者識別碼) 。 如需詳細資訊，請參閱 [單一登入 SAML 通訊協定](../develop/single-sign-on-saml-protocol.md#nameidpolicy)的「NameIDPolicy」一節。

## <a name="the-app-expects-a-different-signature-method-for-the-saml-response"></a>應用程式預期 SAML 回應會有不同的簽章方法

若要變更 Azure AD 將 SAML 權杖的哪些部分經過數位簽署，請遵循下列步驟：

1. 開啟 [Azure 入口網站](https://portal.azure.com/) ，然後以全域系統管理員或共同管理員身分登入。

2. 選取左側導覽窗格頂端的 [ **所有服務** ]，以開啟 Azure AD 延伸模組。

3. 在篩選搜尋方塊中輸入 **Azure Active Directory** ，然後選取 [ **Azure Active Directory**]。

4. 在 Azure AD 流覽窗格中選取 [ **企業應用程式** ]。

5. 選取 [ **所有應用程式** ] 以查看您的應用程式清單。

   > [!NOTE]
   > 如果您沒有看到想要的應用程式，請使用 [**所有應用程式] 清單** 頂端的 **篩選** 控制項。 將 [ **顯示** ] 選項設定為 [所有應用程式]。

6. 選取您要設定單一登入的應用程式。

7. 應用程式載入之後，請在流覽窗格中選取 [ **單一登入** ]。

8. 在 [ **SAML 簽署憑證**] 底下，選取 [  **顯示 advanced Certificate 簽署設定**]。

9. 從下列選項中選取應用程式預期的 **簽章選項** ：

   * **簽署 SAML 回應**
   * **簽署 SAML 回應和判斷提示**
   * **簽署 SAML 判斷提示**

   下次當使用者登入應用程式時，Azure AD 將會簽署您所選取的 SAML 回應部分。

## <a name="the-app-expects-the-sha-1-signing-algorithm"></a>應用程式需要 SHA-1 簽署演算法

根據預設，Azure AD 會使用最安全的演算法簽署 SAML 權杖。 除非應用程式需要 SHA-1，否則建議您不要將簽署演算法變更為 *sha-1* 。

若要變更簽署演算法，請遵循下列步驟：

1. 開啟 [Azure 入口網站](https://portal.azure.com/) ，然後以全域系統管理員或共同管理員身分登入。

2. 選取左側導覽窗格頂端的 [ **所有服務** ]，以開啟 Azure AD 延伸模組。

3. 在篩選搜尋方塊中輸入 **Azure Active Directory** ，然後選取 [ **Azure Active Directory**]。

4. 在 Azure AD 流覽窗格中選取 [ **企業應用程式** ]。

5. 選取 [ **所有應用程式** ] 以查看您的應用程式清單。

   > [!NOTE]
   > 如果您沒有看到想要的應用程式，請使用 [**所有應用程式] 清單** 頂端的 **篩選** 控制項。 將 [ **顯示** ] 選項設定為 [所有應用程式]。

6. 選取您要設定單一登入的應用程式。

7. 應用程式載入之後，請從應用程式左側的流覽窗格中選取 [ **單一登入** ]。

8. 在 [ **SAML 簽署憑證**] 底下，選取 [ **顯示 advanced Certificate 簽署設定**]。

9. 選取 **sha-1** 作為 **簽署演算法**。

   下次當使用者登入應用程式時，Azure AD 會使用 SHA-1 演算法簽署 SAML 權杖。

## <a name="next-steps"></a>後續步驟
[如何對 Azure AD 中的應用程式進行 SAML 型單一登入的偵錯工具](./debug-saml-sso-issues.md)。