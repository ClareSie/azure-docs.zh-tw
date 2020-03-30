---
title: 如何與 Azure 活動目錄集成 |微軟文檔
description: 瞭解將應用程式與 Azure 活動目錄集成的好處，並獲取簡化登錄、標識管理、多重要素驗證和存取控制等功能的資源。
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: d13bba54-96bd-4b81-bee9-c8025ffa1648
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 04/08/2019
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, seoapril2019
ms.openlocfilehash: 5e4c6fb91b93b1035c68350a5f77a3c16db7323f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/28/2020
ms.locfileid: "78300107"
---
# <a name="integrating-with-azure-active-directory"></a>與 Azure Active Directory 整合

[!INCLUDE [active-directory-devguide](../../../includes/active-directory-devguide.md)]

在本文中，您將瞭解將應用程式與 Azure 活動目錄 （Azure AD） 集成的好處，並獲取用於集成的資源。 Azure AD 為組織提供雲應用程式的企業級標識管理。 Azure AD 整合提供使用者簡化的登入程序，並可協助您的應用程式符合 IT 原則。

## <a name="how-to-integrate"></a>如何整合

整合應用程式與 Azure AD 的方式有幾種。 充分利用適合您應用程式使用的這些案例。

### <a name="support-azure-ad-as-a-way-to-sign-in-to-your-application"></a>支援 Azure AD 作為登錄到應用程式的方法

**減少登入的不便，並降低支援成本。**  透過使用 Azure AD 來登入您的應用程式，您的使用者會少一個要記住的名稱和密碼。 身為開發人員，您會少一個要儲存及保護的密碼。 單單無需處理忘記密碼重設就可以省下大量時間與金錢。 Azure AD 支援部分世界上最受歡迎雲端應用程式 (包括 Office 365 和 Microsoft Azure) 的登入。 有來自數百萬個組織的數百萬名使用者，可能是您的使用者已經登入 Azure AD。 深入了解[新增 Azure AD 登入支援](authentication-scenarios.md)。

**簡化應用程式的註冊程序。**   註冊應用程式的過程中，Azure AD 會寄出使用者的基本資訊，讓您可以預先填入註冊表單，或者完全不需要用到。 使用者可以透過類似社交媒體和行動應用程式所使用的常見同意程序，使用其 Azure AD 帳戶來註冊應用程式。 任何使用者都可以註冊並登入已與 Azure AD 整合的應用程式，而無需 IT 人員的介入。 深入了解[註冊應用程式以進行 Azure AD 帳戶登入](../../app-service/configure-authentication-provider-aad.md)。

### <a name="browse-for-users-manage-user-provisioning-and-control-access-to-your-application"></a>流覽使用者、管理使用者預配和控制對應用程式的訪問

