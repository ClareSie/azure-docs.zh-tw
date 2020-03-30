---
title: 存檔 Azure 活動目錄中的新增功能？ | Microsoft Docs
description: 此內容集的 [概觀] 區段所含的新增功能版本資訊包含 6 個月的活動。 6 個月後，這些項目就會從主要文章中移除，並放入此封存文章中。
services: active-directory
author: msmimart
manager: daveba
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: 15f5563c11e6abc5452ace01822e5559d04808df
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/27/2020
ms.locfileid: "80369652"
---
# <a name="archive-for-whats-new-in-azure-active-directory"></a>存檔 Azure 活動目錄中的新增功能？

主要內容在[Azure 活動目錄中新增？ 版本資訊](whats-new.md)文章包含過去六個月的更新，而本文包含所有較舊的資訊。

Azure 活動目錄中的新增功能是什麼？版本資訊提供有關：

- 最新版本
- 已知問題
- 錯誤修正
- 已被取代的功能
- 方案變更

---

## <a name="september-2019"></a>2019 年 9 月

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>更改計畫：棄用 Power BI 內容包

**類型：** 方案變更  
**服務類別：** 報告  
**產品功能：** 監視和報告

從 2019 年 10 月 1 日開始，Power BI 將開始棄用所有內容包，包括 Azure AD Power BI 內容包。 作為此內容包的替代方法，可以使用 Azure AD 活頁簿來深入瞭解 Azure AD 相關服務。 其他活頁簿即將推出，包括有關僅報告模式下的條件訪問策略的活頁簿、基於應用同意的見解等。

