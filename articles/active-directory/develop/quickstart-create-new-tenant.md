---
title: 建立 Azure Active Directory 租用戶 | Microsoft Docs
description: 了解如何建立 Azure AD 租用戶，以供註冊及建置應用程式使用。
services: active-directory
author: rwike77
manager: CelesteDG
ms.assetid: 1f4b24eb-ab4d-4baa-a717-2a0e5b8d27cd
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.topic: quickstart
ms.date: 03/12/2020
ms.author: ryanwi
ms.reviewer: jmprieur
ms.custom: aaddev, identityplatformtop40, fasttrack-edit
ms.openlocfilehash: 1d2140ca0064e2eb6b12726bcee7bf5362de8fbe
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/26/2020
ms.locfileid: "79408278"
---
# <a name="quickstart-set-up-a-tenant"></a>快速入門：設定租用戶

Microsoft 身分識別平台可讓開發人員建置以各種自訂 Microsoft 365 環境與身分識別為目標的應用程式。 若要開始使用 Microsoft 身分識別平台，您將須存取一個環境 (也稱為 Azure AD 租用戶)，在該環境中註冊及管理應用程式、存取 Microsoft 365 的資料，以及部署自訂條件式存取與租用戶限制。

租用戶是組織的代表。 它是組織或應用程式開發人員在與 Microsoft 建立關係 (例如註冊 Azure、Microsoft Intune 或 Microsoft 365) 時收到的 Azure AD 專屬執行個體。

每個 Azure AD 租用戶都與其他 Azure AD 租用戶截然不同，並且具有自己的公司與學校身分識別、消費者身分識別 (如果它是 Azure AD B2C 租用戶) 與應用程式註冊。 租用戶內部的應用程式註冊，只能允許來自您的租用戶或所有租用戶內的帳戶進行驗證。

## <a name="determining-environment-type"></a>決定環境類型

您可以建立兩種環境。 決定您需要的是僅基於您的應用程式將進行驗證之使用者的類型。

* 公司與學校 (Azure AD 帳戶) 或 Microsoft 帳戶 (例如 outlook.com 與 live.com)
* 社交與本機帳戶 (Azure AD B2C)

快速入門分成兩種案例，視您要建置的應用程式類型而定。 如果您需要有關身分識別類型的更多幫助，請查看[關於 Microsoft 身分識別平台](about-microsoft-identity-platform.md)

## <a name="work-and-school-accounts-or-personal-microsoft-accounts"></a>公司與學校帳戶，或個人 Microsoft 帳戶

### <a name="use-an-existing-tenant"></a>使用現有的租用戶

許多開發人員都已透過繫結至 Azure AD 租用戶的服務或訂用帳戶 (例如 Microsoft 365 或 Azure 訂用帳戶) 的方式取得租用戶。

1. 若要檢查租用戶，請使用您要用來管理應用程式的帳戶登入 [Azure 入口網站](https://portal.azure.com)。
1. 請檢查右上角。 如果您有租用戶，系統會自動將您登入，而且您會在您帳戶名稱正下方看到租用戶名稱。
   * 將滑鼠停留在 Azure 入口網站右上方的帳戶名稱上，以查看您的姓名、電子郵件、目錄/租用戶識別碼 (一個 GUID) 與您的網域。
   * 如果帳戶與多個租用戶相關聯，您可以選取帳戶名稱開啟功能表，以切換租用戶。 每個租用戶都有自己的租用戶識別碼。

> [!TIP]
> 如果您要尋找租用戶識別碼，您可以︰
> * 將滑鼠停留在您的帳戶名稱上，以取得目錄/租用戶識別碼，或
> * 在 Azure 入口網站中依序選取 [Azure Active Directory] > [屬性] > [目錄識別碼] 

如果您目前沒有與帳戶相關聯的租用戶，則會在您的帳戶名稱下方看到一個 GUID，且您在依照下一節的步驟執行之前，將無法執行註冊應用程式之類的動作。

### <a name="create-a-new-azure-ad-tenant"></a>建立新的 Azure AD 租用戶

如果您還沒有 Azure AD 租用戶，或是想要針對開發目的建立一個新的租用戶，請參閱[快速入門](../fundamentals/active-directory-access-create-new-tenant.md)，或直接依照[目錄建立經驗](https://portal.azure.com/#create/Microsoft.AzureActiveDirectory)中的指示操作。 您必須提供下列資訊才能建立新的租用戶：

- **組織名稱**
- **初始網域** - 這會是 *.onmicrosoft.com 的一部分。 您可以稍後自訂更多網域設定。
- **國家或區域**

> [!NOTE]
> 在為租用戶命名時，請使用英數字元。 不允許使用特殊字元。 名稱長度不得超過 256 個字元。

## <a name="social-and-local-accounts"></a>社交與本機帳戶

若要開始建置登入社交與本機帳戶的應用程式，您必須建立 Azure AD B2C 租用戶。 若要開始，請依照[建立 Azure AD B2C 租用戶](../../active-directory-b2c/tutorial-create-tenant.md)中的步驟執行。

## <a name="next-steps"></a>後續步驟

* [註冊應用程式](quickstart-register-app.md)，並與 Microsoft 身分識別平台整合。 
* 了解[驗證的基本概念](authentication-scenarios.md)。
* 如需訂用帳戶與 Azure AD 租用戶之間關聯性的詳細資訊，請參閱 [Azure 訂用帳戶如何與 Azure Active Directory 產生關聯](../fundamentals/active-directory-how-subscriptions-associated-directory.md)。