**瀏覽以尋找目錄中的使用者。**  使用 Microsoft 圖形 API 可説明使用者在邀請其他人或授予存取權限時搜索和流覽其組織中的其他人，而不是要求他們鍵入電子郵件地址。 使用者可以使用常見的通訊錄樣式介面進行瀏覽，包括檢視組織階層的詳細資料。 瞭解有關[微軟圖形 API](https://docs.microsoft.com/graph/overview)的更多資訊。

**重複使用您的客戶已經在管理的 Active Directory 群組與通訊群組清單。**   Azure AD 包含您的客戶已經用於電子郵件通訊群組和管理存取的群組。 使用 Microsoft 圖形 API，重用這些組，而不是要求客戶在應用程式中創建和管理一組單獨的組。 群組資訊也可以透過登入權杖傳送至您的應用程式。 瞭解有關[微軟圖形 API](https://docs.microsoft.com/graph/overview)的更多資訊。

**使用 Azure AD 來控制有權存取應用程式的人員。**   系統管理員和 Azure AD 中的應用程式擁有者可以將應用程式的存取權限指派給特定使用者和群組。 使用 Microsoft 圖形 API，可以讀取此清單，並用它來控制應用程式中資源和訪問的預配和取消預配。

**使用 Azure AD 進行角色型存取控制。**   系統管理員和應用程式擁有者可以將使用者和群組指派至您在 Azure AD 中註冊應用程式時所定義的角色。 角色資訊以登錄權杖發送到應用程式，也可以使用 Microsoft 圖形 API 讀取。 深入了解 [使用 Azure AD 進行授權](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles/)。

### <a name="get-access-to-users-profile-calendar-email-contacts-files-and-more"></a>訪問使用者的個人資料、日曆、電子郵件、連絡人、檔等

**Azure AD 是 Office 365 和其他 Microsoft 商務服務的授權伺服器。**   如果您的應用程式支援 Azure AD 登入，或支援使用 OAuth 2.0 將目前使用者帳戶連結至 Azure AD 使用者帳戶，則您可以要求使用者設定檔、行事曆、電子郵件、連絡人、檔案及其他資訊的讀取和寫入存取權限。 您可以順利地將事件寫入使用者的行事曆，並讀取檔案或將檔案寫入使用者的 OneDrive。 深入了解 [存取 Office 365 API](https://msdn.microsoft.com/office/office365/howto/platform-development-overview)。

### <a name="promote-your-application-in-the-azure-and-office-365-marketplaces"></a>在 Azure 和 Office 365 應用商店中推廣應用程式

**將您的應用程式擴廣到數百萬個已使用 Azure AD 的組織。**   搜尋和瀏覽 Marketplace 的使用者已經使用一或多項雲端服務，讓他們成為合格的雲端服務客戶。 深入了解如何在 [Azure Marketplace](https://azure.microsoft.com/marketplace/partner-program/)中推廣您的應用程式。

**當使用者註冊您的應用程式時，它便會出現在其 Azure AD 存取面板和 Office 365 應用程式啟動程式中。**   使用者將能夠迅速且輕鬆地返回您的應用程式，以增加使用者參與度。 深入了解 [Azure AD 存取面板](../user-help/active-directory-saas-access-panel-introduction.md)。

### <a name="secure-device-to-service-and-service-to-service-communication"></a>安全設備到服務和服務到服務通信

**使用 Azure AD 進行服務和裝置的身分識別管理，可減少您需要撰寫的程式碼，並可讓 IT 管理存取權限。**   服務與裝置可以使用 OAuth 取得 Azure AD 的權杖，並使用這些權杖來存取 Web API。 使用 Azure AD，您可以避免撰寫複雜的驗證程式碼。 因為服務和裝置的身分識別會儲存在 Azure AD 中，IT 可以在同一個地方管理金鑰和撤銷，而無需在您的應用程式中分別執行此動作。

## <a name="benefits-of-integration"></a>整合的優點

與 Azure AD 整合帶來您無需撰寫額外程式碼的優點。

### <a name="integration-with-enterprise-identity-management"></a>與企業標識管理集成

**協助您的應用程式符合 IT 原則。**   組織會將其企業身分識別管理系統與 Azure AD 整合，所以當某人離開組織時，它們會自動喪失對應用程式的存取權限，而且 IT 無需採取額外步驟。 IT 可以管理誰可以存取您的應用程式，並判斷需要哪些存取原則 (例如，多因素驗證)，以降低撰寫程式碼以符合複雜公司原則的需求。 Azure AD 提供系統管理員詳盡的稽核記錄，內容包括登入應用程式的使用者，以便 IT 可以追蹤使用情況。

**Azure AD 將 Active Directory 延伸到雲端，讓您的應用程式可以與 AD 整合。**   世界各地許多組織使用 Active Directory 作為其主要登入和身分識別管理系統，並要求他們的應用程式使用 AD。 與 Azure AD 整合即可將 Active Directory 和您的應用程式整合。

### <a name="advanced-security-features"></a>高級安全功能

**多重要素驗證。**   Azure AD 提供原生多因素驗證。 IT 系統管理員可以要求多因素驗證才能存取應用程式，因此您無需自行撰寫這項支援的程式碼。 瞭解有關[多重要素驗證](https://azure.microsoft.com/documentation/services/multi-factor-authentication/)的更多。

**登入偵測異常。**   Azure AD 每天處理超過 10 億個登入，同時使用機器學習演算法來偵測可疑活動，並向 IT 系統管理員通知可能發生的問題。 藉由支援 Azure AD 登入，您的應用程式便會獲得這項保護的好處。 深入了解[檢視 Azure Active Directory 存取報告](../active-directory-view-access-usage-reports.md)。

**條件訪問。**   除了多因素驗證，系統管理員還可以要求使用者在登入您的應用程式之前必須符合特定條件。 可以設定的條件包括用戶端裝置的 IP 位址範圍、指定群組的成員資格，以及用於存取的裝置狀態。 瞭解有關[Azure 活動目錄條件訪問](../active-directory-conditional-access-azure-portal.md)的更多內容。

### <a name="easy-development"></a>易於開發

**業界標準通訊協定。**   Microsoft 致力於支援業界標準。 Microsoft 標識平臺支援行業標準 OAuth 2.0 和 OpenID Connect 1.0 協定。 瞭解有關[微軟身份平臺身份驗證協定的更多資訊](active-directory-v2-protocols.md)。

**開放原始碼程式庫。**   Microsoft 針對常用的語言和平台提供完整支援的開放原始碼程式庫，進而加快開發速度。 原始碼採用 Apache 2.0 授權，您可以任何分岔以及貢獻回饋給專案。 瞭解有關[微軟身份驗證庫 （MSAL）](reference-v2-libraries.md)的更多。

### <a name="worldwide-presence-and-high-availability"></a>全球影響力和高可用性

**Azure AD 會在世界各地的資料中心內進行部署，並受到全天候的管理和監控。**   Azure AD 是適用於 Microsoft Azure 和 Office 365 的身分識別管理系統，並在世界各地的 28 個資料中心內進行部署。 保證將目錄資料複寫到至少三個資料中心。 全域負載平衡器會確保使用者可存取包含其資料的最接近 Azure AD 複本，並在偵測到問題時，自動將要求重新路由到其他資料中心。

## <a name="next-steps"></a>後續步驟

[開始撰寫程式碼](v2-overview.md#getting-started)

[使用 Microsoft 標識平臺登錄使用者](authentication-scenarios.md)

