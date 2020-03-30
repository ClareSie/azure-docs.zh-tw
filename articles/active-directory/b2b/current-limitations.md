---
title: B2B 協作的限制 - Azure 活動目錄 |微軟文檔
description: Azure Active Directory B2B 共同作業目前的限制
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 05/29/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: elisolMS
ms.collection: M365-identity-device-management
ms.openlocfilehash: ffee01488ecf658ce02a20a41252aca19288667c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "79263357"
---
# <a name="limitations-of-azure-ad-b2b-collaboration"></a>Azure AD B2B 共同作業的限制
Azure Active Directory (Azure AD) B2B 共同作業目前受限於本文所述的限制。

## <a name="possible-double-multi-factor-authentication"></a>可能的雙重多重要素驗證
使用 Azure AD B2B，您便可以在資源組織 (邀請組織) 強制執行多重要素驗證。 此方法的原因在[B2B 協作使用者的條件訪問中](conditional-access.md)詳細說明。 如果合作夥伴已設定並強制使用 Multi-Factor Authentication，其使用者可能必須在其所屬組織執行一次驗證，然後在您的組織中再次執行驗證。

## <a name="instant-on"></a>即時
在 B2B 共同作業流程中，我們會新增使用者到目錄中，並在邀請兌換與應用程式指派等期間動態更新它們。 更新與寫入通常會在一個目錄執行個體中進行，而且必須複寫到所有執行個體。 在所有執行個體都更新後，複寫作業便已完成。 有時，在某個執行個體中寫入或更新物件，但另一個執行個體呼叫擷取此物件時，就會發生複寫延遲問題。 如果發生該情況，請重新整理或重試來提供幫助。 如果您正在使用我們的 API 來撰寫應用程式，則採用某種讓步來重試是可減輕此問題的良好防禦性做法。

## <a name="azure-ad-directories"></a>Azure AD 目錄
Azure AD B2B 受限於 Azure AD 服務目錄限制。 如需使用者可建立的目錄數目以及使用者或來賓使用者可屬於的目錄數目詳細資訊，請參閱 [Azure AD 服務限制](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-service-limits-restrictions)。

## <a name="national-clouds"></a>國家/地區雲端
[國家雲](https://docs.microsoft.com/azure/active-directory/develop/authentication-national-cloud)是 Azure 的物理隔離實例。 不支援跨國家雲邊界進行 B2B 協作。 例如，如果您的 Azure 租戶位於公共全域雲中，則無法邀請其帳戶位於國家雲中的使用者。 要與使用者協作，請向他們詢問其他電子郵件地址，或在目錄中為他們創建成員使用者帳戶。

## <a name="azure-us-government-clouds"></a>Azure 美國政府雲
在 Azure 美國政府雲中，B2B 協作目前僅在 Azure 美國政府雲中同時支援並且都支援 B2B 協作的租戶之間提供支援。 如果邀請不屬於 Azure 美國政府雲或尚不支援 B2B 協作的租戶中的使用者，邀請將失敗，或者使用者將無法兌換邀請。 有關其他限制的詳細資訊，請參閱[Azure 活動目錄高級 P1 和 P2 變體](https://docs.microsoft.com/azure/azure-government/documentation-government-services-securityandidentity#azure-active-directory-premium-p1-and-p2)。

### <a name="how-can-i-tell-if-b2b-collaboration-is-available-in-my-azure-us-government-tenant"></a>如何判斷 Azure 美國政府租戶中是否有 B2B 協作？
要瞭解 Azure 美國政府雲租戶是否支援 B2B 協作，請執行以下操作：

1. 在瀏覽器中，轉到以下 URL，將租戶名稱改為*&lt;租戶名稱&gt;*：

   `https://login.microsoftonline.com/<tenantname>/v2.0/.well-known/openid-configuration`

2. 在`"tenant_region_scope"`JSON 回應中查找：

   - 如果`"tenant_region_scope":"USGOV”`出現，則支援 B2B。
   - 如果`"tenant_region_scope":"USG"`出現，則不支援 B2B。

## <a name="next-steps"></a>後續步驟

請參閱下列有關 Azure AD B2B 共同作業的文章：

- [何謂 Azure AD B2B 共同作業？](what-is-b2b.md)
- [委派 B2B 共同作業邀請](delegate-invitations.md)

