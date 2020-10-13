---
title: Azure AD Connect 雲端佈建常見問題集
description: 本文件說明雲端佈建的常見問題。
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 06/25/2020
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28fb9da04c9c9d3e98b5226e1aee4cf5dde7183b
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/09/2020
ms.locfileid: "91628667"
---
# <a name="azure-active-directory-connect-cloud-provisioning-faq"></a>Azure Active Directory Connect 雲端布建常見問題

閱讀 Azure Active Directory (Azure AD) Connect 雲端佈建的常見問題。

## <a name="general-installation"></a>一般安裝

**問：雲端佈建的執行頻率為何？**

雲端佈建排程為每 2 分鐘執行一次。 每隔 2 分鐘，就會將任何使用者、群組和密碼雜湊變更佈建到 Azure AD。

**問：第一次執行時發生密碼雜湊同步失敗的狀況。原因為何？**

這是預期行為。 失敗的原因是使用者物件不存在於 Azure AD 中。 使用者佈建到 Azure AD 後，應該會接著執行密碼雜湊的佈建。 請等候幾次執行，並確認密碼雜湊同步作業不再發生錯誤。

**問：如果 Active Directory 實例具有雲端布建 (不支援的屬性（例如目錄延伸) ），會發生什麼事？**

雲端佈建將會執行及佈建支援的屬性。 不支援的屬性將不會佈建到 Azure AD。 請參閱 Active Directory 中的目錄延伸，並確定您不需要將這些屬性傳送至 Azure AD。 如果需要一個或多個屬性，請考慮使用 Azure AD Connect 同步，或將所需的資訊移至其中一個支援的屬性 (例如，延伸模組屬性 1-15)。

**問：Azure AD Connect 同步與雲端佈建之間有何差異？**

使用 Azure AD Connect 同步時，會在內部部署同步伺服器上執行佈建。 組態會儲存在內部部署同步伺服器上。 使用 Azure AD Connect 雲端佈建時，佈建組態會儲存在雲端中，並在 Azure AD 佈建服務執行的過程中於雲端中執行。 

**問：我是否可使用雲端佈建從多個 Active Directory 樹系進行同步處理？**

是。 您可以使用雲端佈建從多個 Active Directory 樹系進行同步處理。 在多樹系環境中，所有參考 (例如管理員) 都必須位於網域內。  

**問：如何更新代理程式？**

Microsoft 會自動升級代理程式。 對於 IT 小組而言，這會減輕其必須測試和驗證新代理程式版本的負擔。 

**問：是否可以停用自動升級？**

目前不支援停用自動升級。

**問：是否可以變更雲端佈建的來源錨點？**

根據預設，雲端佈建會使用 ms-ds-consistency-GUID 作為來源錨點，並且可回復為 ObjectGUID。 目前不支援變更來源錨點。

**問：在使用雲端佈建時，我看到具有 AD 網域名稱的新服務主體。這是正常行為嗎？**

是，雲端佈建會使用網域名稱作為服務主體名稱，為佈建組態建立服務主體。 請勿對服務主體組態進行任何變更。

**問：如果已同步的使用者在下次登入時需要變更密碼，會發生什麼情況？**

如果已在雲端布建中啟用密碼雜湊同步處理，且已同步處理的使用者在內部部署 AD 中的下一次登入時需要變更密碼，則雲端布建不會將「變更」密碼雜湊布建至 Azure AD。 使用者變更密碼後，就會將使用者密碼雜湊從 AD 佈建到 Azure AD。

**問：雲端佈建是否支援任何物件的 ms-ds-consistencyGUID 回寫？**

否，雲端佈建不支援任何物件的 ms-ds-consistencyGUID 回寫 (包括使用者物件)。 

**問：我使用雲端佈建進行使用者的佈建。我已刪除組態。為何我仍在 Azure AD 中看到已同步的舊物件？** 

當您刪除設定時，雲端布建不會自動移除 Azure AD 中已同步處理的物件。 若要確保不會有舊物件，請將組態的範圍變更為空的群組或組織單位。 佈建執行並清除物件後，請停用並刪除組態。 

**問：Exchange 混合不受支援是什麼意思？**

Exchange 混合式部署功能可讓您同時存在內部部署和 Microsoft 365 中的 Exchange 信箱。 Azure AD Connect 會將一組特定的屬性從 Azure AD 同步處理回內部部署目錄。  雲端佈建代理程式目前不會將這些屬性同步處理回您的內部部署目錄，因此不支援使用該代理程式來取代 Azure AD Connect。

**問：我是否可在 Windows Server Core 上安裝雲端佈建代理程式？**

否，不支援在 Server Core 上安裝代理程式。

**問：是否可以搭配使用預備伺服器與雲端布建代理程式？**

否，不支援暫存伺服器。

**問：我可以同步處理來賓使用者帳戶嗎？**

否，不支援同步處理來賓使用者帳戶。

**問：如果我將使用者從雲端布建範圍內的 OU 移至範圍為 Azure AD Connect 的 OU，會發生什麼事？**

使用者將會被刪除並重新建立。  從雲端布建範圍的 OU 移動使用者，將會被視為刪除操作。  如果使用者移至受 Azure AD Connect 管理的 OU，則會重新布建至 Azure AD，並建立新的使用者。

**問：如果我重新命名或移動雲端布建篩選範圍內的 OU，在 Azure AD 中建立的使用者會發生什麼事？**

不做任何動作。  如果重新命名或移動 OU，則不會刪除使用者。

## <a name="next-steps"></a>後續步驟 

- [什麼是佈建？](what-is-provisioning.md)
- [什麼是 Azure AD Connect 雲端佈建？](what-is-cloud-provisioning.md)
