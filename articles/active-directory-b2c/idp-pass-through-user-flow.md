---
title: 透過使用者流程將存取權杖傳遞至您的應用程式
titleSuffix: Azure AD B2C
description: 瞭解如何在 Azure Active Directory B2C 的使用者流程中，以宣告的形式傳遞 OAuth 2.0 身分識別提供者的存取權杖。
services: active-directory-b2c
author: msmimart
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: how-to
ms.date: 08/17/2019
ms.author: mimart
ms.subservice: B2C
ms.openlocfilehash: 5b834dda926b7da1241a325e1453143eccafaf30
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "87488766"
---
# <a name="pass-an-access-token-through-a-user-flow-to-your-application-in-azure-active-directory-b2c"></a>透過使用者流程將存取權杖傳遞到 Azure Active Directory B2C 中的應用程式

Azure Active Directory B2C (Azure AD B2C) 中的 [使用者流程](user-flow-overview.md) ，可讓您的應用程式使用者有機會註冊或使用身分識別提供者登入。 當發生這種情況時，Azure AD B2C 會從身分識別提供者處收到[存取權杖](tokens-overview.md)。 Azure AD B2C 會使用該權杖來擷取使用者的相關資訊。 您在使用者流程中啟用宣告，以將權杖傳遞至您在 Azure AD B2C 中註冊的應用程式。

Azure AD B2C 目前僅支援傳遞 [OAuth 2.0](authorization-code-flow.md) 身分識別提供者（包括 [Facebook](identity-provider-facebook.md) 和 [Google](identity-provider-google.md)）的存取權杖。 對於所有其他識別提供者，宣告會傳回空白。

## <a name="prerequisites"></a>Prerequisites

* 您的應用程式必須使用 [建議的使用者流程](user-flow-versions.md)。
* 您的自訂原則是使用 OAuth 2.0 識別提供者設定。

## <a name="enable-the-claim"></a>啟用宣告

1. 以 Azure AD B2C 租用戶的全域管理員身分登入 [Azure 入口網站](https://portal.azure.com/)。
2. 確定您使用的目錄包含您的 Azure AD B2C 租用戶。 在頂端功能表中選取 [目錄 + 訂用帳戶] 篩選，然後選擇包含您租用戶的目錄。
3. 選擇 Azure 入口網站左上角的 [所有服務]，搜尋並選取 [Azure AD B2C]。
4. 選取 [ **使用者流程] (原則) **，然後選取您的使用者流程。 例如，B2C_1_signupsignin1  。
5. 選取 [應用程式宣告]****。
6. 啟用身分 **識別提供者存取權杖** 宣告。

    ![啟用身分識別提供者存取權杖宣告](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-app-claim.png)

7. 按一下 [儲存]**** 以儲存使用者流程。

## <a name="test-the-user-flow"></a>測試使用者流程

在 Azure AD B2C 中測試應用程式時，將 Azure AD B2C 權杖傳回到 `https://jwt.ms` 以檢閱其中的宣告很有用。

1. 在使用者流程的 [概觀] 頁面中，選取 [執行使用者流程]****。
2. 針對**應用程式**，請選取您先前註冊的應用程式。 若要查看下面範例中的權杖，**回覆 URL** 應該會顯示 `https://jwt.ms`。
3. 按一下 [執行使用者流程]****，然後使用您的帳戶認證登入。 您應該會在 **idp_access_token** 宣告中看到識別提供者的存取權杖。

    您應該會看到類似下列範例的內容：

    ![已反白顯示 idp_access_token 區塊的 jwt.ms 中已解碼 token](./media/idp-pass-through-user-flow/idp-pass-through-user-flow-token.PNG)

## <a name="next-steps"></a>接下來的步驟

若要深入瞭解 [Azure AD B2C 權杖的總覽](tokens-overview.md)，請深入瞭解。
