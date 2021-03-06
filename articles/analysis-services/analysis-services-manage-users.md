---
title: Azure Analysis Services 的驗證和使用者權限 | Microsoft Docs
description: 此文章說明 Azure Analysis Services 如何使用 Azure Active Directory (Azure AD) 進行身分識別管理和使用者驗證。
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 12/01/2020
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 551bae56565140da3754e74a23b1cc18087f1171
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/02/2020
ms.locfileid: "96487434"
---
# <a name="authentication-and-user-permissions"></a>驗證和使用者權限

Azure Analysis Services 會使用 Azure Active Directory (Azure AD) 進行身分識別管理和使用者驗證。 任何建立、管理或連線到 Azure Analysis Services 伺服器的使用者在相同的訂用帳戶中必須 [Azure AD 租用戶](../active-directory/fundamentals/active-directory-whatis.md)的有效使用者身分識別。

Azure Analysis Services 支援 [Azure AD B2B 共同作業](../active-directory/external-identities/what-is-b2b.md)。 透過 B2B，組織外部的使用者可以受邀成為 Azure AD 目錄中的來賓使用者。 來賓可以來自另一個 Azure AD 租用戶目錄或任何有效的電子郵件地址。 使用者一旦受邀並接受 Azure 透過電子郵件傳送的邀請，使用者身分識別就會新增至租用戶目錄。 這些身分識別可以新增至安全性群組，或成為伺服器管理員或資料庫角色的成員。

