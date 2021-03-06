---
title: Azure Active Directory 的目錄同步作業
description: 使用 Azure Active Directory 達到目錄同步作業的架構指引。
services: active-directory
author: BarbaraSelden
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: fundamentals
ms.topic: conceptual
ms.date: 10/10/2020
ms.author: baselden
ms.reviewer: ajburnle
ms.custom: it-pro, seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 748f91b2fe77667969e9736f8084a9dd24018425
ms.sourcegitcommit: d22a86a1329be8fd1913ce4d1bfbd2a125b2bcae
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/26/2020
ms.locfileid: "96172464"
---
# <a name="directory-synchronization"></a>目錄同步作業

許多組織都有包含內部部署和雲端元件的混合式基礎結構。 在本機與雲端目錄之間同步處理使用者的身分識別，可讓使用者使用單一認證集來存取資源。 

同步處理是 

* 根據特定條件建立物件
* 保持物件的更新
* 不再符合條件時移除物件。 

內部部署布建牽涉到從內部部署來源布建 (例如 Active Directory) Azure Active Directory (Azure AD) 。 

## <a name="use-when"></a>使用時機

您必須將內部部署 Active Directory 環境中的身分識別資料同步處理到 Azure AD。

![架構圖](./media/authentication-patterns/dir-sync-auth.png)

## <a name="components-of-system"></a>系統的元件

* **使用者**：使用 Azure AD 來存取應用程式。

* **Web 瀏覽器**：使用者與之互動的元件，以存取應用程式的外部 URL。

* **應用程式**：依賴使用 Azure AD 進行驗證和授權的 Web 應用程式。

* **Azure AD**：透過 Azure AD Connect 從組織的內部部署目錄同步身分識別資訊。 

* **Azure AD Connect**：用來將內部部署身分識別基礎結構連線至 Microsoft Azure AD 的工具。 Wizard 和引導式體驗可協助您部署和設定連線所需的必要條件和元件，包括同步處理和從 Active Directory 登入 Azure AD。 

* **Active Directory**： Active Directory 是大部分 Windows Server 作業系統中包含的目錄服務。 執行 Active Directory Domain Services (AD DS) 的伺服器稱為網域控制站。 他們會驗證並授權網域中的所有使用者和電腦。

## <a name="implement-directory-synchronization-with-azure-ad"></a>使用 Azure AD 來執行目錄同步作業

* [什麼是身分識別保護？](../cloud-provisioning/what-is-provisioning.md) 

* [混合式身分識別目錄整合工具](../hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md) 

* [Azure AD Connect 安裝藍圖](../hybrid/how-to-connect-install-roadmap.md)