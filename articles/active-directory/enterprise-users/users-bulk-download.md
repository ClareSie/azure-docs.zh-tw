---
title: 在 Azure Active Directory 入口網站中下載使用者清單 |Microsoft Docs
description: 在 Azure Active Directory 的 Azure 系統管理中心內大量下載使用者記錄。
services: active-directory
author: curtand
ms.author: curtand
manager: daveba
ms.date: 12/02/2020
ms.topic: how-to
ms.service: active-directory
ms.subservice: enterprise-users
ms.workload: identity
ms.custom: it-pro
ms.reviewer: krbain
ms.collection: M365-identity-device-management
ms.openlocfilehash: 042139a39c28ee4944a7f3f766fc61b163629843
ms.sourcegitcommit: 16c7fd8fe944ece07b6cf42a9c0e82b057900662
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/03/2020
ms.locfileid: "96574389"
---
# <a name="download-a-list-of-users-in-azure-active-directory-portal"></a>在 Azure Active Directory 入口網站中下載使用者清單

Azure Active Directory (Azure AD) 支援大量使用者匯入 (建立) 作業。

## <a name="required-permissions"></a>所需的權限

若要從 Azure AD 系統管理中心下載使用者清單，您必須使用指派給一或多個組織層級系統管理員角色的使用者登入 Azure AD (使用者管理員是) 所需的最低角色。 來賓邀請者和應用程式開發人員不會被視為系統管理員角色。

## <a name="to-download-a-list-of-users"></a>下載使用者清單

1. 以組織中的使用者系統管理員帳戶登[入您的 Azure AD 組織](https://aad.portal.azure.com)。
2. 流覽至 Azure Active Directory > 使用者。 然後，選取您要包含在下載中的使用者，方法是在每位使用者旁邊的左邊資料行中勾選方塊。 注意：目前沒有任何方法可選取要匯出的所有使用者。 每一個都必須個別選取。
3. 在 Azure AD 中，選取 [**使用者**  >  **下載使用者**]。
4. 在 [ **下載使用者** ] 頁面上，選取 [ **開始** ] 以接收列出使用者設定檔屬性的 CSV 檔案。 如果發生錯誤，您可以在 [大量作業結果] 頁面上，下載並檢視結果檔案。 此檔案包含每個錯誤的原因。

   ![選取您想要下載的使用者清單](./media/users-bulk-download/bulk-download.png)

   下載檔案將包含篩選過的使用者清單。

   包含下列使用者屬性：

   - userPrincipalName
   - displayName
   - surname
   - mail
   - givenName
   - objectId
   - userType
   - jobTitle
   - department
   - accountEnabled
   - usageLocation
   - streetAddress
   - state
   - country
   - physicalDeliveryOfficeName
   - city
   - postalCode
   - telephoneNumber
   - mobile
   - authenticationPhoneNumber
   - authenticationAlternativePhoneNumber
   - authenticationEmail
   - alternateEmailAddress
   - ageGroup
   - consentProvidedForMinor
   - legalAgeGroupClassification

## <a name="check-status"></a>檢查狀態

您可以在 [ **大量作業結果** ] 頁面中看到暫止大量要求的狀態。

[![檢查大量作業結果頁面中的狀態。](./media/users-bulk-download/bulk-center.png)](./media/users-bulk-download/bulk-center.png#lightbox)

## <a name="bulk-download-service-limits"></a>大量下載服務限制

每個建立使用者清單的大量活動最多可執行一小時。 這可讓您建立及下載至少500000個使用者的清單。

## <a name="next-steps"></a>後續步驟

- [[大量新增使用者](users-bulk-add.md)
- [大量刪除使用者](users-bulk-delete.md)
- [大量還原使用者](users-bulk-restore.md)