![Azure Analysis Services 驗證架構](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>驗證

所有用戶端應用程式和工具會使用一或多個 Analysis Services [用戶端程式庫](/analysis-services/client-libraries?view=azure-analysis-services-current&preserve-view=true) (AMO、MSOLAP、ADOMD) 連線到伺服器。 

這三個用戶端程式庫全都支援 Azure AD 互動式流程和非互動式驗證方法。 兩種非互動式方法 (Active Directory 密碼和 Active Directory 整合式驗證) 可以在利用 AMOMD 和 MSOLAP 的應用程式中使用。 這兩種方法絕對不會產生快顯對話方塊。

Excel 和 Power BI Desktop 等用戶端應用程式，以及 SSMS 和適用於 Visual Studio 的 Analysis Services 專案延伸模組等工具會在更新至最新版本時，安裝最新版的程式庫。 Power BI Desktop、SSMS 和 Analysis Services 專案延伸模組每月均會更新一次。 Excel 會 [以 Microsoft 365 更新](https://support.microsoft.com/office/when-do-i-get-the-newest-features-for-microsoft-365-da36192c-58b9-4bc9-8d51-bb6eed468516)。 Microsoft 365 更新較不頻繁，有些組織會使用延後通道，這表示更新會延後到三個月。

視您使用的用戶端應用程式或工具而定，驗證類型和登入方式可能會不同。 每個應用程式支援使用不同的功能來連線到 Azure Analysis Services 等雲端服務。

Power BI Desktop、Visual Studio 和 SSMS 支援 Active Directory 通用驗證，這是一種互動式方法，也支援 Azure AD Multi-Factor Authentication (MFA) 。 Azure AD MFA 有助於保護對資料和應用程式的存取，同時提供簡單的登入程式。 MFA 也提供具有數種驗證選項 (電話、簡訊、具有 PIN 的智慧卡或行動應用程式通知) 的強式驗證功能。 搭配 Azure AD 使用互動式 MFA 時，會出現快顯對話方塊以進行驗證。 **建議使用通用驗證**。

如果使用 Windows 帳戶登入 Azure，但未選取或無法使用通用驗證 (Excel)，就需要 [Active Directory 同盟服務 (AD FS)](/windows-server/identity/ad-fs/deployment/how-to-connect-fed-azure-adfs)。 使用同盟，Azure AD 和 Microsoft 365 使用者會使用內部部署認證進行驗證，而且可以存取 Azure 資源。

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)

Azure Analysis Services 伺服器使用 Windows 驗證、Active Directory 密碼驗證和 Active Directory 通用驗證，支援來自 [SSMS V17.1](/sql/ssms/download-sql-server-management-studio-ssms) 和更高版本的連線。 一般而言，建議您使用 Active Directory 通用驗證，因為：

*  支援互動式和非互動式驗證方法。

*  支援受邀加入 AS Azure 租用戶的 Azure B2B 來賓使用者。 連線到伺服器時，來賓使用者必須選取 Active Directory 通用驗證。

*  支援 Multi-Factor Authentication (MFA)。 Azure AD MFA 有助於保護對資料和應用程式的存取，並提供一系列的驗證選項：通話、文字訊息、具有 pin 的智慧卡或行動代理程式更新。 搭配 Azure AD 使用互動式 MFA 時，會出現快顯對話方塊以進行驗證。

### <a name="visual-studio"></a>Visual Studio

Visual Studio 會使用 Active Directory 通用驗證搭配 MFA 支援來連線到 Azure Analysis Services。 系統會提示使用者在首次部署時登入 Azure。 使用者必須在其部署至的伺服器上，使用具有伺服器管理員權限的帳戶登入 Azure 。 第一次登入 Azure 時會指派權杖。 系統會將此權杖快取到記憶體內部，以用於未來重新連線。

### <a name="power-bi-desktop"></a>Power BI Desktop

Power BI Desktop 會使用 Active Directory 通用驗證搭配 MFA 支援來連線到 Azure Analysis Services。 系統會提示使用者在首次連線時登入 Azure。 使用者必須使用伺服器管理員或資料庫角色隨附的帳戶來登入 Azure。

### <a name="excel"></a>Excel

Excel 使用者可以使用 Windows 帳戶、組織識別碼 (電子郵件地址) 或外部電子郵件地址，連線到伺服器。 外部電子郵件身分識別必須以來賓使用者的身分存在於 Azure AD 中。

## <a name="user-permissions"></a>使用者權限

**伺服器管理員** 專屬於 Azure Analysis Services 伺服器執行個體。 其會使用 Azure 入口網站、SSMS 和 Visual Studio 等工具進行連線，以執行新增資料庫和管理使用者角色等工作。 根據預設，系統會將建立伺服器的使用者自動新增成為 Analysis Services 伺服器管理員。 使用 Azure 入口網站或 SSMS，可以新增其他系統管理員。 伺服器管理員必須在相同的訂用帳戶中具有 Azure AD 租用戶的帳戶。 若要深入了解，請參閱[管理伺服器管理員](analysis-services-server-admins.md)。 

**資料庫使用者** 會使用 Excel 或 Power BI 等用戶端應用程式來連線到模型資料庫。 使用者必須新增到資料庫角色。 資料庫角色會定義資料庫的系統管理員、處理或讀取權限。 請務必了解具有系統管理員權限之角色的資料庫使用者與伺服器管理員不同。 不過，根據預設，伺服器管理員也是資料庫管理員。 若要深入了解，請參閱[管理資料庫角色和使用者](analysis-services-database-users.md)。

**Azure 資源擁有者**。 資源擁有者可管理 Azure 訂用帳戶的資源。 資源擁有者可以使用 Azure 入口網站中的 [存取控制]，或使用 Azure Resource Manager 範本，將 Azure AD 使用者身分識別新增至訂用帳戶內的擁有者或參與者角色。 

![Azure 入口網站中的存取控制](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

此層級的角色會套用到使用者或帳戶，讓他們可在入口網站或使用 Azure Resource Manager 範本完成需要執行的工作。 若要深入瞭解，請參閱 [azure 角色型存取控制 (AZURE RBAC) ](../role-based-access-control/overview.md)。 

## <a name="database-roles"></a>資料庫角色

 針對表格式模型定義的角色就是資料庫角色。 也就是，這些角色的成員是由具有特定權限的 Azure AD 使用者和安全性群組所組成，而這些權限會定義成員可對模型資料庫採取的動作。 資料庫角色會建立為資料庫中的個別物件，而且只適用於該角色建立所在的資料庫。   
  
 根據預設，當您建立新的表格式模型專案時，模型專案沒有任何角色。 在 Visual Studio 中，可以使用 [角色管理員] 對話方塊來定義角色。 在模型專案設計期間定義角色時，它們只會套用至模型工作區資料庫。 部署模型時，會將相同的角色套用至已部署的模型。 部署模型之後，伺服器和資料庫管理員可以使用 SSMS 來管理角色和成員。 若要深入了解，請參閱[管理資料庫角色和使用者](analysis-services-database-users.md)。
  
## <a name="next-steps"></a>後續步驟

[使用 Azure Active Directory 群組來管理資源的存取權](../active-directory/fundamentals/active-directory-manage-groups.md)   
[管理資料庫角色和使用者](analysis-services-database-users.md)  
[管理伺服器管理員](analysis-services-server-admins.md)  
[Azure 角色型存取控制 (Azure RBAC)](../role-based-access-control/overview.md)