有關活頁簿的詳細資訊，請參閱[如何使用 Azure 活動目錄報表的 Azure 監視器活頁簿](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks)。 有關內容包棄用的詳細資訊，請參閱["宣佈電源 BI 範本應用通用性](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/)博客文章"。

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>我的個人資料正在重命名並與 Microsoft Office 帳戶頁集成

**類型：** 方案變更  
**服務類別：** 我的個人資料/帳戶  
**產品功能：** 共同作業

從 10 月開始，"我的個人資料"體驗將成為我的帳戶。 作為這一更改的一部分，當前顯示的任何地方，"**我的個人資料**將更改為**我的帳戶**"。 除了命名更改和一些設計改進外，更新後的體驗還將提供與 Microsoft Office 帳戶頁的額外集成。 具體來說，您將能夠從 **"概述帳戶"** 頁訪問 Office 安裝和訂閱，以及**隱私**頁面中與 Office 相關的連絡人首選項。

有關"我的個人資料（預覽）"體驗的詳細資訊，請參閱["我的個人資料（預覽）"門戶概述](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview)。

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>在 Azure AD 門戶（公共預覽）中使用 CSV 檔批量管理組和成員

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

我們很高興地在 Azure AD 門戶中宣佈批量組管理體驗的公開預覽可用性。 現在，您可以使用 CSV 檔和 Azure AD 門戶來管理組和成員清單，包括：

- 從組添加或刪除成員。

- 從目錄中下載組清單。

- 下載特定組的組成員清單。

有關詳細資訊，請參閱[大量新增成員](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members)、[大量刪除成員](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members)、[批量下載成員清單](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members)和[批量下載組清單](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download)。

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>動態同意現在通過新的管理員同意終結點支援

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

我們創建了一個新的管理員同意終結點來支援動態同意，這對想要在 Microsoft 標識平臺上使用動態同意模型的應用很有説明。

有關如何使用此新終結點的詳細資訊，請參閱[使用管理員同意終結點](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 9 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 9 月，我們在應用庫中添加了這 29 個具有聯合支援的新應用：

[計畫展望](https://schedulelook.bbsonlineservices.net/)， [MS Azure SSO 訪問 Ethidex 合規辦公室&trade;- 單一登入](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial)， [iServer 門戶](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial)， [SKYSITE，](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial) [Concur 旅行和費用](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial)，[工作板](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial)， `https://apps.yeeflow.com/`， [ARC 設施](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial)，[盧瓦斯特拉圖斯團隊](https://stratus.emea.luware.cloud/login)，[廣泛的想法](https://wideideas.online/wideideas/)，[棱鏡雲](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial)， [JDLT 用戶端中心](https://clients.jdlt.co.uk/login)， [RENRAKU，](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial)[密封路徑安全瀏覽器](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive)，[棱鏡雲](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial)， `https://app.penneo.com/` `https://app.testhtm.com/settings/email-integration` [Cintoo Cloud](https://aec.cintoo.com/login) [白源](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial)，[託管遺產線上 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial)， [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial)， [CakeHR](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial)， [BIS，](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial)[庫凱團隊建設](https://ms-contacts.coo-kai.jp/)，[聲納庫貝](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial)， [Adobe身份管理](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial)，[發現福利 SSO，](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial) [Amelio](https://app.amelio.co/)，`https://itask.yipinapp.com/`

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="new-azure-ad-global-reader-role"></a>新的 Azure AD 全域讀取器角色

**類型：** 新功能  
**服務類別：** Rbac  
**產品功能：** 存取控制

從 2019 年 9 月 24 日開始，我們將開始推出一個名為全域讀取器的新 Azure 活動目錄 （AD） 角色。 此次推出將從全球雲客戶和全球雲客戶 （GCC） 開始，將于 10 月在全球完成。

全域讀取器角色是全域管理員的唯讀對應角色。 此角色中的使用者可以跨 Microsoft 365 服務讀取設置和管理資訊，但不能執行管理操作。 我們創建了"全球讀者"角色，以説明減少組織中的全球管理員數量。 由於全域管理員帳戶功能強大且容易受到攻擊，因此我們建議您擁有少於 5 個全域管理員。 我們建議使用全域閱讀器角色進行規劃、審核或調查。 我們還建議將全域閱讀器角色與其他有限的管理員角色（如 Exchange Administrator）結合使用，以説明完成工作，而無需全域管理員角色。

全域閱讀器角色與新的 Microsoft 365 管理中心、交換管理中心、團隊管理中心、安全中心、合規性中心、Azure AD 管理中心和裝置管理管理中心配合使用。

>[!NOTE]
> 在公共預覽開始時，全域閱讀器角色將不起作用：SharePoint、特權訪問管理、客戶密碼箱、敏感度標籤、團隊生命週期、團隊報告&呼叫分析、團隊 IP 電話裝置管理和團隊應用目錄。 

有關詳細資訊，請參閱[Azure 活動目錄中的管理員角色許可權](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)。

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>使用 Azure 活動目錄應用程式代理從 Power BI 移動應用訪問本地報表服務器

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

Power BI 移動應用和 Azure AD 應用程式代理之間的新集成允許您安全地登錄到 Power BI 移動應用並查看組織託管在本地 Power BI 報表伺服器上的任何報告。

有關 Power BI 移動應用的資訊（包括下載應用的位置），請參閱 Power [BI 網站](https://powerbi.microsoft.com/mobile/)。 有關如何使用 Azure AD 應用程式代理設置 Power BI 移動應用的詳細資訊，請參閱[使用 Azure AD 應用程式代理啟用對 Power BI 移動的遠端存取](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi)。

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>AzureAD 預覽電源外殼模組的新版本可用

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 目錄

新 Cmdlet 已添加到 AzureADPreview 模組中，以説明定義和分配 Azure AD 中的自訂角色，包括：

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>新版本的 Azure AD 連接

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 目錄

我們發佈了適用于自動升級客戶的 Azure AD Connect 的更新版本。 此新版本包括幾個新功能、改進和錯誤修復。 有關此新版本的詳細資訊，請參閱 Azure [AD 連接：版本發佈歷史記錄](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250)。

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Azure 多重要素驗證 （MFA） 伺服器，版本 8.0.2 現已推出

**類型：** 固定  
**服務類別：** Mfa  
**產品功能：** 身份安全&保護

如果您是在 2019 年 7 月 1 日之前啟動 MFA 伺服器的現有客戶，現在可以下載最新版本的 MFA 伺服器（版本 8.0.2）。 在此新版本中，我們：

- 修復了問題，因此當 Azure AD 同步將使用者從"已禁用"更改為"已啟用"時，將通過電子郵件發送給該使用者。

- 修復了問題，以便客戶可以成功升級，同時繼續使用"標記"功能。

- 添加了科索沃 （+383） 國家代碼。

- 將一次性繞過稽核記錄記錄添加到多因數AuthSvc.log 中。

- 提高了 Web 服務 SDK 的性能。

- 修復了其他小錯誤。

從 2019 年 7 月 1 日起，Microsoft 停止為新部署提供 MFA 伺服器。 需要多重要素驗證的新客戶應使用基於雲的 Azure 多重要素驗證。 有關詳細資訊，請參閱[規劃基於雲的 Azure 多重要素驗證部署](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)。

---

## <a name="august-2019"></a>2019 年 8 月

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Azure AD 門戶（公共預覽版）中提供了增強的組搜索、篩選和排序功能

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

我們很高興地在 Azure AD 門戶中宣佈增強的組相關體驗的公開預覽可用性。 這些增強功能通過提供：

- 高級搜索功能，如組清單上的子字串搜索。
- 成員和擁有者清單上的高級篩選和排序選項。
- 成員和擁有者清單的新搜索功能。
- 大組的更準確的組計數。

有關詳細資訊，請參閱在[Azure 門戶中管理組](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)。

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>新的自訂角色可用於應用註冊管理（公共預覽）

**類型：** 新功能  
**服務類別：** Rbac  
**產品功能：** 存取控制

自訂角色（隨 Azure AD P1 或 P2 訂閱一起提供）現在可以通過允許您創建具有特定許可權的角色定義，然後將這些角色指派給特定資源，從而説明您提供細細微性訪問。 目前，您通過使用管理應用註冊的許可權，然後將角色指派給特定應用來創建自訂角色。 有關自訂角色的詳細資訊，請參閱[Azure 活動目錄（預覽）中的自訂管理員角色](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview)。

如果您需要支援的其他許可權或資源（您當前看不到），您可以將回饋發送到我們的[Azure 回饋網站](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032)，我們將將您的請求添加到更新路線圖中。

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>新的預配日誌可以説明您監視和排除應用預配部署（公共預覽）

**類型：** 新功能  
**服務類別：** 應用程式佈建  
**產品功能：** 身份生命週期管理

新的預配日誌可説明您監視和排除使用者和組預配部署。 這些新日誌檔包括有關以下資訊：

- 在[ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial)中成功創建哪些組
- 從亞馬遜 Web[服務 （AWS）](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws)導入的角色
- 哪些員工不是從[工作日](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial)導入的

有關詳細資訊，請參閱[Azure 活動目錄門戶中的預配報表（預覽）。](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs)

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>所有 Azure AD 管理員的新安全報告（常規可用性）

**類型：** 新功能  
**服務類別：** 身份保護  
**產品功能：** 身份安全&保護

預設情況下，所有 Azure AD 管理員將很快能夠在 Azure AD 中訪問現代安全報告。 在 9 月底之前，您將能夠使用現代安全報告頂部的橫幅返回到舊報告。

現代安全報告將提供舊版本的其他功能，包括：

- 高級篩選和排序
- 批量操作，例如消除使用者風險
- 確認已洩露或安全實體
- 風險狀態，涵蓋範圍：風險、已排除、修復和已確認已洩露
- 新的風險相關檢測（適用于 Azure AD 高級訂閱者）

有關詳細資訊，請參閱[風險使用者](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users)、[風險登錄](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins)和[風險檢測](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risk-detections)。

---

### <a name="user-assigned-managed-identity-is-available-for-virtual-machines-and-virtual-machine-scale-sets-general-availability"></a>使用者分配的託管標識可用於虛擬機器和虛擬機器規模集（通用版本）

**類型：** 新功能  
**服務類別：** Azure 資源的託管標識  
**產品功能：** 開發人員體驗

使用者分配的託管標識現在通常可用於虛擬機器和虛擬機器縮放集。 作為其中的一部分，Azure 可以在 Azure AD 租戶中創建受正在使用的訂閱信任的標識，並可以分配給一個或多個 Azure 服務實例。 有關使用者分配的託管標識的詳細資訊，請參閱[Azure 資源的託管標識是什麼？](https://aka.ms/azuremanagedidentity)

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>使用者可以使用移動應用或硬體權杖重置其密碼（通用版）

**類型：** 已變更的功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 使用者驗證

已向組織註冊移動應用的使用者現在可以通過批准來自 Microsoft 身份驗證器應用的通知或從移動應用或硬體權杖輸入代碼來重置自己的密碼。

有關詳細資訊，請參閱[其工作原理：Azure AD 自助服務密碼重設](https://aka.ms/authappsspr)。 有關使用者體驗的詳細資訊，請參閱[重置自己的工作或學校密碼概述](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview)。

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET忽略代表方案的MSAL.NET共用緩存

**類型：** 固定  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

從 Azure AD 身份驗證庫 （ADAL.NET） 版本 5.0.0 預覽開始，應用開發人員必須[為每個帳戶序列化 Web 應用和 Web API 的一個緩存](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api)。 否則，使用[代流](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow)的某些方案以及 某些特定的用例`UserAssertion`可能會導致特權的提升。 為了避免此漏洞，ADAL.NET現在忽略代表方案的 dotnet（MSAL.NET） 共用緩存的 Microsoft 身份驗證庫。

有關此問題的詳細資訊，請參閱 Azure[活動目錄身份驗證庫提升許可權漏洞](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 8 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 8 月，我們在應用庫中添加了這 26 個具有聯合支援的新應用：

[公民平臺](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial)，[亞馬遜業務](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial)， [ProNovos 運營經理](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial)， [Cognidox](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial)， [Viareport的Inativ門戶 （歐洲）](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial)， [Azure Databricks，](https://azure.microsoft.com/services/databricks)[羅賓](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial)，[學院出勤，](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial)[優先矩陣](https://sync.appfluence.com/pmwebng/)，[庫斯特 MySpace，](https://cousto.platformers.be/account/login)[上傳護理](https://uploadcare.com/accounts/signup/)，[碳化物端點備份](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial)， [CPQSync由 Cincom，](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial)[收費比](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial)，[交付.&trade;媒體門戶](https://portal.deliver.media)，[前線教育](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial)， [F5，](https://www.f5.com/products/security/access-policy-manager) [藏匿點AD連接](https://www.stashcat.com)，[閃爍](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial)，[沃科利](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial)，[普羅諾沃斯分析](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial)，[西格斯特](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial)，[達爾文盒](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial)，[按顏色觀看](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial)，[線束](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial)， [EAB 導航戰略護理](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>AzureAD 電源外殼和 AzureAD 預覽電源外殼模組的新版本可用

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 目錄

AzureAD 和 AzureAD 預覽電源外殼模組的新更新可用：

- AzureAD`-Filter`模組中的`Get-AzureADDirectoryRole`參數中添加了一個新參數。 此參數可説明您篩選 Cmdlet 返回的目錄角色。
- 新 Cmdlet 已添加到 AzureADPreview 模組中，以説明定義和分配 Azure AD 中的自訂角色，包括：

    - `Get-AzureADMSRoleAssignment`
    - `Get-AzureADMSRoleDefinition`
    - `New-AzureADMSRoleAssignment`
    - `New-AzureADMSRoleDefinition`
    - `Remove-AzureADMSRoleAssignment`
    - `Remove-AzureADMSRoleDefinition`
    - `Set-AzureADMSRoleDefinition`

---

### <a name="improvements-to-the-ui-of-the-dynamic-group-rule-builder-in-the-azure-portal"></a>Azure 門戶中動態組規則產生器的 UI 改進

**類型：** 已變更的功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

我們對 Azure 門戶中提供的動態組規則產生器進行了一些 UI 改進，以説明您更輕鬆地設置新規則或更改現有規則。 此設計改進允許您創建最多包含五個運算式的規則，而不僅僅是一個運算式。 我們還更新了裝置屬性清單以刪除已棄用的裝置屬性。

有關詳細資訊，請參閱[管理動態成員資格規則](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)。

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>新的 Microsoft 圖形應用許可權可用於訪問審核

**類型：** 已變更的功能  
**服務類別：** 訪問評論  
**產品功能：** 身份治理

我們引入了新的 Microsoft Graph 應用許可權，`AccessReview.ReadWrite.Membership`它允許應用自動創建和檢索組成員身份和應用分配的訪問審核。 此許可權可以由計畫作業使用，也可以作為自動化的一部分使用，而無需登錄使用者上下文。

有關詳細資訊，請參閱[如何使用使用 PowerShell 博客使用 Microsoft 圖形應用許可權創建 Azure AD 訪問評論的示例](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241)。

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Azure AD 活動日誌現在可用於 Azure 監視器中的政府雲實例

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們很高興地宣佈 Azure AD 活動日誌現在可用於 Azure 監視器中的政府雲實例。 現在，您可以將 Azure AD 日誌發送到存儲帳戶或事件中心，以便與 SIEM 工具（如[相撲](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic)[、Splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk)和[ArcSight）](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight)集成。 

有關設置 Azure 監視器的詳細資訊，請參閱[Azure 監視器 中的 Azure AD 活動日誌](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations)。

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>將使用者更新為新的增強的安全資訊體驗

**類型：** 已變更的功能  
**服務類別：** 身份驗證（登錄）   
**產品功能：** 使用者驗證

2019 年 9 月 25 日，我們將關閉舊的非增強型安全資訊體驗，用於註冊和管理使用者安全資訊，並僅打開新的[增強版本](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)。 這意味著您的使用者將不再能夠使用舊體驗。

有關增強的安全資訊體驗的詳細資訊，請參閱我們的[管理文檔](https://aka.ms/securityinfodocs)和[使用者文檔](https://aka.ms/securityinfoguide)。

#### <a name="to-turn-on-this-new-experience-you-must"></a>要開啟此新體驗，您必須：

1. 以全域管理員或使用者管理員身份登錄到 Azure 門戶。

2. 轉到**Azure 活動目錄>使用者設置>管理訪問面板預覽功能的設置**。

3. 在"**使用者可以使用預覽功能註冊和管理安全資訊增強**區域"中，選擇 **"選定**"，然後選擇一組使用者，或選擇 **"全部"** 為租戶中的所有使用者打開此功能。

4. 在 [使用者可以使用預覽功能註冊和管理安全 [資訊] 區域，選擇 **"無**"。

5. 儲存您的設定。

    保存設置後，您將無法再訪問舊的安全資訊體驗。

>[!Important]
>如果在 2019 年 9 月 25 日之前未完成這些步驟，則 Azure 活動目錄租戶將自動啟用以進行增強體驗。 如果您有任何疑問，請于 聯繫我們registrationpreview@microsoft.com。

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>使用 POST 登錄的身份驗證請求將受到更嚴格的驗證

**類型：** 已變更的功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 標準

從 2019 年 9 月 2 日開始，使用 POST 方法的身份驗證請求將針對 HTTP 標準進行更嚴格的驗證。 具體而言，空格和雙引號 （"） 將不再從請求表單值中刪除。 這些更改不會破壞任何現有用戶端，並且有助於確保每次可靠地處理髮送到 Azure AD 的請求。

有關詳細資訊，請參閱 Azure [AD 中斷更改通知](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored)。

---

## <a name="july-2019"></a>2019 年 7 月

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>更改計畫：應用程式代理服務更新僅支援 TLS 1.2

**類型：** 方案變更  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

為了説明為您提供我們最強的加密，我們將開始將應用程式代理服務訪問限制為僅 TLS 1.2 協定。 此限制最初將推廣到已經使用 TLS 1.2 協定的客戶，因此您不會看到影響。 TLS 1.0 和 TLS 1.1 協定的完整棄用將于 2019 年 8 月 31 日完成。 仍在使用 TLS 1.0 和 TLS 1.1 的客戶將收到提前通知，以準備此更改。

要在整個更改過程中保持與應用程式代理服務的連接，我們建議您確保用戶端-伺服器和瀏覽器-伺服器組合更新為使用 TLS 1.2。 我們還建議您確保包含員工用於訪問通過應用程式代理服務發佈的應用的任何用戶端系統。

有關詳細資訊，請參閱在[Azure 活動目錄中添加用於遠端存取的應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application)。

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>更改計畫：應用程式庫即將進行設計更新

**類型：** 方案變更  
**服務類別：** 企業應用  
**產品功能：** SSO

新的使用者介面更改即將從 **"添加應用程式**"邊欄選項卡**的庫區域進行"添加**"的設計。 這些更改將説明您更輕鬆地查找支援自動預配、OpenID 連接、安全斷言標記語言 （SAML） 和密碼單一登入 （SSO） 的應用。

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>更改計畫：從 Office 365 IP 位址中刪除 MFA 伺服器 IP 位址

**類型：** 方案變更  
**服務類別：** Mfa  
**產品功能：** 身份安全&保護

我們將從[Office 365 IP 位址和 URL Web 服務](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service)中刪除 MFA 伺服器 IP 位址。 如果當前依賴這些頁面來更新防火牆設置，則必須確保還包括[Azure](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements) **多重要素驗證伺服器防火牆要求**部分中記錄的 IP 位址清單。

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>僅限應用的權杖現在要求用戶端應用存在於資源租戶中

**類型：** 固定  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

2019 年 7 月 26 日，我們改變了通過[用戶端憑據授予](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)提供僅應用權杖的方式。 以前，應用可以獲取權杖來調用其他應用，而不管用戶端應用是否位於租戶中。 我們更新了此行為，因此單個租戶資源（有時稱為 Web API）只能由資源租戶中存在的用戶端應用調用。

如果應用不在資源租戶中，則會收到一條錯誤訊息，`The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.`指出"要解決此問題，必須在租戶中創建用戶端應用服務主體，使用[管理員同意終結點](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint)或通過[PowerShell](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell)，這可確保租戶已授予應用在租戶內操作的許可權。

有關詳細資訊，請參閱[身份驗證的新增功能？](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant)

> [!NOTE]
> 用戶端和 API 之間的現有同意仍然不需要。 應用仍應執行自己的授權檢查。

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>使用 FIDO2 安全金鑰組 Azure AD 進行新的無密碼登錄

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

Azure AD 客戶現在可以為其組織的使用者和組設置策略來管理 FIDO2 安全金鑰。 最終使用者還可以自行註冊其安全金鑰，在支援 FIDO 的設備上使用金鑰登錄到其 Microsoft 帳戶，以及登錄到其 Azure AD 加入的 Windows 10 設備。

有關詳細資訊，請參閱為[Azure AD 啟用無密碼登錄（預覽）](/azure/active-directory/authentication/concept-authentication-passwordless)以獲取與管理員相關的資訊，以及[設置安全資訊以使用安全金鑰（預覽）](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key)以獲取最終使用者相關資訊。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 7 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 7 月，我們在應用庫中添加了這 18 個具有聯合支援的新應用：

[Ungerboeck 軟體](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial)，[明亮的模式全管道聯繫中心](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial)，[聰明的內利](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial)，[收購IO，](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial) [Looop](https://www.looop.co/schedule-a-demo/)，[產品板](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial)， [MS Azure SSO 訪問 Ethidex 合規辦公室&trade;](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso)，[海普](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial)，[抽象](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial)，[阿森提斯](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial)，[翻轉小吃](https://www.flipsnack.com/accounts/sign-in-sso.html)，[旺達](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial)， [TwineSocial，](https://twinesocial.com/) [Kallidus，](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial)[海蘭，](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial)[芬尼廢物證人](https://www.pharmid.com/)， [i2B 連接](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/)， [JFrog 文物](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>為這些新支援的 SaaS 應用自動設定使用者帳戶

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 監視和報告

現在，您可以自動創建、更新和刪除這些新集成應用的使用者帳戶：

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

有關如何使用自動使用者帳戶預配更好地保護組織的詳細資訊，請參閱[使用 Azure AD 自動將使用者預配到 SaaS 應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>網路安全性群組的新 Azure AD 域服務標記

**類型：** 新功能  
**服務類別：** Azure AD Domain Services  
**產品功能：** Azure AD Domain Services

如果您厭倦了管理 IP 位址和範圍的長清單，則可以使用 Azure 網路安全性群組中的新**AzureActiveDirectoryDomainServices**網路服務標記來説明保護到 Azure AD 域服務虛擬網路子網的入站流量。

有關此新服務標記的詳細資訊，請參閱[Azure AD 域服務的網路安全性群組](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports)。

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD 域服務的新安全審核（公共預覽）

**類型：** 新功能  
**服務類別：** Azure AD Domain Services  
**產品功能：** Azure AD Domain Services

我們很高興地宣佈發佈 Azure AD 域服務安全審核以進行公共預覽。 安全審核通過使用 Azure AD 域服務將安全審核事件資料流到目標資源（包括 Azure 存儲、Azure 日誌分析工作區和 Azure 事件中心），説明您深入瞭解身份驗證服務門戶。

有關詳細資訊，請參閱為[Azure AD 域服務啟用安全審核（預覽）。](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events)

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>新的身份驗證方法使用&見解（公共預覽）

**類型：** 新功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 監視和報告

新的身份驗證方法使用&見解報告可以説明您瞭解如何註冊和使用 Azure 多重要素驗證和自助服務密碼重設等功能，包括每個功能的註冊使用者數、使用自助服務密碼重設重置密碼的頻率以及重置方法。

有關詳細資訊，請參閱[身份驗證方法使用方式&見解（預覽）](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights)。

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>新的安全報告適用于所有 Azure AD 管理員（公共預覽版）

**類型：** 新功能  
**服務類別：** 身份保護  
**產品功能：** 身份安全&保護

所有 Azure AD 管理員現在都可以選擇現有安全報告頂部的橫幅（如**標記為風險報告的使用者**）開始使用新的安全體驗，如**風險使用者**和**風險登錄**報告所示。 隨著時間的推移，所有安全報告都將從舊版本移動到新版本，新報告將為您提供以下附加功能：

- 高級篩選和排序

- 批量操作，例如消除使用者風險

- 確認已洩露或安全實體

- 風險狀態，涵蓋範圍：風險、已排除、修復和已確認已洩露

有關詳細資訊，請參閱[風險使用者報告](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users)和[風險登錄報告](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins)。

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Azure AD 域服務的新安全審核（公共預覽）

**類型：** 新功能  
**服務類別：** Azure AD Domain Services  
**產品功能：** Azure AD Domain Services

我們很高興地宣佈發佈 Azure AD 域服務安全審核以進行公共預覽。 安全審核通過使用 Azure AD 域服務將安全審核事件資料流到目標資源（包括 Azure 存儲、Azure 日誌分析工作區和 Azure 事件中心），説明您深入瞭解身份驗證服務門戶。

有關詳細資訊，請參閱為[Azure AD 域服務啟用安全審核（預覽）。](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events)

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>使用 SAML/WS-Fed 的新 B2B 直接聯合（公共預覽版）

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

直接聯合通過使用支援 SAML 或 WS-Fed 標準的標識系統，説明您更輕鬆地與 IT 託管標識解決方案不是 Azure AD 的合作夥伴合作。 與合作夥伴建立直接聯合關係後，您從該域邀請的任何新來賓使用者都可以使用其現有組織帳戶與您協作，從而使來賓的使用者體驗更加無縫。

有關詳細資訊，請參閱與[AD FS 和協力廠商供應商的直接聯合，以便提供來賓使用者（預覽）。](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>為這些新支援的 SaaS 應用自動設定使用者帳戶

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 監視和報告

現在，您可以自動創建、更新和刪除這些新集成應用的使用者帳戶：

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

有關如何使用自動使用者帳戶預配更好地保護組織的詳細資訊，請參閱[使用 Azure AD 自動向 SaaS 應用程式預配使用者](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>在 Azure AD 門戶中檢查重複組名稱的新檢查

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

現在，當您從 Azure AD 門戶創建或更新組名稱時，我們將執行檢查以查看是否複製了資源中的現有組名稱。 如果我們確定該名稱已被其他組使用，系統將要求您修改您的姓名。

有關詳細資訊，請參閱在[Azure AD 門戶中管理組](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context)。

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD 現在支援答覆（重定向）URI 中的靜態查詢參數

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

Azure AD 應用現在可以註冊和使用具有靜態查詢參數（例如`https://contoso.com/oauth2?idp=microsoft`）的回應（重定向）URI的 OAuth 2.0 請求。 靜態查詢參數受答覆 URI 的字串匹配，就像答覆 URI 的任何其他部分一樣。 如果沒有與 URL 解碼重定向 uri 匹配的註冊字串，則請求將被拒絕。 如果找到答覆 URI，則整個字串用於重定向使用者，包括靜態查詢參數。

動態答覆 URI 仍然被禁止，因為它們表示安全風險，不能用於跨身份驗證請求保留狀態資訊。 為此，請使用 參數`state`。

目前，Azure 門戶的應用註冊螢幕仍阻止查詢參數。 但是，您可以手動編輯應用清單以在應用中添加和測試查詢參數。 有關詳細資訊，請參閱[身份驗證的新增功能？](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters)

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Azure AD 的活動日誌 （MS 圖形 API） 現在可通過 PowerShell Cmdlet 獲得

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們很高興地宣佈，Azure AD 活動日誌（審核和登錄報告）現在可通過 Azure AD PowerShell 模組獲得。 以前，您可以使用 MS Graph API 終結點創建自己的腳本，現在我們將此功能擴展到 PowerShell Cmdlet。

有關如何使用這些 Cmdlet 的詳細資訊，請參閱 Azure [AD PowerShell Cmdlet 以進行報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting)。

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Azure AD 中審核和登錄日誌的更新篩選器控制項

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們更新了審核和登錄日誌報表，因此您現在可以應用各種篩選器，而無需將它們作為列添加到報表螢幕上。 此外，您現在可以決定要在螢幕上顯示的篩選器數。 這些更新協同工作，使報表更易於閱讀，並更廣泛地滿足您的需求。

有關這些更新的詳細資訊，請參閱[篩選稽核記錄](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs)和[篩選器登錄活動](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities)。

---

## <a name="june-2019"></a>2019 年 6 月

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>新的風險檢測 API 用於微軟圖形（公共預覽版）

**類型：** 新功能  
**服務類別：** 身份保護  
**產品功能：** 身份安全&保護

我們很高興地宣佈，微軟圖形的新風險檢測API現已公開預覽。 您可以使用此新 API 查看組織的標識保護相關使用者和登錄風險檢測的清單。 您還可以使用此 API 更有效地查詢風險檢測，包括有關檢測類型、狀態、級別等的詳細資訊。

有關詳細資訊，請參閱[風險檢測 API 參考文檔](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 6 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 6 月，我們在應用庫中添加了這 22 個具有聯合支援的新應用：

[Azure AD SAML 工具組](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial)， [Otsuka Shokai （*）](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial)， [ANAQUA，](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial) [Azure VPN 用戶端](https://portal.azure.com/)，[費用In，](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial)[説明器説明器](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial)，[成本點](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial)， [GlobalOne，](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial)[梅賽德斯-賓士車載辦公室](https://me.secure.mercedes-benz.com/)，[斯科雷](https://app.justskore.it/)，[甲骨文雲基礎設施主控台](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial)， [CyberArk SAML 認證](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial)， [Scrible Edu，](https://www.scrible.com/sign-in/#/create-account) [PandaDoc，](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial)[感知，](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial)[普裡斯特作業系統](https://proptimise.co.uk/software/)， [Vtiger CRM （SAML）](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial)零售銷售，甲骨文電子商務套件的甲骨文訪問經理，甲骨文IDCS電子商務套件，甲骨文IDCS為人軟，甲骨文IDCS為JD愛德華茲

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>為這些新支援的 SaaS 應用自動設定使用者帳戶

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 監視和報告

現在，您可以自動創建、更新和刪除這些新集成應用的使用者帳戶：

- [縮放](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Envoy](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

有關如何使用自動使用者帳戶預配更好地保護組織的詳細資訊，請參閱[使用 Azure AD 自動將使用者預配到 SaaS 應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>查看 Azure AD 預配服務的即時進度

**類型：** 已變更的功能  
**服務類別：** 應用程式佈建  
**產品功能：** 身份生命週期管理

我們更新了 Azure AD 預配體驗，以包括一個新的進度列，用於顯示您在使用者預配過程中有多遠。 此更新體驗還提供有關當前週期內預配的使用者數以及迄今為止預配的使用者數的資訊。

有關詳細資訊，請參閱[檢查使用者預配的狀態](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user)。

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>公司品牌現在出現在登出和錯誤螢幕上

**類型：** 已變更的功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

我們更新了 Azure AD，以便您的公司品牌現在顯示在登出和錯誤螢幕以及登錄頁上。 無需執行任何操作即可打開此功能，Azure AD 只需使用已在 Azure 門戶的 **"公司"品牌**區域中設置的資產。

有關設置公司品牌的詳細資訊，請參閱[向組織的 Azure 活動目錄頁面添加品牌](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding)。

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>Azure 多重要素驗證 （MFA） 伺服器不再可用於新部署

**類型：** 已被取代  
**服務類別：** Mfa  
**產品功能：** 身份安全&保護

自 2019 年 7 月 1 日起，Microsoft 將不再為新部署提供 MFA 伺服器。 希望在組織中需要多重要素驗證的新客戶現在必須使用基於雲的 Azure 多重要素驗證。 在 7 月 1 日之前啟動 MFA 伺服器的客戶不會看到更改。 您仍然可以下載最新版本、獲取將來的更新並生成啟動憑據。

有關詳細資訊，請參閱使用[Azure 多重要素驗證伺服器入門](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy)。 有關基於雲的 Azure 多重要素驗證的詳細資訊，請參閱[規劃基於雲的 Azure 多重要素驗證部署](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted)。

---

## <a name="may-2019"></a>2019 年 5 月

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>服務更改：將來僅支援應用程式代理服務上的 TLS 1.2 協定

**類型：** 方案變更  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

為了説明為客戶提供一流的加密，我們將僅訪問應用程式代理服務上的 TLS 1.2 協定。 此更改正在逐步推廣到已經只使用 TLS 1.2 協定的客戶，因此您不應看到任何更改。

TLS 1.0 和 TLS 1.1 的棄用發生在 2019 年 8 月 31 日，但我們將提供其他高級通知，因此您有時間準備此更改。 為準備此更改，請確保更新用戶端-伺服器和瀏覽器-伺服器組合（包括使用者用於訪問通過應用程式代理髮布的應用的任何用戶端）以使用 TLS 1.2 協定來維護與應用程式的連接代理服務。 有關詳細資訊，請參閱在[Azure 活動目錄中添加用於遠端存取的應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin)。

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>使用使用方式和見解報告查看與應用相關的登錄資料

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 監視和報告

現在，您可以使用位於 Azure 門戶**的企業應用程式**區域中的使用方式和見解報告來獲取登錄資料以應用程式為中心的視圖，包括有關以下資訊：

- 為您的組織使用的頂級應用

- 登錄失敗最多的應用

- 每個應用的頂級登錄錯誤

有關此功能的詳細資訊，請參閱 Azure[活動目錄門戶中的使用方式和見解報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report)

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>使用 Azure AD 自動將使用者預配到雲應用

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 監視和報告

請按照這些新教程使用 Azure AD 預配服務自動創建、刪除和更新以下基於雲的應用的使用者帳戶：

- [科米特](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [動態信號](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [保管員安全](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

您還可以按照這個新的[Dropbox 教程](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial)，提供有關如何預配組物件的資訊。

有關如何通過自動使用者帳戶預配更好地保護組織的詳細資訊，請參閱[使用 Azure AD 自動向 SaaS 應用程式預配使用者](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)。

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>標識安全分數現在可在 Azure AD 中提供（常規版本）

**類型：** 新功能  
**服務類別：** N/A  
**產品功能：** 身份安全&保護

現在，您可以使用 Azure AD 中的標識安全評分功能來監視和改進身份安全狀態。 標識安全評分功能使用單個儀表板來説明您：

- 根據介於 1 和 223 之間的分數客觀地衡量您的身份安全姿勢。

- 規劃身份安全改進

- 查看安全改進的成功

有關標識安全分數功能的詳細資訊，請參閱 Azure[活動目錄中的標識安全分數是什麼？](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>新的應用註冊體驗現已推出（一般版本）

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 開發人員體驗

新的[應用註冊](https://aka.ms/appregistrations)體驗現已處於通用性。 此新體驗包括 Azure 門戶和應用程式註冊門戶中熟悉的所有主要功能，並通過以下功能對其進行改進：

- **更好的應用管理。** 您現在可以在一個位置查看所有應用，而不是跨不同門戶查看應用。

- **簡化應用註冊。** 從改進的導航體驗到改進的許可權選擇體驗，現在可以更輕鬆地註冊和管理應用。

- **更多詳細資訊。** 您可以找到有關應用的更多詳細資訊，包括快速入門手冊等。

有關詳細資訊，請參閱[Microsoft 標識平臺](https://docs.microsoft.com/azure/active-directory/develop/)，[應用程式註冊體驗現已普遍可用！](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) 博客公告。

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>風險使用者 API 中用於標識保護的新功能

**類型：** 新功能  
**服務類別：** 身份保護  
**產品功能：** 身份安全&保護

我們很高興地宣佈，您現在可以使用風險使用者 API 檢索使用者的風險歷史記錄，解雇風險使用者，並確認使用者已受損。 此更改可説明您更有效地更新使用者的風險狀態，並瞭解其風險歷史記錄。

有關詳細資訊，請參閱[風險使用者 API 參考文檔](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 5 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 5 月，我們在應用庫中添加了這 21 個具有聯合支援的新應用：

[弗裡德坎普](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial)，[真正的連結](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial)，[基安達](https://app.kianda.com/sso/OpenID/AzureAD/)，[簡單標誌](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial)，[布拉茲](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial)，[顯示器](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial)，[坦普拉菲](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial)，[市場銷售參與](https://toutapp.com/login)， [ACLP，](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial) [OutSystems，](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial) [Meta4 全球人力資源](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial)，[量子工作場所](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial)，[鈷](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial)，[網路方法 API 雲](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial)，[紅旗](https://pocketstop.com/redflag/)，[什麼修復](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial)，[控制](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial)， [JOBHUB，](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial) [NEOGOV，](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial)[食品，](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial) [MyVR](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>改進 Azure AD 門戶中的組創建和管理體驗

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

我們在 Azure AD 門戶中改進了與組相關的體驗。 這些改進使管理員能夠更好地管理組清單、成員清單並提供其他創建選項。

改善項目包括：

- 按成員身份類型和組類型進行基本篩選。

- 添加新列，如"源"和"電子郵件地址"。

- 能夠多選擇組、成員和擁有者清單，以便輕鬆刪除。

- 能夠在組創建期間選擇電子郵件地址並添加擁有者。

如需詳細資訊，請參閱[使用 Azure Active Directory 建立基本群組並新增成員](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal)。

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>在 Azure AD 門戶中為 Office 365 組配置命名策略（通用版）

**類型：** 已變更的功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

管理員現在可以使用 Azure AD 門戶為 Office 365 組配置命名策略。 此更改有助於為組織中的使用者創建或編輯的 Office 365 組強制實施一致的命名約定。

您可以通過兩種不同的方式為 Office 365 組配置命名策略：

- 定義自動添加到組名稱的首碼或尾碼。

- 為您的組織上傳一組自訂的阻止字詞，這些單詞不允許在組名稱中使用（例如，"CEO、工資單、HR"）。

有關詳細資訊，請參閱為[Office 365 組強制實施命名策略](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy)。

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Microsoft 圖形 API 終結點現在可用於 Azure AD 活動日誌（常規可用性）

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們很高興地宣佈 Microsoft 圖形 API 終結點對 Azure AD 活動日誌的支援。 使用此版本，現在可以使用 Azure AD 稽核記錄的版本 1.0 以及登錄日誌 API。

有關詳細資訊，請參閱[Azure AD 稽核記錄 API 概述](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0)。

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>管理員現在可以將條件訪問用於合併註冊過程（公共預覽）

**類型：** 新功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全&保護  

管理員現在可以創建條件訪問策略，供組合註冊頁使用。 這包括在以下方面應用策略以允許註冊：

- 使用者位於受信任的網路上。

- 使用者登錄風險較低。

- 使用者位於託管設備上。

- 使用者同意組織的使用條款 （TOU）。

有關條件訪問和密碼重設的詳細資訊，請參閱 Azure AD[組合 MFA 和密碼重設註冊體驗博客文章的條件訪問](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348)。 有關合併註冊過程的條件訪問策略的詳細資訊，請參閱[合併註冊的條件訪問策略](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration)。 有關 Azure AD 使用條件功能的詳細資訊，請參閱[Azure 活動目錄使用條款功能](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。

---

## <a name="april-2019"></a>2019 年 4 月

### <a name="new-azure-ad-threat-intelligence-detection-is-now-available-as-part-of-azure-ad-identity-protection"></a>新的 Azure AD 威脅智慧檢測現在作為 Azure AD 標識保護的一部分提供

**類型：** 新功能  
**服務類別：** Azure AD 標識保護  
**產品功能：** 身份安全&保護

Azure AD 威脅智慧檢測現在可作為更新的 Azure AD 標識保護功能的一部分提供。 此新功能有助於指示特定使用者或活動基於 Microsoft 內部和外部威脅情報源的已知攻擊模式的異常使用者活動。

有關刷新版本的 Azure AD 標識保護的詳細資訊，請參閱[四個主要 Azure AD 標識保護增強功能現在位於公共預覽](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Four-major-Azure-AD-Identity-Protection-enhancements-are-now-in/ba-p/326935)博客中[，什麼是 Azure 活動目錄標識保護（刷新）？](https://docs.microsoft.com/azure/active-directory/identity-protection/overview-v2) 。 有關 Azure AD 威脅智慧檢測的詳細資訊，請參閱[Azure 活動目錄標識保護風險檢測](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks)一文。

---

### <a name="azure-ad-entitlement-management-is-now-available-public-preview"></a>Azure AD 授權管理現已可用（公共預覽版）

**類型：** 新功能  
**服務類別：** 身份治理  
**產品功能：** 身份治理

Azure AD 授權管理（現在處於公共預覽版中）可説明客戶委派訪問包的管理，該管理定義員工和業務合作夥伴如何請求訪問、必須批准的人員以及他們具有存取權限的時間。 訪問包可以管理 Azure AD 和 Office 365 組的成員身份、企業應用程式中的角色指派以及 SharePoint Online 網站的角色指派。 在[Azure AD 授權管理概述](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview)中瞭解有關授權管理的詳細資訊。 要瞭解有關 Azure AD 標識治理功能（包括特權標識管理、訪問審核和使用條款）廣度的更多內容，請參閱[什麼是 Azure AD 標識治理？](../governance/identity-governance-overview.md).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-public-preview"></a>在 Azure AD 門戶中為 Office 365 組配置命名策略（公共預覽）

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

管理員現在可以使用 Azure AD 門戶為 Office 365 組配置命名策略。 此更改有助於為組織中的使用者創建或編輯的 Office 365 組強制實施一致的命名約定。

您可以通過兩種不同的方式為 Office 365 組配置命名策略：

- 定義自動添加到組名稱的首碼或尾碼。

- 為您的組織上傳一組自訂的阻止字詞，這些單詞不允許在組名稱中使用（例如，"CEO、工資單、HR"）。

有關詳細資訊，請參閱為[Office 365 組強制實施命名策略](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy)。

---

### <a name="azure-ad-activity-logs-are-now-available-in-azure-monitor-general-availability"></a>Azure AD 活動日誌現在可在 Azure 監視器中提供（常規可用性）

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

為了説明使用 Azure AD 活動日誌解決有關視覺化效果的回饋，我們將在日誌分析中引入新的見解功能。 此功能可説明您使用我們的互動式範本（稱為活頁簿）獲得有關 Azure AD 資源的見解。 這些預構建的活頁簿可以為應用或使用者提供詳細資訊，包括：

- **登錄。** 為應用和使用者提供詳細資訊，包括登錄位置、正在使用的作業系統或瀏覽器用戶端和版本以及成功或失敗的登錄數。

- **舊版身份驗證和條件訪問。** 為使用舊版身份驗證的應用和使用者提供詳細資訊，包括由條件訪問策略觸發的多重要素驗證使用方式、使用條件訪問策略的應用等。

- **登錄失敗分析。** 説明您確定登錄錯誤是否由於使用者操作、策略問題或基礎結構而發生。

- **自訂報表。** 您可以創建新的活頁簿或編輯現有活頁簿，以説明為組織自訂見解功能。

有關詳細資訊，請參閱如何使用[Azure 活動目錄報表的 Azure 監視器活頁簿](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---april-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 4 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 4 月，我們在應用庫中添加了這 21 個具有聯合支援的新應用：

[SAP Fiori，](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-fiori-tutorial) [HRworks 單簽，](https://docs.microsoft.com/azure/active-directory/saas-apps/hrworks-single-sign-on-tutorial) [Percolate，](https://docs.microsoft.com/azure/active-directory/saas-apps/percolate-tutorial) [MobiControl，](https://docs.microsoft.com/azure/active-directory/saas-apps/mobicontrol-tutorial) [Citrix NetScaler，](https://docs.microsoft.com/azure/active-directory/saas-apps/citrix-netscaler-tutorial) [Shibumi，](https://docs.microsoft.com/azure/active-directory/saas-apps/shibumi-tutorial) [Benchling，](https://docs.microsoft.com/azure/active-directory/saas-apps/benchling-tutorial) [MileIQ，](https://mileiq.onelink.me/991934284/7e980085) [PageDNA，](https://docs.microsoft.com/azure/active-directory/saas-apps/pagedna-tutorial) [EduBrite LMS，](https://docs.microsoft.com/azure/active-directory/saas-apps/edubrite-lms-tutorial) [RStudio Connect，](https://docs.microsoft.com/azure/active-directory/saas-apps/rstudio-connect-tutorial) [AMMS，](https://docs.microsoft.com/azure/active-directory/saas-apps/amms-tutorial) [Mitel Connect，](https://docs.microsoft.com/azure/active-directory/saas-apps/mitel-connect-tutorial)[阿裡巴巴雲 （基於角色的 SSO），](https://docs.microsoft.com/azure/active-directory/saas-apps/alibaba-cloud-service-role-based-sso-tutorial) [Certent 股權管理](https://docs.microsoft.com/azure/active-directory/saas-apps/certent-equity-management-tutorial)， [Sectigo 證書管理器](https://docs.microsoft.com/azure/active-directory/saas-apps/sectigo-certificate-manager-tutorial)，[格林Orbit，](https://docs.microsoft.com/azure/active-directory/saas-apps/greenorbit-tutorial)[工作網格](https://docs.microsoft.com/azure/active-directory/saas-apps/workgrid-tutorial)， [monday.com，](https://docs.microsoft.com/azure/active-directory/saas-apps/mondaycom-tutorial) [調查猴子企業](https://docs.microsoft.com/azure/active-directory/saas-apps/surveymonkey-enterprise-tutorial)，[憤慨戈](https://indiggolead.com/)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="new-access-reviews-frequency-option-and-multiple-role-selection"></a>新的訪問審核頻率選項和多個角色選擇

**類型：** 新功能  
**服務類別：** 訪問評論  
**產品功能：** 身份治理

Azure AD 訪問審核中的新更新允許您：

- 除了以前存在的每週、每月、每季度和每年的選項之外，將訪問審核的頻率更改為**每半年**一次。

- 創建單個訪問審閱時，請選擇多個 Azure AD 和 Azure 資源角色。 在此情況下，所有角色都使用相同的設置設置，並同時通知所有檢閱者。

有關如何創建訪問審核的詳細資訊，請參閱[在 Azure AD 訪問審閱中創建組或應用程式的訪問審核](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)。

---

### <a name="azure-ad-connect-email-alert-systems-are-transitioning-sending-new-email-sender-information-for-some-customers"></a>Azure AD 連接電子郵件警報系統正在轉換，為某些客戶發送新的電子郵件寄件者資訊

**類型：** 已變更的功能  
**服務類別：** AD 同步  
**產品功能：** 平台

Azure AD Connect 正在轉換電子郵件警報系統，可能會向某些客戶顯示新的電子郵件寄件者。 要解決此問題，必須添加到`azure-noreply@microsoft.com`組織的允許清單，否則將無法繼續接收來自 Office 365、Azure 或同步服務的重要警報。

---

### <a name="upn-suffix-changes-are-now-successful-between-federated-domains-in-azure-ad-connect"></a>UPN 尾碼更改現在在 Azure AD 連接中的聯合域之間成功

**類型：** 固定  
**服務類別：** AD 同步  
**產品功能：** 平台

現在，您可以在 Azure AD Connect 中成功地將使用者的 UPN 尾碼從一個聯合域更改為另一個聯合域。 此修復意味著在同步週期期間不應再遇到聯合域域更改錯誤錯誤訊息，或收到通知電子郵件，指出"無法在 Azure 活動目錄中更新此物件，因為屬性 |聯合使用者.使用者主體名稱*，無效。 更新本地目錄服務中的值"。

有關詳細資訊，請參閱[在同步期間排除錯誤](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sync-errors#federateddomainchangeerror)。

---

### <a name="increased-security-using-the-app-protection-based-conditional-access-policy-in-azure-ad-public-preview"></a>使用 Azure AD 中基於應用保護的條件訪問策略提高安全性（公共預覽版）

**類型：** 新功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全&保護

使用 **"需要應用保護**"策略，現在可以使用基於應用保護的條件訪問。 此新策略通過説明防止：

- 無需 Microsoft Intune 許可證即可訪問應用的使用者。

- 使用者無法獲取 Microsoft Intune 應用保護原則。

- 無需配置的 Microsoft Intune 應用保護原則即可訪問應用的使用者。

有關詳細資訊，請參閱[如何要求應用保護原則的雲應用訪問與條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-protection-based-conditional-access)。

---

### <a name="new-support-for-azure-ad-single-sign-on-and-conditional-access-in-microsoft-edge-public-preview"></a>對 Microsoft 邊緣中的 Azure AD 單一登入和條件訪問的新支援（公共預覽版）

**類型：** 新功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全&保護

我們增強了對 Microsoft Edge 的 Azure AD 支援，包括為 Azure AD 單一登入和條件訪問提供新的支援。 如果您以前使用過微軟 Intune 託管瀏覽器，現在可以改用 Microsoft Edge。

有關使用條件訪問設置和管理設備和應用的詳細資訊，請參閱[使用條件訪問需要託管設備進行雲應用訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices)，[以及要求通過條件訪問進行雲應用訪問。](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) 有關如何使用 Microsoft Edge 與 Microsoft Intune 策略管理訪問的詳細資訊，請參閱[使用 Microsoft Intune 策略保護的瀏覽器管理 Internet 訪問](https://docs.microsoft.com/intune/app-configuration-managed-browser)。

---

## <a name="march-2019"></a>2019 年 3 月

### <a name="identity-experience-framework-and-custom-policy-support-in-azure-active-directory-b2c-is-now-available-ga"></a>Azure 活動目錄 B2C 中的標識體驗框架和自訂策略支援現已可用 （GA）

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C

現在，您可以在 Azure AD B2C 中創建自訂策略，包括以下任務，這些任務在規模和 Azure SLA 下得到支援：

- 使用自訂策略創建和上傳自訂身份驗證使用者旅程。

- 逐步將使用者旅程描述為宣告提供者之間的交換。

- 定義使用者旅程中的條件式分支。

- 轉換和映射用於即時決策和通信的聲明。

- 在自訂身份驗證使用者旅程中使用啟用 REST API 的服務。 例如，對於電子郵件供應商、VM 和專有授權系統。

- 與符合 OpenIDConnect 協定的標識提供程式聯合。 例如，對於多租戶 Azure AD、社交帳戶提供程式或雙重檢查器提供者。

有關創建自訂策略的詳細資訊，請參閱[Azure 活動目錄 B2C 中自訂策略的開發人員注釋](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-developer-notes-custom)，並閱讀[Alex Simon 的博客文章，包括案例研究](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-custom-policies-to-build-your-own-identity-journeys/ba-p/382791)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---march-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 3 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

2019 年 3 月，我們向應用庫添加了這 14 個具有聯合支援的新應用：

[ISEC7 移動交換代表](https://www.isec7.com/english/)， [MediusFlow，](https://office365.cloudapp.mediusflow.com/) [ePlatform，](https://docs.microsoft.com/azure/active-directory/saas-apps/eplatform-tutorial) [Fulcrum，](https://docs.microsoft.com/azure/active-directory/saas-apps/fulcrum-tutorial) [ExcelityGlobal，](https://docs.microsoft.com/azure/active-directory/saas-apps/excelityglobal-tutorial)[解釋為基礎的審計系統](https://docs.microsoft.com/azure/active-directory/saas-apps/explanation-based-auditing-system-tutorial)，[精益](https://docs.microsoft.com/azure/active-directory/saas-apps/lean-tutorial)，[動力學校性能事項](https://docs.microsoft.com/azure/active-directory/saas-apps/powerschool-performance-matters-tutorial)， [Cinode，](https://cinode.com/) [Iris 內聯網](https://docs.microsoft.com/azure/active-directory/saas-apps/iris-intranet-tutorial)， [Empactis，](https://docs.microsoft.com/azure/active-directory/saas-apps/empactis-tutorial)[智慧繪製](https://docs.microsoft.com/azure/active-directory/saas-apps/smartdraw-tutorial)，[確認地平線](https://docs.microsoft.com/azure/active-directory/saas-apps/confirmit-horizons-tutorial)， [TAS](https://docs.microsoft.com/azure/active-directory/saas-apps/tas-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="new-zscaler-and-atlassian-provisioning-connectors-in-the-azure-ad-gallery---march-2019"></a>Azure AD 庫中的新 Zscaler 和 Atlass 預配連接器 - 2019 年 3 月

**類型：** 新功能  
**服務類別：** 應用程式佈建  
**產品功能：** 協力廠商整合

自動創建、更新和刪除以下應用的使用者帳戶：

[茲卡爾，](https://aka.ms/ZscalerProvisioning)[茲卡爾貝塔](https://aka.ms/ZscalerBetaProvisioning)，[茲卡爾，](https://aka.ms/ZscalerOneProvisioning)[茨卡爾二](https://aka.ms/ZscalerTwoProvisioning)世，[茲卡爾，茨卡爾三](https://aka.ms/ZscalerThreeProvisioning)[，茲卡爾瑟·茲雲](https://aka.ms/ZscalerZSCloudProvisioning)，[阿特拉斯雲](https://aka.ms/atlassianCloudProvisioning)

有關如何通過自動使用者帳戶預配更好地保護組織的詳細資訊，請參閱[使用 Azure AD 自動向 SaaS 應用程式預配使用者](https://aka.ms/ProvisioningDocumentation)。

---

### <a name="restore-and-manage-your-deleted-office-365-groups-in-the-azure-ad-portal"></a>在 Azure AD 門戶中還原和管理已刪除的 Office 365 組

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

您現在可以從 Azure AD 門戶查看和管理已刪除的 Office 365 組。 此更改可説明您查看哪些組可供還原，同時允許您永久刪除組織不需要的任何組。

有關詳細資訊，請參閱[還原過期或刪除的組](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-restore-deleted#view-and-manage-the-deleted-office-365-groups-that-are-available-to-restore)。

---

### <a name="single-sign-on-is-now-available-for-azure-ad-saml-secured-on-premises-apps-through-application-proxy-public-preview"></a>現在，通過應用程式代理（公共預覽）對 Azure AD SAML 安全的本地應用提供單一登入

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

現在，您可以為本地 SAML 身份驗證的應用提供單個登錄 （SSO） 體驗，以及通過應用程式代理對這些應用的遠端存取。 有關如何使用本地應用設置 SAML SSO 的詳細資訊，請參閱[使用應用程式代理（預覽）的本地應用程式的單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-on-premises-apps)。

---

### <a name="client-apps-in-request-loops-will-be-interrupted-to-improve-reliability-and-user-experience"></a>請求迴圈中的用戶端應用將被中斷以提高可靠性和使用者體驗

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

用戶端應用可能會在短時間內錯誤地發出數百個相同的登錄請求。 這些請求（無論是否成功）都會導致 IDP 的使用者體驗不佳和工作負載增加，增加所有使用者的延遲並降低 IDP 的可用性。

此更新發送錯誤`invalid_grant`：`AADSTS50196: The server terminated an operation because it encountered a loop while processing a request`向在短時間內多次發出重複請求的用戶端應用發送錯誤，超出正常操作範圍。 遇到此問題的用戶端應用應顯示互動式提示，要求使用者重新登錄。 有關此更改的詳細資訊以及如何修復應用（如果遇到此錯誤），請參閱[身份驗證的新增功能？](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#looping-clients-will-be-interrupted)

---

### <a name="new-audit-logs-user-experience-now-available"></a>新的稽核記錄使用者體驗現已可用

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們創建了一個新的 Azure AD**稽核記錄**頁，以説明提高可讀性以及搜索資訊的方式。 要查看新的**稽核記錄**頁，請在 Azure AD**的活動部分**中選擇**稽核記錄**。

![新的稽核記錄頁面，包含示例資訊](media/whats-new/audit-logs-page.png)

有關新的**稽核記錄**頁的詳細資訊，請參閱 Azure[活動目錄門戶中的審核活動報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#audit-logs)。

---

### <a name="new-warnings-and-guidance-to-help-prevent-accidental-administrator-lockout-from-misconfigured-conditional-access-policies"></a>新的警告和指導，有助於防止意外管理員鎖定，避免配置錯誤的條件訪問策略

**類型：** 已變更的功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全&保護

為了説明防止管理員通過配置錯誤的條件訪問策略意外將自己鎖定自外租戶，我們在 Azure 門戶中創建了新的警告並更新了指南。 有關新指南的詳細資訊，請參閱 Azure[活動目錄條件訪問中的什麼是服務依賴項](https://docs.microsoft.com/azure/active-directory/conditional-access/service-dependencies)。

---

### <a name="improved-end-user-terms-of-use-experiences-on-mobile-devices"></a>改進行動裝置上的最終使用者使用條款體驗

**類型：** 已變更的功能  
**服務類別：** 使用條款  
**產品功能：** 治理

我們更新了現有的使用條款體驗，以説明改進您查看和同意行動裝置使用條款的方式。 您現在可以放大和縮小、返回、下載資訊以及選擇超連結。 有關更新的使用條款的詳細資訊，請參閱[Azure 活動目錄的使用條款功能](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#what-terms-of-use-looks-like-for-users)。

---

### <a name="new-azure-ad-activity-logs-download-experience-available"></a>新的 Azure AD 活動日誌下載體驗可用

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

現在，可以直接從 Azure 門戶下載大量活動日誌。 此更新允許您：

- 下載多達 250，000 行。

- 下載完成後收到通知。

- 自訂檔案名。

- 確定您的輸出格式，無論是 JSON 還是 CSV。

有關此功能的詳細資訊，請參閱[快速入門：使用 Azure 門戶下載審核報告](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-download-audit-report)

---

### <a name="breaking-change-updates-to-condition-evaluation-by-exchange-activesync-eas"></a>重大更改：通過 Exchange ActiveSync （EAS） 更新條件評估

**類型：** 方案變更  
**服務類別：** 條件式存取  
**產品功能：** 存取控制

我們正在更新 Exchange ActiveSync （EAS） 評估以下條件的方式：

- 使用者位置，基於國家/地區、區域或 IP 位址

- 登入風險

- 裝置平台

如果您以前在條件訪問策略中使用過這些條件，請注意條件行為可能會更改。 例如，如果您以前在策略中使用使用者位置條件，您可能會發現現在會根據使用者的位置跳過策略。

---

## <a name="february-2019"></a>2019 年 2 月

### <a name="configurable-azure-ad-saml-token-encryption-public-preview"></a>可配置的 Azure AD SAML 權杖加密（公共預覽版） 

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO

您現在可以配置任何受支援的 SAML 應用以接收加密的 SAML 權杖。 當與應用一起配置和使用時，Azure AD 將使用從 Azure AD 中存儲的證書獲取的公開金鑰對已發出的 SAML 斷言進行加密。

有關配置 SAML 權杖加密的詳細資訊，請參閱配置[Azure AD SAML 權杖加密](https://docs.microsoft.com/azure/active-directory/manage-apps/howto-saml-token-encryption)。

---

### <a name="create-an-access-review-for-groups-or-apps-using-azure-ad-access-reviews"></a>使用 Azure AD 訪問審核為組或應用創建訪問審核

**類型：** 新功能  
**服務類別：** 訪問評論  
**產品功能：** 治理

現在，您可以在單個 Azure AD 訪問審核中包含多個組或應用，以進行組成員身份或應用分配。 使用相同的設置設置具有多個組或應用的訪問審閱，並同時通知所有包含的檢閱者。

有關如何使用 Azure AD 訪問審核創建訪問審核的詳細資訊，請參閱[在 Azure AD 訪問審核中創建組或應用程式的訪問審核](https://docs.microsoft.com/azure/active-directory/governance/create-access-review)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---february-2019"></a>Azure AD 應用庫中提供的新聯合應用 - 2019 年 2 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
2019 年 2 月，我們在應用庫中添加了這 27 個具有聯合支援的新應用：

[歐洲監控護照](https://docs.microsoft.com/azure/active-directory/saas-apps/euromonitor-passport-tutorial)， [MindTickle，](https://docs.microsoft.com/azure/active-directory/saas-apps/mindtickle-tutorial) [FAT FINGER，](https://seeforgetest-exxon.azurewebsites.net/Account/create?Length=7) [AirStack，](https://docs.microsoft.com/azure/active-directory/saas-apps/airstack-tutorial)[甲骨文融合ERP，](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-fusion-erp-tutorial) [IDrive，](https://docs.microsoft.com/azure/active-directory/saas-apps/idrive-tutorial) [Skyward Qmlativ，](https://docs.microsoft.com/azure/active-directory/saas-apps/skyward-qmlativ-tutorial)[光明，](https://docs.microsoft.com/azure/active-directory/saas-apps/brightidea-tutorial)[警報，](https://docs.microsoft.com/azure/active-directory/saas-apps/alertops-tutorial) [Soloinsight-CloudGate SSO，](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-tutorial)許可權點擊，[品牌資料夾](https://docs.microsoft.com/azure/active-directory/saas-apps/brandfolder-tutorial)，[存儲gateSmartFile，](https://docs.microsoft.com/azure/active-directory/saas-apps/smartfile-tutorial) [Pexip，](https://docs.microsoft.com/azure/active-directory/saas-apps/pexip-tutorial)[風暴板](https://docs.microsoft.com/azure/active-directory/saas-apps/stormboard-tutorial)，[地震](https://docs.microsoft.com/azure/active-directory/saas-apps/seismic-tutorial)，[分享夢想](https://www.shareadream.org/how-it-works)，[蟲子，](https://docs.microsoft.com/azure/active-directory/saas-apps/bugsnag-tutorial)[網路方法集成雲](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial)，[知識任何地方 LMS，](https://docs.microsoft.com/azure/active-directory/saas-apps/knowledge-anywhere-lms-tutorial) [OU 校園](https://docs.microsoft.com/azure/active-directory/saas-apps/ou-campus-tutorial)，[內窺鏡資料](https://docs.microsoft.com/azure/active-directory/saas-apps/periscope-data-tutorial)，[內托普門戶](https://docs.microsoft.com/azure/active-directory/saas-apps/netop-portal-tutorial)， [smartvid.io，](https://docs.microsoft.com/azure/active-directory/saas-apps/smartvid.io-tutorial)[純雲由創世紀](https://docs.microsoft.com/azure/active-directory/saas-apps/purecloud-by-genesys-tutorial)，[點擊生產力平臺](https://docs.microsoft.com/azure/active-directory/saas-apps/clickup-productivity-platform-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="enhanced-combined-mfasspr-registration"></a>增強的 MFA/SSPR 聯合註冊

**類型：** 已變更的功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 使用者驗證

為了回應客戶回函，我們增強了組合的 MFA/SSPR 註冊預覽體驗，説明您的使用者更快地註冊 MFA 和 SSPR 的安全資訊。 

**要打開當前使用者的增強體驗，請按照以下步驟操作：**

1. 作為全域管理員或使用者管理員，登錄到 Azure 門戶並轉到 Azure**活動目錄>使用者設置>管理訪問面板預覽功能的設置**。 

2. 在 **"可以使用預覽功能註冊和管理安全資訊 + 刷新"選項中的"使用者"** 中，選擇打開**選定使用者組**或**所有使用者**的功能。

在接下來的幾周裡，我們將取消為尚未打開的租戶打開舊的組合 MFA/SSPR 註冊預覽體驗的能力。

**要查看是否將刪除租戶的控制項，請按照以下步驟操作：**

1. 作為全域管理員或使用者管理員，登錄到 Azure 門戶並轉到 Azure**活動目錄>使用者設置>管理訪問面板預覽功能的設置**。  

2. 如果**可以使用預覽功能註冊和管理安全資訊選項的使用者**設置為 **"無**"，則該選項將從租戶中刪除。

無論您以前是否為使用者打開了舊的組合 MFA/SSPR 註冊預覽體驗，舊體驗都將在將來關閉。 因此，我們強烈建議您儘快轉向新的增強型體驗。

有關增強的註冊體驗的詳細資訊，請參閱 Azure [AD 組合 MFA 和密碼重設註冊體驗的 Cool 增強功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271)。

---

### <a name="updated-policy-management-experience-for-user-flows"></a>更新使用者流的策略管理體驗

**類型：** 已變更的功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C

我們更輕鬆地更新了使用者流的策略創建和管理過程（以前稱為內置策略）。 此新體驗現在是所有 Azure AD 租戶的預設體驗。

您可以使用門戶螢幕頂部的 **"向我們發送"回饋**區域中的微笑或皺眉圖示提供其他回饋和建議。

有關新的策略管理體驗的詳細資訊，請參閱[Azure AD B2C 現在具有 JavaScript 自訂和更多新功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)博客。

---

### <a name="choose-specific-page-element-versions-provided-by-azure-ad-b2c"></a>選擇 Azure AD B2C 提供的特定頁面元素版本

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C

現在，您可以選擇 Azure AD B2C 提供的頁面元素的特定版本。 通過選擇特定版本，您可以在更新顯示在頁面上之前對其進行測試，並可以獲取可預測的行為。 此外，您現在可以選擇強制實施特定的頁面版本，以允許 JavaScript 自訂。 要打開此功能，請轉到使用者流中的 **"屬性"** 頁。

有關選擇特定版本的頁面元素的詳細資訊，請參閱[Azure AD B2C 現在具有 JavaScript 自訂和更多新功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)博客。

---

### <a name="configurable-end-user-password-requirements-for-b2c-ga"></a>B2C （GA） 的可配置最終使用者密碼要求

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C

現在，您可以為最終使用者設置組織的密碼複雜性，而不必使用本機 Azure AD 密碼原則。 從使用者流的 **"屬性"** 邊欄選項卡（以前稱為內置策略）中，您可以選擇 **"簡單**"或"**強"** 的密碼複雜性，也可以創建**自訂**要求集。

有關密碼複雜性要求配置的詳細資訊，請參閱[在 Azure 活動目錄 B2C 中配置密碼的複雜性要求](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-password-complexity)。

---

### <a name="new-default-templates-for-custom-branded-authentication-experiences"></a>用於自訂品牌身份驗證體驗的新預設範本

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C

您可以使用我們位於使用者流的 **"主頁"佈局**邊欄選項卡上的新預設範本（以前稱為內置策略）為您的使用者創建自訂品牌身份驗證體驗。

有關使用範本的詳細資訊，請參閱[Azure AD B2C 現在具有 JavaScript 自訂和更多新功能](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-B2C-now-has-JavaScript-customization-and-many-more-new/ba-p/353595)。

---

## <a name="january-2019"></a>2019 年 1 月

### <a name="active-directory-b2b-collaboration-using-one-time-passcode-authentication-public-preview"></a>使用單次密碼驗證 (公開預覽) 的 Active Directory B2B 共同作業

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

對於無法透過如 Azure AD、Microsoft 帳戶 (MSA) 或 Google 同盟等方式驗證的 B2B 來賓使用者，我們已加入單次密碼驗證 (OTP)。 這個新的驗證方法表示來賓使用者不需要建立新的 Microsoft 帳戶。 另一方便，兌換邀請或存取共用的資源時，來賓使用者可以要求臨時代碼來傳送電子郵件地址。 使用此組臨時代碼時，來賓使用者可以繼續登入。

如需詳細資訊，請參閱[電子郵件單次密碼驗證 (預覽版)](https://docs.microsoft.com/azure/active-directory/b2b/one-time-passcode) 和部落格 [Azure AD 可讓任何帳戶的任何使用者順利共用和共同作業](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-makes-sharing-and-collaboration-seamless-for-any-user/ba-p/325949)。

### <a name="new-azure-ad-application-proxy-cookie-settings"></a>新增 Azure AD 應用程式 Proxy Cookie 設定

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

我們引進了三項新的 cookie 設定，以供透過應用程式 Proxy 發佈的應用程式使用：

- **使用僅 HTTP Cookie。** 在您的應用程式 Proxy 存取和工作階段 Cookie 上設定 **HTTPOnly** 旗標。 開啟此設定可提供額外的安全性優點，例如協助防止透過用戶端指令碼複製或修改 Cookie。 我們建議您開啟這個旗標 (選擇 [是]****)，以獲得附加好處。

- **使用安全 Cookie。** 在您的應用程式 Proxy 存取和工作階段 Cookie 上設定 [安全]**** 旗標。 開啟此設定可提供額外的安全性優點，例如確定 Cookie 只會透過 TLS 安全通道 (例如 HTTPS) 傳輸。 我們建議您開啟這個旗標 (選擇 [是]****)，以獲得附加好處。

- **使用持久性 Cookie。** 防止存取 Cookie 在關閉 Web 瀏覽器關閉時過期。 這些 Cookie 會在存取權杖的存留期中持續存在。 不過，如果已達到期時間，或是使用者手動刪除 Cookie，便會重設 Cookie。 我們建議您保留預設設定 [否]****，僅針對程序間不共用 Cookie 的舊版應用程式開啟此設定。

如需新 Cookie 的詳細資訊，請參閱 [Azure Active Directory 中用來存取內部部署應用程式的 Cookie 設定](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2019"></a>Azure AD 應用程式庫推出新的同盟應用程式 - 2019 年 1 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們已在 2019 年 1 月將下列這 35 個提供同盟支援的全新應用程式新增至應用程式庫：

[Firstbird](https://docs.microsoft.com/azure/active-directory/saas-apps/firstbird-tutorial)、[Folloze](https://docs.microsoft.com/azure/active-directory/saas-apps/folloze-tutorial)、[Talent Palette](https://docs.microsoft.com/azure/active-directory/saas-apps/talent-palette-tutorial)、[Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloud-suite-tutorial)、[Cisco Umbrella](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-umbrella-tutorial)、[Zscaler Internet Access Administrator](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-internet-access-administrator-tutorial)、[Expiration Reminder](https://docs.microsoft.com/azure/active-directory/saas-apps/expiration-reminder-tutorial)、[InstaVR Viewer](https://docs.microsoft.com/azure/active-directory/saas-apps/instavr-viewer-tutorial)、[CorpTax](https://docs.microsoft.com/azure/active-directory/saas-apps/corptax-tutorial)、[Verb](https://app.verb.net/login)、[OpenLattice](https://openlattice.com/agora)、[TheOrgWiki](https://www.theorgwiki.com/signup)、[Pavaso Digital Close](https://docs.microsoft.com/azure/active-directory/saas-apps/pavaso-digital-close-tutorial)、[GoodPractice Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/goodpractice-toolkit-tutorial)、[Cloud Service PICCO](https://docs.microsoft.com/azure/active-directory/saas-apps/cloud-service-picco-tutorial)、[AuditBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/auditboard-tutorial)、[iProva](https://docs.microsoft.com/azure/active-directory/saas-apps/iprova-tutorial)、[Workable](https://docs.microsoft.com/azure/active-directory/saas-apps/workable-tutorial)、[CallPlease](https://webapp.callplease.com/create-account/create-account.html)、[GTNexus SSO System](https://docs.microsoft.com/azure/active-directory/saas-apps/gtnexus-sso-module-tutorial)、[CBRE ServiceInsight](https://docs.microsoft.com/azure/active-directory/saas-apps/cbre-serviceinsight-tutorial)、[Deskradar](https://docs.microsoft.com/azure/active-directory/saas-apps/deskradar-tutorial)、[Coralogixv](https://docs.microsoft.com/azure/active-directory/saas-apps/coralogix-tutorial)、[Signagelive](https://docs.microsoft.com/azure/active-directory/saas-apps/signagelive-tutorial)、[ARES for Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/ares-for-enterprise-tutorial)、[K2 for Office 365](https://www.k2.com/O365)、[Xledger](https://www.xledger.net/)、[iDiD Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/idid-manager-tutorial)、[HighGear](https://docs.microsoft.com/azure/active-directory/saas-apps/highgear-tutorial)、[Visitly](https://docs.microsoft.com/azure/active-directory/saas-apps/visitly-tutorial)、[Korn Ferry ALP](https://docs.microsoft.com/azure/active-directory/saas-apps/korn-ferry-alp-tutorial)、[Acadia](https://docs.microsoft.com/azure/active-directory/saas-apps/acadia-tutorial)、[Adoddle cSaas Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/adoddle-csaas-platform-tutorial)<!-- , [CaféX Portal (Meetings)](https://docs.microsoft.com/azure/active-directory/saas-apps/cafexportal-meetings-tutorial), [MazeMap Link](https://docs.microsoft.com/azure/active-directory/saas-apps/mazemaplink-tutorial)-->  

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="new-azure-ad-identity-protection-enhancements-public-preview"></a>新增 Azure AD Identity Protection 增強功能 (公開預覽)

**類型：** 已變更的功能  
**服務類別：** 身份保護  
**產品功能：** 身份安全&保護

我們很興奮地宣布，我們在 Azure AD Identity Protection 公開預覽供應項目中新增了下列增強功能，包括：

- 已更新且整合性更高的使用者介面

- 其他 API

- 透過機器學習服務改良風險評估

- 有風險的使用者與有風險的登入間的全產品對照

如需增強功能的詳細資訊，請參閱[什麼是 Azure Active Directory Identity Protection (已重新整理)？](https://aka.ms/IdentityProtectionDocs) 以深入了解相關資訊並透過產品內的提示分享您的想法。

---

### <a name="new-app-lock-feature-for-the-microsoft-authenticator-app-on-ios-and-android-devices"></a>為 iOS 及 Android 裝置上的 Microsoft Authenticator 應用程式新增應用程式鎖定功能

**類型：** 新功能  
**服務類別：** 微軟身份驗證器應用程式  
**產品功能：** 身份安全&保護

若要讓單次密碼、應用程式資訊和應用程式設定更加安全，您可以在 Microsoft Authenticator 應用程式中開啟應用程式鎖定功能。 打開應用鎖定意味著每次打開 Microsoft 身份驗證器應用時，系統都會要求您使用 PIN 或生物識別進行身份驗證。

如需詳細資訊，請參閱 [Microsoft Authenticator 應用程式常見問題集](https://docs.microsoft.com/azure/active-directory/user-help/microsoft-authenticator-app-faq)。

---

### <a name="enhanced-azure-ad-privileged-identity-management-pim-export-capabilities"></a>增強 Azure AD Privileged Identity Management (PIM) 的匯出功能

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management

Privileged Identity Management (PIM) 系統管理員現在可以針對特定資源匯出所有作用中且合格的角色指派，其中包含所有子資源的角色指派。 之前系統管理員很難取得訂用帳戶的角色指派完整清單，而且他們必須針對每個特定資源匯出角色指派。

如需詳細資訊，請參閱[在 PIM 中檢視 Azure 資源角色的活動和稽核記錄](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)。

---

## <a name="novemberdecember-2018"></a>2018 年 11 月/12月

### <a name="users-removed-from-synchronization-scope-no-longer-switch-to-cloud-only-accounts"></a>從同步處理範圍移除的使用者不會再切換為僅雲端帳戶

**類型：** 固定  
**服務類別：** 使用者管理  
**產品功能：** 目錄

>[!Important]
>我們已收到訊息，且了解您對此修正並不滿意。 因此，我們已將這項變更還原到可讓您較易於在組織中實作修正的時間點。

我們修復了一個錯誤，當活動目錄域服務 （AD DS） 物件從同步作用域中排除，然後在後續同步週期移動到 Azure AD 中的回收站時，使用者的 DirSyncEnabled 標誌將錯誤地切換到**False。** 由於此修正的結果，如果從同步處理範圍排除使用者，並在之後從 Azure AD 資源回收桶還原，使用者帳戶會如預期地保留與內部部署 AD 的同步，且無法從雲端管理，因為其授權來源 (SoA) 仍然是內部部署 AD。

在此修正之前，於 DirSyncEnabled 旗標切換為 False 時會發生問題。 這會給人錯誤的印象，認為這些帳戶已被轉換為僅限雲端物件，且該帳戶可在雲端中進行管理。 不過，帳戶仍然保留其 SoA 為內部部署，且所有已同步的屬性 (陰影屬性) 皆來自內部部署 AD。 這種情況會在 Azure AD 中造成多個問題，且其他雲端工作負載 (例如 Exchange Online) 預期將這些帳戶視為從 AD 進行同步處理，但現在行為卻類似僅雲端帳戶。

這時，將 AD 帳戶同步處理真正轉換為僅雲端帳戶的唯一方式，是停用租用戶層級的 DirSync，這會觸發後端作業以傳輸 SoA。 這類 SoA 變更必須 (但不限於) 清除所有與內部部署相關的屬性 (例如 LastDirSyncTime 和陰影屬性)，並且傳送信號給其他雲端工作負載，以使其各自的物件也轉換為僅雲端帳戶。

因此，此修正防止直接更新從 AD 同步處理之使用者的 ImmutableID 屬性 (這在過去的某些案例中是必要的)。 根據設計，Azure AD 中物件的 ImmutableID 顧名思義就是不可變更的。 Azure Active Directory Connect Health 和 Azure AD Connect 同步用戶端實作的新功能，可用來解決這類案例：

- **以分段方式完成大規模更新許多使用者的 ImmutableID**
  
  例如，您需要執行冗長的 AD DS 內部樹系移轉。 解決方案：使用 Azure AD 連接**來配置源錨點**，並在使用者遷移時，將現有的不可移動 ID 值從 Azure AD 複製到本地 AD DS 使用者的 ms-DS-一致性-Guid 屬性的新林。 如需詳細資訊，請參閱[使用 ms-DS-ConsistencyGuid 作為 sourceAnchor](/azure/active-directory/hybrid/plan-connect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor)。

- **一次完成大規模更新許多使用者的 ImmutableID**

  例如，您在實作 Azure AD Connect 時發生錯誤，而現在您需要變更 SourceAnchor 屬性。 解決方案：在租戶級別禁用 DirSync 並清除所有不正確 Immuid 值。 如需詳細資訊，請參閱[關閉 Office 365 的目錄同步處理](/office365/enterprise/turn-off-directory-synchronization)。

- **重新比對內部部署使用者與 Azure AD 中的現有使用者** 例如，在 AD DS 中重新建立了某個使用者，在 Azure AD 帳戶中產生重複，而不是重新比對現有的 Azure AD 帳戶 (孤立的物件)。 解決方案：在 Azure 門戶中使用 Azure AD 連接運行狀況重新映射源錨點/不可改變 ID。 如需詳細資訊，請參閱[孤立物件案例](/azure/active-directory/hybrid/how-to-connect-health-diagnose-sync-errors#orphaned-object-scenario)。

### <a name="breaking-change-updates-to-the-audit-and-sign-in-logs-schema-through-azure-monitor"></a>中斷更改：通過 Azure 監視器更新審核和登錄日誌架構

**類型：** 已變更的功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們目前透過 Azure 監視器發佈稽核和登入記錄資料流，因此您可以將記錄檔與 SIEM 工具或 Log Analytics完美整合。 根據您的意見反應，並準備此功能正式發行的宣佈，我們對結構描述進行下列變更。 這些結構描述變更及其相關文件更新將在 1 月的第一週進行。

#### <a name="new-fields-in-the-audit-schema"></a>稽核結構描述中的新欄位
我們正在新增新的 [操作類型]**** 欄位，以提供對資源執行的操作類型。 例如，**新增**、**更新**或**刪除**。

#### <a name="changed-fields-in-the-audit-schema"></a>稽核結構描述中變更的欄位
稽核結構描述中的下列欄位會變更：

|欄位名稱|變更內容|舊值|新值|
|----------|------------|----------|----------|
|類別|這以前是 [服務名稱]**** 欄位。 現在是 [稽核類別]**** 欄位。 [服務名稱]**** 已重新命名為 [loggedByService]**** 欄位。|<ul><li>帳戶佈建</li><li>核心目錄</li><li>自助式密碼重設</li></ul>|<ul><li>使用者管理</li><li>群組管理</li><li>應用程式管理</li></ul>|
|targetResources|包含最高層級的 **TargetResourceType**。|&nbsp;|<ul><li>原則</li><li>App</li><li>User</li><li>群組</li></ul>|
|loggedByService|提供產生稽核記錄之服務的名稱。|Null|<ul><li>帳戶佈建</li><li>核心目錄</li><li>自助式密碼重設</li></ul>|
|結果|提供稽核記錄的結果。 在過去這是列舉的，但我們現在會顯示實際值。|<ul><li>0</li><li>1</li></ul>|<ul><li>Success</li><li>失敗</li></ul>|

#### <a name="changed-fields-in-the-sign-in-schema"></a>登入結構描述中變更的欄位
登入結構描述中的下列欄位會變更：

|欄位名稱|變更內容|舊值|新值|
|----------|------------|----------|----------|
|appliedConditionalAccessPolicies|這以前是 [conditionalaccessPolicies]**** 欄位。 現在是 [appliedConditionalAccessPolicies]**** 欄位。|沒有變更|沒有變更|
|conditionalAccessStatus|在登入時提供條件式存取原則狀態的結果。 在過去這是列舉的，但我們現在會顯示實際值。|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Success</li><li>失敗</li><li>未套用</li><li>已停用</li></ul>|
|appliedConditionalAccessPolicies: result|在登入時提供個別條件式存取原則狀態的結果。 在過去這是列舉的，但我們現在會顯示實際值。|<ul><li>0</li><li>1</li><li>2</li><li>3</li></ul>|<ul><li>Success</li><li>失敗</li><li>未套用</li><li>已停用</li></ul>|

如需結構描述的相關詳細資訊，請參閱[解譯 Azure 監視器中的 Azure AD 稽核記錄結構描述 (預覽)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-azure-monitor-audit-log-schema)

---

### <a name="identity-protection-improvements-to-the-supervised-machine-learning-model-and-the-risk-score-engine"></a>受監督的機器學習模型和風險分數引擎的身分識別保護增強功能

**類型：** 已變更的功能  
**服務類別：** 身份保護  
**產品功能：** 風險分數

對身分識別保護相關使用者和登入風險評估引擎的增強功能，有助於改善使用者風險精確度和涵蓋範圍。 系統管理員可能會注意到，使用者風險層級不再直接與特定偵測的風險層級相關聯，並且有風險的登入事件的數量和等級也會增加。

風險檢測現在由受監督的機器學習模型進行評估，該模型使用使用者登錄的其他功能和檢測模式來計算使用者風險。 根據此模型，系統管理員可能會發現具有高風險分數的使用者，即使與該使用者相關聯的偵測屬於低或中層級的風險。 

---

### <a name="administrators-can-reset-their-own-password-using-the-microsoft-authenticator-app-public-preview"></a>系統管理員可以使用 Microsoft Authenticator 應用程式重設其密碼 (公開預覽)

**類型：** 已變更的功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 使用者驗證

Azure AD 系統管理員現在可以使用 Microsoft Authenticator 應用程式通知，或來自任何行動驗證器應用程式或硬體權杖的程式碼重設其密碼。 若要重設其密碼，系統管理員現在可以使用下列兩個方法：

- Microsoft Authenticator 應用程式通知

- 其他行動驗證器應用程式 / 硬體權杖

- 電子郵件

- 撥打電話

- 簡訊

如需使用 Microsoft Authenticator 應用程式重設密碼的相關詳細資訊，請參閱 [Azure AD 自助式密碼重設 - 行動裝置應用程式和 SSPR (預覽)](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#mobile-app-and-sspr)

---

### <a name="new-azure-ad-cloud-device-administrator-role-public-preview"></a>新的 Azure AD 雲端裝置系統管理員角色 (公開預覽)

**類型：** 新功能  
**服務類別：** 設備註冊和管理  
**產品功能：** 存取控制

系統管理員可以將使用者指派至新的雲端裝置系統管理員角色，以執行雲端裝置系統管理員工作。 使用者指派的雲端裝置系統管理員角色者可以啟用、停用和刪除 Azure AD 中的裝置，且能在 Azure 入口網站中讀取 Windows 10 BitLocker 金鑰 (如果有的話)。

如需有關角色和權限的詳細資訊，請參閱[在 Azure Active Directory 中指派系統管理員角色](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)

---

### <a name="manage-your-devices-using-the-new-activity-timestamp-in-azure-ad-public-preview"></a>使用 Azure AD 中的新活動時間戳記管理裝置 (公開預覽)

**類型：** 新功能  
**服務類別：** 設備註冊和管理  
**產品功能：** 設備生命週期管理

我們意識到，隨著時間的推移，您必須在 Azure AD 中刷新和停用組織的設備，以避免環境中有陳舊的設備。 為了協助進行此程序，Azure AD 現在會使用新的活動時間戳記更新您的裝置，協助您管理您的裝置生命週期。

有關如何獲取和使用此時間戳記的詳細資訊，請參閱[如何：在 Azure AD 中管理陳舊的設備](https://docs.microsoft.com/azure/active-directory/devices/manage-stale-devices)

---

### <a name="administrators-can-require-users-to-accept-a-terms-of-use-on-each-device"></a>管理員可以要求使用者接受每台設備上的使用條款

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 治理
 
管理員現在可以打開 **"要求使用者在每台設備上同意**"選項，以要求使用者接受您在租戶上使用的每個設備上的使用條款。

有關詳細資訊，請參閱 Azure[活動目錄使用函數中的"每設備使用條款"部分](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#per-device-terms-of-use)。

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-a-recurring-schedule"></a>管理員可以根據定期計畫將使用條款配置為過期

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 治理
 

管理員現在可以打開 **"過期同意"** 選項，以便根據指定的定期計畫使所有使用者的使用條款過期。 排程可以是每年、每兩年、每季，或每個月。 使用條款到期後，使用者必須重新接受。

有關詳細資訊，請參閱 Azure[活動目錄使用條款的"添加使用條款"部分](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)。

---

### <a name="administrators-can-configure-a-terms-of-use-to-expire-based-on-each-users-schedule"></a>管理員可以根據每個使用者的排程將使用條款配置為過期

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 治理

管理員現在可以指定使用者必須重新接受使用條款的持續時間。 例如，管理員可以指定使用者必須每 90 天重新接受使用條款。

有關詳細資訊，請參閱 Azure[活動目錄使用條款的"添加使用條款"部分](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use#add-terms-of-use)。
 
---

### <a name="new-azure-ad-privileged-identity-management-pim-emails-for-azure-active-directory-roles"></a>Azure Active Directory 角色的新 Azure AD Privileged Identity Management (PIM) 電子郵件

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
使用 Azure AD Privileged Identity Management (PIM) 的客戶現在可以收到每週摘要電子郵件，其中包括過去七天的下列資訊：

- 最高合格和永久角色指派概觀

- 啟用角色的使用者數目

- 指派給 PIM 中角色的使用者數目

- 指派給 PIM 外部角色的使用者數目

- PIM 中「永久的」使用者數目

如需 PIM 和可用電子郵件通知的相關詳細資訊，請參閱 [PIM 中的電子郵件通知](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-email-notifications)。

---

### <a name="group-based-licensing-is-now-generally-available"></a>群組型授權現已正式推出

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 目錄

群組型授權不在公開預覽範圍內，而且現在已正式推出。 作為此正式發行的一部分，我們使這項功能更具擴充性，並新增了為單一使用者重新處理群組型授權指派的功能，以及搭配 Office 365 E3/A3 授權使用群組型授權的功能。

如需群組型授權的相關詳細資訊，請參閱[什麼是Azure Active Directory 中的群組型授權？](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-licensing-whatis-azure-portal)

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 11 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 11 月將下列這 26 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[CoreStack](https://cloud.corestack.io/site/login)、[HubSpot](https://docs.microsoft.com/azure/active-directory/saas-apps/HubSpot-tutorial)、[GetThere](https://docs.microsoft.com/azure/active-directory/saas-apps/getthere-tutorial)、[Gra-Pe](https://docs.microsoft.com/azure/active-directory/saas-apps/grape-tutorial)、[eHour](https://getehour.com/try-now)、[Consent2Go](https://docs.microsoft.com/azure/active-directory/saas-apps/Consent2Go-tutorial)、[Appinux](https://docs.microsoft.com/azure/active-directory/saas-apps/appinux-tutorial)、[DriveDollar](https://azuremarketplace.microsoft.com/marketplace/apps/savitas.drivedollar-azuread?tab=Overview)、[Useall](https://docs.microsoft.com/azure/active-directory/saas-apps/useall-tutorial)、[Infinite Campus](https://docs.microsoft.com/azure/active-directory/saas-apps/infinitecampus-tutorial)、[Alaya](https://alayagood.com/en/demo/)、[HeyBuddy](https://docs.microsoft.com/azure/active-directory/saas-apps/heybuddy-tutorial)、[Wrike SAML](https://docs.microsoft.com/azure/active-directory/saas-apps/wrike-tutorial)、[Drift](https://docs.microsoft.com/azure/active-directory/saas-apps/drift-tutorial)、[Zenegy for Business Central 365](https://accounting.zenegy.com/)、[Everbridge 成員入口網站](https://docs.microsoft.com/azure/active-directory/saas-apps/everbridge-tutorial)、[IDEO](https://profile.ideo.com/users/sign_up)、[Ivanti Service Manager (ISM)](https://docs.microsoft.com/azure/active-directory/saas-apps/ivanti-service-manager-tutorial)、[Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-tutorial)、[Allbound SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/allbound-sso-tutorial)、[Plex 應用程式 - 傳統測試](https://test.plexonline.com/signon)、[Plex 應用程式 – 傳統 ](https://www.plexonline.com/signon)、[Plex 應用程式 - UX 測試](https://test.cloud.plex.com/sso)、[Plex 應用程式 – UX](https://cloud.plex.com/sso)、[Plex 應用程式 – IAM](https://accounts.plex.com/)、[CRAFTS - 育兒記錄、出席與財務追蹤系統](https://getcrafts.ca/craftsregistration) 

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

## <a name="october-2018"></a>2018 年 10 月

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Azure AD 記錄現在適用於 Azure Log Analytics (公開預覽)

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

我們很高興宣佈您現在可以將您的 Azure AD 記錄轉送到 Azure Log Analytics！ 此呼聲最高的功能可讓您更容易存取業務、作業和安全性的分析，以及提供協助監視基礎結構的方法。 如需詳細資訊，請參閱 [Azure Active Directory Activity logs in Azure Log Analytics now available](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843) (現在可以使用 Azure Log Analytics 中的 Azure Active Directory 記錄) 部落格。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 10 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

我們已在 2018 年 10 月將下列這 14 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[My Award Points](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial)、[Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial)、Ambyint、[MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial)、[BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial)、Dialpad、[ON24 Virtual Environment](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial)、[RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial)、[Zscaler Three](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial)、[Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial)、[Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial)、[Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial)、[Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial)、[Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="azure-ad-domain-services-email-notifications"></a>Azure AD Domain Services 電子郵件通知

**類型：** 新功能  
**服務類別：** Azure AD Domain Services  
**產品功能：** Azure AD Domain Services

Azure Active Directory Domain Services 會在 Azure 入口網站上提供有關受控網域設定錯誤或問題的相關警示。 這些警示包含逐步指南，因此您可以嘗試修正問題，而不需連絡支援人員。

從十月開始，您就能夠自訂受控網域的通知設定，以便在出現新的警示時，將電子郵件傳送給指定的一群人，而不需要經常檢查入口網站中是否有更新。

如需詳細資訊，請參閱 [Azure AD Domain Services 中的通知設定](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications)。

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>Azure AD 入口網站支援使用 ForceDelete 網域 API 來刪除自訂網域 

**類型：** 已變更的功能  
**服務類別：** 目錄管理  
**產品功能：** 目錄

我們很高興宣佈您現在可以使用 ForceDelete 網域 API，以非同步方式將參考資料 (例如使用者、群組和應用程式) 從自訂網域名稱 (contoso.com) 重新命名回初始預設網域名稱 (contoso.onmicrosoft.com)，藉此刪除自訂網域名稱。

如果貴組織不再使用您的自訂網域名稱，或如果您需要在另一個 Azure AD 中使用該網域名稱，此變更可協助您更快速地刪除該網域名稱。

如需詳細資訊，請參閱[刪除自訂網域名稱](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name)。

---

## <a name="september-2018"></a>2018 年 9 月
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>已更新動態群組的系統管理員角色權限

**類型：** 固定  
**服務類別：** 群組管理  
**產品功能：** 共同作業

我們已修正某個問題，讓特定系統管理員角色現在可以建立和更新動態成員資格規則，而不必成為群組的擁有者。

角色如下：

- 全域管理員

- Intune 系統管理員

- 使用者管理員

如需詳細資訊，請參閱[建立動態群組並檢查狀態](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>簡化了某些第三方應用程式的單一登入 (SSO) 組態設定

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO

我們了解，因為每個應用程式的組態都有獨特的本質，所以為軟體即服務 (SaaS) 應用程式設定單一登入 (SSO) 並不容易。 我們已建置經過簡化的組態體驗，來為下列第三方 SaaS 應用程式自動填入 SSO 組態設定：

- Zendesk

- ArcGis Online

- Jamf Pro

要開始使用此一鍵式體驗，請轉到應用的**Azure 門戶** > **SSO 配置**頁。 如需詳細資訊，請參閱 [SaaS 應用程式與 Azure Active Directory 的整合](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>[Azure Active Directory - 資料位於何處？] 頁面

**類型：** 新功能  
**服務類別：** 其他  
**產品功能：** GoLocal

從 [Azure Active Directory - 資料位於何處]**** 頁面選取公司的區域，以檢視是哪一個 Azure 資料中心裝載了所有 Azure AD 服務的 Azure AD 待用資料。 您可以根據貴公司區域的特定 Azure AD 服務來篩選資訊。

若要存取這項功能以及需要詳細資訊，請參閱 [Azure Active Directory - 資料位於何處](https://aka.ms/AADDataMap)。

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>適用於 [我的應用程式] 存取面板的新部署方案

**類型：** 新功能  
**服務類別：** 我的應用程式  
**產品功能：** SSO

查看適用於 [我的應用程式] 存取面板的新部署方案 (https://aka.ms/deploymentplans)。
[我的應用程式] 存取面板可讓使用者在單一位置尋找及存取其應用程式。 此入口網站也提供使用者自助服務的機會，例如要求存取應用程式和群組，或代表其他人管理這些資源的存取權。

如需詳細資訊，請參閱[什麼是「我的應用程式」入口網站？](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Azure 入口網站的 [登入記錄] 頁面上有新的 [疑難排解和支援] 索引標籤

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

Azure**門戶登錄頁上**的新 **"故障排除和支援**"選項卡旨在説明管理員和支援工程師解決與 Azure AD 登錄相關的問題。此新選項卡提供錯誤代碼、錯誤訊息和修正建議（如果有）以説明解決問題。 如果您無法解決問題，我們也提供了新的辦法供您使用**複製到剪貼簿**體驗來新建支援票證，其會在支援票證的記錄檔中填入 [要求識別碼]**** 和 [日期 (UTC)]**** 欄位。  

![顯示新選項卡的登錄日誌](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>針對用來建立動態成員資格規則的自訂擴充屬性強化其支援

**類型：** 已變更的功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業

經由此更新，您現在可以從動態使用者群組規則建立器按一下 [取得自訂擴充屬性]**** 連結，輸入唯一的應用程式識別碼，然後接收在為使用者建立動態成員資格規則時所要使用的自訂擴充屬性完整清單。 您也可以重新整理這份清單，來針對該應用程式取得任何新的自訂擴充屬性。

如需針對動態成員資格規則使用自訂擴充屬性的詳細資訊，請參閱[擴充屬性和自訂擴充屬性](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>基於 Azure AD 應用的條件訪問的新已批准用戶端應用

**類型：** 方案變更  
**服務類別：** 條件式存取  
**產品功能：** 身份安全和保護

下列應用程式已列入[已核准的用戶端應用程式](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)清單：

- Microsoft To-Do

- Microsoft Stream

如需詳細資訊，請參閱

- [基於 Azure AD 應用的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>針對 Windows 7/8/8.1 鎖定畫面中的自助式密碼重設提供了新的支援

**類型：** 新功能  
**服務類別：** SSPR  
**產品功能：** 使用者驗證

在設定這項新功能之後，使用者就會在執行 Windows 7、Windows 8 或 Windows 8.1 的裝置中，從其 [鎖定]**** 畫面看到用以重設其密碼的連結。 藉由按一下該連結，系統就會引導使用者完成密碼重設流程，過程就和透過網頁瀏覽器所進行的一樣。

如需詳細資訊，請參閱[如何從 Windows 7、8 和 8.1 啟用密碼重設](https://aka.ms/ssprforwindows78)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>變更通知：授權碼將不再供重複使用 

**類型：** 方案變更  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

從 2018 年 11 月 15 日起，Azure AD 將不再接受將先前使用的驗證碼用於應用程式。 這項安全性變更有助於讓 Azure AD 與 OAuth 規格一致，且將會對 v1 和 v2 端點強制執行。

如果您的應用程式重複使用授權碼取得多個資源的權杖，建議您先使用授權碼取得重新整理權杖，再使用該重新整理權杖取得其他資源的額外權杖。 授權碼只能使用一次，但重新整理權杖可跨多個資源多次使用。 在 OAuth 程式碼流程期間嘗試重複使用驗證碼的應用程式，會收到 invalid_grant 錯誤。

針對這項變更以及其他與通訊協定相關的變更，請參閱[新驗證功能的完整清單](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 9 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 9 月將下列這 16 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial)、[Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial)、[Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial)、[ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial)、[Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial)、[JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial)、[Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial)、NavigoCloud、[Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial)、join.me、[ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial)、[Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial)、Riverbed Xirrus EasyPass、[Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial)、Enlyft SSO for Azure、SurveyMonkey、[Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial)、[dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="support-for-additional-claims-transformations-methods"></a>支援其他宣告轉換方法

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO

我們已導入新的宣告轉換方法 ToLower() 和 ToUpper()，您可從以 SAML 為基礎的 [單一登入設定]**** 頁面套用至 SAML 權杖。

如需詳細資訊，請參閱[如何針對 Azure AD 中的企業應用程式自訂 SAML 權杖中發出的宣告](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>經過更新的 SAML 型應用程式設定 UI (預覽)

**類型：** 已變更的功能  
**服務類別：** 企業應用  
**產品功能：** SSO

在經過更新的 SAML 型應用程式設定 UI 中，您會獲得：

- 經過更新的 SAML 型應用程式設定逐步解說體驗。

- 更加了解設定中少了什麼或有何錯誤。

- 能夠新增多個用於到期憑證通知的電子郵件地址。

- 新的宣告轉換方法 ToLower() 和 ToUpper() 等等。

- 有辦法為企業應用程式上傳您自己的權杖簽署憑證。

- 有辦法設定 SAML 應用程式的 NameID 格式以及將 NameID 值設定為目錄擴充。

若要開啟此更新後的檢視，請從 [單一登入]**** 頁面頂端按一下 [試用新的體驗]**** 連結。 如需詳細資訊，請參閱[教學課程：為應用程式設定透過 Azure Active Directory 進行的 SAML 型單一登入](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications)。

---

## <a name="august-2018"></a>2018 年 8 月

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Azure Active Directory IP 位址範圍的變更

**類型：** 方案變更  
**服務類別：** 其他  
**產品功能：** 平台

我們正在將更大的 IP 範圍引入 Azure AD 中，這表示如果您已為您的防火牆、路由器或網路安全性群組設定 Azure AD IP 位址範圍，則必須進行更新。 我們會完成此更新，因此您不需要在 Azure AD 新增端點時，再次變更防火牆、路由器或網路安全群組的 IP 範圍組態。 

接下來的兩個月，網路流量會移至這些新的範圍。 若要繼續使用不中斷的服務，您必須在 2018 年 9 月 10 日之前，將這些更新值新增至您的 IP 位址：

- 20.190.128.0/18 

- 40.126.0.0/18 

我們強烈建議您，在所有網路流量都移至新的範圍之前，不要移除舊的 IP 位址範圍。 若要了解有關移動的更新及可移除舊範圍的時機，請參閱 [Office 365 URL 與 IP 位址範圍](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)。

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>變更通知：授權碼將不再供重複使用 

**類型：** 方案變更  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證

從 2018 年 11 月 15 日起，Azure AD 將不再接受將先前使用的驗證碼用於應用程式。 這項安全性變更有助於讓 Azure AD 與 OAuth 規格一致，且將會對 v1 和 v2 端點強制執行。

如果您的應用程式重複使用授權碼取得多個資源的權杖，建議您先使用授權碼取得重新整理權杖，再使用該重新整理權杖取得其他資源的額外權杖。 授權碼只能使用一次，但重新整理權杖可跨多個資源多次使用。 在 OAuth 程式碼流程期間嘗試重複使用驗證碼的應用程式，會收到 invalid_grant 錯誤。

針對這項變更以及其他與通訊協定相關的變更，請參閱[新驗證功能的完整清單](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes)。
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>適用於自助式密碼 (SSPR) 和 Multi-Factor Authentication ( MFA) 的融合式安全性資訊管理

**類型：** 新功能  
**服務類別：** SSPR  
**產品功能：** 使用者驗證
 
這項新功能可協助人員以單一位置和相同方式管理其 SSPR 和 MFA 的安全性資訊 (例如，電話號碼、行動應用程式等等)；不同於先前必須在兩個不同的位置管理。

此融合式體驗也適用於使用 SSPR 或 MFA 的人員。 此外，如果您的組織未強制執行 MFA 或 SSPR 註冊，人員仍可從「我的應用程式」入口網站註冊您的組織所允許的任何 MFA 或 SSPR 安全性資訊方法。

這是可選擇加入的公開預覽。 系統管理員可以為租用戶中選定的群組或所有使用者開啟新體驗 (如有需要)。 如需融合式體驗的詳細資訊，請參閱[融合式體驗部落格](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Azure AD 應用程式 Proxy 應用程式中新的僅限 HTTP Cookie 設定

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

您的應用程式 Proxy 應用程式中有新的設定，名為「僅限 HTTP Cookies」****。 此設定有助於提供更高的安全性，因為它可在應用程式 Proxy 存取和工作階段 Cookie 的 HTTP 回應標頭中加入 HTTPOnly 旗標，以禁止透過用戶端指令碼存取 Cookie，並進一步防止複製或修改 Cookie 之類的動作。 儘管此標誌以前未使用過，但您的 Cookie 始終通過 TLS 連接進行加密和傳輸，以説明防止不當修改。

此設定與使用 ActiveX 控制項的應用程式不相容，例如遠端桌面。 面臨此情況時，建議您關閉這項設定。

如需「僅限 HTTP Cookie」設定的詳細資訊，請參閱[使用 Azure AD 應用程式 Proxy 發佈應用程式](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal)。

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>適用於 Azure 資源的 Privileged Identity Management (PIM) 支援「管理群組」資源類型

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
Just-In-Time 的啟用和指派設定現已可套用至「管理群組」資源類型，一如您對訂用帳戶、資源群組和資源 (例如 VM、App Service 等) 所做的一般。 此外，任何人只要具備為管理群組提供系統管理員存取權的角色，都可以在 PIM 中探索及管理該項資源。

如需 PIM 和 Azure 資源的詳細資訊，請參閱[使用 Privileged Identity Management 探索與管理 Azure 資源](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>「應用程式存取」(預覽) 可加快存取 Azure AD 入口網站的速度

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
目前，在啟用使用 PIM 的角色時，權限可能要 10 分鐘以上才會生效。 如果您選擇使用目前處於公開預覽狀態的「應用程式存取」，系統管理員將可在啟用要求完成後隨即存取 Azure AD 入口網站。

目前，「應用程式存取」僅支援 Azure AD 入口網站體驗和 Azure 資源。 如需 PIM 和「應用程式存取」的詳細資訊，請參閱[什麼是 Azure AD Privileged Identity Management？](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Azure AD 應用程式資源庫中有新的同盟應用程式可用 - 2018 年 8 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 8 月將下列這 16 個提供同盟支援的全新應用程式新增至應用程式資源庫：

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial)、[Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial)、[Sauce Labs - 行動和 Web 測試](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial)、[Meta Networks Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial)、[Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial)、[Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial)、[ProMaster (by Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial)、SchoolBooking、[4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial)、[Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial)、[N2F - Expense reports](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial)、[Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial)、[SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial)、[ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial)、[eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial)、[Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial)。

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Azure AD 應用程式 Proxy 現已提供原生 Tableau 支援

**類型：** 已變更的功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制

在我們將預先驗證通訊協定從 OpenID Connect 更新為 OAuth 2.0 Code Grant 通訊協定後，您無須再進行任何額外的設定，即可搭配使用 Tableau 與應用程式 Proxy。 此一通訊協定變更也有助於應用程式 Proxy 更妥善地支援更現代化的應用程式，因為此後只需使用 HTTP 重新導向即可，而這在 JavaScript 和 HTML 標記中通常都可受到支援。

如需與我們的 Tableau 原生支援有關的詳細資訊，請參閱 [Azure AD 應用程式 Proxy 現已具備原生 Tableau 支援](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support)。

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>在 Azure Active Directory (預覽) 中新增 Google 供 B2B 來賓使用者作為識別提供者的新支援

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

藉由在組織中設定與 Google 的同盟，您可以讓受邀的 Gmail 使用者使用其現有的 Google 帳戶登入您的共用應用程式和資源，而不需要建立個人 Microsoft 帳戶 (MSA) 或 Azure AD 帳戶。

這是可選擇加入的公開預覽。 如需 Google 同盟的詳細資訊，請參閱[將 Google 新增為 B2B 來賓使用者的識別提供者](https://docs.microsoft.com/azure/active-directory/b2b/google-federation)。

---

## <a name="july-2018"></a>2018 年 7 月

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Azure Active Directory 電子郵件通知的改進功能

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 身分識別生命週期管理
 
Azure Active Directory (Azure AD) 電子郵件現在提供更新的設計，以及從下列服務傳送時，對寄件者電子郵件地址和寄件者顯示名稱進行的變更：
 
- Azure AD 存取權檢閱
- Azure AD Connect Health 
- Azure AD Identity Protection 
- Azure AD 特殊權限身分識別管理
- 企業應用程式過期憑證通知
- 企業應用程式佈建服務通知
 
電子郵件通知將會從下列電子郵件地址和顯示名稱傳送：

- 電子郵件地址：azure-noreply@microsoft.com
- 顯示名稱：Microsoft Azure
 
如需一些新電子郵件設計的範例和詳細資訊，請參閱 [Azure AD PIM 中的電子郵件通知](https://go.microsoft.com/fwlink/?linkid=2005832) \(英文\)。

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Azure AD 活動記錄現已可透過 Azure 監視器取得

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

Azure AD 活動記錄現在已可在 Azure 監視器的公開預覽 (Azure 的全平台監視服務) 中取得。 除了這些改善，Azure 監視器還會為您提供長期保留與無縫整合：

- 藉由將記錄檔路由傳送到您自己的 Azure 儲存體帳戶來進行的長期保留。

- 無縫的 SIEM 整合，而不需您撰寫或維護自訂指令碼。

- 與您自己的自訂解決方案、分析工具或事件管理解決方案進行的無縫整合。

如需這些新功能的詳細資訊，請參閱我們的部落格 [Azure 監視器診斷中的 Azure AD 活動記錄目前處於公開預覽狀態](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) \(英文\) 與 [Azure 監視器 (預覽) 中的 Azure Active Directory 活動記錄](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview)文件。

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>添加到 Azure AD 登錄報表的條件訪問資訊

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 身份安全&保護
 
此更新可讓您查看當使用者登入時要評估哪些原則以及原則結果。 此外，報告現在包含使用者所使用的用戶端應用程式類型，讓您可以識別舊版通訊協定流量。 現在也會針對相互關聯識別碼搜尋報告項目，其可在使用者所看到的錯誤訊息中找到，而且可用來識別相符的登入要求並進行疑難排解。

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>透過登入活動記錄來檢視舊版驗證

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告
 
隨著登入活動記錄中 **Client App** 欄位的導入，客戶現在可看見使用舊版驗證的使用者。 客戶將能夠使用登錄 Microsoft 圖形 API 或通過 Azure AD 門戶中的登錄活動日誌訪問此資訊，您可以使用**用戶端應用**控制項篩選舊版身份驗證。 如需更多詳細資料，請查看文件。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Azure AD 應用程式庫中現在提供新的同盟應用程式 - 2018 年 7 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 7 月將下列這 16 個提供同盟支援的全新應用程式新增至應用程式庫：

[Innovation Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial)、[Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial)、[Certain Admin SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial)、PSUC Staging、[iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial)、[Screencast-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial)、PowerSchool Unified Classroom、[Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial)、[Bomgar Remote Support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial)、[Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial)、[Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial)、[Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial)、[SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial)、[Kanbanize](../saas-apps/kanbanize-tutorial.md)、[SmartLPA](../saas-apps/smartlpa-tutorial.md)、[Skills Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://aka.ms/azureadapprequest)。

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>新的使用者佈建 SaaS 應用程式整合 - 2018 年 7 月

**類型：** 新功能  
**服務類別：** 應用程式佈建  
**產品功能：** 協力廠商整合
 
Azure AD 可讓您自動化在 SaaS 應用程式 (例如 Dropbox、Salesforce、ServiceNow 等等) 中建立、維護和移除使用者身分識別的作業。 我們已在 2018 年 7 月，於 Azure AD 應用程式庫中，為以下應用程式新增了使用者佈建支援：

- [思科 WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [獎金](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

如需 Azure AD 資源庫中支援使用者佈建的所有應用程式清單，請參閱 [SaaS 應用程式與 Azure Active Directory 的整合](https://aka.ms/appstutorial)。

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>適用於同步處理的 Connect Health - 這個更簡單的方式可用來修正孤立和重複屬性同步處理錯誤

**類型：** 新功能  
**服務類別：** AD Connect  
**產品功能：** 監視和報告
 
Azure AD Connect Health 導入了自助式補救，以協助您醒目提示並修正同步處理錯誤。 此功能會對重複屬性同步處理錯誤進行疑難排解，並修正來自 Azure AD 的孤立物件。 此診斷具有下列好處：

- 縮減重複屬性同步處理錯誤，提供特定的修正

- 針對專用的 Azure AD 案例套用修正，在單一步驟中解決錯誤

- 無須升級或設定即可啟用開啟並使用此功能

如需詳細資訊，請參閱[對重複屬性同步處理錯誤進行診斷和修復](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors)

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>對於 Azure AD 和 MSA 登入體驗的視覺效果更新

**類型：** 已變更的功能  
**服務類別：** Azure AD  
**產品功能：** 使用者驗證

我們已更新 Microsoft 線上服務登入體驗的 UI，例如針對 Office 365 與 Azure。 此變更讓畫面變得更簡潔且更直接。 如需此變更的詳細資訊，請參閱[即將推出的 Azure AD 登入體驗改善](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) \(英文\) 部落格。

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Azure AD Connect 的新版本 - 2018 年 7 月

**類型：** 已變更的功能  
**服務類別：** 應用程式佈建  
**產品功能：** 身份生命週期管理

Azure AD Connect 的最新版本包括： 

- 錯誤 (bug) 修正和可支援性更新 

- Ping Federate 整合已正式運作

- 最新 SQL 2012 用戶端的更新 

如需此更新的詳細資訊，請參閱 [Azure AD Connect︰版本發行歷程記錄](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-end-user-ui"></a>最終使用者 UI 的使用條款的更新

**類型：** 已變更的功能  
**服務類別：** 使用條款  
**產品功能：** 治理

我們正在更新 TOU 終端使用者 UI 中的接受字串。

**當前文本。** 若要存取 [tenantName] 資源，您必須接受使用規定。<br>**新文字。** 若要存取 [tenantName] 資源，您必須閱讀使用規定。

**目前的文字：** 選擇接受即表示您接受上述所有使用規定。<br>**新文字：** 請按一下 [接受] 以確認您已閱讀並了解使用規定。

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>傳遞驗證支援舊版通訊協定和應用程式

**類型：** 已變更的功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
傳遞驗證現在支援舊版通訊協定和應用程式。 現在完全支援下列限制：

- 使用者登入舊版 Office 用戶端應用程式 Office 2010 和 Office 2013，而不需新式驗證。

- 僅在 Exchange 混合式環境中的 Office 2010 上存取行事曆共用和空閒/忙碌資訊。

- 使用者不需新式驗證，即可登入商務用 Skype 用戶端應用程式。

- 使用者登入 PowerShell 1.0 版。

- 使用 iOS 安裝小幫手的 Apple 裝置註冊計劃 (Apple DEP)。 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>適用於自助式密碼重設及 Multi-Factor Authentication 的融合式安全性資訊管理

**類型：** 新功能  
**服務類別：** SSPR  
**產品功能：** 使用者驗證

這項新功能可讓使用者在單一體驗中，針對自助式密碼重設 (SSPR) 與 Multi-Factor Authentication (MFA) 管理其安全性資訊 (例如，電話號碼、電子郵件地址、行動裝置應用程式等等)。 使用者將不再需要在兩個不同體驗中，針對 SSPR 和 MFA 註冊相同的安全性資訊。 這個新體驗也適用於具有 SSPR 或 MFA 的使用者。

如果組織未強制執行 MFA 或 SSPR 註冊，使用者就能透過 **My Apps** 入口網站註冊他們的安全性資訊。 使用者可以從這裡註冊已針對 MFA 或 SSPR 啟用的任何方法。 

這是可選擇加入的公開預覽。 系統管理員可以針對一組選取的使用者或租用戶中的所有使用者開啟新體驗 (如有需要)。

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>當您重設密碼時，請使用 Microsoft Authenticator 應用程式來驗證您的身分識別

**類型：** 已變更的功能  
**服務類別：** SSPR  
**產品功能：** 使用者驗證

此功能可讓非系統管理員在使用來自 Microsoft Authenticator (或任何其他驗證器應用程式) 的通知或代碼重設密碼時驗證他們的身分識別。 在系統管理員開啟這個自助式密碼重設方法之後，已透過 aka.ms/mfasetup aka.ms/setupsecurityinfo 註冊行動裝置應用程式的使用者就能在重設密碼時，使用其行動裝置應用程式作為驗證方法。

行動裝置應用程式通知可以只開啟來作為需要兩個方法來重設密碼之原則的一部分。

---

## <a name="june-2018"></a>2018 年 6 月

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>變更注意事項：對使用 Azure AD 活動記錄 API 的應用程式委派授權流程進行安全性修正

**類型：** 方案變更  
**服務類別：** 報告  
**產品功能：** 監視和報告

由於我們更強的安全強制，我們不得不更改使用委派授權流訪問[Azure AD 活動日誌 API](https://aka.ms/aadreportsapi)的應用的許可權。 這項變更會在 **2018 年 6 月 26 日**生效。

如果任何應用使用 Azure AD 活動日誌 API，請按照以下步驟操作，以確保應用在更改發生後不會中斷。

**更新您的應用程式權限**

1. 登入 Azure 入口網站、選取 [Azure Active Directory]****，然後選取 [應用程式註冊]****。
2. 選取使用 Azure AD 活動記錄 API 的應用程式、選取 [設定]****、選取 [必要權限]****，然後選取 [Windows Azure Active Directory]**** API。
3. 在 [啟用存取]**** 刀鋒視窗的 [委派權限]**** 區域中，選取 [讀取目錄]**** 資料旁的方塊，然後選取 [儲存]****。
4. 選取 [授與權限]****，然後選取 [是]****。
    
    >[!Note]
    >您必須是全域管理員，才能授與權限給應用程式。

如需詳細資訊，請參閱必要條件的 [授與權限](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) 區域來存取 Azure AD 報告 API 文章。

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>將 TLS 設定為連線至 Azure AD 服務，以符合 PCI DSS 合規性

**類型：** 新功能  
**服務類別：** N/A  
**產品功能：** 平台

傳輸層安全性 (TLS) 是提供兩個通訊應用程式之間隱私權和資料完整性的通訊協定，是目前所使用最廣泛部署的安全性通訊協定。

[PCI 安全性標準委員會](https://www.pcisecuritystandards.org/)判定 TLS 和安全通訊端層 (SSL) 的早期版本必須停用，以利啟用更安全的全新應用程式通訊協定，此合規性自 **2018 年 6 月 30 日**起開始。 這項變更意味著如果連線到 Azure AD 服務並要求符合 PCI DSS，就必須停用 TLS 1.0。 TLS 有許多版本可用，但 TLS 1.2 是 Azure Active Directory 服務最新的可用版本。 若是結合使用用戶端/伺服器及瀏覽器/伺服器，強烈建議您直接使用 TLS 1.2。

過時的瀏覽器可能不支援較新的 TLS 版本，例如 TLS 1.2。 若要查看瀏覽器支援的 TLS 版本，請前往 [Qualys SSL 實驗室](https://www.ssllabs.com/)網站並按一下 [測試您的瀏覽器]****。 建議您升級到最新版本的網頁瀏覽器，最好只啟用 TLS 1.2。

**透過瀏覽器啟用 TLS 1.2**

- **Microsoft Edge 和 Internet Explorer (這兩者都使用 Internet Explorer 設定)**

    1. 打開互聯網瀏覽器，選擇**工具** > **互聯網選項** > **高級**。
    2. 在 [安全性]**** 區域中，選取 [使用 TLS 1.2]****，然後選取 [確定]****。
    3. 關閉所有瀏覽器視窗，並重新啟動 Internet Explorer。 

- **Google Chrome**

    1. 開啟 Google Chrome、在位址列輸入 *chrome://settings/*，然後按 **Enter** 鍵。
    2. 展開 [進階]**** 選項、前往 [系統]**** 區域，然後選取 [開啟 Proxy 設定]****。
    3. 在 [網際網路內容]**** 方塊中，選取 [進階]**** 索引標籤、前往 [安全性]**** 區域、選取 [使用 TLS 1.2]****，然後選取 [確定]****。
    4. 關閉所有瀏覽器視窗，並重新啟動 Google Chrome。

- **Mozilla Firefox**

    1. 開啟 Firefox、在位址列輸入 *about:config*，然後按 **Enter** 鍵。
    2. 搜尋字詞 *TLS*，然後選取 [security.tls.version.max]**** 項目。
    3. 將值設為 **3** 以強制瀏覽器使用最新版本 TLS 1.2，然後選取 [確定]****。

        >[!NOTE]
        >Firefox 60.0 版本支援 TLS 1.3，因此也可以將 security.tls.version.max 值設為 **4**。

    4. 關閉所有瀏覽器視窗，並重新啟動 Mozilla Firefox。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Azure AD 應用程式庫中有新的同盟應用程式可用 - 2018 年 6 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們已在 2018 年 6 月將下列這 15 個提供同盟支援的全新應用程式新增至應用程式庫：

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial)、[Settling music](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial)、[SAML 1.1 Token enabled LOB App](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial)、[Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial)、[Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial)、[Endpoint Backup](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial)、[Skyhigh Networks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial)、Smartway2、[TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial)、[Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial)、[Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial)、[SharePoint on-premises](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial)、[ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial)、[Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial)、[ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。 如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>Azure AD 密碼保護可在公開預覽時使用

**類型：** 新功能  
**服務類別：** 身份保護  
**產品功能：** 使用者驗證

使用 Azure AD 密碼保護可協助排除所在環境中容易猜到的密碼。 排除這些密碼可協助降低密碼噴灑 (password spray) 類型攻擊危害的風險。

具體來說，Azure AD 密碼保護可協助您：

- 保護 Azure AD 和 Windows Server Active Directory (AD) 中的組織帳戶。 
- 避免使用者使用超過 500 個最常用密碼清單中的密碼，以及這些密碼超過 100 萬個字元的替換變化。
- 針對 Azure AD 和內部部署 Windows Server AD，從 Azure AD 入口網站中的單一位置管理 Azure AD 密碼保護。

如需 Azure AD 密碼保護的詳細資訊，請參閱[排除組織中的不當密碼](https://aka.ms/aadpasswordprotectiondocs)。

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-creation"></a>在使用條款創建期間創建的新"所有來賓"條件訪問策略範本

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 治理

在創建使用條款期間，還會為"所有來賓"和"所有應用"創建新的條件訪問策略範本。 這個全新原則範本適用於新建立的 ToU，簡化了來賓建立和強制程序。

如需詳細資訊，請參閱 [Azure Active Directory 使用規定功能](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-creation"></a>在創建使用條款期間創建的新"自訂"條件訪問策略範本

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 治理

在創建使用條款期間，還會創建新的"自訂"條件訪問策略範本。 此新策略範本允許您創建 ToU，然後立即轉到條件訪問策略創建邊欄選項卡，而無需手動流覽門戶。

如需詳細資訊，請參閱 [Azure Active Directory 使用規定功能](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>關於部署 Azure Multi-Factor Authentication 的全新完整指導

**類型：** 新功能  
**服務類別：** 其他  
**產品功能：** 身份安全&保護
 
我們針對如何在組織中部署 Azure Multi-Factor Authentication (MFA) 推出了全新的逐步指導。

如需檢視 MFA 部署指南，請前往 GitHub 上的[識別部署指南](https://aka.ms/DeploymentPlans)存放庫。 如需提供有關部署指南的意見反應，請使用[部署方案意見反應單](https://aka.ms/deploymentplanfeedback)。 如果對於部署指南有任何疑問，請於 [IDGitDeploy](mailto:idgitdeploy@microsoft.com) 聯絡我們。

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Azure AD 委派的應用程式管理角色位於公開預覽

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 存取控制

管理員現在可以委派應用程式管理工作，而無須指派全域管理員角色。 全新角色和功能包括：

- **全新的標準 Azure AD 管理員角色：**

    - **應用程式管理員。** 授予管理所有應用程式各方面的權限，包括註冊、SSO 設定、應用程式指派和授權、應用程式 Proxy 設定及同意 (Azure AD 資源除外)。

    - **雲應用程式管理員。** 授予所有應用程式系統管理員的能力，但應用程式 Proxy 除外，因為應用程式 Proxy 不提供內部存取。

    - **應用程式開發人員。** 授予建立應用程式註冊的權限，即使已關閉 [允許使用者註冊應用程式]**** 選項亦同。

- **擁有權 (設定每個應用程式註冊與每個企業應用程式，類似群組擁有權的程序：**
 
    - **應用程式註冊擁有者。** 授予管理自有應用程式註冊各方面之權限，包括應用程式資訊清單，以及新增其他擁有者。

    - **企業應用程式擁有者。** 授予管理自有企業應用程式許多方面的權限，包括 SSO 設定、應用程式指派及同意 (Azure AD 資源除外)。

如需公開預覽的詳細資訊，請參閱 [Azure AD 委派的應用程式管理角色位於公開預覽！](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) 部落格。 有關角色和許可權的詳細資訊，請參閱在[Azure 活動目錄中分配管理員角色](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal)。

---

## <a name="may-2018"></a>2018 年 5 月

### <a name="expressroute-support-changes"></a>ExpressRoute 支援變更

**類型：** 方案變更  
**服務類別：** 驗證 (登入)  
**產品功能：** 平台  

Azure Active Directory (Azure AD) 等這類 SaaS 供應項目旨在設計為適合直接透過網際網路使用，而不需要 ExpressRoute 或任何其他私用 VPN 通道。 因為這個緣故，在 **2018 年 8 月 1 日**，我們會對使用 Azure 公用對等互連和在 Microsoft 對等互連中使用 Azure 社群的 Azure AD 服務，停止支援 ExpressRoute。 受此變更影響的任何服務可能會注意到 Azure AD 流量逐漸從 ExpressRoute 移到網際網路。

雖然即將變更支援，但我們也知道在有些狀況下，您仍需要對驗證流量使用一組特定線路。 因為這個緣故，Azure AD 將會繼續支援使用 ExpressRoute 的每個租用戶 IP 範圍限制，以及針對包含「其他 Office 365 線上服務」社群的 Microsoft 對等互連，支援其中已存在的服務。 如果服務會受到影響，但您需要 ExpressRoute，則必須執行下列作業：

- **如果您位在 Azure 公用對等互連上。** 移至 Microsoft 對等互連，並註冊**其他的 Office 365 線上服務 (12076:5100)** 社群。 如需如何從 Azure 公用對等互連移至 Microsoft 對等互連的詳細資訊，請參閱[將公用對等互連移至 Microsoft 對等互連](https://docs.microsoft.com/azure/expressroute/how-to-move-peering)一文。

- **如果您在 Microsoft 對等互連上。** 註冊**其他 Office 365 線上服務 (12076:5100)** 社群。 如需路由需求的詳細資訊，請參閱 ExpressRoute 路由需求文章中的 [BGP 社群支援區段](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp)。

如果您必須繼續使用特定線路，必須與您的 Microsoft 帳戶小組討論，了解如何取得使用**其他 Office 365 線上服務 (12076:5100)** 社群的授權。 由 MS Office 管理的檢閱面板會確認您是否需要這些線路，並確定您了解保留這些線路的技術含意。 未經授權的訂用帳戶若嘗試建立 Office 365 路由篩選，將會收到錯誤訊息。 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph API 適用於 TOU 的系統管理案例

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 開發人員體驗
 
我們添加了用於管理 Azure AD 使用條款的 Microsoft 圖形 API。 您可以創建、更新、刪除使用物件。

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>新增 Azure AD 多租用戶端點作為 Azure AD B2C 中的識別提供者

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C
 
您現在可以使用自訂原則，新增 Azure AD 通用端點作為 Azure AD B2C 中的識別提供者。 這使您能夠為登入您應用程式的所有 Azure AD 使用者提供單一進入點。 如需詳細資訊，請參閱 [Azure Active Directory B2C：允許使用者使用自訂原則登入多租用戶 Azure AD 識別提供者](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom)。

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>使用內部 URL，透過我們的 My Apps 登入延伸模組與 Azure AD 應用程式 Proxy 從任何地方存取應用程式

**類型：** 新功能  
**服務類別：** 我的應用程式  
**產品功能：** SSO
 
使用者現在即使在公司網路外，也能使用適用於 Azure AD 的 My Apps Secure Sign-in 延伸模組，透過內部 URL 存取應用程式。 這適用於您使用 Azure AD 應用程式 Proxy 發行的所有應用程式，以及亦安裝存取面板瀏覽器延伸模組的所有瀏覽器。 URL 重新導向功能會在使用者登入延伸模組後自動啟用。 此延伸模組可在 [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176)、[Chrome](https://go.microsoft.com/fwlink/?linkid=866367) 及 [Firefox](https://go.microsoft.com/fwlink/?linkid=866366) 下載。

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory - 歐洲客戶在歐洲中的資料

**類型：** 新功能  
**服務類別：** 其他  
**產品功能：** GoLocal

歐洲客戶必須將其資料保留在歐洲，而不得於歐洲資料中心外部複寫，以期符合隱私權與歐洲法律。 [本文](https://go.microsoft.com/fwlink/?linkid=872328)提供了特定詳細資料，說明有哪些身分識別資訊會儲存在歐洲，並提供會儲存在歐洲資料中心外之資訊的詳細資料。 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>新的使用者佈建 SaaS 應用程式整合 - 2018 年 5 月

**類型：** 新功能  
**服務類別：** 應用程式佈建  
**產品功能：** 協力廠商整合
 
Azure AD 可讓您自動化在 SaaS 應用程式 (例如 Dropbox、Salesforce、ServiceNow 等等) 中建立、維護和移除使用者身分識別的作業。 我們在 2018 年 5 月，於 Azure AD 應用程式庫為以下應用程式新增了使用者佈建支援：

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [cornerstone ondemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

有關 Azure AD 庫中支援使用者預配的所有應用程式的清單，請參閱[https://aka.ms/appstutorial](https://aka.ms/appstutorial)。

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>群組和應用程式存取的 Azure AD 存取權檢閱現在提供定期檢閱

**類型：** 新功能  
**服務類別：** 訪問評論  
**產品功能：** 治理
 
Azure AD Premium P2 中現已正式提供群組和應用程式的存取權檢閱功能。  系統管理員能夠設定群組成員資格的存取權檢閱以及應用程式指派，使其在固定時間間隔重複發生，例如每月或每季。

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Azure AD 活動記錄 (登入與稽核) 現可透過 MS Graph 使用

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告
 
Azure AD 活動日誌（包括登錄日誌和稽核記錄）現在可通過 Microsoft 圖形 API 提供。 我們通過 Microsoft 圖形 API 公開了兩個端點來訪問這些日誌。 請查看我們的[文件](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)，了解如何開始以程式設計方式存取 Azure AD 報告 API。 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>改善 B2B 兌換體驗，以及離開組織

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

**及時兌換：** 使用 B2B API 與來賓使用者共用資源後，無需發送特殊邀請電子郵件。 在大多數情況下，來賓使用者可存取資源，並且會即時帶過兌換體驗。 錯過的電子郵件不會產生任何影響。 不再詢問您的訪客使用者"您是否點擊了系統向您發送的兌換連結？ 這表示只要 SPO 使用了邀請管理員，所有使用者都能得到相同標準的雲端附件 URL，不論使用者處於內外部，也不論其兌換狀態。

**新式兌換體驗：** 兌換登陸頁面不再分割。 使用者會看到新式的同意體驗，包含組織隱私權聲明的邀請，這與第三方應用程式的做法相似。

**來賓使用者可以離開組織：** 一旦使用者與組織的關係結束，他們可以自行離開組織。 不再調用邀請組織管理員"刪除"，不再提高支援票證。

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Azure AD 應用程式庫中有新的同盟應用程式可用 - 2018 年 5 月

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合
 
我們在 2018 年 5 月將下列這 18 個提供同盟支援的全新應用程式新增至應用程式庫：

[獎勵斯普林](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial)， Infogix Data360治理，[約德克](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial)， [Jamf Pro，](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial)[知識貓頭鷹](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial)，[環境MMIS，](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial)[啟動黑暗，](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial) [Adobe Captivate Prime，](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial)[蒙太奇線上](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial)， [*](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial)* * ， OpenReel， [Arc 出版 - SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial)，[計畫網格](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial)， [iWellnessnow，](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial)[代理點擊](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial)，[風險軟體](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial)， [Flock，](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial)[評論快照](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。

如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>全新 Azure Active Directory 逐步部署指南

**類型：** 新功能  
**服務類別：** 其他  
**產品功能：** 目錄
 
有關如何部署 Azure 活動目錄 （Azure AD） 的新分步指南，包括自助服務密碼重設 （SSPR）、單一登入 （SSO）、條件訪問 （CA）、應用代理、使用者預配、活動目錄聯合服務 （ADFS） 到直通身份驗證 （PTA） 和 ADFS 到密碼雜湊同步 （PHS）。

如需檢視部署指南，請前往 GitHub 上的[識別部署指南](https://aka.ms/DeploymentPlans)存放庫。 如需提供有關部署指南的意見反應，請使用[部署方案意見反應單](https://aka.ms/deploymentplanfeedback)。 如果對於部署指南有任何疑問，請於 [IDGitDeploy](mailto:idgitdeploy@microsoft.com) 聯絡我們。

---

### <a name="enterprise-applications-search---load-more-apps"></a>企業應用程式搜尋 - 載入更多應用程式

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO
 
尋找您的應用程式/服務主體時發生問題嗎？ 我們已新增讓您載入企業應用程式清單中更多應用程式的功能。 根據預設，我們會顯示 20 項應用程式。 您現在可以按一下"**載入更多**"以查看其他應用程式。 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>五月發行的 AADConnect 包含與 PingFederate 之整合、重要安全性更新、許多 Bug 修正和全新絕佳疑難排解工具的公開預覽。 

**類型：** 已變更的功能  
**服務類別：** AD Connect  
**產品功能：** 身份生命週期管理
 
五月發行的 AADConnect 包含與 PingFederate 之整合、重要安全性更新、許多 Bug 修正和全新絕佳疑難排解工具的公開預覽。 您可以在[這裡](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190)找到版本資訊。

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Azure AD 存取權檢閱：自動套用

**類型：** 已變更的功能  
**服務類別：** 訪問評論  
**產品功能：** 治理

Azure AD Premium P2 中現已正式提供群組和應用程式的存取權檢閱功能。 系統管理員可以設定為在存取權檢閱完成時，自動將檢閱者的變更套用至該群組或應用程式。 系統管理員也可以指定在檢閱者未回應、移除存取權、保留存取權或採取系統建議時，使用者繼續存取所會發生的動作。 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-response_mode-for-new-apps"></a>新的應用程式無法再使用 query response_mode 傳回 ID 權杖。 

**類型：** 已變更的功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
在 2018 年 4 月 25 日當天或之後所建立的應用程式，將無法再使用 **query** response_mode 來要求 **id_token**。  這會使 Azure AD 能與 OIDC 規格保持一致，協助減少您的應用程式受攻擊面。  在 2018/4/25 之前所建立的應用程式，可以使用 **query** response_mode 與 **id_token** 的 response_type。  從 AAD 請求id_token時返回的錯誤為**AADSTS70007："查詢"在請求權杖時不是"response_mode"的受支援值**。

在建立新應用程式物件時 (例如，應用程式 Proxy 使用方式)，**fragment** 與 **form_post** response_modes 會繼續生效，請務必在這些 response_modes 建立新應用程式前使用其中一個 response_modes。  

---
 
## <a name="april-2018"></a>2018 年 4 月 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Azure AD B2C 存取權杖已正式推出

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C 

您現在可以使用存取權杖來存取受 Azure AD B2C 保護的 Web API。 這項功能將從公開預覽移至正式推出 (GA)。 設定 Azure AD B2C 應用程式和 Web API 的 UI 經驗已改進，並進行了其他小幅的改良。
 
如需詳細資訊，請參閱 [Azure AD B2C：要求存取權杖](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens)。

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>為 SAML 式應用程式測試單一登入設定

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO

在設定 SAML 型 SSO 應用程式時，您無法在設定頁面上測試整合。 如果您在登入時發生錯誤，您可以提供測試經驗中的錯誤，而 Azure AD 會提供給您解決特定問題的解決步驟。

如需詳細資訊，請參閱

- [設定對不在 Azure Active Directory 應用程式庫中的應用程式的單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [如何偵錯 SAML 型單一登入 Azure Active Directory 中的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Azure AD 的使用條款現在具有每個使用者報告

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 合規性
 
系統管理員現在可以選取特定 ToU，並查看同意該 ToU 的所有使用者以及其發生日期/時間。

如需詳細資訊，請參閱 [Azure AD 使用規定特性](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health：AD FS 外部網路鎖定保護的具風險 IP 

**類型：** 新功能  
**服務類別：** 其他  
**產品功能：** 監視和報告

Connect Health 現在能夠支援每小時或每日偵測超出失敗 U/P 登入閾值的 IP 位址。 此功能所提供的功能如下：

- 完整報告，其中顯示 IP 位址，以及每小時/每日產生的失敗登入數目 (可自訂閾值)。
- 電子郵件形式警示，其中顯示已超出每小時/每日失敗 U/P 登入閾值的特定 IP 位址。
- 用來執行詳細資料分析的下載選項

如需詳細資訊，請參閱[有風險的 IP 報告](https://aka.ms/aadchriskyip)。

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>輕鬆以中繼資料檔案或 URL 設定應用程式

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO

在企業應用程式頁面上，系統管理員可以上傳 SAML 中繼資料檔，以設定 AAD 資源庫和非資源庫應用程式的 SAML 型登入。

此外，您可以使用 Azure AD 應用程式同盟中繼資料 URL 來設定目標應用程式的 SSO。

如需詳細資訊，請參閱[設定對不在 Azure Active Directory 應用程式庫中的應用程式的單一登入](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)。

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Azure AD 使用規定現已正式推出

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 合規性
 

Azure AD 的使用條款已經從公共預覽版移動到一般可用。

如需詳細資訊，請參閱 [Azure AD 使用規定特性](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>允許或封鎖對特定組織的 B2B 使用者的邀請

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C
 

您現在可以指定想要共用的夥伴組織，以及在 Azure AD B2B 共同作業中與其共同作業。 若要這樣做，您可以選擇建立特定的允許或拒絕網域清單。 使用這些功能封鎖網域時，員工便無法再傳送邀請給該網域中的人員。

這可協助您控制資源的存取，同時讓已核准的使用者擁有平順的經驗。

此 B2B 協作功能適用于所有 Azure 活動目錄客戶，可與 Azure AD 高級功能（如條件訪問和標識保護）結合使用，以便更精細地控制外部企業用戶簽署時和方式並獲取存取權限。

如需詳細資訊，請參閱[允許或封鎖對特定組織的 B2B 使用者的邀請](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list)。

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD 應用程式庫中可用的新同盟應用程式

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

我們在 2018 年 4 月將下列這 13 個提供同盟支援的全新應用程式新增至應用程式庫：

Criterion HCM、[FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial)、[Secret Server (On-Premises)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial)、[Dynamic Signal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial)、[mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial)、[OrgChart Now](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial)、[AppNeta Performance Monitor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial)、[Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial)、[Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial)、[Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial)、Shelf、[SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。

如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>授與 Azure AD 中的 B2B 使用者您內部部署應用程式的存取權 (公開預覽)

**類型：** 新功能  
**服務類別：** B2B  
**產品功能：** B2B/B2C

作為使用 Azure Active Directory (Azure AD) B2B 共同作業功能邀請夥伴組織的來賓使用者進入您 Azure AD 的組織，您現在可以對這些 B2B 使用者提供內部部署應用程式的存取權。 這些內部部署應用程式可以搭配 Kerberos 限制委派 (KCD) 來使用 SAML 型驗證或整合式 Windows 驗證 (IWA)。

有關詳細資訊，請參閱在[Azure AD 中授予 B2B 使用者訪問本地應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises)。

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>從 Azure Marketplace 取得 SSO 整合教學課程

**類型：** 已變更的功能  
**服務類別：** 其他  
**產品功能：** 協力廠商整合

如果 [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1)中所列的應用程式支援 SAML 型單一登入，按一下**立即取得**即可為您提供與該應用程式相關聯的整合教學課程。 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Azure AD 自動將使用者佈建至 SaaS 應用程式的效能更快

**類型：** 已變更的功能  
**服務類別：** 應用程式佈建  
**產品功能：** 協力廠商整合
 
先前使用 SaaS 應用程式 (例如 Salesforce、ServiceNow 及 Box) 適用之 Azure Active Directory 使用者佈建連接器 的客戶，如果其 Azure AD 租用戶包含合計超過 100,000 個使用者和群組，而且使用使用者和群組指派來判斷應該佈建哪些使用者，則可能會遭遇低落的效能。

在 2018 年 4 月 2 日，Azure AD 佈建服務部署了非常顯著的效能增強功能，可大幅減少在 Azure Active Directory 與目標 SaaS 應用程式之間執行首次同步處理所需的時間量。

因此，許多曾經花許多天或未曾完成應用程式首次同步處理的客戶，現在只需要幾分鐘或幾小時數就能完成首次同步處理。

如需詳細資訊，請參閱[佈建期間會發生什麼事？](/azure//active-directory/app-provisioning/how-provisioning-works)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>從已加入混合式 Azure AD 之電腦的 Windows 10 鎖定畫面進行自助式密碼重設

**類型：** 已變更的功能  
**服務類別：** 自助式密碼重設  
**產品功能：** 使用者驗證
 
我們已更新 Windows 10 SSPR 功能，進而包含已加入混合式 Azure AD 的電腦支援。 這項功能適用於 Windows 10 RS4，可讓使用者從 Windows 10 電腦的鎖定畫面重設其密碼。 已啟用且註冊自助式密碼重設的使用者可以利用這項功能。

如需詳細資訊，請參閱[從登入畫面重設 Azure AD 密碼](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows)。

---

## <a name="march-2018"></a>2018 年 3 月
 
### <a name="certificate-expire-notification"></a>憑證到期通知

**類型：** 固定  
**服務類別：** 企業應用  
**產品功能：** SSO
 
當資源庫或非資源庫應用程式即將到期時，Azure AD 會傳送通知。 

針對 SAML 型單一登入設定的企業應用程式，有些使用者未收到相關通知。 此問題已解決。 若憑證將在 7、30 和 60 天內到期，Azure AD 就會傳送通知。 您可以在稽核記錄中看到此事件。 

如需詳細資訊，請參閱

- [在 Azure Active Directory 中管理同盟單一登入的憑證](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Azure Active Directory 入口網站中的稽核活動報告](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Azure AD B2C 中的 Twitter 與 GitHub 識別提供者

**類型：** 新功能  
**服務類別：** B2C - 取用者身分識別管理  
**產品功能：** B2B/B2C
 
您現在能以身分識別提供者的身分在 Azure AD B2C 中新增 Twitter 或 GitHub。 Twitter 將從公開預覽進行至 GA。 GitHub 即將在公開預覽中發行。

如需詳細資訊，請參閱[什麼是 Azure AD B2B 共同作業？](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)。
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>使用 Intune 託管瀏覽器限制瀏覽器訪問，使用基於 Azure AD 應用程式的條件訪問進行 iOS 和 Android 訪問

**類型：** 新功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全&保護
 
**現已在公開預覽版中推出！**

**Intune Managed Browser SSO：** 您的員工可以在各個原生用戶端 (例如 Microsoft Outlook) 上使用單一登入，以及針對所有與 Azure AD 連接的應用程式使用 Intune Managed Browser。

**Intune 託管瀏覽器條件訪問支援：** 現在，您可以要求員工使用基於應用程式的條件訪問策略使用 Intune 託管瀏覽器。

如需深入了解，請參閱我們的[部落格文章](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/) \(英文\)。

如需詳細資訊，請參閱

- [設置基於應用程式的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [設定 Managed Browser 原則](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>PowerShell GA 模組中的應用程式代理 Cmdlet

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 存取控制
 
支援應用程式代理 Cmdlet 現在位於 PowerShell GA 模組中！ 這確實需要您在 PowerShell 模組上保持更新 - 如果您落後一年多，一些 Cmdlet 可能會停止工作。 

如需詳細資訊，請參閱 [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0)。
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>無縫式 SSO 是使用非互動式通訊協定來支援 Office 365 原生用戶端

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
使用 Office 365 原生用戶端 (版本 16.0.8730.xxxx 和更新版本) 的使用者在使用無縫式 SSO 時，能獲得無訊息登入體驗。 能有此項支援是因為已對 Azure AD 新增非互動式通訊協定 (WS-Trust)。

有關詳細資訊，請參閱[使用無縫 SSO 的本機用戶端上的登錄如何工作？](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>如果應用程式將登入要求傳送至 Azure AD 的租用戶端點，使用者便可在使用無縫式 SSO 時獲得無訊息登入體驗

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
如果應用程式 (例如，`https://contoso.sharepoint.com`) 將登入要求傳送至 Azure AD 的租用戶端點 - 亦即 `https://login.microsoftonline.com/contoso.com/<..>` 或 `https://login.microsoftonline.com/<tenant_ID>/<..>` - 而不是 Azure AD 的一般端點 (`https://login.microsoftonline.com/common/<...>`)，使用者便可在使用無縫式 SSO 時獲得無訊息登入體驗。

有關詳細資訊，請參閱[Azure 活動目錄無縫單一登入](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)。 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>只需要將一個 Azure AD URL，而不是先前的兩個 URL 新增至使用者的內部網路區域設定，即可推出無縫式 SSO

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
若要對使用者推出無縫式 SSO，您必須使用 Active Directory 中的群組原則，僅將一個 Azure AD URL 新增至使用者的內部網路區域設定：`https://autologon.microsoftazuread-sso.com`。 過去，客戶必須新增兩個 URL。

有關詳細資訊，請參閱[Azure 活動目錄無縫單一登入](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)。 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD 應用程式庫中可用的新同盟應用程式

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

我們在 2018 年 3 月將下列這 15 個提供同盟支援的全新應用程式新增至應用程式庫：

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial)、[CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial)、Wrike、[SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial)、Assistant by FirstAgenda、[YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial)、Vtiger CRM、Inwink、[Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial)、[Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial)、[ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial)、[Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial)、[Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial)、[Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial)、[Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial)。
 
如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。

如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>Azure 資源的 PIM 已正式推出

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
如果您將 Azure AD Privileged Identity Management 用於目錄角色，現在可以將 PIM 的時間限制存取和指派功能用於 Azure 資源角色，例如訂用帳戶、資源群組、虛擬機器和 Azure Resource Manager 支援的任何其他資源。 啟用角色 Just-In-Time 時，強制執行 Multi-Factor Authentication，並搭配核准的變更時間範圍排定啟用。 此外，此版本新增公開預覽期間未提供的增強功能，包括更新的 UI、核准工作流程，並且能夠延長即將到期的角色和更新過期的角色。

如需詳細資訊，請參閱 [適用於 Azure 資源的 PIM (預覽)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>將選用宣告新增至您的應用程式權杖 (公開預覽)

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
您的 Azure AD 應用程式現在可以在 JWT 或 SAML 權杖中要求自訂或選用宣告。  這些有關使用者或租用戶的宣告並未預設包含在權杖中，因為有大小或適用性方面的條件約束。  這目前在 1.0 和 2.0 版端點上的 Azure AD 應用程式中處於公開預覽階段。  請參閱文件，以了解可新增的宣告以及如何編輯應用程式資訊清單來要求這些宣告。  

如需詳細資訊，請參閱 [Azure AD 中的選用宣告](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims)。
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD 支援 PKCE 以獲得更安全的 OAuth 流程

**類型：** 新功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
已更新 Azure AD 文件以發佈 PKCE 的支援，PKCE 可讓 OAuth 2.0 授權碼授與流程期間的通訊更安全。  1.0 和 2.0 版端點上同時支援 S256 與純文字 code_challenges。 

有關詳細資訊，請參閱[請求授權代碼](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code)。 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-get_workers-api"></a>Workday Get_Workers API 中提供佈建所有使用者屬性值的支援

**類型：** 新功能  
**服務類別：** 應用程式佈建  
**產品功能：** 協力廠商整合
 
輸入佈建 (從 Workday 至 Active Directory 和 Azure AD) 的公開預覽現在支援擷取和佈建 Workday Get_Workers API 中可用的所有屬性值。 在隨附於 Workday 輸入佈建連接器初始版本的屬性之外，這會新增數百個其他標準和自訂屬性的支援。

如需詳細資訊，請參閱：[自訂 Workday 使用者屬性的清單](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>將群組成員資格從動態變更為靜態，反之亦然

**類型：** 新功能  
**服務類別：** 群組管理  
**產品功能：** 共同作業
 
您可以變更在群組中管理成員資格的方式。 當您想要在系統中保留相同的群組名稱和識別碼，讓任何現有的群組參考仍然有效時，這非常實用；建立新的群組需要更新這些參考。
我們已更新 Azure AD 系統管理中心，以支援這項功能。 現在，客戶可以將現有的群組從動態成員資格轉換為指派的成員資格，反之亦然。 現有的 PowerShell Cmdlet 還是可以使用。

有關詳細資訊，請參閱[Azure 活動目錄中組的動態成員資格規則](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>透過無縫式 SSO 改進登出行為

**類型：** 已變更的功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者驗證
 
在過去，即使使用者明確登出受 Azure AD 保護的應用程式，但他們如果嘗試在其公司網路內從已加入網域的裝置再次存取 Azure AD 應用程式，系統就會使用無縫式 SSO 自動將他們登入。 而這項變更會用來支援登出。  這可讓使用者選擇使用相同或不同的 Azure AD 帳戶來進行登入，而不是使用「無縫 SSO」來自動登入。

如需詳細資訊，請參閱 [Azure Active Directory 無縫單一登入](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso)
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>應用程式 Proxy 連接器 1.5.402.0 版已發行

**類型：** 已變更的功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 身份安全&保護
 
此連接器版本會在 11 月前逐漸推出。 這個新的連接器版本包含下列變更：

- 連接器現在會設定網域層級而非子網域層級的 cookie。 這可確保較順暢的 SSO 體驗，並避免多餘的驗證提示。
- 支援區塊編碼要求
- 改良的連接器健康情況監視 
- 多項 錯誤 (bug) 修正及穩定性改進

如需詳細資訊，請參閱[了解 Azure AD 應用程式 Proxy 連接器](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors)。
 
---

## <a name="february-2018"></a>2018 年 2 月
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>改進使用者和群組的管理瀏覽體驗

**類型：** 方案變更  
**服務類別：** 目錄管理  
**產品功能：** 目錄

使用者和群組的管理瀏覽體驗已經簡化。 您現在可以從目錄概觀直接瀏覽至所有使用者清單，且更容易存取已刪除的使用者清單。 您也可以從目錄概觀直接瀏覽至所有群組清單，且更容易存取群組管理設定。 此外，從目錄概觀頁面，您還可搜尋使用者、群組、企業應用程式或應用程式註冊。 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>21Vianet (Azure 中國 21Vianet) 所營運的 Microsoft Azure 提供登入和稽核報告

**類型：** 新功能  
**服務類別：** Azure Stack  
**產品功能：** 監視和報告

21Vianet (Azure 中國 21Vianet) 執行個體所營運的 Microsoft Azure 現已提供 Azure AD 活動記錄報告。 以下是包含的記錄：

- **登入活動記錄** - 包括與您租用戶相關的所有登入記錄。

- **自助密碼稽核記錄** - 包括所有 SSPR 稽核記錄。

- **目錄管理稽核記錄** - 包括所有目錄管理相關的稽核記錄，例如使用者管理、應用程式管理等。

藉由這些記錄，您便可深入了解您環境的運作情況。 提供的資料可讓您：

- 判斷使用者如何利用您的應用程式和服務。

- 針對會阻止使用者完成其工作的問題進行疑難排解。

如需有關如何使用這些報告的詳細資訊，請參閱 [Azure Active Directory 報告](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)。

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>使用「報告讀取者」角色 (非管理員角色) 來檢視 Azure AD 活動報告

**類型：** 新功能  
**服務類別：** 報告  
**產品功能：** 監視和報告

作為客戶回函的一部分，使非管理員角色能夠訪問 Azure AD 活動日誌，我們啟用了處於"報表讀取器"角色的使用者訪問 Azure 門戶中的登錄和審核活動以及使用 Microsoft 圖形的能力Api。 

如需有關如何使用這些報告的詳細資訊，請參閱 [Azure Active Directory 報告](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal)。 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>提供 EmployeeID 宣告作為使用者屬性和使用者識別碼

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** SSO
 
您可以從企業應用程式 UI 設定 **EmployeeID**，作為 SAML 登入應用程式中成員使用者和 B2B 來賓的使用者識別碼和使用者屬性。

如需詳細資訊，請參閱[針對 Azure Active Directory 中的企業應用程式自訂 SAML 權杖中發出的宣告](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)。

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>在 Azure AD 應用程式 Proxy 中使用萬用字元來簡化應用程式管理

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 使用者驗證
 
為了讓應用程式部署更簡單並減輕您的系統管理額外負荷，我們現已支援使用萬用字元來發佈應用程式的功能。 若要發佈萬用字元應用程式，您可以依照標準的應用程式發佈流程，但在內部和外部 URL 中使用萬用字元。

如需詳細資訊，請參閱 [Azure Active Directory 應用程式 Proxy 中的萬用字元應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard)

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>可支援設定應用程式 Proxy 的新 Cmdlet

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 平台

最新版的「AzureAD PowerShell 預覽」模組包含可讓客戶使用 PowerShell 來設定「應用程式 Proxy 應用程式」的新 Cmdlet。

這些新 Cmdlet 包括： 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>可支援設定群組的新 Cmdlet

**類型：** 新功能  
**服務類別：** 應用程式 Proxy  
**產品功能：** 平台

最新版的 AzureAD PowerShell 模組包含可管理 Azure AD 中群組的 Cmdlet。 這些 Cmdlet 先前是在 AzureADPreview 模組中提供的，現已新增到 AzureAD 模組中

現已發行而可正式運作的 Group Cmdlet 包括： 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>提供新版 Azure AD Connect

**類型：** 新功能  
**服務類別：** AD 同步  
**產品功能：** 平台
 
Azure AD Connect 是在 Azure AD 與內部部署資料來源 (包括 Windows Server Active Directory 和 LDAP) 之間同步處理資料時，建議使用的工具。

>[!Important]
>這個組建導入了結構描述和同步處理規則變更。 「Azure AD Connect 同步處理服務」會在升級後觸發「完整匯入」和「完整同步處理」步驟。 如需有關如何變更此行為的資訊，請參閱[如何延遲升級之後的完整同步處理](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade)。

此版本包含下列更新和變更：

**已修復問題**

- 修正計時視窗切換至下一個頁面時，在 [資料分割篩選] 頁面背景工作的問題。

- 修正在 ConfigDB 自訂動作期間造成存取違規的錯誤 (bug)。

- 修正從 SQL 復原時連線逾時的錯誤 (bug)。

- 修正含 SAN 萬用字元的憑證在進行必要條件檢查時失敗的錯誤 (bug)。

- 修正造成 miiserver.exe 在 AAD 連接器匯出期間當機的錯誤 (bug)。

- 修正當執行動作造成 AAD Connect 精靈變更設定時，錯誤密碼嘗試會記錄在 DC 上的錯誤 (bug)

**新功能和改進**
 
- 應用程式遙測 - 系統管理員可以開啟/關閉此類別的資料。

- Azure AD 健康情況資料 - 系統管理員必須瀏覽健康情況入口網站，才能控制其健康情況設定。 一旦變更服務原則，代理程式會讀取並強制執行它。

- 新增裝置回寫設定動作和頁面初始化的進度列。

- 藉由 HTML 報告改善一般診斷並以 ZIP 文字/HTML 報告提供完整資料收集。

- 改善自動升級的可靠性，並新增其他遙測，以確保您可以判斷伺服器的健康情況。

- 限制具特殊權限帳戶對 AD 連接器帳戶的可用權限。 針對新的安裝，精靈會在建立 MSOL 帳戶之後，限制具特殊權限帳戶對 MSOL 帳戶的權限。 這些變更會影響「自動建立」帳戶的相關快速安裝和自訂安裝。

- 將安裝程式變更為在進行 AADConnect 全新安裝時不需要 SA 權限。

- 可針對特定物件的同步處理問題進行疑難排解的新公用程式。 目前，此公用程式會檢查下列各項：

    - 同步處理的使用者物件與 Azure AD 租用戶中使用者帳戶的 UserPrincipalName 不符。
  
    - 是否因為網域篩選而從同步處理篩選物件
  
    - 是否因為組織單位 (OU) 篩選而從同步處理篩選物件

- 可針對特定使用者帳戶同步處理儲存在內部部署 Active Directory 中之目前密碼雜湊的新公用程式。 此公用程式不需要變更密碼。 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>支援 Intune 應用保護原則的應用程式添加，以便與基於 Azure AD 應用程式的條件訪問一起使用

**類型：** 已變更的功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全&保護

我們添加了更多支援基於應用程式的條件訪問的應用程式。 現在，您可以使用這些已核准的用戶端應用程式，來存取 Office 365 及其他已連線至 Azure AD 的雲端應用程式。

以下是會在二月底前新增的應用程式：

- Microsoft Power BI

- Microsoft Launcher

- Microsoft Invoicing

如需詳細資訊，請參閱

- [已核准的用戶端應用程式需求](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [基於 Azure AD 應用的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>移動體驗的使用條款更新 

**類型：** 已變更的功能  
**服務類別：** 使用條款  
**產品功能：** 合規性

顯示使用條款時，現在可以按一下"**查看時遇到問題？"點擊這裡**。 按一下此連結會在您裝置上以原生方式開啟使用規定。 不論文件中的字型大小或裝置的螢幕大小為何，您都可以視需要縮放並閱讀文件。 

---
 
## <a name="january-2018"></a>2018 年 1 月
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Azure AD 應用程式庫中可用的新同盟應用程式 

**類型：** 新功能  
**服務類別：** 企業應用  
**產品功能：** 協力廠商整合

在 2018 年 1 月，於應用程式庫中新增了下列支援同盟的新應用程式：

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698)、[OneTrust Privacy Management Software](https://go.microsoft.com/fwlink/?linkid=861660)、[Dealpath](https://go.microsoft.com/fwlink/?linkid=863526)、[IriusRisk Federated Directory 及 [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701)。

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。

如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>偵測到有額外風險的登入

**類型：** 新功能  
**服務類別：** 身份保護  
**產品功能：** 身份安全&保護

檢測到的風險檢測的見解與 Azure AD 訂閱相關聯。 使用 Azure AD Premium P2 版本時，您會獲得有關所有基礎偵測的最詳細資訊。

使用 Azure AD 高級 P1 版本，許可證未涵蓋的檢測顯示為風險檢測登錄，並檢測到其他風險。

如需詳細資訊，請參閱 [Azure Active Directory 風險偵測](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events)。
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>從使用者存取面板中隱藏 Office 365 應用程式

**類型：** 新功能  
**服務類別：** 我的應用程式  
**產品功能：** SSO

您現在可以透過新的使用者設定，以更好的方式管理 Office 365 應用程式在使用者存取面板上的顯示方式。 如果您偏好只在 Office 入口網站中顯示 Office 應用程式，此選項將有助於減少使用者存取面板中的應用程式數量。 此設定位於 [使用者設定]**** 中，並且標示為 [使用者只能在 Office 365 入口網站中看見 Office 365 應用程式]****。

如需詳細資訊，請參閱[從 Azure Active Directory 的使用者體驗中隱藏應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)。

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>啟用無縫應用程式登入功能，可直接從應用程式的 URL 進行密碼 SSO 

**類型：** 新功能  
**服務類別：** 我的應用程式  
**產品功能：** SSO

現在透過便利的工具即可使用 [我的應用程式] 瀏覽器擴充功能，此工具會以捷徑的形式在您的瀏覽器中提供 [我的應用程式] 單一登入功能。 安裝之後，使用者將會在其瀏覽器中看到一個鬆餅圖示，可讓使用者快速存取應用程式。 使用者現在可以利用：

- 從應用的登錄頁面直接登錄到基於密碼 SSO 的應用的功能
- 使用快速搜尋功能來啟動任何應用程式
- 擴充功能所提供的最近使用過的應用程式捷徑
- 此擴充功能適用於 Microsoft Edge、Chrome 及 Firefox。
 
如需詳細資訊，請參閱[我的應用程式安全登入擴充功能](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension)。

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Azure 傳統入口網站中的 Azure AD 系統管理體驗已淘汰

**類型：** 已被取代   
**服務類別：** Azure AD  
**產品功能：** 目錄

Azure 傳統入口網站中的 Azure AD 系統管理體驗自 2018 年 1 月 8 日起已淘汰。 Azure 傳統入口網站本身也隨之一起淘汰。 日後您應該使用 [Azure AD 系統管理中心](https://aad.portal.azure.com)來進行所有以入口網站為基礎的 Azure AD 系統管理。
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>PhoneFactor Web 入口網站已淘汰

**類型：** 已被取代  
**服務類別：** Azure AD  
**產品功能：** 目錄
 
PhoneFactor Web 入口網站自 2018 年 1 月 8 日起已淘汰。 此入口網站是用來進行 MFA 伺服器的系統管理，但這些功能已移至位於 portal.azure.com 的 Azure 入口網站。 

MFA 設定位於：**Azure Active Directory \> MFA Server**
 
---
 
### <a name="deprecate-azure-ad-reports"></a>取代 Azure AD 報告

**類型：** 已被取代  
**服務類別：** 報告  
**產品功能：** 身份生命週期管理  


在新的「Azure Active Directory 系統管理」主控台正式運作且現在已有適用於活動和安全性報告的新 API 之後，"/reports" 端點底下的報告 API 自 2017 年 12 月 31 日結束起已淘汰。

**可用的內容？**

在轉換至新系統管理主控台的過程中，我們提供了 2 個新 API 來擷取 Azure AD 活動記錄。 新的 API 集合除了能提供更豐富的稽核和登入活動外，還提供更豐富的篩選和排序功能。 以前通過安全報告提供的資料現在可以通過 Microsoft Graph 中的標識保護風險檢測 API 進行訪問。

如需詳細資訊，請參閱

- [開始使用 Azure Active Directory 報告 API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [開始使用 Azure Active Directory Identity Protection 和 Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>2017 年 12 月

### <a name="terms-of-use-in-the-access-panel"></a>存取面板中的使用規定

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 合規性
 
您現在可以移至「存取面板」來檢視先前所接受的使用規定。

請遵循下列步驟：

1. 前往 [MyApp 入口網站](https://myapps.microsoft.com)並登入。

2. 在右上角中，選取您的名稱，然後從清單中選取 [設定檔]****。 

3. 在您的 [設定檔]**** 上，選取 [檢閱使用規定]****。 

4. 現在您可以檢閱您已接受的使用規定。 

如需詳細資訊，請參閱 [Azure AD 使用規定特性 (預覽)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>新的 Azure AD 登入體驗

**類型：** 新功能  
**服務類別：** Azure AD  
**產品功能：** 使用者身份驗證
 
Azure AD 和 Microsoft 帳戶身分識別系統 UI 已經過重新設計，具有一致的外觀與風格。 此外，Azure AD 登入頁面會先收集使用者名稱，然後在第二個畫面收集認證。

如需詳細資訊，請參閱[新的 Azure AD 登入體驗 (目前為公開預覽)](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) \(英文\)。
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>較少的登入提示次數：適用於 Azure AD 登入的新「讓我保持登入」體驗

**類型：** 新功能  
**服務類別：** Azure AD  
**產品功能：** 使用者身份驗證
 
Azure AD 登入頁面上的 [讓我保持登入]**** 核取方塊已被新的提示取代，此提示會在您成功驗證之後出現。 

如果您對此提示的回應是 [是]****，服務就會提供您一個持續性的重新整理權杖。 此行為與在舊體驗中選取 [讓我保持登入]**** 核取方塊相同。 針對同盟租用戶，此提示會在您已成功向同盟服務驗證之後顯示。

如需詳細資訊，請參閱[較少的登入提示次數：適用於 Azure AD 登入的新「讓我保持登入」體驗 (預覽)](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) \(英文\)。 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>新增設定以要求在接受使用規定前先展開使用規定

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 合規性
 
一個系統管理員選項，可要求其使用者在接受使用規定之前，先展開該規定。

請選取 [開啟]**** 或 [關閉]**** 來要求使用者展開使用規定。 [開啟]**** 設定會要求使用者必須先檢視使用規定，才能接受該規定。

如需詳細資訊，請參閱 [Azure AD 使用規定特性 (預覽)](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>適用於合格角色指派的限域啟用

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
您可以使用限域啟用，以比原始指派預設值更少的自主性來啟用合格的 Azure 資源角色指派。 例如，假設您獲指派為租用戶中訂用帳戶的擁有者。 在使用限域啟用的情況下，您最多可以針對該訂用帳戶內包含的 5 個資源 (例如資源群組與虛擬機器) 啟用擁有者角色。 限制啟用範圍可降低對重要 Azure 資源執行不需要之變更的可能性。

如需詳細資訊，請參閱[什麼是 Azure AD Privileged Identity Management？](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure)。
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Azure AD App 資源庫中的新同盟應用程式

**類型：** 新功能  
**服務類別：** 企業應用程式  
**產品功能：** 協力廠商整合

我們在 2017 年 12 月將下列這些提供同盟支援的全新應用程式新增至應用程式庫：

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523) \(機器翻譯\)、Adobe Experience Manager、[EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685) \(機器翻譯\)、[Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe \(機器翻譯\)、[FactSet](https://go.microsoft.com/fwlink/?linkid=863525) \(機器翻譯\)、[IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517) \(機器翻譯\)、[MOBI](https://go.microsoft.com/fwlink/?linkid=863521)[MobileIron Azure AD 整合](https://go.microsoft.com/fwlink/?linkid=858027) \(機器翻譯\)、[Reflektive](https://go.microsoft.com/fwlink/?linkid=863518) \(機器翻譯\)、[SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520) \(機器翻譯\)、[SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519)[Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522) \(機器翻譯\)、WebHR、Zenegy Azure AD 整合。

如需應用程式的詳細資訊，請參閱[與 Azure Active Directory 整合的 SaaS 應用程式](https://aka.ms/appstutorial)。

如需在 Azure AD 應用程式庫中列出應用程式的詳細資訊，請參閱[在 Azure Active Directory 應用程式庫中列出您的應用程式](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing)。 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Azure AD 目錄角色的核准工作流程

**類型：** 已變更的功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
Azure AD 目錄角色的核准工作流程已正式運作。

使用核准工作流程時，特殊權限角色系統管理員可以要求合格角色成員必須先要求角色啟用，才能使用特殊權限角色。 可以將核准責任委派給多個使用者和群組。 合格角色成員會在核准完成且其角色生效時收到通知。

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>傳遞驗證：商務用 Skype 支援

**類型：** 已變更的功能  
**服務類別：** 驗證 (登入)  
**產品功能：** 使用者身份驗證

傳遞驗證現在支援使用者登入可支援新式驗證 (包括線上和混合式拓撲) 的「商務用 Skype」用戶端應用程式。 

如需詳細資訊，請參閱[以新式驗證支援的商務用 Skype 技術](https://technet.microsoft.com/library/mt803262.aspx) \(英文\)。
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>適用於 Azure RBAC 的 Azure AD Privileged Identity Management 更新 (預覽)

**類型：** 已變更的功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management
 
有了適用於 Azure 角色型存取控制 (RBAC) 的 Azure AD Privileged Identity Management (PIM) 公開預覽更新，您現在可以：

* 使用恰到好處的系統管理。
* 要求必須經過核准才能啟用資源角色。
* 排定要求必須經過核准才能取得 Azure AD 和 Azure RBAC 角色的未來角色啟用。
 
如需詳細資訊，請參閱[適用於 Azure 資源的 Privileged Identity Management (預覽)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac) \(機器翻譯\)。

---
 
## <a name="november-2017"></a>2017 年 11 月
 
### <a name="access-control-service-retirement"></a>存取控制服務淘汰

**類型：** 方案變更  
**服務類別：** 存取控制服務  
**產品功能：** 存取控制服務 

Azure Active Directory 存取控制 (也稱為「存取控制」服務) 將於 2018 年底淘汰。 在未來幾週，我們將會提供包括詳細排程和概要移轉指引的詳細資訊。 您可以在此頁面留言詢問有關「存取控制」服務的任何問題，小組成員將會回答您的留言。

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>限制瀏覽器對 Intune Managed Browser 的存取 

**類型：** 方案變更  
**服務類別：** 條件式存取  
**產品功能：** 身份安全和保護

您可以藉由使用 Intune Managed Browser 作為已核准的應用程式，限制瀏覽器對 Office 365 和其他已連線至 Azure AD 之雲端應用程式的存取。 

現在，您可以為基於應用程式的條件訪問配置以下條件：

**用戶端應用：** 瀏覽器

**變更的影響為何？**

目前，當您使用此條件時，系統會封鎖存取。 當預覽已可供使用時，所有存取都會要求使用 Managed Browser 應用程式。 

請在即將推出的部落格和版本資訊中，查看此功能及更多資訊。 

有關詳細資訊，請參閱[Azure AD 中的條件訪問](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)。
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>基於 Azure AD 應用的條件訪問的新已批准用戶端應用

**類型：** 方案變更  
**服務類別：** 條件式存取  
**產品功能：** 身份安全和保護

下列應用程式已列入[已核准的用戶端應用程式](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)清單：

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- Microsoft StaffHub

如需詳細資訊，請參閱

- [已核准的用戶端應用程式需求](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [基於 Azure AD 應用的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>針對多種語言的使用規定支援

**類型：** 新功能    
**服務類別：** 使用條款  
**產品功能：** 合規性

系統管理員現在可以建立包含多個 PDF 文件的新使用規定。 您能以相對應的語言標記這些 PDF 文件。 系統會根據使用者的喜好設定，顯示相符語言的 PDF。 如果沒有相符項目，則會顯示預設語言。

---
 
### <a name="real-time-password-writeback-client-status"></a>即時密碼回寫用戶端狀態

**類型：** 新功能  
**服務類別**自助式密碼重設  
**產品功能：** 使用者身份驗證

您現在可以檢閱內部部署密碼回寫用戶端的狀態。 此選項可於 [[密碼重設]](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset) 頁面的 [內部部署整合]**** 區段中取得。 

如果您在連線至內部部署回寫用戶端時遇到問題，將會看見包含下列資訊的錯誤訊息：

- 有關無法連線至內部部署回寫用戶端之原因的資訊。
- 可協助您解決該問題之文件的連結。 

有關詳細資訊，請參閱[本地集成](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration)。

---

### <a name="azure-ad-app-based-conditional-access"></a>基於 Azure AD 應用的條件訪問 
 
**類型：** 新功能  
**服務類別：** Azure AD  
**產品功能：** 身份安全和保護

現在，您可以使用[基於 Azure AD 應用的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)，將對 Office 365 和其他 Azure AD 連接的雲應用的訪問限制為支援 Intune 應用保護原則[的已批准用戶端應用](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)。 系統會使用 Intune 應用程式防護原則，來設定及保護這些用戶端應用程式上的公司資料。

通過將[基於應用](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)的條件訪問策略與[基於設備](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications)的條件訪問策略相結合，您可以靈活地保護個人和公司設備的資料。

以下條件和控制項現在可用於基於應用的條件訪問：

**支援的平台條件**

- iOS
- Android

**用戶端應用程式條件**

- 行動裝置應用程式和桌面用戶端

**存取控制**

- 需要已核准的用戶端應用程式

有關詳細資訊，請參閱基於[Azure AD 應用的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)。
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>在 Azure 入口網站中管理 Azure AD 裝置

**類型：** 新功能  
**服務類別：** 設備註冊和管理  
**產品功能：** 身份安全和保護

您現在只要在一個位置，就可以找到所有已連線至 Azure AD 的裝置，以及與裝置相關的活動。 現已推出在 Azure 入口網站中管理所有裝置身分識別和設定的新系統管理體驗。 在這個版本中，您可以︰

- 查看 Azure AD 中可用於條件訪問的所有設備。
- 檢視屬性，包括您的混合式已加入 Azure AD 的裝置。
- 尋找已加入 Azure AD 之裝置的 BitLocker 金鑰、透過 Intune 管理裝置等等。
- 管理 Azure AD 裝置相關的設定。

如需詳細資訊，請參閱[使用 Azure 入口網站來管理裝置](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal)。

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>支援 macOS 作為 Azure AD 條件訪問的設備平臺 

**類型：** 新功能    
**服務類別：** 條件式存取  
**產品功能：** 身份安全和保護 

現在，您可以在 Azure AD 條件訪問策略中包括（或排除）macOS 作為設備平臺條件。 除了將 macOS 新增為支援的裝置平台之外，您也可以：

- **使用 Intune 來註冊和管理 macOS 裝置。** 與其他平台 (例如 iOS 和 Android) 類似，有可供 macOS 用來進行統一註冊的公司入口網站應用程式。 您可以使用適用於 macOS 的新公司入口網站應用程式，向 Intune 註冊裝置，並向 Azure AD 註冊它。
- **確保 macOS 裝置遵守組織在 Intune 中定義的合規性原則。** 您現在可以在 Azure 入口網站上的 Intune 中，設定適用於 macOS 裝置的合規性原則。 
- **限制只有符合規範的 macOS 裝置才能存取 Azure AD 中的應用程式。** 條件訪問策略創作將 macOS 作為單獨的設備平臺選項。 現在，您可以為 Azure 中的目標應用程式集編寫特定于 macOS 的條件訪問策略。

如需詳細資訊，請參閱

- [使用 Intune 為 macOS 裝置建立裝置合規性原則](https://aka.ms/macoscompliancepolicy)
- [Azure AD 中的條件訪問](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>適用於 Azure Multi-Factor Authentication 的網路原則伺服器擴充功能 

**類型：** 新功能    
**服務類別：** 多重要素驗證  
**產品功能：** 使用者身份驗證

適用於 Azure Multi-Factor Authentication 的「網路原則伺服器」擴充功能會藉由使用現有的伺服器，將雲端式 Multi-Factor Authentication 功能新增至您的驗證基礎結構。 有了「網路原則伺服器」擴充功能，您便可以在現有的驗證流程中新增通話、簡訊或電話應用程式驗證。 您無須安裝、設定及維護新的伺服器。 

這個擴充功能是針對想要保護虛擬私人網路連線但又不想部署 Azure Multi-Factor Authentication Server 的組織所建立。 「網路原則伺服器」擴充功能會作為 RADIUS 與雲端式 Azure Multi-Factor Authentication 之間的配接器，可為同盟或同步的使用者提供第二驗證要素。

如需詳細資訊，請參閱[將現有的網路原則伺服器基礎結構與 Azure Multi-Factor Authentication 整合](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension)。
 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>還原或永久移除已刪除的使用者

**類型：** 新功能    
**服務類別：** 使用者管理  
**產品功能：** 目錄 

在 Azure AD 系統管理中心中，您現在可以：

- 還原已刪除的使用者。 
- 永久刪除使用者。

**試用此功能：**

1. 在 Azure AD 管理中心中，在 **"管理**"部分中選擇[所有使用者](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All)。 

2. 從 [顯示]**** 清單，選取 [最近刪除的使用者]****。 

3. 選取一或多個最近刪除的使用者，然後將這些使用者還原或是永久刪除。
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>基於 Azure AD 應用的條件訪問的新已批准用戶端應用
 
**類型：** 已變更的功能  
**服務類別：** 條件式存取  
**產品功能：** 身份安全和保護

下列應用程式已新增至[已核准的用戶端應用程式](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)清單：

- Microsoft Planner
- Azure 資訊保護 

如需詳細資訊，請參閱

- [已核准的用戶端應用程式需求](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-conditions#client-apps-preview)
- [基於 Azure AD 應用的條件訪問](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>在條件訪問策略中的控制項之間使用"OR" 

**類型：** 已變更的功能    
**服務類別：** 條件式存取  
**產品功能：** 身份安全和保護
 
現在，您可以將"OR"（需要其中一個選定控制項）用於條件訪問控制項。 您可以使用此功能建立在存取控制項之間具有 "OR" 的原則。 例如，您可以使用此功能來建立原則，以要求使用者使用 Multi-Factor Authentication 進行登入，「或」使用符合規範的裝置進行登入。

有關詳細資訊，請參閱[Azure AD 條件訪問 中的控制項](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls)。
 
---

### <a name="aggregation-of-real-time-risk-detections"></a>即時風險檢測的聚合

**類型：** 已變更的功能    
**服務類別：** 身份保護  
**產品功能：** 身份安全和保護

在 Azure AD 標識保護中，現在針對每種風險檢測類型聚合源自給定日相同 IP 位址的所有即時風險檢測。 此更改限制了顯示的風險檢測量，而使用者安全性沒有任何變化。

基礎即時偵測仍然會在使用者每次登入時運作。 如果您已設定登入風險安全性原則以進行 Multi-Factor Authentication 或封鎖存取，在每次發生具風險的登入時，仍然會觸發該原則。
 
---
 
## <a name="october-2017"></a>2017 年 10 月

### <a name="deprecate-azure-ad-reports"></a>取代 Azure AD 報告

**類型：** 方案變更  
**服務類別：** 報告  
**產品功能：** 身份生命週期管理  

Azure 入口網站可讓您使用：

- 新的 Azure AD 管理主控台。
- 活動和安全報告的新 API。
 
由於有這些新功能，因此 /reports 端點下的報告 API 已在 2017 年 12 月 10 日停用。 

---

### <a name="automatic-sign-in-field-detection"></a>自動登入欄位偵測

**類型：** 固定   
**服務類別：** 我的應用程式  
**產品功能：** 單一登入  

Azure AD 針對呈現 HTML 使用者名稱和密碼欄位的應用程式，支援自動登入欄位偵測。 [如何自動擷取應用程式的登入欄位](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-password-single-sign-on-non-gallery-applications-problems#manually-capture-sign-in-fields-for-an-app)記錄下列步驟。 您可以在藉由在 [Azure 入口網站](https://aad.portal.azure.com) 的 [企業應用程式]**** 頁面上新增 [非資源庫]** 應用程式來尋找此功能。 此外，您也可以在此新應用程式上將 [單一登入]**** 模式設定為 [密碼單一登入]****，輸入 Web URL，然後再儲存頁面。
 
之前因為服務問題，而將此功能暫時停用。 此問題已解決，因此已恢復自動登入欄位偵測功能。

---

### <a name="new-multi-factor-authentication-features"></a>新的 Multi-Factor Authentication 功能

**類型：** 新功能  
**服務類別：** 多重要素驗證  
**產品功能：** 身份安全和保護  

多重要素驗證 (MFA) 是保護您組織不可或缺的環節。 為了使認證更具彈性，並讓使用體驗更加順暢，我們已新增下列功能： 

- 將多重要素挑戰結果直接整合至 Azure AD 登入報告中，包括以程式設計方式存取 MFA 的結果。
- 將 MFA 設定更深入地整合至 Azure 入口網站的 Azure AD 設定體驗中。

在此公開預覽中，MFA 管理和報告功能是核心 Azure AD 設定體驗相當重要的一部分。 現在您可以在 Azure AD 體驗內管理 MFA 管理入口網站功能。

如需詳細資訊，請參閱[在 Azure 入口網站中 MFA 報告的參考](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa)。 

---

### <a name="terms-of-use"></a>使用規定

**類型：** 新功能  
**服務類別：** 使用條款  
**產品功能：** 合規性  

您可以使用 Azure AD 使用規定向使用者顯示資訊，例如法務或合規性需求的相關免責聲明。

在下列案例中，您可以使用 Azure AD 使用規定：

- 適用於貴組織中所有使用者的一般使用規定
- 依使用者屬性 (例如，醫生與護士或國內員工與國際員工 (藉由動態群組的方式完成)) 區分的特定使用規定
- 適用於存取高影響力商務應用程式 (例如 Salesforce) 的特定使用規定

如需詳細資訊，請參閱 [Azure AD 使用規定](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use)。

---

### <a name="enhancements-to-privileged-identity-management"></a>針對 Privileged Identity Management 的增強功能

**類型：** 新功能  
**服務類別：** Privileged Identity Management  
**產品功能：** Privileged Identity Management  

有了 Azure AD Privileged Identity Management，您便可以管理、控制及監視下列項目對組織內 Azure 資源 (預覽) 的存取行為：

- 訂用帳戶
- 資源群組
- 虛擬機器 

所有在 Azure 入口網站內利用 Azure RBAC 功能的資源，都可以運用 Azure AD Privileged Identity Management 所提供的安全性和生命週期管理功能。

如需詳細資訊，請參閱[適用於 Azure 資源的 Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac)。

---

### <a name="access-reviews"></a>存取權檢閱

**類型：** 新功能  
**服務類別：** 存取權檢閱  
**產品功能：** 合規性  

組織可以使用存取權檢閱 (預覽) 來有效率地管理群組成員資格，以及對企業應用程式的存取權： 

- 您可以重新認證來賓使用者存取權，方法為使用應用程式的存取權和群組的成員資格的存取權檢閱。 檢閱者可以根據存取權檢閱所提供的深入解析，有效率地決定是否要允許來賓繼續存取。
- 您可以利用存取權檢閱，重新認證員工對應用程式的存取權和群組成員資格。

您可以將存取權檢閱控制項收集到與組織相關的程式，以追蹤合規性或與風險相關之應用程式的檢閱。

如需詳細資訊，請參閱 [Azure AD 存取權檢閱](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview)。

---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>從 [我的應用程式] 和 Office 365 應用程式啟動器隱藏協力廠商應用程式

**類型：** 新功能  
**服務類別：** 我的應用程式  
**產品功能：** 單一登入  

您現在可以透過新的**隱藏應用程式**屬性，以更好的方式管理在使用者入口網站上顯示的應用程式。 當出現後端服務的應用程式圖格或重複的圖格，而造成使用者的應用程式啟動器凌亂時，您可以隱藏應用程式來協助改善此情況。 該切換參數位於協力廠商應用程式的 [屬性]**** 區段，標示為 [是否要向使用者顯示?]**** 您也可以透過 PowerShell 以程式設計方式隱藏應用程式。 

如需詳細資訊，請參閱[從 Azure AD 的使用者體驗中隱藏協力廠商應用程式](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)。 


**可用的內容？**

 在轉換至新系統管理主控台的過程中，有兩個可擷取 Azure AD 活動記錄的新 API 可供使用。 新的 API 集合除了能提供更豐富的稽核和登入活動外，還提供更豐富的篩選和排序功能。 以前通過安全報告提供的資料現在可以通過 Microsoft Graph 中的標識保護風險檢測 API 進行訪問。


## <a name="september-2017"></a>2017 年 9 月

### <a name="hotfix-for-identity-manager"></a>適用於 Identity Manager 的 Hotfix

**類型：** 已變更的功能  
**服務類別：** Identity Manager  
**產品功能：** 身分識別生命週期管理  

Identity Manager 2016 Service Pack 1 的 Hotfix 彙總套件 (組建 4.4.1642.0) 已自 2017 年 9 月 25 日起可供使用。 此彙總套件：

- 可解決問題並新增增強功能。
- 是一個累積更新，可取代到 Identity Manager 2016 組建 4.4.1459.0 為止的所有 Identity Manager 2016 Service Pack 1 更新。 
- 要求您必須具備 Identity Manager 2016 組建 4.4.1302.0。 

如需詳細資訊，請參閱 [適用於 Identity Manager 2016 Service Pack 1 的 Hotfix 彙總套件 (組建 4.4.1642.0) 已可供使用](https://support.microsoft.com/help/4021562) \(機器翻譯\)。 

---
