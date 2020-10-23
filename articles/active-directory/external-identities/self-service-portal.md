---
title: B2B 共同作業的自助式註冊入口網站 - Azure AD
description: 瞭解如何自訂 Azure Active Directory B2B 使用者的上線工作流程，以符合您組織的需求。
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: f72c255ddac1889131234fefd7d6b014b453d417
ms.sourcegitcommit: 9b8425300745ffe8d9b7fbe3c04199550d30e003
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/23/2020
ms.locfileid: "92441720"
---
# <a name="self-service-for-azure-ad-b2b-collaboration-sign-up"></a>Azure AD B2B 共同作業註冊的自助服務

客戶可以使用透過 [Azure 入口網站](https://portal.azure.com)公開的內建功能進行許多工作，而終端使用者，則可使用透過[應用程式存取面板](https://myapps.microsoft.com)公開的內建功能。 不過，您可能需要自訂適用於 B2B 使用者的上線工作流程，以符合貴組織的需求。

## <a name="azure-ad-entitlement-management-for-b2b-guest-user-sign-up"></a>適用於 B2B 來賓使用者註冊的 Azure AD 權利管理

提出邀請的組織可能事先不知道需要存取其資源的外部共同作業者分別有誰。 您需要一個方法，讓合作夥伴公司的使用者透過您所控制的原則來自行註冊。 如果您想要讓其他組織的使用者要求存取權，並在核准時以來賓帳戶進行佈建並指派給群組、應用程式和 SharePoint Online 網站，您可以使用 [Azure AD 權利管理](../governance/entitlement-management-overview.md)來設定原則，以[管理外部使用者的存取權](../governance/entitlement-management-external-users.md#how-access-works-for-external-users)。

## <a name="azure-active-directory-b2b-invitation-api"></a>Azure Active Directory B2B 邀請 API

組織可以使用 [Microsoft Graph 邀請管理員 API](/graph/api/resources/invitation?view=graph-rest-1.0)，為 B2B 來賓使用者建立自己的上線體驗。 如果您想要提供自助式的 B2B 來賓使用者註冊，建議您使用 [Azure AD 權利管理](../governance/entitlement-management-overview.md)。 但是，如果您想要建立自己的體驗，則可以使用[建立邀請 API](/graph/api/invitation-post?tabs=http&view=graph-rest-1.0)，自動將自訂的邀請電子郵件直接傳送給 B2B 使用者等收件者。 或者，您的應用程式可以使用建立回應中所傳回的 inviteRedeemUrl，製作您自己的邀請 (透過您選擇的通訊機制) 給受邀使用者。

## <a name="next-steps"></a>後續步驟

* [何謂 Azure AD B2B 共同作業？](what-is-b2b.md)
* [Azure AD B2B 共同作業授權](licensing-guidance.md)
* [Azure Active Directory B2B 共同作業常見問題 (FAQ)](faq.md)